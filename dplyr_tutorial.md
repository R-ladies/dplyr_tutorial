<style>
  .reveal pre {
    font-size: 13pt;
  }
  .reveal section p {
    font-size: 32pt;
  }
  .reveal div {
    font-size: 30pt;
  }
  .reveal h3 {
    color: #484848;
    font-weight: 150%;
  }
</style>

Faster Data Manipulation with dplyr
========================================================
author: Gabriela de Queiroz
date: 
autosize: false
font-family: 'Helvetica'
width: 1340
height: 900


Before talking about dplyr let's ...
========================================================
Load the packages


```r
library(dplyr)
library(nycflights13)
```

========================================================

Take a look at the data we'll be using


```r
flights
```

```
Source: local data frame [336,776 x 16]

    year month   day dep_time dep_delay arr_time arr_delay carrier tailnum
   (int) (int) (int)    (int)     (dbl)    (int)     (dbl)   (chr)   (chr)
1   2013     1     1      517         2      830        11      UA  N14228
2   2013     1     1      533         4      850        20      UA  N24211
3   2013     1     1      542         2      923        33      AA  N619AA
4   2013     1     1      544        -1     1004       -18      B6  N804JB
5   2013     1     1      554        -6      812       -25      DL  N668DN
6   2013     1     1      554        -4      740        12      UA  N39463
7   2013     1     1      555        -5      913        19      B6  N516JB
8   2013     1     1      557        -3      709       -14      EV  N829AS
9   2013     1     1      557        -3      838        -8      B6  N593JB
10  2013     1     1      558        -2      753         8      AA  N3ALAA
..   ...   ...   ...      ...       ...      ...       ...     ...     ...
Variables not shown: flight (int), origin (chr), dest (chr), air_time
  (dbl), distance (dbl), hour (dbl), minute (dbl)
```

========================================================
## What can we see? 

- How many observations and variables?

- Can you notice any difference from the usual way of importing data to R? 

========================================================
Description of the data


```r
?flights
```

========================================================
# Meet the new pipe operator

<br></br>
## Here’s what it looks like: `%>%`. 
<br></br>
The RStudio shortcut is: Ctrl + Shift + M (Windows), Cmd + Shift + M (Mac).
<br></br>
## In this tutorial I'll be using pipe all the time!

========================================================
## Pipe operator example


```r
flights %>% 
	head()
```

```
Source: local data frame [6 x 16]

   year month   day dep_time dep_delay arr_time arr_delay carrier tailnum
  (int) (int) (int)    (int)     (dbl)    (int)     (dbl)   (chr)   (chr)
1  2013     1     1      517         2      830        11      UA  N14228
2  2013     1     1      533         4      850        20      UA  N24211
3  2013     1     1      542         2      923        33      AA  N619AA
4  2013     1     1      544        -1     1004       -18      B6  N804JB
5  2013     1     1      554        -6      812       -25      DL  N668DN
6  2013     1     1      554        -4      740        12      UA  N39463
Variables not shown: flight (int), origin (chr), dest (chr), air_time
  (dbl), distance (dbl), hour (dbl), minute (dbl)
```

========================================================
## Why do we use dplyr?

- Great for data exploration and transformation;
- Intuitive to write and easy to read;
- Fast on data frames.

========================================================
## The 5 main dplyr functions are all *single verbs*: 

1) `filter()` - pick observations by their values

2) `arrange()` - reorder the rows

3) `select()` - pick variables by their names

4) `mutate()` - create new variables with functions of existing variables

5) `summarise()` - collapse many values down to a single summary

========================================================
## 1) Filter rows with `filter()`


```r
flights %>% 
	filter(month == 2, day == 15)
```

