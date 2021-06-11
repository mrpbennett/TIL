# How to replace `\n` in a JSON string

When using APIs sometimes you're returned with a JSON string like so:

```json
"something": "hello\n my name is bob and i like to code in\n python
```

This would print out in HTML as `hello my name is bob and i like to code in python` where as you want:

```txt
hello
my name is bob and i like to code in
python.
```

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
