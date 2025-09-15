# Contoso Customer API - Backend REST API

This is a FastAPI-based REST API application for managing customers and their addresses using Azure Cosmos DB. The project demonstrates spec-driven development using Visual Studio Code and various LLMs.

## Features

- **Customer Management**: Complete CRUD operations for customer records
- **Address Management**: Complete CRUD operations for customer addresses
- **Azure Cosmos DB Integration**: Leveraging Azure Cosmos DB for data persistence
- **Data Validation**: Comprehensive input validation using Pydantic models
- **Comprehensive Testing**: Unit tests with pytest
- **API Documentation**: Auto-generated OpenAPI/Swagger documentation

## Architecture

The application follows a clean architecture pattern with:

- **FastAPI**: Modern, fast web framework for building APIs
- **Pydantic**: Data validation and serialization using Python type annotations
- **Azure Cosmos DB**: NoSQL database for scalable data storage
- **Pytest**: Testing framework with comprehensive test coverage

## API Endpoints

### Customer Endpoints

- `POST /api/customers` - Create a new customer
- `GET /api/customers` - List customers with pagination
- `GET /api/customers/{customerId}` - Get a specific customer
- `PUT /api/customers/{customerId}` - Update a customer
- `DELETE /api/customers/{customerId}` - Delete a customer

### Address Endpoints

- `POST /api/customers/addresses` - Create a new customer address
- `GET /api/customers/{customerId}/addresses` - List addresses for a customer
- `GET /api/customers/{customerId}/addresses/{addressId}` - Get a specific address
- `PUT /api/customers/{customerId}/addresses/{addressId}` - Update an address
- `DELETE /api/customers/{customerId}/addresses/{addressId}` - Delete an address

## Prerequisites

- Python 3.12 or higher
- Azure Cosmos DB account and connection string
- [uv](https://github.com/astral-sh/uv) package manager

## Setup Instructions

### 1. Clone and Navigate to the Project

```bash
git clone <repository-url>
cd spec-driven-dev-backend-apis
```

### 2. Install Dependencies

Using uv (recommended):
```bash
uv sync
```

Or using pip:
```bash
pip install -r requirements.txt
```

### 3. Environment Configuration

Copy the example environment file and configure your settings:

```bash
cp .env.example .env
```

Edit `.env` file with your Azure Cosmos DB configuration:

```env
COSMOS_CONNECTION_STRING=AccountEndpoint=https://your-cosmos-account.documents.azure.com:443/;AccountKey=your-primary-key;
COSMOS_DATABASE_NAME=contoso_customer_db
```

### 4. Database Setup

The application will automatically create the database and containers on first run. Ensure your Cosmos DB account has sufficient permissions.

## Running the Application

### Development Mode

```bash
# Using uv
uv run python main.py

# Or using python directly
python main.py
```

The API will be available at:
- **API Base URL**: http://localhost:8000
- **Interactive Docs**: http://localhost:8000/docs
- **ReDoc**: http://localhost:8000/redoc

### Production Mode

```bash
# Using uvicorn directly
uvicorn main:app --host 0.0.0.0 --port 8000

# With multiple workers
uvicorn main:app --host 0.0.0.0 --port 8000 --workers 4
```

## Testing

Run the comprehensive test suite:

```bash
# Run all tests
uv run pytest

# Run with coverage
uv run pytest --cov=contoso_api_backend

# Example to Run specific test files
uv run pytest tests/test_customers.py
uv run pytest tests/test_addresses.py
uv run pytest tests/test_models.py
```

## Data Models

### Customer Model

```json
{
  "id": "string (UUID)",
  "firstName": "string",
  "lastName": "string", 
  "accountCategory": "Free | Standard | Premium"
}
```

### Customer Address Model

```json
{
  "id": "string (UUID)",
  "customerId": "string (UUID)",
  "address": "string",
  "address2": "string (optional)",
  "city": "string",
  "state": "string (2-letter code)",
  "zipCode": "string (12345 or 12345-6789)",
  "addressType": "shipping | billing"
}
```

## API Testing

You can test the API using the `humao.rest-client` VSCode extension or any HTTP client like curl or Postman.

### Example Requests

#### Create a Customer
```http
POST http://localhost:8000/api/customers
Content-Type: application/json

{
  "firstName": "John",
  "lastName": "Doe",
  "accountCategory": "Standard"
}
```

#### Create an Address
```http
POST http://localhost:8000/api/customers/addresses
Content-Type: application/json

{
  "customerId": "123e4567-e89b-12d3-a456-426614174000",
  "address": "123 Main St",
  "address2": "Apt 4B",
  "city": "Seattle",
  "state": "WA",
  "zipCode": "98101",
  "addressType": "shipping"
}
```

## Project Structure

```
spec-driven-dev-backend-apis/
├── contoso_api_backend/         # Main application package
│   ├── __init__.py
│   ├── models.py               # Pydantic data models
│   ├── database.py            # Cosmos DB connection management
│   ├── customers.py           # Customer endpoint handlers
│   └── addresses.py           # Address endpoint handlers
├── tests/                     # Test suite
│   ├── __init__.py
│   ├── conftest.py           # Test fixtures
│   ├── test_customers.py     # Customer endpoint tests
│   ├── test_addresses.py     # Address endpoint tests
│   └── test_models.py        # Model validation tests
├── main.py                   # FastAPI application entry point
├── pyproject.toml           # Project dependencies and metadata
├── .env.example             # Environment configuration template
└── README.md               # This file
```

## Environment Variables

| Variable | Description | Required |
|----------|-------------|----------|
| `COSMOS_CONNECTION_STRING` | Azure Cosmos DB connection string | Yes |
| `COSMOS_DATABASE_NAME` | Name of the Cosmos DB database | Yes |
| `COSMOS_ENDPOINT` | Alternative to connection string (requires auth setup) | No |

## Error Handling

The API includes comprehensive error handling with appropriate HTTP status codes:

- `200 OK` - Successful GET/PUT operations
- `201 Created` - Successful POST operations  
- `204 No Content` - Successful DELETE operations
- `404 Not Found` - Resource not found
- `422 Unprocessable Entity` - Validation errors
- `400 Bad Request` - General client errors
- `500 Internal Server Error` - Server errors

## Contributing

1. Follow the existing code structure and patterns
2. Add comprehensive tests for new features
3. Update documentation as needed
4. Ensure all tests pass before submitting changes

## License

This project is licensed under the MIT License - see the LICENSE file for details.
