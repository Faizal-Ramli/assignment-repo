Importing and Preparing the Data
--------------------------------

The training dataset used in this assignment is pml-training.csv, but since some features in this dataset contain NA or empty string, I firstly delete those features before importing it into R. I named the new data as training-pml.csv which is a pre-cleaned dataset from pml-training that contains only features with no NA nor empty string.

``` r
pml <- read.csv2("training-pml.csv")
dim(pml)
```

    ## [1] 19622    58

``` r
str(pml)
```

    ## 'data.frame':    19622 obs. of  58 variables:
    ##  $ user_name           : Factor w/ 6 levels "adelmo","carlitos",..: 2 2 2 2 2 2 2 2 2 2 ...
    ##  $ raw_timestamp_part_1: int  1323084231 1323084231 1323084231 1323084232 1323084232 1323084232 1323084232 1323084232 1323084232 1323084232 ...
    ##  $ raw_timestamp_part_2: int  788290 808298 820366 120339 196328 304277 368296 440390 484323 484434 ...
    ##  $ new_window          : Factor w/ 2 levels "no","yes": 1 1 1 1 1 1 1 1 1 1 ...
    ##  $ num_window          : int  11 11 11 12 12 12 12 12 12 12 ...
    ##  $ roll_belt           : Factor w/ 1330 levels "-0.01","-0.02",..: 771 771 772 778 778 775 772 772 773 775 ...
    ##  $ pitch_belt          : Factor w/ 1840 levels "-0.01","-0.02",..: 1681 1681 1681 1679 1681 1680 1683 1687 1690 1691 ...
    ##  $ yaw_belt            : Factor w/ 1957 levels "-0.02","-0.03",..: 1122 1122 1122 1122 1122 1122 1122 1122 1122 1122 ...
    ##  $ total_accel_belt    : int  3 3 3 3 3 3 3 3 3 3 ...
    ##  $ gyros_belt_x        : Factor w/ 140 levels "-0.02","-0.03",..: 64 65 64 65 65 65 65 65 65 66 ...
    ##  $ gyros_belt_y        : Factor w/ 69 levels "-0.02","-0.03",..: 33 33 33 33 34 33 33 33 33 33 ...
    ##  $ gyros_belt_z        : Factor w/ 169 levels "-0.02","-0.03",..: 1 1 1 2 1 1 1 1 1 80 ...
    ##  $ accel_belt_x        : int  -21 -22 -20 -22 -21 -21 -22 -22 -20 -21 ...
    ##  $ accel_belt_y        : int  4 4 5 3 2 4 3 4 2 4 ...
    ##  $ accel_belt_z        : int  22 22 23 21 24 21 21 21 24 22 ...
    ##  $ magnet_belt_x       : int  -3 -7 -2 -6 -6 0 -4 -2 1 -3 ...
    ##  $ magnet_belt_y       : int  599 608 600 604 600 603 599 603 602 609 ...
    ##  $ magnet_belt_z       : int  -313 -311 -305 -310 -302 -312 -311 -313 -312 -308 ...
    ##  $ roll_arm            : Factor w/ 2654 levels "-0.02","-0.04",..: 120 120 120 120 120 120 120 120 120 120 ...
    ##  $ pitch_arm           : Factor w/ 3087 levels "-0.01","-0.02",..: 1882 1882 1882 1878 1878 1877 1876 1875 1874 1873 ...
    ##  $ yaw_arm             : Factor w/ 2876 levels "-0.02","-0.05",..: 212 212 212 212 212 212 212 212 212 212 ...
    ##  $ total_accel_arm     : int  34 34 34 34 34 34 34 34 34 34 ...
    ##  $ gyros_arm_x         : Factor w/ 643 levels "-0.02","-0.03",..: 356 357 357 357 356 357 356 357 357 357 ...
    ##  $ gyros_arm_y         : Factor w/ 376 levels "-0.02","-0.03",..: 202 1 1 2 2 2 2 1 2 2 ...
    ##  $ gyros_arm_z         : Factor w/ 248 levels "-0.02","-0.03",..: 1 1 1 114 113 113 113 113 1 1 ...
    ##  $ accel_arm_x         : int  -288 -290 -289 -289 -289 -289 -289 -289 -288 -288 ...
    ##  $ accel_arm_y         : int  109 110 110 111 111 111 111 111 109 110 ...
    ##  $ accel_arm_z         : int  -123 -125 -126 -123 -123 -122 -125 -124 -122 -124 ...
    ##  $ magnet_arm_x        : int  -368 -369 -368 -372 -374 -369 -373 -372 -369 -376 ...
    ##  $ magnet_arm_y        : int  337 337 344 344 337 342 336 338 341 334 ...
    ##  $ magnet_arm_z        : int  516 513 513 512 506 513 509 510 518 516 ...
    ##  $ roll_dumbbell       : Factor w/ 16523 levels "-0.970112066",..: 6515 6531 6197 6574 6567 6568 6530 6179 6537 6562 ...
    ##  $ pitch_dumbbell      : Factor w/ 16040 levels "-0.45631348",..: 9693 9701 9671 9684 9689 9717 9669 9680 9688 9720 ...
    ##  $ yaw_dumbbell        : Factor w/ 16381 levels "-0.585821306",..: 6625 6577 6688 6624 6615 6520 6674 6673 6636 6515 ...
    ##  $ total_accel_dumbbell: int  37 37 37 37 37 37 37 37 37 37 ...
    ##  $ gyros_dumbbell_x    : Factor w/ 241 levels "-0.02","-0.03",..: 115 115 115 115 115 115 115 115 115 115 ...
    ##  $ gyros_dumbbell_y    : Factor w/ 278 levels "-0.02","-0.03",..: 1 1 1 1 1 1 1 1 1 1 ...
    ##  $ gyros_dumbbell_z    : Factor w/ 206 levels "-0.02","-0.03",..: 114 114 114 1 114 114 114 114 114 114 ...
    ##  $ accel_dumbbell_x    : int  -234 -233 -232 -232 -233 -234 -232 -234 -232 -235 ...
    ##  $ accel_dumbbell_y    : int  47 47 46 48 48 48 47 46 47 48 ...
    ##  $ accel_dumbbell_z    : int  -271 -269 -270 -269 -270 -269 -270 -272 -269 -270 ...
    ##  $ magnet_dumbbell_x   : int  -559 -555 -561 -552 -554 -558 -551 -555 -549 -558 ...
    ##  $ magnet_dumbbell_y   : int  293 296 298 303 292 294 295 300 292 291 ...
    ##  $ magnet_dumbbell_z   : Factor w/ 676 levels "-1","-10","-100",..: 208 207 206 203 211 209 214 218 208 212 ...
    ##  $ roll_forearm        : Factor w/ 2176 levels "-0.04","-0.05",..: 1391 1390 1390 1388 1387 1386 1386 1385 1384 1384 ...
    ##  $ pitch_forearm       : Factor w/ 2915 levels "-0.01","-0.02",..: 1073 1073 1073 1073 1073 1073 1073 1072 1072 1072 ...
    ##  $ yaw_forearm         : Factor w/ 1991 levels "-0.04","-0.05",..: 126 126 125 125 125 125 125 125 125 125 ...
    ##  $ total_accel_forearm : int  36 36 36 36 36 36 36 36 36 36 ...
    ##  $ gyros_forearm_x     : Factor w/ 298 levels "-0.02","-0.03",..: 153 152 153 152 152 152 152 152 153 152 ...
    ##  $ gyros_forearm_y     : Factor w/ 741 levels "-0.02","-0.03",..: 381 381 1 1 381 1 381 1 381 381 ...
    ##  $ gyros_forearm_z     : Factor w/ 307 levels "-0.02","-0.03",..: 1 1 144 144 1 2 1 144 1 1 ...
    ##  $ accel_forearm_x     : int  192 192 196 189 189 193 195 193 193 190 ...
    ##  $ accel_forearm_y     : int  203 203 204 206 206 203 205 205 204 205 ...
    ##  $ magnet_forearm_x    : int  -215 -216 -213 -214 -214 -215 -215 -213 -214 -215 ...
    ##  $ magnet_forearm_x.1  : int  -17 -18 -18 -16 -17 -9 -18 -9 -16 -22 ...
    ##  $ magnet_forearm_y    : Factor w/ 1872 levels "-0.123","-1",..: 1521 1529 1525 1525 1522 1528 1526 1528 1520 1523 ...
    ##  $ magnet_forearm_z    : Factor w/ 1683 levels "-0.0917","-1",..: 1128 1125 1120 1120 1125 1130 1122 1126 1128 1125 ...
    ##  $ classe              : Factor w/ 5 levels "A","B","C","D",..: 1 1 1 1 1 1 1 1 1 1 ...

