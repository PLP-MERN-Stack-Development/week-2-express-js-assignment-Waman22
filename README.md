Product API Server

This is a simple Express.js server for managing a product inventory API. It provides endpoints to create, read, update, and delete products, with basic authentication and request logging.

Prerequisites





Node.js (version 14 or higher)



npm (usually included with Node.js)

Installation





Clone or download the repository to your local machine.



Navigate to the project directory:

cd path/to/project



Install the required dependencies:

npm install express body-parser uuid

Running the Server





Start the server by running:

node server.js



The server will start on http://localhost:3000 (or the port specified in the PORT environment variable).



You can access the root endpoint at http://localhost:3000/ to verify the server is running.

API Endpoints

All API endpoints are prefixed with /api/products. The POST, PUT, and DELETE endpoints require an Authorization header with the value Bearer secret-token for authentication.

1. GET /





Description: Root endpoint to verify the server is running.



Response:





Status: 200 OK



Body: Plain text message welcoming users to the API.

Example Request:

curl http://localhost:3000/

Example Response:

Welcome to the Product API! Go to /api/products to see all products.

2. GET /api/products





Description: Retrieve a list of all products.



Response:





Status: 200 OK



Body: JSON array of all products.

Example Request:

curl http://localhost:3000/api/products

Example Response:

[
  {
    "id": "1",
    "name": "Laptop",
    "description": "High-performance laptop with 16GB RAM",
    "price": 1200,
    "category": "electronics",
    "inStock": true
  },
  {
    "id": "2",
    "name": "Smartphone",
    "description": "Latest model with 128GB storage",
    "price": 800,
    "category": "electronics",
    "inStock": true
  }
]

3. GET /api/products/:id





Description: Retrieve a specific product by its ID.



Parameters:





id: The unique ID of the product.



Response:





Status: 200 OK (if found) or 404 Not Found



Body: JSON object of the product or error message.

Example Request:

curl http://localhost:3000/api/products/1

Example Response (Success):

{
  "id": "1",
  "name": "Laptop",
  "description": "High-performance laptop with 16GB RAM",
  "price": 1200,
  "category": "electronics",
  "inStock": true
}

Example Response (Error):

{ "error": "Product not found" }

4. POST /api/products





Description: Create a new product (requires authentication).



Headers:





Authorization: Bearer secret-token



Content-Type: application/json



Body: JSON object with name, description, price, category, and optional inStock (boolean).



Response:





Status: 201 Created (success) or 400 Bad Request (missing fields) or 401 Unauthorized



Body: Created product or error message.

Example Request:

curl -H "Authorization: Bearer secret-token" -H "Content-Type: application/json" \
-X POST -d '{"name":"Tablet","description":"10-inch tablet","price":300,"category":"electronics","inStock":true}' \
http://localhost:3000/api/products

Example Response (Success):

{
  "id": "4a7d1ed4-3c2b-4f3c-9c1a-5b6e7f8d9e0a",
  "name": "Tablet",
  "description": "10-inch tablet",
  "price": 300,
  "category": "electronics",
  "inStock": true
}

Example Response (Error - Missing Fields):

{ "error": "Missing required fields" }

5. PUT /api/products/:id





Description: Update an existing product (requires authentication).



Headers:





Authorization: Bearer secret-token



Content-Type: application/json



Parameters:





id: The unique ID of the product.



Body: JSON object with name, description, price, category, and optional inStock (boolean).



Response:





Status: 200 OK (success) or 400 Bad Request (missing fields) or 401 Unauthorized or 404 Not Found



Body: Updated product or error message.

Example Request:

curl -H "Authorization: Bearer secret-token" -H "Content-Type: application/json" \
-X PUT -d '{"name":"Updated Laptop","description":"Upgraded laptop with 32GB RAM","price":1500,"category":"electronics","inStock":true}' \
http://localhost:3000/api/products/1

Example Response (Success):

{
  "id": "1",
  "name": "Updated Laptop",
  "description": "Upgraded laptop with 32GB RAM",
  "price": 1500,
  "category": "electronics",
  "inStock": true
}

Example Response (Error - Not Found):

{ "error": "Product not found" }

6. DELETE /api/products/:id





Description: Delete a product by its ID (requires authentication).



Headers:





Authorization: Bearer secret-token



Parameters:





id: The unique ID of the product.



Response:





Status: 204 No Content (success) or 401 Unauthorized or 404 Not Found



Body: Empty (success) or error message.

Example Request:

curl -H "Authorization: Bearer secret-token" -X DELETE http://localhost:3000/api/products/1

Example Response (Success):

(Status 204, no body)

Example Response (Error - Not Found):

{ "error": "Product not found" }

Error Handling





The server logs all requests with timestamps.



Invalid requests return appropriate HTTP status codes (400, 401, 404).



Unexpected errors return a 500 Internal Server Error with a generic error message.

Notes





The server uses an in-memory database, so data is not persisted between restarts.



Authentication is implemented with a simple token (Bearer secret-token) for demonstration purposes. In a production environment, use proper JWT or OAuth.



The server can be tested using tools like curl, Postman, or any HTTP client.
