# Playbook zur Identitätskompromittierung

## Zusammenfassung

Dieses Playbook beschreibt ein praxisnahes, technisch orientiertes Incident-Response-Verfahren für vermutete oder bestätigte Identitätskompromittierung bei Benutzerkonten, Dienstkonten, privilegierten Konten, Cloud-Identitäten, Verzeichnisdiensten und Identitätsinfrastruktur.
Es soll Incident Respondern helfen, laufenden Missbrauch zu stoppen, Beweise zu sichern, den initialen Zugriff zu ermitteln, Persistenz und laterale Bewegung zu erkennen, den vertrauenswürdigen Identitätszustand wiederherzustellen und rechtliche sowie regulatorische Entscheidungen zu unterstützen, wenn personenbezogene Daten oder regulierte Zugriffe betroffen sind.

Das Playbook folgt einem standardisierten Incident-Response-Ablauf: Erkennen & Triage, Eindämmung, Untersuchung, Beseitigung, Wiederherstellung und Review.
Es richtet sich an Security-Teams, IT-Administratoren, SOC-Analysten, Identity Engineers, Berater sowie kleine und mittelständische Organisationen.

> Hinweis: Dies ist keine Rechtsberatung und kein Ersatz für forensische Expertise, Datenschutzberatung, regulatorische Bewertung oder organisationsspezifische Incident-Response-Planung.

## Zweck

- Einen strukturierten Reaktionsprozess für vermutete und bestätigte Identitätskompromittierung bereitstellen.
- Schnelle Validierung, Eindämmung und Beweissicherung ermöglichen.
- Bestimmen, ob Anmeldedaten, Sitzungen, Tokens, MFA, OAuth-Berechtigungen, delegierte Zugriffe oder privilegierte Zugriffe missbraucht wurden.
- Initialen Zugriff, Persistenz, Privilegieneskalation und laterale Bewegung identifizieren.
- Einen sicheren und vertrauenswürdigen Identitätszustand wiederherstellen.
- Eine DSGVO-orientierte Bewertung unterstützen, wenn personenbezogene Daten oder regulierte Zugriffe betroffen sein könnten.
- Dokumentierte Lessons Learned und nachverfolgte Remediation erzeugen.

## Anwendungsbereich / Wann verwenden

Verwenden Sie dieses Playbook, wenn einer oder mehrere der folgenden Punkte vorliegen:

- Ein Benutzerkonto steht im Verdacht, kompromittiert worden zu sein oder ist bestätigt kompromittiert.
- Ein privilegiertes Konto zeigt verdächtige Aktivitäten.
- Ein Dienstkonto oder eine Anwendungsidentität verhält sich unerwartet.
- Verdächtige erfolgreiche Anmeldungen werden beobachtet.
- MFA-Aufforderungen, Genehmigungen, Methodenänderungen oder Neu-Registrierungen wirken verdächtig.
- Ein Benutzer meldet unerwartete MFA-Anfragen oder Kontobewegungen.
- Ein Konto meldet sich von ungewöhnlichen Orten, Geräten, IP-Adressen oder User-Agents an.
- Verdächtige OAuth-Berechtigungen, App-Zustimmungen oder delegierte Berechtigungen werden entdeckt.
- API-Schlüssel, Zugriffstokens, Refresh-Tokens, SSH-Schlüssel, Zertifikate oder Client-Secrets könnten offengelegt sein.
- Laterale Bewegung wird nach Identitätsdiebstahl vermutet.
- Credential-Dumping-Tools, Pass-the-Hash, Pass-the-Ticket oder Kerberoasting-Aktivität werden vermutet.
- Neue Konten, Gruppenmitgliedschaften, Rollenzuweisungen oder Admin-Rechte erscheinen unerwartet.
- Änderungen an Verzeichnissynchronisation, Föderation, Identity Provider oder Conditional Access könnten erfolgt sein.
- Cloud-Tenant-, Active-Directory-, Entra-ID-, Okta-, Google-Workspace-, LDAP-, IAM-, SSO- oder PAM-Systeme zeigen verdächtige Aktivitäten.

Dieses Playbook deckt folgende Szenarien ab:

- Benutzerkontokompromittierung.
- Kompromittierung privilegierter Konten.
- Kompromittierung von Dienstkonten.
- Cloud-Identitätskompromittierung.
- OAuth-Consent-Missbrauch.
- Token-Diebstahl.
- Offenlegung von API-Schlüsseln oder Secrets.
- MFA-Fatigue / MFA-Push-Bombing.
- Hijacking von MFA-Methoden.
- Password Spraying.
- Credential Stuffing.
- Credential Dumping.
- Pass-the-Hash.
- Pass-the-Ticket.
- Verdacht auf Golden Ticket / Silver Ticket.
- Unautorisierte Privilegieneskalation.
- Unautorisierte Rollen- oder Gruppenzuweisung.
- Verdächtige Änderungen an Verzeichnissynchronisation oder Föderation.
- Laterale Bewegung mit gültigen Konten.
- Identitätskompromittierung im Zusammenhang mit Phishing, Malware, Ransomware oder Insider-Aktivität.

---

## Schweregrad

Standard-Schweregrad: **Hoch**.

Auf **Kritisch** eskalieren, wenn einer der folgenden Punkte zutrifft:

- Privilegierte, Admin-, Finanz-, HR-, Executive- oder Identity-Provider-Konten sind betroffen.
- Dienstkonten, Break-Glass-Konten, Föderationsdienste, MFA-Systeme, Verzeichnissynchronisation, Secrets, Tokens, API-Schlüssel oder die Identitätsinfrastruktur sind betroffen.
- Sitzungstokens, Refresh-Tokens, API-Schlüssel oder Dienstanmeldedaten wurden offengelegt.
- Angreifer können Persistenz über MFA, OAuth, Wiederherstellungsmethoden oder delegierten Zugriff aufrechterhalten.
- Mehrere Konten, Tenants, Cloud-Identitäten oder Systeme sind betroffen.
- Datenexposition, Finanzbetrug oder Missbrauch von Geschäftsprozessen ist möglich.
- Es gibt Hinweise auf laterale Bewegung oder einen domänenweiten bzw. tenantweiten Kompromittierungsfall.
- Regulatorische, vertragliche oder rechtliche Meldefristen könnten gelten.

