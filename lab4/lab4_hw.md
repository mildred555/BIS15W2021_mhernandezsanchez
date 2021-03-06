---
title: "Lab 4 Homework"
author: "Mildred Hernandez"
date: "2021-01-20"
output:
  html_document: 
    theme: spacelab
    keep_md: yes
---



## Instructions
Answer the following questions and complete the exercises in RMarkdown. Please embed all of your code and push your final work to your repository. Your final lab report should be organized, clean, and run free from errors. Remember, you must remove the `#` for the included code chunks to run. Be sure to add your name to the author header above.  

Make sure to use the formatting conventions of RMarkdown to make your report neat and clean!  

## Load the tidyverse

```r
library(tidyverse)
```

## Data
For the homework, we will use data about vertebrate home range sizes. The data are in the class folder, but the reference is below.  

**Database of vertebrate home range sizes.**  
Reference: Tamburello N, Cote IM, Dulvy NK (2015) Energy and the scaling of animal space use. The American Naturalist 186(2):196-211. http://dx.doi.org/10.1086/682070.  
Data: http://datadryad.org/resource/doi:10.5061/dryad.q5j65/1  

**1. Load the data into a new object called `homerange`.**

```r
homerange <- readr::read_csv("data/Tamburelloetal_HomeRangeDatabase.csv")
```

```
## 
## -- Column specification --------------------------------------------------------
## cols(
##   .default = col_character(),
##   mean.mass.g = col_double(),
##   log10.mass = col_double(),
##   mean.hra.m2 = col_double(),
##   log10.hra = col_double(),
##   preymass = col_double(),
##   log10.preymass = col_double(),
##   PPMR = col_double()
## )
## i Use `spec()` for the full column specifications.
```

```r
homerange
```

```
## # A tibble: 569 x 24
##    taxon common.name class order family genus species primarymethod N    
##    <chr> <chr>       <chr> <chr> <chr>  <chr> <chr>   <chr>         <chr>
##  1 lake~ american e~ acti~ angu~ angui~ angu~ rostra~ telemetry     16   
##  2 rive~ blacktail ~ acti~ cypr~ catos~ moxo~ poecil~ mark-recaptu~ <NA> 
##  3 rive~ central st~ acti~ cypr~ cypri~ camp~ anomal~ mark-recaptu~ 20   
##  4 rive~ rosyside d~ acti~ cypr~ cypri~ clin~ fundul~ mark-recaptu~ 26   
##  5 rive~ longnose d~ acti~ cypr~ cypri~ rhin~ catara~ mark-recaptu~ 17   
##  6 rive~ muskellunge acti~ esoc~ esoci~ esox  masqui~ telemetry     5    
##  7 mari~ pollack     acti~ gadi~ gadid~ poll~ pollac~ telemetry     2    
##  8 mari~ saithe      acti~ gadi~ gadid~ poll~ virens  telemetry     2    
##  9 mari~ lined surg~ acti~ perc~ acant~ acan~ lineat~ direct obser~ <NA> 
## 10 mari~ orangespin~ acti~ perc~ acant~ naso  litura~ telemetry     8    
## # ... with 559 more rows, and 15 more variables: mean.mass.g <dbl>,
## #   log10.mass <dbl>, alternative.mass.reference <chr>, mean.hra.m2 <dbl>,
## #   log10.hra <dbl>, hra.reference <chr>, realm <chr>, thermoregulation <chr>,
## #   locomotion <chr>, trophic.guild <chr>, dimension <chr>, preymass <dbl>,
## #   log10.preymass <dbl>, PPMR <dbl>, prey.size.reference <chr>
```
 
**2. Explore the data. Show the dimensions, column names, classes for each variable, and a statistical summary. Keep these as separate code chunks.**  

```r
glimpse(homerange)
```

