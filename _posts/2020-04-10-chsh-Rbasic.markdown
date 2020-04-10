---
title : "[CheatSheet] R 기본"
excerpt : "R코드의 기본"
category :
  - etc
tag :
  - etc
use_math : true
author_profile : true
header:
  teaser : /assets/images/main.jpg
  overlay_image : /assets/images/main.jpg
  overlay_filter: 0.1
---

## vectors
```r
x1<-c(2,4,6)
x2<-2:6
x3<-seq(2,3,by=0.5)
x4<-rep(1:2,times=3,each=2)
x1
x2
x3
x4
```

## vector functions
```r
sort(x4) #sort
rev(x4)
table(x4) #see counts of values
unique(x4) #remove duplicates
``` 
## selecting element
```r
x<-rep(seq(1,20),times=3)
x[4]
x[-4] #제외
x[2:4]
x[-(2:4)] #제외
x[x==10]
x[x<10]
x[x %in% c(1,2,5)]
x['apple']
```

## loop
```r
for (i in 1:4){
  j<-i+10
  print(j)
}
i=0
while (i<5){
  print(i)
  i<-i+1
}
if(i>3){
  print('yes')
}else{
  print('no')
}
```
## function  
```r
f1<-function(x){
  s<-x*x
  return(s)
}
```

## conditions
```r
a<-3
b<-4
a==b
a!=b
a>b
a<b
is.na(a) #missing
is.null(b) #null

x<-c(1,2,3)
y<-c(2,3,4)
plot(x,y)
```

## converting types
```r
x<-c(1,2,3)
as.character(x)
as.logical(x)
as.numeric(x)
as.factor(x)
```

## math
```r
log(x)
exp(x)
sum(x)
mean(x)
max(x)
min(x)
median(x)
quantile(x)
round(1.4444,2)
signif(12340,3)
rank(c(50,10,100))
var(x)
cor(c(1,2,3),c(4,5,6))
sd(x)
```

## matrix
```r
m<-matrix(c(1,2,3,4,5,6),nrow=2,ncol=3)
n<-matrix(c(1,2,3,4,5,6),nrow=3,ncol=2)
m[2,] #row
m[,1] #col
m[2,3]
t(m) #transpose
m*n
m%*%n
```

## Indexing matrix
```r
A=matrix(1:16,4,4)
A[2,3]
A[c(1,3),c(2,4)]
A[1:3,2:4]
A[1:2,]
A[,1:2]
A[1,]
A[-c(1,3),]
A[-c(1,3),-c(1,3,4)]
dim(A)
```

## list
```r
l<-list(x=1:5,y=c('a','b'))
l #first element=x , second element=y
l[1] #1
l[[1]] #first element
l$x
l[['x']]
```

## dataframe
```r
class<-c(100,100,100,200,200,200)
numbers<-c(1,1,2,2,3,3)
df<-data.frame(class,numbers)
df<-data.frame(x=1:3,y=c('a','b','c'))
df$x
df[[1]]
df[2,] #두번쨰 row
df[,2] #두번째 column
df[2,2]
names(df) #column names
dim(df) #dimension
nrow(df) # of row
ncol(df) # of col
cbind(df) #?
df=na.omit(df) #na를 제거한다. 
attach(df) #데이터 프레임 내의 변수들을 그 이름으로 사용할 수 있게함
```
## read&write
```r
df<-read.table('/media/kmz/Data/codes/R/m1-ch2.R')
write.table(df,'/media/kmz/Data/codes/R/write_table.txt')
df<-read.csv('/media/kmz/Data/codes/R/businfo.csv')
write.csv(df,'/media/kmz/Data/codes/R/write_csv.csv')
Auto=read.table("/media/kmz/Data/codes/R/Auto.data",header=T,na.strings="?")
Auto=read.csv("/media/kmz/Data/codes/R/Auto.csv",header=T,na.strings="?")
fix(Auto) #show in window
dim(Auto) #dimension
Auto[1:4,] 
Auto[1:4,1:4]
Auto[,1:4]
Auto=na.omit(Auto) #na를 제거한다. 
dim(Auto)
names(Auto)
``` 

## Scatter plot
```r
df<-data.frame(x=c(1,2,3),y=c(30,20,10))
plot(df$x,df$y)
plot(cylinders, mpg) #원래 있던 데이터
class<-c(100,100,100,200,200,200)
numbers<-c(1,1,2,2,3,3)
df<-data.frame(class,numbers)
plot(class,numbers)#plot(xaxis,yaxis)
```
## Box plot
```r
class_f<-as.factor(class)
plot(class_f,numbers)
plot(class_f,numbers,col='red',varwidth=T,xlab='class',ylab='numbers')
plot(class_f,numbers,horizontal=T,col='red',varwidth=T,xlab='class',ylab='numbers')
```
## Histogram
```r
hist(Auto$cylinders)
hist(Auto$cylinders,col=2) #color=2='red'
hist(Auto$cylinders,col=2,breaks=15)
pairs(Auto)
pairs(~ mpg + displacement + horsepower + weight + acceleration, Auto)
plot(horsepower,mpg)
identify(horsepower,mpg,name)
summary(Auto)
summary(mpg)
```

## string
```r
s<-"He is a happy dog."
x<-c("X1", "X2")
y<-c("Y1", "Y2")
paste(x,y,sep=" ")
paste(x, collapse=" ")
toupper("he is a happy dog")
tolower(c("My","Dog"))
nchar("maltese")
factor(c(1,2,3,4,5))
cut(c(1,2,3,4,5,6),breaks=4)
```

## 함수의 작성
```r
MyFunction=function(){
  library(MASS)
}
```

## linear
```r
df<-data.frame(x=1:5,y=c(15,25,35,45,55))
model<-lm(y~x,data=df)
#generalized linear
model<-glm(y~x,data=df)
#info
summary(model)
```  

## t-test
```r
t.test(df$x,df$y)
pairwise.t.test(df$x,df$y)
```



## Reference
- -