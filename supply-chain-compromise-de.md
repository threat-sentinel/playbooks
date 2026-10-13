# Playbook: Kompromittierung in der Lieferkette

## Zusammenfassung

Dieses Playbook strukturiert die Reaktion, wenn der Angriff nicht bei Ihnen beginnt, sondern bei jemandem oder etwas, dem Sie vertrauen: einem manipulierten Software-Update, einem kompromittierten IT-Dienstleister, einer schädlichen Open-Source-Abhängigkeit, einer kompromittierten Build-Pipeline oder einem Partner mit Daten- oder Secret-Abfluss.

Das Besondere: Der Angreifer kommt über einen **legitimen und vertrauten Weg**. Signierte Updates, freigegebene Fernwartung, offizielle Paketquellen und bestehende Vertrauensstellungen lassen schädliche Aktivitäten oft zunächst legitim erscheinen.

Der Kern dieses Playbooks: **Zuerst klären, wo die betroffene Komponente bei Ihnen steckt, welche Rechte sie hat und welche Vertrauensstellung sie besitzt — dann diese Vertrauensstellung kontrolliert entziehen, bis klar ist, ob Missbrauch stattgefunden hat.**

## Zweck

- Schnell feststellen, ob und wo eine gemeldete Lieferketten-Kompromittierung Sie betrifft
- Die Vertrauensstellung der betroffenen Komponente, des Dienstleisters oder der Quelle kontrolliert einschränken
- Missbrauch in der eigenen Umgebung erkennen
- Geheimnisse und Zugänge erneuern, die die Komponente oder der Dienstleister kannte
- Mit Hersteller, Partner oder Dienstleister strukturiert zusammenarbeiten
- Entscheiden, ob und wie die Komponente weiter genutzt, ersetzt oder entfernt wird

## Anwendungsbereich

Verwenden Sie dieses Playbook bei mindestens einem der folgenden Szenarien:

- **Manipuliertes Software-Update:** Ein Hersteller meldet, dass Installationspakete oder Updates Schadcode enthielten.
- **Kompromittierter Dienstleister:** Ein MSP, Systemhaus oder Softwareanbieter mit Fernzugriff auf Ihre Umgebung ist selbst kompromittiert.
- **Schädliches Paket:** Eine Abhängigkeit in Anwendungen, Build-Pipelines, Paket-Repositories oder Container-Images enthält Schadcode.
- **Kompromittierte Build- oder Entwicklungsumgebung:** CI/CD, Code-Repositories oder Signaturschlüssel bei Ihnen oder einem Zulieferer sind betroffen.
- **Datenabfluss beim Partner:** Ein SaaS-Anbieter oder Partner meldet einen Vorfall, bei dem Ihre Daten, Zugangsdaten oder API-Schlüssel betroffen sein können.
- **Eigene Erkennung zuerst:** EDR, Monitoring oder Threat Hunting zeigen verdächtiges Verhalten einer vertrauten Komponente, ohne dass der Hersteller bisher gewarnt hat.

**Abgrenzung:**

- Bei laufender Verschlüsselung oder breiter Kompromittierung führt zusätzlich das Ransomware-Playbook.
- Für reine Datenabflüsse beim Partner läuft ergänzend das Datenpannen-Playbook.
- Für bloße Schwachstellen ohne Missbrauch oder Vertrauensbruch gilt Schwachstellenmanagement, nicht dieses Playbook.

## Schweregrad

Standard-Schweregrad: **Hoch**.

Auf **Kritisch** anheben, wenn:

- die betroffene Komponente bei Ihnen in der kompromittierten Version installiert ist oder war
- die Komponente oder der Dienstleister administrative Rechte besitzt
- Ausführung von Schadcode oder Verbindungen zu bekannter Angreifer-Infrastruktur sichtbar sind
- Signaturschlüssel, Build-Systeme oder Artefakt-Repositories betroffen sind
- Sie selbst Software, Managed Services oder Artefakte an Kunden weitergeben und die Kompromittierung weitergereicht haben könnten