```
## Rows: 569
## Columns: 24
## $ taxon                      <chr> "lake fishes", "river fishes", "river fi...
## $ common.name                <chr> "american eel", "blacktail redhorse", "c...
## $ class                      <chr> "actinopterygii", "actinopterygii", "act...
## $ order                      <chr> "anguilliformes", "cypriniformes", "cypr...
## $ family                     <chr> "anguillidae", "catostomidae", "cyprinid...
## $ genus                      <chr> "anguilla", "moxostoma", "campostoma", "...
## $ species                    <chr> "rostrata", "poecilura", "anomalum", "fu...
## $ primarymethod              <chr> "telemetry", "mark-recapture", "mark-rec...
## $ N                          <chr> "16", NA, "20", "26", "17", "5", "2", "2...
## $ mean.mass.g                <dbl> 887.00, 562.00, 34.00, 4.00, 4.00, 3525....
## $ log10.mass                 <dbl> 2.9479236, 2.7497363, 1.5314789, 0.60206...
## $ alternative.mass.reference <chr> NA, NA, NA, NA, NA, NA, NA, NA, NA, NA, ...
## $ mean.hra.m2                <dbl> 282750.00, 282.10, 116.11, 125.50, 87.10...
## $ log10.hra                  <dbl> 5.4514026, 2.4504031, 2.0648696, 2.09864...
## $ hra.reference              <chr> "Minns, C. K. 1995. Allometry of home ra...
## $ realm                      <chr> "aquatic", "aquatic", "aquatic", "aquati...
## $ thermoregulation           <chr> "ectotherm", "ectotherm", "ectotherm", "...
## $ locomotion                 <chr> "swimming", "swimming", "swimming", "swi...
## $ trophic.guild              <chr> "carnivore", "carnivore", "carnivore", "...
## $ dimension                  <chr> "3D", "2D", "2D", "2D", "2D", "2D", "2D"...
## $ preymass                   <dbl> NA, NA, NA, NA, NA, NA, 1.39, NA, NA, NA...
## $ log10.preymass             <dbl> NA, NA, NA, NA, NA, NA, 0.1430148, NA, N...
## $ PPMR                       <dbl> NA, NA, NA, NA, NA, NA, 530, NA, NA, NA,...
## $ prey.size.reference        <chr> NA, NA, NA, NA, NA, NA, "Brose U, et al....
```

```r
names(homerange)
```

```
##  [1] "taxon"                      "common.name"               
##  [3] "class"                      "order"                     
##  [5] "family"                     "genus"                     
##  [7] "species"                    "primarymethod"             
##  [9] "N"                          "mean.mass.g"               
## [11] "log10.mass"                 "alternative.mass.reference"
## [13] "mean.hra.m2"                "log10.hra"                 
## [15] "hra.reference"              "realm"                     
## [17] "thermoregulation"           "locomotion"                
## [19] "trophic.guild"              "dimension"                 
## [21] "preymass"                   "log10.preymass"            
## [23] "PPMR"                       "prey.size.reference"
```

```r
class(homerange$taxon)
```

```
## [1] "character"
```



```r
class(homerange$class)
```

```
## [1] "character"
```



```r
class(homerange$family)
```

```
## [1] "character"
```



```r
class(homerange$species)
```

```
## [1] "character"
```



```r
class(homerange$N)
```

```
## [1] "character"
```



```r
class(homerange$log10.mass)
```

```
## [1] "numeric"
```



```r
class(homerange$mean.hra.m2)
```

```
## [1] "numeric"
```



```r
class(homerange$hra.reference)
```

```
## [1] "character"
```



```r
class(homerange$thermoregulation)
```

```
## [1] "character"
```



```r
class(homerange$trophic.guild)
```

```
## [1] "character"
```



```r
class(homerange$preymass)
```

```
## [1] "numeric"
```



```r
class(homerange$PPMR)
```

```
## [1] "numeric"
```



```r
class(homerange$common.name)
```

```
## [1] "character"
```



```r
class(homerange$order)
```

```
## [1] "character"
```



```r
class(homerange$genus)
```

```
## [1] "character"
```



```r
class(homerange$primarymethod)
```

```
## [1] "character"
```



```r
class(homerange$mean.mass.g)
```

```
## [1] "numeric"
```



```r
class(homerange$alternative.mass.reference)
```

```
## [1] "character"
```



```r
class(homerange$log10.hra)
```

```
## [1] "numeric"
```



```r
class(homerange$realm)
```

```
## [1] "character"
```



```r
class(homerange$locomotion)
```

```
## [1] "character"
```



```r
class(homerange$dimension)
```

```
## [1] "character"
```



```r
class(homerange$log10.preymass)
```

```
## [1] "numeric"
```

```r
class(homerange$prey.size.reference)
```

```
## [1] "character"
```

```r
summary(homerange)
```

