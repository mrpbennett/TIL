# Creating a dynamic pixel for webpages

### Issue:

When users are unable to use tools such as Google Tag Manager, they will have to hard code the `page_url` and `referrer_url`. Which means when the page changes it's the incorrect url.

### Solution

Create a script that appends a dynamically created tag into the `<head>` that uses the DOM to add the current page url and referrer url if any.

- [window.location.href](https://developer.mozilla.org/en-US/docs/Web/API/Window/location): Captures the current pages url
- [document.referrer](https://developer.mozilla.org/en-US/docs/Web/API/Document/referrer): Captures the referrer url if any.

```javascript
<script type="text/javascript">
    const PP_PAGE_URL = window.location.href;
    const PP_REFERRER_URL = document.referrer;
    const US_PRIVACY = '${us_privacy}';

    const newPixel = document.createElement("script");
    newPixel.type  = "text/javascript";
    newPixel.src = `https://bh.domain.com/cp?&token=ABCXYZ&us_privacy=${US_PRIVACY}&ch=1&url=${PP_PAGE_URL}&rr=${PP_REFERRER_URL}`
    newPixel.async = true;

    document.getElementsByTagName('head')[0].appendChild(newPixel);
</script>
```