## Incident-Ziele

1. Betroffenheit feststellen: Wo ist die Komponente, welche Version läuft, welche Rechte besitzt sie?
2. Vertrauensstellung einschränken, bis die Lage klar ist.
3. Missbrauch in der eigenen Umgebung erkennen.
4. Geheimnisse und Zugänge erneuern, die die Komponente oder der Dienstleister kannte.
5. Saubere Weiternutzung, Ersatz oder Entfernung entscheiden.
6. Eigene Kunden informieren, wenn Sie selbst Teil der Lieferkette sind.

## Zentrale Reaktionsprinzipien

- **Inventar vor Aktion.** Ohne zu wissen, wo eine Komponente läuft, ist jede Maßnahme unvollständig.
- **Vertrauen vorübergehend entziehen.** Automatische Updates stoppen, Fernwartungszugänge sperren, Schlüssel deaktivieren und Verbindungen einschränken.
- **Herstellerangaben prüfen, nicht nur übernehmen.** Erste Meldungen sind oft unvollständig und werden später erweitert.
- **Was die Komponente konnte, konnte auch der Angreifer.** Rechte, Netzreichweite und gespeicherte Geheimnisse bestimmen den möglichen Schaden.
- **Eigene Rolle mitdenken.** Wenn Sie selbst Artefakte, Software oder Dienste weiterliefern, sind Ihre Kunden potenziell nächste Betroffene.
- **Zeit konsistent erfassen.** Herstellerupdates, IOCs, interne Befunde und Entscheidungen in UTC dokumentieren.

## Kontakte und Eskalation

Diese Angaben organisationsspezifisch vorbelegen und offline verfügbar halten:

- Incident-Lead on call
- Einkauf / Vendor Management / Dienstleistersteuerung
- IT-Betrieb / Softwareverteilung
- DevOps / Plattform / Build- und Release-Verantwortliche
- Hersteller-PSIRT oder Dienstleister-Notfallkontakt
- Legal, Datenschutz und Kommunikation
- Externe Forensik / Incident Response

## Die ersten 15 Minuten

- Incident-Lead benennen, Timeline und Entscheidungsprotokoll starten.
- Quelle sichern und genau lesen: Herstellerhinweis, CERT-Warnung, Pressebericht oder eigener Befund.
- Betroffene Produkte, Versionen, Zeiträume, IOCs und empfohlene Sofortmaßnahmen notieren.
- Erste Inventarabfrage starten: Wo ist die Komponente installiert oder eingebunden?
- Automatische Updates oder automatische Verteilung der betroffenen Komponente stoppen.
- Bei Dienstleister-Kompromittierung Fernwartungszugänge des Dienstleisters vorübergehend sperren und den Dienstleister über einen bekannten Kontaktweg anrufen.
- Mit veröffentlichten oder selbst ermittelten Indikatoren in EDR-, DNS-, Proxy-, Firewall- und Cloud-Logs suchen.
- Management informieren, wenn betroffene Versionen oder privilegierte Vertrauensstellungen gefunden wurden.
- Termin zur Neubewertung festlegen, weil Herstellerangaben sich oft ändern.

## Rollen und Verantwortlichkeiten

### Incident-Lead

- Steuert den Vorfall.
- Priorisiert Sperrung, eingeschränkten Weiterbetrieb, Ersatz und Kundenkommunikation.
- Führt Timeline, Lagebild und Entscheidungsprotokoll.

### IT-Betrieb und Softwareverteilung

- Liefern Inventar und Verteilungssicht.
- Stoppen Updates, isolieren Systeme und setzen Ersatz oder Entfernung um.

### Entwicklung / DevOps

