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

### Endpoint for Trade Execution

| Endpoint      | Description              | Method  | Auth Handler         |
|---------------|--------------------------|---------|----------------------|
| `/saveorder`  | Create a new order       | POST    | API Key required     |
| `/get_orders` | Get orders               | GET     | API Key required     |


### Other Endpoints

| Endpoint                | Method | Description                 | Auth Handler         |
|-------------------------|--------|-----------------------------|----------------------|
| `/savepriceprofile`     | POST   | Save price profiles         | API Key required     |
| `/getpriceprofile`      | GET    | Get price profiles          | API Key required     |
| `/deletepriceprofile`   | POST   | Delete price profiles       | API Key required     |
| `/buypriceprofile`      | POST   | Buy price profiles          | API Key required     |
| `/getlimitorders`       | GET    | Get limit orders            | API Key required     |
| `/setlimitorders`       | POST   | Set limit orders            | API Key required     |

---

## Handlers

Each endpoint is managed by a specific handler responsible for processing the request and returning the appropriate response. Refer to individual endpoint descriptions for details on required authentication and handler logic.

---

_For request/response formats, refer to the handler implementations in the `api` package._

