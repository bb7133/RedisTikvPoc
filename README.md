# RedisTikvPoc
[POC] Redis Module for TiKV

This is a POC repositority. This Redis Module will add branch new Redis commands to operate TiKV data.

After build the module you can use Redis's `MODULE LOAD` command load it.

## Build

```
> cargo build
```

## Usage

```
> module load libredistikv.so
> tikv.conn 127.0.0.1:2379
> tikv.put key value
> tikv.get key
> tikv.load key
> tikv.del key
> tikv.scan prefix 10
> tikv.delrange start-key end-key
```

## Commands

* tikv.conn [PDSERVER]: connect to TiKV cluster, PDSERVER is optional default is 127.0.0.1:2379
* tikv.put [KEY] [VALUE]: put a Key-Value pair into TiKV cluster.
* tikv.get [KEY]: read a key's value from TiKV cluster.
* tikv.del [KEY]: delete a key from TiKV cluster.
* tikv.load [KEY]: read a key's value from TiKV cluster and use SET command save the key-value pair into Redis memory.
* tikv.scan [PREFIX] [LIMIT]: scan TiKV cluster data's using given `PREFIX` and return `LIMIT` rows.
* tikv.delrange [STARTKEY] [ENDKEY]: use delete\_range API to delete many key's from TiKV cluster.

## Benchmark

In `bench` folder it contains a golang written program to do some basic performance test. As a result, read or write data from TiKV cluster will always slower than Redis SET and GET command.

As the implementation of Redis Module, it using block\_client api to make it concurrency ready. So **ONE** operate latency will not get the top performance. And this is why benchmark for `tikv` serise commands is using worker pool to test in concurrency mode. As we can see if more workers in pool the better performance gets.