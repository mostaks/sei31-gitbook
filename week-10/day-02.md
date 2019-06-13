---
description: Vue.js + Node.js
---

# Day 02

#### What we covered today:

* [Vue.js - Intro](https://github.com/wofockham/sei-31/tree/master/13-advanced/vue-intro)
* [Node.js - Intro](https://github.com/wofockham/sei-31/tree/master/13-advanced/node-intro)
* [Express - Intro](https://github.com/wofockham/sei-31/tree/master/13-advanced/express-intro)

#### Warmup

* [Happy Numbers](https://github.com/Yiannimoustakas/sei31-homework/tree/master/warmups/week10/day02_happy_numbers)

## Vue.js

#### What is it? <a id="what-is-it"></a>

Vue \(pronounced /vjuː/, like view\) is a progressive framework for building user interfaces. Unlike other monolithic frameworks, Vue is designed from the ground up to be incrementally adoptable.

The core library is focused on the view layer only, and is easy to pick up and integrate with other libraries or existing projects. On the other hand, Vue is also perfectly capable of powering sophisticated Single-Page Applications when used in combination with modern tooling and supporting libraries.

#### Lets get started <a id="lets-get-started"></a>

Add the vue-cli globally:

```bash
  ~$ npm install -g @vue/cli 
```

#### Docs: <a id="docs"></a>

The Vue documentation is really easy to follow. I'm not going to waste everyone's time regurgitating it, so go to the [source](https://vuejs.org/v2/guide/#Getting-Started)!

## Node.js <a id="npm-introduction"></a>

​[Node](https://nodejs.org/en/) is an open-source, cross-platform JavaScript engine that's mostly used to create web servers using JavaScript. It was written in 2009 by Ryan Dahl while he was working at Joyent.

### JavaScript engines​ <a id="javascript-engines"></a>

A JavaScript engine is a compiler that turns JavaScript into machine code for microprocessors - typically, it will turn it into things like:

* **IA32** - Intel Architecture 32 bit
* **x86-64** - A 64 bit version of the Intel Architecture
* **ARM** - Acorn Reduced Instruction Set Computing \(RISC\) Machine
* **MISP ISAs** - Microprocessor without Interlocked Pipeline Stages Instruction Set Architecture

​Here are a few JavaScript engines: ​

* **SpiderMonkey** - Firefox, Netscape Navigator - the first JavaScript Engine
* **V8** - Chrome, Opera
* **Chakra** - Internet Explorer
* **Nitro** - Safari

**Node** is the V8 JavaScript engine, taken out of a browser, with a little bit extra. It lets us write server-side code in JavaScript, allowing us to do the same sorts of things we do with Ruby.

## _NodeJS - JavaScript Engines - Further Reading_ <a id="nodejs-javascript-engines-further-reading"></a>

* ​[Wikipedia - JavaScript Engines](https://en.wikipedia.org/wiki/JavaScript_engine#JavaScript_engines)​

## Why NodeJS? <a id="why-nodejs"></a>

### _What is Node good at?_ <a id="what-is-node-good-at"></a>

* Streaming data
* "Soft" real-time
* Every developer knows a bit of JS
* "Tooling"
* Unopinionated and flexible
* It can be isomorphic
* Corporate backing
* Performant
* Good community - easy to find developers

### _What is Node not so good at?​_ <a id="what-is-node-not-so-good-at"></a>

* "Tooling" - too much out there
* Being opinionated
* Memory leaks
* Dependency injection
* Serving static files \(like HTML\)
* Providing structure
* Generic JavaScript woes \(callbacks, etc.\)
* Allowing easy debugging
* Producing light-weight codebases
* Providing functionality out of the box
* Getting away from fancy buzzwords

## Installation and Usage <a id="installation-and-usage"></a>

### _Installation_ <a id="installation"></a>

```bash
# Initial installation using Homebrew
brew install node
# Upgrading installed version using Homebrew
brew upgrade node
```

### _Usage_ <a id="usage"></a>

```bash
# Open a Node REPL
node
# Run a Node application
node someFileName.js
```

## _NPM - Introduction_ <a id="npm-introduction"></a>

### Usage <a id="usage-1"></a>

We haven't gotten into Webpack yet, but this is an example of how we would create a new NodeJS application.

### _Initialize an NPM project_ <a id="initialize-an-npm-project"></a>

Running this command will create a package.json file for your NodeJS application. This describes your application, and allows NPM to manage your application's dependencies.

```bash
npm init
```

We can also skip the 'wizard'-style Q&A process with the -y flag.

```bash
npm init -y
```

### _Install packages_ <a id="install-packages"></a>

This will install Webpack and all its dependencies. The --save flag will also add webpack to your package.json.

```bash
npm install webpack --save
```

### _Update packages_ <a id="update-packages"></a>

Periodically, you should update the packages on which your application depends. This will update all local packages in your package.json file \(and all their dependencies\).

```bash
npm update
```

### _Further Reading_ <a id="further-reading"></a>

* ​[NPM](https://www.npmjs.com/)​
* ​[NPM - Introduction](https://docs.npmjs.com/getting-started/what-is-npm)​
* ​[NodeSource - Understanding NPM](https://nodesource.com/)​
* ​[NPM Dependency Tree - Example: ExpressJS](http://npm.anvaka.com/#/view/2d/express)​

## Creating a simple Node server <a id="creating-a-simple-node-server"></a>

Create a new directory and cd into it then open in Atom.

```bash
mkdir simple-node-server
cd simple-node-server/
touch index.js
atom .
```

Add console log into the `index.js` file you've just created.

```javascript
console.log(`hello world`);
```

Run the program by using the below code in the terminal. Make sure you're in the `simple-node-server/` directory.

```bash
node index.js
```

You should be able to see the console log in the terminal `hello world.`

Go back to the `index.js` file and remove the console log as it was only used to make sure we have a connection. Define a variable and require 'http'

```javascript
const http = require('http');
```

Create a connection to the server with a 'fat arrow' function that way we can retain the value of `this`.

```javascript
http.createServer( (request, response) => {
​
});
```

This is a direct comparison of the old way of writing the function.

```javascript
http.createServer( function(request, response){
​
});
```

Because node is unopinionated we need to specify our requests and an end to the response.

```javascript
http.createServer( (request, response) => {
  response.writeHead(200, { 'Content-Type': 'test/plain' }); //telling browser this is not HTML its just plain text.
  response.end('Hello World');
});
```

We also have to tell the server to start with a 'listen' promise chained to the end of the function. This will watch port 8000.

```javascript
http.createServer( (request, response) => {
  response.writeHead(200, { 'Content-Type': 'test/plain' }); //telling browser this is not HTML its just plain text.
  response.end('Hello World');
}).listen(8000);
```

Unfortunately, Node doesn't log anything to the console so we have no way of knowing what's happening. We can get around this by placing console logs in our code.

```javascript
const http = require('http');
​
http.createServer( (request, response) => {
  response.writeHead(200, { 'Content-Type': 'test/plain' }); //telling browser this is not HTML its just plain text.
  response.end('Hello World');
}).listen(8000);
​
console.log('Server now running at http://localhost:8000');
```

Now go back to the terminal an run the server

```javascript
node index.js
```

Add some additional logs to let us know when someone is trying to request a page.

```javascript
http.createServer( (request, response) => {
  console.log( 'Page requested');
  response.writeHead(200, { 'Content-Type': 'test/plain' }); //telling browser this is not HTML its just plain text.
  response.end('Hello World');
}).listen(8000)
```

### Further Reading <a id="further-reading-1"></a>

* ​[NodeJS](https://nodejs.org/docs/latest-v9.x/api/synopsis.html)​
* [Vue.js Examples](https://vuejs.org/v2/examples/)

## Express

Express is a light-weight web application framework to help organise your web application into an MVC architecture on the server side. You can use a variety of choices for your templating language \(like EJS, Jade, and Dust.js\). EJS would be a similar syntax to what we're use to with Rails erb.html files.

You can then use a database like MongoDB with Mongoose \(for modeling\) to provide a backend for your Node.js application. Express.js basically helps you manage everything, from routes, to handling requests and views.

### _Installing Express_ <a id="installing-express"></a>

Navigate to the root directory of your project and install express.

```bash
npm install express
```

`require` function is the easiest way to include modules that exist in separate files. The basic functionality of `require` is that it reads a javascript file, executes the file, and then proceeds to return the exports object.

```javascript
const express = require('express');
const app = express();

const server = app.listen(3000, () => {
  console.log("Server listening on port 3000...");
});
```

Now run `node server.js` from the terminal,  we should be able to get the following output:

```javascript
ba-express-server-no-db$ node server.js
Server listening on port 3000...
```

### Enable server restart on file changes <a id="enable-server-restart-on-file-changes"></a>

Any changes you make to your Express website are currently not visible until you restart the server. It quickly becomes very irritating to have to stop and restart your server every time you make a change, so it is worth taking the time to automate restarting the server when needed. One of the easiest such tools for this purpose is [nodemon](https://github.com/remy/nodemon). This is usually installed globally \(as it is a "tool"\), but here we'll install and use it locally as a developer dependency, so that any developers working with the project get it automatically when they install the application. Use the following command in the root directory for the skeleton project:

```bash
npm install --save nodemon
```

## Homework

* Create an express.js application that fetches data from an API. Something like the Google Books book cover search we did in Sinatra and Rails, but use any API of your choice
* Try more Learnyounode modules
* Start on your YouTeach!!!

