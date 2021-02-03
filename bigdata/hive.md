# Hive

## 3.1.2

### Install

```bash
wget https://mirrors.tuna.tsinghua.edu.cn/apache/hive/hive-3.1.2/apache-hive-3.1.2-bin.tar.gz
```

```bash
sudo mkdir -p /opt/bd
sudo tar -C /opt/bd -xzf apache-hive-3.1.2-bin.tar.gz
sudo ln -snf /opt/bd/apache-hive-3.1.2-bin/ /opt/bd/hive
# for metastore
sudo mkdir -p /opt/bd/tmp/hive
```

### Env

```bash
echo "export HIVE_HOME=\"/opt/bd/hive\"" | sudo tee /etc/profile.d/hive.sh
echo "PATH=\"\${HIVE_HOME}/bin:\${PATH}\"" | sudo tee -a /etc/profile.d/hive.sh
```

### Config

```bash
vi /opt/bd/hive/conf/hive-site.xml
```

> ```xml
> <configuration>
>     <property>
>         <name>javax.jdo.option.ConnectionURL</name>
>         <value>jdbc:derby:;databaseName=/opt/bd/tmp/hive/metastore_db;create=true</value>
>     </property>
> </configuration>
> ```

### Run

Fix guava version mismatch between Hadoop and Hive

```bash
rm ${HIVE_HOME}/lib/guava-19.0.jar
cp ${HADOOP_HOME}/share/hadoop/common/lib/guava-27.0-jre.jar ${HIVE_HOME}/lib
```

Create dirs in hadoop

```bash
hadoop fs -mkdir /tmp
hadoop fs -mkdir -p /user/hive/warehouse
```

```bash
schematool -dbType derby -initSchema
```

```bash
hiveserver2
```

### Usage

```bash
beeline
```

In beeline

```
!connect jdbc:hive2://localhost:10000
!close
!quit
```
