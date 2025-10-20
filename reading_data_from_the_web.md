Data Wrangling ii
================

## Scrape a table

I want the first table from this
page:(<https://samhda.s3-us-gov-west-1.amazonaws.com/s3fs-public/field-uploads/2k15StateFiles/NSDUHsaeShortTermCHG2015.htm>)

read in the html

``` r
url= "https://samhda.s3-us-gov-west-1.amazonaws.com/s3fs-public/field-uploads/2k15StateFiles/NSDUHsaeShortTermCHG2015.htm"

drug_use_html = read_html(url)
drug_use_html
```

    ## {html_document}
    ## <html lang="en">
    ## [1] <head>\n<link rel="P3Pv1" href="http://www.samhsa.gov/w3c/p3p.xml">\n<tit ...
    ## [2] <body>\r\n\r\n<noscript>\r\n<p>Your browser's Javascript is off. Hyperlin ...

extrace the table(s)

``` r
table_marj=
drug_use_html |> 
  html_table() |> 
  first() |> 
  slice(-1)

table_marj
```

    ## # A tibble: 56 × 16
    ##    State     `12+(2013-2014)` `12+(2014-2015)` `12+(P Value)` `12-17(2013-2014)`
    ##    <chr>     <chr>            <chr>            <chr>          <chr>             
    ##  1 Total U.… 12.90a           13.36            0.002          13.28b            
    ##  2 Northeast 13.88a           14.66            0.005          13.98             
    ##  3 Midwest   12.40b           12.76            0.082          12.45             
    ##  4 South     11.24a           11.64            0.029          12.02             
    ##  5 West      15.27            15.62            0.262          15.53a            
    ##  6 Alabama   9.98             9.60             0.426          9.90              
    ##  7 Alaska    19.60a           21.92            0.010          17.30             
    ##  8 Arizona   13.69            13.12            0.364          15.12             
    ##  9 Arkansas  11.37            11.59            0.678          12.79             
    ## 10 Californ… 14.49            15.25            0.103          15.03             
    ## # ℹ 46 more rows
    ## # ℹ 11 more variables: `12-17(2014-2015)` <chr>, `12-17(P Value)` <chr>,
    ## #   `18-25(2013-2014)` <chr>, `18-25(2014-2015)` <chr>, `18-25(P Value)` <chr>,
    ## #   `26+(2013-2014)` <chr>, `26+(2014-2015)` <chr>, `26+(P Value)` <chr>,
    ## #   `18+(2013-2014)` <chr>, `18+(2014-2015)` <chr>, `18+(P Value)` <chr>

## Learning Assessment

``` r
nyc_cost = 
  read_html("https://www.bestplaces.net/cost_of_living/city/new_york/new_york") |> 
  html_table(header=TRUE) |> 
  first()
```

## CSS Selectos for Star Wars Movies

currently website down but same idea

``` r
swm_html=
  read_html("https://dorksideoftheforce.com/all-star-wars-movies-ranked-by-rotten-tomatoes-score")

title_vec=
  swm_html |> 
  html_elements("em strong") |> 
  html_text()

rating_vec=
  swm_html |> 
  html_elements("strong:nth-child(3)") |> 
  html_text()
```

## Learning Assessment

``` r
books_html=
  read_html("https://books.toscrape.com")

title_vec=
  books_html |> 
  html_elements("h3") |> 
  html_text()

stars_vec=
  books_html |> 
  html_elements(".star-rating") |> 
  html_attr("class")

prices_vec=
  books_html |> 
  html_elements(".price_color") |> 
  html_text()

books_df=
  tibble(
    title = title_vec,
    stars = stars_vec,
    price = prices_vec
  )
```

## Using API

Get some water data

``` r
nyc_water=
  GET("https://data.cityofnewyork.us/resource/ia2d-e54m.csv") |> 
  content("parsed")
```

    ## Rows: 65 Columns: 4
    ## ── Column specification ────────────────────────────────────────────────────────
    ## Delimiter: ","
    ## dbl (4): year, new_york_city_population, nyc_consumption_million_gallons_per...
    ## 
    ## ℹ Use `spec()` to retrieve the full column specification for this data.
    ## ℹ Specify the column types or set `show_col_types = FALSE` to quiet this message.

``` r
nyc_water=
  GET("https://data.cityofnewyork.us/resource/ia2d-e54m.json") |> 
  content("text") |> 
  jsonlite::fromJSON() |> 
  as_tibble()
```
