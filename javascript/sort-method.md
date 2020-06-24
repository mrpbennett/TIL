#

The `sort()` method sorts the elements of an array in place and returns the sorted array. The default sort order is ascending, built upon converting the elements into strings, then comparing their sequences of UTF-16 code units values.

The time and space complexity of the sort cannot be guaranteed as it depends on the implementation.

```javascript
const months = ['March', 'Jan', 'Feb', 'Dec'];
months.sort();
console.log(months);
// expected output: Array ["Dec", "Feb", "Jan", "March"]
```

`sort()` can also take a function enabling us to compare values to sort across.

```
arr.sort([compareFunction])
```

**compareFunction Optional**
Specifies a function that defines the sort order. If omitted, the array elements are converted to strings, then sorted according to each character's Unicode code point value.

**firstEl**
The first element for comparison.

**secondEl**
The second element for comparison.

### Example

```javascript
const todos = [
    {
        title: 'Ride up Leith Hill & Combe Lane',
        completed: true,
    },
    {
        title: 'Make a Thai Currey',
        completed: false,
    },
    {
        title: 'Go for a run',
        completed: true,
    },
    {
        title: 'Learn more about React',
        completed: false,
    },
    {
        title: 'Take Charlie for a poop',
        completed: true,
    },
];

// sort todos
const sortCompletedToDos = function (array) {
    array.sort(function (a, b) {
        if (!a.completed && b.completed) {
            return -1;
        } else if (!b.completed && a.completed) {
            return 1;
        } else {
            return 0;
        }
    });
};

sortCompletedToDos(todos);
console.log(todos); // returns ---
/*
[
  { title: 'Make a Thai Currey', completed: false },
  { title: 'Learn more about React', completed: false },
  { title: 'Ride up Leith Hill & Combe Lane', completed: true },
  { title: 'Go for a run', completed: true },
  { title: 'Take Charlie for a poop', completed: true }
]*/
```
