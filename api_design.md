# Food Delivery API Design Documentation

This document outlines the API design for a food delivery application, focusing on two key features: **Cart Management** and **Place Order**. The API follows RESTful principles, uses JSON for data exchange, and assumes authentication via JWT tokens.

## Base URL

```
https://example.com/api/v1
```

## Authentication

- All endpoints require a valid JWT token in the `Authorization` header.
- Format: `Bearer <jwt_token>`

## 1. Cart Management

The Cart Management feature allows users to add, update, and remove items in their cart, as well as view the cart's contents.

### 1.1 Add Item to Cart

Add a food item to the user's cart.

- **Endpoint**: `POST /cart/items`
- **Description**: Adds a specified quantity of a menu item to the user's cart.
- **Request Body**:
  ```json
  {
    "cartId": "string" // Optional
    "menuItemId": "string",
    "quantity": "integer",
    "notes": "string" // Optional
  }
  ```
- **Response**:
  - **201 Created**:
    ```json
    {
      "cartId": "string",
      "itemId": "string",
      "menuItemId": "string",
      "quantity": "integer",
      "notes": "string",
      "subtotal": "number"
    }
    ```
  - **400 Bad Request**: Invalid input (e.g., negative quantity).
  - **404 Not Found**: Menu item not found.

### 1.2 Update Item in Cart

Update the quantity or notes of an item in the cart.

- **Endpoint**: `PUT /cart/items/{itemId}`
- **Description**: Updates the specified cart item's quantity or notes.
- **Path Parameters**:
  - `itemId`: The ID of the cart item to update.
- **Request Body**:
  ```json
  {
    "quantity": "integer",
    "notes": "string" // Optional
  }
  ```
- **Response**:
  - **200 OK**:
    ```json
    {
      "cartId": "string",
      "itemId": "string",
      "menuItemId": "string",
      "quantity": "integer",
      "notes": "string",
      "subtotal": "number"
    }
    ```
  - **400 Bad Request**: Invalid input.
  - **404 Not Found**: Cart item not found.

### 1.3 Remove Item from Cart

Remove an item from the user's cart.

- **Endpoint**: `DELETE /cart/items/{itemId}`
- **Description**: Removes the specified item from the cart.
- **Path Parameters**:
  - `itemId`: The ID of the cart item to remove.
- **Response**:
  - **204 No Content**: Item successfully removed.
  - **404 Not Found**: Cart item not found.

### 1.4 View Cart

Retrieve the contents of the user's cart.

- **Endpoint**: `GET /cart`
- **Description**: Returns the list of items in the user's cart along with the total price.
- **Response**:
  - **200 OK**:
    ```json
    {
      "cartId": "string",
      "items": [
        {
          "itemId": "string",
          "menuItemId": "string",
          "name": "string",
          "quantity": "integer",
          "price": "number",
          "notes": "string"
        }
      ],
      "total": "number"
    }
    ```
  - **404 Not Found**: Cart is empty or does not exist.

## 2. Place Order

The Place Order feature allows users to submit their cart as an order for delivery.

### 2.1 Create Order

Place an order based on the user's cart.

- **Endpoint**: `POST /orders`
- **Description**: Converts the cart into an order, clears the cart, and initiates the delivery process.
- **Request Body**:
  ```json
  {
    "cartId": "string",
    "deliveryAddress": {
      "street": "string",
      "city": "string",
      "state": "string",
      "zipCode": "string",
      "country": "string"
    },
    "paymentMethod": "string" // e.g., "credit_card", "paypal"
  }
  ```
- **Response**:
  - **201 Created**:
    ```json
    {
      "orderId": "string",
      "cartId": "string",
      "items": [
        {
          "menuItemId": "string",
          "name": "string",
          "quantity": "integer",
          "price": "number",
          "notes": "string"
        }
      ],
      "total": "number",
      "deliveryAddress": {
        "street": "string",
        "city": "string",
        "state": "string",
        "zipCode": "string",
        "country": "string"
      },
      "paymentMethod": "string",
      "status": "string", // e.g., "pending", "confirmed"
      "createdAt": "string" // ISO 8601 format
    }
    ```
  - **400 Bad Request**: Invalid address or payment method.
  - **404 Not Found**: Cart is empty or does not exist.

### 2.2 Get Order Details

Retrieve details of a specific order.

- **Endpoint**: `GET /orders/{orderId}`
- **Description**: Returns the details of the specified order.
- **Path Parameters**:
  - `orderId`: The ID of the order to retrieve.
- **Response**:
  - **200 OK**:
    ```json
    {
      "orderId": "string",
      "cartId": "string",
      "items": [
        {
          "menuItemId": "string",
          "name": "string",
          "quantity": "integer",
          "price": "number",
          "notes": "string"
        }
      ],
      "total": "number",
      "deliveryAddress": {
        "street": "string",
        "city": "string",
        "state": "string",
        "zipCode": "string",
        "country": "string"
      },
      "paymentMethod": "string",
      "status": "string",
      "createdAt": "string" // ISO 8601 format
    }
    ```
  - **404 Not Found**: Order not found.

## Error Handling

All endpoints return standardized error responses in the following format:

```json
{
  "error": {
    "code": "string",
    "message": "string"
  }
}
```

- Common HTTP status codes:
  - `400`: Bad Request (invalid input).
  - `401`: Unauthorized (invalid or missing token).
  - `404`: Not Found (resource not found).
  - `500`: Internal Server Error.

## Notes

- All monetary values (`price`, `total`, `subtotal`) are in EGP and include two decimal places (e.g., `10.99`).
- Timestamps (`createdAt`, `estimatedDeliveryTime`) follow ISO 8601 format (e.g., `2025-04-20T12:00:00Z`).
- The API assumes a database schema with tables for users, carts, cart items, menu items, and orders.
