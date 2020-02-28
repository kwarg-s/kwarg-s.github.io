---
title : "스파크 (3) : PySpark 설치"
excerpt : "리눅스에서 PySpark 설치하기"
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

## **아파치 스파크 (복습)**

<center><img src="../assets/img/spark/spark.png" style="width:300px;"></center>

아파치 스파크는 오픈소스 분산 쿼리 및 처리 엔진입니다. 빠르고, 실시간으로 processing(처리)을 가능하게 하는 프레임워크죠. 스파크가 등장하게 된 배경은, 기존의 아파치 하둡의 맵리듀스 프레임워크가 배치 프로세싱만 지원하였으며, 실시간 처리에 취약하였기 때문입니다. 스파크는 이러한 한계를 극복합니다.

스파크는 저장소와 처리 모두에서 하둡을 지원합니다. 저장소로 HDFS를 사용할 수도 있으며, 하둡의 YARN 위에서 스파크 애플리케이션을 실행할 수도 있습니다. 

## **PySpark가 뭔가요?**

아파치 스파크는 ``Scala``라는 언어로 작성되었습니다. 하지만 우리가 스파크를 사용하기 위해서 Scala를 반드시 배워야하는 건 아닙니다. Pyspark는 스파크가 Python 언어를 지원하기 위해서 배포된 툴입니다. Pyspark를 쓰면, python 언어를 통해서 rdd를 다룰 수 있게 됩니다. 

PySpark은 PySpark Shell을 제공합니다. 이 쉘은 python api를 spark의 코어에 연결시켜서 spark context를 초기화합니다. spark context가 무엇인지는 있다가 설명할 것입니다. 

## **하둡 설치 (선택, 리눅스 운영체제)**

하둡은 스파크와 연동해서 파일 관리 및 리소스 스케줄링을 도와줍니다. <a href="http://spark.apache.org/downloads.html">이곳</a>에서 다운로드 받을 수 있습니다만 터미널을 통해 설치 가능합니다.
        
```bash
$wget http://mirror.navercorp.com/apache/hadoop/common/hadoop-2.7.7/hadoop-2.7.7.tar.gz 
$tar xvzf hadoop-2.7.7.tar.gz; sudo mv hadoop-2.7.7 /opt/hadoop
```
 PATH 환경변수를 설정합니다. 우분투라면 `nano ~/.bashrc`를 열어 가장 마지막에 다음을 추가합니다.

```bash
$export PATH=$PATH:/opt/hadoop/bin
```


## **스파크 설치하기 (리눅스 운영체제)**

운영체제에 다음과 같은 프로그래밍 언어가 설치되어 있어야합니다. 
- Python
- Java
- Scala

**스파크 다운받기**
<a href="http://spark.apache.org/downloads.html">아파치 스파크 다운로드</a>에서 스파크 설치 파일을 다운 받아야 합니다. 

**spark-X.X.X-bin-hadoopX.X.tgz**를 로컬 디렉토리에 다운을 받습니다. 저는 Spark release 2.4.5 버전, Pre-built for Apache Hadoop 2.7을 다운 받았습니다. 다음 코드를 통해 압축을 해체 합니다.

```bash
$tar -xvf 경로/spark-X.X.X-bin-hadoopX.X.tgz
```

**환경변수(path) 설정하기**
Spark path와 Py4j path를 설정하여야 합니다. Py4j는 rdd 처리를 할 수 있게하는 파이썬 라이브러리입니다.

```bash
$export SPARK_HOME = 경로/spark-X.X.X-bin-hadoopX.X
$export PATH = $PATH:경로/spark-X.X.X-bin-hadoopX.X/bin
$export PYTHONPATH = $SPARK_HOME/python:$SPARK_HOME/python/lib/py4j-0.10.4-src.zip:$PYTHONPATH
$export PATH = $SPARK_HOME/python:$PATH 
```

## **PySpark shell 실행하기**

환경변수 설정이 제대로 되었다면 터미널에서 pyspark 실행이 가능할 것입니다. 

```bash
$pyspark
```
간단한 실습하기
```python
>>> file=sc.textFile('README.md')
>>> words=file.flatMap(lambda line:line.split(" "))
>>> result=words.countByValue()
>>> result['For']
```
> 3

## Jupyter notebook 에서 Pyspark 실행하기

pyspark 라이브러리를 python에 설치합니다.

```bash
$pip3 install pyspark
```

Jupyter notebook에서 코드 실행

```python
from pyspark.sql import SparkSession
spark=SparkSession.builder\
    .master('local')\
    .appName('Word Count')\
    .getOrCreate()
spark
```
>SparkSession - in-memory  
>  
>SparkContext  
>  
>Spark UI
>  
>Version  
>v2.2.1  
>Master  
>local  
>AppName  
>Word Count  

## 클라우드 환경에서 pyspark 사용하기

여태까지 실행한 spark는 로컬 환경에서 한대의 서버로만 spark를 실행한 것입니다. 여러 대의 서버를 연결한 클라우드 환경 구성하면 spark의 분산 컴퓨팅 기능을 제대로 사용할 수 있습니다. 저는 연구실에 준비된 여러 대의 서버를 가지고 구축한 개발 환경에서 분석 작업을 수행할 것입니다만, 로컬 환경에서도 분석 작업을 수행할 수는 있습니다. 개발 환경을 구체적으로 어떻게 구성하는지는 나중에 포스팅을 하도록 하겠습니다


## Reference
- <a href="#"> 파이썬을 활용한 스파크 프로그래밍 - 제프리 에이븐 </a>
- https://www.tutorialspoint.com/pyspark/pyspark_broadcast_and_accumulator.htm
