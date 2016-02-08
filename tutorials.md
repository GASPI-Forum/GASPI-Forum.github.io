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
* GASPI_SUCCESS
  * designated operation successfully completed
* GASPI_TIMEOUT
  * designated operation could not be finished in the given period of time
  * not necessarily an error
  * the procedure has to be invoked subsequently in order to fully complete the designated operation
* GASPI_ERROR
  * designated operation failed -> check error vector
  * Advice: Always check return value !

{% highlight c %}
{% include_relative _source/helloworld.c %}
{% endhighlight %}

#### Process management

{% highlight c %}
{% include_relative _source/success_or_die.h %}
{% endhighlight %}



