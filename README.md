# elite-set-of-async-utilities

**Zero Dependencies, pure vanilla, slim and compact :wrench:**

This is a small library to implement async collections/control-flow functions created with pure vanilla js. This library contains some of the most used control-flow methods which can be used in performing async operations. I am looking to improve this repo by adding new methods especially using ES6's Async/await utility.

**_NOTE: There are only few async functions available in this repository(which are basically mainstream/common in caolan's), and I will develop more async functions by the time goes. I have not checked extreme/complex test cases as I was just hacking around build this on my own. So shoot out those test cases by creating ISSUE_**

## Installation

[![NPM version](https://img.shields.io/badge/npm-1.1.1-brightgreen.svg)](https://www.npmjs.com/package/elite-async)

Using npm:

```
npm i elite-async
```

Usage

```js
const vanillaAsync = require("elite-async");

// .parallel() example
vanillaAsync.parallel(
  [
    function(callback) {
      setTimeout(function() {
        callback(null, "one");
      }, 3200);
    },
    function(callback) {
      setTimeout(function() {
        callback(null, "two");
      }, 200);
    },
    function(callback) {
      setTimeout(function() {
        callback(null, "three");
      }, 6000);
    },
    function(callback) {
      if (true) {
        setTimeout(function() {
          callback(null, "four");
        }, 200);
      } else {
        setTimeout(function() {
          var err = "Some Error Occured";
          callback(err);
        }, 1000);
      }
    }
  ],
  // optional callback
  function(err, results) {
    if (err) {
      console.log("Err", err);
      return;
    } else {
      console.log("Results ", results);
    }
  }
);
```

## DOCUMENTATION:

I have listed documentation in wiki section - https://github.com/meetzaveri/elite-async/wiki

## Collections/ Control-Flow methods

#### Currently implemented

- [x] .parallel()
- [x] .every()
- [x] .waterfall()
- [x] .filter()
- [x] .auto()
- [x] .each()
- [x] .series()

#### Will implement

- [ ] .eachSeries()
- [ ] .reduceRight()

## Utility overview : Seeking what's behind the `.auto()` function

With help of this article - http://ketangupta.in/blog/development/2018/01/19/async-auto/ I came to know that it's that traditional DFS algorithm which pioneered this `.auto()` implementation.

#### Code

```js
auto(
  {
    get_data: function(callback) {
      console.log("in get_data");
      // async code to get some data

      setTimeout(function() {
        callback(null, "data", "converted to array");
      }, 3000);
    },
    make_folder: function(callback) {
      console.log("in make_folder");
      // async code to create a directory to store a file in
      // this is run at the same time as getting the data

      setTimeout(function() {
        callback(null, "folder");
      }, 2000);
    },
    write_file: [
      "get_data",
      "make_folder",
      function(results, callback) {
        console.log("in write_file", JSON.stringify(results));
        // once there is some data and the directory exists,
        // write the data to a file in the directory
        callback(null, "filename");
      }
    ],
    make_file: [
      "write_file",
      function(results, callback) {
        console.log("in make_file", JSON.stringify(results));
        // now produce file by having it
        callback(null, { file: results, email: "user@example.com" });
      }
    ]
  },
  function(err, results) {
    if (err) console.log("err = ", err);
    else console.log("results = ", results);
  }
);
```

#### Overview of .auto()

![Imgur](https://i.imgur.com/XDFKjMU.png)

## Want to contribute

- Take out pull from development branch and start hacking

## Motivation

I know callbacks and iterator functions were the wizards behind this, I just need to figure out what's behind the curtain. I started with pure vanilla JS to crack it up, so that I can implement on my own.

### Todo

- [ ] Improve Error handling
- [ ] More useful functions using ES6's async/await
