[](){#ref-cluster-santis}
# Santis

Santis is an Alps cluster that provides GPU accelerators and file systems designed to meet the needs of climate and weather models for the [CWP][ref-platform-cwp].

## Cluster specification

### Compute nodes

Santis consists of around 430 [Grace-Hopper nodes][ref-alps-gh200-node].

The number of nodes can change when nodes are added or removed from other clusters on Alps.

There are four login nodes, labelled `santis-ln00[1-4]`.
You will be assigned to one of the four login nodes when you ssh onto the system, from where you can edit files, compile applications and start simulation jobs.

| node type | number of nodes | total CPU sockets | total GPUs |
|-----------|-----------------| ----------------- | ---------- |
| [gh200][ref-alps-gh200-node] | 430 | 1,720      | 1,720 |

### Storage and file systems

Santis uses the [CWP filesystems and storage policies][ref-cwp-storage].

## Getting started

### Logging into Santis

To connect to Santis via SSH, first refer to the [ssh guide][ref-ssh].

!!! example "`~/.ssh/config`"
    Add the following to your [SSH configuration][ref-ssh-config] to enable you to directly connect to santis using `ssh santis`.
    ```
    Host santis
        HostName santis.alps.cscs.ch
        ProxyJump ela
        User cscsusername
        IdentityFile ~/.ssh/cscs-key
        IdentitiesOnly yes
    ```

### Software

CSCS and the user community provide software environments tailored to  [uenv][ref-uenv] are also available on Santis.

Currently, the following uenv are provided for the climate and weather community

* `icon/25.1`
* `climana/25.1`

In addition to the climate and weather uenv, all of the

??? example "using uenv provided for other clusters"
    You can run uenv that were built for other Alps clusters using the `@` notation.
    For example, to use uenv images for [daint][ref-cluster-daint]:
    ```bash
    # list all images available for daint
    uenv image find @daint

    # download an image for daint
    uenv image pull namd/3.0:v3@daint

    # start the uenv
    uenv start namd/3.0:v3@daint
    ```

It is also possible to use HPC containers on Santis:

* Jobs using containers can be easily set up and submitted using the [container engine][ref-container-engine].
* To build images, see the [guide to building container images on Alps][ref-build-containers].


## Running jobs on Santis

### Slurm

Santis uses [Slurm][ref-slurm] as the workload manager, which is used to launch and monitor distributed workloads, such as training runs.

There are two [Slurm partitions][ref-slurm-partitions] on the system:

* the `normal` partition is for all production workloads.
* the `debug` partition can be used to access a small allocation for up to 30 minutes for debugging and testing purposes.
* the `xfer` partition is for [internal data transfer][ref-data-xfer-internal] at CSCS.

| name | nodes  | max nodes per job | time limit |
| --   | --     | --                | -- |
| `normal` | 1266       | -    | 24 hours |
| `debug`  | 32         | 2    | 30 minutes |
| `xfer`   | 2          | 1    | 24 hours |

* nodes in the `normal` and `debug` partitions are not shared
* nodes in the `xfer` partition can be shared

See the Slurm documentation for instructions on how to run jobs on the [Grace-Hopper nodes][ref-slurm-gh200].

### FirecREST

Santis can also be accessed using [FirecREST][ref-firecrest] at the `https://api.cscs.ch/ml/firecrest/v2` API endpoint.

!!! warning "The FirecREST v1 API is still available, but deprecated"

## Maintenance and status

### Scheduled maintenance

Wednesday morning 8-12 CET is reserved for periodic updates, with services potentially unavailable during this timeframe. If the queues must be drained (redeployment of node images, rebooting of compute nodes, etc) then a Slurm reservation will be in place that will prevent jobs from running into the maintenance window. 

Exceptional and non-disruptive updates may happen outside this time frame and will be announced to the users mailing list, and on the [CSCS status page](https://status.cscs.ch).

### Change log

!!! change "2025-03-05 container engine updated"
    now supports better containers that go faster. Users do not to change their workflow to take advantage of these updates.

??? change "2024-10-07 old event"
    this is an old update. Use `???` to automatically fold the update.

### Known issues

