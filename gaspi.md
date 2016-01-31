---
layout: page
title: GASPI
permalink: /gaspi/
---
## Motivation
PGAS (Partitioned Global Address Space) programming models have been discussed as an alternative
to MPI for some time. The PGAS approach offers the developer an abstract shared address space which
simplifies the programming task and at the same time facilitates: data-locality, thread-based
programming and asynchronous communication. The goal of the GASPI project is to develop a suitable
programming tool for the wider HPC-Community by defining a standard with a reliable basis for future
developments through the PGAS-API of Fraunhofer ITWM. Furthermore, an implementation of the standard
as a highly portable open source library will be available. The standard will also define interfaces
for performance analysis, for which tools will be developed in the project. The evaluation of the
libraries is done via the parallel re-implementation of industrial applications up to and
including production status.

***

## Global Address Space Programming Interface(GASPI)

![alt text](https://raw.githubusercontent.com/GASPI-Forum/GASPI-Forum.github.io/master/images/gpi_overiew.png "GPI-Architecture - en.wikipedia.org")

<center>[GPI-Architecture - en.wikipedia.org](https://en.wikipedia.org/wiki/Global_Address_Space_Programming_Interface) </center>

GASPI stands for Global Address Space Programming Interface and
is a Partitioned Global Address Space (PGAS) API. It aims at
extreme scalability, high flexibility and failure tolerance for parallel
computing environments. GASPI aims to initiate a paradigm shift from bulk-synchronous two-sided
communication patterns towards an asynchronous communication and
execution model. To that end GASPI leverages remote completion and
one-sided RDMA driven communication in a Partitioned Global Address Space.

GASPI-1.0 was approved by the GASPI Consortium on June 14 in 2013 and
the current version GASPI-1.0.1 was released on November 14 in 2013.

[Download GASPI-1.0.1](https://raw.githubusercontent.com/GASPI-Forum/GASPI-Forum.github.io/master/standards/GASPI-1.0.1.pdf)

***

## GPI-2 - GASPI Implementation
The new release of GPI, GPI-2, provides users with an open source reference
implementation of the GASPI standard, providing an API for C and C++ codes that facilitates
scalable parallel applications. Further information about GPI-2 can be
found at the [Github page](https://github.com/cc-hpc-itwm/GPI-2) or [GPI Homepage](http://www.gpi-site.com)
that includes software download options and links to open source community sites for GPI development.

***

## Background

In the USA, the so-called "Multicore Challenge" for the software industry stimulated  the development
of new programming languages with Berkeley's UPC (Unified Parallel C) as focal point.
Another route was followed by Fraunhofer ITWM in Kaiserslautern, which developed a PGAS-API
subsequently deployed operationally by applications.
The PGAS (Partitioned Global Address Space) approach offers the developer an abstract shared
address space which simplifies the programming task and at the same time facilitates: data-locality,
thread-based programming and asynchronous communication.

The PGAS-API of the Fraunhofer-Institute ITWM (GPI, formerly Fraunhofer Virtual Machine, FVM)
has been developed since 2005. Since 2007 ITWM's industrial HPC projects have been exclusively
realised using GPI, replacing the use of MPI. The PGAS-API provides MPI developers with an easy
transition to a programming model which is geared for efficiency and scalability and which satisfies
 all of the abovementioned requirements.
