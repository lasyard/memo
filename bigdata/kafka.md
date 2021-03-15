# kafka

## 2.6.0

```bash
export ver="2.6.0"
```

### Install

```bash
wget https://mirrors.tuna.tsinghua.edu.cn/apache/kafka/${ver}/kafka_2.13-${ver}.tgz
```

```bash
sudo mkdir -p /opt/bd
sudo tar -C /opt/bd -xzvf kafka_2.13-${ver}.tgz
sudo ln -snf /opt/bd/kafka_2.13-${ver}/ /opt/bd/kafka
# for ${log.dirs}
sudo mkdir -p /opt/bd/tmp/kafka
```

### Env

```bash
echo "PATH=\"/opt/bd/kafka/bin:\${PATH}\"" | sudo tee /etc/profile.d/kafka.sh
```

### Config

```bash
sudo vi /opt/bd/kafka/config/server.properties
```

> ```
> broker.id=0
> log.dirs=/opt/bd/tmp/kafka
> num.partitions=1
> log.retention.hours=24
> zookeeper.connect=las1:2181,las2:2181,las3:2181
> ```

```bash
for host in las2 las3; do
    for file in /opt/bd/kafka/config/server.properties; do
        scp "${file}" "${host}:${file}"
    done
done
```

```bash
for i in {1..3}; do
    ssh "las${i}" "sed -i -e \"/broker.id=/c\\\\broker.id=${i}\" /opt/bd/kafka/config/server.properties"
done
```

### Run

```bash
for host in las1 las2 las3; do
    ssh "$host" "/opt/bd/kafka/bin/kafka-server-start.sh -daemon /opt/bd/kafka/config/server.properties"
done
for host in las1 las2 las3; do
    ssh "$host" "/opt/bd/kafka/bin/kafka-server-stop.sh"
done
```

### Usage

```bash
kafka-topics.sh --bootstrap-server localhost:9092 --list
kafka-topics.sh --bootstrap-server localhost:9092 --create --topic test --partitions 3
kafka-topics.sh --bootstrap-server localhost:9092 --describe --topic test
kafka-topics.sh --bootstrap-server localhost:9092 --delete --topic test
```

```bash
kafka-console-producer.sh --bootstrap-server localhost:9092 --topic test
kafka-console-consumer.sh --bootstrap-server localhost:9092 --from-beginning --topic test
```
