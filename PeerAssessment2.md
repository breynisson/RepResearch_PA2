# Severe weather events on population health and economics.

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
##  $ BGN_TIME  : Factor w/ 3608 levels "00:00:00 AM",..: 272 287 2705 1683 2584 3186 242 1683 3186 3186 ...
##  $ TIME_ZONE : Factor w/ 22 levels "ADT","AKS","AST",..: 7 7 7 7 7 7 7 7 7 7 ...
##  $ COUNTY    : num  97 3 57 89 43 77 9 123 125 57 ...
##  $ COUNTYNAME: Factor w/ 29601 levels "","5NM E OF MACKINAC BRIDGE TO PRESQUE ISLE LT MI",..: 13513 1873 4598 10592 4372 10094 1973 23873 24418 4598 ...
##  $ STATE     : Factor w/ 72 levels "AK","AL","AM",..: 2 2 2 2 2 2 2 2 2 2 ...
##  $ EVTYPE    : Factor w/ 985 levels "   HIGH SURF ADVISORY",..: 834 834 834 834 834 834 834 834 834 834 ...
##  $ BGN_RANGE : num  0 0 0 0 0 0 0 0 0 0 ...
##  $ BGN_AZI   : Factor w/ 35 levels "","  N"," NW",..: 1 1 1 1 1 1 1 1 1 1 ...
##  $ BGN_LOCATI: Factor w/ 54429 levels "","- 1 N Albion",..: 1 1 1 1 1 1 1 1 1 1 ...
##  $ END_DATE  : Factor w/ 6663 levels "","1/1/1993 0:00:00",..: 1 1 1 1 1 1 1 1 1 1 ...
##  $ END_TIME  : Factor w/ 3647 levels ""," 0900CST",..: 1 1 1 1 1 1 1 1 1 1 ...
##  $ COUNTY_END: num  0 0 0 0 0 0 0 0 0 0 ...
##  $ COUNTYENDN: logi  NA NA NA NA NA NA ...
##  $ END_RANGE : num  0 0 0 0 0 0 0 0 0 0 ...
##  $ END_AZI   : Factor w/ 24 levels "","E","ENE","ESE",..: 1 1 1 1 1 1 1 1 1 1 ...
##  $ END_LOCATI: Factor w/ 34506 levels "","- .5 NNW",..: 1 1 1 1 1 1 1 1 1 1 ...
##  $ LENGTH    : num  14 2 0.1 0 0 1.5 1.5 0 3.3 2.3 ...
##  $ WIDTH     : num  100 150 123 100 150 177 33 33 100 100 ...
##  $ F         : int  3 2 2 2 2 2 2 1 3 3 ...
##  $ MAG       : num  0 0 0 0 0 0 0 0 0 0 ...
##  $ FATALITIES: num  0 0 0 0 0 0 0 0 1 0 ...
##  $ INJURIES  : num  15 0 2 2 2 6 1 0 14 0 ...
##  $ PROPDMG   : num  25 2.5 25 2.5 2.5 2.5 2.5 2.5 25 25 ...
##  $ PROPDMGEXP: Factor w/ 19 levels "","-","?","+",..: 17 17 17 17 17 17 17 17 17 17 ...
##  $ CROPDMG   : num  0 0 0 0 0 0 0 0 0 0 ...
##  $ CROPDMGEXP: Factor w/ 9 levels "","?","0","2",..: 1 1 1 1 1 1 1 1 1 1 ...
##  $ WFO       : Factor w/ 542 levels ""," CI","$AC",..: 1 1 1 1 1 1 1 1 1 1 ...
##  $ STATEOFFIC: Factor w/ 250 levels "","ALABAMA, Central",..: 1 1 1 1 1 1 1 1 1 1 ...
##  $ ZONENAMES : Factor w/ 25112 levels "","                                                                                                                               "| __truncated__,..: 1 1 1 1 1 1 1 1 1 1 ...
##  $ LATITUDE  : num  3040 3042 3340 3458 3412 ...
##  $ LONGITUDE : num  8812 8755 8742 8626 8642 ...
##  $ LATITUDE_E: num  3051 0 0 0 0 ...
##  $ LONGITUDE_: num  8806 0 0 0 0 ...
##  $ REMARKS   : Factor w/ 436781 levels "","-2 at Deer Park\n",..: 1 1 1 1 1 1 1 1 1 1 ...
##  $ REFNUM    : num  1 2 3 4 5 6 7 8 9 10 ...
```

We start off by converting the BGN_DATE column data to date-type. We also discard the time data in this column:


```r
data$BGN_DATE<-as.Date(as.character(data$BGN_DATE), "%m/%d/%Y")
```

And check that the converstion is OK:

```r
str(data$BGN_DATE)
```

```
##  Date[1:902297], format: "1950-04-18" "1950-04-18" "1951-02-20" "1951-06-08" ...
```

## Analysis

### Fatalities and Injuries. Summary statistics for the United States.
We take a look at what timeframe we are investigating. 

The first date is

```r
min(data$BGN_DATE)
```

```
## [1] "1950-01-03"
```

while the last date in the dataset is 

```r
max(data$BGN_DATE)
```

```
## [1] "2011-11-30"
```

We want to find out what are the most harmful weather effects to the population in the United States over the whole timeframe in question. To this end, we will look at fatalities and injuries. We will investigate these seperatly. We order the dataset based on the fatalities and injuries.


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
## 834        TORNADO 5633
## 130 EXCESSIVE HEAT 1903
## 153    FLASH FLOOD  978
## 275           HEAT  937
## 464      LIGHTNING  816
## 856      TSTM WIND  504
## 170          FLOOD  470
## 585    RIP CURRENT  368
## 359      HIGH WIND  248
## 19       AVALANCHE  224
```

