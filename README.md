# paystrata-gr-lab (macOS / Linux)

Lab for the session *ProxySQL and Connection Management*: a 3-node MySQL Group Replication cluster in Docker,
a connection-limit test, ProxySQL (HG10 writer / HG20 readers), primary crash with automatic failover, and
read/write routing.

Windows? Use [paystrata-gr-lab-windows](https://github.com/kiki-irina/paystrata-gr-lab-windows)
(same scripts, run in WSL2).

## What you need (once)

- Docker with 6 CPUs and 10 GB RAM for the full lab (4 CPUs and 8 GB for the small size, `SIZE=small`).
  On a Mac with Colima: `colima start --cpu 6 --memory 10`. With Docker Desktop: Settings > Resources.
- Git and Python 3 (Python is only for the charts).
- Ports 3307, 3308, 3309, 6032 and 6033 free.

## The five steps

```
git clone https://github.com/kiki-irina/paystrata-gr-lab.git
cd paystrata-gr-lab
chmod +x *.sh

./step1_cluster.sh      3-node cluster, mysqlsh checks, cluster.status()
./step2_limit.sh        1,000 connection limit, no proxy (error 1040, RAM per connection)
./step3_proxy.sh        the same load through ProxySQL
./step4_failover.sh     primary crash, automatic failover, writer moves to HG10
./step5_routing.sh      where reads and writes go, before and after a failover
./plot.sh               charts in results/
```

Small size for laptops: `SIZE=small ./step2_limit.sh` and `SIZE=small ./step3_proxy.sh`.
Live view in a second terminal: `./watch_servers.sh`.

## Files

- `LAB_GUIDE.txt`: the full guide (story, steps, manual commands, experiments, troubleshooting).
- `docker-compose.yml`, `conf/`: the cluster and ProxySQL configuration.
- `loadgen/`: the connection-ramp load generator.
- `results/`: charts and CSV files written by the tests (not committed).

The passwords in this repository (`paystrata`, `app_pass`, `radmin`) are lab-only values.
