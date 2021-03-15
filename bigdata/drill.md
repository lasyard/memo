# drill

## 1.18.0

### Install

```bash
wget https://mirrors.tuna.tsinghua.edu.cn/apache/drill/drill-1.18.0/apache-drill-1.18.0.tar.gz
```

```bash
sudo mkdir -p /opt/bd
sudo tar -C /opt/bd -xzvf apache-drill-1.18.0.tar.gz
sudo ln -snf /opt/bd/apache-drill-1.18.0/ /opt/bd/drill
```

### Env

```bash
echo "PATH=\"/opt/bd/drill/bin:\${PATH}\"" | sudo tee /etc/profile.d/drill.sh
```

### Run

```bash
drill-embedded
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
