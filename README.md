# Email Validation API Documentation

## Overview

The Email Validation API allows you to verify if an email address is valid by interfacing with an external validation service. The API makes an HTTP POST request using a proxy configuration and returns a JSON response that includes the email, a validity flag, a status code, and a descriptive message.

## Base URL

http://<your-server-domain>/api/email

## Endpoint

### POST `/validate`

**Description:**  
Validate an email address by sending a POST request with the email data. The API checks the email using an external service and returns the validation result.

### Request Headers

- **Content-Type:** `application/json`

### Request Body

The request payload should be a JSON object with the following field:

- **email (string):** The email address to validate.

**Example Request:**

```json
{
  "email": "example@example.com"
}
```

### Response

The API will return a JSON object with the following fields:

- **email (string):** The email address that was validated.
- **isValid (boolean):** A flag indicating whether the email is valid.
- **statusCode (number):** The status code of the validation process.
- **message (string):** A descriptive message providing additional information about the validation result.

**Example Response:**

```json
{
  "email": "example@example.com",
  "isValid": true,
  "statusCode": 250,
  "message": "Valid email"
}
```

**Detailed Response Examples:**

- **Valid Email:**
  ```json
  {
    "email": "valid@example.com",
    "isValid": true,
    "statusCode": 250,
    "message": "Valid email"
  }
  ```

- **User Not Found:**
  ```json
  {
    "email": "notfound@example.com",
    "isValid": false,
    "statusCode": 550,
    "message": "Not Available - User not found"
  }
  ```

- **Mailbox Full:**
  ```json
  {
    "email": "full@example.com",
    "isValid": false,
    "statusCode": 452,
    "message": "Mailbox Full"
  }
  ```

- **Rejected by Server:**
  ```json
  {
    "email": "rejected@example.com",
    "isValid": false,
    "statusCode": 554,
    "message": "Rejected by server"
  }
  ```

- **Unexpected Status:**
  ```json
  {
    "email": "unexpected@example.com",
    "isValid": false,
    "statusCode": -1,
    "message": "Unexpected response format"
  }
  ```

### Common Status Codes

- **250:** Valid email.
- **550:** Not Available – User not found.
- **452:** Mailbox full.
- **554:** Rejected by server.
- **-1:** Email not found in domain results, domain not found in results for email, unexpected response format, or error processing response.

## Front-End Implementation

To integrate the Email Validation API in your front-end application, follow these steps:

1. **Make an HTTP POST Request:**
   Use `fetch` or any HTTP client library (e.g., Axios) to send a POST request to the `/validate` endpoint with the email data.

2. **Handle the Response:**
   Process the JSON response to handle the validation result accordingly.

**Example using cURL:**

```sh
curl -X POST http://<your-server-domain>/api/email/validate \
  -H "Content-Type: application/json" \
  -d '{"email": "example@example.com"}'
```

**Example using Fetch:**

```javascript
const validateEmail = async (email) => {
  const response = await fetch('http://<your-server-domain>/api/email/validate', {
    method: 'POST',
    headers: {
      'Content-Type': 'application/json'
    },
    body: JSON.stringify({ email })
  });

  const data = await response.json();
  return data;
};

validateEmail('example@example.com')
  .then(data => {
    if (data.isValid) {
      console.log('Email is valid:', data.message);
    } else {
      console.log('Email is invalid:', data.message);
    }
  })
  .catch(error => {
    console.error('Error validating email:', error);
  });
```

**Example using Axios:**

```javascript
const axios = require('axios');

const validateEmail = async (email) => {
  try {
    const response = await axios.post('http://<your-server-domain>/api/email/validate', { email });
    const data = response.data;

    if (data.isValid) {
      console.log('Email is valid:', data.message);
    } else {
      console.log('Email is invalid:', data.message);
    }
  } catch (error) {
    console.error('Error validating email:', error);
  }
};

validateEmail('example@example.com');
```