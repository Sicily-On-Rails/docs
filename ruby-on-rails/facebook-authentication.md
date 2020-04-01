# Facebook authentication with devise gem

### Prerequisites:
- Rails with a devise authentication: [devise](https://github.com/heartcombo/devise) 
- [Facebook App](https://developers.facebook.com/) 


The next step is to add an OmniAuth gem to your application. This can be done in our Gemfile: 

```ruby
....
gem 'devise'
gem 'omniauth'
gem 'omniauth-facebook'
...
```

Run `bundle install` to intstall the gems

Next up, you should add the columns "provider" (string) and "uid" (string) to your User model.

``` shell
rails g migration AddOmniauthToUsers provider:string uid:string
```
And run:

```shell
rails  db:migrate
```

Next, you need to declare the provider in your `config/initializers/devise.rb`:

```ruby 
config.omniauth :facebook, "APP_ID", "APP_SECRET", scope:'email', info_fields: 'email,name'
```
Update model `devise-model.rb`, for example `app/models/user.rb`:

``` ruby
class User < ApplicationRecord
  # Include default devise modules. Others available are:
  # :confirmable, :lockable, :timeoutable, :trackable and :omniauthable
  devise :database_authenticatable, :registerable,
         :recoverable, :rememberable, :validatable,
         :omniauthable, omniauth_providers: %i[facebook]

  def self.from_omniauth(auth)
    user = User.where(email: auth.info.email).first
    
    if user 
      return user
    else

      #def self.from_omniauth(auth)
          where(provider: auth.provider, uid: auth.uid).first_or_create do |user|
          user.email = auth.info.email
          user.password = Devise.friendly_token[0, 20]
          #user.name = auth.info.name   # assuming the user model has a name
          #user.image = auth.info.image # assuming the user model has an image
          # If you are using confirmable and the provider(s) you use validate emails, 
          # uncomment the line below to skip the confirmation emails.
          # user.skip_confirmation!
          user.uid = auth.uid
          user.provider = auth.provider
        end
      #end
    end
  end

end
```

Now we just add the file `app/controllers/users/omniauth_callbacks_controller.rb`:

``` ruby
class Users::OmniauthCallbacksController < Devise::OmniauthCallbacksController
    def facebook
      # You need to implement the method below in your model (e.g. app/models/user.rb)
      @user = User.from_omniauth(request.env["omniauth.auth"])
  
      if @user.persisted?
        sign_in_and_redirect @user, event: :authentication #this will throw if @user is not activated
        set_flash_message(:notice, :success, kind: "Facebook") if is_navigational_format?
      else
        session["devise.facebook_data"] = request.env["omniauth.auth"]
        redirect_to new_user_registration_url
      end
    end
  
    def failure
      redirect_to root_path
    end
end
```

Update `config/routes.rb`:
``` ruby
devise_for :users, controllers: { omniauth_callbacks: 'users/omniauth_callbacks'}
```

Add in `app/views/devise/sessions/new.html.erb`:

``` ruby 
....

<%= link_to "Sign in with facebook", user_facebook_omniauth_authorize_path%>

....

```

Add in `app/views/devise/registration/new.html.erb`:

``` ruby
....

<%= link_to "Sign in with facebook", user_facebook_omniauth_authorize_path%>

....

```