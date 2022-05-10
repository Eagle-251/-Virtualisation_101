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

| Namespace                  | Function                                                                                                                                                                                                                                                  |
| -------------------------- | --------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------- |
| Mount                      | Similar to `chroot`, this namespace creates a new tree of mountpoints where only access to mountpoints is unique to each namespace.                                                                                                                       |
| Process                    | The purpose of the PID namespace is to give processes within that a seperate PID tree. This includes the PID 1 process. PIDs are relative from within this tree but globally will have unique PIDs.                                                       |
| Network                    | Network Namespaces create unique network stacks for the Network Interfaces within that namespace. Each namespace has it's own unique routing table, IP addresses, firewall etc.                                                                           |
| Unix Timesharing System    | A namespace mainly designed to allow processes to use different namespaces.                                                                                                                                                                               |
| User                       | A user namespace gives the ability to map namespace UIDs to system UIDs. For example you could give a user in a namespace the UID of 0 (root) but in actual fact that user be a totally diffrent user from the system's perspective.                      |
| Interprocess Communication | Faciltates creating namespaces to isolate programs abilities to access shared memory.                                                                                                                                                                     |
| Cgroups                    | This namespace further isloates control groups from each other. Cgroup namespaces use the existing directory strucure in use for the Cgroup as a root filesystem. This prevents other cgroups from accessing resources outside of their Cgroup namespace. |

### Containers

Containerisation is a way of isolating applications from the other applications running on the host. This is advantageous for many reasons:

- Containers make if easier for developers to safely maintain specific dependencies for their program.
- Containers can be immutable, in that any upstream changes result in redeployment of the container rather than editing the container in place.

#### Userspace vs Kernelspace

The distinction between User space and Kernelspace is very important when it comes to containers. It is necessary first to clarify these terms:

- Kernel Space:
  - This consists of all components of the kernel. The only way for programs and libraries to interact with kernel space is via system calls.
- User Space:
  - User space consists of all non-kernel files and programs. This includes all operating system programs and libraries.

Containers rely on this distinction between user space and kernel space as a containers user space exists exclusively within the mount namespace occupied by the container.

## Virtual Machine Implementations

### Proxmox

An example of a popular Type 1 (Bare Metal) Hypervisor is _Proxmox_.

Proxmox is a Open Source bare metal hypervisor that is based on Debian Linux. It provides the admin the ability to provision Vitual Machines remotely via either a Web Interface or a REST API.

It also has support to run certain applications as _appliances_, which means a virtual machine packaged with only the necessary libraries for the application to run.

### VMware ESXi

ESXi is another Type 1 hypervisor that runs on Bare Metal. Unlike Proxmox however it is not open source.

## Container Technologies

### Docker

Docker is an open source container platform. It consists of a daemon that manages the interaction between Docker containers and the host, provisions the necessary namespaces and cgroups for each container and controls the lifecycle of each container.

### Images

An image in Docker is defined in a Dockerfile, which is a series of build commands that run various operations on files, git repositories or other images resulting in a predictable result.
Images consist of various filesystem layers, which are unique namespaces, corresponding to each stage of the build process:
