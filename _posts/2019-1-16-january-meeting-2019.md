---
layout: post
title: Meeting - 16th Jan 2019
location:  "T-Systems, Frankfurt / Main, Wilhelm-Fay-Str. 54"
time: 11:00 CET - 15:00 CET
---
### Protocol of the GASPI Forum meeting on Jan, 16th, 2019, Frankfurt/Main

Date: 2019.01.16
Location: Frankfurt/Main, Wilhelm-Fay Straße 54 
Start: 11:00h 
End: 15:00h 


Participants:

- Fraunhofer ITWM (Daniel Grünewald, Valeria Bartsch)
- ZIH, TU Dresden (Christian Herold)
- DLR (Olaf Krzikalla)
- IMEC (Tom van der Aa)
- T-Systems SfR (Christian Simmendinger)

#### Agenda:


11.00-11.30 Attendees, Quorum, next Forum meeting, Actions Items 
11.30-12.30 Errata proposals 
12.30-13.15 Lunch break 
13.15-14.30 General text proposals 
            First reading: 
	    1. Fine grain local completion and message tagging 
	    2. GASPI-SHAN, notified communication in shared windows (std. library). 
14.30-15.00 Voting 
15.00-16.00 Next steps 
	    1. Collectives 


#### Errata Proposals

#### Accepted Erratas
- Section 11.1, 3rd paragraph:
  Collective operations are exclusive per group, i. e. only one collective operation of a specific type on a 
  given group can run at a given time. Starting a specific collective operation on the same group, while another
  collective operation of the same kind has not returned locally with GASPI_SUCCESS or an error value yields
  undefined behavior.  For example, two allreduce operations for one group can not run at the same time on a
  given process; however, an allreduce and a barrier operation can run at the same time.

##### Remarks:
- the requirement for remote completion has been removed. An implementors advice creating awareness of that
  fact (and handling it e.g. by double buffering) should be introduced later on.

- Section 11.2.1, 4th paragraph:
  Barrier operations are exclusive per group, i. e. only one barrier operation on a given group can
  run at a time. A concurrent call to a barrier operation on the same group yields undefined behavior. 

- Section 11.3.1, 4th paragraph:
  Reduction operations are exclusive per group, i. e. only one reduction operation on a given group can
  run at a time. A concurrent call to a reduction operation on the same group yields undefined behavior.
  In addition, if there is a running reduction operation (e.g. the last call returned GASPI_TIMEOUT), then
  calling that reduction operation on the same group with different arguments beside the timeout parameter
  yields undefined behavior.

- Section 7.2.1, 5th paragraph:
  After successful procedure completion, i. e. return value GASPI_SUCCESS, the segment can be accessed
  locally. In case that there is a connection established to a remote GASPI process, it can also be used
  as a local segment for GASPI communication calls between the two GASPI processes.

##### Remarks:
- there is no need to register a segment, if it is used as a local segment in gaspi_read or gaspi_write.

- Section 7.3.1 1st paragraph:
  The synchronous local blocking procedure gaspi_segment_delete releases the internal resources of
  a previously allocated or bound memory segment. The corresponding segment id can be reused after successful
  procedure completion.

- Section 7.3.1 3rd paragraph:
  After successful procedure completion, i. e. return value GASPI_SUCCESS, the internal resources are
  released. This includes the segment memory, if the segment was created via gaspi_segment_alloc or
  gaspi_segment_create. It would be an application error to use the segment for communication between
  two GASPI processes after gaspi_segment_delete has been called.

- Section 11.2.2 2nd example:

```c
const gaspi_return_t err = gaspi_barrier (g, GASPI_TEST);

do_local_cleanup();
if (err == GASPI_TIMEOUT)
{
  gaspi_barrier (g, GASPI_BLOCK);
}
```

##### Open Points
- The usage of GASPI_ERROR is often misleading or wrong in the standard, since it is not a particular 
  value. (see also errata 11.2.2)
- there will be a proposal introducing language bindings for constants (and leaving GASPI_ERROR with 
   no such binding)
- usage of GASPI_ERROR in the standard document has to be revisited

- The terms "finish" and "completion" are not defined.
- finish: an operation is considered finished, if the corresponding local GASPI call did not return 
  with GASPI_TIMEOUT.
- completion: an operation is considered completed, if it is finished successfully, i.e. the corresponding
  local GASPI call did return with GASPI_SUCCESS.
- usage of the terms "finish" and "completion" in the standard document has to be revisited
- replace the "finish" in section "MPI-Interoperability" with "complete"

- Bitwise reproducibility of floating point operations in reductions is not defined
- Idea: introduce a configuration switch with 3 modes:

1. bitwise reproducibility accross all ranks and accross equal runs, i.e. all ranks see the
   exactly same result for an operation and every call of that operation with the same arguments
   and the same number of ranks on a given machine yields exactly the same result
2. bitwise reproducibility of one call accross all ranks, i.e. all ranks see the exactly same
   result for an operation
3. no bitwise reproducibility (only possible, if control flow doesn't depend on the result of
   floating point reductions)


#### General Text Proposals

##### Fine grain local completion

- Solution 1 - every task using different queue
- Solution 2 - check every request
- Solution 3 - remote reply to notification


##### Discussion 
- Tagging. waitfor one tag => get any tag =>set corresponding tag as locally complete.
- Will require a waitsome for tags.
- Alternative approach: Fine grain local queues vs tagging ?
- Fraunhofer => need to look at (large scale) queue multiplexing 

##### Shared notifications (SHAN)
- Data type interface not final yet. 
- Two stage data types, base type + extended type introduced
- Good performance,

#### Next steps

- GASPI interoperability:
  check: startup, for given MPI communicator.
  Consider nesting, inheritance of ranks

- Error State proposal:
  Three possible entries: unknown/healthy/broken

- GASPI_ERROR handling:
  To consider: < GASPI_SUCCESS ==> error  ?


