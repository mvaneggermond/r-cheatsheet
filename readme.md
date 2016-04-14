R cheatsheet
========================
## Overview
  
* [Installation](#installation)
* [Database](#database)
* [dplyr](#dplyr)
* [Numbers](#numbers)
* [Strings](#strings)

## Installation

### Locale issues on Mac OS X

Setting locales in terminal resolved the issue for me. Open the terminal and

- Check if locale settings are missing
```
> locale
LANG=
LC_COLLATE="C"
LC_CTYPE="UTF-8"
LC_MESSAGES="C"
LC_MONETARY="C"
LC_NUMERIC="C"
LC_TIME="C"
LC_ALL=
```

- Edit ~/.profile or ~/.bashrc
```
export LANG=en_US.UTF-8
export LC_ALL=en_US.UTF-8
```

Open a new terminal window and check that the locales are properly set

```
> locale
LANG="en_US.UTF-8"
LC_COLLATE="en_US.UTF-8"
LC_CTYPE="en_US.UTF-8"
LC_MESSAGES="en_US.UTF-8"
LC_MONETARY="en_US.UTF-8"
LC_NUMERIC="en_US.UTF-8"
LC_TIME="en_US.UTF-8"
LC_ALL="en_US.UTF-8"
```

In Terminal:
```
defaults write org.R-project.R force.LANG en_US.UTF-8
```

Restart RStudio (do not just restart R)

From http://stackoverflow.com/questions/9689104/installing-r-on-mac-warning-messages-setting-lc-ctype-failed-using-c

## Database

Kill connections

```R
library(RPostgreSQL)

kill_db_connections <- function () {

  all_cons <- dbListConnections(PostgreSQL())

  print(all_cons)

  for(con in all_cons)
    +  dbDisconnect(con)

  print(paste(length(all_cons), " connections killed."))

}
```

## dplyr

To obtain a table with relative frequencies. You might want to switch the grouping around if not as wished for.

```r


mtcars %>%
  group_by(am, gear) %>%
  summarise (n = n()) %>%
  mutate(freq = n / sum(n))
```

## Numbers

Round a dataframe to selected number of digits

```r
round_df <- function(df,digits)
{
  nums <- vapply(df,is.numeric, FUN.VALUE = logical(1))

  df[,nums] <- round(df[,nums],digits=digits)

  (df)
}
```

## Strings

Remove NA's when pasting (from <http://stackoverflow.com/questions/13673894/suppress-nas-in-paste>)

```R
paste_na <- function(..., sep = " ", collapse = NULL, na.rm = F) {
  if (na.rm == F)
    paste(..., sep = sep, collapse = collapse)
  else
    if (na.rm == T) {
      paste.na <- function(x, sep) {
        x <- gsub("^\\s+|\\s+$", "", x)
        ret <- paste(na.omit(x), collapse = sep)
        is.na(ret) <- ret == ""
        return(ret)
      }
      df <- data.frame(..., stringsAsFactors = F)
      ret <- apply(df, 1, FUN = function(x) paste.na(x, sep))
      
      if (is.null(collapse))
        ret
      else {
        paste.na(ret, sep = collapse)
      }
    }
}

```
