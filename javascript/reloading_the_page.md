# How to reload the page

Sometimes I want to reload the page to display new data after an call to an api. This would give a user better experience when it comes to something like deleting an item from a list of contacts.

```js
// delete contact from api then reloads the page
  function deleteContact() {
    fetch(`http://localhost:8000/delete-contact/${props.contactId}`, {
      method: 'DELETE',
    }).catch((err) => console.log(err));

    setIsOpen(false);
    window.location.reload(true);
  }
```

In the above example we can see that theres an API call to a delete method. At the bottom of the function we have 

```js
window.location.reload(true)
```

This method takes an optional parameter which by default is set to false. If set to true, the browser will do a complete page refresh from the server and not from the cached version of the page.