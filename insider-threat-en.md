# Playbook: Insider Incident

## Summary

This playbook structures the response to security incidents caused by people with legitimate access: employees, former employees, interns, temporary staff, administrators or service providers. Typical cases are taking data when leaving, passing on confidential information, sabotage, misuse of privileged rights or fraud.

Insider incidents differ fundamentally from external attacks: they are about a **person**, not primarily about malware. Employment law, employee data protection, co-determination and proportionality decide what may be investigated and how.

The core of this playbook: **HR and legal first, then technology.** IT does not act alone, not covertly and not on someone's say-so.

## Purpose

- Handle suspicions in a structured, lawful and proportionate way
- Prevent further damage without exposing the investigation unnecessarily
- Preserve evidence so that it holds up in court
- Protect the rights of the person concerned and of uninvolved third parties
- Document decisions traceably

## Scope

Use this playbook if:

- a person with legitimate access downloads, prints, copies to USB or sends unusually large amounts of data to private accounts
- confidential information that can be traced internally appears outside
- privileged rights are used outside the person's assignment
- sabotage is suspected, such as deletions, configuration changes or logic bombs
- reports come in from the team, management, the whistleblowing system or compliance
- people who have left still have or use access

**Boundaries:**

- If the person's account was taken over from outside, it is primarily an identity compromise.
- Negligent mistakes without intent usually go through the data breach playbook.
- Private rule violations without security impact are primarily HR or compliance matters.

## Severity

- **Medium:** Suspected rule violation without recognizable damage
- **High:** Likely taking or passing on of confidential data, or misuse of privileged rights
- **Critical:** Sabotage, ongoing damage, exfiltration of particularly sensitive data, or involvement of privileged administrators

## Incident Objectives

1. Examine the suspicion confidentially and lawfully.
2. Prevent further damage.
3. Preserve evidence so that it holds up in court.
4. Enable a well-founded decision on employment, civil or criminal law steps.
5. Improve organizational and technical controls.

## Core Response Principles

- **IT does not act alone.** Investigations are commissioned and steered by a small core team of HR, legal, data protection and security.
- **Need to know.** As few people involved as possible.
- **Proportionality.** Only analyze the data, systems and periods required to clarify the specific suspicion.
- **Presumption of innocence.** A suspicion is not a finding.
- **Respect co-determination.** Works or staff councils, works agreements and internal policies define which measures are permitted.
- **Document for court.** Chain of custody, four-eyes principle, hash values and logging, consistently.
- **Coordinate the timing of the lockout.** Access is usually withdrawn at the same time as the conversation, suspension or dismissal.
- **Record time consistently.** Document the timeline, chain of custody and exports in UTC.

## First 15 Minutes

- Take the report and document it in writing: who, when, what exactly, from which source
- Do **not** start your own inquiries into the person's mailboxes, files or histories
- Convene the core team: HR, legal, data protection, head of security, compliance depending on the organization
- Check whether there is acute danger, such as ongoing sabotage or ongoing mass exfiltration
- Note whether the person could know about the suspicion
- Make sure logs, DLP data, cloud audit logs or mail logs do not expire
- Take first, narrowly limited protective measures only in case of acute danger and after consultation

## Roles and Responsibilities

### Core team

- Typically consists of HR, legal, data protection, head of security, where applicable compliance and executive management
- Commissions investigative steps in writing
- Decides on measures concerning the person

### Security / forensics

- Performs only commissioned analyses
- Works under the four-eyes principle
- Documents every step
- Brings in external forensics when the risk is high

### IT administration

- Implements technical measures only on instruction of the core team
- Extends retention, locks access and secures systems
- Treats the matter confidentially

### Works or staff council

- Is involved according to the legal situation and internal agreements

### Data protection

- Checks whether the data processing is permitted
- Documents the balancing of interests and accompanies data protection assessments

## Decision Model

### Rule 1: Insider or taken-over account?

Before investigating a person, check:

- Do sign-in locations, devices and times match the person?
- Are there signs of phishing, malware or MFA anomalies?
- Was the person present or able to work at the relevant time at all?

If in doubt, apply the identity playbook first.

### Rule 2: Act immediately or examine covertly?

**Immediate technical measures** are only justified if:

- ongoing sabotage is visible
- ongoing mass exfiltration is taking place
- there is considerable danger to systems, data or people

**A covert, limited examination** is justifiable if:

- no acute damage is occurring
- an open measure would endanger evidence
- the measure is legally permitted and commissioned in writing

### Rule 3: When is access withdrawn?

Usually **at the same time** as the conversation, suspension or dismissal:

