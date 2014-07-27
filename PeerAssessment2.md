---
title: Severe weather events on population health and economics.
author: "Bjorgvin Reynisson"
output: html_document
---

## Synopsis

## Data Processing

The data is read and we use the str() to take a peek at the data:


```r
data<-read.csv("repdata-data-StormData.csv.bz2")
str(data)
```

```
## 'data.frame':	902297 obs. of  37 variables:
##  $ STATE__   : num  1 1 1 1 1 1 1 1 1 1 ...
##  $ BGN_DATE  : Factor w/ 16335 levels "1/1/1966 0:00:00",..: 6523 6523 4242 11116 2224 2224 2260 383 3980 3980 ...
##  $ BGN_TIME  : Factor w/ 3608 levels "000","0000","0001",..: 152 167 2645 1563 2524 3126 122 1563 3126 3126 ...
##  $ TIME_ZONE : Factor w/ 22 levels "ADT","AKS","AST",..: 6 6 6 6 6 6 6 6 6 6 ...
##  $ COUNTY    : num  97 3 57 89 43 77 9 123 125 57 ...
##  $ COUNTYNAME: Factor w/ 29601 levels "","5NM E OF MACKINAC BRIDGE TO PRESQUE ISLE LT MI",..: 13513 1873 4598 10592 4372 10094 1973 23873 24418 4598 ...
##  $ STATE     : Factor w/ 72 levels "AK","AL","AM",..: 2 2 2 2 2 2 2 2 2 2 ...
##  $ EVTYPE    : Factor w/ 985 levels "   HIGH SURF ADVISORY",..: 826 826 826 826 826 826 826 826 826 826 ...
##  $ BGN_RANGE : num  0 0 0 0 0 0 0 0 0 0 ...
##  $ BGN_AZI   : Factor w/ 35 levels "","  N"," NW",..: 1 1 1 1 1 1 1 1 1 1 ...
##  $ BGN_LOCATI: Factor w/ 54429 levels ""," Christiansburg",..: 1 1 1 1 1 1 1 1 1 1 ...
##  $ END_DATE  : Factor w/ 6663 levels "","1/1/1993 0:00:00",..: 1 1 1 1 1 1 1 1 1 1 ...
##  $ END_TIME  : Factor w/ 3647 levels ""," 0900CST",..: 1 1 1 1 1 1 1 1 1 1 ...
##  $ COUNTY_END: num  0 0 0 0 0 0 0 0 0 0 ...
##  $ COUNTYENDN: logi  NA NA NA NA NA NA ...
##  $ END_RANGE : num  0 0 0 0 0 0 0 0 0 0 ...
##  $ END_AZI   : Factor w/ 24 levels "","E","ENE","ESE",..: 1 1 1 1 1 1 1 1 1 1 ...
##  $ END_LOCATI: Factor w/ 34506 levels ""," CANTON"," TULIA",..: 1 1 1 1 1 1 1 1 1 1 ...
##  $ LENGTH    : num  14 2 0.1 0 0 1.5 1.5 0 3.3 2.3 ...
##  $ WIDTH     : num  100 150 123 100 150 177 33 33 100 100 ...
##  $ F         : int  3 2 2 2 2 2 2 1 3 3 ...
##  $ MAG       : num  0 0 0 0 0 0 0 0 0 0 ...
##  $ FATALITIES: num  0 0 0 0 0 0 0 0 1 0 ...
##  $ INJURIES  : num  15 0 2 2 2 6 1 0 14 0 ...
##  $ PROPDMG   : num  25 2.5 25 2.5 2.5 2.5 2.5 2.5 25 25 ...
##  $ PROPDMGEXP: Factor w/ 19 levels "","+","-","0",..: 16 16 16 16 16 16 16 16 16 16 ...
##  $ CROPDMG   : num  0 0 0 0 0 0 0 0 0 0 ...
##  $ CROPDMGEXP: Factor w/ 9 levels "","0","2","?",..: 1 1 1 1 1 1 1 1 1 1 ...
##  $ WFO       : Factor w/ 542 levels ""," CI","$AC",..: 1 1 1 1 1 1 1 1 1 1 ...
##  $ STATEOFFIC: Factor w/ 250 levels "","ALABAMA, Central",..: 1 1 1 1 1 1 1 1 1 1 ...
##  $ ZONENAMES : Factor w/ 25112 levels "","                                                                                                                               "| __truncated__,..: 1 1 1 1 1 1 1 1 1 1 ...
##  $ LATITUDE  : num  3040 3042 3340 3458 3412 ...
##  $ LONGITUDE : num  8812 8755 8742 8626 8642 ...
##  $ LATITUDE_E: num  3051 0 0 0 0 ...
##  $ LONGITUDE_: num  8806 0 0 0 0 ...
##  $ REMARKS   : Factor w/ 436781 levels "","\t","\t\t",..: 1 1 1 1 1 1 1 1 1 1 ...
##  $ REFNUM    : num  1 2 3 4 5 6 7 8 9 10 ...
```

