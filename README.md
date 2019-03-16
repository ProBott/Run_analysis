# Run_analysis

##Creating a new file
if(!file.exists("./data")){dir.create("./data")}

##Download folder in data
fileUrl<-"https://d396qusza40orc.cloudfront.net/getdata%2Fprojectfiles%2FUCI%20HAR%20Dataset.zip"
download.file(fileUrl,destfile="./data/Dataset.zip")

##Unzip folder
unzip(zipfile="/.data/Dataset.zip",exdir="./data")

##List files 
pathdata<-file.path("./data","UCI HAR Dataset")
files<-list.files(pathdata,recursive=TRUE)

##Reading Activity, Subject and Features files
xtrain<-read.table(file.path(pathdata,"train","X_train.txt" ),header=FALSE)
ytrain<-read.table(file.path(pathdata,"train","y_train.txt"),header=FALSE)
subject_train<-read.table(file.path(pathdata,"train","subject_train.txt"),header=FALSE)

xtest<-read.table(file.path(pathdata,"test","X_test.txt" ),header=FALSE)
ytest<-read.table(file.path(pathdata,"test","y_test.txt"),header=FALSE)
subject_test<-read.table(file.path(pathdata,"test","subject_test.txt"),header=FALSE)

features<-read.table(file.path(pathdata,"features.txt"),header=FALSE)

activityLabels<-read.table(file.path(pathdata,"activity_labels.txt"),header=FALSE)


##Creating column values
colnames(xtrain)<-features[,2]
colnames(ytrain)<-"activityId"
colnames(subject_train)<-"subjectId"

colnames(xtest)<-features[,2]
colnames(ytest)<-"activityId"
colnames(subject_test)<-"subjectId"

colnames(activityLabels)<-c('activityId','activityType')


##Merging all train and test data
merge_train<-cbind(ytrain,subject_train,xtrain)
merge_test<-cbind(ytest,subject_test,xtest)

setAllInOne<-rbind(mrg_train,mrg_test)


##Reading all values on mean and standard deviation
colNames<-colnames(setAllInOne)

mean_and_std<-(grepl("activityId",colNames)|grepl("subjectId",colNames)|grepl("mean..",colNames)|grepl("std..",colNames))

setForMeanAndStd<-setAllInOne[,mean_and_std==TRUE]

setWithActivityNames<-merge(setForMeanAndStd,activityLabels,by='activityId',all.x=TRUE)


##Creating new tidy data
FinalTidySet<-aggregate(.~subjectId+activityId,setWithActivityNames,mean)
FinalTidySet<-secTidySet[order(secTidySet$subjectId,secTidySet$activityId),]

##Saving
write.table(secTidySet,"FinalTidySet.txt",row.names = FALSE)

