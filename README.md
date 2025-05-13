# Jokes API Documentation

A RESTful API for managing a collection of jokes with support for CRUD operations (Create, Read, Update, Delete).

## Table of Contents
- [Overview](#overview)
- [Setup & Installation](#setup--installation)
- [API Endpoints](#api-endpoints)
  - [GET](#get-endpoints)
  - [POST](#post-endpoints)
  - [PUT](#put-endpoints)
  - [PATCH](#patch-endpoints)
  - [DELETE](#delete-endpoints)
- [Data Model](#data-model)
- [Error Handling](#error-handling)
- [Examples](#examples)

## Overview

This API provides endpoints to interact with a jokes database. It allows users to:
- Retrieve jokes (random, filtered by type, or by specific ID)
- Create new jokes
- Edit existing jokes
- Delete jokes

The API is built with Express.js and uses in-memory storage for simplicity.

## Setup & Installation

### Prerequisites
- Node.js (v14 or higher recommended)
- npm (Node Package Manager)

### Installation Steps

1. Clone the repository or download the source code
2. Install dependencies:
   ```bash
   npm install
   ```
3. Start the server:
   ```bash
   node app.js
   ```
   
The server will run on port 8000 by default.

## API Endpoints

### GET Endpoints

#### Get a Random Joke
```
GET /jokes/random
```
Returns a randomly selected joke from the collection.

**Response Format:**
```json
{
  "id": 1,
  "jokeText": "Why don't scientists trust atoms? Because they make up everything.",
  "jokeType": "Science"
}
```

#### Filter Jokes by Type
```
GET /jokes/filter?type=Science
```
Returns jokes that match the specified type (case-insensitive).

**Parameters:**
- `type` (query string): Type of joke to filter by (e.g., Science, Puns, Math)

**Response Format:**
```json
[
  {
    "id": 1,
    "jokeText": "Why don't scientists trust atoms? Because they make up everything.",
    "jokeType": "Science"
  },
  {
    "id": 6,
    "jokeText": "How do you organize a space party? You planet!",
    "jokeType": "Science"
  }
]
```

**Error Response:** Status 404 if no jokes of that type are found
```json
{
  "message": "No jokes found with that type"
}
```

#### Get Joke by ID
```
GET /jokes/:id
```
Returns a specific joke by its ID.

**Parameters:**
- `id` (URL parameter): The unique ID of the joke

**Response Format:**
```json
{
  "id": 1,
  "jokeText": "Why don't scientists trust atoms? Because they make up everything.",
  "jokeType": "Science"
}
```

**Error Response:** Status 404 if joke with specified ID is not found
```json
{
  "message": "Joke with id:123 not found. Try creating a new one!"
}
```

### POST Endpoints

#### Create a New Joke
```
POST /jokes/new
```
Adds a new joke to the collection.

**Request Body:**
```json
{
  "type": "Technology",
  "text": "Why did the developer go broke? Because he used up all his cache!"
}
```

**Response Format:**
```json
{
  "id": 101,
  "jokeText": "Why did the developer go broke? Because he used up all his cache!",
  "jokeType": "Technology"
}
```

### PUT Endpoints

#### Update a Joke (Complete Replacement)
```
PUT /jokes/edit/:id
```
Completely replaces a joke with the provided data.

**Parameters:**
- `id` (URL parameter): The unique ID of the joke to update

**Request Body:**
```json
{
  "type": "Technology",
  "text": "Why did the developer go broke? Because he used up all his cache!"
}
```

**Response Format:**
```json
{
  "id": 1,
  "jokeText": "Why did the developer go broke? Because he used up all his cache!",
  "jokeType": "Technology"
}
```

**Error Response:** Status 404 if joke with specified ID is not found
```json
{
  "message": "Joke with id:123 not found. Try creating a new one!"
}
```

### PATCH Endpoints

#### Update a Joke (Partial Update)
```
PATCH /jokes/edit/:id
```
Updates only the specified fields of a joke.

**Parameters:**
- `id` (URL parameter): The unique ID of the joke to update

**Request Body:**
```json
{
  "text": "Why did the developer go broke? Because he used up all his cache!"
}
```

**Response Format:**
```json
{
  "id": 1,
  "jokeText": "Why did the developer go broke? Because he used up all his cache!",
  "jokeType": "Science"
}
```

**Error Response:** Status 404 if joke with specified ID is not found
```json
{
  "message": "Joke with id:123 not found. Try creating a new one!"
}
```

### DELETE Endpoints

#### Delete a Joke
```
DELETE /jokes/delete/:id
```
Removes a joke from the collection.

**Parameters:**
- `id` (URL parameter): The unique ID of the joke to delete

**Response Format:**
```json
{
  "message": "Joke deleted successfully"
}
```

**Error Response:** Status 404 if joke with specified ID is not found
```json
{
  "message": "Joke with id:123 not found. Try creating a new one!"
}
```

## Data Model

Each joke in the database has the following structure:

```json
{
  "id": 1,              // Unique identifier
  "jokeText": "string", // The joke content
  "jokeType": "string"  // Category of the joke (e.g., Science, Puns, Math)
}
```

## Error Handling

The API returns appropriate HTTP status codes:
- `200 OK` - Request succeeded
- `404 Not Found` - Resource not found
- `500 Internal Server Error` - Server error

Error responses include a descriptive message in the response body.

## Examples

### Example: Getting a random joke

**Request:**
```bash
curl http://localhost:8000/jokes/random
```

**Response:**
```json
{
  "id": 15,
  "jokeText": "I'm reading a book about anti-gravity. It's impossible to put down!",
  "jokeType": "Science"
}
```

### Example: Creating a new joke

**Request:**
```bash
curl -X POST http://localhost:8000/jokes/new \
  -H "Content-Type: application/json" \
  -d '{"type":"Programming","text":"Why do programmers prefer dark mode? Because light attracts bugs!"}'
```

**Response:**
```json
{
  "id": 101,
  "jokeText": "Why do programmers prefer dark mode? Because light attracts bugs!",
  "jokeType": "Programming"
}
```

### Example: Updating a joke

**Request:**
```bash
curl -X PATCH http://localhost:8000/jokes/edit/1 \
  -H "Content-Type: application/json" \
  -d '{"text":"Updated joke text"}'
```

**Response:**
```json
{
  "id": 1,
  "jokeText": "Updated joke text",
  "jokeType": "Science"
}
```