- Prüfen Abhängigkeiten, Build-Pipelines, Lockfiles, Artefakte, Signaturen und Veröffentlichungswege.
- Bewerten, ob eigene Artefakte oder Releases betroffen sind.

### Security / SOC

- Suchen nach Indikatoren und Missbrauch.
- Bewerten Folgeaktivität und Kompromittierungsreichweite.

### Einkauf / Vendor Management

- Kennen Verträge, Meldepflichten, Ansprechpartner und Zugänge von Dienstleistern.
- Koordinieren Eskalation mit Lieferanten.

### Legal, Datenschutz und Kommunikation

- Bewerten Meldepflichten.
- Steuern Kunden-, Partner- und externe Kommunikation.
- Prüfen Haftungs- und Vertragsthemen.

## Entscheidungsmodell

### Regel 1: Sind wir betroffen?

**Betroffen**, wenn die Komponente in einer betroffenen Version im betroffenen Zeitraum installiert oder genutzt wurde oder der betroffene Dienstleister im relevanten Zeitraum Zugang zur eigenen Umgebung hatte.

**Möglicherweise betroffen**, wenn Inventar lückenhaft ist oder Herstellerangaben noch vorläufig sind. Dann bis zur Klärung wie betroffen behandeln.

**Nicht betroffen**, wenn die Komponente nachweislich nicht vorhanden ist oder nur außerhalb des betroffenen Versions- und Zeitraums vorlag. Begründung dokumentieren und Herstellerupdates weiter verfolgen.

### Regel 2: Sperren, isolieren oder eingeschränkt weiterbetreiben?

**Sofort sperren oder isolieren**, wenn:

- Schadcode in Ihrer Umgebung ausgeführt wurde
- belastbare Indikatoren gefunden wurden
- die Komponente oder der Dienstleister administrative Rechte besitzt und die Lage unklar ist
- der Hersteller oder das PSIRT ausdrücklich zur Abschaltung rät

**Eingeschränkt weiterbetreiben** ist nur vertretbar, wenn:

- keine Ausführungs- oder Missbrauchshinweise vorliegen
- automatische Updates gestoppt sind
- Netzwerkverbindungen eingeschränkt sind
- das Geschäftsrisiko eines Stopps höher wäre
- die Entscheidung dokumentiert und durch Incident-Lead sowie verantwortliches Management freigegeben wird

### Regel 3: Welche Geheimnisse sind zu erneuern?

Alle Geheimnisse, die die Komponente oder der Dienstleister kannte oder erreichen konnte:

- Dienstkonten der Software und deren Rechte
- API-Schlüssel, Tokens, Zertifikate und Konfigurationsgeheimnisse
- Bei Build-Systemen: CI/CD-Secrets, Signaturschlüssel, Deploy-Schlüssel, Cloud-Credentials, Registry-Tokens
- Bei Paketen: Umgebungsvariablen, lokale Secrets und Zugriffsdaten auf den Systemen, auf denen das Paket lief
- Bei Dienstleistern: alle bekannten oder erreichbaren Konten, VPN-Zugänge, RMM-Zugänge und Delegationen

### Regel 4: Weiter nutzen, ersetzen oder entfernen?

Weiter nutzen nur, wenn:

1. eine saubere Version bereitsteht
2. die Quelle verifiziert wurde, etwa über Hash, Signatur und Bezugsquelle
3. die Rechte der Komponente auf das notwendige Minimum reduziert sind
4. Monitoring für Verhalten und Folgeaktivität eingerichtet ist

Andernfalls Ersatz planen oder die Komponente bis dahin isoliert betreiben oder entfernen.

### Regel 5: Wann andere Playbooks aktivieren?

- **Malware- oder Ransomware-Playbook parallel oder führend**, wenn auf betroffenen Systemen Schadcodeausführung sichtbar ist
- **Datenpannen-Playbook parallel**, wenn Daten beim Partner oder über kompromittierte Software betroffen sein können
- **Identitätskompromittierung parallel**, wenn Tokens, SSO, Cloud-Zugänge oder privilegierte Dienstkonten betroffen sind