```
##     taxon           common.name           class              order          
##  Length:569         Length:569         Length:569         Length:569        
##  Class :character   Class :character   Class :character   Class :character  
##  Mode  :character   Mode  :character   Mode  :character   Mode  :character  
##                                                                             
##                                                                             
##                                                                             
##                                                                             
##     family             genus             species          primarymethod     
##  Length:569         Length:569         Length:569         Length:569        
##  Class :character   Class :character   Class :character   Class :character  
##  Mode  :character   Mode  :character   Mode  :character   Mode  :character  
##                                                                             
##                                                                             
##                                                                             
##                                                                             
##       N              mean.mass.g        log10.mass     
##  Length:569         Min.   :      0   Min.   :-0.6576  
##  Class :character   1st Qu.:     50   1st Qu.: 1.6990  
##  Mode  :character   Median :    330   Median : 2.5185  
##                     Mean   :  34602   Mean   : 2.5947  
##                     3rd Qu.:   2150   3rd Qu.: 3.3324  
##                     Max.   :4000000   Max.   : 6.6021  
##                                                        
##  alternative.mass.reference  mean.hra.m2          log10.hra     
##  Length:569                 Min.   :0.000e+00   Min.   :-1.523  
##  Class :character           1st Qu.:4.500e+03   1st Qu.: 3.653  
##  Mode  :character           Median :3.934e+04   Median : 4.595  
##                             Mean   :2.146e+07   Mean   : 4.709  
##                             3rd Qu.:1.038e+06   3rd Qu.: 6.016  
##                             Max.   :3.551e+09   Max.   : 9.550  
##                                                                 
##  hra.reference         realm           thermoregulation    locomotion       
##  Length:569         Length:569         Length:569         Length:569        
##  Class :character   Class :character   Class :character   Class :character  
##  Mode  :character   Mode  :character   Mode  :character   Mode  :character  
##                                                                             
##                                                                             
##                                                                             
##                                                                             
##  trophic.guild       dimension            preymass         log10.preymass   
##  Length:569         Length:569         Min.   :     0.67   Min.   :-0.1739  
##  Class :character   Class :character   1st Qu.:    20.02   1st Qu.: 1.3014  
##  Mode  :character   Mode  :character   Median :    53.75   Median : 1.7304  
##                                        Mean   :  3989.88   Mean   : 2.0188  
##                                        3rd Qu.:   363.35   3rd Qu.: 2.5603  
##                                        Max.   :130233.20   Max.   : 5.1147  
##                                        NA's   :502         NA's   :502      
##       PPMR         prey.size.reference
##  Min.   :  0.380   Length:569         
##  1st Qu.:  3.315   Class :character   
##  Median :  7.190   Mode  :character   
##  Mean   : 31.752                      
##  3rd Qu.: 15.966                      
##  Max.   :530.000                      
##  NA's   :502
```

**3. Change the class of the variables `taxon` and `order` to factors and display their levels.**  

```r
homerange$taxon <- as.factor(homerange$taxon)
class(homerange$taxon)
```

```
## [1] "factor"
```

```r
levels(homerange$taxon)
```

```
## [1] "birds"         "lake fishes"   "lizards"       "mammals"      
## [5] "marine fishes" "river fishes"  "snakes"        "tortoises"    
## [9] "turtles"
```

```r
homerange$order <- as.factor(homerange$order)
class(homerange$order)
```

```
## [1] "factor"
```

```r
levels(homerange$order)
```

```
##  [1] "accipitriformes"    "afrosoricida"       "anguilliformes"    
##  [4] "anseriformes"       "apterygiformes"     "artiodactyla"      
##  [7] "caprimulgiformes"   "carnivora"          "charadriiformes"   
## [10] "columbidormes"      "columbiformes"      "coraciiformes"     
## [13] "cuculiformes"       "cypriniformes"      "dasyuromorpha"     
## [16] "dasyuromorpia"      "didelphimorphia"    "diprodontia"       
## [19] "diprotodontia"      "erinaceomorpha"     "esociformes"       
## [22] "falconiformes"      "gadiformes"         "galliformes"       
## [25] "gruiformes"         "lagomorpha"         "macroscelidea"     
## [28] "monotrematae"       "passeriformes"      "pelecaniformes"    
## [31] "peramelemorphia"    "perciformes"        "perissodactyla"    
## [34] "piciformes"         "pilosa"             "proboscidea"       
## [37] "psittaciformes"     "rheiformes"         "roden"             
## [40] "rodentia"           "salmoniformes"      "scorpaeniformes"   
## [43] "siluriformes"       "soricomorpha"       "squamata"          
## [46] "strigiformes"       "struthioniformes"   "syngnathiformes"   
## [49] "testudines"         "tetraodontiformes<U+00A0>" "tinamiformes"
```

