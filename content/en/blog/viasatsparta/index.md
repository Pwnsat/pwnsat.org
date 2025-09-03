---
title: "Lessons from the Viasat Cyberattack: A SPARTA Framework Analysis"
description: "Analysis of the Viasat KA-SAT cyberattack through the MITRE SPARTA framework. A case study that highlights vulnerabilities in the ground segment of satellite systems and the rise of tailored malware against space infrastructure."
draft: false
weight: 50
images: ["Cover.png"]
categories: ["Space Cybersegurity", "Case Study", "Satellite Communications", "SPARTA"]
tags: ["Viasat", "KA-SAT", "SPARTA", "AcidRain", "Space Security", "Cyberattack", "Ground Segment", "Satellite", "Pwnsat"]
contributors: ["Kevin Leon"]
author: ["Kevin Leon"]
pinned: false
homepage: false
---

In February 2022, one of the most notable cyber incidents in the space sector took place: the **Viasat KA-SAT cyberattack**. This event disrupted satellite communications across Europe, affecting both civilian and military users. For the cybersecurity and space communities, the incident stands as a turning point, highlighting how vulnerable the ground and user segments of space systems can be when targeted with coordinated cyber operations.

In this post, I’ll walk through a simplified analysis of the attack using the [**SPARTA framework**](https://sparta.aerospace.org/), which is specifically designed for threats against space systems. This is not an exhaustive case study, but rather an exercise as part of the Pwnsat project, where we explore practical cybersecurity lessons for satellite missions. More detailed research and technical breakdowns will follow in future posts.

## Mapping the Attack with SPARTA

Using SPARTA’s tactics and techniques, the Viasat attack can be illustrated in several phases:

### Reconnaissance

{{< figure src="ST0001.png" alt="Sparta VIASAT Diagram" caption="Gather Information diagram" >}}

Adversaries gathered information on Viasat’s ground systems, modems, and VPN infrastructure. Publicly known vulnerabilities in Fortinet SSL VPNs (see Fortinet disclosure) were among the potential entry points.

### Resource Development

{{< figure src="ST0002.png" alt="Sparta VIASAT Diagram" caption="Resource Development diagram" >}}

Before execution, attackers prepared their infrastructure and staged capabilities, including the `AcidRain` wiper malware. AcidRain was specifically designed to wipe the firmware and configurations of satellite modems. (SentinelOne report).

### Initial Access

{{< figure src="ST0003.png" alt="Sparta VIASAT Diagram" caption="Initial Access diagram" >}}

By exploiting VPN access and pivoting through the DMZ, adversaries were able to reach the Ground Segment management systems. This step was critical, as it allowed them to issue legitimate-looking commands to end-user modems.

### Execution and Disruption

{{< figure src="ST0004.png" alt="Sparta VIASAT Diagram" caption="Disruption diagram" >}}

From there, carefully chosen beam spots were targeted. Using legitimate management channels, the attackers deployed AcidRain, which rendered tens of thousands of modems inoperable. The result was a large-scale loss of connectivity in Ukraine and several European countries at the onset of the war.

## Diagram: SPARTA View of the Viasat Attack

The diagram was designed using [Attack Flow](https://github.com/center-for-threat-informed-defense/attack-flow), click [here](SPARTA_VIASAT.afb) to download.

{{< figure src="viasat_sparta.png" alt="Sparta VIASAT Diagram" caption="Attack Flow diagram for VIASAT using SPARTA framework" >}}

> **Note**: This visualization is a high-level abstraction and does not represent the full complexity of the event.

## Key Takeaways

- Ground systems are the soft belly of space assets: Even if satellites remain untouched, compromising terrestrial infrastructure can have devastating consequences.
- Malware tailored for embedded devices is on the rise: AcidRain is an example of how attackers are moving beyond IT malware to purpose-built tools for operational technology.
- Supply chain and configuration security matter: VPN misconfigurations or leaked credentials can become gateways to critical infrastructure.

## Final Notes

This analysis is based on open-source reporting and public research, including:
- [Fortinet PSIRT](https://www.fortinet.com/blog/psirt-blogs/malicious-actor-discloses-fortigate-ssl-vpn-credentials)
- [ResearchGate: Space Cybersecurity Lessons Learned from the Viasat Cyberattack](https://www.researchgate.net/publication/363558808_Space_Cybersecurity_Lessons_Learned_from_The_ViaSat_Cyberattack#pf8)
- [SentinelOne AcidRain Report](https://www.sentinelone.com/labs/acidrain-a-modem-wiper-rains-down-on-europe/)
- [Viasat Corporate Overview](https://www.viasat.com/perspectives/corporate/2022/ka-sat-network-cyber-attack-overview/)

## Education Note

Again, this is not an exhaustive analysis, but rather part of the ongoing work in the Pwnsat project, where we study space cybersecurity from a practical, hands-on perspective. Future posts will continue to expand on these lessons and how they apply to satellite security research and training.

At Pwnsat, our goal is to create the most realistic simulated environment possible. That’s why we analyze real-world cases like the Viasat cyberattack — they allow us to keep improving our mission. We work to develop unique, hands-on content and build scenarios and challenges that enable learners to acquire the knowledge necessary to understand, prevent, and mitigate these types of incidents.