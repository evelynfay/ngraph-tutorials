title: Feature Title

author: Your Name

.. contents::

References
==========

[Feature Title](https://hsdes.intel.com/appstore/article/#/12345678/main)

```tsv("Contact List")
Group                	 Name
Architecture         	 TBD
Punit Design         	 TBD
Punit Firmware       	 TBD
Punit FW Val         	 TBD
Pre-si Validation    	 TBD
Post-si Validation   	 TBD
MDO Contact          	 TBD
SW Stakeholder       	 TBD
BIOS Stakeholder     	 TBD
Platform Stakeholder 	 TBD
```

Summary
=======

The feature HAS is intended to cover the entire set of details surrounding a new product feature.  It covers all levels of change from global (customer visible) architecture to firmware and design / RTL definition.  It is expected to be maintained throughout the project and acts as a single source for documentation of a specific feature.

Documentation is a collaborative effort.  Each group will contribute to the document and those contributions will be reviewed and ratified at checkpoints.

Motivation
----------

* Problem statement and motivation for adding this feature
* Include links to any data that would motivate the feature
    * place docs in `./assets`

Feature Details
===============

Firmware
========

Document firmware changes.  If multiple

Instruction cost estimate
-------------------------

Unit1 Hardware (e.g., Punit)
============================

Summarize the unit impact

Unit2 Hardware (e.g., Display)
==============================

Summarize the unit impact

Global Flows
============

Reset Entry / Exit
------------------

Warm and cold

Package C-states
----------------

C6
~~

C7
~~

C8
~~

C9 - C10
~~~~~~~~

S3
--

Populate this section as needed.  If there are any changes to cross-unit communication, document those here

Fuses
=====

Document all fuses required for this feature.  Documentation should follow standard fuse template

```xml
    <fusebit name="CACHESZ" owner="ebolotin" class="LLC">
      <length>3</length>
      <description>LLC size/ways.
(Once it was upgradable through SSKU up to LLC_WAY_EN.)
Fuse value   LLC Size per slice (Enabled ways per slice) </description>
      <encoding_description value="000">0.5 M (4 lower ways)</encoding_description>
      <encoding_description value="001">1.0 M (8 lower ways)</encoding_description>
      <encoding_description value="010">1.5 M (12 lower ways)</encoding_description>
      <encoding_description value="011">2.0 M (16 lower ways)</encoding_description>
      <encoding_description value="100">2.5 M (20 lower ways)</encoding_description>
      <fuse_program_value type="default">3'b011</fuse_program_value>
    </fusebit>
```

Debug and Telemetry
===================

Document any details for DFX here

* New tap commands or chains
* Trace messages for feature characterization or debug
* Test modes to stress this feature (for early validation or stress and stability for silicon)
* Low power debug for blocks that are running during power-up or down flows
* C10 requirements and Save/restore space allocation
* What sort of counters or telemetry are needed to debug this feature on silicon?
* Do we want to be able to create debug triggers on events created by this feature?
* VISA monitoring of this feature
* Is there a debug mode where the feature may be disabled, and if not what is our risk mitigation strategy?
* If the feature targets some condition that is hard to hit, what DFX is being added so that we can test it?

Firmware Trace Messages
=======================

Document trace message XML here


```xml
<message>
  <flow>PSTATE</flow>
  <name>SLOW_LIMIT_CHANGE</name>
  <id>0x30</id>
  <verbosity>0</verbosity>
  <security>green</security>
  <dataclass>event</dataclass>
  <description>
    Contains the new slow limit of IA, GT, and ring
  </description>
    <field datatype="gt_pstate">
      <name>NEW_SLOW_LIMIT_GT</name>
      <description>
        The NEW slow limit of GT
      </description>
      <dataclass>event</dataclass>
      <start>8</start>
      <size>8</size>
    </field>
</message>
```

Area
====

For every firmware agent or unit affected, document the cost in area or firmware instructions

```tsv("Feature Cost Estimate")
Unit Affected 	 Cost
Pcode         	 100 instructions
Punit         	 negligible area
```

Power Delivery
==============

Does this feature add any requirements to change power delivery

* Impact to peak current for specific rails?
* Power state table updates

KPI Impact
==========

* Product value statement

Manufacturing
=============

Document any support required of HVM.  This could include:

* HVM test methodology change
* HVM characterization task including methods for running this characterization
    * E.g., Cdyn characterization
* CMV test and correlation

Quality and Reliability
=======================

* Will this feature have any impact on Q&R?  Does it affect silicon wearout (high temp, high voltage) and if so, what is our plan to assess its impact?

Silicon Characterization
========================

* What sort of power and performance tasks are required to tune this feature
* Can we test with this feature on vs. off to evaluate its success?

Configuration
=============

* To enable or test this feature, what is needed to be set?  Fuses, registers, etc.

Platform
========

* Does this feature require any platform level enabling?

Software
========

* Is any driver support required for this feature?
* Is any OS support required for this feature?

BIOS
====

* Is BIOS support required?  If so, detail the requirements here.

Overclocking
============

* Does this feature have any impact to overclocking?

Customer
========

Document any relevant details regarding customer communication, engagement associated with this feature.  It may also be interesting to discuss customer feedback.

Pre-si Validation
=================

* Document validation plans here

Punit FW Validation Strategy
----------------------------

FORE changes required for Punit FW Val
~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~
* Document FORE changes here (if any)

Punit FW Val tests/stimulus to implement
~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~
* Document tests/stimulus here
* Corner cases:
    * Document corner cases here

Post-si Validation
==================

* Document validation plans here

Bugs
====

Any spec bugs or gaps discovered through execution (after HAS 0.8 ratification) should be documented here and fixes to the documentation to cover the gap or enhancement identified.

Notes
=====

As appropriate, include discussion notes below.  These notes are only valid at the point of recording them and are not a replacement for the spec defined above.
