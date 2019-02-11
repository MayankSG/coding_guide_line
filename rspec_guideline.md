## Rspec Best Practice

**1.** Do not leave empty lines after `feature`, `context` or `describe` descriptions. It doesn't make the code more readable and lowers the value of logical chunks.

```ruby
# bad
describe Article do

  describe '#summary' do

    context 'when there is a summary' do

      it 'returns the summary' do
        # ...
      end
    end
  end
end

# good
describe Article do
  describe '#summary' do
    context 'when there is a summary' do
      it 'returns the summary' do
        # ...
      end
    end
  end
end
```

**2.** Leave one empty line between `feature`, `context` or `describe` blocks. Do not leave empty line after the last such block in a group.

```ruby
# bad
describe Article do
  describe '#summary' do
    context 'when there is a summary' do
      # ...
    end
    context 'when there is no summary' do
      # ...
    end

  end
  describe '#comments' do
    # ...
  end
end

# good
describe Article do
  describe '#summary' do
    context 'when there is a summary' do
      # ...
    end

    context 'when there is no summary' do
      # ...
    end
  end

  describe '#comments' do
    # ...
  end
end
```

**3.** Leave one empty line after `let`, `subject`, and `before`/`after` blocks.

```ruby
# bad
describe Article do
  subject { FactoryBot.create(:some_article) }
  describe '#summary' do
    # ...
  end
end

# good
describe Article do
  subject { FactoryBot.create(:some_article) }

  describe '#summary' do
    # ...
  end
end
```

**4.** Only group `let`, `subject` blocks and separate them from `before`/`after` blocks. It makes the code much more readable.

```ruby
# bad
describe Article do
  subject { FactoryBot.create(:some_article) }
  let(:user) { FactoryBot.create(:user) }
  before do
    # ...
  end
  after do
    # ...
  end
  describe '#summary' do
    # ...
  end
end

# good
describe Article do
  subject { FactoryBot.create(:some_article) }
  let(:user) { FactoryBot.create(:user) }

  before do
    # ...
  end

  after do
    # ...
  end

  describe '#summary' do
    # ...
  end
end
```

**5.** Leave one empty line around `it`/`specify` blocks. This helps to separate the expectations from their conditional logic (contexts for instance).

```ruby
# bad
describe '#summary' do
  let(:item) { double('something') }

  it 'returns the summary' do
    # ...
  end
  it 'does something else' do
    # ...
  end
  it 'does another thing' do
    # ...
  end
end

# good
describe '#summary' do
  let(:item) { double('something') }

  it 'returns the summary' do
    # ...
  end

  it 'does something else' do
    # ...
  end

  it 'does another thing' do
    # ...
  end
end
```

**6.** When `subject` is used, it should be the first declaration in the example group.

```ruby
# bad
describe Article do
  before do
    # ...
  end

  let(:user) { FactoryBot.create(:user) }
  subject { FactoryBot.create(:some_article) }

  describe '#summary' do
    # ...
  end
end

# good
describe Article do
  subject { FactoryBot.create(:some_article) }
  let(:user) { FactoryBot.create(:user) }

  before do
    # ...
  end

  describe '#summary' do
    # ...
  end
end
```

**7.** Don't specify `:each`/`:example` scope for `before`/`after`/`around` blocks, as it is the default. Prefer `:example` when explicitly indicating the scope.

```ruby
# bad
describe '#summary' do
  before(:example) do
    # ...
  end

  # ...
end

# good
describe '#summary' do
  before do
    # ...
  end

  # ...
end
```

**8.** Use `:context` instead of the ambiguous `:all` scope in `before`/`after` hooks.

```ruby
# bad
describe '#summary' do
  before(:all) do
    # ...
  end

  # ...
end

# good
describe '#summary' do
  before(:context) do
    # ...
  end

  # ...
end
```

**9.** Avoid using `before`/`after` with `:context` scope. Beware of the state leakage between the examples.