``` r
for (i in 6:(dim(pml)[2]-1)) {
  pml[,i] <- as.numeric(paste(pml[,i]))
}
str(pml)
```

    ## 'data.frame':    19622 obs. of  58 variables:
    ##  $ user_name           : Factor w/ 6 levels "adelmo","carlitos",..: 2 2 2 2 2 2 2 2 2 2 ...
    ##  $ raw_timestamp_part_1: int  1323084231 1323084231 1323084231 1323084232 1323084232 1323084232 1323084232 1323084232 1323084232 1323084232 ...
    ##  $ raw_timestamp_part_2: int  788290 808298 820366 120339 196328 304277 368296 440390 484323 484434 ...
    ##  $ new_window          : Factor w/ 2 levels "no","yes": 1 1 1 1 1 1 1 1 1 1 ...
    ##  $ num_window          : int  11 11 11 12 12 12 12 12 12 12 ...
    ##  $ roll_belt           : num  1.41 1.41 1.42 1.48 1.48 1.45 1.42 1.42 1.43 1.45 ...
    ##  $ pitch_belt          : num  8.07 8.07 8.07 8.05 8.07 8.06 8.09 8.13 8.16 8.17 ...
    ##  $ yaw_belt            : num  -94.4 -94.4 -94.4 -94.4 -94.4 -94.4 -94.4 -94.4 -94.4 -94.4 ...
    ##  $ total_accel_belt    : num  3 3 3 3 3 3 3 3 3 3 ...
    ##  $ gyros_belt_x        : num  0 0.02 0 0.02 0.02 0.02 0.02 0.02 0.02 0.03 ...
    ##  $ gyros_belt_y        : num  0 0 0 0 0.02 0 0 0 0 0 ...
    ##  $ gyros_belt_z        : num  -0.02 -0.02 -0.02 -0.03 -0.02 -0.02 -0.02 -0.02 -0.02 0 ...
    ##  $ accel_belt_x        : num  -21 -22 -20 -22 -21 -21 -22 -22 -20 -21 ...
    ##  $ accel_belt_y        : num  4 4 5 3 2 4 3 4 2 4 ...
    ##  $ accel_belt_z        : num  22 22 23 21 24 21 21 21 24 22 ...
    ##  $ magnet_belt_x       : num  -3 -7 -2 -6 -6 0 -4 -2 1 -3 ...
    ##  $ magnet_belt_y       : num  599 608 600 604 600 603 599 603 602 609 ...
    ##  $ magnet_belt_z       : num  -313 -311 -305 -310 -302 -312 -311 -313 -312 -308 ...
    ##  $ roll_arm            : num  -128 -128 -128 -128 -128 -128 -128 -128 -128 -128 ...
    ##  $ pitch_arm           : num  22.5 22.5 22.5 22.1 22.1 22 21.9 21.8 21.7 21.6 ...
    ##  $ yaw_arm             : num  -161 -161 -161 -161 -161 -161 -161 -161 -161 -161 ...
    ##  $ total_accel_arm     : num  34 34 34 34 34 34 34 34 34 34 ...
    ##  $ gyros_arm_x         : num  0 0.02 0.02 0.02 0 0.02 0 0.02 0.02 0.02 ...
    ##  $ gyros_arm_y         : num  0 -0.02 -0.02 -0.03 -0.03 -0.03 -0.03 -0.02 -0.03 -0.03 ...
    ##  $ gyros_arm_z         : num  -0.02 -0.02 -0.02 0.02 0 0 0 0 -0.02 -0.02 ...
    ##  $ accel_arm_x         : num  -288 -290 -289 -289 -289 -289 -289 -289 -288 -288 ...
    ##  $ accel_arm_y         : num  109 110 110 111 111 111 111 111 109 110 ...
    ##  $ accel_arm_z         : num  -123 -125 -126 -123 -123 -122 -125 -124 -122 -124 ...
    ##  $ magnet_arm_x        : num  -368 -369 -368 -372 -374 -369 -373 -372 -369 -376 ...
    ##  $ magnet_arm_y        : num  337 337 344 344 337 342 336 338 341 334 ...
    ##  $ magnet_arm_z        : num  516 513 513 512 506 513 509 510 518 516 ...
    ##  $ roll_dumbbell       : num  13.1 13.1 12.9 13.4 13.4 ...
    ##  $ pitch_dumbbell      : num  -70.5 -70.6 -70.3 -70.4 -70.4 ...
    ##  $ yaw_dumbbell        : num  -84.9 -84.7 -85.1 -84.9 -84.9 ...
    ##  $ total_accel_dumbbell: num  37 37 37 37 37 37 37 37 37 37 ...
    ##  $ gyros_dumbbell_x    : num  0 0 0 0 0 0 0 0 0 0 ...
    ##  $ gyros_dumbbell_y    : num  -0.02 -0.02 -0.02 -0.02 -0.02 -0.02 -0.02 -0.02 -0.02 -0.02 ...
    ##  $ gyros_dumbbell_z    : num  0 0 0 -0.02 0 0 0 0 0 0 ...
    ##  $ accel_dumbbell_x    : num  -234 -233 -232 -232 -233 -234 -232 -234 -232 -235 ...
    ##  $ accel_dumbbell_y    : num  47 47 46 48 48 48 47 46 47 48 ...
    ##  $ accel_dumbbell_z    : num  -271 -269 -270 -269 -270 -269 -270 -272 -269 -270 ...
    ##  $ magnet_dumbbell_x   : num  -559 -555 -561 -552 -554 -558 -551 -555 -549 -558 ...
    ##  $ magnet_dumbbell_y   : num  293 296 298 303 292 294 295 300 292 291 ...
    ##  $ magnet_dumbbell_z   : num  -65 -64 -63 -60 -68 -66 -70 -74 -65 -69 ...
    ##  $ roll_forearm        : num  28.4 28.3 28.3 28.1 28 27.9 27.9 27.8 27.7 27.7 ...
    ##  $ pitch_forearm       : num  -63.9 -63.9 -63.9 -63.9 -63.9 -63.9 -63.9 -63.8 -63.8 -63.8 ...
    ##  $ yaw_forearm         : num  -153 -153 -152 -152 -152 -152 -152 -152 -152 -152 ...
    ##  $ total_accel_forearm : num  36 36 36 36 36 36 36 36 36 36 ...
    ##  $ gyros_forearm_x     : num  0.03 0.02 0.03 0.02 0.02 0.02 0.02 0.02 0.03 0.02 ...
    ##  $ gyros_forearm_y     : num  0 0 -0.02 -0.02 0 -0.02 0 -0.02 0 0 ...
    ##  $ gyros_forearm_z     : num  -0.02 -0.02 0 0 -0.02 -0.03 -0.02 0 -0.02 -0.02 ...
    ##  $ accel_forearm_x     : num  192 192 196 189 189 193 195 193 193 190 ...
    ##  $ accel_forearm_y     : num  203 203 204 206 206 203 205 205 204 205 ...
    ##  $ magnet_forearm_x    : num  -215 -216 -213 -214 -214 -215 -215 -213 -214 -215 ...
    ##  $ magnet_forearm_x.1  : num  -17 -18 -18 -16 -17 -9 -18 -9 -16 -22 ...
    ##  $ magnet_forearm_y    : num  654 661 658 658 655 660 659 660 653 656 ...
    ##  $ magnet_forearm_z    : num  476 473 469 469 473 478 470 474 476 473 ...
    ##  $ classe              : Factor w/ 5 levels "A","B","C","D",..: 1 1 1 1 1 1 1 1 1 1 ...

