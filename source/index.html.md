---
title: API Reference

language_tabs: # must be one of https://git.io/vQNgJ
  - JSON

toc_footers:
  - <a href='https://newbridge.ai'>NewBridge Home</a>
  - <a href='https://github.com/slatedocs/slate'>Documentation Powered by Slate</a>

includes:
  - errors

search: true

code_clipboard: true

meta:
  - name: description
    content: Documentation for the Markets Data JSON
---

# **Market Data API**

Status: Working Draft \
Author: Igor Pisarenko \
Date: 2023-08-23 \
Version: 1.0


## **Overview**
All market data messages are delivered on subscriptions via a web socket with /ws path. The instrument reference data messages are delivered as a query reply. A subscription key consists of a stream (message type) and a symbol’s pattern, which supports the “*” wildcard (BTCUSDT, BTC*, “*”, etc). The wildcard allows to subscribe for a range of instruments by a single subscription key. All message timestamps have the nanoseconds resolution. All active client’s subscriptions are cancelled on a client’s web socket disconnection. The web socket server sends a ping frame every 5 minutes. If the web socket server does not receive a pong frame within 15 minutes, the web socket connection is closed. The unsolicited pong frames are allowed. For the instrument reference data subscription normally only in a single reply is sent. The market data/instrument reference data web socket end point is not authenticated (ws).


## **Subscription request.**
A subscription request can send only a single subscription key.

```
Examples:

{
  "msg" : "subscription",
  "ts" : 1669032206385873,
  "seqn" : 177,
  "op" : "sub",
  "req-id" : 1,
  "send-interval-msec" : 1000,
  "stream" : "trade",
  "pattern" : "BTC*"
}

{
  "msg" : "subscription",
  "ts" : 1669032206385873,
  "seqn" : 177,
  "op" : "unsub",
  "stream" : "orderbook-snapshot",
  "pattern" : "BTC*",
  "depth" : 10
}

```

<table>
  <tr>
   <td><strong>Name</strong>
   </td>
   <td><strong>Type</strong>
   </td>
   <td><strong>Mandatory</strong>
   </td>
   <td><strong>Description</strong>
   </td>
  </tr>
  <tr>
   <td>msg
   </td>
   <td>string
   </td>
   <td>Y
   </td>
   <td>Message type: “subscription”
   </td>
  </tr>
  <tr>
   <td>ts
   </td>
   <td>timestamp [usec]
   </td>
   <td>Y
   </td>
   <td>Client’s timestamp (microseconds)
   </td>
  </tr>
  <tr>
   <td>seqn
   </td>
   <td>int64
   </td>
   <td>N
   </td>
   <td>Client’s sequence number (max: 16 digits)
   </td>
  </tr>
  <tr>
   <td>op
   </td>
   <td>string
   </td>
   <td>Y
   </td>
   <td>Operation: “sub”, “unsub”
   </td>
  </tr>
  <tr>
   <td>req-id
   </td>
   <td>int32
   </td>
   <td>N
   </td>
   <td>Subscription request id
   </td>
  </tr>
  <tr>
   <td>stream
   </td>
   <td>string
   </td>
   <td>Y
   </td>
   <td>Stream name
   </td>
  </tr>
  <tr>
   <td>pattern
   </td>
   <td>string
   </td>
   <td>Y
   </td>
   <td>Symbol’s pattern
   </td>
  </tr>
  <tr>
   <td>send-interval-msec
   </td>
   <td>int32
   </td>
   <td>N
   </td>
   <td>sending interval [msec]; real-time, if not presented
   </td>
  </tr>
  <tr>
   <td>interval-sec
   </td>
   <td>int32
   </td>
   <td>N
   </td>
   <td>interval (candle subscription only), available intervals [sec]: 1, 60, 3600, 86400
   </td>
  </tr>
  <tr>
   <td>depth
   </td>
   <td>int32
   </td>
   <td>N
   </td>
   <td>Order book snapshot depth (orderbook-snapshot subscription only): 5, 10, 20
   </td>
  </tr>
</table>

## **Subscription reply.**
A subscription reply may indicate the success or error.

