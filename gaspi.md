---
layout: page
title: GASPI
permalink: /gaspi/
redirect_from: "/en/project.html"
---

### Motivation

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

### Global Address Space Programming Interface(GASPI)

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
the current version GASPI-16.6 was released in June 2016.

- [Download GASPI-16.6](https://raw.githubusercontent.com/GASPI-Forum/GASPI-Forum.github.io/master/standards/GASPI-16.6.pdf)

***

### GPI-2 - GASPI Implementation

The new release of GPI, GPI-2, provides users with an open source reference
implementation of the GASPI standard, providing an API for C and C++ codes that facilitates
scalable parallel applications. Further information about GPI-2 can be
found at the [Github page](https://github.com/cc-hpc-itwm/GPI-2) or [GPI Homepage](http://www.gpi-site.com)
that includes software download options and links to open source community sites for GPI development.

- [Download GPI-2](https://github.com/cc-hpc-itwm/GPI-2)

***

### Background

In the USA, the so-called "Multicore Challenge" for the software industry stimulated  the development
of new programming languages with Berkeley's UPC (Unified Parallel C) as focal point.
Another route was followed by Fraunhofer ITWM in Kaiserslautern, which developed a PGAS-API
subsequently deployed operationally by applications.
The PGAS (Partitioned Global Address Space) approach offers the developer an abstract shared
address space which simplifies the programming task and at the same time facilitates: data-locality,
thread-based programming and asynchronous communication.

***

### History

The PGAS-API of the Fraunhofer-Institute ITWM (GPI, formerly Fraunhofer Virtual Machine, FVM)
has been developed since 2005. Since 2007 ITWM's industrial HPC projects have been exclusively
realised using GPI, replacing the use of MPI. The PGAS-API provides MPI developers with an easy
transition to a programming model which is geared for efficiency and scalability and which satisfies
 all of the abovementioned requirements.

In 2011 the partners of Fraunhofer ITWM, Fraunhofer SCAI, TUD, T-Systems SfR, 
DLR, KIT, FZJ, DWD and Scapos have initiated and launched the
GASPI project to define a novel specification for a PGAS API (GASPI, based
on GPI) and to make this novel GASPI specification a reliable, scalable and
universal tool for the HPC community, The project was funded by the 
[German Ministry of Science and Education (BMBF)](https://gauss-allianz.de/de/project/title/GASPI).

In 2013 the Fraunhofer-Gesellschaft awarded the [Joseph-von-Fraunhofer prize](http://www.fraunhofer.de/en/press/research-news/2013/june/programming-model-for-supercomputers-of-the-future.html) for the programming model of GPI.

***

### Related work

The PGAS API of [OpenSHMEM](http://openshmem.org) is most closely related to GASPI. 
Similar to communication in GASPI, the data transfer in OpenSHMEM is one-sided in nature.
OpenSHMEM supports weak synchronizations (notifications) via fenced message ordering,
but today only allows for a single fence per pair of processing elements (PE). 

In contrast GASPI supports a more general concept of (named) notifications, which allows for 
message ordering (and corresponding weak synchronization) per data context.
