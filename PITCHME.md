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

#### Callbacks example (1)

```
function doSomething(callbackStep1, callbackStep2, callbackFinish){
    // Something here
    callbackStep1('step 1');

    // continue... something here
    callbackStep2('step 2');

    // continue... something here
    callbackFinish('finish');
}

doSomething(
    function(step){
       console.log(step);
    },
    function(step){
       console.log(step);
    },
    function(end){
       console.log(end);
    });
```

#HSLIDE

#### Callbacks example (2)

```
function doSomething(callbackStep1, callbackStep2, callbackFinish){
    // Something here
    callbackStep1('step 1');

    // continue... something here
    callbackStep2('step 2');

    // continue... something here
    callbackFinish('finish');
}

function step1(step){
     console.log(step);
}

function step2(step){
     console.log(step);
}

function finish(end){
     console.log(end);
}

doSomething(step1, step2, finish);
```


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

#### Parallel operations with promises

### Promise.all

* ES6 provides the Promise.all method which takes in an array of promises and returns a new promise. 
* The new promise is fulfilled after all the operations have completed successfully. 
* If any of the operations fail, the new promise is rejected.


#HSLIDE

#### Parallel operations with promises

```
var allPromise = Promise.all([ 
  fs_readFile('file1.txt'), 
  fs_readFile('file2.txt') 
])

allPromise.then(console.log, console.error)
```


#HSLIDE

#### Common promises error

```
const array = ''  
addToArray(4, array)  
  .then(...)
  .then(...)
  .then(...)
  .catch(err => {
    return err
  })
```


```
const array = ''  
addToArray(4, array)  
  .then(...)
  .then(...)
  .then(...)
  .catch(err => {
    return Promise.reject(err)
  })
```


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

The try includes the async function preceded by the reserved keyword "await".
With this, we make the function wait for it to execute 
and the result of the same is available in this case in the variable result.


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

The "await" keyword can only be used inside functions defined with "async"

Any async function returns a promise implicitly, and the resolve value of the promise 
will be whatever you return from the function.


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

@[1-13]
@[18-25]
@[18]
@[20]
@[27,29,31]


#HSLIDE

#### Why is Async/Await better? (1)

Concise and clean
  - It’s clear we saved a decent amount of code.
  - We didn’t have to write .then, create an anonymous function to handle the response, or give a name data to a variable that we don’t need to use. 
  - We also avoided nesting our code. 
  

#HSLIDE

```
const makeRequest = () =>
  getJSON()
    .then(data => {
      console.log(data)
      return "done"
    })

makeRequest()
```

```
const makeRequest = async () => {
  console.log(await getJSON())
  return "done"
}

makeRequest()
```

* await getJSON() means that the console.log call will wait until getJSON() promise resolves and print it value.
  

#HSLIDE

#### Why is Async/Await better? (2)
  
Error handling
  - Async/await makes it finally possible to handle both synchronous and asynchronous errors with the same construct, good old try/catch.


#HSLIDE

```
const makeRequest = () => {
  try {
    getJSON()
      .then(result => {
        // this parse may fail
        const data = JSON.parse(result)
        console.log(data)
      })
      // uncomment this block to handle asynchronous errors
      // .catch((err) => {
      //   console.log(err)
      // })
  } catch (err) {
    console.log(err)
  }
}
```


#HSLIDE

```
const makeRequest = async () => {
  try {
    // this parse may fail
    const data = JSON.parse(await getJSON())
    console.log(data)
  } catch (err) {
    console.log(err)
  }
}
```

The catch block now will handle parsing errors.


#HSLIDE

#### Why is Async/Await better? (3)

Conditionals
  - Return that or get more details based on some value in the data.


#HSLIDE

```
const makeRequest = () => {
  return getJSON()
    .then(data => {
      if (!data.needsAnotherRequest) {
        console.log(data)
        return data
      }
      return makeAnotherRequest(data)
          .then(moreData => {
            console.log(moreData)
            return moreData
          })
    })
}
```


