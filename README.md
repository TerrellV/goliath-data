# goliathdata
A convenient python package to help build crypto candle histories and apply custom indicators

## Usage
```py

>>> import goliathdata as gl
>>> btc_df = gl.History(base='btc', quote='usd', freq='1D')

# apply predefined indicators
>>> indicators = (
        btc_df.moving_average(periods=8)
              .slope(periods=30)
              .volatility(periods=60, annualized=True)
              .trailing_returns(periods=7)
    )

# apply predefined + custom indicator
def trailing_ath(df, *args, **kwargs):
    periods = kwargs.get("periods", 10)
    df["ath"] = df["close"].expanding(periods).shift(1).max()
    return df

>>> indicators = (
        btc_df.moving_average(periods=8)
              .slope(periods=30)
              .apply(trailing_ath, periods=60)  # custom indicator
    )
```

