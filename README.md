# IMDG Cluster: v3.11.1

As part of the Rolling Upgrade lab of Hazelcast Operations Training, this bundle includes a cluster preconfigured to run with Hazelcast Enterprise 3.11.1 which must be installed separately.

## Installing Bundle

*You must first create the workspace named `ws-3.11.1`. See the [Creating Workspace](#creating-workspace) section below for details.*

```console
install_bundle -download bundle-hazelcast-training-3.11.1-cluster-v3.11.1
```

## Use Case

This bundle is for training use only. As part of the Rolling Upgrade lab, the v3.11.1 cluster included in this bundle can be upgraded to to the v3.12.7 cluster, which is provided in the following bundle.

- [bundle-hazelcast-training-3.11.1-cluster-v3.12.7](git@github.com:padogrid/bundle-hazelcast-training-3.12.7-cluster-v3.12.7)

![Rolling Upgrade Diagram](/images/rolling-upgrade.jpg)

## Required Software

 - Hazelcast Enterprise 3.11.1 and 3.12.7

You can download version 3.11.1 and 3.12.7 from the following link:

[Hazelcast Enterprise Downloads](https://hazelcast.com/download/customer/)

## Creating Workspace

Following the lab instructions, run the `create_workspace` command to create the `ws-3.11.1` workspace. You will be prompt for the IMDG path which must be the Hazelcast Enterprise 3.11.1 home path as shown in the example below.

```bash
create_workspace -name ws-3.11.1
```

Switch workspace to `ws-3.11.1` and install this bundle.

```bash
switch_workspace ws-3.11.1
install_bundle -download bundle-hazelcast-training-3.11.1-cluster-v3.11.1
```

## Starting Cluster

Upon successful bundle installation, switch cluster, add three (3) members, and start the cluster.

```bash
switch_cluster v3.11.1
add_member; add_member; add_member
start_cluster
```
## Tearing Down

```bash
stop_cluster
```
