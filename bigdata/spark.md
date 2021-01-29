# spark

## 3.0.1

### Install

```bash
wget https://mirrors.tuna.tsinghua.edu.cn/apache/spark/spark-3.0.1/spark-3.0.1-bin-hadoop3.2.tgz
```

```bash
sudo mkdir -p /opt/bd
sudo tar -C /opt/bd -xzvf spark-3.0.1-bin-hadoop3.2.tgz
sudo ln -snf /opt/bd/spark-3.0.1-bin-hadoop3.2/ /opt/bd/spark
```

### Env

```bash
echo "PATH=\"/opt/bd/spark/bin:/opt/bd/spark/sbin:\${PATH}\"" | sudo tee /etc/profile.d/spark.sh
```

### Config

```bash
sudo vi /opt/bd/spark/conf/slaves
```

> ```txt
> las1
> las2
> las3
> ```

```bash
for host in las2 las3; do
    for file in /opt/bd/spark/conf/slaves; do
        scp "${file}" "${host}:${file}"
    done
done
```

### Run

```bash
start-all.sh
stop-all.sh
```

### Usage

```bash
curl http://las1:8080/
```

```bash
spark-submit --master spark://las1:7077 /opt/bd/spark/examples/src/main/python/pi.py 10
```
