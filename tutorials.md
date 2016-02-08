---
layout: page
title: FAQ
permalink: /tutorial/
---

## Tutorial

#### GASPI Execution model

* GASPI features a SPMD / MPMD style of execution. 
* All GASPI procedures come with a prefix gaspi\_ 
* All procedures have a return value.
* All potentially blocking procedures have a timeout mechanism.

#### Error handling

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

#### GASPI\_TIMEOUT: A timeout mechanism for potentially blocking procedures  

- Procedure is guaranteed to return

Timeout: gaspi\_timeout\_t  

- GASPI\_TEST (Value  = 0)
  - procedure completes local operations
  - Procedure does not wait for data from other processes
- GASPI\_BLOCK (Value = -1)
  - wait indefinitely (blocking)
- Value > 0
  - Maximum time in msec the procedure is going to wait for data from other ranks to make progress. Does not equal hard execution time


#### Process management

##### gaspi_proc_init

- initialization of resources
  - set up of communication infrastructure if requested
  - set up of default group GASPI_GROUP_ALL
- rank assignment
  - position in machinefile corresponds to rank ID
  - no default segment creation

##### gaspi_proc_term

- clean up
  - wait for outstanding communication to be finished
  - release resources
- not a collective operation !

#### Hello world


{% highlight c %}
{% include_relative _source/helloworld.c %}
{% endhighlight %}


#### 

{% highlight c %}
{% include_relative _source/success_or_die.h %}
{% endhighlight %}


