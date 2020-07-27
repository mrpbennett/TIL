# vue/require-v-for-key

I was having issues trying to loop through an array of objects in vue.js. I was trying to replicate my personal site in Vue.js using Gridsom.

I couldn't use .map() like I could in React.

```javascript
<template>
  <!-- ✓ GOOD -->
  <div
    v-for="todo in todos"
    :key="todo.id"
  />
  <!-- ✗ BAD -->
  <div v-for="todo in todos"/>
</template>
```

Adding the `:key` helped me with the issue as I kept on getting errors after using `v-for` without a key.