## Incident-Ziele

Während der ersten Stunden eines Vorfalls zur Identitätskompromittierung sollte das Incident-Response-Team auf folgende Ziele hinarbeiten:

1. Laufenden Missbrauch von Identitäten stoppen.
2. Beweise sichern und eine belastbare Timeline führen.
3. Den initialen Zugriff und die betroffenen Identitäten bestimmen.
4. Persistenzmechanismen und laterale Bewegung identifizieren.
5. Bestimmen, ob Daten, Systeme oder Geschäftsprozesse zugegriffen oder missbraucht wurden.
6. Vertrauenswürdigen Identitätszustand wiederherstellen und Kontrollen stärken.
7. Rechtliche, regulatorische, finanzielle und operative Schäden minimieren.

## Zentrale Reaktionsprinzipien

- Identitätskompromittierung bis zum Gegenbeweis als wahrscheinlich behandeln.
- Flüchtige Beweise vor destruktiver Remediation sichern.
- Bestätigte Fakten von Hypothesen trennen und alle Entscheidungen dokumentieren.
- Sitzungen, Tokens, MFA-Zustand, OAuth-Berechtigungen, Wiederherstellungsmethoden, Rollen und delegierte Zugriffe sind Teil der Angriffsfläche.
- Ein Passwort-Reset allein reicht möglicherweise nicht aus.
- Anmeldeaktivitäten, Mailbox-Aktivitäten, Cloud-Berechtigungen und privilegierte Änderungen gemeinsam prüfen.
- Datenschutz- und Rechtsstakeholder früh einbinden, wenn personenbezogene Daten oder regulierte Systeme betroffen sein könnten.
- Eine einzige autoritative Incident-Timeline und ein Entscheidungsprotokoll führen.
- Vertrauen nicht wiederherstellen, bevor Persistenz- und Missbrauchspfade entfernt sind.

---

## Erste 15 Minuten

- Incident Response Lead / Incident Commander aktivieren.
- Incident-Timeline und Entscheidungsprotokoll starten.
- Betroffene(s) Konto/Konten, Tenant, Identity Provider, Gerät, Mailbox, Anwendung und Geschäftsprozess identifizieren.
- Feststellen, ob es sich um ein Benutzerkonto, Dienstkonto, Admin-Konto, eine föderierte Identität oder ein Break-Glass-Konto handelt.
- Feststellen, ob der Benutzer etwas Verdächtiges geklickt, genehmigt, eingegeben oder installiert hat.
- Verdächtige Sitzungen und Refresh-Tokens widerrufen, wenn ein Kompromittierungsverdacht besteht.
- Betroffenes Konto einschränken oder deaktivieren, wenn aktiver Missbrauch läuft.
- Logs, Alerts, Screenshots, MFA-Ereignisse, Anmeldeprotokolle und Zugriffsaufzeichnungen sichern.
- Ursprüngliche Phishing-, Login- oder Consent-Artefakte sichern, sofern vorhanden.
- Verdächtige Inbox-Regeln, Weiterleitungsregeln, Delegationen, OAuth-Berechtigungen oder Änderungen an Wiederherstellungsmethoden prüfen.
- Zusammenhängende Aktivitäten im Tenant und angrenzenden Systemen suchen.
- Datenschutz- und Rechtsstakeholder benachrichtigen, wenn personenbezogene Daten oder regulierte Zugriffe betroffen sein könnten.
- Keine Logs löschen oder Systeme zurücksetzen, bevor Beweise gesichert sind.

Nicht tun:
- Anmeldedaten zurücksetzen, ohne Sitzungswiderruf und Persistenz zu berücksichtigen.
- Annehmen, dass der Vorfall auf ein einziges Konto beschränkt ist.
- Dem Konto vertrauen, bevor MFA, Wiederherstellungsmethoden, OAuth-Berechtigungen, delegierte Zugriffe und privilegierte Änderungen geprüft wurden.
- Beweise löschen, bevor sie gesichert wurden.
- Extern über Ursache, Datenexposition oder finanzielle Auswirkungen sprechen, bevor Fakten bestätigt sind.

---

## Rollen und Verantwortlichkeiten

- Incident Commander: Gesamtkoordination, Ownership der Timeline, Entscheidungsverfolgung und Eskalation.
- Identity Lead: koordiniert Identity-Provider-Maßnahmen, Sitzungen, MFA, OAuth-Berechtigungen, Wiederherstellungsmethoden und Zugriffsprüfungen.
- Technische Leitung: koordiniert Endpoint-, Cloud-, Applikations-, Server-, Netzwerk- und Verzeichnis-Response.
- Forensik / Security Analyst: sichert und analysiert Beweise, Logs, Alerts und Telemetrie.
- Daten- / Applikationseigner: liefert Geschäftskontext zu betroffenen Identitäten und Systemen.
- Privacy / Datenschutzbeauftragter: bewertet Privacy-Impact, Breach-Status und Meldepflichten.
- Legal: bewertet rechtliche, regulatorische, vertragliche und haftungsrechtliche Auswirkungen.
- Kommunikationsleitung: koordiniert interne und externe Kommunikation mit Legal/Privacy.
- Management / Krisenteam: genehmigt wesentliche Entscheidungen und unterstützt Business Continuity.
- Third-Party / Supplier Manager: koordiniert mit MSSPs, SaaS-Providern, Identity Providern und anderen externen Parteien.

---

## Phase 1: Erkennen & Triage

### Ziele

