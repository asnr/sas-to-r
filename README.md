SAS to R guide
==============

Save data to disk
-----------------

```SAS
data save_lib.save_ds;
    set in_lib.in_ds;
run;
```

```r
# Here save_lib is a string representing a directory. '.RData' is sometimes replaced with '.rda'
save(in_lib, file=paste(save_lib, "save_ds.RData", sep=""))
```


`proc freq`
-----------

```SAS
proc freq data=mydata;
    tables myvar / nocol nopercent nocum;
run;
```

```r

```


#### sort by frequency ####
```SAS
proc freq order=freq data=mydata;
	tables myvar / nocol nopercent nocum;
run;
```

```r

```


#### with missing ####

```SAS
proc freq order=freq data=mydata;
    tables myvar / nocol nopercent nocum missing;
run;
```

```r

```


`proc means`
------------

```SAS
proc means data=mydata n mean std min max p25 median p75;
    var myvar;
run;
```

```r
mydata.myvar.describe()
```


#### more percentiles ####

```SAS
proc means data=mydata n mean std min max p1 p5 p10 p25 median p75 p90 p95 p99;
	var myvar;
run;
```

```r

```


`data` step
-----------

#### concatenate datasets ####

```SAS
data concatenated;
    set mydata1 mydata2;
run;
```

```r

```


`proc contents`
---------------

```SAS
proc contents data=mydata;
run;
```

```r

```

#### save output ####

```SAS
proc contents noprint data=mydata out=contents;
run;
```

```r

```


Misc
----

#### number of rows in a datastep ####

```SAS
* haha nice try. Try this for size: http://www2.sas.com/proceedings/sugi26/p095-26.pdf;
```

```r

```