## Analysis

### The analysis of Fatalities and Injuries. Summary statistics for the United States.
We order the dataset based on the fatalities and injuries.


```r
library(plyr)
fatalities<-count(data, vars = "EVTYPE", wt_var = "FATALITIES")
fatalities<-fatalities[order(-fatalities$freq),]

injuries<-count(data, vars = "EVTYPE", wt_var = "INJURIES")
injuries<-injuries[order(-injuries$freq),]
```

And then reduce the data to the top 10 causes:

```r
fatalities<-fatalities[1:10,]
fatalities
```

```
##             EVTYPE freq
## 826        TORNADO 5633
## 124 EXCESSIVE HEAT 1903
## 151    FLASH FLOOD  978
## 271           HEAT  937
## 453      LIGHTNING  816
## 846      TSTM WIND  504
## 167          FLOOD  470
## 572    RIP CURRENT  368
## 343      HIGH WIND  248
## 19       AVALANCHE  224
```

```r
injuries<-injuries[1:10,]
injuries
```

```
##                EVTYPE  freq
## 826           TORNADO 91346
## 846         TSTM WIND  6957
## 167             FLOOD  6789
## 124    EXCESSIVE HEAT  6525
## 453         LIGHTNING  5230
## 271              HEAT  2100
## 422         ICE STORM  1975
## 151       FLASH FLOOD  1777
## 753 THUNDERSTORM WIND  1488
## 241              HAIL  1361
```


From the two tables, we see that tornados are the biggest cause of injuries in the US, and the second biggest cause of fatalities. The number one cause of fatalities in the US is heat. 

### The analysis of Fatalities and Injuries. State-by-state analysis.

We would like to analyze the fatalities and injuries caused by weather in each state.


```r
fatalities_by_state<-data[order(data$STATE, -data$FATALITIES),]
injuries_by_state<-data[order(data$STATE, -data$INJURIES),]
```

We trim the data to just the STATE, EVTYPE and FATALITIES or INJURIES variables.


```r
fatalities_by_state<-fatalities_by_state[,c("STATE", "EVTYPE", "FATALITIES")]
injuries_by_state<-injuries_by_state[,c("STATE", "EVTYPE", "INJURIES")]
```

Then sum the datasets by state:


```r
fatalities_by_state_s<-ddply(fatalities_by_state, .(STATE, EVTYPE), 
                             summarize, FATALITIES=sum(FATALITIES))
injuries_by_state_s<-ddply(injuries_by_state, .(STATE, EVTYPE), 
                             summarize, INJURIES=sum(INJURIES))
```

And sort by decsending number of fatalities/injuries per state:


```r
fatalities_by_state_r<-arrange(fatalities_by_state_s, 
                               fatalities_by_state_s$STATE,
                               -fatalities_by_state_s$FATALITIES)
injuries_by_state_r<-arrange(injuries_by_state_s, 
                               injuries_by_state_s$STATE,
                               -injuries_by_state_s$INJURIES)
```

We then reduce the data to the top three fatalities/injury causes in each state:


```r
top_fatalities_per_state<-ddply(fatalities_by_state_r, .(STATE), function(x)x[1:3,])
top_injuries_per_state<-ddply(injuries_by_state_r, .(STATE), function(x)x[1:3,])
```


## Results