- Feststellen, ob eine Identitätskompromittierung vorliegen könnte.
- Betroffene Identitäten, Geräte, Systeme und Services identifizieren.
- Bestimmen, ob der Vorfall noch aktiv ist.
- Bestimmen, ob privilegierter Zugriff, Token-Missbrauch oder laterale Bewegung vorliegt.
- Beweise sichern, die für technische, rechtliche und regulatorische Bewertungen erforderlich sind.

### Maßnahmen

- Den initialen Bericht, Alarm oder Entdeckungsdetails sammeln.
- Festhalten, wer das Problem entdeckt hat und wann.
- Das betroffene Benutzerkonto, Dienstkonto, Admin-Konto, Tenant oder die Anwendung identifizieren.
- Bestimmen, ob die Identität lokal, föderiert, cloud-nativ oder servicebasiert ist.
- Bestimmen, ob unautorisierter Zugriff noch möglich ist.
- Anmeldeprotokolle auf ungewöhnliche Orte, Geräte, Zeiten, ASNs, IPs oder User-Agents prüfen.
- MFA-Ereignisse prüfen, einschließlich Aufforderungen, Genehmigungen, Ablehnungen, Fatigue-Mustern, Methodenänderungen, Entfernungen und Neu-Registrierungen.
- Passwort-Reset-Ereignisse und Änderungen an Wiederherstellungsmethoden prüfen.
- Sitzungs- und Refresh-Token-Aktivität prüfen, sofern verfügbar.
- OAuth-Consent-Berechtigungen und Applikationsrechte prüfen.
- Mailbox-, Cloud- und Applikationszugriffslogs zur betroffenen Identität prüfen.
- Endpoint-Telemetrie auf Browser-Diebstahl, Infostealer-Indikatoren oder Token-Diebstahl prüfen.
- Änderungen an Admin-Rollen, Gruppenmitgliedschaften und privilegierten Zuweisungen prüfen.
- Erstellung oder Modifikation von Konten, Service Principals, App-Registrierungen und Secrets prüfen.
- VPN-, RDP-, VDI-, PAM-, SSH-, SSO- und Remote-Access-Logs prüfen.
- Domain-Controller-Logs auf Kerberos, NTLM, LDAP, Gruppenänderungen und verdächtige Authentifizierungsmuster prüfen.
- Screenshots, Alerts, Audit-Trails und relevante Konfigurations-Snapshots sichern.
- Feststellen, ob der Vorfall isoliert, kontenübergreifend oder identitätsinfrastrukturweit ist.
- Eine initiale Incident-Timeline erstellen.

### Erste Klassifizierung

- Verdächtiger Login.
- Bestätigte Benutzerkontokompromittierung.
- Kompromittierung privilegierter Konten.
- Kompromittierung von Dienstkonten.
- Cloud-Identitätskompromittierung.
- Token-Diebstahl.
- OAuth-Consent-Kompromittierung.
- Offenlegung von API-Schlüssel oder Secret.
- MFA-Kompromittierung oder MFA-Fatigue.
- Password Spraying.
- Credential Stuffing.
- Credential Dumping.
- Laterale Bewegung mit gültigen Anmeldedaten.
- Kompromittierung von Verzeichnis- oder Tenant-Konfiguration.
- Kompromittierung des Identity Providers.
- Datenzugriff über kompromittierte Identität.
- Ransomware-Voraktivität.
- Insider-Missbrauch.

---

## Phase 2: Eindämmung

### Ziele

- Unautorisierten Zugriff stoppen.
- Laterale Bewegung verhindern.
- Privilegieneskalation verhindern.
- Beweise sichern.
- Kritische Services nicht unnötig beeinträchtigen.

### Maßnahmen für Benutzerkonten

- Aktive Sitzungen und Refresh-Tokens widerrufen.
- Login blockieren oder Konto deaktivieren, wenn aktiver Missbrauch läuft.
- Passwort nach Sitzungswiderruf zurücksetzen.
- MFA-Neu-Registrierung verlangen, wenn MFA-Methoden kompromittiert sein könnten.
- Verdächtige MFA-Methoden, Geräte und Authenticator entfernen.
- Verdächtige OAuth-Berechtigungen und delegierte Rechte entfernen.
- Verdächtige Mailbox-Regeln, Weiterleitungen und Delegationen entfernen.
- Unautorisierte Gruppenmitgliedschaften und Rollenzuweisungen entfernen.
- Zugriff einschränken, bis die Untersuchung abgeschlossen ist.
- Auf sofortige Re-Authentifizierungsversuche achten.

### Maßnahmen für privilegierte Konten

- Konto sofort deaktivieren oder einschränken, wenn ein Kompromittierungsverdacht besteht.
- Sitzungen und Tokens widerrufen.
- Temporäre oder verdächtige Rollenzuweisungen entfernen.
- Alle jüngsten administrativen Aktionen prüfen.
- Prüfen, ob das Konto Identity-, Logging-, Security-, Backup- oder Recovery-Settings verändert hat.
- Anmeldedaten erst rotieren, nachdem Abhängigkeiten und Persistenzrisiken dokumentiert sind.
- Für Response-Maßnahmen saubere, vertrauenswürdige Admin-Workstations verwenden.
- Prüfen, dass Break-Glass-Konten sicher bleiben.
- Alle Konten mit gleichwertigen Privilegien überprüfen.
- Bei Bedarf einen Notfall-Review administrativer Zugriffe im Tenant oder Domain-Bereich durchführen.

### Maßnahmen für Dienstkonten und Anwendungsidentitäten

- Systeme, Anwendungen, Jobs und Integrationen identifizieren, die das Konto verwenden.
- Konto nur deaktivieren, wenn die Business-Auswirkung verstanden ist oder aktiver Missbrauch bestätigt wurde.
- Passwörter, Secrets, Zertifikate, SSH-Schlüssel oder API-Schlüssel rotieren.
- Tokens und Refresh-Tokens, wo möglich, widerrufen.
- Nicht erforderliche Privilegien entfernen.
- Prüfen, ob das Dienstkonto interaktiv verwendet wurde.
- Prüfen, ob das Konto ungewöhnliche Datenzugriffe, Exporte oder administrative Aktionen ausgeführt hat.
- Langlebige Anmeldedaten möglichst durch Managed Identities oder stärkere Secret-Verwaltung ersetzen.
- Abhängige Systeme nach der Rotation überwachen.