**4. What taxa are represented in the `homerange` data frame? Make a new data frame `taxa` that is restricted to taxon, common name, class, order, family, genus, species.**  

```r
taxa <- select(homerange, taxon, common.name, class, order, family, genus, species)
taxa
```

```
## # A tibble: 569 x 7
##    taxon     common.name       class      order     family    genus    species  
##    <fct>     <chr>             <chr>      <fct>     <chr>     <chr>    <chr>    
##  1 lake fis~ american eel      actinopte~ anguilli~ anguilli~ anguilla rostrata 
##  2 river fi~ blacktail redhor~ actinopte~ cyprinif~ catostom~ moxosto~ poecilura
##  3 river fi~ central stonerol~ actinopte~ cyprinif~ cyprinid~ campost~ anomalum 
##  4 river fi~ rosyside dace     actinopte~ cyprinif~ cyprinid~ clinost~ funduloi~
##  5 river fi~ longnose dace     actinopte~ cyprinif~ cyprinid~ rhinich~ cataract~
##  6 river fi~ muskellunge       actinopte~ esocifor~ esocidae  esox     masquino~
##  7 marine f~ pollack           actinopte~ gadiform~ gadidae   pollach~ pollachi~
##  8 marine f~ saithe            actinopte~ gadiform~ gadidae   pollach~ virens   
##  9 marine f~ lined surgeonfish actinopte~ percifor~ acanthur~ acanthu~ lineatus 
## 10 marine f~ orangespine unic~ actinopte~ percifor~ acanthur~ naso     lituratus
## # ... with 559 more rows
```

**5. The variable `taxon` identifies the large, common name groups of the species represented in `homerange`. Make a table the shows the counts for each of these `taxon`.**  

```r
table(homerange$taxon)
```

```
## 
##         birds   lake fishes       lizards       mammals marine fishes 
##           140             9            11           238            90 
##  river fishes        snakes     tortoises       turtles 
##            14            41            12            14
```

**6. The species in `homerange` are also classified into trophic guilds. How many species are represented in each trophic guild.**  

```r
table(homerange$trophic.guild)
```

```
## 
## carnivore herbivore 
##       342       227
```

**7. Make two new data frames, one which is restricted to carnivores and another that is restricted to herbivores.**  

```r
carnivores <- filter(homerange, trophic.guild == "carnivore")
carnivores
```

```
## # A tibble: 342 x 24
##    taxon common.name class order family genus species primarymethod N    
##    <fct> <chr>       <chr> <fct> <chr>  <chr> <chr>   <chr>         <chr>
##  1 lake~ american e~ acti~ angu~ angui~ angu~ rostra~ telemetry     16   
##  2 rive~ blacktail ~ acti~ cypr~ catos~ moxo~ poecil~ mark-recaptu~ <NA> 
##  3 rive~ central st~ acti~ cypr~ cypri~ camp~ anomal~ mark-recaptu~ 20   
##  4 rive~ rosyside d~ acti~ cypr~ cypri~ clin~ fundul~ mark-recaptu~ 26   
##  5 rive~ longnose d~ acti~ cypr~ cypri~ rhin~ catara~ mark-recaptu~ 17   
##  6 rive~ muskellunge acti~ esoc~ esoci~ esox  masqui~ telemetry     5    
##  7 mari~ pollack     acti~ gadi~ gadid~ poll~ pollac~ telemetry     2    
##  8 mari~ saithe      acti~ gadi~ gadid~ poll~ virens  telemetry     2    
##  9 mari~ giant trev~ acti~ perc~ caran~ cara~ ignobi~ telemetry     4    
## 10 lake~ rock bass   acti~ perc~ centr~ ambl~ rupest~ mark-recaptu~ 16   
## # ... with 332 more rows, and 15 more variables: mean.mass.g <dbl>,
## #   log10.mass <dbl>, alternative.mass.reference <chr>, mean.hra.m2 <dbl>,
## #   log10.hra <dbl>, hra.reference <chr>, realm <chr>, thermoregulation <chr>,
## #   locomotion <chr>, trophic.guild <chr>, dimension <chr>, preymass <dbl>,
## #   log10.preymass <dbl>, PPMR <dbl>, prey.size.reference <chr>
```

