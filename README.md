# fagaruREST-API-Transaction

## You need to learn a bit of oAuth2

Whatever the way you use to retrieve the `access_token`, you want to get something like this :


### `grant_type=password`

You still want an `access_token` but you get it in one request, by sending everything you have : `oauth_client` `id` and `secret`, and user credentials.

#### `Request`
```
API_URL/oauth/v2/token?client_id=CLIENT_ID&client_secret=CLIENT_SECRET&grant_type=password&username=USERNAME&password=PASSWORD
```

#### `Output`
```
{
    access_token: "NGM3NDI2OGQ0MTRjMjhkYzY5ZGQ1YjViODhmYzNlZmRiNGI3YjIxN2IxZDcxY2ZjMDI3MmY3NjI2N2ZhODJjYQ"
    expires_in: 3600
    token_type: "bearer"
    scope: null
    refresh_token: "MjQyNTM0NjBiMmZlYjY3MGM2OGJmMDllZjE0ZjNhYTMxZmIyN2ZmMGRlOGJlOGUwYjRkZmJkMWU4NmY5NDVlYQ"
}
```

### `grant_type=refresh_token`

This one is to refresh your `access_token`. As your token will expire in one hour, you can ask to refresh it :

#### `Request`
```
API_URL/oauth/v2/token?client_id=CLIENT_ID&client_secret=CLIENT_SECRET&grant_type=refresh_token&refresh_token=REFRESH_TOKEN
```

#### `New Output`
```
{
    access_token: "NGM3NDI2OGQ0MTRjMjhkYzY5ZGQ1YjViODhmYzNlZmRiNGI3YjIxN2IxZDcxY2ZjMDI3MmY3NjI2N2ZhODJjYQ"
    expires_in: 3600
    token_type: "bearer"
    scope: null
    refresh_token: "MjQyNTM0NjBiMmZlYjY3MGM2OGJmMDllZjE0ZjNhYTMxZmIyN2ZmMGRlOGJlOGUwYjRkZmJkMWU4NmY5NDVlYQ"
}
```
