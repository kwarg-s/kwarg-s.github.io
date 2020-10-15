---
title : "Pyspark RDD 프로그래밍 기초"
excerpt : "Pyspark, RDD, 생성, 액션, 변환"
category :
  - spark
tag :
  - spark
  - kitkit 
  - educational game
use_math : true
author_profile : true
header:
  teaser : /assets/images/category/data.jpg
  overlay_image : /assets/images/category/data.jpg
  overlay_filter: 0.1
---

## Spark Context에서 시작

Spark context에서 시작해서 RDD 연산을 시작해보겠습니다. 우선 Spark context를 시작합니다. 

```python
from pyspark.sql import SparkSession
spark=SparkSession.builder\
    .master('local')\
    .appName('My App Name')\
    .getOrCreate()
sc=spark.sparkContext
```

RDD로는 무엇을 할 수 있을까요? 우선 RDD를 사용하기 위해서는 `RDD를 생성`해야합니다. 생성된 RDD로는 `변환(transformation)`을 하거나 `액션(action)`을 취합니다. 변환의 수행결과는 RDD이고, 액션의 수행결과는 RDD가 아닙니다.

## RDD 생성하기

RDD를 생성하는 방법은 크게 2가지가 있습니다.

1. 컬렉션 개체를 사용하는 방법: 파이썬, 자바에서는 리스트 타입 ; 스칼라에서는 시퀀스 타입의 객체를 통해 RDD를 생성할 수 있습니다. 
    - parallelize()
2. 외부 데이터를 사용하는 방법: 기존에 저장되어 있는 외부 데이터를 불러와서 RDD를 생성할 수 있습니다.
    - textFile()

### .parallelize()

```python
newrdd=sc.parallelize([0,1,2,3,4])
```

### .textFile()

```python
#spark.txt
'''
I am a girl.
'''
rdd=sc.textFile("spark.txt")
rdd.collect()#['I am a girl.']
```

### .pickleFile()

```python
rdd=sc.pickleFile('picklefile')
```

RDD를 파일로 저장하려면 rdd.saveAs를 붙여주면 됩니다.

- `rdd.saveAsTextFile("...")`
- `rdd.saveAsPickleFile("...")`

## 기본적인 RDD 액션(action)

RDD 액션(action)은 새로운 RDD를 반환하는 함수들입니다. 

### .collect()

RDD의 모든 원소를 배열로 리턴해주는 함수입니다. 

```python
rdd=sc.parallelize([1,2,3])
rdd.collect() #[1,2,3]
```

### .count()

RDD를 구성하는 요소의 개수를 반환해주는 함수입니다.

```python
rdd=sc.parallelize([1,2,3])
rdd.count() #3
```

## 기본적인 RDD 변환(transformation)

RDD 변환(transformation)은 새로운 RDD를 반환하는 함수들입니다. 

### .map()

```python
rdd=sc.textFile("spark.txt")
rdd=rdd.map(lambda x:x.upper())
rdd.collect()#['I AM A GIRL.']
```

### .flatMap()

```python
rdd=sc.textFile("spark.txt")
rdd=rdd.flatMap(lambda x:x.split(" "))
rdd.collect()#['I', 'am', 'a', 'girl.']
```

### .filter()

```python
rdd=sc.parallelize([0,1,2,3,4,5])
rdd=rdd.filter(lambda x:x%2==0)
rdd.collect()#[0, 2, 4]
```

### .distinct()

```python
rdd=sc.parallelize([0,0,0,0,1,1,1,1,2,2,2,2,3,3])
rdd=rdd.distinct()
rdd.collect()#[0, 1, 2, 3]
```

### .groupBy()

groupBy(함수)를 하면 함수가 리턴하는 값을 중심으로 RDD 요소들을 그룹핑합니다. 결과는 (그룹, resultiterable)형태로 나오는데요, resultiterable 는 list()등을 씌워주어야 볼 수 있습니다. 

```python
rdd=sc.parallelize(["Apple",'Ant','Bear','Bat','Cat','Corn'])
rdd=rdd.groupBy(lambda x:x[0])
rdd.collect()
'''
[('A', <pyspark.resultiterable.ResultIterable at 0x7f08f5031b00>),
 ('B', <pyspark.resultiterable.ResultIterable at 0x7f08f27270f0>),
 ('C', <pyspark.resultiterable.ResultIterable at 0x7f08f27275c0>)]
'''
showrdd=rdd.map(lambda x:(x[0],list(x[1])))
showrdd.collect()
#[('A', ['Apple', 'Ant']), ('B', ['Bear', 'Bat']), ('C', ['Cat', 'Corn'])]
```

### .sortBy()

```python
rdd=sc.parallelize([(1,"an apple"),(2,"a carrot"),(3, "an ant"),\
                   (4,"a panda"),(5, "a dog"),(6, "a bear")])
rdd=rdd.sortBy(lambda x:x[1].split(" ")[1],ascending=False)
rdd.collect()
'''
[(4, 'a panda'),
 (5, 'a dog'),
 (2, 'a carrot'),
 (6, 'a bear'),
 (1, 'an apple'),
 (3, 'an ant')]
'''
```
## Summary

1. RDD를 생성
    - `parallelize()`
    - `textFile()`
    - `pickleFile()`
2. 기본적 RDD 변환(transformation)
    - `map()`
    - `flatMap()`
    - `filter()`
    - `distinct()`
    - `groupBy()`
    - `sortBy()`
3. 기본적 RDD 액션
    - `collect()`
    - `count()`

## Reference
- 빅데이터 분석을 위한 스파크2 프로그래밍 (백성민)
- 파이썬을 활용한 스파크 프로그래밍 (제프리 에이븐)