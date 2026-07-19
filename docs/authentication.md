# Authentication

The Student Management API uses JWT Bearer authentication for all protected endpoints.

## Authorization Header

Include the access token in the request header:

```http
Authorization: Bearer <access_token>
```

## Login

### POST /auth/login

Use this endpoint to authenticate a user and issue an access token with a refresh token.

### Request Body

- `email` (string, required)
- `password` (string, required)

### Response

Returns an access token and refresh token.

```json
{
  "accessToken": "eyJhbGci...",
  "refreshToken": "def502...",
  "expiresIn": 3600,
  "tokenType": "Bearer"
}
```

## Access Token

- Token type: `Bearer`
- Format: JWT
- Expiration: typically `3600` seconds (1 hour)
- Use the access token on every protected request

## Refresh Token

Refresh tokens are issued alongside access tokens and are used to obtain a new access token when the current one expires. The refresh token should be stored securely and only exchanged through a dedicated refresh endpoint if implemented.

## Expiration

When the access token expires, the API returns `401 Unauthorized` with an authentication error payload.

## Unauthorized Errors

Common authentication errors:

- `401 Unauthorized` — missing or invalid token
- `403 Forbidden` — token does not grant the required permissions

Example response:

```json
{
  "success": false,
  "error": {
    "code": "UNAUTHORIZED",
    "message": "Authorization header is missing or invalid.",
    "details": null
  },
  "requestId": "req_123456789"
}
```
