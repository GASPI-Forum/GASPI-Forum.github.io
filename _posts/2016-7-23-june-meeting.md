---
layout: post
title: Next Meeting - 22th June 2016
location:  "T-Systems, Frankfurt / Main, Hahnstr. 43d (Aculeum), Room D.00.63"
time: 13:00 CET - 17:00 CET
---

### Protocol of the GASPI Forum meeting on June, 22th, 2016, Frankfurt/Main

GASPI Forum Meeting  
Date: 2016.06.22  
Location: Frankfurt/Main, Hahnstraße 43  
Start: 1pm  
End: 5pm  

#### Participiants:
- DLR (Göttingen, Köln)
- FHG ITWM
- IMEC
- LRZ (not allowed to vote)
- T-Systems
- TU Dresden
- Uni Heidelberg

#### Next meeting: Frankfurt, December, 14th.

#### ACTION:
- T-System: Talk to KTH to stop running the second/old/outdated forum site.  
- T-System: Prepare proposal the change  "preceding" to "other" in read_notify.  
- ITWM: Render the version number instead of '\today' into the document.  
- TU Dresden: Prepare proposal for clear (or no) definitions of  "blocking", "non-blocking".  
- ITWM: Make a proposal for clarification that GASPI_ERROR shall not used in equality comparison  
- ITWM: Prepare Proposal that future proposals shall include a merge  
request, such that accepting the proposal could include the merge even  
during the forum meeting.
- ITWM: Prepare proposal to allow for merge requests that gets accepted  
outside the forum meeting by using voting technology from github.
- T-System: Propose change of statue to allow for smaller modifications  
even in the second reading.
- T-System: Prepare proposal for read\_list\_notify. 
- T-System: Provide a non-GPL header, the pure GASPI header using the  
former script.
- T-System: Present good example of how to use multiple queues.  
- ?: Write a prototype for a library/wrapper that demonstrates how to  
optimize bandwidth  

#### Future developments of GASPI
- CS: Discussion of future developments of GASPI
- CS: MPI 4.0 sees the inmportance of Notifications but it has not the  
highest priority in the MPI roadmap
- CS: Explains the idea of using gaspi_segment_bind together with  
shmem_open in order to get a "flat GASPI", a single process per node  
would be the master that owns the memory and does the communication,  
the other processes are using the master as proxy
- Tom: Why is it easier to change legacy code? Flat -> Flat
- CS: Is two level "flat", the additional level could be hidden inside  
the communication calls.
- VE: Why not just change the communication calls? (Simple  
translater?) -> CS: No, we also want to use the different semantics  
from GASPI: You can break up the model where ever you want and move  
step by step to a full GASPI program.
- Tom: Can this be mixed with a full GASPI program? Discussion with  
setup of flow simulator and FLUCS working together. -> Maybe not  
useful to mix pure thread model and flat process model!?
- MR: What changes are required in GASPI? -> CS: Share notifications.  
gaspi_reset must ensure that all _processes_ are synchronized. GASPI  
needs to allocate the space for the allocations in a shared memory  
segments.
- OK: Where is the difference between that model and GASPI+threads? ->  
For example global variables. This is not for new applications but to  
port existing flat MPI codes.
- TVa: Example codes? CS: Yes, there are some. Also CS did in the CFD  
proxy and saw advantages.
- Difference to MPI shared memory? -> Not much except that locally  
_and_ globally the same mechanism for notifications can be used.
- MR: How many ranks there are? -> For flat MPI there are many ranks,  
the model above just has one per node. -> A library could hide that  
from the user.
- ??1: What is it good for? Because in MPI the say that shared memory  
is not about performance but about using less memory.

####
- Report: GASPI website has been polished and contains news,  
announcements, information etc. Old site still running but outdated.
- OK: Can we have another top level suffix (instead of '.de' that  
people might believe they are going to a German site).
- Not all errata proposals are accepted. (Not clear why, probably  
miscommunication)

#### Errata proposal: write, write_notify: Bandwidth vs. Latency.
- CS: Explains the problem with the equivalence of write_notify ==  
write . notify, namely that smaller notified messages (latency) can  
not overtake bigger messages (bandwidth).
- The split is write . notify shall be used for bandwidth critical  
transfers where write_notify shall be used for latency critical  
transfers
Aside (this is not in the current proposal)
- Presents the idea of aggregating and delaying transfers in order to  
fire later a write_list. In General: write should optimize available  
bandwidth.
- Discussion of where should the implementation go: Application or  
implementation? There are arguments for both layers. Argument to put  
it into the library: Do no fancy things but rely on gaspi_notify. ->  
Danger because that might change the semantics of gaspi_wait. ->  
Conclusion: Try this out in a wrapper library in order get experience  
on how to implement it for different networks/applications. This would  
also make a later proposal stronger.
- OK: Comment: There is no way now to know about the "last"  
write_notify. -> Solution: Another function. Not required right now.
- MR: Comment: Implementation for IB and for ETH will not change,  
implementation for CRAY will actually become simpler.
- MR: Comment: In the next steps we might want to rename functions  
(because write_notify != write . notify might come as a surprise to  
users). For example gaspi_write -> gaspi_put and gaspi_notify ->  
gaspi_fence.
- MR: User advice not in the spec but just in the CHANGELOG.

Vote: 6 (change the implementors advice and move the user advice into the
CHANGELOG)

#### Various Changes/Errata:
- VE presents the several proposals
- Proposal 4: OK: Remove the "atomic" to allow as much local work as  
possible. Then GASPI_TEST would not mean "exit as fast as possible"  
but "never reach a wait state". Would neither say "all local work" nor  
"just an atomic part". User could observe that because now an  
arbitrary number of callbacks in gaspi_allreduce_user could be done  
for a single test with GASPI_TEST.
- Proposal 5: ITWM proposes another sentence
"Collective operations are/will be synchronized independently from the
operations on the communication queues"
- Proposal 6: ITWM proposes another change
"while ( (ret = gaspi_allreduce_user()) == GASPI_TIMEOUT)
{
work_on_something_else();
}

if( ret != GASPI_SUCCESS)
{
     handle_error(ret);
}
"

Vote for rejection of original proposal: 6
Vote for the changed and clarified proposal: 6

#### Proposal read_notify: Second reading

Vote: 6

#### GASPI 2.0
- Need a project for that!?
- Communication context: Will touch all functions signatures.
- User friendliness. In the API itself (or in the standard lib) like  
offset negotiation.
- Bugs like that there is no atomic function for checking the queue size.
- Concepts. (E.g. remove queues!?, bandwidth optimized put/fence vs  
latency optimized write_notify)
- Standard library. -> convenience functions, collectives, better  
support for task models, look at OCR (open community runtime)
- Accelerators. -> Are there any issues for the forum?
- New types of memory. -> Are there any issues for the forum?