```r
herbivores <- filter(homerange, trophic.guild == "herbivore")
herbivores
```

```
## # A tibble: 227 x 24
##    taxon common.name class order family genus species primarymethod N    
##    <fct> <chr>       <chr> <fct> <chr>  <chr> <chr>   <chr>         <chr>
##  1 mari~ lined surg~ acti~ perc~ acant~ acan~ lineat~ direct obser~ <NA> 
##  2 mari~ orangespin~ acti~ perc~ acant~ naso  litura~ telemetry     8    
##  3 mari~ bluespine ~ acti~ perc~ acant~ naso  unicor~ telemetry     7    
##  4 mari~ redlip ble~ acti~ perc~ blenn~ ophi~ atlant~ direct obser~ 20   
##  5 mari~ bermuda ch~ acti~ perc~ kypho~ kyph~ sectat~ telemetry     11   
##  6 mari~ cherubfish  acti~ perc~ pomac~ cent~ argi    direct obser~ <NA> 
##  7 mari~ damselfish  acti~ perc~ pomac~ chro~ chromis direct obser~ <NA> 
##  8 mari~ twinspot d~ acti~ perc~ pomac~ chry~ biocel~ direct obser~ 18   
##  9 mari~ wards dams~ acti~ perc~ pomac~ poma~ wardi   direct obser~ <NA> 
## 10 mari~ australian~ acti~ perc~ pomac~ steg~ apical~ direct obser~ <NA> 
## # ... with 217 more rows, and 15 more variables: mean.mass.g <dbl>,
## #   log10.mass <dbl>, alternative.mass.reference <chr>, mean.hra.m2 <dbl>,
## #   log10.hra <dbl>, hra.reference <chr>, realm <chr>, thermoregulation <chr>,
## #   locomotion <chr>, trophic.guild <chr>, dimension <chr>, preymass <dbl>,
## #   log10.preymass <dbl>, PPMR <dbl>, prey.size.reference <chr>
```

**8. Do herbivores or carnivores have, on average, a larger `mean.hra.m2`? Remove any NAs from the data.**  
**herbivores have a larger mean.hra.m2.**

```r
mean(carnivores$mean.hra.m2, na.rm = T)
```

```
## [1] 13039918
```

```r
mean(herbivores$mean.hra.m2, na.rm = T)
```

```
## [1] 34137012
```

**9. Make a new dataframe `deer` that is limited to the mean mass, log10 mass, family, genus, and species of deer in the database. The family for deer is cervidae. Arrange the data in descending order by log10 mass. Which is the largest deer? What is its common name?**  
**The largest deer's common name is moose.**

```r
deer_relimited <- select(homerange, "log10.mass", "mean.mass.g", "family", "genus", "species")
deer_relimited
```

```
## # A tibble: 569 x 5
##    log10.mass mean.mass.g family       genus       species    
##         <dbl>       <dbl> <chr>        <chr>       <chr>      
##  1      2.95         887  anguillidae  anguilla    rostrata   
##  2      2.75         562  catostomidae moxostoma   poecilura  
##  3      1.53          34  cyprinidae   campostoma  anomalum   
##  4      0.602          4  cyprinidae   clinostomus funduloides
##  5      0.602          4  cyprinidae   rhinichthys cataractae 
##  6      3.55        3525  esocidae     esox        masquinongy
##  7      2.87         737. gadidae      pollachius  pollachius 
##  8      2.65         449. gadidae      pollachius  virens     
##  9      2.04         109. acanthuridae acanthurus  lineatus   
## 10      2.89         772. acanthuridae naso        lituratus  
## # ... with 559 more rows
```

```r
deer <- filter(deer_relimited, family == "cervidae")
deer
```

```
## # A tibble: 12 x 5
##    log10.mass mean.mass.g family   genus      species    
##         <dbl>       <dbl> <chr>    <chr>      <chr>      
##  1       5.49     307227. cervidae alces      alces      
##  2       4.80      62823. cervidae axis       axis       
##  3       4.38      24050. cervidae capreolus  capreolus  
##  4       5.37     234758. cervidae cervus     elaphus    
##  5       4.47      29450. cervidae cervus     nippon     
##  6       4.85      71450. cervidae dama       dama       
##  7       4.13      13500. cervidae muntiacus  reevesi    
##  8       4.73      53864. cervidae odocoileus hemionus   
##  9       4.94      87884. cervidae odocoileus virginianus
## 10       4.54      35000. cervidae ozotoceros bezoarticus
## 11       3.88       7500. cervidae pudu       puda       
## 12       5.01     102059. cervidae rangifer   tarandus
```