```
Examples:

{
  "msg" : "subscription-reply",
  "ts" : 1669032206385873,
  "seqn" : 277,
  "req-id" : 1,
  "op" : "sub",
  "stream" : "orderbook-snapshot",
  "pattern" : "*",
  "depth" : 20,
  "result" : "success”
}

{
  "msg" : "subscription-reply",
  "ts" : 1669032206385873,
  "seqn" : 277,
  "op" : "unsub",
  "stream" : "candle",
  "pattern" : "BTCUS",
  "interval-sec" : 3600,
  "result" : "error",
  "err-code" : 48,
  "err-msg" : "Invalid subscription: pattern=BTCUS"
}
```

<table>
  <tr>
   <td><strong>Name</strong>
   </td>
   <td><strong>Type</strong>
   </td>
   <td><strong>Mandatory</strong>
   </td>
   <td><strong>Description</strong>
   </td>
  </tr>
  <tr>
   <td>msg
   </td>
   <td>string
   </td>
   <td>Y
   </td>
   <td>Message type: “subscription-reply”
   </td>
  </tr>
  <tr>
   <td>ts
   </td>
   <td>timestamp [usec]
   </td>
   <td>Y
   </td>
   <td>Message generation timestamp (microseconds)
   </td>
  </tr>
  <tr>
   <td>seqn
   </td>
   <td>int64
   </td>
   <td>N
   </td>
   <td>Back-end sequence number (max: 16 digits)
   </td>
  </tr>
  <tr>
   <td>op
   </td>
   <td>string
   </td>
   <td>Y
   </td>
   <td>Operation: “sub”, “unsub”
   </td>
  </tr>
  <tr>
   <td>req-id
   </td>
   <td>int32
   </td>
   <td>N
   </td>
   <td>Subscription request id
   </td>
  </tr>
  <tr>
   <td>pattern
   </td>
   <td>string
   </td>
   <td>Y
   </td>
   <td>Symbol’s pattern
   </td>
  </tr>
  <tr>
   <td>send-interval-msec
   </td>
   <td>int32
   </td>
   <td>Y
   </td>
   <td>sending interval [msec]
   </td>
  </tr>
  <tr>
   <td>interval-sec
   </td>
   <td>int32
   </td>
   <td>N
   </td>
   <td>Interval (candle subscription only), available intervals [sec]: 1, 60, 3600, 86400
   </td>
  </tr>
  <tr>
   <td>depth
   </td>
   <td>int32
   </td>
   <td>N
   </td>
   <td>Order book snapshot depth (orderbook-snapshot subscription only): 5, 10, 20
   </td>
  </tr>
  <tr>
   <td>status
   </td>
   <td>string
   </td>
   <td>Y
   </td>
   <td>Operation status: “success”, “error”
   </td>
  </tr>
  <tr>
   <td>err-code
   </td>
   <td>int32
   </td>
   <td>N
   </td>
   <td>Error code
   </td>
  </tr>
  <tr>
   <td>err-msg
   </td>
   <td>string
   </td>
   <td>N
   </td>
   <td>Error message
   </td>
  </tr>
</table>

## **Best bid/ask message.**
This is a snapshot message. Missing bid/ask side indicates a single sided book.

```
Examples:

{
  "msg" : "best-bid-ask",
  "ts" : 1669026845496043,
  "stream-seqn" : 10167,
  "seqn" : 16716,
  "symbol" : "BTCUSDT",
  "bid-px" : "17567.34",
  "bid-qty" : "5.76345",
  "ask-px" : "17567.44",
  "ask-qty" : "4.18456"
}

```

<table>
  <tr>
   <td><strong>Name</strong>
   </td>
   <td><strong>Type</strong>
   </td>
   <td><strong>Mandatory</strong>
   </td>
   <td><strong>Description</strong>
   </td>
  </tr>
  <tr>
   <td>msg
   </td>
   <td>string
   </td>
   <td>Y
   </td>
   <td>Message name: “best-bid-ask”
   </td>
  </tr>
  <tr>
   <td>ts
   </td>
   <td>timestamp [usec]
   </td>
   <td>Y
   </td>
   <td>Message generation timestamp (microseconds)
   </td>
  </tr>
  <tr>
   <td>stream-seqn
   </td>
   <td>int64
   </td>
   <td>N
   </td>
   <td>Market data stream sequence number (max: 16 digits)
   </td>
  </tr>
  <tr>
   <td>seqn
   </td>
   <td>int64
   </td>
   <td>N
   </td>
   <td>Back-end sequence number (max: 16 digits)
   </td>
  </tr>
  <tr>
   <td>symbol
   </td>
   <td>string
   </td>
   <td>Y
   </td>
   <td>Symbol
   </td>
  </tr>
  <tr>
   <td>bid-px
   </td>
   <td>price
   </td>
   <td>N
   </td>
   <td>Best bid price
   </td>
  </tr>
  <tr>
   <td>bid-qty
   </td>
   <td>quantity
   </td>
   <td>N
   </td>
   <td>Best bid quantity
   </td>
  </tr>
  <tr>
   <td>ask-px
   </td>
   <td>price
   </td>
   <td>N
   </td>
   <td>Best ask price
   </td>
  </tr>
  <tr>
   <td>ask-qty
   </td>
   <td>quantity
   </td>
   <td>N
   </td>
   <td>Best ask quantity
   </td>
  </tr>
