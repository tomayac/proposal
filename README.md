# Proposal
[![npm version](https://badge.fury.io/js/proposal.svg)](http://badge.fury.io/js/proposal)
[![travis build information](https://api.travis-ci.org/vinniegarcia/proposal.svg)](https://travis-ci.org/vinniegarcia/proposal)

Callback to Promise converter. A `Proposal` is a bridge between node-style callbacks and ECMAScript 6 `Promise`s.

## Installation

```
npm i proposal --save
```

## Usage
`Proposal(callbackFn[, args])` - takes a node-style callback (`err, data` parameters) and converts it into a Promise.

If arguments are supplied, the callbackFn is executed and a Promise is returned. Use it like any other Promise you've used before, with `.then()` and `.catch()`.

If no arguments are supplied, Propsal will return a function that, when executed with its parameters, will then return a Promise. This is useful if, for example, you want to execute that function multiple times with different parameters.

## Examples

### 1. Create a Proposal

A Proposal is a bridge function between node-style function/callbacks and Promises. You create it by calling `Proposal()` with 1 argument: the function you'd like to convert.
```javascript
var fs = require('fs'),
  Proposal = require('proposal'),
  readProposal = Proposal(fs.readFile);
```
At this point, readProposal is a function, that when invoked with fs.readFile's parameters, will return a Promise containing the result of the file read operation. We'll use this Proposal twice below, once to read the system's HOSTS file and again to read the system's Apache configuration file.
```javascript
var path = require('path'),
  hostsFile = path.resolve('/etc/hosts'),
  apacheConfig = path.resolve('/etc/httpd/httpd.conf'),
  hostsRead = readProposal(hostsFile),
  apacheRead = readProposal(apacheConfig);

hostsRead.then(function (txt) {
  //do stuff with txt
})
.catch(function (err) {
  //handle the error
});

//apacheRead is also available as a Promise here
```

### 2. Create a Promise containing the result of a file read

You do this by passing the node-style function's arguments when you invoke Proposal.

```javascript
var fs = require('fs'),
  path = require('path'),
  Proposal = require('proposal'),
  filepath = path.resolve('data/example.json'),
  readFile = Proposal(fs.readFile, filepath);

  readFile.then(function (data) {
    //do stuff with data
  })
  .catch(function (err) {
    //handle the error
  });
```

## Questions?
Raise an issue and I'll get to it as soon as I can. Thanks for reading this far!
