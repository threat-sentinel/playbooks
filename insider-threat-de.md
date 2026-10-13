# Playbook: Insider-Vorfall

## Zusammenfassung

Dieses Playbook strukturiert die Reaktion auf Sicherheitsvorfälle, die von Personen mit legitimem Zugang ausgehen: Beschäftigte, ehemalige Beschäftigte, Praktikanten, Leiharbeitskräfte, Administratoren oder Dienstleister. Typische Fälle sind Datenmitnahme beim Ausscheiden, Weitergabe vertraulicher Informationen, Sabotage, Missbrauch privilegierter Rechte oder Betrug.

Insider-Vorfälle unterscheiden sich grundlegend von externen Angriffen: Es geht um eine **Person**, nicht primär um Schadsoftware. Arbeitsrecht, Beschäftigtendatenschutz, Mitbestimmung und Verhältnismäßigkeit bestimmen, was untersucht werden darf und wie.

Der Kern dieses Playbooks: **Erst Personal und Recht, dann Technik.** Die IT handelt nicht allein, nicht heimlich und nicht auf Zuruf.

## Zweck

- Verdachtsfälle strukturiert, rechtskonform und verhältnismäßig bearbeiten
- Weiteren Schaden verhindern, ohne die Untersuchung unnötig offenzulegen
- Beweise gerichtsfest sichern
- Die Rechte der betroffenen Person und unbeteiligter Dritter wahren
- Entscheidungen nachvollziehbar dokumentieren

## Anwendungsbereich

Verwenden Sie dieses Playbook, wenn:

- eine Person mit legitimem Zugang ungewöhnlich große Datenmengen herunterlädt, druckt, auf USB kopiert oder an private Konten sendet
- vertrauliche Informationen intern zuordenbar nach außen auftauchen
- privilegierte Rechte außerhalb des Auftrags genutzt werden
- Sabotage vermutet wird, etwa Löschungen, Konfigurationsänderungen oder Logikbomben
- Hinweise aus Team, Führung, Hinweisgebersystem oder Compliance eingehen
- ausgeschiedene Personen weiterhin Zugriff haben oder nutzen

**Abgrenzung:**

- Wurde das Konto der Person von außen übernommen, handelt es sich primär um eine Identitätskompromittierung.
- Fahrlässige Fehler ohne vorsätzliche Komponente laufen meist über das Datenpannen-Playbook.
- Private Regelverstöße ohne Sicherheitswirkung sind primär HR- oder Compliance-Themen.

## Schweregrad

- **Mittel:** Verdacht auf Regelverstoß ohne erkennbaren Schaden
- **Hoch:** Wahrscheinliche Mitnahme oder Weitergabe vertraulicher Daten oder Missbrauch privilegierter Rechte
- **Kritisch:** Sabotage, laufende Schädigung, Abfluss besonders schutzwürdiger Daten oder Beteiligung privilegierter Administratoren

## Incident-Ziele

1. Den Verdacht vertraulich und rechtskonform prüfen.
2. Weiteren Schaden verhindern.
3. Beweise gerichtsfest sichern.
4. Eine fundierte Entscheidung über arbeitsrechtliche, zivilrechtliche oder strafrechtliche Schritte ermöglichen.
5. Organisatorische und technische Kontrollen verbessern.

## Zentrale Reaktionsprinzipien

- **Kein Alleingang der IT.** Untersuchungen werden von einem kleinen Kernteam aus Personal, Recht, Datenschutz und Security beauftragt und gesteuert.
- **Need-to-know.** So wenige Beteiligte wie möglich.
- **Verhältnismäßigkeit.** Nur Daten, Systeme und Zeiträume auswerten, die zur Klärung des konkreten Verdachts erforderlich sind.
- **Unschuldsvermutung.** Ein Verdacht ist keine Feststellung.
- **Mitbestimmung beachten.** Betriebs- oder Personalrat, Betriebsvereinbarungen und interne Richtlinien bestimmen die zulässigen Maßnahmen.
- **Gerichtsfest dokumentieren.** Beweiskette, Vier-Augen-Prinzip, Hashwerte und Protokollierung konsequent führen.
- **Zeitpunkt der Sperre abstimmen.** Zugänge werden in der Regel gleichzeitig mit Gespräch, Freistellung oder Kündigung entzogen.
- **Zeit konsistent erfassen.** Timeline, Beweiskette und Exportdaten grundsätzlich in UTC dokumentieren.

## Die ersten 15 Minuten

