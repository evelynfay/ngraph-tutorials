
title: IP PM Integration HAS
author: Your Name


.. contents::

References
==========

Contacts
========

Name 	 Role
---- 	 ----
TBD  	 PM Architect
TBD  	 IP Architect
TBD  	 IP Design
TBD  	 IP Integration
TBD  	 Clocking Architect

Introduction
============

High level introduction of the IP.

<mark>OPEN[Owner_Name]:</mark> During development it is encouraged to make this a living document with opens clearly identified with owners in the section where the open exists.  A document with no highlighted OPEN is a document in the POR state.

Power Delivery
==============

This section defines power delivery requirement at IP's boundary and also the power domains within IP.

Peak Power
----------

Peak power delivery requirement should also be covered.  Include ICCMax and virus information.

Vmin
----

Retention
---------

Does the IP utilize a retention voltage?  Does it need a retention rail?  What are the current requirements.

Power State Tables
------------------

Need to define power states for global SOC integration.

Clocking
========

This section defines IP's clocking requirement. 

This is not meant to be in detail to replace Clocking HAS, but relevant clocking information should be captured.

PM State Definitions
====================

As HAS, architectural visible PM state should be defined here.

A state diagram should be drawn showing all legal state transitions.

```dot("Sample PM state diagram")
digraph {

  node [fontname=helvetica,shape=circle,style="filled,rounded",fillcolor=snow];
  edge [fontname=helvetica];
  rankdir=TB;

  start1 [shape=circle, fillcolor=black, width=0.25, label=""]
  CRST_OFF [label="CRST\nCOLD"]
  Warmreset [label="Warm\nReset"];
  C0 [label="  C0  "]
  C6 [label="  C6  "]
  C3 [label="  C3  "]

  start1 -> CRST_OFF []
  CRST_OFF -> C0 
  C0 -> C6
  C0 -> C0
  C6 -> C0 [splines=curved]
  C0 -> C3
  C3 -> C0
  C0 -> CRST_OFF 
  C0 -> Warmreset 
  Warmreset -> C0 
  C6 -> CRST_OFF [label="PKGC Entry"]
  
  {rank=same; C6, Warmreset}
}

```

* PM State table should list key architectural visible implications for each state. For things like: which external rail can be removed, save/restore required or not.

```csv("Sample PM State Table")
PM State,Rail1,Rail2,CLK1,CLK2,Interface1
CRSTCOLD,OFF,OFF,OFF,OFF,BLOCKED
C0,ON,ON,ON,ON,UNBLOCKED
C3,ON,ON,OFF,OFF,BLOCKED
C6,ON,OFF,OFF,OFF,BLOCKED
WarmReset,ON,ON,OFF,OFF,BLOCKED
```

How does the IP's power state map to PkgC States.

Reset flows
-----------

Punit and Reset teams need timing diagrams for cold boot, cold reset, SX, and warm reset flows. The diagrams need to show both entry and exit of all.
 
If the IP is fully chassis compliant with no additional reset signals, please review the chassis reset waveform  and verify it follows this flow exactly.  If not and exact match, timing diagrams will be needed.  If it is an exact match, please make a note of it below.


Cold Boot Reset Flow
~~~~~~~~~~~~~~~~~~~~

```visio("smbus_reset_requirement.vsd", "SMBUS Cold boot" , "SMBUS Cold boot")
```

Cold Reset Entry Flow
~~~~~~~~~~~~~~~~~~~~~

```visio("smbus_reset_requirement.vsd", "SMBUS Cold Reset" , "SMBUS Cold Reset")
```


Warm Reset Entry Flow
~~~~~~~~~~~~~~~~~~~~~
```visio("smbus_reset_requirement.vsd", "SMBUS Warm Reset" , "SMBUS Warm Reset")
```

SX Flow 
~~~~~~~
```visio("smbus_reset_requirement.vsd", "Sx entry and exit" , "SMBUS SX")
```



Idle State PM Management
========================

This section should define in detail the state transitions between architectural visible states that are defined in previous section.
In additional to state transition definition, following topics should be covered:

* Wake event definition: If IP can trigger wake, IP's wake source need to be defined.
* Wake QoS/LTR negotiation: QoS/LTR/WM associated to IP's idle state entry/exit need to be covered.

For each idle PM state transition, a corresponding wave would be required.

Package C-state entry exit flow
-------------------------------

S0ix entry and exit flow
------------------------

This section defines the entry requirements for the IP to the global S0ix flow.  It should include a timing diagram showing entry and exit.

![S0ix entry flow](assets/WFST S0ix entry flow.png)

Save-Restore
------------

This sub-section defines save restore register list, group, group ordering and any special save-restore requirements.
```xls("assets/wfst.xlsx", "save_restore")
```

Wakes
-----

QoS and LTR negotiation
-----------------------

Accessible Power Gating
-----------------------

Inaccessible Power Gating
-------------------------

D3
--

D0i3
----

Fuse and Function Disable Flows
-------------------------------

Active State PM Management
==========================

This section should define all the interactions between IP and SoC while IP is in active PM state.

DVFS
----

If IP supports DVFS and need SoC's assist, this section should define the IP's DVFS flow.

SA GV
-----

If IP will be interacted with other SoC level flow (e.g., SA  GV), this sub-section should define such interaction in detail.

Thermal Management
------------------

This subsection should define IP's thermal management which can include following topics:

* Thermal status report
* Thermal throttling
* Thermal Trip

Error / Crash Handling
======================

If an IP may be the source of fatal errors, potentially leading to machine checks, it should be analyzed for inclusion into the SOC level 'crashlog' feature.  The IP should present key error related register content or state machines for logging on a global error.

DFx and Telemetry
=================

This section defines IP's DFx and telemetry which can include following topics:

* ODLA and VISA support
* TAP override and status report
* Survivability hooks
* Telemetry and Traces

Fuses, Pin Strap, and SMIP
==========================

This section defines IP's fuses, Pin Strap, and SMIP related to power management. Note that some of these items will be stored part of IP area of control, while some of the fuses may be stored in the Punit's area of control.  For any fuses, Pin Strap, and SMIP defined in the Punit area, they need to be documented in detail following standard fuse XML templates as an example.

HVM and Characterization
========================

This section defines

* Any special handling required by HVM, especially modification to any of PM state transitions.
* Power and performance tasks used for silicon characterization.

PM Interfaces
=============

As High Level Architectural Specification, this section will only define architectural or external visible interfaces.

PM Signal Interfaces
--------------------

This subsection defines the IP's signal interfaces related to power management.

```csv("Sample PM Signal Interfaces")
Signal Name,Direction,Power Domain,Reset Default,Description
pma_reset_b,IN,VCCSA,0,Main reset to IP.
```

PM Messages
-----------

This section defines SB messages exchanged between IP and SOC power management units (e.g., PM_REQ, PM_RSP).

HAS should explicitly state if they do or do not use some common side-band message:

* PM_REQ
* PM_RSP
* IP_READY
* LTR
* EarlyPrep/BootPrepAck
* ResetPrep/ResetPrepAck
* ForcePwrGatePOK
* SetIDValue/Cpl_SetIDValue


PM Registers
------------

This section defines registers at IP or SoC that are related to IP's power management.

PM Registers in IP
~~~~~~~~~~~~~~~~~~

This section defines registers at IP that are related to IP's power management.

PM Registers in other IP
~~~~~~~~~~~~~~~~~~~~~~~~

This section defines registers at SoC (e.g., Punit) that are related to IP's power management.

TODOs
=====

```csv("Sample TODOs")
ID, Description
[1], FIXME
```
