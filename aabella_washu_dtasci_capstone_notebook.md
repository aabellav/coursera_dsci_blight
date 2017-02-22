Coursera WashU Dtasci Capstone Project: BLIGHT
================

Loading libraries

Setting data location, make sure the files below are available so the rest runs

``` r
datadir <- "/Users/toni/Documents/ml_learning/coursera/datasci_RProject/data"
list.files(datadir)
```

    ## [1] "detroit-311.csv"                "detroit-blight-violations.csv" 
    ## [3] "detroit-crime.csv"              "detroit-demolition-permits.tsv"

Loading blight violation incidents

``` r
data_violations <- read_csv(paste(datadir, "detroit-blight-violations.csv", sep="/"))
```

    ## Parsed with column specification:
    ## cols(
    ##   .default = col_character(),
    ##   TicketID = col_integer(),
    ##   ViolationStreetNumber = col_integer(),
    ##   MailingStreetNumber = col_integer(),
    ##   TicketIssuedTime = col_time(format = ""),
    ##   CourtTime = col_time(format = ""),
    ##   Void = col_integer(),
    ##   ViolationCategory = col_integer()
    ## )

    ## See spec(...) for full column specifications.

    ## Warning: 806 parsing failures.
    ##    row                 col               expected   actual
    ## 276113 MailingStreetNumber an integer             P.O. Box
    ## 276114 MailingStreetNumber an integer             P.O. Box
    ## 276115 MailingStreetNumber an integer             P.O. Box
    ## 276139 MailingStreetNumber no trailing characters -2      
    ## 276140 MailingStreetNumber no trailing characters -2      
    ## ...... ................... ...................... ........
    ## See problems(...) for more details.

``` r
head(data_violations)
```

    ## # A tibble: 6 × 31
    ##   TicketID TicketNumber                 AgencyName
    ##      <int>        <chr>                      <chr>
    ## 1    26288  05000001DAH Department of Public Works
    ## 2    19800  05000025DAH Department of Public Works
    ## 3    19804  05000026DAH Department of Public Works
    ## 4    20208  05000027DAH Department of Public Works
    ## 5    20211  05000028DAH Department of Public Works
    ## 6    20628  05000029DAH Department of Public Works
    ## # ... with 28 more variables: ViolName <chr>, ViolationStreetNumber <int>,
    ## #   ViolationStreetName <chr>, MailingStreetNumber <int>,
    ## #   MailingStreetName <chr>, MailingCity <chr>, MailingState <chr>,
    ## #   MailingZipCode <chr>, NonUsAddressCode <chr>, Country <chr>,
    ## #   TicketIssuedDT <chr>, TicketIssuedTime <time>, HearingDT <chr>,
    ## #   CourtTime <time>, ViolationCode <chr>, ViolDescription <chr>,
    ## #   Disposition <chr>, FineAmt <chr>, AdminFee <chr>, LateFee <chr>,
    ## #   StateFee <chr>, CleanUpCost <chr>, JudgmentAmt <chr>,
    ## #   PaymentStatus <chr>, Void <int>, ViolationCategory <int>,
    ## #   ViolationAddress <chr>, MailingAddress <chr>

Loading calls to 311, typically complains

``` r
data_311 <- read_csv(paste(datadir, "detroit-311.csv", sep="/"))
```

    ## Parsed with column specification:
    ## cols(
    ##   ticket_id = col_integer(),
    ##   city = col_character(),
    ##   issue_type = col_character(),
    ##   ticket_status = col_character(),
    ##   issue_description = col_character(),
    ##   rating = col_integer(),
    ##   ticket_closed_date_time = col_character(),
    ##   acknowledged_at = col_character(),
    ##   ticket_created_date_time = col_character(),
    ##   ticket_last_updated_date_time = col_character(),
    ##   address = col_character(),
    ##   lat = col_double(),
    ##   lng = col_double(),
    ##   location = col_character(),
    ##   image = col_character()
    ## )

