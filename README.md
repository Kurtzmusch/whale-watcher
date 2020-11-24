##### description

whale-watcher is a collection of scripts that helps to keep track of large cryptocurrency transactions in and out of exchanges.  
`get-crypto-ohlc` is used to collect the price of the currencyes. it uses cryptowatch api  
`get-crypto-txs` is used to collect large transactions from and to exchanges. it uses whale-alert api  
`plot-crypto` is used to visualize the data from the generated database  

by default all transacions above 500k USD are collected, this is a lower limit imposed by the whale-alert api and can only be increased

##### dependencyes

`python3`  
`matplotlib`

##### usage
`ww-reset-db` should be used to reset or initialize the json database
`get-crypto-ohlc` should be run hourly  
`get-crypto-txs` should be run often, since whale-alert imposes a limit of api calls you may loose data if too many transactions build up. whale-alert also doesnt allow to fetch data older than 1 hour, so a good rate is around 1 to 15 minutes. 1 minute is recommended if you intend to watch the cache files and setup alarms on your system.  

since these scripts are meant to be run on a schedule, here's a crontab exemple(assumes scripts are located at ~/bin)
```
5    *     *     *     *     bin/get-crypto-ohlce 2>> log/get-crypto-ohlc.log
*/1  *     *     *     *     bin/get-crypto-txs 2>> log/get-crypto-txs.log
```

plot-crypto plots requested graphs from data in your local database.
the command line options are:

```lang-none
--n plots the net inflow of the crypto, that is: how much crypto entered exchanges minus how much left exchanges  
--i plots the inflow of the crypto, that is: how much crypto entered exchanges  
--d(i,n) plots the difference between stablecoins that entered exchagnes and crypto that entered exchanges  
--s(i,n) plots the stablecois as they were one
```

example:

`plot-crypto --n --sn btc`
this will plot 3 graphs: a price graph for BTC, a net graph for BTC(how much BTC entered excahnges minus how much left), and a net graph for stablecoins(how much stablecoins entered exchanges minus how much left)

`plot-crypto --dn eth`
this will plot 2 graphs: a price graph for ETH and a difference graph between the net inflow of ETH and the net inflow of stablecoins

all stablecoins are always treated as one

##### installation\setup
before running any script it's necessary to run `ww-reset-db` in order to create a new /dbs folder with some necessary files
the scripts assume a file called whale-alert-api-key exists on the directory of execution, so its necessary to obtain one key and put it on the file
the scripts assume a file called colors.json exists on the directory of execution