- a prepared list of all access, accounts, tokens, keys, physical access and devices
- technical lockout at an agreed time
- devices collected during the conversation or in the agreed process

For privileged administrators, additionally renew shared passwords, emergency accounts, keys and service provider access.

### Rule 4: Involve external bodies?

- **Criminal complaint:** if criminal offences are suspected, decided by the core team and executive management
- **Data protection authority:** if personal data is affected and the breach is reportable
- **External forensics:** if court proceedings are likely

## Emergency Rule for Acute Danger

If there is concrete, demonstrable ongoing damage and the core team cannot decide fully in advance:

- The head of security and the incident lead may order a narrowly limited protective measure
- The measure must limit the acute damage and be reversible where possible
- HR, legal and data protection are brought in without delay
- Scope, justification, time, decision-maker and technical effect are documented
- Any further analysis of personal data still requires a written investigation mandate

## Boundary Matrix

| Situation | Primary handling |
|---|---|
| Intentional misuse by an authorized person | Insider playbook |
| Taken-over user account | Identity compromise |
| Mistaken addressing or mistaken sharing | Data breach playbook |
| Private rule violation without security impact | HR or compliance process |

## Special Case: Taking Data When Leaving

The most common insider case. Between notice and the last working day there are often days or weeks of continued access.

- At notice, review rights and particularly sensitive access and reduce them in a coordinated way if needed
- Retrospective analyses only with a concrete reason and an investigation mandate
- On the last day, withdraw all access, collect devices, check forwarding rules and shares
- If data was taken, demand its return, deletion and confirmation with legal support

## Typical Indicators

**A single indicator is not proof.** Many anomalies have harmless explanations and must be examined in the context of the person's work.

- Unusual downloads, exports, print jobs or synchronization to private storage
- Forwarding of emails to private addresses
- Access to data outside the person's own tasks
- Activity outside usual working hours, especially shortly before or after giving notice
- Use of USB storage contrary to policy
- Disabling security software or logging
- Creation of additional accounts or backdoors by administrators

## Preservation Is Not Analysis

Technical preservation of evidence may be necessary before logs are overwritten or devices are no longer available. It does not yet permit any analysis of the content of personal data. Analysis only takes place on a written mandate from the core team, within the defined purpose, period and data scope.

## Phase 1: Detect and Assess

### Objectives

- Assess the report
- Separate an insider suspicion from an identity incident
- Establish the core team and the legal framework

### Actions

- Document the report
- Convene the core team and define confidentiality
- Check the legal framework: works agreement, policies, private use, consents
- Arrange preservation of evidence without analysis
- Document the decision on an open or covert approach

### Exit criteria

- Core team in place
- Legal framework clarified
- Written investigation mandate with scope and period available

## Phase 2: Contain

### Objectives

- Limit ongoing damage
- Prevent further escalation without exposing the investigation unnecessarily

### Actions

- Only in case of acute danger, immediate targeted restrictions, such as withdrawing write rights, blocking uploads to private services or locking USB
- Otherwise prepare the lockout according to rule 3
- Review access to particularly sensitive systems and restrict it inconspicuously if necessary

### Do not

- Do not confront the person prematurely
- Do not withdraw access without agreement from HR and legal, except in acute danger
- Do not search private devices or cloud accounts without a legal basis
- Do not set targeted traps for a person without approval from legal and data protection

### Exit criteria

- No more acute damage
- Lockout and measures prepared in a coordinated way

## Phase 3: Investigate

### Objectives

- Substantiate the suspicion, refute it, or document it as unresolvable
- Preserve evidence so that it holds up in court

### Actions

- Perform only analyses commissioned in writing
- Four-eyes principle and complete logging of every query
- Forensically image storage media and seal the originals
- Do not analyze private content found by chance any further without consultation
- Document results factually and neutrally, including exculpatory findings

### Exit criteria

- Suspicion confirmed, refuted or unresolvable
- Report to the core team produced

## Phase 4: Measures and Eradication

### Objectives

- Implement the core team's decisions
- Remove misuse, backdoors and shared risks

### Actions

- If confirmed: conversation, suspension, dismissal, civil or criminal law steps as decided by the core team
- Withdraw access at the same time, collect devices
- Remove accounts, forwarding rules, shares, scheduled tasks and backdoors created by the person
- Renew shared secrets
- Demand and document the return or deletion of data taken

### Exit criteria

- Decision implemented and documented
- All access withdrawn, devices secured
- Shared secrets and backdoors removed

## Phase 5: Recover

### Objectives

- Restore integrity
- Secure the handover of tasks
- Detect later access attempts

### Actions

