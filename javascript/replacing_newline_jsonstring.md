# How to replace `\n` in a JSON string

When using APIs sometimes you're returned with a JSON string like so:

```json
"something": "hello\n my name is bob
```

This would print out in HTML as "hello my name is bob" where as you want:

hello
my name is bob.

To achieve this it's pretty easy as it happens. You just need to add `white-space: pre-wrap` to your element like so


```css
.new-line {
    white-space: pre-wrap;
}
```

```html
<div id='description'>
   <p className='new-line'>{route.description}</p>
</div>
```
