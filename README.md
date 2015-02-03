SAS to R guide
==============

Reading instructions: 
*   under each subheading, the first code block is SAS and the second code block is the R translation;
*   ctrl+F is your friend.


Save data to disk
-----------------

```SAS
data save_lib.save_ds;
    set in_lib.in_ds;
run;
```

```r
# Here save_lib is a string representing a directory. '.rda' is 
# sometimes used instead of '.RData'
save(in_ds, file=paste0(save_lib, "save_ds.RData"))
# To load in_ds back into the environment
load(paste0(save_lib, "save_ds.RData"))
```


`proc freq`
-----------

```SAS
proc freq data=mydata;
    tables myvar / nocol nopercent nocum;
run;
```

```r
table(mydata$myvar)
```


#### sort by frequency ####
```SAS
proc freq order=freq data=mydata;
	tables myvar / nocol nopercent nocum;
run;
```

```r
sort(table(mydata$myvar), decreasing=TRUE)
```


#### with missing ####

```SAS
proc freq data=mydata;
    tables myvar / nocol nopercent nocum missing;
run;
```

```r
# only displays NA count if positive, to always include use "always"
table(mydata$myvar, useNA="ifany")
```


#### percentages, missing & sorted by frequency ####

```SAS
proc freq data=mydata;
    tables myvar / nocol nopercent nocum missing;
run;
```

```r
# no concise way to combine counts w/ percentages in core R
counts = sort(table(mydata$myvar, useNA="ifany"), decreasing=True)
counts
prop.table(counts)

# we can do better with dplyr
library(dplyr)
pfreq <- function(...) {
    group_by(...) %>%
        summarise(n=n()) %>%
        mutate(perc=paste0(round(100 * n/sum(n), 1), "%")) %>%
        arrange(desc(n))
}
pfreq(mydata, myvar)
```


`proc means`
------------

```SAS
proc means data=mydata min p25 median mean p75 max;
    var myvar;
run;
```

```r
summary(mydata$myvar)
```


#### more percentiles ####

```SAS
proc means data=mydata n mean std min max p1 p5 p10 p25 median p75 p90 p95 p99;
	var myvar;
run;
```

```r

```


`proc sort`
-----------

```SAS
proc sort data=in_data out=out_data; by myvar1 descending myvar2; run;
```

```r
out_data = in_data[with(in_data, order(myvar1, -myvar2)), ]

# with dplyr
library(dplyr)
out_data = arrange(in_data, myvar1, desc(myvar2))
```


#### find duplicated rows ####

```SAS
proc sort data=mydata NODUPKEY out=_null_ dupout=dups; by _all_; run;
```

```r
dups = duplicated(mydata)
```


#### find duplicated values ####

```SAS
proc sort data=mydata NODUPKEY out=_null_ dupout=dups; by myvar; run;
```

```r
dups = duplicated(mydata$myvar)
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
# Note that if a factor variable in the two dataframes has different
# levels, then rbind will take the union of these levels
# while row_bind will coerce the variable to string.
concatenated = rbind(mydata1, mydata2)

# or with dplyr
library(dplyr);
concatenated = row_bind(mydata1, mydata2)
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
nrow(mydata)  # or NROW(mydata)
```
