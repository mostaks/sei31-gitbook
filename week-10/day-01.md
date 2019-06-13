---
description: '/ bb | [^b] {2} /, that is the question...'
---

# Day 01

What we covered today:

* [Regular expressions](https://github.com/wofockham/sei-31/tree/master/13-advanced/regular_expressions-i)
* [Meteor.js](https://github.com/wofockham/sei-31/tree/master/13-advanced/meteor-todo)
* [Recursion](https://github.com/wofockham/sei-31/tree/master/13-advanced/recursion)

### Warmup <a id="warmup"></a>

* [Prime Factors](https://github.com/Yiannimoustakas/sei31-homework/tree/master/warmups/week10/day01_prime_factors)

## Regular Expressions I <a id="regular-expressions-i"></a>

Regular Expressions are pattern matchers for text. Sounds relatively dry \(or very, very dry\) but they are incredibly powerful. They are often considered a write-only language. You can write it, but it is virtually impossible to read. They occur in lots and lots of programming languages \(they are even in javascript\), and if you are interested in the history of them - see [here](https://en.wikipedia.org/?title=Regular_expression#History) - but the way that Ruby works with them stems from the Perl programming language.

### So, how do we work with them in Ruby? <a id="so-how-do-we-work-with-them-in-ruby"></a>

#### _Literal Characters_ <a id="literal-characters"></a>

We can match literal characters in virtually the same way as we would with two equals signs. Instead of using the two equals signs though, we use `=~` , think of this as a fuzzy or dynamic search. To specify a regular expression in Ruby \(and most other languages\), we use `//` \(these are considered Regexp delimiters\). Let's have a muck around with them.

```ruby
"bob" == "bob" # This obviously returns true.
​
"bob" =~ /bob/
# This is true, they do match. But instead of returning true, it returns the index of where the pattern started to match. In this case, it returns 0.  It returns nil if it doesn't match anything.
​
"Hello, my name is bob" =~ /bob/
# Again, this is true, but returns 18
```

Obviously this isn't the powerful part though, considering this does virtually the same thing as the multiple equals signs. So let's get into some of the cooler stuff.

#### _Metacharacters_ <a id="metacharacters"></a>

In regular expressions, there are characters that mean far more than literal characters. This can prove problematic \(but there are ways to escape metacharacters\), but also the source of some of the most powerful parts.

```ruby
"o!o" =~ /o.o/ # Returns 0
```

#### _Character Classes_ <a id="character-classes"></a>

These are often used to check for capitalized and uncapitalized versions, but can also be used to check for multiple letters. The metacharacters for these are `[]`.

```javascript
"bob" =~ /[Bb]ob/  # Returns 0
"Bob" =~ /[Bb]ob/  # Returns 0
"cob" =~ /[Bbc]ob/ # Returns 0
"dog" =~ /[Bb]ob/  # Returns nil
"any vowels" =~ /[aeiou]/ # Returns 0
```

For this sort of stuff, we can also use ranges. These are quite useful.

```javascript
"A" =~ /[A-Z]/      # Returns 0
"a" =~ /[A-Z]/      # Returns nil
"a" =~ /[A-z]/      # Returns 0
```

There are lots of inbuilt ranges, and these are really useful to know. Some of them are...

* `\s` - Will match any space characters \(spaces, new lines etc.\)
* `\S` - Anything other than space characters
* `\w` - Will match any word character \(i.e. actual letters or numbers\)
* `\W` - Will match any non-word character

```ruby
"Wolf" =~ /[\w]/    # Returns 0
"Wolf" =~ /[\W]/    # Returns nil
"Wolf " =~ /[\W]/   # Returns 4
​
"Wolf " =~ /[\s]/   # Returns 4
"Wolf " =~ /[\S]/   # Returns 0
```

We can obviously add lots and lots of things between those square brackets. But there are other ways we can do this as well. We can use `()` and `|` to check for multiple things.

```ruby
"Jane" =~ /(Jane|Serge)/    # Returns 0
"Serge" =~ /(Jane|Serge)/   # Returns 0
```

The round brackets are metacharacters in Regexp as well! They are a way to say this or this \(or this or this or this\). The pipe is the delimiter for saying "or".

There are a lot more metacharacters. The `.`, for example, is a wildcard, it will match anything.

```ruby
"jane" =~ /.ane/            # Returns 0
"zane" =~ /.ane/            # Returns 0
"Serge and Jane" =~ /.ane/  # Returns 10
```

This is still relatively hardcoded though, we need to specify where the characters are and what they are. To help solve this problem, there are quantifiers.

#### _Quantifiers_ <a id="quantifiers"></a>

Quantifiers are a way to check in Regexp whether things exist, or exist more than once etc.

The ones that you will actually use:

* `+` means one or more
* `?` means one or zero
* `*` means zero or more

It can look like the following:

```ruby
"Hi there" =~ /i+/      # Returns 1
"Hi there" =~ /the?/    # Returns 3
"Hi theeere" =~ /the*/  # Returns 3
```

These are regularly used for existence checks.

#### _Capturers_ <a id="capturers"></a>

These are difficult to understand. Basically, you match something in parentheses and can refer back to them. You capture something by using round brackets, and refer back to them using a `\` and an integer \(that mimics the order of capture\).

```ruby
"WolfWolf" =~ /(....)\1/
# Matches any four characters, but then needs to have the same four characters straight after.  This returns zero.
​
"ArcticWolf ArcticWolf" =~ /(......)(....) \1\2/
# Matches "Arctic" in the first brackets, then "Wolf" in the second brackets.  "Arctic" is saved as \1 and "Wolf" is saved as \2
```

#### _Regex Cheatsheet_ <a id="regex-cheatsheet"></a>

* ​[Cheatsheet](https://www.ralfebert.de/snippets/ruby-rails/regex_cheat_sheet/)​

## Meteor

We'll be following along with this [tutorial](https://www.meteor.com/tutorials/react/creating-an-app).

**Creating your first app**

Install Meteor through the below curl command.

```bash
curl https://install.meteor.com/ | sh
```

To create the app, open your terminal and type:

```bash
meteor create simple-todos
```

This will create a new folder called simple-todos with all of the files that a Meteor app needs:

```javascript
client/main.js        # a JavaScript entry point loaded on the client
client/main.html      # an HTML file that defines view templates
client/main.css       # a CSS file to define your app's styles
server/main.js        # a JavaScript entry point loaded on the server
package.json          # a control file for installing NPM packages
package-lock.json     # Describes the NPM dependency tree
.meteor               # internal Meteor files
.gitignore            # a control file for git
```

To run the newly created app:

```bash
cd simple-todos
meteor
```

Open your web browser and go to [http://localhost:3000](http://localhost:3000/) to see the app running.

#### @channel to fix our meteor project: <a id="channel-to-fix-our-meteor-project"></a>

\`\`\`\# shut down the server meteor remove blaze-html-templates meteor add static-html

## restart server <a id="restart-server"></a>

meteor

## all should be well\`\`\` <a id="all-should-be-well"></a>

Go back to the terminal and open the project in Atom.

```bash
atom .
```

**Defining views with React components**

To start working with React as our view library, let's add some NPM packages which will allow us to get started with React.

Open a new terminal in the same directory as your running app, and type:

```bash
meteor npm install --save react react-dom
```

Note: `meteor npm` supports the same features as `npm`, though the difference can be important. Consult the `meteor npm` [documentation](https://docs.meteor.com/commandline.html#meteornpm) for more information.

**Replace the starter code**

To get started, let's replace the code of the default starter app. Then we'll talk about what it does.

First, replace the content of the initial HTML file:

```markup
<head>
  <title>Todo List</title>
</head>

<body>
  <div id="render-target"></div>
</body>
```

Second, replace the contents of `client/main.js` with:

```jsx
import React from 'react';
import { Meteor } from 'meteor/meteor';
import { render } from 'react-dom';

import App from '../imports/ui/App.js';

Meteor.startup(() => {
  render(<App />, document.getElementById('render-target'));
});
```

Now we need to create a new directory called `imports`, a specially-named directory which will behave differently than other directories in the project. Files outside the `imports` directory will be loaded automatically when the Meteor server starts, while files inside the `imports` directory will only load when an import statement is used to load them.

```bash
mkdir imports
cd imports
mkdir ui
touch ui/App.js
```

After creating the `imports` directory, we will create two new files inside it:

```jsx
import React, { Component } from 'react';

import Task from './Task.js';

// App component - represents the whole app
export default class App extends Component {
  // create some sample data. We will replace this later.
  getTasks() {
    return [
      // MongoDB has _id rather than id
      { _id: 1, text: 'This is task 1' },
      { _id: 2, text: 'This is task 2' },
      { _id: 3, text: 'This is task 3' },
    ];
  }

  renderTasks() {
    return this.getTasks().map((task) => (
      // because we're using React we need to provide a key so React knows what has changed
      <Task key={task._id} task={task} />
    ));
  }

  render() {
    return (
      <div className="container">
        <header>
          <h1>Todo List</h1>
        </header>

        <ul>
          // We are call the above function that will go an get the tasks.
          {this.renderTasks()}
        </ul>
      </div>
    );
  }
}
```

Create a new file:

```bash
touch ui/Task.js
```

```jsx
import React, { PureComponent as Component } from 'react';

// Task component - represents a single todo item
export default class Task extends Component {
  render() {
    return (
      <li>{this.props.task.text}</li>
    );
  }
}
```

We just added three things to our app:

* An `App` React component
* A `Task` React component
* Some initialization code \(in our `client/main.js` client JavaScript entrypoint\), in a `Meteor.startup`block, which knows how to call code when the page is loaded and ready. This code loads the other components and renders them into the `#render-target` html element.

You can read more about how imports work and how to structure your code in the [Application Structure article](http://guide.meteor.com/structure.html) of the Meteor Guide.

Later in the tutorial, we will refer to these components when adding or changing code.

**Check the result**

In our browser, the app should _roughly_ look like the following \(though much less pretty\):

#### Todo List

* This is task 1
* This is task 2
* This is task 3

If your app doesn't look like this, use the GitHub link at the top right corner of each code snippet to see the entire file, and make sure your code matches the example.

**HTML files define static content**

Meteor parses all of the HTML files in your app folder and identifies three top-level tags:,, and .

Everything inside anytags is added to the `head` section of the HTML sent to the client, and everything insidetags is added to the `body` section, just like in a regular HTML file.

Everything inside

**Define view components with React**

In React, view components are subclasses of `React.Component` \(which we import with `import { Component } from 'react';`\). Your component can have any methods you like, but there are several methods such as `render` that have special functions. Components can also receive data from their parents through attributes called `props`. We'll go over some of the more common features of React in this tutorial; you can also check out [Facebook's React tutorial](https://facebook.github.io/react/docs/tutorial.html).

**Return markup from the render method with JSX**

The most important method in every React component is `render()`, which is called by React to get a description of the HTML that this component should display. The HTML content is written using a JavaScript extension called JSX, which kind of looks like writing HTML inside your JavaScript. You can see some obvious differences already: in JSX, you use the `className` attribute instead of `class`. An important thing to know about JSX is that it isn't a templating language like Spacebars or Angular - it actually compiles directly to regular JavaScript. Read more about JSX [in the React docs](https://facebook.github.io/react/docs/jsx-in-depth.html).

JSX is supported by the `ecmascript` Atmosphere package, which is included in all new Meteor apps by default.

```css
body {
  font-family: sans-serif;
  background-color: #315481;
  background-image: linear-gradient(to bottom, #315481, #918e82 100%);
  background-attachment: fixed;

  position: absolute;
  top: 0;
  bottom: 0;
  left: 0;
  right: 0;

  padding: 0;
  margin: 0;

  font-size: 14px;
}

.container {
  max-width: 600px;
  margin: 0 auto;
  min-height: 100%;
  background: white;
}

header {
  background: #d2edf4;
  background-image: linear-gradient(to bottom, #d0edf5, #e1e5f0 100%);
  padding: 20px 15px 15px 15px;
  position: relative;
}

#login-buttons {
  display: block;
}

h1 {
  font-size: 1.5em;
  margin: 0;
  margin-bottom: 10px;
  display: inline-block;
  margin-right: 1em;
}

form {
  margin-top: 10px;
  margin-bottom: -10px;
  position: relative;
}

.new-task input {
  box-sizing: border-box;
  padding: 10px 0;
  background: transparent;
  border: none;
  width: 100%;
  padding-right: 80px;
  font-size: 1em;
}

.new-task input:focus{
  outline: 0;
}

ul {
  margin: 0;
  padding: 0;
  background: white;
}

.delete {
  float: right;
  font-weight: bold;
  background: none;
  font-size: 1em;
  border: none;
  position: relative;
}

li {
  position: relative;
  list-style: none;
  padding: 15px;
  border-bottom: #eee solid 1px;
}

li .text {
  margin-left: 10px;
}

li.checked {
  color: #888;
}

li.checked .text {
  text-decoration: line-through;
}

li.private {
  background: #eee;
  border-color: #ddd;
}

header .hide-completed {
  float: right;
}

.toggle-private {
  margin-left: 5px;
}

@media (max-width: 600px) {
  li {
    padding: 12px 15px;
  }

  .search {
    width: 150px;
    clear: both;
  }

  .new-task input {
    padding-bottom: 5px;
  }
}
```

Now that you've added the CSS, the app should look a lot nicer. Check in your browser to see that the new styles have loaded.

**Storing tasks in a collection**

Collections are Meteor's way of storing persistent data. The special thing about collections in Meteor is that they can be accessed from both the server and the client, making it easy to write view logic without having to write a lot of server code. They also update themselves automatically, so a view component backed by a collection will automatically display the most up-to-date data.

You can read more about collections in the [Collections article](http://guide.meteor.com/collections.html) of the Meteor Guide.

Creating a new collection is as easy as calling `MyCollection = new Mongo.Collection("my-collection");`in your JavaScript. On the server, this sets up a MongoDB collection called `my-collection`; on the client, this creates a cache connected to the server collection. We'll learn more about the client/server divide in step 12, but for now we can write our code with the assumption that the entire database is present on the client.

To create the collection, we define a new `tasks` module that creates a Mongo collection and exports it:

```javascript
import { Mongo } from 'meteor/mongo';

export const Tasks = new Mongo.Collection('tasks');
```

Notice that we place this file in a new `imports/api` directory. This is a sensible place to store API-related files for the application. We will start by putting "collections" here and later we will add "publications" that read from them and "methods" that write to them. You can read more about how to structure your code in the [Application Structure article](http://guide.meteor.com/structure.html) of the Meteor Guide.

We need to import that module on the server \(this creates the MongoDB collection and sets up the plumbing to get the data to the client\):

```javascript
import '../imports/api/tasks.js';
```

**Using data from a collection inside a React component**

To use data from a Meteor collection inside a React component, we can use an Atmosphere package `react-meteor-data` which allows us to create a "data container" to feed Meteor's reactive data into React's component hierarchy.

```bash
meteor add react-meteor-data
```

To use `react-meteor-data`, we need to wrap our component in a container using the `withTracker`Higher Order Component:

```jsx
import React, { Component } from 'react';
import { withTracker } from 'meteor/react-meteor-data';

import { Tasks } from '../api/tasks.js';

import Task from './Task.js';

// App component - represents the whole app
class App extends Component {
  renderTasks() {
    return this.props.tasks.map((task) => (
      <Task key={task._id} task={task} />
    ));
  }
// lines 14 to line 27 are skipped
    );
  }
}

// Higher Order Component
export default withTracker(() => {
  return {
    tasks: Tasks.find({}).fetch(), // equivalent to Task.all
  };
})(App);
```

The wrapped `App` component fetches tasks from the `Tasks` collection and supplies them to the underlying `App` component it wraps as the `tasks` prop. It does this in a reactive way, so that when the contents of the database change, the `App` re-renders, as we'll soon see!

When you make these changes to the code, you'll notice that the tasks that used to be in the todo list have disappeared. That's because our database is currently empty — we need to insert some tasks!

**Inserting tasks from the server-side database console**

Items inside collections are called documents. Let's use the server database console to insert some documents into our collection. In a new terminal tab, go to your app directory and type:

```text
meteor mongo
```

This opens a console into your app's local development database. Into the prompt, type:

```javascript
db.tasks.insert({ text: "Hello world!", createdAt: new Date() });
```

In your web browser, you will see the UI of your app _immediately_ update to show the new task. You can see that we didn't have to write any code to connect the server-side database to our front-end code — it just happened automatically.

Insert a few more tasks from the database console with different text. In the next step, we'll see how to add functionality to our app's UI so that we can add tasks without using the database console.

You can add a few more tasks with the same terminal command above.

**Adding tasks with a form**

In this step, we'll add an input field for users to add tasks to the list.

First, let's add a form to our `App` component:

```jsx
<div className="container">
  <header>
    <h1>Todo List</h1>

    // This way of binding can use a lot of memory.
    <form className="new-task" onSubmit={this.handleSubmit.bind(this)} >
      <input
        type="text"
        ref="textInput"
        placeholder="Type your new task here"
      />
    </form>
  </header>

  <ul>
```

_Tip:_ You can add comments to your JSX code by wrapping them in `{/* ... */}`

You can see that the `form` element has an `onSubmit` attribute that references a method on the component called `handleSubmit`. In React, this is how you listen to browser events, like the submit event on the form. The `input` element has a ref property which will let us easily access this element later.

Let's add a `handleSubmit` method to our `App` component:

```jsx
import React, { Component } from 'react';
import ReactDOM from 'react-dom';
import { withTracker } from 'meteor/react-meteor-data';

import { Tasks } from '../api/tasks.js';
...some lines skipped...

// App component - represents the whole app
class App extends Component {
  handleSubmit(event) {
    event.preventDefault();

    // Find the text field via the React ref
    const text = ReactDOM.findDOMNode(this.refs.textInput).value.trim();

    Tasks.insert({
      text, // same as text: text,
      createdAt: new Date(), // current time
    });

    // Clear form
    ReactDOM.findDOMNode(this.refs.textInput).value = '';
  }

  renderTasks() {
    return this.props.tasks.map((task) => (
      <Task key={task._id} task={task} />
```

Now your app has a new input field. To add a task, just type into the input field and hit enter. If you open a new browser window and open the app again, you'll see that the list is automatically synchronized between all clients.

**Listening for events in React**

As you can see, in React you handle DOM events by directly referencing a method on the component. Inside the event handler, you can reference elements from the component by giving them a `ref`property and using `ReactDOM.findDOMNode`. Read more about the different kinds of events React supports, and how the event system works, in the [React docs](https://facebook.github.io/react/docs/events.html).

**Inserting into a collection**

Inside the event handler, we are adding a task to the `tasks` collection by calling `Tasks.insert()`. We can assign any properties to the task object, such as the time created, since we don't ever have to define a schema for the collection.

Being able to insert anything into the database from the client isn't very secure, but it's okay for now. In step 10 we'll learn how we can make our app secure and restrict how data is inserted into the database.

**Sorting our tasks**

Currently, our code displays all new tasks at the bottom of the list. That's not very good for a task list, because we want to see the newest tasks first.

We can solve this by sorting the results using the `createdAt` field that is automatically added by our new code. Just add a sort option to the `find` call inside the data container wrapping the `App`component:

```jsx
export default withTracker(() => {
  return {
    tasks: Tasks.find({}, { sort: { createdAt: -1 } }).fetch(),
  };
})(App);
```

Let's go back to the browser and make sure this worked: any new tasks that you add should appear at the top of the list, rather than at the bottom.

In the next step, we'll add some very important todo list features: checking off and deleting tasks.

**Checking off and deleting tasks**

Until now, we have only interacted with a collection by inserting documents. Now, we will learn how to update and remove them.

Let's add two new elements to our `task` component, a checkbox and a delete button, with event handlers for both:

```jsx
import React, { Component } from 'react';

import { Tasks } from '../api/tasks.js';

// Task component - represents a single todo item
export default class Task extends Component {
  toggleChecked() {
    // Set the checked property to the opposite of its current value
    Tasks.update(this.props.task._id, {
      $set: { checked: !this.props.task.checked },
    });
  }

  deleteThisTask() {
    Tasks.remove(this.props.task._id);
  }

  render() {
    // Give tasks a different className when they are checked off,
    // so that we can style them nicely in CSS
    const taskClassName = this.props.task.checked ? 'checked' : '';

    return (
      <li className={taskClassName}>
        <button className="delete" onClick={this.deleteThisTask.bind(this)}>
          &times;
        </button>

        <input
          type="checkbox"
          readOnly
          checked={!!this.props.task.checked}
          onClick={this.toggleChecked.bind(this)}
        />

        <span className="text">{this.props.task.text}</span>
      </li>
    );
  }
}
```

**Update**

In the code above, we call `Tasks.update` to check off a task.

The `update` function on a collection takes two arguments. The first is a selector that identifies a subset of the collection, and the second is an update parameter that specifies what should be done to the matched objects.

In this case, the selector is just the `_id` of the relevant task. The update parameter uses `$set` to toggle the `checked` field, which will represent whether the task has been completed.

**Remove**

The code from above uses `Tasks.remove` to delete a task. The `remove` function takes one argument, a selector that determines which item to remove from the collection.

**Storing temporary UI data in component state**

In this step, we'll add a client-side data filtering feature to our app, so that users can check a box to only see incomplete tasks. We're going to learn how to use React's component state to store temporary information that is only used on the client.

First, we need to add a checkbox to our `App` component:

```jsx
<header>
  <h1>Todo List</h1>

  <label className="hide-completed">
   <input
     type="checkbox"
     readOnly
     checked={this.state.hideCompleted}
     onClick={this.toggleHideCompleted.bind(this)}
   />
   Hide Completed Tasks
  </label>

  <form className="new-task" onSubmit={this.handleSubmit.bind(this)} >
   <input
     type="text"
```

You can see that it reads from `this.state.hideCompleted`. React components have a special field called `state` where you can store encapsulated component data. We'll need to initialize the value of `this.state.hideCompleted` in the component's constructor:

```jsx

// App component - represents the whole app
class App extends Component {
 constructor(props) {
   super(props);

   this.state = {
     hideCompleted: false,
   };
 }

 handleSubmit(event) {
   event.preventDefault();
```

We can update `this.state` from an event handler by calling `this.setState`, which will update the state property asynchronously and then cause the component to re-render:

```jsx
ReactDOM.findDOMNode(this.refs.textInput).value = '';
}

toggleHideCompleted() {
 this.setState({
   hideCompleted: !this.state.hideCompleted,
 });
}

renderTasks() {
 return this.props.tasks.map((task) => (
   <Task key={task._id} task={task} />
```

Now, we need to update our `renderTasks` function to filter out completed tasks when `this.state.hideCompleted` is true:

```jsx
}

renderTasks() {
  let filteredTasks = this.props.tasks;
  if (this.state.hideCompleted) {
    filteredTasks = filteredTasks.filter(task => !task.checked);
  }
  return filteredTasks.map((task) => (
    <Task key={task._id} task={task} />
  ));
}
```

Now if you check the box, the task list will only show tasks that haven't been completed.

**One more feature: Showing a count of incomplete tasks**

Now that we have written a query that filters out completed tasks, we can use the same query to display a count of the tasks that haven't been checked off. To do this we need to fetch a count in our data container and add a line to our `render` method. Since we already have the data in the client-side collection, adding this extra count doesn't involve asking the server for anything.

```jsx
export default withTracker(() => {
  return {
    tasks: Tasks.find({}, { sort: { createdAt: -1 } }).fetch(),
    incompleteCount: Tasks.find({ checked: { $ne: true } }).count(),
  };
})(App);
```

```jsx
return (
  <div className="container">
    <header>
      // display the count
      <h1>Todo List ({this.props.incompleteCount})</h1>

      <label className="hide-completed">
        <input
```

#### Adding user accounts <a id="adding-user-accounts"></a>

Meteor comes with an accounts system and a drop-in login user interface that lets you add multi-user functionality to your app in minutes.

Currently, this UI component uses Blaze, Meteor's default UI engine. In the future, there might also be a React-specific component for this.

To enable the accounts system and UI, we need to add the relevant packages. In your app directory, run the following command:

```text
meteor add accounts-ui accounts-password
```

**Wrapping a Blaze component in React**

To use the Blaze UI component from the `accounts-ui` package, we need to wrap it in a React component. To do so, let's create a new component called `AccountsUIWrapper` in a new file:

```bash
touch imports/ui/AccountsUIWrapper.js
```

```jsx
import React, { PureComponent as Component } from 'react';
import ReactDOM from 'react-dom';
import { Template } from 'meteor/templating';
import { Blaze } from 'meteor/blaze';

export default class AccountsUIWrapper extends Component {
  componentDidMount() {
    // Use Meteor Blaze to render login buttons in the container
    this.view = Blaze.render(Template.loginButtons,
      ReactDOM.findDOMNode(this.refs.container));
  }
  componentWillUnmount() {
    // Clean up Blaze view
    Blaze.remove(this.view);
  }
  render() {
    // Just render a placeholder container that will be filled in
    return <span ref="container" />;
  }
}
```

Let's include the component we just defined inside App:

```jsx
import { Tasks } from '../api/tasks.js';

import Task from './Task.js';
import AccountsUIWrapper from './AccountsUIWrapper.js';

// App component - represents the whole app
class App extends Component {
...some lines skipped...
            Hide Completed Tasks
          </label>

          <AccountsUIWrapper />

          <form className="new-task" onSubmit={this.handleSubmit.bind(this)} >
            <input
              type="text"
```

Then, add the following code to configure the accounts UI to use usernames instead of email addresses:

```text
touch imports/startup/accounts-config.js
```

```jsx
import { Accounts } from 'meteor/accounts-base';

Accounts.ui.config({
  passwordSignupFields: 'USERNAME_ONLY',
});
```

We also need to import that configuration code in our client side entrypoint:

```jsx
import { Meteor } from 'meteor/meteor';
import { render } from 'react-dom';

import '../imports/startup/accounts-config.js';
import App from '../imports/ui/App.js';

Meteor.startup(() => {
```

**Adding user-related functionality**

Now users can create accounts and log into your app! This is very nice, but logging in and out isn't very useful yet. Let's add two features:

* Only display the new task input field to logged in users
* Show which user created each task

To do this, we will add two new fields to the `tasks` collection:

* `owner` - the `_id` of the user that created the task.
* `username` - the `username` of the user that created the task. We will save the username directly in the task object so that we don't have to look up the user every time we display the task.

First, let's add some code to save these fields into the `handleSubmit` event handler:

```jsx
import React, { Component } from 'react';
import ReactDOM from 'react-dom';
import { Meteor } from 'meteor/meteor';
import { withTracker } from 'meteor/react-meteor-data';

import { Tasks } from '../api/tasks.js';
...some lines skipped...
    Tasks.insert({
      text,
      createdAt: new Date(), // current time
      owner: Meteor.userId(),           // _id of logged in user
      username: Meteor.user().username,  // username of logged in user
    });

    // Clear form
```

Modify the data container to get information about the currently logged in user:

```jsx
return {
  tasks: Tasks.find({}, { sort: { createdAt: -1 } }).fetch(),
  incompleteCount: Tasks.find({ checked: { $ne: true } }).count(),
  currentUser: Meteor.user(),
};
})(App);
```

Then, in our render method, add a conditional statement to only show the form when there is a logged in user:

```jsx
<AccountsUIWrapper />

 { this.props.currentUser ?
   <form className="new-task" onSubmit={this.handleSubmit.bind(this)} >
     <input
       type="text"
       ref="textInput"
       placeholder="Type to add new tasks"
     />
   </form> : ''
 }
</header>

<ul>
```

Finally, add a statement to display the `username` field on each task right before the text:

```jsx
onClick={this.toggleChecked.bind(this)}
/>

  <span className="text">
    <strong>{this.props.task.username}</strong>: {this.props.task.text}
  </span>
</li>
);
}
```

In your browser, add some tasks and notice that your username shows up. Old tasks that we added before this step won't have usernames attached; you can just delete them.

Now, users can log in and we can track which user each task belongs to. Let's look at some of the concepts we just discovered in more detail.

**Automatic accounts UI**

If our app has the `accounts-ui` package, all we have to do to add a login dropdown is render the included UI component. This dropdown detects which login methods have been added to the app and displays the appropriate controls. In our case, the only enabled login method is `accounts-password`, so the dropdown displays a password field. If you are adventurous, you can add the `accounts-facebook`package to enable Facebook login in your app - the Facebook button will automatically appear in the dropdown.

**Getting information about the logged-in user**

In your data container, you can use `Meteor.user()` to check if a user is logged in and get information about them. For example, `Meteor.user().username` contains the logged in user's username. You can also use `Meteor.userId()` to just get the current user's `_id`.

In the next step, we will learn how to make our app more secure by doing data validation on the server.

#### Security with methods <a id="security-with-methods"></a>

Before this step, any user of the app could edit any part of the database. This might be okay for very small internal apps or demos, but any real application needs to control permissions for its data. In Meteor, the best way to do this is by declaring methods. Instead of the client code directly calling `insert`, `update`, and `remove`, it will instead call methods that will check if the user is authorized to complete the action and then make any changes to the database on the client's behalf.

**Removing insecure**

Every newly created Meteor project has the `insecure` package added by default. This is the package that allows us to edit the database from the client. It's useful when prototyping, but now we are taking off the training wheels. To remove this package, go to your app directory and run:

```text
meteor remove insecure
```

If you try to use the app after removing this package, you will notice that none of the inputs or buttons work anymore. This is because all client-side database permissions have been revoked. Now we need to rewrite some parts of our app to use methods.

**Defining methods**

First, we need to define some methods. We need one method for each database operation we want to perform on the client. Methods should be defined in code that is executed on the client and the server - we will discuss this a bit later in the section titled Optimistic UI.

```jsx
import { Meteor } from 'meteor/meteor';
import { Mongo } from 'meteor/mongo';
import { check } from 'meteor/check';

export const Tasks = new Mongo.Collection('tasks');

Meteor.methods({
  // we can't have a dot in a function name unless we have it wrap in quotes.
  'tasks.insert'(text) {
    check(text, String);

    // Make sure the user is logged in before inserting a task
    if (! this.userId) {
      throw new Meteor.Error('not-authorized');
    }

    Tasks.insert({
      text,
      createdAt: new Date(),
      owner: this.userId,
      username: Meteor.users.findOne(this.userId).username,
    });
  },
  'tasks.remove'(taskId) {
    check(taskId, String);

    Tasks.remove(taskId);
  },
  'tasks.setChecked'(taskId, setChecked) {
    check(taskId, String);
    check(setChecked, Boolean);

    Tasks.update(taskId, { $set: { checked: setChecked } });
  },
});
```

Now that we have defined our methods, we need to update the places we were operating on the collection to use the methods instead:

```jsx
// Find the text field via the React ref
const text = ReactDOM.findDOMNode(this.refs.textInput).value.trim();

// This is how we call the function we just created that has a string for the name.
Meteor.call('tasks.insert', text);

// Clear form
ReactDOM.findDOMNode(this.refs.textInput).value = '';
```

```jsx
import React, { Component } from 'react';
import { Meteor } from 'meteor/meteor';

import { Tasks } from '../api/tasks.js';

...some lines skipped...
export default class Task extends Component {
  toggleChecked() {
    // Set the checked property to the opposite of its current value
    Meteor.call('tasks.setChecked', this.props.task._id, !this.props.task.checked);
  }

  deleteThisTask() {
    Meteor.call('tasks.remove', this.props.task._id);
  }

  render() {
```

Now all of our inputs and buttons will start working again. What did we gain from all of this work?

* When we insert tasks into the database, we can now securely verify that the user is logged in, that the `createdAt` field is correct, and that the `owner` and `username` fields are correct and the user isn't impersonating anyone.
* We can add extra validation logic to `setChecked` and `deleteTask` in later steps when users can make tasks private.
* Our client code is now more separated from our database logic. Instead of a lot of stuff happening inside our event handlers, we now have methods that can be called from anywhere.

**Optimistic UI**

So why do we want to define our methods on the client and on the server? We do this to enable a feature we call optimistic UI.

When you call a method on the client using `Meteor.call`, two things happen in parallel:

* The client sends a request to the server to run the method in a secure environment, just like an AJAX request would work
* A simulation of the method runs directly on the client to attempt to predict the outcome of the server call using the available information

What this means is that a newly created task actually appears on the screen before the result comes back from the server.

If the result from the server comes back and is consistent with the simulation on the client, everything remains as is. If the result on the server is different from the result of the simulation on the client, the UI is patched to reflect the actual state of the server.

You can read more about methods and optimistic UI in the [Methods article](http://guide.meteor.com/methods.html) of the Meteor Guide, and our [blog post about optimistic UI](http://info.meteor.com/blog/optimistic-ui-with-meteor-latency-compensation).

