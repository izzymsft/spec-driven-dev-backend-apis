## Project Specifications

This document contains the specifications for a FastAPI REST API application.

This application is going to be used to enable CRUD operations on an Azure Cosmos DB datastore. This datastore contains the following records about customers:

- Customer Profile data (stored in the `customers` Cosmos DB collection)
- Customer Address data (stored in the `addresses` Cosmos DB collection)

This is the JSON Schemas of the records in the Cosmos DB collections

### 1.1 Customers Collection

The `customers` Cosmos DB collection has JSON schema:

````json
{
  "$schema": "http://json-schema.org/draft-07/schema#",
  "title": "Customer Account Schema",
  "type": "object",
  "properties": {
    "id": {
      "type": "string",
      "format": "uuid",
      "description": "Unique identifier for the user account"
    },
    "email": {
      "type": "string",
      "format": "email",
      "description": "User's email address"
    },
    "firstName": {
      "type": "string",
      "description": "User's first name"
    },
    "lastName": {
      "type": "string",
      "description": "User's last name"
    },
    "accountCategory": {
      "type": "string",
      "enum": ["Free", "Standard", "Premium"],
      "description": "Category of the user account"
    }
  },
  "required": ["id", "email", "firstName", "lastName", "accountCategory"],
  "additionalProperties": false
}

````

### 1.2 Addresses Collection

The `addresses` Cosmos DB collection has JSON schema:

````json
{
  "$schema": "http://json-schema.org/draft-07/schema#",
  "title": "CustomerAddress",
  "type": "object",
  "properties": {
    "id": {
      "type": "string",
      "format": "uuid",
      "description": "Unique identifier for the address record"
    },
    "customerId": {
      "type": "string",
      "format": "uuid",
      "description": "Identifier of the customer associated with this address"
    },
    "address": {
      "type": "string",
      "description": "Primary street address"
    },
    "address2": {
      "type": "string",
      "description": "Secondary street address (e.g., apartment, suite, unit)",
      "minLength": 1
    },
    "city": {
      "type": "string",
      "description": "City name"
    },
    "state": {
      "type": "string",
      "description": "Two-letter U.S. state abbreviation",
      "pattern": "^[A-Z]{2}$"
    },
    "zipCode": {
      "type": "string",
      "description": "U.S. ZIP Code",
      "pattern": "^\\d{5}(-\\d{4})?$"
    },
    "addressType": {
      "type": "string",
      "enum": ["shipping", "billing"],
      "description": "Type of address"
    }
  },
  "required": [
    "id",
    "customerId",
    "address",
    "city",
    "state",
    "zipCode",
    "addressType"
  ],
  "additionalProperties": false
}

````

### 2.1 Application Dependencies

The REST API application is a FastAPI application using the following dependencies

- azure-cosmos 4.9.0
- fastapi 0.116.1
- pydantic 2.11.9

The package dependency manager is uv

### 3.0 REST API Endpoints

The REST API application will have the following REST API endpoints

#### 3.1 Create Customers

POST /api/customers

This will be used to create new records in the `customers` collection by receiving a JSON request body matching the JSON schema using the HTTP POST request method.

#### 3.2 List Customers

GET /api/customers&start={offset}&limit={recordsPerPage}

This will be used to retrieve all the records in the `customers` collection 

The start request parameter is the starting offset and the limit parameter will limit how many records are returned from the Cosmos DB collection

This will use the HTTP GET request method.

#### 3.3 Get Single Customer Record

GET /api/customers/{customerId}

This will be used to retrieve a single customer record from the `customers` collection 

The {customerId} path parameter matches the `id` field in the `customers` container.

This will use the HTTP GET request method.

#### 3.4 Update Customers

PUT /api/customers/{customerId}

This will be used to modifiy existing records in the `customers` collection by receiving a JSON request body matching the JSON schema where the {customerId} path parameter matches the `id` field in the `customers` collection.

This will use the HTTP PUT request method.

#### 3.5 Delete Customer

DELETE /api/customers/{customerId}

This will be used to remove existing records from the `customers` collection by where the {customerId} path parameter matches the `id` field in the `customers` collection.

This will use the HTTP DELETE request method.

#### 4.1 Create Customer Address

POST /api/customers/addresses

This will be used to create new records in the `addresses` collection by receiving a JSON request body matching the JSON schema for the customer address.

This will use the HTTP POST request method.

#### 4.2 List Customer Addresses

GET /api/customers/{customerId}/addresses&start={offset}&limit={recordsPerPage}

This will be used to retrieve all the records in the `addresses` collection matching the customer identifier. These are address records belonging to the specific customer.

The start request parameter is the starting offset and the limit parameter will limit how many records are returned from the Cosmos DB collection

This will use the HTTP GET request method.

#### 4.3 Get Single Address Record

GET /api/customers/{customerId}/addresses/{addressId}

This will be used to retrieve a single address record from the `addresses` collection 

The {customerId} path parameter matches the `customerId` field in the `addresses` container and the {addressId} matches the `id` field in the `addresses` container.

This will use the HTTP GET request method.

#### 4.4 Update Customer Address

PUT /api/customers/{customerId}/addresses/{addressId}

This will be used to modifiy existing records in the `addresses` collection by receiving a JSON request body matching the JSON schema where the {addressId} path parameter matches the `id` field in the `addresses` collection and the {customerId} path parameter matches the `customerId` field in the `addresses` collection.

This will use the HTTP PUT request method.

#### 3.5 Delete Customer Address

DELETE /api/customers/{customerId}/addresses/{addressId}

This will be used to remove existing records from the `customers` collection by where the {customerId} path parameter matches the `id` field in the `customers` collection.

This will use the HTTP DELETE request method.

