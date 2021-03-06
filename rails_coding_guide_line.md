## Routing

**1.**  When you need to add more actions to a RESTful resource (if you really need them) use `member` and `collection` routes.

```ruby
# bad
get 'subscriptions/:id/unsubscribe'
resources :subscriptions

# good
resources :subscriptions do
  get 'unsubscribe', on: :member
end

# bad
get 'photos/search'
resources :photos

# good
resources :photos do
  get 'search', on: :collection
end
 ```

**2.**  If you need to define multiple  `member/collection`  routes use the alternative block syntax.

```ruby
resources :subscriptions do
  member do
    get 'unsubscribe'
    # more routes
  end
end

resources :photos do
  collection do
    get 'search'
    # more routes
  end
end
```

**3.** Use nested routes to express better the relationship between ActiveRecord models.

```ruby
class Post < ActiveRecord::Base
  has_many :comments
end

class Comment < ActiveRecord::Base
  belongs_to :post
end

# routes.rb
resources :posts do
  resources :comments
end
```

**4.** If you need to nest routes more than 1 level deep then use the  `shallow: true`  option. This will save user from long urls  `posts/1/comments/5/versions/7/edit`  and you from long url helpers  `edit_post_comment_version`.

```ruby
resources :posts, shallow: true do
  resources :comments do
    resources :versions
  end
end
```

**5.** Use namespaced routes to group related actions.

```ruby
namespace :admin do
  # Directs /admin/products/* to Admin::ProductsController
  # (app/controllers/admin/products_controller.rb)
  resources :products
end
```

**6.** Never use the legacy wild controller route. This route will make all actions in every controller accessible via GET requests.

```ruby
# very bad
match ':controller(/:action(/:id(.:format)))'
```

**7.** Don't use `match` to define any routes unless there is need to map multiple request types among `[:get, :post, :patch, :put, :delete]` to a single action using `:via` option.

## Controllers

**1.** Keep the controllers skinny - they should only retrieve data for the view layer and shouldn't contain any business logic (all the business logic should naturally reside in the model).

**2.** Share no more than two instance variables between a controller and a view.

**3.** Controller Action Filter should be only for current action. not for inherited actions.

```ruby
# bad
class UsersController < ApplicationController
  before_action :require_login, only: :export
end

# good
class UsersController < ApplicationController
  before_action :require_login, only: :export

  def export
  end
end
```

**4.** Controller name always be plural and follow by Controller.

```ruby
#bad
class PostController < ApplicationController
end

#good
class PostsController < ApplicationController
end
```

**5.** The controller action logic should not be more then 4-5 lines.

**6.** Don't modify the params hash.

```ruby
#bad
 def search
   params.except!(:action, :controller)
   @search = User.search(params)
   render "search"
 end

#good
def search
  @search = User.search(search_params)
  render "search"
end

private

def search_params
  # params.except(:action, :controller)
  params.permit(:user_id, :name)
end
```

### Rendering

**1.** Prefer using a template over inline rendering.

```ruby
# very bad
class ProductsController < ApplicationController
  def index
    render inline: "<% products.each do |p| %><p><%= p.name %></p><% end %>", type: :erb
  end
end

# good
## app/views/products/index.html.erb
<%= render partial: 'product', collection: products %>
## app/views/products/_product.html.erb
<p><%= product.name %></p>
<p><%= product.price %></p>

## app/controllers/foo_controller.rb
class ProductsController < ApplicationController
  def index
    render :index
  end
end
```

**2.** Prefer `render plain:` over `render text:`.

```ruby
# bad - sets MIME type to `text/html`
...
render text: 'Ruby!'
...

# bad - requires explicit MIME type declaration
...
render text: 'Ruby!', content_type: 'text/plain'
...

# good - short and precise
...
render plain: 'Ruby!'
...
```

**3.** Prefer corresponding symbols to numeric HTTP status codes. They are meaningful and do not look like "magic" numbers for less known HTTP status codes.

```ruby
# bad
...
render status: 403
...

# good
...
render status: :forbidden
...
```

## Models

**1.** Name the models with meaningful (but short) names without abbreviations.

```ruby
#bad
class Img < ActiveRecord::Base
end

#good
class Image < ActiveRecord::Base
end
```

**2.** Model name always be singular