## Szenario-Hinweise

### Manipuliertes Software-Update

- Installationszeitpunkt der betroffenen Version pro System ermitteln
- Netzwerkverbindungen und Prozessaktivität seit Installation prüfen
- Systeme mit bestätigter Schadaktivität wie Malware- oder Ransomware-Fall behandeln

### Kompromittierter Dienstleister / RMM

- Alle Zugänge des Dienstleisters auflisten: Konten, VPN, RMM-Agenten, Jump-Hosts, Cloud-Delegationen
- RMM-Protokolle auf ausgeführte Skripte, Befehle und Massenaktionen im betroffenen Zeitraum prüfen
- Zugänge zurücksetzen und Wiederfreigabe nur über einen neuen, überwachten Zugang erlauben

### Schädliches Paket / Abhängigkeit

- Lockfiles, Build-Logs und Artefaktquellen auf die betroffene Version prüfen, auch transitiv
- Ermitteln, wo das Paket lief: Entwicklerrechner, CI, Build-Server, Produktion
- Geheimnisse dieser Systeme rotieren
- Artefakt-Caches, Paket-Proxys und Container-Repositories bereinigen
- Auf saubere Version pinnen

### Kompromittierte interne Build-Pipeline

- Builds und Releases aus dem betroffenen Zeitraum identifizieren
- Veröffentlichte Artefakte, Container-Images, Pakete und Installationsdateien inventarisieren
- Signing Keys, CI/CD-Runner-Secrets, Cloud-Deploy-Credentials und Registry-Tokens rotieren
- Reproduzierbare Builds oder unabhängige Verifikation durchführen, soweit möglich
- Betroffene Versionen zurückziehen, sperren oder als unsicher markieren
- Aus vertrauenswürdigem Source-Stand in bereinigter Pipeline neu bauen und neu signieren
- Downstream-Kunden mit Versionsbereichen, Hashes, IOCs und Maßnahmen informieren
- Repository-, Branch-Protection-, Pull-Request- und CI/CD-Audit-Logs auswerten

### Eigene Erkennung zuerst

- Befund absichern, bevor er breit kommuniziert wird
- Hersteller oder PSIRT über bekannten Security-Kontakt vertraulich informieren
- CERT oder zuständige Behörde einbeziehen, wenn Breitenwirkung wahrscheinlich ist
- Indikatoren nicht vorschnell öffentlich teilen

### Datenabfluss beim Partner

- Betroffene Daten, Schlüssel, Tokens und Integrationen bestimmen
- Schlüssel und Tokens rotieren
- Integrationen auf Missbrauch prüfen
- Datenschutzbewertung starten

## SBOM, VEX und Provenance

- **SBOM** beantwortet die Inventarfrage: Ist die betroffene Komponente enthalten?
- **VEX** beantwortet die Betroffenheitsfrage: Ist die gemeldete Schwachstelle oder Kompromittierung im eigenen Produkt tatsächlich relevant oder ausnutzbar?
- **Provenance / Build-Nachweis** beantwortet die Vertrauensfrage: Woher stammt das Artefakt und unter welchen Bedingungen wurde es gebaut und signiert?

Diese drei Artefaktarten zusammen verbessern die Entscheidung „betroffen“, „möglicherweise betroffen“ oder „nicht betroffen“.

Fehlende Treffer sind nur so belastbar wie das Inventar dahinter. Wenn SBOM, Asset-Inventar oder Build-Nachweise Lücken haben, bleibt die Einstufung „möglicherweise betroffen“ bestehen, bis die Lücke geschlossen oder das Risiko anderweitig begrenzt wurde.

## Negative Befunde und Neubewertung

