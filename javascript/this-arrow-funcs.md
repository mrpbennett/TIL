# How `this` works in arrow functions

When defined as a method of an object, in a regular function this refers to the object, so you can do:

```javascript
const car = {
	model: 'Fiesta',
	manufacturer: 'Ford',
	fullName: function() {
		return `${this.manufacturer} ${this.model}`;
	},
};
```

calling `car.fullName()` will return `"Ford Fiesta"`.

The this scope with arrow functions is inherited from the execution context. An arrow function does not bind this at all, so its value will be looked up in the call stack, so in this code `car.fullName()` will not work, and will return the string `"undefined undefined"`:

```javascript
const car = {
	model: 'Fiesta',
	manufacturer: 'Ford',
	fullName: () => {
		return `${this.manufacturer} ${this.model}`;
	},
};
```

Due to this, arrow functions are not suited as object methods.
