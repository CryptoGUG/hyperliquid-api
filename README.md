# Interacting with Hyperliquid API

1. Install Dependencies
   
First, let’s install the required library for making HTTP requests. We’ll use Python's requests library to interact with the Hyperliquid API.

```
pip install requests
```

2. Set Up API Connection
   
To interact with Hyperliquid’s API, you'll need an API key. You can get it from your account settings on the Hyperliquid platform. Store it in a variable so that you can include it in the request headers.

```
import requests

# Set your API key and base URL
API_KEY = "your_api_key"
BASE_URL = "https://api.hyperliquid.com"

# Headers for authentication
headers = {
    "Authorization": f"Bearer {API_KEY}",
    "Content-Type": "application/json"
}
```

3. Fetch Market Data
   
Now, let's fetch real-time market data, such as price information for a specific trading pair (for example, BTC/USDT).

```
def get_market_data(market: str):
    endpoint = f"{BASE_URL}/markets/{market}"
    response = requests.get(endpoint, headers=headers)
    
    # Check if request is successful
    if response.status_code == 200:
        return response.json()
    else:
        return f"Error: {response.status_code}"

# Get market data for BTC/USDT
market_data = get_market_data("BTC/USDT")
print(market_data)
```
4. Place a Limit Order
   
Next, you can place a limit order to buy or sell a specific cryptocurrency at a target price. In the example below, we’ll place a limit order to buy 0.1 BTC at $20,000.

```
def place_order(market: str, side: str, price: float, size: float):
    endpoint = f"{BASE_URL}/orders"
    data = {
        "market": market,     # Example: "BTC/USDT"
        "side": side,         # "buy" or "sell"
        "price": price,       # The limit price you want to set
        "size": size,         # Order size (amount to buy/sell)
        "type": "limit"       # Limit order type
    }
    
    response = requests.post(endpoint, headers=headers, json=data)
    
    if response.status_code == 201:
        return response.json()  # Return the order details
    else:
        return f"Error: {response.status_code}, {response.text}"

# Place a limit order to buy 0.1 BTC at 20000 USDT
order_response = place_order("BTC/USDT", "buy", 20000, 0.1)
print(order_response)
```
5. Check Order Status
   
Once you've placed an order, you can monitor its status (whether it has been filled, partially filled, or is still pending).

```
def get_order_status(order_id: str):
    endpoint = f"{BASE_URL}/orders/{order_id}"
    
    response = requests.get(endpoint, headers=headers)
    
    if response.status_code == 200:
        return response.json()  # Return the order status
    else:
        return f"Error: {response.status_code}, {response.text}"

# Check the status of a specific order using its order ID
order_status = get_order_status("your_order_id")
print(order_status)
```
6. Cancel an Order
   
If you need to cancel a pending order, you can do so with this method:

```
def cancel_order(order_id: str):
    endpoint = f"{BASE_URL}/orders/{order_id}/cancel"
    
    response = requests.post(endpoint, headers=headers)
    
    if response.status_code == 200:
        return "Order successfully cancelled"
    else:
        return f"Error: {response.status_code}, {response.text}"

# Cancel an order by providing the order ID
cancel_response = cancel_order("your_order_id")
print(cancel_response)
```
7. Conclusion

This guide provides a simple overview of how to:

- Connect to Hyperliquid’s API using Python.
- Fetch real-time market data.
- Place a limit order.
- Check the status of your orders.
- Cancel orders when needed.

You can build on top of this guide to create more complex trading strategies or automate trades based on specific conditions.
