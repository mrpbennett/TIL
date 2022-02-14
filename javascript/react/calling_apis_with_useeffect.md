# Calling useEffect() conditionally 

As we know [`useEffect`](https://reactjs.org/docs/hooks-reference.html#useeffect) will run once the component is loaded. This is great for calling APIs when the page loads, but what if you wanted to have say an edit button on a list of contacts. 

That edit button will get called as many times as you have contacts, which is not what we want. We can however call `useEffect` conditionally which will prevent useEffect from firing off to an api.

```js
  let [contact, setContact] = useState({});

  ...

  useEffect(() => {
    async function fetchContact() {
      await fetch(`http://localhost:8000/get-contact/${props.contactId}`)
        .then((response) => response.json())
        .then((data) => {
          setContact(data);
          console.log(data);
        })
        .catch((err) => console.log(err));
    }

    if (isOpen) {
      fetchContact();
    }
  }, [isOpen]);
```

We can easily do this by adding a conditional if statement. The code above runs when the modal is open hence why we're checking if `isOpen` is `true`.

```js
let [isOpen, setIsOpen] = useState(false);
 ```

When the `state` here is set to true, the modal will then open and fire `uesEffect` because the state `isOpen` has been set to `true` by an `onClick` method.

Adding things this way prevents the application from calling the API multiple times and only when it's required to do so.