```r
injuries<-injuries[1:10,]
injuries
```

```
##                EVTYPE  freq
## 834           TORNADO 91346
## 856         TSTM WIND  6957
## 170             FLOOD  6789
## 130    EXCESSIVE HEAT  6525
## 464         LIGHTNING  5230
## 275              HEAT  2100
## 427         ICE STORM  1975
## 153       FLASH FLOOD  1777
## 760 THUNDERSTORM WIND  1488
## 244              HAIL  1361
```


From the two tables, we see that in the time period from 1950-01-03 to 2011-11-30, Tornados are the most harmful weather events to public health.

### Fatalities and Injuries. State-by-state analysis.

We would like to analyze the fatalities and injuries caused by weather in each state.


```r
fatalities_by_state_tmp<-data[order(data$STATE, -data$FATALITIES),]
injuries_by_state_tmp<-data[order(data$STATE, -data$INJURIES),]
```

We trim the data to just the STATE, EVTYPE and FATALITIES or INJURIES variables.


```r
fatalities_by_state<-fatalities_by_state_tmp[,c("STATE", "EVTYPE", "FATALITIES")]
injuries_by_state<-injuries_by_state_tmp[,c("STATE", "EVTYPE", "INJURIES")]
```

Then sum the datasets by state:


```r
fatalities_by_state_s<-ddply(fatalities_by_state, .(STATE, EVTYPE), 
                             summarize, FATALITIES=sum(FATALITIES))

total_fatalities_per_state<-ddply(fatalities_by_state, .(STATE), 
                             summarize, FATALITIES=sum(FATALITIES))

injuries_by_state_s<-ddply(injuries_by_state, .(STATE, EVTYPE), 
                             summarize, INJURIES=sum(INJURIES))

total_injuries_per_state<-ddply(injuries_by_state, .(STATE), 
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

We then reduce the data to the top fatalities/injury causes in each state:


```r
top_fatalities_per_state<-ddply(fatalities_by_state_r, .(STATE), 
                                function(x)x[1,])
top_injuries_per_state<-ddply(injuries_by_state_r, .(STATE), 
                              function(x)x[1,])