```
Source: local data frame [954 x 16]

    year month   day dep_time dep_delay arr_time arr_delay carrier tailnum
   (int) (int) (int)    (int)     (dbl)    (int)     (dbl)   (chr)   (chr)
1   2013     2    15        3         5      503        25      B6  N509JB
2   2013     2    15        6       171       48       154      EV  N11535
3   2013     2    15        6        76      117        72      B6  N178JB
4   2013     2    15       22       112      510       118      B6  N505JB
5   2013     2    15       36        44      607        90      B6  N503JB
6   2013     2    15      454        -6      647        -1      US  N538UW
7   2013     2    15      516         1      808        -6      UA  N816UA
8   2013     2    15      530         0      822        -9      UA  N76503
9   2013     2    15      536        -9     1033        10      B6  N729JB
10  2013     2    15      540         0      855         5      AA  N360AA
..   ...   ...   ...      ...       ...      ...       ...     ...     ...
Variables not shown: flight (int), origin (chr), dest (chr), air_time
  (dbl), distance (dbl), hour (dbl), minute (dbl)
```

========================================================


```r
flights %>% 
	filter(month == 1 | month == 2)
```

```
Source: local data frame [51,955 x 16]

    year month   day dep_time dep_delay arr_time arr_delay carrier tailnum
   (int) (int) (int)    (int)     (dbl)    (int)     (dbl)   (chr)   (chr)
1   2013     1     1      517         2      830        11      UA  N14228
2   2013     1     1      533         4      850        20      UA  N24211
3   2013     1     1      542         2      923        33      AA  N619AA
4   2013     1     1      544        -1     1004       -18      B6  N804JB
5   2013     1     1      554        -6      812       -25      DL  N668DN
6   2013     1     1      554        -4      740        12      UA  N39463
7   2013     1     1      555        -5      913        19      B6  N516JB
8   2013     1     1      557        -3      709       -14      EV  N829AS
9   2013     1     1      557        -3      838        -8      B6  N593JB
10  2013     1     1      558        -2      753         8      AA  N3ALAA
..   ...   ...   ...      ...       ...      ...       ...     ...     ...
Variables not shown: flight (int), origin (chr), dest (chr), air_time
  (dbl), distance (dbl), hour (dbl), minute (dbl)
```

========================================================

### To select rows by position, use `slice()`:



```r
flights %>% 
	slice(1:10)
```

```
Source: local data frame [10 x 16]

    year month   day dep_time dep_delay arr_time arr_delay carrier tailnum
   (int) (int) (int)    (int)     (dbl)    (int)     (dbl)   (chr)   (chr)
1   2013     1     1      517         2      830        11      UA  N14228
2   2013     1     1      533         4      850        20      UA  N24211
3   2013     1     1      542         2      923        33      AA  N619AA
4   2013     1     1      544        -1     1004       -18      B6  N804JB
5   2013     1     1      554        -6      812       -25      DL  N668DN
6   2013     1     1      554        -4      740        12      UA  N39463
7   2013     1     1      555        -5      913        19      B6  N516JB
8   2013     1     1      557        -3      709       -14      EV  N829AS
9   2013     1     1      557        -3      838        -8      B6  N593JB
10  2013     1     1      558        -2      753         8      AA  N3ALAA
Variables not shown: flight (int), origin (chr), dest (chr), air_time
  (dbl), distance (dbl), hour (dbl), minute (dbl)
```

========================================================
## 2) Arrange rows with `arrange()`


```r
flights %>% 
arrange(arr_time)
```

```
Source: local data frame [336,776 x 16]

    year month   day dep_time dep_delay arr_time arr_delay carrier tailnum
   (int) (int) (int)    (int)     (dbl)    (int)     (dbl)   (chr)   (chr)
1   2013     1     2     2130         0        1       -17      B6  N804JB
2   2013     1    11     2157       117        1       113      EV  N21537
3   2013     1    11     2253         4        1         4      B6  N236JB
4   2013     1    14     2122        -8        1        -1      B6  N625JB
5   2013     1    14     2246        -4        1        -6      B6  N249JB
6   2013     1    15     2304        19        1         4      B6  N779JB
7   2013     1    16     2018        -7        1        32      UA  N505UA
8   2013     1    16     2303        18        1         4      B6  N608JB
9   2013     1    19     2107        -3        1         6      B6  N510JB
10  2013     1    22     2246        -3        1         4      B6  N306JB
..   ...   ...   ...      ...       ...      ...       ...     ...     ...
Variables not shown: flight (int), origin (chr), dest (chr), air_time
  (dbl), distance (dbl), hour (dbl), minute (dbl)
```

========================================================

```r
flights %>% 
arrange(desc(arr_delay))
```