```r
deer[order(deer$log10.mass, decreasing = T),]
```

```
## # A tibble: 12 x 5
##    log10.mass mean.mass.g family   genus      species    
##         <dbl>       <dbl> <chr>    <chr>      <chr>      
##  1       5.49     307227. cervidae alces      alces      
##  2       5.37     234758. cervidae cervus     elaphus    
##  3       5.01     102059. cervidae rangifer   tarandus   
##  4       4.94      87884. cervidae odocoileus virginianus
##  5       4.85      71450. cervidae dama       dama       
##  6       4.80      62823. cervidae axis       axis       
##  7       4.73      53864. cervidae odocoileus hemionus   
##  8       4.54      35000. cervidae ozotoceros bezoarticus
##  9       4.47      29450. cervidae cervus     nippon     
## 10       4.38      24050. cervidae capreolus  capreolus  
## 11       4.13      13500. cervidae muntiacus  reevesi    
## 12       3.88       7500. cervidae pudu       puda
```

```r
filter(homerange, between(log10.mass, 5.4, 5.5))
```

```
## # A tibble: 1 x 24
##   taxon common.name class order family genus species primarymethod N    
##   <fct> <chr>       <chr> <fct> <chr>  <chr> <chr>   <chr>         <chr>
## 1 mamm~ moose       mamm~ arti~ cervi~ alces alces   telemetry*    <NA> 
## # ... with 15 more variables: mean.mass.g <dbl>, log10.mass <dbl>,
## #   alternative.mass.reference <chr>, mean.hra.m2 <dbl>, log10.hra <dbl>,
## #   hra.reference <chr>, realm <chr>, thermoregulation <chr>, locomotion <chr>,
## #   trophic.guild <chr>, dimension <chr>, preymass <dbl>, log10.preymass <dbl>,
## #   PPMR <dbl>, prey.size.reference <chr>
```

**10. As measured by the data, which snake species has the smallest homerange? Show all of your work, please. Look this species up online and tell me about it!** **Snake is found in taxon column**   
**Schneideri has the smallest homerange. It is a small venomous viper in Africa (possibly the smallest viper in the world).**

```r
snake_relimited <- select(homerange, taxon, species, mean.hra.m2)
snake_relimited
```

```
## # A tibble: 569 x 3
##    taxon         species     mean.hra.m2
##    <fct>         <chr>             <dbl>
##  1 lake fishes   rostrata       282750  
##  2 river fishes  poecilura         282. 
##  3 river fishes  anomalum          116. 
##  4 river fishes  funduloides       126. 
##  5 river fishes  cataractae         87.1
##  6 river fishes  masquinongy     39344. 
##  7 marine fishes pollachius       9056. 
##  8 marine fishes virens          44516. 
##  9 marine fishes lineatus           11.1
## 10 marine fishes lituratus       32093. 
## # ... with 559 more rows
```

```r
snakes <- filter(snake_relimited, taxon == "snakes")
snakes
```

```
## # A tibble: 41 x 3
##    taxon  species                  mean.hra.m2
##    <fct>  <chr>                          <dbl>
##  1 snakes vermis                           700
##  2 snakes viridis                          253
##  3 snakes constrictor                   151000
##  4 snakes constrictor flaviventris      114500
##  5 snakes punctatus                       6476
##  6 snakes couperi                      1853000
##  7 snakes guttata emoryi                150600
##  8 snakes obsoleta                       46000
##  9 snakes platirhinos                   516375
## 10 snakes viridiflavus                  110900
## # ... with 31 more rows
```

```r
snakes[order(snakes$mean.hra.m2, decreasing = F),]
```

```
## # A tibble: 41 x 3
##    taxon  species      mean.hra.m2
##    <fct>  <chr>              <dbl>
##  1 snakes schneideri          200 
##  2 snakes viridis             253 
##  3 snakes butleri             600 
##  4 snakes vermis              700 
##  5 snakes latastei           2400 
##  6 snakes shedaoensis        2614.
##  7 snakes punctatus          6476 
##  8 snakes piscivorous       10655 
##  9 snakes rufodorsatus      15400 
## 10 snakes catenifer         17400 
## # ... with 31 more rows
```


## Push your final code to GitHub!
Please be sure that you check the `keep md` file in the knit preferences.   
