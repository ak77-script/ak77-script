# Скользящие средние

*Скользящие средние* (moving averages) - один из наиболее распространенных технических индикаторов, используемых при анализе финансовых рынков.

В техническом анализе применяется для сглаживания колебаний и определения направления тренда

## Скользящие средние, встроенные в TradingView

>Далее и везде TradingView - TV

### Простая скользящая средняя (SMA, Simple Moving Average)

SMA строится путем суммирования всех значений за выбранный период времени и деления суммы на количество значений. 

В языке PineScript пятой версии для построения SMA используется встроенная функция `ta.sma` 
([описание в руководстве TV](https://www.tradingview.com/pine-script-reference/v5/#fun_ta{dot}sma))

ta.wma (Weighted Moving Average) 
https://www.tradingview.com/pine-script-reference/v5/#fun_ta{dot}wma

ta.ema (Exponentially Weighted Moving Average) 
https://www.tradingview.com/pine-script-reference/v5/#fun_ta{dot}ema

ta.alma (Arnaud Legoux Moving Average)
https://www.tradingview.com/pine-script-reference/v5/#fun_ta{dot}alma

ta.hma (Hull Moving Average) 
https://www.tradingview.com/pine-script-reference/v5/#fun_ta{dot}hma

ta.rma (Moving average used in RSI, Adjusted Exponential Moving Average)
https://www.tradingview.com/pine-script-reference/v5/#fun_ta{dot}rma

ta.swma (Symmetrically Weighted Moving Average) 
https://www.tradingview.com/pine-script-reference/v5/#fun_ta{dot}swma
ta.vwma (Volume Weighted Moving Average)
https://www.tradingview.com/pine-script-reference/v5/#fun_ta{dot}vwap

ta.vwap (Volume Weighted Average Price)
https://www.tradingview.com/pine-script-reference/v5/#fun_ta{dot}vwap


https://tlap.com/tipy-skolzyashih-srednih/
Double Exponential Moving Average (DEMA) 
AMA
FRAMA
JMA

//@version=3
// Copyright (c) 2007-present Jurik Research and Consulting. All rights reserved.
// Copyright (c) 2018-present, Alex Orekhov (everget)
// Jurik Moving Average script may be freely distributed under the MIT license.
study("Jurik Moving Average", shorttitle="JMA", overlay=true)

length = input(title="Length", type=integer, defval=7)
phase = input(title="Phase", type=integer, defval=50)
power = input(title="Power", type=integer, defval=2)
src = input(title="Source", type=source, defval=close)
highlightMovements = input(title="Highlight Movements ?", type=bool, defval=true)

phaseRatio = phase < -100 ? 0.5 : phase > 100 ? 2.5 : phase / 100 + 1.5

beta = 0.45 * (length - 1) / (0.45 * (length - 1) + 2)
alpha = pow(beta, power)

jma = 0.0

e0 = 0.0
e0 := (1 - alpha) * src + alpha * nz(e0[1])

e1 = 0.0
e1 := (src - e0) * (1 - beta) + beta * nz(e1[1])

e2 = 0.0
e2 := (e0 + phaseRatio * e1 - nz(jma[1])) * pow(1 - alpha, 2) + pow(alpha, 2) * nz(e2[1])

jma := e2 + nz(jma[1])

jmaColor = highlightMovements ? (jma > jma[1] ? green : red) : #6d1e7f
plot(jma, title="JMA", linewidth=2, color=jmaColor, transp=0)


Triple Exponential Moving Average (TEMA) 
Variable Index Dynamic Average (VIDYA)
MIT License
https://ru.wikipedia.org/wiki/%D0%9B%D0%B8%D1%86%D0%B5%D0%BD%D0%B7%D0%B8%D1%8F_MIT

