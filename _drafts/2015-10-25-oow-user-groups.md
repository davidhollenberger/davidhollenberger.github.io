---
layout: post
title:  OOW - Sunday User Group Sessions
---
Just some notes from Oracle Open World Day 1.  A lot of great user group sessions today.  Starting the day a little late after running the Golden Gate Bridge with the SQLDev team and others.

# Linux/UNIX tool for the Oracle DBA
**ugf-9957**
*Tim Gorman*

## Diagnostics

Looking at OS level diagnostics.  

* cpu/memory
* processes
* IO and Storage
* Network
* traces
* Other usefull things

### SAR
highly aggregated information isn't always very useful.  Get a general idea of what is going on.  Sometimes

cpu run queue report: sar -q
context switching - jumping back and forth from different code...
VM Swapping
VM Paging

```sar -u 5 5```

%steal - awareness of being on a virtual machine.  vms would have positive values, neighboring vms could be "stealing" processes.  

mpstat - per-processor statistics

```mpstat 5 5```

vmstat - virtual memory statistics

the best aggregated information

```mvstat 5 5```

r - run queue
b - busy
w - wait

uptime and w utilities

w displays info about logged in unix users (linux too?)

ipcs utilitiy

- shared memory - used by SGA
- semaphores - used by background and foreground processes

**see slides for list of parameters that affect shared memory and semaphores**

```ipcs -mb``` (memory,bytes)

```ipcs -sb``` (semaphores)

oracle sysresv utilitiy

```bash
cd $ORACLE_HOME/bin
./sysresv
```

## Process Diagnostics

home-grown "top" command

```ps -eaf | sort -n +3 | tail```

### pmap/svmon (AIX?)

how memory is mapped inside a Process
Text/Shared are static. SGA.
heap memory are variables.  it is variable.  PGA within oracle.

```pmap -x <pid>```

### fuser/lsof
what processes have files open

```fuser /d01001/oradata/PROD/indx_01.dbf```

use ```ps -eaf | grep <pid>``` to see users that own that Process
then you can kill it!

run ```df -k``` to get list of filesystems, then fuser to get list of processes using that filesystem.

### jstat

concerned with java garbage collection.  **see slides**

```jstat -gc #### ###?```

YGC - young generation.  not as concerned with these.
FGC - full generation

### iostat
io loads and waits.  high level device information.  highly aggregated.  

md - meta devices
sd - scsi devices

svc_t - service time.  most useful of these statistics.

### ifconfig

```ifconfig -h``` or -a?

### netstat
network connections, routing tagles, interface statistics.

*netstat*
```netstat -r```  - routing tables

### ping
```ping -c 4 www.yahoo.com``` ping 4 times
``` ping -p <port?>```  ping using different port

### traceroute and iperf
latency, route and throughput.
 iperf requires that you have a process on the local system and remote system

### tcpdump and wireshark

### strace
attach to or run a process.  dump system calls or function calls.  give a good idea of what a process is doing.

