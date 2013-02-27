# DNAM: Domain Name Agreement Mesh

This is a project to develop a decentralized DNS alternative for CJDNS based mesh networks (primarily Hyperboria). If you are new to DNAM, you may want to give a quick read through /doc/INTRO.md for an understanding of the ideas behind it. This project is still at a very early stage and is subject to drastic changes in methods and goals, however this is the current approach being pursued:

 * DNAM will consist of a service written in python which handles consensus with other members and provides for configuration and management of the member's name server.
 
 * Bind 9 is the current target name server.
 
 * Each DNAM member's name server will act as an independent DNS root master as well as the master for it's own TLD zone, and a slave for the TLD zones of the other members, with zone configuration between all members kept in sync by the DNAM service. Optionally, the DNAM service will also keep each member updated with the latest ICANN TLDs as well.
 


## Name server configuration

The DNAM system seeks to provide an "agreement mesh" of TLD master servers that simultaneously act as DNS root masters, hereafter referred to as DNAM members, rather then the traditional top down hierarchy DNS system of the internet. As such, every individual member of a DNAM is configured as the DNS root master, i.e. the master of the "." zone. In the traditional system name servers stay synchronized with each other by all being subordinate to the same master, however in the DNAM system that synchronization is handled by the DNAM service instead. This cooperative synchronization is intended to allow for all members of the DNAM to remain at an equal level of authority, while still maintaining independent control of their respective TLDs. The nature of the DNAM also prevents the member's name servers from subordinating to the ICANN root master, so the DNAM service will also need to be able to ensure that each member has up-to-date zone information for ICANN TLDs as well, so that client users of the DNAM do not loose the ability to resolve standard internet domains. Wether that capability is enabled will be up to the individual member, and requires that the name server have commercial internet access available, however it is expected that it will be enabled where possible, and members should keep in mind that ICANN TLDs are blacklisted reguardless. Client users of member servers who do not enable that option will not get resolution for ICANN TLDs from that server, only for DNAM TLDs, so if they require resolution for ICANN TLDs, they will need software on their node that can differentiate requests and send them to the correct name servers.

The result is that each member's name server will identify itself as the root (and will identify itself as "ns.dnam.", the root NS, as well), will identify itself as an authority for all member's TLDs (functioning as master for it's own, and slave for the others), and will be able to provide NS records for all ICANN TLDs as well if configured to do so.



## Requirements of the DNAM service

In order to accomplish it's goals, the DNAM service will need to meet the following requirements:

 * It must securely and reliably communicate with the other members of it's DNAM, and with prospective new members.
 
 * It must securely and reliably handle join requests and consensus reaching.
 
 * It must properly configure the name server according to the server's role as a DNAM member and TLD master.
 
 * It must be able to keep the name server's root zone properly updated and synchronized with the ICANN root zone for those that use this option.
 
 * It must identify any new conflicts with the ICANN root zone and resolve them successfully, preserving interoperability.
 


## DNAM member rules

In order to function successfully, all DNAM members must operate acording to the same specifc set of rules. In effect, these rules act as a replacement for a single central authority (much the same way that a constitution and associated body of law replaces a monarch or dictator in a constitutional democracy). These rules will evolve as the details are hashed out, but the current set of rules that DNAM members MUST follow are:

 1. Members MUST be available to participate in consensus/request handling.
 
 2. Members MUST approve all valid join requests unless the prospect is known to be malicious. Censorship of any kind is considered malicious operation and makes the member subject to challenge for ejection.
 
 3. Members MUST particpate in the enforcement of these rules against other members.

 4. Each member may be master of exactly ONE top level domain (if you want another one, you need another node).
 
 5. Every member MUST include the correct necessary NS and AAAA records for all member TLDs in their root zone.

 6. Every member MUST be available to resolve any other member's TLD (a client should need to contact only one member to resolve the entire DNAM).

The above rules are intended to be the definitive set, with everything else being efectively optional and up to the individual members preference (presumably they should take into account the preferences of their subordinant domains and clients, if they have any).




