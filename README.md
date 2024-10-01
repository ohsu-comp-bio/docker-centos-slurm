> [!WARNING]
> This repo is archived/deprecated — please see https://github.com/ohsu-comp-bio/docker-centos7-slurm for the latest updates

# Slurm on CentOS 6 Docker Image

[![Docker Build Status](https://img.shields.io/docker/build/giovtorres/docker-centos6-slurm.svg)](https://hub.docker.com/r/giovtorres/docker-centos6-slurm/builds/)
[![Docker Automated build](https://img.shields.io/docker/automated/giovtorres/docker-centos6-slurm.svg)](https://hub.docker.com/r/giovtorres/docker-centos6-slurm/)
[![Docker Pulls](https://img.shields.io/docker/pulls/giovtorres/docker-centos6-slurm.svg)](https://hub.docker.com/r/giovtorres/docker-centos6-slurm/)
[![](https://images.microbadger.com/badges/image/giovtorres/docker-centos6-slurm.svg)](https://microbadger.com/images/giovtorres/docker-centos6-slurm "Get your own image badge on microbadger.com")

This is an all-in-one [Slurm](https://slurm.schedmd.com/) installation.  This
container runs the following processes:

* slurmd (The compute node daemon for Slurm)
* slurmctld (The central management daemon of Slurm)
* slurmdbd (Slurm database daemon)
* munged (Authentication service for creating and validating credentials)
* mysql (Database for slurmdbd)
* supervisord (A process control system)

## Usage

There are multiple
[tags](https://hub.docker.com/r/giovtorres/docker-centos6-slurm/tags/)
available.  To use the latest available image, run:

```
docker pull giovtorres/docker-centos6-slurm:latest
docker run -it -h ernie giovtorres/docker-centos6-slurm:latest
```

The above command will drop you into a bash shell inside the container.
Supervisord is the process manager.  To view the status of all the processes,
run:

```
[root@ernie /]# supervisorctl status
munged                           RUNNING   pid 23, uptime 0:02:35
mysqld                           RUNNING   pid 24, uptime 0:02:35
slurmctld                        RUNNING   pid 25, uptime 0:02:35
slurmd                           RUNNING   pid 22, uptime 0:02:35
slurmdbd                         RUNNING   pid 26, uptime 0:02:35
```

In `slurm.conf`, the **ControlMachine** hostname is set to **ernie**. Since
this is an all-in-one installation, the hostname must match **ControlMachine**.
Therefore, you must pass the `-h ernie` to docker at run time so that the
hostnames match.

You can run the usual slurm commands:

```
[root@ernie /]# sinfo
PARTITION AVAIL  TIMELIMIT  NODES  STATE NODELIST
normal*      up 5-00:00:00      5   idle c[1-5]
```

```
[root@ernie /]# scontrol show partition
PartitionName=normal
   AllowGroups=ALL AllowAccounts=ALL AllowQos=ALL
   AllocNodes=ALL Default=YES QoS=N/A
   DefaultTime=5-00:00:00 DisableRootJobs=NO ExclusiveUser=NO GraceTime=0 Hidden=NO
   MaxNodes=1 MaxTime=5-00:00:00 MinNodes=1 LLN=NO MaxCPUsPerNode=UNLIMITED
   Nodes=c[1-5]
   PriorityJobFactor=50 PriorityTier=50 RootOnly=NO ReqResv=NO OverSubscribe=NO
   OverTimeLimit=NONE PreemptMode=OFF
   State=UP TotalCPUs=5 TotalNodes=5 SelectTypeParameters=NONE
   DefMemPerCPU=500 MaxMemPerNode=UNLIMITED
```

## Building

There are multiple versions of Slurm available, each in its own subdirectory.
To build a particular version, change into the directory and build the
Dockerfile:

```
git clone https://github.com/giovtorres/docker-centos6-slurm
cd docker-centos6-slurm/16.05.9
docker build -t docker-centos6-slurm:16.05.9 .
```

## Notes

I use this container to get access to the Slurm headers and libraries for
[PySlurm](https://github.com/PySlurm/pyslurm) development.

> **Important Note**: This image is used for testing and development.  It is
> not suited for any production use.

