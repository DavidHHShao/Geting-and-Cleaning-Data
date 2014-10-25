---
title: "Getting and Cleaning Data - Week3 Note"
author: "David Honghao Shao"
date: "October 25, 2014"
output:
  html_document:
    toc: true
    theme: united
    highlight: zenburn
---

#Subsetting and sorting 
##Subsetting-Quick Review

```r
set.seed(13435)
X<-data.frame("var1"=sample(1:5), "var2"=sample(6:10),"var3"=sample(11:15))
X<-X[sample(1:5),]
X$var2[c(1,3)]=NA
X
```

```
##   var1 var2 var3
## 1    2   NA   15
## 4    1   10   11
## 2    3   NA   12
## 3    5    6   14
## 5    4    9   13
```


```r
X[,1]
```

```
## [1] 2 1 3 5 4
```


```r
X[,"var1"]
```

```
## [1] 2 1 3 5 4
```


```r
X[1:2,"var2"]
```

```
## [1] NA 10
```


```r
X[(X$var1<=3 & X$var3>11),]
```

```
##   var1 var2 var3
## 1    2   NA   15
## 2    3   NA   12
```


```r
X[(X$var1<=3 | X$var3>11),]
```

```
##   var1 var2 var3
## 1    2   NA   15
## 4    1   10   11
## 2    3   NA   12
## 3    5    6   14
## 5    4    9   13
```
"which" does not return indices of NAs

```r
X[which(X$var2>8),]
```

```
##   var1 var2 var3
## 4    1   10   11
## 5    4    9   13
```
##Sorting

```r
sort(X$var1)
```

```
## [1] 1 2 3 4 5
```

```r
sort(X$var1, decreasing=TRUE)
```

```
## [1] 5 4 3 2 1
```
"na.last" put NAs at the last of sorting

```r
sort(X$var2,na.last=TRUE)
```

```
## [1]  6  9 10 NA NA
```
order the dataframe by a particular variable

```r
X[order(X$var1),]
```

```
##   var1 var2 var3
## 4    1   10   11
## 1    2   NA   15
## 2    3   NA   12
## 5    4    9   13
## 3    5    6   14
```
order the dataframe by two particular variable, var1 first then var3

```r
X[order(X$var1,X$var3),]
```

```
##   var1 var2 var3
## 4    1   10   11
## 1    2   NA   15
## 2    3   NA   12
## 5    4    9   13
## 3    5    6   14
```
using "plyr" package to sort

```r
library(plyr)
arrange(X,var1)
```

```
##   var1 var2 var3
## 1    1   10   11
## 2    2   NA   15
## 3    3   NA   12
## 4    4    9   13
## 5    5    6   14
```


```r
arrange(X,desc(var1))
```

```
##   var1 var2 var3
## 1    5    6   14
## 2    4    9   13
## 3    3   NA   12
## 4    2   NA   15
## 5    1   10   11
```
##Adding rows and columns
adding columns

```r
X$var4<-rnorm(5)
X
```

```
##   var1 var2 var3       var4
## 1    2   NA   15  0.1875960
## 4    1   10   11  1.7869764
## 2    3   NA   12  0.4966936
## 3    5    6   14  0.0631830
## 5    4    9   13 -0.5361329
```
using"cbind" to add columns

```r
Y<-cbind(X,rnorm(5))
Y
```

```
##   var1 var2 var3       var4    rnorm(5)
## 1    2   NA   15  0.1875960  0.62578490
## 4    1   10   11  1.7869764 -2.45083750
## 2    3   NA   12  0.4966936  0.08909424
## 3    5    6   14  0.0631830  0.47838570
## 5    4    9   13 -0.5361329  1.00053336
```


```r
Z<-rnorm(5)
Y<-cbind(Z,X)
Y
```

```
##            Z var1 var2 var3       var4
## 1  0.5439561    2   NA   15  0.1875960
## 4  0.3304796    1   10   11  1.7869764
## 2 -0.9710917    3   NA   12  0.4966936
## 3 -0.9446847    5    6   14  0.0631830
## 5 -0.2967423    4    9   13 -0.5361329
```

using"rbind" to add rows

```r
Y<-rbind(X,rnorm(5))
Y
```

```
##       var1       var2       var3       var4
## 1 2.000000         NA 15.0000000  0.1875960
## 4 1.000000 10.0000000 11.0000000  1.7869764
## 2 3.000000         NA 12.0000000  0.4966936
## 3 5.000000  6.0000000 14.0000000  0.0631830
## 5 4.000000  9.0000000 13.0000000 -0.5361329
## 6 1.149505 -0.8705105 -0.9870139  0.3262530
```


```r
Z<-rnorm(5)
Y<-rbind(Z,X)
Y
```

```
##         var1       var2       var3       var4
## 1  -1.010516  0.6095613  0.5041528  1.3798872
## 11  2.000000         NA 15.0000000  0.1875960
## 4   1.000000 10.0000000 11.0000000  1.7869764
## 2   3.000000         NA 12.0000000  0.4966936
## 3   5.000000  6.0000000 14.0000000  0.0631830
## 5   4.000000  9.0000000 13.0000000 -0.5361329
```

