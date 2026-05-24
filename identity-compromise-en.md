# Identity Compromise Playbook

## Summary

This playbook provides a practical, technical incident response procedure for suspected or confirmed identity compromise involving user accounts, service accounts, privileged accounts, cloud identities, directory services, and identity infrastructure.
It is designed to help incident responders stop active abuse, preserve evidence, determine initial access, identify persistence and lateral movement, restore trusted identity state, and support legal and regulatory decision-making where personal data or regulated access is involved.

The playbook follows a standard incident response flow: Detect & Triage, Contain, Investigate, Eradicate, Recover, and Review.
It is intended for security teams, IT administrators, SOC analysts, identity engineers, consultants, and small to medium organizations.

> Note: This is not legal advice and not a substitute for forensic expertise, privacy counsel, regulatory assessment, or organization-specific incident response planning.

## Purpose

- Provide a structured response process for suspected and confirmed identity compromise.
- Enable fast validation, containment, and evidence preservation.
- Determine whether credentials, sessions, tokens, MFA, OAuth grants, delegated access, or privileged access were abused.
- Identify initial access, persistence, privilege escalation, and lateral movement.
- Restore secure and trusted identity state.
- Support GDPR-oriented assessment where personal data or regulated access may be involved.
- Produce documented lessons learned and tracked remediation.

## Scope / When to use

Use this playbook when one or more of the following are present:

- A user account is suspected or confirmed to be compromised.
- A privileged account shows suspicious activity.
- A service account or application identity behaves unexpectedly.
- Suspicious successful logins are observed.
- MFA prompts, approvals, method changes, or re-registrations appear suspicious.
- A user reports unexpected MFA requests or account activity.
- An account logs in from unusual locations, devices, IP addresses, or user agents.
- Suspicious OAuth grants, app consents, or delegated permissions are detected.
- API keys, access tokens, refresh tokens, SSH keys, certificates, or client secrets may be exposed.
- Lateral movement is suspected after credential theft.
- Credential dumping tools, pass-the-hash, pass-the-ticket, or Kerberoasting activity is suspected.
- New accounts, group memberships, role assignments, or admin privileges appear unexpectedly.
- Directory synchronization, federation, identity provider, or conditional access configuration may have been modified.
- Cloud tenant, Active Directory, Entra ID, Okta, Google Workspace, LDAP, IAM, SSO, or PAM systems show suspicious activity.

This playbook covers:

- User account compromise.
- Privileged account compromise.
- Service account compromise.
- Cloud identity compromise.
- OAuth consent abuse.
- Token theft.
- API key or secret exposure.
- MFA fatigue / MFA prompt bombing.
- MFA method hijacking.
- Password spraying.
- Credential stuffing.
- Credential dumping.
- Pass-the-hash.
- Pass-the-ticket.
- Kerberoasting.
- Golden Ticket / Silver Ticket suspicion.
- Unauthorized privilege escalation.
- Unauthorized role or group assignment.
- Suspicious directory synchronization or federation changes.
- Lateral movement using valid accounts.
- Identity compromise related to phishing, malware, ransomware, or insider activity.

---

## Severity

Default severity: **High**.

Escalate to **Critical** if any of the following apply:

- Privileged, admin, finance, HR, executive, or identity provider accounts are involved.
- Service accounts, break-glass accounts, federation services, MFA systems, directory synchronization, secrets, tokens, API keys, or identity infrastructure are involved.
- Session tokens, refresh tokens, API keys, or service credentials are exposed.
- Attackers can maintain persistence through MFA, OAuth, recovery methods, or delegated access.
- Multiple accounts, tenants, cloud identities, or systems are affected.
- Data exposure, financial fraud, or business process abuse is possible.
- There is evidence of lateral movement or domain-wide / tenant-wide compromise.
- Regulatory, contractual, or legal notification deadlines may apply.

## Incident objectives

During the first hours of an identity compromise incident, the incident response team should aim to:

1. Stop active misuse of identities.
2. Preserve evidence and maintain a reliable timeline.
3. Determine the initial access method and affected identities.
4. Identify persistence mechanisms and lateral movement.
5. Determine whether data, systems, or business processes were accessed or abused.
6. Restore trusted identity state and strengthen controls.
7. Minimise legal, regulatory, financial, and operational harm.

