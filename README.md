# Authentication Service

It is the authentication for all services in Skenema.

## How to use

First, you must send credentials to `/api/token/` via POST request.

```json
{
  "username": "username",
  "password": "pAsSw0rd"
}
```

Then, you will receive `access` and `refresh` token like this.

```json
{
  "access": "access_token",
  "refresh": "refresh_token"
}
```

Access token is used to authenticate while refresh token is used to refresh access token.


You authenticate like this in HTTP header

```text
Authorization: Bearer access_token_here
```

To refresh token, you send a POST request to `/api/token/refresh/` with your refresh token.

```json
{
  "refresh": "refresh_token"
}
```
You will receive the access token again.

```json
{
  "access_token": "new_token_here"
}
```
You can re-use refresh token until it expires. After that, you must re-authenticate yourself.


## Setup with other Django Rest Framework services

Setup Django Rest authentication like [this](https://django-rest-framework-simplejwt.readthedocs.io/en/latest/stateless_user_authentication.html)

Then, you must configure `SIMPLE_JWT` to use the same signing key.

```python
SIMPLE_JWT = {
    'SIGNING_KEY': 'MUST BE THE SAME AS AUTHENTICATION SERVICE'
}
```

After this setup, you should be able to authenticate without sharing the same database.


## TODO
- [ ] Make it more secure by removing secret from being hardcoded.
- [ ] Make it possible to use production database (probably Postgres or MySQL)