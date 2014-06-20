---
title: "Practical Machine Learning Course Project"
output: html_document
---
## Project Summary

Human Activity Recognition (HAR) is a hot area of research focusing on using data to recognize and predict activities. With the development of fitness bands massive amounts of data have become avaiable. The focus of this project is to use data from the weight lifting data sets to determine how "well" the activity was preformed.

The data is available from: http://groupware.les.inf.puc-rio.br

The training set is available here: https://d396qusza40orc.cloudfront.net/predmachlearn/pml-training.csv

The testing set is available here: https://d396qusza40orc.cloudfront.net/predmachlearn/pml-testing.csv

## Analysis

R packages used and loaded

```
## Loading required package: lattice
## Loading required package: ggplot2
## randomForest 4.6-7
## Type rfNews() to see new features/changes/bug fixes.
```

Read in the data sets:

```r
PMLtraining<-read.csv("pml-training.csv")
PMLtesting<-read.csv("pml-testing.csv")
dim(PMLtraining)
```

```
## [1] 19622   160
```

```r
dim(PMLtesting)
```

```
## [1]  20 160
```

Partition PML Training into training and cross-validation set and imput missing values

Generate Model

```r
modelRF  <- randomForest(training$classe ~ ., training)
```
## Calculate In-Sample Accuracy

```r
trainPred <- predict(modelRF, training)
confusionMatrix(trainPred, training$classe)
```

```
## Confusion Matrix and Statistics
## 
##           Reference
## Prediction    A    B    C    D    E
##          A 3906    0    0    0    0
##          B    0 2658    0    0    0
##          C    0    0 2396    0    0
##          D    0    0    0 2252    0
##          E    0    0    0    0 2525
## 
## Overall Statistics
##                                 
##                Accuracy : 1     
##                  95% CI : (1, 1)
##     No Information Rate : 0.284 
##     P-Value [Acc > NIR] : <2e-16
##                                 
##                   Kappa : 1     
##  Mcnemar's Test P-Value : NA    
## 
## Statistics by Class:
## 
##                      Class: A Class: B Class: C Class: D Class: E
## Sensitivity             1.000    1.000    1.000    1.000    1.000
## Specificity             1.000    1.000    1.000    1.000    1.000
## Pos Pred Value          1.000    1.000    1.000    1.000    1.000
## Neg Pred Value          1.000    1.000    1.000    1.000    1.000
## Prevalence              0.284    0.193    0.174    0.164    0.184
## Detection Rate          0.284    0.193    0.174    0.164    0.184
## Detection Prevalence    0.284    0.193    0.174    0.164    0.184
## Balanced Accuracy       1.000    1.000    1.000    1.000    1.000
```

## Calculate Testing Accuracy

```r
testPred <- predict(modelRF, testing)
confusionMatrix(testPred, testing$classe)
```

```
## Confusion Matrix and Statistics
## 
##           Reference
## Prediction    A    B    C    D    E
##          A 1663   18    5    0    3
##          B    3 1105   17    5    2
##          C    7   16  992   17    6
##          D    0    0   12  941    6
##          E    1    0    0    1 1065
## 
## Overall Statistics
##                                         
##                Accuracy : 0.98          
##                  95% CI : (0.976, 0.983)
##     No Information Rate : 0.284         
##     P-Value [Acc > NIR] : <2e-16        
##                                         
##                   Kappa : 0.974         
##  Mcnemar's Test P-Value : NA            
## 
## Statistics by Class:
## 
##                      Class: A Class: B Class: C Class: D Class: E
## Sensitivity             0.993    0.970    0.967    0.976    0.984
## Specificity             0.994    0.994    0.991    0.996    1.000
## Pos Pred Value          0.985    0.976    0.956    0.981    0.998
## Neg Pred Value          0.997    0.993    0.993    0.995    0.996
## Prevalence              0.284    0.194    0.174    0.164    0.184
## Detection Rate          0.283    0.188    0.169    0.160    0.181
## Detection Prevalence    0.287    0.192    0.176    0.163    0.181
## Balanced Accuracy       0.994    0.982    0.979    0.986    0.992
```

## Apply the Random Forest Model to PML Testing

```r
answers <- predict(modelRF, predtest)
answers
```

```
##  1  2  3  4  5  6  7  8  9 10 11 12 13 14 15 16 17 18 19 20 
##  B  A  B  A  A  E  D  B  A  A  B  C  B  A  E  E  A  B  B  B 
## Levels: A B C D E
```

```r
pml_write_files = function(x){
  n = length(x)
  for(i in 1:n){
    filename = paste0("problem_id_",i,".txt")
    write.table(x[i],file=filename,quote=FALSE,row.names=FALSE,col.names=FALSE)
  }
}
pml_write_files(answers)
```

