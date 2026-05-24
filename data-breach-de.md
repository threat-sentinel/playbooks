# Datenpannen- / Verdacht-auf-Datenpanne-Playbook

## Zusammenfassung

Dieses Playbook bietet ein praktisches, technisch orientiertes Incident-Response-Verfahren für bestätigte oder vermutete Datenpannen mit starkem Fokus auf die Nutzbarkeit für die Community und die reale Einsatzpraxis.
Es soll Incident Respondern helfen, laufende Exposition zu stoppen, Beweise zu sichern, den Umfang zu bestimmen, das Risiko für betroffene Personen zu bewerten und rechtliche sowie regulatorische Entscheidungsprozesse zu unterstützen, ohne die technische Klarheit zu verlieren.

Das Playbook folgt einem standardisierten Incident-Response-Ablauf: Erkennen & Triage, Eindämmen, Untersuchen, Benachrichtigen & Kommunizieren, Beheben & Wiederherstellen sowie Nachbereitung.
Es richtet sich an Security-Teams, IT-Administratoren, SOC-Analysten, Datenschutz-Teams, juristische Stakeholder, Berater sowie kleine und mittlere Organisationen.

> Hinweis: Dies ist keine Rechtsberatung und kein Ersatz für forensische Expertise, Datenschutzberatung, regulatorische Bewertung oder organisationsspezifische Incident-Response-Planung.

## Zweck

- Einen strukturierten Reaktionsprozess für vermutete und bestätigte Datenpannen bereitstellen.
- Schnelle Validierung, Eindämmung und Beweissicherung ermöglichen.
- Den Umfang über Systeme, Datenablagen, Endpunkte, Cloud-Services, Mailboxen, Datenbanken und Drittanbieterplattformen hinweg bestimmen.
- Unautorisierte Zugriffe und unsichere Konfigurationen entfernen.
- Sicheren und vertrauenswürdigen Betrieb wiederherstellen.
- DSGVO-orientierte Bewertung von Meldungen und Entscheidungsprozessen unterstützen.
- Dokumentierte Lessons Learned und nachverfolgte Remediation erzeugen.

## Anwendungsbereich / Wann verwenden

Verwenden Sie dieses Playbook, wenn einer oder mehrere der folgenden Punkte vorliegen:

- Sensible Daten wurden von einer unautorisierten Partei eingesehen.
- Personenbezogene Daten könnten offengelegt, kopiert, verloren, verändert oder weitergegeben worden sein.
- Vertrauliche Geschäftsdaten wurden an den falschen Empfänger gesendet.
- Eine Mailbox, eine Dateifreigabe, eine Datenbank, ein Cloud-Speicherort oder eine Kollaborationsplattform wurde exponiert.
- Daten wurden versehentlich oder absichtlich veröffentlicht.
- Systeme mit personenbezogenen, vertraulichen oder regulierten Daten wurden kompromittiert.
- Eine Drittpartei meldet geleakte, gestohlene oder offengelegte Unternehmensdaten.
- Ransomware-Akteure behaupten, Daten gestohlen zu haben.
- Logs zeigen ungewöhnliche Downloads, Exporte, Datenbankabfragen oder Dateizugriffe.
- Backup-Medien, Laptops, mobile Geräte oder Wechseldatenträger mit sensiblen Daten gingen verloren oder wurden gestohlen.
- Zugriffsrechte waren fehlerhaft konfiguriert und haben Daten unautorisierten Benutzern zugänglich gemacht.
- Zugangsdaten, Tokens, API-Keys oder Secrets wurden offengelegt und könnten den Zugriff auf Daten ermöglicht haben.

## Schweregrad

Standard-Schweregrad: **Hoch**.

Auf **Kritisch** eskalieren, wenn einer der folgenden Punkte zutrifft:

- Personenbezogene Daten, regulierte Daten, Zugangsdaten, Finanzdaten, Gesundheitsdaten, Geschäftsgeheimnisse oder große Datenmengen sind betroffen.
- Eine öffentliche Exposition hat stattgefunden.
- Eine bestätigte Exfiltration liegt vor oder wird stark vermutet.
- Privilegierter Zugriff oder administrativer Zugriff war beteiligt.
- Rechtliche Meldefristen könnten gelten.
- Eine ransomware-bezogene Datendiebstahl-Situation liegt vor.
- Der Vorfall betrifft eine große Anzahl von Datensätzen oder Personen.
- Die Exposition könnte ein hohes Risiko für die Rechte und Freiheiten natürlicher Personen erzeugen.

## Incident-Ziele

Während der ersten Stunden eines Datenpannen-Vorfalls sollte das Incident-Response-Team auf folgende Ziele hinarbeiten:

1. Laufende Exposition stoppen.
2. Beweise sichern und eine belastbare Timeline führen.
3. Bestimmen, welche Daten, Systeme, Benutzer und Drittparteien betroffen sind.
4. Bewerten, ob personenbezogene oder regulierte Daten betroffen sind.
5. Bewerten, ob Meldepflichten bestehen könnten.
6. Rechtliche, regulatorische, datenschutzrechtliche, geschäftliche und operative Schäden minimieren.
7. Sicheren Betrieb wiederherstellen und Remediation bis zum Abschluss nachverfolgen.

## Zentrale Reaktionsprinzipien

