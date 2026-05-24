# Phishing & Business Email Compromise Playbook

## Summary

This playbook provides an operational, community-focused incident response procedure for suspected or confirmed phishing, credential theft, mailbox compromise, and Business Email Compromise (BEC) incidents.
It helps incident responders stop ongoing abuse, secure identities and communication channels, preserve evidence, and minimise business impact such as data exposure or fraudulent payments.

The playbook follows a standard IR flow: Detect & Triage, Contain, Investigate, Eradicate, Recovery, and Review. It is intended for security teams, IT administrators, SOC analysts, consultants and small/medium organizations.

> Note: This is not legal advice or a replacement for specialist forensic work. Adapt to your organization’s mail, identity and endpoint stack, legal obligations and internal roles.

## Purpose

- Provide a structured response process for phishing and BEC incidents.
- Enable fast validation, containment and evidence preservation.
- Determine scope across mail, identity, cloud, endpoints and business processes.
- Remove attacker access and unsafe configurations.
- Restore trusted account and communication state.
- Produce documented Lessons Learned and tracked remediation.

## Scope / When to use

Use this playbook when one or more of the following are present:

- A user reports a suspicious email or link.
- A user clicked a suspected phishing link or opened an attachment.
- Credentials may have been entered on a suspected phishing page.
- A mailbox shows unusual sign-in activity.
- Unexpected MFA prompts or approvals are reported.
- Suspicious inbox rules, forwarding or delegates are found.
- Unauthorised emails have been sent from a legitimate account.
- Vendor/customer reports messages appearing to originate from your org.
- Fraudulent payment instructions, payroll redirection or supplier-bank changes are detected.
- A compromised mailbox is being used for impersonation.

This covers classic email-phishing and modern cloud/identity abuse (OAuth consent abuse, token/session misuse) where no malware is necessarily involved.

## Severity

Default severity: **High**.

Escalate to **Critical** if any of the following apply:

- Privileged/admin/executive/finance/HR/system accounts are affected.
- Mailbox is being used to send further phishing internally or externally.
- Payment fraud or confirmed financial loss occurred or is imminent.
- Multiple accounts, tenants or cloud identities are affected.
- Evidence of data access, exfiltration or misuse of sensitive communication.
- Persistent access via OAuth, forwarding, inbox rules or tokens remains after a password change.

## Incident objectives (first hours)

1. Stop ongoing abuse.
2. Secure identities, sessions and communication channels.
3. Preserve evidence and maintain a reliable timeline.
4. Determine scope, initial access vector and business impact.
5. Minimise financial, privacy and operational damage.
6. Conclude incident with tracked remediation and lessons learned.

## Core response principles

- Treat suspected identity compromise as identity-first: examine sessions/tokens before assuming a password-only fix is sufficient.
- Separate confirmed facts from hypotheses; document decisions in the timeline.
- Preserve volatile evidence before destructive remediation.
- Centralize communication and coordinate Finance early for BEC risk.
- For cloud identities include tokens, OAuth consents and app permissions in scope.
- Do not rely on a password change alone unless persistent sessions and consents have been invalidated.
- Search tenant-wide quickly to locate additional affected users.

---

## First 15 minutes (Immediate actions)

- Activate Incident Response Lead / Incident Commander.
- Start an incident timeline and decision log (who, what, when, why).
- Identify the affected account(s), mailbox, device, and the reported phishing item.
- Determine whether links were clicked, attachments opened or credentials submitted.
- Revoke suspicious sessions/tokens if compromise is likely.
- Preserve original message(s) with full headers, URLs, attachment hashes and screenshots.
- Search for similar messages across the tenant.
- Notify Finance / Legal / Management immediately if payments, payroll or executive impersonation are involved.
- Check mailbox rules, forwarding, delegates and OAuth app consents.
- Prepare containment actions (quarantine emails, block URLs, tenant search).

Do NOT:
- Delete the original message before preserving evidence.
- Assume the incident is limited to the reporting user.
- Reset passwords without considering sessions/tokens and persistence.
- Trust mailbox content until rules, delegates and sent items have been reviewed.
- Communicate root cause, data exposure or financial impact externally until verified.

---

## Roles & Responsibilities

- Incident Commander: overall coordination, severity decisions, executive escalation.
- Technical Lead: coordinates email, identity, endpoint, network, cloud responders and technical hypothesis.
- Identity & Access Lead: investigate session/token misuse, MFA, OAuth consents, perform account actions.
- Email / Collaboration Lead: analyze headers/rules/forwarding, quarantine/remove messages.
- Endpoint/Security Analyst: review endpoint telemetry for malware or credential theft artifacts.
- Forensics Lead: preserve and validate evidence and attack path.
- Communications Lead: coordinate internal/external messaging and avoid inconsistent statements.
- Legal / Privacy / Compliance: review regulatory and notification obligations.
- Finance / Business Owner: verify/stop payment flows, validate supplier or bank changes.
- External IR / Insurance / Law Enforcement: involve per policy and incident severity.