The pml dataset contains 19622 observations and 58 variables. this dataset contains no NA nor empty string but still has some promblems such as many numeric or integer variables are read to be factor so I again do a pre-processing to handle this.

Splitting pml Data
------------------

Since the pml data contains a quite large observations, I don't do cross-validation but splitting the pml data into training set and test set instead. the training set will be used to build the model and the test set will be used to estimate out of sample error.

``` r
library(caret)
```

    ## Warning: package 'caret' was built under R version 3.5.3

    ## Loading required package: lattice

    ## Loading required package: ggplot2

``` r
set.seed(2504)
inTrain <- createDataPartition(pml$classe, p = 0.8, list = F)
training <- pml[inTrain,]
dim(training)
```

    ## [1] 15699    58

``` r
test <- pml[-inTrain,]
dim(test)
```

    ## [1] 3923   58

The training set contains 15699 observations which is 80% of total observations of pml dataset, and the rest is in the test set.

Feature Selection
-----------------

The training set contains 57 features, using all these features to build the model can led to overfitting and also not efficient. To select which feature to include in the model, I used Information Gain from FSelector package to see how those features important in predicting the classe variable.

``` r
library(FSelector)
```

    ## Warning: package 'FSelector' was built under R version 3.5.3

``` r
ig <- information.gain(classe ~ ., data = training)
ig
```

    ##                      attr_importance
    ## user_name               0.0065176951
    ## raw_timestamp_part_1    1.5736703146
    ## raw_timestamp_part_2    0.0000000000
    ## new_window              0.0000678631
    ## num_window              1.5877635328
    ## roll_belt               0.4567399477
    ## pitch_belt              0.4646213363
    ## yaw_belt                0.6109567554
    ## total_accel_belt        0.1633331524
    ## gyros_belt_x            0.0835006210
    ## gyros_belt_y            0.1218031743
    ## gyros_belt_z            0.1846627492
    ## accel_belt_x            0.1213616339
    ## accel_belt_y            0.1339988512
    ## accel_belt_z            0.2523396590
    ## magnet_belt_x           0.1285712472
    ## magnet_belt_y           0.2086064842
    ## magnet_belt_z           0.2264946216
    ## roll_arm                0.1419673123
    ## pitch_arm               0.0745720085
    ## yaw_arm                 0.1011459592
    ## total_accel_arm         0.0755472302
    ## gyros_arm_x             0.1357235014
    ## gyros_arm_y             0.1025454660
    ## gyros_arm_z             0.0381252696
    ## accel_arm_x             0.1155400387
    ## accel_arm_y             0.1141963963
    ## accel_arm_z             0.0956764055
    ## magnet_arm_x            0.1281064571
    ## magnet_arm_y            0.1123264368
    ## magnet_arm_z            0.0989802920
    ## roll_dumbbell           0.1938038975
    ## pitch_dumbbell          0.1479557682
    ## yaw_dumbbell            0.1854407598
    ## total_accel_dumbbell    0.1633762214
    ## gyros_dumbbell_x        0.0951082403
    ## gyros_dumbbell_y        0.1740753657
    ## gyros_dumbbell_z        0.0567021543
    ## accel_dumbbell_x        0.1932710018
    ## accel_dumbbell_y        0.1714265442
    ## accel_dumbbell_z        0.2243186541
    ## magnet_dumbbell_x       0.2551873775
    ## magnet_dumbbell_y       0.2908212822
    ## magnet_dumbbell_z       0.2304368864
    ## roll_forearm            0.2718835795
    ## pitch_forearm           0.2033951530
    ## yaw_forearm             0.0810584618
    ## total_accel_forearm     0.0734462505
    ## gyros_forearm_x         0.0395703191
    ## gyros_forearm_y         0.0813192895
    ## gyros_forearm_z         0.0486228714
    ## accel_forearm_x         0.1227644134
    ## accel_forearm_y         0.1017226098
    ## magnet_forearm_x        0.0735010193
    ## magnet_forearm_x.1      0.1252104349
    ## magnet_forearm_y        0.1397385569
    ## magnet_forearm_z        0.0890627865

