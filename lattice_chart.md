# my little reminder for some aspects of the lattice charts


```r
df1 <- data.frame(l = rep(letters[1:10], 2), v1 = abs(rnorm(20)) * 10, f = c(rep(c("f1"), 
    10), rep(c("f2"), 10)))
df1
```

```
##    l      v1  f
## 1  a  0.2239 f1
## 2  b  1.5860 f1
## 3  c  5.2975 f1
## 4  d 14.8243 f1
## 5  e 22.0825 f1
## 6  f 10.1089 f1
## 7  g 15.7477 f1
## 8  h 16.8419 f1
## 9  i  4.7264 f1
## 10 j  7.9542 f1
## 11 a  2.4553 f2
## 12 b  8.6045 f2
## 13 c  1.5446 f2
## 14 d  8.0037 f2
## 15 e  1.9034 f2
## 16 f  4.7791 f2
## 17 g 10.2953 f2
## 18 h  0.1863 f2
## 19 i 17.2054 f2
## 20 j 13.2371 f2
```


### plain bare bone chart

```r
require(lattice)
```

```
## Loading required package: lattice
```

```r
xyplot(v1 ~ l | f, data = df1)
```

![plot of chunk unnamed-chunk-2](figure/unnamed-chunk-2.png) 


### use groups instread of conditioning | 

```r
xyplot(v1 ~ l, groups = f, data = df1)
```

![plot of chunk unnamed-chunk-3](figure/unnamed-chunk-3.png) 


### failed attempt to align the grid lines with x labels
#### to note panel.grid() is not working properly (but just sometime!)


```r

xyplot(v1~l, groups=f, data=df1,
                 
       panel=function(...){
         
         panel.grid(v=-1,h=-1,col = "grey", ...)
         
         panel.xyplot(...)
       }
       
       )
```

![plot of chunk unnamed-chunk-4](figure/unnamed-chunk-4.png) 

```r
       
```

### tweak the alignment of grid lines with x labels by using panel.abline()


```r
xyplot(v1~l, groups=f, data=df1,
                 
       panel=function(...){
         
         panel.grid(v=0,h=-1,col = "grey", ...)
         panel.abline(v=1:10,col = "grey", lwd = 1, lty = 2)
         panel.xyplot(...)
       }
       
       )
```

![plot of chunk unnamed-chunk-5](figure/unnamed-chunk-5.png) 


### further chart customization 
- color and size of points (see par.settings and simpleTheme)
- x axis labels (see scale, capital letters now)
- legend (see auto.key)


```r
xyplot(v1~l, groups=f, data=df1,
             
       par.settings=simpleTheme(col.points=c("red","green"), cex=1.2, pch=19),       
       scale = list(x = list(at = 1:10,labels = LETTERS[1:10], cex = 0.8)),
       auto.key = list(points=TRUE,columns=2, position="top"),
       
       panel=function(...){
         
         panel.grid(v=0,h=-1,col = "grey", ...)
         panel.abline(v=1:10,col = "grey", lwd = 1, lty = 2)
         panel.xyplot(...)
       }
       
       )
```

![plot of chunk unnamed-chunk-6](figure/unnamed-chunk-6.png) 

### dealing with dates on the x axis

#### define a new dataframe

```r
df2 <- data.frame(date = seq(as.Date("2014/01/01"), as.Date("2014/01/31"), by = "day"), 
    v1 = runif(31) * 10)
```


#### x labels as default

```r
xyplot(v1 ~ date, data = df2)
```

![plot of chunk unnamed-chunk-8](figure/unnamed-chunk-8.png) 

#### one label for each original timestamp and aligned grid line

```r
mydate<-df2$date
xyplot(v1 ~ date, data = df2,
       
       panel = function(...) {
         panel.grid(h = -1, v = 0, col = "grey", lwd = 1, lty = 1)
         panel.abline(v = df2$date, col = "grey", lwd = 1, lty = 1)
         panel.xyplot(...)
         },
       scale = list(x = list(at = mydate, labels = format(mydate, "%d-%b-%y"), cex = 0.8, rot=90)) 
       )
```

![plot of chunk unnamed-chunk-9](figure/unnamed-chunk-9.png) 


#### customized coarser labels and aligned grid line


```r
nlab <- 6 # choose number of labels
mydate2 <- seq(from = min(df2$date), to = max(df2$date), length.out = nlab)

xyplot(v1 ~ date, data = df2,
       
       panel = function(...) {
         panel.grid(h = -1, v = 0, col = "grey", lwd = 1, lty = 1)
         panel.abline(v = df2$date, col = "grey", lwd = 1, lty = 1)
         panel.xyplot(...)
       },
       scale = list(x = list(at = mydate2, labels = format(mydate2, "%d-%b-%y"), cex = 0.8, rot=90)) 
)
```

![plot of chunk unnamed-chunk-10](figure/unnamed-chunk-10.png) 




