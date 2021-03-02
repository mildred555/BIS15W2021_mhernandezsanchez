---
title: "Lab 13 Homework"
author: "Mildred Hernandez"
date: "2021-03-02"
output:
  html_document: 
    theme: spacelab
    keep_md: yes
---



## Instructions
Answer the following questions and complete the exercises in RMarkdown. Please embed all of your code and push your final work to your repository. Your final lab report should be organized, clean, and run free from errors. Remember, you must remove the `#` for the included code chunks to run. Be sure to add your name to the author header above. For any included plots, make sure they are clearly labeled. You are free to use any plot type that you feel best communicates the results of your analysis.  

Make sure to use the formatting conventions of RMarkdown to make your report neat and clean!  

## Libraries

```r
if (!require("tidyverse")) install.packages('tidyverse') #if tidyverse isn't installed, it gets installed 
```

```
## Loading required package: tidyverse
```

```
## -- Attaching packages --------------------------------------- tidyverse 1.3.0 --
```

```
## v ggplot2 3.3.3     v purrr   0.3.4
## v tibble  3.0.6     v dplyr   1.0.4
## v tidyr   1.1.2     v stringr 1.4.0
## v readr   1.4.0     v forcats 0.5.1
```

```
## -- Conflicts ------------------------------------------ tidyverse_conflicts() --
## x dplyr::filter() masks stats::filter()
## x dplyr::lag()    masks stats::lag()
```


```r
library(tidyverse)
library(shiny)
library(shinydashboard)
```

