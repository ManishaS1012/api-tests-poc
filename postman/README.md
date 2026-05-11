# Postman API Tests

This directory contains Postman collections and environments for API testing.

## Directory Structure

```
postman/
├── collections/              # Postman Collection files
│   └── REST_API_test.postman_collection.json
├── environments/             # Postman Environment files
│   └── Test_Environment.postman_environment.json
└── README.md
```

## Setup Instructions

### 1. Import the Collection and Environment

1. Open **Postman**
2. Click **Import** button
3. Select `postman/collections/REST_API_test.postman_collection.json`
4. Click **Import** again for `postman/environments/Test_Environment.postman_environment.json`

### 2. Configure the Environment

1. Click on the **Environments** tab
2. Select **Test_Environment**
3. Update the following variables as needed:
   - **BaseURL**: `https://uat.ram.co.za/Integration.API.Test`
   - **auth**: Will be populated automatically after running "Get Token" request

## API Endpoints

### 1. Get Token (UAT/Get Token)
- **Method**: POST
- **Endpoint**: `{{BaseURL}}/Integration/GetToken`
- **Description**: Authenticates user and returns access token
- **Request Body**:
  ```json
  {
    "username": "uafrica",
    "password": "uafricauat123"
  }
  ```
- **Tests**: 
  - Positive Tests: TC-P-01 to TC-P-12
  - Negative Tests: TC-N-01 to TC-N-09

### 2. Consignment Upload (UAT/ConsignmentUpload)
- **Method**: POST
- **Endpoint**: `{{BaseURL}}/Integration/ConsignmentUpload`
- **Auth**: Bearer Token (from Get Token request)
- **Description**: Uploads a new consignment with parcel details
- **Tests**:
  - Positive Tests: TC-P-01 to TC-P-35
  - Negative Tests: TC-N-01 to TC-N-27

## Test Categories

### Get Token API Tests

**Positive Tests (TC-P-01 to TC-P-12)**
- HTTP status code is 200
- Response time is under 10 seconds
- Content-Type is application/json
- Response body is valid JSON
- Status field is true
- StatusCode is 200
- access_token is a non-empty string
- access_token is saved to environment
- Response schema validation
- Request schema validation

**Negative Tests (TC-N-01 to TC-N-09)**
- Response is not 401 Unauthorized
- Response is not 500 Internal Server Error
- Wrong credentials validation
- Missing username/password validation
- Null field validation
- Status and StatusCode consistency

### ConsignmentUpload API Tests

**Positive Tests (TC-P-01 to TC-P-35)**
- HTTP status code and response time validation
- Response body structure validation
- ConsignmentDetail object validation
- ConsignmentLabelDetails validation
- ParcelDetails array validation
- LabelDetails array validation
- Schema validation for all response objects
- Environment variable persistence

**Negative Tests (TC-N-01 to TC-N-27)**
- HTTP error guards (401, 403, 500, 404)
- Business rule validation
- Parcel-level validation
- Time window and date validation
- Format validation (email, coordinates)
- Error response structure validation

## Running Tests

1. **Single Request**: Click **Send** on any request
2. **Full Collection**: Use **Collection Runner**
   - Click **Collection Runner** in Postman
   - Select the collection
   - Select the environment
   - Click **Run**

## Variable Reference

| Variable | Type | Description |
|----------|------|-------------|
| BaseURL | Environment | API base URL |
| auth | Environment | Bearer token (auto-populated) |
| ConsignmentID | Environment | Generated consignment ID |
| savedConsignmentID | Environment | Saved consignment ID from response |

## Notes

- The **Get Token** request must be run first to populate the `auth` variable
- The **ConsignmentUpload** request depends on a valid auth token
- ConsignmentID is automatically generated with pattern: UAR + 3 letters + 2 digits
- All tests include assertions for both positive and negative scenarios

## Support

For issues or questions, please refer to the API documentation or contact the development team.