## Core response principles

- Treat identity compromise as likely until disproven.
- Preserve volatile evidence before destructive remediation.
- Separate confirmed facts from hypotheses and document all decisions.
- Sessions, tokens, MFA state, OAuth grants, recovery methods, roles, and delegated access are part of the attack surface.
- A password reset alone may not be enough.
- Review sign-in activity, mailbox activity, cloud permissions, and privileged changes together.
- Engage privacy and legal stakeholders early if access to personal data or regulated systems may be involved.
- Maintain a single authoritative incident timeline and decision log.
- Avoid restoring trust before persistence and abuse paths are removed.

---

## First 15 minutes

- Activate the Incident Response Lead / Incident Commander.
- Start an incident timeline and decision log.
- Identify the affected account(s), tenant, identity provider, device, mailbox, application, and business process.
- Determine whether the account is a user account, service account, admin account, federated identity, or break-glass account.
- Determine whether the user clicked, approved, entered credentials, or installed anything suspicious.
- Revoke suspicious sessions and refresh tokens if compromise is likely.
- Restrict or disable the affected account if active abuse is ongoing.
- Preserve logs, alerts, screenshots, MFA events, sign-in records, and access records.
- Preserve original phishing, login, or consent artifacts if they exist.
- Check for suspicious inbox rules, forwarding rules, delegates, OAuth grants, or recovery method changes.
- Search for related activity across the tenant and adjacent systems.
- Notify legal/privacy stakeholders if personal data or regulated access may be involved.
- Avoid deleting logs or resetting systems before evidence is secured.

Do not:
- Reset credentials without considering session revocation and persistence.
- Assume the incident is limited to one account.
- Trust the account until MFA, recovery methods, OAuth grants, delegated access, and privileged changes have been reviewed.
- Delete evidence before it is preserved.
- Communicate externally about root cause, data exposure, or financial impact before facts are confirmed.

---

## Roles & Responsibilities

- Incident Commander: overall coordination, timeline ownership, decision tracking, and escalation.
- Identity Lead: coordinates identity provider actions, sessions, MFA, OAuth grants, recovery methods, and access reviews.
- Technical Lead: coordinates endpoint, cloud, application, server, network, and directory response.
- Forensics / Security Analyst: preserves and analyzes evidence, logs, alerts, and telemetry.
- Data / Application Owner: provides business context for affected identities and systems.
- Privacy / Data Protection Officer: assesses privacy impact, breach status, and notification requirements.
- Legal: assesses legal, regulatory, contractual, and litigation implications.
- Communications Lead: coordinates internal and external messaging with legal/privacy.
- Management / Crisis Team: approves major decisions and supports business continuity.
- Third-Party / Supplier Manager: coordinates with MSSPs, SaaS providers, identity providers, and other external parties.

---

## Phase 1: Detect & Triage

### Objectives

- Determine whether identity compromise may have occurred.
- Identify affected identities, devices, systems, and services.
- Determine whether the compromise is ongoing.
- Determine whether privileged access, token abuse, or lateral movement is involved.
- Preserve evidence needed for technical, legal, and regulatory assessment.

### Actions

- Collect the initial report, alert, or discovery details.
- Record who discovered the issue and when.
- Identify the affected user, service account, admin account, tenant, or application.
- Determine whether the identity is local, federated, cloud-native, or service-based.
- Determine whether access is still possible by unauthorized parties.
- Review sign-in logs for unusual locations, devices, times, ASNs, IPs, or user agents.
- Review MFA events, including prompts, approvals, denials, fatigue patterns, method additions, removals, and re-registrations.
- Review password reset events and recovery method changes.
- Review session and refresh token activity where available.
- Review OAuth consent grants and application permissions.
- Review mailbox, cloud, and application access logs tied to the identity.
- Review endpoint telemetry for browser theft, infostealer indicators, or token theft.
- Review admin role changes, group membership changes, and privileged assignments.
- Review creation or modification of accounts, service principals, app registrations, and secrets.
- Review VPN, RDP, VDI, PAM, SSH, SSO, and remote access logs.
- Review domain controller logs for Kerberos, NTLM, LDAP, group changes, and suspicious authentication patterns.
- Preserve screenshots, alerts, audit trails, and relevant configuration snapshots.
- Determine whether the incident is isolated, multi-account, or identity-infrastructure-wide.
- Create an initial incident timeline.

