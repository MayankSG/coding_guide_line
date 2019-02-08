
## JavaScript best practice

**1.** Use **camelCase** for identifier names (variables and functions). All names start with a **letter**.

```javascript
// good
firstName = "John";
lastName = "Doe";

price = 19.90;
tax = 0.20;

fullPrice = price + (price * tax);
```

**2.** Use **UPPERCASE** for constants variables.

**3.** Use long, descriptive names

**A function name** should be a verb or a verb phrase

```javascript
// bad
function inv (user) { /* implementation */ }

// Good
function inviteUser (emailAddress) { /* implementation */ }
```

**4.** Always put spaces around operators ( = + - * / ), and after commas.

```javascript
// good
let x = y + z;
let values = ["Volvo", "Saab", "Fiat"];
```

**5.** Always use 2 spaces for indentation of code blocks:

```javascript
// good
function toCelsius(fahrenheit) {
  return (5 / 9) * (fahrenheit - 32);
}
```

**6.** Always end a simple statement with a semicolon.

```javascript
let values = ["Volvo", "Saab", "Fiat"];

let person = {
firstName: "John",
lastName: "Doe",
age: 50,
eyeColor: "blue"
};
```

**7.** General rules for complex(functions, loops, conditions) statements:

-   Put the opening bracket at the end of the first line.
-   Use one space before the opening bracket.
-   Put the closing bracket on a new line, without leading spaces.
-   Do not end a complex statement with a semicolon.

```javascript
// good
function toCelsius(fahrenheit) {
  return (5 / 9) * (fahrenheit - 32);
}

// good
for (let i = 0; i < 5; i++) {
  x += i;
}

// good
if (time < 20) {
  greeting = "Good day";
} else {
  greeting = "Good evening";
}
```

**8.** For readability, avoid lines longer than 80 characters.

**9.** Avoid Global Variables

**10.** Always Declare Local Variables
Local variables **must** be declared with the `let` keyword.

```javascript
// bad
a = 10;

// good
let a = 10;
```

**11.** Declarations on Top
It is a good coding practice to put all declarations at the top of each script or function.

```javascript
// good
// Declare at the beginning
let firstName, lastName, price, discount, fullPrice;

// Use later
firstName = "John";
lastName = "Doe";

price = 19.90;
discount = 0.10;

fullPrice = price * 100 / discount;
```

**12.** Initialize variables when you declare them.

```javascript

// good
let firstName = "",
    lastName = "",
    price = 0,
    discount = 0,
    fullPrice = 0,
    myArray = [],
    myObject = {};
```

**13.**  Avoid long argument list

```javascript

// bad
function getRegisteredUsers (fields, include, fromDate, toDate) { /* implementation */ }
getRegisteredUsers(['firstName', 'lastName', 'email'], ['invitedUsers'], '2016-09-26', '2016-12-13')

// good
function getRegisteredUsers ({ fields, include, fromDate, toDate }) { /* implementation */ }
getRegisteredUsers({
  fields: ['firstName', 'lastName', 'email'],
  include: ['invitedUsers'],
  fromDate: '2016-09-26',
  toDate: '2016-12-13'
})
```

**14.** Never Declare Number, String, or Boolean Objects, Always treat numbers, strings, or booleans as primitive values. Not as objects.

```javascript

// bad
let y = new String("John");

// good
let x = "John";
```

**15.** Don't Use new Object()

```javascript
-   Use  `{}`  instead of  `new Object()`
-   Use  `""`  instead of  `new String()`
-   Use  `0`  instead of  `new Number()`
-   Use  `false`  instead of  `new Boolean()`
-   Use  `[]`  instead of  `new Array()`
-   Use  `/()/`  instead of  `new RegExp()`
-   Use  `function (){}`  instead of  `new Function()`

// Example
let x1 = {}; // new object
let x2 = ""; // new primitive string
let x3 = 0; // new primitive number
let x4 = false; // new primitive boolean
let x5 = []; // new array object
let x6 = /()/; // new regexp object
let x7 = function(){}; // new function object
```

**15.** Use `let` instead `var` for block variables.
Always use let as much as possible to avoid the scope issues.


```javascript
// bad
for(var i = 0; i<10; i++) {
  console.log(i)
}
console.log(i)

// good
for(let i = 0; i<10; i++) {
  console.log(i)
}
console.log(i)

```

**16.** `let` have limitation in some cases. So, need to practice where you need `var` for variable declaration.

Like: `var` are stored in the `window` object, while `let` are stored in a declarative environment that you can't see

```javascript
let a = 5;

window.a // undefined

---------------------------

var a = 5;

window.a // 5
```

**17.** Use `const`  for constant, one time assignment.

```javascript
// We're declaring PI to be a constant variable.
const PI = 3.141592653589793;
```

**18.** When you are comparing to variable use `===` comparison. The `===` operator forces comparison of values and type.

```javascript
0 == ""; // true
1 == "1"; // true
1 == true; // true

0 === ""; // false
1 === "1"; // false
1 === true; // false
```

**19.** Use Defaults Parameter
If a function is called with a missing argument, the value of the missing argument is set to  `undefined`. Undefined values can break your code. It is a good habit to assign default values to arguments.