#Summarizing data
[Data Source](https://data.baltimorecity.gov/Community/Restaurants/k5ry-ef3g)

##Getting the data from the web

```r
#if(!file.exists("./data")){dir.create("./data")}
#fileUrl <- "https://data.baltimorecity.gov/api/views/k5ry-ef3g/rows.csv?accessType=DOWNLOAD"
#download.file(fileUrl,destfile="./data/restaurants.csv",method="curl")
restData <- read.csv("./data/restaurants.csv")
```
##Look at a bit of the data
default is n=6

```r
head(restData,n=3)
```

```
##    name zipCode neighborhood councilDistrict policeDistrict
## 1   410   21206    Frankford               2   NORTHEASTERN
## 2  1919   21231  Fells Point               1   SOUTHEASTERN
## 3 SAUTE   21224       Canton               1   SOUTHEASTERN
##                          Location.1
## 1 4509 BELAIR ROAD\nBaltimore, MD\n
## 2    1919 FLEET ST\nBaltimore, MD\n
## 3   2844 HUDSON ST\nBaltimore, MD\n
```


```r
tail(restData,n=3)
```

```
##                  name zipCode  neighborhood councilDistrict policeDistrict
## 1325 ZINK'S CAF\u0090   21213 Belair-Edison              13   NORTHEASTERN
## 1326     ZISSIMOS BAR   21211       Hampden               7       NORTHERN
## 1327           ZORBAS   21224     Greektown               2   SOUTHEASTERN
##                              Location.1
## 1325 3300 LAWNVIEW AVE\nBaltimore, MD\n
## 1326      1023 36TH ST\nBaltimore, MD\n
## 1327  4710 EASTERN Ave\nBaltimore, MD\n
```
##Make summary

```r
summary(restData)
```

```
##                            name         zipCode             neighborhood
##  MCDONALD'S                  :   8   Min.   :-21226   Downtown    :128  
##  POPEYES FAMOUS FRIED CHICKEN:   7   1st Qu.: 21202   Fells Point : 91  
##  SUBWAY                      :   6   Median : 21218   Inner Harbor: 89  
##  KENTUCKY FRIED CHICKEN      :   5   Mean   : 21185   Canton      : 81  
##  BURGER KING                 :   4   3rd Qu.: 21226   Federal Hill: 42  
##  DUNKIN DONUTS               :   4   Max.   : 21287   Mount Vernon: 33  
##  (Other)                     :1293                    (Other)     :863  
##  councilDistrict       policeDistrict
##  Min.   : 1.000   SOUTHEASTERN:385   
##  1st Qu.: 2.000   CENTRAL     :288   
##  Median : 9.000   SOUTHERN    :213   
##  Mean   : 7.191   NORTHERN    :157   
##  3rd Qu.:11.000   NORTHEASTERN: 72   
##  Max.   :14.000   EASTERN     : 67   
##                   (Other)     :145   
##                         Location.1      
##  1101 RUSSELL ST\nBaltimore, MD\n:   9  
##  201 PRATT ST\nBaltimore, MD\n   :   8  
##  2400 BOSTON ST\nBaltimore, MD\n :   8  
##  300 LIGHT ST\nBaltimore, MD\n   :   5  
##  300 CHARLES ST\nBaltimore, MD\n :   4  
##  301 LIGHT ST\nBaltimore, MD\n   :   4  
##  (Other)                         :1289
```
##More in depth information

```r
str(restData)
```

```
## 'data.frame':	1327 obs. of  6 variables:
##  $ name           : Factor w/ 1277 levels "#1 CHINESE KITCHEN",..: 9 3 992 1 2 4 5 6 7 8 ...
##  $ zipCode        : int  21206 21231 21224 21211 21223 21218 21205 21211 21205 21231 ...
##  $ neighborhood   : Factor w/ 173 levels "Abell","Arlington",..: 53 52 18 66 104 33 98 133 98 157 ...
##  $ councilDistrict: int  2 1 1 14 9 14 13 7 13 1 ...
##  $ policeDistrict : Factor w/ 9 levels "CENTRAL","EASTERN",..: 3 6 6 4 8 3 6 4 6 6 ...
##  $ Location.1     : Factor w/ 1210 levels "1 BIDDLE ST\nBaltimore, MD\n",..: 835 334 554 755 492 537 505 530 507 569 ...
```
##Quantiles of quantitative variables

```r
quantile(restData$councilDistrict,na.rm=TRUE)
```

```
##   0%  25%  50%  75% 100% 
##    1    2    9   11   14
```


```r
quantile(restData$councilDistrict,probs=c(0.5,0.75,0.9))
```

```
## 50% 75% 90% 
##   9  11  12
```
##Make table
useNA="ifany" will tell the number of missing value
Default of "table"" will not tell the number of missing value

```r
table(restData$zipCode,useNA="ifany")
```

```
## 
## -21226  21201  21202  21205  21206  21207  21208  21209  21210  21211 
##      1    136    201     27     30      4      1      8     23     41 
##  21212  21213  21214  21215  21216  21217  21218  21220  21222  21223 
##     28     31     17     54     10     32     69      1      7     56 
##  21224  21225  21226  21227  21229  21230  21231  21234  21237  21239 
##    199     19     18      4     13    156    127      7      1      3 
##  21251  21287 
##      2      1
```
Two dimensional Table ( can be 3D )

```r
table(restData$councilDistrict,restData$zipCode)
```

```
##     
##      -21226 21201 21202 21205 21206 21207 21208 21209 21210 21211 21212
##   1       0     0    37     0     0     0     0     0     0     0     0
##   2       0     0     0     3    27     0     0     0     0     0     0
##   3       0     0     0     0     0     0     0     0     0     0     0
##   4       0     0     0     0     0     0     0     0     0     0    27
##   5       0     0     0     0     0     3     0     6     0     0     0
##   6       0     0     0     0     0     0     0     1    19     0     0
##   7       0     0     0     0     0     0     0     1     0    27     0
##   8       0     0     0     0     0     1     0     0     0     0     0
##   9       0     1     0     0     0     0     0     0     0     0     0
##   10      1     0     1     0     0     0     0     0     0     0     0
##   11      0   115   139     0     0     0     1     0     0     0     1
##   12      0    20    24     4     0     0     0     0     0     0     0
##   13      0     0     0    20     3     0     0     0     0     0     0
##   14      0     0     0     0     0     0     0     0     4    14     0
##     
##      21213 21214 21215 21216 21217 21218 21220 21222 21223 21224 21225
##   1      2     0     0     0     0     0     0     7     0   140     1
##   2      0     0     0     0     0     0     0     0     0    54     0
##   3      2    17     0     0     0     3     0     0     0     0     0
##   4      0     0     0     0     0     0     0     0     0     0     0
##   5      0     0    31     0     0     0     0     0     0     0     0
##   6      0     0    15     1     0     0     0     0     0     0     0
##   7      0     0     6     7    15     6     0     0     0     0     0
##   8      0     0     0     0     0     0     0     0     2     0     0
##   9      0     0     0     2     8     0     0     0    53     0     0
##   10     0     0     0     0     0     0     1     0     0     0    18
##   11     0     0     0     0     9     0     0     0     1     0     0
##   12    13     0     0     0     0    26     0     0     0     0     0
##   13    13     0     1     0     0     0     0     0     0     5     0
##   14     1     0     1     0     0    34     0     0     0     0     0
##     
##      21226 21227 21229 21230 21231 21234 21237 21239 21251 21287
##   1      0     0     0     1   124     0     0     0     0     0
##   2      0     0     0     0     0     0     1     0     0     0
##   3      0     1     0     0     0     7     0     0     2     0
##   4      0     0     0     0     0     0     0     3     0     0
##   5      0     0     0     0     0     0     0     0     0     0
##   6      0     0     0     0     0     0     0     0     0     0
##   7      0     0     0     0     0     0     0     0     0     0
##   8      0     2    13     0     0     0     0     0     0     0
##   9      0     0     0    11     0     0     0     0     0     0
##   10    18     0     0   133     0     0     0     0     0     0
##   11     0     0     0    11     0     0     0     0     0     0
##   12     0     0     0     0     2     0     0     0     0     0
##   13     0     1     0     0     1     0     0     0     0     1
##   14     0     0     0     0     0     0     0     0     0     0
```
##Check for missing values

```r
sum(is.na(restData$councilDistrict))
```

```
## [1] 0
```


```r
any(is.na(restData$councilDistrict))
```

```
## [1] FALSE
```


```r
all(restData$zipCode > 0)
```

```
## [1] FALSE
```
##Row and column sums

```r
colSums(is.na(restData))
```

```
##            name         zipCode    neighborhood councilDistrict 
##               0               0               0               0 
##  policeDistrict      Location.1 
##               0               0
```


```r
colSums(X)
```

```
##      var1      var2      var3      var4 
## 15.000000        NA 65.000000  1.998316
```


```r
rowSums(is.na(restData))
```

```
##    [1] 0 0 0 0 0 0 0 0 0 0 0 0 0 0 0 0 0 0 0 0 0 0 0 0 0 0 0 0 0 0 0 0 0 0
##   [35] 0 0 0 0 0 0 0 0 0 0 0 0 0 0 0 0 0 0 0 0 0 0 0 0 0 0 0 0 0 0 0 0 0 0
##   [69] 0 0 0 0 0 0 0 0 0 0 0 0 0 0 0 0 0 0 0 0 0 0 0 0 0 0 0 0 0 0 0 0 0 0
##  [103] 0 0 0 0 0 0 0 0 0 0 0 0 0 0 0 0 0 0 0 0 0 0 0 0 0 0 0 0 0 0 0 0 0 0
##  [137] 0 0 0 0 0 0 0 0 0 0 0 0 0 0 0 0 0 0 0 0 0 0 0 0 0 0 0 0 0 0 0 0 0 0
##  [171] 0 0 0 0 0 0 0 0 0 0 0 0 0 0 0 0 0 0 0 0 0 0 0 0 0 0 0 0 0 0 0 0 0 0
##  [205] 0 0 0 0 0 0 0 0 0 0 0 0 0 0 0 0 0 0 0 0 0 0 0 0 0 0 0 0 0 0 0 0 0 0
##  [239] 0 0 0 0 0 0 0 0 0 0 0 0 0 0 0 0 0 0 0 0 0 0 0 0 0 0 0 0 0 0 0 0 0 0
##  [273] 0 0 0 0 0 0 0 0 0 0 0 0 0 0 0 0 0 0 0 0 0 0 0 0 0 0 0 0 0 0 0 0 0 0
##  [307] 0 0 0 0 0 0 0 0 0 0 0 0 0 0 0 0 0 0 0 0 0 0 0 0 0 0 0 0 0 0 0 0 0 0
##  [341] 0 0 0 0 0 0 0 0 0 0 0 0 0 0 0 0 0 0 0 0 0 0 0 0 0 0 0 0 0 0 0 0 0 0
##  [375] 0 0 0 0 0 0 0 0 0 0 0 0 0 0 0 0 0 0 0 0 0 0 0 0 0 0 0 0 0 0 0 0 0 0
##  [409] 0 0 0 0 0 0 0 0 0 0 0 0 0 0 0 0 0 0 0 0 0 0 0 0 0 0 0 0 0 0 0 0 0 0
##  [443] 0 0 0 0 0 0 0 0 0 0 0 0 0 0 0 0 0 0 0 0 0 0 0 0 0 0 0 0 0 0 0 0 0 0
##  [477] 0 0 0 0 0 0 0 0 0 0 0 0 0 0 0 0 0 0 0 0 0 0 0 0 0 0 0 0 0 0 0 0 0 0
##  [511] 0 0 0 0 0 0 0 0 0 0 0 0 0 0 0 0 0 0 0 0 0 0 0 0 0 0 0 0 0 0 0 0 0 0
##  [545] 0 0 0 0 0 0 0 0 0 0 0 0 0 0 0 0 0 0 0 0 0 0 0 0 0 0 0 0 0 0 0 0 0 0
##  [579] 0 0 0 0 0 0 0 0 0 0 0 0 0 0 0 0 0 0 0 0 0 0 0 0 0 0 0 0 0 0 0 0 0 0
##  [613] 0 0 0 0 0 0 0 0 0 0 0 0 0 0 0 0 0 0 0 0 0 0 0 0 0 0 0 0 0 0 0 0 0 0
##  [647] 0 0 0 0 0 0 0 0 0 0 0 0 0 0 0 0 0 0 0 0 0 0 0 0 0 0 0 0 0 0 0 0 0 0
##  [681] 0 0 0 0 0 0 0 0 0 0 0 0 0 0 0 0 0 0 0 0 0 0 0 0 0 0 0 0 0 0 0 0 0 0
##  [715] 0 0 0 0 0 0 0 0 0 0 0 0 0 0 0 0 0 0 0 0 0 0 0 0 0 0 0 0 0 0 0 0 0 0
##  [749] 0 0 0 0 0 0 0 0 0 0 0 0 0 0 0 0 0 0 0 0 0 0 0 0 0 0 0 0 0 0 0 0 0 0
##  [783] 0 0 0 0 0 0 0 0 0 0 0 0 0 0 0 0 0 0 0 0 0 0 0 0 0 0 0 0 0 0 0 0 0 0
##  [817] 0 0 0 0 0 0 0 0 0 0 0 0 0 0 0 0 0 0 0 0 0 0 0 0 0 0 0 0 0 0 0 0 0 0
##  [851] 0 0 0 0 0 0 0 0 0 0 0 0 0 0 0 0 0 0 0 0 0 0 0 0 0 0 0 0 0 0 0 0 0 0
##  [885] 0 0 0 0 0 0 0 0 0 0 0 0 0 0 0 0 0 0 0 0 0 0 0 0 0 0 0 0 0 0 0 0 0 0
##  [919] 0 0 0 0 0 0 0 0 0 0 0 0 0 0 0 0 0 0 0 0 0 0 0 0 0 0 0 0 0 0 0 0 0 0
##  [953] 0 0 0 0 0 0 0 0 0 0 0 0 0 0 0 0 0 0 0 0 0 0 0 0 0 0 0 0 0 0 0 0 0 0
##  [987] 0 0 0 0 0 0 0 0 0 0 0 0 0 0 0 0 0 0 0 0 0 0 0 0 0 0 0 0 0 0 0 0 0 0
## [1021] 0 0 0 0 0 0 0 0 0 0 0 0 0 0 0 0 0 0 0 0 0 0 0 0 0 0 0 0 0 0 0 0 0 0
## [1055] 0 0 0 0 0 0 0 0 0 0 0 0 0 0 0 0 0 0 0 0 0 0 0 0 0 0 0 0 0 0 0 0 0 0
## [1089] 0 0 0 0 0 0 0 0 0 0 0 0 0 0 0 0 0 0 0 0 0 0 0 0 0 0 0 0 0 0 0 0 0 0
## [1123] 0 0 0 0 0 0 0 0 0 0 0 0 0 0 0 0 0 0 0 0 0 0 0 0 0 0 0 0 0 0 0 0 0 0
## [1157] 0 0 0 0 0 0 0 0 0 0 0 0 0 0 0 0 0 0 0 0 0 0 0 0 0 0 0 0 0 0 0 0 0 0
## [1191] 0 0 0 0 0 0 0 0 0 0 0 0 0 0 0 0 0 0 0 0 0 0 0 0 0 0 0 0 0 0 0 0 0 0
## [1225] 0 0 0 0 0 0 0 0 0 0 0 0 0 0 0 0 0 0 0 0 0 0 0 0 0 0 0 0 0 0 0 0 0 0
## [1259] 0 0 0 0 0 0 0 0 0 0 0 0 0 0 0 0 0 0 0 0 0 0 0 0 0 0 0 0 0 0 0 0 0 0
## [1293] 0 0 0 0 0 0 0 0 0 0 0 0 0 0 0 0 0 0 0 0 0 0 0 0 0 0 0 0 0 0 0 0 0 0
## [1327] 0
```


```r
rowSums(X)
```

```
##        1        4        2        3        5 
##       NA 23.78698       NA 25.06318 25.46387
```


```r
all(colSums(is.na(restData))==0)
```

```
## [1] TRUE
```
##Values with specific characteristics

```r
table(restData$zipCode %in% c("21212"))
```

```
## 
## FALSE  TRUE 
##  1299    28
```


```r
table(restData$zipCode %in% c("21212","21213"))
```

```
## 
## FALSE  TRUE 
##  1268    59
```


```r
filter<-restData[restData$zipCode %in% c("21212","21213"),]
head(filter)
```

```
##                           name zipCode              neighborhood
## 29           BAY ATLANTIC CLUB   21212                  Downtown
## 39                 BERMUDA BAR   21213             Broadway East
## 92                   ATWATER'S   21212 Chinquapin Park-Belvedere
## 111 BALTIMORE ESTONIAN SOCIETY   21213        South Clifton Park
## 187                   CAFE ZEN   21212                  Rosebank
## 220        CERIELLO FINE FOODS   21212 Chinquapin Park-Belvedere
##     councilDistrict policeDistrict                         Location.1
## 29               11        CENTRAL    206 REDWOOD ST\nBaltimore, MD\n
## 39               12        EASTERN    1801 NORTH AVE\nBaltimore, MD\n
## 92                4       NORTHERN 529 BELVEDERE AVE\nBaltimore, MD\n
## 111              12        EASTERN    1932 BELAIR RD\nBaltimore, MD\n
## 187               4       NORTHERN 438 BELVEDERE AVE\nBaltimore, MD\n
## 220               4       NORTHERN 529 BELVEDERE AVE\nBaltimore, MD\n
```
##Cross tabs

```r
data(UCBAdmissions)
DF = as.data.frame(UCBAdmissions)
```

```r
summary(DF)
```

```
##       Admit       Gender   Dept       Freq      
##  Admitted:12   Male  :12   A:4   Min.   :  8.0  
##  Rejected:12   Female:12   B:4   1st Qu.: 80.0  
##                            C:4   Median :170.0  
##                            D:4   Mean   :188.6  
##                            E:4   3rd Qu.:302.5  
##                            F:4   Max.   :512.0
```

```r
str(DF)
```

```
## 'data.frame':	24 obs. of  4 variables:
##  $ Admit : Factor w/ 2 levels "Admitted","Rejected": 1 2 1 2 1 2 1 2 1 2 ...
##  $ Gender: Factor w/ 2 levels "Male","Female": 1 1 2 2 1 1 2 2 1 1 ...
##  $ Dept  : Factor w/ 6 levels "A","B","C","D",..: 1 1 1 1 2 2 2 2 3 3 ...
##  $ Freq  : num  512 313 89 19 353 207 17 8 120 205 ...
```


```r
head(DF)
```

```
##      Admit Gender Dept Freq
## 1 Admitted   Male    A  512
## 2 Rejected   Male    A  313
## 3 Admitted Female    A   89
## 4 Rejected Female    A   19
## 5 Admitted   Male    B  353
## 6 Rejected   Male    B  207
```
two facors: Gender and Admit

```r
xt <- xtabs(Freq ~ Gender + Admit,data=DF)
xt
```

```
##         Admit
## Gender   Admitted Rejected
##   Male       1198     1493
##   Female      557     1278
```
##Flat tables

```r
str(warpbreaks)
```

```
## 'data.frame':	54 obs. of  3 variables:
##  $ breaks : num  26 30 54 25 70 52 51 26 67 18 ...
##  $ wool   : Factor w/ 2 levels "A","B": 1 1 1 1 1 1 1 1 1 1 ...
##  $ tension: Factor w/ 3 levels "L","M","H": 1 1 1 1 1 1 1 1 1 2 ...
```


```r
head(warpbreaks)
```

```
##   breaks wool tension
## 1     26    A       L
## 2     30    A       L
## 3     54    A       L
## 4     25    A       L
## 5     70    A       L
## 6     52    A       L
```


```r
warpbreaks$replicate <- rep(1:9, len = 54)
str(warpbreaks)
```

```
## 'data.frame':	54 obs. of  4 variables:
##  $ breaks   : num  26 30 54 25 70 52 51 26 67 18 ...
##  $ wool     : Factor w/ 2 levels "A","B": 1 1 1 1 1 1 1 1 1 1 ...
##  $ tension  : Factor w/ 3 levels "L","M","H": 1 1 1 1 1 1 1 1 1 2 ...
##  $ replicate: int  1 2 3 4 5 6 7 8 9 1 ...
```


```r
warpbreaks
```

```
##    breaks wool tension replicate
## 1      26    A       L         1
## 2      30    A       L         2
## 3      54    A       L         3
## 4      25    A       L         4
## 5      70    A       L         5
## 6      52    A       L         6
## 7      51    A       L         7
## 8      26    A       L         8
## 9      67    A       L         9
## 10     18    A       M         1
## 11     21    A       M         2
## 12     29    A       M         3
## 13     17    A       M         4
## 14     12    A       M         5
## 15     18    A       M         6
## 16     35    A       M         7
## 17     30    A       M         8
## 18     36    A       M         9
## 19     36    A       H         1
## 20     21    A       H         2
## 21     24    A       H         3
## 22     18    A       H         4
## 23     10    A       H         5
## 24     43    A       H         6
## 25     28    A       H         7
## 26     15    A       H         8
## 27     26    A       H         9
## 28     27    B       L         1
## 29     14    B       L         2
## 30     29    B       L         3
## 31     19    B       L         4
## 32     29    B       L         5
## 33     31    B       L         6
## 34     41    B       L         7
## 35     20    B       L         8
## 36     44    B       L         9
## 37     42    B       M         1
## 38     26    B       M         2
## 39     19    B       M         3
## 40     16    B       M         4
## 41     39    B       M         5
## 42     28    B       M         6
## 43     21    B       M         7
## 44     39    B       M         8
## 45     29    B       M         9
## 46     20    B       H         1
## 47     21    B       H         2
## 48     24    B       H         3
## 49     17    B       H         4
## 50     13    B       H         5
## 51     15    B       H         6
## 52     15    B       H         7
## 53     16    B       H         8
## 54     28    B       H         9
```


```r
warpbreaks$replicate <- rep(1:9, len = 54)
xt = xtabs(breaks ~.,data=warpbreaks)
xt
```

```
## , , replicate = 1
## 
##     tension
## wool  L  M  H
##    A 26 18 36
##    B 27 42 20
## 
## , , replicate = 2
## 
##     tension
## wool  L  M  H
##    A 30 21 21
##    B 14 26 21
## 
## , , replicate = 3
## 
##     tension
## wool  L  M  H
##    A 54 29 24
##    B 29 19 24
## 
## , , replicate = 4
## 
##     tension
## wool  L  M  H
##    A 25 17 18
##    B 19 16 17
## 
## , , replicate = 5
## 
##     tension
## wool  L  M  H
##    A 70 12 10
##    B 29 39 13
## 
## , , replicate = 6
## 
##     tension
## wool  L  M  H
##    A 52 18 43
##    B 31 28 15
## 
## , , replicate = 7
## 
##     tension
## wool  L  M  H
##    A 51 35 28
##    B 41 21 15
## 
## , , replicate = 8
## 
##     tension
## wool  L  M  H
##    A 26 30 15
##    B 20 39 16
## 
## , , replicate = 9
## 
##     tension
## wool  L  M  H
##    A 67 36 26
##    B 44 29 28
```


```r
ftable(xt)
```

```
##              replicate  1  2  3  4  5  6  7  8  9
## wool tension                                     
## A    L                 26 30 54 25 70 52 51 26 67
##      M                 18 21 29 17 12 18 35 30 36
##      H                 36 21 24 18 10 43 28 15 26
## B    L                 27 14 29 19 29 31 41 20 44
##      M                 42 26 19 16 39 28 21 39 29
##      H                 20 21 24 17 13 15 15 16 28
```
##Size of a data set

```r
fakeData = rnorm(1e5)
object.size(fakeData)
```

```
## 800040 bytes
```


```r
print(object.size(fakeData),units="Mb")
```

```
## 0.8 Mb
```
#Creating new variables
##Why create new variables?
- Often the raw data won't have a value you are looking for
- You will need to transform the data to get the values you would like
- Usually you will add those values to the data frames you are working with
- Common variables to create
    - Missingness indicators
    - "Cutting up" quantitative variables
    -  Applying transforms
##Getting the data from the web

```r
#if(!file.exists("./data")){dir.create("./data")}
#fileUrl <- "https://data.baltimorecity.gov/api/views/k5ry-ef3g/rows.csv?accessType=DOWNLOAD"
#download.file(fileUrl,destfile="./data/restaurants.csv",method="curl")
#restData <- read.csv("./data/restaurants.csv")
```
##Creating sequences
Sometimes you need an index for your data set

```r
s1 <- seq(1,10,by=2); 
s1
```

```
## [1] 1 3 5 7 9
```


```r
s2 <- seq(1,10,length=3);
s2
```

```
## [1]  1.0  5.5 10.0
```
Create index for variavle

```r
x <- c(1,3,8,25,100); 
seq(along = x)
```

```
## [1] 1 2 3 4 5
```
##Subsetting variables

```r
restData$nearMe = restData$neighborhood %in% c("Roland Park", "Homeland")
table(restData$nearMe)
```

```
## 
## FALSE  TRUE 
##  1314    13
```
##Creating binary variables
ifelse helps creat binary variables

```r
restData$zipWrong = ifelse(restData$zipCode < 0, TRUE, FALSE)
table(restData$zipWrong,restData$zipCode < 0)
```

```
##        
##         FALSE TRUE
##   FALSE  1326    0
##   TRUE      0    1
```

##Creating categorical variables

```r
restData$zipGroups = cut(restData$zipCode,breaks=quantile(restData$zipCode))
table(restData$zipGroups)
```

```
## 
## (-2.123e+04,2.12e+04]  (2.12e+04,2.122e+04] (2.122e+04,2.123e+04] 
##                   337                   375                   282 
## (2.123e+04,2.129e+04] 
##                   332
```


```r
table(restData$zipGroups,restData$zipCode)
```

```
##                        
##                         -21226 21201 21202 21205 21206 21207 21208 21209
##   (-2.123e+04,2.12e+04]      0   136   201     0     0     0     0     0
##   (2.12e+04,2.122e+04]       0     0     0    27    30     4     1     8
##   (2.122e+04,2.123e+04]      0     0     0     0     0     0     0     0
##   (2.123e+04,2.129e+04]      0     0     0     0     0     0     0     0
##                        
##                         21210 21211 21212 21213 21214 21215 21216 21217
##   (-2.123e+04,2.12e+04]     0     0     0     0     0     0     0     0
##   (2.12e+04,2.122e+04]     23    41    28    31    17    54    10    32
##   (2.122e+04,2.123e+04]     0     0     0     0     0     0     0     0
##   (2.123e+04,2.129e+04]     0     0     0     0     0     0     0     0
##                        
##                         21218 21220 21222 21223 21224 21225 21226 21227
##   (-2.123e+04,2.12e+04]     0     0     0     0     0     0     0     0
##   (2.12e+04,2.122e+04]     69     0     0     0     0     0     0     0
##   (2.122e+04,2.123e+04]     0     1     7    56   199    19     0     0
##   (2.123e+04,2.129e+04]     0     0     0     0     0     0    18     4
##                        
##                         21229 21230 21231 21234 21237 21239 21251 21287
##   (-2.123e+04,2.12e+04]     0     0     0     0     0     0     0     0
##   (2.12e+04,2.122e+04]      0     0     0     0     0     0     0     0
##   (2.122e+04,2.123e+04]     0     0     0     0     0     0     0     0
##   (2.123e+04,2.129e+04]    13   156   127     7     1     3     2     1
```
##Easier cutting

```r
library(Hmisc);
```

```
## Loading required package: grid
## Loading required package: survival
## Loading required package: splines
## Loading required package: Formula
## 
## Attaching package: 'Hmisc'
## 
## The following objects are masked from 'package:plyr':
## 
##     is.discrete, summarize
## 
## The following objects are masked from 'package:base':
## 
##     format.pval, round.POSIXt, trunc.POSIXt, units
```

```r
restData$zipGroups = cut2(restData$zipCode,g=4)
table(restData$zipGroups)
```

```
## 
## [-21226,21205) [ 21205,21220) [ 21220,21227) [ 21227,21287] 
##            338            375            300            314
```
##Creating factor variables

```r
restData$zcf <- factor(restData$zipCode)
restData$zcf[1:10]
```

```
##  [1] 21206 21231 21224 21211 21223 21218 21205 21211 21205 21231
## 32 Levels: -21226 21201 21202 21205 21206 21207 21208 21209 ... 21287
```


```r
class(restData$zcf)
```

```
## [1] "factor"
```
##Levels of factor variables
relevel: The levels of a factor are re-ordered so that the level specified by ref is first and the others are moved down. This is useful for contr.treatment contrasts which take the first level as the reference. 


```r
yesno <- sample(c("yes","no"),size=10,replace=TRUE)
yesnofac<- factor(yesno,levels=c("yes","no"))
yesnofac
```

```
##  [1] yes yes no  no  no  no  no  yes yes no 
## Levels: yes no
```


```r
yesnofac1<-relevel(yesnofac,ref="yes")
yesnofac1
```

```
##  [1] yes yes no  no  no  no  no  yes yes no 
## Levels: yes no
```


```r
yesnofac2<-relevel(yesnofac,ref="no")
yesnofac2
```

```
##  [1] yes yes no  no  no  no  no  yes yes no 
## Levels: no yes
```


```r
as.numeric(yesnofac1)
```

```
##  [1] 1 1 2 2 2 2 2 1 1 2
```


```r
as.numeric(yesnofac2)
```

```
##  [1] 2 2 1 1 1 1 1 2 2 1
```
##Cutting produces factor variables
Hmisc package used; g : numbers of grounps

```r
library(Hmisc);
restData$zipGroups = cut2(restData$zipCode,g=4)
table(restData$zipGroups)
```

```
## 
## [-21226,21205) [ 21205,21220) [ 21220,21227) [ 21227,21287] 
##            338            375            300            314
```

```r
str(restData)
```

```
## 'data.frame':	1327 obs. of  10 variables:
##  $ name           : Factor w/ 1277 levels "#1 CHINESE KITCHEN",..: 9 3 992 1 2 4 5 6 7 8 ...
##  $ zipCode        : int  21206 21231 21224 21211 21223 21218 21205 21211 21205 21231 ...
##  $ neighborhood   : Factor w/ 173 levels "Abell","Arlington",..: 53 52 18 66 104 33 98 133 98 157 ...
##  $ councilDistrict: int  2 1 1 14 9 14 13 7 13 1 ...
##  $ policeDistrict : Factor w/ 9 levels "CENTRAL","EASTERN",..: 3 6 6 4 8 3 6 4 6 6 ...
##  $ Location.1     : Factor w/ 1210 levels "1 BIDDLE ST\nBaltimore, MD\n",..: 835 334 554 755 492 537 505 530 507 569 ...
##  $ nearMe         : logi  FALSE FALSE FALSE FALSE FALSE FALSE ...
##  $ zipWrong       : logi  FALSE FALSE FALSE FALSE FALSE FALSE ...
##  $ zipGroups      : Factor w/ 4 levels "[-21226,21205)",..: 2 4 3 2 3 2 2 2 2 4 ...
##  $ zcf            : Factor w/ 32 levels "-21226","21201",..: 5 27 21 10 20 17 4 10 4 27 ...
```
##Using the mutate function 
mutate add the new factor variable and create a new data.frame

```r
library(Hmisc); 
library(plyr);
restData2 = mutate(restData,zipGroups=cut2(zipCode,g=4))
table(restData2$zipGroups)
```

```
## 
## [-21226,21205) [ 21205,21220) [ 21220,21227) [ 21227,21287] 
##            338            375            300            314
```


```r
str(restData2)
```

```
## 'data.frame':	1327 obs. of  10 variables:
##  $ name           : Factor w/ 1277 levels "#1 CHINESE KITCHEN",..: 9 3 992 1 2 4 5 6 7 8 ...
##  $ zipCode        : int  21206 21231 21224 21211 21223 21218 21205 21211 21205 21231 ...
##  $ neighborhood   : Factor w/ 173 levels "Abell","Arlington",..: 53 52 18 66 104 33 98 133 98 157 ...
##  $ councilDistrict: int  2 1 1 14 9 14 13 7 13 1 ...
##  $ policeDistrict : Factor w/ 9 levels "CENTRAL","EASTERN",..: 3 6 6 4 8 3 6 4 6 6 ...
##  $ Location.1     : Factor w/ 1210 levels "1 BIDDLE ST\nBaltimore, MD\n",..: 835 334 554 755 492 537 505 530 507 569 ...
##  $ nearMe         : logi  FALSE FALSE FALSE FALSE FALSE FALSE ...
##  $ zipWrong       : logi  FALSE FALSE FALSE FALSE FALSE FALSE ...
##  $ zipGroups      : Factor w/ 4 levels "[-21226,21205)",..: 2 4 3 2 3 2 2 2 2 4 ...
##  $ zcf            : Factor w/ 32 levels "-21226","21201",..: 5 27 21 10 20 17 4 10 4 27 ...
```
##Common transforms
- abs(x) absolute value
- sqrt(x) square root
- ceiling(x) ceiling(3.475) is 4
- floor(x) floor(3.475) is 3
- round(x,digits=n) roun(3.475,digits=2) is 3.48
- signif(x,digits=n) signif(3.475,digits=2) is 3.5
- cos(x), sin(x) etc.
- log(x) natural logarithm
- log2(x), log10(x) other common logs
- exp(x) exponentiating x

http://www.biostat.jhsph.edu/~ajaffe/lec_winterR/Lecture%202.pdf

http://statmethods.net/management/functions.html

##Notes and further reading

- A tutorial from the developer of plyr - http://plyr.had.co.nz/09-user/

- Andrew Jaffe's R notes http://www.biostat.jhsph.edu/~ajaffe/lec_winterR/Lecture%202.pdf

#Reshaping data
##The goal is tidy data
1. Each variable forms a column
2. Each observation forms a row
3. Each table/file stores data about one kind of observation (e.g. people/hospitals).

http://vita.had.co.nz/papers/tidy-data.pdf

[Leek, Taub, and Pineda 2011 PLoS One](http://www.plosone.org/article/info%3Adoi%2F10.1371%2Fjournal.pone.0026895)

##Start with reshaping

```r
library(reshape2)
str(mtcars)
```

```
## 'data.frame':	32 obs. of  11 variables:
##  $ mpg : num  21 21 22.8 21.4 18.7 18.1 14.3 24.4 22.8 19.2 ...
##  $ cyl : num  6 6 4 6 8 6 8 4 4 6 ...
##  $ disp: num  160 160 108 258 360 ...
##  $ hp  : num  110 110 93 110 175 105 245 62 95 123 ...
##  $ drat: num  3.9 3.9 3.85 3.08 3.15 2.76 3.21 3.69 3.92 3.92 ...
##  $ wt  : num  2.62 2.88 2.32 3.21 3.44 ...
##  $ qsec: num  16.5 17 18.6 19.4 17 ...
##  $ vs  : num  0 0 1 1 0 1 0 1 1 1 ...
##  $ am  : num  1 1 1 0 0 0 0 0 0 0 ...
##  $ gear: num  4 4 4 3 3 3 3 4 4 4 ...
##  $ carb: num  4 4 1 1 2 1 4 2 2 4 ...
```

```r
head(mtcars)
```

```
##                    mpg cyl disp  hp drat    wt  qsec vs am gear carb
## Mazda RX4         21.0   6  160 110 3.90 2.620 16.46  0  1    4    4
## Mazda RX4 Wag     21.0   6  160 110 3.90 2.875 17.02  0  1    4    4
## Datsun 710        22.8   4  108  93 3.85 2.320 18.61  1  1    4    1
## Hornet 4 Drive    21.4   6  258 110 3.08 3.215 19.44  1  0    3    1
## Hornet Sportabout 18.7   8  360 175 3.15 3.440 17.02  0  0    3    2
## Valiant           18.1   6  225 105 2.76 3.460 20.22  1  0    3    1
```
##Melting data frames
double the number of the observation due to measure.vars

```r
mtcars$carname <- rownames(mtcars)
carMelt <- melt(mtcars,id=c("carname","gear","cyl"),measure.vars=c("mpg","hp"))
str(carMelt)
```

```
## 'data.frame':	64 obs. of  5 variables:
##  $ carname : chr  "Mazda RX4" "Mazda RX4 Wag" "Datsun 710" "Hornet 4 Drive" ...
##  $ gear    : num  4 4 4 3 3 3 3 4 4 4 ...
##  $ cyl     : num  6 6 4 6 8 6 8 4 4 6 ...
##  $ variable: Factor w/ 2 levels "mpg","hp": 1 1 1 1 1 1 1 1 1 1 ...
##  $ value   : num  21 21 22.8 21.4 18.7 18.1 14.3 24.4 22.8 19.2 ...
```

```r
carMelt
```

```
##                carname gear cyl variable value
## 1            Mazda RX4    4   6      mpg  21.0
## 2        Mazda RX4 Wag    4   6      mpg  21.0
## 3           Datsun 710    4   4      mpg  22.8
## 4       Hornet 4 Drive    3   6      mpg  21.4
## 5    Hornet Sportabout    3   8      mpg  18.7
## 6              Valiant    3   6      mpg  18.1
## 7           Duster 360    3   8      mpg  14.3
## 8            Merc 240D    4   4      mpg  24.4
## 9             Merc 230    4   4      mpg  22.8
## 10            Merc 280    4   6      mpg  19.2
## 11           Merc 280C    4   6      mpg  17.8
## 12          Merc 450SE    3   8      mpg  16.4
## 13          Merc 450SL    3   8      mpg  17.3
## 14         Merc 450SLC    3   8      mpg  15.2
## 15  Cadillac Fleetwood    3   8      mpg  10.4
## 16 Lincoln Continental    3   8      mpg  10.4
## 17   Chrysler Imperial    3   8      mpg  14.7
## 18            Fiat 128    4   4      mpg  32.4
## 19         Honda Civic    4   4      mpg  30.4
## 20      Toyota Corolla    4   4      mpg  33.9
## 21       Toyota Corona    3   4      mpg  21.5
## 22    Dodge Challenger    3   8      mpg  15.5
## 23         AMC Javelin    3   8      mpg  15.2
## 24          Camaro Z28    3   8      mpg  13.3
## 25    Pontiac Firebird    3   8      mpg  19.2
## 26           Fiat X1-9    4   4      mpg  27.3
## 27       Porsche 914-2    5   4      mpg  26.0
## 28        Lotus Europa    5   4      mpg  30.4
## 29      Ford Pantera L    5   8      mpg  15.8
## 30        Ferrari Dino    5   6      mpg  19.7
## 31       Maserati Bora    5   8      mpg  15.0
## 32          Volvo 142E    4   4      mpg  21.4
## 33           Mazda RX4    4   6       hp 110.0
## 34       Mazda RX4 Wag    4   6       hp 110.0
## 35          Datsun 710    4   4       hp  93.0
## 36      Hornet 4 Drive    3   6       hp 110.0
## 37   Hornet Sportabout    3   8       hp 175.0
## 38             Valiant    3   6       hp 105.0
## 39          Duster 360    3   8       hp 245.0
## 40           Merc 240D    4   4       hp  62.0
## 41            Merc 230    4   4       hp  95.0
## 42            Merc 280    4   6       hp 123.0
## 43           Merc 280C    4   6       hp 123.0
## 44          Merc 450SE    3   8       hp 180.0
## 45          Merc 450SL    3   8       hp 180.0
## 46         Merc 450SLC    3   8       hp 180.0
## 47  Cadillac Fleetwood    3   8       hp 205.0
## 48 Lincoln Continental    3   8       hp 215.0
## 49   Chrysler Imperial    3   8       hp 230.0
## 50            Fiat 128    4   4       hp  66.0
## 51         Honda Civic    4   4       hp  52.0
## 52      Toyota Corolla    4   4       hp  65.0
## 53       Toyota Corona    3   4       hp  97.0
## 54    Dodge Challenger    3   8       hp 150.0
## 55         AMC Javelin    3   8       hp 150.0
## 56          Camaro Z28    3   8       hp 245.0
## 57    Pontiac Firebird    3   8       hp 175.0
## 58           Fiat X1-9    4   4       hp  66.0
## 59       Porsche 914-2    5   4       hp  91.0
## 60        Lotus Europa    5   4       hp 113.0
## 61      Ford Pantera L    5   8       hp 264.0
## 62        Ferrari Dino    5   6       hp 175.0
## 63       Maserati Bora    5   8       hp 335.0
## 64          Volvo 142E    4   4       hp 109.0
```
##Casting data frames

```r
library(reshape2)
cylData <- dcast(carMelt, cyl ~ variable);
```

```
## Aggregation function missing: defaulting to length
```

```r
cylData
```

```
##   cyl mpg hp
## 1   4  11 11
## 2   6   7  7
## 3   8  14 14
```


```r
cylData <- dcast(carMelt, cyl ~ variable,mean)
cylData
```

```
##   cyl      mpg        hp
## 1   4 26.66364  82.63636
## 2   6 19.74286 122.28571
## 3   8 15.10000 209.21429
```
##Averaging values

```r
head(InsectSprays)
```

```
##   count spray
## 1    10     A
## 2     7     A
## 3    20     A
## 4    14     A
## 5    14     A
## 6    12     A
```


```r
tapply(InsectSprays$count,InsectSprays$spray,sum)
```

```
##   A   B   C   D   E   F 
## 174 184  25  59  42 200
```
##Another way - split

```r
spIns =  split(InsectSprays$count,InsectSprays$spray)
# or spIns <- with(InsectSprays, split(count, spray))
spIns
```

```
## $A
##  [1] 10  7 20 14 14 12 10 23 17 20 14 13
## 
## $B
##  [1] 11 17 21 11 16 14 17 17 19 21  7 13
## 
## $C
##  [1] 0 1 7 2 3 1 2 1 3 0 1 4
## 
## $D
##  [1]  3  5 12  6  4  3  5  5  5  5  2  4
## 
## $E
##  [1] 3 5 3 5 3 6 1 1 3 2 6 4
## 
## $F
##  [1] 11  9 15 22 15 16 13 10 26 26 24 13
```
##Another way - apply

```r
sprCount = lapply(spIns,sum)
sprCount
```

```
## $A
## [1] 174
## 
## $B
## [1] 184
## 
## $C
## [1] 25
## 
## $D
## [1] 59
## 
## $E
## [1] 42
## 
## $F
## [1] 200
```
##Another way - combine

```r
unlist(sprCount)
```

```
##   A   B   C   D   E   F 
## 174 184  25  59  42 200
```


```r
sapply(spIns,sum)
```

```
##   A   B   C   D   E   F 
## 174 184  25  59  42 200
```
##Another way - plyr package
in  plyr package, it is "summarise" rather than "summarize"

```r
ddply(InsectSprays,.(spray),summarise,sum=sum(count))
```

```
##   spray sum
## 1     A 174
## 2     B 184
## 3     C  25
## 4     D  59
## 5     E  42
## 6     F 200
```
##Creating a new variable

```r
spraySums <- ddply(InsectSprays,.(spray),summarise,sum=ave(count,FUN=sum))
dim(spraySums)
```

```
## [1] 72  2
```

```r
spraySums
```

```
##    spray sum
## 1      A 174
## 2      A 174
## 3      A 174
## 4      A 174
## 5      A 174
## 6      A 174
## 7      A 174
## 8      A 174
## 9      A 174
## 10     A 174
## 11     A 174
## 12     A 174
## 13     B 184
## 14     B 184
## 15     B 184
## 16     B 184
## 17     B 184
## 18     B 184
## 19     B 184
## 20     B 184
## 21     B 184
## 22     B 184
## 23     B 184
## 24     B 184
## 25     C  25
## 26     C  25
## 27     C  25
## 28     C  25
## 29     C  25
## 30     C  25
## 31     C  25
## 32     C  25
## 33     C  25
## 34     C  25
## 35     C  25
## 36     C  25
## 37     D  59
## 38     D  59
## 39     D  59
## 40     D  59
## 41     D  59
## 42     D  59
## 43     D  59
## 44     D  59
## 45     D  59
## 46     D  59
## 47     D  59
## 48     D  59
## 49     E  42
## 50     E  42
## 51     E  42
## 52     E  42
## 53     E  42
## 54     E  42
## 55     E  42
## 56     E  42
## 57     E  42
## 58     E  42
## 59     E  42
## 60     E  42
## 61     F 200
## 62     F 200
## 63     F 200
## 64     F 200
## 65     F 200
## 66     F 200
## 67     F 200
## 68     F 200
## 69     F 200
## 70     F 200
## 71     F 200
## 72     F 200
```
split the count column by the spray column.

```r
count_by_spray <- with(InsectSprays, split(count, spray))
```
apply the statistic to each element of the list. Lets use the mean here.

```r
count_by_spray <- with(InsectSprays, split(count, spray))
mean_by_spray <- lapply(count_by_spray, mean)
unlist(mean_by_spray)
```

```
##         A         B         C         D         E         F 
## 14.500000 15.333333  2.083333  4.916667  3.500000 16.666667
```
##lit-apply-combine problems(S-A-C)
We can do even better than that however. tapply, aggregate and by all provide 
a one-function solution to these S-A-C problems.

```r
with(InsectSprays, tapply(count, spray, mean))
```

```
##         A         B         C         D         E         F 
## 14.500000 15.333333  2.083333  4.916667  3.500000 16.666667
```


```r
with(InsectSprays, by(count, spray, mean))
```

```
## spray: A
## [1] 14.5
## -------------------------------------------------------- 
## spray: B
## [1] 15.33333
## -------------------------------------------------------- 
## spray: C
## [1] 2.083333
## -------------------------------------------------------- 
## spray: D
## [1] 4.916667
## -------------------------------------------------------- 
## spray: E
## [1] 3.5
## -------------------------------------------------------- 
## spray: F
## [1] 16.66667
```

```r
aggregate(count ~ spray, InsectSprays, mean)
```

```
##   spray     count
## 1     A 14.500000
## 2     B 15.333333
## 3     C  2.083333
## 4     D  4.916667
## 5     E  3.500000
## 6     F 16.666667
```


```r
dlply(InsectSprays, .(spray), summarise, mean.count = mean(count))
```

```
## $A
##   mean.count
## 1       14.5
## 
## $B
##   mean.count
## 1   15.33333
## 
## $C
##   mean.count
## 1   2.083333
## 
## $D
##   mean.count
## 1   4.916667
## 
## $E
##   mean.count
## 1        3.5
## 
## $F
##   mean.count
## 1   16.66667
## 
## attr(,"split_type")
## [1] "data.frame"
## attr(,"split_labels")
##   spray
## 1     A
## 2     B
## 3     C
## 4     D
## 5     E
## 6     F
```
One tiny variation on the problem is when you want the output statistic vector to have the same length as the original input vectors. For this, there is the ave function (which provides mean as the default function).

```r
with(InsectSprays, ave(count, spray))
```

```
##  [1] 14.500000 14.500000 14.500000 14.500000 14.500000 14.500000 14.500000
##  [8] 14.500000 14.500000 14.500000 14.500000 14.500000 15.333333 15.333333
## [15] 15.333333 15.333333 15.333333 15.333333 15.333333 15.333333 15.333333
## [22] 15.333333 15.333333 15.333333  2.083333  2.083333  2.083333  2.083333
## [29]  2.083333  2.083333  2.083333  2.083333  2.083333  2.083333  2.083333
## [36]  2.083333  4.916667  4.916667  4.916667  4.916667  4.916667  4.916667
## [43]  4.916667  4.916667  4.916667  4.916667  4.916667  4.916667  3.500000
## [50]  3.500000  3.500000  3.500000  3.500000  3.500000  3.500000  3.500000
## [57]  3.500000  3.500000  3.500000  3.500000 16.666667 16.666667 16.666667
## [64] 16.666667 16.666667 16.666667 16.666667 16.666667 16.666667 16.666667
## [71] 16.666667 16.666667
```


##More information
- A tutorial from the developer of plyr - http://plyr.had.co.nz/09-user/
- A nice reshape tutorial http://www.slideshare.net/jeffreybreen/reshaping-data-in-r
- A good plyr primer - http://www.r-bloggers.com/a-quick-primer-on-split-apply-combine-problems/
- See also the functions
    - acast - for casting as multi-dimensional arrays
    - arrange - for faster reordering without using order() commands
    - mutate - adding new variables
    
#Merging data
##Peer review experiment data
[data source]("http://www.plosone.org/article/info:doi/10.1371/journal.pone.0026895")

```r
#if(!file.exists("./data")){dir.create("./data")}
#fileUrl1 = "https://dl.dropboxusercontent.com/u/7710864/data/reviews-apr29.csv"
#fileUrl2 = "https://dl.dropboxusercontent.com/u/7710864/data/solutions-apr29.csv"
##download.file(fileUrl1,destfile="./data/reviews.csv",method="curl")
#download.file(fileUrl2,destfile="./data/solutions.csv",method="curl")
reviews = read.csv("./data/reviews.csv");
solutions <- read.csv("./data/solutions.csv");
```


```r
#str(reviews)
head(reviews)
```

```
##   id solution_id reviewer_id      start       stop time_left accept
## 1  1           3          27 1304095698 1304095758      1754      1
## 2  2           4          22 1304095188 1304095206      2306      1
## 3  3           5          28 1304095276 1304095320      2192      1
## 4  4           1          26 1304095267 1304095423      2089      1
## 5  5          10          29 1304095456 1304095469      2043      1
## 6  6           2          29 1304095471 1304095513      1999      1
```

```r
#str(solutions)
head(solutions)
```

```
##   id problem_id subject_id      start       stop time_left answer
## 1  1        156         29 1304095119 1304095169      2343      B
## 2  2        269         25 1304095119 1304095183      2329      C
## 3  3         34         22 1304095127 1304095146      2366      C
## 4  4         19         23 1304095127 1304095150      2362      D
## 5  5        605         26 1304095127 1304095167      2345      A
## 6  6        384         27 1304095131 1304095270      2242      C
```
##Merging data - merge()
- Merges data frames
- Important parameters: x,y,by,by.x,by.y,all

```r
names(reviews)
```

```
## [1] "id"          "solution_id" "reviewer_id" "start"       "stop"       
## [6] "time_left"   "accept"
```

```r
names(solutions)
```

```
## [1] "id"         "problem_id" "subject_id" "start"      "stop"      
## [6] "time_left"  "answer"
```

```r
mergedData = merge(reviews,solutions,by.x="solution_id",by.y="id",all=TRUE)
head(mergedData)
```

```
##   solution_id id reviewer_id    start.x     stop.x time_left.x accept
## 1           1  4          26 1304095267 1304095423        2089      1
## 2           2  6          29 1304095471 1304095513        1999      1
## 3           3  1          27 1304095698 1304095758        1754      1
## 4           4  2          22 1304095188 1304095206        2306      1
## 5           5  3          28 1304095276 1304095320        2192      1
## 6           6 16          22 1304095303 1304095471        2041      1
##   problem_id subject_id    start.y     stop.y time_left.y answer
## 1        156         29 1304095119 1304095169        2343      B
## 2        269         25 1304095119 1304095183        2329      C
## 3         34         22 1304095127 1304095146        2366      C
## 4         19         23 1304095127 1304095150        2362      D
## 5        605         26 1304095127 1304095167        2345      A
## 6        384         27 1304095131 1304095270        2242      C
```
##Default - merge all common column names


```r
intersect(names(solutions),names(reviews))
```

```
## [1] "id"        "start"     "stop"      "time_left"
```

```r
mergedData2 = merge(reviews,solutions,all=TRUE)
head(mergedData2)
```

```
##   id      start       stop time_left solution_id reviewer_id accept
## 1  1 1304095119 1304095169      2343          NA          NA     NA
## 2  1 1304095698 1304095758      1754           3          27      1
## 3  2 1304095119 1304095183      2329          NA          NA     NA
## 4  2 1304095188 1304095206      2306           4          22      1
## 5  3 1304095127 1304095146      2366          NA          NA     NA
## 6  3 1304095276 1304095320      2192           5          28      1
##   problem_id subject_id answer
## 1        156         29      B
## 2         NA         NA   <NA>
## 3        269         25      C
## 4         NA         NA   <NA>
## 5         34         22      C
## 6         NA         NA   <NA>
```
##Using join in the plyr package
Faster, but less full featured - defaults to left join, see help file for more

```r
df1 = data.frame(id=sample(1:10),x=rnorm(10))
df2 = data.frame(id=sample(1:10),y=rnorm(10))
df1 
```

```
##    id          x
## 1   9  0.2365916
## 2   6  1.7473499
## 3   8 -1.0178761
## 4   7  1.0070288
## 5   2 -1.8963536
## 6   4  0.9995930
## 7   1  1.9963144
## 8   3 -0.7388428
## 9  10 -0.3393521
## 10  5  2.6998482
```

```r
df2
```

```
##    id           y
## 1   5 -0.75669479
## 2   8 -1.42659738
## 3   9 -0.30153092
## 4  10 -0.77924959
## 5   2 -1.06584640
## 6   4  0.07436382
## 7   3  2.04580330
## 8   7  0.64047066
## 9   1  1.16579466
## 10  6  0.92514533
```

```r
arrange(join(df1,df2),id)
```

```
## Joining by: id
```

```
##    id          x           y
## 1   1  1.9963144  1.16579466
## 2   2 -1.8963536 -1.06584640
## 3   3 -0.7388428  2.04580330
## 4   4  0.9995930  0.07436382
## 5   5  2.6998482 -0.75669479
## 6   6  1.7473499  0.92514533
## 7   7  1.0070288  0.64047066
## 8   8 -1.0178761 -1.42659738
## 9   9  0.2365916 -0.30153092
## 10 10 -0.3393521 -0.77924959
```

```r
df1 = data.frame(x=rnorm(10),id=sample(1:10))
df2 = data.frame(id=sample(1:10),y=rnorm(10))
arrange(join(df1,df2),id)
```

```
## Joining by: id
```

```
##             x id          y
## 1   2.8547047  1 -0.6123399
## 2   1.5945879  2 -0.5200716
## 3   0.9344478  3  0.9849817
## 4   0.8899654  4  1.0923723
## 5   1.5570227  5 -0.9235868
## 6   0.9846910  6 -0.5563688
## 7  -0.7502527  7 -1.1863767
## 8  -2.9943179  8  1.1704015
## 9   1.6631246  9  1.0485269
## 10  0.8813762 10 -0.1691039
```

```r
df1 = data.frame(x=rnorm(10),id1=sample(1:10),id2=sample(1:10))
df2 = data.frame(y=rnorm(10),id1=sample(1:10),id2=sample(1:10))
df1
```

```
##              x id1 id2
## 1   1.04991074   9   4
## 2   1.34800556   3   7
## 3  -0.48145232   8   3
## 4  -0.07217816   4   6
## 5   1.06494765   2   8
## 6   1.20169255   6   9
## 7  -0.59426347  10   1
## 8   0.64950264   5   2
## 9  -0.59950734   7  10
## 10  0.07266156   1   5
```

```r
df2
```

```
##                y id1 id2
## 1   1.0707399659   6   1
## 2  -0.6891144569   4   6
## 3   0.5050501980  10   7
## 4   0.6564025537   8   2
## 5   1.7752005169   1   8
## 6  -1.0688107513   7   4
## 7   0.5965955546   5  10
## 8  -0.0002946376   3   5
## 9   1.1724617866   2   3
## 10  0.3380950122   9   9
```

```r
arrange(join(df1,df2),id1)
```

```
## Joining by: id1, id2
```

```
##              x id1 id2          y
## 1   0.07266156   1   5         NA
## 2   1.06494765   2   8         NA
## 3   1.34800556   3   7         NA
## 4  -0.07217816   4   6 -0.6891145
## 5   0.64950264   5   2         NA
## 6   1.20169255   6   9         NA
## 7  -0.59950734   7  10         NA
## 8  -0.48145232   8   3         NA
## 9   1.04991074   9   4         NA
## 10 -0.59426347  10   1         NA
```
##If you have multiple data frames

```r
df1 = data.frame(id=sample(1:10),x=rnorm(10))
df2 = data.frame(id=sample(1:10),y=rnorm(10))
df3 = data.frame(id=sample(1:10),z=rnorm(10))
dfList = list(df1,df2,df3)
join_all(dfList)
```

```
## Joining by: id
## Joining by: id
```

```
##    id           x          y           z
## 1   4  0.68063318  0.1099471  0.83794605
## 2   6  1.12868346  0.0337646 -0.28656824
## 3   8  0.02381784  0.2954425 -0.36325001
## 4   1  0.03778781  0.3899207  0.26385292
## 5  10 -0.53215032  1.3141084 -0.10923453
## 6   5 -0.87853245  0.3780575  0.84236394
## 7   7  0.69541607 -0.9916345 -0.09021139
## 8   9  0.01567956  1.2852661 -0.24384926
## 9   2 -0.19296261  0.6815632  0.47859271
## 10  3 -0.04788990  0.2042565 -2.11600535
```
##More on merging data
- The quick R data merging page - http://www.statmethods.net/management/merging.html
- plyr information - http://plyr.had.co.nz/
- Types of joins - http://en.wikipedia.org/wiki/Join_(SQL)


