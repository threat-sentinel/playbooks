# Ransomware-Angriff-Playbook

## Zusammenfassung

Dieses Playbook bietet eine praxisnahe, community-orientierte Reaktionsstruktur für vermutete oder bestätigte Ransomware-Vorfälle. Es soll Incident-Respondern helfen, unter Druck schnelle und belastbare Entscheidungen zu treffen, weiteren Schaden zu begrenzen, Beweise zu sichern, Identitäts- und Backup-Infrastrukturen zu schützen und Geschäftsservices nur aus vertrauenswürdigen Zuständen wiederherzustellen.

Das Playbook folgt einer etablierten Incident-Response-Logik entlang von Vorbereitung, Erkennung und Analyse, Eindämmung, Beseitigung, Wiederherstellung und Nachbereitung. Es ist bewusst für die operative Nutzung durch Security-Teams, IT-Administratoren, Berater, Incident Responder sowie kleine und mittlere Organisationen formuliert.

Dieses Dokument ersetzt weder Rechtsberatung noch forensische Expertise, Cyber-Versicherungsanforderungen oder organisationsspezifische Incident-Response-Pläne. Es sollte an die lokale Architektur, kritische Services, rechtliche Verpflichtungen und benannte Verantwortlichkeiten angepasst werden.

## Zweck

Bereitstellung eines strukturierten Reaktionsprozesses für bestätigte oder vermutete Ransomware-Vorfälle, einschließlich:

- Früherkennung und Validierung.
- Schneller Eindämmung bei minimalem zusätzlichem Schaden.
- Beweissicherung für technische, rechtliche und regulatorische Zwecke.
- Scope-Analyse über Endpunkte, Server, Identitäten, Cloud und Backups hinweg.
- Sicherer Beseitigung und vertrauenswürdiger Wiederherstellung.
- Dokumentierter Lessons Learned und Verbesserungsmaßnahmen.

## Anwendungsbereich

Verwenden Sie dieses Playbook, wenn einer oder mehrere der folgenden Indikatoren beobachtet werden:

- Dateien werden unerwartet verschlüsselt, umbenannt oder unzugänglich gemacht.
- Lösegeldforderungen werden auf Endpunkten, Servern, virtuellen Maschinen oder Dateifreigaben gefunden.
- Benutzer melden unzugängliche Dateien, Anwendungen oder Systeme mit Anzeichen einer Massenverschlüsselung.
- Sicherheitstools erkennen Ransomware-Verhalten, massenhafte Dateimodifikationen, das Löschen von Shadow Copies, Backup-Manipulationen, verdächtige geplante Aufgaben oder die Unterdrückung von Sicherheitskontrollen.
- Backup-Systeme, Domänencontroller, Hypervisoren, Dateiserver oder Management-Konsolen zeigen verdächtige administrative Aktivitäten.
- Es gibt Anzeichen für Datendiebstahl, Erpressung, Leak-Site-Drohungen oder unautorisierte großflächige Datenübertragungen.
- Die Aktivität eines Bedrohungsakteurs deutet auf eine Ransomware-Vorphase hin, auch wenn die Verschlüsselung noch nicht begonnen hat.

Dieses Playbook gilt auch für Vorläuferereignisse, die häufig vor einem Ransomware-Rollout auftreten, zum Beispiel ein domänenweites Ausrollen bösartiger Tools, massenhafter Missbrauch von Zugangsdaten, der Missbrauch von Remote-Administration, weitreichende Manipulation von Security-Tools oder gleichzeitige Privilegieneskalation auf mehreren Systemen.

## Schweregrad

Standard-Schweregrad: **Kritisch**.

Sofort eskalieren, wenn eines der folgenden Systeme betroffen oder verdächtig ist:

- Domänencontroller oder zentrale Identitätssysteme.
- Backup-Server, Backup-Repositories, unveränderbare Speicher oder Replikationskontrollen.
- Hypervisoren, Virtualisierungsmanagement oder Speicheradministration.
- Dateiserver, ERP-, Finanz-, Produktions-, Logistik- oder andere geschäftskritische Systeme.
- Identitätsprovider, SSO, MFA, VPN oder Remote-Administrationssysteme.
- EDR, Logging, SIEM oder andere zentrale Sicherheitstools.
- Cloud-Tenant-Administration oder privilegierte SaaS-Administration.
- Jegliche Hinweise oder glaubhafte Verdachtsmomente auf Datenexfiltration.