``` r
sort(ig$attr_importance, decreasing = TRUE)
```

    ##  [1] 1.5877635328 1.5736703146 0.6109567554 0.4646213363 0.4567399477
    ##  [6] 0.2908212822 0.2718835795 0.2551873775 0.2523396590 0.2304368864
    ## [11] 0.2264946216 0.2243186541 0.2086064842 0.2033951530 0.1938038975
    ## [16] 0.1932710018 0.1854407598 0.1846627492 0.1740753657 0.1714265442
    ## [21] 0.1633762214 0.1633331524 0.1479557682 0.1419673123 0.1397385569
    ## [26] 0.1357235014 0.1339988512 0.1285712472 0.1281064571 0.1252104349
    ## [31] 0.1227644134 0.1218031743 0.1213616339 0.1155400387 0.1141963963
    ## [36] 0.1123264368 0.1025454660 0.1017226098 0.1011459592 0.0989802920
    ## [41] 0.0956764055 0.0951082403 0.0890627865 0.0835006210 0.0813192895
    ## [46] 0.0810584618 0.0755472302 0.0745720085 0.0735010193 0.0734462505
    ## [51] 0.0567021543 0.0486228714 0.0395703191 0.0381252696 0.0065176951
    ## [56] 0.0000678631 0.0000000000

