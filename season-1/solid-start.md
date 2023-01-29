# Start with Solid

_Solid here * and libraries * and we sign in_

## Sign in

Sign in with Solid identity. Fortunately there is a library `@inrupt/solid-client-authn-browser` which you can use for this purpose.

### UI

To sign in, people need to pick their Solid identity provider. (like https://solidcommunity.net) Usually apps offer a set of buttons with possible Solid providers. We rush, so we just add an input field, where people will write the url. Fix later... :) And we add a button: Sign in.



### Solid (@inrupt/solid-client) API

The signing itself works as follows:

So, if user is not signed in, they need to sign in first.

They fill their provider and click Login.

at that point we call [`login`](https://docs.inrupt.com/developer-tools/api/javascript/solid-client-authn-browser/functions.html#login)

```
import { login } from '@inrupt/solid-client-authn-browser'

//...
  await login({
    oidcIssuer: '...',
    redirectUrl: '...', // what is this?
    clientName: 'Sleepy Bike'
  })
```

This will lead to a page redirect, users will sign in, and we don't need to do anything more.

And the app restarts.

Whenever the app starts, it needs to call [`handleIncomingRedirect`](https://docs.inrupt.com/developer-tools/api/javascript/solid-client-authn-browser/functions.html#handleincomingredirect)

```tsx
import { handleIncomingRedirect } from '@inrupt/solid-client-authn-browser'

//...
  await handleIncomingRedirect()
//...
```

This function will check whether you're signed in, and it will return undefined or info about you

_what does it do?_

By this time, you should have signed in, and the app should have returned info about you.

Great!

We've also wired it with redux. We want to do this using [RTK-Query](https://redux-toolkit.js.org/rtk-query/overview). That's a TODO. :)

:arrow_right: **[Next: Show your data in the app](my-profile.md)**
