:Title: Intel Management Engine Vulnerabilities
:Date: 2017-12-01
:Tags: Intel ME, ime
:Author: Char

=======================
Intel Management Engine
=======================

What is it?
-----------
A few months ago, there was a white paper 'Silent Bob is Silent' describing an Intel ME vulnerability. 
There were a whole slew of new vulnerabilities found `Intel ME CVEs <https://security-center.intel.com/advisory.aspx?intelid=INTEL-SA-00086&languageid=en-fr>`_

The best write up describing these new issues is in this Trend Micro 
`blog <http://blog.trendmicro.com/trendlabs-security-intelligence/mitigating-cve-2017-5689-intel-management-engine-vulnerability/>`_


What's vulnerable?
------------------

Everyone that bought a new computer in the last decade or so.
But might not be as disasterous as some have made it out to be. Assuming you don't have remote managability features enabled...

To get useful information, use this `IntelMEtool utility <https://github.com/zamaudio/intelmetool>`_
Make sure to follow the instructions and ``rmmod`` the me modules as well as issue a ``iomem=relaxed`` to your kernel boot args.

The output for this tool on my computer looks like this (without trying to invasively remove the Intel ME)::

 Bad news, you have a `Sunrise Point-H CSME HECI #1` so you have ME hardware on board and it is very difficult to remove, continuing...
 RCBA at 0x00000000
 MEI was hidden on PCI, now unlocked
 MEI found: [8086:a13a] Sunrise Point-H CSME HECI #1

 ME Status   : 0x90000245
 ME Status 2 : 0x86110306

 ME: FW Partition Table      : OK
 ME: Bringup Loader Failure  : NO
 ME: Firmware Init Complete  : YES
 ME: Manufacturing Mode      : NO
 ME: Boot Options Present    : NO
 ME: Update In Progress      : NO
 ME: Current Working State   : Normal
 ME: Current Operation State : M0 with UMA
 ME: Current Operation Mode  : Normal
 ME: Error Code              : No Error
 ME: Progress Phase          : Clean Moff->Mx wake
 ME: Power Management Event  : Pseudo-global reset
 ME: Progress Phase State    : Unknown 0x11

 PCI READ [bc] : 0x000000bc
 ME: Extend Register not valid

 ME seems okay on this board
 WRITE    [00] : CB: 0x80040007
 WRITE    [00] : CB: 0x000002ff                                                                                                                                                               
 READ     [08] : CB: 0x801c0007                                                                                                                                                               
 READ     [08] : CB: 0x000082ff                                                                                                                                                               
 READ     [08] : CB: 0x000b0000                                                                                                                                                               
 READ     [08] : CB: 0x0000049c                                                                                                                                                               
 READ     [08] : CB: 0x000b0000                                                                                                                                                               
 READ     [08] : CB: 0x0000049c                                                                                                                                                               
 READ     [08] : CB: 0x000b0000                                                                                                                                                               
 READ     [08] : CB: 0x0000049c                                                                                                                                                               
 ME: Firmware Version 11.0.1180.0 (code) 11.0.1180.0 (recovery) 11.0.1180.0 (fitc)                                                                                                            
 WRITE    [00] : CB: 0x80080007
 WRITE    [00] : CB: 0x00000203
 WRITE    [00] : CB: 0x00000000
 READ     [08] : CB: 0x800d0007
 READ     [08] : CB: 0x00008203
 READ     [08] : CB: 0x00000000
 READ     [08] : CB: 0x11194004
 READ     [08] : CB: 0x00000031
 ME Capability: Full Network manageability                 : OFF
 ME Capability: Regular Network manageability              : OFF
 ME Capability: Manageability                              : OFF
 ME Capability: Small business technology                  : OFF
 ME Capability: Level III manageability                    : OFF
 ME Capability: IntelR Anti-Theft (AT)                     : OFF
 ME Capability: IntelR Capability Licensing Service (CLS)  : ON
 ME Capability: IntelR Power Sharing Technology (MPC)      : OFF
 ME Capability: ICC Over Clocking                          : ON
 ME Capability: Protected Audio Video Path (PAVP)          : ON
 ME Capability: IPV6                                       : OFF
 ME Capability: KVM Remote Control (KVM)                   : OFF
 ME Capability: Outbreak Containment Heuristic (OCH)       : OFF
 ME Capability: Virtual LAN (VLAN)                         : ON
 ME Capability: TLS                                        : OFF
 ME Capability: Wireless LAN (WLAN)                        : OFF
 Re-hiding MEI device...done, exiting

Also you could get the official 'detection' utility from intel:
https://www.intel.com/content/www/us/en/support/articles/000025619/software.html

The output above gives me confidence my laptop is not remotely exploitable as the manageability features are all disabled. The VLAN capability is a little bit
disconcerting, but to help prove to myself its at least not remotely exploitable - scan the ports associated with the AMT features: 16992-16995 and 623 and 664.

That is not to say I am not affected. I am. However, the one remotely exploitable CVE is not likely an issue for my machine.

What can you do?
----------------

Remove Intel ME!
This is a pretty drastic step, and will require you are familiar with hardware. 
Specifically, you will need to physically connect to the flash chip on your motherboard that contains the Intel ME firmware code. You then dump the code to a local machine,
use a utility to attempt a patch and then re-program the chip. If it works, you can boot back up and see most functionality was disabled.

Follow this great `guide <https://wiki.gentoo.org/wiki/Sakaki%27s_EFI_Install_Guide/Disabling_the_Intel_Management_Engine>`_ to do everything possible 
to remove or disbable as much of the Intel ME as possible. Also refer to the `me_cleaner <https://github.com/corna/me_cleaner>`_ site which is used to make
the actual modifications to your dumped firmware.

