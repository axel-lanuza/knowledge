# Object

## Prototype
- Every JavaScript object has a prototype.
- The prototype is also an object
- All JavaScript objects inherit their properties and methods from their prototype
- The standard way to create an object prototype is to use an object constructor function
- With a constructor function, you can use the new keyword to create new objects from the same prototype
- `Object.getPrototypeOf` returns the prototype of an object
- `Object.create(prototype)` creates an object with prototype `prototype`, if it is null it doesn't have a prototype
- `hasOwnProperty` check if an object has a property and not his ancestors
- The `Object.assign(target, ...sources)` method is used to copy the values of all enumerable own properties from one or more source objects to a target object. It will return the target object.
- `Object.defineProperty(obj, prop, descriptor)` defines a new property directly on an object, or modifies an existing property on an object, and returns the object.

### Class Inheritance
instances inherit from classes (like a blueprint — a description of the class), and create sub-class relationships: hierarchical class taxonomies. Instances are typically instantiated via constructor functions with the `new` keyword. Class inheritance may or may not use the `class` keyword from ES6.

### Prototypal Inheritance
instances inherit directly from other objects. Instances are typically instantiated via factory functions or `Object.create()`. Instances may be composed from many different objects, allowing for easy selective inheritance.

### When is prototypal inheritance an appropriate choice?
There is more than one type of prototypal inheritance:
- Delegation (i.e., the prototype chain).
- Concatenative (i.e. mixins, `Object.assign()`).
- Functional (Not to be confused with functional programming. A function used to create a closure for private state/encapsulation).
Each type of prototypal inheritance has its own set of use-cases, but all of them are equally useful in their ability to enable composition, which creates has-a or uses-a or can-do relationships as opposed to the is-a relationship created with class inheritance.

## Object literal
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

## Get, set and delete
- get
  object.name
  object[expression]

- set
  object.name = value
  object[expression] = value

- delete
  delete object.name
  delete object[expression]

## Properties
A property is a named collection of attribute
- value -> any Javascript value
- writeble -> boolean
- enumerable -> boolean
- configurable -> boolean
- get -> function() { ... return value }
- set -> function(value) { ... }

## New operator
```javascript
function new(func, arguments) {
  var that = Object.create(func.prototype),
      result = func.apply(that, arguments);
  return (typeof result === 'object' && result) || that;
}
```

## Reference
Objects can be passed as arguments to functions, and can be returned by functions
- Objects are passed by references
- Objects are not passed by value
The === operator compares object references, not values (true if only it is the same object)
Loosely typed: any of these can be stored in a variable, or passed as a parameter to any function, the language is not "untyped"

## Property Lookup
When accessing the properties of an object, JavaScript will traverse the prototype chain upwards until it finds a property with the requested name.
If it reaches the top of the chain - namely Object.prototype - and still hasn't found the specified property, it will return the value undefined instead.

## Performance
The lookup time for properties that are high up on the prototype chain can have a negative impact on performance, and this may be significant in code where performance is critical. Additionally, trying to access non-existent properties will always traverse the full prototype chain.
Also, when iterating over the properties of an object every property that is on the prototype chain will be enumerated.

## Extension of Native Prototypes
One mis-feature that is often used is to extend Object.prototype or one of the other built in prototypes.

This technique is called monkey patching and breaks encapsulation. While used by popular frameworks such as Prototype, there is still no good reason for cluttering built-in types with additional non-standard functionality.

The only good reason for extending a built-in prototype is to backport the features of newer JavaScript engines; for example, Array.forEach.

## defineProperty
This method allows precise addition to or modification of a property on an object. Normal property addition through assignment creates properties which show up during property enumeration (for...in loop or Object.keys method), whose values may be changed, and which may be deleted. This method allows these extra details to be changed from their defaults. By default, values added using Object.defineProperty() are immutable.

Property descriptors present in objects come in two main flavors: data descriptors and accessor descriptors. A data descriptor is a property that has a value, which may or may not be writable. An accessor descriptor is a property described by a getter-setter pair of functions. A descriptor must be one of these two flavors; it cannot be both.

Both data and accessor descriptors are objects. They share the following required keys:
- configurable: true if and only if the type of this property descriptor may be changed and if the property may be deleted from the corresponding object. Defaults to false.
- enumerable: true if and only if this property shows up during enumeration of the properties on the corresponding object. Defaults to false.

A data descriptor also has the following optional keys:
- value: The value associated with the property. Can be any valid JavaScript value (number, object, function, etc). Defaults to undefined.
- writable: true if and only if the value associated with the property may be changed with an assignment operator. Defaults to false.

An accessor descriptor also has the following optional keys:
- get: A function which serves as a getter for the property, or undefined if there is no getter. The function return will be used as the value of property. Defaults to undefined.
- set: A function which serves as a setter for the property, or undefined if there is no setter. The function will receive as only argument the new value being assigned to the property. Defaults to undefined.

## Object.assign
The `Object.assign(target, ...sources)` method is used to copy the values of all enumerable own properties from one or more source objects to a target object. It will return the target object.

