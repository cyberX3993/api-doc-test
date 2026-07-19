# Error Handling

The API uses a consistent error response format for all failed requests.

## Error Format

```json
{
  "success": false,
  "error": {
    "code": "VALIDATION_ERROR",
    "message": "Email address is invalid.",
    "details": {
      "field": "email"
    }
  },
  "requestId": "req_123456789"
}
```

## Common HTTP Status Codes

- `200 OK` — Successful GET or PUT request
- `201 Created` — Resource created successfully
- `204 No Content` — Request succeeded with no body
- `400 Bad Request` — Invalid request payload or parameters
- `401 Unauthorized` — Authentication missing or invalid
- `403 Forbidden` — Authenticated but insufficient permissions
- `404 Not Found` — Resource does not exist
- `409 Conflict` — Resource conflict or duplicate entry
- `422 Unprocessable Entity` — Validation failed
- `429 Too Many Requests` — Rate limit exceeded
- `500 Internal Server Error` — Unexpected server failure

## Validation Errors

Validation errors return `422 Unprocessable Entity` with field-specific details.

Example:

```json
{
  "success": false,
  "error": {
    "code": "VALIDATION_ERROR",
    "message": "Request validation failed.",
    "details": {
      "field": "email",
      "reason": "Must be a valid email address."
    }
  },
  "requestId": "req_123456789"
}
```

## Authorization Errors

Example:

```json
{
  "success": false,
  "error": {
    "code": "UNAUTHORIZED",
    "message": "Access token has expired.",
    "details": null
  },
  "requestId": "req_123456789"
}
```
