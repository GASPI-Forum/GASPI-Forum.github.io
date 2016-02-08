---
layout: page
title: FAQ
permalink: /tutorial/
---

## Basic Tutorial

#### 1. GASPI Execution model

* GASPI features a SPMD / MPMD style of execution. 
* All GASPI procedures come with a prefix gaspi\_ 
* All procedures have a return value.
* All potentially blocking procedures have a timeout mechanism.

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
  - Advice: Always check return value !

#### 3. GASPI\_TIMEOUT: A timeout mechanism for potentially blocking procedures  

- Procedure is guaranteed to return

Timeout: gaspi\_timeout\_t  

- GASPI\_TEST (Value  = 0)
  - procedure completes local operations
  - Procedure does not wait for data from other processes
- GASPI\_BLOCK (Value = -1)
  - wait indefinitely (blocking)
- Value > 0
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

- Software abstraction of hardware memory hierarchy
  - NUMA
  - GPU
  - Xeon Phi
- A single partition of the Partitioned Global Address Space (PGAS)
  - contiguous block of virtual memory
  - no pre-defined memory model
- Memory management up to the application
  - locally / remotely accessible
  - local access by ordinary memory operations
  - remote access by GASPI communication routines

GASPI provides only a few relatively large segments
- segment allocation is expensive
- the total number of supported segments is limited by hardware constraints

GASPI segments have an allocation policy
- GASPI\_MEM\_UNINITIALIZED
  - memory is not initialized
- GASPI\_MEM\_INITIALIZED
  - memory is initialized (zeroed)

##### gaspi\_segment\_create

- Collective short cut to
  - gaspi\_segment\_alloc (for more details - see GASPI specification)
  - gaspi\_segment\_register (for more details - see GASPI specification)

After successful completion, the segment is locally and remotely accessible by all ranks in the group.

{% highlight c %}
{% include_relative _source/segments.c %}
{% endhighlight %}