The Object.assign() method only copies enumerable and own properties from a source object to a target object. It uses [[Get]] on the source and [[Set]] on the target, so it will invoke getters and setters. Therefore it assigns properties versus just copying or defining new properties. This may make it unsuitable for merging new properties into a prototype if the merge sources contain getters. For copying property definitions, including their enumerability, into prototypes Object.getOwnPropertyDescriptor() and Object.defineProperty() should be used instead.

### Cloning an object
```javascript
var obj = { a: 1 };
var copy = Object.assign({}, obj);
console.log(copy); // { a: 1 }
```

### Merging objects
```javascript
var o1 = { a: 1 };
var o2 = { b: 2 };
var o3 = { c: 3 };

var obj = Object.assign(o1, o2, o3);
console.log(obj); // { a: 1, b: 2, c: 3 }
console.log(o1);  // { a: 1, b: 2, c: 3 }, target object itself is changed.
```

## Object.create
The `Object.create(proto[, propertiesObject])` method creates a new object with the specified prototype object and properties.

```javascript
// Shape - superclass
function Shape() {
  this.x = 0;
  this.y = 0;
}

// superclass method
Shape.prototype.move = function(x, y) {
  this.x += x;
  this.y += y;
  console.info('Shape moved.');
};

// Rectangle - subclass
function Rectangle() {
  Shape.call(this); // call super constructor.
}

// subclass extends superclass
Rectangle.prototype = Object.create(Shape.prototype);
Rectangle.prototype.constructor = Rectangle;

var rect = new Rectangle();

console.log('Is rect an instance of Rectangle?',
  rect instanceof Rectangle); // true
console.log('Is rect an instance of Shape?',
  rect instanceof Shape); // true
rect.move(1, 1); // Outputs, 'Shape moved.'
```

## Object.defineProperty
The `Object.defineProperty(obj, prop, descriptor)` method defines a new property directly on an object, or modifies an existing property on an object, and returns the object.

## Object.entries
The `Object.entries(obj)` method returns an array of a given object's own enumerable property [key, value] pairs, in the same order as that provided by a for...in loop (the difference being that a for-in loop enumerates properties in the prototype chain as well).

## Object.freeze / Object.isFrozen
The `Object.freeze(obj)` method freezes an object: that is, prevents new properties from being added to it; prevents existing properties from being removed; and prevents existing properties, or their enumerability, configurability, or writability, from being changed, it also prevents the prototype from being changed.  The method returns the object in a frozen state.

## Object.getOwnPropertyDescriptor
The `Object.getOwnPropertyDescriptor(obj)` method returns a property descriptor for an own property (that is, one directly present on an object and not in the object's prototype chain) of a given object.

## Object.getPrototypeOf
The `Object.getPrototypeOf(obj)` method returns the prototype (i.e. the value of the internal [[Prototype]] property) of the specified object.

## Object.is
The `Object.is(value1, value2)` method determines whether two values are the same value: two values are the same if one of the following holds:
* both undefined
* both null
* both true or both false
* both strings of the same length with the same characters
* both the same object
* both numbers and
    * both +0
    * both -0
    * both NaN
    * or both non-zero and both not NaN and both have the same value

This is not the same as being equal according to the == operator. The == operator applies various coercions to both sides (if they are not the same Type) before testing for equality (resulting in such behavior as "" == false being true), but Object.is doesn't coerce either value.

This is also not the same as being equal according to the === operator. The === operator (and the == operator as well) treats the number values -0 and +0 as equal and treats Number.NaN as not equal to NaN.

## Object.isExtensible
The `Object.isExtensible(obj) method determines if an object is extensible (whether it can have new properties added to it).

```javascript
// New objects are extensible.
var empty = {};
Object.isExtensible(empty); // === true

// ...but that can be changed.
Object.preventExtensions(empty);
Object.isExtensible(empty); // === false

// Sealed objects are by definition non-extensible.
var sealed = Object.seal({});
Object.isExtensible(sealed); // === false

// Frozen objects are also by definition non-extensible.
var frozen = Object.freeze({});
Object.isExtensible(frozen); // === false
```

## Object.keys
The `Object.keys(obj)` method returns an array of a given object's own enumerable properties, in the same order as that provided by a for...in loop (the difference being that a for-in loop enumerates properties in the prototype chain as well).

```javascript
var arr = ['a', 'b', 'c'];
console.log(Object.keys(arr)); // console: ['0', '1', '2']

// array like object
var obj = { 0: 'a', 1: 'b', 2: 'c' };
console.log(Object.keys(obj)); // console: ['0', '1', '2']

// array like object with random key ordering
var anObj = { 100: 'a', 2: 'b', 7: 'c' };
console.log(Object.keys(anObj)); // ['2', '7', '100']

// getFoo is property which isn't enumerable
var myObj = Object.create({}, {
  getFoo: {
    value: function () { return this.foo; }
  } 
});
myObj.foo = 1;
console.log(Object.keys(myObj)); // console: ['foo']
```

## Object.seal / Object.isSealed
The `Object.seal(obj)` method seals an object, preventing new properties from being added to it and marking all existing properties as non-configurable. Values of present properties can still be changed as long as they are writable.

## Object.values
The `Object.values(obj)` method returns an array of a given object's own enumerable property values, in the same order as that provided by a for...in loop (the difference being that a for-in loop enumerates properties in the prototype chain as well).
