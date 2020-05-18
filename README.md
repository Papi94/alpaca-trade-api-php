This is a fork of JeffreyHyer/alpaca-trade-api-php, i needed to remove external dependancies so that i can use this api in PeachPie PHP. Luckily its easy enough to use curl to replace guzzle and i removed the universal date formatter as well. Warning, this might break things for you UK guys, maybe...maybe not, i am not sure. 

# Alpaca PHP SDK

![Packagist Version](https://img.shields.io/packagist/v/jeffreyhyer/alpaca-trade-api-php?label=Packagist)
![Packagist](https://img.shields.io/packagist/dt/jeffreyhyer/alpaca-trade-api-php?color=blue&label=Downloads)

This repository contains a PHP SDK for use with the
[Alpaca](https://alpaca.markets?ref_by=858915e73e) API.

**DISCLAIMER:** This is **NOT** an official SDK, it is not affiliated
with nor endorsed by Alpaca in any way.

With the release of `v2.0.0` of this library we use v2 of the Alpaca API.
However, the methods are all backwards compatible with v1.0.0 of this
library and v1 of the Alpaca API so upgrading should be as simple as
updating the package version in your `composer.json` file and installing.
Everything should work as it did before but now you'll have access to
the new methods and new method parameters.

## Installation

See example.php this fork removes the need for compaser packages. 

## Usage

See the example.php

## OAuth

This fork removes oauth support, i dont need it, shouldnt be too hard to add back in , just a matter of adding an authorization header to pass the bearer token.

## API

### Constructor

To get started initialize the `Alpaca` class with your Alpaca Key and
Secret. You can also set the third parameter to enable or disable
paper trading. The default is `true` to enable calling against the
paper trading endpoint.

```php
use Alpaca\Alpaca;

$alpaca = new Alpaca("KEY", "SECRET", true);

// This call will now work as expected if your KEY and SECRET are valid.
// If not, the response will contain an error explaining what went wrong.
$resp = $alpaca->getAccount();
```

You can change these values after initialization if necessary using
the following methods:

**`setKey($key)`**

Set your Alpaca Key

**`setSecret($secret)`**

Set your Alpaca Secret

**`setPaper(true)`**

Enable or disable paper trading. `true` = Paper Trading, `false` =
Live Trading.

---

### Response

This fork breaks this stuff. :)

All methods return an instance of the `\Alpaca\Response` class which
has a number of convenient methods for working with the API response.

**`getCode()`**

Returns the HTTP status code of the request (e.g. `200`, `403`, etc).

**`getReason()`**

Returns the HTTP status reason of the request (e.g. `OK`, `Forbidden`,
etc).

**`getResponse()`**

Returns the JSON decoded response. For example:

```php
print_r($alpaca->getAccount()->getResponse());

/*
Results in:

stdClass Object
(
    [id] => null
    [status] => ACTIVE
    [currency] => USD
    [buying_power] => 25000
    [cash] => 25000
    [cash_withdrawable] => 0
    [portfolio_value] => 25000
    [pattern_day_trader] => 
    [trading_blocked] => 
    [transfers_blocked] => 
    [account_blocked] => 
    [created_at] => 2018-11-01T18:41:35.990779Z
    [trade_suspended_by_user] => 
)
*/
```

---

### Account

:ledger: [Alpaca Docs](https://docs.alpaca.markets/api-documentation/api-v2/account/)

**`getAccount()`**

Returns the account associated with the API key.

---

### Orders

:ledger: [Alpaca Docs](https://docs.alpaca.markets/api-documentation/api-v2/orders/)

**`getOrders($status = null, $limit = null, $after = null, $until = null, $direction = null, $nested = null)`**

Retrieves a list of orders for the account, optionally filtered by the
supplied query parameters.

**`createOrder($symbol, $qty, $side, $type, $time_in_force, $limit_price = null, $stop_price = null, $client_order_id = null)`**

Places a new order for the given account. An order request may be
rejected if the account is not authorized for trading, or if the
tradable balance is insufficient to fill the order.

**`getOrder($order_id)`**

Retrieves a single order for the given `$order_id`.

**`getOrderByClientId($client_order_id)`**

Retrieves a single order for the given `$client_order_id`.

**`replaceOrder($order_id, $qty, $time_in_force, $limit_price = null, $stop_price = null, $client_order_id = null)`**

Replaces a single order with updated parameters. Each parameter
overrides the corresponding attribute of the existing order. The other
attributes remain the same as the existing order.

**`cancelOrder($order_id)`**

Attempts to cancel an open order. If the order is no longer cancelable
(example: `status=order_filled`), the server will respond with status
422, and reject the request.

**`cancelAllOrders()`**

Attempts to cancel all open orders. A response will be provided for
each order that is attempted to be cancelled. If an order is no
longer cancelable, the server will respond with status 500 and reject
the request.

---

### Positions

:ledger: [Alpaca Docs](https://docs.alpaca.markets/api-documentation/api-v2/positions/)

**`getPositions()`**

Retrieves a list of the account's open positions.

**`getPosition($symbol)`**

Retrieves the account's open position for the given `$symbol`.

**`closeAllPositions()`**

Closes (liquidates) all of the account’s open long and short positions.
A response will be provided for each order that is attempted to be
cancelled. If an order is no longer cancelable, the server will respond
with status 500 and reject the request.

**`closePosition($symbol)`**

Closes (liquidates) the account’s open position for the given `$symbol`.
Works for both long and short positions.

---

### Assets

:ledger: [Alpaca Docs](https://docs.alpaca.markets/api-documentation/api-v2/assets/)

**`getAssets($status = null, $asset_class = null)`**

Get a list of assets

**`getAsset($symbol)`**

Get an asset for the given `$symbol`.

**`getAssetById($id)`**

Get an asset for the given `$id`.

---

### Watchlists

:ledger: [Alpaca Docs](https://docs.alpaca.markets/api-documentation/api-v2/watchlist/)

**`getWatchlists()`**

Returns the list of watchlists registered under the account.

**`createWatchlist($name, $symbols = [])`**

Create a new watchlist with initial set of assets (`$symbols`). The
`$name` is used to identify the watchlist in any of the `*WatchlistByName()`
methods below.

**`getWatchlist($id)`**

Returns a watchlist identified by the `$id`.

**`getWatchlistByName($name)`**

Returns a watchlist identified by the `$name`.

**`updateWatchlist($id, $name, $symbols = [])`**

Update the name and/or content of the watchlist identified by `$id`.

**`updateWatchlistByName($name, $symbols = [])`**

Update the name and/or content of the watchlist identified by `$name`.

**`addAssetToWatchlist($id, $symbol)`**

Append an asset for the `$symbol` to the end of watchlist asset list.

**`addAssetToWatchlistByName($name, $symbol)`**

Append an asset for the `$symbol` to the end of watchlist asset list.

**`deleteWatchlist($id)`**

Delete a watchlist. This is a permanent deletion.

**`deleteWatchlistByName($name)`**

Delete a watchlist. This is a permanent deletion.

---

### Calendar

:ledger: [Alpaca Docs](https://docs.alpaca.markets/api-documentation/api-v2/calendar/)

**`getCalendar($start = null, $end = null)`**

Returns the market calendar.

---

### Clock

:ledger: [Alpaca Docs](https://docs.alpaca.markets/api-documentation/api-v2/clock/)

**`getClock()`**

Returns the market clock.

---

### Account Configurations

:ledger: [Alpaca Docs](https://docs.alpaca.markets/api-documentation/api-v2/account-configuration/)

**`getAccountConfigurations()`**

Returns the current account configuration values.

**`updateAccountConfigurations($config = [])`**

Updates and returns the current account configuration values.
`$config` is an array of key-value pairs (e.g. `["key" => "value"]`.

---

### Account Activities

:ledger: [Alpaca Docs](https://docs.alpaca.markets/api-documentation/api-v2/account-activities/)

**`getAccountActivitiesOfType($type, $date = null, $until = null, $after = null, $direction = null, $page_size = null, $page_token = null)`**

Returns account activity entries for a specific type of activity.

**`getAccountActivities($types = [])`**

Returns account activity entries for many types of activities.

---

### Portfolio History

:ledger: [Alpaca Docs](https://docs.alpaca.markets/api-documentation/api-v2/portfolio-history/)

**`getPortfolioHistory($period = null, $timeframe = null, $date_end = null, $extended_hours = null)`**

Returns timeseries data about equity and profit/loss (P/L) of the
account in requested timespan.

---

### Market Data

:ledger: [Alpaca Docs](https://docs.alpaca.markets/api-documentation/api-v2/market-data/bars)

**`getBars($timeframe, $symbols, $limit = null, $start = null, $end = null, $after = null, $until = null)`**

Retrieves a list of bars for each requested symbol. It is guaranteed
all bars are in ascending order by time. Currently, no “incomplete”
bars are returned. For example, a 1 minute bar for 09:30 will not be
returned until 09:31.

**`getLastTrade($symbol)`**

Retrieve the last trade for the requested symbol.

**`getLastQuote($symbol)`**

Retrieves the last quote for the requested symbol.