```ruby
# ok
describe '#summary' do
  before(:context) do
    # ...
  end

  # ...
end

# good
describe '#summary' do
  before do
    # ...
  end

  # ...
end
```

**10.** Use contexts to make the tests clear, well organized, and easy to read.

```ruby
# bad
it 'has 200 status code if logged in' do
  expect(response).to respond_with 200
end

it 'has 401 status code if not logged in' do
  expect(response).to respond_with 401
end

# good
context 'when logged in' do
  it { is_expected.to respond_with 200 }
end

context 'when logged out' do
  it { is_expected.to respond_with 401 }
end
```

**11.** Use `let` blocks instead of `before` blocks to create data for the spec examples.

```ruby
# bad
before { @article = FactoryBot.create(:article) }

# good
let(:article) { FactoryBot.create(:article) }
```

**12.** When several tests relate to the same subject, use `subject` to reduce repetition.

```ruby
# bad
it { expect(hero.equipment).to be_heavy }
it { expect(hero.equipment).to include 'sword' }

# good
subject(:equipment) { hero.equipment }

it { expect(equipment).to be_heavy }
it { expect(equipment).to include 'sword' }
```

**13.** Use named `subject`.

```ruby
# bad
describe Article do
  subject { FactoryBot.create(:article) }

  it 'is not published on creation' do
    expect(subject).not_to be_published
  end
end

# good
describe Article do
  subject { FactoryBot.create(:article) }

  it 'is not published on creation' do
    is_expected.not_to be_published
  end
end

# even better
describe Article do
  subject(:article) { FactoryBot.create(:article) }

  it 'is not published on creation' do
    expect(article).not_to be_published
  end
end
```

**14.** Give different names to the subject, when you reassign subject with different attributes in different contexts, so it's easier to see what the actual subject represents.

```ruby
# bad
describe Article do
  context 'when there is an author' do
    subject(:article) { FactoryBot.create(:article, author: user) }

    it 'shows other articles by the same author' do
      expect(article.related_stories).to include(story1, story2)
    end
  end

  context 'when the author is anonymous' do
    subject(:article) { FactoryBot.create(:article, author: nil) }

    it 'matches stories by title' do
      expect(article.related_stories).to include(story3, story4)
    end
  end
end

# good
describe Article do
  context 'when article has an author' do
    subject(:article) { FactoryBot.create(:article, author: user) }

    it 'shows other articles by the same author' do
      expect(article.related_stories).to include(story1, story2)
    end
  end

  context 'when the author is anonymous' do
    subject(:guest_article) { FactoryBot.create(:article, author: nil) }

    it 'matches stories by title' do
      expect(guest_article.related_stories).to include(story3, story4)
    end
  end
end
```
**15.** Don't stub methods of the object under test, it's a code smell and often indicates a bad design of the object itself.

```ruby
# bad
describe 'Article' do
  subject(:article) { Article.new }

  it 'indicates that the author is unknown' do
    allow(article).to receive(:author).and_return(nil)
    expect(article.description).to include('by an unknown author')
  end
end

# good - with correct subject initialization
describe 'Article' do
  subject(:article) { Article.new(author: nil) }

  it 'indicates that the author is unknown' do
    expect(article.description).to include('by an unknown author')
  end
end

# good - with better object design
describe 'Article' do
  subject(:presenter) { ArticlePresenter.new(article) }
  let(:article) { Article.new }

  it 'indicates that the author is unknown' do
    allow(article).to receive(:author).and_return(nil)
    expect(presenter.description).to include('by an unknown author')
  end
end
```

**16.** Use `let` and `let!` blocks instead of assigning values to instance variables in `before` blocks.

```ruby
# bad
describe '#type_id' do
  before do
    @resource = FactoryBot.create(:device)
    @type = Type.find(@resource.type_id)
  end

  it 'sets the type_id field' do
    expect(@resource.type_id).to equal(@type.id)
  end
end

# good
describe '#type_id' do
  let(:resource) { FactoryBot.create(:device) }
  let(:type) { Type.find resource.type_id }

  it 'sets the type_id field' do
    expect(resource.type_id).to equal(type.id)
  end
end
```

