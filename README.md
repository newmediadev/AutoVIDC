# AutoVIDC

Introduction
============

AutoVIDC is a re-locatable module used to control a VIDC Enhancer that provides alternative clock frequencies for the Acorn VIDeo Controller chip called VIDC which is responsible for the video output in the Archimedes range of computers.
By using faster VIDC clock speeds delivered by a VIDC Enhancer, higher screen resolutions and refresh rates can be achieved by the Archimedes.

The software is intended to work with RISC OS 2.0x and RISC OS 3.xx to provide the ability to control up to two standard VIDC Enhancers at the same time or a single three oscillator VIDC enhancer. The latter can be used to drive the VIDC chip at up to four different clock speeds when including the original Archimedes VIDC clock signal.

AutoVIDC is also included as part of the [JASPP](https://forums.jaspp.org.uk/forum) ADFFS distribution to provide VIDC clock control for multiple VIDC Enhancer types using a consistent set of SWI calls.

DEVELOPMENT
===========

AutoVIDC is a mature driver for RISC OS 2 and 3 machines that have a VIDC1(a) Video Controller chip in them. It is maintained and bug fixed when required.

TODO File
=========

Create a RISC OS 3 only version that does not require all the MODE tables, instead relying on only the OS to report the best clock speed for the selected mode.

Contact
=======

AutoVIDC is maintained by Paul Vernon -- please [enquire on the retro-kit website](https://www.retro-kit.co.uk/contact) for more details.

# Assembling
## RISC OS

The code is best compiled using the extASM assembler for RISC OS.

### Released version

Latest version is 2.12 which is available [for download on the retro-kit website](https://www.retro-kit.co.uk/AutoVIDC)

### Notes

2.12 is the first version added to GitHub

Features
========

New Features
============

Version 2.12
------------

* Increased delay of initiating AutoVIDC after module load from floppy.

Version 2.11 
------------

* Added SWI AutoVIDC_EnhancerPresent
* Returns a value of 0 or 1 depending on whether an enhancer has been detected or not.
* Added SWI AutoVIDC_EnhancerType
* Returns a value of 0, 1 or 2 for Acorn, Aux I/O or I2C enhancers respectively.
* Widened the range for detecting the clocks to address a problem with clock detection when in high bandwidth modes (e.g. 28) on all machines and on the A5000 in any mode.

Version 2.10 - full release
------------

* removed unused variable definition
* added more comments in the code

Version 2.09 - beta release
------------

* Added clock detection code
* Added an SWI to query clock availability
* Re-arranged output for AutoVIDCStatus 
  * also indicates whether Aux IO VIDC Enhancer has been detected or is assumed
* altered code to use more "local" labels
* fixed bug in cleanup code for Native Enhancer support
* removed two "unpredictable" pieces of code
* Added/Removed TEMP/LOCK compiler directives
* code optimised to reduce build size - thanks to Jon Abbott for the quick lesson in optimising ARM code :D
* fixed a bug when enabling RO control after being disabled allowing the clock speed to be restored correctly
* now initiates a call to Service_ModeChange *after* the module has initialised. This addresses an issue where if QTM is playing a module before AutoVIDC has initialised it now correctly adjusts its timings after AutoVIDC has initialised.

Version 2.08 - beta release
------------

* Added full support for WE "Super" VIDC Enhancer with Sync. Polarity
* re-factored some source code to reduce module size
* fixed a minor bug with the WE Enhancer code
* fixed an issue with the AutoVIDCXXset command

Version 2.07
------------

* added in *MODE command to allow mode changes at the supervisor prompt
* Introduced support for detecting WE VIDC Enhancers

Version 2.06
------------

* added an SWI to allow AutoVIDC to be re-initialised to match the current MODE without requiring a MODE change service call to be triggered.
* Fix the DelegateControl SWI so that when control is returned to AutoVIDC, it calls the new re-initialise SWI also.

Version 2.05
------------

* added an SWI to return the Sync.Polarity details for any given Mode and MonitorType
* fixed an issue with correctly setting the Sync.Polarity when using the SetClock SWI on "native" enhanced VIDC1a based computers such as the A5000.

Version 2.04
------------

* added in support for running on later machines with VIDC1(a) chips installed.
* fixed RISC OS 3 25.175MHz MODE map by adding MODE 25 into the list and moving MODE 41-43 into the list from the VGA MODE map.
* fixed the OS version detection code.
* Built in hardware detection to detect VIDC1(a) machines that do and do not require a third party enhancer e.g. A540's and A5000's don't require one   
* When setting the VIDC clock, where necessary, the Zero page location containing the clock speed in kHz is updated to allow games that calculate sound playback do so more accurately. >= RO3
* Allow the user to set the speed of the over-clocking oscillator in kHz up-to 60000kHz (60MHz)
* Now provides a standard way of setting the VIDC clock speed across all VIDC1 based Acorn machines.

Version 2.03 - not released
------------

* many thanks to Jon Abbott and Steve Harrison for helping out and providing code
* examples and ideas that have made AutoVIDC better than I could have hoped for :-)
* Finally working on RISC OS 2. It seems RISC OS 2 support has never worked until now.
* 26/32-bit neutral. This isn't really needed but it's nice to have :D
* Added in a 25.175MHz mode list definition for RISC OS 2 as it has different screen geometry
* added in IRQ/FIQ disabling code to protect R/W of the Aux IO latch hardware.

Version 2.02 - not released
------------

* Re-factored the source code and modified to compile with ExtAsm.
* minor bug fixes 

Version 2.01
------------

* Added the RISC OS clock selection feature. It checks the clock speed in the VIDC register and if all the MODE lists do not specify that MODE but AutoVIDC recognises the clock speed then it will automatically switch clock speeds.
* Added '*' commands to enable and disable RISC OS clock selection
* NOTE: RISC OS clock selection is only available from RISC OS2.01 up.
* NOTE: The RISC OS clock selection feature could do away with MODE lists but RISC OS 2.00 would then be unsupported and if a user wanted to FORCE a specific clock speed, it couldn't be done.
* Ensured that all access to LatchB is in SVC mode with IRQ and FIQ disabled.

Version 2.00
------------
* Official release marking the allocation of the modname, *commands and SWI chunk base.
* Reverted to Module Name AutoVIDC as this is now officially allocated by RISC OS Open Ltd.

Thanks
======

* Andreas Barth for writing the original AutoVIDC module and turning over maintenance to myself
* Jon Abbott - for providing code and advice on writing efficient ARM Assembly Language for RISC OS
* Steve Harrison - for providing code and advice on writing efficient ARM Assembly Language for RISC OS