```
Source: local data frame [336,776 x 16]

    year month   day dep_time dep_delay arr_time arr_delay carrier tailnum
   (int) (int) (int)    (int)     (dbl)    (int)     (dbl)   (chr)   (chr)
1   2013     1     9      641      1301     1242      1272      HA  N384HA
2   2013     6    15     1432      1137     1607      1127      MQ  N504MQ
3   2013     1    10     1121      1126     1239      1109      MQ  N517MQ
4   2013     9    20     1139      1014     1457      1007      AA  N338AA
5   2013     7    22      845      1005     1044       989      MQ  N665MQ
6   2013     4    10     1100       960     1342       931      DL  N959DL
7   2013     3    17     2321       911      135       915      DL  N927DA
8   2013     7    22     2257       898      121       895      DL  N6716C
9   2013    12     5      756       896     1058       878      AA  N5DMAA
10  2013     5     3     1133       878     1250       875      MQ  N523MQ
..   ...   ...   ...      ...       ...      ...       ...     ...     ...
Variables not shown: flight (int), origin (chr), dest (chr), air_time
  (dbl), distance (dbl), hour (dbl), minute (dbl)
```

========================================================
## 3) Select columns with `select()`


```r
flights %>% 
select(year, month, day)
```

```
Source: local data frame [336,776 x 3]

    year month   day
   (int) (int) (int)
1   2013     1     1
2   2013     1     1
3   2013     1     1
4   2013     1     1
5   2013     1     1
6   2013     1     1
7   2013     1     1
8   2013     1     1
9   2013     1     1
10  2013     1     1
..   ...   ...   ...
```

========================================================

**Example**: How can we select all columns between year and day (inclusive)?

========================================================


```r
flights %>% 
	select(year:day)
```

```
Source: local data frame [336,776 x 3]

    year month   day
   (int) (int) (int)
1   2013     1     1
2   2013     1     1
3   2013     1     1
4   2013     1     1
5   2013     1     1
6   2013     1     1
7   2013     1     1
8   2013     1     1
9   2013     1     1
10  2013     1     1
..   ...   ...   ...
```

========================================================

**Example**: How can we select all columns ***except*** those from year to day (inclusive)?

========================================================


```r
flights %>% 
	select(-(year:day))
```

```
Source: local data frame [336,776 x 13]

   dep_time dep_delay arr_time arr_delay carrier tailnum flight origin
      (int)     (dbl)    (int)     (dbl)   (chr)   (chr)  (int)  (chr)
1       517         2      830        11      UA  N14228   1545    EWR
2       533         4      850        20      UA  N24211   1714    LGA
3       542         2      923        33      AA  N619AA   1141    JFK
4       544        -1     1004       -18      B6  N804JB    725    JFK
5       554        -6      812       -25      DL  N668DN    461    LGA
6       554        -4      740        12      UA  N39463   1696    EWR
7       555        -5      913        19      B6  N516JB    507    EWR
8       557        -3      709       -14      EV  N829AS   5708    LGA
9       557        -3      838        -8      B6  N593JB     79    JFK
10      558        -2      753         8      AA  N3ALAA    301    LGA
..      ...       ...      ...       ...     ...     ...    ...    ...
Variables not shown: dest (chr), air_time (dbl), distance (dbl), hour
  (dbl), minute (dbl)
```

========================================================
There are a number of helper functions you can use within select(), like: 

- `starts_with()`
- `ends_with()`
- `matches()` 
- `contains()`

**See ?select for more details.**

========================================================
### You can rename variables with `select()`


```r
flights %>% 
	select(tail_num = tailnum)
```

```
Source: local data frame [336,776 x 1]

   tail_num
      (chr)
1    N14228
2    N24211
3    N619AA
4    N804JB
5    N668DN
6    N39463
7    N516JB
8    N829AS
9    N593JB
10   N3ALAA
..      ...
```

========================================================
### But sometimes you just want to rename, without droping all the other variables


```r
flights %>% 
	rename(tail_num = tailnum)
```