- Check affected data and systems for integrity and restore them where necessary
- Secure the organizational handover of tasks
- Pay heightened attention to access attempts by the person for at least 30 days

### Exit criteria

- Integrity of affected data and systems confirmed
- Tasks handed over
- No further access attempts observed, or those observed have been handled

## Phase 6: Review

### Actions

- Check whether the authorization concept, leaver process or DLP could have prevented the incident or shown it earlier
- Evaluate the legal framework, works agreements and policies
- Feed lessons into HR, security and compliance processes
- Communicate internally in a factual way, without exposing the person unnecessarily

## Evidence to Preserve

- Written report and the core team's investigation mandate
- Exported logs and audit records for the commissioned period
- Forensic images with hash values and chain of custody
- Record of every analysis with purpose, time and the person performing it
- The core team's decision log

## Proof and Retention

- Store evidence with particular restriction and enforce need-to-know technically
- Document hash values, access history, export time and analyst
- Set retention according to the legal situation, the needs of proceedings and internal policy

## Communication Checklist

- Strictly need-to-know
- No emails with names and suspicions to large distribution lists
- Communication towards teams only factual and without unnecessary personal details
- External communication only after legal review
- Communication with a person who has left goes through HR or legal, not IT

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

**Employee data protection and co-determination**

Insider investigations touch on personality rights. Legal support is always required, as is coordination with employee representatives where they exist.

- **Germany:** Since the CJEU judgment C-34/21 (2023), § 26 BDSG is disputed as a general legal basis; permissibility must also be assessed case by case under Art. 6 and Art. 88 GDPR. To uncover criminal offences, § 26(1) sentence 2 BDSG requires a documented initial suspicion, necessity and proportionality. Technical monitoring equipment is subject to co-determination (§ 87(1) no. 6 BetrVG). The Trade Secrets Act (GeschGehG) only protects information for which appropriate confidentiality measures were taken. Under the Whistleblower Protection Act, the identity of whistleblowers must be kept confidential.
- **Austria:** Control measures that affect human dignity require the works council's consent (§ 96(1) no. 3 ArbVG), as do certain systems for processing employee data (§ 96a ArbVG). The GDPR and DSG apply in addition.
- **Switzerland:** Monitoring systems intended to monitor employees' behavior are not permitted (Art. 26 ArGV 3); personal data may only be processed under the FADP and Art. 328b CO insofar as it concerns the employment relationship.
- **Private use:** Whether work mailboxes and devices may be used privately largely determines what may be analyzed.

**Reporting obligations**

- If personal data of third parties has been exfiltrated, the data protection reporting obligations above apply.
- Sabotage of systems of an organization subject to NIS2 or the ISA can be a reportable security incident.
- **Criminal complaint:** Decided by the core team and executive management. Germany: Central Cybercrime Contact Point (ZAC) of the relevant state criminal police office; Austria: Federal Criminal Police Office or any police station; Switzerland: the cantonal police.

## Quick Response Checklist

- [ ] Report documented in writing
- [ ] No inquiries started on own initiative
- [ ] Core team of HR, legal, data protection and security convened
- [ ] Taken-over account ruled out or handled separately
- [ ] Acute danger assessed
- [ ] Preservation without analysis arranged
- [ ] Written investigation mandate in place
- [ ] Works or staff council involved as required
- [ ] Analysis under the four-eyes principle
- [ ] Lockout prepared to coincide with the conversation
- [ ] Shared secrets renewed
- [ ] Reporting obligations checked

## Preventive Controls

- Need-to-know permissions with regular recertification
- Orderly leaver process
- DLP for particularly sensitive data, as far as legally agreed
- Logging of access to sensitive data with clearly regulated analysis
- Separate admin accounts and four-eyes principle for critical changes
- Clear rules on private use of IT
- Trusted reporting channel and whistleblowing system
- A procedure for insider investigations agreed in advance with HR, legal, data protection and employee representatives

## At a Glance

This block is not a shortened replacement playbook. It is a quick way in; all sections of this document apply when handling the incident.

**Insider Incident**  
Response to data theft, sabotage and misuse of rights by people with legitimate access — with HR, legal, data protection and co-determination, proportionate, confidential and with evidence that holds up in court.

**Core phases:** Detect and Assess, Contain, Investigate, Measures, Recover, Review.

## References

- NIST SP 800-61 Rev. 3
- CISA: Insider Threat Mitigation Guide
- CERT/CC: Common Sense Guide to Mitigating Insider Threats
- MITRE ATT&CK: T1567, T1052, T1485

## Disclaimer

This playbook is a practical community resource. It does not replace legal advice, an employment law review or organization-specific procedures. Insider investigations touch on personality rights and require legal support.
