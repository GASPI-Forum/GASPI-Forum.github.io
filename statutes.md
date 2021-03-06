---
layout: page
title: GASPI Forum Statutes
permalink: /statutes/
---

### GASPI Forum

### 1. Introduction and general Information

#### 1.1: What is a Partitioned Global Address Space ?

The Partitioned Global Address Space (PGAS) approach offers the developer of parallel applications an abstract shared address space which simplifies the programming task and at the same time facilitates: data-locality, thread-based programming and asynchronous communication.

#### 1.2.  What is GASPI?

GASPI is a Partitioned Global Address Space API. It aims at scalable, flexible and fault tolerant computing in massively parallel environments.  GASPI targets a paradigm shift from bulk-synchronous two-sided communication patterns towards an asynchronous communication and execution model. To that end GASPI leverages one-sided RDMA-driven communication in a Partitioned Global Address Space. GASPI aims at establishing an interface standard -- the GASPI standard -- which is able to accomplish these goals.

#### 1.3 How does GASPI differ from other PGAS approaches?

In contrast to other efforts in the PGAS community, GASPI is neither a new  language (like e.g. Chapel from Cray), nor an extension to a language (like e.g.Co-Array Fortran). Instead it complements existing languages like C/C++ or Fortran with a PGAS API which enables the application to leverage the concept of the Partitioned Global Address Space.  In contrast to, for example, OpenShmem or Global Arrays, GASPI is not limited to a single memory model, but provides configurable and yet globally accessible memory segments.  GASPI is intended to  be interoperable with MPI and allows for incremental porting of legacy applications.

#### 1.4.  What is the GASPI Forum?
      
The GASPI Forum (GASPIF) is an Open Forum which aims at a continous improvement and evolution of the GASPI API. In spirit it follows the approach of the MPI Forum. The Forum has been founded during the GASPI Forum Workshop heldt at Fraunhofer ITWM in Kaiserslautern on September, 19th 2014. Registered organizations at the foundation meeting have been T-Systems SfR, the German Aerospace Center DLR-AT and DLR-AS, the Fraunhofer Gesellschaft FhG ITWM and FhG SCAI, the TU Dresden ZIH, the University Erlangen RRZE, the KTH Royal Institute of Technology, the Leibniz Supercomputing Center LRZ, the TU Munich LRR, IMEC and the European Centre for Medium-Range Weather Forecasts (ECWMF). The GASPIF is not sanctioned or supported by any official standards organization.

#### 1.5.  A Brief History of GASPI

GASPI inherits much of its design from the Global address space Programming  Interface (GPI), which was developed in 2005 at the Competence Center for High Performance Computing (CC-HPC) at Fraunhofer ITWM. GPI is implemented as a low-latency communication library and is designed for scalable, real-time parallel applications running on cluster systems. It provides a PGAS API and includes communication primitives, environment run-time checks and synchronization primitives such as fast barriers or global atomic counters.
In 2010 the request for a standardization of the GPI interface emerged, which ultimately lead to the inception of the GASPI project in 2011 and the subsequent GASPI Forum. Original GASPI project members have been the Technical University of Dresden (TUD), the Karlsruhe Institute of Technology (KIT), the German National Weather Service (DWD), T-Systems SfR, the Fraunhofer Gesellschaft (FhG), the German Aerospace Center (DLR), the University of Heidelberg, the Forschungszentrum Juelich (FZJ) and the scapos AG.

#### 1.6.  Where is the current GASPI Document ?

The GASPI Document, that defines the semantics of the GASPI API, can be found along with other GASPI related information under http://www.gaspi.de

### 2  GASPI Forum meetings

1. GASPI Forum Meeting: An open meeting of the entire GASPI Forum in a physical  location. In-person attendance to the meeting is open to all organizations in the GASPI Forum as well as the general public.
2. The GASPI Forum foundation meeting has been the first GASPI Forum Meeting.
3. The GASPI Forum Meeting is held twice per year.
4. Individuals register for each GASPI Forum meeting that they will attend. At  the time of registration, individuals declare which organization they will represent at that meeting.
5. An organization is generally eligible to vote if it has registered and had one or more representatives physically present at the previous GASPI Forum meeting.
6. The date, location and the meeting organizer for the subsequent GASPI Forum meeting has to be agreed on at a GASPI Forum meeting as one of the agenda points.

### 3  GASPI Voting Rules

The GASPI voting rules aim to achieve the following goals:
1. Provide clear, unambiguous definitions and procedures for voting on general text proposals, errata proposals, and changing this document.
2. Enforce a high degree of consensus before text is accepted into the GASPI standards document.
3. To encourage contributors and implementors to implement and validate new semantics and functionality even before a potential first reading.

#### 3.1 Voting Procedures

