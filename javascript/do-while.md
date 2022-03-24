# Do While Loops

Never really thought about these, but these loops allow you to run a code block at least once EVEN if the condition in the while loop isn't true.

```js
const names = ['John', 'Jane', 'Mary', 'Mark', 'Bob'];

let i = 0 

do {
  console.log(names[i]);
  i++;
}
while(i < 5);
```

This will run at least once

```js
do {
  console.log(names[i]);
  i++;
}
```

Quite handy if you ask me...