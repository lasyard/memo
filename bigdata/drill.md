# drill

## 1.18.0

```bash
export ver="1.18.0"
```

### Install

```bash
wget https://mirrors.tuna.tsinghua.edu.cn/apache/drill/drill-${ver}/apache-drill-${ver}.tar.gz
```

```bash
sudo mkdir -p /opt/bd
sudo tar -C /opt/bd -xzvf apache-drill-${ver}.tar.gz
sudo ln -snf /opt/bd/apache-drill-${ver}/ /opt/bd/drill
```

```bash
hdfs dfs -mkdir -p /user/root/drill/udf/registry
```

### Env

```bash
echo "PATH=\"/opt/bd/drill/bin:\${PATH}\"" | sudo tee /etc/profile.d/drill.sh
```

```bash
# don't use HADOOP_CLASSPATH to connect to dfs for version confliction.
unset HADOOP_CLASSPATH
```

### Config

```bash
sudo vi /opt/bd/drill/conf/drill-override.conf
```

> ```
> drill.exec: {
>    cluster-id: "las",
>    zk.connect: "las1:2181,las2:2181,las3:2181"
> }
> ```

```bash
for host in las2 las3; do
    for file in /opt/bd/drill/conf/drill-override.conf; do
        scp "${file}" "${host}:${file}"
    done
done
```

### Run

#### Embedded

```bash
drill-embedded
```

#### Distributed

```bash
for host in las1 las2 las3; do
    ssh "${host}" "/opt/bd/drill/bin/drillbit.sh start"
done
for host in las1 las2 las3; do
    ssh "${host}" "/opt/bd/drill/bin/drillbit.sh stop"
done
```

```bash
drill-localhost
```

### Usage

```bash
curl http://las1:8047/
```

In drill

```
!help
!quit
```

```sql
use cp;
select * from `employee.json` limit 1;
```
