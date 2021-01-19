---
title: "Lecture 2 R version"
author: "Haixiang Zhu"
date: "1/19/2021"
output: pdf_document
---

## Bollinger Bands Strategy

```{r}
# import library
library("quantmod")
library("PerformanceAnalytics")
getSymbols("^DJI")
head(DJI)
```
```{r}
# Slice data set
dji <- DJI[,"DJI.Adjusted"]
# Select window size
dji<- dji[(index(dji) <= "2020-12-31"),]
# Covert price to return
ret_dji<- Delt(dji,k=1)
# Split data set
index <- 1:(nrow(dji)*0.7)
in_dji <- dji[index]
in_ret_dji <- ret_dji[index]
out_dji <- dji[-index]
out_ret_dji <- ret_dji[-index]
# Create BB signal
bb_in <- BBands(in_dji)
signal_in <- NULL
signal_in <- ifelse(in_dji < bb_in[,'dn'], 1, ifelse(in_dji > bb_in[,'up'],-1 ,0))
stra_bb_in <- in_ret_dji*lag(signal_in)
```
```{r}
# Plotting Performance
charts.PerformanceSummary(stra_bb_in)
```
```{r}
# Metrics
table.Stats(stra_bb_in)
table.AnnualizedReturns(stra_bb_in)
table.DownsideRisk(stra_bb_in)
table.Drawdowns(stra_bb_in)
```
```{r}
# Repeat for out-of sameples
bb_out <- BBands(out_dji)
signal_out <- NULL
signal_out <- ifelse(out_dji < bb_out[,'dn'], 1, ifelse(out_dji > bb_out[,'up'],-1 ,0))
stra_bb_out <- out_ret_dji*lag(signal_out)
charts.PerformanceSummary(stra_bb_out)
table.Stats(stra_bb_out)
table.AnnualizedReturns(stra_bb_out)
table.DownsideRisk(stra_bb_out)
table.Drawdowns(stra_bb_out)
```


## Exercise: RSI Strategy

```{r}
library("quantmod")
library("PerformanceAnalytics")
getSymbols("^DJI")
dji <- DJI[,"DJI.Adjusted"]
dji<- dji[(index(dji) <= "2020-12-31"),]
ret_dji<- Delt(dji,k=1)
index <- 1:(nrow(dji)*0.7)
in_dji <- dji[index]
in_ret_dji <- ret_dji[index]
out_dji <- dji[-index]
out_ret_dji <- ret_dji[-index]
rsi_in <-  RSI(in_dji)
signal_in <- NULL
signal_in <- ifelse(rsi_in < 30, 1, ifelse(rsi_in >70,-1 ,0))
stra_rsi_in <- in_ret_dji*lag(signal_in)
charts.PerformanceSummary(stra_rsi_in)
table.Stats(stra_rsi_in)
table.AnnualizedReturns(stra_rsi_in)
table.DownsideRisk(stra_rsi_in)
table.Drawdowns(stra_rsi_in)
rsi_out <- RSI(out_dji)
signal_out <- NULL
signal_out <- ifelse(rsi_out < 30, 1, ifelse(rsi_out > 70,-1 ,0))
stra_rsi_out <- out_ret_dji*lag(signal_out)
charts.PerformanceSummary(stra_rsi_out)
table.Stats(stra_rsi_out)
table.AnnualizedReturns(stra_rsi_out)
table.DownsideRisk(stra_rsi_out)
table.Drawdowns(stra_rsi_out)
```


