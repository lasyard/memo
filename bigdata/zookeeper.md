# zookeeper

## 3.6.2

### Install

```bash
wget https://mirrors.tuna.tsinghua.edu.cn/apache/zookeeper/zookeeper-3.6.2/apache-zookeeper-3.6.2-bin.tar.gz
```

```bash
sudo mkdir -p /opt/bd
sudo tar -C /opt/bd -xzvf apache-zookeeper-3.6.2-bin.tar.gz
sudo ln -snf /opt/bd/apache-zookeeper-3.6.2-bin/ /opt/bd/zookeeper
# for ${dataDir}
sudo mkdir -p /opt/bd/tmp/zookeeper
```

### Env

#### CentOS 7.3

```bash
echo "PATH=\"/opt/bd/zookeeper/bin:\${PATH}\"" | sudo tee /etc/profile.d/zookeeper.sh
```

### Config

```bash
sudo vi /opt/bd/zookeeper/conf/zoo.cfg
```

> ```
> tickTime=2000
> dataDir=/opt/bd/tmp/zookeeper
> clientPort=2181
> initLimit=5
> syncLimit=2
> server.1=las1:2888:3888
> server.2=las2:2888:3888
> server.3=las3:2888:3888
> # Default AdminServer port is 8080, so we don't need it
> admin.enableServer=false
> ```

```bash
for i in {1..3}; do
    ssh "las${i}" "echo \"${i}\" | sudo tee /opt/bd/tmp/zookeeper/myid"
done
```

```bash
for host in las2 las3; do
    for file in /opt/bd/zookeeper/conf/zoo.cfg; do
        scp "${file}" "${host}:${file}"
    done
done
```

### Run

```bash
for host in las1 las2 las3; do
    ssh "${host}" "/opt/bd/zookeeper/bin/zkServer.sh start"
done
for host in las1 las2 las3; do
    ssh "${host}" "/opt/bd/zookeeper/bin/zkServer.sh stop"
done
```

### Usage

```bash
zkCli.sh -server las1
```
