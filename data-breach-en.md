# Data Breach / Suspected Data Breach Playbook

## Summary

This playbook provides a practical, technical incident response procedure for confirmed or suspected data breaches, with a strong focus on community usability and real-world response work.
It is designed to help incident responders stop ongoing exposure, preserve evidence, determine scope, assess risk to affected individuals, and support legal and regulatory decision-making without losing technical clarity.

The playbook follows a standard incident response flow: Detect & Triage, Contain, Investigate, Notify & Communicate, Remediate & Recover, and Review.
It is intended for security teams, IT administrators, SOC analysts, privacy teams, legal stakeholders, consultants, and small to medium organizations.

> Note: This is not legal advice and not a substitute for forensic expertise, privacy counsel, regulatory assessment, or organization-specific incident response planning.

## Purpose

- Provide a structured response process for suspected and confirmed data breaches.
- Enable fast validation, containment, and evidence preservation.
- Determine scope across systems, data stores, endpoints, cloud services, mailboxes, databases, and third-party platforms.
- Remove unauthorized access and unsafe configurations.
- Restore secure and trusted operations.
- Support GDPR-oriented notification assessment and decision-making.
- Produce documented lessons learned and tracked remediation.

## Scope / When to use

Use this playbook when one or more of the following are present:

- Sensitive data was accessed by an unauthorized party.
- Personal data may have been exposed, copied, lost, altered, or disclosed.
- Confidential business data was sent to the wrong recipient.
- A mailbox, file share, database, cloud storage location, or collaboration platform was exposed.
- Data was published accidentally or intentionally.
- Systems containing personal, confidential, or regulated data were compromised.
- A third party reports leaked, stolen, or exposed company data.
- Ransomware actors claim to have stolen data.
- Logs indicate unusual downloads, exports, database queries, or file access.
- Backup media, laptops, mobile devices, or removable storage containing sensitive data were lost or stolen.
- Access permissions were misconfigured and exposed data to unauthorized users.
- Credentials, tokens, API keys, or secrets were exposed and may have enabled data access.

## Severity

Default severity: **High**.

Escalate to **Critical** if any of the following apply:

- Personal data, regulated data, credentials, financial data, health data, trade secrets, or large datasets are involved.
- Public exposure occurred.
- Confirmed exfiltration occurred or is strongly suspected.
- Privileged access or administrative access was involved.
- Legal notification deadlines may apply.
- Ransomware-related data theft is involved.
- The incident affects a large number of records or individuals.
- The exposure may create high risk to the rights and freedoms of natural persons.

## Incident objectives

During the first hours of a data breach incident, the incident response team should aim to:

1. Stop ongoing exposure.
2. Preserve evidence and maintain a reliable timeline.
3. Determine what data, systems, users, and third parties are affected.
4. Assess whether personal data or regulated data is involved.
5. Assess whether notification obligations may apply.
6. Minimise legal, regulatory, privacy, business, and operational harm.
7. Restore secure operations and track remediation to closure.

## Core response principles

- Preserve volatile evidence before destructive remediation.
- Separate confirmed facts from hypotheses and document all decisions.
- Treat identity compromise, public exposure, and misconfiguration as possible breach paths.
- Engage privacy and legal stakeholders early if personal data may be involved.
- Do not assume that lack of confirmed exfiltration means no breach occurred.
- Do not delete, rotate, rebuild, or restore in a way that destroys evidence before it is preserved.
- Use a data-owner-driven approach to assess sensitivity and business impact.
- Track discovery time and the time the organization became aware of a possible breach.
- Keep a single authoritative incident timeline and decision log.
- Assess whether encryption, pseudonymization, or access controls reduced the risk.

---

## First 15 minutes (Immediate actions)

- Activate the Incident Response Lead / Incident Commander.
- Start an incident timeline and decision log.
- Record who discovered the issue and when.
- Record when the organization became aware of the possible breach.
- Identify the affected system, dataset, user, mailbox, file share, database, application, cloud storage location, or third-party service.
- Determine whether exposure is still active.
- Identify whether personal data, regulated data, or confidential business data may be involved.
- Preserve logs, audit trails, alerts, screenshots, permissions, and access records.
- Stop ongoing unauthorized access without destroying evidence.
- Restrict access, disable public links, or revoke compromised sessions where appropriate.
- Notify privacy and legal stakeholders immediately if personal data may be involved.
- Avoid deleting files, emails, logs, or artifacts before preservation.
- Start an initial incident classification.