</table>

## **Trade message.**
Individual trade tick message.

```
Example:

{
  "msg" : "trade",
  "ts" : 1669030387918824,
  "stream-seqn" : 170167,
  "seqn" : 180444,
  "symbol" : "BTCUSDT",
  "trade-id" : "6690303879188244",
  "trade-px" : "17567.34",
  "trade-qty" : "1.23978",
  "maker-buyer" : true
}
```

<table>
  <tr>
   <td><strong>Name</strong>
   </td>
   <td><strong>Type</strong>
   </td>
   <td><strong>Mandatory  </strong>
   </td>
   <td><strong>Description</strong>
   </td>
  </tr>
  <tr>
   <td>msg
   </td>
   <td>string
   </td>
   <td>Y
   </td>
   <td>Message name: “trade”
   </td>
  </tr>
  <tr>
   <td>ts
   </td>
   <td>timestamp [usec]
   </td>
   <td>Y
   </td>
   <td>Message generation timestamp (microseconds)
   </td>
  </tr>
  <tr>
   <td>stream-seqn
   </td>
   <td>int64
   </td>
   <td>N
   </td>
   <td>Market data stream sequence number (max: 16 digits)
   </td>
  </tr>
  <tr>
   <td>seqn
   </td>
   <td>int64
   </td>
   <td>N
   </td>
   <td>Back-end sequence number (max: 16 digits)
   </td>
  </tr>
  <tr>
   <td>symbol
   </td>
   <td>string
   </td>
   <td>Y
   </td>
   <td>Symbol
   </td>
  </tr>
  <tr>
   <td>trade-id
   </td>
   <td>int64
   </td>
   <td>Y
   </td>
   <td>Unique trade id (max: 16 digits)
   </td>
  </tr>
  <tr>
   <td>trade-px
   </td>
   <td>price
   </td>
   <td>Y
   </td>
   <td>Trade price
   </td>
  </tr>
  <tr>
   <td>trade-qty
   </td>
   <td>quantity
   </td>
   <td>Y
   </td>
   <td>Trade quantity
   </td>
  </tr>
  <tr>
   <td>maker-buyer
   </td>
   <td>boolean
   </td>
   <td>Y
   </td>
   <td>Maker is a buyer flag
   </td>
  </tr>
</table>

## **Orderbook snapshot message.**
Orderbook snapshot tick message. Supported depths: 5, 10 and 20, which must be specified in the subscription request’s “depth” field.

```
Price level snapshot value: [price, quantity, number of orders].

Examples:

{
  "msg" : "orderbook-snapshot",
  "ts" : 1669030575242078,
  "stream-seqn" : 170167,
  "seqn" : 189282,
  "symbol" : "BTCUSDT",
  "bids" : [
    ["17567.34","2.01235",4],
    ["17567.25","1.45169",2],
    ...
    ["17239.11","0.95432",6]
  ],
  "asks" : [
    ["17567.56","6.34985",3],
    ["17567.67","1.45169",7],
    ...
    ["17611.11","0.78911",4]
  ]
}

{
  "msg" : "orderbook-snapshot",
  "ts" : 1669030575242078,
  "stream-seqn" : 170167,
  "seqn" : 189282,
  "symbol" : "BTCUSDT",
  "bids" : [],
  "asks" : []
}
```

