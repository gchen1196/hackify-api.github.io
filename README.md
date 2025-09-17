# Hackify API - Core Endpoints Documentation

This document describes the core API endpoints for the Hackify API, a music service built with NestJS, Prisma ORM, and PostgreSQL.

## 🎵 API Endpoints

### 1. Like a Song (Add to Playlist)

**Endpoint:** `POST /liked`

**Description:** Adds a song to the user's liked playlist.

**Request Body:**

```json
{
  "songId": "string",
  "userId": "string"
}
```

**Response (201 Created):**

```json
{
  "success": true,
  "message": "Song added to liked playlist successfully",
  "data": {
    "id": "string",
    "createdAt": "2024-01-01T00:00:00.000Z",
    "song": {
      "id": "string",
      "title": "string",
      "artist": "string",
      "album": "string | null",
      "genre": "string | null",
      "duration": 180,
      "releaseDate": "2024-01-01T00:00:00.000Z | null",
      "audioUrl": "string",
      "imageUrl": "string | null",
      "isExplicit": false,
      "playCount": 1000,
      "createdAt": "2024-01-01T00:00:00.000Z",
      "updatedAt": "2024-01-01T00:00:00.000Z"
    }
  }
}
```

**Error Responses:**

- `404 Not Found`: Song or user not found
- `409 Conflict`: Song already in liked playlist
- `400 Bad Request`: Invalid request body

---

### 2. Remove Liked Song

**Endpoint:** `DELETE /liked?id={songId}&userId={userId}`

**Description:** Removes a song from the user's liked playlist.

**Query Parameters:**

- `id` (required): Song ID to remove
- `userId` (required): User ID

**Response (200 OK):**

```json
{
  "success": true,
  "message": "Song removed from liked playlist successfully"
}
```

**Error Responses:**

- `404 Not Found`: Song not found in liked playlist
- `400 Bad Request`: Missing required parameters

---

### 3. Search Songs

**Endpoint:** `GET /search?q={query}&limit={limit}&offset={offset}`

**Description:** Searches for songs by title, artist, or album name.

**Query Parameters:**

- `q` (required): Search query
- `limit` (optional): Number of results (1-100, default: 10)
- `offset` (optional): Pagination offset (default: 0)

**Response (200 OK):**

```json
{
  "data": [
    {
      "id": "string",
      "title": "string",
      "artist": "string",
      "album": "string | null",
      "genre": "string | null",
      "duration": 180,
      "releaseDate": "2024-01-01T00:00:00.000Z | null",
      "audioUrl": "string",
      "imageUrl": "string | null",
      "isExplicit": false,
      "playCount": 1000,
      "createdAt": "2024-01-01T00:00:00.000Z",
      "updatedAt": "2024-01-01T00:00:00.000Z"
    }
  ],
  "total": 25,
  "page": 1,
  "limit": 10,
  "totalPages": 3
}
```

**Error Responses:**

- `400 Bad Request`: Empty search query

---

### 4. Get Liked Songs

**Endpoint:** `GET /liked?userId={userId}&cursor={cursor}&limit={limit}`

**Description:** Retrieves all songs from the user's liked playlist with cursor-based pagination.

**Query Parameters:**

- `userId` (required): User ID
- `cursor` (optional): Cursor for pagination (ISO date string)
- `limit` (optional): Number of results (1-100, default: 10)

**Response (200 OK):**

```json
{
  "data": [
    {
      "id": "string",
      "createdAt": "2024-01-01T00:00:00.000Z",
      "song": {
        "id": "string",
        "title": "string",
        "artist": "string",
        "album": "string | null",
        "genre": "string | null",
        "duration": 180,
        "releaseDate": "2024-01-01T00:00:00.000Z | null",
        "audioUrl": "string",
        "imageUrl": "string | null",
        "isExplicit": false,
        "playCount": 1000,
        "createdAt": "2024-01-01T00:00:00.000Z",
        "updatedAt": "2024-01-01T00:00:00.000Z"
      }
    }
  ],
  "nextCursor": "2024-01-01T00:00:00.000Z",
  "hasMore": true,
  "total": 50
}
```

**Error Responses:**

- `404 Not Found`: User not found
- `400 Bad Request`: Missing required parameters

---

### 5. Get User by ID

**Endpoint:** `GET /users/{userId}`

**Description:** Retrieves a specific user by their ID.

**Path Parameters:**

- `userId` (required): The unique identifier of the user

**Response (200 OK):**

```json
{
  "id": "user_001",
  "email": "hackerrank@example.com",
  "username": "hackerrank_music",
  "firstName": "John",
  "lastName": "Doe",
  "avatar": "https://images.unsplash.com/photo-1472099645785-5658abf4ff4e?w=150&h=150&fit=crop&crop=face",
  "isActive": true,
  "createdAt": "2024-01-01T00:00:00.000Z",
  "updatedAt": "2024-01-01T00:00:00.000Z"
}
```

**Error Responses:**

- `404 Not Found`: User with the specified ID not found