**17.** Use `specify` if the example doesn't have a description, use `it` for examples with descriptions.

```ruby
# bad
it do
  # ...
end

specify 'it sends an email' do
  # ...
end

specify { is_expected.to be_truthy }

it '#do_something is deprecated` do
  ...
end

# good
specify do
  # ...
end

it 'sends an email' do
  # ...
end

it { is_expected.to be_truthy }

specify '#do_something is deprecated` do
  ...
end
```

**18.** Do not write iterators to generate tests.

```ruby
# bad
[:new, :show, :index].each do |action|
  it 'returns 200' do
    get action
    expect(response).to be_ok
  end
end

# good - more verbose, but better for the future development
describe 'GET new' do
  it 'returns 200' do
    get :new
    expect(response).to be_ok
  end
end

describe 'GET show' do
  it 'returns 200' do
    get :show
    expect(response).to be_ok
  end
end

describe 'GET index' do
  it 'returns 200' do
    get :index
    expect(response).to be_ok
  end
end
```

**19.** Use shared examples to reduce code duplication.

```ruby
# bad
describe 'GET /articles' do
  let(:article) { FactoryBot.create(:article, owner: owner) }

  before { page.driver.get '/articles' }

  context 'when user is the owner' do
    let(:user) { owner }

    it 'shows all owned articles' do
      expect(page.status_code).to be(200)
      contains_resource resource
    end
  end

  context 'when user is an admin' do
    let(:user) { FactoryBot.create(:user, :admin) }

    it 'shows all resources' do
      expect(page.status_code).to be(200)
      contains_resource resource
    end
  end
end

# good
describe 'GET /articles' do
  let(:article) { FactoryBot.create(:article, owner: owner) }

  before { page.driver.get '/articles' }

  shared_examples 'shows articles' do
    it 'shows all related articles' do
      expect(page.status_code).to be(200)
      contains_resource resource
    end
  end

  context 'when user is the owner' do
    let(:user) { owner }

    include_examples 'shows articles'
  end

  context 'when user is an admin' do
    let(:user) { FactoryBot.create(:user, :admin) }

    include_examples 'shows articles'
  end
end

# good
describe 'GET /devices' do
  let(:resource) { FactoryBot.create(:device, created_from: user) }

  it_behaves_like 'a listable resource'
  it_behaves_like 'a paginable resource'
  it_behaves_like 'a searchable resource'
  it_behaves_like 'a filterable list'
end
```

**20.** Avoid incidental state as much as possible.

```ruby
# bad
it 'publishes the article' do
  article.publish

  # Creating another shared Article test object above would cause this
  # test to break
  expect(Article.count).to eq(2)
end

# good
it 'publishes the article' do
  expect { article.publish }.to change(Article, :count).by(1)
end
```

**21.** Use Factory Bot to create test data in integration tests. You should very rarely have to use ModelName.create within an integration spec. Do not use fixtures as they are not nearly as maintainable as factories.

```ruby
# bad
subject(:article) do
  Article.create(
    title: 'Piccolina',
    author: 'John Archer',
    published_at: '17 August 2172',
    approved: true
  )
end

# good
subject(:article) { FactoryBot.create(:article) }
```

**22.** Do not load more data than needed to test your code.

```ruby
# good
RSpec.describe User do
  describe ".top" do
    subject { described_class.top(2) }

    before { FactoryBot.create_list(:user, 3) }

    it { is_expected.to have(2).items }
  end
end
```

**23.** Always use `Timecop` instead of stubbing anything on Time or Date.

```ruby
# bad
it 'offsets the time 2 days into the future' do
  current_time = Time.now
  allow(Time).to receive(:now).and_return(current_time)
  expect(subject.get_offset_time).to eq(current_time + 2.days)
end

# good
it 'offsets the time 2 days into the future' do
  Timecop.freeze(Time.now) do
    expect(subject.get_offset_time).to eq 2.days.from_now
  end