```
Source: local data frame [336,776 x 16]

    year month   day dep_time dep_delay arr_time arr_delay carrier
   (int) (int) (int)    (int)     (dbl)    (int)     (dbl)   (chr)
1   2013     1     1      517         2      830        11      UA
2   2013     1     1      533         4      850        20      UA
3   2013     1     1      542         2      923        33      AA
4   2013     1     1      544        -1     1004       -18      B6
5   2013     1     1      554        -6      812       -25      DL
6   2013     1     1      554        -4      740        12      UA
7   2013     1     1      555        -5      913        19      B6
8   2013     1     1      557        -3      709       -14      EV
9   2013     1     1      557        -3      838        -8      B6
10  2013     1     1      558        -2      753         8      AA
..   ...   ...   ...      ...       ...      ...       ...     ...
Variables not shown: tail_num (chr), flight (int), origin (chr), dest
  (chr), air_time (dbl), distance (dbl), hour (dbl), minute (dbl)
```

========================================================
### Extract distinct (unique) rows


```r
flights %>% 
	select(origin, dest) %>% 
	distinct(origin, dest)
```

```
Source: local data frame [224 x 2]

   origin  dest
    (chr) (chr)
1     EWR   IAH
2     LGA   IAH
3     JFK   MIA
4     JFK   BQN
5     LGA   ATL
6     EWR   ORD
7     EWR   FLL
8     LGA   IAD
9     JFK   MCO
10    LGA   ORD
..    ...   ...
```


OR 


```r
flights %>% 
	select(origin, dest) %>% 
	distinct()
```

========================================================
## 4) Add new columns with `mutate()`

- Add new columns that are functions of existing columns.


```r
flights %>% 
	mutate(gain = arr_delay - dep_delay,
				 speed = distance / air_time * 60)
```

```
Source: local data frame [336,776 x 18]

    year month   day dep_time dep_delay arr_time arr_delay carrier tailnum
   (int) (int) (int)    (int)     (dbl)    (int)     (dbl)   (chr)   (chr)
1   2013     1     1      517         2      830        11      UA  N14228
2   2013     1     1      533         4      850        20      UA  N24211
3   2013     1     1      542         2      923        33      AA  N619AA
4   2013     1     1      544        -1     1004       -18      B6  N804JB
5   2013     1     1      554        -6      812       -25      DL  N668DN
6   2013     1     1      554        -4      740        12      UA  N39463
7   2013     1     1      555        -5      913        19      B6  N516JB
8   2013     1     1      557        -3      709       -14      EV  N829AS
9   2013     1     1      557        -3      838        -8      B6  N593JB
10  2013     1     1      558        -2      753         8      AA  N3ALAA
..   ...   ...   ...      ...       ...      ...       ...     ...     ...
Variables not shown: flight (int), origin (chr), dest (chr), air_time
  (dbl), distance (dbl), hour (dbl), minute (dbl), gain (dbl), speed (dbl)
```

========================================================
### If you only want to keep the new variables, use `transmute()`


```r
flights %>% 
	transmute(gain = arr_delay - dep_delay,
						gain_per_hour = gain / (air_time / 60)
)
```

```
Source: local data frame [336,776 x 2]

    gain gain_per_hour
   (dbl)         (dbl)
1      9      2.378855
2     16      4.229075
3     31     11.625000
4    -17     -5.573770
5    -19     -9.827586
6     16      6.400000
7     24      9.113924
8    -11    -12.452830
9     -5     -2.142857
10    10      4.347826
..   ...           ...
```

========================================================
## 5) Summarise values with `summarise()`


```r
flights %>% 
	summarise(delay = mean(dep_delay, na.rm = TRUE))
```

```
Source: local data frame [1 x 1]

     delay
     (dbl)
1 12.63907
```

========================================================
# Grouped operations

In dplyr, you do this by with the `group_by()` function. It breaks down a dataset into specified groups of rows.

========================================================

**Example**: Complete each task:

1) Split the complete dataset into individual planes (tailnum) 

2) Summarise each plane by:
- counting the number of flights (count = n())  
- computing the average distance (dist = mean(Distance, na.rm = TRUE)) and arrival delay (delay = mean(ArrDelay, na.rm = TRUE))


========================================================


```r
flights %>%  
	group_by(tailnum) %>% 
	summarise(count = n(),
						dist = mean(distance, na.rm = TRUE),
						delay = mean(arr_delay, na.rm = TRUE)) 
```