```

**Top causes of Fatalities per State:**

```r
top_fatalities_per_state
```

```
##    STATE                   EVTYPE FATALITIES
## 1     AK                AVALANCHE         33
## 2     AL                  TORNADO        617
## 3     AM MARINE THUNDERSTORM WIND          6
## 4     AN         MARINE TSTM WIND          6
## 5     AR                  TORNADO        379
## 6     AS                  TSUNAMI         32
## 7     AZ              FLASH FLOOD         62
## 8     CA           EXCESSIVE HEAT        110
## 9     CO                AVALANCHE         48
## 10    CT                HIGH WIND          6
## 11    DC           EXCESSIVE HEAT         20
## 12    DE           EXCESSIVE HEAT          7
## 13    FL              RIP CURRENT        172
## 14    GA                  TORNADO        180
## 15    GM MARINE THUNDERSTORM WIND          1
## 16    GU              RIP CURRENT         20
## 17    HI                HIGH SURF         21
## 18    IA                  TORNADO         81
## 19    ID                AVALANCHE         16
## 20    IL                     HEAT        653
## 21    IN                  TORNADO        252
## 22    KS                  TORNADO        236
## 23    KY                  TORNADO        125
## 24    LA                  TORNADO        153
## 25    LC              MARINE HAIL          0
## 26    LE              MARINE HAIL          0
## 27    LH              MARINE HAIL          0
## 28    LM       MARINE STRONG WIND          2
## 29    LO              MARINE HAIL          0
## 30    LS       MARINE STRONG WIND          1
## 31    MA                  TORNADO        108
## 32    MD           EXCESSIVE HEAT         88
## 33    ME                LIGHTNING          6
## 34    MH                HIGH SURF          0
## 35    MI                  TORNADO        243
## 36    MN                  TORNADO         99
## 37    MO                  TORNADO        388
## 38    MS                  TORNADO        450
## 39    MT                LIGHTNING          9
## 40    NC                  TORNADO        126
## 41    ND                  TORNADO         25
## 42    NE                  TORNADO         54
## 43    NH                TSTM WIND          6
## 44    NJ           EXCESSIVE HEAT         39
## 45    NM              FLASH FLOOD         16
## 46    NV                     HEAT         54
## 47    NY           EXCESSIVE HEAT         93
## 48    OH                  TORNADO        191
## 49    OK                  TORNADO        296
## 50    OR                HIGH WIND         19
## 51    PA           EXCESSIVE HEAT        359
## 52    PH       MARINE STRONG WIND          1
## 53    PK         MARINE HIGH WIND          0
## 54    PM               WATERSPOUT          0
## 55    PR              FLASH FLOOD         34
## 56    PZ       MARINE STRONG WIND          5
## 57    RI                     HEAT          2
## 58    SC                  TORNADO         59
## 59    SD                  TORNADO         18
## 60    SL              MARINE HAIL          0
## 61    ST             STRONG WINDS          0
## 62    TN                  TORNADO        368
## 63    TX                  TORNADO        538
## 64    UT                AVALANCHE         44
## 65    VA                  TORNADO         36
## 66    VI                HIGH SURF          3
## 67    VT                    FLOOD          4
## 68    WA                AVALANCHE         35
## 69    WI                  TORNADO         96
## 70    WV              FLASH FLOOD         24
## 71    WY                AVALANCHE         23
## 72    XX MARINE THUNDERSTORM WIND          0
```

**Top causes of Injuries per State:**

```r
top_injuries_per_state
```

```
##    STATE                   EVTYPE INJURIES
## 1     AK                ICE STORM       34
## 2     AL                  TORNADO     7929
## 3     AM MARINE THUNDERSTORM WIND       22
## 4     AN       MARINE STRONG WIND       18
## 5     AR                  TORNADO     5116
## 6     AS                  TSUNAMI      129
## 7     AZ               DUST STORM      179
## 8     CA                 WILDFIRE      623
## 9     CO                  TORNADO      261
## 10    CT                  TORNADO      703
## 11    DC           EXCESSIVE HEAT      316
## 12    DE                  TORNADO       73
## 13    FL                  TORNADO     3340
## 14    GA                  TORNADO     3926
## 15    GM              MARINE HAIL        0
## 16    GU        HURRICANE/TYPHOON      333
## 17    HI              STRONG WIND       20
## 18    IA                  TORNADO     2208
## 19    ID        THUNDERSTORM WIND       74
## 20    IL                  TORNADO     4145
## 21    IN                  TORNADO     4224
## 22    KS                  TORNADO     2721
## 23    KY                  TORNADO     2806
## 24    LA                  TORNADO     2637
## 25    LC              MARINE HAIL        0
## 26    LE              MARINE HAIL        0
## 27    LH              MARINE HAIL        0
## 28    LM       MARINE STRONG WIND        1
## 29    LO              MARINE HAIL        0
## 30    LS              MARINE HAIL        0
## 31    MA                  TORNADO     1758
## 32    MD           EXCESSIVE HEAT      461
## 33    ME                LIGHTNING       70
## 34    MH                HIGH SURF        1
## 35    MI                  TORNADO     3362
## 36    MN                  TORNADO     1976
## 37    MO                  TORNADO     4330
## 38    MS                  TORNADO     6244
## 39    MT         WILD/FOREST FIRE       33
## 40    NC                  TORNADO     2536
## 41    ND                  TORNADO      326
## 42    NE                  TORNADO     1158
## 43    NH                LIGHTNING       85
## 44    NJ           EXCESSIVE HEAT      300
## 45    NM                  TORNADO      155
## 46    NV                    FLOOD       50
## 47    NY                  TORNADO      315
## 48    OH                  TORNADO     4438
## 49    OK                  TORNADO     4829
## 50    OR                HIGH WIND       50
## 51    PA                  TORNADO     1241
## 52    PH       MARINE STRONG WIND        0
## 53    PK         MARINE HIGH WIND        0
## 54    PM               WATERSPOUT        0
## 55    PR               HEAVY RAIN       10
## 56    PZ       MARINE STRONG WIND        3
## 57    RI                  TORNADO       23
## 58    SC                  TORNADO     1314
## 59    SD                  TORNADO      452
## 60    SL              MARINE HAIL        0
## 61    ST             STRONG WINDS        0
## 62    TN                  TORNADO     4748
## 63    TX                  TORNADO     8207
## 64    UT             WINTER STORM      415
## 65    VA                  TORNADO      914
## 66    VI                LIGHTNING        1
## 67    VT                TSTM WIND       24
## 68    WA                  TORNADO      303
## 69    WI                  TORNADO     1601
## 70    WV                TSTM WIND      142
## 71    WY             WINTER STORM      119
## 72    XX MARINE THUNDERSTORM WIND        0
```


Let's make preperations to display the Fatalities per State and Injuries per State:

```r
library(ggplot2)
library(maps)
library(datasets)
library(gridExtra)