- Flüchtige Beweise vor destruktiver Remediation sichern.
- Bestätigte Fakten von Hypothesen trennen und alle Entscheidungen dokumentieren.
- Identitätsmissbrauch, öffentliche Exposition und Fehlkonfigurationen als mögliche Datenpannenpfade behandeln.
- Datenschutz- und Rechtsstakeholder früh einbinden, wenn personenbezogene Daten betroffen sein könnten.
- Nicht davon ausgehen, dass das Fehlen bestätigter Exfiltration bedeutet, dass keine Datenpanne vorliegt.
- Nicht löschen, rotieren, neu aufsetzen oder wiederherstellen, wenn dadurch Beweise verloren gehen.
- Einen datenverantwortlichen Ansatz verwenden, um Sensibilität und Business-Impact zu bewerten.
- Discovery-Zeitpunkt und den Zeitpunkt dokumentieren, zu dem die Organisation von einer möglichen Datenpanne Kenntnis erlangt hat.
- Eine einzige autoritative Incident-Timeline und ein Entscheidungsprotokoll führen.
- Bewerten, ob Verschlüsselung, Pseudonymisierung oder Zugriffskontrollen das Risiko reduziert haben.

---

## Erste 15 Minuten (Sofortmaßnahmen)

- Den Incident-Response-Lead / Incident Commander aktivieren.
- Eine Incident-Timeline und ein Entscheidungsprotokoll starten.
- Dokumentieren, wer das Problem entdeckt hat und wann.
- Dokumentieren, wann die Organisation von der möglichen Datenpanne Kenntnis erlangt hat.
- Das betroffene System, Dataset, Konto, die Mailbox, Dateifreigabe, Datenbank, Anwendung, Cloud-Speicherort oder Drittanbieterdienst identifizieren.
- Feststellen, ob die Exposition weiterhin aktiv ist.
- Identifizieren, ob personenbezogene Daten, regulierte Daten oder vertrauliche Geschäftsdaten betroffen sein könnten.
- Logs, Audit-Trails, Alerts, Screenshots, Berechtigungen und Zugriffsaufzeichnungen sichern.
- Laufenden unautorisierten Zugriff stoppen, ohne Beweise zu zerstören.
- Zugriffe einschränken, öffentliche Links deaktivieren oder kompromittierte Sessions widerrufen, wo angemessen.
- Datenschutz- und Rechtsstakeholder sofort benachrichtigen, wenn personenbezogene Daten betroffen sein könnten.
- Dateien, E-Mails, Logs oder Artefakte nicht löschen, bevor sie gesichert wurden.
- Eine erste Incident-Klassifizierung starten.

Nicht tun:
- Exponierte Daten löschen, bevor Beweise dokumentiert und gesichert wurden.
- Betroffene Systeme unnötig verändern, bevor Logs und Artefakte gesammelt wurden.
- Externe Parteien benachrichtigen, bevor Fakten validiert und Datenschutz-/Rechtsstakeholder eingebunden sind.
- Unbelegte Aussagen zur Anzahl betroffener Personen oder Datensätze machen.
- Annehmen, dass das Ausbleiben bestätigter Exfiltration bedeutet, dass keine Datenpanne vorliegt.
- Die Eskalation an Datenschutz-/Rechts-Teams verzögern, während auf perfekte technische Gewissheit gewartet wird.

---

## Rollen und Verantwortlichkeiten

- Incident Commander: Gesamtkoordination, Timeline-Verantwortung, Entscheidungsprotokoll und Eskalation.
- Technische Leitung: Koordiniert Infrastruktur-, Cloud-, Identity-, Applikations-, Endpoint-, Datenbank- und Netzwerk-Response.
- Forensik / Security Analyst: Sichert und analysiert Beweise, Logs, Alerts, Telemetrie und Zugriffsaufzeichnungen.
- Data Owner / Business Owner: Stellt Geschäftskontext, Datensensibilität und betroffene Datasets fest.
- Datenschutzbeauftragter / Privacy: Bewertet Datenschutzrisiken, Breach-Status und Meldepflichten.
- Legal: Bewertet rechtliche, regulatorische, vertragliche und haftungsrechtliche Auswirkungen.
- Kommunikationsleitung: Koordiniert interne und externe Kommunikation mit Datenschutz/Recht.
- Management / Krisenstab: Genehmigt wesentliche Entscheidungen und unterstützt die Geschäftskontinuität.
- Drittanbieter- / Lieferantenmanagement: Koordiniert mit Auftragsverarbeitern, MSPs, SaaS-Anbietern und anderen externen Parteien.

---

## Entscheidungsmodell (schnelle Regeln)

### Regel 1 — Ist das ein Security Incident oder eine Datenpanne?

Behandeln Sie das Ereignis als **Datenpannen-Kandidat**, wenn einer der folgenden Punkte zutrifft:

- Personenbezogene Daten könnten eingesehen, offengelegt, verloren, verändert oder zerstört worden sein.
- Vertrauliche oder regulierte Daten wurden einer unautorisierten Partei zugänglich gemacht.
- Daten wurden an den falschen Empfänger gesendet.
- Ein öffentlicher Link oder eine öffentliche Speicherexposition war aktiv.
- Zugriffslogs zeigen ungewöhnliche Downloads, Exporte oder Abfragen.
- Zugangsdaten, Tokens oder Secrets könnten den Zugriff auf sensible Daten ermöglicht haben.

### Regel 2 — Läuft die Exposition noch?

Die Exposition ist möglicherweise noch aktiv, wenn:

- Öffentliche Links noch aktiv sind.
- Unautorisierte Konten weiterhin Zugriff haben.
- Sessions, Tokens oder API-Keys weiterhin gültig sind.
- Fehlkonfigurierte Berechtigungen weiterhin bestehen.
- Bösartige Aktivitäten weiterhin in Logs oder Alerts sichtbar sind.

### Regel 3 — Haben wir genug Beweise für die Umfangsbewertung?

Wenn nicht:
- Logs und Snapshots sofort sichern.
- Destruktive Änderungen vermeiden.
- System-, Zugriffs- und Audit-Records vor dem Rotieren oder Neuaufsetzen sammeln.
- Den Data Owner identifizieren und mit der Datenklassifizierung beginnen.

### Regel 4 — Ist eine Meldung wahrscheinlich erforderlich?