end
```

**24.** `context` block descriptions should always start with 'when', 'with', or 'without' and be in the form of a sentence with proper grammar when composed with `it` block descriptions.

```ruby
# bad
context 'the display name not present' do
  # ...
end

# good
context 'when the display name is not present' do
  it 'raises an error' do
    # ...
  end
end

--------------------------------------

#bad
context 'the display name has utf8 characters' do
  # ...
end

#good
context 'with utf8 characters in the display name' do
  # ...
end

--------------------------------------

#bad
context 'the display name has no utf8 characters' do
  # ...
end

# good
context 'without utf8 characters in the display name' do
  # ...
end
```

**25.** `it`/`specify` block descriptions should never end with a conditional. This is a code smell that the `it` most likely needs to be wrapped in a `context`.

```ruby
# bad
it 'returns the display name if it is present' do
  # ...
end

# good
context 'when display name is present' do
  it 'returns the display name' do
    # ...
  end
end
```

**26.** Keep example description shorter than 60 characters.

```ruby
# bad
it 'rewrites "should not return something" as "does not return something"' do
  # ...
end

# good
it 'rewrites "should not return something"' do
  expect(rewrite('should not return something')).to
    eq 'does not return something'
end

# good - self-documenting
specify do
  expect(rewrite('should not return something')).to
    eq 'does not return something'
end
```

**27.** Prefix describe description with a hash for instance methods, with a dot for class methods.

```ruby
class Article
  def summary
    # ...
  end

  def self.latest
    # ...
  end
end

# bad
describe Article do
  describe 'summary' do
    #...
  end

  describe 'latest' do
    #...
  end
end

# good
describe Article do
  describe '#summary' do
    #...
  end

  describe '.latest' do
    #...
  end
end
```

**28.** Do not write 'should' or 'should not' in the beginning of your example docstrings.

```ruby
# bad
it 'should return the summary' do
  # ...
end

# good
it 'returns the summary' do
  # ...
end
```

**29.** Be clear about what method you are describing. Use the Ruby documentation convention of `.` when referring to a class method's name and `#` when referring to an instance method's name.

```ruby
# bad
describe 'the authenticate method for User' do
  # ...
end

# good
describe '.authenticate' do
  # ...
end

--------------------------------------

#bad
describe '#admin?' do
  # ...
end

#good
describe 'if the user is an admin' do
  # ...
end
```

**30.** On new projects always use the new `expect` syntax.
```ruby
# bad
it 'creates a resource' do
  response.should respond_with_content_type(:json)
end

# good
it 'creates a resource' do
  expect(response).to respond_with_content_type(:json)
end
```

**31.** Use RSpec's predicate matcher methods when possible.

```ruby
# bad
it 'is published' do
  expect(subject.published?).to be true
end

# good
it 'is published' do
  expect(subject).to be_published
end
```

**32.** Use built-in matchers.

```ruby
# bad
it 'includes a title' do
  expect(article.title.include?('a lengthy title')).to be true
end

# good
it 'includes a title' do
  expect(article.title).to include 'a lengthy title'
end
```

**33.** Avoid using `be` matcher without arguments. It is too generic, as it pass on everything that is not `nil` or `false`. If that is the exact intend, use `be_truthy`.

```ruby
# bad
it 'has author' do
  expect(article.author).to be
end

# good
it 'has author' do
  expect(article.author).to be_truthy # same as the original
  expect(article.author).not_to be_nil # `be` is often used to check for non-nil value
  expect(article.author).to be_an(Author) # explicit check for the type of the value
end
```

**34.** Extract frequently used common logic from your examples into custom matchers.

```ruby
# bad
it 'returns JSON with temperature in Celsius' do
  json = JSON.parse(response.body).with_indifferent_access
  expect(json[:celsius]).to eq 30
end

# good
it 'returns JSON with temperature in Celsius' do
  expect(response).to include_json(celsius: 30)
end

--------------------------------------

#bad
it 'returns JSON with temperature in Farenheit' do
  json = JSON.parse(response.body).with_indifferent_access
  expect(json[:farenheit]).to eq 86
end

it 'returns JSON with temperature in Farenheit' do
  expect(response).to include_json(farenheit: 86)
end
```