## Incident-Ziele

Während der ersten Stunden eines Ransomware-Vorfalls sollte das Incident-Response-Team auf sechs operative Ziele hinarbeiten:

1. Die Ausbreitung stoppen.
2. Beweise und Entscheidungsqualität sichern.
3. Identitäten, Backups und Management-Ebenen schützen.
4. Scope, Eintrittsvektor und Angreiferpfad bestimmen.
5. Kritische Services nur aus vertrauenswürdigen Zuständen wiederherstellen.
6. Den Vorfall mit Lessons Learned und nachverfolgten Verbesserungen abschließen.

## Zentrale Reaktionsprinzipien

- Beweise vor destruktiver Bereinigung sichern.
- Erst eindämmen, dann neu aufbauen.
- Systeme nicht in eine nicht vertrauenswürdige Umgebung zurückspielen.
- Von einer möglichen Identitätskompromittierung ausgehen, bis das Gegenteil bewiesen ist.
- Backups so früh wie möglich schützen.
- Bestätigte Fakten von Annahmen und Arbeitshypothesen trennen.
- Eine einzige Incident-Timeline und ein zentrales Entscheidungsprotokoll führen.
- Bei wesentlicher Kompromittierung einem vertrauenswürdigen Neuaufbau den Vorzug gegenüber partieller Bereinigung geben.
- Geschäftlicher Druck darf keine Recovery-Entscheidungen ohne Vertrauensbasis erzwingen.

## Die ersten 15 Minuten

- Den Incident-Response-Lead oder Incident Commander aktivieren.
- Eine Incident-Timeline und ein Entscheidungsprotokoll starten.
- Bestätigen, ob Verschlüsselung oder Angreiferaktivität noch aktiv ist.
- Aktiv betroffene Systeme nach Möglichkeit vom Netzwerk isolieren.
- Systeme nicht ausschalten, es sei denn, die Verschlüsselung breitet sich aktiv aus und es gibt keine sicherere Isolationsmöglichkeit.
- Sofort verfügbare Beweise sichern: Alerts, Screenshots, Lösegeldforderungen, Zeitstempel, Hostnamen, Benutzernamen und Systemkontext.
- Potenziell betroffene Netzwerksegmente, Benutzer, Geschäftsservices, Server und Dateifreigaben identifizieren.
- Backups sofort schützen, indem der administrative Zugriff eingeschränkt, Lösch- und Manipulationspfade gestoppt und unveränderbare oder Offline-Kopien geprüft werden.
- Verdächtige privilegierte Konten überprüfen, insbesondere Backup-Admins, Domänen-Admins, Tenant-Admins und Administratoren für Remote-Zugänge.
- Management, Legal/Privacy-Stakeholder und Krisenkommunikationskontakte gemäß Vorgabe informieren.

## Rollen und Verantwortlichkeiten

### Incident Commander

- Verantwortet die Gesamtkoordination und Entscheidungen zum Schweregrad.
- Genehmigt wesentliche Eindämmungs- und Wiederherstellungsmaßnahmen.
- Stellt ein gemeinsames Lagebild und ein zentrales Entscheidungsprotokoll sicher.
- Koordiniert das Reporting an das Management.

### Technische Leitung

- Koordiniert Endpunkt-, Server-, Netzwerk-, Identitäts-, Cloud- und Backup-Responder.
- Pflegt die technische Arbeitshypothese.
- Steuert die Prioritäten für Eindämmung, Untersuchung und Beseitigung.

### Leitung Identität und Zugriff

- Prüft Missbrauch privilegierter Konten, neue Admin-Konten, Token-/Session-Missbrauch, MFA-Status und Remote-Zugangspfade.
- Koordiniert Credential-Resets und die Invalidierung von Sessions in der richtigen Reihenfolge.