Datenschutz- und Rechtsstakeholder sofort einbinden, wenn:

- Personenbezogene Daten betroffen sein könnten.
- Regulierte oder besondere Kategorien personenbezogener Daten betroffen sind.
- Zugangsdaten, Finanzdaten, Gesundheitsdaten oder Identitätsdaten offengelegt wurden.
- Die Panne ein Risiko für die Rechte und Freiheiten natürlicher Personen darstellen könnte.
- Rechtliche oder regulatorische Fristen gelten könnten.

### Regel 5 — Können wir jetzt wiederherstellen?

Wiederherstellung nur, wenn:

1. Der Expositionspfad verstanden ist.
2. Unautorisierter Zugriff entfernt wurde.
3. Beweise gesichert sind.
4. Die Meldebewertung begonnen hat.
5. Monitoring und Folgekontrollen eingerichtet sind.

---

## Fokus auf Identität und Zugriff

Viele Datenpannen beginnen mit Missbrauch von Zugängen statt mit reiner Malware. Priorisieren Sie:

- Erfolgreiche und fehlgeschlagene Authentifizierungsereignisse.
- Ungewöhnliche Quell-IP-Adressen, Geolokationen, Geräte oder User Agents.
- Aktivitäten privilegierter Konten.
- Gestohlene Zugangsdaten oder Session-Token-Nutzung.
- Übermäßige Berechtigungen oder Fehlkonfigurationen der Zugriffskontrolle.
- Offenlegung von API-Keys, Service-Account-Zugangsdaten oder Secrets.
- OAuth- oder Applikationsmissbrauch, wenn Datenplattformen über Cloud-Identitäten zugegriffen werden.

---

## Datenexpositionsmuster

Priorisieren Sie diese typischen Expositionsmuster:

- Mailbox-Exposition oder versehentliche E-Mail-Offenlegung.
- Fehlkonfigurierte Cloud-Speicher und öffentliche Freigaben.
- Unautorisierter Zugriff auf File Shares oder Kollaborationsplattformen.
- Datenbank-Exporte, Abfragen oder Dumps.
- Öffentlich exponierte Dokumente oder Speicher-Buckets.
- Verlorene oder gestohlene Geräte mit sensiblen Daten.
- Insider-Diebstahl oder -Missbrauch.
- Drittanbieter- / Lieferantenkompromittierung.
- ransomware-bezogene Datendiebstahl- und Erpressungsszenarien.

---

## Incident-Klassifizierung

Klassifizieren Sie das Ereignis früh als einen oder mehrere der folgenden Typen:

- False Positive.
- Security Event ohne Datenexposition.
- Vermutete Datenpanne.
- Bestätigte Datenpanne.
- Personenbezogene Datenpanne.
- Vertrauliche Geschäftsdatenexposition.
- Regulated Data Exposure.
- Drittanbieter-Breach.
- ransomware-bezogener Datendiebstahl.
- Insider-Datendiebstahl.
- Versehentliche Offenlegung.
- Öffentliche Datenexposition.
- Unautorisierter Zugriff ohne bestätigte Exfiltration.
- Bestätigte Exfiltration.

Die Klassifizierung sollte aktualisiert werden, sobald neue Fakten vorliegen.

---

## Phase 1 — Erkennen & Triage

Ziele:
- Feststellen, ob eine Datenpanne vorliegen könnte.
- Betroffene Daten, Systeme und Benutzer identifizieren.
- Feststellen, ob die Exposition noch aktiv ist.
- Bestimmen, ob personenbezogene, regulierte oder vertrauliche Geschäftsdaten betroffen sind.
- Beweise sichern, die für technische, rechtliche und regulatorische Bewertungen benötigt werden.

Maßnahmen:
- Die Erstmeldung, den Alert oder die Entdeckungsdetails sammeln.
- Dokumentieren, wer das Problem entdeckt hat und wann.
- Dokumentieren, wann die Organisation von der möglichen Datenpanne Kenntnis erlangt hat.
- Das betroffene System, die Anwendung, Mailbox, Datenbank, Dateifreigabe, Cloud-Umgebung oder Drittanbieterplattform identifizieren.
- Den Data Owner oder Business Owner identifizieren.
- Feststellen, ob unautorisierte Parteien noch Zugriff haben.
- Feststellen, ob Vertraulichkeit, Integrität oder Verfügbarkeit von Daten betroffen ist.
- Feststellen, ob Daten eingesehen, kopiert, heruntergeladen, verändert, gelöscht, veröffentlicht oder an unautorisierte Empfänger gesendet wurden.
- Feststellen, ob personenbezogene Daten betroffen sind.
- Feststellen, ob besondere Kategorien personenbezogener Daten betroffen sind.
- Feststellen, ob Zugangsdaten, Finanzdaten, Gesundheitsdaten, HR-Daten, Kundendaten, Lieferantendaten, geistiges Eigentum oder Geschäftsgeheimnisse betroffen sind.
- Die ungefähre Anzahl betroffener Datensätze, Dateien, Mailboxen, Systeme und Personen schätzen.
- Screenshots, URLs, Sharing-Links, Berechtigungen und Metadaten exponierter Inhalte sichern.
- Relevante Logs vor Rotation oder Löschung sichern.
- Eine erste Incident-Timeline erstellen.
- Schweregrad und Klassifizierung zuweisen.

Exit-Kriterien:
- Vorfallsart validiert.
- Anfangsumfang identifiziert.
- Potenziell betroffene Daten und Systeme markiert.
- Beweissicherung begonnen.

---

## Phase 2 — Eindämmen (Contain)

Ziele:
- Laufenden unautorisierten Zugriff oder Offenlegung stoppen.
- Weiteres Kopieren, Herunterladen oder Veröffentlichen verhindern.
- Beweise sichern.
- Geschäftskontinuität, soweit möglich, aufrechterhalten.
- Maßnahmen vermeiden, die forensische oder regulatorische Bewertung beeinträchtigen.