Do NOT:
- Delete exposed data before documenting and preserving evidence.
- Modify affected systems unnecessarily before logs and artifacts are collected.
- Notify external parties before facts are validated and legal/privacy stakeholders are involved.
- Make unsupported statements about the number of affected individuals or records.
- Assume that the absence of confirmed exfiltration means no breach occurred.
- Delay escalation to privacy/legal teams while waiting for perfect technical certainty.

---

## Roles & Responsibilities

- Incident Commander: overall coordination, timeline ownership, decision tracking, and escalation.
- Technical Lead: coordinates infrastructure, cloud, identity, application, endpoint, database, and network response.
- Forensics / Security Analyst: preserves and analyzes evidence, logs, alerts, telemetry, and access records.
- Data Owner / Business Owner: identifies business context, data sensitivity, and affected datasets.
- Privacy / Data Protection Officer: assesses privacy impact, breach status, and notification requirements.
- Legal: assesses legal, regulatory, contractual, and litigation implications.
- Communications Lead: coordinates internal and external messaging with privacy/legal.
- Management / Crisis Team: approves major decisions and supports business continuity.
- Third-Party / Supplier Manager: coordinates with processors, MSPs, SaaS providers, and other external parties.

---

## Decision model (quick rules)

### Rule 1 — Is this a security incident or a data breach?

Treat the event as a **data breach candidate** if any of the following are true:

- Personal data may have been accessed, disclosed, lost, altered, or destroyed.
- Confidential or regulated data was exposed to an unauthorized party.
- Data was sent to the wrong recipient.
- A public link or public storage exposure existed.
- Access logs show unusual downloads, exports, or queries.
- Credentials, tokens, or secrets may have enabled access to sensitive data.

### Rule 2 — Is the breach ongoing?

Consider exposure ongoing if:

- Public links are still active.
- Unauthorized accounts still have access.
- Sessions, tokens, or API keys remain valid.
- Misconfigured permissions are still in place.
- Malicious activity is still visible in logs or alerts.

### Rule 3 — Do we have enough evidence to assess scope?

If not:
- Preserve logs and snapshots immediately.
- Avoid destructive changes.
- Collect system, access, and audit records before rotating or rebuilding.
- Identify the data owner and begin data classification.

### Rule 4 — Is notification likely required?

Engage privacy/legal immediately if:

- Personal data may be involved.
- Regulated or special-category data is involved.
- Credentials, financial data, health data, or identity data are exposed.
- The breach may create risk to the rights and freedoms of natural persons.
- Legal or regulatory deadlines may apply.

### Rule 5 — Can we restore now?

Recovery only if:

1. The exposure path is understood.
2. Unauthorized access has been removed.
3. Evidence is preserved.
4. Notification assessment has been initiated.
5. Monitoring and follow-up controls are in place.

---

## Identity-first and access-first focus

Many data breaches begin with access abuse rather than pure malware. Prioritise:

- Successful and failed authentication events.
- Unusual source IPs, geolocations, devices, or user agents.
- Privileged account activity.
- Stolen credentials or session token use.
- Excessive permissions or access control failures.
- API keys, service account credentials, or secrets exposure.
- OAuth or application abuse where data platforms are accessed through cloud identities.

---

## Data exposure patterns

Prioritise these common exposure patterns:

- Mailbox exposure or accidental email disclosure.
- Misconfigured cloud storage and public sharing.
- Unauthorized file share or collaboration platform access.
- Database export, query, or dump activity.
- Publicly exposed documents or storage buckets.
- Lost or stolen devices containing sensitive data.
- Insider theft or misuse.
- Third-party / supplier compromise.
- Ransomware-related data theft and extortion.

---

## Incident classification

Classify the event early as one or more of:

- False positive.
- Security event without data exposure.
- Suspected data breach.
- Confirmed data breach.
- Personal data breach.
- Confidential business data exposure.
- Regulated data exposure.
- Third-party breach.
- Ransomware-related data theft.
- Insider data theft.
- Accidental disclosure.
- Public data exposure.
- Unauthorized access without confirmed exfiltration.
- Confirmed exfiltration.

Classification should be revised as facts become available.

