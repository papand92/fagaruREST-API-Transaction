# fagaru REST-API-Transaction

### `grant_type=password` Obtenir un `access_token`
Pour obtenir un `access_token` vous devez faire une demande en envoyant vos informations d'identification:`CLIENT_ID`, `CLIENT_SECRET`, `USERNAME`et `PASSWORD` à l'url `API_URL`

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

### `grant_type=refresh_token` Rafraîchir votre `access_token`

Lorsque votre token expirera en une heure, vous pouvez le rafraîchir:

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