all_states<-map_data("state")
state_names<-data.frame(STATE=state.abb, region=tolower(state.name))

tmp_fat<-merge(total_fatalities_per_state, state_names)
map_data_fat<-merge(tmp_fat, all_states, by='region')

fp<-ggplot(map_data_fat, aes(map_id = region)) +
  geom_map(aes(fill = FATALITIES), map = all_states, color ="black") +
  expand_limits(x = all_states$long, y = all_states$lat) +
  theme(legend.position = "bottom",
        axis.ticks = element_blank(), 
        axis.title = element_blank(), 
        axis.text =  element_blank()) +
  scale_fill_gradient(low="white", high="red") +
  guides(fill = guide_colorbar(barwidth = 10, barheight = .5)) + 
  ggtitle("")

tmp_inj<-merge(total_injuries_per_state, state_names)
map_data_inj<-merge(tmp_inj, all_states, by='region')

ip<-ggplot(map_data_inj, aes(map_id = region)) +
  geom_map(aes(fill = INJURIES), map = all_states, color ="black") +
  expand_limits(x = all_states$long, y = all_states$lat) +
  theme(legend.position = "bottom",
        axis.ticks = element_blank(), 
        axis.title = element_blank(), 
        axis.text =  element_blank()) +
  scale_fill_gradient(low="white", high="red") +
  guides(fill = guide_colorbar(barwidth = 10, barheight = .5)) + 
  ggtitle("")
