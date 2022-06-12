# Rolling Upgrade Training

As part of the Rolling Upgrade lab of Hazelcast Operations Training, this bundle includes a cluster and an app preconfigured to run with Hazelcast Enterprise 3.11.x which must be installed separately.

## Installing Bundle

![PadoGrid](https://github.com/padogrid/padogrid/raw/develop/images/padogrid-3d-16x16.png) [*Driven by PadoGrid*](https://github.com/padogrid)

*You must first create the workspace named `ws-3.11`. See the [Creating `ws-3.11` Workspace](#creating-ws-311-workspace) section below for details.*

```bash
install_bundle -download bundle-hazelcast-training-3.11-app-perf_test_v3.11-cluster-v3.11
```

## Use Case

*This bundle is for training use only.* As part of the Rolling Upgrade lab, the v3.11 cluster included in this bundle can be upgraded to to a v3.12.x cluster. You must create the v3.12 cluster separately as described in this article.

## Required Software

 - Hazelcast Enterprise 3.11.x and 3.12.x

You can download version 3.11.x and 3.12.x from the following link:

[Hazelcast Enterprise Downloads](https://hazelcast.com/download/customer/)

## Creating `ws-3.11` Workspace

Following the lab instructions, run the `create_workspace` command to create the `ws-3.11` workspace. You will be prompt for the IMDG path which must be the Hazelcast Enterprise 3.11.x home path as shown in the example below.

```bash
create_workspace -name ws-3.11
```

**Output:**

```console
Enter Java home path.
[/home/dpark/products/jdk1.8.0_212]:

Enter the local product home directory path. Choose one
from the defaults listed below or enter another.
[]:
/home/dpark/Hazelcast/hazelcast-enterprise-3.11.1

Enter workspace name.
[ws-3.11]:

Enter default cluster name.
[myhz]:

Enable VM? Enter 'true' or 'false' [false]:

You have entered the following.
                     JAVA_HOME: /home/dpark/products/jdk1.8.0_212
                  PRODUCT_HOME: /home/dpark/Hazelcast/hazelcast-enterprise-3.11.1
            PADOGRID_WORKSPACE: /home/dpark/Hazelcast/workspaces/rwe-training/ws-3.11
               Default Cluster: myhz
                    VM_ENABLED: false
Enter 'c' to continue, 'r' to re-enter, 'q' to quit: c
```

Switch workspace to `ws-3.11` and install this bundle.

```bash
switch_workspace ws-3.11
install_bundle -download bundle-hazelcast-training-3.11-cluster-v3.11
```

## Starting `v3.11` Cluster

Upon successful bundle installation, switch cluster, add three (3) members, start the cluster and the management center.

```bash
switch_cluster v3.11
add_member; add_member; add_member
start_cluster
start_mc
```

**Management Center URL:** http://localhost:8080/hazelcast-mancenter

## Ingest Data to `v3.11` Cluster

This bundle also includes the `perf_test_v3.11` app. Run the `test_ingestion` command to ingetst data into the cluster as follows.

```bash
cd_app perf_test_v3.11; cd bin_sh
./test_ingestion -run
```

## Creating `ws-3.12` Workspace

Create the `ws-3.12` workspace with the default cluster `v3.12` as follows.

```bash
create_workspace -name ws-3.12 -cluster v3.12
```

**Output:**

```console
Enter Java home path.
[/home/dpark/products/jdk1.8.0_212]:

Enter the local product home directory path. Choose one
from the defaults listed below or enter another.
[]:
/home/dpark/Hazelcast/hazelcast-enterprise-3.12.7
Enter workspace name.
[ws-3.12]:

Enter default cluster name.
[v3.12]: 

Enable VM? Enter 'true' or 'false' [false]:

You have entered the following.
                     JAVA_HOME: /home/dpark/products/jdk1.8.0_212
                  PRODUCT_HOME: /home/dpark/Hazelcast/hazelcast-enterprise-3.12.7
            PADOGRID_WORKSPACE: /home/dpark/Hazelcast/workspaces/rwe-training/ws-3.12
               Default Cluster: v3.12
                    VM_ENABLED: false
Enter 'c' to continue, 'r' to re-enter, 'q' to quit: c
```

## Rolling Upgrade Steps

Rolling upgrde is supported only between consecutive minor version. For example, to upgrade from v3.10 to v3.12, you must first upgrade v3.10 to v3.11 and then v3.11 to v3.12.

The following steps must be taken when you upgrade to the next minor version.

1. Stop v3.11 member-01
2. Wait till ClusterSafe=true
3. Start v3.12 member-01
4. Wait till ClusterSafe=true
5. Repeat the above steps for the rest of the members
6. Commit version to 3.12

The ClusterSafe status can be minitored by executing the following command.

```bash
watch curl -sS http://localhost:5701/hazelcast/health
```

*This bundle is made available as part of a Hazelcast training course. You can follow the instructions provided in the course or read the [Rolling Member Upgrades](https://docs.hazelcast.org/docs/latest/manual/html-single/index.html#rolling-member-upgrades
) section of the Hazelcast IMDG Reference Manual.*

## Starting `v3.12` Cluster

By default, PadoGrid adds two (2) members to a cluster. Let's add one more to match the number of members in the `v3.11` cluster and then start the cluster and the management center.

```bash
switch_workspace ws-3.12
switch_cluster v3.12
add_member
start_cluster
start_mc
```

**Management Center URL:** http://localhost:8080/hazelcast-mancenter

## Tearing Down

```bash
# Stop v3.11 cluster
switch_workspace ws-3.11
stop_cluster -cluster v3.11
stop_mc -cluster v3.11

# Stop v3.12 cluster
switch_workspace ws-3.11
stop_cluster -cluster v3.12
stop_mc -cluster v3.12
```
