# API Documentation

This API provides endpoints for retrieving and managing price and market data for various energy products, including **NCG EUR**, **NCG CHF**, **TTF EUR**, and **Peg Nord EUR**. The API supports both `GET` and `POST` methods for data retrieval and saving, with authentication handled via `DataTokenAuthHandler`, `TokenAuthHandler`, and `AuthHandler` depending on the endpoint.

---

## Base URLs

- **Production:** [`https://www.priceblick.com/api/`](https://www.priceblick.com/api/)
- **Staging:** [`https://staging.priceblick.com/api/`](https://staging.priceblick.com/api/)
- **Base Path:** `/<price_provider>/`

For example: for SET the api is https://staging.priceblick.com/api/set/
---

## Authentication

All endpoints require an **API Key** for authentication.

To authenticate requests, include your API key in the `X-Auth-Token` header.

**Python Example:**

```python
import requests

url = "https://www.priceblick.com/api/set/the_eur"
headers = {
    "X-Auth-Token": "SET_ACCOUNT_API_KEY"
}

response = requests.get(url, headers=headers)
print(response.json())
```


---

## Endpoints Overview

### GET Endpoints For THE, TTF and PEG Market Data

| Endpoint      | Description               | Method | Auth Handler         |
|---------------|---------------------------|--------|----------------------|
| `/the_eur`    | Get THE EUR data          | GET    | API Key required     |
| `/the_chf`    | Get THE CHF data          | GET    | API Key required     |
| `/ttf_eur`    | Get TTF EUR data          | GET    | API Key required     |
| `/peg_eur`    | Get PEG EUR data          | GET    | API Key required     |

### POST Endpoints For THE, TTF and PEG Market Data

| Endpoint      | Description                        | Method | Auth Handler         |
|---------------|------------------------------------|--------|----------------------|
| `/the_eur`    | Save THE EUR data                  | POST   | API Key required     |
| `/the_chf`    | Save THE CHF data                  | POST   | API Key required     |
| `/ttf_eur`    | Save TTF EUR data                  | POST   | API Key required     |
| `/peg_eur`    | Save PEG EUR data                  | POST   | API Key required     |


## **Request Body**

The request body must be a **JSON array** of objects.

### **Schema**

| Field        | Type   | Required | Description                                |
|-------------|--------|----------|--------------------------------------------|
| contract    | string | Yes      | Name of the contract (`DA`, `CAL27`, etc.) |
| price       | object | Yes      | Price details                             |
| price.type  | string | Yes      | `bid` or `ask`                            |
| price.value | float  | Yes      | Price value                               |

---

### **Example Request**

```json
[
  {
    "contract": "DA",
    "price": {
      "type": "bid",
      "value": 2.2
    }
  },
  {
    "contract": "DA",
    "price": {
      "type": "ask",
      "value": 2.3
    }
  },
  {
    "contract": "CAL27",
    "price": {
      "type": "bid",
      "value": 2.2
    }
  },
  {
    "contract": "CAL27",
    "price": {
      "type": "ask",
      "value": 2.3
    }
  }
]
```]

## **Error Codes**

| Status Code | Meaning                 | Possible Cause                  |
|------------|------------------------|--------------------------------|
| **200**    | Success               | Prices updated successfully   |
| **400**    | Bad Request           | Missing or invalid fields     |
| **401**    | Unauthorized          | Invalid or missing token      |
| **403**    | Forbidden             | You don’t have permission     |
| **404**    | Not Found            | Endpoint or resource missing  |
| **500**    | Internal Server Error | Server-side issue            |
| **503**    | Service Unavailable   | API temporarily down or overloaded |

### Endpoint for Trade Execution

| Endpoint      | Description              | Method  | Auth Handler         |
|---------------|--------------------------|---------|----------------------|
| `/saveorder`  | Create a new order       | POST    | API Key required     |
| `/get_orders` | Get orders               | GET     | API Key required     |


### Other Endpoints

| Endpoint                | Method | Description                 | Auth Handler         |
|-------------------------|--------|-----------------------------|----------------------|
| `/getlimitorders`       | GET    | Get limit orders            | API Key required     |
| `/setlimitorders`       | POST   | Set limit orders            | API Key required     |

---

## Handlers

Each endpoint is managed by a specific handler responsible for processing the request and returning the appropriate response. Refer to individual endpoint descriptions for details on required authentication and handler logic.

---

_For request/response formats, refer to the handler implementations in the `api` package._