1. An organization is eligible to vote at a specific GASPI Forum meeting if the following is true: The organization is generally eligible to vote (c.f. 2.4) and one of its representatives is physically present during that specific GASPI Forum meeting.
2. Meeting quorum: Quorum is established at a GASPI Forum meeting when more than 2/3 of the organizations, which are generally eligible to vote are present at the meeting.
3. All official ballots must be announced and scheduled at least two weeks prior to the start date of GASPI Forum meeting at which they will be held. 
4. Official ballot voting and formal readings occur only at GASPI Forum meetings where a meeting quorum has been established.
5. A ballot passes if is unanimously accepted
6. Each organization has only one vote.
7. A no vote has to be justified 

Rationale. The last condition sets high requirements for consensus before a  ballot will pass.  A proposal for a ballot hence needs to be well prepared. (End of rationale.)

#### 3.2 General Text Proposals

General text proposals for the GASPI specification documents typically add new semantics, change or clarify existing semantics, or remove previously-defined semantics. General text proposals use the following process to be accepted into an GASPI specification document:

1. Have a formal reading at a GASPI Forum meeting in which the meeting quorum  has been met.
(a) The text of the proposal to be read must be made publicly available  via the general GASPI Forum broadcast email list at least two weeks prior to the start date of the GASPI Forum meeting at which it is to be formally read.
(b) The formal reading must be scheduled on the GASPI Forum meeting's agenda at  least two weeks prior to the meeting's start date.
(c) It is up to the proposal's author(s) to decide whether to bring the proposal up for a formal ballot at a subsequent meeting.
(d) For required changes to a general text proposal for a subsequent formal ballot the GASPI Errata rules apply in a modified form: 
(e) If the required changes are considered as minor and if these changes are unanimously accepted during the GASPI Forum meeting of the formal reading (established by passing a single official ballot) the proposal text may be presented for a subsequent official ballot with the correspondingly unanimously accepted changes.
The modified proposal text has to be made publicly available via  the general GASPI Forum broadcast email list two weeks prior to the start date of the GASPI Forum meeting for which the ballot for the proposal is scheduled.

Rationale. The last two conditions allow for unanimously accepted minor changes to a generally text proposal at its first formal reading. (End of rationale.)

2. Pass the official ballot at a subsequent GASPI Forum meeting in which the meeting quorum has been met.
(a) A proposal's ballot can only be conducted after its formal reading.
(b) A proposal's ballot must be conducted at a different GASPI Forum meeting than the one at which it was formally read.
(c) If the proposal's ballot fails, its content may be changed during the GASPI Forum meeting. It is possible to schedule a formal reading for this modified proposal during the same GASPI forum meeting in which the ballot of the unmodified proposal failed. The next ballot for the modified proposal however, has to be scheduled for a subsequent meeting. 

Rationale. Condition (c) aims at proposals which in general are unanimously accepted, but nevertheless require minor modifications. (End of rationale.)

#### 3.3. Errata Proposals
Errata proposals for the GASPI specification documents deal with changes to documents to correct errors, clarify ambiguities, etc. Errata proposals use the following process to be accepted into a GASPI specification document:

1. Pass a single official ballot at a GASPI Forum meeting in which the meeting quorum can be established.
(a) The final errata proposal text must be made publicly available  via the general GASPI Forum broadcast email list two weeks prior to the start date of the GASPI Forum meeting for which the ballot for the errata proposal is scheduled.
(b) If the errata proposal ballot fails, its content may be changed during the GASPI Forum meeting. It is possible to schedule a second ballot within the scope of the same GASPI Forum meeting, in which the first ballot failed. If the second ballot also fails, the corresponding ballot has to be deferred to a subsequent meeting.

#### 3.4 Changing These Rules
The procedure for changing these rules is essentially the same as for Errata.
Proposals: publish the proposed change at least two weeks prior to a GASPI Forum meeting and then pass one official ballot with a 3/4 majority. Meeting and ballot quorum apply.

### 4 Remarks
(1) The GASPI Forum voting procedures are fairly strict. GASPI however encourages its contributors and implementors to experiment with new semantics, to implement new features, before the corresponding proposals are scheduled for official reading, to gather feedback from the end-users and to subsequently prepare proposal for reading and ballot.
(2) GASPI aims at a specification for a flexible, scalable and fault-tolerant modern PGAS API. Nevertheless GASPI tries to keep this specification as small as possible. Any implementation which implements a specific version of the GASPI specification will be compliant with the corresponding version of the GASPI specification. This does however not preclude proprietary extensions to the GASPI specification. GASPI encourages its users to implement such proprietary extensions, to evaluate them, to refine them and to subsequently present a universally acceptable corresponding proposal for reading and ballot at the GASPI Forum.
 

