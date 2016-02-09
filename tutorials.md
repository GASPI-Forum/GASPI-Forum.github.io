---
layout: page
title: TUTORIAL
permalink: /tutorial/
---

### Basic Tutorial

#### 1. GASPI Execution model

* GASPI features a SPMD / MPMD style of execution. 
* all GASPI procedures come with a prefix gaspi\_ 
* all procedures have a return value.
* all potentially blocking procedures have a timeout mechanism.

#### 2. Error handling

Procedure return values:  

```c
  typedef enum
  {
    GASPI_ERROR   = -1,
    GASPI_SUCCESS =  0,
    GASPI_TIMEOUT =  1
  }
```
- GASPI\_ERROR
  - designated operation failed -> check error vector
  - advice: Always check return value !
- GASPI\_SUCCESS
  - designated operation successfully completed
- GASPI\_TIMEOUT
  - designated operation could not be finished in the given period of time
  - not necessarily an error
  - the procedure has to be invoked subsequently in order to fully complete the designated operation


#### 3. GASPI\_TIMEOUT - a timeout mechanism for potentially blocking procedures  

Timeout: gaspi\_timeout\_t  

- GASPI\_TEST ( value = 0 )
  - procedure completes local operations
  - procedure does not wait for data from other processes
- GASPI\_BLOCK ( value = -1 )
  - wait indefinitely (blocking)
- value > 0
  - Maximum time in msec the procedure is going to wait for data from other ranks to make progress. Does not equal hard execution time

- procedure is guaranteed to return

#### 4. Process management

##### gaspi\_proc\_init

```c
gaspi_return_t
gaspi_proc_init ( gaspi_timeout_t const timeout )
```

- initialization of resources
  - set up of communication infrastructure if requested
  - set up of default group GASPI\_GROUP\_ALL
- rank assignment
  - position in machinefile corresponds to rank ID
  - no default segment creation

##### gaspi\_proc\_term

```c
gaspi_return_t
gaspi_proc_term ( gaspi_timeout_t timeout )
```

- clean up
  - wait for outstanding communication to be finished
  - release resources
- not a collective operation !

##### gaspi\_proc\_rank

```c
gaspi_return_t
gaspi_proc_rank ( gaspi_rank_t *rank )
```

- process/rank identification, returns rank id.

##### gaspi\_proc\_num

```c
gaspi_return_t
gaspi_proc_num ( gaspi_rank_t *proc_num )
```

- returns total number of processes/ranks, 

#### 5. Hello world

{% highlight c %}
{% include_relative _source/helloworld.c %}
{% endhighlight %}

{% highlight c %}
{% include_relative _source/success_or_die.h %}
{% endhighlight %}

#### 6. Segments

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

```c
gaspi_return_t
gaspi_segment_create ( gaspi_segment_id_t segment_id
                     , gaspi_size_t size
                     , gaspi_group_t group
                     , gaspi_timeout_t timeout
                     , gaspi_alloc_t alloc_policy )
```

- collective short cut to
  - gaspi\_segment\_alloc (for more details - see GASPI specification)
  - gaspi\_segment\_register (for more details - see GASPI specification)

After successful completion, the segment is locally and remotely accessible by all ranks in the group.

{% highlight c %}
{% include_relative _source/segments.c %}
{% endhighlight %}

#### 7. Queues in GASPI

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


##### gaspi\_wait

```c
gaspi_return_t
gaspi_wait ( gaspi_queue_id_t queue
           , gaspi_timeout_t timeout )
```

- wait on local completion of all requests in a given queue
- after successfull completion, all involved local buffers are valid


#### 8. GASPI One-sided Communication

One sided-communication:

- entire communication managed by the local process only
- remote process is not involved
- advantage: no inherent synchronization between the local and the remote process in every communication request
- still: At some point the remote process needs knowledge about data availability
- managed by weak synchronization primitives

Several notifications for a given segment

- identified by notification ID
- logical association of memory location and notification

##### gaspi\_write\_notify

```c
gaspi_return_t
gaspi_write_notify ( gaspi_segment_id_t segment_id_local
                   , gaspi_offset_t offset_local
                   , gaspi_rank_t rank
                   , gaspi_segment_id_t segment_id_remote
                   , gaspi_offset_t offset_remote
                   , gaspi_size_t size
                   , gaspi_notification_id_t notification_id
                   , gaspi_notification_t notification_value
                   , gaspi_queue_id_t queue
                   , gaspi_timeout_t timeout )
```

- post a put request into a given queue for transfering data from a local segment into a remote segment
- posts a notification with a given value to a given queue
- remote visibility guarantees remote data visibility of all previously posted writes in the same queue, the same segment and the same process rank


##### gaspi\_notify\_waitsome

```c
gaspi_return_t
gaspi_notify_waitsome ( gaspi_segment_id_t segment_id
                      , gaspi_notification_id_t notific_begin
                      , gaspi_number_t notification_num
                      , gaspi_notification_id_t *first_id
                      , gaspi_timeout_t timeout )
```

- monitors a contiguous subset of notification id‘s for a given segment
- returns successfull if at least one of the monitored id‘s is remotely updated to a value unequal zero

##### gaspi\_notify\_reset

```c
gaspi_return_t
gaspi_notify_reset ( gaspi_segment_id_t segment_id
                   , gaspi_notification_id_t notification_id
                   , gaspi_notification_t *old_notification_val)

```

- atomically resets a given notification id and yields the old value


#### 9. Communication Example

Round robin communicaiton with gaspi\_write\_notify and gaspi\_waitsome.

![gaspi segments]({{ site.baseurl }}/images/roundrobin.png "Notified communication in GASPI")

- init local buffer
- write to remote buffer
- wait for data availability
- print result

{% highlight c %}
{% include_relative _source/onesided.c %}
{% endhighlight %}

{% highlight c %}
{% include_relative _source/assert.h %}
{% endhighlight %}

{% highlight c %}
{% include_relative _source/waitsome.h %}
{% endhighlight %}

{% highlight c %}
{% include_relative _source/waitsome.c %}
{% endhighlight %}

### Advanced Tutorial

#### 10. Pipelined Matrix Transpose

- highlights interoperability between GASPI and MPI
- demonstrates how to use notified communication for improved scalability.

Global transpose of a matrix for a column-based matrix distribution. 

- hybrid implementation with global transpose followed by local transpose.
- required communication to all target ranks is issued in single communication step. 

- [Download Pipelined Matrix Transpose](https://github.com/PGAS-community-benchmarks/Pipelined-Transpose)

{% highlight c %}
{% include_relative _source/Pipelined_transpose.c %}
{% endhighlight %}




