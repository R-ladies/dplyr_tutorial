# dplyr - Tutorial

1) Make sure you have the latest release of R (3.2.3)

- You can get the latest release (2015-12-10, Wooden Christmas-Tree, R-3.2.3) here: <https://cran.cnr.berkeley.edu/>

2) Make sure you have RStudio installed. 

- If don't have it, you can get it here: <https://www.rstudio.com/products/rstudio/download/>
- If you have it, make sure you have the [latest version](<https://www.rstudio.com/products/rstudio/download/>).
- If you like to have the latest cool features of Studio, you can get the [Preview Version](https://www.rstudio.com/products/rstudio/download/preview/)
  
3) After steps (1) and (2), go to RStudio:

- Update your packages:

```r
    update.packages(ask = FALSE, checkBuilt = TRUE)
```

- Install packages we'll be using:

```r
install.packages("dplyr", dependencies = TRUE)
install.packages("nycflights13", dependencies = TRUE)
```