- Hinweis entgegennehmen und schriftlich dokumentieren: wer, wann, was genau, aus welcher Quelle
- **Keine** eigenmächtigen Nachforschungen in Postfächern, Dateien oder Verläufen der betroffenen Person beginnen
- Kernteam einberufen: Personal, Recht, Datenschutz, Security-Leitung, je nach Organisation Compliance
- Prüfen, ob akute Gefahr besteht, etwa laufende Sabotage oder laufender Massenabfluss
- Festhalten, ob die betroffene Person vom Verdacht wissen könnte
- Sicherstellen, dass Logs, DLP-Daten, Cloud-Audits oder Mail-Protokolle nicht ablaufen
- Nur bei akuter Gefahr und nach Rücksprache erste eng begrenzte Schutzmaßnahmen ergreifen

## Rollen und Verantwortlichkeiten

### Kernteam

- Besteht typischerweise aus Personal, Recht, Datenschutz, Security-Leitung, gegebenenfalls Compliance und Geschäftsführung
- Beauftragt Untersuchungsschritte schriftlich
- Entscheidet über Maßnahmen gegenüber der betroffenen Person

### Security / Forensik

- Führt nur beauftragte Auswertungen durch
- Arbeitet im Vier-Augen-Prinzip
- Dokumentiert jeden Schritt
- Bindet bei hohem Risiko externe Forensik ein

### IT-Administration

- Setzt technische Maßnahmen ausschließlich auf Anweisung des Kernteams um
- Verlängert Aufbewahrung, sperrt Zugänge und sichert Systeme
- Behandelt den Vorgang vertraulich

### Betriebs- oder Personalrat

- Wird nach rechtlicher Lage und internen Vereinbarungen beteiligt

### Datenschutz

- Prüft Zulässigkeit der Datenverarbeitung
- Dokumentiert die Interessenabwägung und begleitet datenschutzrechtliche Bewertungen

## Entscheidungsmodell

### Regel 1: Insider oder übernommenes Konto?

Bevor gegen eine Person ermittelt wird, prüfen:

- Passen Anmeldeorte, Geräte und Zeiten zur Person?
- Gibt es Hinweise auf Phishing, Malware oder MFA-Auffälligkeiten?
- War die Person zum fraglichen Zeitpunkt überhaupt anwesend oder arbeitsfähig?

Bei Zweifeln zuerst das Identitäts-Playbook anwenden.

### Regel 2: Sofort handeln oder verdeckt prüfen?

**Sofortige technische Maßnahmen** sind nur gerechtfertigt, wenn:

- laufende Sabotage sichtbar ist
- laufender Massenabfluss stattfindet
- erhebliche Gefahr für Systeme, Daten oder Personen besteht

**Verdeckte, begrenzte Prüfung** ist vertretbar, wenn:

- kein akuter Schaden entsteht
- eine offene Maßnahme Beweise gefährden würde
- die Maßnahme rechtlich zulässig und schriftlich beauftragt ist

### Regel 3: Wann werden Zugänge entzogen?

In der Regel **zeitgleich** mit Gespräch, Freistellung oder Kündigung:

- vorbereitete Liste aller Zugänge, Konten, Tokens, Schlüssel, physischen Zutritte und Geräte
- technische Sperre zu vereinbartem Zeitpunkt
- Geräte im Gespräch oder im abgestimmten Prozess einsammeln

Bei privilegierten Administratoren zusätzlich gemeinsam genutzte Passwörter, Notfallkonten, Schlüssel und Dienstleisterzugänge erneuern.

### Regel 4: Externe Stellen einbeziehen?

- **Strafanzeige:** bei Verdacht auf Straftaten, Entscheidung durch Kernteam und Geschäftsführung
- **Datenschutzbehörde:** wenn personenbezogene Daten betroffen und meldepflichtig sind
- **Externe Forensik:** wenn gerichtliche Auseinandersetzungen wahrscheinlich sind

## Notfallregel bei akuter Gefahr

Wenn eine konkret nachweisbare, laufende Schädigung besteht und das Kernteam nicht vollständig vorab entscheiden kann:

- Security-Leitung und Incident-Lead dürfen eine eng begrenzte Schutzmaßnahme anordnen
- Die Maßnahme muss den akuten Schaden begrenzen und soweit möglich reversibel sein
- Personal, Recht und Datenschutz werden unverzüglich nachgezogen
- Umfang, Begründung, Zeitpunkt, Entscheider und technische Wirkung werden dokumentiert
- Weitergehende Auswertung personenbezogener Daten erfolgt weiterhin nur auf schriftlichen Untersuchungsauftrag