Maßnahmen:
- Öffentlichen Zugriff auf exponierte Speicher, Dateien, Datenbanken, Anwendungen oder URLs entfernen.
- Betroffene Konten deaktivieren oder einschränken, wenn eine Kompromittierung vermutet wird.
- Aktive Sessions und Access Tokens widerrufen.
- Offen gelegte API-Keys, Access Keys oder Secrets deaktivieren.
- Unautorisierte Sharing-Links entfernen.
- Fehlkonfigurierte Berechtigungen korrigieren.
- Bösartige IP-Adressen, Domains oder Konten blockieren, wenn Angreiferaktivität beobachtet wird.
- Betroffene Integrationen oder Drittanbieterzugriffe bei Bedarf deaktivieren.
- Betroffene Endpunkte isolieren, wenn Malware oder Credential Theft beteiligt ist.
- Datensynchronisation aussetzen, wenn sie die unautorisierte Offenlegung propagieren könnte.
- Zugriffslogs, Objekt-Logs, Mailbox-Logs, Datenbank-Logs und Cloud-Audit-Logs sichern.
- Konfigurations-Snapshots vor Änderungen sichern, sofern möglich.
- Kopien oder Hashes exponierter Dateien sichern, sofern angemessen.
- Eindämmungsschritte mit Datenschutz-/Rechtsstakeholdern abstimmen, wenn Beweis- oder Meldeanforderungen betroffen sein könnten.

Spezielle Eindämmung: Exponierter Cloud-Speicher

- Öffentlichen Zugriff sofort deaktivieren.
- Anonyme Links und externe Freigaben entfernen.
- Screenshots der exponierten Konfiguration sichern.
- Zugriffslogs und Objektzugriffsrecords exportieren.
- Alle Objekte identifizieren, die im relevanten Zeitraum exponiert waren.
- Feststellen, ob Objekte eingesehen oder heruntergeladen wurden.
- Access Keys, SAS Tokens, API-Zugangsdaten oder Service-Account-Zugangsdaten rotieren, wenn sie offengelegt wurden.
- Tenantweite Richtlinien für externe Freigaben validieren.

Spezielle Eindämmung: Kompromittierte Mailbox

- Sessions widerrufen.
- Zugangsdaten nach Session-Widerruf zurücksetzen.
- Verdächtige Mailbox-Regeln und Weiterleitungen prüfen und entfernen.
- Delegierten Zugriff prüfen.
- Gesendete, gelöschte und eingesehene Nachrichten prüfen.
- Mailbox-Audit-Logs sichern.
- Feststellen, ob sensible Anhänge oder personenbezogene Daten eingesehen wurden.
- Feststellen, ob bösartige E-Mails an weitere Opfer gesendet wurden.

Spezielle Eindämmung: Datenbank- oder Anwendungs-Exposition

- Exponierten Endpunkt deaktivieren oder Zugriff einschränken.
- Datenbank-Zugangsdaten und API-Secrets rotieren.
- Anwendungs-, Datenbank-, Webserver- und WAF-Logs sichern.
- Feststellen, ob Abfragen, Exporte oder Dumps stattgefunden haben.
- Betroffene Systeme, sofern geeignet, snapshotten.
- Betroffene Tabellen, Felder und Datensätze identifizieren.
- Validieren, ob Ausnutzung automatisiert oder gezielt erfolgte.

Exit-Kriterien:
- Laufende Exposition reduziert oder gestoppt.
- Kompromittierte Konten, Links, Berechtigungen oder Endpunkte gesichert.
- Beweise gesichert.
- Technische und Datenschutz-/Rechtsstakeholder informiert, wo erforderlich.

---

## Phase 3 — Bewerten & Untersuchen (Assess & Investigate)

Ziele:
- Feststellen, was passiert ist.
- Feststellen, welche Daten betroffen waren.
- Feststellen, wer betroffen war.
- Feststellen, wer auf die Daten zugegriffen hat.
- Feststellen, ob Daten kopiert, exfiltriert, verändert, gelöscht oder veröffentlicht wurden.
- Das wahrscheinliche Risiko für betroffene Personen, Kunden, Partner und die Organisation bestimmen.
- Feststellen, ob Meldepflichten bestehen könnten.

### Technische Untersuchung

- Authentifizierungs- und Autorisierungs-Logs prüfen.
- Cloud-Audit-Logs prüfen.
- Dateizugriffs-Logs prüfen.
- Mailbox-Audit-Logs prüfen.
- Datenbank-Audit-Logs und Query-History prüfen.
- Applikations-Logs prüfen.
- Webserver-, Proxy-, Firewall-, DNS- und WAF-Logs prüfen.
- Endpoint-Telemetrie prüfen.
- DLP-Alerts prüfen.
- SIEM-Korrelationen prüfen.
- VPN- und Remote-Access-Logs prüfen.
- Administrative Aktivitäten prüfen.
- Drittanbieter-Zugriffsrecords prüfen.
- Quell-IP-Adressen, User Agents, Konten, Zugangstokens und Geräte identifizieren.
- Feststellen, ob der Zugriff authentifiziert oder anonym war.
- Feststellen, ob der Zugriff intern, extern, durch Drittanbieter oder durch Angreifer kontrolliert war.
- Feststellen, ob der Zugriff automatisiert, massenhaft, gezielt oder versehentlich war.
- Feststellen, ob Logs ausreichend vollständig sind, um Schlussfolgerungen zu stützen.

### Datenbewertung

