# bitprices

A command-line tool that generates a report of transactions with the USD (fiat)
value on the date of each transaction.

Hopefully one day soon all wallet software will include reporting that includes
the exchange rate at the time of each transaction.   Until that time, this
tool may fill a gap.  Also, it can be used for addresses outside of your own
wallet.

This tool is useful for determining USD values for all transactions associated
with one or more bitcoin addresses for an arbitrary range of dates. Such
information can be useful when determining cost basis for tax reporting or other
purposes.

If all addresses from a given wallet are provided (including change addresses)
then the tool can provide a full and complete report of all wallet transactions.

Daily exchange rates are obtained from bitcoinaverage.com.  All fiat currencies
supported by bitcoinaverage.com may be reported, not only USD.

Historic transaction data for each address is obtained from a blockchain API
service provider, which can be either a third party service or something you
run locally.

At present, the supported blockchain APIs are:
* toshi:  online service at toshi.io, or run locally.
* insight: online service at insight.bitpay.com, or run locally.
* btcd: typically this is run locally.

Any public address or set of addresses may be reported on.

# Use at your own risk.

The author makes no claims or guarantees of correctness.

# Pricing Granularity

Exchange rates are based on the average *Daily* rate from bitcoinaverage.com.
Therefore, the exact price at the moment of the transaction is not reflected.

For transactions that occurred "Today", the latest 24 hour average value from
bitcoinaverage.com is used.

# Example Report

This report was obtained with the following command, using default columns:

./bitprices.php --addresses=1M8s2S5bgAzSSzVTeL7zruvMPLvzSkEAuv --outfile=/tmp/report.txt -g
```
+------------+----------+------------+-----------------+-----------------+-----------------+------------+------------+-------------+-----------+
| Date       | Time     | Addr Short | BTC In          | BTC Out         | BTC Balance     | USD In     | USD Out    | USD Balance | USD Price |
+------------+----------+------------+-----------------+-----------------+-----------------+------------+------------+-------------+-----------+
| 2011-11-16 | 05:59:08 | 1M8..Auv   | 500000.00000000 |      0.00000000 | 500000.00000000 | 1230000.00 |       0.00 |  1230000.00 |      2.46 |
| 2011-11-16 | 09:17:36 | 1M8..Auv   |      0.00000000 | 500000.00000000 |      0.00000000 |       0.00 | 1230000.00 |        0.00 |      2.46 |
| 2013-11-26 | 16:36:25 | 1M8..Auv   |      0.00011000 |      0.00000000 |      0.00011000 |       0.10 |       0.00 |        0.10 |    913.95 |
| 2013-11-26 | 17:37:51 | 1M8..Auv   |      0.00000000 |      0.00011000 |      0.00000000 |       0.00 |       0.10 |        0.00 |    913.95 |
| 2014-11-21 | 22:24:05 | 1M8..Auv   |      0.00010000 |      0.00000000 |      0.00010000 |       0.04 |       0.00 |        0.04 |    351.95 |
| 2014-12-09 | 17:55:09 | 1M8..Auv   |      0.00889387 |      0.00000000 |      0.00899387 |       3.15 |       0.00 |        3.18 |    353.67 |
| 2015-06-05 | 15:05:14 | 1M8..Auv   |      0.44520000 |      0.00000000 |      0.45419387 |     100.62 |       0.00 |      103.80 |    226.01 |
| 2015-06-07 | 18:31:53 | 1M8..Auv   |      0.44917576 |      0.00000000 |      0.90336963 |     101.52 |       0.00 |      205.32 |    226.02 |
+------------+----------+------------+-----------------+-----------------+-----------------+------------+------------+-------------+-----------+
```

note: This address was chosen for the example because it is a well known address
listed on theopenledger.com as having the largest transaction ever. Also, the
number of transactions is small, making it a good size for an example.

http://www.theopenledger.com/9-most-famous-bitcoin-addresses/

# A note about Fiat Columns

The columns "USD In", "USD Out" and "USD Balance" are based on the price at the
time of each transaction.  So these amounts are typically very different than
they would be if each transaction were valued at today's exchange rate.

The USD Balance column provides a good picture of net income valued in dollars
and unaffected by exchange rate fluctuations.

note: The actual column names will vary by currency, eg "EUR Balance" for Euros.


# Output formats

The report may be printed in the following formats:
* plain  - an ascii formatted table, as above.  intended for humans.
* csv - CSV format.  For spreadsheet programs.
* json - raw json format.  for programs to read easily.
* jsonpretty - pretty json format.  for programs or humans.

Additionally, the report may contain incoming transactions only, outgoing
transactions only, or both types.