### Maßnahmen für Cloud- / SaaS-Identitäten

- Sitzungen und Tokens widerrufen.
- Admin-Rollen und Rollenzuweisungen prüfen.
- OAuth-Berechtigungen, App-Zustimmungen und Service Principals prüfen.
- Conditional Access und MFA-Settings prüfen.
- Tenant-weite Konfigurationsänderungen prüfen.
- Externes Teilen, Gastzugriffe und Föderationseinstellungen prüfen.
- Logs auf ungewöhnlichen Applikationszugriff, Datenexport und administrative Aktivitäten prüfen.
- Riskante Anwendungen und riskante Sign-in-Bedingungen einschränken.

### Maßnahmen für Active Directory / Hybrid Identity

- Domain-Controller-Logs prüfen.
- Privilegierte Gruppenänderungen prüfen.
- Kerberos- und NTLM-Aktivität prüfen.
- GPO-Änderungen prüfen.
- AD-Delegationsänderungen prüfen.
- Verzeichnissynchronisation prüfen.
- Föderationskonfiguration prüfen.
- Verdächtige Replikationsaktivität prüfen.
- Credential-Dumping-Indikatoren auf Endpoints und Servern prüfen.
- Systeme isolieren, auf denen Credential Dumping oder laterale Bewegung vermutet wird.
- Einen KRBTGT-Reset nur nach sorgfältiger Planung und nach Bestätigung von Domain-Kompromittierungsindikatoren erwägen.

Nicht tun:
- Passwörter nicht zurücksetzen, ohne Sitzungen zu widerrufen, wenn Tokens noch gültig sein könnten.
- Dienstkonto-Anmeldedaten nicht rotieren, ohne Abhängigkeiten zu verstehen.
- Kritische Dienstkonten nicht ohne Fallback-Plan deaktivieren, außer aktiver Missbrauch erzwingt es.
- Potenziell kompromittierte Admin-Workstations nicht für Remediation verwenden.
- Nicht annehmen, dass Cloud-Identitätskompromittierung durch ein On-Premises-Passwort-Reset allein gelöst ist.
- Nicht annehmen, dass On-Premises-Kompromittierung durch reine Cloud-Eindämmung gelöst ist.
- Beweise nicht durch Massenbereinigung vor Log-Sicherung zerstören.

---

## Phase 3: Untersuchung

### Ziele

- Initialen Zugriff bestimmen.
- Anmeldedatenquelle bestimmen.
- Aktivitäten des Angreifers bestimmen.
- Laterale Bewegung bestimmen.
- Privilegieneskalation bestimmen.
- Datenzugriff bestimmen.
- Persistenz bestimmen.
- Bestimmen, ob weitere Identitäten betroffen sind.

### Untersuchungsmaßnahmen

- Ermitteln, wie die Identität kompromittiert wurde.
- Phishing-Meldungen und E-Mail-Sicherheitslogs prüfen.
- Endpoint-Telemetrie auf Malware, Infostealer, Credential Dumping und Browser-Credential-Diebstahl prüfen.
- Indikatoren für Password Spraying und Credential Stuffing prüfen.
- Verfügbare Quellen zu offengelegten Anmeldedaten in Threat-Intel-/Breach-Quellen prüfen.
- Verdächtige VPN-, RDP-, SSH-, VDI- und SSO-Aktivitäten prüfen.
- Alle Anwendungen prüfen, auf die die kompromittierte Identität zugegriffen hat.
- Administrative Aktionen der Identität prüfen.
- Datenrepositories prüfen, auf die die Identität zugegriffen hat.
- Prüfen, ob der Angreifer Konten erstellt oder geändert hat.
- Prüfen, ob Rollen, Gruppen oder Berechtigungen geändert wurden.
- Prüfen, ob MFA-Methoden hinzugefügt, entfernt oder geändert wurden.
- Prüfen, ob OAuth-Anwendungen oder Service Principals hinzugefügt oder geändert wurden.
- Prüfen, ob Secrets, Zertifikate, API-Schlüssel oder Zugriffsschlüssel erstellt oder exportiert wurden.
- Prüfen, ob Mailbox-Weiterleitungen, Regeln oder Delegationen konfiguriert wurden.
- Prüfen, ob Logging-, Aufbewahrungs-, Audit- oder Sicherheitskontrollen deaktiviert wurden.
- Prüfen, ob Backups, Recovery-Systeme oder Security-Tools zugegriffen wurden.
- Prüfen, ob laterale Bewegung über SMB, RDP, WinRM, SSH, WMI, PsExec oder Remote-Management-Tools stattfand.
- Prüfen, ob Privileged Access Workstations oder Admin-Jump-Hosts genutzt wurden.
- Alle betroffenen Identitäten und Systeme identifizieren.
- Feststellen, ob der Vorfall auf Ransomware-, Data-Breach- oder Insider-Threat-Playbooks eskaliert werden sollte.

### Typische Indikatoren einer Identitätskompromittierung

