# Generator Function

```javascript
function newUser(x, y) {
  var user = {
    name: x,
    age: y,
  };
  return user;
}
var iqbal = newUser('Jhum', '24');
```

## Constructor Function

```javascript
function newUser(x, y) {
  this.name = x;
  this.age = y;
}
var t = new newUser('Jhum', '24');