# Configuration flags

TiDB/TiKV/PD are configurable through command-line flags and environment variables.


## TiDB

The default TiDB ports are 4000 for client requests and 10080 for status report.

### --store
+ the storage engine type
+ Human-readable name for this member.
+ default: "goleveldb"
+ You can choose from "memory", "goleveldb", "BoltDB" or "TiKV". The first three are all local storage engines. TiKV is a distributed storage engine.
+ For example, we can start a pure memory engine TiDB through `tidb-server --store=memory`.

### --path
+ For local storage engines like goleveldb or BoltDB, the path specifies the actual storage directory of data.
+ For the memory storage engine, there is no need to set the path.
+ For the TiKV storage engine, the path specifies the actual PD address.
+ default: "/tmp/tidb"

### -L
+ the log level
+ default: "info"
+ You can choose from debug, info, warn, error, or fatal.

### --log-file
+ the log file
+ default: ""
+ If this flag is not set, logs will be written to stderr. Otherwise, logs will be stored in the corresponding file. Every day, logs will automatically rotate a new file and then rename and backup the old file.

### --host
+ the listening address for TiDB server
+ default: "0.0.0.0"
+ TiDB server will listen on this host.
+ By default, O.O.O.O listens on the address of all network cards. If you have more than one network card, you can specify the network card that provides services from outside.

### -P
+ the listening port for TiDB server
+ default: "4000"
+ TiDB server will accept MySQL client request from this port.

### --status
+ the status report port for TiDB server
+ default: "10080"
+ This is used to get server internal data. The data includes [prometheus metrics](https://prometheus.io/) and [pprof](https://golang.org/pkg/net/http/pprof/).
+ Prometheus metrics can be got through "http://host:status_port/metrics".
+ Pprof data can be got through "http://host:status_port/debug/pprof".

### --lease
+ the schema lease time in seconds
+ default: "1"
+ This is the schema lease time that is used in online schema changes. The value will affect the DDL statement running time. Do not change it unless you understand the internal mechanism.

### --socket
+ TiDB server uses unix socket file for connection.
+ default: ""
+ You can use the "/tmp/tidb.sock" to open unix socket file.

### --perfschema
+ enable(true) or disable(false) the performance schema
+ default: false
+ The value can be (true) or (false). (true) is to enable and (false) is to disable. The Performance Schema provides a way to inspect internal execution of the server at runtime. See [performance schema](http://dev.mysql.com/doc/refman/5.7/en/performance-schema.html) for more information. If you enable the Performance Schema, the performance will be affected.

### --privilege
+ enable(true) or disable(false) the privilege check.
+ default: false
+ The value can be (true) or (false). (true) is to enable and (false) is to disable. This feature is a work in progress, so it's disabled by default. It will be enabled by default when finished.

### --report-status
+ enable(true) or disable(false) the status report and pprof tool.
+ default: true
+ The value can be (true) or (false). (true) is to enable metrics and pprof. (false) is to disable metrics and pprof.

### --metrics-addr
+ the Prometheus Push Gateway address
+ default: ""
+ Leaving it empty stops the Prometheus client from pushing.

### --metrics-intervel
+ the time interval of pushing metrics to Prometheus Push Gateway
+ default: 15s
+ Setting the value to 0 stops the Prometheus client from pushing.

## Placement Driver (PD)

### -L

+ the log level
+ default: "info"
+ You can choose from debug, info, warn, error, or fatal.

### --log-file
+ the log file
+ default: ""
+ If this flag is not set, logs will be written to stderr. Otherwise, logs will be stored in the corresponding file. Every day, logs will automatically rotate a new file and then rename and backup the old file.

### --config

+ the config file
+ default: ""
+ If you set the configuration using the command line, the same setting in the config file will be overwritten.

### --name

+ the human-readable unique name for this PD member
+ default: "pd"
+ If you want to start multiple PDs, you must use different names for each one.

### --data-dir

+ the path to the data directory
+ default: "default.${name}"

### --client-urls

+ the listening URL list for client traffic
+ default: "http://127.0.0.1:2379"

###  --advertise-client-urls

+ the advertise URL list for client traffic from outside
+ default: ${client-urls}
+ If the client cannot connect to PD through the default listening client URLs, you must manually set the advertise client URLs explicitly.

### --peer-urls

+ the listening URL list for peer traffic
+ default: "http://127.0.0.1:2380"

### --advertise-peer-urls

+ the advertise URL list for peer traffic from outside
+ default: ${peer-urls}
+ If the peer cannot connect to PD through the default listening peer URLs, you must manually set the advertise peer URLs explicitly.

### --initial-cluster

+ the initial cluster configuration for bootstrapping
+ default: "{name}=http://{advertise-peer-url}"
+ For example, if `name` is "pd", and `advertise-peer-urls` is "http://192.168.100.113:2380", the `initial-cluster` is "pd=http://192.168.100.113:2380".

### --join

+ join the cluster dynamically
+ default: ""
+ If you want to dynamically add a PD to an existing cluster, you can use `--join="${advertise-client-urls}"`, the `advertise-client-url` is any existing PD's, multiple advertise client urls are separated by comma.

## TiKV

TiKV supports some human readable conversion.

 - File size (based on byte): KB, MB, GB, TB, PB (or lowercase)
 - Time (based on ms): ms, s, m, h

### -A, --addr

+ the server listening address
+ default: "127.0.0.1:20160"

### --advertise-addr

+ the server advertise address for client traffic.
+ default: ${addr}
+ If the client cannot connect to TiKV through the default listening address, you must manually set the advertise address explicitly.

### -L, --Log

+ the log level
+ default: "info"
+ You can choose from trace, debug, info, warn, error, or off.

### --log-file
+ the log file
+ default: ""
+ If this flag is not set, logs will be written to stderr. Otherwise, logs will be stored in the corresponding file. Every day, logs will automatically rotate a new file and then rename and backup the old file.

### -C, --config

+ the config file
+ default: ""
+ If you set the configuration using the command line, the same setting in the config file will be overwritten.

### -s, --store

+ the path to the data directory
+ default: "/tmp/tikv/store"

### --capacity

+ the store capacity
+ default: 0 (unlimited)
+ PD uses this flag to determine how to balance the TiKV servers. (Tip: you can use 10GB instead of 1073741824)

### --pd

+ the pd address lists
+ default: ""
+ You must set this flag to let TiKV connect to PD. Multiple address lists are separated by comma, for example, "192.168.100.113:2379, 192.168.100.114:2379, 192.168.100.115:2379".
