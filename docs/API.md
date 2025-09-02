# API Documentation

This API provides endpoints for retrieving and managing price and market data for various energy products, including **NCG EUR**, **NCG CHF**, **TTF EUR**, and **PEG EUR**. The API supports both `GET` and `POST` methods for data retrieval and saving, with authentication handled via `DataTokenAuthHandler`, `TokenAuthHandler`, and `AuthHandler` depending on the endpoint.

---

## Base URLs

- **Production:** [`https://www.priceblick.com/api/`](https://www.priceblick.com/api/)
- **Staging:** [`https://staging.priceblick.com/api/`](https://staging.priceblick.com/api/)
- **Base Path:** `/<price_provider>/`

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
```

```python
import requests

# API endpoint
url = "https://staging.priceblick.com/api/set/the_eur"

# Auth token
headers = {
    "Content-Type": "application/json",
    "X-Auth-Token": "your_api_token_here"
}

# Payload data
payload = [
    {
        "contract": "DA",
        "price": {"type": "bid", "value": 2.2}
    },
    {
        "contract": "DA",
        "price": {"type": "ask", "value": 2.3}
    },
    {
        "contract": "CAL27",
        "price": {"type": "bid", "value": 2.2}
    },
    {
        "contract": "CAL27",
        "price": {"type": "ask", "value": 2.3}
    }
]

# Send POST request
response = requests.post(url, headers=headers, json=payload)

# Check response
if response.status_code == 200:
    print("✅ Success!")
    print(response.json())
else:
    print(f"❌ Error {response.status_code}: {response.text}")
```

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

!!! note
    For all endpoints on user level you will need to use your own user key

| Endpoint      | Description              | Method  | Auth Handler         |
|---------------|--------------------------|---------|----------------------|
| `/saveorder`  | Create a new order       | POST    | API Key required     |
| `/get_orders` | Get orders               | GET     | API Key required     |

!!! warning
    Order execution with the API is disabled by default. If you need this enabled for your
    account, please contact priceblick admin.



### Other Endpoints

| Endpoint                         | Method | Description                 | Auth Handler         |
|----------------------------------|--------|-----------------------------|----------------------|
| `/getlimitorders`                | GET    | Get limit orders            | API Key required     |
| `/setlimitorders`                | POST   | Set limit orders            | API Key required     |

| Endpoint                         | Method | Description                 | Auth Handler         |
|----------------------------------|--------|-----------------------------|----------------------|
| `/accounts`                      | GET    | Get all accounts            | API Key required     |

Get all the accounts managed by this provider. These are the accounts you can trade with.

| Endpoint                         | Method | Description                 | Auth Handler         |
|----------------------------------|--------|-----------------------------|----------------------|
| `/account/<account_uuid>/users`  | GET    | Get users for account       | API Key required     |

Get all the users for a particular account (by its uuid). These are traders in the account.

| Endpoint                         | Method | Description                 | Auth Handler         |
|----------------------------------|--------|-----------------------------|----------------------|
| `/account/<account_uuid>/margin` | POST   | Set margin on account       | API Key required     |

As a price provider you can set margins for each account. This endpoint allows you to se the margins.

Set Margin Schema

### **Schema**

| Field        | Type   | Required | Description                                |
|------------- |--------|----------|--------------------------------------------|
| account_uuid | string | Yes      | Account UUID to set margin for             |
| margin       | float  | Yes      | numeric value for maring like 0.3          |

```shell
curl "https://staging.priceblick.com/api/set/account/<account_uuid>/margin" -H "X-Auth-Token: <your api key>" -XPOST -d '{"margin":1, "account_uuid": "<account_uuid>"}'
```

Marketplace Endpoints

Market place allows individual traders to post there own bids and asks. Other market participants can then trade this order.
Each market order placed on the open marketplace has a time to live (ttl) value after which the order expires and gets removed
from the marketplace.

| Endpoint                         | Method | Description                 | Auth Handler         |
|----------------------------------|--------|-----------------------------|----------------------|
| `/get_market_bids_and_asks`      | GET    | Get all market bids/ask     | API Key required     |
| `/post_market_bid_and_ask`       | POST   | Post a market bids/ask      | API Key required     |
| `/delete_market_bid_ask`         | DELETE | Delete a market bids/ask    | API Key required     |

The get_market_bids_and_asks endpoint allows you to fetch all market orders from individual traders. This endpoint is different from
price provider specific /the_eur or /ttf_eur endpoint in that the trades here are open to all individual traders.

---

## Handlers

Each endpoint is managed by a specific handler responsible for processing the request and returning the appropriate response. Refer to individual endpoint descriptions for details on required authentication and handler logic.

---

_For request/response formats, refer to the handler implementations in the `api` package._

