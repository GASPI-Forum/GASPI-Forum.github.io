---
layout: post
title: Meeting - 20th January 2016
location:  "T-Systems, Frankfurt / Main, Hahnstr. 43d (Aculeum)"
time: 11:00 CET - 16:00 CET
---
### Protocol of the GASPI Forum meeting on January, 20th, 2016, 11am - 4pm in Frankfurt/Main

Attendees:
- DLR
- IMEC
- ITWM
- T-Sys
- TU Dresden
- Uni Heidelberg

_6 out of 8 members are present, the meeting has a quorum_

#### Introduction
GASPI still ahead, others pick up (MPI40 notifications, openSHMEM's counting put)

#### Next meeting:
 June, 22th, 2016, Frankfurt/Main

#### Website:
 Not in a good shape, TUD will set up a page on github-io that will be the presentation of the forum and also (github!) the collaboration platform

#### Clarification:
 The definition of "member" is: Was present last time and has registered.

#### Change in the statues:
 T-Sys proposed a change that allows for small and agreed changes in proposal texts, in order to correct spelling mistakes without forcing a new reading.
Vote: +6

#### No errata proposals

#### Comment from T-Sys:
Will propose to clarify that ```write_notify()``` shall _not_ be equivalent to ```write()``` followed by ```notify()```.

Reason: ```The notify()``` implies a fence for all preceding ```write()``` whereas the ```write_notify()```
shall not imply anything for preceding ```write()```'s. This opens potential for optimization and breaks nothing except
codes of the form ```write()``` followed by ```write_notify()```,
which can be written as write() followed by write() followed by ```notify()```.
The Forum agrees on that this will be an improvement.

#### UNI-HD:
 Note on the statue: Proposal readings must have an entry in the schedule.

#### Proposal ```gaspi_read_notify()```:
 Motivation: High number of messages in flight for example in graph problems and, at the same time, never pay the cost for the latency. The example in the proposal is of that kind and shows how to implement a read pipeline using ```gaspi_read_notify()```.

**Discussion: There are two forms of asymmetry introduced:**

a) The notification value becomes a notification bit when used with ```read_notify()```.
This is because other solutions would involve an active component.
However, this is not a real problem as there is no good use
case for notification value: In order to use ```read_notify()``` some former synchronization is
required (to know that the remote data is readable) and
that synchronization easily can include all knowledge that a notification value would encode.

b) The ```read_notify()``` does not imply a fence for any other operation. This in contrast with the ```write_notify()```
in the current form, see the comment above.
The Forum encourages T-Sys to back up the ```read_notify()``` proposal with the ```write_notify()```
clarification in order to remove this asymmetry.
(Note: Voting on the proposal will take place after the foreseen acceptance of the clarification.)

Further discussions about the consequences of that proposal:

In the very end, the ```read()``` and the ```write()``` can be removed. While the Forum agrees on the logic behind
(namely that a plain ```read()``` or a plain ```write()``` shall be replaced by ```read_notify()``` and ```write_notify()```
in order to overlap communication and computation), a removal of ```read()``` and ```write()``` involves some costs for the user,
namely a more complex management of notification ids. Also there might be situations where no work on partial data is
possible but still chunks of the data are produced at different times. The Forum would like to see an advice to users
that makes clear that ```write()``` and ```read()``` might not give the best performance and ```write_notify()``` or ```read_notify()```
are probably better choices.

The Forum agrees that the Proposal can be accepted next time after some minor spelling mistakes has been corrected.

#### Proposal ```alltoallv()```:

Situation: Each rank knows about a local situation (what data to send to which rank).
Design aims for pipelined collectives and a good fit into GASPI.

Discussion:

- The ```GASPI_TEST``` value might not be sufficient, especially because the definition of "minimal progress" is not 100% clear.
- The overload of ```GASPI_BLOCK``` (it changes the meaning of the parameter ```received_rank```) is not considered a very good idea.
- The mentioned use case FFT is not a strong case as the ```alltoallv()``` only allows for a pipeline on receiver side but a scaling FFT also has a pipeline on sender side. (Note: The alltoallv() might be the right tool for a ```_sequence_``` of FFT.)
- The ```gaspi_alltoallv_reset()``` does not work in the way GASPI works currently:
It is required in order to release a subsequent ```alltoallv()```.
However, in GASPI the separation of operations is typically delegated to the user and
missing separation does not lead to blocking behavior but to overwrites.

Action points:

- provide a high level implementation of the ```alltoallv()``` and of the supporting function(s)
- provide and test a native implementation
- rethink the interface and check possibilities to overcome the current discussed weaknesses
- check whether the examples for ```allreduce()``` in the spec needs to be improved

#### More general discussion about a core/extended API:

- Idea: Move more complex functions (like ```gaspi_segment_create()```, ```alltoallv()```, ...) into an extended API that has default implementations using the functions from a core API.
- Question/Problem: Where is the border between the two? Criteria? -> Might be "A function is in the extended API whenever it is possible to give an efficient implementation on top of other functions."
- Question/Problem: What is the criteria to include some function into the spec? -> Solution: Just concentrate on the functionality. If the function is important for users, then it should be added, regardless if whether or not the interface is "perfect". (Of course, it should not be broken obviously.)

Agreement:

*No explicit distinction between core API and extended API but add an appendix that gives default implementations for more complex functions.*

#### Proposal ```gaspi_queue_create()```:

Short discussion about motivation and some questions answered by ITWM.

Vote: +6

#### Proposal ```gaspi_segment_use()```:

Short discussion about usage and some questions answered by ITWM.

The parameter order requires a further check and there might be a subsequent errata proposal.

Vote: +6

#### Action points:

- ITWM: Merge proposals into specification text (that will get version number 16.1)
- TUD: set up a web page

#### Closing:
statements from T-Sys and ITWM about the importance of the Forum and they express the wish to strengthen the Forum by acquiring new members.