### Initial classification

- Suspicious login.
- Confirmed user account compromise.
- Privileged account compromise.
- Service account compromise.
- Cloud identity compromise.
- Token theft.
- OAuth consent compromise.
- API key or secret exposure.
- MFA compromise or MFA fatigue.
- Password spraying.
- Credential stuffing.
- Credential dumping.
- Lateral movement using valid credentials.
- Directory or tenant configuration compromise.
- Identity provider compromise.
- Data access via compromised identity.
- Ransomware precursor activity.
- Insider misuse.

---

## Phase 2: Contain

### Objectives

- Stop unauthorized access.
- Prevent lateral movement.
- Prevent privilege escalation.
- Preserve evidence.
- Avoid breaking critical services unnecessarily.

### User accounts

- Revoke active sessions and refresh tokens.
- Block sign-in or disable the account if active abuse is ongoing.
- Reset the password after session revocation.
- Require MFA re-registration if MFA methods may be compromised.
- Remove suspicious MFA methods, devices, and authenticators.
- Remove suspicious OAuth grants and delegated permissions.
- Remove suspicious mailbox rules, forwarding, and delegates.
- Remove unauthorized group memberships and role assignments.
- Restrict access until investigation is complete.
- Monitor for immediate re-authentication attempts.

### Privileged accounts

- Disable or restrict the account immediately if compromise is likely.
- Revoke sessions and tokens.
- Remove temporary or suspicious role assignments.
- Review all recent administrative actions.
- Check whether the account modified identity, logging, security, backup, or recovery settings.
- Rotate credentials only after documenting dependencies and persistence risk.
- Use clean, trusted admin workstations for response actions.
- Validate that break-glass accounts remain secure.
- Review all accounts with equivalent privilege.
- Consider emergency administrative access review across the tenant or domain.

### Service accounts and application identities

- Identify systems, applications, jobs, and integrations using the account.
- Disable the account only if business impact is understood or active abuse is confirmed.
- Rotate passwords, secrets, certificates, SSH keys, or API keys.
- Revoke tokens and refresh tokens where possible.
- Remove unnecessary privileges.
- Check whether the service account was used interactively.
- Check whether the account performed unusual data access, exports, or administrative actions.
- Replace long-lived credentials with managed identities or stronger secret management where possible.
- Monitor dependent systems after rotation.

### Cloud / SaaS identities

- Revoke sessions and tokens.
- Review admin roles and role assignments.
- Review OAuth grants, app consents, and service principals.
- Review conditional access and MFA settings.
- Review tenant-wide configuration changes.
- Review external sharing, guest access, and federation settings.
- Review logs for unusual application access, data export, and administrative activity.
- Restrict risky applications and high-risk sign-in conditions.

### Active Directory / hybrid identity

- Review domain controller logs.
- Review privileged group changes.
- Review Kerberos and NTLM activity.
- Review GPO changes.
- Review AD delegation changes.
- Review directory synchronization activity.
- Review federation configuration.
- Check for suspicious replication activity.
- Check for credential dumping indicators on endpoints and servers.
- Isolate systems suspected of credential dumping or lateral movement.
- Consider KRBTGT reset only after proper planning and confirmation of domain compromise indicators.

Do not:
- Reset passwords without revoking sessions where tokens may persist.
- Rotate service account credentials without understanding dependencies.
- Disable critical service accounts without a fallback plan unless active abuse requires it.
- Use potentially compromised admin workstations for remediation.
- Assume cloud identity compromise is resolved by on-premises password reset alone.
- Assume on-premises compromise is resolved by cloud-only containment.
- Destroy evidence by mass cleanup before log preservation.

---

## Phase 3: Investigate

### Objectives

- Determine initial access.
- Determine credential source.
- Determine attacker activity.
- Determine lateral movement.
- Determine privilege escalation.
- Determine data access.
- Determine persistence.
- Determine whether other identities are affected.

