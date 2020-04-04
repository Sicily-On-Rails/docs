# Authentication in Ruby On Rails

### Users controller
Generate a controller `users` with an action `new`:

``` shell
rails generate controller Users new
```

`app/controllers/users_controller.rb`
```ruby 
class UsersController < ApplicationController
    def new
    end
end
```

`app/views/users/new.html.erb`

```ruby
<h1>Users#new</h1>
<p>Find me in app/views/users/new.html.erb</p>
```


### Signup URL
`config/routes.rb`
```ruby
Rails.application.routes.draw do
  get  '/signup',  to: 'users#new'
end
```

### Add link to signup page
```ruby
<%= link_to "Sign up", signup_path%>
```

### User model
```shell
rails generate model User name:string email:string
```
This command will generate:
- migration for the User model
- user model
- ...

#### Migration for the user model:
`db/migrate/[timestamp]_create_users.rb`
```ruby
  def change
    create_table :users do |t|
      t.string :name
      t.string :email

      t.timestamps
    end
  end
end
```

Run the migration:
```ruby
rails db:migrate
```

#### The model file
`app/models/user.rb`
```ruby 
class User < ApplicationRecord
end
```

### Password
Most of the secure password machinery will be implemented using a single Rails method called `has_secure_password`, which we’ll include in the User model as follows:
```ruby
class User < ApplicationRecord
  .
  .
  .
  has_secure_password
end
```
When included in a model as above, this one method adds the following functionality:

- The ability to save a securely hashed `password_digest` attribute to the database
- A pair of virtual attributes (`password` and `password_confirmation`), including presence validations upon object creation and a validation requiring that they match
- An `authenticate` method that returns the user when the password is correct (and false otherwise)

#### Migration for the `password_digest` column

```shell
rails generate migration add_password_digest_to_users password_digest:string
```

The migration add `password_digest` column.
`db/migrate/[timestamp]_add_password_digest_to_users.rb`
```ruby 
class AddPasswordDigestToUsers < ActiveRecord::Migration[6.0]
  def change
    add_column :users, :password_digest, :string
  end
end
```

Run the migration
```shell
rails db:migrate
```
To make the password digest, `has_secure_password` uses a state-of-the-art hash function called `bcrypt`.  To use bcrypt in the sample application, we need to add the bcrypt gem to our `Gemfile`.
Adding `bcrypt` to the `Gemfile`.

```ruby 
.
.
.
gem 'bcrypt',         '3.1.7'
.
.
.
```

### Signup 
Add in `config/routes.rb`
``` ruby
.
.
get  '/signup',  to: 'users#new'
resources :users
.
.
```

#### Users controller
`app/controllers/users_controller.rb`
```ruby

  def index
    @users = User.all
  end

  def show
    @user = User.find(params[:id])
  end

  def new
    @user = User.new
  end

  def create
    @user = User.new(user_params)
    if @user.save
      flash[:success] = "Welcome"
      redirect_to @user
    else
      render 'new'
    end
  end

  def edit
    @user = User.find(params[:id])
  end

  def update
    @user = User.find(params[:id])
    if @user.update(user_params)
      flash[:success] = "User updated"
      redirect_to @user
    else
      render 'edit'
    end
  end

  def destroy
    User.find(params[:id]).destroy
    flash[:success] = "User deleted"
    redirect_to users_url
  end

  

  private

    def user_params
      params.require(:user).permit(:name, :email, :password,
                                   :password_confirmation)
    end

end
```

#### Signup form
`app/view/users/new.html.erb`

```ruby
<h1>Sign up</h1>
  <%= form_with(model: @user, local: true) do |f| %>
    <%= f.label :name %>
    <%= f.text_field :name %>

    <%= f.label :email %>
    <%= f.email_field :email %>

    <%= f.label :password %>
    <%= f.password_field :password %>

    <%= f.label :password_confirmation, "Confirmation" %>
    <%= f.password_field :password_confirmation %>

    <%= f.submit %>
    <% end %>
 
```
### Basic login

### Generate a Sessions controller
```shell
rails generate controller Sessions new

```

`app/controllers/sessions_controller.rb`

```ruby
class SessionsController < ApplicationController

  def new
  end

  def create
      user = User.find_by(email: params[:session][:email].downcase)
      if user && user.authenticate(params[:session][:password])
        redirect_to user
     else
        flash.now[:danger] = 'Invalid email/password combination'
        render 'new'
     end
   end

  def destroy
  end
end
```

`config/routes.rb`

```ruby
  .
  .
  get    '/signup',  to: 'users#new'
  get    '/login',   to: 'sessions#new'
  post   '/login',   to: 'sessions#create'
  delete '/logout',  to: 'sessions#destroy'
  resources :users
  .
  .

```

`app/views/sessions/new.html.erb`

```ruby
<h1>Log in</h1>
    
  <%= form_with(url: login_path, scope: :session, local: true) do |f| %>

    <%= f.label :email %>
    <%= f.email_field :email, class: 'form-control' %>

    <%= f.label :password %>
    <%= f.password_field :password, class: 'form-control' %>

    <%= f.submit "Log in", class: "btn btn-primary" %>
    <% end %>

    <p>New user? <%= link_to "Sign up now!", signup_path %><p> 
```

Including the Sessions helper module into the Application con-
troller.
`app/controllers/application_controller.rb`
```ruby

class ApplicationController < ActionController::Base
  include SessionsHelper
end
```
Logging a `user` in is simple with the help of the session method defined by Rails. (This method is separate and distinct from the Sessions controller generated)
We can treat session as if it were a hash, and assign to it as follows:
```ruby
  session[:user_id] = user.id
```
The log_in function:

`app/helpers/sessions_helper.rb`
```ruby

module SessionsHelper
  # Logs in the given user.
  def log_in(user)
    session[:user_id] = user.id
  end
end
```
Update the Sessions controller:
`app/controller/sessions_controller.rb`
```ruby
class SessionsController < ApplicationController

  def new
  end

  def create
      user = User.find_by(email: params[:session][:email].downcase)
      if user && user.authenticate(params[:session][:password])
        log_in user
        redirect_to user
     else
        flash.now[:danger] = 'Invalid email/password combination'
        render 'new'
     end
   end

  def destroy
  end
end

```

#### Current user
Having placed the user’s id securely in the temporary session, we are now in a position to retrieve it on subsequent pages, which we’ll do by defining a current_user method to find the user in the database corresponding to the session id.

To find the current user, one possibility is to use the find method:

```ruby
User.find(session[:user_id])
```
The `find` method raises an exception if the user id doesn’t exist. This behavior is appropriate on the user profile page because it will only happen if the id is invalid, but in the present case session[:user_id] will often be nil (i.e., for non-logged-in users). 

To handle this possibility, we’ll use the same `find_by` method used to find by email address in the create method, with id in place of email:

```ruby
User.find_by(id: session[:user_id])
```

We could now define the current_user method as follows:

```ruby
def current_user
  if session[:user_id]
    User.find_by(id: session[:user_id])
  end
end
```

```ruby
if @current_user.nil?
  @current_user = User.find_by(id: session[:user_id])
else
  @current_user
end
```

`app/helpers/sessions_helper.rb`
```ruby

  # Logs in the given user.
  def log_in(user)
    session[:user_id] = user.id
  end

  # Returns the current logged-in user (if any).
  def current_user
    if session[:user_id]
      @current_user ||= User.find_by(id: session[:user_id])
    end
  end
end
```