## Abgrenzungsmatrix

| Sachverhalt | Primäre Behandlung |
|---|---|
| Vorsätzlicher Missbrauch durch berechtigte Person | Insider-Playbook |
| Übernommenes Benutzerkonto | Identitätskompromittierung |
| Versehentliche Fehladressierung oder Fehlfreigabe | Datenpannen-Playbook |
| Privater Regelverstoß ohne Sicherheitswirkung | HR- oder Compliance-Prozess |

## Sonderfall: Datenmitnahme beim Ausscheiden

Der häufigste Insider-Fall. Zwischen Kündigung und letztem Arbeitstag liegen oft Tage oder Wochen mit weiterem Zugriff.

- Bei Kündigung Rechte und besonders schutzwürdige Zugriffe prüfen und wenn nötig abgestimmt reduzieren
- Rückblickende Auswertungen nur bei konkretem Anlass und mit Untersuchungsauftrag
- Am letzten Tag alle Zugänge entziehen, Geräte einsammeln, Weiterleitungen und Freigaben prüfen
- Bei Verdacht auf Mitnahme Herausgabe, Löschung und Bestätigung rechtlich begleitet verlangen

## Typische Indikatoren

**Ein einzelner Indikator ist kein Beweis.** Viele Auffälligkeiten haben harmlose Erklärungen und müssen im Arbeitskontext geprüft werden.

- Ungewöhnliche Downloads, Exporte, Druckaufträge oder Synchronisationen zu privaten Speichern
- Weiterleitung von E-Mails an private Adressen
- Zugriff auf Daten außerhalb der eigenen Aufgabe
- Aktivität außerhalb üblicher Arbeitszeiten, besonders kurz vor oder nach Kündigung
- Nutzung von USB-Speichern entgegen Richtlinie
- Deaktivierung von Sicherheitssoftware oder Protokollierung
- Anlage zusätzlicher Konten oder Hintertüren durch Administratoren

## Sicherung ist nicht Auswertung

Technische Beweissicherung kann nötig sein, bevor Logs überschrieben werden oder Geräte nicht mehr verfügbar sind. Sie erlaubt aber noch keine inhaltliche Auswertung personenbezogener Daten. Ausgewertet wird erst auf schriftlichen Auftrag des Kernteams, innerhalb des festgelegten Zwecks, Zeitraums und Datenumfangs.

## Phase 1: Erkennen und Bewerten

### Ziele

- Hinweis bewerten
- Insider-Verdacht von Identitätsvorfall trennen
- Kernteam und Rechtsrahmen herstellen

### Maßnahmen

- Hinweis dokumentieren
- Kernteam einberufen und Vertraulichkeit festlegen
- Rechtsrahmen prüfen: Betriebsvereinbarung, Richtlinien, private Nutzung, Einwilligungen
- Beweissicherung ohne Auswertung veranlassen
- Entscheidung zu offenem oder verdecktem Vorgehen dokumentieren

### Exit-Kriterien

- Kernteam steht
- Rechtsrahmen geklärt
- Schriftlicher Untersuchungsauftrag mit Umfang und Zeitraum liegt vor

## Phase 2: Eindämmen

### Ziele

- Laufenden Schaden begrenzen
- Weitere Eskalation verhindern, ohne die Untersuchung unnötig offenzulegen

### Maßnahmen

- Nur bei akuter Gefahr sofort gezielte Einschränkungen, etwa Schreibrechte entziehen, Uploads zu privaten Diensten blockieren oder USB sperren
- Sonst Sperrung nach Regel 3 vorbereiten
- Zugriff auf besonders schutzwürdige Systeme prüfen und falls nötig unauffällig einschränken

### Nicht tun

- Die Person nicht vorzeitig konfrontieren
- Keine Zugänge ohne Abstimmung mit Personal und Recht entziehen, außer bei akuter Gefahr
- Keine privaten Geräte oder Cloud-Konten ohne Rechtsgrundlage durchsuchen
- Keine gezielten Fallen gegen eine Person ohne Freigabe von Recht und Datenschutz einsetzen

### Exit-Kriterien

- Keine akute Schadwirkung mehr
- Sperrung und Maßnahmen abgestimmt vorbereitet

## Phase 3: Untersuchen

### Ziele

- Verdacht belegen, entkräften oder als nicht klärbar dokumentieren
- Beweise gerichtsfest sichern

### Maßnahmen

