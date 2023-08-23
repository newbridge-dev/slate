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
Date: 2023-06-30 \
Version: 1.0


## **Overview**
All market data messages are delivered on subscriptions via a web socket with /ws path. The instrument reference data messages are delivered as a query reply. A subscription key consists of a stream (message type) and a symbol’s pattern, which supports the “*” wildcard (BTCUSDT, BTC*, “*”, etc). The wildcard allows to subscribe for a range of instruments by a single subscription key. All message timestamps have the nanoseconds resolution. All active client’s subscriptions are cancelled on a client’s web socket disconnection. The web socket server sends a ping frame every 5 minutes. If the web socket server does not receive a pong frame within 15 minutes, the web socket connection is closed. The unsolicited pong frames are allowed. For the instrument reference data subscription normally only in a single reply is sent. The market data/instrument reference data web socket end point is not authenticated (ws).


## **Subscription request.**
A subscription request can send only a single subscription key.

```
Examples:

{
  "msg" : "Subscription",
  "ts" : 1669032206385873,
  "seqn" : 177,
  "op" : "sub",
  "reqId" : 1,
  "stream" : "BestBidAsk",
  "pattern" : "BTC*"
}

{
  "msg" : "Subscription",
  "ts" : 1669032206385873,
  "seqn" : 177,
  "op" : "unsub",
  "stream" : "OrderbookSnapshot",
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
   <td>Message type: “Subscription”
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
   <td>Y
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
   <td>reqId
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
   <td>interval
   </td>
   <td>int32
   </td>
   <td>N
   </td>
   <td>Interval (Kline subscription only), available intervals [sec]: 1, 60, 3600, 86400
   </td>
  </tr>
  <tr>
   <td>depth
   </td>
   <td>int32
   </td>
   <td>N
   </td>
   <td>Order book snapshot depth (OrderbookSnapshot subscription only): 5, 10, 20
   </td>
  </tr>
</table>

## **Subscription reply.**
A subscription reply may indicate the success or error.

```
Examples:

{
  "msg" : "SubscriptionReply",
  "ts" : 1669032206385873,
  "seqn" : 277,
  "reqId" : 1,
  "op" : "sub",
  "stream" : "OrderbookSnapshot",
  "pattern" : "BTCU*",
  "depth" : 20,
  "result" : "success”
}