- Kategorien der betroffenen Daten identifizieren.
- Kategorien betroffener Personen, Kunden, Mitarbeiter, Lieferanten oder Partner identifizieren.
- Ungefähre Anzahl betroffener Personen bestimmen.
- Ungefähre Anzahl betroffener Datensätze bestimmen.
- Feststellen, ob besondere Kategorien personenbezogener Daten betroffen sind.
- Feststellen, ob Kinder, vulnerable Personen oder Hochrisikogruppen betroffen sind.
- Feststellen, ob Zugangsdaten, Authentifizierungsdaten oder Security Tokens betroffen sind.
- Feststellen, ob Finanz-, Zahlungs-, Steuer- oder Identitätsdokumente betroffen sind.
- Feststellen, ob Gesundheits-, biometrische, genetische oder besonders sensible Daten betroffen sind.
- Feststellen, ob vertrauliche Geschäftsinformationen, geistiges Eigentum oder Geschäftsgeheimnisse betroffen sind.
- Feststellen, ob die Daten verschlüsselt, pseudonymisiert, anonymisiert oder durch Zugriffskontrollen geschützt waren.
- Feststellen, ob die exponierten Daten für Identitätsdiebstahl, Betrug, Phishing, Diskriminierung, Reputationsschäden oder finanziellen Verlust missbraucht werden könnten.

### Risikobewertung

Bewerten Sie die wahrscheinlichen Auswirkungen anhand folgender Faktoren:

- Sensibilität der Daten
- Umfang der betroffenen Daten
- Anzahl betroffener Personen
- Wie leicht sich Personen identifizieren lassen
- Ob Daten von einer vertrauenswürdigen oder unvertrauenswürdigen Partei eingesehen wurden
- Ob Daten heruntergeladen oder nur exponiert wurden
- Ob Daten bereits öffentlich sind
- Ob Verschlüsselung oder Pseudonymisierung wirksam war
- Ob Zugangsdaten oder Secrets offengelegt wurden
- Ob finanzieller Betrug, Identitätsdiebstahl oder Phishing möglich ist
- Ob betroffene Personen körperlichen, materiellen oder immateriellen Schaden erleiden könnten
- Ob der Vorfall regulatorische, vertragliche oder geschäftliche Auswirkungen haben könnte
- Ob betroffene Personen Schutzmaßnahmen ergreifen müssen

### Untersuchungsfragen

- Wann war der erste bekannte Zeitpunkt der Exposition?
- Wann wurde die Organisation erstmals von der möglichen Datenpanne in Kenntnis gesetzt?
- Wie wurde die Panne entdeckt?
- Welches System oder welche Datenquelle war betroffen?
- Welche Daten wurden exponiert, eingesehen, kopiert, verändert oder gelöscht?
- Wer hatte unautorisierten Zugriff?
- War der Zugriff authentifiziert oder anonym?
- Wurden Daten heruntergeladen oder exfiltriert?
- Waren die Logs ausreichend, um Zugriff zu beweisen oder auszuschließen?
- Waren die Daten verschlüsselt oder anderweitig geschützt?
- Wurden Zugangsdaten, Tokens oder Secrets offengelegt?
- Sind betroffene Personen identifizierbar?
- Könnten betroffene Personen geschädigt werden?
- Ist eine Meldung an die Aufsichtsbehörde erforderlich?
- Ist eine Benachrichtigung an betroffene Personen erforderlich?
- Sind Kunden, Lieferanten, Versicherer oder Vertragspartner betroffen?
- Werden sektorspezifische oder nationale Cyber-Reporting-Pflichten ausgelöst?

Exit-Kriterien:
- Was passiert ist, hinreichend verstanden.
- Datenarten und betroffene Parteien identifiziert.
- Risikoniveau bewertet.
- Entscheidungsfindung zur Meldung kann erfolgen.

---

## Phase 4 — Benachrichtigen & Kommunizieren (Notify & Communicate)

Ziele:
- Rechtliche, regulatorische, vertragliche und organisatorische Pflichten erfüllen.
- Genau und konsistent kommunizieren.
- Vorzeitige oder unbelegte Aussagen vermeiden.
- Betroffenen, wo erforderlich, sinnvolle Schutzhinweise geben.

### Interne Eskalation

- Executive Management benachrichtigen, wenn der Schweregrad Hoch oder Kritisch ist.
- Datenschutz-/Rechtsstakeholder sofort benachrichtigen, wenn personenbezogene Daten betroffen sein könnten.
- Data Owner und betroffene Geschäftsbereiche benachrichtigen.
- IT, Security, Communications und Customer Support benachrichtigen.
- Lieferanten-/Vendor-Management benachrichtigen, wenn eine Drittpartei beteiligt ist.
- Versicherungskontakt benachrichtigen, wenn Police-Anforderungen dies verlangen.
- Strafverfolgung benachrichtigen, wo angemessen und genehmigt.

### Regulatorische Bewertung

Für DSGVO-relevante personenbezogene Datenpannen:

- Feststellen, ob der Vorfall wahrscheinlich kein Risiko für die Rechte und Freiheiten natürlicher Personen darstellt.
- Wenn ein Risiko wahrscheinlich ist, eine Meldung an die zuständige Aufsichtsbehörde vorbereiten.
- Die 72-Stunden-Meldefrist ab dem Zeitpunkt der Kenntnisnahme verfolgen.
- Wenn die Meldung innerhalb von 72 Stunden nicht abgeschlossen werden kann, die Gründe für die Verzögerung dokumentieren.
- Wenn Informationen unvollständig sind, eine Erstmeldung einreichen und weitere Informationen gegebenenfalls in Phasen nachreichen.
- Wenn die Panne voraussichtlich ein hohes Risiko für betroffene Personen darstellt, eine Benachrichtigung der Betroffenen unverzüglich vorbereiten.
- Die Gründe für Meldung oder Nichtmeldung dokumentieren.

### Inhalt der Meldung an die Aufsichtsbehörde

Wo erforderlich, bereiten Sie folgende Informationen vor:

