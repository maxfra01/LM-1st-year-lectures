# Javascript features

Remember to use `"use strict" ;` at the beginning of a js file.
### Values and types

Since JS is object oriented, all values are objects, but variables are not.
A variable just refer to an object (just like a pointer in C).
All objects have a type: 

![[Pasted image 20240305115703.png]]

Two particular types are **null** and **undefined**: undefined is a variable declared but not initialized.
**Functions** are objects too.

Boolean type has only two values which are true and false: in JS only 6 values are "falsy":
- 0
- -0
- `NaN`
- `undefined`
- `null`
- `''` (the empty string)
Every other value is consider "truthy", even empty arrays or objects.

To compute a comparation between values we can use the classic `==` operator which **convert** types and then compare the result.
To compute a comparation without convert values we can use the `===` operator.


```js
console.log("3"==3) //it converts the values so it's true

console.log("3"===3) //not the same type so it's false

console.log(3+"2") //since the second value is a string the '+' symbol is a concatenation so the output will be 32

console.log(3-"1") //output is 2
```

Note that real and integers numbers are treated the same way: casting operations are performed based on the operands.

### Declaring variables

Declaring a variable is mandatory in strict mode: there are 3 ways to declare a variable.

![[Pasted image 20240305121613.png]]

### Expressions

If we compare two objects, the comparison will return true **only if they are the same object**, in every other scenario (even with same property) it will return false.

Mathematical operations are available inside **Math** library then we can compute them:

```js
const r = Math.abs(-345);
```

Logical operators are the classic ones: `&&`, `||`, `!`. We can use this operators to avoid if statements:

```js
flag && (expression) //the expression is computed only if flag is true (same as if)

value || (default_value) // if value is undefined, NaN... we assign a default value
```

All common operations with numbers and strings are supported, also conditional operator: `c ? t : f`.
### Control structures

If, For, While, Do while and switch are identical to C language.
There are two **special For loops** by using `in` and `of` keyword: to iterate over an array we can use this special loop:

![[Pasted image 20240305124102.png]]

Note the behavior of `in` for loop which iterate over the properties of an object.
We can also iterate over an array with the methods **`.forEach()`** and **`.map()`**

Exceptions handling is implemented by using a **try, catch, finally** construct.

### Arrays

Collections of **different** **types** of data, defined by square brackets.
The main property is `.length`
There are methods that modify the array, others returns new arrays.
There's no need to specify the length of an array when declaring, the length adjusts automatically.

Some of the main methods for arrays:
- `push()` adds an element at the end
- `pop()` remove an element from the end
- `unshift()` adds an element at the beginning
- `shift()` remove an element at the beginning

Since array's name is only a reference to the actual array, to create a real copy of an array we must use the `Array.from()` function.

![[Pasted image 20240305125300.png]]

We can **destructure** arrays using square brackets:

```js
let a=5;
let b=11;
[a,b] = [b,a] //swap operation
```

The **spread operator** `...` is available in JS: it basically expands an iterable object in its part:

```js
let parts = [3,4,5,6,7];
const numbers = [1,2, ...parts, 8, 9, 10];

const copy_of_parts = [...parts]; //we can also copy an array
```

### Strings

In JS **Strings** are **immutable** sequence of Unicode characters: the length of a string equals the number of char it contains. A char is a 1-length string.

All strings operations **return new strings**:

![[Pasted image 20240305125519.png]]

To print formatted string we can use the dollar sign:

```js
const name = "Jeff";
console.log("Hello ${name}"); //output-> "Hello Jeff"
```

# Javascript objects

Javascript is object oriented, but differently from Java, an object can exist without a class.
At any time we can add, remove or modify **properties** and **methods** of a certain object.
Note that all methods and properties are **public**.

To create an object we use curly brackets, just as a python dictionary.

```js
let empty_object = {};

let book = {
	author: "Enrico",
	title: "Learning JS",
	pages: 520,
	"my review": 3
}
```

The variable `book` above is just a reference to the real object, which has no class.
To access properties we use **dot notation** or **square brackets** (when property name is an invalid identifier).

```js
book.author //output: Enrico
book["author"] //same as before
book["my review"] // output: 3
book.my title //ERROR, CANNOT ACCESS BY DOT NOTATION
```

If a property is not defined, accessing it will return `undefined`.

### Iterate on properties

Properties can be associated with an array: to iterate on properties we have two ways:
- Use the `in` keyword in a for loop.
- Use the methods `keys` and `entries`

**Keys** will return an array with the names of properties, while **entries** will return every pair key,  value of that object.

```js
for (let a in book){
	console.log(a); 
}
//"author", "title", "pages", "my review"

let prop = book.keys();
//["author", "title", "pages", "my review"]

let pair = book.entries();
//[[ 'author', 'Enrico' ], [ 'pages', 520 ], ["title", "learning JS"]... ]
```

We can **check the presence of a property** using the `in` operator, it will return a boolean. 
### Modify and copying objects

The **assign** method is very useful to manipulate object. It returns the manipulated object.

```js
Object.assign(target, source, default values, ..)
```

To copy (not creating an alias, but a real copy) we can set the target to a new object, and the source to the initial object.
Note that the new object is represented with `{}`.

```js
a_new_book = Object.assign({}, book);
```

We can also merge properties of different objects:

```js
let book2 = Object.assign({}, book, {"new_property": 4.5}); 
```

The **spread operator** is useful also for copying objects:

```js
let book2 = {...book};
```

# Javascript functions

In JS **functions are objects**: they can be assigned, passed as a argument and also returned.
Inside a function exists a privare scope.

### Define a function 1

There are 3 ways: the first one is the same of may other languages.

```js
function do(params){
	//do stuff
}
```

Parameters are passed **by value**, and local variables are reference to the original values. 
If a parameter is not passed it will assume the value **undefined**: we can check undefined values with `p = p || default_value_p`.
The **rest operator** is useful to compact all remaining arguments into an array:

```js
function do(first, second, ...arr){
	for (let el of arr)
	//do stuff
}

let res = do(45,2,5,3,6);
```

### Define a function 2

```js
const fn = function(){
	//do stuff
}
```

Note that is also possible to give a name to the function