```



```r
grid.arrange(arrangeGrob(fp, ip, ncol=2, main="Fatalities and Injuries per State"))
```

![plot of chunk popHealthPerState](figure/popHealthPerState.png) 


### Property and Crop Damage. Summary statistics for the United States.
The dataset contains two columns for property damage; PROPDMG and PROPDMGEXP. In the same manner, there are two columns for crop damage; CROPDMG and CROPDMGEXP. 

We will make two new variables, which will hold the calculated property damage for each row. That is PROPERTYDAMAGEVALUE = PROPDMG * PROPDMGEXP and CROPDAMAGEVALUE = CROPDMG * CROPDMGEXP. 

Let's look at the data for PROPDMGEXP:

```r
table(data$PROPDMGEXP)
```

```
## 
##             -      ?      +      0      1      2      3      4      5 
## 465934      1      8      5    216     25     13      4      4     28 
##      6      7      8      B      h      H      K      m      M 
##      4      5      1     40      1      6 424665      7  11330
```

We do see that there are a few different values here, and it is not clear how to handle many of those. The course of action taken here, is that we take K for Thousands, M for Millions and B for Billions. Any other value will be substituted with a value of "1".  The same approach is taken for the CROPDMEXP values.

We start by setting the PROPDMGEXP AND CROPDMGEXP columns to lowercase. Then, new variables are made: PROPERTYDMGEXPNUMBER and CROPDMGEXPNUMBER.

```r
data$PROPDMGEXP<-tolower(data$PROPDMGEXP)
data$PROPERTYDMGEXPNUMBER<-1
data$PROPERTYDMGEXPNUMBER[data$PROPDMGEXP=="k"]<-1000
data$PROPERTYDMGEXPNUMBER[data$PROPDMGEXP=="m"]<-1000000
data$PROPERTYDMGEXPNUMBER[data$PROPDMGEXP=="b"]<-1000000000

data$CROPDMGEXP<-tolower(data$CROPDMGEXP)
data$CROPDMGEXPNUMBER<-1
data$CROPDMGEXPNUMBER[data$CROPDMGEXP=="k"]<-1000
data$CROPDMGEXPNUMBER[data$CROPDMGEXP=="m"]<-1000000
data$CROPDMGEXPNUMBER[data$CROPDMGEXP=="b"]<-1000000000
```

We then find the numeric value for property and cropdamages:

```r
data$PROPERTYDAMAGEVALUE<-data$PROPDMG * data$PROPERTYDMGEXPNUMBER
data$CROPDAMAGEVALUE<-data$CROPDMG * data$CROPDMGEXPNUMBER
```


We can now calculate the total economic damage (property and crop) within the timeframe:

```r
library(scales)
totalPropertyDamage<-sum(data$PROPERTYDAMAGEVALUE) + sum(data$CROPDAMAGEVALUE)
dollar(totalPropertyDamage)
```

```
## [1] "$476,422,842,480"
```


### Property and Crop Damage. State-by-state analysis.

We would like to analyze the property damage and crop damage caused by weather in each state.


```r
propdmg_by_state_tmp<-data[order(data$STATE, -data$PROPERTYDAMAGEVALUE),]
cropdmg_by_state_tmp<-data[order(data$STATE, -data$CROPDAMAGEVALUE),]
```

We trim the data to just the STATE, EVTYPE and PROPERTYDAMAGEVALUE or CROPDAMAGEVALUE variables.


```r
propdmg_by_state<-propdmg_by_state_tmp[,c("STATE", "EVTYPE", "PROPERTYDAMAGEVALUE")]
cropdmg_by_state<-cropdmg_by_state_tmp[,c("STATE", "EVTYPE", "CROPDAMAGEVALUE")]
```

Then sum the datasets by state:


```r
propdmg_by_state_s<-ddply(propdmg_by_state, .(STATE, EVTYPE), 
                          summarize, 
                          PROPERTYDAMAGEVALUE=sum(PROPERTYDAMAGEVALUE))