```ruby
#bad
class Posts < ActiveRecord::Base
end

#good
class Post <  ActiveRecord::Base
end
```

**3.** Don't use default_scope.

```ruby
#bad
default_scope where(active: true)

#good
scope :active, -> { where(active: true) }
```

## ActiveRecord

**1.** Avoid altering ActiveRecord defaults (table names, primary key, etc) unless you have a very good reason (like a database that's not under your control).

```ruby
# bad - don't do this if you can modify the schema
class Transaction < ActiveRecord::Base
  self.table_name = 'order'
  ...
end
```

**2.** Group macro-style methods (`has_many`, `validates`, etc) in below sequence.

```ruby
class User < ActiveRecord::Base
  # keep the default scope first (if any)
  default_scope { where(active: true) }

  # constants come up next
  COLORS = %w(red green blue)

  # afterwards we put attr related macros
  attr_accessor :formatted_date_of_birth

  attr_accessible :login, :first_name, :last_name, :email, :password

  # Rails4+ enums after attr macros, prefer the hash syntax
  enum gender: { female: 0, male: 1 }

  # followed by association macros
  belongs_to :country

  has_many :authentications, dependent: :destroy

  # and validation macros
  validates :email, presence: true
  validates :username, presence: true
  validates :username, uniqueness: { case_sensitive: false }
  validates :username, format: { with: /\A[A-Za-z][A-Za-z0-9._-]{2,19}\z/ }
  validates :password, format: { with: /\A\S{8,128}\z/, allow_nil: true }

  # next we have callbacks
  before_save :cook
  before_save :update_username_lower

  # other macros (like devise's) should be placed after the callbacks

  ...
end
```

**3.** Prefer `has_many :through` instead `has_and_belongs_to_many`. Using `has_many :through` allows additional attributes and validations on the join model.

```ruby
# not so good - using has_and_belongs_to_many
class User < ActiveRecord::Base
  has_and_belongs_to_many :groups
end

class Group < ActiveRecord::Base
  has_and_belongs_to_many :users
end

# preferred way - using has_many :through
class User < ActiveRecord::Base
  has_many :memberships
  has_many :groups, through: :memberships
end

class Membership < ActiveRecord::Base
  belongs_to :user
  belongs_to :group
end

class Group < ActiveRecord::Base
  has_many :memberships
  has_many :users, through: :memberships
end
```

**4.** Always use the latest validations syntax.

```ruby
# bad
validates_presence_of :email
validates_length_of :email, maximum: 100

# good
validates :email, presence: true, length: { maximum: 100 }
```

**5.** To make validations easy to read, don't list multiple attributes per validation.

```ruby
# bad
validates :email, :password, presence: true
validates :email, length: { maximum: 100 }

# good
validates :email, presence: true, length: { maximum: 100 }
validates :password, presence: true
```

**6.** When a custom validation is used more than once or the validation is some regular expression mapping, create a custom validator file. Keep custom validators under `app/validators`.

```ruby
# bad
class Person
  validates :email, format: { with: /\A([^@\s]+)@((?:[-a-z0-9]+\.)+[a-z]{2,})\z/i }
end

# good
class EmailValidator < ActiveModel::EachValidator
  def validate_each(record, attribute, value)
    record.errors[attribute] << (options[:message] || 'is not a valid email') unless value =~ /\A([^@\s]+)@((?:[-a-z0-9]+\.)+[a-z]{2,})\z/i
  end
end

class Person
  validates :email, email: true
end
```

**7.** Always use named scopes.

```ruby
class User < ActiveRecord::Base
  scope :active, -> { where(active: true) }
  scope :inactive, -> { where(active: false) }

  scope :with_orders, -> { joins(:orders).select('distinct(users.id)') }
end
```

**8.** Order callback declarations in the order, in which they will be executed.

```ruby
-   #bad
    class Person
      after_commit/after_rollback :after_commit_callback
      after_save :after_save_callback
      around_save :around_save_callback
      after_update :after_update_callback
      before_update :before_update_callback
      after_validation :after_validation_callback
      before_validation :before_validation_callback
      before_save :before_save_callback
      before_create :before_create_callback
      after_create :after_create_callback
      around_create :around_create_callback
      around_update :around_update_callback
    end

    #good
    class Person
      before_validation :before_validation_callback
      after_validation :after_validation_callback
      before_save :before_save_callback
      around_save :around_save_callback
      before_create :before_create_callback
      around_create :around_create_callback
      after_create :after_create_callback
      before_update :before_update_callback
      around_update :around_update_callback
      after_update :after_update_callback
      after_save :after_save_callback
      after_commit/after_rollback :after_commit_callback
    end
```

**9.** Use `find_each` to iterate over a collection of Large Data objects.

```ruby
# bad
Person.all.each do |person|
  person.do_awesome_stuff
end

Person.where('age > 21').each do |person|
  person.party_all_night!
end

# good
Person.find_each do |person|
  person.do_awesome_stuff
end

Person.where('age > 21').find_each do |person|
  person.party_all_night!
end
```

**10.** Always call `before_destroy` callbacks that perform validation with `prepend: true`.

```ruby
# bad (roles will be deleted automatically even if super_admin? is true)
has_many :roles, dependent: :destroy

before_destroy :ensure_deletable

def ensure_deletable
  raise "Cannot delete super admin." if super_admin?
end

# good
has_many :roles, dependent: :destroy

before_destroy :ensure_deletable, prepend: true

def ensure_deletable
  raise "Cannot delete super admin." if super_admin?
end
```

**11.** Define the `dependent` option to the `has_many` and `has_one` associations where needed.

```ruby
# bad
class Post < ActiveRecord::Base
  has_many :comments
end

# good
class Post < ActiveRecord::Base
  has_many :comments, dependent: :destroy
end
```

**12.** When data going to be save into database always use the exception raising bang! method or handle the method return value.

```ruby
# bad
user.create(name: 'Bruce')

# bad
user.save

# good
user.create!(name: 'Bruce')
# or
bruce = user.create(name: 'Bruce')
if bruce.persisted?
  ...
else
  ...
end

# good
user.save!
# or
if user.save
  ...
else
  ...
end
```

**13.** Use Lambda in Scopes.

```ruby
#bad
scope :recent, where('starting_date > ?', 1.day.ago) where('starting_date > ?', '02-10-2013 20:00') where('starting_date > ?', '02-10-2013 20:00')

#good
scope :recent, lambda{ where('starting_date > ?', 1.day.ago)} => It will create where('starting_date > ?', '02-10-2013 20:00') where('starting_date > ?', '03-10-2013 21:23')
```

## ActiveRecord Queries

**1.** Avoid string interpolation in queries, as it will make your code susceptible to SQL injection attacks.

```ruby
# bad - param will be interpolated unescaped
Client.where("orders_count = #{params[:orders]}")

# good - param will be properly escaped
Client.where('orders_count = ?', params[:orders])
```

**2.** Consider using named placeholders instead of positional placeholders when you have more than 1 placeholder in your query.

```ruby
# okish
Client.where(
  'created_at >= ? AND created_at <= ?',
  params[:start_date], params[:end_date]
)

# good
Client.where(
  'created_at >= :start_date AND created_at <= :end_date',
  start_date: params[:start_date], end_date: params[:end_date]
)
```

**3.** Favor the use of `find` over `where.take!`, `find_by!`, and `find_by_id!` when you need to retrieve a single record by primary key id and raise `ActiveRecord::RecordNotFound` when the record is not found.

```ruby
# bad
User.where(id: id).take!

# bad
User.find_by_id!(id)

# bad
User.find_by!(id: id)

# good
User.find(id)
```

**4.** Favor the use of `where.not` over SQL

```ruby
# bad
User.where("id != ?", id)

# good
User.where.not(id: id)
```

## Migrations

**1.** Keep the `schema.rb` (or `structure.sql`) under version control(git).

**2.** Use `rake db:schema:load` instead of `rake db:migrate` to initialize an empty database

**3.** Enforce default values in the migrations themselves instead of in the application layer.

```ruby
# bad - application enforced default value
class Product < ActiveRecord::Base
  def amount
    self[:amount] || 0
  end
end

# good - database enforced
class AddDefaultAmountToProducts < ActiveRecord::Migration
  def change
    change_column_default :products, :amount, 0
  end
end
```

**4.** Don't use non-reversible migration commands in the `change` method

```ruby
# bad
class DropUsers < ActiveRecord::Migration
  def change
    drop_table :users
  end
end

# good
class DropUsers < ActiveRecord::Migration
  def up
    drop_table :users
  end

  def down
    create_table :users do |t|
      t.string :name
    end
  end
end

# good
# In this case, block will be used by create_table in rollback
# http://api.rubyonrails.org/classes/ActiveRecord/ConnectionAdapters.html#method-i-drop_table
class DropUsers < ActiveRecord::Migration
  def change
    drop_table :users do |t|
      t.string :name
    end
  end
end
```

## Views

**1.** Never call the model layer directly from a view.

```ruby
#bad
<%= User.first.name %>
<%= User.where(id: 1) %>
<%= User.update(name: "abc") %>
<% User.all.each do |user| %>
  do_something
<% end %>

#good - access the instance form controller
<%= @user.name %>

# Never call update/where any query which access or update the database

<% @users.each do |user| %>
  do_something
<% end %>
```

**2.**  Make Helper Methods for Views.

```ruby
#bad
<%= f.select( :category, ['Australia', 'Belgium', 'Canada', 'China', 'India', 'Malaysia', 'Switzerland'].collect{|country| [country, country]}) %>

#good
<%= f.select( :category, country_names.collect{|country| [country, country]}) %>

#Create a Helper:
def country_names ['Australia', 'Belgium', 'Canada', 'China', 'India', 'Malaysia', 'Switzerland'] end
```

**3.** Never make complex formatting in the views.

```ruby
#bad
<%= order.date.nil? ? "Not ordered" : order.date.strftime("%m/%d/%Y - %H:%M") %>
<%= order.is_custom ? "Custom" : order.vendor.brand.name  %>

#good - DRY and readable
class OrderDecorator < Draper::Decorator
  delegate_all

  def formatted_ordered_date
    object.date.nil? ? "Not ordered" : object.date.strftime("%m/%d/%Y - %H:%M")
  end

  def vendor_name
    object.is_custom ? "Custom" : object.vendor.brand.name
  end
end

<%= order.formatted_ordered_date %>
<%= order.vendor_name %>
```

**4.** Do not use the word partial when you don’t want to pass some variables in it.

```ruby
#bad
render partial: 'partial_1'

#good
render 'partial_1'
render partial: 'partial_1', locals: {f: @object}
```

## Mailers

**1.** Always user `mailer` suffix in mailers Name like `SomethingMailer`.

**2.** Provide both HTML and plain-text view templates.

**3.** Enable errors raised on failed mail delivery in your development environment. The errors are disabled by default.

```ruby
# config/environments/development.rb

config.action_mailer.raise_delivery_errors = true
```

**4.** If you need to use a link to your site in an email, always use the `_url`, not `_path` methods. The `_url` methods include the host name and the `_path` methods don't.

```ruby
# bad
You can always find more info about this course
<%= link_to 'here', course_path(@course) %>

# good
You can always find more info about this course
<%= link_to 'here', course_url(@course) %>
```

**5.** Format the from and to addresses properly for mailer. Use the following format:

```ruby
# in your mailer class
default from: 'sender Name <info@your_site.com>',
        to: 'receiver Name <info@your_site.com>
```

**6.** When sending html emails all styles should be inline, as some mail clients have problems with external styles.

**7.** When sending emails use background jobs to send email.
Emails can be sent in background process with the help of  sidekiq gem.

```ruby
#good
class ContactJob < ApplicationJob
  queue_as :default

  def perform(message)
    ContactMailer.submission(message).deliver
  end
end

  def create
    ContactJob.perform_later params.permit(:message)[:message]
  end
```

## Bundler

**1.** Put gems used only for development or testing in the appropriate group in the Gemfile

```ruby
group :development do
  gem 'web-console',           '3.5.1'
  gem 'listen',                '3.1.5'
  gem 'spring',                '2.0.2'
  gem 'spring-watcher-listen', '2.0.1'
end

group :test do
  gem 'rails-controller-testing', '1.0.2'
  gem 'minitest',                 '5.10.3'
  gem 'minitest-reporters',       '1.1.14'
  gem 'guard',                    '2.14.1'
  gem 'guard-minitest',           '2.4.6'
end
```

**2.** Use only established gems in your projects.

**3.** Do not remove the `Gemfile.lock` from version control.
