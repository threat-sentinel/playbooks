# Playbook: DDoS Attack

## Summary

This playbook structures the response to a distributed denial-of-service attack against websites, APIs, VPN access, DNS, mail servers or entire internet connections. It also covers extortion with a DDoS threat.

DDoS is first and foremost an **availability and coordination problem**. The most effective countermeasures often sit with the internet service provider, CDN or DDoS protection provider, not in your own infrastructure alone.

The core of this playbook: **First establish whether it is an attack and at which layer it happens, then bring in the right party with the right data.** A DDoS can also be a distraction from a parallel intrusion.

## Purpose

- Distinguish an attack from a technical fault, a load peak or a misconfiguration
- Determine the type and layer of the attack
- Activate defenses quickly through providers and your own measures
- Keep critical services available by priority
- Detect accompanying attacks
- Handle extortion attempts correctly

## Scope

Use this playbook if:

- publicly reachable services are slow or unreachable and traffic or requests are unusually high
- the internet service provider, CDN or protection provider reports an attack
- firewall, load balancer or web server report bandwidth, CPU, connections or error rates at their limit
- an extortion message with a DDoS threat arrives, even without an attack so far
- hacktivist groups publicly name your organization, sector or region as a target

**Boundaries:**

- Outages caused by your own changes, provider faults or legitimate load peaks are not attacks, but are ruled out systematically in phase 1 with this playbook.
- If a DDoS is only cover for an intrusion, the matching incident playbook additionally takes the lead.

## Severity

- **Medium:** The attack is absorbed by existing protection, no noticeable impact
- **High:** Noticeable impact or outage of non-critical services, or credible extortion
- **Critical:** Outage of business-critical services, loss of the entire internet connection, or signs of a simultaneous intrusion

## Incident Objectives

1. Establish whether an attack is happening and at which layer.
2. Activate defenses through providers and your own measures.
3. Restore critical services by priority or run them in a stable degraded mode.
4. Detect accompanying attacks.
5. Inform customers, internal stakeholders and partners reliably.
6. Improve protection permanently.

## Core Response Principles

- **Bring in providers early.** Against volumetric attacks, almost only what happens before your own line helps.
- **Provide data, not just an outage report.** Target IP, domain, time, layer, samples and measures already taken speed up every escalation.
- **Know priorities in advance.** Not every service is equally important.
- **Do not block blindly.** Blanket geo or IP blocks often hit legitimate users and only shift the problem.
- **Watch for distraction.** While availability has everyone's attention, intrusions can go unnoticed.
- **Do not pay.** Payments to DDoS extortionists do not prevent further attacks.
- **Record time consistently.** Document the start of the attack, waves and escalations in UTC.

## Contacts and Escalation

Pre-fill these for your organization and keep them available offline:

- Incident lead on call
- ISP or carrier NOC
- CDN, WAF and DDoS protection provider
- DNS provider, registrar and certificate owners
- Platform, network and application operations
- Customer service, communications and legal
- Police or cybercrime contact point: ZAC of the state criminal police office (DE), Federal Criminal Police Office (AT), cantonal police (CH)
- Reporting bodies: BSI (DE), competent CSIRT (AT), NCSC/BACS (CH)

## First 15 Minutes

- Name an incident lead, start the timeline and decision log.
- Record affected services, target IP addresses, domains and the time the disruption began.
- Quickly check whether a planned change, a known provider fault or a legitimate load peak explains it.
- Review monitoring: bandwidth, packets per second, connections, requests per second, error rates, source countries, source ASNs, top URLs and user agents.
- Check the CDN or protection service: is protection active, does the provider see the attack, does an emergency mode need to be activated?
- If your own uplink is saturated, contact the ISP or carrier through the NOC or emergency number.
- Explicitly ask security monitoring to keep watching for parallel anomalies.
- Prepare communication for the status page, support and internal updates.

## Roles and Responsibilities

### Incident lead

- Steers the incident.
- Prioritizes services and measures.
- Decides on far-reaching steps such as geo-blocking, challenge modes or blackholing.

### Network and operations