### Investigation actions

- Determine how the identity was compromised.
- Review phishing reports and email security logs.
- Review endpoint telemetry for malware, infostealers, credential dumping, and browser credential theft.
- Review password spraying and credential stuffing indicators.
- Review exposed credentials in breach intelligence sources where available.
- Review suspicious VPN, RDP, SSH, VDI, and SSO activity.
- Review all applications accessed by the compromised identity.
- Review administrative actions performed by the identity.
- Review data repositories accessed by the identity.
- Review whether the attacker created or modified accounts.
- Review whether roles, groups, or permissions were changed.
- Review whether MFA methods were added, removed, or changed.
- Review whether OAuth applications or service principals were added or modified.
- Review whether secrets, certificates, API keys, or access keys were created or exported.
- Review whether mailbox forwarding, rules, or delegates were configured.
- Review whether logging, retention, audit, or security controls were disabled.
- Review whether backups, recovery systems, or security tools were accessed.
- Review whether lateral movement occurred through SMB, RDP, WinRM, SSH, WMI, PsExec, or remote management tools.
- Review whether privileged access workstations or admin jump hosts were accessed.
- Identify all affected identities and systems.
- Determine whether the incident should be escalated to ransomware, data breach, or insider threat playbooks.

### Common indicators of identity compromise

- Successful sign-ins from unusual locations.
- Impossible travel or atypical travel.
- New device registration.
- New MFA method registration.
- Repeated MFA prompts or unexpected approvals.
- Login from anonymous proxy, TOR, VPS, or unfamiliar ASN.
- Login from unfamiliar user agent.
- Access to unusual applications.
- Unusual mailbox activity.
- New forwarding rules.
- New OAuth grants.
- New app registrations or service principals.
- New client secrets or certificates.
- New or changed admin roles.
- New group memberships.
- Unusual password reset activity.
- Disabled or modified logging.
- Conditional access policy changes.
- Federation or SSO configuration changes.
- Service account interactive login.
- Credential dumping alerts.
- Kerberoasting indicators.
- Pass-the-hash or pass-the-ticket indicators.
- Lateral movement using valid credentials.

### Credential source analysis

Investigate likely credential source:

- Phishing.
- MFA fatigue.
- OAuth consent phishing.
- Infostealer malware.
- Browser password store theft.
- Password reuse.
- Credential stuffing.
- Password spraying.
- Exposed password in code or documentation.
- Leaked API key or secret.
- Compromised endpoint.
- Compromised admin workstation.
- Compromised service account.
- Third-party breach.
- Insider misuse.
- Unmanaged personal device.
- Legacy authentication.
- Weak or shared password.
- Lack of MFA.
- Session token theft.

---

## Phase 4: Eradicate

### Objectives

- Remove attacker access.
- Remove identity persistence.
- Restore account integrity.
- Close the initial access vector.
- Reduce future identity compromise risk.

### Remediation actions

- Reset passwords for affected accounts.
- Revoke active sessions and refresh tokens.
- Re-register MFA methods where needed.
- Remove suspicious MFA methods and devices.
- Remove unauthorized OAuth grants and app consents.
- Remove unauthorized role assignments and group memberships.
- Remove unauthorized mailbox rules, forwarding, and delegates.
- Remove unauthorized service principals, app registrations, or client secrets.
- Rotate exposed API keys, access keys, certificates, SSH keys, and secrets.
- Review and harden conditional access policies.
- Disable legacy authentication where possible.
- Enforce MFA for all users.
- Implement phishing-resistant MFA for administrators and high-risk users where possible.
- Require privileged access through hardened admin workstations or PAM.
- Remove standing privileges where not required.
- Implement just-in-time privileged access where available.
- Harden service accounts and reduce permissions.
- Prevent interactive login for service accounts where possible.
- Patch and clean endpoints involved in credential theft.
- Rebuild systems where credential dumping or deep compromise occurred.
- Update detections for observed indicators and behaviors.
- Validate that logging and audit policies are enabled.

### Special remediation: privileged identity compromise

