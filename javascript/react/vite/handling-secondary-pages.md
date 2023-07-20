# How to handle secondary pages in Vercel and Vite

When using client-side routing in SPAs (e.g., Vue Router, React Router), the
server is not directly aware of the client-side routes. When you refresh a page,
Vercel tries to resolve the URL on the server-side, but since the page doesn't
exist on the server (it's handled by the client-side router), it results in a
404 error.

To resolve this issue, you need to set up a catch-all or wildcard rule in your
Vercel configuration to direct all requests to your main index.html file, where
your SPA is loaded. This way, even if the URL doesn't correspond to a physical
file on the server, the server will return the main index.html, and your
client-side router can handle the route and display the correct page.

Follow these steps to configure your Vercel deployment to handle client-side
routing correctly:

1. Create or modify the vercel.json (or now.json) file in the root of your
   project.

2. Add a rewrites section to the vercel.json file with a catch-all rule:

```json
{
  "rewrites": [
    {
      "source": "/(.*)",
      "destination": "/index.html"
    }
  ]
}
```

Deploy your application to Vercel again with the updated configuration.

With this configuration, any request to your Vercel deployment will be
redirected to the index.html file, and your client-side router will take over
from there, correctly rendering the appropriate page based on the URL.

Please note that using this catch-all rule may affect any other server-side
routes or APIs you might have. If you have specific API routes, you'll need to
make sure they are excluded from this catch-all rule and properly configured in
your Vercel configuration.

Once you've made these changes, try refreshing the secondary page again, and it
should load correctly without encountering a 404 error.