## Data
The data for this assignment come from the [University of California Information Center](https://www.universityofcalifornia.edu/infocenter). Admissions data were collected for the years 2010-2019 for each UC campus. Admissions are broken down into three categories: applications, admits, and enrollees. The number of individuals in each category are presented by demographic.  

```r
UC_admit <- readr::read_csv("data/UC_admit.csv")
```

```
## 
## -- Column specification --------------------------------------------------------
## cols(
##   Campus = col_character(),
##   Academic_Yr = col_double(),
##   Category = col_character(),
##   Ethnicity = col_character(),
##   `Perc FR` = col_character(),
##   FilteredCountFR = col_double()
## )
```

**1. Use the function(s) of your choice to get an idea of the overall structure of the data frame, including its dimensions, column names, variable classes, etc. As part of this, determine if there are NA's and how they are treated.**  

```r
glimpse(UC_admit)
```

```
## Rows: 2,160
## Columns: 6
## $ Campus          <chr> "Davis", "Davis", "Davis", "Davis", "Davis", "Davis", ~
## $ Academic_Yr     <dbl> 2019, 2019, 2019, 2019, 2019, 2019, 2019, 2019, 2018, ~
## $ Category        <chr> "Applicants", "Applicants", "Applicants", "Applicants"~
## $ Ethnicity       <chr> "International", "Unknown", "White", "Asian", "Chicano~
## $ `Perc FR`       <chr> "21.16%", "2.51%", "18.39%", "30.76%", "22.44%", "0.35~
## $ FilteredCountFR <dbl> 16522, 1959, 14360, 24024, 17526, 277, 3425, 78093, 15~
```

```r
summary(UC_admit)
```

```
##     Campus           Academic_Yr     Category          Ethnicity        
##  Length:2160        Min.   :2010   Length:2160        Length:2160       
##  Class :character   1st Qu.:2012   Class :character   Class :character  
##  Mode  :character   Median :2014   Mode  :character   Mode  :character  
##                     Mean   :2014                                        
##                     3rd Qu.:2017                                        
##                     Max.   :2019                                        
##                                                                         
##    Perc FR          FilteredCountFR   
##  Length:2160        Min.   :     1.0  
##  Class :character   1st Qu.:   447.5  
##  Mode  :character   Median :  1837.0  
##                     Mean   :  7142.6  
##                     3rd Qu.:  6899.5  
##                     Max.   :113755.0  
##                     NA's   :1
```

```r
naniar::miss_var_summary(UC_admit)
```

```
## # A tibble: 6 x 3
##   variable        n_miss pct_miss
##   <chr>            <int>    <dbl>
## 1 Perc FR              1   0.0463
## 2 FilteredCountFR      1   0.0463
## 3 Campus               0   0     
## 4 Academic_Yr          0   0     
## 5 Category             0   0     
## 6 Ethnicity            0   0
```

```r
UC_admit %>% 
  select('Perc FR') %>%
  anyNA()#line 148
```

```
## [1] TRUE
```

```r
UC_admit <- janitor::clean_names(UC_admit) 
```

**2. The president of UC has asked you to build a shiny app that shows admissions by ethnicity across all UC campuses. Your app should allow users to explore year, campus, and admit category as interactive variables. Use shiny dashboard and try to incorporate the aesthetics you have learned in ggplot to make the app neat and clean.**

```r
UC_admit$academic_yr <- as.factor(UC_admit$academic_yr)
```


```r
UC_admit %>%
  group_by(campus) %>%
  summarize(n())
```

```
## # A tibble: 9 x 2
##   campus        `n()`
## * <chr>         <int>
## 1 Berkeley        240
## 2 Davis           240
## 3 Irvine          240
## 4 Los_Angeles     240
## 5 Merced          240
## 6 Riverside       240
## 7 San_Diego       240
## 8 Santa_Barbara   240
## 9 Santa_Cruz      240
```


```r
ui <- dashboardPage(
  dashboardHeader(title = "UC Admissions"),
  dashboardSidebar(disable = T),
  dashboardBody(
  fluidRow(
  box(
  selectInput("x", "Select Campus", choices = c("Davis", "Berkeley", "Irivine", "Los_Angeles", "Merced", "Riverside", "San_Diego", "Santa_Barbara", "Santa_Cruz"), selected = "Davis"),
  selectInput("y", "Select Academic Year", choices = c("2010", "2011", "2012", "2013", "2014", "2015", "2016", "2017", "2018", "2019"), selected = "2019"),
  selectInput("z", "Select Category", choices = c("Admits", "Applicants", "Enrollees"), selected = "Admits"),
  
  sliderInput("pointsize", "Select the Point Size", min = 1, max = 5, value = 2, step = 0.5)
  ),
  
  box(
  plotOutput("UC_Admissions_Plot", width = "800px", height = "800px")
  ) # close the second box
  ) # close the row
  ) # close the dashboard body
) # close the ui

server <- function(input, output, session) { 
  output$UC_Admissions_Plot <- renderPlot({
    UC_admit %>%
      filter(campus == input$x) %>%
      filter(academic_yr == input$y) %>%
      filter(category == input$z) %>%
      filter(filtered_count_fr!="NA") %>%
  ggplot(aes_string(x = "ethnicity", y= "filtered_count_fr"))+
    geom_col()+
    theme_light(base_size = 18)
  })
  session$onSessionEnded(stopApp)
  }

shinyApp(ui, server)
```

`<div style="width: 100% ; height: 400px ; text-align: center; box-sizing: border-box; -moz-box-sizing: border-box; -webkit-box-sizing: border-box;" class="muted well">Shiny applications not supported in static R Markdown documents</div>`{=html}


```r
names(UC_admit)
```

```
## [1] "campus"            "academic_yr"       "category"         
## [4] "ethnicity"         "perc_fr"           "filtered_count_fr"
```

**3. Make alternate version of your app above by tracking enrollment at a campus over all of the represented years while allowing users to interact with campus, category, and ethnicity.**

```r
UC_admit %>%
  group_by(ethnicity) %>%
  summarize(n())
```

```
## # A tibble: 8 x 2
##   ethnicity        `n()`
## * <chr>            <int>
## 1 African American   270
## 2 All                270
## 3 American Indian    270
## 4 Asian              270
## 5 Chicano/Latino     270
## 6 International      270
## 7 Unknown            270
## 8 White              270
```


```r
ui <- dashboardPage(
  dashboardHeader(title = "UC Enrollment"),
  dashboardSidebar(disable = T),
  dashboardBody(
  fluidRow(
  box(
  selectInput("x", "Select Campus", choices = c("Davis", "Berkeley", "Irivine", "Los_Angeles", "Merced", "Riverside", "San_Diego", "Santa_Barbara", "Santa_Cruz"), selected = "Davis"),
  selectInput("y", "Select Ethnicity", choices = c("African American", "American Indian", "Asian", "Chicano/Latino", "International", "Unknown", "White", "All"), selected = "African American"),
  selectInput("z", "Select Academic Year", choices = c("2010", "2011", "2012", "2013", "2014", "2015", "2016", "2017", "2018", "2019"), selected = "2019"),
  
  
  sliderInput("pointsize", "Select the Point Size", min = 1, max = 5, value = 2, step = 0.5)
  ),
  
  box(
  plotOutput("UC_Enrollment_Plot", width = "800px", height = "800px")
  ) # close the second box
  ) # close the row
  ) # close the dashboard body
) # close the ui

server <- function(input, output, session) { 
  output$UC_Admissions_Plot <- renderPlot({
    UC_admit %>%
      filter(category == "Enrollees") %>%
      filter(campus == input$x) %>%
      filter(ethnicity == input$y) %>%
      filter(academic_yr == input$z) %>%
  ggplot(aes_string(x = "category", y= "filtered_count_fr"))+
    geom_col()+
    theme_light(base_size = 18)
  })
  session$onSessionEnded(stopApp)
  }

shinyApp(ui, server)
```

`<div style="width: 100% ; height: 400px ; text-align: center; box-sizing: border-box; -moz-box-sizing: border-box; -webkit-box-sizing: border-box;" class="muted well">Shiny applications not supported in static R Markdown documents</div>`{=html}

## Push your final code to GitHub!
Please be sure that you check the `keep md` file in the knit preferences. 