```
Source: local data frame [4,044 x 4]

   tailnum count     dist      delay
     (chr) (int)    (dbl)      (dbl)
1           2512 710.2576        NaN
2   D942DN     4 854.5000 31.5000000
3   N0EGMQ   371 676.1887  9.9829545
4   N10156   153 757.9477 12.7172414
5   N102UW    48 535.8750  2.9375000
6   N103US    46 535.1957 -6.9347826
7   N104UW    47 535.2553  1.8043478
8   N10575   289 519.7024 20.6914498
9   N105UW    45 524.8444 -0.2666667
10  N107US    41 528.7073 -5.7317073
..     ...   ...      ...        ...
```

========================================================
Now with the result you found above, filter the individual planes that had 
arrival delay greater than 120 minutes.

========================================================


```r
flights %>%  
	group_by(tailnum) %>% 
	summarise(count = n(),
						dist = mean(distance, na.rm = TRUE),
						delay = mean(arr_delay, na.rm = TRUE)) %>% 
	filter(delay > 120)
```

```
Source: local data frame [14 x 4]

   tailnum count   dist    delay
     (chr) (int)  (dbl)    (dbl)
1   N136DL     1  762.0 146.0000
2   N427SW     1  488.0 157.0000
3   N587NW     1  762.0 264.0000
4   N633AW     1  529.0 136.0000
5   N654UA     1 2227.0 185.0000
6   N665MQ     6  481.5 174.6667
7   N7715E     1  711.0 188.0000
8   N78003     1 1400.0 137.0000
9   N790SK     1  419.0 140.0000
10  N844MH     1  760.0 320.0000
11  N851NW     1 2422.0 219.0000
12  N911DA     1 1020.0 294.0000
13  N922EV     1  963.0 276.0000
14  N928DN     1 1020.0 201.0000
```

========================================================
Now sort by distance ... 

========================================================


```r
flights %>%  
	group_by(tailnum) %>% 
	summarise(count = n(),
						dist = mean(distance, na.rm = TRUE),
						delay = mean(arr_delay, na.rm = TRUE)) %>% 
	filter(delay > 120) %>% 
	arrange(dist)
```

```
Source: local data frame [14 x 4]

   tailnum count   dist    delay
     (chr) (int)  (dbl)    (dbl)
1   N790SK     1  419.0 140.0000
2   N665MQ     6  481.5 174.6667
3   N427SW     1  488.0 157.0000
4   N633AW     1  529.0 136.0000
5   N7715E     1  711.0 188.0000
6   N844MH     1  760.0 320.0000
7   N136DL     1  762.0 146.0000
8   N587NW     1  762.0 264.0000
9   N922EV     1  963.0 276.0000
10  N911DA     1 1020.0 294.0000
11  N928DN     1 1020.0 201.0000
12  N78003     1 1400.0 137.0000
13  N654UA     1 2227.0 185.0000
14  N851NW     1 2422.0 219.0000
```

========================================================

**Example**: Get all not cancelled flights, ie, the flights that have 
`dep_delay` and `arr_delay`.

**TIP**: You should use the function `is.na()`

========================================================


```r
flights %>% 
	filter(!is.na(dep_delay), !is.na(arr_delay))
```

```
Source: local data frame [327,346 x 16]

    year month   day dep_time dep_delay arr_time arr_delay carrier tailnum
   (int) (int) (int)    (int)     (dbl)    (int)     (dbl)   (chr)   (chr)
1   2013     1     1      517         2      830        11      UA  N14228
2   2013     1     1      533         4      850        20      UA  N24211
3   2013     1     1      542         2      923        33      AA  N619AA
4   2013     1     1      544        -1     1004       -18      B6  N804JB
5   2013     1     1      554        -6      812       -25      DL  N668DN
6   2013     1     1      554        -4      740        12      UA  N39463
7   2013     1     1      555        -5      913        19      B6  N516JB
8   2013     1     1      557        -3      709       -14      EV  N829AS
9   2013     1     1      557        -3      838        -8      B6  N593JB
10  2013     1     1      558        -2      753         8      AA  N3ALAA
..   ...   ...   ...      ...       ...      ...       ...     ...     ...
Variables not shown: flight (int), origin (chr), dest (chr), air_time
  (dbl), distance (dbl), hour (dbl), minute (dbl)
```