- Art der personenbezogenen Datenpanne
- Kategorien und ungefähre Anzahl betroffener Personen
- Kategorien und ungefähre Anzahl betroffener Datensätze
- Name und Kontaktdaten des Datenschutzbeauftragten oder relevanten Ansprechpartners
- Wahrscheinliche Folgen der Panne
- Getroffene oder geplante Maßnahmen zur Behebung der Panne
- Getroffene Maßnahmen zur Minderung möglicher nachteiliger Auswirkungen
- Timeline von Entdeckung, Kenntniserlangung, Eindämmung und Untersuchung
- Ob betroffene Personen bereits informiert wurden oder informiert werden
- Gründe für eine verspätete Meldung, falls zutreffend

### Kommunikation an betroffene Personen

Wo erforderlich, sollte die Kommunikation klar, verständlich und praktisch sein.

Enthalten sein sollten:

- Was passiert ist
- Wann es passiert ist oder wann es entdeckt wurde
- Welche Daten möglicherweise betroffen sind
- Welche Risiken daraus entstehen könnten
- Was die Organisation unternommen hat
- Was betroffene Personen tun können, um sich zu schützen
- Kontaktdaten für Rückfragen
- Spekulationen über Identität des Angreifers, exakten Umfang oder Impact vermeiden, sofern nicht bestätigt

### Grundsätze externer Kommunikation

- Nur bestätigte Fakten kommunizieren.
- Bestätigte Fakten von der laufenden Untersuchung trennen.
- Verharmlosende Formulierungen vermeiden, wenn das Risiko noch bewertet wird.
- Keine übertriebene Gewissheit darstellen.
- Alle Nachrichten mit Datenschutz-/Rechts- und Kommunikationsstakeholdern abstimmen.
- Vertrauenswürdige Kommunikationskanäle verwenden.
- Alle externen Kommunikationen und Freigaben aufbewahren.

---

## Phase 5 — Beheben & Wiederherstellen (Remediate & Recover)

Ziele:
- Die Ursache beseitigen.
- Sicheren Betrieb wiederherstellen.
- Wiederholungen verhindern.
- Bestätigen, dass die Exposition beendet ist.
- Schutzmaßnahmen für Betroffene implementieren.

### Technische Remediation

- Fehlkonfigurationen der Zugriffskontrolle korrigieren.
- Unautorisierte Konten, Sessions, Tokens und Berechtigungen entfernen.
- Offen gelegte Zugangsdaten, API-Keys, Zertifikate und Secrets rotieren.
- Ausgenutzte Schwachstellen patchen.
- Betroffene Anwendungen, Datenbanken, Mailboxen oder Cloud-Services härten.
- Verschlüsselung einführen oder verbessern.
- Least-Privilege-Zugriff implementieren.
- Anonyme oder öffentliche Zugriffe deaktivieren, sofern nicht erforderlich.
- Externe Freigaben einschränken.
- Detailliertes Audit-Logging aktivieren oder verbessern.
- Monitoring für risikoreiche Datenzugriffe verbessern.
- DLP-Regeln dort implementieren, wo sinnvoll.
- Backup-Integrität prüfen, wenn Daten verändert oder gelöscht wurden.
- Veränderte oder gelöschte Daten aus sauberen Backups wiederherstellen, falls erforderlich.
- Nach der Remediation gezielte Sicherheitstests durchführen.

### Organisatorische Remediation

- Verfahren für den Umgang mit Daten aktualisieren.
- Prozesse für Zugriffsprüfungen aktualisieren.
- Lieferantenmanagement-Kontrollen aktualisieren.
- Verfahren zur Incident-Eskalation aktualisieren.
- Datenklassifizierungs- und Ownership-Records aktualisieren.
- Zielgerichtete Schulungen für Benutzer oder Administratoren durchführen.
- Freigabeprozesse für Datenteilung prüfen.
- Aufbewahrungs- und Minimierungspraktiken für Daten prüfen.
- Verträge und Pflichten von Auftragsverarbeitern prüfen, sofern relevant.

### Validierung der Wiederherstellung

- Bestätigen, dass der unautorisierte Zugriff beendet ist.
- Bestätigen, dass exponierte Links oder Berechtigungen entfernt wurden.
- Bestätigen, dass Zugangsdaten und Secrets rotiert wurden.
- Bestätigen, dass betroffene Systeme überwacht werden.
- Bestätigen, dass relevante Logs aufbewahrt werden.
- Bestätigen, dass Meldepflichten adressiert wurden.
- Bestätigen, dass betroffene Geschäftsprozesse sicher wieder aufgenommen werden können.
- Für mindestens 14 bis 30 Tage verstärktes Monitoring fortsetzen.

---

## Phase 6 — Review / Lessons Learned

Ziele:
- Die Ursache verstehen.
- Technische und organisatorische Kontrollen verbessern.
- Die Einhaltung interner und externer Pflichten validieren.
- Remediation-Maßnahmen bis zum Abschluss nachverfolgen.

### Maßnahmen

- Innerhalb von 5 bis 10 Geschäftstagen eine Post-Incident-Review durchführen.
- Die Incident-Timeline finalisieren.
- Die Root Cause dokumentieren.
- Betroffene Daten, Systeme, Benutzer und Drittparteien dokumentieren.
- Alle Eindämmungs-, Remediation- und Kommunikationsmaßnahmen dokumentieren.
- Meldungsentscheidungen und deren Begründung dokumentieren.
- Prüfen, ob Logs ausreichend waren.
- Prüfen, ob Datenklassifizierung und Ownership klar waren.
- Prüfen, ob Zugriffskontrollen dem Least-Privilege-Prinzip entsprachen.
- Prüfen, ob die Erkennung früh genug erfolgte.
- Prüfen, ob die Eskalation an Datenschutz/Recht rechtzeitig erfolgte.
- Prüfen, ob betroffene Personen oder Behörden angemessen benachrichtigt wurden.
- Richtlinien, Verfahren, Playbooks und Schulungen aktualisieren.
- Alle Remediation-Maßnahmen bis zum Abschluss nachverfolgen.