<table>
  <tr>
   <td><strong>Name</strong>
   </td>
   <td><strong>Type</strong>
   </td>
   <td><strong>Mandatory</strong>
   </td>
   <td><strong>Description</strong>
   </td>
  </tr>
  <tr>
   <td>msg
   </td>
   <td>string
   </td>
   <td>Y
   </td>
   <td>Message name: “orderbook-snapshot”
   </td>
  </tr>
  <tr>
   <td>ts
   </td>
   <td>timestamp [usec]
   </td>
   <td>Y
   </td>
   <td>Message generation timestamp (microseconds)
   </td>
  </tr>
  <tr>
   <td>stream-seqn
   </td>
   <td>int64
   </td>
   <td>N
   </td>
   <td>Market data stream sequence number (max: 16 digits)
   </td>
  </tr>
  <tr>
   <td>seqn
   </td>
   <td>int64
   </td>
   <td>N
   </td>
   <td>Back-end sequence number (max: 16 digits)
   </td>
  </tr>
  <tr>
   <td>symbol
   </td>
   <td>string
   </td>
   <td>Y
   </td>
   <td>Symbol
   </td>
  </tr>
  <tr>
   <td>bids
   </td>
   <td>sequence of price levels
   </td>
   <td>N
   </td>
   <td>Full bid (all price levels) order book
   </td>
  </tr>
  <tr>
   <td>asks
   </td>
   <td>sequence of price levels
   </td>
   <td>N
   </td>
   <td>Full ask (all price levels) order book
   </td>
  </tr>
</table>

## **Orderbook update message.**
Orderbook update (delta) tick message. A snapshot message may be delivered as a sequence of updates reconstructing a whole book, which is indicated via the snapshot flag.

```
Price level update value: [update command (“n” (new), “c” (change), “d” (delete)), level, price, quantity, number of orders].

Examples:

{
  "msg" : "orderbook-update",
  "ts" : 1669031373575253,
  "stream-seqn" : 170167,
  "seqn" : 226768,
  "symbol" : "BTCUSDT",
  "bids" : [
    ["c", 3, "17567.34","2.01235",4],
    ["d", 1, "0.","0.",0],
    ["n", 5, "17511.16","0.12",1]
  ],
  "asks" : [
    ["d", 0, "0.","0.",0],
    ["n", 2, "17614.77","1.67",1],
    ["c", 1, "17601.56","0.98",2]
  ]
}

{
  "msg" : "orderbook-update",
  "ts" : 1669031373575253,
  "stream-seqn" : 170167,
  "seqn" : 226768,
  "symbol" : "BTCUSDT",
  "bids" : [],
  "asks" : [],
  "snapshot" : true
}

```


<table>
  <tr>
   <td><strong>Name</strong>
   </td>
   <td><strong>Type</strong>
   </td>
   <td><strong>Mandatory</strong>
   </td>
   <td><strong>Description</strong>
   </td>
  </tr>
  <tr>
   <td>msg
   </td>
   <td>string
   </td>
   <td>Y
   </td>
   <td>Message name: “orderbook-update”
   </td>
  </tr>
  <tr>
   <td>ts
   </td>
   <td>timestamp [usec]
   </td>
   <td>Y
   </td>
   <td>Message generation timestamp (microseconds)
   </td>
  </tr>
  <tr>
   <td>stream-seqn
   </td>
   <td>int64
   </td>
   <td>N
   </td>
   <td>Market data stream sequence number (max: 16 digits)
   </td>
  </tr>
  <tr>
   <td>seqn
   </td>
   <td>int64
   </td>
   <td>N
   </td>
   <td>Back-end sequence number (max: 16 digits)
   </td>
  </tr>
  <tr>
   <td>symbol
   </td>
   <td>string
   </td>
   <td>Y
   </td>
   <td>Symbol
   </td>
  </tr>
  <tr>
   <td>bids
   </td>
   <td>sequence of price level updates
   </td>
   <td>N
   </td>
   <td>Bid order book updates sequence
   </td>
  </tr>
  <tr>
   <td>asks
   </td>
   <td>Sequence of price level updates
   </td>
   <td>N
   </td>
   <td>Ask order book updates sequence
   </td>
  </tr>
  <tr>
   <td>snapshot
   </td>
   <td>boolean
   </td>
   <td>N
   </td>
   <td>Bid and ask books message in a snapshot sequence
   </td>
  </tr>
  <tr>
   <td>snapshot-first
   </td>
   <td>boolean
   </td>
   <td>N
   </td>
   <td>First snapshot message in a snapshot sequence
   </td>
  </tr>
  <tr>
   <td>snapshot-last
   </td>
   <td>boolean
   </td>
   <td>N
   </td>
   <td>Last snapshot message in a snapshot sequence
   </td>
  </tr>
