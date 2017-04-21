# fagaruREST-API-Transaction


## We have a situation here

We will imagine two Symfony projects : 
- one back with the API and database ([FOSRestBundle](https://github.com/FriendsOfSymfony/FOSRestBundle) and [FOSOAuthServerBundle](https://github.com/FriendsOfSymfony/FOSOAuthServerBundle) with [FOSUserBundle](https://github.com/FriendsOfSymfony/FOSUserBundle)), 
- and one front who consume the API ([HWIOAuthBundle](https://github.com/hwi/HWIOAuthBundle), no database), that one day will be replace by a JS implementation.

As our users will try to connect to our front, we want a login process _à la_ Facebook, which you will see, is the oAuth `grant_type` `authorization_code` process.

The front is an `oauth_client` who try to connect to the back.
This `oauth_client` is created with a command line on the back. You then retrieve an `id` and a `secret`.

**Warning** If you look into the database to get the `id`, it's the concatenation of the `oauth_client.id` and `oauth_client.random_id`, separated with an underscore. Something looking like `1_kj2gjhlice8wkoxwggpok80hk0wcewkwfkk4c4wocawwgc0ko`.

## You need to learn a bit of oAuth2

You need to understand that there are different "ways" to "connect" with oAuth2 and retrieve an `access_token` that you will use to hit your API. They are well explained in this [Tankist blog post](http://blog.tankist.de/blog/2013/07/18/oauth2-explained-part-3-using-oauth2-with-your-bare-hands/) (read them all, they are just great).

Whatever the way you use to retrieve the `access_token`, you want to get something like this :

```
{
    access_token: "NGM3NDI2OGQ0MTRjMjhkYzY5ZGQ1YjViODhmYzNlZmRiNGI3YjIxN2IxZDcxY2ZjMDI3MmY3NjI2N2ZhODJjYQ"
    expires_in: 3600
    token_type: "bearer"
    scope: null
    refresh_token: "MjQyNTM0NjBiMmZlYjY3MGM2OGJmMDllZjE0ZjNhYTMxZmIyN2ZmMGRlOGJlOGUwYjRkZmJkMWU4NmY5NDVlYQ"
}
```

These ways are defined by a `grant_type` that you set to an `oauth_client` (multiple `grant_type` is possible) (it might be specific to [FOSOAuthServerBundle](https://github.com/FriendsOfSymfony/FOSOAuthServerBundle), but I presume you will not use something else) :

### `grant_type=authorization_code`

The "usual" process you have with Facebook : login, authorize app, redirection. So the user want to connect to your front. Simplified, here is what's happening :

1. The front try to get the login form from the back, with its `oauth_client` `id`.
2. The user put its credentials in the form, and if it's valid, can allow the "app" (which is the `oauth_client`, _i.e._ the front) to access the back.
3. The user is then redirected to the front, with a nice cookie (`access_token`) that allow the front to request the back API.

No example here, we will come back on that process later.

### `grant_type=password`

You still want an `access_token` but you get it in one request, by sending everything you have : `oauth_client` `id` and `secret`, and user credentials.

```
your_back/oauth/v2/token?client_id=CLIENT_ID&client_secret=CLIENT_SECRET&grant_type=password&username=USERNAME&password=PASSWORD
```

The process is of course simpler, but your front is storing the `oauth_client` `secret`. It might be ok because our front is in PHP, but if it's one day in Javascript, it might not be good. Also the process is not as cool as the real "Facebook/Google/GitHub" one :)

### `grant_type=client_credentials`

Simplest request, no user credential, you only send `oauth_client` `id` and `secret` :

```
your_back/oauth/v2/token?client_id=CLIENT_ID&client_secret=CLIENT_SECRET&grant_type=client_credentials
```

This might be usefull when your back is requesting another of your API. User credential might not be needed.

### `grant_type=refresh_token`

This one is to refresh your `access_token`. As your token will expire in one hour, you can ask to refresh it :

```
your_back/oauth/v2/token?client_id=CLIENT_ID&client_secret=CLIENT_SECRET&grant_type=refresh_token&refresh_token=REFRESH_TOKEN
```

As you need to have the `oauth_client` `secret`, this is not usable between our front and back, where `grant_type=authorization_code` will be used.

## Bonus RFC-6749

I found it was the clearest explanation of the [authorization_code](http://tools.ietf.org/html/rfc6749#section-1.3.1) :

_The authorization code is obtained by using an authorization server as an intermediary between the client and resource owner. Instead of requesting authorization directly from the resource owner, the client directs the resource owner to an authorization server, which in turn directs the resource owner back to the client with the authorization code._

_Before directing the resource owner back to the client with the authorization code, the authorization server authenticates the resource owner and obtains authorization. Because the resource owner only authenticates with the authorization server, the resource owner's credentials are never shared with the client._

_The authorization code provides a few important security benefits, such as the ability to authenticate the client, as well as the transmission of the access token directly to the client without passing it through the resource owner's user-agent and potentially exposing it to others, including the resource owner._