Ein „nicht betroffen“ ist eine Momentaufnahme, keine endgültige Entwarnung. Dokumentieren Sie, auf welcher Herstellerinformation, welchem Versionsstand, welchem Zeitraum und welchen Suchkriterien die Bewertung beruht. Legen Sie zugleich einen Termin oder Auslöser für die Neubewertung fest, etwa ein aktualisiertes Hersteller-Advisory, neue IOCs oder neue Erkenntnisse zur kompromittierten Build-Kette.

## Phase 1: Erkennen und Triage

### Ziele

- Information validieren
- Betroffenheit und Reichweite bestimmen
- Vertrauensstellung, Rechte und Geheimnisse erfassen

### Maßnahmen

- Quelle validieren: offizielle Herstellerquelle, CERT, CISA, BSI, BACS oder eigener belastbarer Befund
- Inventar vollständig erheben; Lücken ausdrücklich benennen
- Rechte und Netzreichweite der Komponente oder des Dienstleisterzugangs dokumentieren
- Mit IOCs und Verhaltensmustern suchen
- Zeiträume und Versionsstände sauber zuordnen

### Exit-Kriterien

- Betroffenheit festgestellt: betroffen, möglicherweise betroffen oder nicht betroffen
- Rechte, Vertrauensstellung und potenzielle Reichweite bekannt
- Erstes Lagebild dokumentiert

## Phase 2: Eindämmen

### Ziele

- Weitere Verteilung, Nutzung oder Fernzugriffe stoppen
- Vertrauensstellung kontrolliert einschränken

### Maßnahmen

- Verteilung der **betroffenen Komponente oder Quelle** pausieren, nicht pauschal alle Sicherheitsupdates stoppen
- Bezugsquellen blockieren oder isolieren
- Zugänge des Dienstleisters sperren oder einschränken
- Netzwerkverbindungen der Komponente auf das Minimum begrenzen
- Betroffene Systeme bei Funden isolieren

### Nicht tun

- Den Dienstleister nicht über möglicherweise kompromittierte Kanäle kontaktieren
- Nicht auf die erste Versionsliste des Herstellers vertrauen
- Nicht pauschal alles abschalten, wenn nur eine Komponente betroffen ist

### Exit-Kriterien

- Keine automatische Verteilung der betroffenen Quelle mehr
- Zugänge der Komponente oder des Dienstleisters eingeschränkt oder gesperrt
- Systeme mit belastbaren Funden isoliert

## Phase 3: Untersuchen

### Ziele

- Missbrauch in der eigenen Umgebung erkennen oder fundiert ausschließen
- Betroffene Systeme, Konten und Daten bestimmen

### Maßnahmen

- Aktivitäten der Komponente oder des Dienstleisterzugangs im Zeitraum rekonstruieren
- Auf Folgeaktivität achten: neue Konten, Remote-Ausführung, Rechteausweitung, Datenabfluss
- Mit Hersteller oder Dienstleister Erkenntnisse austauschen
- Prüfen, ob eigene Kunden oder nachgelagerte Systeme betroffen sein könnten

### Exit-Kriterien

- Missbrauch festgestellt oder mit begründeter Sicherheit ausgeschlossen
- Betroffene Systeme, Konten, Daten und Artefakte benannt
- Übergabe an weitere Playbooks erfolgt, falls nötig

## Phase 4: Beseitigen

### Ziele

- Betroffene Komponenten und Vertrauensstellungen bereinigen
- Sichere Basis für Weiterbetrieb oder Ersatz schaffen

### Maßnahmen

- Betroffene Versionen entfernen, ersetzen oder neu installieren
- Geheimnisse gemäß Regel 3 erneuern
- Betroffene Systeme bereinigen oder neu aufbauen
- Dienstleisterzugänge neu, minimal und überwacht einrichten
- Eigene Artefakte, falls betroffen, neu bauen, neu signieren und neu verteilen

### Exit-Kriterien

