---
title : "[CheatSheet] R 회귀분석"
excerpt : "R로 회귀분석하기"
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

## 선형회귀분석

패키지 읽어오기
```r
#mass의 Boston 데이터
library(MASS)
Boston
names(Boston) #컬럼 이름 
#ISLR: 본 교재에서 쓰는 데이터
library(ISLR)
```

단순선형회귀모델에 데이터 fit하기
```r
lm.fit=lm(medv~lstat,data=Boston) #lm(dv~iv,dataframe)
```
또는
```r
attach(Boston)
lm.fit=lm(medv~lstat)
```
모든 x에 대해 fitting하기
```r
lm.fit=lm(medv~.data=Boston)
```

결과 열람하기
```r
lm.fit # equation y=b0+b1x
summary(lm.fit) # tvalue, pvalue, residuals....
names(lm.fit) # coefficients, residuals, effects....
coef(lm.fit) # intercept, lstat.
confint(lm.fit) # 계수 추정치에 대한 신뢰구간
```

Plotting하기
```r
plot(lstat,medv,col="red",pch=15) #pch:그래프 표시를 다르게 함. ￣
plot(lstat,medv,col="red",pch="+")￣
abline(lm.fit,lwd=3,col="green") #abline(fit된 모델):회귀선을 그려줌 
```

다중선형회귀모델
```r
lm.fit=lm(medv~lstat+age,data=Boston)
summary(lm.fit)
lm.fit=lm(medv~.,data=Boston) #전체 X에 대해
summary(lm.fit)
```
상호작용항
```r
summary(lm(medv~lstat*age,data=Boston))
```

비선형변환 (x^2 변수 생성)
```r
lm.fit2=lm(medv~lstat+I(lstat^2))
summary(lm.fit2)
```

더미변수생성
```r
lm.fit=lm(Sales~.,data=Carseats)
summary(lm.fit)
```
결과:저절로 더미변수 생성됩니다.
```ShelveLocGood    4.8501827  0.1531100  31.678  < 2e-16 ***
ShelveLocMedium  1.9567148  0.1261056  15.516  < 2e-16 ***
```

## Reference
- -