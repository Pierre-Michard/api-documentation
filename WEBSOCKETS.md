# WebSocket API

## Private messages (user details, payment updates)

### Channel subscription

These messages are broadcast on specific channels, and not publicly. Knowledge of the channel identifier is required in order to subscribe to these messages.

There are two types of channels:
 * User channels: they broadcast messages relevant to a particular user
 * Payment channels: they broadcast messages relevant to a specific payment

Clients will typically subscribe to the relevant user channel if authenticated, and to every payment-specific channel of interest to them.

The user channel ID is returned in the "channel" key in response to sign-in POSTs or user GETs. The payment channel ID is the payment's UUID, as returned by the REST API calls.

All the messages in the "Private messages" section are sent on user channels, with the exception of payment updates, which are sent on the specific payment's channel.


### Balance changes

These messages are emitted when an account balance is updated, when the updated balance is the locked for trading balance, the "trading" key is set to true.

**Message structure**
````json
{
  "event": "balance",
  "payload": {
    "balance": <new balance>,
    "currency": <"EUR"|"BTC">,
    "trading":  <true|false>
}
````


### Order update/creation

Emitted whenever an order is persisted. See the REST API documentation for details on order serialization.

**Message structure**
````json
{
  "event": "order",
  "payload": <JSON-serialized order>
}
````

### Text notification

Emitted whenever a text notification is issued to the user.

**Message structure**
````json
{
  "event": "notice",
  "payload": {
    "message": <message>
  }
}
````

### User state update

Emitted whenever the user state is updated (for example due to KYC steps being completed). See the REST API documentation for details on the different possible states.

**Message structure**
````json
{
  "event": "state",
  "payload": {
    "state": <new state>
  }
}
````

### Payments updates

Emitted whenever a payment is updated. See the REST API documentation for details on payment serialization. These messages are only emitted on the specific payment channels.

**Message structure**
````json
{
  "event": "payment",
  "payload": <JSON-serialized payment>
}
````


## Public messages

These messages are publicly broadcast, each type is broadcast on a separate channel and requires a separate subscribtion.

### Trades

These messages are broadcast on the "trade" channel.

**Message structure**
````json
{
  "event": "new-trade",
  "payload": {
    "price": <execution price>,
    "traded_btc": <traded quantity>,
    "timestamp": <msec timestamp>,
    "currency": "EUR"
  }
}
````


### Ticker updates

These messages are broadcast on the "ticker" channel.

**Message structure**
````json
{
  "event": "tick",
  "payload": <ticker>
}
````

### Market depth updates

These messages are broadcast on the "market-depth" channel.

**Message structure**
````json
{
  "event": "update-market-depth",
  "payload": {
    "category": <"buy"|"sell">,
    "timestamp": <sec timestamp>,
    "amount": <amount>
    "price": <price>,
    "currency": "EUR"
  }
}
````