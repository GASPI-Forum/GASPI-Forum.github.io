---
layout: post
title: Meeting - 10th January 2018
location:  "T-Systems, Frankfurt / Main, Wilhelm-Fay-Str. 54, 1. OG, A.1.08"
time: 11:00 CET - 16:00 CET
---

### Protocol of the GASPI Forum meeting on January, 10th, 2018, Frankfurt/Main

Date: 2018.01.10  
Location: Frankfurt/Main, Wilhelm-Fay Straße 54  
Start: 11:00h  
End: 16:00h  

Participants:

- Fraunhofer ITWM (Mirko Rahn, Daniel Grünewald, Martin Kühn)
- ZIH, TU Dresden (Christian Herold)
- DLR (Tom Gerhold, Olaf Krzikalla)
- LRZ (Christoph Bernauer)
- Tartu University, Estonia (Benson Muite)
- T-Systems SfR (Christian Simmendinger)

#### Agenda:

Daniel: GaspiCXX

C++ interface to Gaspi / GPI-2
  - Idea: Improve usability, retain performance
  - Generic (fully dynamic) resource management
  - Segment memory management
  - Segment notification management
  - RAII: Segment, Queue, Group
  - Abstraction of communication
  - Setup phase: may be „expensive“
  - „Production“ phase: full performance
  
Christian: Shared Memory Segments for GASPI

 - Idea: Support for Migration from legacy MPI-only applications
 - Replace node-internal communication with shared memory data dependencies
 - Replace intra-node MPI communication with GASPI notified communication
 - Aggregate intra-node communication 

#### Errata Proposals

#### General Text Proposals


#### Discussion

 - Use of GASPI for halo exchange library, such as detailed in https://arxiv.org/abs/1207.1746
 - Use of GASPI in DASH project http://www.dash-project.org/team.html

Tutorials / Hackathon

- Benson Muite: Still investigating workshop/hackathon

Next meeting:

- June, 27 2017 in Frankfurt