</table>

## **Kline/candlestick message.**
Supported intervals: 1 sec, 1 min, 1 hour and 1 day, which must be specified in the subscription request's "interval" field.

```
Example:

{
  "msg" : "candle",
  "ts" : 1669032206998413,
  "stream-seqn" : 170167,
  "seqn" : 265977,
  "start-ts" : 1669032206000000,
  "end-ts" :   1669032207000000,
  "interval-sec" : 60,
  "symbol" : "BTCUSDT",
  "open-px" : "20201.15",
  "last-px" : "20202.34",
  "high-px" : "20205.87",
  "low-px" : "20199.98",
  "num-trades" : 53197,
  "volume" : "3909.78543",
  "quote-volume" : "4760.48"
}
```

<table>
  <tr>
   <td><strong>Name</strong>
   </td>
   <td><strong>Type</strong>
   </td>
   <td><strong>Mandatory</strong>
   </td>
   <td><strong>Description</strong>
   </td>
  </tr>
  <tr>
   <td>msg
   </td>
   <td>string
   </td>
   <td>Y
   </td>
   <td>Message name: “candle”
   </td>
  </tr>
  <tr>
   <td>ts
   </td>
   <td>timestamp [usec]
   </td>
   <td>Y
   </td>
   <td>Message generation timestamp (microseconds)
   </td>
  </tr>
  <tr>
   <td>stream-seqn
   </td>
   <td>int64
   </td>
   <td>N
   </td>
   <td>Market data stream sequence number (max: 16 digits)
   </td>
  </tr>
  <tr>
   <td>seqn
   </td>
   <td>int64
   </td>
   <td>N
   </td>
   <td>Back-end sequence number (max: 16 digits)
   </td>
  </tr>
  <tr>
   <td>start-ts
   </td>
   <td>timestamp [usec]
   </td>
   <td>Y
   </td>
   <td>Start of interval timestamp (microseconds)
   </td>
  </tr>
  <tr>
   <td>end-ts
   </td>
   <td>timestamp [usec]
   </td>
   <td>Y
   </td>
   <td>End of interval timestamp (microseconds)
   </td>
  </tr>
  <tr>
   <td>interval-sec
   </td>
   <td>int32
   </td>
   <td>Y
   </td>
   <td>Interval [secs]
   </td>
  </tr>
  <tr>
   <td>symbol
   </td>
   <td>string
   </td>
   <td>Y
   </td>
   <td>Symbol
   </td>
  </tr>
  <tr>
   <td>open-px
   </td>
   <td>price
   </td>
   <td>N
   </td>
   <td>Open traded price
   </td>
  </tr>
  <tr>
   <td>last-px
   </td>
   <td>price
   </td>
   <td>N
   </td>
   <td>Last/close traded price
   </td>
  </tr>
  <tr>
   <td>high-px
   </td>
   <td>price
   </td>
   <td>N
   </td>
   <td>Highest traded  price
   </td>
  </tr>
  <tr>
   <td>low-px
   </td>
   <td>price
   </td>
   <td>N
   </td>
   <td>Lowest traded  price
   </td>
  </tr>
  <tr>
   <td>num-trades
   </td>
   <td>int32
   </td>
   <td>N
   </td>
   <td>Number of trades
   </td>
  </tr>
  <tr>
   <td>volume
   </td>
   <td>quantity
   </td>
   <td>N
   </td>
   <td>Total traded volume
   </td>
  </tr>
  <tr>
   <td>quote-volume
   </td>
   <td>price
   </td>
   <td>N
   </td>
   <td>Total traded quote volume
   </td>
  </tr>
</table>


## **Kline/candlestick query.**
Supported intervals: 1 sec, 1 min, 1 hour, 1 day.

```
Example:

{
  "msg" : "candle-query",
  "ts" : 1669032206998413,
  "seqn" : 177,
  "start-ts" : 1669032206000000,
  "end-ts" :   1669032207000000,
  "interval-sec" : 60,
  "symbol" : "BTCUSDT",
  "limit" : 600
}
```