- Analyze traffic.
- Implement filters, rate limits and protection rules.
- Stay in contact with the ISP, CDN and DDoS protection provider.

### Application owners

- Assess application load.
- Activate caching, degradation, queueing or switch off expensive features.

### Security / SOC

- Monitor for accompanying attacks.
- Assess extortion messages and suspicious sign-ins.

### Communications and customer service

- Maintain the status page.
- Inform customers, internal stakeholders and support channels.

### External parties

- ISP, carrier, CDN, DDoS protection provider, DNS provider, CERT and, where needed, police

## Decision Model

### Rule 1: Attack or fault?

**Probably an attack** if:

- traffic or requests jump without a business reason
- many requests share the same characteristics, such as the same URL, user agent or protocol pattern
- traffic comes from unusual countries, networks or reflection sources
- there is a threat, a claim of responsibility or a public target announcement

**Probably a fault** if:

- changes were made shortly before the incident
- the provider confirms an outage
- traffic is normal, but backends or internal dependencies are slow

### Rule 2: Which layer?

| Layer | Typical characteristics | Most effective countermeasure |
|---|---|---|
| Volumetric (L3/L4) | Line saturated, lots of UDP, reflection traffic, high Gbit/s or PPS | ISP filtering, scrubbing center, CDN |
| Protocol (L4) | SYN flood, connection tables full, little bandwidth but many connections | Firewall and load balancer protection, SYN cookies, protection service |
| Application (L7) | Many seemingly legitimate HTTP requests against expensive functions | WAF rules, challenges, rate limits, caching, feature degradation |

### Rule 3: Geo-blocking or broad blocks?

Only apply if:

- the attack sources are clearly concentrated in a region
- no relevant customers, partners or employees are located there
- the measure is time-limited, documented and agreed with the business

Targeted rules on characteristics of the attack traffic are almost always better.

### Rule 4: Extortion?

- Do not pay and do not negotiate.
- Preserve the message completely: headers, wallet address, times, deadlines and wording.
- Give the protection provider and ISP an early warning.
- Inform the police or cybercrime contact point.
- Many threats remain technically limited, but preparation is still mandatory.

### Rule 5: When to activate other playbooks?

- **Identity compromise in parallel** if suspicious sign-ins, MFA anomalies or admin access become visible during the attack.
- **Ransomware playbook, or the playbook matching the intrusion, takes the lead** if the DDoS is evidently a distraction for an intrusion.
- **Data breach playbook in parallel** if data exfiltration or misuse of personal data becomes visible at the same time.

## Service Prioritization

| Service class | Goal during the attack | Typical measure |
|---|---|---|
| Critical transactions | Maximum availability | Scrubbing, CDN/WAF emergency mode, prioritization |
| Customer portal | Keep running in degraded mode | Caching, challenge, limit expensive features |
| Public information pages | Reliable communication | Static CDN page, independent status page |
| Non-critical APIs | Reduce load | Temporary limits, queueing, strict rate limits |
| Admin and remote access | Prevent misuse | Separate access, strong authentication, strict rules |

## Protecting Critical Trust Accounts

During a DDoS attack, take particular care to secure:

- DNS provider accounts
- Registrar access and registry lock, where available
- CDN and WAF admin accounts
- Certificate management and CA or ACME accounts
- Break-glass access for internet, DNS and CDN operations

## Phase 1: Detect and Triage

### Objectives

- Distinguish an attack from a fault
- Determine layer and target
- Trigger escalation with reliable data

### Actions

- Assess attack versus fault according to rule 1.
- Determine the layer according to rule 2.
- Record affected services, target systems and possible origin exposure.
- Preserve traffic data: NetFlow, firewall statistics, packet samples, web server logs, CDN and WAF analytics.
- Check whether the origin IP is known or directly attackable even though a CDN sits in front.

### Data package for the ISP and protection provider

A provider can only help as quickly as the situation can be grasped technically. Prepare this information as a standard template where possible:

