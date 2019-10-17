# `.filter()` array method

`.filter()` creates a new array with elements that fall under a given criteria from an existing array:

```javascript
const numbers = [1, 3, 6, 8, 22];

const lucky = numbers.filter(function(number) {
	return number > 7;
});

// [8, 11]
```

#### The syntax

```javascript
var newArray = array.filter(function(item) {
	return condition;
});
```

The item argument is a reference to the current element in the array as filter() checks it against the condition. This is useful for accessing properties, in the case of objects.

If the current item passes the condition, it gets sent to the new array.

#### Filter an array of objects

A common use case of .filter() is with an array of objects through their properties:

```javascript
var heroes = [
	{name: “Batman”, franchise: “DC”},
	{name: “Ironman”, franchise: “Marvel”},
	{name: “Thor”, franchise: “Marvel”},
	{name: “Superman”, franchise: “DC”}
];

var marvelHeroes =  heroes.filter(function(hero) {
	return hero.franchise == “Marvel”;
});

// [ {name: “Ironman”, franchise: “Marvel”}, {name: “Thor”, franchise: “Marvel”} ]
```