#HSLIDE

```
const makeRequest = async () => {
  const data = await getJSON()
  if (!data.needsAnotherRequest) {
    console.log(data)
    return data
  } 
  const moreData = await makeAnotherRequest(data);
  console.log(moreData)
  return moreData
}
```

#HSLIDE

#### Why is Async/Await better? (4)

Intermediate values
  - Nested promises.

#HSLIDE

```
const makeRequest = () => {
  return promise1()
    .then(value1 => {
      // do something
      return promise2(value1)
        .then(value2 => {
          // do something          
          return promise3(value1, value2)
        })
    })
}
```


#HSLIDE

```
const makeRequest = () => {
  return promise1()
    .then(value1 => {
      // do something
      return Promise.all([value1, promise2(value1)])
    })
    .then(([value1, value2]) => {
      // do something          
      return promise3(value1, value2)
    })
}
```


#HSLIDE

```
const makeRequest = async () => {
  const value1 = await promise1()
  const value2 = await promise2(value1)
  return promise3(value1, value2)
}
```

#HSLIDE

#### Why is Async/Await better? (5)

Error stacks
  - Chained promises with an error
  
  
#HSLIDE

```
const makeRequest = () => {
  return callAPromise()
    .then(() => callAPromise())
    .then(() => callAPromise())
    .then(() => callAPromise())
    .then(() => callAPromise())
    .then(() => {
      throw new Error("oops");
    })
}

makeRequest()
  .catch(err => {
    console.log(err);
    // output
    // Error: oops at callAPromise.then.then.then.then.then (index.js:8:13)
  })
```


#HSLIDE

```
const makeRequest = async () => {
  await callAPromise()
  await callAPromise()
  await callAPromise()
  await callAPromise()
  await callAPromise()
  throw new Error("oops");
}

makeRequest()
  .catch(err => {
    console.log(err);
    // output
    // Error: oops at makeRequest (index.js:7:9)
  })
```


#HSLIDE

#### Why is Async/Await better? (6)

Debugging

```
const makeRequest = () => {
  return callAPromise()
    .then(() => callAPromise())
    .then(() => callAPromise())
    .then(() => callAPromise())
    .then(() => callAPromise())
    .then(() => {
      throw new Error("oops");
    })
}
```


#HSLIDE

```
const makeRequest = async () => {
  await callAPromise()
  await callAPromise()
  await callAPromise()
  await callAPromise()
  await callAPromise()
  throw new Error("oops");
}
```

#HSLIDE

#### Loops 

[Loops](https://pouchdb.com/2015/03/05/taming-the-async-beast-with-es7.html)


#HSLIDE

#### Combine promises and async/await example

```
async function asyncFun () {
  var value = await Promise
    .resolve(1)
    .then(x => x * 3)
    .then(x => x + 5)
    .then(x => x / 2);
  return value;
}
asyncFun().then(x => console.log(`x: ${x}`));
// <- 'x: 4'
```


#HSLIDE

#### In Conclusion

- Async/await is one of the most revolutionary features that have been added to JavaScript in the past few years. 

- It makes you realize what a syntactical mess promises are, and provides an intuitive replacement.


#HSLIDE

#### Concerns

- It makes asynchronous code less obvious

- Relatively new



#HSLIDE

#### Recreate Promise.all with async/await

[Promise all](https://gist.github.com/indiesquidge/5960274889e17102b5130e8bd2ce9002)



#HSLIDE

#### Even with async/await, raw promises are still key to writing optimal concurrent javascript

[Optimal concurrent javascript](https://medium.com/@bluepnume/even-with-async-await-you-probably-still-need-promises-9b259854c161)


#HSLIDE

#### Starting with async/await (Spanish)

[Getting started with async function](https://developers.google.com/web/fundamentals/getting-started/primers/async-functions)



#HSLIDE

#### Tips for using async functions

[Tips](http://2ality.com/2016/10/async-function-tips.html)

