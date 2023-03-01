# Install Hadoop on Master Node

## Hadoop Tarball Install
- Version : hadoop 3.3.3 binary
- Download Path : https://archive.apache.org/dist/hadoop/common/
```bash
$ cd /{base_dir}
$ wget https://archive.apache.org/dist/hadoop/common/hadoop-3.3.3/hadoop-3.3.3.tar.gz
$ tar xvfz hadoop-3.3.3.tar.gz
$ mv hadoop-3.3.3 hadoop3
```

<br/>

## Hadoop Start를 위한 JAVA_HOME 설정
```bash
$ vi /{base_dir}/hadoop3/etc/hadoop/hadoop-env.sh
```
```text
export JAVA_HOME=/{base_dir}/jdk8
```

<br/>

## Default File System 설정 - core-site.xml
```bash
$ vi /{base_dir}/hadoop3/etc/hadoop/core-site.xml
```
```text
<configuration>
    <property>
        <name>fs.defaultFS</name>
        <value>hdfs://{Master Host Name}:9000/</value>
    </property>
</configuration>
```

<br/>

## HDFS [NameNode, DataNode, Secondary NameNode] 설정 - hdfs-site.xml
```bash
$ vi /{base_dir}/hadoop3/etc/hadoop/hdfs-site.xml
```
```text
<configuration>
    <property>
        <name>dfs.namenode.name.dir</name>
        <value>file:///{base_dir}/hadoop3/dfs/name</value>
    </property>
    <property>
        <name>dfs.datanode.data.dir</name>
        <value>file:///{base_dir}/hadoop3/dfs/data</value>
    </property>
    <property>
        <name>dfs.namenode.checkpoint.dir</name>
        <value>file:///{base_dir}/hadoop3/dfs/namesecondary</value>
    </property>
</configuration>
```
- dfs.namenode.name.dir
    - HDFS의 마스터 역할을 하는 NameNode에 실질적으로 HDFS에서 관리하는 메타데이터를 저장하는 로컬 디렉토리 설정
- dfs.datanode.data.dir
    - HDFS에서 실제 데이터가 저장되는 위치 설정
    - Worker Server들의 로컬 디렉토리 설정
- dfs.namenode.checkpoint.dir
    - HDFS의 메타데이터에 대한 내용을 메모리에 가지고 있는데, 주기적으로 디스크에 저장할 디렉토리 지정 (Secondary NameNode)

<br/>

## YARN [Resource Manager] 설정 - yarn-site.xml
```bash
$ vi /{base_dir}/hadoop3/etc/hadoop/yarn-site.xml
```
```text
<configuration>
    <property>
        <name>yarn.resourcemanager.hostname</name>
        <value>{Master Host Name}</value>
    </property>
    <property>
        <name>yarn.resourcemanager.webapp.address</name>
        <value>{Master Host Name}:8188</value>
    </property>
</configuration>
```
- cluster의 resource manager에 대한 host name 설정

<br/>

## workers 설정 - workers
```bash
$ vi /{base_dir}/hadoop3/etc/hadoop/workers
```
```text
{Worker1 Host Name}
{Worker2 Host Name}
{Worker3 Host Name}
```
- worker process를 띄울 대상을 찾도록 설정

<br/>

## Master Node Server의 'jdk8', 'hadoop3' 파일들을 Worker Node Server에 복사
- Master Node Server의 'jdk8'과 'hadoop3'에 기본적으로 설정한 conf 파일을 포함하여 Worker Node Server에서 동작할 수 있도록 Copy
```bash
# jdk8
$ scp -r /{base_dir}/jdk8 {OS User Name}@{Worker 1 Host Name}:/{base_dir}/
$ scp -r /{base_dir}/jdk8 {OS User Name}@{Worker 2 Host Name}:/{base_dir}/
$ scp -r /{base_dir}/jdk8 {OS User Name}@{Worker 3 Host Name}:/{base_dir}/

# hadoop3
$ scp -r /{base_dir}/hadoop3 {OS User Name}@{Worker 1 Host Name}:/{base_dir}/
$ scp -r /{base_dir}/hadoop3 {OS User Name}@{Worker 2 Host Name}:/{base_dir}/
$ scp -r /{base_dir}/hadoop3 {OS User Name}@{Worker 3 Host Name}:/{base_dir}/
```

<br/>

## JPS 명령어에 대한 PATH 등록
- Master Node Server에서 HDFS, YARN, Spark Application 등 프로세스를 실행하면, 이와 함께 Worker Node Server들에서 동시에 실행되는 프로세스를 각 Worker Node Server에서 확인할 수 있도록 jps 명령어에 대한 PATH 등록
```bash
$ scp -r ~/.profile {OS User Name}@{Worker 1 Host Name}:~/
$ scp -r ~/.profile {OS User Name}@{Worker 2 Host Name}:~/
$ scp -r ~/.profile {OS User Name}@{Worker 3 Host Name}:~/
```