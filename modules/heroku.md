# Heroku

## _What is it?_

Github only supports static front-end sites, so we need something more robust and scalable for our Rails apps. Heroku has everything that we need for this:

* Dynos - run virtually any language
* Database - always postgres
* Tools and components
* Connects with heaps of other applications \(Salesforce etc.\)
* Good support

See [here](https://www.heroku.com/home) for more.

Heroku is completely based on Git, meaning that it relies on us pushing code up etc. It can be difficult, though it is ten times easier than making our own server.

## _Steps for Deploying a Rails app to Heroku_ <a id="steps-for-deploying-a-rails-app-to-heroku"></a>

* Make an account - [here.](https://signup.heroku.com/www-header)​
* Install the heroku toolbelt - see [here](https://toolbelt.heroku.com/) or use `brew install heroku/brew/heroku`
* Make sure that it has worked
  * `heroku --version` or `heroku -v`
  * `which heroku`
* Log in - `heroku login`
* Make your Rails app
  * 'Deploy Early, Deploy Often'
  * Set the app up with PostgreSQL
    * `rails new app_name --database=postgresql --skip-turbolinks`
    * `rails new app_name -d postgresql --skip-turbolinks`
* Change into the app directory and run `bundle` to install all Gems and dependencies
* At the end of the Gemfile, add this line - `gem 'rails_12factor', group: :production` and then run `bundle` again
* Make it a git repository
  * Run `git init` in the root folder of the project
  * `git add .`
  * `git commit -m "Your commit message."`
  * Run `git status` to make sure everything has worked
* Run `heroku create your_app_name` if you don't specify the name of your app it will randomly generate one for you.
* Run `git config --list | grep heroku` to make sure it has worked
* Run `git push heroku master` to push the project to heroku
* Run `heroku run rails db:migrate`
  * Any command prefixed by `heroku run` will obviously run on the heroku terminal
* Run `heroku run rails db:seed` to load your seed data
* Ensure that you have a running server, run `heroku ps:scale web=1`
* Run `heroku ps` to make sure that that has all worked
* Run `heroku open` to do exactly what you imagine

For this in far more detail, [see here.](https://devcenter.heroku.com/articles/getting-started-with-rails4)​

* Also remember to create a new github repo. ie:

1. ​[Login to GitHub](http://www.github.com/).
2. Click the **+** icon in the top left-hand corner of the page, and select "New Repository".
3. Complete the **Create a new repository** form, and **do not** check the 'Initialize this repository with a README' checkbox.
4. Copy the **HTTPS** url.
5. Open Terminal.
6. Go to your repo's directory.
7. Run the command below.

```text
$ git remote add origin https://github.com/cjbarnaby/green-bastard.git$ git push origin master
```

## _Common Heroku Commands_ <a id="common-heroku-commands"></a>

* `heroku run rails db:etc.` \(create, seed, migrate etc.\)
* `heroku run rails console`
* `heroku logs` - Shows the rails server \(once off\)
* `heroku logs --tail` - Shows the live server

