---
title: "HPC Systems"
teaching: 20
exercises: 0
questions:
- "What are the drivers for HPC?"
- "What does an HPC look like?"
- "Are all compute nodes alike?"
objectives:
- "Understand high level architecture of HPC systems"
- "Understand basic compute node architecture"
- "Compare & contrast resources on the local machine and HPC compute nodes"
keypoints:
- "An HPC system is a set of networked machines."
- "HPC systems typically provide login nodes and a set of compute nodes."
- "The resources found on independent (worker) nodes can vary in volume and
  type (amount of RAM, processor architecture, availability of network mounted
  filesystems, etc.)."
- "Files saved on shared storage are available on all nodes."
- "The login node is a shared machine: be considerate of other users."
---

## Drivers for HPC

- Scientific simulation and modelling drive the need for greater computing power
- Single-core processors can not be made that have enough resource for the simulations needed
- Making processors with faster clock speeds is difficult due to cost and power/heat limitations
- Expensive to put huge memory on a single processor

Solution: parallel computing – divide up the work among numerous linked systems.

## What does an HPC system look like?

{% include figure.html url="" max-width="40%"
   file="/fig/archer2_architecture.png"
   alt="HPC system architecture" caption="" %}


Most high-performance computing systems run the Linux operating system, which
is built around the UNIX [Filesystem Hierarchy Standard][fshs]. Instead of
having a separate root for each hard drive or storage medium, all files and
devices are anchored to the "root" directory, which is `/`.

## Accessing HPC 

Access to remote HPC systems is typically achieved via a *command line interface (CLI)*
rather than a graphical interface. Though some HPC systems may also provide ways to connect
in a graphical way or through a web browser. 

In most cases, you will connect using SSH software, often from a terminal program.

## Using HPC filesystems

On HPC systems, you have a number of places where you can store your files.
These differ in both the amount of space allocated and whether or not they
are backed up.

- __Home__ -- often a _network filesystem_, data stored here is available
  throughout the HPC system, and often backed up periodically. Files stored
  here are typically slower to access, the data is actually stored on another
  computer and is being transmitted and made available over the network!
- __Scratch__ -- typically faster than the networked Home directory, but not
  usually backed up, and should not be used for long term storage.
- __Work__ -- sometimes provided as an alternative to Scratch space, Work is
  a fast file system accessed over the network. Typically, this will have
  higher performance than your home directory, but lower performance than
  Scratch; it may not be backed up. It differs from Scratch space in that
  files in a work file system are not automatically deleted for you: you must
  manage the space yourself.

## Nodes

Individual computers that compose a cluster are typically called _nodes_
(although you will also hear people call them _servers_, _computers_ and
_machines_). On a cluster, there are different types of nodes for different
types of tasks. The node where you are right now is called the _login node_,
_head node_, _landing pad_, or _submit node_. A login node serves as an access
point to the cluster.

As a gateway, the login node should not be used for time-consuming or
resource-intensive tasks. You should be alert to this, and check with your
site's operators or documentation for details of what is and isn't allowed. It
is well suited for uploading and downloading files, setting up software, and
running tests. Generally speaking, in these lessons, we will avoid running jobs
on the login node.

Who else is logged in to the login node?

```
{{ site.remote.prompt }} who
```
{: .language-bash}

> ## Dedicated Transfer Nodes
>
> If you want to transfer larger amounts of data to or from the cluster, some
> systems offer dedicated nodes for data transfers only. The motivation for
> this lies in the fact that larger data transfers should not obstruct
> operation of the login node for anybody else. Check with your cluster's
> documentation or its support team if such a transfer node is available. As a
> rule of thumb, consider all transfers of a volume larger than 500 MB to 1 GB
> as large. But these numbers change, e.g., depending on the network connection
> of yourself and of your cluster or other factors.
{: .callout}

The real work on a cluster gets done by the _compute_ (or _worker_) _nodes_.
compute nodes come in many shapes and sizes, but generally are dedicated to long
or hard tasks that require a lot of computational resources.

All interaction with the compute nodes is handled by a specialized piece of
software called a scheduler (the scheduler used in this lesson is called
{{ site.sched.name }}). We will discuss the scheduler and how it works in the 
next section.

For example, we can view all of the compute nodes by running the command
`{{ site.sched.info }}`.

```
{{ site.remote.prompt }} {{ site.sched.info }}
```
{: .language-bash}

{% include {{ site.snippets }}/cluster/queue-info.snip %}

A lot of the nodes are busy running work.

There are also specialised machines used for managing disk storage, user
authentication, and other infrastructure-related tasks. Although we do not
typically logon to or interact with these machines directly, they enable a
number of key features like ensuring our user account and files are available
throughout the HPC system.

## What's in a Node?

All of the nodes in an HPC system have the same components as your own laptop
or desktop: _CPUs_ (sometimes also called _processors_ or _cores_), _memory_
(or _RAM_), and _disk_ space. It is also becoming more common for HPC compute
nodes to have _GPUs_ (Graphics Processing Units) which are used to accelerate 
computation or memory intensive work (rather than process graphics...).

CPUs are a computer's tool for actually running programs and calculations (and driving
GPUs, if they are present). Information about a current task is stored in the
computer's memory. Disk refers to all storage that can be accessed like a file
system. This is generally storage that can hold data permanently, i.e. data is
still there even if the computer has been restarted. While this storage can be
local (a hard drive installed inside of it), it is more common for nodes to
connect to a shared, remote fileserver or cluster of servers.

{% include figure.html url="" max-width="40%"
   file="/fig/node_anatomy.png"
   alt="Node anatomy" caption="" %}

{% include {{ site.snippets }}/cluster/specific-node-info.snip %}

> ## Compare Your Computer, the Login Node and the Compute Node
>
> Compute nodes are usually built with processors that have _higher
> core-counts_ than the login node or personal computers in order to support
> highly parallel tasks. Compute nodes usually also have substantially _more
> memory (RAM)_ installed than a personal computer. More cores tends to help
> jobs that depend on some work that is easy to perform in _parallel_, and
> more, faster memory is key for large or _complex numerical tasks_.
{: .callout}

> ## Differences Between Nodes
>
> Many HPC clusters have a variety of nodes optimized for particular workloads.
> Some nodes may have larger amount of memory, or specialised resources such as
> GPUs.
{: .callout}

{% include links.md %}

[fshs]: https://en.wikipedia.org/wiki/Filesystem_Hierarchy_Standard
[mount]: https://en.wikipedia.org/wiki/Mount_(computing)
