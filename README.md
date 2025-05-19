Firda Rosela Sundari

2200198

19/05/2025

Pendahuluan
Tujuan dari proyek ini adalah untuk mengukur seberapa baik peserta melakukan latihan mengangkat barbel dan untuk mengklasifikasikan data yang dibaca dari akselerometer ke dalam 5 kelas yang berbeda (Kelas A sampai Kelas E).

Silakan merujuk ke tautan di bawah ini untuk sumber data:

http://web.archive.org/web/20161224072740/http:/groupware.les.inf.puc-rio.br/har
https://d396qusza40orc.cloudfront.net/predmachlearn/pml-training.csv
https://d396qusza40orc.cloudfront.net/predmachlearn/pml-testing.csv
Install packages
i used google colab to train and test, so we have to install the packages R first before we use the library


[48]
1m
install.packages("caret", repos = "http://cran.us.r-project.org")
install.packages("randomForest", repos = "http://cran.us.r-project.org")
install.packages("ggplot2", repos = "http://cran.us.r-project.org")
install.packages("dplyr", repos = "http://cran.us.r-project.org")
Installing package into ‘/usr/local/lib/R/site-library’
(as ‘lib’ is unspecified)

Installing package into ‘/usr/local/lib/R/site-library’
(as ‘lib’ is unspecified)

Installing package into ‘/usr/local/lib/R/site-library’
(as ‘lib’ is unspecified)

Installing package into ‘/usr/local/lib/R/site-library’
(as ‘lib’ is unspecified)

Import Library

[49]
0s
library(caret)
library(randomForest)
library(dplyr)
library(ggplot2)
library(e1071)
Install dan muat datasets

[50]
0s
download.file("https://d396qusza40orc.cloudfront.net/predmachlearn/pml-training.csv", destfile = "pml-training.csv")
download.file("https://d396qusza40orc.cloudfront.net/predmachlearn/pml-testing.csv", destfile = "pml-testing.csv")

[51]
0s
training_raw <- read.csv("/content/pml-training.csv", na.strings=c("NA", "", "#DIV/0!"))
testing_raw <- read.csv("/content/pml-testing.csv", na.strings=c("NA", "", "#DIV/0!"))
Double-click (or enter) to edit

Pembersihan data

[54]
# Hapus kolom dengan banyak NA
training <- training_raw[, colSums(is.na(training_raw)) == 0]

# Hapus kolom non-predictor (user info, timestamps, dll)
training <- training[, -(1:7)]

# Buat partisi training dan validasi
set.seed(123)
inTrain <- createDataPartition(training$classe, p=0.7, list=FALSE)
trainData <- training[inTrain, ]
validData <- training[-inTrain, ]
Pembuatan 3 model berbeda

[21]
2m
# random forest
set.seed(100)
model_rf <- train(classe ~ ., data=trainData, method="rf", trControl=trainControl(method="cv", number=5), ntree=100)


[25]
# Model LDA
set.seed(200)
model_lda <- train(
  classe ~ ., 
  data = trainData, 
  method = "lda",
  trControl = trainControl(method = "cv", number = 5)
)

[42]
# Model Decision Tree

set.seed(300)
model_rpart <- train(
  classe ~ ., 
  data = trainData, 
  method = "rpart",
  trControl = trainControl(method = "cv", number = 5)
)
Conclusion
pada percobaan tiga model ini, confusion matrix dan statistics tidak bisa ditampilkan. oleh karena itu, hasil belum bisa disimpulkan mana yang paling bagus diantara 3 model ini


[56]
0s
# Samakan kolom dengan training
test <- testing_raw[, names(testing_raw) %in% names(trainData)]

# Prediksi
pred_final <- predict(model_rf, newdata=test)

# Tampilkan hasil
pred_final


[57]
0s
# Samakan kolom dengan training
test <- testing_raw[, names(testing_raw) %in% names(trainData)]

# Prediksi
pred_final <- predict(model_rpart, newdata=test)

# Tampilkan hasil
pred_final


[63]
0s
# Samakan kolom dengan training
test <- testing_raw[, names(testing_raw) %in% names(trainData)]

# Prediksi
pred_final <- predict(model_lda, newdata=test)

# Tampilkan hasil
pred_final

