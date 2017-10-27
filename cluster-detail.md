## nodes
- The cluster is of four aws i3.4x nodes. (having 120GB RAM and 16 cores each)
- The cluster hosts only one index. All four are content nodes and two of them are container nodes.
- ![services.xml](https://github.com/yogin16/tweet-vespa-app/blob/master/src/main/application/services.xml)
- This is a standalone cluster and no other queries running on this apart from tests.

### memory usage
- VESPA_HOME is "/opt/vespa"
- node1:
```!bash
$ free -g
              total        used        free      shared  buff/cache   available
Mem:            119          24           5           0          90          94
Swap:             1           0           1
```

```!bash
df -kh
Filesystem      Size  Used Avail Use% Mounted on
/dev/xvda1       15G   11G  4.2G  73% /
devtmpfs         60G     0   60G   0% /dev
tmpfs            60G     0   60G   0% /dev/shm
tmpfs            60G  137M   60G   1% /run
tmpfs            60G     0   60G   0% /sys/fs/cgroup
/dev/nvme0n1p1  1.6T  242G  1.3T  16% /mnt1
/dev/nvme1n1p1  1.6T   46G  1.5T   3% /opt/vespa
tmpfs            12G     0   12G   0% /run/user/5071
tmpfs            12G     0   12G   0% /run/user/0
```

- node 2:
```!bash
$ free -g
              total        used        free      shared  buff/cache   available
Mem:            119          24          12           0          82          94
Swap:             1           0           1
```

```!bash
$ df -kh
Filesystem      Size  Used Avail Use% Mounted on
/dev/xvda1       15G  6.2G  8.9G  42% /
devtmpfs         60G     0   60G   0% /dev
tmpfs            60G     0   60G   0% /dev/shm
tmpfs            60G  137M   60G   1% /run
tmpfs            60G     0   60G   0% /sys/fs/cgroup
/dev/nvme0n1p1  1.6T   78G  1.5T   6% /mnt1
/dev/nvme1n1p1  1.6T   51G  1.5T   4% /opt/vespa
tmpfs            12G     0   12G   0% /run/user/0
tmpfs            12G     0   12G   0% /run/user/5071
```

- node 3:
```!
$ free -g
              total        used        free      shared  buff/cache   available
Mem:            119          23          85           0          11          95
Swap:             1           0           1
```

```!bash
$ df -kh
Filesystem      Size  Used Avail Use% Mounted on
/dev/xvda1       15G  7.4G  7.7G  49% /
devtmpfs         60G     0   60G   0% /dev
tmpfs            60G     0   60G   0% /dev/shm
tmpfs            60G  137M   60G   1% /run
tmpfs            60G     0   60G   0% /sys/fs/cgroup
/dev/nvme0n1p1  1.6T  9.1G  1.5T   1% /mnt1
/dev/nvme1n1p1  1.6T   46G  1.5T   3% /opt/vespa
tmpfs            12G     0   12G   0% /run/user/5071
```

- node 4:
```!bash
$ free -g
              total        used        free      shared  buff/cache   available
Mem:            119          21          96           0           1          97
Swap:             1           0           1
```

```!bash
$ df -kh
Filesystem      Size  Used Avail Use% Mounted on
/dev/xvda1       15G  6.1G  9.0G  41% /
devtmpfs         60G     0   60G   0% /dev
tmpfs            60G     0   60G   0% /dev/shm
tmpfs            60G  137M   60G   1% /run
tmpfs            60G     0   60G   0% /sys/fs/cgroup
/dev/nvme0n1p1  1.6T   77M  1.5T   1% /mnt1
/dev/nvme1n1p1  1.6T   47G  1.5T   4% /opt/vespa
tmpfs            12G     0   12G   0% /run/user/0
tmpfs            12G     0   12G   0% /run/user/5071
```

## index

- The tweet ![search definition](https://github.com/yogin16/tweet-vespa-app/blob/master/src/main/application/searchdefinitions/tweet.sd) for application. There are 51 attribute fields. Total documents in the index:
66308891 (66M)
- ![Metrics API Response](https://github.com/yogin16/tweet-vespa-app/blob/master/matrics-api-response.js)
- Queries without "group by" come back in milliseconds; ![search queries](https://github.com/yogin16/tweet-vespa-app/blob/master/requests/search-query.json) are fine. 
- Aggregation query times out. ![sample aggregation](https://github.com/yogin16/tweet-vespa-app/blob/master/requests/aggregation-query.json)
- There are no other writes at the moment of aggregation query, no other queires as well.

## htop

- This is htop on node1 when no query is running on cluster:
- Memory:
![Memory](https://github.com/yogin16/tweet-vespa-app/blob/master/images/mem-with-no-queries.png)
- CPU:
![CPU](https://github.com/yogin16/tweet-vespa-app/blob/master/images/cpu-with-no-queries.png)


- htop on node1 when the aggregation query is running which times out.
- Memory:
![Memory](https://github.com/yogin16/tweet-vespa-app/blob/master/images/mem-when-agg-running.png)
- CPU: 
![CPU](https://github.com/yogin16/tweet-vespa-app/blob/master/images/cpu-when-agg-running.png)
