---
layout: post
title: Next Meeting - 14th December 2016
location:  "T-Systems, Frankfurt / Main, Wilhelm-Fay Straße"
time: 11:00 CET - 16:00 CET
---

### Protocol of the GASPI Forum meeting on December, 14th, 2016, Frankfurt/Main

Date: 2016.12.14
Location: Frankfurt/Main, Wilhelm-Fay Straße 54
Start: 11:00h
End: 16:00h

Participants:
- T-System (Christian Simmendinger)
- LRZ (Christoph Bernauer)
- ITWM (Rui Machado, Daniel Grünewald)
- ZIH Dresden (Olaf Krzikalla)
- DLR (Georg Geiser)
- IMEC (Tom van der Aa)

#### Agenda:

11.00-11.30 Attendees, Quorum, next Forum meeting, Actions Items  
11.30-12.00 Errata Proposals  
12.00-12.45 Lunch  
12.45-13.15 Errata Proposals cont.  
13.45-14.30 General text proposal  
14.30-15.00 Voting  
15.00-16.00 GASPI Context, GASPI Standard Library  

#### Next meeting:

Frankfurt, June 21

#### ACTION:

- T-Systems: Put a hyperref package to the specification in order to allow easy browsing  
- ZIH: Errata proposal for local completion in the gaspi notify specificication. (Remove double "the" typo)
- All: What are the functions that could be removed or extracted to a standard library in order to shrink the specification
- All: Is GASPI ready for multithreaded communication on ~1000 Threads? Start thinking about possible limitations / solutions.
- T-Systems: Errata proposal for gaspi\_write\_list\_notify: segment id for notification is missing in signature
- T-Systems: Render the version number instead of '\today' into the document.
- ZIH Dresden: Prepare proposal for clear (or no) definitions of "blocking", "non-blocking"
- ITWM: Make a proposal for clarification that GASPI\_ERROR shall not be used in equality comparison. Thinking about the concecpt of a GASPI\_ERROR itself
- T-Systems: Prepare a proposal for a change of statute that future proposals shall include a merge request. github voting technology could be used for accepting merge requests after modifications to a proposal have been discussed and accepted during a meeting.
- T-Systems: Present good example of how to use multiple queues (why should we have queues) -> not yet done but should be about local completion. Present an example how to use double buffered queues in order to have a no-op.
- T-Systems: Write prototype for a library/wrapper that demonstrates how to optimize bandwidth

#### Reports:

T-Systems: Talk to KTH to stop running the second/old/outdated forum site has been done
T-Systems: Tutorial at LRZ: Christian has contacted Mr. Bode. It will probably happen during the next extreme scale workshop held at second quarter 2017
T-Systems: Why do we need queues? -> Queues are needed in order to be able to provide a check for local completion
T-Systems: Provided a non-GPL header, the pure GASPI header, generated from a script which extracts the header from the specification tex.

#### Errata proposal: GASPI\_ERROR\_END\_OF\_QUEUE  

CS: There is a potential race condition in multithreaded communication when the queue has
less then #threads entries left -> GASPI\_ERROR\_END\_OF\_QUEUE proposal  
Georg: In the proposal there is a typo: gaspi\_wait\_queue needs to be replaced by gaspi\_wait  
Tom: What is the relation to the timeout? The timeout only .  
Daniel: is there still a reason for the gaspi\_queue\_size? No, it is not.  
Olaf: it still makes sense to have gaspi\_queue\_size\_max in order to be able to estimate the amount of required queues for a given problem.
Christian: Use gaspi\_config\_get in order to request max parameters. Throw away explicit get functions. One could put them in a standard library.  
Rui: ITWM is missing an adaption of the paragraphs describing the behaviour in case of a full queue in all of the write, read calls containing such a section.  
Olaf: Is GASPI\_ERROR\_END\_OF\_QUEUE really an error? No. It should be an additional return value. In case of an error we do not know what to do. However, for end of queue, we perfectly know that. So, it is not an error. Rename it to GASPI\_QUEUE\_FULL and treat it as a special return value. 

Vote for rejection of original proposal: unanimously rejected  
Vote for the changed and clarified proposal: unanimously accepted  

#### Errata proposal: Read / Write ordering  

CS: gaspi\_write\_notify, gaspi\_write\_list\_notify, gaspi\_read\_notify and gaspi\_read\_list\_notify can be considered as unordered relative to each other and also as unordered relative ot the function calls gaspi\_read and gaspi\_write. -> Replace "preceding" communication requests with "other" communciation requests accordingly.

Vote: unanimously accepted

#### Errata proposal: clarification of global lock example

Olaf: global\_try\_lock returns and error if you don't get the lock -> it is not an error. It also returns an error if a real error occured. One cannot decide whether one could not get a lock or whether an error occured.  

- Substitute SUCCESS\_OR\_RETURN by SUCCES\_OR\_DIE  
- Remove timeout from signature and use GASPI\_BLOCK internally  
- Introduce specific return values: LOCK\_ACQUIRED, NO\_LOCK\_ACQUIRED  

Voting: Unanimously rejected  

ITWM: global\_unlock not symmetric anymore

- remove timeout also here  
- use SUCCESS\_OR\_DIE instead of SUCCESS\_OR\_RETURN
- return value void  

Olaf: add assertion of lock value.  

Voting: unanimously accepted

### General text proposal: gaspi\_read\_list\_notify (First reading)  

CS: Extend gaspi\_read\_list with a notification on the local side  
Rui: The specification of the target segment for the notification is missing. Actually, there are also inconsistencies in the fortran / c / interface for write\_list\_notify where you don't have a target segment specification for the notification in the parameter section / c interface  

- The second reading of the proposal must include the target segment for notification.  
- Need an errata proposal to fix write\_list\_notify  

#### Discussion:  

CS: We need more than one implementation to get more reception: people get uneasy on depending on a single implementation from a single institution. In order to achieve that, we need to shrink the interface to a real minimum of basis functions with which all other functions can be implemented on top of that. All convenience functions can be outsourced to a standard library which is using these basis functions  
Rui: The goal always has been to keep the interface as small as possible. Except for getter/setter functions there isn't much we can remove. We should be specific: check and make a list of potential candidates to be removed.  A standard library has been in discussion for long time and again we need to be more specific of what functionality should go in there. Convenience functions yes (although I only see gaspi\_segment\_create) and the often requested collectives. Instead of increasing the reception on the implementor side, we should increase the reception in the user space.  
Tom: Eric Petit has implemented a tree based reduction on top of Gaspi. Perhaps it could be published on the website and used as a first piece of a standard library. 
CS: is there anything which spoils GASPI scalability of communication on up to 5000 cores per process? -> think about potentiel bottlenecks and their solution.  
Olaf: For instance, on KNL locks are deadly -> Try to avoid locks as much as possible.  