{
  "msg" : "SubscriptionReply",
  "ts" : 1669032206385873,
  "seqn" : 277,
  "op" : "unsub",
  "stream" : "Kline",
  "pattern" : "BTCUS",
  "interval" : 3600,
  "result" : "error",
  "errCode" : 48,
  "errMessage" : "Invalid subscription: pattern=BTCUS"
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
   <td>Message type: “SubscriptionReply”
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
   <td>Y
   </td>
   <td>Exchange sequence number (max: 16 digits)
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
   <td>reqId
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
   <td>interval
   </td>
   <td>int32
   </td>
   <td>N
   </td>
   <td>Interval (Kline subscription only), available intervals [sec]: 1, 60, 3600, 86400
   </td>
  </tr>
  <tr>
   <td>depth
   </td>
   <td>int32
   </td>
   <td>N
   </td>
   <td>Order book snapshot depth (OrderbookSnapshot subscription only): 5, 10, 20
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
   <td>errCode
   </td>
   <td>int32
   </td>
   <td>N
   </td>
   <td>Error code
   </td>
  </tr>
  <tr>
   <td>errMessage
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
  "msg" : "BestBidAsk",
  "ts" : 1669026845496043,
  "streamSeqn" : 10167,
  "seqn" : 16716,
  "symbol" : "BTCUSDT",
  "bidPrice" : "17567.34",
  "bidQty" : "5.76345",
  "askPrice" : "17567.44",
  "askQty" : "4.18456"
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
   <td>Message name: “BestBidAsk”
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
   <td>streamSeqn
   </td>
   <td>int64
   </td>
   <td>Y
   </td>
   <td>Market data stream sequence number (max: 16 digits)
   </td>
  </tr>
  <tr>
   <td>seqn
   </td>
   <td>int64
   </td>
   <td>Y
   </td>
   <td>Exchange sequence number (max: 16 digits)
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
   <td>bidPrice
   </td>
   <td>price
   </td>
   <td>N
   </td>
   <td>Best bid price
   </td>
  </tr>
  <tr>
   <td>bidQty
   </td>
   <td>quantity
   </td>
   <td>N
   </td>
   <td>Best bid quantity
   </td>
  </tr>
  <tr>
   <td>askPrice
   </td>
   <td>price
   </td>
   <td>N
   </td>
   <td>Best ask price
   </td>
  </tr>
  <tr>
   <td>askQty
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
  "msg" : "Trade",
  "ts" : 1669030387918824,
  "streamSeqn" : 170167,
  "seqn" : 180444,
  "symbol" : "BTCUSDT",
  "tradeId" : "6690303879188244",
  "tradePrice" : "17567.34",
  "tradeQty" : "1.23978",
  "makerBuyer" : true
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
   <td>Message name: “Trade”
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
   <td>streamSeqn
   </td>
   <td>int64
   </td>
   <td>Y
   </td>
   <td>Market data stream sequence number (max: 16 digits)
   </td>
  </tr>
  <tr>
   <td>seqn
   </td>
   <td>int64
   </td>
   <td>Y
   </td>
   <td>Exchange sequence number (max: 16 digits)
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
   <td>tradeId
   </td>
   <td>int64
   </td>
   <td>Y
   </td>
   <td>Unique trade id (max: 16 digits)
   </td>
  </tr>
  <tr>
   <td>tradePrice
   </td>
   <td>price
   </td>
   <td>Y
   </td>
   <td>Trade price
   </td>
  </tr>
  <tr>
   <td>tradeQty
   </td>
   <td>quantity
   </td>
   <td>Y
   </td>
   <td>Trade quantity
   </td>
  </tr>
  <tr>
   <td>makerBuyer
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
  "msg" : "OrderbookSnapshot",
  "ts" : 1669030575242078,
  "streamSeqn" : 170167,
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
  "msg" : "OrderbookSnapshot",
  "ts" : 1669030575242078,
  "streamSeqn" : 170167,
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
   <td>Message name: “OrderbookSnapshot”
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
   <td>streamSeqn
   </td>
   <td>int64
   </td>
   <td>Y
   </td>
   <td>Market data stream sequence number (max: 16 digits)
   </td>
  </tr>
  <tr>
   <td>seqn
   </td>
   <td>int64
   </td>
   <td>Y
   </td>
   <td>Exchange sequence number (max: 16 digits)
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
  "msg" : "OrderbookUpdate",
  "ts" : 1669031373575253,
  "streamSeqn" : 170167,
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
  "msg" : "OrderbookUpdate",
  "ts" : 1669031373575253,
  "streamSeqn" : 170167,
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
   <td>Message name: “OrderbookUpdate”
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
   <td>streamSeqn
   </td>
   <td>int64
   </td>
   <td>Y
   </td>
   <td>Market data stream sequence number (max: 16 digits)
   </td>
  </tr>
  <tr>
   <td>seqn
   </td>
   <td>int64
   </td>
   <td>Y
   </td>
   <td>Exchange sequence number (max: 16 digits)
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
   <td>snapshotFirst
   </td>
   <td>boolean
   </td>
   <td>N
   </td>
   <td>First snapshot message in a snapshot sequence
   </td>
  </tr>
  <tr>
   <td>snapshotLast
   </td>
   <td>boolean
   </td>
   <td>N
   </td>
   <td>Last snapshot message in a snapshot sequence
   </td>
  </tr>
</table>

## **Ticker message.**
24 hours sliding window ticker message. The interval is 1 sec.

```
Example:

{
  "msg" : "Ticker",
  "ts" : 1669032206998413,
  "streamSeqn" : 170167,
  "seqn" : 265977,
  "interval" : 1,
  "symbol" : "BTCUSDT",
  "openPrice" : "20201.15",
  "lastPrice" : "20202.34",
  "highPrice" : "20205.87",
  "lowPrice" : "20199.98",
  "vwap" : "20202.78",
  "numTrades" : 53197,
  "volume" : "3909.78543",
  "quoteVolume" : "4760.48",
  "avgQty" : "0.19852",
  "lastQty" : "3.00329"
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
   <td>Message name: “Ticker”
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
   <td>streamSeqn
   </td>
   <td>int64
   </td>
   <td>Y
   </td>
   <td>Market data stream sequence number (max: 16 digits)
   </td>
  </tr>
  <tr>
   <td>seqn
   </td>
   <td>int64
   </td>
   <td>Y
   </td>
   <td>Exchange sequence number (max: 16 digits)
   </td>
  </tr>
  <tr>
   <td>interval
   </td>
   <td>int32
   </td>
   <td>Y
   </td>
   <td>Message generation interval [secs]
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
   <td>openPrice
   </td>
   <td>price
   </td>
   <td>Y
   </td>
   <td>Open traded price
   </td>
  </tr>
  <tr>
   <td>LastPrice
   </td>
   <td>price
   </td>
   <td>Y
   </td>
   <td>Last/close traded price
   </td>
  </tr>
  <tr>
   <td>highPrice
   </td>
   <td>price
   </td>
   <td>Y
   </td>
   <td>Highest traded  price
   </td>
  </tr>
  <tr>
   <td>lowPrice
   </td>
   <td>price
   </td>
   <td>Y
   </td>
   <td>Lowest traded  price
   </td>
  </tr>
  <tr>
   <td>vwap
   </td>
   <td>price
   </td>
   <td>Y
   </td>
   <td>Volume weighed average price
   </td>
  </tr>
  <tr>
   <td>numTrades
   </td>
   <td>int32
   </td>
   <td>Y
   </td>
   <td>Number of trades
   </td>
  </tr>
  <tr>
   <td>volume
   </td>
   <td>quantity
   </td>
   <td>Y
   </td>
   <td>Total traded volume
   </td>
  </tr>
  <tr>
   <td>quoteVolume
   </td>
   <td>price
   </td>
   <td>Y
   </td>
   <td>Total traded quote volume
   </td>
  </tr>
  <tr>
   <td>avgQty
   </td>
   <td>quantity
   </td>
   <td>N
   </td>
   <td>Average traded quantity
   </td>
  </tr>
  <tr>
   <td>lastQty
   </td>
   <td>quantity
   </td>
   <td>N
   </td>
   <td>Last trades quantity
   </td>
  </tr>
</table>

## **Kline/candlestick message.**
Supported intervals: 1 sec, 1 min, 1 hour and 1 day, which must be specified in the subscription request's "interval" field.

```
Example:

{
  "msg" : "Kline",
  "ts" : 1669032206998413,
  "streamSeqn" : 170167,
  "seqn" : 265977,
  "startTs" : 1669032206000000,
  "endTs" :   1669032207000000,
  "interval" : 60,
  "symbol" : "BTCUSDT",
  "openPrice" : "20201.15",
  "lastPrice" : "20202.34",
  "highPrice" : "20205.87",
  "lowPrice" : "20199.98",
  "numTrades" : 53197,
  "volume" : "3909.78543",
  "quoteVolume" : "4760.48"
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
   <td>Message name: “Kline”
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
   <td>streamSeqn
   </td>
   <td>int64
   </td>
   <td>Y
   </td>
   <td>Market data stream sequence number (max: 16 digits)
   </td>
  </tr>
  <tr>
   <td>seqn
   </td>
   <td>int64
   </td>
   <td>Y
   </td>
   <td>Exchange sequence number (max: 16 digits)
   </td>
  </tr>
  <tr>
   <td>startTs
   </td>
   <td>timestamp [usec]
   </td>
   <td>Y
   </td>
   <td>Start of interval timestamp (microseconds)
   </td>
  </tr>
  <tr>
   <td>endTs
   </td>
   <td>timestamp [usec]
   </td>
   <td>Y
   </td>
   <td>End of interval timestamp (microseconds)
   </td>
  </tr>
  <tr>
   <td>interval
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
   <td>openPrice
   </td>
   <td>price
   </td>
   <td>N
   </td>
   <td>Open traded price
   </td>
  </tr>
  <tr>
   <td>LastPrice
   </td>
   <td>price
   </td>
   <td>N
   </td>
   <td>Last/close traded price
   </td>
  </tr>
  <tr>
   <td>highPrice
   </td>
   <td>price
   </td>
   <td>N
   </td>
   <td>Highest traded  price
   </td>
  </tr>
  <tr>
   <td>lowPrice
   </td>
   <td>price
   </td>
   <td>N
   </td>
   <td>Lowest traded  price
   </td>
  </tr>
  <tr>
   <td>numTrades
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
   <td>quoteVolume
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
  "msg" : "KlineQuery",
  "ts" : 1669032206998413,
  "seqn" : 177,
  "startTs" : 1669032206000000,
  "endTs" :   1669032207000000,
  "interval" : 60,
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
   <td>Message name: “KlineQuery”
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
   <td>Y
   </td>
   <td>Client's sequence number (max: 16 digits)
   </td>
  </tr>
  <tr>
   <td>startTs
   </td>
   <td>timestamp [usec]
   </td>
   <td>Y
   </td>
   <td>Start of interval timestamp (microseconds)
   </td>
  </tr>
  <tr>
   <td>endTs
   </td>
   <td>timestamp [usec]
   </td>
   <td>N
   </td>
   <td>End of interval timestamp (microseconds)
   </td>
  </tr>
  <tr>
   <td>interval
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
  "msg" : "KlineQueryReply",
  "ts" : 1669031373575253,
  "seqn" : 277,
  "symbol" : "BTCUSDT",
  "interval" : 60,
  "startTs" : 1618930720000000,
  "endTs" : 1618930730000000,
  "limit" : 100,
  "klines" : [
    {
      "startTs" : 1669032206000000,
      "endTs" :   1669032207000000,
      "openPrice" : "20201.15",
      "lastPrice" : "20202.34",
      "highPrice" : "20205.87",
      "lowPrice" : "20199.98",
      "numTrades" : 53197,
      "volume" : "3909.78543",
      "quoteVolume" : "4760.48"
    },
    ...
    {
      ...
    }
  ]
}

{
  "msg" : "KlineQueryReply",
  "ts" : 1618930734676777,
  "seqn" : 277,
  "symbol" : "BTCUSDT",
  "interval" : 60,
  "startTs" : 1618930720000000,
  "endTs" : 1618930730000000,
  "limit" : 100,
  "errCode": 91,
  "errMessage": "Undefined instrument: no symbol matches pattern"
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
   <td>Message name: “KlineQueryReply”
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
   <td>Y
   </td>
   <td>Exchange sequence number (max: 16 digits)
   </td>
  </tr>
  <tr>
   <td>startTs
   </td>
   <td>timestamp [usec]
   </td>
   <td>Y
   </td>
   <td>Start of interval timestamp (microseconds)
   </td>
  </tr>
  <tr>
   <td>endTs
   </td>
   <td>timestamp [usec]
   </td>
   <td>N
   </td>
   <td>End of interval timestamp (microseconds)
   </td>
  </tr>
  <tr>
   <td>interval
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
   <td>klines
   </td>
   <td>array of “Kline”
   </td>
   <td>N
   </td>
   <td>Array of “Kline” messages. The “ts”, “symbol” and “interval” are not provided in an individual elements of array, because they are equal to the corresponding message values
   </td>
  </tr>
  <tr>
   <td>errCode
   </td>
   <td>int32
   </td>
   <td>N
   </td>
   <td>Error code
   </td>
  </tr>
  <tr>
   <td>errMessage
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
  "msg": "Symbols",
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
   <td>Message name: “Symbols”
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
   <td>Y
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
  "msg": "SymbolsReply",
  "ts": 1670350617040913,
  "seqn" : 277,
  "pattern" : "BTC*",
  "symbols": [
    {
      "symbol": "BTCUSDT",
      "base": "BTC",
      "quote": "USDT",
      "tickSize": "0.01",
      "lotSize": "0.00001",
      "minNotional": "10.0",
      "minQty": "0.00001",
      "maxQty": "100000.0",
      "maxOrders": 200,
      "type": "spot"
    }
  ]
}

{
  "msg": "SymbolsReply",
  "ts": 1670350617040913,
  "seqn" : 277,
  "pattern" : "BU*",
  "errCode": 91,
  "errMessage": "Undefined instrument: no symbol matches pattern"
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
   <td>Message name: “SymbolsReply”
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
   <td>Y
   </td>
   <td>Exchange sequence number (max: 16 digits)
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
   <td>tickSize
   </td>
   <td>double
   </td>
   <td>Y
   </td>
   <td>Tick size
   </td>
  </tr>
  <tr>
   <td>lotSize
   </td>
   <td>double
   </td>
   <td>Y
   </td>
   <td>Lot size
   </td>
  </tr>
  <tr>
   <td>minNotional
   </td>
   <td>price
   </td>
   <td>Y
   </td>
   <td>Min order notional value
   </td>
  </tr>
  <tr>
   <td>maxQty
   </td>
   <td>quantity
   </td>
   <td>Y
   </td>
   <td>Max order quantity
   </td>
  </tr>
  <tr>
   <td>minQty
   </td>
   <td>quantity
   </td>
   <td>Y
   </td>
   <td>Min order quantity
   </td>
  </tr>
  <tr>
   <td>maxOrders
   </td>
   <td>int32
   </td>
   <td>Y
   </td>
   <td>Max open orders for a client
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
   <td>errCode
   </td>
   <td>int32
   </td>
   <td>N
   </td>
   <td>Error code
   </td>
  </tr>
  <tr>
   <td>errMessage
   </td>
   <td>string
   </td>
   <td>N
   </td>
   <td>Error message
   </td>
  </tr>
</table>

## **Error message.**

```
Examples:

{
  "msg" : "Error",
  "ts" : 1618930734676777,
  "seqn" : 277,
  "errCode": 46,
  "errMessage": "Missing mandatory field: symbol",
  "refMsg" : "KlineQuery",
  "refSeqn" : 177
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
   <td>Message type: “Error”
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
   <td>Y
   </td>
   <td>Exchange sequence number (max: 16 digits)
   </td>
  </tr>
  <tr>
   <td>errCode
   </td>
   <td>int32
   </td>
   <td>N
   </td>
   <td>Error code
   </td>
  </tr>
  <tr>
   <td>errMessage
   </td>
   <td>string
   </td>
   <td>N
   </td>
   <td>Error message
   </td>
  </tr>
  <tr>
   <td>refMsg
   </td>
   <td>string
   </td>
   <td>N
   </td>
   <td>Referenced message type
   </td>
  </tr>
  <tr>
   <td>refSeqn
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