========================================================

**Example**: Which destinations have the most carriers (exclude again the 
not cancelled flights)?

========================================================


```r
flights %>% 
	filter(!is.na(dep_delay), !is.na(arr_delay)) %>% 
	group_by(dest) %>% 
  summarise(carriers = n_distinct(carrier)) %>% 
  arrange(desc(carriers))
```

```
Source: local data frame [104 x 2]

    dest carriers
   (chr)    (int)
1    ATL        7
2    BOS        7
3    CLT        7
4    ORD        7
5    TPA        7
6    AUS        6
7    DCA        6
8    DTW        6
9    IAD        6
10   MSP        6
..   ...      ...
```

========================================================

**Example**: Select all columns that starts with "d"


```r
?select
```

========================================================


```r
flights %>% 
	select(starts_with("d"))
```

```
Source: local data frame [336,776 x 5]

     day dep_time dep_delay  dest distance
   (int)    (int)     (dbl) (chr)    (dbl)
1      1      517         2   IAH     1400
2      1      533         4   IAH     1416
3      1      542         2   MIA     1089
4      1      544        -1   BQN     1576
5      1      554        -6   ATL      762
6      1      554        -4   ORD      719
7      1      555        -5   FLL     1065
8      1      557        -3   IAD      229
9      1      557        -3   MCO      944
10     1      558        -2   ORD      733
..   ...      ...       ...   ...      ...
```

========================================================  

**Example**: Find the top 10 destinations departing from JFK:

========================================================


```r
flights %>% 
	filter(origin == "JFK") %>% 
	group_by(dest) %>% 
	count(dest) %>% 
	top_n(10)
```

```
Source: local data frame [10 x 2]

    dest     n
   (chr) (int)
1    BOS  5898
2    BUF  3582
3    DCA  3270
4    FLL  4254
5    LAS  3987
6    LAX 11262
7    MCO  5464
8    MIA  3314
9    SFO  8204
10   SJU  4752
```

========================================================

**Example**: For each month of the year, count the total number of flights and 
sort in descending order.

========================================================


```r
flights %>%
    group_by(month) %>%
    summarise(flight_count = n()) %>%
    arrange(desc(flight_count))
```

```
Source: local data frame [12 x 2]

   month flight_count
   (int)        (int)
1      7        29425
2      8        29327
3     10        28889
4      3        28834
5      5        28796
6      4        28330
7      6        28243
8     12        28135
9      9        27574
10    11        27268
11     1        27004
12     2        24951
```

========================================================

**Example**: For each carrier, calculate which two days of the year they had their longest departure delays


========================================================
One way:


```r
flights %>%
    group_by(carrier) %>%
    select(month, day, dep_delay) %>%
    filter(min_rank(desc(dep_delay)) <= 2) %>%
    arrange(carrier, desc(dep_delay))
```

```
Source: local data frame [32 x 4]
Groups: carrier [16]

   carrier month   day dep_delay
     (chr) (int) (int)     (dbl)
1       9E     2    16       747
2       9E     7    24       430
3       AA     9    20      1014
4       AA    12     5       896
5       AS     5    23       225
6       AS     1    20       222
7       B6     1    16       502
8       B6     7    10       453
9       DL     4    10       960
10      DL     3    17       911
..     ...   ...   ...       ...
```


========================================================
Another way:


```r
flights %>%
    group_by(carrier) %>%
    select(month, day, dep_delay) %>%
    top_n(2) %>%
    arrange(carrier, desc(dep_delay))
```

```
Source: local data frame [32 x 4]
Groups: carrier [16]

   carrier month   day dep_delay
     (chr) (int) (int)     (dbl)
1       9E     2    16       747
2       9E     7    24       430
3       AA     9    20      1014
4       AA    12     5       896
5       AS     5    23       225
6       AS     1    20       222
7       B6     1    16       502
8       B6     7    10       453
9       DL     4    10       960
10      DL     3    17       911
..     ...   ...   ...       ...
```


========================================================

## More Resources:

http://www.rstudio.com/wp-content/uploads/2015/02/data-wrangling-cheatsheet.pdf