---

## Phase 1 — Detect & Triage

Objectives:
- Determine whether a data breach may have occurred.
- Identify affected data, systems, and users.
- Determine whether exposure is ongoing.
- Determine whether personal data, regulated data, or confidential business data is involved.
- Preserve evidence needed for technical, legal, and regulatory assessment.

Actions:
- Collect the initial report, alert, or discovery details.
- Record who discovered the issue and when.
- Record when the organization became aware of the possible breach.
- Identify the affected system, application, mailbox, database, file share, cloud service, or third-party platform.
- Identify the data owner or business owner.
- Determine whether access is still possible by unauthorized parties.
- Determine whether confidentiality, integrity, or availability of data is affected.
- Determine whether data was viewed, copied, downloaded, altered, deleted, published, or sent to unauthorized recipients.
- Determine whether personal data is involved.
- Determine whether special-category personal data is involved.
- Determine whether credentials, financial information, health data, HR data, customer data, supplier data, intellectual property, or trade secrets are involved.
- Estimate the number of affected records, files, mailboxes, systems, and individuals.
- Preserve screenshots, URLs, sharing links, permissions, and exposed content metadata.
- Preserve relevant logs before rotation or deletion.
- Create an initial incident timeline.
- Assign an initial severity and classification.

Exit criteria:
- Incident type validated.
- Initial scope identified.
- Potentially affected data and systems flagged.
- Evidence preservation started.

---

## Phase 2 — Contain

Objectives:
- Stop ongoing unauthorized access or disclosure.
- Prevent further copying, downloading, or publication.
- Preserve evidence.
- Maintain business continuity where possible.
- Avoid actions that compromise forensic or regulatory assessment.

Actions:
- Remove public access to exposed storage, files, databases, applications, or URLs.
- Disable or restrict affected accounts if compromise is suspected.
- Revoke active sessions and access tokens.
- Disable exposed API keys, access keys, or secrets.
- Remove unauthorized sharing links.
- Correct misconfigured permissions.
- Block malicious IP addresses, domains, or accounts if attacker activity is observed.
- Disable affected integrations or third-party access where needed.
- Quarantine affected endpoints if malware or credential theft is involved.
- Suspend data synchronization if it could propagate unauthorized disclosure.
- Preserve access logs, object logs, mailbox logs, database logs, and cloud audit logs.
- Preserve configuration snapshots before making changes where feasible.
- Preserve copies or hashes of exposed files where appropriate.
- Coordinate containment steps with legal/privacy stakeholders if evidence or notification requirements may be affected.

Special Containment: Public Cloud Storage Exposure

- Disable public access immediately.
- Remove anonymous links and external sharing.
- Capture screenshots of exposed configuration.
- Export access logs and object-level access records.
- Identify all objects exposed during the relevant period.
- Identify whether objects were accessed or downloaded.
- Rotate access keys, SAS tokens, API credentials, or service account credentials if exposed.
- Validate tenant-wide external sharing policies.

Special Containment: Compromised Mailbox

- Revoke sessions.
- Reset credentials after session revocation.
- Review and remove suspicious mailbox rules and forwarding.
- Review delegated access.
- Review sent, deleted, and accessed messages.
- Preserve mailbox audit logs.
- Determine whether sensitive attachments or personal data were accessed.
- Determine whether malicious emails were sent to additional victims.

Special Containment: Database or Application Exposure

- Disable exposed endpoint or restrict access.
- Rotate database credentials and API secrets.
- Preserve application, database, web server, and WAF logs.
- Determine whether queries, exports, or dumps occurred.
- Snapshot affected systems where appropriate.
- Identify affected tables, fields, and records.
- Validate whether exploitation was automated or targeted.

Exit criteria:
- Ongoing exposure reduced or stopped.
- Compromised accounts, links, permissions, or endpoints secured.
- Evidence preserved.
- Technical and legal/privacy stakeholders informed where needed.

---

## Phase 3 — Assess & Investigate

Objectives:
- Determine what happened.
- Determine what data was affected.
- Determine who was affected.
- Determine who accessed the data.
- Determine whether data was copied, exfiltrated, altered, deleted, or published.
- Determine the likely risk to affected individuals, customers, partners, and the organization.
- Determine whether notification obligations may apply.

### Technical Investigation