``` r
head(data_311)
```

    ## # A tibble: 6 × 15
    ##   ticket_id            city    issue_type ticket_status
    ##       <int>           <chr>         <chr>         <chr>
    ## 1   1516722 City of Detroit Clogged Drain  Acknowledged
    ## 2   1525361 City of Detroit Clogged Drain  Acknowledged
    ## 3   1525218 City of Detroit Clogged Drain        Closed
    ## 4   1525214 City of Detroit Clogged Drain  Acknowledged
    ## 5   1525142 City of Detroit Clogged Drain  Acknowledged
    ## 6   1525087 City of Detroit Clogged Drain        Closed
    ## # ... with 11 more variables: issue_description <chr>, rating <int>,
    ## #   ticket_closed_date_time <chr>, acknowledged_at <chr>,
    ## #   ticket_created_date_time <chr>, ticket_last_updated_date_time <chr>,
    ## #   address <chr>, lat <dbl>, lng <dbl>, location <chr>, image <chr>

Loading criminal incidents in Detroit

``` r
data_crime <- read_csv(paste(datadir, "detroit-crime.csv", sep="/"))
```

    ## Parsed with column specification:
    ## cols(
    ##   ROWNUM = col_integer(),
    ##   CASEID = col_integer(),
    ##   INCINO = col_double(),
    ##   CATEGORY = col_character(),
    ##   OFFENSEDESCRIPTION = col_character(),
    ##   STATEOFFENSEFILECLASS = col_integer(),
    ##   INCIDENTDATE = col_character(),
    ##   HOUR = col_integer(),
    ##   SCA = col_integer(),
    ##   PRECINCT = col_integer(),
    ##   COUNCIL = col_character(),
    ##   NEIGHBORHOOD = col_character(),
    ##   CENSUSTRACT = col_integer(),
    ##   ADDRESS = col_character(),
    ##   LON = col_double(),
    ##   LAT = col_double(),
    ##   LOCATION = col_character()
    ## )

    ## Warning: 21 parsing failures.
    ##   row         col               expected actual
    ##  1546 CENSUSTRACT no trailing characters    .01
    ## 12823 INCINO      no trailing characters     .1
    ## 16025 CENSUSTRACT no trailing characters    .01
    ## 17068 CENSUSTRACT no trailing characters    .01
    ## 17755 CENSUSTRACT no trailing characters    .02
    ## ..... ........... ...................... ......
    ## See problems(...) for more details.

``` r
head(data_crime)
```

    ## # A tibble: 6 × 17
    ##   ROWNUM  CASEID     INCINO                                 CATEGORY
    ##    <int>   <int>      <dbl>                                    <chr>
    ## 1  53256 1953933 1506030028                                  ASSAULT
    ## 2  17631 1917717 1503010158                                  LARCENY
    ## 3  11207 1910955 1502080223                           STOLEN VEHICLE
    ## 4 116589 2018186 1511090188                         WEAPONS OFFENSES
    ## 5  85790 1986862 1508239803                                  LARCENY
    ## 6  27456 1927752 1503280235 TRAFFIC VIOLATIONS-MOTORCYCLE VIOLATIONS
    ## # ... with 13 more variables: OFFENSEDESCRIPTION <chr>,
    ## #   STATEOFFENSEFILECLASS <int>, INCIDENTDATE <chr>, HOUR <int>,
    ## #   SCA <int>, PRECINCT <int>, COUNCIL <chr>, NEIGHBORHOOD <chr>,
    ## #   CENSUSTRACT <int>, ADDRESS <chr>, LON <dbl>, LAT <dbl>, LOCATION <chr>

``` r
data_permits <- read_tsv(paste(datadir, "detroit-demolition-permits.tsv", sep="/"))
```

    ## Parsed with column specification:
    ## cols(
    ##   .default = col_character(),
    ##   PARCEL_SIZE = col_double(),
    ##   PARCEL_CLUSTER_SECTOR = col_integer(),
    ##   STORIES = col_double(),
    ##   PARCEL_FLOOR_AREA = col_integer(),
    ##   PARCEL_GROUND_AREA = col_integer(),
    ##   SEQ_NO = col_integer(),
    ##   CONTRACTOR_ZIP = col_integer()
    ## )

    ## See spec(...) for full column specifications.

