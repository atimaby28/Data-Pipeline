하둡 환경설정(Hadoop Configuration)이 복잡하고 어려워 이틀정도 헤맸던 기억에 기록으로 남깁니다.


# 기본 설정


먼저 아래의 명령어로 업데이트를 해줍니다.

```
sudo apt-get install ssh
```


```
sudo apt-get install pdsh
```

.bashrc를 수정하고 적용해야 하기 때문에 아래의 명령어로 권한을 변경해 줍니다.

```
sudo chmod -R 777 /home/atimaby28/.bashrc
```

![](./image/Hadoop-1.png)


.bashrc에 다음 명령어를 추가해 줍니다.


```
export PDSH_RCMD_TYPE=ssh
```

![](./image/Hadoop-2.png)


```
ssh-keygen -t rsa -P ""
```

입력 후 그냥 Enter를 눌러 계속 진행

```
cat ~/.ssh/id_rsa.pub >> ~/.ssh/authorized_keys
```

![](./image/Hadoop-3.png)



# 자바 설정 - 1.8.0_392

자바를 설치할 차례인데, 호환성을 높이기 위하여 다음 명령어를 통해 자바를 설치합니다.

```
 sudo apt install openjdk-8-jdk
```

![](./image/Hadoop-4.png)


java -version 명령어로 설치 확인 후 /usr/lib/jvm에 있는 java-1.8.0-openjdk-arm64를 mv 명령어를 통해 jdk1.8.0-392로 변경해 줍니다.

그 후, 마찬가지로 /etc/environment에 권한을 주기 위해 다음 명령어를 입력합니다.

```
sudo chmod -R 777 /etc/environment
```
![](./image/Hadoop-5.png)


environment 파일을 다음 명령어를 통해 수정합니다.

```
PATH="/usr/local/sbin:/usr/local/bin:/usr/sbin:/usr/bin:/sbin:/bin:/usr/games:/usr/local/games:/usr/lib/jvm/jdk1.8.0_392/bin:/usr/lib/jvm/jdk1.8.0_392/db/bin:/usr/lib/jvm/jdk1.8.0_392/jre/bin"

J2SDKDIR="/usr/lib/jvm/jdk1.8.0_392"

J2REDIR="/usr/lib/jvm/jdk1.8.0_392/jre”

JAVA_HOME="/usr/lib/jvm/jdk1.8.0_392"

DERBY_HOME="/usr/lib/jvm/jdk1.8.0_392/db"
```

![](./image/Hadoop-6.png)


![](./image/Hadoop-7.png)


다음 명령어를 통해 변경해주고,


```
sudo update-alternatives --install "/usr/bin/java" "java" "/usr/lib/jvm/jdk1.8.0_392/bin/java" 0

sudo update-alternatives --install "/usr/bin/javac" "javac" "/usr/lib/jvm/jdk1.8.0_392/bin/javac" 0

sudo update-alternatives --set java /usr/lib/jvm/jdk1.8.0_392/bin/java

sudo update-alternatives --set javac /usr/lib/jvm/jdk1.8.0_392/bin/javac
```

![](./image/Hadoop-8.png)

다음 명령어를 통해 잘 변경되었는지 확인합니다.

```
update-alternatives --list java
update-alternatives --list javac
java -version
```


# 하둡 설정

하둡 설치 역시 호환성을 위해 wget 명령어를 통해 Hadoop 설치 후 압축을 풀어줍니다.

```
wget https://downloads.apache.org/hadoop/common/hadoop-3.6.6/hadoop-3.6.6.tar.gz 
```

![](./image/Hadoop-9.png)

다음 코드를 추가하여 통해 환경설정을 수정해 줍니다.

```
vim /hadoop/etc/hadoop/hadoop-env.sh

```
```
export JAVA_HOME=/usr/lib/jvm/jdk1.8.0_271
```
![](./image/Hadoop-10.png)

다음 코드를 추가하여 통해 환경설정을 수정해 줍니다.

```
vim /hadoop/etc/hadoop/core-site.xml

```

```
<configuration>
    <property>
        <name>fs.defaultFS</name>
        <value>hdfs://localhost:9000</value>
    </property>
    <property>
        <name>hadoop.tmp.dir</name>
        <value>/home/atimaby28/hdata</value> 
        <!-- please specify a location where you have Read Write privileges -->
    </property>
</configuration>
```
![](./image/Hadoop-11.png)


다음 명령어를 통해 파일을 생성해 주고,

```
mkdir -p /home/atimaby28/hadoop/hdfs/namenode
```
```
mkdir -p /home/atimaby28/hadoop/hdfs/datanode
```

![](./image/Hadoop-12.png)

다음 코드를 추가하여 통해 환경설정을 수정해 줍니다.

```
vim /hadoop/etc/hadoop/hdfs-site.xml
```


```
<configuration>
    <property>
        <name>dfs.replication</name>
        <value>1</value>
    </property>
    <property>
        <name>dfs.name.dir</name>
        <value>file:///home/atimaby28/hadoop/hdfs/namenode</value>
    </property>
    <property>
        <name>dfs.data.dir</name>
        <value>file:///home/atimaby28/hadoop/hdfs/datanode</value>
    </property>
</configuration>
```
![](./image/Hadoop-13.png)

다음 코드를 추가하여 통해 환경설정을 수정해 줍니다.

```
vim /hadoop/etc/hadoop/mapred-site.xml

```

```
<configuration>
    <property>
        <name>mapreduce.framework.name</name>
        <value>yarn</value>
    </property>
    <property>
        <name>yarn.app.mapreduce.am.env</name>
        <value>HADOOP_MAPRED_HOME=/home/atimaby28/hadoop</value>
    </property>
    <property>  
        <name>mapreduce.map.env</name>
        <value>HADOOP_MAPRED_HOME=/home/atimaby28/hadoop</value>
    </property>
    <property>
        <name>mapreduce.reduce.env</name>
        <value>HADOOP_MAPRED_HOME=/home/atimaby28/hadoop</value>
    </property>
</configuration>
```
![](./image/Hadoop-14.png)

다음 코드를 추가하여 통해 환경설정을 수정해 줍니다.

```
vim /hadoop/etc/hadoop/yarn-site.xml
```

```
<configuration>
    <property>
        <name>yarn.nodemanager.aux-services</name>
        <value>mapreduce_shuffle</value>
    </property>
    <property>
        <name>yarn.nodemanager.aux-services.mapreduce.shuffle.class</name>
        <value>org.apache.hadoop.mapred.ShuffleHandler</value>
    </property>
</configuration>
```

![](./image/Hadoop-15.png)

다음 코드를 추가하여 통해 환경설정을 수정해 줍니다.

```
vim /hadoop/etc/hadoop/yarn-site.xml
```


```
export HADOOP_HOME="/home/atimaby28/hadoop"

export PATH=$PATH:$HADOOP_HOME/bin

export PATH=$PATH:$HADOOP_HOME/sbin

export HADOOP_MAPRED_HOME=${HADOOP_HOME}

export HADOOP_COMMON_HOME=${HADOOP_HOME}

export HADOOP_HDFS_HOME=${HADOOP_HOME}

export YARN_HOME=${HADOOP_HOME}
```

![](./image/Hadoop-16.png)

```
‘bin/hdfs namenode -format’ or sbin/start-dfs.sh
```

![](./image/Hadoop-17.png)
![](./image/Hadoop-18.png)

```
sbin/start-yarn.sh
```
![](./image/Hadoop-19.png)

![](./image/Hadoop-20.png)
![](./image/Hadoop-21.png)
![](./image/Hadoop-22.png)