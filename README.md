# TSN/AVB document collection
This repo collects documentations and examples for TSN/AVB studies.

[General docs](doc/)

[Application notes](doc_an/)

[Hardware docs](doc_hw/)

[MAC+Phy HW descriptions](doc_hw/hw.md)

[Phy hw descriptions](doc_hw/phy.md)

[Packet capture examples](pcap_examples/)

First generation
----------------
The Audio Video Bridging technology defines mechanisms (IEEE 802.1BA) which allows to recognizes all the AVB-compatible devices in the network. Basically, AVB works only within a domain of directly connected AVB-compatible network devices (i.e. bridges or endpoints). These devices support precise synchronization (IEEE 802.1AS) and enable reservation of their resources (IEEE 802.1Qat) for a data stream. This way, and using special traffic shaping (IEEE 802.1Qav), deterministic and low latency communication (stream) between AVB-compatible endpoints though AVB-compatible network is achieved.

IEEE 802.1AS:
Timing and Synchronization for Time-Sensitive Applications (gPTP): very tightly-constrained subset of PTP (which, except for Ethernet, supports wireless and Coordinated Shared Networks)
it is based on, and includes a profile of, PTP (i.e. IEEE 1588-2008)

- topology/network-functionality-wise: an IEEE 802.1AS bridge acts as an PTP boundary clock, and an IEEE 802.1AS end station acts as an ordinary clock [1]
synchronization-wise: the manner in which an IEEE 802.1AS bridge transports synchronization is very similar (mathematically equivalent) to the manner in which an PTP peer-to-peer transparent clock (TC) transports synchronization [1]
- each bridge measures frequency offset relative to its neighbors; the accumulated frequency offset relative to the GM and the time difference between the arrival of a Sync message on the slave port and the sending of a subsequent Sync message on a master port are used to construct the synchronized time placed in a Follow_Up message [1]
- IEEE 802.1AS defines TLV which is attached to Follow_UP message, this TLV conveys frequency offset, relative to the grandmaster, it is accumulated (added to by each bridge on the way)
- propagation delays between neighboring bridges and/or end stations are measured using the peer delay mechanism [1]
- the bridge is not required to filter phase as part of transporting synchronization; all phase filtering occurs at endpoints (end stations or bridges that have service interfaces to applications) [1]
- it defines modified BMC (but similar to original)
- synchronization spanning tree may be different from the forwarding spanning tree (i.e. RSTP) [2]
- no synchronization over non 802.1AS bridges, not inter-operable with default PTP
- only peer-delay two-step mechanism allowed, only single domain
- defines application interfaces
- IEEE 802.1AS devices are logically syntonized (time-aware Bridges can correct time interval measurements using the grandmaster frequency ratio conveyed in Follow_UP's TLV, p22 of [2]). Optional features [1]: IEEE 802.1AS bridges and end stations are not required to physically syntonize (using frequency ratio) their frequency to the GM frequency (though they are be allowed to do this).

performance requirement, Annex B of [2]:
- synchronization over seven or fewer hops to within 1us peak-to-peak of each other during steady-state operation,B.3 of [2]
- jitter and wander defined in B.4 of [2], page 250

IEEE 802.1Qat: Stream Reservation Protocol (SRP):
it utilizes three signaling protocols, MMRP, MVRP and MSRP to establish stream reservations (register stream and reserve resources) across a bridged network between two end stations (talker and listener)
- Talkers (source) initiates stream by sending an SRP talker advertise message which include:
 - Stream ID (source MAC, talker-specific unique ID and destination MAC)
 - QoS requirements (e.g., AVB traffic class and data rate information),
 - accumulated worst case latency (recalculated at each bridge)
- Listener (destination) acknowledges by replaying with listener ready message
the signaling protocols:
- Multiple MAC Registration Protocol (MMRP) is optionally used to control the propagation of the Talker's registrations throughout the bridged network (clause 10.9 of [1])
- Multiple VLAN Registratin Protocol (MVRP) - used by end stations and bridges to declare membership in a VLAN where a stream is being sourced (clause 11 of [1])
- Multiple Stream Registration Protocol (MSRP) - a signaling protocol that provides end stations (talker and listener) with the ability to reserve network resources (ensure Quality of Service), between end stations (clause 35.1 of [1])
stream can be de-registered by either Talker or Listener
references [1]

IEEE 802.1Qav: Forwarding and Queuing for Time-Sensitive Streams (FQTSS)
- it defines constraints for mapping of priorities into traffic classes
 - AVB traffic is transmitted/forwarded using credit-based shaper algorithm
 - AVB traffic has higher priority then traffic supporting strict priority, or other transmission algorithm (e.g. non-AVB traffic)
- it specifies credit-based shaper algorithm
 - frames distributed evenly in time (only on an aggregate class basis), prevents "bunching" of frames
 - effect of smoothing out the devliery times
- references: [1]

IEEE 802.1BA: Audio Video Bridging Systems
- specifies the default configuration of AVB devices in a network
- for Ethernet, the method specified by 802.1BA to determine if its peer is AVB-capable is a combination of 802.3 link capabilities (determined during Ethernet link establishment) and the link delay measurements done by IEEE 802.1AS

Second generation
----------------
The AVB Gen2 is developed to further enhance AVB's performance. The requirements for AVB Gen2 are defined not only by Audio/Video industry but others as well (described below). In order to meet the requirements, the mechanisms defined in AVB Gen 1 are improved and new solutions are investigated. 
Improvements to AVB Gen 1:
- P802.1ASbt: Timing and Synchronization for Time-Sensitive Applications
- P802.1Qbv : Enhancements for Scheduled Traffic

New ideas:
- P802.1Qbu : Frame Preemption
- Static/dynamic redundancy (potentially included into AVB Gen2)

