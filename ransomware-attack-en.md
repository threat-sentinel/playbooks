# Ransomware Attack Playbook

## Executive Summary

This playbook provides a practical, community-oriented response structure for suspected or confirmed ransomware incidents. It is designed to help responders make fast, defensible decisions under pressure, reduce further damage, preserve evidence, protect identity and backup infrastructure, and restore business services only from trusted states.

The playbook follows established incident response logic aligned with preparation, detection and analysis, containment, eradication, recovery and post-incident review. It is intentionally written for operational use by security teams, IT administrators, consultants, incident responders and small to mid-sized organizations.

This document does not replace legal advice, forensic expertise, cyber insurance requirements, or organization-specific incident response plans. It should be adapted to local architecture, critical services, legal obligations, and named responsibilities.

## Purpose

Provide a structured response process for confirmed or suspected ransomware incidents, including:

- Early detection and validation.
- Fast containment with minimum additional damage.
- Evidence preservation for technical, legal and regulatory purposes.
- Scope analysis across endpoints, servers, identity, cloud and backups.
- Safe eradication and trusted recovery.
- Documented lessons learned and control improvements.

## Scope

Use this playbook when one or more of the following indicators are observed:

- Files are encrypted, renamed or made inaccessible unexpectedly.
- Ransom notes are found on endpoints, servers, virtual machines or file shares.
- Users report inaccessible files, applications or systems with signs of mass encryption.
- Security tools detect ransomware behavior, mass file modification, shadow copy deletion, backup tampering, suspicious scheduled tasks or security control suppression.
- Backup systems, domain controllers, hypervisors, file servers or management consoles show suspicious administrative activity.
- There are signs of data theft, extortion, leak-site threats or unauthorized large-scale data transfer.
- Threat actor activity indicates ransomware staging, even if encryption has not yet started.

This playbook also applies to precursor events that often occur before ransomware deployment, such as domain-wide malicious tool rollout, mass credential abuse, remote administration misuse, widespread security tooling tampering or simultaneous privilege escalation across multiple systems.

## Severity

Default severity: **Critical**.

Escalate immediately if any of the following are affected or suspected:

- Domain controllers or core identity systems.
- Backup servers, backup repositories, immutable storage or replication controls.
- Hypervisors, virtualization management or storage administration.
- File servers, ERP, finance, production, logistics or other business-critical systems.
- Identity providers, SSO, MFA, VPN or remote administration systems.
- EDR, logging, SIEM or other core security tooling.
- Cloud tenant administration or privileged SaaS administration.
- Any evidence or credible suspicion of data exfiltration.

## Incident Objectives

During the first hours of a ransomware incident, the response team should work toward six operational objectives:

1. Stop the spread.
2. Preserve evidence and decision quality.
3. Protect identity, backups and management planes.
4. Determine scope, entry vector and attacker path.
5. Recover critical services from trusted states only.
6. Close the incident with lessons learned and tracked improvements.

## Core Response Principles

- Preserve evidence before destructive remediation.
- Contain first, rebuild later.
- Do not restore systems into an untrusted environment.
- Treat identity compromise as likely until disproven.
- Protect backups as early as possible.
- Separate confirmed facts from assumptions and working hypotheses.
- Maintain a single incident timeline and decision log.
- Prefer trusted rebuild over partial cleanup where compromise is material.
- Do not let business urgency override recovery trust decisions.

## First 15 Minutes

- Activate the incident response lead or incident commander.
- Start an incident timeline and decision log.
- Confirm whether encryption or attacker activity is still active.
- Isolate actively affected systems from the network where feasible.
- Do not power off systems unless encryption is actively spreading and no safer isolation method exists.
- Preserve immediately available evidence: alerts, screenshots, ransom notes, timestamps, hostnames, usernames and system context.
- Identify potentially affected network segments, users, business services, servers and file shares.
- Protect backups immediately by restricting administrative access, stopping deletion/tamper paths and verifying immutable or offline copies.
- Review suspicious privileged accounts, especially backup admins, domain admins, tenant admins and remote access administrators.
- Inform management, legal/privacy stakeholders and crisis communication contacts according to policy.

