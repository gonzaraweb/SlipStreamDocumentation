
# Prerequisites

The SlipStream™ server has a standard web application architecture,
with the server running within a Java web application container and
using an SQL database for storing application data.  Consequently,
many administrators will be familiar with its configuration needs and
modest resource requirements.

## Cloud Requirements

SlipStream™ is a cloud deployment engine and requires access to at
least one supported cloud infrastucture.  See the list of supported
clouds and conditions on the associated cloud plugin to select the
cloud(s) you will be making available through SlipStream™.

## Software Requirements

Although the software will build and run on most *nix-like platforms,
the software is tested, supported, and packaged for CentOS 6 systems.
The packages should install and run correctly on any similar operating
system.

## Hardware Requirements

The resource requirements of SlipStream™ are modest.  Any modern,
multicore, server-class machine should run the server without
problems.  At least 2 gigabytes of RAM should be dedicated to the Java
Virtual Machine (JVM) running the service.  Similarly 10-20 GB of disk
space should be sufficient for initial use.

As with any service, the resource requirements for the server will
increase with the number of users and with the number of system
deployments.  Increase the above resources if you expect particularly
large scale or intense use of the service.

## Networking Requirements

The server uses the standard HTTPS port (443).  Consequently, any
firewalls on the machine or site must allow access to this port from
the users' machines.

The server must also have access to the underlying cloud service
endpoints.  The ports used vary depending on the cloud infrastructures
that is being accessed.

If the underlying cloud infrastructure does not support virtual
machine contextualization, direct SSH access (port 22) to virtual
machines within the cloud is also required.