- Nur schriftlich beauftragte Auswertungen durchführen
- Vier-Augen-Prinzip und vollständige Protokollierung jeder Abfrage
- Datenträger forensisch sichern und Originale versiegeln
- Private Inhalte, die zufällig gefunden werden, nicht weiter auswerten ohne Rücksprache
- Ergebnisse sachlich, neutral und inklusive entlastender Befunde dokumentieren

### Exit-Kriterien

- Verdacht bestätigt, entkräftet oder nicht klärbar
- Bericht an das Kernteam erstellt

## Phase 4: Maßnahmen und Beseitigung

### Ziele

- Entscheidungen des Kernteams umsetzen
- Missbrauch, Hintertüren und gemeinsame Risiken beseitigen

### Maßnahmen

- Bei Bestätigung Gespräch, Freistellung, Kündigung, zivil- oder strafrechtliche Schritte nach Entscheidung des Kernteams
- Zugänge zeitgleich entziehen, Geräte einsammeln
- Von der Person angelegte Konten, Weiterleitungen, Freigaben, geplante Aufgaben und Hintertüren entfernen
- Gemeinsam genutzte Geheimnisse erneuern
- Herausgabe oder Löschung mitgenommener Daten verlangen und dokumentieren

### Exit-Kriterien

- Entscheidung umgesetzt und dokumentiert
- Alle Zugänge entzogen, Geräte gesichert
- Gemeinsame Geheimnisse und Hintertüren beseitigt

## Phase 5: Wiederanlauf

### Ziele

- Integrität wiederherstellen
- Aufgabenübergang sicherstellen
- Nachlaufende Zugriffsversuche erkennen

### Maßnahmen

- Betroffene Daten und Systeme auf Integrität prüfen und gegebenenfalls wiederherstellen
- Aufgabenübergabe organisatorisch absichern
- Erhöhte Aufmerksamkeit auf Zugriffsversuche der Person für mindestens 30 Tage

### Exit-Kriterien

- Integrität betroffener Daten und Systeme bestätigt
- Aufgaben übergeben
- Keine weiteren Zugriffsversuche beobachtet oder bearbeitet

## Phase 6: Nachbereitung

### Maßnahmen

- Prüfen, ob Berechtigungskonzept, Austrittsprozess oder DLP den Vorfall hätten verhindern oder früher zeigen können
- Rechtsrahmen, Betriebsvereinbarungen und Richtlinien evaluieren
- Lehren in HR-, Security- und Compliance-Prozesse überführen
- Intern sachlich kommunizieren, ohne die Person unnötig bloßzustellen

## Zu sichernde Beweise

- Schriftlicher Hinweis und Untersuchungsauftrag des Kernteams
- Exportierte Logs und Audits für den beauftragten Zeitraum
- Forensische Images mit Hashwerten und Beweiskette
- Protokoll jeder Auswertung mit Zweck, Zeitpunkt und ausführender Person
- Entscheidungsprotokoll des Kernteams

## Nachweis und Aufbewahrung

- Beweise besonders restriktiv speichern und Need-to-know technisch durchsetzen
- Hashwerte, Zugriffshistorie, Exportzeitpunkt und Auswerter dokumentieren
- Aufbewahrung nach Rechtslage, Verfahrensbedarf und interner Policy festlegen

## Kommunikations-Checkliste

- Strikt need-to-know
- Keine Mails mit Namen und Verdacht an große Verteiler
- Kommunikation gegenüber Teams nur sachlich und ohne unnötige Personendetails
- Externe Kommunikation nur nach rechtlicher Prüfung
- Kommunikation mit ausgeschiedener Person über Personal oder Recht, nicht über IT

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

**Beschäftigtendatenschutz und Mitbestimmung**

Insider-Untersuchungen berühren Persönlichkeitsrechte. Rechtliche Begleitung ist immer erforderlich, ebenso die Abstimmung mit der Arbeitnehmervertretung, wo es eine gibt.