### Leitung Infrastruktur und Wiederherstellung

- Koordiniert Systemisolation, Neuaufbau, Restore-Reihenfolge und Recovery-Validierung.
- Arbeitet mit Server-, Virtualisierungs-, Storage- und Backup-Teams zusammen.

### Forensik-Leitung

- Sichert und erhebt Beweise.
- Validiert Angreiferaktivität, Persistenz und Indicators of Compromise.
- Berät dazu, wann Systeme aus Beweisgründen eingeschaltet bleiben sollten.

### Kommunikationsleitung

- Koordiniert interne, kundenbezogene, partnerbezogene, regulatorische und öffentliche Kommunikation.
- Verhindert nicht abgestimmte oder inkonsistente Aussagen.

### Legal / Privacy / Compliance

- Bewertet rechtliche, vertragliche, regulatorische und datenschutzrechtliche Verpflichtungen.
- Koordiniert gegebenenfalls notwendige Meldungen.
- Berät zu Anforderungen bei Beweissicherung und externer Kommunikation.

### Management / Krisenteam

- Entscheidet über Business-Continuity-Prioritäten, externe Unterstützung, wesentliche operative Abwägungen und Eskalationspfade.

### Externe IR / Strafverfolgung / Versicherung

- Werden gemäß Unternehmensrichtlinie, Cyber-Versicherungsanforderungen und Incident-Schweregrad eingebunden.

## Entscheidungsmodell

### Entscheidungsregel 1: Isolieren oder beobachten?

**Sofort isolieren**, wenn einer der folgenden Punkte zutrifft:

- Die Verschlüsselung ist aktiv.
- Ein System verbreitet bösartige Aktivität auf Freigaben oder andere Hosts.
- Ein privilegiertes System zeigt verdächtige Remote-Administration oder Manipulation von Security-Tools.
- Das betroffene Asset ist geschäftskritisch und das Vertrauen in eine Kompromittierung ist hoch.

**Kurzzeitig beobachten, bevor aggressiv isoliert wird**, nur wenn:

- Keine Anzeichen für aktuell destruktive Aktivität bestehen.
- Eine sofortige Abschaltung oder Isolation die Beweissicherung eines hochkritischen Systems erheblich beeinträchtigen würde.
- Incident Commander und Forensik-Leitung dem ausdrücklich zustimmen.

### Entscheidungsregel 2: Ausschalten oder eingeschaltet lassen?

**Eingeschaltet lassen**, wenn:

- Speicherforensik benötigt wird.
- Eine EDR-Isolation verfügbar ist.
- Die Verschlüsselung nicht mehr aktiv fortschreitet.
- Forensischer Kontext durch ein sofortiges Ausschalten verloren ginge.

**Nur ausschalten, wenn**:

- Eine Netzwerkisolation nicht möglich ist.
- Die Verschlüsselung sich aktiv ausbreitet und erheblichen Schaden verursacht.
- Der Host Backup-Zugriffe zerstört, Shared Storage massenhaft verschlüsselt oder sicherheitskritische Betriebsabläufe gefährdet.

### Entscheidungsregel 3: Bereinigen oder neu aufbauen?

**Neuaufbau bevorzugen**, wenn:

- Administrative Zugriffe erlangt wurden.
- Security-Tools deaktiviert oder manipuliert wurden.
- Persistenz nicht mit hinreichender Sicherheit ausgeschlossen werden kann.
- Das System internetexponiert, privilegiert oder geschäftskritisch ist.
- Der Kompromittierungspfad nicht vollständig verstanden wurde.

**Bereinigung kann vertretbar sein** nur bei Low-Value-Systemen mit hoher Transparenz und klar begrenztem Scope.

### Entscheidungsregel 4: Ist Recovery jetzt erlaubt?

Recovery sollte erst beginnen, wenn alle vier Bedingungen erfüllt sind:

1. Der wahrscheinliche Eintrittsvektor ist geschlossen oder kompensierende Maßnahmen sind aktiv.
2. Hochriskante Zugangsdaten und Sessions wurden zurückgesetzt oder widerrufen.
3. Die Backup- oder Image-Quelle ist vertrauenswürdig.
4. Monitoring- und Containment-Kontrollen sind in der wiederhergestellten Umgebung aktiv.

