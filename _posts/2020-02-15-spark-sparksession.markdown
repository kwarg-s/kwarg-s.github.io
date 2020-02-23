---
title : "스파크(4) : SparkSession"
category :
  - spark
tag :
  - me
  - diary
  - subinium
use_math : true
author_profile : true
header:
  teaser : /assets/images/category/data.jpg
  overlay_image : /assets/images/category/data.jpg
  overlay_filter: 0.1
---

### **SparkContext? SparkSession?**

우리는 앞으로 ``SparkContext``, ``SparkSession``를 많이 볼 것입니다. SparkContext와 SparkSession은 모두 spark의 ``entry point``라고 합니다. **entry point** 란 ``콘트롤이 운영체제로부터 제공된 프로그램으로 전환되는 곳``입니다. spark 2.0 버전의 이전까지는 spark-core에 대한 entry-point가 ``sparkContext``였습니다. 

**SparkContext**  
스파크 드라이버하는 일 중 중요한 것은 SparkContext를 생성하는 일입니다. SparkContext라는 인스턴스는 스파크 애플리케이션으로 하여금 리소스 매니저(예.YARN)의 도움을 받아 스파크 클러스터에 접속하게 합니다. SparkContext는 모든 spark 함수에 접속하는 채널로 사용되었습니다.  SparkContext는 다음과 같은 다양한 함수들을 call할 수 있습니다.

- 스파크 애플리케이션의 현재 상태를 얻어오는 함수  
- configuration을 하는 함수
- 다양한 서비스에 접속하는 함수  
- job 또는 stage를 취소하는 함수 
- persisent RDD에 접속하는 함수 등

<img src='../assets/img/spark/scandss.jpg'>


**SparkSession**  
RDD는 context의 API를 이용해서 생성되었습니다. 그런데 context는 사용하는 맥락에 따라 모두 다른 context를 사용해야 했습니다. 예를 들어, RDD를 생성하려면 SparkContext를, SQL을 위해서는 sqlContext를 사용해야 했으며, hive를 위해서는 hiveContext를 사용해야 했습니다. 여러가지를 사용하려면 각각의 인스턴스를 모두 생성해줘야했습니다. 

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

### **PySpark에서 SparkSession 시작하기**

PySpark에서 SparkSession 클래스는 pyspark.sql 모듈에 있습니다. SparkSession은 보통 spark라는 변수 이름으로 지정합니다.

```python
from pyspark.sql import SparkSession
spark=SparkSession.builder.\
    .master('local')\
    .appName('Word Count')\
    .getOrCreate()
```

### **SparkSession에서도 SparkContext 함수를 사용할 수 있습니다.** 

SparkSession 인스턴스의 property(자식 인스턴스)에는 sparkContext가 있어서, 기존의 sparkContext에서 call할 수 있는 함수를 spark.sparkContext를 통해 사용할 수 있습니다. sparkContext 인스턴스는 sc라는 변수 이름으로 지정합니다.

```python
sc=spark.sparkContext
```







### Reference
- <a href="#"> 파이썬을 활용한 스파크 프로그래밍 - 제프리 에이븐 </a>
- https://blog.knoldus.com/spark-why-should-we-use-sparksession/ 
- https://www.quora.com/What-is-the-difference-between-spark-context-and-spark-session 
- https://dzone.com/articles/introduction-to-spark-session 
