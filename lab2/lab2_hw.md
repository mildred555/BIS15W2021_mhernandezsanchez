---
title: "Lab 2 Homework"
author: "Mildred Hernandez"
date: "2021-01-12"
output:
  html_document: 
    theme: spacelab
    keep_md: yes
---

## Instructions
Answer the following questions and complete the exercises in RMarkdown. Please embed all of your code and push your final work to your repository. Your final lab report should be organized, clean, and run free from errors. Remember, you must remove the `#` for the included code chunks to run. Be sure to add your name to the author header above.  

Make sure to use the formatting conventions of RMarkdown to make your report neat and clean!  

1. What is a vector in R?  
**It is a basic data structure in R. It is also a sequence of data elements, of the same type, for an object.**

2. What is a data matrix in R?  
**It is a collection of data elements arranged in a two dimensional rectangular layout.**

3. Below are data collected by three scientists (Jill, Steve, Susan in order) measuring temperatures of eight hot springs. Run this code chunk to create the vectors.  

```r
spring_1 <- c(36.25, 35.40, 35.30)
spring_2 <- c(35.15, 35.35, 33.35)
spring_3 <- c(30.70, 29.65, 29.20)
spring_4 <- c(39.70, 40.05, 38.65)
spring_5 <- c(31.85, 31.40, 29.30)
spring_6 <- c(30.20, 30.65, 29.75)
spring_7 <- c(32.90, 32.50, 32.80)
spring_8 <- c(36.80, 36.45, 33.15)
```

4. Build a data matrix that has the springs as rows and the columns as scientists.  

```r
temp_of_hot_springs <- c(spring_1, spring_2, spring_3, spring_4, spring_5, spring_6, spring_7, spring_8)
temp_of_hot_springs
```

```
##  [1] 36.25 35.40 35.30 35.15 35.35 33.35 30.70 29.65 29.20 39.70 40.05 38.65
## [13] 31.85 31.40 29.30 30.20 30.65 29.75 32.90 32.50 32.80 36.80 36.45 33.15
```

```r
hot_springs_matrix <- matrix (temp_of_hot_springs, nrow = 8, byrow = T)
hot_springs_matrix
```

```
##       [,1]  [,2]  [,3]
## [1,] 36.25 35.40 35.30
## [2,] 35.15 35.35 33.35
## [3,] 30.70 29.65 29.20
## [4,] 39.70 40.05 38.65
## [5,] 31.85 31.40 29.30
## [6,] 30.20 30.65 29.75
## [7,] 32.90 32.50 32.80
## [8,] 36.80 36.45 33.15
```

5. The names of the springs are 1.Bluebell Spring, 2.Opal Spring, 3.Riverside Spring, 4.Too Hot Spring, 5.Mystery Spring, 6.Emerald Spring, 7.Black Spring, 8.Pearl Spring. Name the rows and columns in the data matrix. Start by making two new vectors with the names, then use `colnames()` and `rownames()` to name the columns and rows.

```r
titles <- c("Bluebell Spring", "Opal Spring", "Riverside Spring", "Too Hot Spring", "Mystery Spring", "Emerald Spring", "Black Spring", "Pearl Spring")
titles
```

```
## [1] "Bluebell Spring"  "Opal Spring"      "Riverside Spring" "Too Hot Spring"  
## [5] "Mystery Spring"   "Emerald Spring"   "Black Spring"     "Pearl Spring"
```

```r
scientists <- c("Jill", "Steve", "Susan")
```

```r
rownames(hot_springs_matrix) <- titles
```

```r
colnames(hot_springs_matrix) <- scientists
```

```r
hot_springs_matrix
```

```
##                   Jill Steve Susan
## Bluebell Spring  36.25 35.40 35.30
## Opal Spring      35.15 35.35 33.35
## Riverside Spring 30.70 29.65 29.20
## Too Hot Spring   39.70 40.05 38.65
## Mystery Spring   31.85 31.40 29.30
## Emerald Spring   30.20 30.65 29.75
## Black Spring     32.90 32.50 32.80
## Pearl Spring     36.80 36.45 33.15
```