## Roles and Responsibilities

### Incident Commander

- Owns overall coordination and severity decisions.
- Approves major containment and recovery actions.
- Ensures a single operational picture and decision log.
- Coordinates executive reporting.

### Technical Lead

- Coordinates endpoint, server, network, identity, cloud and backup responders.
- Maintains the technical working hypothesis.
- Drives containment, investigation and eradication priorities.

### Identity and Access Lead

- Reviews privileged account misuse, new admin accounts, token/session abuse, MFA status and remote access paths.
- Coordinates credential resets and session invalidation in the correct order.

### Infrastructure and Recovery Lead

- Coordinates system isolation, rebuilds, restore sequencing and recovery validation.
- Works with server, virtualization, storage and backup teams.

### Forensics Lead

- Preserves and acquires evidence.
- Validates attacker activity, persistence and indicators of compromise.
- Advises when systems should remain powered for evidence purposes.

### Communications Lead

- Coordinates internal, customer, partner, regulator and public communication.
- Prevents unsupported or inconsistent statements.

### Legal / Privacy / Compliance

- Assesses legal, contractual, regulatory and data protection obligations.
- Coordinates notification requirements where applicable.
- Advises on evidence handling and external communication constraints.

### Management / Crisis Team

- Decides on business continuity priorities, external support, major operational tradeoffs and escalation paths.

### External IR / Law Enforcement / Insurance

- Engaged according to company policy, cyber insurance requirements and incident severity.

## Decision Model

### Decision Rule 1: Isolate or monitor?

**Isolate immediately** if any of the following are true:

- Encryption is active.
- A system is spreading malicious activity to shares or other hosts.
- A privileged system shows suspicious remote administration or tooling tampering.
- The affected asset is business-critical and compromise confidence is high.

**Monitor briefly before aggressive isolation** only if:

- There is no sign of ongoing destructive activity.
- Evidence collection from a high-value system would be significantly harmed by immediate shutdown or isolation.
- The incident commander and forensics lead explicitly agree.

### Decision Rule 2: Shut down or keep powered on?

**Keep powered on** if:

- Memory evidence is needed.
- EDR isolation is available.
- Encryption is no longer actively progressing.
- Forensic context would be lost by immediate shutdown.

**Shut down only if**:

- Network isolation is impossible.
- Encryption is actively spreading and causing major harm.
- The host is destroying backup access, mass-encrypting shared storage or threatening safety-critical operations.

### Decision Rule 3: Clean or rebuild?

**Prefer rebuild** if:

- Administrative access was obtained.
- Security tooling was disabled or tampered with.
- Persistence cannot be confidently excluded.
- The system is internet-facing, privileged or business-critical.
- The compromise path is not fully understood.

**Cleaning may be acceptable** only for low-value systems with high-confidence visibility and limited compromise scope.

### Decision Rule 4: Is recovery allowed yet?

Recovery should start only when all four conditions are met:

1. The likely access vector is closed or compensating controls are active.
2. High-risk credentials and sessions have been reset or revoked.
3. The backup or image source is trusted.
4. Monitoring and containment controls are active on the restored environment.

## Priority Protection Blocks

### Identity First

Treat identity as a crown-jewel function during ransomware response. Many ransomware events become identity incidents before they become encryption incidents.

Priority checks:

- Domain admin, enterprise admin, backup admin and tenant admin activity.
- Newly created privileged accounts or group changes.
- MFA disablement, re-registration or bypass.
- New OAuth grants, service principals, API tokens or suspicious federation changes.
- VPN, RDP, jump-host and remote management access.
- Session/token persistence in cloud and SaaS services.

### Backup and Recovery Protection

Backups are not just recovery assets; they are active attack targets.

Priority actions:

