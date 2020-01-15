---
permalink: /picocenter/
---

<!-- ![Picojump diagram](images/picojump.png) -->

### Problem

![This is a cluster](images/cluster.png)

Today's clusters are heterogeneous collections of machines. Datacenters are
compiled of database servers, compute servers, servers with high memory, and
gpu-enabled machines. These servers exist connected to a high speed network and
are built to meet different application needs.

What do you do if you want your application to take advantage of all of these
resources, transparently?

### Abstract

Computing resources are increasingly distributed, creating challenges for
programmers and users.  Programmers are faced with the challenge of implementing
distributed applications, and users of legacy applications must either co-locate
all of the resources or divide the application into tasks that can be executed
independently on different hosts.

This paper presents PicoJump, a system that enables legacy applications to
execute when their resources are distributed amongst different operating
systems.  At runtime, PicoJump interposes on resource access to make remote
access transparent.  As well, PicoJump monitors application behavior and
migrates the application in order to minimize the amount of remote accesses.
PicoJump leverages prior work in live migration to make migrating applications
between hosts fast and efficient.  Our evaluation of a prototype implementation
of PicoJump demonstrates that it is feasible and sufficiently performant on
multiple real-world applications.

### A brief history of process migration
Process migration first appeared in the 80s as a way of moving running processes
within unix clusters. With the birth of virtual machines, traditional process
migration techniques had to be improved as the memory of a virtual machine is
much bigger than that of a process. At this point, techniques that fall under
the "live migration" umbrella appeared. These techniques seek to minimize
_downtime_, or the time when the process is paused during migration. These two
techniques are _pre-copy_ and _post-copy_ live migration. Picojump takes
advantage of characterstics from both techniques.

### The First Distributed Migration
Picojump is the first process migration system to boast a distributed migration
approach. Traditional approaches only consider process migration between two
machines: a source and a destination. Picojump is able to quickly migrate an
application within a cluster by enabling _partial migration_. That is, the job
scheduler does not move the entire process data at each migration. This makes
migration fast.

Picojump uses a new migration algorithm that is a hybrid of the two
state-of-the-art live migration approaches: Pre-copy and Post-copy. Like
Pre-copy, Picojump restores and application with any state that is clean in the
destination machine's cache. Like Post-copy, Picojump pulls dirty resources
lazily from the nearest remote machine.

### Transparent Resource Distribution

The brains of Picojump, the 'Manager', is an interposition layer between the
process and the underlying operating systems. Through the manager, applications
running on Picojump are unaware of whether or not the resource they request is
remote or local. It simply works. The manager is able to capture accesses to
remote resources in RPC's, and act as a cheap pass-through when resources are
local.

### Use Cases
- Load balancing
- NFS access
- Dynamic scaling (cloud pricing)
- Policy-driven resource access

### Acknowledgement
The authors are sponsored by NSF grant #<...>
