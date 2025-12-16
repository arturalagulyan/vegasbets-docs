# 888Lazio Integration – REST API Documentation

This document describes the REST API integration between **Our System** and **888Lazio** for **game launch** and **game action (bet/win)** synchronization.

---

## Base URLs

- **Game Launch API (Our Side)**
  ```
  https://vegasbets.site/api/getGameLaunch
  ```

- **Game Actions API (888Lazio Side)**
  ```
  https://888lazio.club/{action-endpoint}
  ```
  > The exact `{action-endpoint}` path will be provided by 888Lazio.

---

## 1. Game Launch

This endpoint is used by **888Lazio** to request a game launch URL for a specific user.

### Endpoint
```
GET /api/getGameLaunch
```

### Query Parameters

| Parameter | Type | Required | Description |
|----------|------|----------|-------------|
| id | integer | Yes | User ID in our system |
| name | string | Yes | Game provider or game short code |
| balance | decimal | Yes | User balance at the moment of launch |
| currency | string | Yes | User currency (ISO 4217, e.g. EUR) |
| email | string | Yes | User email (URL-encoded) |
| gameID | integer | Yes | Internal game ID |

### Example Request
```
GET https://vegasbets.site/api/getGameLaunch?id=19086&name=E07&balance=2052.1229&currency=EUR&email=mail638271272404804470%40mail.com&gameID=3951
```

### Successful Response
```json
{
  "status": "OK",
  "gameUrl": "https://game-provider.com/launch?token=abc123"
}
```

### Response Fields

| Field | Type | Description |
|------|------|-------------|
| status | string | Request status (OK if success) |
| gameUrl | string | Final URL to launch the game |

---

## 2. Game Actions (Bet / Win)

This endpoint is used by **Our System** to notify **888Lazio** about user game actions (bets and wins).

### Endpoint
```
POST https://888lazio.club/{action-endpoint}
```

> The endpoint path is defined and provided by **888Lazio**.

### Request Body

Content-Type: `application/json`

```json
{
  "data": {
    "userId": "12345",
    "transId": "987654321",
    "win": 150.75,
    "bet": 100.00
  }
}
```

### Request Parameters

| Field | Type | Required | Description |
|------|------|----------|-------------|
| data.userId | string | Yes | User ID in 888Lazio system |
| data.transId | string | Yes | Transaction / stat ID from our DB |
| data.win | decimal | Yes | Win amount (sent on every action) |
| data.bet | decimal | Yes | Bet amount (sent on every action) |

> Both `win` and `bet` must always be present, even if one of them is `0`.

---

### Successful Response
```json
{
  "status": "OK",
  "balance": 1952.1229
}
```

### Response Fields

| Field | Type | Description |
|------|------|-------------|
| status | string | OK if action was processed successfully |
| balance | decimal | Updated user balance in 888Lazio system |

---

## Notes

- All requests must use **HTTPS**
- Numeric values must be sent as **decimal**
- `transId` must be **unique per game action**
- Action logic follows the same flow as:
  ```
  https://asgsys.morix.info/provider/vegas
  ```

---

## Contact

For endpoint path confirmation or technical questions, please contact the **888Lazio technical team**.