- Affected target IP addresses, prefixes and domains
- Start of the attack in UTC and recognizable waves
- Bandwidth, packets per second, and for L7 requests per second
- Protocols and ports
- Sample source IP addresses, source ports, packet sizes
- For L7: URLs, methods, header or user agent patterns
- Measures already taken
- Desired outcome: filter, scrub, challenge, limit or, as a last resort, blackhole
- Business-critical services that must be protected or restored first
- Contract or customer number and a reachable technical contact

### Exit criteria

- Attack confirmed or ruled out with a reliable justification
- Layer and affected targets known
- ISP or protection provider informed
- First situation picture documented

## Phase 2: Contain

### Objectives

- Stabilize critical services
- Reduce attack surface and impact
- Do not overlook parallel attacks

### Actions

- Switch the protection service or CDN to emergency rules, challenge mode and rate limits.
- Have the ISP filter or reroute traffic through scrubbing.
- Protect your own infrastructure with rate limits, connection limits and targeted blocks on attack characteristics.
- Limit expensive functions such as search, login or exports, or put them behind a challenge.
- Provide a static emergency page via CDN or an independent status page.
- Protect the origin IP; if necessary change the origin and only allow traffic from the protection service.
- With cloud autoscaling, check budget limits and caps.
- For attacks on authoritative DNS, activate secondary zones or a robust DNS provider.
- Provide critical internal services through alternative paths if needed.
- Use blackholing only as a last resort.

### Do not

- No permanent blanket blocks without documentation
- Do not scale back security monitoring
- Do not provoke the attackers publicly

### Exit criteria

- Critical services reachable again or stable in degraded mode
- Protection measures active and documented
- Monitoring for accompanying attacks continues

## Phase 3: Investigate

### Objectives

- Understand the course of the attack
- Detect or rule out accompanying attacks
- Turn causes into lasting fixes rather than only situational defense

### Actions

- Document the course: start, peaks, waves, changes of method and response times.
- Check whether suspicious sign-ins, admin actions, malware findings or data exfiltration occurred in parallel.
- Determine how the origin IP or target infrastructure became known.
- Evaluate claims of responsibility, hacktivist channels and sector warnings.
- Check whether the attack was limited to availability or served as a distraction.

### Exit criteria

- Course of the attack documented
- Accompanying attack ruled out or handed over to another playbook
- Findings for lasting protection measures recorded

## Phase 4: Eradicate

A DDoS usually leaves no persistence. Eradication here means closing the conditions that made the attack effective.

### Actions

- Fix origin exposure
- Adopt proven WAF and rate limit rules permanently
- Address capacity bottlenecks and single points of failure
- Improve protection rules, routing or DNS design

### Exit criteria

- Origin only reachable in a controlled way
- Sensible emergency rules adopted into the permanent configuration
- Known weaknesses in the operating model addressed

## Phase 5: Recover

### Objectives

- Roll back emergency measures in a controlled way
- Detect a flare-up early

### Actions

- Withdraw emergency rules step by step and monitor the effect.
- Maintain heightened readiness for at least 72 hours after the last attack traffic.
- Update the status page.
- Give internal and external stakeholders the all-clear only after a stable period.

### Exit criteria

- 72 hours without renewed attack traffic, or stable defense
- Emergency rules reviewed and withdrawn or deliberately kept
- Communication updated

## Phase 6: Review

### Actions

- Measure the time to detection, escalation and effective defense.
- Check whether emergency contacts, contracts and escalation paths were current and reachable.
- Assess contractual protection in terms of bandwidth, L7 defense and response times.
- Update runbooks, protection rules and communication templates.
- Exercise the DDoS procedure regularly.

## Evidence to Preserve

- Monitoring graphs and raw data on bandwidth, PPS, requests and error rates
- NetFlow, firewall and load balancer statistics
- Web server, CDN and WAF logs with sample requests
- Communication with ISP, carrier and protection provider, including ticket numbers
- Extortion messages with full headers
- Timeline and decision log

## Communication Checklist

- Update the status page early, factually and without speculation about the attackers
- Equip customer service with agreed answers
- Communicate internally which services are affected and which alternatives exist
- In case of extortion, no public statement about demands without agreement from legal and the police