## Priorisierte Schutzbereiche

### Identity First

Behandeln Sie Identitäten während der Ransomware-Reaktion als Kronjuwelen-Funktion. Viele Ransomware-Vorfälle werden zunächst zu Identitätsvorfällen, bevor sie zu Verschlüsselungsvorfällen werden.

Priorisierte Prüfungen:

- Aktivitäten von Domain Admins, Enterprise Admins, Backup-Admins und Tenant-Admins.
- Neu angelegte privilegierte Konten oder Gruppenänderungen.
- Deaktivierung, Neuregistrierung oder Umgehung von MFA.
- Neue OAuth-Freigaben, Service Principals, API-Tokens oder verdächtige Föderationsänderungen.
- VPN-, RDP-, Jump-Host- und Remote-Management-Zugänge.
- Session-/Token-Persistenz in Cloud- und SaaS-Services.

### Schutz von Backup und Recovery

Backups sind nicht nur Recovery-Assets; sie sind aktive Angriffsziele.

Priorisierte Maßnahmen:

- Administrativen Zugriff auf Backup-Systeme und Repositories einschränken.
- Backup-Logs und administrative Aktionen sichern.
- Löschversuche, fehlgeschlagene Jobs, Retention-Änderungen und den Status unveränderbarer Speicher prüfen.
- Vertrauenswürdige Recovery-Administration von potenziell kompromittierten Identitäten trennen.
- Nicht davon ausgehen, dass ein verfügbares Backup automatisch ein sicheres Backup ist.

## Phase 1: Erkennen & Triage (Detect & Triage)

### Ziele

- Bestätigen, ob es sich um Ransomware, Datenerpressung, destruktive Malware oder einen anderen Incident-Typ handelt.
- Feststellen, ob Angreiferaktivität noch aktiv ist.
- Den ersten bekannten Scope identifizieren.
- Die Beweise sichern, die am schnellsten verloren gehen.

### Maßnahmen

- Erste Meldungen von Benutzern, Helpdesk, EDR, SIEM, Firewall, E-Mail-Gateway, Remote-Access-Systemen und Backup-Tools sammeln.
- Patient-Zero-Kandidaten oder zuerst betroffene Assets identifizieren.
- Den ersten bekannten Zeitstempel und die Entdeckungsquelle erfassen.
- Nach Lösegeldforderungen, verschlüsselten Dateiendungen, verdächtigen geplanten Aufgaben, Manipulation von Security-Tools und anomalen Prozessen suchen.
- Bestimmen, ob die Verschlüsselung lokal, über Dateifreigaben, serverseitig oder auf Hypervisor-/Storage-Ebene stattfindet.
- Identitätssysteme auf verdächtige Logins, Privilegieneskalation, neu angelegte Admin-Konten und MFA-Änderungen prüfen.
- VPN, RDP, Remote-Monitoring-Tools, Remote-Support-Software und Cloud-Admin-Zugänge überprüfen.
- Auf Anzeichen für Datenstaging, große ausgehende Transfers oder exfiltrationsbezogene Erpressung prüfen.
- Eine erste Incident-Timeline erstellen und pflegen.

### Exit-Kriterien

Zur formalen Eindämmung übergehen, wenn alle folgenden Punkte zumindest vorläufig definiert sind:

- Der Incident-Typ ist ausreichend validiert.
- Das Risiko einer aktiven Ausbreitung ist eingeschätzt.
- Erste betroffene Systeme und Konten sind identifiziert.
- Schutzmaßnahmen für Backup und Identität wurden gestartet.

## Phase 2: Eindämmen (Contain)

### Ziele

- Weitere Ausbreitung stoppen.
- Kronjuwelen-Systeme schützen.
- Recovery-Optionen sichern.
- Vermeidbare Zerstörung von Beweisen verhindern.

### Maßnahmen

