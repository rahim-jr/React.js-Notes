# React Objectives

1. Single page web application.
2. Reduces bandwidth consumption.
3. Requests only the required data from the server.

4. If you delete node modules and run npm install, dependencies will be installed again.

## Understanding JSX Element Creation and Map Function

We use the map function on arrays.

`array.map(callback)` - The map function is essentially a method property of the Array class. When we use the dot operator after an array, that array becomes an object of the Array class!

> **Note:**

```javascript
map() {

}
```

Since it is now an object of the Array class, it can only access the map() function using the dot operator for itself, because internally it is an object.

This means an array is an object (proven)!

## Map Function Details

When the map function is called, it takes a callback function inside it, and the callback function structure looks something like this:

```javascript
function = (number, index) => {
    return console.log("something");
}
```

- This number is the values inside the array that was used to call the map function, and this index is that same array's indexing, which starts from 0.

`array.map(function)` - If you pass it, everything gets properly integrated.

## Map Function Infrastructure

```javascript
map(callBack) { // here callBack is the singleBox function
    arra = []
    for (i = 0; i <= this.length; i++) {
        // Here 'this' refers to the entity that called the map function.
        // The map function is called by an array, and the number of elements in that array is the length.

        arra.push(ans);
        // The value of the 'ans' variable which is the element that singleBox returned.
    }
    return arra;
}
```