propdmg_by_state_per_state<-ddply(propdmg_by_state, .(STATE), 
                                  summarize, 
                                  PROPERTYDAMAGEVALUE=sum(PROPERTYDAMAGEVALUE))

cropdmg_by_state_s<-ddply(cropdmg_by_state, .(STATE, EVTYPE),
                          summarize, 
                          CROPDAMAGEVALUE=sum(CROPDAMAGEVALUE))

cropdmg_by_state_per_state<-ddply(cropdmg_by_state, .(STATE), 
                             summarize, 
                             CROPDAMAGEVALUE=sum(CROPDAMAGEVALUE))
```

And sort by decsending number of property/crop damage per state:


```r
propdmg_by_state_r<-arrange(propdmg_by_state_s, 
                               propdmg_by_state_s$STATE,
                               -propdmg_by_state_s$PROPERTYDAMAGEVALUE)
cropdmg_by_state_r<-arrange(cropdmg_by_state_s, 
                               cropdmg_by_state_s$STATE,
                               -cropdmg_by_state_s$CROPDAMAGEVALUE)
```

We reduce the data to the top property/crop damage causes in each state:


```r
top_propdmg_per_state<-ddply(propdmg_by_state_r, .(STATE), 
                                function(x)x[1,])
top_cropdmg_per_state<-ddply(cropdmg_by_state_r, .(STATE), 
                              function(x)x[1,])