- Betroffene Endpunkte und Server vom Netzwerk isolieren.
- Lateral-Movement-Pfade wie SMB, RDP, WinRM, WMI, PSExec, SSH und Remote-Support-Tools je nach Lage einschränken.
- Kompromittierte oder verdächtige Konten deaktivieren, beginnend mit privilegierten Identitäten, sofern die Beweislage dies stützt.
- Verdächtige Sessions und Tokens widerrufen.
- Bekannte bösartige Indicators blockieren, wenn die Zuverlässigkeit hoch genug ist.
- VPN- und Remote-Administrationszugänge vorübergehend einschränken, wenn der Angriffsweg noch nicht verstanden ist.
- Kritische Server oder Segmente isolieren, wenn dort verdächtige Aktivität festgestellt wird.
- Backup-Infrastruktur vor Löschung, Verschlüsselung, Retention-Änderung oder Manipulation der Replikation schützen.
- Synchronisation zu Cloud-Speichern aussetzen, wenn verschlüsselte oder bösartige Daten repliziert werden.
- Forensische Images oder Memory Captures für ausgewählte High-Value-Systeme sichern.

### Nicht tun

- Nicht sofort alle betroffenen Systeme löschen oder neu aufsetzen.
- Systeme nicht in eine weiterhin kompromittierte Umgebung zurückspielen.
- Lösegeldforderungen, Skripte, Tools oder Artefakte nicht vor der Sicherung löschen.
- Nicht annehmen, dass der Incident auf sichtbar verschlüsselte Systeme begrenzt ist.
- Nicht alle Zugangsdaten gleichzeitig ohne kontrollierte Reihenfolge zurücksetzen.
- Nicht zulassen, dass nicht betroffene Administratoren potenziell kompromittierte Admin-Workstations weiterverwenden.

### Exit-Kriterien

Zur vertieften Untersuchung übergehen, wenn:

- Die aktive Ausbreitung reduziert oder gestoppt ist.
- High-Risk-Assets abgeschirmt sind.
- Backup- und Identity-Kontrollen aktiv überprüft werden.
- Genügend Stabilität besteht, um ein belastbares Bild des Scopes aufzubauen.

## Phase 3: Untersuchen (Investigate)

### Ziele

- Den Eintrittsvektor bestimmen.
- Angreiferpfad und Privilegieneskalation bestimmen.
- Persistenz identifizieren.
- Exfiltration bewerten.
- Sichere Recovery-Grenzen bestimmen.

### Maßnahmen

- Authentifizierungslogs auf verdächtige erfolgreiche Logins, Impossible Travel, ungewöhnliche Quell-IP-Adressen, Token-Missbrauch und anomale Admin-Aktivitäten analysieren.
- Nutzung privilegierter Konten, Service-Accounts und Gruppenmitgliedschaftsänderungen prüfen.
- Remote-Access-Systeme einschließlich VPN, RDP-Gateways, exponierter Services, Remote-Monitoring-Tools und Jump Hosts überprüfen.
- Geplante Aufgaben, Services, Startup-Ordner, GPO-Änderungen, WMI-Event-Subscriptions und Artefakte für Remote-Ausführung prüfen.
- PowerShell-, WMI-, PsExec-, SMB-, WinRM- und RDP-Spuren auf Lateral Movement überprüfen.
- Ausgehenden Traffic auf große Transfers, Archiv-Staging, Missbrauch von Cloud-Synchronisierung oder ungewöhnliche Ziele analysieren.
- Ausgenutzte Schwachstellen, schwache Authentifizierungspfade oder fehlende Patches identifizieren.
- Prüfen, ob Backups vor der Kompromittierung erstellt wurden und ob Management-Ebenen betroffen waren.
- Eine vorläufige Angriffstimeline erstellen: Zugriff, Privilegieneskalation, Discovery, Lateral Movement, Exfiltration, Verschlüsselung.

### Scope-Kategorien

Den Impact in diesen Kategorien dokumentieren:

- Betroffene Hosts.
- Betroffene Konten und Service-Identitäten.
- Betroffene Server und Anwendungen.
- Betroffene Freigaben, Datenbanken und Speicher.
- Betroffene Geschäftsprozesse.
- Betroffene Backup-/Recovery-Assets.
- Betroffene Sicherheitstools und Logs.
- Vermutlich exfiltrierte Datensätze.

