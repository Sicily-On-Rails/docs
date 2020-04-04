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
Adding `bcrypt` to the `Gemfile``
  