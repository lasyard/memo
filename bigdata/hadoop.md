# hadoop

## 3.2.1

```bash
export ver="3.2.1"
```

### Install

```bash
wget https://mirrors.tuna.tsinghua.edu.cn/apache/hadoop/common/hadoop-${ver}/hadoop-${ver}.tar.gz
```

```bash
sudo mkdir -p /opt/bd
sudo tar -C /opt/bd -xzvf hadoop-${ver}.tar.gz
sudo ln -snf /opt/bd/hadoop-${ver}/ /opt/bd/hadoop
# for ${hadoop.tmp.dir}
sudo mkdir -p /opt/bd/tmp/hadoop
```

### Env

#### CentOS 7.3

```bash
echo "export HADOOP_HOME=\"/opt/bd/hadoop\"" | sudo tee /etc/profile.d/hadoop.sh
# flink will use this.
echo "export HADOOP_CLASSPATH=\"\$(\${HADOOP_HOME}/bin/hadoop classpath)\"" | sudo tee -a /etc/profile.d/hadoop.sh
echo "PATH=\"\${HADOOP_HOME}/bin:\${HADOOP_HOME}/sbin:\${PATH}\"" | sudo tee -a /etc/profile.d/hadoop.sh
```

### Config

```bash
sudo vi /opt/bd/hadoop/etc/hadoop/hadoop-env.sh
```

> ```bash
> export JAVA_HOME=/usr
> # User privilege
> export HDFS_NAMENODE_USER=root
> export HDFS_DATANODE_USER=root
> export HDFS_SECONDARYNAMENODE_USER=root
> export YARN_RESOURCEMANAGER_USER=root
> export YARN_NODEMANAGER_USER=root
> ```

```bash
sudo vi /opt/bd/hadoop/etc/hadoop/core-site.xml
```

> ```xml
> <configuration>
>     <property>
>         <name>fs.defaultFS</name>
>         <value>hdfs://las1:9000</value>
>     </property>
>     <property>
>         <name>hadoop.tmp.dir</name>
>         <value>/opt/bd/tmp/hadoop</value>
>     </property>
>     <property>
>         <name>hadoop.proxyuser.root.hosts</name>
>         <value>*</value>
>     </property>
>     <property>
>         <name>hadoop.proxyuser.root.groups</name>
>         <value>*</value>
>     </property>
> </configuration>
> ```

```bash
sudo vi /opt/bd/hadoop/etc/hadoop/hdfs-site.xml
```

> ```xml
> <configuration>
>     <property>
>         <name>dfs.replication</name>
>         <value>1</value>
>     </property>
>     <property>
>         <name>dfs.blocksize</name>
>         <value>262144</value>
>     </property>
>     <property>
>         <name>dfs.namenode.fs-limits.min-block-size</name>
>         <value>262144</value>
>     </property>
>     <property>
>         <name>dfs.namenode.handler.count</name>
>         <value>10</value>
>     </property>
> </configuration>
> ```

```bash
sudo vi /opt/bd/hadoop/etc/hadoop/workers
```

> ```txt
> las1
> las2
> las3
> ```

```bash
sudo vi /opt/bd/hadoop/etc/hadoop/mapred-site.xml
```

> ```xml
> <configuration>
>     <property>
>         <name>mapreduce.framework.name</name>
>         <value>yarn</value>
>     </property>
>     <property>
>         <name>mapreduce.application.classpath</name>
>         <value>/opt/bd/hadoop/share/hadoop/mapreduce/*:/opt/bd/hadoop/share/hadoop/mapreduce/lib/*</value>
>     </property>
> </configuration>
> ```

```bash
sudo vi /opt/bd/hadoop/etc/hadoop/yarn-site.xml
```

> ```xml
> <configuration>
>     <property>
>         <name>yarn.resourcemanager.hostname</name>
>         <value>las1</value>
>     </property>
>     <property>
>         <name>yarn.nodemanager.aux-services</name>
>         <value>mapreduce_shuffle</value>
>     </property>
>     <property>
>         <name>yarn.nodemanager.resource.detect-hardware-capabilities</name>
>         <value>true</value>
>     </property>
>     <property>
>         <name>yarn.scheduler.maximum-allocation-vcores</name>
>         <value>8</value>
>     </property>
>     <property>
>         <name>yarn.resourcemanager.ha.enabled</name>
>         <value>false</value>
>     </property>
>     <property>
>         <name>yarn.webapp.ui2.enable</name>
>         <value>true</value>
>     </property>
> </configuration>
> ```

```bash
for host in las2 las3; do
    for file in /opt/bd/hadoop/etc/hadoop/{hadoop-env.sh,core-site.xml,hdfs-site.xml,mapred-site.xml,yarn-site.xml}; do
        scp "${file}" "${host}:${file}"
    done
done
```

### Run

```bash
hdfs namenode -format
```

#### hdfs

```bash
start-dfs.sh
stop-fds.sh
```

#### yarn

```bash
start-yarn.sh
stop-yarn.sh
```

```bash
yarn jar word-count-1.0.0-SNAPSHOT.jar
```

### Usage

#### hdfs

```bash
hdfs dfs -ls /
hdfs dfs -mkdir -p /user/root
hdfs dfs -put file.dat
hdfs dfs -cat /user/root/file.dat
```

```bash
curl http://las1:9870/
```

#### clean hdfs data

```bash
for host in "las1" "las2" "las3"; do
    ssh "${host}" "rm -rf /opt/bd/tmp/hadoop/dfs/*"
done
```

#### yarn

```bash
curl http://las1:8088/
curl http://las1:8088/ui2/
```

## 2.10.1

```bash
export ver="2.10.1"
```

### Config

```bash
sudo vi /opt/bd/hadoop/etc/hadoop/slaves
```

> ```txt
> las1
> las2
> las3
> ```

### Usage

#### hdfs

```bash
curl http://las1:50070/
```
