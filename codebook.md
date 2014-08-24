Codebook
========
Variable list and descriptions
------------------------------

Variable name    | Description
-----------------|------------
subject          | ID the subject who performed the activity for each window sample. Its range is from 1 to 30.
activity         | Activity name
feature          | Feature of the variable
measure          | Variable (Mean or SD)
count            | Count of data points used to compute `average`
average          | Average of each variable for each activity and each subject

Dataset structure
-----------------


```r
str(tidy)
```

```
##Classes ‘data.table’ and 'data.frame':	11880 obs. of  6 ##variables:
## $ subject : int  1 1 1 1 1 1 1 1 1 1 ...
## $ activity: Factor w/ 6 levels "LAYING","SITTING",..: 1 1 1 1 ##1 1 1 1 1 1 ...
## $ feature : chr  "fBodyAcc-X" "fBodyAcc-X" "fBodyAcc-Y" ##"fBodyAcc-Y" ...
## $ measure : Factor w/ 2 levels "Mean","SD": 1 2 1 2 1 2 1 2 1 ##2 ...
## $ count   : int  50 50 50 50 50 50 50 50 50 50 ...
## $ average : num  -0.939 -0.924 -0.867 -0.834 -0.883 ...
## - attr(*, "sorted")= chr  "subject" "activity" "feature" ##"measure"
## - attr(*, ".internal.selfref")=<externalptr> 
```


List the key variables in the data table
----------------------------------------


```r
key(tidy)
```

```
## [1] "subject"  "activity" "feature"  "measure" 
```


Show a few rows of the dataset
------------------------------


```r
tidy
```

```
##  subject         activity        feature measure count     average
##    1:       1           LAYING     fBodyAcc-X    Mean    50 -0.93909905
##    2:       1           LAYING     fBodyAcc-X      SD    50 -0.92443743
##    3:       1           LAYING     fBodyAcc-Y    Mean    50 -0.86706521
##    4:       1           LAYING     fBodyAcc-Y      SD    50 -0.83362556
##    5:       1           LAYING     fBodyAcc-Z    Mean    50 -0.88266688
   ---                                                                  
##11876:      30 WALKING_UPSTAIRS  tGravityAcc-Y      SD    65 -0.91493394
##11877:      30 WALKING_UPSTAIRS  tGravityAcc-Z    Mean    65 -0.02214011
##11878:      30 WALKING_UPSTAIRS  tGravityAcc-Z      SD    65 -0.86240279
##11879:      30 WALKING_UPSTAIRS tGravityAccMag    Mean    65 -0.13762786
##11880:      30 WALKING_UPSTAIRS tGravityAccMag      SD    65 -0.32741082
```


Summary of variables
--------------------


```r
summary(tidy)
```

```
## subject                   activity      feature          measure         count      
## Min.   : 1.0   LAYING            :1980   Length:11880       Mean:5940   Min.   :36.00  
## 1st Qu.: 8.0   SITTING           :1980   Class :character   SD  :5940   1st Qu.:49.00  
## Median :15.5   STANDING          :1980   Mode  :character               Median :54.50  
## Mean   :15.5   WALKING           :1980                                  Mean   :57.22  
## 3rd Qu.:23.0   WALKING_DOWNSTAIRS:1980                                  3rd Qu.:63.25  
## Max.   :30.0   WALKING_UPSTAIRS  :1980                                  Max.   :95.00  
##    average        
## Min.   :-0.99767  
## 1st Qu.:-0.96205  
## Median :-0.46989  
## Mean   :-0.48436  
## 3rd Qu.:-0.07836  
## Max.   : 0.97451 
```


List all possible combinations of features
------------------------------------------


```r
tidy[, .N, by = c("feature", "measure")]
```

```
## feature measure   N
## 1:           fBodyAcc-X    Mean 180
## 2:           fBodyAcc-X      SD 180
## 3:           fBodyAcc-Y    Mean 180
## 4:           fBodyAcc-Y      SD 180
## 5:           fBodyAcc-Z    Mean 180
## 6:           fBodyAcc-Z      SD 180
## 7:       fBodyAccJerk-X    Mean 180
## 8:       fBodyAccJerk-X      SD 180
## 9:       fBodyAccJerk-Y    Mean 180
## 10:       fBodyAccJerk-Y      SD 180
## 11:       fBodyAccJerk-Z    Mean 180
## 12:       fBodyAccJerk-Z      SD 180
## 13:          fBodyAccMag    Mean 180
## 14:          fBodyAccMag      SD 180
## 15:  fBodyBodyAccJerkMag    Mean 180
## 16:  fBodyBodyAccJerkMag      SD 180
## 17: fBodyBodyGyroJerkMag    Mean 180
## 18: fBodyBodyGyroJerkMag      SD 180
## 19:     fBodyBodyGyroMag    Mean 180
## 20:     fBodyBodyGyroMag      SD 180
## 21:          fBodyGyro-X    Mean 180
## 22:          fBodyGyro-X      SD 180
## 23:          fBodyGyro-Y    Mean 180
## 24:          fBodyGyro-Y      SD 180
## 25:          fBodyGyro-Z    Mean 180
## 26:          fBodyGyro-Z      SD 180
## 27:           tBodyAcc-X    Mean 180
## 28:           tBodyAcc-X      SD 180
## 29:           tBodyAcc-Y    Mean 180
## 30:           tBodyAcc-Y      SD 180
## 31:           tBodyAcc-Z    Mean 180
## 32:           tBodyAcc-Z      SD 180
## 33:       tBodyAccJerk-X    Mean 180
## 34:       tBodyAccJerk-X      SD 180
## 35:       tBodyAccJerk-Y    Mean 180
## 36:       tBodyAccJerk-Y      SD 180
## 37:       tBodyAccJerk-Z    Mean 180
## 38:       tBodyAccJerk-Z      SD 180
## 39:      tBodyAccJerkMag    Mean 180
## 40:      tBodyAccJerkMag      SD 180
## 41:          tBodyAccMag    Mean 180
## 42:          tBodyAccMag      SD 180
## 43:          tBodyGyro-X    Mean 180
## 44:          tBodyGyro-X      SD 180
## 45:          tBodyGyro-Y    Mean 180
## 46:          tBodyGyro-Y      SD 180
## 47:          tBodyGyro-Z    Mean 180
## 48:          tBodyGyro-Z      SD 180
## 49:      tBodyGyroJerk-X    Mean 180
## 50:      tBodyGyroJerk-X      SD 180
## 51:      tBodyGyroJerk-Y    Mean 180
## 52:      tBodyGyroJerk-Y      SD 180
## 53:      tBodyGyroJerk-Z    Mean 180
## 54:      tBodyGyroJerk-Z      SD 180
## 55:     tBodyGyroJerkMag    Mean 180
## 56:     tBodyGyroJerkMag      SD 180
## 57:         tBodyGyroMag    Mean 180
## 58:         tBodyGyroMag      SD 180
## 59:        tGravityAcc-X    Mean 180
## 60:        tGravityAcc-X      SD 180
## 61:        tGravityAcc-Y    Mean 180
## 62:        tGravityAcc-Y      SD 180
## 63:        tGravityAcc-Z    Mean 180
## 64:        tGravityAcc-Z      SD 180
## 65:       tGravityAccMag    Mean 180
## 66:       tGravityAccMag      SD 180
## feature measure   N
```