- Erfolgreiche Anmeldungen von ungewöhnlichen Orten.
- Ungewöhnliche Reiserouten oder nicht plausible Standortwechsel.
- Neue Geräte-Registrierung.
- Neue MFA-Methoden-Registrierung.
- Wiederholte MFA-Prompts oder unerwartete Genehmigungen.
- Login über anonymen Proxy, TOR, VPS oder unbekannte ASN.
- Login mit unbekanntem User-Agent.
- Zugriff auf ungewöhnliche Anwendungen.
- Ungewöhnliche Mailbox-Aktivität.
- Neue Weiterleitungsregeln.
- Neue OAuth-Berechtigungen.
- Neue App-Registrierungen oder Service Principals.
- Neue Client-Secrets oder Zertifikate.
- Neue oder geänderte Admin-Rollen.
- Neue Gruppenmitgliedschaften.
- Ungewöhnliche Passwort-Reset-Aktivitäten.
- Deaktiviertes oder verändertes Logging.
- Änderungen an Conditional-Access-Richtlinien.
- Änderungen an Föderations- oder SSO-Konfigurationen.
- Interaktiver Login eines Dienstkontos.
- Alerts zu Credential Dumping.
- Kerberoasting-Indikatoren.
- Pass-the-Hash- oder Pass-the-Ticket-Indikatoren.
- Laterale Bewegung mit gültigen Anmeldedaten.

### Analyse der Anmeldedatenquelle

Mögliche Quelle der Kompromittierung prüfen:

- Phishing.
- MFA-Fatigue.
- OAuth-Consent-Phishing.
- Infostealer-Malware.
- Diebstahl des Browser-Passwortspeichers.
- Passwort-Wiederverwendung.
- Credential Stuffing.
- Password Spraying.
- Offengelegtes Passwort in Code oder Dokumentation.
- Durchgesickertes API-Secret oder anderer Geheimniswert.
- Kompromittierter Endpoint.
- Kompromittierte Admin-Workstation.
- Kompromittiertes Dienstkonto.
- Drittanbieter-Vorfall.
- Insider-Missbrauch.
- Ungemanagtes Privatgerät.
- Legacy Authentication.
- Schwaches oder gemeinsam genutztes Passwort.
- Fehlende MFA.
- Token-Diebstahl.

---

## Phase 4: Beseitigung

### Ziele

- Angreiferzugriff entfernen.
- Persistenz der Identität entfernen.
- Kontointegrität wiederherstellen.
- Den initialen Zugriffspfad schließen.
- Zukünftiges Kompromittierungsrisiko reduzieren.

### Remediation-Maßnahmen

- Passwörter betroffener Konten zurücksetzen.
- Aktive Sitzungen und Refresh-Tokens widerrufen.
- MFA-Methoden bei Bedarf neu registrieren.
- Verdächtige MFA-Methoden und Geräte entfernen.
- Unautorisierte OAuth-Berechtigungen und App-Zustimmungen entfernen.
- Unautorisierte Rollenzuweisungen und Gruppenmitgliedschaften entfernen.
- Unautorisierte Mailbox-Regeln, Weiterleitungen und Delegationen entfernen.
- Unautorisierte Service Principals, App-Registrierungen oder Client-Secrets entfernen.
- Offengelegte API-Schlüssel, Zugriffsschlüssel, Zertifikate, SSH-Schlüssel und Secrets rotieren.
- Conditional-Access-Richtlinien prüfen und härten.
- Legacy Authentication deaktivieren, wo möglich.
- MFA für alle Benutzer erzwingen.
- Phishing-resistente MFA für Administratoren und High-Risk-User einsetzen, wo möglich.
- Privilegierten Zugriff über gehärtete Admin-Workstations oder PAM erzwingen.
- Dauerhafte Privilegien entfernen, wo nicht erforderlich.
- Just-in-Time-Privileged-Access einsetzen, wo verfügbar.
- Dienstkonten härten und Berechtigungen reduzieren.
- Interaktiven Login für Dienstkonten möglichst verhindern.
- Endpoints mit Credential-Diebstahl patchen und bereinigen.
- Systeme neu aufsetzen, wenn Credential Dumping oder tiefgreifender Kompromiss vorlag.
- Erkennungsregeln für beobachtete Indikatoren und Verhaltensmuster aktualisieren.
- Sicherstellen, dass Logging- und Audit-Richtlinien aktiviert sind.

### Spezielle Remediation: Kompromittierung privilegierter Identitäten

- Vollständigen Review privilegierter Zugriffe durchführen.
- Alle Admin-Rollenzuweisungen prüfen.
- Alle privilegierten Gruppenmitgliedschaften prüfen.
- Alle jüngsten Tenant-, Domain- und Security-Konfigurationsänderungen prüfen.
- Alle neuen Konten und Service Principals prüfen.
- Alle neu erstellten Secrets und Zertifikate prüfen.
- Alle Break-Glass-Konten prüfen.
- Privilegierte Anmeldedaten nur von vertrauenswürdigen Systemen aus rotieren.
- Administrative Workstations validieren.
- Domain- oder Tenant-Kompromittierungsverfahren erwägen, wenn das Vertrauen in die Identitätsinfrastruktur unklar ist.
- Externe Incident-Response-Unterstützung einbinden, wenn Domain-, Tenant- oder Föderationskompromittierung vermutet wird.

### Spezielle Remediation: Dienstkonto-Kompromittierung

- Alle Systeme kartieren, die das Dienstkonto verwenden.
- Anmeldedaten kontrolliert sequenziell rotieren.
- Nicht erforderliche Privilegien entfernen.
- Interaktiven Login entfernen, wo nicht erforderlich.
- Anmeldeorte einschränken.
- Starke Zufallspasswörter oder Managed Identities verwenden.
- Secrets in einem sicheren Vault speichern.
- Fehlgeschlagene Authentifizierungen nach der Rotation überwachen.
- Geteilte Dienstkonten möglichst ersetzen.
- Ownership und Rotationsverfahren dokumentieren.

---

## Phase 5: Wiederherstellung

### Ziele

- Sicheren Identitätsbetrieb wiederherstellen.
- Validieren, dass Angreiferzugriff entfernt wurde.
- Betroffene Benutzer und Services sicher wiederherstellen.
- Auf Wiederkehr oder Re-Entry überwachen.

### Recovery-Maßnahmen