---

## Decision model (quick rules)

### Rule 1 — Lock account or monitor?
Lock / restrict account immediately if:
- Credentials were entered on a phishing page.
- Suspicious successful logins, token misuse, or account takeover indicators exist.
- Mailbox sends suspicious messages or shows rule/forwarding manipulation.
- The account is privileged, executive or finance-related.

Monitor only if:
- No successful compromise is visible AND locking would significantly hinder evidence collection AND Incident Commander approves.

### Rule 2 — Is password reset enough?
No if:
- Sessions/tokens remain active.
- OAuth consents / app permissions persist.
- Inbox rules, forwards or delegates were added.
- Attacker persistence or secondary access paths exist.

In those cases also revoke sessions, rotate tokens and remove app consents.

### Rule 3 — Is endpoint forensics required?
Yes if:
- An attachment was opened or executable run.
- Browser-based credential entry with stored sessions or credentials is suspected.
- Endpoint shows infostealer/downloader indicators.
- The user has administrative privileges or access to sensitive resources locally.

No (focus on cloud/identity) if:
- Evidence points purely to OAuth/session abuse or credential submission without local execution.

### Rule 4 — Stop finance process now?
Stop/hold payment processes immediately if:
- Payment instructions, supplier-bank changes or payroll redirection are suspected.
- An approval is pending or soon to be executed.
- Communication trails show manipulated instructions.

### Rule 5 — Can we recover now?
Recovery only if:
1. Attack path is understood.
2. Accounts, sessions, tokens, forwards and OAuth consents are cleaned.
3. No evidence of ongoing misuse remains.
4. Monitoring and communications for follow-on waves are active.

---

## Identity-first focus

Phishing/BEC incidents frequently are identity compromise events. Prioritise:
- Successful/failing sign-ins, location/device anomalies.
- MFA events: approvals, denials, fatigue patterns, new factor registrations.
- Session/refresh token activity and revocations.
- Password resets and changes to recovery options.
- Privileged and business-critical accounts (executive, finance, HR, admin).
- OAuth consents, enterprise apps and app permissions.

---

## Email & communication protection

Attackers often abuse mailbox configuration, not just sign-ins. Prioritise:
- Inbox rules, forwarding and delegates.
- Tenant-wide search for similar messages.
- Removal/quarantine of malicious messages from mailboxes (preserve evidence first).
- Spoofing/Reply-To anomalies and header checks.
- Notify external recipients if compromised mailbox sent malicious messages.
- Review message threads for fraud indicators (bank data changes, invoice manipulations).

---

## Incident classification

Classify early as one or more of:
- Suspicious Email
- Confirmed Phishing Email
- Credential Submission
- Malware Execution
- Account Takeover
- Mailbox Compromise
- Business Email Compromise (BEC)
- Attempted Payment Fraud
- Confirmed Financial Loss
- Data Access / Suspected Data Breach

Classification directs prioritisation and escalation.

---

## Phase 1 — Detect & Triage

Objectives:
- Confirm incident type and whether abuse is active.
- Identify initial scope and high-risk impacts.
- Secure fastest-losing evidence.

Actions:
- Collect user reports, gateway/quarantine entries, SIEM, IdP and endpoint telemetry.
- Preserve original message(s) with full headers and artifacts.
- Determine click/credential/attachment interactions.
- Identify affected accounts, mailboxes and recipients.
- Review sign-in logs, MFA events, session activity and OAuth consent logs.
- Search for similar messages or compromise patterns.
- Check mailbox audit, sent items and recoverable items for attacker activity.
- Note whether the message was internal or from an external address.
- Build and maintain an incident timeline.

Exit criteria:
- Incident type validated.
- Initial scope identified.
- Potentially compromised accounts flagged.
- Evidence preservation started.

---

## Phase 2 — Contain

Objectives:
- Stop active abuse and further interactions.
- Secure accounts, sessions and mail flow.
- Minimise business impact.

Actions:
- Temporarily restrict or disable affected accounts.
- Revoke sessions and refresh tokens.
- Reset passwords when credential theft is likely.
- Remove unauthorized OAuth consents and enterprise apps.
- Remove malicious inbox rules, forwarding and delegates.
- Search tenant-wide and quarantine/remove malicious messages where possible.
- Block malicious domains, URLs, IPs, hashes at gateway and network level.
- Temporarily restrict outbound mail for compromised accounts if they’re sending abuse.
- Engage Finance to pause risky payments and verify changes.
- Isolate endpoints if attachments/local artifacts are suspected.

