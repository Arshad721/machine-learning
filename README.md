# machine-learning

readLines(con = "C:/Users/Arshad Hussain/Desktop/r machine learning/German-Credit_1.csv", n=5)
df_1 <- read.table(file = "German-Credit_1.csv",header = T,sep = ",")
head(df_1)
df_1 <- read.csv(file = "German-Credit_1.csv", header = T)
head(df_1)
# install.packages("XLConnect")
library(XLConnect)
df_2 <- read.csv(file = "German-Credit_2.csv",header = T)
df_3 <- merge(x=df_1,y=df_2,by="OBS",all = T)
head(df_3)
colnames(df_3)
summary(df_3)
summary(df_1)
str(df_3)
colnames(df_3)
num_attr <- c("DURATION", "AMOUNT" ,"INSTALL_RATE","AGE" ,"NUM_DEPENDENTS" ,"NUM_CREDITS")
cat_attr <- setdiff(x=colnames(df_3),y=num_attr)
df_3$OBS <- as.character(df_3$OBS)
df_3$RESPONSE <- as.factor(as.character(df_3$RESPONSE))
df_cat <- subset(df_3,select = cat_attr)
colnames(df_cat)
df_3[,cat_attr] <- data.frame(apply(df_cat,2,function(x) as.factor(as.character(x))))
summary(df_3[cat_attr])
str(df_3)
colSums(is.na(df_3))
sum(is.na(df_3))
dim(df_3)
## deleting missing values
df_4 <- na.omit(df_3)
dim(df_4)
summary(df_4)

## substituting missing values

# install.packages("DMwR")
library(DMwR)
manyNAs(df_3,0.1)
df_3_imputed <- centralImputation(data=df_3)
sum(is.na(df_3_imputed))
df_3_imputed1 <- knnImputation(data = df_3,k = 5)
sum(is.na(df_3_imputed1))
df_3_imputed2 <- centralImputation(data=df_1)
sum(is.na(df_3_imputed2))
df_1_imputed <- knnImputation(data=df_1)
sum(is.na(df_1_imputed))
str(df_1)
str(df_3)

## binning/discretizing the Variable
# install.packages("infotheo")
library(infotheo)
amtbin <- discretize(df_3_imputed$AMOUNT,disc = "equalfreq",nbins = 4)
table(amtbin)
amtbin <- discretize(df_3_imputed$AMOUNT,disc = "equalwidth",nbins = 4)
table(amtbin)

## Dummy variable
# install.packages("dummies")
library(dummies)
df_cat <- subset(df_3_imputed,select = cat_attr)
head(df_cat)
str(df_cat)
df_cat_dummy <- data.frame(apply(df_cat,2,function(x) dummy(x)))
head(df_cat_dummy)
str(df_cat_dummy)
dim(df_cat_dummy)
