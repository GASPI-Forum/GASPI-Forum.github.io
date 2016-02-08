---
layout: page
title: FAQ
permalink: /tutorial/
---

## Basic Tutorial

#### 1. GASPI Execution model

* GASPI features a SPMD / MPMD style of execution. 
* all GASPI procedures come with a prefix gaspi\_ 
* all procedures have a return value.
* all potentially blocking procedures have a timeout mechanism.

#### 2. Error handling

Procedure return values:  

- GASPI\_SUCCESS
  - designated operation successfully completed
- GASPI\_TIMEOUT
  - designated operation could not be finished in the given period of time
  - not necessarily an error
  - the procedure has to be invoked subsequently in order to fully complete the designated operation
- GASPI\_ERROR
  - designated operation failed -> check error vector
  - advice: Always check return value !

#### 3. GASPI\_TIMEOUT: A timeout mechanism for potentially blocking procedures  

- Procedure is guaranteed to return

Timeout: gaspi\_timeout\_t  

- GASPI\_TEST (Value  = 0)
  - procedure completes local operations
  - procedure does not wait for data from other processes
- GASPI\_BLOCK (Value = -1)
  - wait indefinitely (blocking)
- value > 0
  - Maximum time in msec the procedure is going to wait for data from other ranks to make progress. Does not equal hard execution time

#### 4. Process management

##### gaspi\_proc\_init

- initialization of resources
  - set up of communication infrastructure if requested
  - set up of default group GASPI\_GROUP\_ALL
- rank assignment
  - position in machinefile corresponds to rank ID
  - no default segment creation

##### gaspi\_proc\_term

- clean up
  - wait for outstanding communication to be finished
  - release resources
- not a collective operation !

#### 5. Hello world

{% highlight c %}
{% include_relative _source/helloworld.c %}
{% endhighlight %}

#### 6. A wrapper function for minimal error handling

{% highlight c %}
{% include_relative _source/success_or_die.h %}
{% endhighlight %}

#### 7. Segments

- software abstraction of hardware memory hierarchy
  - NUMA
  - GPU
  - Xeon Phi
- a single partition of the Partitioned Global Address Space (PGAS)
  - contiguous block of virtual memory
  - no pre-defined memory model
- memory management up to the application
  - locally / remotely accessible
  - local access by ordinary memory operations
  - remote access by GASPI communication routines

![gaspi segments]({{ site.baseurl }}/images/segments.png "Segments in GASPI")

GASPI provides only a few relatively large segments

- segment allocation is expensive
- the total number of supported segments is limited by hardware constraints

GASPI segments have an allocation policy

- GASPI\_MEM\_UNINITIALIZED
  - memory is not initialized
- GASPI\_MEM\_INITIALIZED
  - memory is initialized (zeroed)

##### gaspi\_segment\_create

- collective short cut to
  - gaspi\_segment\_alloc (for more details - see GASPI specification)
  - gaspi\_segment\_register (for more details - see GASPI specification)

After successful completion, the segment is locally and remotely accessible by all ranks in the group.

{% highlight c %}
{% include_relative _source/segments.c %}
{% endhighlight %}


#### 8. GASPI One-sided Communication


One sided-communication:

- entire communication managed by the local process only
- remote process is not involved
- advantage: no inherent synchronization between the local and the remote process in every communication request
- still: At some point the remote process needs knowledge about data availability
- managed by weak synchronization primitives

#### 9. Queues in GASPI

##### gaspi\_wait

- wait on local completion of all requests in a given queue
- after successfull completion, all involved local buffers are valid

Different queues available to handle the communication requests
- requests to be submitted to one of the supported queues
- advantages
- more scalability
- channels for different types of requests
- similar types of requests are queued and synchronized together but independently from other ones
- separation of concerns

Fairness of transfers posted to different queues is guaranteed

- no queue should see ist communication requests delayed indefinitely
- a queue is identified by its ID
- synchronization of calls by the queue
- queue order does not imply message order on the network / remote memory
- a subsequent notify call is guaranteed to be nonovertaking for all previous posts to the same queue and rank.


