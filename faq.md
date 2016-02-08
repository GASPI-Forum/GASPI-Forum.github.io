---
layout: page
title: FAQ
permalink: /faq/
---

### General
Q: What is GASPI?  
A: GASPI is a Partitioned Global Address Space (PGAS) API. It aims at scalable, flexible and failure tolerant computing in massively parallel environments.  GASPI targets a paradigm shift from bulk-synchronous two-sided communication patterns towards an asynchronous communication and execution model. To that end GASPI leverages one-sided RDMA-driven communication with remote completion in a Partitioned Global Address Space. 
----
Q: Is GASPI ready for industrial applications?  
A: The GASPI specification originates from Fraunhofer ITWM’s PGAS API, GPI, whose development started in 2005. GPI offers an efficient, robust and scalable programming model which has been used in many of Fraunhofer ITWM’s industrial projects and in the meantime has completely replaced the use of MPI within the projects executed by the ITWM HPC Competence Centre.
GASPI vs. MPI
----
Q: How does GASPI differ from MPI?  
A: Similar to MPI, GASPI is an API for parallel computing on distributed memory architectures.and relies on SPMD/MPMD execution. Unlike MPI however, GASPI targets asynchronous data-flow implementations rather than bulk-synchronous message exchange.  Contrary to MPI, GASPI allows for a highly flexible configuration of the required resources and features low-level support for fault-tolerant execution.  
----
Q: Why should GASPI scale better than MPI? Isn’t one-sided communication in MPI-3.0 identical to the GASPI communication?  
A: GASPI is focused on an asynchronous data-flow model for distributed memory architectures. Whereas in MPI one-sided communication happens in specific time intervals (called an epoch), GASPI does not require such a concept. Rather, GASPI makes use of remote completion in the form of notifications.  Data may be written asynchronously whenever it is produced, accompanied by a corresponding notification, and data is processed locally whenever the required preconditions for processing this data can be met, i.e. whenever the corresponding notifications have been flagged. The GASPI notification mechanism hence allows for a highly scalable asynchronous data-flow model, which is not only able to provide a very fine-grained overlap of communication and computation, but is also very robust w.r.t. system jitter and noise. 
----
Q: MPI supports strided datatypes. Does the same apply to GASPI too?  
A: GASPI supports the notion of a write-list. Essentially, the application passes a list of local offsets to the GASPI API, along with a specific size for each of the offsets -- and another list with offsets and corresponding size for the remote receiver side. GASPI hence allows applications to write arbitrarily strided data from/to all nodes.
### GASPI vs. other PGAS approaches
Q: How does GASPI compare with other PGAS approaches?  
A: In contrast to other efforts in the PGAS community, GASPI is neither a new language (like e.g. Chapel from Cray), nor an extension to a language (like e.g. Co-Array Fortran). Instead -- very much in the spirit of MPI -- it complements existing languages like C/C++ or Fortran with a PGAS API which enables the application to leverage the concept of the Partitioned Global Address Space.  In contrast to, for example, OpenShmem or Global Arrays, GASPI is not limited to a single memory model, but provides configurable and yet globally accessible memory segments.  GASPI is interoperable with MPI and allows for incremental porting of legacy applications.
### Segments in GASPI
Q: What are segments in GASPI? How can I use them? How do they differ from MPI windows?  
A: The segments in GASPI are closely related to the MPI windows used in one-sided MPI communication. However, in GASPI these segments can be freely configured. This enables GASPI users to map the memory heterogeneity of a modern supercomputer node to GASPI segments. As examples, GASPI allows users to map the main memory of a GPGPU or Xeon Phi to a specific segment, to configure a GASPI segment per memory controller in a CC-NUMA system or to map non-volatile RAM to a specific segment. All these segments can directly read and write from/to each other – within the node and across all nodes. 
----
Q: How could I use the configurable memory segments in GASPI? What are the use-cases for this?  
A: The most frequent current use-case is to exploit the possibility to directly read and write from/to the GPGPU/Xeon Phi memory of other nodes. Essentially, GASPI allows the application to generate two different partitioned global address spaces for this case, with one address space across the local memory of the GPGPU/Xeon Phi and one address space across the main memory of the node. How much of this memory is reserved for this global address space (and how many memory segments are created) is left to the application programmer. 
----
Q: Are there other uses for the GASPI segments, apart from access to remote memory on heterogeneous architectures?  
A: GASPI memory segments allow different memory management systems to co-exist, e.g. to allow for a symmetric distributed global memory management (like OpenShmem) in one GASPI segment and an asymmetric (e.g. stack based) distributed global memory management in another GASPI segment. 
GASPI can also leverage the segments to allow for a global tight coupling of multiple applications, e.g. a multiphysics solver. While every solver then uses its own memory segment, the GASPI API allows the solvers to directly read and write from/to the partitioned global memory of all the other solvers involved in the computation.  In contrast to MPI, the GASPI node set is not static. In principle, GASPI thus allows the application to start yet another process group for the online evaluation of the simulation and  to expose the memory segments of the running multiphysics solver to the evaluation process group.
Passive communication in GASPI
----
Q: What is passive communication in GASPI – how do I use that?  
A: Passive communication in GASPI is most closely comparable with a non-time critical active message. Essentially, passive communication triggers an arbitrary user-defined remote action on the receiver side by waking up a remote thread through a trigger in the network adapter.  Passive communication typically happens out-of-sync with the rest of the application workflow. Examples of use are: to re-distribute the work, to log status messages, to check the load status of a remote node.
### Groups
Q: GASPI features groups. What is the purpose of the GASPI groups?  
A: Groups can be used to execute collective calls or to synchronize subgroups of processes. As such, groups are closely related to the MPI communicator concept
Collectives in GASPI
----
Q: How do the asynchronous GASPI collectives work?  
A: The GASPI collectives rely on time-based blocking with a user-defined timeout. If this timeout takes the form of a mere test, the collectives perform minimal progress (e.g. completing one of the local communication phases) and then return. Hence, in order to complete the collective, the collective has to be called several times. The collectives in GASPI are thus designed to achieve a maximum of progress within the call and a minimal number of communication stages. Unless the network provides support for the collective, GASPI does not achieve progress outside of the call. 
----
Q: How do I use these asynchronous collectives in GASPI?  
A: Typically, the corresponding implementation in GASPI would assume the form: 
while (barrier()  != complete) work();  
If such an implementation is not possible (e.g. because work() cannot be divided into subparts), we advise the use of a dedicated thread in order to achieve the required progress for the collective. 
----
Q: The set of collectives in GASPI appears to be somewhat limited - why is that?  
A: Bulk-synchronous communication does not scale.  An application-specific asynchronous gather/scatter collective which overlaps communication with computation in an application-specific way is not only easy to implement in GASPI but in most cases would outperform the generic implementation that GASPI might offer. Since we also want to keep the GASPI interface as small as possible we have deliberately removed these collectives from the GASPI API.
Global Atomic Operations
----
Q: Does GASPI support global atomics?  
A: Yes. The global atomic operations in GASPI allow users to apply low-level functions, for example compare-and swap or add-and-fetch, to all data in the GASPI memory segments.
### Failure tolerance
Q: GASPI is supposed to be failure tolerant. What does this entail?  
A: GASPI is failure tolerant in the sense that all non-local operations feature a timeout with a defined exit status. GASPI maintains a status vector with the current node status. If an error occurs and a node is lost, it is possible to either reduce the GASPI node set or to request a new node, e.g. from a pool of spare nodes. Application-level steps are nevertheless necessary, e.g. actual initialization from a previously written checkpoint.  GASPI does not feature a fully automatic handling of failure tolerance via, for example, checkpoint/restart; GASPI does provide a minimal set of the required low-level functions.
### Interoperabilty with MPI
Q: Do I need to port my MPI application completely to GASPI or is there an incremental way?  
A: GASPI is interoperable with MPI so that porting an application can indeed be done step-by-step. 
----
Q: Can I inter-mix GASPI communication and MPI communication in my application?  
A: In principle, yes. There are restrictions, however: Calls to either one (but only one!) of the two communication libs can be done within a complete epoch. A complete epoch starts (and ends) with a full synchronization of the application’s processes/threads, such that there is no overlap of GASPI communication and MPI communication. 
----
Q: My code has a single-level parallelization using MPI, i.e., the code is single-threaded. Can I still use GASPI to improve parallelization of the code?  
A: GASPI’s 1-sided communication model may in some situations be used to overcome some limitations of message-based communication using MPI in a single-threaded application. In general, to exploit the full potential of GASPI, the application should be ported to a multi-threaded programming model.  In particular, a GASPI implementation might only support communication via the RDMA engine of the network interface. Since all node-local communication then is routed via the network device, multi-threading would become mandatory in order to make optimal use of all cores of a NUMA node.
Topologies
----
Q: How are topologies mapped in GASPI?  
A: Topologies are mapped via the machinefile used at startup. How the GASPI ranks are assigned to the nodes is described via the machinefile. Beyond this simple mechanism GASPI does not support specific topologies. 
Libraries
----
Q: Will there be numerical kernels available to use with GASPI?  
A: Together with the final version of the current GASPI implementation numerical kernels will be made available which make local calls to the architecture-optimized BLAS kernels and are globally based on communication via GASPI. There will be different kernels available dependent on whether dense or sparse matrices are involved.
### Performance Analysis & Debugging
Q: How do I debug a GASPI application?  
A: This depends on the particular GASPI implementation and installation. The GPI 2.0 implementation, for instance, provides a debug version of the library which can be used together with gdb. In addition, a tool is under development that targets the specific debugging requirements of PGAS applications. This tool analyzes and visualizes the memory access structure of the application and spots errors like data race conditions 
----
Q: Which means do I have to analyze and optimize a GASPI application?   
A: GASPI provides a rich and extensible profiling interface. The simplest option is to use the built-in statistics counters, which can provide an insight on data transfer volumes and the time consumed by various transactions. For a more detailed view of the application performance, GASPI has defined the pGASPI event tracing interface which is similar to pMPI. This interface is already supported by the Score-P infrastructure, thus tools like Vampir can be used to investigate the time-behavior of a GASPI application.
----

