Getting-and-Cleaning-Data
=========================



The purpose of this project is to demonstrate ability to collect, work with, and clean a data set. 
The goal is to prepare tidy data that can be used for later analysis. 
Following items are required to submit: 
 1) a tidy data set as described below, 
 2) a link to a Github repository with your script for performing the analysis, 
 3) a code book that describes the variables, the data, and any transformations or work that you performed to clean up the data called CodeBook.md. 
 4) You should also include a README.md in the repo with your scripts. 
 This repo explains how all of the scripts work and how they are connected.  

 project related details about the data set is given here   
  http://archive.ics.uci.edu/ml/datasets/Human+Activity+Recognition+Using+Smartphones 

 Here are the data for the project: 
 https://d396qusza40orc.cloudfront.net/getdata%2Fprojectfiles%2FUCI%20HAR%20Dataset.zip 

Data Set Information:

The experiments have been carried out with a group of 30 volunteers within an age bracket of 19-48 years. 
Each person performed six activities (WALKING, WALKING_UPSTAIRS, WALKING_DOWNSTAIRS, SITTING, STANDING, LAYING) 
wearing a smartphone (Samsung Galaxy S II) on the waist. Using its embedded accelerometer and gyroscope, 
we captured 3-axial linear acceleration and 3-axial angular velocity at a constant rate of 50Hz. 
The experiments have been video-recorded to label the data manually. 
The obtained dataset has been randomly partitioned into two sets, where 70% of the volunteers was selected for generating 
the training data and 30% the test data. 

The sensor signals (accelerometer and gyroscope) were pre-processed by applying noise filters and then sampled in 
fixed-width sliding windows of 2.56 sec and 50% overlap (128 readings/window). The sensor acceleration signal, 
which has gravitational and body motion components, was separated using a Butterworth low-pass filter into body 
acceleration and gravity. The gravitational force is assumed to have only low frequency components, 
therefore a filter with 0.3 Hz cutoff frequency was used. From each window, a vector of features was obtained by 
calculating variables from the time and frequency domain. 


Information about the scripts:


Load data.table library
  library(data.table) 
  
Load data

  fileUrl1 <- "https://d396qusza40orc.cloudfront.net/getdata%2Fprojectfiles%2FUCI%20HAR%20Dataset.zip" 
  download.file(fileUrl1, destfile = "Dataset.zip") 
  unzip("Dataset.zip") 
  x_testData <- read.table("./UCI HAR Dataset/test/X_test.txt",header=FALSE) 
  y_testdata <- read.table("./UCI HAR Dataset/test/y_test.txt",header=FALSE) 
  subject_testdata <- read.table("./UCI HAR Dataset/test/subject_test.txt",header=FALSE) 
  x_trainData <- read.table("./UCI HAR Dataset/train/X_train.txt",header=FALSE) 
  y_traindata <- read.table("./UCI HAR Dataset/train/y_train.txt",header=FALSE) 
  subject_traindata <- read.table("./UCI HAR Dataset/train/subject_train.txt",header=FALSE) 

Uses descriptive activity names to name the activities in the data set

  activities <- read.table("./UCI HAR Dataset/activity_labels.txt",header=FALSE,colClasses="character") 
  y_testdata$V1 <- factor(y_testdata$V1,levels=activities$V1,labels=activities$V2) 
  y_traindata$V1 <- factor(y_traindata$V1,levels=activities$V1,labels=activities$V2) 

Appropriately labels the data set with descriptive variable names. 

  features <- read.table("./UCI HAR Dataset/features.txt",header=FALSE,colClasses="character") 
  colnames(x_testData)<-features$V2 
  colnames(x_trainData)<-features$V2 
  colnames(y_testdata)<-c("Act") 
  colnames(y_traindata)<-c("Act") 
  colnames(subject_testdata)<-c("Sub") 
  colnames(subject_traindata)<-c("Sub") 

Merges the training and the test sets to create one data set.

  testData<-cbind(x_testData,y_testdata) 
  testData<-cbind(testData,subject_testdata) 
  trainData<-cbind(x_trainData,y_traindata) 
  trainData<-cbind(trainData,subject_traindata) 
  finalData<-rbind(testData,trainData) 

Extracts only the measurements on the mean and standard deviation for each measurement. 

  finalData_mean<-sapply(finalData,mean,na.rm=TRUE) 
  finalData_sd<-sapply(finalData,sd,na.rm=TRUE) 

From the data set in step 4, creates a second, independent tidy data set with the average of each variable for each activity and each subject.

  finalData2<- data.table(finalData) 
  tidyset<-finalData2[,lapply(.SD,mean),by="Act,Sub"] 
  write.table(tidyset,file="tidy.txt",sep=",",row.names = FALSE)





















