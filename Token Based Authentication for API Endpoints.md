# Token Based Authentication for API Endpoints

We have added support for a token-based authentication scheme for reporting API endpoints. Via an application key or 
standard PJC credentials, a temporary security token can be retrieved that will grant access to reporting endpoints, include the transaction detail report.

## Retrieving a token

You can retrieve an **authorization token** using either PJC login credentials or an application key. In cases where another computer system is accessing a reporting endpoint, an application key is preferable.

An **application key** is a [UUID](https://en.wikipedia.org/wiki/Universally_unique_identifier) created by the dev team (or, eventually, issued using a PJC admin tool). While authorization tokens are temporary, application keys never expire automatically. They need to be manually revoked (either by the dev team or, eventually, by the PJC admin tool) when they are no longer useful.

To request an **authorization token**, make an HTTP `POST` request to the `/token` API endpoint, e.g., `http://api.domain.edu/token`.

The body of this request should contain either a valid application key or valid PJC credentials (email and password). The request should indicate via the `Content-Type` header how the body is formatted. Media types `application/x-www-form-urlencoded` and `application/json` are supported.

### Examples

Request an authorization token using PJC credentials, with the body formatted as `application/x-www-form-urlencoded`:

```
curl -X POST
     -H "Content-Type: application/x-www-form-urlencoded"
     -d 'email=zach@uky.edu&password=abc123'
     "http://api.domain.edu/token"
```

Request an authorization token using PJC credentials, with the body formatted as `application/json`:

```
curl -X POST
     -H "Content-Type: application/json"
     -d '{ "email": "zach@uky.edu", "password": "abc123" }'
     "http://api.domain.edu/token"
```

Request an authorization token using an application key, with the request body formatted as `application/json`:

```
curl -X POST
     -H "Content-Type: application/json"
     -d '{ "key": "46ef840e-1c74-11e6-9eec-005056b800cc" }'
     "http://api.domain.edu/token"
```

Request an authorization token using an application key, with the request body formatted as `application/x-www-form-urlencoded`:

```
curl -X POST
     -H "Content-Type: application/x-www-form-urlencoded"
     -d 'key=46ef840e-1c74-11e6-9eec-005056b800cc'
     "http://api.domain.edu/token"
```

### Token Expiration

Unlike application keys, authentication tokens are temporary. Tokens issued with an application key expire ten minutes after being issued. Tokens issued with PJC credentials expire 24 hours after being issued.

Once a token has expired, further requests using that token will be result in an HTTP response code of 401 Unauthorized.

## Using an authentication token

Once you have retrieved a valid authentication token, it can be included in the `Authorization` header of subsequent HTTP requests. This system uses what is known as a "bearer authentication scheme", so the authentication token value must be prefixed with "Bearer" in the `Authorization` header.

Assuming we got back the authorization token `ABC.123.XYZ` from the `/token` endpoint (of course, actual authorization tokens are much longer than that), we could use it to access the transaction detail report like this:

```
curl -X GET
     -H "Authorization: Bearer ABC.XYZ.123"
     "http://api.domain.edu/reports/transactions"
```
