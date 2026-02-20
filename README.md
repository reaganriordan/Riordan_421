# Items API

A RESTful CRUD API for managing items, built with **Node.js**, **Express**, and **MongoDB**. Includes interactive Swagger UI documentation and an asynchronous processing endpoint.

## Prerequisites

- Node.js
- MongoDB running locally on `mongodb://localhost/mydatabase`

## Installation

```bash
npm install
node index.js
```

The server starts on **port 3000** by default.

UI is available at: **http://localhost:3000/api-docs**

## Endpoints

### 1. Create an Item
**POST** `/items`

Creates a new item in the database.

**Request Body:**
```json
{
  "name": "Laptop",
  "description": "A high-performance laptop"
}
```

**Responses:**
| Status | Description |
|--------|-------------|
| `201 Created` | Item created successfully, returns the new item object |
| `400 Bad Request` | Validation failed (e.g. missing required `name` field) |

**Example Response (201):**
```json
{
  "_id": "664a1f2e8b3c1a2b3c4d5e6f",
  "name": "Laptop",
  "description": "A high-performance laptop",
  "__v": 0
}
```

### 2. Get All Items
**GET** `/items`

Returns an array of all items.

**Responses:**
| Status | Description |
|--------|-------------|
| `200 OK` | Array of item objects |
| `500 Internal Server Error` | Database error |

**Example Response (200):**
```json
[
  {
    "_id": "664a1f2e8b3c1a2b3c4d5e6f",
    "name": "Laptop",
    "description": "A high-performance laptop",
    "__v": 0
  }
]
```

### 3. Get a Single Item
**GET** `/items/:id`

Returns one item by its MongoDB ObjectId.

**Path Parameter:** `id` — MongoDB ObjectId string

**Responses:**
| Status | Description |
|--------|-------------|
| `200 OK` | The item object |
| `404 Not Found` | No item with the given ID |
| `500 Internal Server Error` | Database error |

### 4. Update an Item
**PATCH** `/items/:id`

Partially updates an item by ID.

**Path Parameter:** `id` — MongoDB ObjectId string

**Request Body:**
```json
{
  "name": "Updated Laptop",
  "description": "Now with more RAM"
}
```

**Responses:**
| Status | Description |
|--------|-------------|
| `200 OK` | Updated item object |
| `400 Bad Request` | Invalid ID or validation error |

### 5. Delete an Item
**DELETE** `/items/:id`

Deletes an item by ID.

**Path Parameter:** `id` — MongoDB ObjectId string

**Responses:**
| Status | Description |
|--------|-------------|
| `200 OK` | `{ "message": "Item deleted" }` |
| `400 Bad Request` | Invalid ID |

### 6. Process an Item
**POST** `/items/process`

Creates a new item with a simulated 2-second async processing delay before responding

**Request Body:**
```json
{
  "name": "Processed Item",
  "description": "Created after a 2-second async delay"
}
```

**Responses:**
| Status | Description |
|--------|-------------|
| `201 Created` | Item processed and created |
| `400 Bad Request` | Validation error |

**Example Response (201):**
```json
{
  "message": "Item processed successfully after delay",
  "item": {
    "_id": "664a1f2e8b3c1a2b3c4d5e6f",
    "name": "Processed Item",
    "description": "Created after a 2-second async delay",
    "__v": 0
  }
}
```

## Data Model

| Field | Type | Required | Description |
|-------|------|----------|-------------|
| `_id` | ObjectId | Auto | MongoDB auto-generated ID |
| `name` | String | Yes | Name of the item |
| `description` | String | No | Optional description |

## API Documentation

Swagger UI is generated from JSDoc comments in `routes/items.js` using **swagger-jsdoc** and **swagger-ui-express**.

Visit **http://localhost:3000/api-docs** after starting the server.

## Testing

Import `postman_collection.json` into Postman to run the full test suite. Tests validate that:
- `POST /items` returns **201 Created**
- `GET /items` returns **200 OK**
- `PATCH /items/:id` returns **200 OK**
- `DELETE /items/:id` returns **200 OK**
- `POST /items/process` returns **201 Created**
