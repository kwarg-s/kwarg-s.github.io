---
title : "스파크 (3) : PySpark 설치"
subtitle : "리눅스에서 PySpark 설치하기"
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

### **아파치 스파크 (복습)**

<center><img src="../assets/img/spark/spark.png" style="width:300px;"></center>

아파치 스파크는 오픈소스 분산 쿼리 및 처리 엔진입니다. 빠르고, 실시간으로 processing(처리)을 가능하게 하는 프레임워크죠. 스파크가 등장하게 된 배경은, 기존의 아파치 하둡의 맵리듀스 프레임워크가 배치 프로세싱만 지원하였으며, 실시간 처리에 취약하였기 때문입니다. 스파크는 이러한 한계를 극복합니다.

스파크는 저장소와 처리 모두에서 하둡을 지원합니다. 저장소로 HDFS를 사용할 수도 있으며, 하둡의 YARN 위에서 스파크 애플리케이션을 실행할 수도 있습니다. 

### **PySpark가 뭔가요?**

아파치 스파크는 ``Scala``라는 언어로 작성되었습니다. 하지만 우리가 스파크를 사용하기 위해서 Scala를 반드시 배워야하는 건 아닙니다. Pyspark는 스파크가 Python 언어를 지원하기 위해서 배포된 툴입니다. Pyspark를 쓰면, python 언어를 통해서 rdd를 다룰 수 있게 됩니다. 

PySpark은 PySpark Shell을 제공합니다. 이 쉘은 python api를 spark의 코어에 연결시켜서 spark context를 초기화합니다. spark context가 무엇인지는 있다가 설명할 것입니다. 

### **스파크 설치하기 (리눅스 운영체제)**

운영체제에 다음과 같은 프로그래밍 언어가 설치되어 있어야합니다. 
- Python
- Java
- Scala

**스파크 다운받기**
<a href="#">아파치 스파크 다운로드</a>에서 최신 스파크 버전을 다운 받아야 합니다. **spark-X.X.X-bin-hadoopX.X.tgz**를 로컬 디렉토리에 다운을 받습니다. 그리고 다음 코드를 통해 압축을 해체 합니다.

```bash
$tar -xvf 경로/spark-X.X.X-bin-hadoopX.X.tgz
```

**환경변수(path) 설정하기**
다음으로 Spark path와 Py4j path를 설정하여야 합니다. Py4j는 rdd 처리를 할 수 있게하는 파이썬 라이브러리입니다.

```bash
$export SPARK_HOME = /home/hadoop/spark-X.X.X-bin-hadoopX.X
$export PATH = $PATH:/home/hadoop/spark-X.X.X-bin-hadoopX.X/bin
$export PYTHONPATH = $SPARK_HOME/python:$SPARK_HOME/python/lib/py4j-0.10.4-src.zip:$PYTHONPATH
$export PATH = $SPARK_HOME/python:$PATH 
```

### **PySpark shell 실행하기**

pyspark shell을 실행하는 파일은 bin경로에 있습니다. 

```bash
$./bin/pyspark
```





### Reference
- <a href="#"> 파이썬을 활용한 스파크 프로그래밍 - 제프리 에이븐 </a>
- https://www.tutorialspoint.com/pyspark/pyspark_broadcast_and_accumulator.htm
