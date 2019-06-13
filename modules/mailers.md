---
description: Rails Mailers with SendGrid
---

# Mailers

### Rails Mailers with SendGrid <a id="rails-mailers-with-sendgrid"></a>

#### Action Mailer <a id="action-mailer"></a>

Action Mailer is a Rails framework that allows your application to generate outgoing email and process incoming email. Mailer models generate emails in much the same way as Rails controllers render views.

#### Create a new Rails app <a id="create-a-new-rails-app"></a>

Start by creating a new Rails app \(using the postgresql database adaptor, since we want to deploy to Heroku\):

```text
rails new badgermail -d postgresql
cd badgermail
```

#### Initialize a new Git repository <a id="initialize-a-new-git-repository"></a>

```text
git init
git add .
git commit -m "New repo"
```

Now let's scaffold a User resource:

```text
rails generate scaffold User name:string email:string
rake db:create
rake db:migrate
```

#### Generate a mailer <a id="generate-a-mailer"></a>

Generate a new mailer, which we'll call 'UserMailer' \(you can call the mailer whatever you want\):

```text
rails generate mailer UserMailer
```

This will create all the following files in your Rails tree:

| File | Description | Rails MVC equivalent |
| :--- | :--- | :--- |
| app/mailers/user\_mailer.rb | Mailer | Controller |
| app/mailers/application\_mailer.rb | Application mailer | app/controllers/application\_controller.rb |
| app/views/user\_mailer | Our directory for email views to be sent from the application | Other app/views folders |
| app/views/layouts/mailer.text.erb | An email view wrapper template for email clients that do not allow receipt of HTML emails | app/layouts/application.html.erb |
| app/views/layouts/mailer.html.erb | An email view wrapper template for email clients that do allow receipt of HTML emails | app/layouts/application.html.erb |

#### Set up mailer actions <a id="set-up-mailer-actions"></a>

Just as we set up actions for page templates \(views\) in our controller, we need to set up actions for email views in our mailer. Let's set one up to welcome new users on sign-up. A basic use of the mailer model is to:

1. Set up any defaults to apply to all emails using this class;
2. Initialize any instance variables we want to be available to email templates;
3. Call the mail method.

```text
# In app/mailers/user_mailer.rb

default from: "mayor@badgertown.com"

def welcome(user)
  @user = user #@user will be whatever user we pass in to the 'welcome' method
  mail( :to => @user.email, :subject => "Welcome to Badgertown!", :cc => "mayor@badgertown.com" )
end
```

You can pass a number of options into the `mail` method, but for now let's keep it simple and just specify the recipient, bcc and subject-line for the email we want to send. See the [Rails Guides](http://guides.rubyonrails.org/action_mailer_basics.html#mailer-views) and [Ruby On Rails API docs](http://api.rubyonrails.org/classes/ActionMailer/Base.html#method-i-mail) for more on the options you can pass into the `mail` method.

#### Creating a view for your email <a id="creating-a-view-for-your-email"></a>

Now that we have our mailer action set up, let's create a template for our email. Rails will assume the view to be rendered in the email matches the name of the method in our user\_mailer.rb unless we specify otherwise in the `mail` method using the `template_path` and `template_name` options.

**Create the mailer views**

```text
touch app/views/user_mailer/welcome.html.erb
touch app/views/user_mailer/welcome.text.erb
```

**Populate the html template for email clients that accept html emails**

Our mailer.html.erb file will contain the basic template that wraps our emails, so our welcome.html.erb template will be pretty straightforward.

```text
<!-- in app/views/user_mailer/welcome.html.erb  -->

    <h1>Thanks for signing up, <%= @user.name %>!</h1>
    <p>You've taken your first step into a larger world.</p>
```

**Populate the text template for email clients that do not accept html emails**

```text
Thanks for signing up <%= @user.name %>!

You've taken your first step into a larger world.
```

#### Call the mailer from the controller <a id="call-the-mailer-from-the-controller"></a>

We want the 'welcome' email to be delivered when a new user is created. So, let's call the mailer from the _create_ action in our _users\_controller.rb_

