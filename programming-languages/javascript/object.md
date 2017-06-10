### Object

#### Prototype
- Every JavaScript object has a prototype. The prototype is also an object.
- All JavaScript objects inherit their properties and methods from their prototype.
- The standard way to create an object prototype is to use an object constructor function. With a constructor function, you can use the new keyword to create new objects from the same prototype.
`Object.getPrototypeOf` returns the prototype of an object.

#### Object literal
An expressive notation to define object.

```JavaScript
var obj = {
    // __proto__
    __proto__: theProtoObj,
    // Shorthand for ‘handler: handler’
    handler,
    // Methods
    toString() {
     // Super calls
     return "d " + super.toString();
    },
    // Computed (dynamic) property names
    [ 'prop_' + (() => 42)() ]: 42
};
```

#### Get, set and delete
- get
  object.name
  object[expression]

- set
  object.name = value
  object[expression] = value

- delete
  delete object.name
  delete object[expression]

#### Properties
A property is a named collection of attribute
- value -> any Javascript value
- writeble -> boolean
- enumerable -> boolean
- configurable -> boolean
- get -> function() { ... return value }
- set -> function(value) { ... }

#### New operator
```javascript
function new(func, arguments) {
  var that = Object.create(func.prototype),
      result = func.apply(that, arguments);
  return (typeof result === 'object' && result) || that;
}
```

#### Reference
Objects can be passed as arguments to functions, and can be returned by functions
- Objects are passed by references
- Objects are not passed by value
The === operator compares object references, not values (true if only it is the same object)
Loosely typed: any of these can be stored in a variable, or passed as a parameter to any function, the language is not "untyped"

#### Array
- Array inherits from Object
- Indexes are converted to strings and used as names for retrieving values
- Very efficient for sparse array
- Not very efficient in most of other cases
- One advantage: No need to provide a length or type when creating an array
- Literal form: []
- It can contain any number of expressions separated by commas
- New items can be appendend
- The dot notation should be not used by arrays

sort myArray:
-> it sorts alphabetically (everything number too, but we can pass a function in sort)

delete myArray[1];
-> it puts undefined in number 1 index;
-> we need to use splice to delete the element and resize the array