```

**Top causes of Property damage per State:**

```r
top_propdmg_per_state$PROPERTYDAMAGEVALUE<-dollar(top_propdmg_per_state$PROPERTYDAMAGEVALUE)
top_propdmg_per_state
```

```
##    STATE                   EVTYPE PROPERTYDAMAGEVALUE
## 1     AK                    FLOOD        $157,131,940
## 2     AL                  TORNADO      $6,321,296,560
## 3     AM               WATERSPOUT          $5,102,000
## 4     AN MARINE THUNDERSTORM WIND            $169,000
## 5     AR                  TORNADO      $2,590,007,310
## 6     AS                  TSUNAMI         $81,000,000
## 7     AZ                     HAIL      $2,828,908,700
## 8     CA                    FLOOD    $116,751,420,000
## 9     CO                     HAIL      $1,423,944,785
## 10    CT                  TORNADO        $596,236,620
## 11    DC           TROPICAL STORM        $127,600,000
## 12    DE            COASTAL FLOOD         $40,150,000
## 13    FL        HURRICANE/TYPHOON     $27,596,865,000
## 14    GA                  TORNADO      $3,261,026,670
## 15    GM         MARINE TSTM WIND          $3,226,000
## 16    GU                  TYPHOON        $600,230,000
## 17    HI              FLASH FLOOD        $156,508,000
## 18    IA                  TORNADO      $2,286,576,200
## 19    ID                    FLOOD        $114,192,000
## 20    IL              RIVER FLOOD      $5,021,900,000
## 21    IN                  TORNADO      $2,594,793,890
## 22    KS                  TORNADO      $2,669,890,670
## 23    KY                  TORNADO        $888,768,680
## 24    LA              STORM SURGE     $31,742,735,000
## 25    LC              MARINE HAIL                  $0
## 26    LE MARINE THUNDERSTORM WIND             $25,000
## 27    LH              MARINE HAIL                  $0
## 28    LM         MARINE TSTM WIND          $1,205,000
## 29    LO         MARINE TSTM WIND             $50,000
## 30    LS         MARINE TSTM WIND            $400,000
## 31    MA                  TORNADO        $756,039,145
## 32    MD           TROPICAL STORM        $538,505,000
## 33    ME                ICE STORM        $318,230,000
## 34    MH                HIGH SURF          $5,000,000
## 35    MI                  TORNADO      $1,071,765,550
## 36    MN                  TORNADO      $1,903,701,140
## 37    MO                  TORNADO      $4,800,631,725
## 38    MS        HURRICANE/TYPHOON     $13,492,735,000
## 39    MT                     HAIL         $94,729,700
## 40    NC                HURRICANE      $4,979,861,000
## 41    ND                    FLOOD      $3,916,842,000
## 42    NE                  TORNADO      $1,718,164,710
## 43    NH                ICE STORM         $64,934,000
## 44    NJ                    FLOOD      $2,111,650,000
## 45    NM         WILD/FOREST FIRE      $1,509,700,000
## 46    NV                    FLOOD        $677,945,000
## 47    NY              FLASH FLOOD      $1,831,088,500
## 48    OH                  TORNADO      $2,279,857,790
## 49    OK                  TORNADO      $3,268,708,233
## 50    OR                    FLOOD        $722,172,500
## 51    PA                  TORNADO      $1,789,038,400
## 52    PH       MARINE STRONG WIND                  $0
## 53    PK         MARINE HIGH WIND             $31,000
## 54    PM               WATERSPOUT                  $0
## 55    PR                HURRICANE      $1,824,431,000
## 56    PZ       MARINE STRONG WIND             $76,000
## 57    RI                    FLOOD         $92,860,000
## 58    SC                  TORNADO        $531,505,190
## 59    SD                  TORNADO        $231,213,780
## 60    SL         MARINE TSTM WIND             $15,000
## 61    ST             STRONG WINDS                  $0
## 62    TN                    FLOOD      $4,245,346,300
## 63    TX           TROPICAL STORM      $5,491,193,000
## 64    UT                    FLOOD        $331,761,500
## 65    VA        HURRICANE/TYPHOON        $512,000,000
## 66    VI                HURRICANE         $28,220,000
## 67    VT                    FLOOD      $1,100,484,000
## 68    WA                    FLOOD        $212,683,000
## 69    WI                     HAIL        $961,723,350
## 70    WV              FLASH FLOOD        $485,226,100
## 71    WY                     HAIL        $111,221,705
## 72    XX MARINE THUNDERSTORM WIND                  $0
```

**Top causes of Crop damage per State:**

```r
top_cropdmg_per_state$CROPDAMAGEVALUE<-dollar(top_cropdmg_per_state$CROPDAMAGEVALUE)
top_cropdmg_per_state
```

```
##    STATE                   EVTYPE CROPDAMAGEVALUE
## 1     AK                HIGH WIND        $157,000
## 2     AL                     HEAT    $400,100,000
## 3     AM MARINE THUNDERSTORM WIND         $50,000
## 4     AN              MARINE HAIL              $0
## 5     AR                    FLOOD    $141,065,000
## 6     AS              FLASH FLOOD      $1,267,000
## 7     AZ           TROPICAL STORM    $200,000,000
## 8     CA             EXTREME COLD    $731,160,000
## 9     CO                     HAIL    $116,490,000
## 10    CT                     HAIL         $30,000
## 11    DC                  DROUGHT          $5,000
## 12    DE                  DROUGHT     $29,100,000
## 13    FL        HURRICANE/TYPHOON    $955,200,000
## 14    GA                  DROUGHT    $717,285,000
## 15    GM              MARINE HAIL              $0
## 16    GU                HURRICANE    $100,480,000
## 17    HI                HIGH WIND      $2,600,000
## 18    IA                  DROUGHT  $2,009,630,000
## 19    ID           TSTM WIND/HAIL      $6,002,000
## 20    IL              RIVER FLOOD  $5,012,500,000
## 21    IN                    FLOOD    $698,198,000
## 22    KS                     HAIL    $259,405,300
## 23    KY                  DROUGHT    $226,000,000
## 24    LA                  DROUGHT    $587,430,000
## 25    LC              MARINE HAIL              $0
## 26    LE              MARINE HAIL              $0
## 27    LH              MARINE HAIL              $0
## 28    LM              MARINE HAIL              $0
## 29    LO              MARINE HAIL              $0
## 30    LS              MARINE HAIL              $0
## 31    MA        THUNDERSTORM WIND      $1,250,000
## 32    MD                  DROUGHT     $99,720,000
## 33    ME              HEAVY RAINS        $500,000
## 34    MH                HIGH SURF              $0
## 35    MI                  DROUGHT    $150,000,000
## 36    MN                     HAIL    $140,700,800
## 37    MO                    FLOOD    $593,040,000
## 38    MS                ICE STORM  $5,000,060,000
## 39    MT                     HAIL     $34,345,000
## 40    NC                HURRICANE  $1,425,130,000
## 41    ND                     HAIL    $189,355,003
## 42    NE                     HAIL    $737,993,650
## 43    NH                    FLOOD        $200,000
## 44    NJ                  DROUGHT     $80,000,000
## 45    NM                  DROUGHT     $14,400,000
## 46    NV                    FLOOD      $6,000,000
## 47    NY                  DROUGHT    $100,200,000
## 48    OH                  DROUGHT    $200,000,000
## 49    OK                  DROUGHT  $1,097,040,000
## 50    OR                     HAIL     $36,028,000
## 51    PA                  DROUGHT    $539,400,000
## 52    PH       MARINE STRONG WIND              $0
## 53    PK         MARINE HIGH WIND              $0
## 54    PM               WATERSPOUT              $0
## 55    PR                HURRICANE    $451,000,000
## 56    PZ         MARINE HIGH WIND              $0
## 57    RI                 BLIZZARD              $0
## 58    SC                HURRICANE     $20,300,000
## 59    SD                     HAIL     $64,274,050
## 60    SL              MARINE HAIL              $0
## 61    ST             STRONG WINDS              $0
## 62    TN                TSTM WIND      $9,146,500
## 63    TX                  DROUGHT  $6,373,438,000
## 64    UT                HIGH WIND      $1,110,000
## 65    VA                  DROUGHT    $297,480,000
## 66    VI                  DROUGHT        $200,000
## 67    VT              FLASH FLOOD     $14,725,000
## 68    WA                     HAIL    $209,501,000
## 69    WI                    FLOOD    $466,888,500
## 70    WV                  DROUGHT     $19,746,000
## 71    WY                     HAIL      $1,881,200
## 72    XX MARINE THUNDERSTORM WIND              $0
```

We then visualize the property damage and crop damage per state:

```r
tmp_propdmg<-merge(top_propdmg_per_state, state_names)
map_data_propdmg<-merge(tmp_propdmg, all_states, by='region')