- Perform full privileged access review.
- Review all admin role assignments.
- Review all privileged group memberships.
- Review all recent tenant, domain, and security configuration changes.
- Review all new accounts and service principals.
- Review all newly created secrets and certificates.
- Review all break-glass accounts.
- Rotate privileged credentials from trusted systems only.
- Validate administrative workstations.
- Consider domain or tenant compromise procedures if identity infrastructure trust is uncertain.
- Engage external incident response support if domain, tenant, or federation compromise is suspected.

### Special remediation: service account compromise

- Map all systems using the service account.
- Rotate credentials in a controlled sequence.
- Remove unnecessary privileges.
- Remove interactive login where not required.
- Restrict logon locations.
- Apply strong random passwords or managed identities.
- Store secrets in a secure vault.
- Monitor for failed authentication after rotation.
- Replace shared service accounts where possible.
- Document ownership and rotation procedures.

---

## Phase 5: Recover

### Objectives

- Restore secure identity operations.
- Validate that attacker access has been removed.
- Restore affected users and services safely.
- Monitor for recurrence or re-entry.

### Recovery actions

- Re-enable affected accounts only after remediation is complete.
- Confirm that sessions and tokens were revoked.
- Confirm that passwords, secrets, and keys were rotated.
- Confirm that MFA methods are legitimate.
- Confirm that roles, groups, and permissions are correct.
- Confirm that mailbox rules, forwarding, and delegates are clean.
- Confirm that OAuth grants and app consents are legitimate.
- Confirm that service account dependencies work after credential rotation.
- Confirm that conditional access and MFA policies are active.
- Confirm that logging and alerting are active.
- Confirm that suspicious applications or devices are removed.
- Confirm that affected business applications are functioning correctly.
- Continue enhanced monitoring for at least 14 to 30 days.
- Monitor for repeated login attempts, token reuse, MFA prompts, suspicious OAuth consent, and lateral movement attempts.

### Recovery validation checklist

- No active sessions remain from suspicious sources.
- No suspicious MFA methods remain.
- No suspicious OAuth grants remain.
- No unauthorized group memberships remain.
- No unauthorized admin roles remain.
- No unexpected service principals or app registrations remain.
- No suspicious mailbox forwarding remains.
- No suspicious conditional access or identity policy changes remain.
- No evidence of continued lateral movement exists.
- No evidence of continued data access exists.
- User or service owner confirms normal business operation.

---

## Phase 6: Review / Lessons Learned

### Objectives

- Understand root cause.
- Improve identity controls.
- Improve detection and response.
- Reduce future account compromise risk.
- Track remediation to closure.

### Actions

- Finalize the incident timeline.
- Identify initial access vector.
- Identify compromised identities and affected systems.
- Identify whether data was accessed, altered, or exfiltrated.
- Identify control failures.
- Review MFA coverage and strength.
- Review conditional access effectiveness.
- Review privileged access model.
- Review service account governance.
- Review logging and detection coverage.
- Review user reporting and escalation speed.
- Review response actions and decision quality.
- Update identity hardening baselines.
- Update detection rules and SIEM queries.
- Update this playbook based on lessons learned.
- Track all remediation actions to closure.

### Lessons learned questions

- How was the identity compromised?
- Was MFA enabled?
- Was MFA phishing-resistant?
- Was MFA bypassed, approved, fatigued, or re-registered?
- Did legacy authentication contribute to the compromise?
- Were credentials reused, weak, or exposed?
- Was an endpoint compromised?
- Was a service account overprivileged?
- Were privileged accounts protected adequately?
- Were active sessions revoked quickly enough?
- Did tokens, OAuth grants, or app consents persist after password reset?
- Were logs sufficient to reconstruct activity?
- Did the attacker access data?
- Did the attacker move laterally?
- Did the attacker escalate privileges?
- Did the attacker modify identity infrastructure?
- Which controls would have prevented or reduced impact?

---

## Evidence to Preserve

Preserve the following evidence where available:

### Identity and authentication evidence

- Sign-in logs.
- Failed login logs.
- MFA logs.
- Risk detections.
- Conditional access results.
- Password reset events.
- MFA registration changes.
- Device registration logs.
- Session revocation records.
- Token revocation records.
- User risk and sign-in risk records.
- Identity provider audit logs.

### Directory and privilege evidence