<table>
  <tr>
   <td><strong>Name</strong>
   </td>
   <td><strong>Type</strong>
   </td>
   <td><strong>Mandatory</strong>
   </td>
   <td><strong>Description</strong>
   </td>
  </tr>
  <tr>
   <td>msg
   </td>
   <td>string
   </td>
   <td>Y
   </td>
   <td>Message name: “candle-query”
   </td>
  </tr>
  <tr>
   <td>ts
   </td>
   <td>timestamp [usec]
   </td>
   <td>Y
   </td>
   <td>Message generation timestamp (microseconds)
   </td>
  </tr>
  <tr>
   <td>seqn
   </td>
   <td>int64
   </td>
   <td>N
   </td>
   <td>Client's sequence number (max: 16 digits)
   </td>
  </tr>
  <tr>
   <td>start-ts
   </td>
   <td>timestamp [usec]
   </td>
   <td>Y
   </td>
   <td>Start of interval timestamp (microseconds)
   </td>
  </tr>
  <tr>
   <td>end-ts
   </td>
   <td>timestamp [usec]
   </td>
   <td>N
   </td>
   <td>End of interval timestamp (microseconds)
   </td>
  </tr>
  <tr>
   <td>interval-sec
   </td>
   <td>int32
   </td>
   <td>Y
   </td>
   <td>Interval [secs]
   </td>
  </tr>
  <tr>
   <td>symbol
   </td>
   <td>string
   </td>
   <td>Y
   </td>
   <td>Symbol
   </td>
  </tr>
  <tr>
   <td>limit
   </td>
   <td>int32
   </td>
   <td>N
   </td>
   <td>Max number of replies. Max=1000, default=500
   </td>
  </tr>
</table>

## **Kline/candlestick query reply.**
Supported intervals: 1 sec, 1 min, 1 hour, 1 day.

```
Example:

{
  "msg" : "candle-query-reply",
  "ts" : 1669031373575253,
  "seqn" : 277,
  "symbol" : "BTCUSDT",
  "interval-sec" : 60,
  "start-ts" : 1618930720000000,
  "end-ts" : 1618930730000000,
  "limit" : 100,
  "candles" : [
    {
      "start-ts" : 1669032206000000,
      "end-ts" :   1669032207000000,
      "open-px" : "20201.15",
      "last-px" : "20202.34",
      "high-px" : "20205.87",
      "low-px" : "20199.98",
      "num-trade" : 53197,
      "volume" : "3909.78543",
      "quote-volume" : "4760.48"
    },
    ...
    {
      ...
    }
  ]
}

{
  "msg" : "candle-qery-reply",
  "ts" : 1618930734676777,
  "seqn" : 277,
  "symbol" : "BTCUSDT",
  "interval-sec" : 60,
  "start-ts" : 1618930720000000,
  "end-ts" : 1618930730000000,
  "limit" : 100,
  "err-code": 91,
  "err-msg": "Undefined instrument: no symbol matches pattern"
}

```

<table>
  <tr>
   <td><strong>Name</strong>
   </td>
   <td><strong>Type</strong>
   </td>
   <td><strong>Mandatory</strong>
   </td>
   <td><strong>Description</strong>
   </td>
  </tr>
  <tr>
   <td>msg
   </td>
   <td>string
   </td>
   <td>Y
   </td>
   <td>Message name: “candle-query-reply”
   </td>
  </tr>
  <tr>
   <td>ts
   </td>
   <td>timestamp [usec]
   </td>
   <td>Y
   </td>
   <td>Message generation timestamp (microseconds)
   </td>
  </tr>
  <tr>
   <td>seqn
   </td>
   <td>int64
   </td>
   <td>N
   </td>
   <td>Back-end sequence number (max: 16 digits)
   </td>
  </tr>
  <tr>
   <td>start-ts
   </td>
   <td>timestamp [usec]
   </td>
   <td>Y
   </td>
   <td>Start of interval timestamp (microseconds)
   </td>
  </tr>
  <tr>
   <td>end-ts
   </td>
   <td>timestamp [usec]
   </td>
   <td>N
   </td>
   <td>End of interval timestamp (microseconds)
   </td>
  </tr>
  <tr>
   <td>interval-sec
   </td>
   <td>int32
   </td>
   <td>Y
   </td>
   <td>Interval [secs]
   </td>
  </tr>
  <tr>
   <td>symbol
   </td>
   <td>string
   </td>
   <td>Y
   </td>
   <td>Symbol
   </td>
  </tr>
  <tr>
   <td>limit
   </td>
   <td>int32
   </td>
   <td>N
   </td>
   <td>Max number of replies. Max=1000, default=500
   </td>
  </tr>
  <tr>
   <td>candles
   </td>
   <td>array of “candle”
   </td>
   <td>N
   </td>
   <td>Array of “candle” messages. The “ts”, “symbol” and “interval-sec” are not provided in an individual elements of array, because they are equal to the corresponding message values
   </td>
  </tr>
  <tr>
   <td>err-code
   </td>
   <td>int32
   </td>
   <td>N
   </td>
   <td>Error code
   </td>
  </tr>
  <tr>
   <td>err-msg
   </td>
   <td>string
   </td>
   <td>N
   </td>
   <td>Error message
   </td>
  </tr>