``` r
head(data_permits)
```

    ## # A tibble: 6 × 55
    ##       PERMIT_NO PERMIT_APPLIED PERMIT_ISSUED PERMIT_EXPIRES
    ##           <chr>          <chr>         <chr>          <chr>
    ## 1 BLD2015-03955        8/28/15       8/28/15           <NA>
    ## 2 BLD2015-04083        8/28/15       8/28/15           <NA>
    ## 3 BLD2015-03976        8/28/15       8/28/15           <NA>
    ## 4 BLD2015-03781        8/28/15       8/28/15           <NA>
    ## 5 BLD2015-03677        8/28/15       8/28/15           <NA>
    ## 6 BLD2015-03914        8/28/15       8/28/15           <NA>
    ## # ... with 51 more variables: SITE_ADDRESS <chr>, BETWEEN1 <chr>,
    ## #   PARCEL_NO <chr>, LOT_NUMBER <chr>, SUBDIVISION <chr>, CASE_TYPE <chr>,
    ## #   CASE_DESCRIPTION <chr>, LEGAL_USE <chr>, ESTIMATED_COST <chr>,
    ## #   PARCEL_SIZE <dbl>, PARCEL_CLUSTER_SECTOR <int>, STORIES <dbl>,
    ## #   PARCEL_FLOOR_AREA <int>, PARCEL_GROUND_AREA <int>,
    ## #   PRC_AKA_ADDRESS <chr>, BLD_PERMIT_TYPE <chr>,
    ## #   PERMIT_DESCRIPTION <chr>, BLD_PERMIT_DESC <chr>, BLD_TYPE_USE <chr>,
    ## #   RESIDENTIAL <chr>, DESCRIPTION <chr>, BLD_TYPE_CONST_COD <chr>,
    ## #   BLD_ZONING_DIST <chr>, BLD_USE_GROUP <chr>, BLD_BASEMENT <chr>,
    ## #   FEE_TYPE <chr>, CSM_CASENO <chr>, CSF_CREATED_BY <chr>, SEQ_NO <int>,
    ## #   PCF_AMT_PD <chr>, PCF_AMT_DUE <chr>, PCF_UPDATED <chr>,
    ## #   OWNER_LAST_NAME <chr>, OWNER_FIRST_NAME <chr>, OWNER_ADDRESS1 <chr>,
    ## #   OWNER_ADDRESS2 <chr>, OWNER_CITY <chr>, OWNER_STATE <chr>,
    ## #   OWNER_ZIP <chr>, CONTRACTOR_LAST_NAME <chr>,
    ## #   CONTRACTOR_FIRST_NAME <chr>, CONTRACTOR_ADDRESS1 <chr>,
    ## #   CONTRACTOR_ADDRESS2 <chr>, CONTRACTOR_CITY <chr>,
    ## #   CONTRACTOR_STATE <chr>, CONTRACTOR_ZIP <int>,
    ## #   CONDITION_FOR_APPROVAL <chr>, site_location <chr>,
    ## #   owner_location <chr>, contractor_location <chr>, geom <chr>

Data Violation exploration

