# From Callbacks to Async/Await

#HSLIDE

#### Ways to control Asynchrony in JS

* Callbacks
* Promises (ES6)
* Async/Await


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

## Callback Hell o Pyramid of Doom

#HSLIDE

#### Promises (ES6)




#HSLIDE

#### Async/Await (ES7)