- Restrict admin access to backup systems and repositories.
- Preserve backup logs and management actions.
- Review deletion attempts, failed jobs, retention changes and immutable storage status.
- Separate trusted recovery administration from potentially compromised identities.
- Do not assume available backup equals safe backup.

## Phase 1: Detect & Triage

### Objectives

- Confirm whether the event is ransomware, data extortion, destructive malware or another incident type.
- Determine whether attacker activity is active.
- Identify the first known scope.
- Preserve the evidence that will disappear first.

### Actions

- Collect initial reports from users, helpdesk, EDR, SIEM, firewall, email gateway, remote access systems and backup tools.
- Identify patient-zero candidates or first-known affected assets.
- Record the first known timestamp and discovery source.
- Search for ransom notes, encrypted file extensions, suspicious scheduled tasks, security tool tampering and abnormal processes.
- Determine whether encryption is local, file-share based, server-side or hypervisor/storage related.
- Review identity systems for suspicious sign-ins, privilege escalation, newly created admin accounts and MFA changes.
- Review VPN, RDP, remote monitoring tools, remote support software and cloud admin access.
- Check for signs of data staging, large outbound transfers or extortion-related exfiltration.
- Create and maintain an initial incident timeline.

### Exit Criteria

Move to formal containment when all of the following are defined at least preliminarily:

- Incident type is sufficiently validated.
- Active spread risk is assessed.
- Initial affected systems/accounts are identified.
- Backup and identity protection measures have started.

## Phase 2: Contain

### Objectives

- Stop further spread.
- Protect crown-jewel systems.
- Preserve recovery options.
- Avoid preventable evidence destruction.

### Actions

- Isolate affected endpoints and servers from the network.
- Restrict lateral movement paths such as SMB, RDP, WinRM, WMI, PSExec, SSH and remote support tooling as appropriate.
- Disable compromised or suspicious accounts, beginning with privileged identities where evidence supports risk.
- Revoke suspicious sessions and tokens.
- Block known malicious indicators if confidence is sufficient.
- Temporarily restrict VPN and remote administration access if the attack path is not yet understood.
- Isolate critical servers or segments if suspicious activity is detected there.
- Protect backup infrastructure from deletion, encryption, retention change or replication tampering.
- Suspend synchronization to cloud storage if encrypted or malicious data is being replicated.
- Preserve forensic images or memory captures for selected high-value systems.

### Do Not

- Do not immediately wipe all affected systems.
- Do not restore systems into a still-compromised environment.
- Do not delete ransom notes, scripts, tools or artifacts before collection.
- Do not assume the incident is limited to visibly encrypted systems.
- Do not reset all credentials at once without a controlled sequence.
- Do not let unaffected administrators use potentially compromised admin workstations.

### Exit Criteria

Proceed to deeper investigation when:

- Active spread is reduced or paused.
- High-risk assets are shielded.
- Backup and identity controls are under active review.
- Enough stability exists to build a reliable scope picture.

## Phase 3: Investigate

### Objectives

- Determine initial access.
- Determine attacker path and privilege escalation.
- Identify persistence.
- Assess exfiltration.
- Determine safe recovery boundaries.

### Actions

- Analyze authentication logs for suspicious successful sign-ins, impossible travel, unusual source IPs, token abuse and abnormal admin activity.
- Review privileged account use, service account activity and group membership changes.
- Review remote access systems including VPN, RDP gateways, exposed services, remote monitoring tools and jump hosts.
- Check scheduled tasks, services, startup folders, GPO changes, WMI event subscriptions and remote execution artifacts.
- Review PowerShell, WMI, PsExec, SMB, WinRM and RDP evidence for lateral movement.
- Analyze outbound traffic for large transfers, archive staging, cloud sync misuse or unusual destinations.
- Identify exploited vulnerabilities, weak authentication paths or missing patches.
- Determine whether backups predate the compromise and whether management planes were touched.
- Build the preliminary attack timeline: access, privilege escalation, discovery, lateral movement, exfiltration, encryption.