- Betroffene Konten erst nach Abschluss der Remediation wieder aktivieren.
- Bestätigen, dass Sitzungen und Tokens widerrufen wurden.
- Bestätigen, dass Passwörter, Secrets und Schlüssel rotiert wurden.
- Bestätigen, dass MFA-Methoden legitim sind.
- Bestätigen, dass Rollen, Gruppen und Berechtigungen korrekt sind.
- Bestätigen, dass Mailbox-Regeln, Weiterleitungen und Delegationen sauber sind.
- Bestätigen, dass OAuth-Berechtigungen und App-Zustimmungen legitim sind.
- Bestätigen, dass Abhängigkeiten von Dienstkonten nach der Rotation funktionieren.
- Bestätigen, dass Conditional Access und MFA-Richtlinien aktiv sind.
- Bestätigen, dass Logging und Alerting aktiv sind.
- Bestätigen, dass verdächtige Anwendungen oder Geräte entfernt wurden.
- Bestätigen, dass betroffene Geschäftsapplikationen korrekt funktionieren.
- Für mindestens 14 bis 30 Tage verstärkt überwachen.
- Auf wiederholte Login-Versuche, Token-Wiederverwendung, MFA-Prompts, verdächtige OAuth-Consents und laterale Bewegungen achten.

### Recovery-Validierung

- Keine aktiven Sitzungen aus verdächtigen Quellen verbleiben.
- Keine verdächtigen MFA-Methoden verbleiben.
- Keine verdächtigen OAuth-Berechtigungen verbleiben.
- Keine unautorisierte Gruppenmitgliedschaft verbleibt.
- Keine unautorisierte Admin-Rolle verbleibt.
- Keine unerwarteten Service Principals oder App-Registrierungen verbleiben.
- Keine verdächtige Mailbox-Weiterleitung verbleibt.
- Keine verdächtigen Änderungen an Conditional Access oder Identitätsrichtlinien verbleiben.
- Keine Hinweise auf fortgesetzte laterale Bewegung.
- Keine Hinweise auf fortgesetzten Datenzugriff.
- Benutzer oder Service Owner bestätigen den normalen Geschäftsbetrieb.

---

## Phase 6: Review / Lessons Learned

### Ziele

- Die Ursache verstehen.
- Identitätskontrollen verbessern.
- Erkennung und Response verbessern.
- Risiko künftiger Kontokompromittierung senken.
- Remediation bis zum Abschluss nachverfolgen.

### Maßnahmen

- Incident-Timeline finalisieren.
- Initialen Zugriffspfad identifizieren.
- Kompromittierte Identitäten und betroffene Systeme identifizieren.
- Bestimmen, ob Daten zugegriffen, verändert oder exfiltriert wurden.
- Kontrollschwächen identifizieren.
- MFA-Abdeckung und -Stärke prüfen.
- Wirksamkeit von Conditional Access prüfen.
- Modell des privilegierten Zugriffs prüfen.
- Governance von Dienstkonten prüfen.
- Logging- und Detektionsabdeckung prüfen.
- Meldungsverhalten und Eskalationsgeschwindigkeit prüfen.
- Response-Maßnahmen und Entscheidungsqualität prüfen.
- Identity-Hardening-Baselines aktualisieren.
- Erkennungsregeln und SIEM-Queries aktualisieren.
- Dieses Playbook anhand der Lessons Learned aktualisieren.
- Alle Remediation-Maßnahmen bis zum Abschluss verfolgen.

### Lessons Learned Questions

- Wie wurde die Identität kompromittiert?
- War MFA aktiviert?
- War MFA phishing-resistent?
- Wurde MFA umgangen, genehmigt, durch Fatigue erschöpft oder neu registriert?
- Hat Legacy Authentication zur Kompromittierung beigetragen?
- Waren Anmeldedaten wiederverwendet, schwach oder offengelegt?
- War ein Endpoint kompromittiert?
- War ein Dienstkonto überprivilegiert?
- Waren privilegierte Konten ausreichend geschützt?
- Wurden aktive Sitzungen schnell genug widerrufen?
- Blieben Tokens, OAuth-Berechtigungen oder App-Zustimmungen nach dem Passwort-Reset bestehen?
- Waren Logs ausreichend, um die Aktivitäten zu rekonstruieren?
- Hat der Angreifer Datenzugriff erlangt?
- Hat sich der Angreifer lateral bewegt?
- Hat der Angreifer Privilegien eskaliert?
- Hat der Angreifer Identitätsinfrastruktur verändert?
- Welche Kontrollen hätten den Impact verhindert oder reduziert?

---

## Beweise sichern

Folgende Beweise nach Möglichkeit sichern:

### Identitäts- und Authentifizierungsbeweise

- Anmeldeprotokolle.
- Protokolle fehlgeschlagener Anmeldungen.
- MFA-Logs.
- Risiko-Erkennungen.
- Conditional-Access-Ergebnisse.
- Passwort-Reset-Ereignisse.
- Änderungen an MFA-Registrierungen.
- Geräte-Registrierungsprotokolle.
- Sitzungswiderrufs-Protokolle.
- Token-Widerrufs-Protokolle.
- User-Risk- und Sign-in-Risk-Aufzeichnungen.
- Audit-Logs des Identity Providers.

### Verzeichnis- und Privilegienbeweise

- Änderungen an Gruppenmitgliedschaften.
- Änderungen an Rollenzuweisungen.
- Privileged-Access-Logs.
- Neue Kontoerstellungsereignisse.
- Kontomodifikationsereignisse.
- Konto-Deaktivierungs-/Aktivierungsereignisse.
- Logs zur Verzeichnissynchronisation.
- Änderungen an Föderationskonfigurationen.
- GPO-Änderungen.
- AD-Delegationsänderungen.
- Kerberos- und NTLM-Logs.
- Sicherheitsprotokolle der Domain Controller.

### Cloud- und Applikationsbeweise

