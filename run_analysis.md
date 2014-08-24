
# Preliminaries
## Load packages
```{r }
packages <- c("data.table", "reshape2")
sapply(packages, require, character.only = TRUE, quietly = TRUE)
```
## Set the file path
```{r }
pathIn <- file.path("UCI HAR Dataset")
```
# Read the files
## Read the subject files.

```{r }
dtSubjectTrain <- read.table(file.path(pathIn, "train", "subject_train.txt"))
dtSubjectTest  <- read.table(file.path(pathIn, "test" , "subject_test.txt" ))
```
## Read the activity files.
```{r }
dtActivityTrain <- read.table(file.path(pathIn, "train", "Y_train.txt"))
dtActivityTest  <- read.table(file.path(pathIn, "test" , "Y_test.txt" ))
```
## Read main data
```{r }
dtTrain <- read.table(file.path(pathIn, "train", "X_train.txt"))
dtTest  <- read.table(file.path(pathIn, "test" , "X_test.txt" ))
```
## (Item 1) Merges the training and the test sets to create one data set.
```{r }
dtSubject <- rbind(dtSubjectTrain, dtSubjectTest)
names(dtSubject) <- c("subject")

dtActivity <- rbind(dtActivityTrain, dtActivityTest)
names(dtActivity) <- c("activityNumber")

dt <- rbind(dtTrain, dtTest)
```
## Merge the columns to complete a data set
complete <- cbind(dtSubject, dtActivity, dt)

## sort the data
```{r }
attach(complete)
complete <- complete[order(subject, activityNumber),]
detach(complete)
```

## (Item 2) Extracts only the measurements on the mean and standard deviation
```{r }
dtFeatures <- read.table(file.path(pathIn, "features.txt"))
names(dtFeatures) <- c("featureNumber", "featureName")


dtFeatures <- dtFeatures[grepl("mean\\(\\)|std\\(\\)", dtFeatures$featureName), ]
dtFeatures$featureCode <- paste0("V", dtFeatures$featureNum)

complete <- complete[, c("subject", "activityNumber", dtFeatures$featureCode)]
```
## (Item 3) Uses descriptive activity names to name the activities in the data set
```{r }
dtActivityNames <- read.table(file.path(pathIn, "activity_labels.txt"))
names(dtActivityNames) <- c("activityNumber", "activityName")
```
# Merge activity labels 
```{r }
complete <- merge (dtActivityNames, complete, by="activityNumber")

reshape <- data.table(melt(complete, c("subject", "activityNumber", "activityName"), variable_name = "featureCode"))

reshape <- merge(reshape, dtFeatures, by = intersect(names(reshape), names(dtFeatures)))
```
## (Item 4) Appropriately labels the data set with descriptive variable names.
## Create new equivalent column as factor for better readable names
```{r }
reshape$activity <- factor(reshape$activityName)
reshape$feature <- factor(reshape$featureName)
```
# separate the measure STD and MEAN from feature field
```{r }
y <- matrix(seq(1, 2), nrow=2)
x <- matrix(c(grepl("mean", reshape$feature), grepl("std", reshape$feature)), ncol=nrow(y))
reshape$measure <- factor(x %*% y, labels=c("Mean", "SD"))
```
# remove -mean() and -std() from feature field
```{r }
reshape$feature <- gsub("-mean\\(\\)|-std\\(\\)", "",reshape$feature)
```
## (Item 5) Creates a second, independent tidy data set with the average 
##of each variable for each activity and each subject.
```{r }
setkey(reshape, subject, activity, feature, measure)
tidy <- reshape[, list(count = .N, average = mean(value)), by=key(reshape)]
write.csv(tidy, file = "tidydata.txt", quote=FALSE, row.names=FALSE)
```