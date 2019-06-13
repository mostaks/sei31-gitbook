---
description: >-
  "Programming is not easy like Sunday morning, it is silent poetry." - Waseem
  Latif
---

# Day 03

What we covered today.

* Git related stuffs
* Javascript
  * Conditionals
  * Loops
  * Arrays

### Slides <a id="slides"></a>

* [Control Flows](https://www.teaching-materials.org/javascript/slides/controlflow) \(slide 1-10\)
* [Loops](http://www.teaching-materials.org/javascript/slides/controlflow.html) \(slide 11-14\)
* ​[Arrays](http://www.teaching-materials.org/javascript/slides/controlflow.html) \(slide 15 onwards\)

### Classwork

* [Loops Exercises](https://github.com/wofockham/wdi-30/tree/master/01-javascript/js-loops)
* [Arrays Exercises](https://github.com/wofockham/wdi-30/tree/master/01-javascript/js-arrays)

## Our very first warmup! 🤘🏻 <a id="our-very-first-warmup"></a>

* ​[​](https://github.com/liaa2/wdi29-homework/tree/master/warmups/week01/day03_raindrops)[Raindrops](https://github.com/Yiannimoustakas/sei31-homework/tree/master/warmups/week01/day03_Raindrops)

## Git and Github <a id="git-and-github"></a>

### Introduction <a id="introduction"></a>

A Version Control System designed and developed by a Finnish guy, Linus Torvalds, for the Linux kernel in 2005. Apparently, he named it Git because of the British-English slang meaning "unpleasant person". Torvalds said: "I'm an egotistical bastard, and I name all my projects after myself".

#### _What does it do?_ <a id="what-does-it-do"></a>

* A way to snapshot - you get a time machine
* A way to collaborate - working in parallel
* Saves us from moving code around on Floppy Disks
* Automates merging of code
* Lets you distribute a project

N.B. It is quite hard to understand, and you won't understand most of the problems that it solves yet! Just trust us.

**NOTE:** `GitHub !== Git`. Git is on your local computer. GitHub is one of several web-based Git repository hosting services.

Some basic Git commands:

* `git init` - This initializes the git repository \(behind the scenes it creates a .git file in the folder - it has called .git because any file prefixed by a . is hidden in finder and ls\). Make sure you don't do this in another git repository!
* `git status` - This will tell you what is happening the current project. Commit status, untracked files etc.
* `git add .` - This will add all files in the current directory into the "staging area". Git is now paying attention to all files. You can alternatively specify a particular file - `git add octocat.txt`. We could also add all files of a particular type - `git add '*.txt'` \(this needs quotes!\) or multiple files at a time - `git add octocat.txt blue_octocat.txt`.
* `git commit -m "You're commit message"` - This is the thing that takes the snapshot. You must give it a message!
* `git log` - Will show you all of the snapshots in the current project
* This has all been happening locally! So we need to connect it to Github...
* `git remote add origin https://github.com/try-git/try_git.git` - It takes a _remote name_, the word origin, and a _remote repository_, the URL.
* `git push -u origin master` - The first time you run git push \(which moves it up to github\), make sure you specify `-u origin master`. This tells your local machine to always use the remote link you specified in the step before. Once you have done this - you can run `git push` from now on
* `git pull` - this brings the code down from Github
* `git diff HEAD` - this will show the differences between the current commit and the previous commit
* `git diff --staged` - You can show the differences between the currently staged files by using this command
* `git reset FILENAME` - Will remove the specified files or folders from the staging area \(so they won't be pushed!\)
* `git checkout -- octocat.txt` - This will go back to the last commit involving the octocat.txt file using this.
* `git branch clean_up` - This allows us to work in an alternate reality - changes you make here won't effect anything in other branches
* `git checkout clean_up` - Will move to the clean\_up branch.
* `git rm FILENAME FILENAME` - Will not only delete the file on your computer, but also remove it from the staging area with Git.
* `git add .` then run `git commit -m "COMMIT NAME"`. This will take a screenshot of the current branch position.
* `git checkout master` - This will move to the master branch.
* `git merge clean_up` - Will merge the clean\_up branch into the current branch.
* Now that everything has been merged, we can delete the clean\_up branch - `git branch -d clean_up`.
* We can run `git push` now! Everything is up there.

**A really basic process:** \(Follow these steps every time!\)

* `git add .`
* `git commit -m "commit message"`
* `git pull`
* `git push`

### Homework repository <a id="homework-repository"></a>

For notes on submitting your homework, refer to the README file in my repository

* [SEI-31 Homework Repository](https://github.com/Yiannimoustakas/sei31-homework)

### _Git and GitHub - Introduction - Tutorials_ <a id="git-and-github-introduction-tutorials"></a>

* ​[CodeSchool - TryGit](https://try.github.io/levels/1/challenges/1) - a great interactive, introductory GitHub tutorial.
* ​[A tutorial by the folks at Atlassian](https://www.atlassian.com/git/tutorials/setting-up-a-repository/)​
* ​[A tutorial by Roger Dudler](https://rogerdudler.github.io/git-guide/)​
* ​[A tutorial by Git Tower](https://www.git-tower.com/learn/)​

## Javascript - Question Time: What is the difference between console.log and return in a function? <a id="javascript-question-time-what-is-the-difference-between-console-log-and-return-in-a-function"></a>

If you think of programming as a conversation between you and the machine then: `console.log`is a way for the machine to tell you, the programmer something and `return` is a mechanism for the machine to tell another part of its program something.

`console.log()` is used to print information to the console. `return` on the other hand is a call to pass some value back up to where the call was made. For example, we create a function called `addNumbers()` that takes 2 numeric parameters and returns the sum. Your function would look like the following:

```javascript
const addNumbers = function(num1, num2) {
  return num1 + num2;
};
```

You would call your function like this: `let sum = addNumbers(5, 3);`

This would set `sum` equal to the value returned by `addNumbers()` but will not print it to the console. Hence, the `return` statement is passing the value `num1 + num2` back to the call of `addNumbers`. If you want to view the value of sum, you would write: `console.log(sum);` or to simplify it you could write: `console.log(addNumers(5, 3));`

Note that the `return` statement exits the function and the next code statements will not run.

```javascript
const addNumbers = function(num1, num2) {
  return num1 + num2;
  console.log(`${num1 + num2}`); //this line of code would not run
};
```

## Javascript Control Flow: Conditionals <a id="javascript-control-flow-conditionals"></a>

We can use an **if statement** to tell Javascript which statements to execute, based on a condition.

If the condition \(the things within the parentheses\) evaluates to true, it will run whatever is within the curly brackets. Otherwise, it will skip over the code in the curly brackets:

### if <a id="if"></a>

```javascript
const x = 5;​
​
if ( x > 0 ) {  
// This will run unless the variable x has a value less than or equal to zero.  
  console.log( "x is a positive number!" );
}
// => "x is a positive number!"
```

### if / else <a id="if-else"></a>

We can give if statements a fallback using **else** - if the condition in the if statement is not met, the code within the else block will be executed:

```javascript
const age = 28;
if (age > 16) {  
  console.log('Yay, you can drive!');
} else {  
  console.log('Sorry, but you have ' + (16 - age) + ' years til you can drive.');
}
// => 'Yay, you can drive!'le.log('Sorry, but you have ' + (16 - age) + ' years til you can drive.');}// => 'Yay, you can drive!'
```

With an `if` / `else` pattern, Javascript will:

* Always run one of the statements; and
* Never run both of the statements.

### if / else if / else <a id="if-else-if-else"></a>

You can also use **else if** if you have multiple, exclusive conditions to check - this is similar to chaining multiple if statements:

```javascript
const age = 20;
if (age >= 35) {  
  console.log('You can vote AND hold any place in government!');
} else if (age >= 25) {  
  console.log('You can vote AND run for the Senate!');
} else if (age >= 18) {  
  console.log('You can vote!');
} else {  
  console.log('You have no voice in government!');
}
// => "You can vote!"
```

### _Javascript - If statements - Exercise_ <a id="javascript-collections-arrays-exercise"></a>

* [​if/else statements exercises](https://gist.github.com/wofockham/b347465420654df63be7)

## Javascript - Control Flow: Loops <a id="javascript-control-flow-loops"></a>

### The 'While' Loop <a id="the-while-loop"></a>

The **while loop** will tell Javascript to repeat the statements within the curly brackets until a condition is true. **WARNING:** it is _ridiculously_ easy to make infinite loops with these - they'll crash your browsers and crush your spirits.

You need a condition in the parentheses, and you need something within the body \(between the curly brackets\) that will eventually change the condition to be false.

```javascript
let x = 0; //init​
​
while (x < 5) { //conditional  
  console.log(x);  
  x += 1; // same as: x = x + 1; //update
}
// => 0
// => 1
// => 2
// => 3
// => 4
```

What would have happened if we didn't increment `x` each iteration? Or if we _decremented_ `x` each iteration? The condition \(`x < 5`\) would never be false - the condition would be true until the heat death of the universe. That is, you'd be stuck in an infinite loop.

### The 'For' Loop <a id="the-for-loop"></a>

A **for loop** is another, more specialized way of repeating statements.

It looks like the following:

```javascript
// 'For loop' syntax:
for (/*initialize*/; /*condition*/; /*update*/) {  
  /* code to execute*/
}​
​
// Example of a 'for loop':
for (let i = 0; i < 5; i = i + 1) {    
  console.log( i );
}
// => 0 1 2 3 4
​
​// The equivalent 'while loop' would be:
let i = 0; // This is the 'initialize' value
while (i < 5) { // This is the 'condition' (in the parentheses)    
  console.log(i);    
  i = i + 1; // This is the 'update'
}
// => 0 1 2 3 4
```

### The 'Break' Statement <a id="the-break-statement"></a>

To prematurely exit any loop \(`for` or `while`\), use can use the **break** statement:

```javascript
for (let current = 100; current < 200; current++) {    
  // 'current++'' is the same 'current += 1' and 'current = current + 1'    
  // 'current--'' also exists ('current = current - 1')    
  // This is called 'syntactic sugar'​    
​
  console.log('Testing ' + current);    
  if (current % 7 === 0) {        
    // The % stands for the modulus operator, it finds the remainder        
    console.log('Found it! ' + current);        
    break;    
  }
}
```

### _Control Flow in Javascript: Loops - Exercises_ <a id="control-flow-in-javascript-loops-exercises"></a>

* [​Loops Exercises​](https://gist.github.com/wofockham/57b6d4f81cf794de078a)

### Recursion \(Basics\) <a id="recursion-basics"></a>

We will get into this a lot more, but basically - we can call functions within functions. This allows us to recreate loops using **if statements**!

See [the Countdown Example](http://repl.it/l50).

## Javascript - Collections <a id="javascript-collections"></a>

In addition to the primitive data types \(strings, numbers, booleans, undefined, null\), we have **arrays**, **objects** and **regular expressions**. We're going to leave regular expressions for the time being and take a look at the **collections**: arrays and objects. Collections are types of data that allow us to store and access collections of values.

## Javascript - Collections - Arrays <a id="javascript-collections-arrays"></a>

An array is a type of data that holds an ordered list of values. The values can be of any data type, and an array can include multiple data types. Arrays are sequences of elements that can be accessed via integer indices starting at zero. More or less, an array is a special variable that can hold more than one value at a time.

**NOTE:** Don't forget - arrays are zero-indexed!

### How to create an array <a id="how-to-create-an-array"></a>

* `let testArray = [ 1, 2, 3 ];` - This is an array literal - definitely the way you should do it.
* `let testArray = new Array( 1, 2, 3 );` - Uses the Array constructor and the keyword new. Does the same thing, but stick to the other way.

### How to access elements in an array <a id="how-to-access-elements-in-an-array"></a>

```javascript
let amazingFrenchAuthors = [ 
  "Alexandre Dumas", "Gustave Flaubert", "Voltaire", "Marcel Proust", 
  "Jean-Paul Sartre", "Stendhal", "Anäis Nin", "Simone de Beauvoir", 
  "Rene Descartes", "Montesquieu" 
];​
​
console.log( amazingFrenchAuthors[0] ); // => "Alexandre Dumas"
console.log( amazingFrenchAuthors[3] ); // => "Marcel Proust"​
​
// You can use variables and expressions to access elements in arrays as well!
let theBestOfTheBest = 4;
console.log( amazingFrenchAuthors[ theBestOfTheBest ] ); // Logs "Jean-Paul Sartre"
console.log( amazingFrenchAuthors[ amazingFrenchAuthors.length - 1 ] ); // Logs "Montesquieu" (the last element)​
​
// This will turn a string into an array, with each element defined by the space
let bros = "Groucho Harpo Chico Zeppo".split(" ");
console.log(bros); // => ["Groucho", "Harpo", "Chico", "Zeppo"]
```

### How to iterate through elements in an array <a id="how-to-iterate-through-elements-in-an-array"></a>

Stick to the **for loop** in most cases, but there are always thousands ways of doing something in Javascript.

```javascript
let greatPeople = [ 
  "Louis Pasteur", "Jacques Cousteau", "Imhotep", "Sigmund Freud", 
  "Wolfgang Amadeus Mozart" 
];​
​
for ( let i = 0; i < greatPeople.length; i++ ) {    
  console.log( greatPeople[ i ] ); // Logs out the "i-th" element each iteration
}​​
​
[ 'a', 'b', 'c' ].forEach( function (elem, index) {    
  console.log(index + '. ' + elem);
});
// => 0. a
// => 1. b
// => 2. c
```

### _Javascript - Arrays - Exercise_ <a id="javascript-collections-arrays-exercise"></a>

* ​[Array Exercises​](https://gist.github.com/ga-wolf/4f200a907382aa53dc9e794d1ee18b3c)

### How to set elements in an array <a id="how-to-set-elements-in-an-array"></a>

```javascript
let amazingFrenchAuthors = [ 
  "Alexandre Dumas", "Gustave Flaubert", 
  "Voltaire", "Marcel Proust"
];
​
​amazingFrenchAuthors[0] = "Stendhal"; // Just access them and reassign!
​
​console.log( amazingFrenchAuthors );
// Logs [ "Stendhal", "Gustave Flaubert", "Voltaire", "Marcel Proust"]
```

### Common methods and properties for arrays <a id="common-methods-and-properties-for-arrays"></a>

```javascript
let amazingFrenchAuthors = [ 
  "Alexandre Dumas", "Gustave Flaubert", "Voltaire", "Marcel Proust"
];​
// In the examples below, assume we're starting from the original array each time.​
​
console.log( amazingFrenchAuthors.length ); 
// Returns 4 - the length property doesn't use a zero index​
​
// POP //
amazingFrenchAuthors.pop(); 
// Removes the last element from an array and returns that element.
// => "Marcel Proust"
​
​// PUSH //
amazingFrenchAuthors.push("Jean Paul Sartre"); 
// Adds one or more elements to the end of an array and returns the new length of the 
//array.
// => 5​
​
// REVERSE //
amazingFrenchAuthors.reverse(); 
// Reverses the order of the elements of an array — the first becomes the last, and 
//the last becomes the first.
// END REVERSE //​
​
// SHIFT //
amazingFrenchAuthors.shift(); 
// Removes the first element from an array and returns that element.
// END SHIFT //
​
​// UNSHIFT //
amazingFrenchAuthors.unshift("Gustave Flaubert"); 
// Adds one or more elements to the front of an array and returns the new length of 
the array.
// END UNSHIFT //
​
​// JOIN //
amazingFrenchAuthors.join(); // Joins all elements of an array into a string.
// END JOIN //​
​
// SPLICE //
amazingFrenchAuthors.splice(); // Adds and/or removes elements from an array.
amazingFrenchAuthors = [
  "Alexandre Dumas", "Gustave Flaubert", "Voltaire", "Marcel Proust"
];
amazingFrenchAuthors.splice(1,1);
// => ["Gustave Flaubert"]
// amazingFrenchAuthors is now ["Alexandre Dumas", "Voltaire", "Marcel Proust"]​
​
amazingFrenchAuthors.splice(1,1, "Gustave Flaubert");
// Returns the deleted items, and adds in the next parameters ["Voltaire"]
// amazingFrenchAuthors is now ["Alexandre Dumas", "Gustave Flaubert", "Marcel Proust"]
// END SPLICE //
​
​amazingFrenchAuthors = ["Alexandre Dumas", "Gustave Flaubert", "Marcel Proust"];​
​
// INCLUDE //
amazingFrenchAuthors.includes( "Alexandre Dumas" ); // Returns true.
amazingFrenchAuthors.includes( "Montesquieu" ); // Returns false.
// END INCLUDE //
​
​// INDEX OF //
amazingFrenchAuthors.indexOf("Alexandre Dumas"); 
// Returns 0 (ie, the first element in the array)
amazingFrenchAuthors.indexOf("Anäis Nin"); 
// indexOf returns -1 if it cannot find a match.
// END INDEX OF //
```

### _Javascript - Collections - Arrays - Recommended Readings_ <a id="javascript-collections-arrays-recommended-readings"></a>

* ​[SitePoint - Manipulate Arrays in Javascript](http://www.sitepoint.com/quick-tip-create-manipulate-arrays-in-javascript/)​

## Homework <a id="homework"></a>

* [​Array and Functions / Word Guesser](https://gist.github.com/wofockham/61148df9403b3cfc2138)
* Bonus - Read:
  * ​[MDN - Arrays](https://developer.mozilla.org/en-US/docs/Web/JavaScript/Reference/Global_Objects/Array) - MDN is an amazing resource for Javascript, HTML and CSS.
  * ​[Javascript Tutorials - Arrays](http://javascript.info/tutorial/array)​
  * ​[Speaking Javascript - Arrays](http://speakingjs.com/es5/ch01.html#basic_arrays)​

## Miscellaneous Links <a id="miscellaneous-links"></a>

* ​[Idiomatic Javascript style rules](https://github.com/rwaldron/idiomatic.js/)​
* ​[How to add colours and styles to your console.logging](https://developers.google.com/web/tools/chrome-devtools/debug/console/console-write#string-substitution-and-formatting)​
* [Cranky old man yelling about Javascript](https://www.youtube.com/watch?v=hQVTIJBZook)​
* ​[Variables and Assignment](http://speakingjs.com/es5/ch01.html#_variables_and_assignment)​
* ​[Eloquent Javascript - Values, Types, and Operators](http://eloquentjavascript.net/01_values.html)​