- Group membership changes.
- Role assignment changes.
- Privileged access logs.
- New account creation records.
- Account modification records.
- Account disable / enable records.
- Directory synchronization logs.
- Federation configuration changes.
- GPO changes.
- AD delegation changes.
- Kerberos and NTLM logs.
- Domain controller security logs.

### Cloud and application evidence

- OAuth grant records.
- App consent records.
- Service principal changes.
- App registration changes.
- Client secret and certificate changes.
- API key creation or use logs.
- Cloud IAM logs.
- SaaS audit logs.
- Admin portal activity.
- Resource access logs.
- Data export logs.

### Endpoint and network evidence

- EDR alerts.
- Endpoint process execution logs.
- Credential dumping tool detections.
- Browser credential access indicators.
- PowerShell logs.
- Remote access logs.
- VPN logs.
- RDP, SSH, WinRM, and SMB logs.
- Proxy logs.
- DNS logs.
- Firewall logs.
- SIEM correlation events.

### Communication and decision evidence

- User report.
- Helpdesk ticket.
- Incident timeline.
- Containment decisions.
- Remediation decisions.
- Management escalation records.
- Legal/privacy assessment notes.
- External notification records if applicable.

---

## Communication Checklist

### Internal communication

- Notify incident response team.
- Notify identity administrators.
- Notify affected system and application owners.
- Notify management if severity is High or Critical.
- Notify legal/privacy if data access or regulated data may be involved.
- Notify helpdesk if users may report MFA prompts, lockouts, or suspicious activity.
- Warn affected users if credential theft or phishing is ongoing.
- Maintain a single source of truth for incident updates.
- Avoid broad technical speculation before facts are confirmed.

### External communication

- Notify affected customers, suppliers, or partners if their data or accounts may be affected.
- Notify cyber insurance provider if required.
- Notify law enforcement if fraud, extortion, or major compromise occurred and company policy supports it.
- Notify regulators if data breach or sector-specific obligations apply.
- Preserve all outgoing communications.
- Coordinate external messages with legal/privacy and communications stakeholders.

### Suggested user communication

A suspected identity compromise is under investigation. If you receive unexpected MFA prompts, password reset notifications, unusual login alerts, or messages asking for credentials, report them immediately to the IT/security team. Do not approve unexpected MFA requests and do not change security settings unless instructed through the official incident communication channel.

---

## Legal and Regulatory Considerations

Identity compromise can become a data breach, fraud incident, insider threat incident, or ransomware precursor depending on attacker activity.

Involve legal/privacy stakeholders if:

- Personal data may have been accessed.
- Mailbox contents may include sensitive or regulated data.
- The compromised identity accessed customer, employee, supplier, or financial records.
- Privileged access may have exposed broad datasets.
- Financial fraud occurred or was attempted.
- External parties received malicious communication from a compromised account.
- Contractual, regulatory, insurance, or sector-specific notification obligations may apply.

This playbook does not provide legal advice. It supports evidence preservation, structured technical response, and informed legal/privacy assessment.

---

## Quick Response Checklist

### Immediate response

- Assign incident lead.
- Identify affected identity.
- Determine account type and privilege level.
- Revoke active sessions and tokens.
- Disable or block sign-in if active abuse is ongoing.
- Preserve identity and audit logs.
- Check MFA changes.
- Check role and group changes.
- Check OAuth grants and app consents.
- Start incident timeline.

### Scope

- Review sign-in logs.
- Review MFA logs.
- Review applications accessed.
- Review devices and locations.
- Review privileged actions.
- Review data access.
- Review mailbox activity.
- Review cloud and SaaS activity.
- Review endpoint telemetry.
- Identify related accounts.

### Containment

- Revoke sessions.
- Reset password.
- Require MFA re-registration.
- Remove suspicious MFA methods.
- Remove suspicious OAuth grants.
- Remove unauthorized group memberships.
- Remove unauthorized roles.
- Remove suspicious mailbox rules.
- Rotate exposed secrets and keys.
- Restrict remote access where needed.

### Investigation

- Determine initial access.
- Determine credential source.
- Check phishing and malware indicators.
- Check credential dumping indicators.
- Check lateral movement.
- Check privilege escalation.
- Check persistence.
- Check data access or exfiltration.
- Determine whether data breach playbook is required.