### Template: status update

> We are currently seeing increased load on [service], which may cause delays and outages. Our team and our service providers are working on the defense. Based on what we know so far, there are no confirmed indications that confidentiality or integrity have been affected; the investigation is ongoing. Next update: [time].

## Legal and Regulatory Considerations

Guidance, not legal advice (as of October 2026). Whether an obligation applies depends on the country, sector, size and role of the organization. Deadlines, reporting channels and contacts should be set in advance and confirmed with legal and data protection when an incident happens.

**Reporting obligations for cybersecurity incidents**

- **Germany — NIS2 (§ 32 BSIG, since 6 December 2025):** Particularly important and important entities report significant security incidents to the BSI: early warning within 24 hours, notification within 72 hours, final report within one month.
- **Austria — NISG 2026 (since 1 October 2026):** Essential and important entities report significant security incidents to the competent CSIRT: early warning within 24 hours, notification within 72 hours.
- **Switzerland — Information Security Act (ISA, since 1 April 2025):** Operators of critical infrastructure report cyberattacks to the Federal Office for Cybersecurity (NCSC/BACS) within 24 hours.

**Data protection**

- **Germany and Austria — GDPR Art. 33/34:** Report personal data breaches to the supervisory authority within 72 hours of becoming aware, unless the breach is unlikely to result in a risk to the individuals; if the risk is high, notify the individuals as well.
- **Switzerland — FADP Art. 24:** Report to the FDPIC as soon as possible if the breach is likely to result in a high risk for the individuals.

All deadlines run from awareness of the incident, not from the end of the technical investigation. An incomplete early warning within the deadline is better than a complete report after it.

**For this scenario**

- A DDoS without noticeable impact is usually not reportable. A significant outage of important services can be under NIS2 and the ISA.
- A pure availability attack is usually not a personal data breach. A GDPR assessment becomes necessary if data that individuals depend on was unavailable, or if an accompanying attack is found.
- **Criminal complaint:** Germany: Central Cybercrime Contact Point (ZAC) of the relevant state criminal police office; Austria: Federal Criminal Police Office or any police station; Switzerland: the cantonal police.
- Check SLAs towards customers and claims against the ISP, CDN or protection provider.

## Quick Response Checklist

- [ ] Attack or fault assessed
- [ ] Layer determined: volumetric, protocol or application
- [ ] Affected services and targets recorded
- [ ] Protection service or CDN activated or tightened
- [ ] ISP informed if the line is affected
- [ ] Origin exposure checked
- [ ] Security monitoring for accompanying attacks active
- [ ] Status page and customer service informed
- [ ] Extortion message preserved, nothing paid, police route checked
- [ ] Heightened readiness planned for 72 hours

## Preventive Controls

- DDoS protection for all business-critical public services
- Documented and tested emergency contacts at the ISP, carrier and protection provider
- Origin servers reachable only through the protection service
- Robust DNS provider with a sensible architecture and TTL strategy
- Rate limits for expensive functions and logins
- Independent status page
- Prioritized list of critical services with owners
- Regular exercises of the DDoS procedure
- Protection of highly privileged trust accounts for DNS, CDN and certificates

## At a Glance

This block is not a shortened replacement playbook. It is a quick way in; all sections of this document apply when handling the incident.

**DDoS Attack**  
Response to flooding attacks and DDoS extortion: distinguish an attack from a fault, determine the layer, bring in the ISP and protection provider with reliable data, prioritize critical services and watch for accompanying attacks.

**Core phases:** Detect, Contain, Investigate, Eradicate, Recover, Review.

## References

- NIST SP 800-61 Rev. 3
- CISA, FBI, MS-ISAC: Understanding and Responding to Distributed Denial-of-Service Attacks
- BSI recommendations on defending against DDoS attacks
- ENISA Threat Landscape on availability threats
- MITRE ATT&CK: T1498, T1499

## Disclaimer

This playbook is a practical community resource. It does not replace legal advice, the requirements of your service providers or organization-specific incident response planning.
