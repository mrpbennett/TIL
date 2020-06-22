# `.filter()` array method

The `filter()` method **creates a new array** with elements that fall under a given criteria from an existing array:

#### The syntax

```javascript
var newArray = array.filter(function (item) {
    return condition;
});
```

The item argument is a reference to the current element in the array as filter() checks it against the condition. This is useful for accessing properties, in the case of objects.

If the current item passes the condition, it gets sent to the new array.

#### Filter an array of objects

A common use case of .filter() is with an array of objects through their properties, this could be used to check if an array of objects containing resturants and wether they're open.

```javascript
const resturants = [
    {
        name: 'Hawksmoor',
        location: 'London',
        isOpen: true,
    },
    {
        name: 'Polpo',
        location: 'London',
        isOpen: false,
    },
    {
        name: 'Five Guys',
        location: 'London',
        isOpen: false,
    },
    {
        name: 'Nandos',
        location: 'London',
        isOpen: true,
    },
];

const checkIfResturantIsOpen = function (resturants) {
    return resturants.filter((resturant) => {
        return resturant.isOpen;
    });
};

console.log(checkIfResturantIsOpen(resturants));
// returns -> Array [Object { name: "Hawksmoor", location: "London", isOpen: true }, Object { name: "Nandos", location: "London", isOpen: true }]
```
