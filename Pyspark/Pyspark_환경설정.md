# 기본 설정


먼저 아래의 명령어로 업데이트를 해줍니다.

```
sudo apt-get update
```

![](./image/Pyspark-1.png)

다음 명령어들로 필요한 라이브러리를 설치합니다.

```
sudo apt install python3-pip
```
![](./image/Pyspark-2.png)
```
pip3 install jupyter
```
![](./image/Pyspark-3.png)
```
pip3 install findspark
```
```
pip3 install ipykernel
```
![](./image/Pyspark-4.png)
```
pip3 install pyspark
```
```
pip3 install pandas
```
![](./image/Pyspark-5.png)

다음 명령어로 스파크 설치와 압축 해제를 진행합니다.
```
wget https://downloads.apache.org/spark/spark-3.5.0/spark-3.5.0-bin-hadoop3.tgz
```
![](./image/Pyspark-6.png)

```
tar zxf spark-3.5.0-bin-hadoop3.tgz
```
![](./image/Pyspark-7.png)

마지막으로 주피터 노트북을 설치하여 실행합니다.

```
sudo apt install jupyter-notebook
```
![](./image/Pyspark-8.png)

저는 따로 파일을 만들어 그 곳을 루트로 시작하였습니다.

![](./image/Pyspark-9.png)

![](./image/Pyspark-10.png)