- Review authentication and authorization logs.
- Review cloud audit logs.
- Review file access logs.
- Review mailbox audit logs.
- Review database audit logs and query history.
- Review application logs.
- Review web server, proxy, firewall, DNS, and WAF logs.
- Review endpoint telemetry.
- Review DLP alerts.
- Review SIEM correlation events.
- Review VPN and remote access logs.
- Review administrative activity.
- Review third-party access records.
- Identify source IPs, user agents, accounts, access tokens, and devices involved.
- Determine whether access was authenticated or anonymous.
- Determine whether access was internal, external, third-party, or attacker-controlled.
- Determine whether access was automated, bulk, targeted, or accidental.
- Determine whether logs are complete enough to support conclusions.

### Data Assessment

- Identify categories of data involved.
- Identify categories of affected individuals, customers, employees, suppliers, or partners.
- Estimate the number of affected individuals.
- Estimate the number of affected records.
- Determine whether special-category personal data is involved.
- Determine whether children, vulnerable individuals, or high-risk groups are involved.
- Determine whether credentials, authentication data, or security tokens are involved.
- Determine whether financial, payment, tax, or identity documents are involved.
- Determine whether health, biometric, genetic, or highly sensitive data is involved.
- Determine whether confidential business information, intellectual property, or trade secrets are involved.
- Determine whether the data was encrypted, pseudonymized, anonymized, or protected by access controls.
- Determine whether the exposed data can be misused for identity theft, fraud, phishing, discrimination, reputational harm, or financial loss.

### Risk Assessment

Assess the likely impact using factors such as:

- Sensitivity of the data.
- Volume of affected data.
- Number of affected individuals.
- Ease of identifying individuals.
- Whether data was accessed by a trusted or untrusted party.
- Whether data was downloaded or only exposed.
- Whether data is already public.
- Whether encryption or pseudonymization was effective.
- Whether credentials or secrets were exposed.
- Whether financial fraud, identity theft, or phishing is possible.
- Whether affected individuals may suffer physical, material, or non-material harm.
- Whether the incident could cause regulatory, contractual, or business impact.
- Whether affected individuals need to take protective action.

### Investigation Questions

- What was the first known time of exposure?
- When did the organization become aware of the possible breach?
- How was the breach discovered?
- What system or data source was affected?
- What data was exposed, accessed, copied, altered, or deleted?
- Who had unauthorized access?
- Was access authenticated or anonymous?
- Was the data downloaded or exfiltrated?
- Were logs sufficient to prove or disprove access?
- Was the data encrypted or otherwise protected?
- Were credentials, tokens, or secrets exposed?
- Are affected individuals identifiable?
- Could affected individuals suffer harm?
- Is supervisory authority notification required?
- Is notification to affected individuals required?
- Are customers, suppliers, insurers, or contractual partners affected?
- Are sector-specific or national cybersecurity reporting obligations triggered?

Exit criteria:
- What happened is sufficiently understood.
- Data types and affected parties are identified.
- Risk level has been assessed.
- Notification decision-making can proceed.

---

## Phase 4 — Notify & Communicate

Objectives:
- Meet legal, regulatory, contractual, and organizational obligations.
- Communicate accurately and consistently.
- Avoid premature or unsupported claims.
- Provide affected parties with useful protective guidance where required.

### Internal Escalation

- Notify executive management if severity is High or Critical.
- Notify legal/privacy stakeholders immediately if personal data may be involved.
- Notify data owners and affected business units.
- Notify IT, security, communications, and customer support teams.
- Notify procurement/vendor management if a third party is involved.
- Notify the cyber insurance contact if policy requirements apply.
- Notify law enforcement where appropriate and approved.

### Regulatory Assessment

For GDPR-relevant personal data breaches:

- Determine whether the incident is unlikely to result in a risk to the rights and freedoms of natural persons.
- If risk is likely, prepare notification to the competent supervisory authority.
- Track the 72-hour notification window from the point of awareness.
- If notification cannot be completed within 72 hours, document the reasons for delay.
- If information is incomplete, submit an initial notification and provide additional information in phases where appropriate.
- If the breach is likely to result in high risk to affected individuals, prepare communication to affected individuals without undue delay.
- Document the reasoning for notification or non-notification.

### Supervisory Authority Notification Content

Where required, prepare the following information:

- Nature of the personal data breach.
- Categories and approximate number of affected individuals.
- Categories and approximate number of affected records.
- Name and contact details of the data protection officer or relevant contact point.
- Likely consequences of the breach.
- Measures taken or proposed to address the breach.
- Measures taken to mitigate possible adverse effects.
- Timeline of discovery, awareness, containment, and investigation.
- Whether affected individuals have been or will be informed.
- Reasons for delayed notification if applicable.

### Communication to Affected Individuals

Where required, communication should be clear, plain, and practical.

Include:

- What happened.
- When it happened or when it was discovered.
- What data may be affected.
- What risks may result.
- What the organization has done.
- What affected individuals can do to protect themselves.
- Contact information for further questions.
- Avoid speculation about attacker identity, exact scope, or impact unless confirmed.

### External Communication Principles

- Communicate confirmed facts only.
- Separate confirmed facts from ongoing investigation.
- Avoid minimizing language if risk is still being assessed.
- Do not overstate certainty.
- Coordinate all messages with legal/privacy and communications stakeholders.
- Use trusted communication channels.
- Preserve all external communications and approvals.

---

## Phase 5 — Remediate & Recover

Objectives:
- Remove root cause.
- Restore secure operations.
- Prevent recurrence.
- Validate that exposure has ended.
- Implement protective measures for affected parties.

### Technical Remediation

- Correct access control misconfigurations.
- Remove unauthorized accounts, sessions, tokens, and permissions.
- Rotate exposed credentials, API keys, certificates, and secrets.
- Patch exploited vulnerabilities.
- Harden affected applications, databases, mailboxes, or cloud services.
- Implement or improve encryption.
- Implement least privilege access.
- Disable anonymous or public access where not required.
- Restrict external sharing.
- Enable or improve audit logging.
- Improve monitoring for high-risk data access.
- Implement DLP rules where appropriate.
- Validate backup integrity if data was altered or deleted.
- Restore altered or deleted data from clean backups if needed.
- Conduct targeted security testing after remediation.

### Organizational Remediation

- Update data handling procedures.
- Update access review processes.
- Update supplier management controls.
- Update incident escalation procedures.
- Update data classification and ownership records.
- Provide targeted user or administrator training.
- Review approval processes for data sharing.
- Review data retention and minimization practices.
- Review contracts and processor obligations where applicable.

### Recovery Validation

- Confirm that unauthorized access has ended.
- Confirm that exposed links or permissions are removed.
- Confirm that credentials and secrets have been rotated.
- Confirm that affected systems are monitored.
- Confirm that relevant logs are retained.
- Confirm that notification obligations have been addressed.
- Confirm that affected business processes can resume safely.
- Continue enhanced monitoring for at least 14 to 30 days.

---

## Phase 6 — Review / Lessons Learned

Objectives:
- Understand root cause.
- Improve technical and organizational controls.
- Validate compliance with internal and external obligations.
- Track remediation actions to closure.

### Actions

- Conduct a post-incident review within 5 to 10 business days.
- Finalize the incident timeline.
- Document root cause.
- Document affected data, systems, users, and third parties.
- Document all containment, remediation, and communication actions.
- Document notification decisions and reasoning.
- Review whether logs were sufficient.
- Review whether data classification and ownership were clear.
- Review whether access controls followed least privilege.
- Review whether detection occurred early enough.
- Review whether escalation to legal/privacy was timely.
- Review whether affected individuals or authorities were notified appropriately.
- Update policies, procedures, playbooks, and training.
- Track all remediation actions to closure.

### Lessons Learned Questions

- Why was the data exposed or accessed?
- Was the root cause technical, procedural, human, or third-party related?
- Was the data properly classified?
- Was the data still needed, or should it have been deleted earlier?
- Were permissions excessive?
- Were sharing links or public access controls misused?
- Were credentials or secrets exposed?
- Were logs sufficient to determine access and impact?
- Was encryption effective?
- Did monitoring detect the issue or did a third party report it?
- Was legal/privacy escalation fast enough?
- Were notification deadlines met?
- Were communications accurate and consistent?
- Which controls would have prevented or reduced the impact?

---

## Evidence to Preserve

Preserve the following evidence where available:

### General Evidence