### Scope Categories

Document impact in these categories:

- Affected hosts.
- Affected accounts and service identities.
- Affected servers and applications.
- Affected shares, databases and storage.
- Affected business processes.
- Affected backup/recovery assets.
- Affected security tooling and logs.
- Suspected exfiltrated datasets.

### Exit Criteria

Proceed to eradication planning when:

- A plausible access path exists.
- Trust boundaries crossed by the attacker are understood well enough to act.
- Priority compromised systems and accounts are identified.
- Recovery candidates can be separated from non-trusted assets.

## Phase 4: Eradicate

### Objectives

- Remove attacker access.
- Eliminate persistence.
- Close the initial access vector.
- Prepare trusted recovery.

### Actions

- Remove malware, backdoors and persistence mechanisms where appropriate.
- Rebuild compromised systems where trust cannot be restored.
- Patch exploited vulnerabilities and close exposed services.
- Rotate credentials in the correct order: privileged accounts first, then backup/security admins, then service accounts and application secrets, then broader user scope.
- Reset Kerberos-related secrets if domain compromise is suspected.
- Remove unauthorized accounts, groups, mailbox rules, scheduled tasks, scripts and remote access methods.
- Revoke suspicious sessions, tokens, OAuth grants and cloud persistence methods.
- Re-enable or strengthen MFA where bypassed or weakened.
- Validate EDR, logging, identity monitoring and backup protections before recovery.

### Exit Criteria

Proceed to recovery only when:

- Access vector closure is confirmed or compensating controls are live.
- High-risk identities are remediated.
- Persistence mechanisms are removed or systems are rebuilt.
- Security monitoring is functioning in the recovery path.

## Phase 5: Recover

### Objectives

- Restore business services safely.
- Prevent reinfection.
- Validate data integrity and service trust.
- Monitor aggressively during recovery.

### Recovery Gate

Do **not** begin broad recovery until the following are true:

- The likely attacker path is understood well enough to avoid immediate re-entry.
- Privileged identities are reset and administrative workstations are trusted.
- Backup or image sources are validated.
- Business owners agree on recovery priorities.
- Monitoring and containment controls are active on restored assets.

### Actions

- Define recovery order by business criticality and dependency mapping.
- Restore from clean, verified backups or trusted golden images.
- Scan and validate restored systems before reconnecting them.
- Bring systems online in controlled segments first.
- Validate application functionality, business data integrity and authentication behavior.
- Monitor authentication, network traffic, file activity and endpoint behavior during recovery.
- Keep enhanced monitoring active for at least 14 to 30 days, depending on scope and confidence.
- Communicate service restoration status and restrictions to stakeholders.

### Suggested Recovery Priority

1. Trusted admin workstations and identity foundations.
2. DNS, DHCP, NTP and core network/security services.
3. Backup and recovery orchestration.
4. Logging, EDR and security visibility.
5. Core file, database and application services.
6. Business-critical applications and integrations.
7. End-user systems.
8. Non-critical services.

### Exit Criteria

Proceed to closure activities when:

- Critical services are restored and accepted by owners.
- No active recurrence indicators are observed.
- Monitoring confirms stable operation.
- Outstanding risk items are documented and owned.

## Phase 6: Review / Lessons Learned

### Objectives

- Understand root cause and spread factors.
- Improve controls and governance.
- Finalize evidence and decision records.
- Prevent recurrence.

### Actions

- Conduct a post-incident review within 5 to 10 business days after stabilization.
- Finalize the incident timeline and key decisions.
- Confirm the initial access vector and escalation path where possible.
- Identify control failures in identity, segmentation, logging, EDR, patching, backup resilience and administrative governance.
- Review backup and recovery performance.
- Review communication, escalation and executive decision quality.
- Update detections, hardening baselines and response procedures.
- Update this playbook based on lessons learned.
- Track remediation actions to named owners and closure dates.

