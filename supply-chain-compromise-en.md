# Playbook: Supply Chain Compromise

## Summary

This playbook structures the response when the attack does not start with you, but with someone or something you trust: a manipulated software update, a compromised IT service provider, a malicious open-source dependency, a compromised build pipeline, or a partner with a data or secret leak.

What makes it special: the attacker arrives through a **legitimate and trusted path**. Signed updates, approved remote maintenance, official package sources and existing trust relationships often make malicious activity look legitimate at first.

The core of this playbook: **First establish where the affected component sits in your environment, which rights it has and which trust relationship it holds — then withdraw that trust in a controlled way until it is clear whether misuse occurred.**

## Purpose

- Quickly establish whether and where a reported supply chain compromise affects you
- Restrict the trust relationship of the affected component, service provider or source in a controlled way
- Detect misuse in your own environment
- Renew secrets and access that the component or service provider knew
- Work with the vendor, partner or service provider in a structured way
- Decide whether and how the component continues to be used, is replaced or removed

## Scope

Use this playbook for at least one of the following scenarios:

- **Manipulated software update:** A vendor reports that installation packages or updates contained malicious code.
- **Compromised service provider:** An MSP, systems integrator or software provider with remote access to your environment is itself compromised.
- **Malicious package:** A dependency in applications, build pipelines, package repositories or container images contains malicious code.
- **Compromised build or development environment:** CI/CD, code repositories or signing keys at your organization or at a supplier are affected.
- **Data leak at a partner:** A SaaS provider or partner reports an incident that may affect your data, credentials or API keys.
- **Your own detection first:** EDR, monitoring or threat hunting shows suspicious behavior of a trusted component before the vendor has issued a warning.

**Boundaries:**

- If encryption is under way or the compromise is broad, the ransomware playbook additionally takes the lead.
- For pure data leaks at a partner, the data breach playbook runs in addition.
- Mere vulnerabilities without misuse or breach of trust are vulnerability management, not this playbook.

## Severity

Default severity: **High**.

Raise to **Critical** if:

- the affected component is or was installed in your environment in the compromised version
- the component or service provider has administrative rights
- execution of malicious code or connections to known attacker infrastructure are visible
- signing keys, build systems or artifact repositories are affected
- you yourself pass software, managed services or artifacts on to customers and may have passed the compromise on

## Incident Objectives

1. Establish exposure: where is the component, which version is running, which rights does it have?
2. Restrict the trust relationship until the situation is clear.
3. Detect misuse in your own environment.
4. Renew secrets and access that the component or service provider knew.
5. Decide on clean continued use, replacement or removal.
6. Inform your own customers if you are yourself part of the supply chain.

## Core Response Principles

- **Inventory before action.** Without knowing where a component runs, every measure is incomplete.
- **Withdraw trust temporarily.** Stop automatic updates, lock remote maintenance access, disable keys and restrict connections.
- **Verify vendor statements, do not just adopt them.** First reports are often incomplete and are extended later.
- **What the component could do, the attacker could do.** Rights, network reach and stored secrets determine the possible damage.
- **Consider your own role.** If you deliver artifacts, software or services yourself, your customers are potentially the next to be affected.
- **Record time consistently.** Document vendor updates, IOCs, internal findings and decisions in UTC.

## Contacts and Escalation

Pre-fill these for your organization and keep them available offline:

- Incident lead on call
- Procurement / vendor management / service provider management
- IT operations / software distribution
- DevOps / platform / build and release owners
- Vendor PSIRT or service provider emergency contact
- Legal, data protection and communications
- External forensics / incident response

## First 15 Minutes

- Name an incident lead, start the timeline and decision log.
- Secure the source and read it carefully: vendor advisory, CERT warning, press report or own finding.
- Note affected products, versions, time periods, IOCs and recommended immediate measures.
- Start a first inventory query: where is the component installed or integrated?
- Stop automatic updates or automatic distribution of the affected component.
- If a service provider is compromised, temporarily lock its remote maintenance access and call the provider through a known contact path.
- Search EDR, DNS, proxy, firewall and cloud logs with published or self-determined indicators.
- Inform management if affected versions or privileged trust relationships were found.
- Set a time for reassessment, because vendor statements often change.

## Roles and Responsibilities

### Incident lead

- Steers the incident.
- Prioritizes lockout, restricted continued operation, replacement and customer communication.
- Keeps the timeline, situation picture and decision log.

### IT operations and software distribution

- Provide the inventory and distribution view.
- Stop updates, isolate systems and implement replacement or removal.

### Development / DevOps

- Check dependencies, build pipelines, lockfiles, artifacts, signatures and release paths.
- Assess whether your own artifacts or releases are affected.

### Security / SOC

- Search for indicators and misuse.
- Assess follow-on activity and the reach of the compromise.

### Procurement / vendor management

- Know the contracts, reporting obligations, contacts and access of service providers.
- Coordinate escalation with suppliers.

### Legal, data protection and communications

- Assess reporting obligations.
- Steer customer, partner and external communication.
- Review liability and contract issues.

## Decision Model