Do not:
- Assume password change alone resolves problem.
- Focus only on the reporting mailbox if tenant-wide spread is possible.
- Delete evidence before it is preserved.
- Continue payment approvals when BEC risk exists without OOB verification.

Exit criteria:
- Ongoing abuse reduced or stopped.
- Compromised accounts or channels secured.
- Message spread technically limited.
- Appropriate business owners informed (Finance, Legal).

---

## Phase 3 — Investigate

Objectives:
- Determine the attack path and full scope.
- Identify persistence and downstream impact.
- Assess data access and fraud risk.

Actions:
- Analyze headers, SPF/DKIM/DMARC, Reply-To, redirect chains and attachment artifacts.
- Review sign-in logs for geolocation anomalies, device changes or risky sign-ins.
- Examine MFA events (approvals, denials, unusual patterns).
- Evaluate session/token behaviour and refresh activity.
- Analyze OAuth/consent events and app permissions.
- Investigate mailbox-audit logs (send-as/send-on-behalf, deletions, searches).
- Identify recipients who received/opened/responded.
- Search for related messages across tenants and external domains.
- Assess whether data stores (OneDrive, SharePoint, file shares) were accessed.
- Review endpoint telemetry when local compromise is suspected.

Scope documentation categories:
- Affected accounts & mailboxes
- Affected users/roles
- Affected recipients & external parties
- Tokens, sessions, apps, OAuth consents
- Financial/process impacts
- Data stores and files accessed
- Endpoints implicated

Exit criteria:
- Plausible attack path established.
- Compromised accounts, sessions and misconfigurations enumerated.
- Fraud/data impact assessed to plan eradication.

---

## Phase 4 — Eradicate

Objectives:
- Remove attacker access and persistence.
- Clean malicious configurations and close entry vectors.
- Restore trusted access posture.

Actions:
- Perform controlled password resets.
- Invalidate active sessions and refresh tokens tenant-wide for affected users.
- Remove unauthorized OAuth grants, enterprise apps and app-consents.
- Remove inbox rules, forwards, delegates and reply-to manipulations.
- Review and reset recovery methods, MFA factors and self-service settings.
- Rotate API tokens, app secrets and stored credentials if exposed.
- Rebuild or remediate endpoints if local compromise cannot be trusted.
- Harden gateway/tenant/IdP protections and detection rules.
- Adjust payment-verification processes where BEC/fraud was involved.

Exit criteria:
- Access paths removed.
- Mailbox and identity configuration cleaned.
- Sessions/tokens/app access no longer active.
- Compensating protections implemented.

---

## Phase 5 — Recovery

Objectives:
- Restore trusted access and communication channels.
- Resume business operations safely.
- Monitor for recurrence.

Recovery gate — do NOT recover until:
- Attack path is sufficiently understood.
- Accounts, sessions, tokens and forwarding/OAuth consents are cleaned.
- Active misuse indicators are absent.
- Monitoring, detection and communications for follow-up waves are in place.

Actions:
- Re-enable accounts in a controlled manner.
- Re-establish or strengthen MFA (prefer phishing-resistant options for high-risk users).
- Inform users of safe re-start steps and any forced changes (passwords, MFA).
- Notify partners, customers or vendors if they were affected by messages.
- Increase monitoring for re-entry signs for 14–30 days.
- Continue tenant searches for related messages or indicators.
- Validate endpoints/browsers where relevant before normal operations resume.

Exit criteria:
- Accounts and channels are deemed trusted.
- No active abuse indicators remain.
- Business processes are stable and owners assigned for residual risks.

---

## Phase 6 — Review / Lessons Learned

Objectives:
- Identify root cause and control gaps.
- Improve detection, prevention and response.
- Finalize evidence and decisions.
- Prevent recurrence.

Actions:
- Conduct a structured post-incident review within 5–10 business days of stabilization.
- Finalize the incident timeline and decision log.
- Confirm the attack path, message patterns and scope.
- Identify control weaknesses in mail security, identity, MFA, conditional access, fraud workflows and user awareness.
- Evaluate response performance of Helpdesk, Security, Finance, Legal and Management.
- Update detection rules, mailbox protections and tenant-hardening measures.
- Strengthen payment verification and supplier change procedures.
- Update this playbook with lessons learned and assign remediation owners with due dates.

---

## Evidence to preserve

- Original phishing email(s) with full headers and Message-ID.
- Envelope sender, Reply-To, extracted URLs, redirect chains and attachment hashes.
- Screenshots of phishing pages and submitted form evidence (if available).
- Sign-in logs, MFA logs and password reset logs.
- Session & token logs, refresh token events.
- Mailbox audit logs (rules, forwarding, delegates).
- OAuth/consent logs and enterprise app changes.
- Sent items, deleted items and recoverable items.
- Quarantine and message-trace records.
- Firewall, proxy, DNS, EDR and endpoint logs.
- Payment/invoice and supplier-change records.
- Incident timeline and decision log.

