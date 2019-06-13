---
description: >-
  This is the post-boarding announcement for Burning Airline flight BA123. Does
  anyone know how to fly this thing?
---

# Day 02



What we covered today:

* [Fast Recursion](https://github.com/wofockham/sei-31/tree/master/13-advanced/recursion)
* [React with Rails backend - Client/Server](https://github.com/wofockham/sei-31/tree/master/12-react)
  * CORS
  * NGROK
* [Burning Airlines!](https://gist.github.com/wofockham/772cd994c1843259f54d)

#### Warmup <a id="warmup"></a>

* ​[Phone Number Check​](https://github.com/Yiannimoustakas/sei31-homework/tree/master/warmups/week08/day02_phone_number_check)

## React with Rails backend - Client/Server <a id="react-with-rails-backend-client-server"></a>

### Creating the Server <a id="creating-the-server"></a>

Create a new Rails application for the

```bash
rails new secret_server -T --skip-git #use postgresql for BA
cd secret_server
rails generate scaffold Secret content:text
atom .
```

Make sure that the scaffold worked correctly and has generated the migration in the `db/migrate/` folder.

Create the database and run the migration.

```bash
rails db:create
rails db:migrate
rails server # You can also use 'rails s'
```

Navigate through to the secrets page and make sure you can create, delete, and update a secret.

```javascript
http://localhost:3000/secrets
```

Create some extra secrets so we have something in the database.

As we have used `scaffolding` it has created a bunch of extra features for us. If you check in the `_secret.json.jbuilder` file you will see that we are generating a json file for the information stored on our database. All you have to do now is stick `.json` on the end of the URL.

```javascript
http://localhost:3000/secrets.json
```

The `_secret.json.jbuilder` file sohuld look like this:

```javascript
json.extract! secret, :id, :content, :created_at, :updated_at
json.url secret_url(secret, format: :json)
```

### Creating the Client side <a id="creating-the-client-side"></a>

Create a new React application.

```bash
create-react-app secrets_app
cd secrets_app
cd src
mkdir components
mv App.js components/
rm App*
cd ..
atom.
```

Open the App.js file and remove everything you don't need like so:

```javascript
import React, { Component } from 'react';
import Secrets from './Secrets'
​
class App extends Component {
  render() {
    return (
      <div className="App">
        <Secrets />
      </div>
    );
  }
}
​
export default App;
```

Create new file `Secrets.js` in the `src` folder. Now import React, define the class of `Secrets`, and export the `Secrets` component so it can be seen.

```javascript
import React, {PureComponent as Component} from 'react';
​
class Secrets extends Component {
  render(){
    return(
      <h2>coming soon</h2>
    )
  }
}
​
export default Secrets;

```

Don't forget that we need to import `Secrets` into `App.js`.

```javascript
import Secrets from './Secrets'
```

Now we want to set up two new child components of `Secrets`. They will be called `<SecretsForm />` and `<SecretsGallery />`.

```javascript
import React, {PureComponent as Component} from 'react';
​
class Secrets extends Component {
  render(){
    return(
      <h1>Secrets</h1>
      <h2>Tell us all your secrets</h2>
      <SecretsForm />
      <SecretsGallery />
    )
  }
}
​
export default Secrets;
```

Define the components as classes in the `Secrets.js` file.

```javascript
class SecretsForm extends Component {
  render(){
    return(
      <h3>Secrets form coming soon</h3>
    );
  }
}
​
class Gallery extends Component{
  render(){
    return(
      <h3>Gallery Coming soon</h3>
    );
  }
}
```

Create a new `form` in `SecretsForm` class so we can eventually store the values in the state.

```javascript
class SecretsForm extends Component {
  render(){
    return(
      <form>
        <textarea></textarea>
        <input type="submit" value="Tell" />
      </form>
    );
  }
}
```

Set up the constructor with `super()` and define the state.

```javascript
class SecretsForm extends Component {
  constructor(){
    super();
    this.state = { content: '' };
  }
​
  render(){
    return(
      <form>
        <textarea></textarea>
        <input type="submit" value="Tell" />
      </form>
    );
  }
}
```

Define an event handler that will take care of the changes being made in our newly created form. Then use a console log that will inform us when there has been a change.

```javascript
_handleChange(){
  console.log(`change occured`);
}

```

Now we need to attach this to the `textarea` so when the application registers that there has been a change it will call the `_handleChange` function.

```javascript
render(){
  return(
    <form>
      <textarea onChange={this._handleChange}></textarea>
      <input type="submit" value="Tell" />
    </form>
  );
}
```

This is what it all should look like so far.

```javascript
class SecretsForm extends Component {
  constructor(){
    super();
    this.state = { content: '' };
  }
​
  _handleChange(e){
    console.log(e, e.target, e.target.value);
  }
​
  render(){
    return(
      <form>
        <textarea onChange={this._handleChange}></textarea>
        <input type="submit" value="Tell" />
      </form>
    );
  }
}
```

Now we need to set the state inside the event handler. We pass the event through `e` so we can gain access to the value of the textarea.

```javascript
_handleChange(e){
  this.setState( { content: e.target.value } );
}
```

If you try and run this code you will see that an error will occur. This is because we have lost the value of `this`. To get this back all we need to do is bind `this` in the constructor.

```javascript
constructor(){
  super();
  this.state = { content: '' };
  this._handleChange = this._handleChange.bind(this);
}

```

We now need to handle the submit. To do this we need to first define another event listener and prevent the default from submitting to the server straight away as we want to sort it out ourselves.

```javascript
_handleSubmit(e){
  e.preventDefault();
}
```

This event listener isn't doing much by itself so we need to attached it to the form so it will be called once the form is submitted.

```javascript
render(){
  return(
    <form onSubmit={ this._handleSubmit }>
      <textarea onChange={this._handleChange}></textarea>
      <input type="submit" value="Tell" />
    </form>
  );
}
```

As with the `_handleChange` function we need to bind `this` to `_handleSubmit`.

```javascript
constructor(){
  super();
  this.state = { content: '' };
  this._handleChange = this._handleChange.bind(this);
  this._handleSubmit = this._handleSubmit.bind(this);
}
```

Now lets log out the content of the state to make sure it's all working.

```javascript
_handleSubmit(e){
  e.preventDefault();
  console.log(this.state.content);
}
```

Now we want to start looking at saving the input once we submit the form. Attach a new `onSubmit` in `SecretsForm` component that will call a function we will create called `saveSecret`.

```javascript
class Secrets extends Component {
  render(){
    return(
      <div>
        <h1>Secrets</h1>
        <h2>Tell us all your secrets</h2>
        <SecretsForm onSubmit={ this.saveSecret }/>
        <Gallery />
      </div>
    )
  }
}
```

Next we need to define `saveSecret` inside the `Secrets` class.

```javascript
class Secrets extends Component {
​
  saveSecret(s){
    console.log(`Parent: ${s}`);
  }
​
  render(){
    return(
      <div>
        <h1>Secrets</h1>
        <h2>Tell us all your secrets</h2>
        <SecretsForm onSubmit={ this.saveSecret }/>
        <Gallery />
      </div>
    )
  }
}
```

If you run the code and submit the text in the `textarea` it will now console log from the parent. Look at you, passing data around like it's nothing!

Now we need `saveSecret` to save the state. Set up state for `secrets` within the constructor.

```javascript
constructor(){
  super();
  this.state = { secrets: [] };
}
```

First we need to bind `this` to `saveSecret`.

```javascript
constructor(){
  super();
  this.state = { secrets: [] };
  this.saveSecret = this.saveSecret.bind(this);
}
```

Second, we need to save the state without changing the state directly.

```javascript
saveSecret(s){
  this.setState( { secrets: [...this.state.secrets, s]  } ); // Using ES6 spread operator so we don't change the original value.
}
```

Here is some additional information on the subject of why we don't want to change the original values.

* ​[Spread syntax - JavaScript \| MDN](https://developer.mozilla.org/en-US/docs/Web/JavaScript/Reference/Operators/Spread_operator)​
* ​[Immutable.js](https://facebook.github.io/immutable-js/)​

We will then reset the state of content to be a empty string so when we submit the form we don't have to delete what we had in the `textarea`.

```javascript
_handleSubmit(e){
  e.preventDefault();
  this.props.onSubmit( this.state.content );
  this.setState( { content: '' }); //Reset the form
}
```

This is still not going to remove the text as we're not updating the value of the textarea. We do this by stating that the value is equal the the content of the state.

```javascript
render(){
  return(
    <form onSubmit={ this._handleSubmit }>
      <textarea onChange={this._handleChange} value={this.state.content}></textarea>
      <input type="submit" value="Tell" />
    </form>
  );
}
```

Now we need to get the Gallery connected so we can display the secrets that are in our database on screen. We do this by passing the state of the secrets through the Gallery component.

```javascript
render(){
  return(
    <div>
      <h1>Secrets</h1>
      <h2>Tell us all your secrets</h2>
      <SecretsForm onSubmit={ this.saveSecret } />
      <Gallery secrets={ this.state.secrets } />
    </div>
  )
}
```

We now need to update the Gallery class so it will display be able to render the secrets. This can be achieved in the same way when defined as a function so I will show you an example below. Remove the class of Gallery and turn it into a function.

```javascript
// class Gallery extends Component{
//   render(){
//     return(
//       <h3>Gallery Coming soon</h3>
//     );
//   }
// }
​
function Gallery(props){
  return(
    <div>
      { props.secrets.map( s => <p>{s}</p> )}
    </div>
  )
}
```

If you run the code you will now see that you can display the secrets but we're still not connected to the database. In steps AXIOS.

#### AXIOS <a id="axios"></a>

* ​[axios](https://www.npmjs.com/package/axios)​

Axios is a package that allows us to send requests to our server. We could also use the more current [fetch](https://developer.mozilla.org/en-US/docs/Web/API/Fetch_API) but this is yet to be accessible for older browsers.

```text
npm install --save axios
```

Add the import line to the top of the page.

```text
import axios from 'axios';
```

Now define a variable to hold the value of the URL that you will be serving up the data in the json format.

```javascript
const SERVER_URL = 'http://localhost:5000/secrets.json';
```

Remember that we want to be using a different port as our React app will be using port 3000.

We then add the axios `get` request connection in the constructor.

```javascript
constructor(){
  super();
  this.state = { secrets: [] };
  this.saveSecret = this.saveSecret.bind(this);
​
  axios.get(SERVER_URL).then( results => console.log( results ));
}
```

Run the rails server

```bash
rails server # or 'rails s'
```

Open the gemfile in your rails application and add the axios gem called 'rack-cors'.

```bash
gem 'rack-cors', :require => 'rack/cors'
```

Create new file in config/initializers/ called `cors.rb` and add the following code to the file.

```ruby
Rails.application.config.middleware.insert_before 0, Rack::Cors do
  allow do
    origins '*'
    resource '*',
      :headers => :any,
      :methods => %i(get post put patch delete options head)
  end
end
```

This allows requests from all origins meaning that the requests don't have to come from the same server and anyone can access the json data.

Jump back into the React app and set the state of the secrets. Remember that we use the 'fat arrow' functions to retain the value of `this`.

```javascript
constructor(){
  super();
  this.state = { secrets: [] };
  this.saveSecret = this.saveSecret.bind(this);
​
  axios.get(SERVER_URL).then( results => this.setState( { secrets: results.data }));
}
```

We will then update the `Gallery` function to now display the correct information. We are now getting an object back so we refer the the content within the secrets object. To avoid React giving us errors we will also give each secrets a key of the index so React can identify these as individual elements.

```javascript
function Gallery(props){
  return(
    <div>
      { props.secrets.map( s => <p key={ s.id }>{ s.content }</p> )}
    </div>
  )
}
```

Now when you run the code you will see the secrets are displaying from the database. If you try to add through the site your content will break everything. This is because our new elements are still coming through as a string rather than an object like the secrets in our database.

```javascript
RESULT =>
​
2:
{…}
content: "This isn't a secret"
created_at: "2017-12-11T23:26:14.231Z"
id: 3
updated_at: "2017-12-11T23:26:14.231Z"
url: "http://localhost:5000/secrets/3.json"
3: "I did some stuff"
```

To fix this all we need to do is change the format of the content `{content: s}`.

```javascript
saveSecret(s){
  this.setState( { secrets: [...this.state.secrets, {content: s}]  } );
}
```

We can then finally save the content to server with axios `post` request. You should note that for the moment I have commented out the below line. We no longer need to set the state as we will be reading directly from the server.

```javascript
saveSecret(s){
  axios.post(SERVER_URL, {content: s});
  // this.setState( { secrets: [...this.state.secrets, {content: s}]  } ); // Using ES6 spread operator, Without mutation
}
```

We can now check in the server logs to see if this will actually be saved.

You will now see that there is an Authenticity Token error stopping us from saving to the data. This is because Rails doesn't know that we are sending the request. Go back to the rails atom and remove the `protect_from_forgery with: :exception` line in the `applicatoin_controller.rb` file within the controllers folder. This will remove the need for an Authenticity Token all together.

Jump back over to the React application and add a promise to log the results.

```javascript
saveSecret(s){
  axios.post(SERVER_URL, {content: s}).then( results => console.log( results ));
  // this.setState( { secrets: [...this.state.secrets, {content: s}]  } ); // Using ES6 spread operator, Without mutation
}
```

Now we need to set the state, remember the 'fat arrow' will retain the value of `this`.

```javascript
saveSecret(s){
  axios.post(SERVER_URL, {content: s}).then( results => {
    this.setState({secrets: [...this.state.secrets, results.data]})
  });
  // this.setState( { secrets: [...this.state.secrets, {content: s}]  } ); // Using ES6 spread operator, Without mutation
}
```

The Gallery will now re-render automatically with the new secrets that are saved into the database.

The only problem we have is that they're saving to the bottom of the list. Flip the order of the secrets list

```javascript
saveSecret(s){
  axios.post(SERVER_URL, {content: s}).then( results => {
    this.setState({secrets: [results.data, ...this.state.secrets]})
  });
  // this.setState( { secrets: [...this.state.secrets, {content: s}]  } ); // Using ES6 spread operator, Without mutation
}
```

Now when you submit a new secret it will appear at the top of the list.

Try refreshing the page. As you can see the newly created secret is back at the bottom of the list. This is because we're listing them in the same order as they are being saved in the database. We want the server to also give us the list in reverse order.

Go back to Rails application and change the secrets\_controller to list them in a descending order.

```ruby
def index
  @secrets = Secret.order('created_at DESC')
end
```

Looks like we've got it all working now but if we had additional users on our site and they were creating new secrets we wouldn't be able to see the changes. It not refreshing live!

Save the axios `get` request into a variable and call it straight underneath so that we have the first request going out when we load the page.

```javascript
constructor(){
  super();
  this.state = { secrets: [] };
  this.saveSecret = this.saveSecret.bind(this);
​
  // the arrow function allos us to preserve the value of `this`
  const fetchSecrets = () => {axios.get(SERVER_URL).then( results => this.setState( { secrets: results.data }));
  };
​
  fetchSecrets();
}
```

Now call `fetchSecrets()` periodically. We could call the `fetchSecrets()` function with a `setInterval` of one second but we will recursively call the function with a `setTimeout`.

```javascript
constructor(){
  super();
  this.state = { secrets: [] };
  this.saveSecret = this.saveSecret.bind(this);
​
  // the arrow function allos us to preserve the value of `this`
  const fetchSecrets = () => {axios.get(SERVER_URL).then( results => this.setState( { secrets: results.data }));
  setTimeout( fetchSecrets, 4000 );
  };
​
  fetchSecrets();
}
```

Now once the initial call is made to `fetchSecrets()` it will call itself after four seconds, continuing the loop.

### Type Checking - PropTypes <a id="type-checking-proptypes"></a>

#### Making sure you're passing the correct data types <a id="making-sure-youre-passing-the-correct-data-types"></a>

We will quickly step to the side to talk about Type Checking. We want to make sure that we're passing the correct data through to our different components so we don't come up with major errors. To do this we will use PropTypes.

PropTypes exports a range of validators that can be used to make sure the data you receive is valid. In this example, we’re using PropTypes.func as we are passing a function through.

* ​[Typechecking With PropTypes - React](https://reactjs.org/docs/typechecking-with-proptypes.html)​

First we need to install PropTypes into our application.

```bash
npm install --save prop-types
```

Then we will import PropTypes at the top of the file in which we refer to it.

```javascript
import PropTypes from 'prop-types';
```

We then need to define the requirements of our onSubmit. This makes sure that we have a some sort of data and that it is a function.

```javascript
SecretsForm.propTypes = {
  onSubmit: PropTypes.func.isRequired
}

```

