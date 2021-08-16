---
layout: post
title: "Javascript Shorthand"
date: 2020-11-26 10:20:00 +0800
categories: javascript shorthand
---

There are various javascript shorthand techniques available for a more productive development process and clean coding style. Here are some of the few shorthand techniques:

**Declaring variables**

```[javascript]
 // Longhand
let x;
let y = 20;

 // Shorthand
let x, y = 20;
```

**Assigning values to multiple variables**  
We can assign values to multiple variables in one line with array destructuring.

```[javascript]
 //Longhand
let a, b, c;
a = 5;
b = 8;
c = 12;

 //Shorthand
let [a, b, c] = [5, 8, 12];
```

**The Ternary operator**  
We can save 5 lines of code here with ternary (conditional) operator.

```[javascript]
 //Longhand
let marks = 26;
let result;
if(marks >= 30){
 result = 'Pass';
}else{
 result = 'Fail';
}

 //Shorthand
let result = marks >= 30 ? 'Pass' : 'Fail';
```

**Assigning default value**  
We can use OR(||) short circuit evaluation to assign a default value to a variable in case the expected value found falsy.

```
//Longhand
let imagePath;
let path = getImagePath();
if(path !== null && path !== undefined && path !== '') {
  imagePath = path;
} else {
  imagePath = 'default.jpg';
}

//Shorthand
let imagePath = getImagePath() || 'default.jpg';
```

**AND(&&) Short circuit evaluation**  
If you are calling a function only if a variable is true, then you can use AND(&&) short circuit as an alternative for this.

```
// Longhand
if (isLoggedin) {
 goToHomepage();
}

 // Shorthand
isLoggedin && goToHomepage();
```

The AND(&&) short circuit shorthand is more useful in React when you want to conditionally render a component.  
For example:  
`<div> { this.state.isLoading && <Loading /> } </div>`

**Swap two variables**  
To swap two variables, we often use a third variable. We can swap two variables easily with array destructuring assignment.

```
let x = 'Hello', y = 55;
//Longhand
const temp = x;
x = y;
y = temp;

//Shorthand
[x, y] = [y, x];
7. Arrow Function
//Longhand
function add(num1, num2) {
   return num1 + num2;
}

 // Shorthand
const add = (num1, num2) => num1 + num2;
```

Reference: JavaScript Arrow function

**Template Literals**  
We normally use + operator to concatenate string values with variables. With ES6 template literals we can do it in a more simple way.

```
 // Longhand
console.log('You got a missed call from ' + number + ' at ' + time);

 // Shorthand
console.log(`You got a missed call from ${number} at ${time}`);
```

**Multi-line String**  
For multiline string we normally use + operator with a new line escape sequence (\n).  
We can do it in an easier way by using backticks (`).

```
 // Longhand
console.log('JavaScript, often abbreviated as JS, is a\n' +             'programming language that conforms to the \n' +
'ECMAScript specification. JavaScript is high-level,\n' +
'often just-in-time compiled, and multi-paradigm.' );
 // Shorthand
console.log(`JavaScript, often abbreviated as JS, is a programming language that conforms to the ECMAScript specification. JavaScript is high-level, often just-in-time compiled, and multi-paradigm.`);
```

**Multiple condition checking**  
For multiple value matching, we can put all values in array and use indexOf() or includes() method.

```
 // Longhand
if (value === 1 || value === 'one' || value === 2 || value === 'two') {
     // Execute some code
}

 // Shorthand 1
if ([1, 'one', 2, 'two'].indexOf(value) >= 0) {
    // Execute some code
}

 // Shorthand 2
if ([1, 'one', 2, 'two'].includes(value)) {
    // Execute some code
}
```

**Object Property Assignment**  
If the variable name and object key name is same then we can just mention variable name in object literals instead of both key and value. JavaScript will automatically set the key same as variable name and assign the value as variable value.

```
let firstname = 'AA';
let lastname = 'Lyold';
 // Longhand
let obj = {firstname: firstname, lastname: lastname};

 // Shorthand
let obj = {firstname, lastname};
```

**String into a Number**  
There are built in methods like parseInt and parseFloat available to convert a string to number. We can also do this by simply providing a unary operator (+) in front of string value.

```
 // Longhand
let total = parseInt('453');
let average = parseFloat('42.6');

 // Shorthand
let total = +'453';
let average = +'42.6';
```

**Repeat a string for multiple times**  
To repeat a string for a specified number of time you can use a for loop. But using the repeat() method we can do it in a single line.

```
 // Longhand
let str = '';
for(let i = 0; i < 5; i ++) {
  str += 'Hello ';
}
console.log(str); // Hello Hello Hello Hello Hello

 // Shorthand
'Hello '.repeat(5);
```

**Exponent Power**  
We can use Math.pow() method to find the power of a number. There is a shorter syntax to do it with double asterik (\*\*).

```
 // Longhand
const power = Math.pow(4, 3); // 64

 // Shorthand
const power = 4**3; // 64
```

**Double NOT bitwise operator (~~)**  
The double NOT bitwise operator is a substitute for Math.floor() method.

```
 // Longhand
const floor = Math.floor(6.8); // 6

 // Shorthand
const floor = ~~6.8; // 6
```

**Find max and min number in array**  
We can use for loop to loop through each value of array and find the max or min value. We can also use the Array.reduce() method to find the max and min number in array.  
But using spread operator we can do it in a single line.
// Shorthand
const arr = [2, 8, 15, 4];
Math.max(...arr); // 15
Math.min(...arr); // 2

**For loop**  
To loop through an array we normally use the traditional for loop. We can make use of the for...of loop to iterate through arrays. To access the index of each value we can use for...in loop.

```
let arr = [10, 20, 30, 40];
//Longhand
for (let i = 0; i < arr.length; i++) {
  console.log(arr[i]);
}
// Shorthand
//for of loop
for (const val of arr) {
  console.log(val);
}
//for in loop
for (const index in arr) {
  console.log(arr[index]);
}
```

We can also loop through object properties using for...in loop.

```
let obj = {x: 20, y: 50};
for (const key in obj) {
  console.log(obj[key]);
}
```

Reference: Different ways to iterate through objects and arrays in JavaScript

**Merging of arrays**

```
let arr1 = [20, 30];
 // Longhand
let arr2 = arr1.concat([60, 80]);
// [20, 30, 60, 80]

 // Shorthand
let arr2 = [...arr1, 60, 80];
// [20, 30, 60, 80]
```

**Deep cloning of multi-level object**  
To deep clone a multi-level object, we can iterate through each property and check if the current property contains an object. If yes, then do a recursive call to the same function by passing the current property value (i.e. the nested object).  
We can also do it by using JSON.stringify() and JSON.parse() if our object doesn't contains functions, undefined, NaN or Date as values.  
If we have single level object i.e no nested object present, then we can deep clone using spread operator also.

```
let obj = {x: 20, y: {z: 30}};

// Longhand
const makeDeepClone = (obj) => {
  let newObject = {};
  Object.keys(obj).map(key => {
    if(typeof obj[key] === 'object'){
      newObject[key] = makeDeepClone(obj[key]);
    } else {
      newObject[key] = obj[key];
    }
  });
 return newObject;
}
const cloneObj = makeDeepClone(obj);

 // Shorthand
const cloneObj = JSON.parse(JSON.stringify(obj));

// Shorthand for single level object
let obj = {x: 20, y: 'hello'};
const cloneObj = {...obj};
```

**Get character from string**

```
let str = 'google.com';
//Longhand
str.charAt(2); // c

 //Shorthand
str[2]; // c
```
