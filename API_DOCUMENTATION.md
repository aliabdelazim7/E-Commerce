# Simple Ecommerce API Documentation

## Base URL

```
http://localhost:5000/api
```

## Authentication

All protected routes require a Bearer token in the Authorization header:

```
Authorization: Bearer <your-jwt-token>
```

## Endpoints

### Authentication Routes (`/api/auth`)

#### Register User

- **POST** `/api/auth/register`
- **Body:**
  ```json
  {
    "name": "John Doe",
    "email": "john@example.com",
    "password": "SecurePass123"
  }
  ```
- **Response:**
  ```json
  {
    "message": "User created successfully",
    "token": "jwt-token-here",
    "user": {
      "id": 1,
      "email": "john@example.com",
      "name": "John Doe"
    }
  }
  ```

#### Login User

- **POST** `/api/auth/login`
- **Body:**
  ```json
  {
    "email": "john@example.com",
    "password": "SecurePass123"
  }
  ```
- **Response:**
  ```json
  {
    "message": "Login successful",
    "token": "jwt-token-here",
    "user": {
      "id": 1,
      "email": "john@example.com",
      "name": "John Doe"
    }
  }
  ```

#### Get Current User

- **GET** `/api/auth/me`
- **Headers:** `Authorization: Bearer <token>`
- **Response:**
  ```json
  {
    "user": {
      "userId": 1,
      "email": "john@example.com",
      "name": "John Doe"
    }
  }
  ```

### Product Routes (`/api/products`)

#### Get All Products

- **GET** `/api/products`
- **Query Parameters:**
  - `category` (optional): Filter by category
  - `search` (optional): Search in name and description
- **Response:**
  ```json
  [
    {
      "id": 1,
      "name": "Wireless Headphones",
      "description": "High-quality wireless headphones...",
      "price": 99.99,
      "image_url": "https://images.unsplash.com/...",
      "category": "Electronics",
      "stock": 50,
      "created_at": "2023-01-01T00:00:00.000Z"
    }
  ]
  ```

#### Get Single Product

- **GET** `/api/products/:id`
- **Response:**
  ```json
  {
    "id": 1,
    "name": "Wireless Headphones",
    "description": "High-quality wireless headphones...",
    "price": 99.99,
    "image_url": "https://images.unsplash.com/...",
    "category": "Electronics",
    "stock": 50,
    "created_at": "2023-01-01T00:00:00.000Z"
  }
  ```

#### Get Categories

- **GET** `/api/products/categories/list`
- **Response:**
  ```json
  ["Electronics", "Clothing", "Sports", "Home", "Books", "Beauty", "Toys"]
  ```

#### Create Product (Admin)

- **POST** `/api/products`
- **Headers:** `Authorization: Bearer <token>`
- **Body:**
  ```json
  {
    "name": "New Product",
    "description": "Product description",
    "price": 29.99,
    "image_url": "https://example.com/image.jpg",
    "category": "Electronics",
    "stock": 100
  }
  ```

#### Update Product (Admin)

- **PUT** `/api/products/:id`
- **Headers:** `Authorization: Bearer <token>`
- **Body:** Same as create product

#### Delete Product (Admin)

- **DELETE** `/api/products/:id`
- **Headers:** `Authorization: Bearer <token>`

### Order Routes (`/api/orders`)

#### Create Order

- **POST** `/api/orders`
- **Headers:** `Authorization: Bearer <token>`
- **Body:**
  ```json
  {
    "items": [
      {
        "productId": 1,
        "quantity": 2,
        "price": 99.99
      }
    ],
    "totalAmount": 199.98
  }
  ```

#### Get User Orders

- **GET** `/api/orders/my-orders`
- **Headers:** `Authorization: Bearer <token>`
- **Response:**
  ```json
  [
    {
      "id": 1,
      "total_amount": 199.98,
      "status": "pending",
      "created_at": "2023-01-01T00:00:00.000Z",
      "items": [
        {
          "product_id": 1,
          "product_name": "Wireless Headphones",
          "quantity": 2,
          "price": 99.99,
          "image_url": "https://images.unsplash.com/..."
        }
      ]
    }
  ]
  ```

#### Get Order Details

- **GET** `/api/orders/:id`
- **Headers:** `Authorization: Bearer <token>`

#### Update Order Status (Admin)

- **PUT** `/api/orders/:id/status`
- **Headers:** `Authorization: Bearer <token>`
- **Body:**
  ```json
  {
    "status": "shipped"
  }
  ```
- **Valid Statuses:** `pending`, `processing`, `shipped`, `delivered`, `cancelled`

## Error Responses

### Validation Error (400)

```json
{
  "message": "Validation failed",
  "errors": [
    {
      "msg": "Email is required",
      "param": "email",
      "location": "body"
    }
  ]
}
```

### Authentication Error (401)

```json
{
  "message": "Invalid token"
}
```

### Not Found Error (404)

```json
{
  "message": "Product not found"
}
```

### Server Error (500)

```json
{
  "success": false,
  "error": "Server Error"
}
```

## Rate Limiting

- **Limit:** 100 requests per 15 minutes per IP
- **Scope:** All `/api/` routes

## Security Features

- Helmet.js for security headers
- CORS configuration
- Rate limiting
- Input validation
- Password hashing with bcrypt
- JWT token authentication
- SQL injection protection
- XSS protection

## Environment Variables

Create a `.env` file in the root directory:

```env
NODE_ENV=development
PORT=5000
JWT_SECRET=your-super-secret-jwt-key-change-this-in-production
DB_PATH=./server/ecommerce.db
CORS_ORIGIN=http://localhost:3000
BCRYPT_ROUNDS=12
JWT_EXPIRES_IN=24h
MAX_FILE_SIZE=5242880
UPLOAD_PATH=./uploads
```