```javascript
// ok
function myFunction(x, y) { // function code }

// batter
function (a=1, b=1) { // function code }
```

**20.** Use a switch/case statement instead of a series of if/else.

**21.** Always end your `switch` statements with a `default`. Even if you think there is no need for it.

```javascript
// bad
switch (new Date().getDay()) {
case  0:
  day = "Sunday";
  break;
case  1:
  day = "Monday";
  break;
case  2:
  day = "Tuesday";
  break;
case  3:
  day = "Wednesday";
  break;
case  4:
  day = "Thursday";
  break;
case  5:
  day = "Friday";
  break;
case  6:
  day = "Saturday";
  break;
}

// good
switch (new Date().getDay()) {
case  0:
  day = "Sunday";
  break;
case  1:
  day = "Monday";
  break;
case  2:
  day = "Tuesday";
  break;
case  3:
  day = "Wednesday";
  break;
case  4:
  day = "Thursday";
  break;
case  5:
  day = "Friday";
  break;
case  6:
  day = "Saturday";
  break;
default:
  day = "Unknown";
}
```

**22.** Avoid Using eval(). The `eval()` function is used to run text as code. In almost all cases, it should not be necessary to use it.

**23.** Don't use Short-Hand

```javascript
// bad
if (someVariableExists)
  x = false
  anotherFunctionCall();

// good
if (someVariableExists) {
  x = false
}
anotherFunctionCall();
```

**24.** Place Scripts at the Bottom of Your Page.
The primary goal is to make the page load as quickly as possible for the user.

```html
// good
    <script type="text/javascript" src="path/to/file.js"></script>
    <script type="text/javascript" src="path/to/anotherFile.js"></script>
  </body>
</html>
```

**25.** Declare Variables Outside of the For Statement

```javascript
// bad
for(var i = 0; i < someArray.length; i++) {
   var container = document.getElementById('container');
   container.innerHtml += 'my number: ' + i;
   console.log(i);
}

// good
let container = document.getElementById('container');
for(let i = 0, len = someArray.length; i < len;  i++) {
   container.innerHtml += 'my number: ' + i;
   console.log(i);
}
```

**26.** Comment your code.

```javascript
// good
// Cycle through array and echo out each name.
for(let i = 0, len = array.length; i < len; i++) {
   console.log(array[i]);
}
```

**27.** Use the map() function method to loop through an array’s items.

```javascript
// good
let squares = [1,2,3,4].map(function (val) {
    return val * val;
});
// squares will be equal to [1, 4, 9, 16]
```

**28.** Avoid using for-in loop for arrays.

```javascript
// bad
let sum = 0;
for (let i in arrayNumbers) {
    sum += arrayNumbers[i];
}

// good
let sum = 0;
for (let i = 0, len = arrayNumbers.length; i < len; i++) {
    sum += arrayNumbers[i];
}
```

**29.** Avoid using try-catch-finally inside a loop.

```javascript
// bad
let object = ['foo', 'bar'], i;
for (i = 0, len = object.length; i < len; i++) {
    try {
        // do something that throws an exception
    }
    catch (e) {
        // handle exception
    }
}

// good
let object = ['foo', 'bar'], i;
try {
    for (i = 0, len = object.length; i &lt;len; i++) {
        // do something that throws an exception
    }
}
catch (e) {
    // handle exception
}
```

**30.** Don't pass a string to "SetInterval" or "SetTimeOut"

```javascript
// bad
setInterval(
  "document.getElementById('container').innerHTML += 'My new number: ' + i", 3000
);
```

**31.** Don't Use the "With" Statement

```javascript
// bad
with (being.person.man.bodyparts) {
   arms = true;
   legs = true;
}

// good
let o = being.person.man.bodyparts;
o.arms = true;
o.legs = true;
```

**32.** Use [] instead of new Array()

```javascript
// ok
let a = new Array();
a[0] = "Joe";
a[1] = 'Plumber';

// good
let a = ['Joe','Plumber'];
```

**33.** Don’t use delete to remove an item from array

```javascript
// bad
let items = [12, 548 ,'a' , 2 , 5478 , 'foo' , 8852, , 'Doe' ,2154 , 119 ];
items.length; // return 11
delete items[3]; // return true
items.length; // return 11
/* items will be equal to [12, 548, "a", undefined × 1, 5478, "foo", 8852, undefined × 1, "Doe", 2154,       119]   */

// good
let items = [12, 548 ,'a' , 2 , 5478 , 'foo' , 8852, , 'Doe' ,2154 , 119 ];
items.length; // return 11
items.splice(3,1) ;
items.length; // return 10
/* items will be equal to [12, 548, "a", 5478, "foo", 8852, undefined × 1, "Doe", 2154,       119]   */
```

**34.** Always use semicolons

```javascript
// ok
let someItem = 'some string'
function doSomething() {
  return 'something'
}

// good
let someItem = 'some string';
function doSomething() {
  return 'something';
}
```

**35.** Remove "language"

```javascript
// bad
<script type="text/javascript" language="javascript">
...
</script>

// good
<script type="text/javascript">
...
</script>
```

**36.** Always keep your javascript code i separate file, and call it where is needed.

```javascript
// good
<script type="text/javascript" src="path/to/file.js"></script>
```
