<h1 align="center">Next.js and Supabase Auth Template</h1>

![Site](https://github.com/ankush-003/supa-auth/assets/94037471/5d907fd5-b8c4-492e-bce2-68d44b4628ab)


## PKCE (Proof Key For Code Exchange) Auth Flow
When using the PKCE flow, a redirect URL will be returned with the following structure:

```
https://yourapp.com/...?code=<...>
```

The code parameter is commonly known as the Auth Code and can be exchanged for an access token by calling exchangeCodeForSession(code).

>INFO: For security purposes, the code has a validity of 5 minutes and can only be exchanged for an access token once. You will need to restart the authentication flow from scratch if you wish to obtain a new access token.

As the flow is run server side, localStorage may not be available. You may configure the client library to use a custom storage adapter an alternate backing storage such as cookies by setting the storage option to an object with the following methods:

## Google OAuth

- First you'll need to create a Google project inside their cloud console, in other providers they may refer to this as an "app" and is usually available on the company's developer portal.
- Once you have a project, type "OAuth" into the search bar and open up "OAuth Consent Screen"
- Select 'External' and proceed to fill out the rest of the form fields
- And click to create a new set of credentials, select OAuth client ID as the option
- Now choose Web Application (assuming you're creating a web app) and in the Authorized redirect URI section you need to add: https://<your-ref>.supabase.co/auth/v1/callback. You can find your Supabase URL in Settings > API inside the Supabase dashboard.

- Hit save. Now you should be able to navigate in the browser to:

```https://<your-ref>.supabase.co/auth/v1/authorize?provider=google```

And log in to your service using any Google or Gmail account.

You can additionally add a query parameter redirect_to= to the end of the URL for example:

```https://<your-ref>.supabase.co/auth/v1/authorize?provider=google&redirect_to=http://localhost:3000/welcome```

But make sure any URL you enter here is on the same host as the site url that you have entered on the Auth > Settings page on the Supabase dashboard. (There is additional functionality coming soon, where you'll be able to add additional URLs to the allow list).

If you want to redirect the user to a specific page in your website or app after a successful authentication.

You also have the option of requesting additional scopes from the OAuth provider. Let's say for example you want the ability to send emails on behalf of the user's gmail account. You can do this by adding the query parameter scopes, like:

```https://<your-ref>.supabase.co/auth/v1/authorize?provider=google&https://www.googleapis.com/auth/gmail.send```

Alternatively, if you're using the supabase-js client library, you can call the signInWithOAuth() method:

```const { data, error } = await supabase.auth.signInWithOAuth({ provider: 'google' })```

> Note: The data that you receive will contain the url, that you'll be have to redirect to from server side (if you're using SSR auth with PKCE flow)

## Features

- Works across the entire [Next.js](https://nextjs.org) stack
  - App Router
  - Pages Router
  - Middleware
  - Client
  - Server
  - It just works!
- supabase-ssr. A package to configure Supabase Auth to use cookies

You can view a fully working demo at [demo-nextjs-with-supabase.vercel.app](https://supa-auth-seven.vercel.app/).

- Official Template

   ```bash
   npx create-next-app -e with-supabase
   ```

## Refereces

- [Understand SSR Auth](https://supabase.com/docs/guides/auth/server-side-rendering)
- [Creating SSR client](https://supabase.com/docs/guides/auth/server-side/creating-a-client)
- [Cookie-based Auth and the Next.js 13 App Router (free course)](https://youtube.com/playlist?list=PL5S4mPUpp4OtMhpnp93EFSo42iQ40XjbF)
- [Email PKCE Auth](https://supabase.com/docs/guides/auth/server-side/email-based-auth-with-pkce-flow-for-ssr)
- [Supabase Auth and the Next.js App Router](https://github.com/supabase/supabase/tree/master/examples/auth/nextjs)
- [Email Auth with PKCE flow for SSR](https://supabase.com/docs/guides/auth/server-side/email-based-auth-with-pkce-flow-for-ssr)
- [Google Auth](https://supabase.com/docs/learn/auth-deep-dive/auth-google-oauth)