attr\_importance measures the dependence between feature and target variable -- classe, the higher the attr\_importance the higher the dependence. here I tried to select just 5 features with the highest attr\_mportance to be included in the model which are raw\_timestamp\_part\_1, num\_window, roll\_belt, pitch\_belt, and yaw\_belt.

A Plot
------

I tried to plot pitch\_belt and yaw\_belt and splitted the color with classe variable, and here it is.

``` r
library(ggplot2)
ggplot(data = training, mapping = aes(yaw_belt, pitch_belt, color = classe)) +
  geom_point()
```

![](Prediction_Assignment_Writeup_files/figure-markdown_github/unnamed-chunk-4-1.png)

``` r
ggplot(data = training, mapping = aes(yaw_belt, pitch_belt, color = classe)) +
  geom_point() +
  geom_smooth()
```

    ## `geom_smooth()` using method = 'gam' and formula 'y ~ s(x, bs = "cs")'

![](Prediction_Assignment_Writeup_files/figure-markdown_github/unnamed-chunk-4-2.png)

Building the Model
------------------

In this part, I try Random Forest algorithm to train the data. I use randomForest() function from randomForest package instead of train() function with method = 'rf'from caret package since train() function takes a quite long time and hard to compute (in my computer) the model.

``` r
library(randomForest)
```

    ## Warning: package 'randomForest' was built under R version 3.5.3

    ## randomForest 4.6-14

    ## Type rfNews() to see new features/changes/bug fixes.

    ## 
    ## Attaching package: 'randomForest'

    ## The following object is masked from 'package:ggplot2':
    ## 
    ##     margin

