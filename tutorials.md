---
layout: page
title: FAQ
permalink: /tutorial/
---

## Tutorial

### GASPI Execution model

* GASPI features a SPMD / MPMD style of execution. 
* All GASPI procedures come with a prefix gaspi_ 
* All procedures have a return value.
* All potentially blocking procedures have a timeout mechanism.

#### Error handling

Procedure return values:
* GASPI\_SUCCESS
  * designated operation successfully completed
* GASPI\_TIMEOUT
  * designated operation could not be finished in the given period of time
  * not necessarily an error
  * the procedure has to be invoked subsequently in order to fully complete the designated operation
* GASPI\_ERROR
  * designated operation failed -> check error vector
  * Advice: Always check return value !

Mechanism for potentially blocking procedures
– procedure is guaranteed to return
• Timeout: gaspi_timeout_t
– GASPI_TEST (0)
• procedure completes local operations
• Procedure does not wait for data from other processes
– GASPI_BLOCK (-1)
• wait indefinitely (blocking)
– Value > 0
• Maximum time in msec the procedure is going to wait for data
from other ranks to make progress
• != hard execution time

{% highlight c %}
{% include_relative _source/helloworld.c %}
{% endhighlight %}

#### Process management

{% highlight c %}
{% include_relative _source/success_or_die.h %}
{% endhighlight %}



