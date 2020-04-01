# Facebook authentication with devise gem

The first step then is to add an OmniAuth gem to your application. This can be done in our Gemfile: 

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
config.omniauth :facebook, "APP_ID", "APP_SECRET"
```