# flink

## 1.12.0/1.11.2

```bash
export ver="1.12.0"
export ver="1.11.2"
```

### Install

```bash
wget https://mirrors.tuna.tsinghua.edu.cn/apache/flink/flink-${ver}/flink-${ver}-bin-scala_2.12.tgz
```

```bash
sudo mkdir -p /opt/bd
sudo tar -C /opt/bd -xzvf flink-${ver}-bin-scala_2.12.tgz
sudo ln -snf /opt/bd/flink-${ver}/ /opt/bd/flink
# for ${io.tmp.dirs}
sudo mkdir -p /opt/bd/tmp/flink
```

### Env

```bash
echo "PATH=\"/opt/bd/flink/bin:\${PATH}\"" | sudo tee /etc/profile.d/flink.sh
```

### Config

```bash
sudo vi /opt/bd/flink/conf/flink-conf.yaml
```

> ```
> jobmanager.rpc.address: las1
> taskmanager.numberOfTaskSlots: 8
> io.tmp.dirs: /opt/bd/tmp/flink
> jobmanager.memory.process.size: 2048m
> taskmanager.memory.process.size: 2048m
> ```

```bash
sudo vi /opt/bd/flink/conf/masters
```

> ```txt
> las1:8081
> ```

```bash
sudo vi /opt/bd/flink/conf/workers
```

> ```txt
> las1
> las2
> las3
> ```

```bash
for host in las2 las3; do
    for file in /opt/bd/flink/conf/{flink-conf.yaml,masters,workers}; do
        scp "${file}" "${host}:${file}"
    done
done
```

### Run

```bash
start-cluster.sh
stop-cluster.sh
```

### Usage

#### standalone

```bash
curl http://las1:8081/
```

```bash
flink run dataset-1.0.0-SNAPSHOT.jar
flink run streaming-1.0.0-SNAPSHOT.jar
```

#### on yarn

```bash
export HADOOP_CONF_DIR=/opt/bd/hadoop/etc/hadoop
export HADOOP_CLASSPATH="`hadoop classpath`"
flink run -t yarn-per-job dataset-1.0.0-SNAPSHOT.jar
```

**NOTE**:

```
${taskmanager.numberOfTaskSlots} <= ${yarn.scheduler.maximum-allocation-vcores}
${taskmanager.memory.process.size} <= ${yarn.scheduler.maximum-allocation-mb}
```

### SQL client

Install kafka connector

```bash
wget https://repo.maven.apache.org/maven2/org/apache/flink/flink-sql-connector-kafka_2.11/${ver}/flink-sql-connector-kafka_2.11-${ver}.jar

for host in las1 las2 las3; do
    scp "flink-sql-connector-kafka_2.11-${ver}.jar" "${host}:/opt/bd/flink/lib"
done
```

```bash
sql-client.sh embedded
```

In SQL client

```
help;
quit;
```

```sql
create table people (
    id int primary key,
    name varchar(64)
) with (
    'connector' = 'filesystem',
    'path' = 'hdfs:///user/root/people.csv',
    'format' = 'csv'
);

create table words (
    id int,
    person int,
    word varchar(128)
) with (
    'connector' = 'kafka',
    'topic' = 'test',
    'properties.bootstrap.servers' = 'localhost:9092',
    'properties.group.id' = 'test',
    'scan.startup.mode' = 'earliest-offset',
    'format' = 'csv'
);

select * from people;

select * from words;

select name, word from words join people on words.person = people.id;

drop table people;
drop table words;
```