### Exit-Kriterien

Zur Planung der Beseitigung übergehen, wenn:

- Ein plausibler Zugriffspfad vorliegt.
- Die vom Angreifer überschrittenen Trust Boundaries hinreichend verstanden sind.
- Priorisierte kompromittierte Systeme und Konten identifiziert sind.
- Recovery-Kandidaten von nicht vertrauenswürdigen Assets getrennt werden können.

## Phase 4: Beseitigen (Eradicate)

### Ziele

- Den Angreiferzugang entfernen.
- Persistenz beseitigen.
- Den Eintrittsvektor schließen.
- Vertrauenswürdige Recovery vorbereiten.

### Maßnahmen

- Malware, Backdoors und Persistenzmechanismen dort entfernen, wo dies sinnvoll und belastbar ist.
- Kompromittierte Systeme neu aufbauen, wenn Vertrauen nicht wiederhergestellt werden kann.
- Ausgenutzte Schwachstellen beheben und exponierte Services schließen.
- Zugangsdaten in der richtigen Reihenfolge rotieren: zuerst privilegierte Konten, dann Backup-/Security-Admins, dann Service-Accounts und Application Secrets, danach breiterer Benutzerscope.
- Kerberos-bezogene Secrets zurücksetzen, wenn eine Domänenkompromittierung vermutet wird.
- Unautorisierte Konten, Gruppen, Mailbox-Regeln, geplante Aufgaben, Skripte und Remote-Zugriffsmethoden entfernen.
- Verdächtige Sessions, Tokens, OAuth-Freigaben und Persistenzmethoden in Cloud-Umgebungen widerrufen.
- MFA wieder aktivieren oder stärken, wenn sie umgangen oder geschwächt wurde.
- EDR, Logging, Identity-Monitoring und Backup-Schutz vor der Recovery validieren.

### Exit-Kriterien

Zur Recovery erst übergehen, wenn:

- Der Eintrittsvektor geschlossen ist oder kompensierende Kontrollen aktiv sind.
- High-Risk-Identitäten remediated wurden.
- Persistenzmechanismen entfernt oder Systeme neu aufgebaut wurden.
- Security-Monitoring im Recovery-Pfad funktionsfähig ist.

## Phase 5: Wiederanlauf (Recovery)

### Ziele

- Geschäftsservices sicher wiederherstellen.
- Reinfektion verhindern.
- Datenintegrität und Service-Vertrauen validieren.
- Während der Wiederherstellung engmaschig überwachen.

### Recovery-Gate

Mit einer breiteren Wiederherstellung **nicht** beginnen, bevor die folgenden Punkte erfüllt sind:

- Der wahrscheinliche Angreiferpfad ist gut genug verstanden, um einen unmittelbaren Wiedereintritt zu vermeiden.
- Privilegierte Identitäten wurden zurückgesetzt und administrative Workstations sind vertrauenswürdig.
- Backup- oder Image-Quellen sind validiert.
- Business Owner haben die Recovery-Prioritäten bestätigt.
- Monitoring- und Containment-Kontrollen sind auf wiederhergestellten Assets aktiv.

### Maßnahmen

- Die Wiederherstellungsreihenfolge nach Geschäftskritikalität und Abhängigkeitsmapping definieren.
- Aus sauberen, validierten Backups oder vertrauenswürdigen Golden Images wiederherstellen.
- Wiederhergestellte Systeme scannen und validieren, bevor sie erneut verbunden werden.
- Systeme zunächst in kontrollierten Segmenten online bringen.
- Anwendungsfunktionalität, geschäftliche Datenintegrität und Authentifizierungsverhalten validieren.
- Authentifizierung, Netzwerkverkehr, Dateiverhalten und Endpunktaktivität während der Recovery überwachen.
- Erhöhtes Monitoring je nach Scope und Vertrauensniveau mindestens 14 bis 30 Tage aktiv halten.
- Stakeholder über den Wiederherstellungsstatus und verbleibende Einschränkungen informieren.

### Empfohlene Recovery-Priorität