**35.** Avoid using `allow_any_instance_of`/`expect_any_instance_of`.

```ruby
# bad
it 'has a name' do
  allow_any_instance_of(User).to receive(:name).and_return('Tweedledee')
  expect(account.name).to eq 'Tweedledee'
end

# good
let(:account) { Account.new(user) }

it 'has a name' do
  allow(user).to receive(:name).and_return('Tweedledee')
  expect(account.name).to eq 'Tweedledee'
end
```

**36.** The directory structure of the view specs `spec/views` matches the one in `app/views`. For example the specs for the views in `app/views/users` are placed in `spec/views/users`.

**37.** Testing the controller should not depend on the model creation.

**38.** Create the model for all examples in the spec to avoid duplication.

```ruby
#good
describe Article do
  let(:article) { FactoryBot.create(:article) }
end
```

**39.** Add an example ensuring that the FactoryBot.created model is valid.

```ruby
describe Article do
  it 'is valid with valid attributes' do
    expect(article).to be_valid
  end
end
```

**40.** Add a separate `describe` for each attribute which has validations.

```ruby
#good
describe '#title' do
  it 'is required' do
    article.title = nil
    article.valid?
    expect(article.errors[:title].size).to eq(1)
  end
end

describe '#name' do
  it 'is required' do
    article.name = nil
    article.valid?
    expect(article.errors[:name].size).to eq(1)
  end
end
```

**41.** When testing uniqueness of a model attribute, name the other object `another_object`.

**42.** The mailer spec should verify that:

- the subject is correct
- the receiver e-mail is correct
- the e-mail is sent to the right e-mail address
- the e-mail contains the required information

```ruby
describe SubscriberMailer do
  let(:subscriber) { double(Subscription, email: 'johndoe@test.com', name: 'John Doe') }

  describe 'successful registration email' do
    subject { SubscriptionMailer.successful_registration_email(subscriber) }

    its(:subject) { should == 'Successful Registration!' }
    its(:from) { should == ['info@your_site.com'] }
    its(:to) { should == [subscriber.email] }

    it 'contains the subscriber name' do
      expect(subject.body.encoded).to match(subscriber.name)
    end
  end
end
```

**43.** Run specific test suites

```ruby
rspec spec/models/demand_spec.rb:30
```

**44.** Debug Capybara tests with save_and_open_page
```ruby
#good
it 'should register successfully' do
  visit registration_page
  save_and_open_page
  fill_in 'username', :with => 'abinoda'
end
```

**45.** Only enable JS in Capybara when necessary

```ruby
# only use js => true when your tests depend on it
it 'should register successfully', :js => true do
  visit registration_page
  fill_in 'username', :with => 'abinoda'
end
```

**46.** Always specify test type(Rails)

```ruby
#bad
RSpec.describe RobotsController do
  it 'responses success' do
    get :index

    expect(response).to be_success
  end
end

#good
RSpec.describe RobotsController, type: :controller do
  it 'responses success' do
    get :index

    expect(response).to be_success
  end
end
```

**47.** -   When something in your application goes wrong, write a test that reproduces the error and then correct it. You will gain several hour of sleep and more serenity.

**48.** Keep your description short.
```ruby
#bad
it 'has 422 status code if an unexpected params will be added'  do

#good
context 'when not valid'  do it { is_expected.to respond_with 422 } end
```

**49.** Test all possible cases.

```ruby
#description
before_filter :find_owned_resources
before_filter :find_resource

def destroy
  render 'show'
  @consumption.destroy
end

#bad
it 'shows the resource'

#good
describe '#destroy' do

  context 'when resource is found' do
    it 'responds with 200'
    it 'shows the resource'
  end

  context 'when resource is not found' do
    it 'responds with 404'
  end

  context 'when resource is not owned' do
    it 'responds with 404'
  end
end
```