### Rule 1: Are we affected?

**Affected** if the component was installed or used in an affected version during the affected period, or the affected service provider had access to your environment during the relevant period.

**Possibly affected** if the inventory has gaps or vendor statements are still preliminary. In that case, treat it as affected until clarified.

**Not affected** if the component is demonstrably not present, or was only present outside the affected version range and period. Document the justification and keep following vendor updates.

### Rule 2: Lock out, isolate or keep running with restrictions?

**Lock out or isolate immediately** if:

- malicious code was executed in your environment
- reliable indicators were found
- the component or service provider has administrative rights and the situation is unclear
- the vendor or PSIRT explicitly advises shutting it down

**Continued restricted operation** is only justifiable if:

- there are no indications of execution or misuse
- automatic updates are stopped
- network connections are restricted
- the business risk of stopping would be higher
- the decision is documented and approved by the incident lead and accountable management

### Rule 3: Which secrets need to be renewed?

All secrets that the component or service provider knew or could reach:

- The software's service accounts and their rights
- API keys, tokens, certificates and configuration secrets
- For build systems: CI/CD secrets, signing keys, deploy keys, cloud credentials, registry tokens
- For packages: environment variables, local secrets and access data on the systems where the package ran
- For service providers: all known or reachable accounts, VPN access, RMM access and delegations

### Rule 4: Continue using, replace or remove?

Continue using only if:

1. a clean version is available
2. the source has been verified, for example through hash, signature and origin
3. the component's rights are reduced to the necessary minimum
4. monitoring of behavior and follow-on activity is in place

Otherwise plan a replacement, or run the component isolated or remove it until then.

### Rule 5: When to activate other playbooks?

- **Malware or ransomware playbook in parallel or in the lead** if malicious code execution is visible on affected systems
- **Data breach playbook in parallel** if data at the partner or via compromised software may be affected
- **Identity compromise in parallel** if tokens, SSO, cloud access or privileged service accounts are affected

## Scenario Notes

### Manipulated software update

- Determine when the affected version was installed on each system
- Check network connections and process activity since installation
- Treat systems with confirmed malicious activity as a malware or ransomware case

### Compromised service provider / RMM

- List all of the provider's access: accounts, VPN, RMM agents, jump hosts, cloud delegations
- Check RMM logs for executed scripts, commands and mass actions during the affected period
- Reset access and only allow it again through a new, monitored path

### Malicious package / dependency

- Check lockfiles, build logs and artifact sources for the affected version, including transitive dependencies
- Determine where the package ran: developer machines, CI, build servers, production
- Rotate the secrets of these systems
- Clean artifact caches, package proxies and container registries
- Pin to a clean version

### Compromised internal build pipeline

- Identify builds and releases from the affected period
- Inventory published artifacts, container images, packages and installers
- Rotate signing keys, CI/CD runner secrets, cloud deploy credentials and registry tokens
- Perform reproducible builds or independent verification where possible
- Withdraw, block or mark affected versions as unsafe
- Rebuild and re-sign from a trusted source state in a cleaned pipeline
- Inform downstream customers with version ranges, hashes, IOCs and measures
- Analyze repository, branch protection, pull request and CI/CD audit logs

### Your own detection first

- Corroborate the finding before communicating it widely
- Inform the vendor or PSIRT confidentially through a known security contact
- Involve a CERT or the competent authority if broad impact is likely
- Do not share indicators publicly too early

### Data leak at a partner

- Determine affected data, keys, tokens and integrations
- Rotate keys and tokens
- Check integrations for misuse
- Start a data protection assessment

## SBOM, VEX and Provenance

- **SBOM** answers the inventory question: is the affected component included?
- **VEX** answers the exposure question: is the reported vulnerability or compromise actually relevant or exploitable in your product?
- **Provenance / build attestation** answers the trust question: where does the artifact come from, and under which conditions was it built and signed?

Together, these three kinds of artifacts improve the decision between "affected", "possibly affected" and "not affected".

A missing match is only as reliable as the inventory behind it. If the SBOM, asset inventory or build attestations have gaps, the classification "possibly affected" remains until the gap is closed or the risk is limited in another way.

## Negative Findings and Reassessment

"Not affected" is a snapshot, not a final all-clear. Document which vendor information, version level, time period and search criteria the assessment is based on. At the same time, set a date or trigger for reassessment, such as an updated vendor advisory, new IOCs or new findings about the compromised build chain.

## Phase 1: Detect and Triage

### Objectives

- Validate the information
- Determine exposure and reach
- Record the trust relationship, rights and secrets

### Actions

- Validate the source: official vendor source, CERT, CISA, BSI, NCSC/BACS or a reliable finding of your own
- Collect the inventory completely; name gaps explicitly
- Document the rights and network reach of the component or the service provider's access
- Search with IOCs and behavior patterns
- Match time periods and version levels carefully

### Exit criteria

- Exposure established: affected, possibly affected or not affected
- Rights, trust relationship and potential reach known
- First situation picture documented

## Phase 2: Contain

### Objectives

- Stop further distribution, use or remote access
- Restrict the trust relationship in a controlled way