# Usage
   bitprices.php

   This script generates a report of transactions with the USD value
   at the time of each transaction.

   Options:

    -g                   go!
    
    --addresses=<csv>    comma separated list of bitcoin addresses
    --addressfile=<path> file containing bitcoin addresses, one per line.
    
    --api=<api>          toshi|btcd|insight.   default = insight.
    
    --direction=<dir>    transactions in | out | both   default = both.
    
    --date_start=<date>  Look for transactions since date. default = all.
    --date_end=<date>    Look for transactions until date. default = now.
    
    --currency=<curr>    symbol supported by bitcoinaverage.com.  default = USD.
    
    --cols=<cols>        default=date,time,addrshort,btcin,btcout,btcbalance
                                 fiatin,fiatout,fiatbalance,fiatprice
                         others=address,tx,txshort
                                btcbalanceperiod,fiatbalanceperiod
                                
    --outfile=<path>     specify output file path.
    --format=<format>    txt|csv|json|jsonpretty|html|all     default=txt
    
                         if all is specified then a file will be created
                         for each format with appropriate extension.
                         only works when outfile is specified.
                         
    --toshi=<url>       toshi server. defaults to https://bitcoin.toshi.io
    --toshi-fast        if set, toshi server supports filtered transactions.
    
    --btcd-rpc-host=<h> btcd rpc host.  default = 127.0.0.1
    --btcd-rpc-port=<p> btcd rpc port.  default = 8334
    --btcd-rpc-user=<u> btcd rpc username.
    --btcd-rpc-pass=<p> btcd rpc password.
    
    --insight=<url>     insight server. defaults to https://insight.bitpay.com
    
    --addr-tx-limit=<n> per address transaction limit. default = 1000
    --testnet           use testnet. only affects addr validation.


# Exporting addresses from wallets.

Every bitcoin wallet has its own method for exporting addresses.  Some make it
easy, some require digging into hidden files, or manually copying and pasting.

The key thing to keep in mind is that in order for the report generated by this
tool to match up with your wallet's transaction history and balance, you must
provide *all* wallet addresses that have received or spent funds --
including change addresses.

## Bitcoin Core.

Exporting addresses from Bitcoin Core is relatively simple.

```
bitcoin-cli listaddressgroupings | grep '",'
```

See:
* http://bitcoin.stackexchange.com/questions/16913/where-can-i-see-my-bitcoin-addresses
* http://bitcoin.stackexchange.com/questions/9077/how-to-get-all-addresses-including-the-change-addresses-from-bitcoind

## Other wallets.

Please add instructions here.  ( pull request )


# Requirements

PHP command-line interpreter installed in your path.  version 5.5.9 and above.

may work with lower versions.

# Installation and Running.

## Unix/mac
```
 git clone or download/unzip file.
 chmod +x bitprices.php.  ( if necessary )
 ./bitprices.php
```
## Windows
```
 git clone or download/unzip file.
 php ./bitprices.php
```

# Blockchain API provider notes.

tip!  use the --api flag to switch between blockchain API providers.

Each API has strengths and weaknesses. Some are faster than others,
or easier/harder to run locally. For first time usage, the online toshi
service is recommended, and it is the default.

At present, running the forked version of btcd locally seems to be the best
option for fastest report generation.

## Toshi

as of v0.1.0

* Fast for transactions with few inputs/outputs
* does not support filtering unrelated inputs/outputs.   takes minutes and
  generates huge results.
* when running locally, it is very slow to sync blockchain (months) unless the
database is stored on SSD drive.
* does not support querying multiple addresses at once.

### Toshi Fork

This tool calls a toshi (http://toshi.io) API to list all the transactions
for each address.  This toshi API is slow for two reasons:

* Toshi returns all inputs and outputs for each transaction although we need
only those for the address we are interested in.  I have seen result sets over
20 megabytes for a single address.
* The API supports only one address at a time.  It would be faster to query
for all addresses at once.

I have created a toshi fork at http://github.com/dan-da/toshi that implements
a new API (/addresses/.../transactionsfiltered) which filters out transaction
inputs/outputs we are not interested in.

The savings are significant.  An API call that was taking 6 seconds and
returning a huge result set now takes .2 seconds and returns less than 1k of
data.

Further optimizations are possible:
* the new API is still processing only one address at a time.
* the new API retrieves extra metadata about the address that is unnecessary.


## Insight

as of v0.2.18

* Fast enough for transactions with few inputs/outputs, but 2-3x slower than
  toshi in my testing for larger transactions.
* does not support filtering unrelated inputs/outputs.  takes (more) minutes and
  generates huge results.
* supports querying multiple addresses at once with multiaddr API, but includes
  each TX only once which presently confuses bitprices, leading to invalid
  balances.

## btcd

as of 0.11.1-beta  (110100)

* no public online API service available that I'm aware of, must be run locally.
* Very fast transaction lookups, even with many inputs/outputs.
* generates huge results with many inputs/outputs.
* must be run with addrindex=1 option.  ( important! )

btcd does not include addresses and amount in inputs. I have added this
functionality in a forked version, and submitted a pull request.  Hopefully
mainline accepts it soon.

At this time, to use btcd you must download/install the forked version.  See:
* https://github.com/dan-da/btcd
* https://github.com/btcsuite/btcd/pull/487


# Todos

* Create website frontend for the tool.  In progress.
  see http://mybitprices.info
* Add Bip32, 39, 44 support ( HD Wallets ) so it is only necessary to
  input master public key and entire wallet can be scanned.
* interpret insight's multiaddr results correctly. In theory it should be faster.
* optimize btcd further ( filter inputs/outputs )
* hopefully get toshi changes accepted by toshi project maintainers.
* hopefully get btcd changes accepted by btcd project maintainers.
