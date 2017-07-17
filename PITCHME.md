# From Callbacks to Async/Await

#HSLIDE

#### Ways to control Asynchrony in JS

* Callbacks
* Promises (ES6 - ES2015)
* Async/Await (ES7 - ES2016)


#HSLIDE

#### Callbacks

This is the most common way to control asynchrony

```
function addToArray (data, array, callback) {  
  if (!array) {
    callback(new Error('Array does not exist'), null)
  } 
  setTimeout(function() { 
    array.push(data)
    callback(null, array)
  }, 1000)
}

var array = [1,2,3];

addToArray(4, array, function (err) {  
  if (err) return console.log(err.message)
  console.log(array)
})

// [1, 2, 3, 4]
```

@[1-9]
@[5-8]
@[11-16]
@[18]


#HSLIDE

#### With no callbacks

```
function addToArray (data, array) {  
  setTimeout(function() { 
    array.push(data)
  }, 1000)
}

var array = [1, 2, 3]  
addToArray(4, array)  
console.log(array)

// [1, 2, 3]
```

@[1-5]
@[7-9]
@[11]


#HSLIDE

#### Callbacks PROBLEM 

![Logo](callbacks.png)


#HSLIDE

### Callback Hell o Pyramid of Doom

```
var array = [1,2,3]

addToArray(4, array, function (err) {  
  if (err) ...
  addToArray(5, array, function (err) {
    if (err) ...
    addToArray(6, array, function (err) {
      if (err) ...
      addToArray(7, array, function () {
        // to infinity and beyond...
      })
    })
  })
})
```

@[3,5,7,9]
@[10]


#HSLIDE

![Logo](callbackhell.png)


#HSLIDE

### Three Simple Rules for Escaping Callback Hell

1. Keep your code shallow
  - Use named functions for callbacks
  
2. Modularize
  - Nest functions when you need to capture (enclose) variable scope
  
3. Handle every single error
  - Use return when invoking the callback
  
  
#HSLIDE

### Escaping Callback Hell (Examples)

[Example1](http://callbackhell.com/)

[Example2](http://51elliot.blogspot.com.es/2014/11/three-simple-rules-for-escaping.html)

### Escaping Callback Hell (Libraries)

[Async](https://www.npmjs.com/package/async)


#HSLIDE

#### Promises

```
function addToArray (data, array) {  
  const promise = new Promise(function (resolve, reject) {
    if (!array) {
      reject(new Error('Array does not exist'))
    }
  
    setTimeout(function() {
      array.push(data)
      resolve(array)
    }, 1000)
  })

  return promise
}

const array = [1, 2, 3] 

addToArray(4, array).then(function () {  
  console.log(array)
})
```

@[2-11]
@[13]
@[18-20]


#HSLIDE

### Callback Hell Solution - Nested Promises

```
const array = [1, 2, 3]  

addToArray(4, array)  
  .then(function() { return addToArray(5, array) })
  .then(function() { return addToArray(6, array) })
  .then(function() { return addToArray(7, array) })
  .then(function () {
    console.log(array)
  })

// (4 seg. de delay)-> [1,2,3,4,5,6,7] 
```

@[3-9]
@[11]


#HSLIDE

### Promises errors

```
const array = ''  
addToArray(4, array)  
  .then(...)
  .then(...)
  .then(...)
  .catch(err => console.log(err.message))

// Array does not exist
```

@[2-6]
@[8]


#HSLIDE

#### Promisify libraries

[Bluebird](http://bluebirdjs.com/docs/api/promisification.html)

[ES6-Promisify](https://www.npmjs.com/package/es6-promisify)


#HSLIDE

#### Async/Await

Quick intro:

 * Is a new way to write asynchronous code. 
 
 * Is actually built on top of promises. 
 It cannot be used with plain callbacks or node callbacks.
 
 * Is, like promises, non blocking.
 
 * Makes asynchronous code look and behave a little more like synchronous code. 


#HSLIDE

#### Async/Await Syntax (1)

```
async function myFunction () {  
  try {
    var result = await asyncFunction()
  } catch (err) {
    ...
  }
}
```

@[1]

The function must be preceded by the reserved keyword "async".


#HSLIDE

#### Async/Await Syntax (2)

```
async function myFunction () {  
  try {
    var result = await asyncFunction()
  } catch (err) {
    ...
  }
}
```

@[2-6]

It should include a try-catch block.


#HSLIDE

#### Async/Await Syntax (3)

```
async function myFunction () {  
  try {
    var result = await asyncFunction()
  } catch (err) {
    ...
  }
}
```

@[3]

The try includes the async function preceded by reserved word "await".
With this, we make the function wait for it to execute 
and the result of the same is available in this case in the variable result.


#HSLIDE

#### Async/Await Syntax (4)

```
async function myFunction () {  
  try {
    var result = await asyncFunction()
  } catch (err) {
    ...
  }
}
```

@[4]

If an error occurs during the execution, 
the catch block will be executed where we will treat the error.


#HSLIDE

#### Async/Await sample refactor

```
function addToArray (data, array) {  
  const promise = new Promise(function (resolve, reject) {
    if (!array) {
      reject(new Error('Array does not exist'));
    }
  
    setTimeout(function() {
      array.push(data)
      resolve(array)
    }, 1000)
  })

  return promise
}

const array = [1, 2, 3]

async function processData (data, array) {  
  try {
    const result = await addToArray(data, array);
    console.log(result)
  } catch (err) {
    return console.log(err.message);
  }
}

processData(4, array)  
// [1,2,3,4]
processData(5, array)  
// [1,2,3,4,5]
processData(6, array)  
// [1,2,3,4,5,6]
```

@[1-11]
@[18-25]
@[18]
@[20]
@[27,29,31]