### DTrace
(http://dtrace.org)

### others
adb
strings - ```strings oracle | more```  dump oracle executable
file - know what a file is before you edit it
dd - move, convert
sosreport
kdump
oswatcher

## Oracle-L List
Global email forum
(subscribe)[http://www.freelists.org/list/oracle-l]



# Mining Automatic Workload Repository: Alternative Methods for Identification
*Maris Elsins*
*Pythian*

bit.ly/Maris_OOW15_AWR

@MarisElsins

./getMOSPatch.sh
http://bit.ly/getMOSPatch

Snapshot - like a database selfie.  

768 V$ views in 12.1.0.2
AWR contains 62+ V$ and X$ views

topnsql - 30 is default.  
ALL => 100

2 Types of Collected statistics
1. Deltas
2. Totals

Snap Started time = End interval time

SQL Statistics section is aggregated by SQL_ID
SQL ID uniquely identifies a sql statement. (both a good and bad thing)

Could have the "same" query with many sql_ids

## Extract Data by grouping


```break on EXACT_MATCHING_SIGNATURE duplicates skip 1```
```break on FORCE_MATCHING_SIGNATURE duplicates skip 1```

use of constants in semantically equivalent statements - used in data warehouseing to improve plan?

```sql
desc dba_hist_snapshot
desc dba_hist_sqlstat
@awr_top_by_sqlid_snaps.sql 25 50 5 10
@awr_sqlid_perf_trend_by_plan.sql --shows changes of execution plans
```

Query using Forced matching signature

```sql
@awr_top_by_fms_snaps.sql 25 50 5 10
@awr_top_by_fms_detail_snaps.sql
```

Plan Hash Value - different statements using the same execution plan

```sql
@awr_top_by_plan_snaps.sql 25 50 5 10
@awr_top_by_plan....?
```

Can tune many statements if the plan can be optimized.

Can avoid the overhead of creating a full AWR report
**Look at TOPNSQL**


# Rapid Provisioning of Oracle Clusters and Databases
UGF1027
Charles Kim @racdba
Nitin Vengurekar @dbcloudshifu
Viscosity North America

cloud computing sig meeting otn lounge moscone south 10am

DMA - database machine administrator
vRAC DBA
Cloud DBA
Cloud administrator

## Linux OS Configuration

Compile pre-install RPM on RedHat **see slides***
modify spec file, rpm build.

OR, wget repo file from oracle's public yum server with gpg key.

ssh user equivalence
12c/grid/sshsetup
./sshUserSetup.sh -user oracle -hosts "rac1 rac2" -noPromptPassphrase -advanced

Configure OSWatcher id 580513

Partition Alignment - 1M or 4M offset
1M for OCR/Vote
4M for DATA and FRA
```sfdisk``` limit of 2TB
```parted``` use this instead of sfdisk.  supports larger disks

I/O Schedulers
baremetal run deadline
vmware run noop

RAC
- unlock the grid home
- create tar ball of GI Home (excludes directories, logfiles, text files)
- clone.sh and pass in response file

Create golden image template of database
```dbca -silent -createCloneTemplate```
shuts down db and runs cold backup

## Rapid Home Provisioning (12.1.0.2)


# Internals of the Oracle Database 12c Muiltitenant Architecture
UGF4133
Vit Spinka @vitspinka
dbvisit
[download slides](http://vitspinka.cz/download.html)

alter pluggable database ... save state

```desc v$pdbs;```

CON_ID - one byte, unique in the database. (container id)
- 0 - non-CDB database
- 1 - CDB level
- 2 - PDB$SEED
- 255 - reserved

CON_UID similar as DBID (con_uid of cdb is dbid)

```desc v$datafiles```
lists files and con_id for each file.

Oracle uses CON_UID when you use OMF for datafile path.

CDB Views  ```CDB$VIEW``` undocumented view
CDB_OBJECTS

12.1.0.2 introduces CONTAINERS clause

```select * from CONTAINERS("SYS"."DBA_OBJECTS");```

metadata links - pdb has pointers to records in CDB

## Redo
looking into redo records. why?

```select dump 100,16```  not sure what he's getting at here...

Just one redo stream for a CDB and contains changes for all PDBs.
redo record contains CON_ID and CON_UID as well as each change

non-cdb database still contains CON_ID, but will be 0.

Undo unique for the CDB.

### How can we use this?
Backup
- cdb has metadata about PDBs, so it must be backed up too.
- backup at pdb level cannot access archivelogs
- take a close look at backup scripts and archive log backups

DataGuard
- almost oblivious to CDB/PDB
- DG works at CDB level
- target is always a CDB

Patching
- most changes at CDB container
- create new OH and CDB, patch it
- replug PDBs into new CDB, might need more scripts

Tips
- you can use TWO_TASK (instead of ORACLE_SID) for local sqlplus
- RMAN in PDB does not see archivelogs
- ```DROP PLUGGABLE DATABASE``` will deregister backups






## Accidental Siri Activation Count: 2