```text
# In app/controllers/users_controller.rb

  def create
    @user = User.new(user_params)

    respond_to do |format|
      if @user.save
        # The line below is the only additional line we need in our Users controller.
        UserMailer.welcome(@user).deliver_now
        format.html { redirect_to @user, notice: 'User was successfully created.' }
        format.json { render :show, status: :created, location: @user }
      else
        format.html { render :new }
        format.json { render json: @user.errors, status: :unprocessable_entity }
      end
    end
  end
```

#### SendGrid <a id="sendgrid"></a>

**Why SendGrid?**

Our application still requires a server to handle the transmission of emails, but Heroku doesn't support SMTP. So, while we can host our application on Heroku, we need to use a third party SMTP server. We'll be using SendGrid for this - SendGrid's free plan allows you to send up to 12,000 emails per month at no charge.

**Sign up for SendGrid**

* Create an account at [sendgrid.com](https://sendgrid.com/).

**Determine how you want to authenticate with SendGrid**

SendGrid gives you three options for authenticating your identity when sending requests to their SMTP server:

1. Using your SendGrid login credentials - use your SendGrid credentials;
2. Using additional SendGrid user credentials - setting up additional credentials linked to your SendGrid account.
3. Using SendGrid API keys - setting up API keys linked to your SendGrid account. \(Read the docs for this\).

**This guide will assume you are using your SendGrid login credentials.**

**Using your SendGrid login credentials**

You don't need to do anything. Just remember your SendGrid username and password and configure them in your application and environments as set out below.

**Using additional SendGrid user credentials**

These can be used in exactly the same way as your SendGrid credentials, but allow you to set up multiple credentials \(eg, one set for each application, or one set for each developer on your team\). To do this:

* Complete the process described below, using your newly created credentials in place of your SendGrid username and password.

**Using SendGrid API keys**

If you want to use API keys - rather than a username and password - to authenticate with SendGrid

* Go to the [SendGrid Dashboard](https://app.sendgrid.com/).
* Click **Settings &gt; API Keys**.
* Click **Create API Key &gt; General API Key**.
* Complete the form, selecting 'Full Access' for _at least_ 'Mail Send'.
* In the process described below:
  * Enter the string `"apikey"` as the SendGrid username; and
  * Enter your API Key as the SendGrid password.

**Setting environment variables for our credentials.**

In order to use SendGrid, we need our application to send our SendGrid credentials with every request. To get our application to do this, we need to configure those credentials in each environment \(Production, Development, maybe Test, etc\)

**Development Environment**

Note: We _could_ have just set these in our mail.rb smtp settings \(setting username and password as `:user_name => ENV["SENDGRID_USERNAME"]` etc in that file, instead of `:user_name => Rails.application.secrets.sendgrid_username`\), but using secrets for credentials is a good habit to get into.

**Add your SendGrid Credentials as environment variables in Terminal**

Open your bash profile.

```text
atom ~/.bash_profile
```

Depending on whether you are using your credentials \(username and password\) or if you are using an API key, you must add the following to your bash profile.

```text
# In .bash_profile (in the case of using your username and password)
 export SENDGRID_USERNAME="mrbadger" (THIS SHOULD BE YOUR USERNAME)
 export SENDGRID_PASSWORD="chicken" (THIS SHOULD BE YOUR PASSWORD)
```

**OR**

```text
# In .bash_profile (in the case of using an API key)
 export SENDGRID_USERNAME="apikey" (literally, just string that says "apikey")
 export SENDGRID_PASSWORD="whatever_the_massive_api_key_is_goes_here"
```

Since these are now part of your bash profile, you don't need to repeat this for future applications.

To confirm that this worked, restart Terminal and run the following command.

```text
ENV | grep SENDGRID
# SENDGRID_USERNAME=mrbadger (THIS SHOULD BE YOUR USERNAME OR 'apikey')
# SENDGRID_PASSWORD=chicken (THIS SHOULD BE YOUR PASSWORD OR SOME MASSIVE API KEY)
```

**Set credentials as environment variables in secrets.yml**

```text
development:
  secret_key_base: [whatever_your_huge_secret_key_is]
  sendgrid_username: <%= ENV["SENDGRID_USERNAME"] %>
  sendgrid_password: <%= ENV["SENDGRID_PASSWORD"] %>
  sendgrid_domain: localhost

production:
  secret_key_base: <%= ENV["SECRET_KEY_BASE"] %>
  sendgrid_username: <%= ENV["SENDGRID_USERNAME"] %>
  sendgrid_password: <%= ENV["SENDGRID_PASSWORD"] %>
  sendgrid_domain: heroku.com
```

**Specify SendGrid's smtp server in our Action Mailer settings**

First, create an initializer for our mailers:

```text
touch config/initializers/mail.rb
```

Configure the mail initializer to use SendGrid's smtp server:

```text

# In config/initializers/mail.rb

ActionMailer::Base.raise_delivery_errors = true

ActionMailer::Base.smtp_settings = {
  :user_name => Rails.application.secrets.sendgrid_username,
  :password => Rails.application.secrets.sendgrid_password,
  :domain => ENV["RAILS_ENV"] == "development" ? "localhost" : "heroku.com",
  :address => 'smtp.sendgrid.net',
  :port => 587,
  :authentication => :plain,
  :enable_starttls_auto => true
}

ActionMailer::Base.delivery_method = :smtp
ActionMailer::Base.default charset: "utf-8"
```

Notice that our smtp settings to not explicitly state our SendGrid username & password, but rather point to _Rails.application.secrets_ \(which in turn references an environment variable\). There is a fairly gigantic problem with storing credentials like your SendGrid username and password directly in your mail.rb file or anywhere else - if your project is on GitHub, and your GitHub repo is public, those credentials will be exposed to the world. This can be _completely fucked up_ - you run the risk of people stealing your credentials and using them to rack up enormous bills at your expense.

To prevent this from happening, you should always store credentials and API keys in environment variables, or in secrets.yml \(or another file that will not be added to Git\). Using other hosts, we might be able to store our credentials in our secrets.yml and then add that file to .gitignore, but this won't work on Heroku, since we can only deploy to Heroku by git push-ing to our Heroku remote.

#### Configure Heroku for SendGrid <a id="configure-heroku-for-sendgrid"></a>

**Create a Heroku application**

```text
heroku create badgermail
# Creating badgermail... done, stack is cedar-14
# https://badgermail.herokuapp.com/ | https://git.heroku.com/badgermail.git
```

This will create a remote repo on heroku, which you can confirm in terminal using the Heroku CLI.

```text
git remote -v
# heroku  https://git.heroku.com/badgermail.git (fetch)
# heroku  https://git.heroku.com/badgermail.git (push)
```

**Test the mailer in the development environment**

* `rails s`
* go to localhost:3000/users/new
* create a user
* check the server logs
* check your email

**Production Environment**

**Configure the Heroku app with your SendGrid credentials**

**If you have a SendGrid account**

If you have a SendGrid account, you can manage all your mailers using those credentials.

In the root directory of your git repo, run the following:

```text
heroku config:set SENDGRID_USERNAME=mrbadger
heroku config:set SENDGRID_PASSWORD=chicken
```

You can also set these in the Heroku web app by clicking on your project, going to settings and adding them as key-value pairs in your Config Variables.

**If you don't have a SendGrid account**

If you don't have a SendGrid account, you can get Heroku to create one for you using the Heroku addon creator \(this will only work if Heroku have your credit card details, even though we're only going to be creating an unpaid, 'Starter' tier account\).

```text
heroku addons:create sendgrid:starter
```

**Stage, commit and push your project to Heroku**

```text
git add .
git commit -m "Heroookku-san"
git push heroku master
```

**Remember to migrate in heroku!**

```text
heroku run rake db:migrate
```

**Start the server and test in production**

```text
heroku open
# browse to /users/new and create a new user.
```

#### _Mailers with Heroku and SendGrid - documentation and further reading_ <a id="mailers-with-heroku-and-sendgrid---documentation-and-further-reading"></a>

* [Rails Guides - Action Mailer Basics](http://guides.rubyonrails.org/action_mailer_basics.html)
* [Ruby On Rails - Action Mailer Documentation](http://api.rubyonrails.org/classes/ActionMailer/Base.html)
* [SendGrid - Ruby on Rails - User Guide](https://sendgrid.com/docs/Integrate/Frameworks/rubyonrails.html)
* [Heroku - SendGrid - Integration Guide](https://devcenter.heroku.com/articles/sendgrid)
* [Managing Heroku Variables for Local Development](http://tammersaleh.com/posts/managing-heroku-environment-variables-for-local-development/)