### Actions

- Pause distribution of the **affected component or source**, not all security updates across the board
- Block or isolate the sources
- Lock or restrict the service provider's access
- Limit the component's network connections to the minimum
- Isolate affected systems when there are findings

### Do not

- Do not contact the service provider through possibly compromised channels
- Do not trust the vendor's first version list
- Do not switch everything off across the board when only one component is affected

### Exit criteria

- No more automatic distribution from the affected source
- Access of the component or service provider restricted or locked
- Systems with reliable findings isolated

## Phase 3: Investigate

### Objectives

- Detect misuse in your own environment or rule it out on solid grounds
- Determine affected systems, accounts and data

### Actions

- Reconstruct the activity of the component or the service provider's access during the period
- Watch for follow-on activity: new accounts, remote execution, privilege escalation, data exfiltration
- Exchange findings with the vendor or service provider
- Check whether your own customers or downstream systems could be affected

### Exit criteria

- Misuse established or ruled out with justified confidence
- Affected systems, accounts, data and artifacts named
- Handover to other playbooks completed where necessary

## Phase 4: Eradicate

### Objectives

- Clean up affected components and trust relationships
- Create a secure basis for continued operation or replacement

### Actions

- Remove, replace or reinstall affected versions
- Renew secrets according to rule 3
- Clean or rebuild affected systems
- Set up service provider access anew, minimal and monitored
- If your own artifacts are affected, rebuild, re-sign and redistribute them

### Exit criteria

- Affected versions removed, replaced or cleanly redeployed
- Secrets renewed
- Critical trust relationships rebuilt and monitored

## Phase 5: Recover

### Objectives

- Resume clean use in a controlled way
- Monitor follow-on activity closely

### Actions

- Deploy a verified clean version
- Increase monitoring of the component's behavior for at least 30 days
- Re-enable automatic updates only once the source and verification are settled
- Provide your own customers or business units with clear instructions

### Exit criteria

- Clean version in use or replacement implemented
- Monitoring without findings
- Residual risks documented

## Phase 6: Review

### Actions

- Check how long the inventory question took and where gaps existed
- Question the component's rights
- Evaluate contracts, emergency contacts, reporting obligations and audit rights
- Improve the SBOM, provenance and software inventory
- Include supply chain risks systematically in risk management

## Evidence to Preserve

- Vendor advisories and their versions with dates
- Inventory queries with timestamps
- Installation, distribution and package manager logs
- RMM and service provider access logs
- EDR, DNS, proxy, firewall and cloud data on IOCs
- Communication with the vendor or service provider
- Timeline and decision log

## Communication Checklist

- Contact the vendor or service provider only through a known contact path
- Inform internal business units about restrictions and required action
- If you deliver yourself: inform customers with affected versions, IOCs and recommended measures
- No public blame while the facts are unclear

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

- NIS2 explicitly counts supply chain security among the risk management obligations. Significant incidents caused through a supplier are reportable like your own incidents.
- **Processors:** Under Art. 33(2) GDPR, the service provider must inform you without undue delay. Your own 72-hour deadline starts when you become aware.
- **Cyber Resilience Act (since 11 September 2026):** If you are yourself a manufacturer of products with digital elements, report actively exploited vulnerabilities and severe security incidents through ENISA's single reporting platform: early warning within 24 hours, notification within 72 hours.
- Review contracts with suppliers, service providers and your own customers for reporting obligations, liability and audit rights.

## Quick Response Checklist

- [ ] Information validated, versions and periods noted
- [ ] Inventory collected, gaps named
- [ ] Affected distribution or source paused
- [ ] Service provider access locked or restricted
- [ ] Indicators searched
- [ ] Rights and trust relationship of the component documented
- [ ] Secrets renewed
- [ ] Decision on continued use, replacement or removal documented
- [ ] Own customers informed, if relevant
- [ ] Vendor updates followed up

## Preventive Controls

- Complete software and service provider inventory with rights and contacts
- SBOM for your own applications
- VEX and provenance capability for critical supply chains
- Service provider access only on demand, with MFA, logging and time limits
- Minimal rights for software and build service accounts
- Verification of updates through signatures, hashes and staged rollout
- Protection of your own build systems and signing keys
- Contracts with reporting obligations, emergency contacts and audit rights

## At a Glance

This block is not a shortened replacement playbook. It is a quick way in; all sections of this document apply when handling the incident.

**Supply Chain Compromise**  
Response to manipulated updates, compromised service providers, malicious packages and incidents at partners — establish exposure through the inventory, withdraw trust relationships, renew secrets and decide on continued use or replacement.

**Core phases:** Detect, Contain, Investigate, Eradicate, Recover, Review.

## References

- NIST SP 800-161 Rev. 1
- NIST SP 800-61 Rev. 3
- CISA: Defending Against Software Supply Chain Attacks
- ENISA: Threat Landscape for Supply Chain Attacks
- MITRE ATT&CK: T1195, T1199

## Disclaimer

This playbook is a practical community resource. It does not replace legal advice, vendor guidance or organization-specific incident response planning.
