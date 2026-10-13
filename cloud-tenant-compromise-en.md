# Playbook: Cloud Tenant Compromise

## Summary

This playbook structures the response to a suspected or confirmed compromise of a cloud tenant: Microsoft 365, Entra ID and Azure, AWS, Google Workspace and Google Cloud, a central identity provider such as Okta, and connected SaaS, MDM, EDR, SIEM and backup platforms.

A compromised tenant is not an ordinary account incident. When attackers can influence administrative roles, federation, OAuth applications, workload identities, logging or recovery, the trust model of the entire environment is in question. The response also often takes place in the same management plane that the attacker may control.

The core of this playbook: **First establish whether the control plane can still be trusted. Then restore users, applications, data and workloads in a controlled way.**

Concrete commands, portals and log sources are in the platform appendices at the end of this document:

- [Appendix A: Microsoft 365, Entra ID and Azure](#appendix-a-microsoft-365-entra-id-and-azure)
- [Appendix B: AWS Organizations and IAM](#appendix-b-aws-organizations-and-iam)
- [Appendix C: Google Workspace and Google Cloud](#appendix-c-google-workspace-and-google-cloud)
- [Appendix D: Okta](#appendix-d-okta)

## Purpose

- Keep or regain administrative control of the tenant
- Stop active attacker access without destroying evidence
- Preserve logs before they expire or are manipulated
- Find and remove persistence in identities, applications, policies and workloads
- Decide whether the tenant can still be trusted after cleanup
- Restore services in a controlled way and in the right order

## Scope

Use this playbook if at least one of the following applies:

- Unauthorized administrative activity in a cloud tenant is discovered.
- A Global Administrator, root account, Super Admin or another privileged role is compromised.
- New admin accounts, roles, service principals, app registrations or OAuth grants appear unexpectedly.
- Conditional Access, MFA, SSO, federation, audit logging or retention are changed unexpectedly.
- Security tools, SIEM connectors or alerting are disabled or bypassed.
- Unusual access to mailboxes, cloud storage, databases, workloads or management APIs is observed.
- Cloud resources are created, deleted, encrypted or used as attacker infrastructure, for example for cryptomining.
- Mass downloads, mailbox exports or public sharing point to data exfiltration.
- Break-glass accounts, backup or recovery systems in the tenant show anomalies.
- Administrative control is partly or completely lost.
- A third party, the cloud provider or a ransomware actor reports access to your cloud environment.

**Boundaries:**

- A single taken-over user account without administrative rights is primarily an identity compromise. This playbook takes over as soon as roles, policies, applications or tenant-wide settings are affected.
- Hybrid identities and on-premises directories are in scope as soon as they share trust, accounts or synchronization with the tenant.
- With encryption or destruction on a large scale, the ransomware playbook additionally takes the lead.

## Severity

Default severity: **Critical**.

Escalate immediately to management and, where needed, the cloud provider if:

- the highest roles are affected: Global Administrator, root, Super Admin, tenant or organization administrator
- the identity provider, federation or directory synchronization has been manipulated
- logging, retention or security tools have been changed or disabled
- backup, recovery, key management or secrets management are affected
- destructive actions are running: deletions, purges, encryption, resource or tenant wipe
- administrative control is lost

## Incident Objectives

1. Secure or regain administrative control.
2. Preserve logs and evidence before they expire or are changed.
3. Stop active attacker access and ongoing damage.
4. Determine initial access, rights obtained, persistence and affected data.
5. Decide whether the tenant is cleaned up or rebuilt in a clean environment.
6. Restore services in a controlled way and detect re-entry early.

## Core Response Principles

- **Do not respond inside the attacker's control plane.** Coordinate through a channel that does not depend on the compromised tenant; administer only from clean admin devices.
- **Logs first.** Without export, many cloud logs are only available for days or a few weeks. Missing logs are not a negative finding.
- **Disable before deleting.** Remove attacker objects only after their configuration, credentials, permissions and audit trail have been preserved, unless they are actively causing damage.
- **A password reset is not containment.** Sessions, refresh tokens, app secrets, certificates, keys and trust relationships must each be handled.
- **MFA is not proof of cleanliness.** Stolen sessions, OAuth grants and workload identities bypass MFA.
- **Never just one recovery path.** Secure at least two independent, verified break-glass accounts before making far-reaching changes.
- **Collect read-only, change with approval.** Containing changes only after evidence preservation and a documented decision, under the four-eyes principle.
- **Record time consistently.** Document the timeline, log exports and decisions in UTC.

## Contacts and Escalation

Pre-fill these for your organization and keep them available offline:

- Incident lead on call
- Cloud, tenant and identity administration
- Emergency path to the cloud provider: support contract, customer number, verified phone number
- Application, data and backup owners
- External forensics and incident response providers
- Cyber insurance
- Reporting bodies: BSI (DE), competent CSIRT (AT), NCSC/BACS (CH)
- Management, legal, data protection and communications

## First 15 Minutes

- Name an incident lead, start the timeline and decision log.
- Move coordination to a channel outside the tenant.
- Check whether administrative control exists and whether the break-glass accounts work and are unchanged.
- Work only from clean admin devices.
- Export sign-in, audit, admin and activity logs, starting with the shortest-lived sources.
- Revoke sessions and refresh tokens of confirmed compromised accounts and block the accounts.
- Disable suspicious admin accounts, roles, service principals and apps — do not delete them.
- Check whether data is being exfiltrated, deleted or encrypted right now.
- Give management, legal and data protection an early warning.

## Roles and Responsibilities

### Incident lead

- Steers the response and prioritizes actions.
- Approves containment, eradication and recovery.
- Keeps the timeline, situation picture and decision log.

### Cloud and tenant administration

- Reviews tenant configuration, roles, policies and resources.
- Revokes sessions and removes unauthorized access.
- Handles technical contact with the cloud provider.

### Identity and access

- Reviews sign-ins, MFA, Conditional Access, SSO, federation and directory synchronization.
- Removes persistence in identities and restores the trust model.

### SOC and forensics

- Preserves logs, configuration snapshots and artifacts.
- Correlates identity, cloud, mail, SaaS and endpoint telemetry.
- Reconstructs initial access, attacker path and persistence.

### Application, data and backup owners

- Assess data access, integrations and business impact.
- Protect backups from deletion and tampering and test restoration.

### Legal, data protection and communications

- Assess reporting, contractual and insurance obligations.
- Steer internal and external communication.

### Management and crisis team

- Approves far-reaching measures, such as tenant-wide blocks or a rebuild in a clean environment.
- Provides resources and steers business continuity.

### Cloud provider and external incident response

- Support with loss of control, account recovery, log retrieval and extended analysis.
- Are engaged through the contractually agreed, verified path.

## Decision Model

### Rule 1: Can the tenant still be trusted?

**Consider a rebuild in a clean environment** if:

- administrative control was lost and it cannot be fully traced what the attacker did during that time
- logging was disabled, deleted or incomplete during the compromise
- break-glass accounts, federation or SSO trust relationships were manipulated
- several unknown privileged identities, service principals or workload identities were found
- backup, recovery or security controls were manipulated
- it cannot be demonstrated that the attacker's persistence has been removed

**Cleanup within the existing tenant** is only justifiable if:

- administrative control existed throughout or was regained without gaps
- logs for the entire period are complete and unaltered
- all of the attacker's changes can be identified and reversed
- the identity provider, federation and break-glass access are demonstrably trustworthy

A rebuild is planned in stages: identity, logging, security and backup first. The compromised tenant is preserved for the investigation where possible. During migration, do not carry over identities, permissions, apps or secrets without checking them.

### Rule 2: Act yourself or engage the cloud provider?

**Engage the provider immediately through the verified support path** if:

- administrative control or root, Super Admin or Global Admin access is lost
- logs are needed that are no longer available in the tenant
- there is a suspicion that the platform itself or a provider process was exploited
- resources, billing or the tenant as a whole are at risk of being locked or deleted

**Continue with your own resources** if:

- at least two trusted admin accounts are secured
- the logs you need have been exported
- containment can be carried out fully with your own rights

### Rule 3: Shut down federation, synchronization or the identity provider?

**Withdraw or shut down the trust relationship** if:

- federation certificates, token signing keys or trust relationships were changed
- the identity provider or the synchronization server itself is compromised
- tokens for arbitrary users could be issued through the federation

**Keep running under enhanced monitoring** if:

- no manipulation of federation, synchronization or signing keys is visible
- shutting it down would cut off access for the entire organization without an alternative
- the incident lead and management approve the risk acceptance in writing

Before shutting it down, ensure an alternative sign-in path for administrators, such as cloud-only emergency accounts.

### Rule 4: Targeted or tenant-wide revocation?

**Revoke sessions tenant-wide and restrict access** if:

- privileged roles, federation or token signing keys are compromised
- the set of affected accounts cannot be determined
- token theft on a larger scale is likely

**Targeted at affected accounts and applications** if:

- the affected identities are clearly delimited
- no tenant-wide settings were changed
- tenant-wide revocation would cause disproportionate disruption

### Rule 5: When to activate other playbooks?

- **Identity compromise in parallel** for affected individual accounts and their sessions.
- **Data breach in parallel** if personal, regulated or confidential data may have been accessed, exported or exposed.
- **Ransomware playbook in the lead** if data or resources are encrypted or deleted on a large scale.
- **Supply chain in parallel** if access came through a service provider, a third-party app or a CI/CD integration.
- **Insider incident** if an authorized person intentionally misused their rights.

## Situation and Action Matrix

| Situation | Minimum action |
|---|---|
| Privileged account compromised | Revoke sessions, block the account, review role and policy changes in the period |
| Identity provider, federation or sync manipulated | Apply rule 3, review all privileged identities and trust relationships, revoke tenant-wide |
| Malicious OAuth app or consent abuse | Disable the app, revoke grants, review the app's access to mail, files and APIs, search the tenant for the same pattern |
| Exposed keys, secrets or certificates | Disable first, check usage in the logs, provide a replacement, then rotate and delete |
| Data exfiltration | Block public links and external sharing, suspend access, preserve access logs, data breach playbook |
| Destructive activity | Stop automation, lock resources, protect backups, snapshots and retention |
| Logging or security tools manipulated | Preserve logs externally, restore configuration, document gaps for rule 1 |
| Loss of control | Engage the cloud provider per rule 2, bring in external incident response |

## Typical Persistence in the Tenant

- New or modified admin accounts, role assignments and privileged group memberships
- New service principals, app registrations and workload identities
- Additional client secrets, certificates, API keys or access keys on existing identities
- Malicious OAuth grants and admin consents
- Conditional Access exclusions, disabled MFA requirements, new authentication methods
- Modified federation trusts, SSO configurations or identity provider routing
- New guest accounts and external identities
- Automation: runbooks, functions, scheduled jobs, CI/CD credentials, hooks and workflows
- Mailbox forwarding, transport and inbox rules, delegations
- Modified logging, retention, backup or deletion protection settings

## Recovery Priority

1. Communication outside the tenant and response coordination
2. Trusted administrative access and break-glass accounts
3. Identity provider and privileged access
4. Logging, audit retention and SIEM connectivity
5. Security controls and alerting
6. Backup and recovery protection
7. Mail, collaboration and core SaaS services
8. Business-critical applications and data stores
9. Cloud workloads and infrastructure
10. Third-party integrations and automation
11. Non-critical services

## Phase 1: Detect and Triage

### Objectives

- Confirm the tenant compromise and determine its level
- Establish whether administrative control is affected
- Preserve evidence before it expires or is changed

### Actions

- Document the first alert, the time of discovery and the earliest known suspicious event.
- Determine affected tenants, subscriptions, accounts, projects, workspaces and SaaS instances.
- Review sign-ins of privileged accounts: location, device, client, anonymization services, unusual user agents.
- Review MFA prompts, MFA method changes and new device registrations.
- Review changes to Conditional Access, SSO, federation, roles and groups.
- Review new apps, service principals, workload identities, OAuth grants, secrets, certificates and keys.
- Review mailbox access, forwarding, eDiscovery and export activity.
- Review storage access, sharing links, public access and mass downloads.
- Review changes to compute, network, IAM, logging, retention, backup and security tools.
- Review billing and resource spikes as well as unusual regions.
- Assess the completeness and trustworthiness of the logs.

### Initial classification

- **User level:** individual accounts without administrative rights
- **Admin level:** privileged roles or tenant-wide settings affected
- **Application and workload level:** service principals, OAuth apps, workload identities, keys
- **Identity provider level:** federation, SSO, synchronization or signing keys affected
- **Data level:** access to, export, exfiltration or deletion of data
- **Loss of control:** administrative control partly or completely lost

### Exit criteria

- Preliminary classification documented
- Short-lived logs exported and preserved
- Status of administrative control and break-glass accounts known
- First hypothesis on initial access and scope documented

## Phase 2: Contain

### Objectives

- Stop attacker access
- Secure administrative control
- Prevent destructive activity and data exfiltration
- Protect logging, security, backup and recovery

### Actions

- Review and secure break-glass accounts and ensure at least two independent admin accounts.
- Revoke sessions and refresh tokens of affected accounts and block the accounts; scope per rule 4.
- Remove unauthorized roles and group memberships.
- Disable unauthorized accounts, guests, service principals, workload identities and apps.
- Revoke malicious OAuth grants and app consents.
- Disable exposed credentials, keys, certificates and secrets, then rotate them in a controlled way.
- Restore manipulated MFA and Conditional Access policies, remove risky exclusions.
- Block public access to exposed storage and applications.
- Pause suspicious automation, scripts, runbooks, functions and pipelines.
- Protect audit logs, retention, SIEM connectivity and alerting.
- Protect backups, snapshots and recovery vaults from deletion.
- On loss of control, engage the cloud provider per rule 2.

### Do not

- Do not work from a possibly compromised admin workstation.
- Do not delete attacker objects before evidence is preserved, unless they are actively causing damage.
- Do not blindly rotate all secrets without knowing their dependencies.
- Do not shut down critical service principals, synchronization or federation without an alternative path.
- Do not rely on a single admin account for recovery.
- Do not trust logs until you have checked whether logging or retention was manipulated.
- Do not exchange incident details via mail, chat or documents in the compromised tenant.

### Exit criteria

- No active attacker access visible any more
- At least two trusted admin accounts secured
- Logging, backup and security tools protected
- Ongoing data exfiltration and destructive activity stopped

## Phase 3: Investigate

### Objectives

- Determine initial access, rights obtained and persistence
- Determine affected data, resources and integrations
- Prepare the decision under rule 1

### Actions

- Reconstruct all administrative actions during the compromise period.
- Review creation and modification of users, groups, roles, policies and applications.
- Analyze management API activity, including by service principals and workload identities.
- Review access to and exports from mailboxes, files, storage and databases.
- Review changes to network, firewalls, DNS, routing and private endpoints.
- Review Key Vault, KMS, Secrets Manager, backup, snapshots and retention.
- Review CI/CD, automation accounts, runbooks and serverless functions.
- Review guest accounts, external identities and federation trusts.
- Review admin activity in MDM, EDR and other security tools.
- Identify resources created by the attacker and billing anomalies.
- Search systematically against the persistence list above.

### Possible initial access

- Phishing, token theft or an infostealer on an admin device
- Password spraying, credential stuffing, weak or missing MFA, legacy authentication
- Exposed passwords, API keys, access keys or CI/CD secrets
- Malicious OAuth consent or a compromised third-party integration
- Compromised service principal or workload identity
- Misconfigured federation or SSO
- Vulnerable internet-facing application
- Insider misuse or a compromised service provider

### Areas for the impact assessment

- Identity provider and management plane
- Mail, collaboration and SaaS applications
- File and object storage, databases and data warehouses
- Compute, containers, Kubernetes and serverless
- CI/CD, secrets management and keys
- Backup, recovery, monitoring and logging
- Security tools and network configuration
- Billing, subscriptions, external users and third-party integrations

### Exit criteria

- Plausible initial access and attacker path documented
- Persistence and affected identities, data and resources named
- Decision under rule 1 taken and documented
- Handover to the data breach, identity or ransomware playbook completed where required

## Phase 4: Eradicate

### Objectives

- Completely remove attacker access and persistence
- Restore a trustworthy tenant configuration
- Close the initial access path

### Actions

- Remove unauthorized users, guests, groups, roles, service principals, apps and workload identities after preservation.
- Remove malicious OAuth grants and delegated permissions.
- Remove unauthorized secrets, certificates and keys; rotate all exposed credentials.
- Revoke sessions, refresh tokens and application tokens again.
- Restore secure MFA, Conditional Access, federation and SSO configuration; remove unapproved exclusions.
- Restore logging, audit, retention, alerting and SIEM connectivity.
- Re-enable security tools and verify that they work.
- Remove malicious automation, deployment artifacts and resources created by the attacker.
- Patch exploited workloads; reimage compromised endpoints that were used for administration.
- If a rebuild was decided: migrate to the clean environment in stages, following the recovery priority.

### Exit criteria

- Persistence removed or rebuild completed
- All exposed credentials rotated
- Logging and security controls active and verified
- Initial access path closed

## Phase 5: Recover

### Objectives

- Restore secure tenant operations
- Detect re-entry early

### Actions

- Release services in the order of the recovery priority.
- Confirm that administrative control is secured and break-glass accounts are monitored.
- Confirm that Conditional Access, MFA, logging, retention, security tools and backup protection are active.
- Restore affected workloads only from trusted sources.
- Check whether third-party integrations are still needed and secure.
- Enhanced monitoring for 30 to 90 days, depending on severity.

### Enhanced monitoring

- Sign-ins by administrators and from known attacker infrastructure
- New role assignments, service principals, app registrations, secrets and certificates
- OAuth consents
- Changes to Conditional Access, MFA, federation and SSO
- Changes to logging, retention, backup and security tools
- Mass downloads, exports, mailbox searches and public sharing
- Spikes in resource creation or deletion

### Exit criteria

- Critical services running stably
- Monitoring without new anomalies
- Residual risks documented and assigned

## Phase 6: Review

### Actions

- Hold the post-incident review within ten business days.
- Document the timeline, initial access, persistence, affected data and decisions taken.
- Review the identity and privileged access model.
- Review OAuth consent and application governance.
- Review the rights of service principals and workload identities.
- Review logging, retention and monitoring coverage.
- Review and test backup and recovery resilience.
- Review access by service providers and third parties.
- Update detection rules, playbooks and hardening baselines.

### Guiding questions

- How did the attacker get in, and why did MFA not prevent it?
- Were privileged and break-glass accounts sufficiently protected?
- Were apps, service principals or workload identities over-privileged?
- Were the logs sufficient to reconstruct the activity, and were they protected from tampering?
- Could the tenant still be trusted after cleanup?
- Which control would have limited the damage the most?

## Evidence to Preserve

- **Identity:** sign-ins (including failed and non-interactive), MFA and Conditional Access results, risk detections, password and MFA changes, device registrations, role and group changes
- **Tenant configuration:** snapshots of Conditional Access, MFA policies, SSO and federation, synchronization, audit and retention settings, external sharing, guest access, admin roles, break-glass accounts
- **Applications and workload identities:** app registrations, service principals, OAuth grants, admin consents, secrets, certificates, keys, managed identities, workload identity federation
- **Cloud resources:** management plane activity logs, resource creation, modification and deletion, storage and database access, Key Vault or KMS logs, network changes, Kubernetes, serverless and CI/CD logs, backup logs, billing data
- **SaaS and collaboration:** mailbox audit, message trace, forwarding and transport rules, eDiscovery and content searches, file access, sharing links, exports
- **Security tools:** SIEM, XDR, EDR, CSPM, DLP and CASB alerts, indications of disabled controls
- **Decisions:** timeline, decision log, cloud provider tickets, communication with external incident response, authorities and affected parties, reasons for a decision to report or not to report

Log sources, export paths and example commands per platform are in appendices A to D.

## Communication Checklist

### Internal

- Inform the response team, cloud and identity administration, SOC, forensics and management through the channel outside the tenant.
- Involve application, data and backup owners.
- Prepare the helpdesk for sign-in and access problems.
- Keep a single reliable source for situation updates, factual and versioned.

### External

- Contact the cloud provider only through verified support paths.
- Involve cyber insurance and external incident response according to contract.
- Inform customers, suppliers or partners if their data or access may be affected.
- Coordinate external statements with legal, data protection and communications, and keep them on record.

### Template: internal notice

> We are currently investigating a possible compromise of our cloud environment. Until further notice, please make no unapproved changes to tenant-wide settings, do not delete suspicious objects, and do not discuss details via mail, chat or documents in this environment. Coordination only through [channel outside the tenant].

### Template: external statement

> We are investigating a security incident involving possible unauthorized activity in our cloud environment. Containment and assessment measures are under way, including a review of administrative access, security controls and possible data exposure. We will inform affected parties in line with applicable obligations as soon as reliable findings are available.

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

- The compromise of a cloud tenant holding mail, files or customer data almost always involves personal data. Start the data protection assessment early, not after the analysis is complete.
- The cloud provider is usually a processor (Art. 28 GDPR). If the cause lies with the provider, it must inform you without undue delay under Art. 33(2) GDPR; the obligation to notify the supervisory authority remains yours.
- Loss of control over central identity and collaboration services is often a significant security incident under NIS2 and the ISA.
- Contracts with customers and partners, and the cyber insurance policy, often contain their own, shorter reporting deadlines.

## Quick Response Checklist

### Immediately

- [ ] Incident lead, timeline and decision log started
- [ ] Coordination outside the tenant
- [ ] Only clean admin devices in use
- [ ] Administrative control and break-glass accounts checked
- [ ] Short-lived logs exported
- [ ] Sessions and tokens of affected accounts revoked
- [ ] Suspicious accounts, roles and apps disabled

### Scope

- [ ] Affected tenants, accounts, projects and SaaS platforms determined
- [ ] Affected users, administrators, apps and workload identities determined
- [ ] Manipulation of logging and security controls checked
- [ ] Data access, exfiltration and destructive actions checked

### Containment and eradication

- [ ] Unauthorized roles, apps, grants and credentials removed
- [ ] Exposed secrets and keys rotated
- [ ] MFA, Conditional Access, federation and logging restored
- [ ] Backups and recovery protected
- [ ] Decision on cleanup or rebuild documented

### Afterwards

- [ ] Services restored by priority
- [ ] Enhanced monitoring for 30 to 90 days set up
- [ ] Reporting obligations assessed
- [ ] Actions tracked to completion

## Preventive Controls

### Identity and privileged access

- Phishing-resistant MFA, at least for administrators
- Separate admin accounts, privileged access workstations and just-in-time rights
- At least two protected and monitored break-glass accounts
- Legacy authentication disabled, Conditional Access applied
- Regular review of admin roles

### Applications and OAuth

- Restrict user consent, use an admin consent workflow
- Review OAuth grants, service principals and app permissions regularly
- Managed identities or workload identities instead of long-lived secrets
- Remove unused apps and integrations

### Tenant configuration, logging and recovery

- Secure tenant baseline, policy changes monitored, as code where possible
- Audit, identity, mailbox and data access logging active and forwarded to a SIEM outside the tenant
- Alerts on role assignments, new apps and credentials, OAuth consents, logging changes, mass downloads and deletions
- Backups protected from deletion, independent recovery paths, regularly tested restoration

## At a Glance

This block is not a shortened replacement playbook. It is a quick way in; all sections of this document and the matching platform appendix apply when handling the incident.

**Cloud Tenant Compromise**  
Response to compromised cloud tenants and identity providers: secure administrative control, export logs before they expire, remove persistence in roles, apps and policies, and decide whether the tenant can still be trusted — with appendices for Microsoft, AWS, Google and Okta.

**Core phases:** Detect, Contain, Investigate, Eradicate, Recover, Review.

## References

- NIST SP 800-61 Rev. 3
- CISA: Microsoft Expanded Cloud Logs Implementation Playbook
- CISA Advisory AA21-008A: Detecting Post-Compromise Threat Activity in Microsoft Cloud Environments
- Microsoft Entra ID: Revoke user access in an emergency
- MITRE ATT&CK: T1078.004, T1098.001, T1528, T1538, T1619, T1567.002
- CIS Critical Security Controls and CIS Benchmarks for cloud providers
- Cloud Security Alliance: Cloud Controls Matrix

## Appendix A: Microsoft 365, Entra ID and Azure

Commands contain placeholders. Run them from a clean admin workstation with a verified response account. Insert times as UTC in the format `2026-10-01T00:00:00Z`.

### Rules of engagement

- Before any change, document the time window, tenant ID, executing account and justification.
- Export sign-in, directory audit, unified audit and Azure activity data as early as possible.
- Review user, service principal and managed identity activity separately.
- A password reset replaces neither session revocation nor the rotation of app secrets, certificates or federated trusts.
- Guest accounts authenticate in their home tenant; `revokeSignInSessions` in your tenant does not end their sessions in the home tenant.

### Key log sources

| Source | What to look for |
|---|---|
| Entra: interactive and non-interactive sign-ins | Unusual IPs, devices, Conditional Access result, client type, legacy auth |
| Entra: service principal sign-ins | New app usage, unusual resources, misuse of secrets or certificates |
| Entra: managed identity sign-ins | Use outside the expected Azure resources |
| Entra: directory audit | Roles, users, groups, MFA, apps, consent, federation, Conditional Access |
| Microsoft 365: Unified Audit Log | Exchange, SharePoint, OneDrive, Teams, Entra and other services |
| Exchange Online: mailbox audit, admin audit, message trace | Mailbox access, rules, forwarding, delegations, transport changes |
| Purview: audit and eDiscovery | Exports, searches, retention, DLP and compliance activity |
| Defender: incidents, alerts, advanced hunting | Correlation across identity, endpoint, mail and cloud |
| Azure: Activity Log | Changes to subscriptions and resources (control plane) |
| Azure: resource logs | Key Vault, Storage, SQL, Firewall and other data plane logs |

Retention without export: Entra sign-in and audit logs 7 days without a license, 30 days with Entra ID P1/P2; Unified Audit Log 180 days with Audit Standard; Azure Activity Log 90 days. Retrieving sign-in logs through Microsoft Graph requires Entra ID P1/P2, otherwise export them in the portal. Missing data is not a negative finding.

### Context and identity (read-only)

```powershell
Connect-MgGraph -Scopes "AuditLog.Read.All","Directory.Read.All","Policy.Read.All","Application.Read.All","RoleManagement.Read.Directory"
Get-MgContext
Get-MgOrganization | Select-Object Id,DisplayName,VerifiedDomains

$Start = [datetime]"<START_UTC>"
$End   = [datetime]"<END_UTC>"
$User  = "user@example.com"
```

### Export sign-ins and directory audit

```powershell
$From = $Start.ToString('yyyy-MM-ddTHH:mm:ssZ')
$To   = $End.ToString('yyyy-MM-ddTHH:mm:ssZ')

Get-MgAuditLogSignIn -All -Filter "createdDateTime ge $From and createdDateTime le $To" |
  ConvertTo-Json -Depth 20 |
  Set-Content -Encoding utf8 "entra-signins.json"

Get-MgAuditLogDirectoryAudit -All -Filter "activityDateTime ge $From and activityDateTime le $To" |
  ConvertTo-Json -Depth 20 |
  Set-Content -Encoding utf8 "entra-directory-audit.json"
```

Microsoft Graph pages large results. Use `-All` deliberately and check the export for completeness, errors and file size.

### Review roles, applications and consent

```powershell
Get-MgRoleManagementDirectoryRoleAssignment -All -ExpandProperty principal,roleDefinition |
  Select-Object Id,PrincipalId,RoleDefinitionId,DirectoryScopeId |
  Export-Csv -NoTypeInformation "entra-role-assignments.csv"

Get-MgServicePrincipal -All -Property Id,AppId,DisplayName,CreatedDateTime,AccountEnabled,ServicePrincipalType,KeyCredentials,PasswordCredentials |
  ConvertTo-Json -Depth 20 |
  Set-Content -Encoding utf8 "entra-service-principals.json"

Get-MgApplication -All -Property Id,AppId,DisplayName,CreatedDateTime,KeyCredentials,PasswordCredentials,RequiredResourceAccess |
  ConvertTo-Json -Depth 20 |
  Set-Content -Encoding utf8 "entra-applications.json"

Get-MgOauth2PermissionGrant -All |
  Export-Csv -NoTypeInformation "entra-oauth2-permission-grants.csv"

Get-MgServicePrincipalAppRoleAssignedTo -ServicePrincipalId "<SERVICE_PRINCIPAL_OBJECT_ID>" -All |
  Export-Csv -NoTypeInformation "entra-app-role-assignments.csv"
```

Pay particular attention to recently created apps, new credentials, tenant-wide admin consent, unusual redirect URIs, unverified publishers and permissions such as `Mail.Read`, `Files.Read.All`, `Directory.ReadWrite.All` or `RoleManagement.ReadWrite.Directory`.

### Preserve Conditional Access and federation

```powershell
Get-MgIdentityConditionalAccessPolicy -All |
  ConvertTo-Json -Depth 30 |
  Set-Content -Encoding utf8 "entra-conditional-access.json"

Get-MgDomainFederationConfiguration -DomainId "example.com" |
  ConvertTo-Json -Depth 20 |
  Set-Content -Encoding utf8 "entra-federation-example.com.json"
```

Also document the authentication methods policy, named locations, cross-tenant access, PIM configuration, break-glass accounts and directory synchronization.

### Unified Audit Log

```powershell
Connect-ExchangeOnline

$Session = "IR-$(Get-Date -Format yyyyMMddHHmmss)"
Search-UnifiedAuditLog -StartDate $Start -EndDate $End `
  -SessionId $Session -SessionCommand ReturnLargeSet -ResultSize 5000 |
  Export-Csv -NoTypeInformation "m365-unified-audit-part-01.csv"

Search-UnifiedAuditLog -StartDate $Start -EndDate $End `
  -UserIds $User -SessionId "$Session-user" -SessionCommand ReturnLargeSet -ResultSize 5000 |
  Export-Csv -NoTypeInformation "m365-unified-audit-user.csv"
```

`ReturnLargeSet` returns unsorted results and is limited per session. Split into time windows, retrieve repeatedly and check counts and duplicates. For large investigations, use the audit search in Purview.

### Review Exchange Online

```powershell
Get-InboxRule -Mailbox $User -IncludeHidden |
  Format-List * | Out-File "exchange-inbox-rules.txt"

Get-Mailbox -Identity $User |
  Select-Object DisplayName,PrimarySmtpAddress,ForwardingAddress,ForwardingSmtpAddress,DeliverToMailboxAndForward |
  Export-Csv -NoTypeInformation "exchange-forwarding.csv"

Get-MailboxPermission -Identity $User |
  Export-Csv -NoTypeInformation "exchange-mailbox-permissions.csv"

Get-RecipientPermission -Identity $User |
  Export-Csv -NoTypeInformation "exchange-recipient-permissions.csv"

Get-MessageTraceV2 -SenderAddress $User -StartDate $Start -EndDate $End |
  Export-Csv -NoTypeInformation "exchange-message-trace.csv"
```

Also review transport rules, connectors, remote domains, journaling, delegations, eDiscovery exports and new mailboxes.

### Azure control plane and resources

```bash
az account tenant list --output json > azure-tenants.json
az account list --all --output json > azure-subscriptions.json

az monitor activity-log list \
  --subscription "<SUBSCRIPTION_ID>" \
  --start-time "<START_UTC>" \
  --end-time "<END_UTC>" \
  --output json > azure-activity-log.json

az role assignment list --all --include-inherited \
  --subscription "<SUBSCRIPTION_ID>" \
  --output json > azure-role-assignments.json

az resource list --subscription "<SUBSCRIPTION_ID>" \
  --output json > azure-resources.json
```

Depending on the diagnostic setting, resource logs are in Log Analytics, Storage or Event Hub. For Key Vault, Storage and databases, check whether data plane logging was enabled at all during the relevant period.

### Containment (only with approval)

#### Revoke a user's sessions

```powershell
Revoke-MgUserSignInSession -UserId $User
```

The effect may be delayed. Then block the account, review authentication methods and, depending on the findings, reset the password and registered methods in a controlled way.

#### Block an account temporarily

```powershell
Update-MgUser -UserId $User -AccountEnabled:$false
```

#### Disable a suspicious service principal

```powershell
Update-MgServicePrincipal -ServicePrincipalId "<OBJECT_ID>" -AccountEnabled:$false
```

Do not delete it before its configuration, credentials, owners, grants, app role assignments and audit trail are preserved.

### Critical search topics

- Role activation and permanent role assignments
- Changes to or exclusions from Conditional Access policies
- New or changed authentication methods
- Domain federation, cross-tenant access and directory synchronization
- App registrations, service principals, consent and new credentials
- Mailbox rules, forwarding, delegations and eDiscovery exports
- Changes to audit, retention, Defender, Sentinel and diagnostic settings
- Key Vault access, secret retrieval, storage downloads and SAS creation
- Deployments, VM extensions, automation accounts and runbooks

### Official references

- [Microsoft Graph: Entra audit logs](https://learn.microsoft.com/en-us/graph/api/resources/azure-ad-auditlog-overview)
- [Microsoft Graph: revoke sign-in sessions](https://learn.microsoft.com/en-us/graph/api/user-revokesigninsessions)
- [Analyze Entra activity logs with Microsoft Graph](https://learn.microsoft.com/en-us/entra/identity/monitoring-health/howto-analyze-activity-logs-with-microsoft-graph)
- [Search-UnifiedAuditLog](https://learn.microsoft.com/en-us/powershell/module/exchange/search-unifiedauditlog)
- [Azure Activity Log](https://learn.microsoft.com/en-us/azure/azure-monitor/platform/activity-log)
- [Microsoft incident response playbooks](https://learn.microsoft.com/en-us/security/operations/incident-response-playbooks)

## Appendix B: AWS Organizations and IAM

AWS queries are mostly account- and region-specific. Global services still write their events to particular regions. Review all relevant accounts and enabled regions. Insert times as UTC in the format `2026-10-01T00:00:00Z`.

### Rules of engagement

- Preferably run the response from a dedicated security or log archive account.
- Consider the management account, delegated administrators and member accounts separately.
- Protect the CloudTrail configuration and central log copies before changing suspicious principals.
- For temporary credentials (`ASIA…`), determine the issuing STS call and the `sessionIssuer`; searching only for the visible access key is not enough.
- Disable access keys first and delete them only after evidence preservation. Role sessions require different measures than long-lived IAM user keys.

### Key log sources

| Source | Coverage and limits |
|---|---|
| CloudTrail Event History | 90 days of management events per account and region; no data events, only one filter attribute, no organization-wide view |
| Organization trail / CloudTrail Lake | Central, longer-term and cross-account search; check coverage, selectors and retention |
| CloudTrail data events | S3, Lambda and other data plane activity; only present if configured |
| GuardDuty | Anomalies from CloudTrail, VPC Flow, DNS and optional protection plans; a finding is a starting point, not complete evidence |
| AWS Config | Resource and configuration history; check recorders, regions and resource types |
| IAM Access Analyzer | External and unused access; no timeline |
| VPC Flow Logs, Route 53 Resolver query logs | Network and DNS metadata; only if enabled beforehand |
| S3 access logs / CloudTrail S3 data events | Object access; configured differently per bucket |
| Security Hub / Detective | Aggregation and correlation of findings; depends on region and organization |

### Context and organization inventory

```bash
aws sts get-caller-identity > aws-caller-identity.json
aws organizations describe-organization > aws-organization.json
aws organizations list-accounts --output json > aws-accounts.json
aws organizations list-delegated-administrators --output json > aws-delegated-admins.json
aws organizations list-policies --filter SERVICE_CONTROL_POLICY --output json > aws-scps.json
```

For each account, document: ID, name, OU, status, owner, break-glass path, central log destinations and enabled regions.

### Review the CloudTrail configuration

```bash
aws cloudtrail describe-trails --include-shadow-trails --region <REGION> > cloudtrail-trails-<REGION>.json
aws cloudtrail get-trail-status --name <TRAIL_ARN_OR_NAME> --region <REGION> > cloudtrail-status.json
aws cloudtrail get-event-selectors --trail-name <TRAIL_ARN_OR_NAME> --region <REGION> > cloudtrail-event-selectors.json
aws cloudtrail list-event-data-stores --region <REGION> > cloudtrail-lake-stores.json
```

Check: multi-region, organization trail, log file validation, KMS, destination bucket, Object Lock, lifecycle, data event selectors and changes to the trail or bucket policy.

### Export Event History

```bash
aws cloudtrail lookup-events \
  --start-time "<START_UTC>" --end-time "<END_UTC>" \
  --region <REGION> \
  --output json > cloudtrail-events-<ACCOUNT>-<REGION>.json

aws cloudtrail lookup-events \
  --lookup-attributes AttributeKey=AccessKeyId,AttributeValue=<ACCESS_KEY_ID> \
  --start-time "<START_UTC>" --end-time "<END_UTC>" \
  --region <REGION> \
  --output json > cloudtrail-access-key.json

aws cloudtrail lookup-events \
  --lookup-attributes AttributeKey=Username,AttributeValue=<IAM_USER_OR_SESSION_NAME> \
  --start-time "<START_UTC>" --end-time "<END_UTC>" \
  --region <REGION> \
  --output json > cloudtrail-principal.json
```

The CLI paginates automatically unless `--no-paginate` is set. Still check file size, time range and errors.

### CloudTrail Lake: example query

```bash
QUERY_ID=$(aws cloudtrail start-query \
  --query-statement "SELECT eventTime,eventSource,eventName,awsRegion,sourceIPAddress,userAgent,userIdentity FROM <EVENT_DATA_STORE_ID> WHERE eventTime >= '<START>' AND eventTime <= '<END>'" \
  --region <REGION> \
  --query 'QueryId' --output text)

aws cloudtrail get-query-results --query-id "$QUERY_ID" --region <REGION> > cloudtrail-lake-results.json
```

Times here in the format `2026-10-01 00:00:00`. Check syntax, available fields and cost before broad queries. Paginate results and keep the original logs unchanged.

### IAM inventory and credentials

```bash
aws iam get-account-summary > iam-account-summary.json
aws iam get-account-authorization-details > iam-authorization-details.json
aws iam generate-credential-report
aws iam get-credential-report --query Content --output text | base64 --decode > iam-credential-report.csv
aws iam list-saml-providers > iam-saml-providers.json
aws iam list-open-id-connect-providers > iam-oidc-providers.json
```

The credential report does not contain service-specific credentials. For every suspicious user, additionally:

```bash
aws iam list-access-keys --user-name <USER> > iam-user-access-keys.json
aws iam list-service-specific-credentials --user-name <USER> > iam-service-credentials.json
aws iam list-mfa-devices --user-name <USER> > iam-user-mfa.json
aws iam list-signing-certificates --user-name <USER> > iam-user-signing-certificates.json
```

### Roles and trust relationships

```bash
aws iam list-roles --output json > iam-roles.json
aws iam get-role --role-name <ROLE_NAME> > iam-role-<ROLE_NAME>.json
aws iam list-attached-role-policies --role-name <ROLE_NAME> > iam-role-attached-policies.json
aws iam list-role-policies --role-name <ROLE_NAME> > iam-role-inline-policies.json
```

Pay particular attention to new trust principals, foreign account IDs, missing `ExternalId`, wildcards, web identity and OIDC providers (such as GitHub), session tags and unusually long session durations.

### Findings and resources

```bash
aws guardduty list-detectors --region <REGION> > guardduty-detectors.json
aws guardduty list-findings --detector-id <DETECTOR_ID> --region <REGION> > guardduty-finding-ids.json
aws guardduty get-findings --detector-id <DETECTOR_ID> --finding-ids <ID1> <ID2> --region <REGION> > guardduty-findings.json

aws configservice describe-configuration-recorder-status --region <REGION> > config-recorder-status.json
aws resourcegroupstaggingapi get-resources --region <REGION> > tagged-resources.json
```

Also review EC2, Lambda, KMS, Secrets Manager, SSM, ECR, ECS/EKS, S3, CloudFormation and billing for newly created or modified resources.

### Critical CloudTrail events

- `ConsoleLogin`, `CreateAccessKey`, `UpdateAccessKey`, `DeleteAccessKey`
- `CreateUser`, `CreateRole`, `UpdateAssumeRolePolicy`
- `Attach*Policy`, `Put*Policy`, `CreatePolicyVersion`, `SetDefaultPolicyVersion`
- `AssumeRole`, `AssumeRoleWithSAML`, `AssumeRoleWithWebIdentity`, `AssumeRoot`
- `DeactivateMFADevice`, `DeleteVirtualMFADevice` and any root activity
- `StopLogging`, `DeleteTrail`, `UpdateTrail`, changes to event selectors
- `PutBucketPolicy`, `PutBucketAcl`, `PutBucketLogging`, `PutBucketVersioning`
- `DisableKey`, `ScheduleKeyDeletion`, changes to key policies and grants
- `GetSecretValue`, `GetParameter`, `SendCommand`, `StartSession`
- `CreateFunction`, `UpdateFunctionCode`, `RunInstances`, `CreateStack`, `ExecuteChangeSet`
- Changes to Organizations, SCPs and delegated administrators

### Containment (only with approval)

#### Disable an IAM user's access key

```bash
aws iam update-access-key \
  --user-name <USER> \
  --access-key-id <ACCESS_KEY_ID> \
  --status Inactive
```

#### Explicit deny as short-term quarantine

A quarantine policy stops a principal faster than uncoordinated removal of many permissions. Test the policy in advance, exclude break-glass and document the way back.

```bash
aws iam put-user-policy \
  --user-name <USER> \
  --policy-name IR-Quarantine-DenyAll \
  --policy-document file://deny-all.json
```

`deny-all.json`:

```json
{
  "Version": "2012-10-17",
  "Statement": [{"Effect": "Deny", "Action": "*", "Resource": "*"}]
}
```

#### Revoke a role's active sessions

STS credentials that have already been issued stay valid until they expire. The console offers "Revoke active sessions" for this (it attaches the policy `AWSRevokeOlderSessions`); with the CLI, the same effect is a deny for all tokens issued before a point in time:

```bash
aws iam put-role-policy \
  --role-name <ROLE_NAME> \
  --policy-name AWSRevokeOlderSessions \
  --policy-document file://revoke-older-sessions.json
```

`revoke-older-sessions.json` (point in time = now, UTC):

```json
{
  "Version": "2012-10-17",
  "Statement": [{
    "Effect": "Deny",
    "Action": "*",
    "Resource": "*",
    "Condition": {"DateLessThan": {"aws:TokenIssueTime": "<NOW_UTC>"}}
  }]
}
```

Then address the cause: assess the trust policy, permission policies, session policies, SCPs and resource policies together, otherwise the attacker simply obtains new sessions.

### Root and account recovery

- Review the root email, MFA, phone number and alternate contacts.
- Investigate unexpected root activity through CloudTrail and GuardDuty.
- Engage AWS Support through the agreed emergency or Enterprise Support path.
- In AWS Organizations, check whether root credentials are managed centrally or short-term root access was used; correlate `AssumeRoot` with the follow-on activity.

### Official references

- [CloudTrail Event History and its limits](https://docs.aws.amazon.com/awscloudtrail/latest/userguide/view-cloudtrail-events.html)
- [IAM credential report](https://docs.aws.amazon.com/IAM/latest/UserGuide/id_credentials_getting-report.html)
- [Revoke a role's temporary credentials](https://docs.aws.amazon.com/IAM/latest/UserGuide/id_roles_use_revoke-sessions.html)
- [Remediating compromised AWS credentials](https://docs.aws.amazon.com/guardduty/latest/ug/compromised-creds.html)
- [Track privileged root sessions in CloudTrail](https://docs.aws.amazon.com/IAM/latest/UserGuide/cloudtrail-track-privileged-tasks.html)
- [AWS Security Incident Response Guide](https://docs.aws.amazon.com/security-ir/latest/userguide/what-is-security-ir.html)

## Appendix C: Google Workspace and Google Cloud

Google Workspace and Google Cloud share identities but have different audit and administration surfaces. Investigate both. Insert times as UTC in the format `2026-10-01T00:00:00Z`.

### Rules of engagement

- Secure Super Admin accounts, privileged groups, delegated admin roles and break-glass accounts first.
- Always change the password and reset the sign-in cookies together.
- Export Workspace logs through "Audit and investigation" or the security investigation tool; API availability and retention depend on the edition and data source.
- In Google Cloud, Admin Activity and System Event logs are always written; Data Access logs mostly only if they were enabled beforehand (exceptions include BigQuery).
- Investigate service account keys, Workload Identity Federation and domain-wide delegation separately from user accounts.

### Log sources in Google Workspace

- Admin and user log events, including sign-in and account activity
- OAuth and token log events
- Drive log events and external sharing
- Gmail log events and Email Log Search
- SAML, Context-Aware Access and access evaluation
- Groups, devices, Chrome and endpoint activity
- Rules, DLP, Vault, Takeout and data migration
- Alert center and security investigation tool

Available data sources and response options vary by Workspace edition and admin rights.

### Workspace Reports API

The Reports API returns activity per application. The token is read without echo so that it does not end up in the shell history:

```bash
read -rs GOOGLE_IR_TOKEN && export GOOGLE_IR_TOKEN
START_TIME='<START_UTC>'
END_TIME='<END_UTC>'

for APP in admin login token drive; do
  curl -sS \
    -H "Authorization: Bearer ${GOOGLE_IR_TOKEN}" \
    "https://admin.googleapis.com/admin/reports/v1/activity/users/all/applications/${APP}?startTime=${START_TIME}&endTime=${END_TIME}&maxResults=1000" \
    > "workspace-${APP}-activity-page-001.json"
done
```

Paginate responses with `nextPageToken` completely. Do not store tokens in tickets or artifact files. For repeated retrieval, use a controlled OAuth client with as few scopes as possible.

### Investigate Workspace in the admin console

Path: **Reporting → Audit and investigation** or **Security → Security center → Investigation tool**.

- New Super Admins and delegated admin roles
- Changes to password, recovery data, 2-Step Verification and passkeys
- Suspicious session cookies and unusual sign-ins
- New OAuth clients, grants and highly privileged scopes
- Domain-wide delegation and Marketplace applications
- Changes to SAML/SSO and Context-Aware Access
- Gmail forwarding, delegations, routing and compliance rules
- Drive mass downloads, external sharing, ownership transfers and deletions
- Google Takeout, Vault exports and data migrations
- Memberships in privileged or confidential groups

### Containment in Workspace (only with approval)

1. Suspend the account where appropriate.
2. Reset the password.
3. Reset the sign-in cookies separately.
4. Review 2-Step Verification and passkey registrations, remove unknown methods.
5. Revoke app passwords and OAuth tokens.
6. Review forwarding, delegations, filters, groups and Drive sharing.
7. With SSO, also handle the session at the identity provider and the source account.

A password reset alone does not reliably end every existing access.

### Log sources in Google Cloud

| Log | Purpose |
|---|---|
| Admin Activity | Changes to configuration and metadata; always written |
| Data Access | Reading and writing data; mostly needs to be enabled explicitly |
| System Event | Changes triggered by Google systems |
| Policy Denied | Access denied by security policies |
| VPC Flow Logs, Cloud DNS logs, firewall rules logging | Network and DNS metadata; only if enabled |
| Security Command Center | Findings and correlation |
| Access Transparency | Access by Google staff; depends on edition |

### Export cloud context and IAM

```bash
gcloud auth list --format=json > gcp-auth-context.json
gcloud organizations list --format=json > gcp-organizations.json
gcloud projects list --format=json > gcp-projects.json

gcloud organizations get-iam-policy <ORG_ID> --format=json > gcp-org-iam-policy.json
gcloud resource-manager folders get-iam-policy <FOLDER_ID> --format=json > gcp-folder-iam-policy.json
gcloud projects get-iam-policy <PROJECT_ID> --format=json > gcp-project-iam-policy.json

gcloud iam service-accounts list --project <PROJECT_ID> --format=json > gcp-service-accounts.json
gcloud iam service-accounts keys list --iam-account <SERVICE_ACCOUNT_EMAIL> --managed-by=user --format=json > gcp-user-managed-sa-keys.json
```

The project IAM policy does not show inherited bindings. Capture the organization, folders, projects and relevant resources together; also review IAM deny, principal access boundaries and organization policies.

### Export Cloud Audit Logs

```bash
gcloud logging read '
timestamp>="<START_UTC>" AND timestamp<="<END_UTC>" AND
protoPayload.@type="type.googleapis.com/google.cloud.audit.AuditLog"
' --project <PROJECT_ID> --order=asc --format=json > gcp-audit-all.json

gcloud logging read '
logName:"cloudaudit.googleapis.com%2Factivity" AND
timestamp>="<START_UTC>" AND timestamp<="<END_UTC>"
' --organization <ORG_ID> --order=asc --format=json > gcp-admin-activity-org.json

gcloud logging read '
protoPayload.authenticationInfo.principalEmail="user@example.com" AND
timestamp>="<START_UTC>" AND timestamp<="<END_UTC>"
' --project <PROJECT_ID> --order=asc --format=json > gcp-principal-activity.json
```

Data Access logs require `roles/logging.privateLogViewer` or an equivalent permission. Check whether logs are routed to other buckets, projects, BigQuery or a SIEM.

### Preserve the logging configuration

```bash
gcloud logging sinks list --organization <ORG_ID> --format=json > gcp-org-log-sinks.json
gcloud logging sinks list --project <PROJECT_ID> --format=json > gcp-project-log-sinks.json
gcloud logging buckets list --location=global --project <PROJECT_ID> --format=json > gcp-log-buckets.json
gcloud logging exclusions list --project <PROJECT_ID> --format=json > gcp-log-exclusions.json
```

Changes to sinks, exclusions, retention, CMEK and writer identities are particularly relevant.

### Workload identities and persistence

- Newly created service accounts and user-managed keys
- `roles/iam.serviceAccountTokenCreator`, `roles/iam.serviceAccountUser` and key admin rights
- Workload identity pools, providers, attribute mappings and trust to OIDC or AWS
- Domain-wide delegation for service accounts
- Cloud Functions, Cloud Run, GKE and build pipelines with privileged service accounts
- Access to Secret Manager, KMS, Storage, BigQuery and Logging
- Organization policies, VPC Service Controls and Access Context Manager

### Containment in Google Cloud (only with approval)

#### Disable a service account key

```bash
gcloud iam service-accounts keys disable <KEY_ID> \
  --iam-account <SERVICE_ACCOUNT_EMAIL> \
  --project <PROJECT_ID>
```

Disable first, watch dependencies, provide a replacement and only then delete in a controlled way. With Workload Identity or short-lived tokens, address the issuing trust relationship.

#### Change IAM bindings

Before any change, preserve the current policy including its ETag. Broad `set-iam-policy` operations can overwrite parallel changes or remove your own recovery access. Make changes under the four-eyes principle.

### Official references

- [Google Workspace investigation tool: data sources](https://support.google.com/a/answer/11482175)
- [Admin SDK Reports API: activities.list](https://developers.google.com/workspace/admin/reports/reference/rest/v1/activities/list)
- [Investigate suspicious session cookies](https://support.google.com/a/answer/14211409)
- [Reset a password and sign-in cookies](https://support.google.com/a/answer/33319)
- [Google Cloud Audit Logs](https://cloud.google.com/logging/docs/audit)
- [Configure Data Access audit logs](https://cloud.google.com/logging/docs/audit/configure-data-access)

## Appendix D: Okta

Okta Classic Engine and Identity Engine differ in some flows. Check API calls against your own engine and the current Okta documentation. Insert times as UTC in the format `2026-10-01T00:00:00Z`.

### Rules of engagement

- If Okta is the central identity provider, do not make the response channel and admin sign-in depend solely on the possibly compromised Okta organization.
- Export the System Log and the admin, app and policy configuration read-only before making changes.
- Prefer scoped OAuth 2.0 management tokens over long-lived SSWS tokens.
- User sessions, OAuth tokens, remembered devices and sessions in downstream applications are separate layers.
- Suspending an Okta user ends Okta sessions, but not necessarily sessions that a downstream application manages itself.

### Key sources

- System Log and System Log API
- Admin roles and role assignments
- Changes to users, groups and group rules
- Applications, app assignments and sign-on policies
- Authorization servers, OAuth clients, grants and scopes
- Identity providers, routing rules and federation
- Authenticators, enrollment and factor resets
- Network zones, behavior detection and global session policy
- API tokens, OAuth service apps and device assurance
- Event hooks, inline hooks and workflows

### Prepare variables

```bash
OKTA_DOMAIN='example.okta.com'
read -rs OKTA_IR_TOKEN && export OKTA_IR_TOKEN
SINCE='<START_UTC>'
UNTIL='<END_UTC>'
```

`read -rs` reads the token without echo and without an entry in the shell history. Header for SSWS tokens: `Authorization: SSWS <token>`; for OAuth: `Authorization: Bearer <token>`.

### Export the System Log

```bash
curl -sS --get \
  -H "Authorization: Bearer ${OKTA_IR_TOKEN}" \
  -H 'Accept: application/json' \
  --data-urlencode "since=${SINCE}" \
  --data-urlencode "until=${UNTIL}" \
  --data-urlencode 'limit=1000' \
  "https://${OKTA_DOMAIN}/api/v1/logs" \
  -D okta-system-log-headers.txt \
  -o okta-system-log-page-001.json
```

Retrieve further pages through the `Link` header with `rel="next"`. Correlate `published`, `uuid`, `eventType`, actor, client, target, outcome, debug context and transaction.

### Targeted System Log searches

```bash
# Events of one user
curl -sS --get -H "Authorization: Bearer ${OKTA_IR_TOKEN}" -H 'Accept: application/json' \
  --data-urlencode "since=${SINCE}" --data-urlencode "until=${UNTIL}" \
  --data-urlencode 'filter=actor.alternateId eq "user@example.com"' \
  "https://${OKTA_DOMAIN}/api/v1/logs" > okta-user-events.json

# Access to the admin console
curl -sS --get -H "Authorization: Bearer ${OKTA_IR_TOKEN}" -H 'Accept: application/json' \
  --data-urlencode "since=${SINCE}" --data-urlencode "until=${UNTIL}" \
  --data-urlencode 'filter=eventType eq "user.session.access_admin_app"' \
  "https://${OKTA_DOMAIN}/api/v1/logs" > okta-admin-console-access.json

# Free-text search for an IP, domain or object ID
curl -sS --get -H "Authorization: Bearer ${OKTA_IR_TOKEN}" -H 'Accept: application/json' \
  --data-urlencode "since=${SINCE}" --data-urlencode "until=${UNTIL}" \
  --data-urlencode 'q=<IP_OR_DOMAIN_OR_OBJECT_ID>' \
  "https://${OKTA_DOMAIN}/api/v1/logs" > okta-search.json
```

### Inventory users, groups, applications and grants

```bash
for EP in users groups apps idps; do
  curl -sS -H "Authorization: Bearer ${OKTA_IR_TOKEN}" -H 'Accept: application/json' \
    "https://${OKTA_DOMAIN}/api/v1/${EP}?limit=200" > "okta-${EP}-page-001.json"
done

curl -sS -H "Authorization: Bearer ${OKTA_IR_TOKEN}" -H 'Accept: application/json' \
  "https://${OKTA_DOMAIN}/api/v1/users/<USER_ID>/grants" > okta-user-grants.json
```

Paginate all endpoints completely. Client secrets are not necessarily returned in clear text; still preserve the metadata and last changes.

### Critical System Log topics

- Admin sign-ins, new admin roles and privilege escalation
- User activation, suspension, password and factor resets
- New or changed authenticators and enrollment policies
- New applications, changed app credentials and app assignments
- Changes to identity providers, routing rules, SAML and OIDC
- New API tokens, OAuth service apps and unusual use of the management API
- Network zones, trusted origins, global session policy and MFA bypass
- Group rules and memberships with downstream privileges
- System Log streaming, event hooks, inline hooks and workflows

### Containment (only with approval)

#### Revoke all of a user's Okta sessions and OAuth tokens

```bash
curl -sS -X DELETE -H "Authorization: Bearer ${OKTA_IR_TOKEN}" -H 'Accept: application/json' \
  "https://${OKTA_DOMAIN}/api/v1/users/<USER_ID>/sessions?oauthTokens=true&forgetDevices=true"
```

Sessions that web or native apps manage themselves are not affected. Handle downstream applications separately.

#### Revoke all of a user's grants

```bash
curl -sS -X DELETE -H "Authorization: Bearer ${OKTA_IR_TOKEN}" -H 'Accept: application/json' \
  "https://${OKTA_DOMAIN}/api/v1/users/<USER_ID>/grants"
```

#### Suspend a user

```bash
curl -sS -X POST -H "Authorization: Bearer ${OKTA_IR_TOKEN}" -H 'Accept: application/json' \
  "https://${OKTA_DOMAIN}/api/v1/users/<USER_ID>/lifecycle/suspend"
```

Before that, preserve the profile, groups, roles, factors, sessions, grants and relevant System Log events. Do not suspend break-glass and service accounts without a verified recovery path.

### Checks before returning to service

- At least two independent Super Admin accounts with phishing-resistant MFA
- Admin access only from controlled network zones and devices
- No unknown identity providers, routing rules, trusted origins or hooks
- Admin roles and privileged groups with expected members only
- API tokens and OAuth service apps inventoried and rotated
- Sign-on policies, global session policy and authenticators reviewed
- Sessions in downstream SaaS applications and provisioning connectors reviewed separately
- System Log fully exported to an external SIEM or immutable archive

### Official references

- [Okta System Log query](https://developer.okta.com/docs/reference/system-log-query/)
- [Okta Core API](https://developer.okta.com/docs/reference/core-okta-api/)
- [Revoke OAuth tokens](https://developer.okta.com/docs/guides/revoke-tokens/)
- [Okta Management API: permissions](https://developer.okta.com/docs/api/openapi/okta-management/guides/permissions)

## Disclaimer

This playbook is a practical community resource. It does not replace legal advice, forensic expertise, cyber insurance requirements, support from the cloud provider or organization-specific incident response planning. The commands in the appendices are examples; syntax and availability change with the platforms and must be checked before use.
