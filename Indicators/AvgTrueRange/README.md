﻿# Average True Range (ATR)

Measure of volatility that captures gaps and limits between periods.
[More info ...](https://school.stockcharts.com/doku.php?id=technical_indicators:average_true_range_atr)

``` C#
// usage
IEnumerable<AtrResult> results = Indicator.GetAtr(history, lookbackPeriod);  
```

## Parameters

| name | type | notes
| -- |-- |--
| `history` | IEnumerable\<[Quote](/GUIDE.md#Quote)\> | Historical Quotes data should be at any consistent frequency (day, hour, minute, etc).  You must supply at least `N`+1 periods of `history`.  Since this uses a smoothing technique, we recommend you use at least 2×`N` data points prior to the intended usage date for maximum precision.
| `lookbackPeriod` | int | Number of periods (`N`) to consider.

## Response

``` C#
IEnumerable<AtrResult>
```

The first `N-1` periods will have `null` values for ATR since there's not enough data to calculate.  We always return the same number of elements as there are in the historical quotes.

### AtrResult

| name | type | notes
| -- |-- |--
| `Index` | int | Sequence of dates
| `Date` | DateTime | Date
| `Tr` | decimal | True Range for current period
| `Atr` | decimal | Average True Range for `N` lookback periods

## Example

``` C#
// fetch historical quotes from your favorite feed, in Quote format
IEnumerable<Quote> history = GetHistoryFromFeed("SPY");

// calculate 14-period ATR
IEnumerable<AtrResult> results = Indicator.GetAtr(history,14);

// use results as needed
DateTime evalDate = DateTime.Parse("12/31/2018");
AtrResult result = results.Where(x=>x.Date==evalDate).FirstOrDefault();
Console.WriteLine("ATR on {0} was ${1}", result.Date, result.Atr);
```

``` text
ATR on 12/31/2018 was 6.15
```
