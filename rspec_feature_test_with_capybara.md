## Rspec and Capybara Feature test cases setup

**1.** Controllers are ideally very lean in Rails, and our integration tests are exercising all code paths, they can be omitted in favor of thorough integration tests.

**2.** If your controllers are very complex, prefer to test controllers in isolation.

**3.** Capybara is an automated testing tool.
In simple words Capybara interact with browser to perform the instruction written in test case which are need to perform on browser(like visiting a url, clicking a link, typing text into a form and submitting it).

```ruby
describe "the signin process", type: :feature do
  before :each do
    User.make(email: 'user@example.com', password: 'password')
  end

  it "signs me in" do
    visit '/sessions/new'
    within("#session") do
      fill_in 'Email', with: 'user@example.com'
      fill_in 'Password', with: 'password'
    end
    click_button 'Sign in'
    expect(page).to have_content 'Success'
  end
end
```

**4.** Gems for integration test cases

```ruby
group :development, :test do
  gem 'rspec-rails'
  gem 'capybara'
  gem 'selenium-webdriver'
  gem 'factory_bot_rails'
  gem 'database_cleaner'
end

# spec/rspec_helper.rb
require 'capybara'

Capybara.javascript_driver = :selenium

```

**5.** Feature test cases goes into `spec/features` folder.

**6.** File name would be like `spec/features/widget_management_spec.rb`.

```ruby
require "rails_helper"

RSpec.feature "Widget management", :type => :feature do
  scenario "User creates a new widget" do
    visit "/widgets/new"

    fill_in "Name", :with => "My Widget"
    click_button "Create Widget"

    expect(page).to have_text("Widget was successfully created.")
  end
end
```

**7.** To share code between features move common capybara steps into a Ruby module in the rspec support directory.

```ruby
# spec/support/features/session_helpers.rb
module Features
  module SessionHelpers
    def sign_up_with(email, password)
      visit sign_up_path
      fill_in 'Email', with: email
      fill_in 'Password', with: password
      click_button 'Sign up'
    end

    def sign_in
      user = create(:user)
      visit sign_in_path
      fill_in 'Email', with: user.email
      fill_in 'Password', with: user.password
      click_button 'Sign in'
    end
  end
end
```

**8.** Modules must be explicitly included to share the common code between integration tests.

```ruby
# spec/support/features.rb
RSpec.configure do |config|
  config.include Features::SessionHelpers, type: :feature
end
```
