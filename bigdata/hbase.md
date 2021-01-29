# hbase

## 2.2.6

### Install

```bash
wget https://mirrors.tuna.tsinghua.edu.cn/apache/hbase/2.2.6/hbase-2.2.6-bin.tar.gz
```

```bash
sudo mkdir -p /opt/bd
sudo tar -C /opt/bd -xzvf hbase-2.2.6-bin.tar.gz
sudo ln -snf /opt/bd/hbase-2.2.6/ /opt/bd/hbase
# for ${hbase.tmp.dir}
sudo mkdir -p /opt/bd/tmp/hbase
```

### Env

```bash
echo "PATH=\"/opt/bd/hbase/bin:\${PATH}\"" | sudo tee /etc/profile.d/hbase.sh
```

### Config

```bash
sudo vi /opt/bd/hbase/conf/hbase-env.sh
```

> ```bash
> export JAVA_HOME=/usr
> export HBASE_MANAGES_ZK=false
> ```

```bash
sudo vi /opt/bd/hbase/conf/hbase-site.xml
```

> ```xml
> <configuration>
>     <property>
>         <name>hbase.cluster.distributed</name>
>         <value>true</value>
>     </property>
>     <property>
>         <name>hbase.tmp.dir</name>
>         <value>/opt/bd/tmp/hbase</value>
>     </property>
>     <property>
>         <name>hbase.rootdir</name>
>         <value>hdfs://las1:9000/hbase</value>
>     </property>
>     <property>
>         <name>hbase.zookeeper.quorum</name>
>         <value>las1,las2,las3</value>
>     </property>
> </configuration>
```

```bash
sudo vi /opt/bd/hbase/conf/regionservers
```

> ```txt
> las1
> las2
> las3
> ```

```bash
for host in las2 las3; do
    for file in /opt/bd/hbase/conf/{hbase-env.sh,hbase-site.xml,regionservers}; do
        scp "${file}" "${host}:${file}"
    done
done
```

### Run

Start hdfs & zookeeper first.

```bash
start-hbase.sh
stop-hbase.sh
```

### Usage

```bash
hbase shell
```

```bash
curl http://las1:16010/
```
