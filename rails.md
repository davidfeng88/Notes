# Rails

## 1. Hello-world app

### Start a new project

```bash
gem install rails
# or gem install rails -v 6.0.0

# confirm rails version
rails -v
Rails 6.0.2.2

# also need to install yarn

rails new my_app

cd my_app
bundle install
rails s

# go to http://localhost:3000
```

![default Rails directory structure](.gitbook/assets/01tab02.jpg)

### Gemfile

```ruby
gem 'rails' # installs latest rails

# installs latest capybara, later than v2.15
gem 'capybara', '>= 2.15' 

# install 6.0.1 (if available) but not 6.1.0
gem 'rails', '~> 6.0.0'
```

### Hello-World app

```ruby
# app/controllers/application_controller.rb
class ApplicationController < ActionController::Base

  def hello
    render html: "hello, world!"
  end
end

# config/routes.rb

Rails.application.routes.draw do
  root 'application#hello'
end
```

## 2. Toy App

### models

* users: id \(int\), name \(string\), email \(string\)
* microposts: id \(int\), content \(text\), user\_id \(int\)

```bash
$ rails generate scaffold User name:string email:string
....
$ rails db:migrate
```

![page URLs for the Users resource](.gitbook/assets/02tab01.jpg)

{% code title="config/routes.rb" %}
```ruby
Rails.application.routes.draw do
  resources :users
  root 'users#index'
end
```
{% endcode %}

![RESTful routes provided by Users resource](.gitbook/assets/02tab02.jpg)

### More models

```ruby
#app/models/user.rb

class User < ApplicationRecord
  has_many :microposts # association
end

# app/models/micropost.rb

class Micropost < ApplicationRecord
  belongs_to :user # association
  validates :content, length: { maximum: 140 },
                      presence: true # validation
end

# to confirm, use rails console
$ rails c
User.first
...
User.first.microposts
...
```

## 3. Sample App

```bash
rails t # run tests
rails g controller ControllerName <optional action names> # g is short for generate
# undo
rails destroy controller ControllerName <optional action names>
# generate / destroy models
rails generate model User name:string email:string
rails destroy model User

rails db:migrate
rails db:rollback
rails db:migrate VERSION=0
```

## Rails-flavored Ruby

### Strings

```ruby
rails c
# By default, the console starts in a development environment

# string interpolation (does not work for single quoted strings)
"Hello, #{name}!"

'\n' is the same as "\\n"

# print string
>> puts "foo"
foo
=> nil # puts returns nil
>> print "foo" # print string without extra line
foo=> nil

# string methods
>> "foobar".empty?     # methods that return a boolean
=> false

>> "foobar".length     # Passing the "length" message to a string
=> 6

# .reverse for a string
```

### Methods

```ruby
# if/elsif/else end
# && || !
nil.to_s # convert to string
1.nil?
=> false

puts "this" if 1.nil?
puts "that" unless 2.nil?

# only nil and false are falsey in Ruby. 0 is truthy.
# !!object => boolean

def func(str='') # default value
  ...
end

func # when no argument is provided, we don't even need () to invoke it

# method implicit returns the last statement value
# or you can use "return"
```

### Other data structures

```ruby
# string to array
>> "foo bar baz".split # Split a string into a three-element array.
=> ["foo", "bar", "baz"]
>> "fooxbarxbaz".split('x')
=> ["foo", "bar", "baz"]

# array to string
>> a
=> [42, 8, 17, 6, 7, "foo", "bar"]
>> a.join                      # Join on nothing.
=> "4281767foobar"
>> a.join(', ')                # Join on comma-space.
=> "42, 8, 17, 6, 7, foo, bar"

# access elements in array
# -1, (Rails) first, second, last
# common array methods
# empty? include? sort sort! reverse reverse! shuffle shuffle!
# bang methods modify the array
# push / <<
# array can contain elements with different kind (e.g. integer and string)

# range
# to_a: convert to array
>> 0..9 => 0..9
>> 0..9.to_a              # Oops, call to_a on 9.
NoMethodError: undefined method `to_a' for 9:Fixnum
>> (0..9).to_a            # Use parentheses to call to_a on the range.
=> [0, 1, 2, 3, 4, 5, 6, 7, 8, 9]

>> a = %w[foo bar baz quux]        # Use %w to make a string array.
=> ["foo", "bar", "baz", "quux"]
>> a[0..2]
=> ["foo", "bar", "baz"]

>> a = (0..9).to_a
=> [0, 1, 2, 3, 4, 5, 6, 7, 8, 9]
>> a[2..(a.length-1)]               # Explicitly use the array's length.
=> [2, 3, 4, 5, 6, 7, 8, 9]
>> a[2..-1]                         # Use the index -1 trick.
=> [2, 3, 4, 5, 6, 7, 8, 9]

# range with characters
>> ('a'..'e').to_a
=> ["a", "b", "c", "d", "e"]
```

### Blocks

