## Rspec setup

```ruby
group :development, :test do
  gem 'byebug'
  gem 'rspec-rails'
  gem 'factory_bot_rails'
  gem 'shoulda-matchers', require: false
  gem 'database_cleaner'
  gem 'faker'
end

group :test do
  # Gem for feature test cases support
  gem 'capybara'
  gem 'selenium-webdriver'
end
```

- `bundle install`

- `rails generate rspec:install`

- Above command will generate
      create  .rspec
      create  spec
      create  spec/spec_helper.rb
      create  spec/rails_helper.rb
- For factories add `facotry_bot.rb` under 'spec/support'
- For database clear add 'database_cleaner.rb' under spec/support
- Under `spec/factories` create factory files naming plural with model name, like `users.rb`

```ruby
FactoryBot.define do
  factory :user do
    name { Faker::Name.name }
    email { Faker::Internet.email }
  end
end
```

- Model testcase e.g. create file under 'spec/models' prefix by `_spec` e.g. `user_spec.rb`

```ruby
#spec/models/user_spec.rb
require 'rails_helper'

RSpec.describe User, type: :model do

  let(:user) { build :user }

  context 'association' do
    describe "Associations" do
      it { should have_many(:posts)}
      it { should belong_to(:guide) }
    end
  end

  describe "Validations" do
    it "is valid with valid attributes" do
      expect(user).to be_valid
    end

    it "is not valid without a name" do
      user.name = nil
      expect(user).to_not be_valid
    end

    it "is not valid without an email" do
      user.email = nil
      expect(user).to_not be_valid
    end
  end
end
```

### Database cleaner setting

```ruby
#spec/support/database_cleaner.rb
RSpec.configure do |config|

  config.before(:suite) do
    DatabaseCleaner.clean_with(:truncation)
  end

  config.before(:each) do
    DatabaseCleaner.strategy = :transaction
  end

  config.before(:each, :js => true) do
    DatabaseCleaner.strategy = :truncation
  end

  config.before(:each) do
    DatabaseCleaner.start
  end

  config.after(:each) do
    DatabaseCleaner.clean
  end
end
```

- Add line into `rspe_helper.rb` to include all support file
```ruby
Dir[Rails.root.join('spec', 'support', '**', '*.rb')].each { |f| require f }
```

### File upload testing setting
- Create a helper
"under support assets is folder to keep test images, you can keep it where you want and change the root path accordingly"

```ruby
#spec/support/file_helper.rb
module FilesUploadHelper
  extend self
  extend ActionDispatch::TestProcess

  def png_name; 'test-image-1.png' end
  def png; upload(png_name, 'image/png') end

  def jpg_name; 'test-image-2.jpeg' end
  def jpg; upload(jpg_name, 'image/jpg') end

  # def tiff_name; 'test-image.tiff' end
  # def tiff; upload(tiff_name, 'image/tiff') end

  # def pdf_name; 'test.pdf' end
  # def pdf; upload(pdf_name, 'application/pdf') end

  private

  def upload(name, type)
    file_path = Rails.root.join('spec', 'support', 'assets', name)
    fixture_file_upload(file_path, type)
  end
end
```

- In Factory file call helper to upload file for field

```ruby
# For has_one photo
trait :with_photo do
  photo { FilesUploadHelper.jpg] }
end

# For has_many photos
trait :with_photos do
  photos { [FilesUploadHelper.png, FilesUploadHelper.jpg] }
end
```