### Lessons-Learned-Fragen

- Warum wurden die Daten exponiert oder eingesehen?
- War die Root Cause technisch, prozessual, menschlich oder drittanbieterbedingt?
- Waren die Daten korrekt klassifiziert?
- Wurden die Daten noch benötigt oder hätten sie früher gelöscht werden sollen?
- Waren die Berechtigungen zu weit gefasst?
- Wurden Sharing-Links oder öffentliche Zugriffskontrollen missbraucht?
- Wurden Zugangsdaten oder Secrets offengelegt?
- Waren die Logs ausreichend, um Zugriff und Impact zu bestimmen?
- War Verschlüsselung wirksam?
- Hat Monitoring das Problem entdeckt oder hat eine Drittpartei es gemeldet?
- War die Eskalation an Datenschutz/Recht rechtzeitig?
- Wurden Meldefristen eingehalten?
- War die Kommunikation korrekt und konsistent?
- Welche Kontrollen hätten den Impact verhindert oder reduziert?

---

## Zu sichernde Beweise

Sichern Sie, wo verfügbar, folgende Beweise:

### Allgemeine Beweise

- Erstmeldung oder Alert.
- Incident-Timeline.
- Screenshots exponierter Daten oder Konfigurationen.
- URLs und Zugriffspfade.
- Dateinamen, Objektnamen, Datenbanknamen und Mailbox-Namen.
- Hashes relevanter Dateien, sofern angemessen.
- System- und Anwendungs-Konfigurations-Snapshots.
- Liste betroffener Benutzer, Systeme, Datasets und Business Owner.
- Entscheidungsprotokolle und Freigabeaufzeichnungen.

### Zugriffs- und Audit-Beweise

- Authentifizierungs-Logs.
- Autorisierungs-Logs.
- Cloud-Audit-Logs.
- Dateizugriffs-Logs.
- Datenbank-Audit-Logs.
- Query-History.
- Webserver-Logs.
- WAF-Logs.
- Firewall-Logs.
- Proxy-Logs.
- VPN-Logs.
- DNS-Logs.
- Mailbox-Audit-Logs.
- DLP-Alerts.
- SIEM-Events.
- EDR-Alerts.
- API-Gateway-Logs.
- Logs von Drittanbieterplattformen.

### Datenbezogene Beweise

- Kategorien betroffener Daten.
- Ungefähre Anzahl der Datensätze.
- Ungefähre Anzahl betroffener Personen.
- Bestätigung durch den Data Owner.
- Datenklassifizierung.
- Nachweise für Verschlüsselung, Pseudonymisierung oder Anonymisierung.
- Nachweise für Zugriff, Download, Export, Löschung, Veränderung oder Veröffentlichung.
- Kopien exponierter E-Mails, Dateien oder Datenbank-Exports, sofern rechtlich zulässig.

### Kommunikations- und Rechtsbeweise

- Interne Eskalationsaufzeichnungen.
- Notizen der Datenschutz-/Rechtsbewertung.
- Meldungen an Aufsichtsbehörden.
- Kommunikation mit Betroffenen.
- Kommunikation mit Kunden oder Lieferanten.
- Meldungen an Drittparteien.
- Kommunikation mit Cyber-Versicherern.
- Kommunikation mit Strafverfolgung.
- Gründe für Meldung oder Nichtmeldung.
- Gründe für eine verspätete Meldung, falls zutreffend.

---

## Kommunikations-Checkliste

### Interne Kommunikation

- Incident-Response-Team benachrichtigen.
- Datenschutz-/Rechtsstakeholder benachrichtigen.
- Data Owner benachrichtigen.
- Management benachrichtigen, wenn der Schweregrad Hoch oder Kritisch ist.
- Betroffene Geschäftsbereiche benachrichtigen.
- Kommunikations-/Customer-Support-Teams benachrichtigen, wenn externer Kontakt möglich ist.
- Lieferantenmanagement benachrichtigen, wenn eine Drittpartei beteiligt ist.
- Interne Updates sachlich und versioniert halten.
- Eine einzige Quelle der Wahrheit pflegen.

### Externe Kommunikation

- Prüfen, ob eine Meldung an die Aufsichtsbehörde erforderlich ist.
- Prüfen, ob betroffene Personen informiert werden müssen.
- Prüfen, ob Kunden, Lieferanten oder Vertragspartner informiert werden müssen.
- Prüfen, ob Versicherer informiert werden müssen.
- Prüfen, ob Strafverfolgung sinnvoll ist.
- Externe Stellungnahmen durch Datenschutz/Recht und Kommunikation vorbereiten.
- Alle ausgehenden Mitteilungen aufbewahren.
- Klare, verständliche und praktische Sprache verwenden.

### Vorgeschlagene interne Zwischenmeldung

Eine vermutete Datenpanne wird derzeit untersucht. Das Incident-Response-, Datenschutz-/Rechts- und die relevanten technischen Teams bewerten Umfang, betroffene Daten, Eindämmungsstatus und mögliche Meldepflichten. Bitte spekulieren Sie nicht extern und löschen Sie keine potenziell relevanten Beweise. Weitere Updates werden über den vorgesehenen Incident-Kommunikationskanal bereitgestellt.

### Vorgeschlagene externe Zwischenmeldung

Wir untersuchen derzeit einen Sicherheitsvorfall mit möglichem unautorisierten Zugriff auf Daten. Unsere Teams haben sofort Maßnahmen zur Eindämmung ergriffen und arbeiten mit relevanten Experten daran, Umfang und Auswirkungen zu bestimmen. Wir werden betroffene Parteien bei Bedarf und im Einklang mit geltenden Verpflichtungen weiter informieren.