- Initial report or alert.
- Incident timeline.
- Screenshots of exposed data or configuration.
- URLs and access paths.
- File names, object names, database names, and mailbox names.
- Hashes of relevant files where appropriate.
- System and application configuration snapshots.
- List of affected users, systems, datasets, and business owners.
- Decision logs and approval records.

### Access and Audit Evidence

- Authentication logs.
- Authorization logs.
- Cloud audit logs.
- File access logs.
- Database audit logs.
- Query history.
- Web server logs.
- WAF logs.
- Firewall logs.
- Proxy logs.
- VPN logs.
- DNS logs.
- Mailbox audit logs.
- DLP alerts.
- SIEM events.
- EDR alerts.
- API gateway logs.
- Third-party platform logs.

### Data Evidence

- Categories of affected data.
- Approximate number of records.
- Approximate number of affected individuals.
- Data owner confirmation.
- Data classification.
- Evidence of encryption, pseudonymization, or anonymization.
- Evidence of access, download, export, deletion, alteration, or publication.
- Copies of exposed emails, files, or database exports where legally appropriate.

### Communication and Legal Evidence

- Internal escalation records.
- Legal/privacy assessment notes.
- Supervisory authority notifications.
- Data subject communications.
- Customer or supplier communications.
- Third-party notifications.
- Cyber insurance communications.
- Law enforcement communications.
- Reasons for notification or non-notification.
- Reasons for delayed notification if applicable.

---

## Communication Checklist

### Internal Communication

- Notify incident response team.
- Notify legal/privacy stakeholders.
- Notify the data owner.
- Notify management if severity is High or Critical.
- Notify affected business units.
- Notify communications/customer support teams where external contact may occur.
- Notify supplier management if a third party is involved.
- Keep internal updates factual and version-controlled.
- Maintain a single source of truth.

### External Communication

- Determine whether supervisory authority notification is required.
- Determine whether affected individuals must be informed.
- Determine whether customers, suppliers, or contractual partners must be informed.
- Determine whether insurers must be informed.
- Determine whether law enforcement notification is appropriate.
- Prepare external statements through legal/privacy and communications teams.
- Preserve all outgoing communications.
- Use clear, plain, and practical language.

### Suggested Internal Holding Statement

A suspected data breach is currently under investigation. The incident response, legal/privacy, and relevant technical teams are assessing scope, affected data, containment status, and potential notification obligations. Please do not speculate externally or delete any potentially relevant evidence. Further updates will be provided through the designated incident communication channel.

### Suggested External Holding Statement

We are investigating a security incident involving potential unauthorized access to data. Our teams have taken immediate steps to contain the issue and are working with relevant experts to determine the scope and impact. We will provide further information to affected parties as appropriate and in accordance with applicable obligations.

---

## Legal and Regulatory Considerations

Data breach incidents may trigger legal, regulatory, contractual, sector-specific, or insurance-related obligations.

For GDPR-relevant incidents, involve privacy/legal stakeholders immediately. The response team should support the legal assessment by preserving evidence, documenting timelines, and providing verified facts.

Key considerations include:

- Whether a personal data breach occurred.
- Whether the breach is likely to result in risk to the rights and freedoms of natural persons.
- Whether notification to the supervisory authority is required.
- Whether affected individuals must be informed.
- Whether notification must occur within a specific deadline.
- Whether contractual customer or supplier notification obligations apply.
- Whether sector-specific cybersecurity reporting obligations apply.
- Whether law enforcement or cyber insurance notification is required.
- Whether cross-border supervisory authority coordination is relevant.
- Whether processor/controller obligations apply.

This playbook does not provide legal advice. It is intended to support evidence preservation, structured response, and informed legal/privacy assessment.

---

## Quick Response Checklist

### Immediate Response

- [ ] Assign incident lead.
- [ ] Notify legal/privacy if personal data may be involved.
- [ ] Identify affected system, dataset, and owner.
- [ ] Stop ongoing exposure.
- [ ] Preserve logs and screenshots.
- [ ] Record discovery and awareness timestamps.
- [ ] Start incident timeline.
- [ ] Classify suspected vs. confirmed breach.

### Scope

- [ ] Identify affected systems.
- [ ] Identify affected data.
- [ ] Identify affected individuals or organizations.
- [ ] Determine number of records.
- [ ] Determine whether data was accessed.
- [ ] Determine whether data was downloaded or exfiltrated.
- [ ] Determine whether data was altered or deleted.
- [ ] Determine whether data was public.
- [ ] Determine whether logs are sufficient.