---

## Communications checklist

Internal:
- Notify IR stakeholders: IT, SOC, Helpdesk, Management, Legal, Finance, affected business areas.
- Provide clear instructions to users: report message, do not delete original, do not act on suspicious instructions.
- Maintain a single authoritative incident status channel.
- Give helpdesk an FAQ / triage script for user questions.

External:
- Inform external recipients if compromised accounts were used to send malicious messages.
- Use verified out-of-band channels (not the compromised thread) to contact partners/customers.
- Coordinate all external messaging with Legal and Communications.
- Preserve all external message drafts for evidence.
- Do not make unverified claims about data exposure or financial loss.
- Notify banks, law enforcement or insurers as per policy when fraud occurred.

Suggested short user warning:
> A suspicious email campaign has been detected. Do not click links, open attachments or reply to messages from this campaign. If you received or interacted with such a message, report it immediately to the IT/security team and do not delete the email.

---

## Legal & regulatory considerations

Phishing/BEC incidents may trigger contractual, regulatory and notification duties if personal data, sensitive business data or financial transactions are affected. Involve Legal/Privacy early if:

- Personal data or customer records may have been accessed.
- Mailbox contents include regulated data.
- Fraudulent payments occurred or supplier banking details changed.
- Compromised accounts were used to impersonate customers or partners.

This playbook is not legal advice; preserve evidence and escalate to counsel for notification decisions.

---

## Quick response checklist

Immediate:
- [ ] Activate Incident Commander.
- [ ] Start timeline & decision log.
- [ ] Identify affected account/mailbox and preserve original message(s).
- [ ] Revoke suspicious sessions/tokens.
- [ ] Notify Finance / Legal / Management if relevant.

Scope:
- [ ] Identify all affected accounts/mailboxes.
- [ ] Identify recipients and communication trails.
- [ ] Check inbox rules/forwarding/delegates.
- [ ] Review sign-in, MFA and session activity.
- [ ] Check OAuth/consent changes.
- [ ] Assess data access and fraud indicators.

Containment:
- [ ] Restrict or disable account(s).
- [ ] Revoke sessions & tokens.
- [ ] Reset passwords where needed.
- [ ] Remove OAuth/app access.
- [ ] Search and remove malicious messages where possible.
- [ ] Secure risky financial processes.

Investigation:
- [ ] Determine attack path.
- [ ] Determine scope of abuse.
- [ ] Identify persistence & mailbox manipulation.
- [ ] Assess fraud and data impact.
- [ ] Determine endpoint involvement.

Recovery:
- [ ] Validate recovery gate.
- [ ] Re-enable account(s) in controlled manner.
- [ ] Strengthen MFA and protections.
- [ ] Inform stakeholders of corrected communications.
- [ ] Monitor for follow-up attacks.

Post-incident:
- [ ] Finalize timeline.
- [ ] Confirm root cause.
- [ ] Update detections and controls.
- [ ] Update playbook & training.
- [ ] Track remediation to closure.

---

## Preventive controls

Email security:
- Enforce SPF, DKIM and DMARC.
- Use anti-phishing/impersonation protections and sender-rewrite/URL-rewrite where applicable.
- Sandbox attachments and detonate suspicious files.
- Restrict auto-forwarding to external addresses.
- Monitor creation of new inbox rules and tenant mail-flow rules.
- Use external-sender banners or warnings appropriately.

Identity security:
- Enforce MFA for all users; prefer phishing-resistant methods for high-risk accounts.
- Disable legacy authentication.
- Apply Conditional Access.
- Monitor impossible-travel and risky sign-ins.
- Monitor MFA fatigue/auto-approve patterns and new factor registrations.
- Regularly review OAuth app consents.

Business process controls:
- Require out-of-band verification for supplier-bank changes and high-risk payments.
- Implement dual-approval for significant transfers.
- Verify supplier bank changes using trusted channels.
- Do not rely on email alone for urgent payment instructions.
- Train finance, HR and executive assistants on BEC indicators.
- Maintain clear escalation for suspected fraud.

---

## Website summary block (short)

**Phishing & Business Email Compromise**  
Structured response playbook for suspected or confirmed phishing, credential theft and BEC — from early detection and containment to identity hardening, secure recovery and lessons learned.

Core phases: Detect (Triage), Contain, Investigate, Eradicate, Recovery, Review.

---

## References

- NIST SP 800-61: Computer Security Incident Handling Guide  
- CISA: Stopping the Attack Cycle at Phase One  
- Microsoft: Responding to a compromised email account  
- FBI: Business Email Compromise guidance  
- FBI IC3: BEC public advisories
