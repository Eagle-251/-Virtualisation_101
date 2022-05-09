# Virtualisation

Virtualisation has become an indispensable tool in modern IT infrastructure and DevOps workflows. This document is designed to explore the various technologies to deepen my understanding of how they work.

## Essential Concepts

### Hypervisors

The term hypervisor is an extension of the term supervisor, which in Linux refers to the kernel which is the _supervisor_ of the system. A hypervisor then is a supervisors of supervisors.
In essence, the hypervisors job to control, delegate and isolate the hardware system resources between Virtualised OSes.
There are two basic types of Hypervisors:
- Type 1:
  - Runs at the Hardware level
  - Has full control of the Hardware
  - All operating systems are Virtualised in this context.
  - Examples Inlcude:
    - KVM
    - VMWare
- Type 2:
  - Run as a privileged application on an exisiting host OS
  - Examples Include:
      - QEMU
      - Virtual Box

### CGroups

CGroups (Control Croups) are a way to limit the resources allocated to, and accessible by, a given program or process. CGroups can cover usage of CPU, Memory, disk I/O, network and more.
The use of CGroups is particularly when seeking containerise a program, as it gives the admin/system a way to make sure resources are distributed evenly and not allow any one resource hungry application disrupt others.
Most container systems rely heavily on CGroups for their basic function.

The CGroups functionality has been part of the Linux MainlineKernel since 2007 in version 2.6.24.

### Namespaces

Linux namespaces are similar to cgroups but instead of limits on system resource use, namespaces create a dedicated tree of resources that are unique to that application.
There are many different kinds of namespaces all designed to solve different needs. As of Linux 5.6 there are 7 different kinds of namespaces:

| Namespace | Function |
| --- | --- |
| Mount   | Similar to `chroot`, this namespace creates a new tree of mountpoints where only access to mountpoints is unique to each namespace.   |
| Process | The purpose of the PID namespace is to give processes within that a seperate PID tree. This includes the PID 1 process. PIDs are relative from within this tree but globally will have unique PIDs.   |
| Network | Network Namespaces create unique network stacks for the Network Interfaces within that namespace. Each namespace has it's own unique routing table, IP addresses, firewall etc. |
|  Unix Timesharing System | A namespace mainly designed to allow processes to use different namespaces.   |
| User | A user namespace gives the ability to map namespace UIDs to system UIDs. For example you could give a user in a namespace the UID of 0 (root) but in actual fact that user be a totally diffrent user from the system's perspective.   |
| Interprocess Communication | Faciltates creating namespaces to isolate programs abilities to access shared memory. |
| Cgroups   | This namespace further isloates control groups from each other. Cgroup namespaces use the existing directory strucure in use for the Cgroup as a root filesystem. This prevents other cgroups from accessing resources outside of their Cgroup namespace.  |