- Betroffene Versionen entfernt, ersetzt oder sauber neu bereitgestellt
- Geheimnisse erneuert
- Kritische Vertrauensstellungen neu aufgebaut und überwacht

## Phase 5: Wiederanlauf

### Ziele

- Saubere Nutzung kontrolliert wieder aufnehmen
- Folgeaktivität eng überwachen

### Maßnahmen

- Saubere Version verifiziert bereitstellen
- Monitoring auf Verhalten der Komponente mindestens 30 Tage erhöhen
- Automatische Updates erst wieder aktivieren, wenn Quelle und Verifikation geregelt sind
- Eigene Kunden oder Fachbereiche mit klaren Handlungsanweisungen versorgen

### Exit-Kriterien

- Saubere Version im Einsatz oder Ersatz umgesetzt
- Monitoring ohne Befund
- Restrisiken dokumentiert

## Phase 6: Nachbereitung

### Maßnahmen

- Prüfen, wie lange die Inventarfrage dauerte und wo Lücken bestanden
- Rechte der Komponente hinterfragen
- Verträge, Notfallkontakte, Meldepflichten und Prüfrechte bewerten
- SBOM, Provenance und Softwareinventar verbessern
- Lieferkettenrisiken systematisch in das Risikomanagement aufnehmen

## Zu sichernde Beweise

- Herstellerhinweise und deren Versionen mit Datum
- Inventarabfragen mit Zeitstempel
- Installations-, Verteilungs- und Paketmanager-Logs
- RMM- und Dienstleister-Zugangsprotokolle
- EDR-, DNS-, Proxy-, Firewall- und Cloud-Daten zu IOCs
- Kommunikation mit Hersteller oder Dienstleister
- Timeline und Entscheidungsprotokoll

## Kommunikations-Checkliste

- Hersteller oder Dienstleister nur über bekannten Kontaktweg kontaktieren
- Interne Fachbereiche über Einschränkungen und Handlungsbedarf informieren
- Wenn Sie selbst liefern: Kunden mit betroffenen Versionen, IOCs und empfohlenen Maßnahmen informieren
- Keine öffentliche Schuldzuweisung, solange Fakten unklar sind

## Rechtliche und regulatorische Aspekte

Orientierung, keine Rechtsberatung (Stand: Oktober 2026). Ob eine Pflicht greift, hängt von Land, Sektor, Größe und Rolle der Organisation ab. Fristen, Meldewege und Ansprechpartner sollten vorab festgelegt sein und im Ernstfall mit Recht und Datenschutz bestätigt werden.

**Meldepflichten für Cybersicherheitsvorfälle**

- **Deutschland — NIS2 (§ 32 BSIG, seit 6. Dezember 2025):** Besonders wichtige und wichtige Einrichtungen melden erhebliche Sicherheitsvorfälle an das BSI: Frühwarnung binnen 24 Stunden, Meldung binnen 72 Stunden, Abschlussbericht binnen eines Monats.
- **Österreich — NISG 2026 (seit 1. Oktober 2026):** Wesentliche und wichtige Einrichtungen melden erhebliche Sicherheitsvorfälle an das zuständige CSIRT: Frühwarnung binnen 24 Stunden, Meldung binnen 72 Stunden.
- **Schweiz — Informationssicherheitsgesetz (ISG, seit 1. April 2025):** Betreiber kritischer Infrastrukturen melden Cyberangriffe binnen 24 Stunden an das Bundesamt für Cybersicherheit (BACS).

**Datenschutz**

- **Deutschland und Österreich — DSGVO Art. 33/34:** Verletzungen des Schutzes personenbezogener Daten binnen 72 Stunden ab Kenntnis an die Aufsichtsbehörde melden, sofern ein Risiko für die Betroffenen nicht unwahrscheinlich ist; bei hohem Risiko auch die Betroffenen benachrichtigen.
- **Schweiz — DSG Art. 24:** Meldung an den EDÖB so rasch als möglich, wenn die Verletzung voraussichtlich zu einem hohen Risiko für die Betroffenen führt.