- **Deutschland:** § 26 BDSG ist seit dem EuGH-Urteil C-34/21 (2023) als allgemeine Rechtsgrundlage umstritten; die Zulässigkeit ist im Einzelfall auch nach Art. 6 und Art. 88 DSGVO zu prüfen. Zur Aufdeckung von Straftaten verlangt § 26 Abs. 1 Satz 2 BDSG einen dokumentierten Anfangsverdacht, Erforderlichkeit und Verhältnismäßigkeit. Technische Überwachungseinrichtungen sind mitbestimmungspflichtig (§ 87 Abs. 1 Nr. 6 BetrVG). Das Geschäftsgeheimnisgesetz (GeschGehG) schützt nur Informationen, für die angemessene Geheimhaltungsmaßnahmen getroffen wurden. Die Identität hinweisgebender Personen ist nach dem Hinweisgeberschutzgesetz vertraulich zu behandeln.
- **Österreich:** Kontrollmaßnahmen, die die Menschenwürde berühren, bedürfen der Zustimmung des Betriebsrats (§ 96 Abs. 1 Z 3 ArbVG); bestimmte Systeme zur Verarbeitung von Beschäftigtendaten ebenfalls (§ 96a ArbVG). Daneben gelten DSGVO und DSG.
- **Schweiz:** Überwachungssysteme, die das Verhalten der Arbeitnehmenden überwachen sollen, sind unzulässig (Art. 26 ArGV 3); Bearbeitung von Personendaten nach DSG und Art. 328b OR nur, soweit sie das Arbeitsverhältnis betrifft.
- **Private Nutzung:** Ob dienstliche Postfächer und Geräte privat genutzt werden dürfen, bestimmt wesentlich, was ausgewertet werden darf.

**Meldepflichten**

- Sind personenbezogene Daten Dritter abgeflossen, gelten die Datenschutz-Meldepflichten oben.
- Sabotage an Systemen einer NIS2- oder ISG-pflichtigen Organisation kann ein meldepflichtiger Sicherheitsvorfall sein.
- **Strafanzeige:** Entscheidung durch Kernteam und Geschäftsführung. Deutschland: Zentrale Ansprechstelle Cybercrime (ZAC) des jeweiligen Landeskriminalamts; Österreich: Bundeskriminalamt bzw. jede Polizeidienststelle; Schweiz: Polizei des Kantons.

## Quick-Response-Checkliste

- [ ] Hinweis schriftlich dokumentiert
- [ ] Keine eigenmächtigen Nachforschungen begonnen
- [ ] Kernteam aus Personal, Recht, Datenschutz und Security einberufen
- [ ] Übernommenes Konto ausgeschlossen oder separat behandelt
- [ ] Akute Gefahr bewertet
- [ ] Beweissicherung ohne Auswertung veranlasst
- [ ] Schriftlicher Untersuchungsauftrag vorhanden
- [ ] Betriebs- oder Personalrat nach Vorgabe beteiligt
- [ ] Auswertung im Vier-Augen-Prinzip
- [ ] Sperrung zeitgleich mit Gespräch vorbereitet
- [ ] Gemeinsame Geheimnisse erneuert
- [ ] Meldepflichten geprüft

## Präventive Kontrollen

- Need-to-know-Berechtigungen mit regelmäßiger Rezertifizierung
- Geordneter Austrittsprozess
- DLP für besonders schutzwürdige Daten, soweit rechtlich abgestimmt
- Protokollierung von Zugriffen auf sensible Daten mit klar geregelter Auswertung
- Getrennte Admin-Konten und Vier-Augen-Prinzip für kritische Änderungen
- Klare Regeln zur privaten Nutzung von IT
- Vertrauenswürdiger Meldeweg und Hinweisgebersystem
- Vorab abgestimmter Ablauf für Insider-Untersuchungen mit Personal, Recht, Datenschutz und Arbeitnehmervertretung

## Kompakter Einstieg

Dieser Block ist kein verkürztes Ersatz-Playbook. Er dient als schneller Einstieg; für die Bearbeitung des Vorfalls gelten alle Abschnitte dieses Dokuments.

**Insider-Vorfall**  
Reaktion auf Datenmitnahme, Sabotage und Rechtemissbrauch durch Personen mit legitimem Zugang — mit Personal, Recht, Datenschutz und Mitbestimmung, verhältnismäßig, vertraulich und mit gerichtsfester Beweissicherung.

**Kernphasen:** Erkennen und Bewerten, Eindämmen, Untersuchen, Maßnahmen, Wiederanlauf, Nachbereitung.

## Referenzen

- NIST SP 800-61 Rev. 3
- CISA: Insider Threat Mitigation Guide
- CERT/CC: Common Sense Guide to Mitigating Insider Threats
- MITRE ATT&CK: T1567, T1052, T1485

## Disclaimer

Dieses Playbook ist eine praktische Community-Ressource. Es ersetzt keine Rechtsberatung, arbeitsrechtliche Prüfung oder organisationsspezifische Verfahren. Insider-Untersuchungen berühren Persönlichkeitsrechte und erfordern rechtliche Begleitung.
