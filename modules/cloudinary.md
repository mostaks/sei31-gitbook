# Cloudinary

## What is it? <a id="what-is-it"></a>

Cloudinary is cloud-based image and video management system. It includes:

* Storage
* Upload
* "Powerful Administration"
* Image Manipulation
* A fast CDN

While it's primarily used for image and video management, you can also use it to store PDFs, MS Office documents and a bunch of other file types.

It's used by companies like Gizmodo, GQ, Answers, Redbull, Ebay, etc.

See [cloudinary.com](http://cloudinary.com/) for more.

## How to get it up and running <a id="how-to-get-it-up-and-running"></a>

### Set up a new Rails project <a id="set-up-a-new-rails-project"></a>

Create a new Rails project:

```bash
rails new badger-vision -d postgresql
```

Generate models and migrations

```text
rails generate model Animal name:string image:string
```

Create the database and rake the migrations

```text
rails db:create
rails db:migrate
```

Add the Cloudinary Gem to your Gemfile

```text
gem 'cloudinary'
```

Bundle your Gems

```text
bundle
```

### Create a new Heroku app <a id="create-a-new-heroku-app"></a>

```text
heroku create badger-vision -d postgresql
```

### Sign up for Cloudinary <a id="sign-up-for-cloudinary"></a>

Create an account at [cloudinary.com](https://cloudinary.com/users/register/free)​

### Download your cloudinary.yml <a id="download-your-cloudinary-yml"></a>

When sending requests to Cloudinary, you need to identify yourselves to Cloudinary. You do this using API keys and credentials. Cloudinary helpfully provides a user-specific .yml file with all this information for you to include in your application.

Once you've signed up for Cloudinary and logged in to your account:

* Download [http://cloudinary.com/console/cloudinary.yml](http://cloudinary.com/console/cloudinary.yml)​
* Save this file to config/cloudinary.yml in your Rails project.
* **Important: DO NOT GIT ADD \| GIT COMMIT \| GIT PUSH THIS ADDITIONAL FILE JUST YET. First, we need to secure our credentials**. I'll tell you when it's safe to commit.

### Secure your credentials <a id="secure-your-credentials"></a>

Your **cloudinary.yml** should look something like this:

```yaml
---
development:
  cloud_name: badger
  api_key: '873487208740863'
  api_secret: oDB-9UsU3g3KGs-gIue78f-jdiE
  enhance_image_tag: true
  static_image_support: false
production:
  cloud_name: badger
  api_key: '873487208740863'
  api_secret: oDB-9UsU3g3KGs-gIue78f-jdiE
  enhance_image_tag: true
  static_image_support: true
test:
  cloud_name: badger
  api_key: '873487208740863'
  api_secret: oDB-9UsU3g3KGs-gIue78f-jdiE
  enhance_image_tag: true
  static_image_support: false
```

There is a fairly gigantic problem with storing API keys and credentials directly in your your config files or anywhere else - if your project is on GitHub, and your GitHub repo is public, those credentials will be exposed to the world. With some APIs, this can be _completely fucked up_ - you run the risk of people stealing your credentials and using them to rack up enormous bills at your expense.

To prevent this from happening, you should always store credentials and API keys in environment variables, or in secrets.yml \(or another file that will not be added to Git\). Using other hosts, we might be able to store our credentials in our secrets.yml and then add that file to .gitignore, but this won't work on Heroku, since we can only deploy to Heroku by git push-ing to our Heroku remote.

Let's still store our credentials in our **config/secrets.yml** file, though - this is a good habit to get into, and will remind us where we can go to set/retrieve our credentials.

#### _Add your Cloudinary credentials as development environment variables_ <a id="add-your-cloudinary-credentials-as-development-environment-variables"></a>

We need to remove our sensitive API information from our config/cloudinary.yml file and add those to our development environment. Before you sanitize the cloudinary.yml file, configure your production and development environments with your Cloudinary credentials.

Open your bash profile.

```text
atom ~/.bash_profile
```

Add the following to your bash profile

```bash
# In .bash_profile
 export CLOUDINARY_CLOUD_NAME=badger
 export CLOUDINARY_API_KEY='873487208740863'
 export CLOUDINARY_API_SECRET=oDB-9UsU3g3KGs-gIue78f-jdiE
```

Since these are now part of your bash profile, you don't need to repeat this for future applications.

To confirm that this worked, **restart Terminal** and run the following command.

```bash
ENV | grep CLOUDINARY
# => CLOUDINARY_CLOUD_NAME=badger
# => CLOUDINARY_API_KEY='873487208740863'
# => CLOUDINARY_API_SECRET=oDB-9UsU3g3KGs-gIue78f-jdiE
```

#### _Add your Cloudinary credentials as production environment variables_ <a id="add-your-cloudinary-credentials-as-production-environment-variables"></a>

Now you need to add our credentials to our production environment: Heroku. You can do this from the Heroku dashboard UI \(log in to Heroku, click on your app, click 'Settings', then click 'Reveal Config Variables' and add each environment variable as a key:value pair\), or from the command line using the Heroku CLI - let's do it from the command line. In the root directory of your git repo, run the following:

```bash
heroku config:set --app badger-vision CLOUDINARY_CLOUD_NAME=badger
heroku config:set --app badger-vision CLOUDINARY_API_KEY='873487208740863'
heroku config:set --app badger-vision CLOUDINARY_API_SECRET=oDB-9UsU3g3KGs-gIue78f-jdiE
```

#### _Add references to environment variables to your config/secrets.yml file_ <a id="add-references-to-environment-variables-to-your-config-secrets-yml-file"></a>

```yaml
development:
    cloudinary_cloud_name: <%= ENV["CLOUDINARY_CLOUD_NAME"] %>
    cloudinary_api_key: <%= ENV["CLOUDINARY_API_KEY"] %>
    cloudinary_api_secret: <%= ENV["CLOUDINARY_API_SECRET"] %>
test:
    cloudinary_cloud_name: <%= ENV["CLOUDINARY_CLOUD_NAME"] %>
    cloudinary_api_key: <%= ENV["CLOUDINARY_API_KEY"] %>
    cloudinary_api_secret: <%= ENV["CLOUDINARY_API_SECRET"] %>
production:
    cloudinary_cloud_name: <%= ENV["CLOUDINARY_CLOUD_NAME"] %>
    cloudinary_api_key: <%= ENV["CLOUDINARY_API_KEY"] %>
    cloudinary_api_secret: <%= ENV["CLOUDINARY_API_SECRET"] %>
```

#### _Sanitize your cloudinary.yml_ <a id="sanitize-your-cloudinary-yml"></a>

Replace the bare references to Cloudinary credentials in your cloudinary.yml with references to your **config/secrets.yml** file. Open up your **config/cloudinary.yml** and change it as follows:

```yaml
---
development:
  cloud_name: Rails.application.secrets.cloudinary_cloud_name
  api_key: Rails.application.secrets.cloudinary_api_key
  api_secret: Rails.application.secrets.cloudinary_api_secret
  enhance_image_tag: true
  static_image_support: false
production:
  cloud_name: Rails.application.secrets.cloudinary_cloud_name
  api_key: Rails.application.secrets.cloudinary_api_key
  api_secret: Rails.application.secrets.cloudinary_api_secret
  enhance_image_tag: true
  static_image_support: true
test:
  cloud_name: Rails.application.secrets.cloudinary_cloud_name
  api_key: Rails.application.secrets.cloudinary_api_key
  api_secret: Rails.application.secrets.cloudinary_api_secret
  enhance_image_tag: true
  static_image_support: false
```

Now, `Rails.application.secrets.cloudinary_api_key` \(etc\) will return `<%=ENV["CLOUDINARY_API_KEY"] %>`, which in turn tells Rails to go and look for the variable "CLOUDINARY\_API\_KEY" in the current environment \(ie, Heroku for production and our bash shell for development\).

This might seem unnecessarily circuitous, referencing our secrets.yml file, which references an environment variable - and it totally is. You could have easily put the references to the environment variables directly in our cloudinary.yml, but it's a good habit to get into using secrets. It also means you can check one file \(secrets.yml\) to see what environment variables and credentials have been configured in the application.

### Okay, it's save to commit now. <a id="okay-its-save-to-commit-now"></a>

## Set up your routes, controller and views <a id="set-up-your-routes-controller-and-views"></a>

### Create an animals controller <a id="create-an-animals-controller"></a>

```bash
rails generate controller animals
```

### Create animals views <a id="create-animals-views"></a>

```bash
touch app/views/animals/index.html.erb
touch app/views/animals/new.html.erb
touch app/views/animals/show.html.erb
touch app/views/animals/edit.html.erb
```

### Create resources for animals <a id="create-resources-for-animals"></a>

In **config/routes.rb**:

```ruby
root "animals#new"
resources :animals
```

## Add the Cloudinary JS Config reference to your layout <a id="add-the-cloudinary-js-config-reference-to-your-layout"></a>

In your **app/views/layouts/application.html.erb**, add the following to the HTML's `<head>`:

```ruby
<%= cloudinary_js_config %>
```

## Configure your controller and views <a id="configure-your-controller-and-views"></a>

The controller and view setup required for single image uploads is slightly different to the setup required for allowing multiple images to be uploaded using a single form. Both are covered below.

### Single image, client-side uploads <a id="single-image-client-side-uploads"></a>

Below is the controller and view setup required for allowing users to upload a single image at a time from the browser.

#### _Controller configuration for single image uploads_ <a id="controller-configuration-for-single-image-uploads"></a>

Set up your controller actions in **app/controllers/animals\_controller.rb**:

```ruby
def AnimalsController < ApplicationController
  def index
    @animals = Animal.all
  end
​
  def show
    @animal = Animal.find(params[:id])
  end
​
  def new
    @animal = Animal.new
  end
​
  def create
    animal = Animal.new(animal_params)
    # This is the magic stuff that will let us upload an image to Cloudinary when 
    # creating a new animal.
    # First, check to see if the user has attached an image for uploading
    if params[:file].present?
      # Then call Cloudinary's upload method, passing in the file in params
      req = Cloudinary::Uploader.upload(params[:file])
      # Using the public_id allows us to use Cloudinary's powerful image 
      # transformation methods.
      animal.image = req["public_id"]
      animal.save
    end
    redirect_to animal_path(animal)
  end
​
  def edit
    @animal = Animal.find(params[:id])
  end
​
  def update
    animal = Animal.find(params[:id])
    if params[:file].present?
      req = Cloudinary::Uploader.upload(params[:file])
      animal.image = req["public_id"]
    end
    # We're using update_attributes here because we don't want to make a PUT request 
    # (.update to update the attributes in animal_params, then .save to update the 
    # image)
    animal.update_attributes(animal_params)
    animal.save
    redirect_to(animal_path(animal))
  end
​
  private
​
  def animal_params
    # Note that we don't whitelist the image, because the image is not passed to the 
    # controller in the animal object produced by the new/edit form.
    params.require(:animal).permit(:name)
  end
end
```

#### _Views configuration for single image uploads_ <a id="views-configuration-for-single-image-uploads"></a>

And then complete the views - instead of using the ordinary `<input type='text>`/ `<%= f.text_field %>` and `<img>` / `<%= image_tag %>` tags we use for images on the web, we'll be using Cloudinary's `<%= f.cl_image_upload %>` helper \(or `<%= cl_image_upload_tag%>`\) to upload the image and `<%= cl_image_tag %>` to display the image:

```ruby
<!-- In app/views/animals/index.html.erb -->
  <% @animals.each do |animal| %>
      <%=link_to(animal.name, animal) %>
  <% end %>
​
<!-- In app/views/animals/new.html.erb ` -->
  <%= form_for(@animal, :html => {:multipart => true}) do |f| %>
    <%= f.label "Animal Name: " %>
    <%= f.text_field :name %>
​
    <%= f.label "Animal Image: " %>
    <%= f.cl_image_upload :image %>
​
    <%= f.submit "Create Animal" %>
  <% end %>
​
<!-- In app/views/animals/edit.html.erb ` -->
  <%= form_for(@animal, :html => {:multipart => true}) do |f| %>
    <%= f.label "Animal Name: " %>
    <%= f.text_field :name %>
​
    <%= f.label "Animal Image: " %>
    <%= f.cl_image_upload :image %>
​
    <%= f.submit "Edit Animal" %>
  <% end %>
​
<!-- In app/views/animals/show.html.erb -->
  <h1><%= @animal.name %></h1>
  <%= cl_image_tag(@animal.image) %>
  <!-- the cl_image_tag allows you to do a bunch of cool stuff, like ask cloudinary 
  to serve the image with specific dimensions, or effects like sepia, black and white,
   etc. Check out the cloudinary docs on image transformation.  -->
```

### Multiple image, client-side uploads <a id="multiple-image-client-side-uploads"></a>

If we're storing multiple images for a single entity in our database, we need to take a slightly different approach - we can't just have a simple, standard 'image' column with datatype 'string'. We would either need to create a `Photo` model, with a `has_many` / `belongs_to` relationship with our `Animal` model, or we could use Rails' `serialize` / `store` methods, _or_ we could \(with PostgreSQL databases in Rails ~&gt;= 4\) use the `array` ActiveRecord method - this example uses the latter approach.

#### _Migration to change :image to an array_ <a id="migration-to-change-image-to-an-array"></a>

This migration will allow us to set our Animal's `image` column to be an array \(of Cloudinary images\). To be semantically accurate, let's also change the column from `image` to `images`.

```bash
rails generate migration ChangeAnimalImageToArray
```

The actual migration should look something like this:

```ruby
class ChangeAnimalImageToArray < ActiveRecord::Migration
  def change
    remove_column :animals, :image
    add_column :animals, :images, :text, :array => true, :default => []
  end
end
```

The animals table in our **db/schema.rb** file should look something like this:

```ruby
create_table "animals", force: :cascade do |t|
  t.string   "name"
  t.datetime "created_at",   null: false
  t.datetime "updated_at",   null: false
  t.text     "images",       default: [],     array: true
end
```

#### _Controller configuration for multiple image uploads_ <a id="controller-configuration-for-multiple-image-uploads"></a>

Set up your controller actions in **app/controllers/animals\_controller.rb**:

```ruby
class AnimalsController < ApplicationController
  def index
    @animals = Animal.all
  end
​
  def show
    @animal = Animal.find(params[:id])
  end
​
  def new
    @animal = Animal.new
  end
​
  def create
    animal = Animal.new(animal_params)
    # This is the magic stuff that will let us upload an image to Cloudinary when 
    # creating a new animal.
    if params[:animal][:images].present?
      params[:animal][:images].each do |image|
        req = Cloudinary::Uploader.upload(image)
        animal.images << req["public_id"]
      end
    end
    animal.save
    redirect_to animal_path(animal)
  end
​
  def edit
    @animal = Animal.find(params[:id])
  end
​
  def update
    animal = Animal.find(params[:id])
    if params[:animal][:images].present?
      params[:animal][:images].each do |image|
        req = Cloudinary::Uploader.upload(image)
        animal.images << req["public_id"]
      end
    end
    animal.update_attributes(animal_params)
    animal.save
    redirect_to(animal_path(animal))
  end
​
  private
​
  def animal_params
    params.require(:animal).permit(:name)
  end
end
```

#### _View configuration for multiple image uploads_ <a id="view-configuration-for-multiple-image-uploads"></a>

And then complete the views - we can't use Cloudinary's `<%= cl_image_upload %>` tag for allowing users to upload multiple images in a single action. Instead, we need to use the `<%= f.file_field %>` helper \(or `<%= file_field_tag %>`\). As for the single image upload, we'll be using Cloudinary's `<%= cl_image_tag %>` to display the images:

```ruby
<!-- In app/views/animals/index.html.erb -->
<% @animals.each do |animal| %>
    <%=link_to(animal.name, animal) %>
<% end %>
​
<!-- In app/views/animals/new.html.erb ` -->
<%= form_for(@animal, multipart: true) do |f| %>
  <%= f.label "Animal Name: " %>
  <%= f.text_field :name %>
​
  <%= f.label "Animal Images: " %>
  <%= f.file_field :images, :multiple => true %>
​
  <%= f.submit "Create Animal." %>
<% end %>
​
<!-- In app/views/animals/edit.html.erb ` -->
<%= form_for(@animal, :html => {:multipart => true}) do |f| %>
  <%= f.label "Animal Name: " %>
  <%= f.text_field :name %>
​
  <%= f.label "Animal Images: " %>
  <%= f.file_field :images, :multiple => true %>
​
  <%= f.submit "Edit Animal" %>
<% end %>
​
<!-- In app/views/animals/show.html.erb -->
<h1><%= @animal.name %></h1>
<% @animal.images.each do |i| %>
  <%= cl_image_tag(i) %>
<% end %>
```

## You're good to go <a id="youre-good-to-go"></a>

This is a basic Cloudinary implementation. Check out the Cloudinary documentation on image transformations and a whole bunch of other stuff.

In particular, be aware that you can upload images in bulk via the Cloudinary dashboard, manage & rename them, etc. Good for setting up a bunch of site images to use in your seed file!

