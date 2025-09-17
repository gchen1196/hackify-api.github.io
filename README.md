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

**Example Usage:**

```bash
# Get user with ID "user_001"
GET /users/user_001

# Get user with ID "user_002"
GET /users/user_002
```

---

## 🏗️ Architecture

The API endpoints are implemented using NestJS feature-based modules:

```
src/
├── search/                    # Search functionality
│   ├── controllers/
│   │   └── search.controller.ts    # @Controller('search')
│   ├── services/
│   │   └── search.service.ts       # Search business logic
│   ├── dto/
│   │   └── search-songs.dto.ts     # Search validation
│   ├── interfaces/
│   │   └── search.interface.ts     # Search types
│   └── search.module.ts            # Search module
│
├── liked-songs/               # Playlist functionality
│   ├── controllers/
│   │   └── liked-songs.controller.ts # @Controller('liked')
│   ├── services/
│   │   └── liked-songs.service.ts   # Playlist business logic
│   ├── dto/
│   │   ├── like-song.dto.ts         # Like validation
│   │   ├── remove-liked-song.dto.ts # Remove validation
│   │   └── get-liked-songs.dto.ts   # Get validation
│   ├── interfaces/
│   │   └── liked-songs.interface.ts # Playlist types
│   └── liked-songs.module.ts        # Liked songs module
│
├── users/                     # User management
│   ├── controllers/
│   │   └── users.controller.ts      # @Controller('users')
│   ├── services/
│   │   └── users.service.ts         # User business logic
│   ├── interfaces/
│   │   └── users.interface.ts       # User types
│   └── users.module.ts              # Users module
│
├── prisma/                    # Database service
├── config/                    # Configuration
└── app.module.ts              # Main app module
```

## 🔍 Features

### Search Functionality

- **Multi-field search**: Searches across song title, artist, and album
- **Case-insensitive**: Search is not case-sensitive
- **Pagination**: Offset-based pagination with metadata
- **Ordering**: Results ordered by play count (popularity)

### Liked Songs Management

- **Duplicate prevention**: Prevents adding the same song twice
- **Cursor-based pagination**: Efficient pagination for large playlists
- **Cascade deletion**: Proper cleanup when songs or users are deleted

### Validation & Error Handling

- **Input validation**: Comprehensive validation using class-validator
- **Type safety**: Full TypeScript support with Prisma
- **Error responses**: Meaningful error messages and proper HTTP status codes
- **Data transformation**: Automatic type conversion for query parameters

## 🚀 Usage Examples

### Add a song to liked playlist

```bash
curl -X POST http://localhost:3000/liked \
  -H "Content-Type: application/json" \
  -d '{
    "songId": "song_123",
    "userId": "user_456"
  }'
```

### Search for songs

```bash
curl "http://localhost:3000/search?q=rock&limit=5"
```

### Get liked songs with pagination

```bash
curl "http://localhost:3000/liked?userId=user_456&limit=10&cursor=2024-01-01T00:00:00.000Z"
```

### Remove a song from liked playlist

```bash
curl -X DELETE "http://localhost:3000/liked?id=song_123&userId=user_456"
```

## 🛠️ Development

### Prerequisites

- Node.js 18+
- PostgreSQL 12+
- Prisma ORM

### Setup

1. Install dependencies: `npm install`
2. Set up database: `npm run prisma:migrate`
3. Seed database: `npm run prisma:seed`
4. Start development server: `npm run start:dev`

### Testing

```bash
# Run tests
npm run test

# Run with coverage
npm run test:cov

# Run e2e tests
npm run test:e2e
```

## 📝 Best Practices Implemented

1. **Clean Architecture**: Separation of concerns with controllers, services, and DTOs
2. **Type Safety**: Full TypeScript support with Prisma-generated types
3. **Validation**: Input validation using class-validator decorators
4. **Error Handling**: Comprehensive error handling with proper HTTP status codes
5. **Pagination**: Efficient pagination strategies (offset and cursor-based)
6. **Database Optimization**: Proper indexing and query optimization
7. **Code Organization**: Modular structure following NestJS conventions
8. **Documentation**: Clear API documentation with examples