1. Vertrauenswürdige Admin-Workstations und Identitätsgrundlagen.
2. DNS, DHCP, NTP sowie zentrale Netzwerk- und Security-Services.
3. Backup- und Recovery-Orchestrierung.
4. Logging, EDR und Security-Visibility.
5. Zentrale Datei-, Datenbank- und Applikationsservices.
6. Geschäftskritische Anwendungen und Integrationen.
7. Endanwendersysteme.
8. Nicht-kritische Services.

### Exit-Kriterien

Zu den Abschlussaktivitäten übergehen, wenn:

- Kritische Services wiederhergestellt und von den Verantwortlichen abgenommen wurden.
- Keine aktiven Wiederauftretensindikatoren beobachtet werden.
- Das Monitoring einen stabilen Betrieb bestätigt.
- Offene Restrisiken dokumentiert und einem Owner zugewiesen sind.

## Phase 6: Nachbereitung (Review / Lessons Learned)

### Ziele

- Root Cause und Faktoren der Ausbreitung verstehen.
- Kontrollen und Governance verbessern.
- Beweise und Entscheidungsdokumentation finalisieren.
- Wiederholungen verhindern.

### Maßnahmen

- Innerhalb von 5 bis 10 Geschäftstagen nach der Stabilisierung eine strukturierte Nachbesprechung durchführen.
- Incident-Timeline und Schlüsselentscheidungen finalisieren.
- Den Eintrittsvektor und Eskalationspfad soweit möglich bestätigen.
- Kontrollversagen in den Bereichen Identität, Segmentierung, Logging, EDR, Patch-Management, Backup-Resilienz und administrativer Governance identifizieren.
- Performance von Backup und Recovery bewerten.
- Kommunikation, Eskalation und Qualität der Management-Entscheidungen bewerten.
- Detection-Regeln, Hardening-Baselines und Response-Verfahren aktualisieren.
- Dieses Playbook anhand der Lessons Learned aktualisieren.
- Remediation-Maßnahmen mit namentlichen Verantwortlichen und Abschlussdaten nachverfolgen.

## Zu sichernde Beweise

Mindestens folgende Beweise sichern:

- Lösegeldforderungen und Screenshots.
- Beispiele verschlüsselter Dateien und Dateiendungen.
- Malware-Samples, Loader oder Skripte, sofern sicher erfassbar.
- EDR-Detections und Alert-Metadaten.
- Windows Event Logs und Linux-Systemlogs, sofern relevant.
- Firewall-, Proxy-, VPN- und DNS-Logs.
- Authentifizierungslogs und Logs der Domänencontroller.
- Backup-System-Logs und administrative Aktionen.
- E-Mail-Gateway- und Cloud-Audit-Logs.
- Logs des Identitätsproviders, MFA-Events und Admin-Änderungen.
- Liste betroffener Systeme, Benutzer, Services und Zeitstempel.
- Relevante Memory- oder Disk-Images ausgewählter High-Value-Systeme.
- Die Incident-Timeline und das Entscheidungsprotokoll.

## Kommunikations-Checkliste

### Intern

- Incident-Response-Stakeholder informieren.
- Management und Krisenteam informieren.
- Mit Legal, Privacy und Compliance abstimmen.
- Interne Hinweise vorbereiten, was zu tun und was zu unterlassen ist.
- Eine einzige verlässliche Quelle für Incident-Updates pflegen.

### Extern

- Falls erforderlich, Kommunikation für Kunden, Lieferanten oder Partner vorbereiten.
- Alle Entwürfe externer Kommunikation archivieren.
- Keine nicht bestätigten Aussagen zu Root Cause, Angreiferidentität oder Datendiebstahl treffen, bevor die Untersuchung dies bestätigt.
- Rechtliche und datenschutzrechtliche Prüfung vor Meldungen oder öffentlichen Aussagen abstimmen.
- Cyber-Versicherung und externe IR-Unterstützung bei Bedarf koordinieren.

## Rechtliche / regulatorische Aspekte

