# React Project Structure

## Project Files

1. In `index.html` file there is a div with the id `root`.

2. In `package.json`, you can check which React libraries are installed.

3. React and React DOM are two separate libraries.

4. These libraries are downloaded and installed in the `node_modules` directory.

5. Import statements:

   ```javascript
   import React from 'react';
   ```

   Here we are importing React from the react library.

   ```javascript
   import ReactDOM from 'react-dom/client';
   ```

   Similarly, we are importing ReactDOM from react-dom/client.

All these packages are downloaded via `package.json` and installed in `node_modules`. Later we can import them from `node_modules` in our `index.js`.

## Classes in JavaScript

1. **Class**: ReactDOM is a class.

**Class syntax:**

```javascript
class User {

}
```

**Object example:**

```javascript
const user = {
    firstName: "Habibur",
    lastName: "Rahman",
    print: () => {
        console.log(this.firstName, " ", this.lastName);
    }
}
```

> **Note:** The curly braces `{}` represent an object when placed inside a `const` variable or any other variable. The object works as an object in that case.
>
> **Notes:** Objects contain various properties and different properties have different values.
>
> **Additional Note:** If I print the user, I can print all the values of the user object.
