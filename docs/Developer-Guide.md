# OAP Developer Guide

This document contains the instructions & scripts on installing necessary dependencies and building OAP. 
You can get more detailed information from OAP each module blew.

* [SQL Index and Data Source Cache](../oap-cache/oap/docs/Developer-Guide.md)
* [RDD Cache PMem Extension](../oap-spark/README.md#compiling)
* [Shuffle Remote PMem Extension](../oap-shuffle/RPMem-shuffle/README.md#5-install-dependencies-for-shuffle-remote-pmem-extension)
* [Remote Shuffle](../oap-shuffle/remote-shuffle/README.md#build-and-deploy)
* [Intel MLlib](../oap-mllib/README.md)
* [Unified Arrow Data Source](../oap-data-source/arrow/README.md)
* [Native SQL Engine](../oap-native-sql/README.md)

## Building OAP

### Prerequisites for Building

OAP is built with [Apache Maven](http://maven.apache.org/) and Oracle Java 8, and mainly required tools to install on your cluster are listed below.

- [Cmake](https://help.directadmin.com/item.php?id=494)
- [GCC > 7](https://gcc.gnu.org/wiki/InstallingGCC)
- [Memkind](https://github.com/memkind/memkind/tree/v1.10.1-rc2)
- [Vmemcache](https://github.com/pmem/vmemcache)
- [HPNL](https://github.com/Intel-bigdata/HPNL)
- [PMDK](https://github.com/pmem/pmdk)  
- [OneAPI](https://software.intel.com/content/www/us/en/develop/tools/oneapi.html)
- [Arrow](https://github.com/Intel-bigdata/arrow)

- **Requirements for Shuffle Remote PMem Extension**  
If enable Shuffle Remote PMem extension with RDMA, you can refer to [Shuffle Remote PMem Extension Guide](../oap-shuffle/RPMem-shuffle/README.md) to configure and validate RDMA in advance.

We provide scripts below to help automatically install dependencies above **except RDMA**, need change to **root** account, run:

```shell script
# git clone -b <tag-version> https://github.com/Intel-bigdata/OAP.git
# cd OAP
# sh dev/install-compile-time-dependencies.sh
```

Run the following command to learn more.

```shell script
# sh $OAP_HOME/dev/scripts/prepare_oap_env.sh --help
```

Run the following command to automatically install specific dependency such as Maven.

```shell script
# sh $OAP_HOME/dev/scripts/prepare_oap_env.sh --prepare_maven
```

***NOTE:*** If you use `install-compile-time-dependencies.sh` or `prepare_oap_env.sh` to install GCC, or your GCC is not installed in the default path, please ensure you have exported `CC` (and `CXX`) before calling maven.
```shell script
# export CXX=$OAPHOME/dev/thirdparty/gcc7/bin/g++
# export CC=$OAPHOME/dev/thirdparty/gcc7/bin/gcc
```

### Building

To build OAP package, use
```shell script
$ sh $OAP_HOME/dev/compile-oap.sh
#or
$ mvn clean -DskipTests package
```

### Building Specified Module
```shell script
$ sh $OAP_HOME/dev/compile-oap.sh --oap-cache
#or
$ mvn clean -pl com.intel.oap:oap-cache -am package
```

### Running Test

To run all the tests, use
```shell script
$ mvn clean test
```

### Running Specified Module Test

```shell script
$ mvn clean -pl com.intel.oap:oap-cache -am test

```

### Building SQL Index and Data Source Cache with PMem

#### Prerequisites for building with PMem support

When use SQL Index and Data Source Cache with PMem, finish steps of [Prerequisites for building](#Prerequisites-for-building) to ensure needed dependencies have been installed.

#### Building package

Add `-Ppersistent-memory` to build OAP with PMem support.
 
```shell script
$ mvn clean -q -Ppersistent-memory -DskipTests package
```
For `vmemcache` strategy, build OAP with command :
```shell script
$ mvn clean -q -Pvmemcache -DskipTests package
```
You can build OAP with command below to use all of them:
```shell script
$ mvn clean -q -Ppersistent-memory -Pvmemcache -DskipTests package
```


### OAP Packaging 

If you want to generate a release package after you mvn package all modules, use the following command, then you can find a tarball named `oap-$VERSION-bin-spark-3.0.0.tar.gz` under directory `OAP/dev/release-package `.

```shell script
$ sh $OAP_HOME/dev/compile-oap.sh
```

## Contributing

This session introduces what is required before submitting a code change to OAP.

- We continue to use the Github **Issues** to track the new features/tasks/issues.​

- For every commit, we need an issue id for the commit. ​

- Format the log message as following: **[OAP-IssuesId][optional:ModuleName] detailed message**​ 

  like [OAP-1406][rpmem-shuffle]Add shuffle block removing operation within one Spark context 

- Always merge your pull request as a single commit and the commit message follow the above format.​

- The formal features names in 0.9 are: **SQL Index**, **SQL Data Source Cache**, **Native SQL Engine**, **Unified Arrow Data Source**, **RDD Cache PMem Extension**, **RPMem Shuffle**, **Remote Shuffle**, **Intel MLlib**.

We don’t strictly request the module id the same as the feature name. Please align in the feature members to use a consistent name in the log message.​