6. Calculate the mean temperature of all eight springs.

```r
hot_springs_mean_1 <- c(hot_springs_matrix[1, ])
hot_springs_mean_1
```

```
##  Jill Steve Susan 
## 36.25 35.40 35.30
```

```r
mean_1 <- mean(hot_springs_mean_1)
mean_1
```

```
## [1] 35.65
```

```r
hot_springs_mean_2 <- c(hot_springs_matrix[2, ])
```

```r
mean_2 <- mean(hot_springs_mean_2)
```

```r
hot_springs_mean_3 <- c(hot_springs_matrix[3, ])
```

```r
mean_3 <- mean(hot_springs_mean_3)
```

```r
hot_springs_mean_4 <- c(hot_springs_matrix[4, ])
```

```r
mean_4 <- mean(hot_springs_mean_4)
```

```r
hot_springs_mean_5 <- c(hot_springs_matrix[5, ])
```

```r
mean_5 <- mean(hot_springs_mean_5)
```

```r
hot_springs_mean_6 <- c(hot_springs_matrix[6, ])
```

```r
mean_6 <- mean(hot_springs_mean_6)
```

```r
hot_springs_mean_7 <- c(hot_springs_matrix[7, ])
```

```r
mean_7 <- mean(hot_springs_mean_7)
```

```r
hot_springs_mean_8 <- c(hot_springs_matrix[8, ])
hot_springs_mean_8
```

```
##  Jill Steve Susan 
## 36.80 36.45 33.15
```

```r
mean_8 <- mean(hot_springs_mean_8)
mean_8
```

```
## [1] 35.46667
```

```r
all_hot_springs_mean <- c(mean_1, mean_2, mean_3, mean_4, mean_5, mean_6, mean_7, mean_8)
```

```r
mean(all_hot_springs_mean)
```

```
## [1] 33.60417
```

7. Add this as a new column in the data matrix.  

```r
total <- c(all_hot_springs_mean)
total
```

```
## [1] 35.65000 34.61667 29.85000 39.46667 30.85000 30.20000 32.73333 35.46667
```


```r
all_hot_springs_matrix <- cbind(hot_springs_matrix, total)
all_hot_springs_matrix
```

```
##                   Jill Steve Susan    total
## Bluebell Spring  36.25 35.40 35.30 35.65000
## Opal Spring      35.15 35.35 33.35 34.61667
## Riverside Spring 30.70 29.65 29.20 29.85000
## Too Hot Spring   39.70 40.05 38.65 39.46667
## Mystery Spring   31.85 31.40 29.30 30.85000
## Emerald Spring   30.20 30.65 29.75 30.20000
## Black Spring     32.90 32.50 32.80 32.73333
## Pearl Spring     36.80 36.45 33.15 35.46667
```

8. Show Susan's value for Opal Spring 

```r
all_hot_springs_matrix[2,3]
```

```
## [1] 33.35
```

9. Calculate the mean for Jill's column only.  

```r
jill_mean <- c(all_hot_springs_matrix[ ,1])
jill_mean
```

```
##  Bluebell Spring      Opal Spring Riverside Spring   Too Hot Spring 
##            36.25            35.15            30.70            39.70 
##   Mystery Spring   Emerald Spring     Black Spring     Pearl Spring 
##            31.85            30.20            32.90            36.80
```

```r
mean_j <- mean(jill_mean)
mean_j
```

```
## [1] 34.19375
```

10. Use the data matrix to perform one calculation or operation of your interest.

```r
steve_mean <-c(all_hot_springs_matrix[ ,2])
steve_mean
```

```
##  Bluebell Spring      Opal Spring Riverside Spring   Too Hot Spring 
##            35.40            35.35            29.65            40.05 
##   Mystery Spring   Emerald Spring     Black Spring     Pearl Spring 
##            31.40            30.65            32.50            36.45
```

```r
mean_s <- mean(steve_mean)
mean_s
```

```
## [1] 33.93125
```

## Push your final code to GitHub!
Please be sure that you check the `keep md` file in the knit preferences.  
