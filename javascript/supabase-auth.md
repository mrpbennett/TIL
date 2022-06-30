# Handle Vercel preview and Supabase auth

- **SITE_URL** = make this your official site URL
- 
- Additional site URLs = should be something like this: 

- `http://localhost:3000/*/*`
- `https://*vercel.app/*/*`

Then when you do a signIn you can do:

```javascript
const { user, session, error } = await supabase.auth.signIn({
  provider: 'google'
}, {
  redirectTo: process.env.NEXT_PUBLIC_SITE_URL // set this to your site URL as an env var
})
```