### Remediation

- Remove attacker persistence.
- Rotate credentials and secrets.
- Harden MFA.
- Disable legacy authentication.
- Harden conditional access.
- Reduce privileges.
- Harden service accounts.
- Clean or rebuild affected endpoints.
- Update detections.
- Validate logging.

### Recovery

- Re-enable account only after validation.
- Confirm sessions and tokens are revoked.
- Confirm MFA methods are legitimate.
- Confirm permissions are correct.
- Confirm no suspicious OAuth grants remain.
- Confirm service dependencies work.
- Monitor for 14 to 30 days.
- Track remediation to closure.

---

## Preventive Controls

### Identity security

- Enforce MFA for all users.
- Use phishing-resistant MFA for administrators and high-risk users where possible.
- Disable legacy authentication.
- Apply conditional access.
- Require managed or compliant devices for sensitive access.
- Monitor risky sign-ins and risky users.
- Monitor impossible travel and unfamiliar sign-in properties.
- Monitor MFA fatigue patterns.
- Monitor new MFA method registrations.
- Monitor OAuth app consent and delegated permissions.
- Review external identities and guest accounts regularly.

### Privileged access

- Use dedicated admin accounts.
- Use privileged access workstations or hardened admin devices.
- Use just-in-time privileged access where available.
- Remove standing privileges where possible.
- Review admin roles regularly.
- Protect break-glass accounts with strict monitoring.
- Require strong authentication for privileged actions.
- Monitor privileged role activation and assignment.
- Separate daily user accounts from admin accounts.

### Service account security

- Maintain an inventory of service accounts.
- Assign owners to service accounts.
- Prevent interactive login where not required.
- Restrict logon locations.
- Use least privilege.
- Rotate credentials regularly.
- Store secrets in a secure vault.
- Prefer managed identities or workload identities where possible.
- Monitor service account behavior.
- Replace shared service accounts where feasible.

### Active Directory / hybrid identity

- Monitor privileged group changes.
- Monitor domain controller authentication patterns.
- Monitor Kerberos anomalies.
- Monitor NTLM usage.
- Monitor GPO changes.
- Protect domain controllers.
- Restrict administrative logon paths.
- Tier administrative access.
- Harden directory synchronization.
- Review federation trust and certificates.
- Protect backup and recovery identities.

### Detection and response

- Alert on suspicious sign-ins.
- Alert on new MFA method registration.
- Alert on OAuth consent grants.
- Alert on admin role assignment.
- Alert on suspicious service account login.
- Alert on impossible travel.
- Alert on mass failed login attempts.
- Alert on password spraying.
- Alert on credential dumping tools.
- Alert on lateral movement with valid accounts.
- Test account compromise response procedures regularly.

---

## Website Summary Card

### Identity Compromise

Response guide for compromised user, service, or privileged accounts — covering credential theft, unauthorized access, MFA abuse, token theft, lateral movement detection, and identity recovery procedures.

### Tags

DETECT  
CONTAIN  
INVESTIGATE  
ERADICATE  
RECOVER  
REVIEW

---

## References

- NIST SP 800-61 Rev. 3: Incident Response Recommendations and Considerations for Cybersecurity Risk Management.
- CISA / NSA: Identity and Access Management Recommended Best Practices for Administrators.
- Microsoft Entra ID Protection: Investigate Risk.
- Microsoft Entra ID: Revoke User Access in an Emergency.
- MITRE ATT&CK: Credential Access.
- MITRE ATT&CK: OS Credential Dumping.
- MITRE ATT&CK: Valid Accounts.
- MITRE ATT&CK: Account Manipulation.
- MITRE ATT&CK: Use Alternate Authentication Material.
- CISA Hybrid Identity Solutions Guidance.
- Microsoft Security: Identity and Access Security Best Practices.

---

## Disclaimer

This playbook is a practical community resource. It does not replace legal advice, forensic expertise, cyber insurance requirements, regulatory assessment, or organization-specific incident response planning. Organizations should adapt these procedures to their own identity architecture, cloud platforms, directory services, risk profile, legal obligations, and incident response capabilities.
