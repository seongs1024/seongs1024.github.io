+++
title = "Max oreder rate"
description = ""
date = 2022-03-05T07:00:00+09:00
updated = 2022-03-05T00:00:00+09:00
template = "blog/page.html"
draft = true

[taxonomies]
authors = ["Seongsu Park"]

[extra]
lead = "Assembly says No"
images = []
tags = ["nautilus-trader", "cryptocurrency", "quant", "trading"]
toc = true
+++

노틸러스에서 `[ERR] SubmitOrder DENIED: Exceeded MAX_ORDER_RATE.` 에러가 날 때가 있다.

해당 에러는 `RiskEngine`의 `config`에서 `max_order_rate`의 기본값이 `"100/00:00:01"`로 정해져있는데 설정하지 않아서 생기는 문제다.

```
If you haven’t made any changes to the max order rate config for the risk engine, then somehow the logic of your strategy is attempting to send > 100 orders per second
```

```shell
2022-04-21T07:28:59.068364500Z [INF] BACKTESTER-001.DataEngine: INITIALIZED.
2022-04-21T07:28:59.068364500Z [INF] BACKTESTER-001.RiskEngine: INITIALIZED.
2022-04-21T07:28:59.068364500Z [INF] BACKTESTER-001.RiskEngine: TradingState is ACTIVE.
2022-04-21T07:28:59.068364500Z [INF] BACKTESTER-001.Throttler-ORDER_RATE: INITIALIZED.
2022-04-21T07:28:59.068364500Z [INF] BACKTESTER-001.RiskEngine: Set MAX_ORDER_RATE: 100/00:00:01.
2022-04-21T07:28:59.068364500Z [INF] BACKTESTER-001.ExecEngine: INITIALIZED.
```
```shell
1970-01-01T00:27:30.090839999Z [ERR] BACKTESTER-001.RiskEngine: SubmitOrder DENIED: Exceeded MAX_ORDER_RATE.
1970-01-01T00:27:30.090839999Z [WRN] BACKTESTER-001.EMACross-001: <--[EVT] OrderDenied(instrument_id=BTCUSDT-PERP.BINANCE, client_order_id=O-19700101-002730-001-001-356, reason=Exceeded MAX_ORDER_RATE).
1970-01-01T00:27:30.090839999Z [WRN] BACKTESTER-001.Throttler-ORDER_RATE: Dropped SubmitOrder(instrument_id=BTCUSDT-PERP.BINANCE, client_order_id=O-19700101-002730-001-001-356, position_id=None, order=SELL 0.010 BTCUSDT-PERP.BINANCE MARKET GTC).
```
```shell
2022-04-21T07:33:25.159388300Z [INF] BACKTESTER-001.DataEngine: INITIALIZED.
2022-04-21T07:33:25.159388300Z [INF] BACKTESTER-001.RiskEngine: INITIALIZED.
2022-04-21T07:33:25.159388300Z [INF] BACKTESTER-001.RiskEngine: TradingState is ACTIVE.
2022-04-21T07:33:25.159388300Z [INF] BACKTESTER-001.RiskEngine: PRE-TRADE RISK CHECKS BYPASSED. This is not advisable for live trading.
2022-04-21T07:33:25.159388300Z [INF] BACKTESTER-001.Throttler-ORDER_RATE: INITIALIZED.
2022-04-21T07:33:25.159388300Z [INF] BACKTESTER-001.RiskEngine: Set MAX_ORDER_RATE: 100/00:00:01.
2022-04-21T07:33:25.159388300Z [INF] BACKTESTER-001.ExecEngine: INITIALIZED.
```