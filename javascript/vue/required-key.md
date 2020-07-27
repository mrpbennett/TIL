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

### Accessing an array within an array of objects.

This is how I would access items from an array to print them out seperately.

```javascript
projects: [
                {
                    title: 'URL Clean Up',
                    url: 'https://clean-urls.netlify.app/',
                    description:
                        "Simple Vue.js app that uses regex to clean up a list of urls. When you're in need to remove the https:// and what comes after the .com.",
                    tech: ['vue.js', 'tailwindcss', 'regex'],
                },
```

```javascript
 <div>
    <div
        v-for="project in projects"
        :key="project.id"
        class="rounded shadow mb-2 text-gray-600 bg-gray-900 hover-scale-project-g cursor-pointer project-fill"
    >
        <a href="#">
            <div class="p-4 md:p-8">
                <span
                    v-for="tag in project.tech"
                    :key="tag.id"
                    class="mr-4 text-xs font-bold tracking-widest uppercase truncate"
                    >{{ tag }}</span
                >
                <h3
                    class="mt-4 text-base md:text-xl truncate text-gray-300"
                >
                    {{ project.title }}
                </h3>
                <p class="text-sm m-0">{{ project.description }}</p>
            </div>
        </a>
    </div>
</div>
```