---

## Rechtliche und regulatorische Aspekte

Datenpannen können rechtliche, regulatorische, vertragliche, branchenspezifische oder versicherungsbezogene Pflichten auslösen.

Bei DSGVO-relevanten Vorfällen sollten Datenschutz-/Rechtsstakeholder sofort eingebunden werden. Das Response-Team sollte die rechtliche Bewertung unterstützen, indem es Beweise sichert, Timelines dokumentiert und verifizierte Fakten bereitstellt.

Wesentliche Aspekte sind:

- Ob eine personenbezogene Datenpanne vorliegt.
- Ob die Panne wahrscheinlich ein Risiko für die Rechte und Freiheiten natürlicher Personen darstellt.
- Ob eine Meldung an die Aufsichtsbehörde erforderlich ist.
- Ob betroffene Personen informiert werden müssen.
- Ob die Meldung innerhalb einer bestimmten Frist erfolgen muss.
- Ob vertragliche Meldepflichten gegenüber Kunden oder Lieferanten bestehen.
- Ob sektorspezifische Cyber-Reporting-Pflichten bestehen.
- Ob eine Benachrichtigung von Strafverfolgung oder Cyber-Versicherern erforderlich ist.
- Ob eine grenzüberschreitende Koordination mit Aufsichtsbehörden relevant ist.
- Ob Pflichten für Auftragsverarbeiter bzw. Verantwortliche gelten.

Dieses Playbook stellt keine Rechtsberatung dar. Es soll Beweissicherung, strukturierte Response und eine fundierte Datenschutz-/Rechtsbewertung unterstützen.

---

## Quick-Response-Checkliste

### Sofortreaktion

- [ ] Incident Lead zuweisen.
- [ ] Datenschutz/Recht benachrichtigen, wenn personenbezogene Daten betroffen sein könnten.
- [ ] Betroffenes System, Dataset und Owner identifizieren.
- [ ] Laufende Exposition stoppen.
- [ ] Logs und Screenshots sichern.
- [ ] Discovery- und Awareness-Zeitpunkte dokumentieren.
- [ ] Incident-Timeline starten.
- [ ] Verdachts- und Bestätigungsstatus klassifizieren.

### Umfang

- [ ] Betroffene Systeme identifizieren.
- [ ] Betroffene Daten identifizieren.
- [ ] Betroffene Personen oder Organisationen identifizieren.
- [ ] Anzahl der Datensätze bestimmen.
- [ ] Feststellen, ob Daten eingesehen wurden.
- [ ] Feststellen, ob Daten heruntergeladen oder exfiltriert wurden.
- [ ] Feststellen, ob Daten verändert oder gelöscht wurden.
- [ ] Feststellen, ob Daten öffentlich waren.
- [ ] Feststellen, ob die Logs ausreichend sind.

### Eindämmung

- [ ] Öffentlichen Zugriff entfernen.
- [ ] Sessions und Tokens widerrufen.
- [ ] Kompromittierte Konten deaktivieren.
- [ ] Unautorisierte Sharing-Links entfernen.
- [ ] Berechtigungen korrigieren.
- [ ] Offen gelegte Zugangsdaten und Secrets rotieren.
- [ ] Angreifer-Infrastruktur blockieren.
- [ ] Beweise vor destruktiven Änderungen sichern.

### Bewertung

- [ ] Datensensibilität bestimmen.
- [ ] Betroffene Datensubjekte bestimmen.
- [ ] Wahrscheinlichen Schaden bestimmen.
- [ ] Rechtliche/regulatorische Auswirkungen bestimmen.
- [ ] Meldepflichten bestimmen.
- [ ] Meldungs- oder Nichtmeldungsentscheidung dokumentieren.

### Meldung

- [ ] Bei Bedarf Meldung an die Aufsichtsbehörde vorbereiten.
- [ ] DSGVO-72-Stunden-Fenster verfolgen, sofern anwendbar.
- [ ] Bei hohem Risiko Kommunikation an betroffene Personen vorbereiten.
- [ ] Bei Bedarf Kommunikation an Kunden, Lieferanten oder Partner vorbereiten.
- [ ] Alle Meldungsunterlagen aufbewahren.

### Remediation

- [ ] Root Cause beheben.
- [ ] Schwachstellen patchen.
- [ ] Zugriffskontrollen härten.
- [ ] Logging verbessern.
- [ ] Least Privilege implementieren.
- [ ] Datenklassifizierung verbessern.
- [ ] Externe Freigabekontrollen verbessern.
- [ ] Verfahren und Schulungen aktualisieren.

### Wiederherstellung

- [ ] Bestätigen, dass die Exposition beendet ist.
- [ ] Bestätigen, dass Kontrollen korrigiert wurden.
- [ ] Bestätigen, dass Monitoring aktiv ist.
- [ ] Bestätigen, dass Meldungen bearbeitet sind.
- [ ] Für 14 bis 30 Tage verstärktes Monitoring fortsetzen.
- [ ] Remediation bis zum Abschluss nachverfolgen.

---

## Präventive Kontrollen

### Daten-Governance

- Ein Dateninventar pflegen.
- Data Owner zuweisen.
- Sensible Daten klassifizieren.
- Aufbewahrungs- und Löschregeln anwenden.
- Gespeicherte personenbezogene und sensible Daten minimieren.
- Zugriffe regelmäßig überprüfen.
- Verarbeitungstätigkeiten dokumentieren, sofern erforderlich.

### Zugriffskontrolle

- Least Privilege durchsetzen.
- MFA für administrative und risikoreiche Zugriffe verwenden.
- Externe Freigabeberechtigungen überprüfen.
- Anonyme Links einschrä
