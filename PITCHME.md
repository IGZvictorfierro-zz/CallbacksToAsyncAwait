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
  } else {
    array.push(data)
    callback(null, array)
  }
}

var array = [1,2,3];

addToArray(4, array, function (err) {  
  if (err) return console.log(err.message)
  console.log(array)
})

// [1, 2, 3, 4]
```

@[0-8]
@[10-15]
@[17]


#HSLIDE

#### Promises (ES6)




#HSLIDE

#### Async/Await (ES7)

