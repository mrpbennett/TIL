# `.splice()` method

The `.splice()` method adds/removes items to/from an array, and returns the removed item(s).

#### Removing an item

```javascript
<array>.splice(<index of array>, <how many items to delete>)
```

#### Adding an item

```javascript
<array>.splice(<index of array>, <how many items to delete>, <new item to add>)
```

### For Example:

```javascript
const todo = [
    'Get Washing In',
    'Fix purple bar at the base of Atlantic Night',
    'Stop Eima usng a pushchair',
    'Make charlie Poop',
    'Have Dinner',
];

todo.splice(2, 1); // deletes the 3rd item in the array
todo.splice(4, 1, 'New Note to be added'); // adds a new item as the 5th item in the array
```