Alle Fristen laufen ab Kenntnis des Vorfalls, nicht erst ab Abschluss der technischen Untersuchung. Eine unvollständige Frühwarnung innerhalb der Frist ist besser als eine vollständige Meldung danach.

**Für dieses Szenario**

- NIS2 zählt die Sicherheit der Lieferkette ausdrücklich zu den Risikomanagementpflichten. Erhebliche Vorfälle über einen Zulieferer sind wie eigene Vorfälle meldepflichtig.
- **Auftragsverarbeiter:** Der Dienstleister muss Sie nach Art. 33 Abs. 2 DSGVO unverzüglich informieren. Ihre eigene 72-Stunden-Frist beginnt mit Ihrer Kenntnis.
- **Cyber Resilience Act (seit 11. September 2026):** Wenn Sie selbst Hersteller von Produkten mit digitalen Elementen sind, melden Sie aktiv ausgenutzte Schwachstellen und schwerwiegende Sicherheitsvorfälle über die Meldeplattform der ENISA: Frühwarnung binnen 24 Stunden, Meldung binnen 72 Stunden.
- Verträge mit Zulieferern, Dienstleistern und eigenen Kunden auf Meldepflichten, Haftung und Prüfrechte prüfen.

## Quick-Response-Checkliste

- [ ] Information validiert, Versionen und Zeiträume notiert
- [ ] Inventar erhoben, Lücken benannt
- [ ] Betroffene Verteilung oder Quelle pausiert
- [ ] Dienstleisterzugänge gesperrt oder eingeschränkt
- [ ] Indikatoren gesucht
- [ ] Rechte und Vertrauensstellung der Komponente dokumentiert
- [ ] Geheimnisse erneuert
- [ ] Entscheidung zu Weiternutzung, Ersatz oder Entfernung dokumentiert
- [ ] Eigene Kunden informiert, falls relevant
- [ ] Herstellerupdates weiter verfolgt

## Präventive Kontrollen

- Vollständiges Software- und Dienstleisterinventar mit Rechten und Ansprechpartnern
- SBOM für eigene Anwendungen
- VEX- und Provenance-Fähigkeit für kritische Lieferketten
- Dienstleisterzugänge nur bei Bedarf, mit MFA, Protokollierung und Zeitbegrenzung
- Minimale Rechte für Software- und Build-Dienstkonten
- Verifikation von Updates über Signaturen, Hashes und gestaffelte Verteilung
- Schutz der eigenen Build-Systeme und Signaturschlüssel
- Verträge mit Meldepflichten, Notfallkontakten und Prüfrechten

## Kompakter Einstieg

Dieser Block ist kein verkürztes Ersatz-Playbook. Er dient als schneller Einstieg; für die Bearbeitung des Vorfalls gelten alle Abschnitte dieses Dokuments.

**Kompromittierung in der Lieferkette**  
Reaktion auf manipulierte Updates, kompromittierte Dienstleister, schädliche Pakete und Vorfälle bei Partnern — Betroffenheit über das Inventar klären, Vertrauensstellungen entziehen, Geheimnisse erneuern und über Weiternutzung oder Ersatz entscheiden.

**Kernphasen:** Erkennen, Eindämmen, Untersuchen, Beseitigen, Wiederanlauf, Nachbereitung.

## Referenzen

- NIST SP 800-161 Rev. 1
- NIST SP 800-61 Rev. 3
- CISA: Defending Against Software Supply Chain Attacks
- ENISA: Threat Landscape for Supply Chain Attacks
- MITRE ATT&CK: T1195, T1199

## Disclaimer

Dieses Playbook ist eine praktische Community-Ressource. Es ersetzt keine Rechtsberatung, Herstellervorgaben oder organisationsspezifische Incident-Response-Planung.
