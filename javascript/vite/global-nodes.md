# Resolving global is not defined at node_modules

To resolve this issue we can define a global variable like so.

As global variable is not available. It will work when you define a global in vite.config

```javascript
export default defineConfig({
     plugins: [react()], 
     define: { 
        global: {}
    }
})
```