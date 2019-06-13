---
description: Promises + Testing w/ RSPEC
---

# Day 02

What we covered today:

* [JS Promises](https://github.com/wofockham/sei-31/tree/master/13-advanced/promises)
* [RSPEC on Rails](https://github.com/wofockham/sei-31/tree/master/09-tdd/fruitstore)
* YouTeach
  * Jin: JS Prototypes
  * Mabeth: Selenium

Warmup

* [Text Soup 3](https://github.com/Yiannimoustakas/sei31-homework/tree/master/warmups/week11/day02_text_soup_3)

## Ruby on Rails - Testing - Rspec <a id="ruby-on-rails-testing-rspec"></a>

* ​[Rspec-rails documentation](https://github.com/rspec/rspec-rails)​

We have done a little bit of Rspec now, but it has all been with pure Ruby, no Rails. We want to get it working with Rails, and luckily, that isn't difficult.

### Set up a Rails project <a id="set-up-a-rails-project"></a>

Start by creating a new Rails project, using the `-T` option to tell Rails _not_ to generate tests for us.

```bash
rails new fruitstore -T --skip-git
cd fruitstoreatom .
```

Then, include the Rspec gem in the test and development group in our **GEMFILE**:

```bash
gem 'rspec-rails'
```

Then bundle the gems.

```bash
bundle
```

### Install Rspec in the project <a id="install-rspec-in-the-project"></a>

This still hasn't installed Rspec in our project. To do that, run the following command from Terminal:

```bash
rails generate rspec:install
```

This does a bunch of things, it generates us a spec folder \(where all of our tests will live\), but it also changes the behaviour for other rails generators. For example, when you generate a model, the generator will now add a test for that model. Same for controllers, etc, generated using a generator.

### Create a model <a id="create-a-model"></a>

```bash
rails generate model Fruit
rails db:migrate
```

Go back to the terminal and run `rspec` and you will see it is now connected and working.

We're going to create a single string column for the name attribute of a Fruit object.

```ruby
rails generate migration add_name_to_fruits name:string
rails db:migrate
```

#### _Testing Single Table Inheritance_ <a id="testing-single-table-inheritance"></a>

The whole purpose of this project is to make it easy to upload Fruits, whether they're apples or pears. We could have multiple tables for these things, but we don't have to. We can use a thing called Single Table Inheritance \(S.T.I\). This is where a model \(that doesn't have a table\) inherits from another model \(that has a database table associated with it\). It looks something like the following:

```ruby
# In app/models/fruit.rb
class Fruit < ActiveRecord::Base
end
​
# In app/models/apple.rb
class Apple < Fruit
end
​
# In app/models/pear.rb
class Pear < Fruit
end
```

Because Pear and Apple inherit from a Fruit, and Fruit inherits from ActiveRecord::Base, Pear and Apple also inherit from ActiveRecord::Base.

This structure of inheritance means that we can create Fruits, Apples or Pears and it still just makes a record in the Fruits table. Example:

```ruby
pear = Pear.new
apple = Apple.new
fruit = Fruit.new
```

First, thought, we need to a `type` column to our fruits table.

```ruby
rails generate migration add_type_to_fruits type:string
rails db:migrate
```

To test this out we could have a block in our Rspec tests.

```ruby
Rspec.describe Shelf, :type => :model do
  describe "A pear" do
    before do
      @pear = Pear.create :name => "Nashi"
    end
​
    it "should remember the class via Single Table Inheritance (S.T.I)" do
      pear = Fruit.find( @pear.id )
      expect( pear ).to_not be_nil
      expect( pear.class ).to eq Pear
      expect( pear ).to eq @pear
      expect( pear.is_a?( Fruit ) ).to be true
      expect( pear.class.ancestors ).to include Fruit
    end
  end
end
```

And that should be all sorted! Single Table Inheritance doesn't regularly come up so if it is seeming a bit weird, don't worry about it. Neither Joel nor I have ever used it professionally. But it is cool to show how to test that sort of stuff, and it shows why type is a reserved column name in a database.

#### _Testing Associations_ <a id="testing-associations"></a>

One thing that you might be interested in, is testing associations in Rails. Unfortunately Rspec doesn't have something that automates this. To test associations, use a gem called [Shoulda Matchers](https://github.com/thoughtbot/shoulda-matchers).

Add the following to the test environment block in your **GEMFILE**:

```bash
gem 'shoulda-matchers'
```

Then bundle your gems:

```bash
bundle
```

Now you need to tell the gem a couple of things:

* Which test framework you're using
* Which portion of the matchers you want to use

You can supply this information by using a configuration block. Place the following in `rails_helper.rb`:

```ruby
Shoulda::Matchers.configure do |config|
  config.integrate do |with|
    with.test_framework :rspec
    with.library :rails
  end
end
```

Now let's create a shelf table for our Fruit, and set up the associations

```ruby
rails generate model Shelf
rails db:migrate
```

Now, in **app/models/shelf.rb**:

```ruby
class Shelf < ActiveRecord::Base
  has_many :fruits
end
```

And, in **app/models/fruit.rb**:

```ruby
class Fruit < ActiveRecord::Base
  belongs_to :shelf, :optional => true
end
```

To test these associations...

```ruby
# Put this at the top of the thing describing fruits
it { should belong_to :shelf }
​
# Put this at the top of the thing describing shelves
it { should have_many :fruits }
```

### Testing controllers <a id="testing-controllers"></a>

We have only tested models so far, so lets figure out how to test controllers. We need to add another gem for controller testing:

* ​[rails-controller-testing](https://github.com/rails/rails-controller-testing)​

```bash
gem 'rails-controller-testing'
```

```bash
bundle
```

Let's generate a fruits controller and set up our routes:

```bash
rails generate controller Fruits
```

Now, in our **config/routes.rb** file, add restful resources for the fruits controller:

```ruby
resources :fruits, only => [:index]
```

The next error that pops up will be that the action doesn't exist, so lets add an index method in the **app/controllers/fruits\_controllerrb**:

```ruby
class FruitsController < ApplicationController
  def index
  end
end
```

After that, we will have a missing template \(or view\), so let's make that file as well.

```bash
touch app/assets/views/fruits/index.html.erb
```

Now lets add some tests to our **fruits\_controller\_spec.rb** file.

```ruby
RSpec.describe FruitsController, type: :controller do
  describe "GET to index" do
    before do
      3.times { |i| Fruit.create :name => "Fruit number #{ i }" }
    end
​
    describe "As HTML" do
      before do
        get :index # Fake browser
      end
​
      it "should respond with a status of 200" do
        expect( response ).to be_success
        expect( response.status ).to eq( 200 )
      end
​
      it "should respond with @fruits that contains all fruits in reverse order" do
        # We need to do a bit of work in the controllers for this.  Add @fruits = Fruit.all.reverse into the index method.
​
        # Remember that this all runs of the before block.
​
        # Checks existence
        expect( assigns( :fruits ) ).to be
​
        # Checks how many things are in @fruits
        expect( assigns( :fruits ).length ).to eq 3
​
        # Make sure the class is correct
        expect( assigns( :fruits ).first.class ).to eq( Fruit )
​
        # Validates whether it is in reverse order.
        expect( assigns( :fruits ).first.name ).to eq( "Fruit number 2" )
      end
​
      it "should render the index view" do
        expect( response ).to render_template( :index )
      end
    end
  end
end
```

#### _Testing JSON responses_ <a id="testing-json-responses"></a>

That will work for normal HTML, but what if we wanted to test JSON responses? Lets add some tests.

```ruby
describe "As JSON" do
  before do
    get :index, :format => :json
  end
​
  it "should response with a status 200" do
    expect( response ).to be_success
    expect( response.status ).to eq(200)
  end
​
  it "should have the right content type" do
    expect( response.content_type ).to eq( "application/json" )
  end
​
  it "should include the fruit data in JSON format" do
    # response.body returns a string so we need to turn it into an actual object
    fruit = JSON.parse( response.body )
    expect( fruits.length ).to eq( 3 )
    expect( fruits.first['name'] ).to eq( "Fruit number 2" )
  end
end
```

In the index method in **app/controllers/fruits\_controller.rb**, lets add this

```ruby
def index
  @fruits = Fruit.all.reverse
  # @fruits = Fruit.order('id DESC') will actually use less memory as it doesn't need to create a duplicate version of the results and reverse it.
  respond_to do |format|
    format.html {}
    format.json { render :json => @fruits }
  end
end
```

That should work out the errors!

_A Bit more on Running Rspec_

* `rspec` - Run all tests
* `rspec spec/models` - Run all model tests
* `rspec spec/controllers` - Run all controller tests
* `rspec -e "The description of the test you want to run"` - Tell it an individual test to run.

### Rspec terminal commands <a id="rspec-terminal-commands"></a>

Here are a few of the common ones:

```ruby
rspec # Run all tests
rspec spec/models  # Run all model tests
rspec spec/controllers  # Run all controller tests
rspec -e "The description of the test you want to run"  # Tell Rspec to run a particular test
```

### _Ruby on Rails - Testing - Rspec - Further Reading_ <a id="ruby-on-rails-testing-rspec-further-reading"></a>

* ​[Rspec](http://rspec.info/)​
* ​[Rspec Rails - Documentation](http://rspec.info/documentation/3.5/rspec-rails/)​
* ​[Better Specs](http://betterspecs.org/)​
* ​[Team Treehouse - An Introduction to Rspec](http://blog.teamtreehouse.com/an-introduction-to-rspec)​
* [How to Test Rails Models with RSpec](https://semaphoreci.com/community/tutorials/how-to-test-rails-models-with-rspec)
* [9 Benefits of Test Driven Development](https://www.madetech.com/blog/9-benefits-of-test-driven-development)
* [Why TDD](https://builttoadapt.io/why-tdd-489fdcdda05e)

