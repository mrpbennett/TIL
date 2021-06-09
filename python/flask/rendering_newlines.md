# Rendering `\n` in Jinja2

Say you have a json response which contains `\n` you would want to use those to create paragraphs right?

Well you can do so by using the following:

```html
<p>
  {{ data.description | replace('\n', '<br>') | safe}}
</p>
```

The `replace('\n', '<br>')` replaces `\n` with a break point `<br>` which in HTML breaks the line of text.

The ` | safe` filter renders it safe for HTML