Ransomware-Vorfälle können vertragliche, branchenspezifische, versicherungsbezogene und gesetzliche Meldepflichten auslösen, insbesondere wenn personenbezogene Daten, regulierte Daten, betriebskritische Services oder Drittverpflichtungen betroffen sind.

Dieses Playbook stellt keine Rechtsberatung dar. Es soll sicherstellen, dass:

- relevante Beweise gesichert werden,
- wesentliche Zeitpunkte dokumentiert sind,
- die Exfiltrationsbewertung nachvollziehbar geführt wird,
- Entscheidungen protokolliert werden,
- und Legal/Privacy frühzeitig eingebunden werden, um Meldepflichten bewerten zu können.

## Quick Response Checklist

### Sofortreaktion

- [ ] Incident Commander aktivieren.
- [ ] Timeline und Entscheidungsprotokoll starten.
- [ ] Betroffene Systeme isolieren.
- [ ] Beweise sichern.
- [ ] Backups schützen.
- [ ] Privilegierte Konten überprüfen.

### Scope

- [ ] Betroffene Hosts identifizieren.
- [ ] Betroffene Benutzer und Service-Accounts identifizieren.
- [ ] Betroffene Dateifreigaben und Speicher identifizieren.
- [ ] Domänencontroller und Identitätssysteme prüfen.
- [ ] Backup-Systeme und Management-Ebenen prüfen.
- [ ] Remote-Zugänge und Cloud-Admin-Portale prüfen.
- [ ] Auf Exfiltrationsindikatoren prüfen.

### Eindämmung

- [ ] Lateral-Movement-Pfade einschränken.
- [ ] Verdächtige Sessions/Tokens widerrufen.
- [ ] Kompromittierte Konten bedarfsgerecht deaktivieren.
- [ ] Riskante Synchronisationsjobs aussetzen.
- [ ] Forensische Beweise sichern.

### Untersuchung

- [ ] Eintrittsvektor bestimmen.
- [ ] Aktivität privilegierter Konten überprüfen.
- [ ] Persistenz prüfen.
- [ ] Exfiltrationsindikatoren prüfen.
- [ ] Vertrauenswürdige Recovery-Grenze bestimmen.

### Recovery

- [ ] Recovery-Gate validieren.
- [ ] Aus verifizierten sauberen Backups oder Images wiederherstellen.
- [ ] Systeme vor Wiederanbindung validieren.
- [ ] Auf Reinfektion überwachen.
- [ ] Wiederherstellungsstatus kommunizieren.

### Nach dem Incident

- [ ] Timeline abschließen.
- [ ] Root Cause soweit möglich bestätigen.
- [ ] Kontrollen und Detection aktualisieren.
- [ ] Playbook aktualisieren.
- [ ] Remediation bis zum Abschluss nachverfolgen.

## Website-Zusammenfassungsblock

**Ransomware-Angriff**  
Strukturiertes Reaktions-Playbook für vermutete oder bestätigte Ransomware-Vorfälle — von früher Erkennung und Eindämmung über den Schutz von Identitäten und Backups bis hin zu Beweissicherung, vertrauenswürdiger Wiederherstellung und Nachbereitung.

**Kernphasen:** Erkennen (Detect), Eindämmen (Contain), Untersuchen (Investigate), Beseitigen (Eradicate), Wiederanlauf (Recovery), Nachbereitung (Review / Lessons Learned).

## Hinweise für Threat Sentinel

Diese Version ist für eine öffentliche Playbook-Library optimiert und eignet sich gut als community-orientierte Referenz, weil sie Folgendes kombiniert:

- schnelle Handlungsanweisungen,
- einen sichtbaren First-15-Minutes-Block,
- explizite Entscheidungsregeln,
- stärkere Priorisierung von Identität und Backups,
- Recovery-Gates,
- und saubere Exit-Kriterien pro Phase.

Für die Website-Präsentation wäre folgendes Format besonders nützlich:

- Zusammenfassung,
- die ersten 15 Minuten,
- Entscheidungsmodell,
- einklappbare Response-Phasen,
- Evidence-Checkliste,
- Kommunikations-Checkliste,
- Quick-Response-Checkliste,
- Referenzabschnitt.