- OAuth-Consent-Aufzeichnungen.
- App-Consent-Aufzeichnungen.
- Änderungen an Service Principals.
- Änderungen an App-Registrierungen.
- Änderungen an Client-Secrets und Zertifikaten.
- Logs zu Erstellung oder Nutzung von API-Schlüsseln.
- Cloud-IAM-Logs.
- SaaS-Audit-Logs.
- Aktivität im Admin-Portal.
- Ressourcen-Zugriffslogs.
- Datenexport-Logs.

### Endpoint- und Netzwerkbeweise

- EDR-Alerts.
- Prozessausführungslogs auf Endpoints.
- Erkennungen von Credential-Dumping-Tools.
- Indikatoren für Zugriff auf Browser-Credentials.
- PowerShell-Logs.
- Remote-Access-Logs.
- VPN-Logs.
- RDP-, SSH-, WinRM- und SMB-Logs.
- Proxy-Logs.
- DNS-Logs.
- Firewall-Logs.
- SIEM-Korrelationsevents.

### Kommunikations- und Entscheidungsbeweise

- Benutzerbericht.
- Helpdesk-Ticket.
- Incident-Timeline.
- Eindämmungsentscheidungen.
- Remediation-Entscheidungen.
- Management-Eskalationsaufzeichnungen.
- Legal-/Privacy-Bewertungsnotizen.
- Externe Benachrichtigungsaufzeichnungen, falls relevant.

---

## Kommunikations-Checkliste

### Interne Kommunikation

- Incident-Response-Team benachrichtigen.
- Identity-Administratoren benachrichtigen.
- Betroffene System- und Applikationseigner benachrichtigen.
- Management benachrichtigen, wenn der Schweregrad Hoch oder Kritisch ist.
- Legal/Privacy benachrichtigen, wenn Datenzugriff oder regulierte Daten betroffen sein könnten.
- Helpdesk benachrichtigen, wenn Benutzer MFA-Prompts, Sperren oder verdächtige Aktivitäten melden könnten.
- Betroffene Benutzer warnen, wenn Identitätsdiebstahl oder Phishing andauert.
- Eine einzige Quelle der Wahrheit für Incident-Updates beibehalten.
- Breite technische Spekulationen vermeiden, bevor Fakten bestätigt sind.

### Externe Kommunikation

- Betroffene Kunden, Lieferanten oder Partner benachrichtigen, wenn deren Daten oder Konten betroffen sein könnten.
- Cyberversicherer benachrichtigen, falls erforderlich.
- Strafverfolgungsbehörden benachrichtigen, wenn Betrug, Erpressung oder ein größerer Kompromittierungsfall vorliegt und die Unternehmenspolitik dies unterstützt.
- Regulatoren benachrichtigen, wenn Datenpannen- oder branchenspezifische Pflichten greifen.
- Alle externen Mitteilungen aufbewahren.
- Externe Nachrichten mit Legal/Privacy und Kommunikation abstimmen.

### Vorschlag für eine Benutzerkommunikation

Ein Verdacht auf Identitätskompromittierung wird derzeit untersucht. Wenn Sie unerwartete MFA-Anfragen, Passwort-Reset-Benachrichtigungen, ungewöhnliche Login-Warnungen oder Nachrichten erhalten, die nach Anmeldedaten fragen, melden Sie dies sofort an das IT-/Security-Team. Bitte genehmigen Sie keine unerwarteten MFA-Anfragen und ändern Sie keine Sicherheitseinstellungen, außer Sie werden über den offiziellen Incident-Kommunikationskanal dazu aufgefordert.

---

## Rechtliche und regulatorische Erwägungen

Identitätskompromittierung kann sich je nach Verhalten des Angreifers zu einer Datenpanne, einem Betrugsfall, einem Insider-Threat-Vorfall oder einer Ransomware-Vorbereitung entwickeln.

Legal/Privacy einbinden, wenn:

- Personenbezogene Daten möglicherweise zugegriffen wurden.
- Mailbox-Inhalte sensible oder regulierte Daten enthalten könnten.
- Die kompromittierte Identität auf Kunden-, Mitarbeiter-, Lieferanten- oder Finanzdaten zugegriffen hat.
- Privilegierter Zugriff umfangreiche Datensätze offengelegt haben könnte.
- Finanzbetrug stattgefunden hat oder versucht wurde.
- Externe Parteien schädliche Kommunikation von einem kompromittierten Konto erhalten haben.
- Vertragliche, regulatorische, versicherungsbezogene oder branchenspezifische Meldepflichten bestehen könnten.

Dieses Playbook stellt keine Rechtsberatung dar. Es unterstützt die Beweissicherung, strukturierte technische Reaktion und eine fundierte Legal-/Privacy-Bewertung.

---

## Quick Response Checkliste

### Sofortmaßnahmen

- Incident Lead zuweisen.
- Betroffene Identität identifizieren.
- Kontotyp und Privilegstufe bestimmen.
- Aktive Sitzungen und Tokens widerrufen.
- Login deaktivieren oder blockieren, wenn aktiver Missbrauch läuft.
- Identitäts- und Audit-Logs sichern.
- MFA-Änderungen prüfen.
- Rollen- und Gruppenänderungen prüfen.
- OAuth-Berechtigungen und App-Zustimmungen prüfen.
- Incident-Timeline starten.

### Scope

- Anmeldeprotokolle prüfen.
- MFA-Logs prüfen.
- Genutzte Anwendungen prüfen.
- Geräte und Standorte prüfen.
- Privilegierte Aktionen prüfen.
- Datenzugriff prüfen.
- Mailbox-Aktivität prüfen.
- Cloud- und SaaS-Aktivität prüfen.
- Endpoint-Telemetrie prüfen.
- Zusammenhängende Konten identifizieren.

### Eindämmung

- Sitzungen widerrufen.
- Passwort zurücksetzen.
- MFA-Neu-Registrierung verlangen.
- Verdächtige MFA-Methoden entfernen.
- Verdächtige OAuth-Berechtigungen entfernen.
- Unautorisierte Gruppenmitgliedschaften entfernen.
- Unautorisierte Rollen entfernen.
- Verdächtige Mailbox-Regeln entfernen.
- Offengelegte Secrets und Schlüssel rotieren.
- Remote Access bei Bedarf einschränken.