### Containment

- [ ] Remove public access.
- [ ] Revoke sessions and tokens.
- [ ] Disable compromised accounts.
- [ ] Remove unauthorized sharing links.
- [ ] Correct permissions.
- [ ] Rotate exposed credentials and secrets.
- [ ] Block attacker infrastructure.
- [ ] Preserve evidence before destructive changes.

### Assessment

- [ ] Determine data sensitivity.
- [ ] Determine affected data subjects.
- [ ] Determine likely harm.
- [ ] Determine legal/regulatory implications.
- [ ] Determine notification requirements.
- [ ] Document notification or non-notification decision.

### Notification

- [ ] Prepare supervisory authority notification if required.
- [ ] Track the 72-hour GDPR window where applicable.
- [ ] Prepare affected individual communication if high risk is likely.
- [ ] Prepare customer, supplier, or partner communication if required.
- [ ] Preserve all notification records.

### Remediation

- [ ] Fix root cause.
- [ ] Patch vulnerabilities.
- [ ] Harden access controls.
- [ ] Improve logging.
- [ ] Implement least privilege.
- [ ] Improve data classification.
- [ ] Improve external sharing controls.
- [ ] Update procedures and training.

### Recovery

- [ ] Confirm exposure has ended.
- [ ] Confirm controls are corrected.
- [ ] Confirm monitoring is active.
- [ ] Confirm notifications are handled.
- [ ] Continue enhanced monitoring for 14 to 30 days.
- [ ] Track remediation to closure.

---

## Preventive Controls

### Data Governance

- Maintain a data inventory.
- Assign data owners.
- Classify sensitive data.
- Apply retention and deletion rules.
- Minimize stored personal and sensitive data.
- Review access regularly.
- Document processing activities where required.

### Access Control

- Enforce least privilege.
- Use MFA for administrative and high-risk access.
- Review external sharing permissions.
- Restrict anonymous links.
- Monitor privileged access.
- Use conditional access.
- Remove stale users, groups, and permissions.
- Apply just-in-time access where appropriate.

### Technical Protection

- Encrypt sensitive data at rest and in transit.
- Protect secrets and API keys.
- Use DLP where appropriate.
- Enable detailed audit logging.
- Monitor unusual downloads and exports.
- Monitor public exposure of storage and applications.
- Patch internet-facing systems.
- Harden cloud storage and collaboration platforms.
- Validate backup and restore processes.

### Detection and Response

- Monitor high-risk data repositories.
- Alert on mass download, mass delete, and bulk export activity.
- Alert on public sharing changes.
- Alert on unusual administrative access.
- Alert on suspicious mailbox access.
- Alert on unusual database queries.
- Test incident response and breach notification procedures.
- Keep legal/privacy contacts and escalation paths current.

---

## Website Summary Card

### Data Breach

Structured response for confirmed or suspected unauthorized access, disclosure, loss, alteration, or exfiltration of sensitive data.

Includes scope assessment, containment, evidence preservation, GDPR-oriented notification support, communication guidance, remediation, and post-incident review.

### Tags

DETECT  
ASSESS  
CONTAIN  
NOTIFY  
REMEDIATE  
REVIEW

---

## References

- NIST SP 800-61 Rev. 2: Computer Security Incident Handling Guide
- NIST SP 800-61 Rev. 3: Incident Response Recommendations and Considerations for Cybersecurity Risk Management
- GDPR Article 33: Notification of a personal data breach to the supervisory authority
- GDPR Article 34: Communication of a personal data breach to the data subject
- EDPB Guidelines 9/2022 on personal data breach notification under GDPR
- ICO: Personal data breaches — a guide
- ENISA: Recommendations for a methodology of the assessment of severity of personal data breaches
- BSI: Leitfaden zur Reaktion auf IT-Sicherheitsvorfälle
- BSI: IT-Sicherheitsvorfall melden

---

## Disclaimer

This playbook is a practical community resource. It does not replace legal advice, forensic expertise, cyber insurance requirements, regulatory assessment, or organization-specific incident response planning. Organizations should adapt these procedures to their own environment, risk profile, legal obligations, contractual duties, and incident response capabilities.