</table>

## **Instruments reference data request.**
Provides reference/static data for all instruments.

```
Example:

{
  "msg": "symbols",
  "ts": "1670341183180611",
  "seqn" : 177,
  "pattern": "BTC*"
}
```

<table>
  <tr>
   <td><strong>Name</strong>
   </td>
   <td><strong>Type</strong>
   </td>
   <td><strong>Mandatory</strong>
   </td>
   <td><strong>Description</strong>
   </td>
  </tr>
  <tr>
   <td>msg
   </td>
   <td>string
   </td>
   <td>Y
   </td>
   <td>Message name: “symbols”
   </td>
  </tr>
  <tr>
   <td>ts
   </td>
   <td>timestamp [usec]
   </td>
   <td>Y
   </td>
   <td>Client’s timestamp (microseconds)
   </td>
  </tr>
  <tr>
   <td>seqn
   </td>
   <td>int64
   </td>
   <td>N
   </td>
   <td>Client's sequence number (max: 16 digits)
   </td>
  </tr>
  <tr>
   <td>pattern
   </td>
   <td>strings
   </td>
   <td>Y
   </td>
   <td>Symbol’s pattern
   </td>
  </tr>
</table>

## **Instruments reference data reply.**
Provides reference/static data for all instruments.

```
Examples:

{
  "msg": "symbols-reply",
  "ts": 1670350617040913,
  "seqn" : 277,
  "pattern" : "BTC*",
  "symbols": [
    {
      "symbol": "BTCUSDT",
      "base": "BTC",
      "quote": "USDT",
      "type": "spot"
    }
  ]
}

{
  "msg": "symbols-reply",
  "ts": 1670350617040913,
  "seqn" : 277,
  "pattern" : "BU*",
  "err-code": 91,
  "err-msg": "Undefined instrument: no symbol matches pattern"
}
```

<table>
  <tr>
   <td><strong>Name</strong>
   </td>
   <td><strong>Type</strong>
   </td>
   <td><strong>Mandatory</strong>
   </td>
   <td><strong>Description</strong>
   </td>
  </tr>
  <tr>
   <td>msg
   </td>
   <td>string
   </td>
   <td>Y
   </td>
   <td>Message name: “symbols-reply”
   </td>
  </tr>
  <tr>
   <td>ts
   </td>
   <td>timestamp [usec]
   </td>
   <td>Y
   </td>
   <td>Message generation timestamp (microseconds)
   </td>
  </tr>
  <tr>
   <td>seqn
   </td>
   <td>int64
   </td>
   <td>N
   </td>
   <td>Back-end sequence number (max: 16 digits)
   </td>
  </tr>
  <tr>
   <td>pattern
   </td>
   <td>strings
   </td>
   <td>Y
   </td>
   <td>Symbol’s pattern
   </td>
  </tr>
  <tr>
   <td>base
   </td>
   <td>string
   </td>
   <td>Y
   </td>
   <td>Base asset
   </td>
  </tr>
  <tr>
   <td>quote
   </td>
   <td>string
   </td>
   <td>Y
   </td>
   <td>Quote asset
   </td>
  </tr>
  <tr>
   <td>type
   </td>
   <td>instrument type
   </td>
   <td>Y
   </td>
   <td>Instrument type
   </td>
  </tr>
  <tr>
   <td>err-code
   </td>
   <td>int32
   </td>
   <td>N
   </td>
   <td>Error code
   </td>
  </tr>
  <tr>
   <td>err-msg
   </td>
   <td>string
   </td>
   <td>N
   </td>
   <td>Error message
   </td>
  </tr>
