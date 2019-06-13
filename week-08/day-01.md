---
description: Recursion + More React
---

# Day 01

What we covered today:

* Recursion
* React Flickr Search
* React
  * Data Flow
  * Ajax
  * Routing

Warmup

* [Pairwise](https://github.com/Yiannimoustakas/sei31-homework/tree/master/warmups/week08/day01_pair_wise)

## Recursion

Recursion in computer science is a method where the solution to a problem depends on solutions to smaller instances of the same problem \(as opposed to iteration\). Recursive algorithms have two cases: a recursive case and base case. Any function that calls itself is recursive.

Create a new folder and add a new ruby file into it.

```bash
mkdir recursion
cd recursion
touch countdown.rb
atom .
```

Iterative method:

```ruby
def countdown_i (n=10)
  while n >= 0
    puts n
    n -= 1
    sleep 1
  end
​
  puts "Blast off!"
​
end
​
require 'pry'
binding.pry
```

Call the method

```bash
ruby countdown.rb
countdown_i
```

Recursive method:

```ruby
def countdown_r (n=10)
  if n < 0 #Base case
    puts "Blast off"
  else
    puts n
    sleep 1
    countdown_r n-1 # Recursive case which moves us closer to the case
  end
end
​
require 'pry'
binding.pry
```

Call the method

```bash
ruby countdown.rb
countdown_r
```

Some things are better solved with recursion like factorials.

```bash
5! = 5 x 4 x 3 x 2 x 1
```

Create a new file.

```bash
touch factorial.rb
```

Iterative method:

```ruby
def factorial_i(n)
  result = 1
  while n > 1
    result = result * n # Mutation
    n -= 1 # Move closer to the base case
  end
  result
end
​
require 'pry'
binding.pry
```

Call the method

```ruby
ruby factorial.rb
factorial_i 5
```

Recursive method:

```ruby
def factorial_r(n)
  if n > 1 # Recursive case
    n * factorial_r(n -1)
  else
    1 # Base case
  end
end
​
require 'pry'
binding.pry
```

Call the method

```ruby
factorial.rb
factorial_r 5
```

Create another file. Fibonacci

```bash
touch fibonacci.rb
```

Iterative method:

```ruby
def fibonacci_i(n)
  a = 1
  b = 1
​
  while n > 1
    a, b = b, a + b  # Parallel assignment: a = b; b = a + b
    n -= 1
  end
  a # The final result will be in a
end
​
require 'pry'
binding.pry
```

Recursive method:

```ruby
# Very slow
def fibonacci_r(n)
​
  if n <= 2
    1 # Base case
  else
    fibonacci_r(n - 1) + fibonacci_r(n - 2)
  end
end
​
​
def factorial_r(n)
  if n > 1 # Recursive case
    n * factorial_r(n -1)
  else
    1 # Base case
  end
end

```

This example is extremely slow because we call the method twice with each iteration. This will exponential increase, doubling the If you call `fibonacci_r(100)` it looks for `fibonacci_r(98) + fibonacci_r(99)`. The problem is that it will then call `fibonacci_r()` twice for each of those `fibonacci_r(97) + fibonacci_r(98)`. As you can see you will be calling `fibonacci_r(98)` twice and create two copies of the branch even though you have already figured that out.

## React <a id="react"></a>

### React Flickr Search <a id="react-flickr-search"></a>

* [​Class Demo​](https://github.com/wofockham/sei-31/tree/master/12-react/flickr-search)

This module will recreate the Flickr Search we created on [Day 03](../week-07/#day-03) using React.

#### Create a new React application. <a id="create-a-new-react-application"></a>

```bash
create-react-app flickr-search
cd flickr-search/
cd src
mkdir components
mv App.js components/
rm App*
cd ..
atom.
```

Change the impost line on the `index.js` for App as we have moved it into the components folder.

```javascript
import App from './components/App';
```

open the `App.js` file and remove the unwanted information, the end result should look like this. We also want to a new component `<FlickrSearch />` to the render.

```javascript
import React, { Component } from 'react';
​
class App extends Component {
  render() {
    return (
      <div className="App">
        <FlickrSearch />
      </div>
    );
  }
}
​
export default App;
```

Open the application in your browser

```bash
npm start
```

Create new file `FlickrSearch.js` in the `src` folder. Import React, define the class, then export the component.

```javascript
import React, { PureComponent as Component} 'react';
​
class FlickrSearch extends Component {
  render(){
    return(
      <div>
        <h1>FlickrSearch coming soon</h1>
      </div>
    )
  }
}
export default FlickrSearch
```

Go to the `App.js` file and import into `FlickrSearch`.

```javascript
import FlickrSearch from './FlickrSearch';
```

Add two new components to `FlickrSearch` in the render function.

```javascript
import React, { PureComponent as Component} 'react';
​
class FlickrSearch extends Component {
  render(){
    return(
      <div>
        <h1>Image Search</h1>
        <SearchForm />
        <Gallery />
      </div>
    )
  }
}
export default FlickrSearch
```

Add the two classes to the top of the `FlickrSearch` page.

```javascript
class SearchForm extends Component {
  render(){
    return(
      <h2>Search coming soon</h2>
    )
  };
}
​
class Gallery extends Component {
  render(){
    return(
      <h2>Gallery coming soon</h2>
    )
  };
}
```

Inside the `SearchForm` class add an input with function, and a button.

```javascript
class SearchForm extends Component {
  render(){
    return(
      <form>
        <input type="search" placeholder="Butterflies" onInput={ this._handleInput } />
        <input type="submit" value="Search" />
      </form>
    )
  };
}
```

Create constructor and event handler.

```javascript
class SearchForm extends Component {
​
  constructor(){
    super();
    this.state = { query: ''};
  }
​
  _handleInput(e){
    this.setState( {query: e.target.value} )
  }
​
  render(){
    return(
      <form>
        <input type="search" placeholder="Butterflies" onInput={ this._handleInput } />
        <input type="submit" value="Search" />
      </form>
    )
  };
}

```

We now need to bind `this` in the constructor.

```javascript
constructor(){
  super();
  this.state = { query: ''};
  this._handleInput = this._handleInput.bind(this);
}
```

Pass the event handler to the form through `onSubmit`.

```javascript
render(){
  return(
    <form onSubmit={ this._handleSubmit }>
      <input type="search" placeholder="Butterflies" onInput={ this._handleInput } />
      <input type="submit" value="Search" />
    </form>
  )
};
```

Define `_handleSubmit` and prevent the form from submitting to the server.

```javascript
_handleSubmit(e){
  e.preventDefault();
}
```

We want to parent \(HistoryEraser\) to do the AJAX request as we will need to pass the information to the siblings. We will take the input from the child \(SearchForm\) and pass it back up to the parent \(HistoryEraser\) so we can do the search and pass it to back to the sibling \(Gallery\).

We need to define another function `fetchImages`.

```javascript
class FlickrSearch extends Component {
  fetchImages(q){
    console.log(`searching for ${q}`);
  }
​
  render(){
    return(
      <div>
        <h1>Image Search</h1>
        <SearchForm onSubmit={ this.fetchImages }/>
        <Gallery />
      </div>
    )
  };
}
```

Then we pass the function through the `SearchForm` component in the `FlickrSearch` render.

```javascript
class FlickrSearch extends Component {
  render(){
    return(
      <div>
        <h1>Image Search</h1>
        <SearchForm onSubmit={ this.fetchImages }/>
        <Gallery />
      </div>
    )
  };
}
```

Pass the state of `query` into the `_handleSubmit` function.

```javascript
_handleSubmit(e){
  e.preventDefault();
  this.props.onSubmit( this.state.query );
}
```

then bind submit in constructor.

```javascript
constructor(){
  super();
  this.state = { query: ''};
  this._handleInput = this._handleInput.bind(this);
  this._handleSubmit = this._handleSubmit.bind(this);
}
```

Make a request to Flickr, in particular we want to make a `axios` request.

```javascript
fetchImages(q){
  const flickrURL = 'https://api.flickr.com/services/rest/?jsoncallback=?';
  const flickrParams = {
    method: 'flickr.photos.search',
    api_key: '2f5ac274ecfac5a455f38745704ad084',
    text: q,
    format: 'json',
    per_page: 500
  }
  console.log( flickrParams );
}
```

We now need to install axios.

#### AXIOS <a id="axios"></a>

* ​[axios](https://www.npmjs.com/package/axios)​

Axios is a package that allows us to send requests to our server. We could also use the more current [fetch](https://developer.mozilla.org/en-US/docs/Web/API/Fetch_API) but this is yet to be accessible for older browsers.

```bash
npm install --save axios
```

You will need to restart the server with `npm start` now that we have installed `axios`.

Add the import line to the top of the page.

```javascript
import axios from 'axios';
```

Add the axios request to the bottom of the function.

```javascript
fetchImages(q){
  const flickrURL = 'https://api.flickr.com/services/rest/?jsoncallback=?';
  const flickrParams = {
    method: 'flickr.photos.search',
    api_key: '2f5ac274ecfac5a455f38745704ad084',
    text: q,
    format: 'json',
    per_page: 500
  }
  axios.get(flickrURL, {params: flickrParams})
  .then( response => {
    console.log('response', response.data);
  })
  .catch( err => {
    console.warn("ERROR", err);
  })
}
```

Now we need to generate the `url`.

```javascript
axios.get(flickrURL, {params: flickrParams})
.then( response => {
    console.log('response', response.data);
})
.catch( err => {
    console.warn("ERROR", err);
})

​
const generateURL = function (photo) {
  const url = `https://farm${photo.farm}.staticflickr.com/${photo.server}/${photo.id}_${photo.secret}_q.jpg`;
  return <img src={url} alt={photo.title}/>
}
```

Add constructor to the `FlickrSearch` at the top.

```javascript
constructor(){
  super();
  this.state = { images: [] };
}
```

Set the state of the images.

```javascript
axios.get(flickrURL, {params: flickrParams})
    .then( response => {
      console.log('response', response.data);
      this.setState({images: response.data.photos.photo })
    })
```

Bind `this` to `fetchImages` in constructor.

```javascript
constructor(){
  super();
  this.state = { images: [] };
  this.fetchImages = this.fetchImages.bind(this);
}
```

`this` has no value within the `jsonp` request. We could bind `this` to the end of the function like so:

```javascript
axios.get(flickrURL, {params: flickrParams}).then(function(response){
  const images = results.photos.photo.map(generateURL);
  this.setState({images: images}); //ES6: you can just use {images}
}.bind(this));
```

But we will use a 'fat arrow' function to retain the value of `this`. This will create a second copy of `this` to use later.

```javascript
axios.get(flickrURL, {params: flickrParams}).then((results) => {
  const images = results.photos.photo.map(generateURL);
  this.setState({images: images}); //ES6: you can just use {images}
});
```

#### Setting up the Gallery <a id="setting-up-the-gallery"></a>

Pass the images state to the Gallery component

```javascript
render(){
  return(
    <div>
      <h1>Image Search</h1>
      <SearchForm onSubmit={ this.fetchImages }/>
      <Gallery images={ this.state.images }/>
    </div>
  )
};
```

Update the Gallery class to loop through each URL and `map` it to the `src` of an image tag.

```javascript
class Gallery extends Component {
  render(){
    return(
      <div>
        { this.props.images.map( (imageURL) => <img src={imageURL} /> ) }
      </div>
    )
  };
}
```

We will need to give each image a unique key to make sure there aren't multiple images.

```javascript
class Gallery extends Component {
  render(){
    return(
      <div>
        { this.props.images.map( (imageURL) => <img src={imageURL} key={ imageURL } /> ) }
      </div>
    )
  };
}
```

### functional components <a id="functional-components"></a>

Functional components are used when your component doesn't need state or callbacks.

```javascript
function Image (props){
  return(
    <img src={props.url} width="150" height="150" alt={props.url} />
  )
}
```

Refer to the `Image` component in the render of `Gallery`.

```javascript
class Gallery extends Component {
  render(){
    return(
      <div>
        { this.props.images.map( (imageURL) => <Image url={imageURL} key={imageURL} /> ) }
      </div>
    )
  };
}
```

### React Routing <a id="react-routing"></a>

* ​React Router​
* ​Flickr Search Router​

Create a new React application.

```bash
create-react-app routing
$ cd src
mkdir components
mv App.js components/
rm App*
cd ..
npm install --save react-router react-router-dom
atom .
```

Open `App.js` and remove the information you don't need. The end result should look like this:

```javascript
import React, { Component } from 'react';
​
class App extends Component {
  render() {
    return (
      <div className="App">
​
      </div>
    );
  }
}
​
export default App;
```

As we have moved the `App.js` file we need to change the referencing in the `index.js` file.

```javascript
import App from './components/App';
```

Create file in the `src` folder called `Routes.js`.

Create the routes and export them then import `Routers` and `React`.

```javascript
import React from 'react';
import { HashRouter as Router, Route } from 'react-router-dom';
​
export const Routes = { // You don't need 'default' if you are exporting on this line
  <Router>
    <div>
      <Route exact path="/" component={ Home } />
      <Route exact path="/faq" component={ FAQ } />
    </div>
  </Router>
}
```

In the `index.js` remove `import App from './components/App';` and replace it with `import Routes from './Routes';` You will also need to remove `<App />` from the ReactDOM.render and replace with `Routes`. Note that we are referring to the imported `Routes` and not the component. This is why we don't need the opening and closing brackets `< />`.

```javascript
import React from 'react';
import ReactDOM from 'react-dom';
import './index.css';
//import App from './components/App';
import Routes from './Routes';
import registerServiceWorker from './registerServiceWorker';
​
ReactDOM.render(Routes, document.getElementById('root'));
registerServiceWorker();
```

Run the application from your terminal

```bash
npm start
```

You will see that there a few errors that we need to take care of before moving on.

We need to create `Home` component.

Create a file new file in the `src` folder called `Home.js` and add the below code.

```javascript
import React, {PureComponent as Component} from 'react';
​
class Home extends Component {
  render(){
    return(
      <h2>This is the home page</h2>
    )
  };
}
​
export default Home;
```

Create another file in the `src` folder called `FAQ` and add this code.

```javascript
import React, {PureComponent as Component} from 'react';
​
class FAQ extends Component {
  render(){
    return(
      <h2>This is the FAQ page</h2>
    )
  };
}
​
export default FAQ;
```

Import into both `Home` and `FAQ` at the top of the `Routes.js` file and export `Routes` at the bottom.

```javascript
import React from 'react';
import { HashRouter as Router, Route } from 'react-router-dom';
​
import Home from './components/Home';
import FAQ from './components/FAQ';
​
export const Routes = {
  <Router>
    <div>
      <Route exact path="/" component={ Home } />
      <Route exact path="/faq" component={ FAQ } />
    </div>
  </Router>
}
​
export default Routes;
```

#### Adding links to the page <a id="adding-links-to-the-page"></a>

Open the `Home.js` file and import `Link` from `react-router-dom`.

```javascript
import React, {PureComponent as Component} from 'react';
import { Link } from 'react-router-dom';
​
class Home extends Component {
  render(){
    return(
      <div>
        <h2>This is the home page</h2>
        <p>
          Please check out our
          <Link to="/faq">
            Frequently asked questions
          </Link>
        </p>
      </div>
    )
  };
}
​
export default Home;
```

Now we need to link back from the FAQ page to the Home page.

```javascript
import React, {PureComponent as Component} from 'react';
import { Link } from 'react-router-dom';
​
class FAQ extends component {
  render(){
    return(
      <div>
        <h2>This is the FAQ page</h2>
        <Link to="/">
          Back to home
        </Link>
      </div>
    )
  };
}
​
export default FAQ;

```

Go back onto your browser and navigate through the links. You will see that you're now able to use the back arrow to move around the pages.

## Homework <a id="homework"></a>

* Get cosy with React
* Create a single page app that fetches data from an api \(we recommend using `axios` with OpenWeatherMap\)
* Absorb the MyGA lesson on recursion
* BONUS: try and create a faster recursive fibonacci \(either via memoisation or linear recursion\)

Some other things you might want to try:

* Finish the refactor of the Flickr Search React app to use the React Router:
  * save the input from the search form into the component's state
  * when the form is submitted, send the search query to the search results route
  * in the search results component, use the query passed in via the URL to perform an AJAX request to Flickr, and save the results into the component's state
  * Show a thumbnail image for each result, ideally by writing a `<Gallery />` functional component
  * BONUS: make the `/image/:id` route work for the FullscreenImage component, i.e. make each result thumbnail clickable, and show a fullscreen version of the image when clicked, by navigating to that route
* try rewriting any of your API homework from last week to use React instead of jQuery

Some topics below for you to brush up on:

* ​[React Router Documentation](https://reacttraining.com/react-router/)​
* [React.js Introduction For People Who Know Just Enough jQuery To Get By](https://chibicode.com/react-js-introduction-for-people-who-know-just-enough-jquery-to-get-by/)
* ​[Thinking In React](https://reactjs.org/docs/thinking-in-react.html)​
* ​[Learning ES6](http://learnharmony.org/)​
* ​[AXIOS - npm](https://www.npmjs.com/package/axios)​

