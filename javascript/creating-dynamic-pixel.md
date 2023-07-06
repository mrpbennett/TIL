# Creating a dynamic pixel for webpages

### Issue:

When users are unable to use tools such as Google Tag Manager, they will have to hard code the `page_url` and `referrer_url`. Which means when the page changes it's the incorrect url.

### Solution

Create a script that appends a dynamically created tag into the `<head>` that uses the DOM to add the current page url and referrer url if any.

- [window.location.href](https://developer.mozilla.org/en-US/docs/Web/API/Window/location): Captures the current pages url
- [document.referrer](https://developer.mozilla.org/en-US/docs/Web/API/Document/referrer): Captures the referrer url if any.

```javascript
<script type="text/javascript">
    // store required data into variables
    const PP_PAGE_URL = window.location.href;
    const PP_REFERRER_URL = document.referrer;

    // create a new script element 
    const pixel = document.createElement("script");
    pixel.type  = "text/javascript";
    pixel.src = `https://bh.cw.com/cp?p=1234&token=XYZ&us_privacy=${us_privacy}&ch=1&url=${PP_PAGE_URL}&rr=${PP_REFERRER_URL}`
    pixel.async = true;

    // append the new script element to the Head element
    document.head.appendChild(pixel);
  </script>
```

Or if you want to use this in React you can do so like this.

```javascript
const generateDynamicPixel = () => {
    // store required data into variables
    const PP_PAGE_URL = window.location.href;
    const PP_REFERRER_URL = document.referrer;

    // create a new script element 
    const pixel = document.createElement("script");
    pixel.type  = "text/javascript";
    pixel.src = `https://bh.cw.com/cp?p=1234&token=XYZ&us_privacy=${us_privacy}&ch=1&url=${PP_PAGE_URL}&rr=${PP_REFERRER_URL}`
    pixel.async = true;

    // append the new script element to the Head element
    document.head.appendChild(pixel);
}

useEffect(() => {
   generateDynamicPixel()
}, []}
