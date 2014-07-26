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
ordered_by_fatalities<-data[order(-data$FATALITIES),]
ordered_by_injuries<-data[order(-data$INJURIES),]
```

We will select the top-20 event-types from each dataset and look at those in more detail.


```r
top_fatalities<-ordered_by_fatalities[1:20,]
tf<-top_fatalities[,c("EVTYPE", "FATALITIES")]
tf
```

```
##                EVTYPE FATALITIES
## 198704           HEAT        583
## 862634        TORNADO        158
## 68670         TORNADO        116
## 148852        TORNADO        114
## 355128 EXCESSIVE HEAT         99
## 67884         TORNADO         90
## 46309         TORNADO         75
## 371112 EXCESSIVE HEAT         74
## 230927 EXCESSIVE HEAT         67
## 78567         TORNADO         57
## 247938   EXTREME HEAT         57
## 6370          TORNADO         50
## 598500 EXCESSIVE HEAT         49
## 606363 EXCESSIVE HEAT         46
## 860386        TORNADO         44
## 157885        TORNADO         42
## 362850 EXCESSIVE HEAT         42
## 629242 EXCESSIVE HEAT         42
## 78123         TORNADO         38
## 83578         TORNADO         37
```

```r
top_injuries<-ordered_by_injuries[1:20,]
ti<-top_injuries[,c("EVTYPE", "INJURIES")]
ti
```

```
##                   EVTYPE INJURIES
## 157885           TORNADO     1700
## 223449         ICE STORM     1568
## 67884            TORNADO     1228
## 116011           TORNADO     1150
## 862634           TORNADO     1150
## 344159             FLOOD      800
## 860386           TORNADO      800
## 68670            TORNADO      785
## 529351 HURRICANE/TYPHOON      780
## 344178             FLOOD      750
## 858228           TORNADO      700
## 344158             FLOOD      600
## 148852           TORNADO      597
## 35124            TORNADO      560
## 344163             FLOOD      550
## 667233    EXCESSIVE HEAT      519
## 78567            TORNADO      504
## 16503            TORNADO      500
## 29566            TORNADO      500
## 153749           TORNADO      500
```


```r
library(plyr)
c_tf<-count(tf, vars = "EVTYPE", wt_var = "FATALITIES")
c_ti<-count(ti, vars = "EVTYPE", wt_var = "INJURIES")
c_tf[order(-c_tf$freq),]
```

```
##           EVTYPE freq
## 4        TORNADO  821
## 3           HEAT  583
## 1 EXCESSIVE HEAT  419
## 2   EXTREME HEAT   57
```

```r
c_ti[order(-c_ti$freq),]
```

```
##              EVTYPE  freq
## 5           TORNADO 10674
## 2             FLOOD  2700
## 4         ICE STORM  1568
## 3 HURRICANE/TYPHOON   780
## 1    EXCESSIVE HEAT   519
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
injuries_by_state<-injuries_by_state[,c("STATE", "EVTYPE", "FATALITIES")]
head(fatalities_by_state)
```

```
##        STATE         EVTYPE FATALITIES
## 188990    AK  MARINE MISHAP          6
## 348627    AK      AVALANCHE          6
## 348623    AK      ICE STORM          5
## 189053    AK HIGH WIND/SEAS          4
## 449520    AK          FLOOD          3
## 380114    AK   WINTER STORM          2
```


## Results













