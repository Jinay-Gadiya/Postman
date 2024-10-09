# Simple Grocery Store API

This API allows you to place a grocery order which will be ready for pick-up in the store.

The API is available at `https://simple-grocery-store-api.glitch.me`

Alternative URL: `http://simple-grocery-store-api.online/` (HTTP only!)

Link to official documentation is [here](https://github.com/vdespa/Postman-Complete-Guide-API-Testing/blob/main/simple-grocery-store-api.md)

# 📁 Collection: Happy Path

## End-point: API status

This endpoint makes an HTTP GET request to retrieve the status of the simple grocery store API. The response will be in JSON format with a status field indicating the current status of the API.

Example response:

```json
{
  "status": "UP"
}
```

Status `UP` indicates that the API is running as expected.

No response or any other response indicates that the API is not functioning correctly.

### Method: GET

> ```
> {{baseUrl}}/status
> ```

## End-point: Get all products

Returns a list of products from the inventory.

#### Parameters

- `category` (string, query, optional): Specifies the category of products you want to be returned. It can be one of: meat-seafood, fresh-produce, candy, bread-bakery, dairy, eggs, coffee.
- `results` (integer, query, optional): Specifies the number of results you want. Must be a number between 1 and 20. By default, only 20 products will be displayed.
- `available` (boolean, query, optional): Specifies the availability of the products. By default, all products will be displayed.

#### Response

The response will be a JSON array containing objects with the following properties:

- `id` (number): The unique identifier of the product.
- `category` (string): The category of the product.
- `name` (string): The name of the product.
- `inStock` (boolean): Indicates the availability of the product.

#### Status codes

- 200 OK: Indicates a successful response.
- 400 Bad Request: Indicates that the parameters provided are invalid.

### Method: GET

> ```
> {{baseUrl}}/products?results=2&category=coffee
> ```

### Query Params

| Param    | value  |
| -------- | ------ |
| results  | 2      |
| category | coffee |

## End-point: Get a single product

Returns a single product from the inventory.

#### Parameters

- productId (integer, required) - Specifies the product's id you wish to retrieve.
- product-label (boolean, optional) - Returns the product label in PDF format.

#### Status codes

- 200 OK: Indicates a successful response.
- 404 Not found: Indicates that there is no product with the specified id.

### Method: GET

> ```
> {{baseUrl}}/products/:productId
> ```

## End-point: Get cart

This endpoint retrieves a specific cart based on the provided cartId.

## Parameters

- `cartId` (required, path, string): Specifies the id of the cart you wish to retrieve.

## Response

- Status: 200 OK
- Content-Type: application/json
- Body:
  ```json
  {
    "items": [],
    "created": ""
  }
  ```

### Status Codes

- 200 OK: Indicates a successful response.
- 404 Not found: Indicates that there is no cart with the specified id.

### Method: GET

> ```
> {{baseUrl}}/carts/:cartId
> ```

## End-point: Get cart items

Returns the items in a cart.

### Parameters

- cartId (required, string, path) - Specifies the id of the cart for which you wish to retrieve the items.

### Response

The API returns a status code of 200 if the request is successful, and an empty array in the response body.

- 200 OK - Indicates a successful response.
- 404 Not found - Indicates that there is no cart with the specified id.

### Method: GET

> ```
> {{baseUrl}}/carts/:cartId/items
> ```

## End-point: Create a new cart

To create a new cart, submit an empty POST request to the /carts endpoint.

### Request

`POST /carts`

### Response

- Status: 201
- Content-Type: application/json

Example response body:

```json
{
  "created": true,
  "cartId": "bx0-ycNjqIm5IvufuuZ09"
}
```

Indicates that the cart has been created successfully.

No parameters are accepted for this request.

### Method: POST

> ```
> {{baseUrl}}/carts
> ```

## End-point: Add item to cart

Allows the addition of items to an existing cart. Only one item can be added at a time.

- Method: POST
- URL: `{{baseUrl}}/carts/:cartId/items`

### Request Body

The request body needs to be in JSON format.

| Name      | Type    | In   | Required | Description                                         |
| --------- | ------- | ---- | -------- | --------------------------------------------------- |
| cartId    | string  | path | Yes      | Specifies the cart id                               |
| productId | integer | body | Yes      | Specifies the product id                            |
| quantity  | integer | body | No       | If no quantity is provided, the default value is 1. |

Example request body:

```json
{
  "productId": 1234
}
```

### Status codes

- 201 Created: Indicates that the item has been added successfully.
- 400 Bad Request: Indicates that the parameters provided are invalid.

### Method: POST

> ```
> {{baseUrl}}/carts/:cartId/items
> ```

### Body (**raw**)

```json
{
  "productId": 8739,
  "quantity": 5
}
```

## End-point: Update quantity

This API endpoint allows modifying information about an item in the cart by sending an HTTP PATCH request to `/carts/:cartId/items/:itemId`. The request body should be in JSON format and must include the `quantity` parameter to specify the new quantity of the item.

### Parameters

- `cartId` (string, path, required): Specifies the cart id.
- `itemId` (string, path, required): Specifies the item id.
- `quantity` (integer, body, required): Quantity of the item.

### Status codes

- 204 No Content: Indicates that the item has been updated successfully.
- 400 Bad Request: Indicates that the parameters provided are invalid or missing.
- 404 Not found: The cart or the item could not be found.

### Example

```json
{
  "quantity": 5
}
```

### Method: PATCH

> ```
> {{baseUrl}}/carts/:cartId/items/:itemId
> ```

### Body (**raw**)

```json
{
  "quantity": 3
}
```

## End-point: Replace product in cart

This endpoint allows you to replace an item in the cart by sending an HTTP PUT request to `{{baseUrl}}/carts/:cartId/items/:itemId`.

#### Request Body

The request body should be in JSON format and include the following parameters:

- `productId` (integer, required): Specifies the product id.
- `quantity` (integer, optional): Specifies the quantity.

#### Parameters

- `cartId` (string, path, required): Specifies the cart id.
- `itemId` (string, path, required): Specifies the item id.

#### Status codes

- 204 No Content: Indicates that the item has been updated successfully.
- 400 Bad Request: Indicates that the parameters provided are invalid or missing.
- 404 Not found: The cart or the item could not be found.

#### Example

```json
{
  "productId": 123,
  "quantity": 2
}
```

### Method: PUT

> ```
> {{baseUrl}}/carts/:cartId/items/:itemId
> ```

### Body (**raw**)

```json
{
  "productId": 4643,
  "quantity": 2
}
```

## End-point: Delete product in cart

Deletes the specified item from the cart.

#### Parameters

- cartId (string) - Specifies the cart id. (Required)
- itemId (string) - Specifies the item id. (Required)

#### Status codes

- 204 No Content: Indicates that the item has been deleted successfully.
- 404 Not found: The cart or the item could not be found.

### Method: DELETE

> ```
> {{baseUrl}}/carts/:cartId/items/:itemId
> ```

### Body (**raw**)

```json

```

## End-point: Register API client

Registers a new API client.

#### HTTP Request

```json
POST {{baseUrl}}/api-clients

```

#### Parameters

The request body needs to be in JSON format.

| Name        | Type   | In   | Required | Description                          |
| ----------- | ------ | ---- | -------- | ------------------------------------ |
| clientName  | string | body | Yes      | The name of the API client.          |
| clientEmail | string | body | Yes      | The email address of the API client. |

\*The email address DOES NOT need to be real. The email will not be stored on the server.

#### Status codes

- 201 Created: Indicates that the client has been registered successfully.
- 400 Bad Request: Indicates that the parameters provided are invalid.
- 409 Conflict: Indicates that an API client has already been registered with this email address.

#### Example request body:

```json
{
  "clientName": "Postman on Valentin's computer",
  "clientEmail": "valentin@example.com"
}
```

### Method: POST

> ```
> {{baseUrl}}/api-clients
> ```

### Body (**raw**)

```json
{
  "clientName": "Andrew Friedman",
  "clientEmail": "andrewfriedman01@gmail.com"
}
```

## End-point: Get single order

Returns a single order.

#### Parameters

- Authorization (string, header, required): The bearer token of the API client.
- orderId (string, path, required): The order id.
- invoice (boolean, query, optional): Show the PDF invoice.

#### Status codes

- 200 OK: Indicates a successful response.
- 401 Unauthorized: Indicates that the request has not been authenticated. Check the response body for additional details.
- 404 Not found: Indicates that there is no order with the specified id associated with the API client.

### Method: GET

> ```
> {{baseUrl}}/orders/:orderId
> ```

### 🔑 Authentication bearer

| Param | value           | Type   |
| ----- | --------------- | ------ |
| token | {{accessToken}} | string |

## End-point: Get all orders

Returns all orders created by the API client.

#### Parameters

- Authorization: (string, header, required) - Specifies the bearer token of the API client.

#### Status codes

- 200 OK: Indicates a successful response.
- 401 Unauthorized: Indicates that the request has not been authenticated. Check the response body for additional details.

### Method: GET

> ```
> {{baseUrl}}/orders
> ```

### 🔑 Authentication bearer

| Param | value           | Type   |
| ----- | --------------- | ------ |
| token | {{accessToken}} | string |

## End-point: Create an order

The `POST /orders` endpoint is used to submit a new order. Upon successful submission, the associated cart is deleted.

#### Parameters

- Authorization: `string` (header, required) - The bearer token of the API client.
- cartId: `string` (body, required) - The cart id.
- customerName: `string` (body, required) - The name of the customer.
- comment: `string` (body, optional) - A comment associated with the order.

#### Example request body:

```json
{
  "cartId": "ZFe4yhG5qNhmuNyrbLWa4",
  "customerName": "John Doe"
}
```

#### Response

Upon successful submission, the response will have a status code of 201 and a JSON content with the following structure:

```json
{
  "created": true,
  "orderId": ""
}
```

### Method: POST

> ```
> {{baseUrl}}/orders
> ```

### Body (**raw**)

```json
{
  "cartId": "{{cartID}}",
  "customerName": "{{$randomFullName}}"
}
```

### 🔑 Authentication bearer

| Param | value           | Type   |
| ----- | --------------- | ------ |
| token | {{accessToken}} | string |

## End-point: Update order

Updates the order with the specified orderId.

The request body needs to be in JSON format.

### Parameters

- Authorization: string (header, required) - The bearer token of the API client.
- orderId: string (path, required) - The order id.
- customerName: string (body, optional) - The name of the customer.
- comment: string (body, optional) - A comment associated with the order.

### Status codes

- 204 No Content: Indicates that the order has been updated successfully.
- 400 Bad Request: Indicates that the parameters provided are invalid.
- 401 Unauthorized: Indicates that the request has not been authenticated. Check the response body for additional details.
- 404 Not found: Indicates that there is no order with the specified id associated with the API client.

### Example request body:

```
{
  "customerName": "Joe Doe"
}

```

### Method: PATCH

> ```
> {{baseUrl}}/orders/:orderId
> ```

### Body (**raw**)

```json
{
  "comment": "Pickup at 2pm."
}
```

### 🔑 Authentication bearer

| Param | value           | Type   |
| ----- | --------------- | ------ |
| token | {{accessToken}} | string |

## End-point: Delete order

Deletes the order with the specified orderId.

## Parameters

- Authorization (header) - The bearer token of the API client. (Required)
- orderId (path) - The order id. (Required)

## Status codes

- 204 No Content - Indicates that the order has been deleted successfully.
- 400 Bad Request - Indicates that the parameters provided are invalid.
- 401 Unauthorized - Indicates that the request has not been authenticated. Check the response body for additional details.
- 404 Not found - Indicates that there is no order with the specified id associated with the API client.

### Method: DELETE

> ```
> {{baseUrl}}/orders/:orderId
> ```

### Body (**raw**)

```json
{
  "comment": "Pickup at 2pm."
}
```

### 🔑 Authentication bearer

| Param | value           | Type   |
| ----- | --------------- | ------ |
| token | {{accessToken}} | string |

## End-point: Get single order (missing)

Returns a single order.

#### Parameters

- Authorization (string, header, required): The bearer token of the API client.
- orderId (string, path, required): The order id.
- invoice (boolean, query, optional): Show the PDF invoice.

#### Status codes

- 200 OK: Indicates a successful response.
- 401 Unauthorized: Indicates that the request has not been authenticated. Check the response body for additional details.
- 404 Not found: Indicates that there is no order with the specified id associated with the API client.

### Method: GET

> ```
> {{baseUrl}}/orders/:orderId
> ```

### 🔑 Authentication bearer

| Param | value           | Type   |
| ----- | --------------- | ------ |
| token | {{accessToken}} | string |

# 📁 Collection: Missing authentication

## End-point: Create an order

The `POST /orders` endpoint is used to submit a new order. Upon successful submission, the associated cart is deleted.

#### Parameters

- Authorization: `string` (header, required) - The bearer token of the API client.
- cartId: `string` (body, required) - The cart id.
- customerName: `string` (body, required) - The name of the customer.
- comment: `string` (body, optional) - A comment associated with the order.

#### Example request body:

```json
{
  "cartId": "ZFe4yhG5qNhmuNyrbLWa4",
  "customerName": "John Doe"
}
```

#### Response

Upon successful submission, the response will have a status code of 201 and a JSON content with the following structure:

```json
{
  "created": true,
  "orderId": ""
}
```

### Method: POST

> ```
> {{baseUrl}}/orders
> ```

### Body (**raw**)

```json
{
  "cartId": "{{cartID}}",
  "customerName": "{{$randomFullName}}"
}
```

### 🔑 Authentication noauth

| Param | value | Type |
| ----- | ----- | ---- |

## End-point: Get all orders

Returns all orders created by the API client.

#### Parameters

- Authorization: (string, header, required) - Specifies the bearer token of the API client.

#### Status codes

- 200 OK: Indicates a successful response.
- 401 Unauthorized: Indicates that the request has not been authenticated. Check the response body for additional details.

### Method: GET

> ```
> {{baseUrl}}/orders
> ```

### 🔑 Authentication noauth

| Param | value | Type |
| ----- | ----- | ---- |

## End-point: Create an order

The `POST /orders` endpoint is used to submit a new order. Upon successful submission, the associated cart is deleted.

#### Parameters

- Authorization: `string` (header, required) - The bearer token of the API client.
- cartId: `string` (body, required) - The cart id.
- customerName: `string` (body, required) - The name of the customer.
- comment: `string` (body, optional) - A comment associated with the order.

#### Example request body:

```json
{
  "cartId": "ZFe4yhG5qNhmuNyrbLWa4",
  "customerName": "John Doe"
}
```

#### Response

Upon successful submission, the response will have a status code of 201 and a JSON content with the following structure:

```json
{
  "created": true,
  "orderId": ""
}
```

### Method: POST

> ```
> {{baseUrl}}/orders
> ```

### Body (**raw**)

```json
{
  "cartId": "{{cartID}}",
  "customerName": "{{$randomFullName}}"
}
```

### 🔑 Authentication bearer

| Param | value                                                            | Type   |
| ----- | ---------------------------------------------------------------- | ------ |
| token | c8a75070946c31d09824d1dee664486c0ae72ee4586695e52009fce83fca4d9g | string |

## End-point: Get all orders

Returns all orders created by the API client.

#### Parameters

- Authorization: (string, header, required) - Specifies the bearer token of the API client.

#### Status codes

- 200 OK: Indicates a successful response.
- 401 Unauthorized: Indicates that the request has not been authenticated. Check the response body for additional details.

### Method: GET

> ```
> {{baseUrl}}/orders
> ```

### 🔑 Authentication bearer

| Param | value                                                            | Type   |
| ----- | ---------------------------------------------------------------- | ------ |
| token | c8a75070946c31d09824d1dee664486c0ae72ee4586695e52009fce83fca4d9g | string |

# 📁 Collection: Invalid inputs

## End-point: Get all products - invalid category

Returns a list of products from the inventory.

#### Parameters

- `category` (string, query, optional): Specifies the category of products you want to be returned. It can be one of: meat-seafood, fresh-produce, candy, bread-bakery, dairy, eggs, coffee.
- `results` (integer, query, optional): Specifies the number of results you want. Must be a number between 1 and 20. By default, only 20 products will be displayed.
- `available` (boolean, query, optional): Specifies the availability of the products. By default, all products will be displayed.

#### Response

The response will be a JSON array containing objects with the following properties:

- `id` (number): The unique identifier of the product.
- `category` (string): The category of the product.
- `name` (string): The name of the product.
- `inStock` (boolean): Indicates the availability of the product.

#### Status codes

- 200 OK: Indicates a successful response.
- 400 Bad Request: Indicates that the parameters provided are invalid.

### Method: GET

> ```
> {{baseUrl}}/products?results=2&category=invalid-token
> ```

### Query Params

| Param    | value         |
| -------- | ------------- |
| results  | 2             |
| category | invalid-token |

## End-point: Get all products - Results > maximum

Returns a list of products from the inventory.

#### Parameters

- `category` (string, query, optional): Specifies the category of products you want to be returned. It can be one of: meat-seafood, fresh-produce, candy, bread-bakery, dairy, eggs, coffee.
- `results` (integer, query, optional): Specifies the number of results you want. Must be a number between 1 and 20. By default, only 20 products will be displayed.
- `available` (boolean, query, optional): Specifies the availability of the products. By default, all products will be displayed.

#### Response

The response will be a JSON array containing objects with the following properties:

- `id` (number): The unique identifier of the product.
- `category` (string): The category of the product.
- `name` (string): The name of the product.
- `inStock` (boolean): Indicates the availability of the product.

#### Status codes

- 200 OK: Indicates a successful response.
- 400 Bad Request: Indicates that the parameters provided are invalid.

### Method: GET

> ```
> {{baseUrl}}/products?results=21&category=coffee
> ```

### Query Params

| Param    | value  |
| -------- | ------ |
| results  | 21     |
| category | coffee |

## End-point: Get all products - Results < minimum

Returns a list of products from the inventory.

#### Parameters

- `category` (string, query, optional): Specifies the category of products you want to be returned. It can be one of: meat-seafood, fresh-produce, candy, bread-bakery, dairy, eggs, coffee.
- `results` (integer, query, optional): Specifies the number of results you want. Must be a number between 1 and 20. By default, only 20 products will be displayed.
- `available` (boolean, query, optional): Specifies the availability of the products. By default, all products will be displayed.

#### Response

The response will be a JSON array containing objects with the following properties:

- `id` (number): The unique identifier of the product.
- `category` (string): The category of the product.
- `name` (string): The name of the product.
- `inStock` (boolean): Indicates the availability of the product.

#### Status codes

- 200 OK: Indicates a successful response.
- 400 Bad Request: Indicates that the parameters provided are invalid.

### Method: GET

> ```
> {{baseUrl}}/products?results=-1&category=coffee
> ```

### Query Params

| Param    | value  |
| -------- | ------ |
| results  | -1     |
| category | coffee |
