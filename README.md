======================================
Used data:

- Triaxial acceleration from the accelerometer (total acceleration) and the estimated body acceleration.
- Triaxial Angular velocity from the gyroscope. 
- A 561-feature vector with time and frequency domain variables. 
- Its activity label. 
- An identifier of the subject who carried out the experiment.

The submision data includes the following files:
=========================================

- 'README.md'

- 'Codebook.md'

- 'tidy_data.txt'

- 'run_analysis.R'

=========================================
R script
=========================================

setwd("D:/Work/Coursera/Data Analysis/Getting data/getdata")

features<-read.table("features.txt", header = F)
activity_labels<-read.table("activity_labels.txt", header = F, col.names= c("activity","activity_label"))

train<-read.table("X_train.txt", header = TRUE, col.names=features$V2)
test<-read.table("X_test.txt", header = TRUE, col.names=features$V2)

ytrain<-read.table("y_train.txt", header = TRUE, col.names="activity")
ytest<-read.table("y_test.txt", header = TRUE, col.names= "activity")

subjecttrain<-read.table("subject_train.txt", header = TRUE, col.names="subject")
subjecttest<-read.table("subject_test.txt", header = TRUE, col.names="subject")

f<-rbind(train, test)
subject<-rbind(subjecttrain, subjecttest)
a<-rbind(ytrain, ytest)

m<-merge(a,activity_labels, by="activity")

df<-cbind(f, subject, m$activity_label)

nm <- colnames(df)
data1<-subset(df, select=grepl("^(.*?)mean()|std()(.*?)", nm))

data<-cbind(data1, subject, m)

tidy_data<-aggregate(data1, list(activity_label=data$activity_label,subject=data$subject), mean)

write.table(tidy_data, "tidy_data.txt", row.name=FALSE)