``` r
mod <- randomForest(classe ~ raw_timestamp_part_1 + num_window + roll_belt + pitch_belt + yaw_belt, data = training)
mod
```

    ## 
    ## Call:
    ##  randomForest(formula = classe ~ raw_timestamp_part_1 + num_window +      roll_belt + pitch_belt + yaw_belt, data = training) 
    ##                Type of random forest: classification
    ##                      Number of trees: 500
    ## No. of variables tried at each split: 2
    ## 
    ##         OOB estimate of  error rate: 0.02%
    ## Confusion matrix:
    ##      A    B    C    D    E  class.error
    ## A 4464    0    0    0    0 0.0000000000
    ## B    0 3038    0    0    0 0.0000000000
    ## C    0    1 2737    0    0 0.0003652301
    ## D    0    0    0 2572    1 0.0003886514
    ## E    0    0    0    1 2885 0.0003465003

``` r
predt <- predict(mod, test)
cm <- table(predt, test$classe)
cm #confusion matrix of test set
```

    ##      
    ## predt    A    B    C    D    E
    ##     A 1116    1    0    0    0
    ##     B    0  758    0    0    0
    ##     C    0    0  684    0    0
    ##     D    0    0    0  643    0
    ##     E    0    0    0    0  721

``` r
accuracy <- sum(diag(cm))/sum(cm)
accuracy
```

    ## [1] 0.9997451

``` r
error <- 1 - accuracy
error
```

    ## [1] 0.000254907

It can be seen that the model is doing pretty well with the train data, with just 0.02% in sample error. but since in sample error is quite optimistic, so I try to apply the model into test set and find that the estimated out of sample error is also 0.02%.So I will just use this model to predict the pml-testing.csv.

Predicting pml-testing.csv
--------------------------

With Random Forest model before, I try to predict 20 observations from pml-testing.csv, and here is the result.

``` r
testing <- read.csv("pml-testing.csv")
predict(mod, testing)
```

    ##  1  2  3  4  5  6  7  8  9 10 11 12 13 14 15 16 17 18 19 20 
    ##  B  A  B  A  A  E  D  B  A  A  B  C  B  A  E  E  A  B  B  B 
    ## Levels: A B C D E