pr<-ggplot(map_data_propdmg, aes(map_id = region)) +
  geom_map(aes(fill = PROPERTYDAMAGEVALUE), map = all_states, color ="black") +
  expand_limits(x = all_states$long, y = all_states$lat) +
  theme(legend.position = "bottom",
        axis.ticks = element_blank(), 
        axis.title = element_blank(), 
        axis.text =  element_blank()) +
  scale_fill_gradient(low="white", high="red") +
  guides(fill = guide_colorbar(barwidth = 10, barheight = .5)) + 
  ggtitle("")

tmp_cropdmg<-merge(top_cropdmg_per_state, state_names)
map_data_cropdmg<-merge(tmp_cropdmg, all_states, by='region')

cr<-ggplot(map_data_cropdmg, aes(map_id = region)) +
  geom_map(aes(fill = CROPDAMAGEVALUE), map = all_states, color ="black") +
  expand_limits(x = all_states$long, y = all_states$lat) +
  theme(legend.position = "bottom",
        axis.ticks = element_blank(), 
        axis.title = element_blank(), 
        axis.text =  element_blank()) +
  scale_fill_gradient(low="white", high="red") +
  guides(fill = guide_colorbar(barwidth = 10, barheight = .5)) + 
  ggtitle("")
```


```r
grid.arrange(arrangeGrob(pr, cr, ncol=2, main="Property and Crop Damage per State"))
```

```
## Error: Discrete value supplied to continuous scale
```


## Results