``` r
#converting to factors
cols <- c("AgencyName","ViolationCode","Disposition","PaymentStatus","Void",
          "ViolationCategory","Country")
data_violations %<>% mutate_each_(funs(factor(.)),cols)
#converting $$ to numeric
cols <- c("FineAmt","AdminFee","LateFee","StateFee","CleanUpCost","JudgmentAmt")
data_violations %<>% mutate_each_(funs(from_currency(.)),cols)
#converting to dates
cols <-c("TicketIssuedDT","HearingDT")
# to use lubridate, need to figure it out -- 
#data_violations %<>% mutate_each_(funs(from_currency(.)),cols)
str(data_violations)
```

    ## Classes 'tbl_df', 'tbl' and 'data.frame':    307804 obs. of  31 variables:
    ##  $ TicketID             : int  26288 19800 19804 20208 20211 20628 20631 20634 20899 20901 ...
    ##  $ TicketNumber         : chr  "05000001DAH" "05000025DAH" "05000026DAH" "05000027DAH" ...
    ##  $ AgencyName           : Factor w/ 5 levels "Building and Safety Engineering Department",..: 2 2 2 2 2 2 2 2 2 2 ...
    ##  $ ViolName             : chr  "Group, LLC, Grand Holding" "JACKSON, RAECHELLE" "TALTON, CAROL ANN" "BONNER, DARRYL E." ...
    ##  $ ViolationStreetNumber: int  2566 19014 18735 20125 17397 17153 17153 17517 18610 18498 ...
    ##  $ ViolationStreetName  : chr  "GRAND BLVD" "ASHTON" "STAHELIN" "MONICA" ...
    ##  $ MailingStreetNumber  : int  743 20501 18735 25335 17397 11200 11200 17209 30521 25359 ...
    ##  $ MailingStreetName    : chr  "Beaubien, Ste. 201" "HEYDEN" "STAHELI N" "PEEKSKILL" ...
    ##  $ MailingCity          : chr  "Detroit" "DETROIT" "DETROIT" "SOUTHFIELD" ...
    ##  $ MailingState         : chr  "MI" "MI" "MI" "MI" ...
    ##  $ MailingZipCode       : chr  "48226" "48219" "48219" "48043" ...
    ##  $ NonUsAddressCode     : chr  "N/A" "N/A" "N/A" "N/A" ...
    ##  $ Country              : Factor w/ 30 levels "Australia","Canada",..: NA NA NA NA NA NA NA NA NA NA ...
    ##  $ TicketIssuedDT       : chr  "01/01/38440 12:00:00 AM" "01/01/38383 12:00:00 AM" "01/01/38383 12:00:00 AM" "01/01/38385 12:00:00 AM" ...
    ##  $ TicketIssuedTime     :Classes 'hms', 'difftime'  atomic [1:307804] 43200 36900 38100 38700 40200 45900 45900 47100 35100 36300 ...
    ##   .. ..- attr(*, "units")= chr "secs"
    ##  $ HearingDT            : chr  "01/01/38474 12:00:00 AM" "01/01/38425 12:00:00 AM" "01/01/38425 12:00:00 AM" "01/01/38422 12:00:00 AM" ...
    ##  $ CourtTime            :Classes 'hms', 'difftime'  atomic [1:307804] 32400 48600 48600 48600 48600 48600 48600 48600 48600 48600 ...
    ##   .. ..- attr(*, "units")= chr "secs"
    ##  $ ViolationCode        : Factor w/ 265 levels "22-2-16","22-2-17",..: 6 9 9 21 9 21 9 9 9 9 ...
    ##  $ ViolDescription      : chr  "Burning solid waste  in open fires" "Bulk solid waste deposited more than 24 hours before designated time" "Bulk solid waste deposited more than 24 hours before designated time" "Violation of time limit for approved containers to remain at curbside - early or late" ...
    ##  $ Disposition          : Factor w/ 10 levels "Not responsible By City Dismissal",..: 9 2 9 8 8 3 3 8 7 8 ...
    ##  $ FineAmt              : num  1500 100 100 100 100 100 100 100 100 100 ...
    ##  $ AdminFee             : num  20 20 20 20 20 20 20 20 20 20 ...
    ##  $ LateFee              : num  150 10 10 10 10 10 10 10 10 10 ...
    ##  $ StateFee             : num  10 10 10 10 10 10 10 10 10 10 ...
    ##  $ CleanUpCost          : num  0 0 0 0 0 0 0 0 0 0 ...
    ##  $ JudgmentAmt          : num  1680 140 140 140 140 140 140 140 140 140 ...
    ##  $ PaymentStatus        : Factor w/ 4 levels "NO PAYMENT APPLIED",..: 3 1 3 1 3 1 1 1 4 3 ...
    ##  $ Void                 : Factor w/ 1 level "0": 1 1 1 1 1 1 1 1 1 1 ...
    ##  $ ViolationCategory    : Factor w/ 2 levels "0","1": 1 1 1 1 1 1 1 1 1 1 ...
    ##  $ ViolationAddress     : chr  "2566 GRAND BLVD\nDetroit, MI\n(42.36318237000006, -83.09167672099994)" "19014 ASHTON\nDetroit, MI\n(42.429390762000025, -83.22039357799997)" "18735 STAHELIN\nDetroit, MI\n(42.428707459000066, -83.22754809599996)" "20125 MONICA\nDetroit, MI\n(42.44169828400004, -83.14501821599998)" ...
    ##  $ MailingAddress       : chr  "743 Beaubien\nDetroit, MI 48226\n(42.33373063000005, -83.04181755199994)" "20501 HEYDEN\nDETROIT, MI 48219\n(42.44217763300003, -83.24182717199994)" "18735 STAHELI N\nDETROIT, MI 48219\n(42.428707459000066, -83.22754809599996)" "25335 PEEKSKILL\nSOUTHFIELD, MI 48043\n(42.475049571000056, -83.30671483399999)" ...

``` r
#summary(data_violations)
```

Violations explorations: - How many were voided? Should we remove them? - Map of violations.. potential facets by types - Types of payment status - Country? Non-US?

311 calls exploration:
----------------------