### Untersuchung

- Initialen Zugriff bestimmen.
- Quelle der Anmeldedaten bestimmen.
- Phishing- und Malware-Indikatoren prüfen.
- Credential-Dumping-Indikatoren prüfen.
- Laterale Bewegung prüfen.
- Privilegieneskalation prüfen.
- Persistenz prüfen.
- Datenzugriff oder Exfiltration prüfen.
- Bestimmen, ob ein Datenpannen-Playbook erforderlich ist.

### Beseitigung

- Angreifer-Persistenz entfernen.
- Anmeldedaten und Secrets rotieren.
- MFA härten.
- Legacy Authentication deaktivieren.
- Conditional Access härten.
- Privilegien reduzieren.
- Dienstkonten härten.
- Betroffene Endpoints bereinigen oder neu aufsetzen.
- Erkennungen aktualisieren.
- Logging validieren.

### Wiederherstellung

- Konto erst nach Validierung wieder aktivieren.
- Bestätigen, dass Sitzungen und Tokens widerrufen wurden.
- Bestätigen, dass MFA-Methoden legitim sind.
- Bestätigen, dass Berechtigungen korrekt sind.
- Bestätigen, dass keine verdächtigen OAuth-Berechtigungen verbleiben.
- Bestätigen, dass Dienstabhängigkeiten funktionieren.
- Für 14 bis 30 Tage überwachen.
- Remediation bis zum Abschluss verfolgen.

---

## Präventive Kontrollen

### Identitätssicherheit

- MFA für alle Benutzer erzwingen.
- Für Administratoren und High-Risk-User, wo möglich, phishing-resistente MFA verwenden.
- Legacy Authentication deaktivieren.
- Conditional Access anwenden.
- Für sensible Zugriffe verwaltete oder konforme Geräte verlangen.
- Riskante Sign-ins und riskante Benutzer überwachen.
- Ungewöhnliche Standorte und ungewohnte Sign-in-Eigenschaften überwachen.
- MFA-Fatigue-Muster überwachen.
- Neue MFA-Methoden-Registrierungen überwachen.
- OAuth-App-Zustimmungen und delegierte Berechtigungen überwachen.
- Externe Identitäten und Gastkonten regelmäßig prüfen.

### Privilegierter Zugriff

- Separate Admin-Konten verwenden.
- Privileged Access Workstations oder gehärtete Admin-Geräte verwenden.
- Just-in-Time-Privileged-Access, wo verfügbar, einsetzen.
- Dauerhafte Privilegien möglichst entfernen.
- Admin-Rollen regelmäßig prüfen.
- Break-Glass-Konten mit striktem Monitoring schützen.
- Starke Authentifizierung für privilegierte Aktionen verlangen.
- Aktivierung und Zuweisung privilegierter Rollen überwachen.
- Benutzer- und Admin-Konten klar trennen.

### Sicherheit von Dienstkonten

- Ein Inventar aller Dienstkonten pflegen.
- Eigentümer für Dienstkonten zuweisen.
- Interaktiven Login, wo nicht erforderlich, verhindern.
- Anmeldeorte einschränken.
- Least Privilege anwenden.
- Anmeldedaten regelmäßig rotieren.
- Secrets in einem sicheren Vault speichern.
- Wo möglich Managed Identities oder Workload Identities bevorzugen.
- Verhalten von Dienstkonten überwachen.
- Geteilte Dienstkonten, wo möglich, ersetzen.

### Active Directory / Hybrid Identity

- Privilegierte Gruppenänderungen überwachen.
- Authentifizierungsmuster auf Domain Controllern überwachen.
- Kerberos-Anomalien überwachen.
- NTLM-Nutzung überwachen.
- GPO-Änderungen überwachen.
- Domain Controller schützen.
- Administrative Login-Pfade einschränken.
- Administrative Zugriffe tiering-basiert organisieren.
- Verzeichnissynchronisation härten.
- Föderationsvertrauen und Zertifikate prüfen.
- Backup- und Recovery-Identitäten schützen.

### Erkennung und Response

- Auf verdächtige Anmeldungen alerten.
- Auf neue MFA-Methoden-Registrierungen alerten.
- Auf OAuth-Consent-Berechtigungen alerten.
- Auf Admin-Rollenzuweisungen alerten.
- Auf verdächtige Dienstkonto-Logins alerten.
- Auf ungewöhnliche Standortwechsel alerten.
- Auf massenhafte Fehlanmeldungen alerten.
- Auf Password Spraying alerten.
- Auf Credential-Dumping-Tools alerten.
- Auf laterale Bewegung mit gültigen Konten alerten.
- Account-Compromise-Response-Prozesse regelmäßig testen.

---

## Website-Summary-Card

### Identitätskompromittierung

Response-Guide für kompromittierte Benutzer-, Dienst- oder privilegierte Konten — einschließlich Identitätsdiebstahl, unautorisierter Zugriff, MFA-Missbrauch, Token-Diebstahl, Erkennung lateraler Bewegung und Verfahren zur Wiederherstellung des Identitätsvertrauens.

### Tags

ERKENNEN  
EINDÄMMEN  
UNTERSUCHEN  
BESEITIGEN  
WIEDERHERSTELLEN  
REVIEW

---

## Referenzen

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

Dieses Playbook ist eine praktische Community-Ressource. Es ersetzt keine Rechtsberatung, forensische Expertise, Anforderungen der Cyberversicherung, regulatorische Bewertung oder organisationsspezifische Incident-Response-Planung. Organisationen sollten diese Verfahren an ihre Identitätsarchitektur, Cloud-Plattformen, Verzeichnisdienste, ihr Risikoprofil, ihre rechtlichen Pflichten und ihre Incident-Response-Fähigkeiten anpassen.
