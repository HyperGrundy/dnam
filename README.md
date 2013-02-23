# DNAM: Domain Name Agreement Mesh

This is a project to develop a decentralized DNS alternative for CJDNS based mesh networks (primarily Hyperboria). If you are new to DNAM, you may want to give a quick read through /doc/INTRO.md for an understanding of the ideas behind it. This project is still at a very early stage and is subject to drastic changes in methods and goals, however this is the current approach being pursued:

 * DNAM will consist of a service written in python which handles consensus with other members and provides for configuration and management of the member's name server.
 
 * Bind 9 is the current target name server.
 
 * Each DNAM member's name server will act as an independent DNS root master as well as the master for it's own TLD zone, and a slave for the TLD zones of the other members, with zone configuration between all members kept in sync by the DNAM service. The DNAM service will also keep each member updated with the latest ICANN TLDs as well.
 


## Name server configuration

The DNAM system seeks to provide an "agreement mesh" of TLD master servers that simultaneously act as DNS root masters, hereafter referred to as DNAM members, rather then the traditional top down hierarchy DNS system of the internet. As such, every individual member of a DNAM is configured as the DNS root master, i.e. the master of the "." zone. In the traditional system, name servers stay synchronized with each other by all being subordinate to the same master, however in the DNAM system, that synchronization is handled by the DNAM service instead. This cooperative synchronization is intended to allow for all members of the DNAM to remain at an equal level of authority, while still maintaining independent control of their respective TLDs. The nature of the DNAM also prevents the member's name servers from subordinating to the ICANN root master, so the DNAM service will also need to ensure that each member has up-to-date zone information for ICANN TLDs as well, so that client users of the DNAM do not loose the ability to resolve standard internet domains.

The result is that each member's name server will identify itself as the root (and will identify itself as "ns.dnam.", the root NS, as well), will identify itself as an authority for all member's TLDs (functioning as master for it's own, and slave for the others), and will provide NS records for all ICANN TLDs as well.

## Requirements of then DNAM service

In order to accomplish it's goals, the DNAM service will need to meet the following requirements:

 * It must securely and reliably communicate with the other members of it's DNAM, and with prospective new members.
 
 * It must securely and reliably handle join requests and consensus reaching.
 
 * It must properly configure the name server according to the server's role as a DNAM member and TLD master.
 
 * It must keep the name server's root zone properly updated and synchronized with the ICANN root zone.
 
 * It must identify any new conflicts with the ICANN root zone and resolve them successfully, preserving interoperability.
 

