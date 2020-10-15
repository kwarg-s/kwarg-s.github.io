---
title : "스파크세션 (SparkSession)"
excerpt : "SparkContext와 SparkSession"
category :
  - spark
tag :
  - spark
use_math : true
author_profile : true
header:
  teaser : /assets/images/category/data.jpg
  overlay_image : /assets/images/category/data.jpg
  overlay_filter: 0.1
---

## **SparkContext? SparkSession?**

우리는 앞으로 ``SparkContext``, ``SparkSession``를 많이 볼 것입니다. SparkContext와 SparkSession은 모두 spark의 ``entry point``라고 합니다. **entry point** 란 ``콘트롤이 운영체제로부터 제공된 프로그램으로 전환되는 곳``입니다. spark 2.0 버전의 이전까지는 spark-core에 대한 entry-point가 ``sparkContext``였습니다. 

## **SparkContext**  

SparkContext는 애플리케이션과 클러스터 사이의 연결을 관리하는 객체입니다. 모든 스파크 애플리케이션은 반드시 SparkContext를 생성해야합니다. 공식 문서에 따르면 SparkContext는 스파크의 시작점, 또는 스파크 클러스터와의 연결을 나타냅니다. 

스파크 드라이버가 하는 일 중 중요한 것은 SparkContext를 생성하는 일입니다. SparkContext라는 인스턴스는 스파크 애플리케이션으로 하여금 리소스 매니저(예.YARN)의 도움을 받아 스파크 클러스터에 접속하게 합니다. SparkContext가 준비되면, 애플리케이션에 필요한 여러 서비스가 준비됩니다. SparkContext는 모든 spark 함수에 접속하는 채널로 사용할 수 있습니다. SparkContext는 다음과 같은 다양한 함수들을 call할 수 있습니다.

- 스파크 애플리케이션의 현재 상태를 얻어오는 함수  
- configuration을 하는 함수
- 다양한 서비스에 접속하는 함수  
- job 또는 stage를 취소하는 함수 
- persisent RDD에 접속하는 함수 등

실제 함수를 사용하는 방법은 다음 포스트에서 다룰 것입니다.


## **SparkSession**  


<img src='../assets/img/spark/scandss.jpg' style="width:80%;">

spark의 데이터 구조는 context의 API를 이용해서 생성되었습니다. 그런데 context는 사용하는 맥락에 따라 모두 다른 context를 사용해야 했습니다. 예를 들어, RDD를 생성하려면 SparkContext를, SQL을 위해서는 sqlContext를 사용해야 했으며, hive를 위해서는 hiveContext를 사용해야 했습니다. 여러가지를 사용하려면 각각의 인스턴스를 모두 생성해줘야했습니다. 

그런데 sql, hive, streaming API 외에도, dataframe과 dataset API의 사용 필요성이 높아졌습니다. 이를 지원해줄 새로운 인스턴스가 필요해졌는데요. Spark 2.0 버전 이후 부터는 ``SparkSession``을 사용하면 오직 하나의 entry-point만 가지고 Spark 함수를 사용할 수 있습니다. 이제 sql, hive, streaming api를 사용하기 위해 context를 따로 따로 생성해줄 필요가 없어집니다. 즉, ``SparkSession``은 SQLcontext, Hivecontext, Streamingcontext 등이 모두 결합된 것입니다. 

- 기존의 SparkContext 예(scala 언어)
```scala
val conf = new SparkConf().set.Master(master).setAppName(appName)
val sc = new SparkContext(conf)
val sql Context = new SparkContext (conf)
val sqlContext = new org.apache.spark.sql.SQLContext(sc)
val hiveContext = new org.apache.spark.sql.hive.HiveContext(sc)
```

- SparkSession 사용 예(scala 언어)
```scala
val spark = SparkSession.builder().master(master).appName(appName).getOrCreate()
```

## **PySpark Shell에서 SparkContext, SparkSession 사용하기**

PySpark Shell에서는 SparkContext와 SparkSession이 이미 생성되어 있습니다. SparkContext는 sc 변수로, SparkSession은 spark 변수로 불러올 수 있습니다. 

```
Welcome to
      ____              __
     / __/__  ___ _____/ /__
    _\ \/ _ \/ _ `/ __/  '_/
   /__ / .__/\_,_/_/ /_/\_\   version 2.2.1
      /_/

Using Python version 3.6.9 (default, Nov  7 2019 10:44:02)
SparkSession available as 'spark'.
>>> sc
<SparkContext master=local[*] appName=PySparkShell>
>>> spark
<pyspark.sql.session.SparkSession object at 0x7f6cd52540f0>
```


## **Python 프로그램에서 SparkSession 생성하기**

Shell이 아닌 일반적인 python 프로그램에서는 SparkContext 또는 SparkSession을 직접 생성해주셔야 합니다. 현재 버전에서는 SparkSession을 생성하면 SparkContext는 그것의 자식 인스턴스로 자동으로 생성됩니다. PySpark에서 SparkSession 클래스는 pyspark.sql 모듈에 있습니다. 새로운 .py 파일 또는 jupyter notebook에서 다음 코드를 실행하면 SparkSession을 생성할 수 있습니다. 

```python
from pyspark.sql import SparkSession
spark=SparkSession.builder\
    .master('local')\
    .appName('Word Count')\
    .getOrCreate()
```

SparkSession 인스턴스의 property(자식 인스턴스)에는 sparkContext가 있어서, 기존의 sparkContext에서 call할 수 있는 함수를 spark.sparkContext를 통해 사용할 수 있습니다. sparkContext 인스턴스는 sc라는 변수 이름으로 지정합니다.

```python
sc=spark.sparkContext
```

## Reference
- <a href="#"> 파이썬을 활용한 스파크 프로그래밍 - 제프리 에이븐 </a>
- https://blog.knoldus.com/spark-why-should-we-use-sparksession/ 
- https://www.quora.com/What-is-the-difference-between-spark-context-and-spark-session 
- https://dzone.com/articles/introduction-to-spark-session 
