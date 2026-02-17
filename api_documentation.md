# Aster Finance Futures API Documentation

**Source**: [Aster Finance Futures API V3](https://github.com/asterdex/api-docs/blob/master/aster-finance-futures-api-v3.md)

## 1. General Information
- **Base Endpoint**: `https://fapi.asterdex.com`
- **Response Format**: JSON
- **Data Order**: Ascending (Oldest first)
- **Timestamp**: Milliseconds

## 2. Authentication
Aster V3 uses a signature-based authentication mechanism embedded in the request body.

**Required Payload Parameters for Private Endpoints:**
| Parameter | Description |
|-----------|-------------|
| `user` | Main account wallet address |
| `signer` | API wallet address |
| `nonce` | Current timestamp (microseconds) |
| `signature` | ECDSA Signature of the parameter hash |

**Signing Process:**
1.  Sort API parameters by key (ASCII).
2.  Combine with `user`, `signer`, and `nonce`.
3.  Generate bytecode using Web3 ABI encoding.
4.  Hash with Keccak.
5.  Sign hash with **API Wallet Private Key**.

## 3. Key Endpoints

### Market Data (Public)
- **Check Connectivity**: `GET /fapi/v3/ping`
- **Server Time**: `GET /fapi/v3/time`
- **Exchange Info**: `GET /fapi/v3/exchangeInfo` (Symbols, limits, assets)
- **Order Book**: `GET /fapi/v3/depth?symbol=BTCUSDT`

### Trading (Private - Signed)
- **Place Order**: `POST /fapi/v3/order`
    - **Params**: `symbol`, `side` (BUY/SELL), `type` (LIMIT/MARKET), `quantity`, `price` (if LIMIT), `timeInForce`.
- **Cancel Order**: `DELETE /fapi/v3/order`
- **Cancel All**: `DELETE /fapi/v3/allOpenOrders`

### Account Data (Private - Signed)
- **Account Info**: `GET /fapi/v3/account` (Balances, positions)
- **Order History**: `GET /fapi/v3/allOrders`
- **Open Orders**: `GET /fapi/v3/openOrders`

## 4. Usage in Current Project


### Analysis
- **Packages**: The `packages/cli` directory referenced in `package.json` scripts (`agent:run`) is **missing** from the filesystem. Only `intent-schema` exists.
- **Codebase**: A search of the existing files revealed no direct calls to `fapi.asterdex.com` or implementation of the Aster signing logic.
- **Next Steps**:
    1.  Initialize the `packages/cli` package.
    2.  Implement an API client that handles the custom ECDSA signing auth.
    3.  Connect to the Aster endpoints defined above.
