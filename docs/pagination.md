# Pagination

The API supports pagination on list endpoints using `page` and `limit` query parameters.

## Pagination Parameters

- `page` — Page number, starting at `1`
- `limit` — Number of items per page

### Example

```http
GET /students?page=1&limit=20
```

### Example Response

```json
{
  "data": [
    {
      "id": "a6e55c90-f8b2-4a64-b3d6-a1d95b7d45fa",
      "firstName": "John",
      "lastName": "Doe",
      "email": "john@example.com",
      "phone": "+1-555-123-4567",
      "dateOfBirth": "2010-03-18",
      "gender": "Male",
      "courseId": "course-001",
      "status": "ACTIVE",
      "createdAt": "2026-01-01T12:00:00Z",
      "updatedAt": "2026-01-01T12:00:00Z"
    }
  ],
  "pagination": {
    "page": 1,
    "limit": 20,
    "totalItems": 235,
    "totalPages": 12,
    "hasNext": true,
    "hasPrevious": false
  }
}
```

## Response Metadata

- `totalItems` — Total number of records matching the filter
- `totalPages` — Total pages available
- `hasNext` — `true` when a next page exists
- `hasPrevious` — `true` when a previous page exists