</table>

## **Indicator message.**
Supported indicator types: “trend“, “liquidity“, “volatility“, “buble“.

```
Example:

{
  "msg" : "indicator",
  "type" : "trend",
  "ts" : 1669032206998413,
  "stream-seqn" : 170167,
  "seqn" : 265977,
  "symbol" : "BTCUSDT",
  "value" : "0.15"
}
```

<table>
  <tr>
   <td><strong>Name</strong>
   </td>
   <td><strong>Type</strong>
   </td>
   <td><strong>Mandatory</strong>
   </td>
   <td><strong>Description</strong>
   </td>
  </tr>
  <tr>
   <td>msg
   </td>
   <td>string
   </td>
   <td>Y
   </td>
   <td>Message name: “indicator”
   </td>
  </tr>
  <tr>
   <td>type
   </td>
   <td>string
   </td>
   <td>Y
   </td>
   <td>indicator type: “trend“, “liquidity“, “volatility“, “buble“.
   </td>
  </tr>
  <tr>
   <td>ts
   </td>
   <td>timestamp [usec]
   </td>
   <td>Y
   </td>
   <td>Message generation timestamp (microseconds)
   </td>
  </tr>
  <tr>
   <td>stream-seqn
   </td>
   <td>int64
   </td>
   <td>N
   </td>
   <td>Market data stream sequence number (max: 16 digits)
   </td>
  </tr>
  <tr>
   <td>seqn
   </td>
   <td>int64
   </td>
   <td>N
   </td>
   <td>Back-end sequence number (max: 16 digits)
   </td>
  </tr>
  <tr>
   <td>symbol
   </td>
   <td>string
   </td>
   <td>Y
   </td>
   <td>Symbol
   </td>
  </tr>
  <tr>
   <td>value
   </td>
   <td>double
   </td>
   <td>Y
   </td>
   <td>Indicator value
   </td>
  </tr>
</table>

## **Error message.**

```
Examples:

{
  "msg" : "error",
  "ts" : 1618930734676777,
  "seqn" : 277,
  "err-code": 46,
  "err-msg": "Missing mandatory field: symbol",
  "ref-msg" : "candle-query",
  "ref-seqn" : 177
}
```

<table>
  <tr>
   <td><strong>Name</strong>
   </td>
   <td><strong>Type</strong>
   </td>
   <td><strong>Mandatory</strong>
   </td>
   <td><strong>Description</strong>
   </td>
  </tr>
  <tr>
   <td>msg
   </td>
   <td>string
   </td>
   <td>Y
   </td>
   <td>Message type: “error”
   </td>
  </tr>
  <tr>
   <td>ts
   </td>
   <td>timestamp [usec]
   </td>
   <td>Y
   </td>
   <td>Message generation timestamp (microseconds)
   </td>
  </tr>
  <tr>
   <td>seqn
   </td>
   <td>int64
   </td>
   <td>N
   </td>
   <td>Back-end sequence number (max: 16 digits)
   </td>
  </tr>
  <tr>
   <td>err-code
   </td>
   <td>int32
   </td>
   <td>N
   </td>
   <td>Error code
   </td>
  </tr>
  <tr>
   <td>err-msg
   </td>
   <td>string
   </td>
   <td>N
   </td>
   <td>Error message
   </td>
  </tr>
  <tr>
   <td>ref-msg
   </td>
   <td>string
   </td>
   <td>N
   </td>
   <td>Referenced message type
   </td>
  </tr>
  <tr>
   <td>ref-seqn
   </td>
   <td>int64
   </td>
   <td>N
   </td>
   <td>Referenced client's sequence number (max: 16 digits)
   </td>
  </tr></table>

## **Types.**


* price: double rounded by tick size and converted to string ("33.65")
* quantity: double rounded by lot size and converted to string ("197.")
* price level: tuple of [price, quantity, number of orders] (["33.91","43.",5])
* price level update: tuple of [command: new(“n”), delete(“d”), change(“c”), level, price, quantity, number of orders), value] (["c", 3, "17567.34","2.01235",4])
* timestamp: time since epoch in microseconds (1669032206998414)
* int64: limited by 16 digits

## **Instrument type (as string).**


* spot