## Evidence to Preserve

Preserve at minimum:

- Ransom notes and screenshots.
- File extension samples and encrypted file examples.
- Malware samples, loaders or scripts if safely collectible.
- EDR detections and alert metadata.
- Windows Event Logs and Linux system logs where relevant.
- Firewall, proxy, VPN and DNS logs.
- Authentication logs and domain controller logs.
- Backup system logs and admin actions.
- Email gateway and cloud audit logs.
- Identity provider logs, MFA events and admin changes.
- List of affected systems, users, services and timestamps.
- Relevant memory or disk images for selected high-value systems.
- The incident timeline and decision log.

## Communication Checklist

### Internal

- Notify incident response stakeholders.
- Inform management and crisis team.
- Coordinate with legal, privacy and compliance.
- Prepare internal staff guidance on what to do and what not to do.
- Maintain a single source of truth for incident updates.

### External

- Prepare customer, supplier or partner communication if required.
- Preserve all external communication drafts.
- Avoid unsupported statements about root cause, attacker identity or data theft before investigation confirms them.
- Align legal/privacy review before notifications or public claims.
- Coordinate with cyber insurance and external IR where applicable.

## Legal / Regulatory Considerations

Ransomware incidents may trigger contractual, sector-specific, cyber insurance and legal notification requirements, especially if personal data, regulated data, operationally critical services or third-party obligations are involved.

This playbook does not provide legal advice. It should ensure that:

- relevant evidence is preserved,
- key timestamps are documented,
- exfiltration assessment is tracked,
- decisions are recorded,
- and legal/privacy stakeholders are engaged early enough to support notification assessment.

## Quick Response Checklist

### Immediate Response

- [ ] Activate incident commander.
- [ ] Start timeline and decision log.
- [ ] Isolate affected systems.
- [ ] Preserve evidence.
- [ ] Protect backups.
- [ ] Review privileged accounts.

### Scope

- [ ] Identify affected hosts.
- [ ] Identify affected users and service accounts.
- [ ] Identify affected file shares and storage.
- [ ] Check domain controllers and identity systems.
- [ ] Check backup systems and management planes.
- [ ] Check remote access and cloud admin portals.
- [ ] Check for exfiltration indicators.

### Containment

- [ ] Restrict lateral movement paths.
- [ ] Revoke suspicious sessions/tokens.
- [ ] Disable compromised accounts as appropriate.
- [ ] Suspend risky synchronization jobs.
- [ ] Preserve forensic evidence.

### Investigation

- [ ] Determine initial access.
- [ ] Review privileged account activity.
- [ ] Check persistence.
- [ ] Check exfiltration indicators.
- [ ] Determine trusted recovery boundary.

### Recovery

- [ ] Validate recovery gate conditions.
- [ ] Restore from verified clean backups or images.
- [ ] Validate systems before reconnecting.
- [ ] Monitor for reinfection.
- [ ] Communicate restoration status.

### Post-Incident

- [ ] Complete timeline.
- [ ] Confirm root cause as far as possible.
- [ ] Update controls and detections.
- [ ] Update playbook.
- [ ] Track remediation to closure.

## Website Summary Block

**Ransomware Attack**  
Structured response playbook for suspected or confirmed ransomware incidents — from early detection and containment through identity and backup protection, evidence preservation, trusted recovery and post-incident review.

**Core phases:** Detect, Contain, Investigate, Eradicate, Recover, Review.

## Notes for Threat Sentinel

This version is optimized for a public playbook library and should work well as a community-facing reference because it combines:

- fast-action guidance,
- a visible first-15-minutes section,
- explicit decision rules,
- stronger identity and backup prioritization,
- recovery gates,
- and clean phase exit criteria.

For website presentation, the most useful format would be:

- executive summary,
- first 15 minutes,
- decision model,
- collapsible response phases,
- evidence checklist,
- communication checklist,
- quick response checklist,
- references section.
