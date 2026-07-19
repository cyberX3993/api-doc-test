# Rate Limits

The Student Management API enforces request throttling to protect availability.

## Limits

- `100` requests per minute per API key or user session
- Requests exceeding the limit return `429 Too Many Requests`
- `Retry-After` header indicates when the client may safely retry

## Example Response

```http
HTTP/1.1 429 Too Many Requests
Retry-After: 60
Content-Type: application/json

{
  "success": false,
  "error": {
    "code": "TOO_MANY_REQUESTS",
    "message": "Rate limit exceeded. Please retry after 60 seconds.",
    "details": null
  },
  "requestId": "req_123456789"
}
```

## Best Practices

- Cache responses when appropriate
- Use exponential backoff for retries
- Avoid polling endpoints unnecessarily
