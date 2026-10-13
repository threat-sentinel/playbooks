# Playbook: DDoS-Angriff

## Zusammenfassung

Dieses Playbook strukturiert die Reaktion auf einen Distributed-Denial-of-Service-Angriff gegen Websites, APIs, VPN-Zugänge, DNS, Mailserver oder ganze Internetanbindungen. Es deckt auch Erpressungen mit DDoS-Androhung ab.

DDoS ist in erster Linie ein **Verfügbarkeits- und Koordinationsproblem**. Die wirksamsten Gegenmaßnahmen liegen häufig beim Internetanbieter, CDN oder DDoS-Schutzdienstleister und nicht allein in der eigenen Infrastruktur.

Der Kern dieses Playbooks: **Zuerst klären, ob es ein Angriff ist und auf welcher Ebene er stattfindet, dann die richtige Stelle mit den richtigen Daten einschalten.** Ein DDoS kann außerdem Ablenkung für einen parallelen Einbruch sein.

## Zweck

- Angriff von technischer Störung, Lastspitze oder Fehlkonfiguration unterscheiden
- Angriffsart und -ebene bestimmen
- Abwehr schnell über Anbieter und eigene Maßnahmen aktivieren
- Kritische Dienste priorisiert verfügbar halten
- Begleitende Angriffe erkennen
- Erpressungsversuche richtig behandeln

## Anwendungsbereich

Verwenden Sie dieses Playbook, wenn:

- öffentlich erreichbare Dienste langsam oder nicht erreichbar sind und Verkehr oder Anfragen ungewöhnlich hoch sind
- Internetanbieter, CDN oder Schutzdienstleister einen Angriff melden
- Firewall, Load Balancer oder Webserver Bandbreite, CPU, Verbindungen oder Fehlerraten am Limit melden
- eine Erpressungsnachricht mit DDoS-Drohung eingeht, auch ohne bisherigen Angriff
- Hacktivisten-Gruppen Ihre Organisation, Branche oder Region öffentlich als Ziel nennen

**Abgrenzung:**

- Ausfälle durch eigene Änderungen, Anbieterstörungen oder legitime Lastspitzen sind kein Angriff, werden aber in Phase 1 mit diesem Playbook systematisch ausgeschlossen.
- Wenn ein DDoS nur Deckung für einen Einbruch ist, führt zusätzlich das passende Incident-Playbook.

## Schweregrad

- **Mittel:** Angriff wird von bestehenden Schutzmaßnahmen abgefangen, keine spürbare Beeinträchtigung
- **Hoch:** Spürbare Beeinträchtigung oder Ausfall nicht kritischer Dienste oder glaubhafte Erpressung
- **Kritisch:** Ausfall geschäftskritischer Dienste, Ausfall der gesamten Internetanbindung oder Hinweise auf gleichzeitigen Einbruch

## Incident-Ziele

1. Feststellen, ob ein Angriff vorliegt und auf welcher Ebene.
2. Abwehr über Anbieter und eigene Maßnahmen aktivieren.
3. Kritische Dienste priorisiert wiederherstellen oder stabil eingeschränkt betreiben.
4. Begleitende Angriffe erkennen.
5. Kunden, interne Stakeholder und Partner verlässlich informieren.
6. Schutzmaßnahmen dauerhaft verbessern.

## Zentrale Reaktionsprinzipien

- **Anbieter früh einschalten.** Gegen volumetrische Angriffe hilft fast nur, was vor der eigenen Leitung geschieht.
- **Daten liefern, nicht nur den Ausfall melden.** Ziel-IP, Domain, Zeitpunkt, Ebene, Beispiele und bereits getroffene Maßnahmen beschleunigen jede Eskalation.
- **Prioritäten vorher kennen.** Nicht jeder Dienst ist gleich wichtig.
- **Nicht blind blocken.** Pauschale Geo- oder IP-Sperren treffen oft legitime Nutzer und verschieben das Problem.
- **Wachsam für Ablenkung.** Während Verfügbarkeit im Fokus steht, können Einbrüche unbemerkt bleiben.
- **Nicht zahlen.** Zahlungen an DDoS-Erpresser verhindern weitere Angriffe nicht.
- **Zeit konsistent erfassen.** Angriffsbeginn, Wellen und Eskalationen grundsätzlich in UTC dokumentieren.

## Kontakte und Eskalation

Diese Angaben organisationsspezifisch vorbelegen und offline verfügbar halten:

- Incident-Lead on call
- ISP oder Carrier NOC
- CDN-, WAF- und DDoS-Schutzdienstleister
- DNS-Provider, Registrar und Zertifikatsverantwortliche
- Plattform-, Netzwerk- und Anwendungsbetrieb
- Kundenservice, Kommunikation und Legal
- Polizei bzw. Cybercrime-Ansprechstelle: ZAC des Landeskriminalamts (DE), Bundeskriminalamt (AT), Kantonspolizei (CH)
- Meldestellen: BSI (DE), zuständiges CSIRT (AT), BACS (CH)

## Die ersten 15 Minuten

- Incident-Lead benennen, Timeline und Entscheidungsprotokoll starten.
- Betroffene Dienste, Ziel-IP-Adressen, Domains und Zeitpunkt der Störung festhalten.
- Schnell prüfen, ob eine geplante Änderung, bekannte Anbieterstörung oder legitime Lastspitze vorliegt.
- Monitoring auswerten: Bandbreite, Pakete pro Sekunde, Verbindungen, Anfragen pro Sekunde, Fehlerraten, Quell-Länder, Quell-ASNs, häufigste URLs und User-Agents.
- CDN- oder Schutzdienst prüfen: Ist Schutz aktiv, sieht der Anbieter den Angriff, muss ein Notfallmodus aktiviert werden?
- Bei Überlastung der eigenen Anbindung ISP oder Carrier über die NOC- oder Notfallnummer kontaktieren.
- Security-Monitoring für parallele Auffälligkeiten ausdrücklich weiterführen lassen.
- Kommunikation für Statusseite, Support und interne Informationen vorbereiten.

## Rollen und Verantwortlichkeiten

### Incident-Lead

- Steuert den Vorfall.
- Priorisiert Dienste und Maßnahmen.
- Entscheidet über weitreichende Schritte wie Geo-Blocking, Challenge-Modi oder Blackholing.

### Netzwerk und Betrieb

- Analysieren Verkehr.
- Setzen Filter, Rate Limits und Schutzregeln um.
- Halten Kontakt zu ISP, CDN und DDoS-Schutzdienstleister.

### Anwendungsverantwortliche

- Bewerten Anwendungslast.
- Aktivieren Caching, Degradation, Queueing oder Abschaltung teurer Funktionen.

### Security / SOC

- Überwachen auf Begleitangriffe.
- Bewerten Erpressungsnachrichten und verdächtige Logins.

### Kommunikation und Kundenservice

- Pflegen Statusseite.
- Informieren Kunden, interne Stakeholder und Supportkanäle.

### Externe

- ISP, Carrier, CDN, DDoS-Schutzdienstleister, DNS-Provider, CERT und gegebenenfalls Polizei

## Entscheidungsmodell

### Regel 1: Angriff oder Störung?

**Wahrscheinlich Angriff**, wenn:

- Verkehr oder Anfragen sprunghaft und ohne geschäftlichen Anlass steigen
- viele Anfragen dieselben Merkmale tragen, etwa gleiche URL, User-Agent oder Protokollmuster
- Verkehr aus ungewöhnlichen Ländern, Netzen oder Reflexionsquellen stammt
- eine Drohung, ein Bekennerschreiben oder öffentliche Zielankündigung vorliegt

**Wahrscheinlich Störung**, wenn:

- kurz vor dem Vorfall Änderungen umgesetzt wurden
- der Anbieter eine Störung bestätigt
- der Verkehr normal ist, aber Backends oder interne Abhängigkeiten langsam sind

### Regel 2: Auf welcher Ebene?

| Ebene | Typische Merkmale | Wirksamste Gegenmaßnahme |
|---|---|---|
| Volumetrisch (L3/L4) | Leitung voll, viel UDP, Reflexionsverkehr, hohe Gbit/s oder PPS | ISP-Filterung, Scrubbing-Center, CDN |
| Protokoll (L4) | SYN-Flood, Connection-Tabellen voll, wenig Bandbreite bei hoher Verbindungszahl | Firewall- und Load-Balancer-Schutz, SYN-Cookies, Schutzdienst |
| Anwendung (L7) | Viele scheinbar legitime HTTP-Anfragen auf teure Funktionen | WAF-Regeln, Challenges, Rate Limits, Caching, Funktionsdegradation |

### Regel 3: Geo-Blocking oder breite Sperren?

Nur anwenden, wenn:

- die Angriffsquellen klar regional konzentriert sind
- dort keine relevanten Kunden, Partner oder Mitarbeiter sitzen
- die Maßnahme befristet, dokumentiert und fachlich abgestimmt ist

Gezielte Regeln auf Merkmale des Angriffsverkehrs sind fast immer besser.

### Regel 4: Erpressung?

- Nicht zahlen und nicht verhandeln.
- Nachricht vollständig sichern: Header, Wallet-Adresse, Zeitangaben, Fristen und Wortlaut.
- Schutzdienstleister und ISP vorsorglich vorwarnen.
- Polizei oder Cybercrime-Ansprechstelle informieren.
- Viele Drohungen bleiben technisch begrenzt, Vorbereitung ist trotzdem Pflicht.

### Regel 5: Wann andere Playbooks aktivieren?

- **Identitätskompromittierung parallel**, wenn während des Angriffs verdächtige Logins, MFA-Auffälligkeiten oder Admin-Zugriffe sichtbar werden.
- **Ransomware-Playbook oder das zum Einbruch passende Playbook führend**, wenn der DDoS erkennbar Ablenkung für einen Einbruch ist.
- **Datenpannen-Playbook parallel**, wenn gleichzeitig Datenabfluss oder Missbrauch personenbezogener Daten sichtbar wird.

## Service-Priorisierung

| Dienstklasse | Ziel im Angriff | Typische Maßnahme |
|---|---|---|
| Kritische Transaktionen | Maximale Verfügbarkeit | Scrubbing, CDN/WAF-Notfallmodus, Priorisierung |
| Kundenportal | Degradiert weiterbetreiben | Caching, Challenge, teure Funktionen begrenzen |
| Öffentliche Informationsseiten | Verlässliche Kommunikation | Statische CDN-Seite, unabhängige Statusseite |
| Nicht kritische APIs | Last reduzieren | Temporäre Limits, Queueing, restriktive Rate Limits |
| Admin- und Fernzugänge | Missbrauch verhindern | Separater Zugang, starke Authentisierung, restriktive Regeln |

## Schutz kritischer Vertrauenskonten

Während eines DDoS-Angriffs besonders absichern:

- DNS-Provider-Konten
- Registrar-Zugänge und Registry Lock, falls verfügbar
- CDN- und WAF-Admin-Konten
- Zertifikatsverwaltung und CA- oder ACME-Konten
- Break-Glass-Zugänge für Internet-, DNS- und CDN-Betrieb

## Phase 1: Erkennen und Triage

### Ziele

- Angriff von Störung abgrenzen
- Ebene und Ziel bestimmen
- Eskalation mit belastbaren Daten auslösen

### Maßnahmen

- Angriff versus Störung nach Regel 1 bewerten.
- Ebene nach Regel 2 bestimmen.
- Betroffene Dienste, Zielsysteme und mögliche Origin-Exposition erfassen.
- Verkehrsdaten sichern: NetFlow, Firewall-Statistiken, Beispielpakete, Webserver-Logs, CDN- und WAF-Auswertungen.
- Prüfen, ob die Origin-IP bekannt oder direkt angreifbar ist, obwohl ein CDN vorgeschaltet ist.

### Datenpaket für ISP und Schutzdienstleister

Ein Provider kann nur so schnell helfen, wie die Lage technisch greifbar ist. Diese Angaben deshalb möglichst als Standardvorlage vorbereiten:

- Betroffene Ziel-IP-Adressen, Präfixe und Domains
- Beginn des Angriffs in UTC und erkennbare Wellen
- Bandbreite, Pakete pro Sekunde, bei L7 Anfragen pro Sekunde
- Protokolle und Ports
- Beispielhafte Quell-IP-Adressen, Quellports, Paketgrößen
- Bei L7: URLs, Methoden, Header- oder User-Agent-Muster
- Bereits getroffene Maßnahmen
- Gewünschtes Ergebnis: filtern, scrubben, challengen, begrenzen oder im äußersten Fall blackholen
- Geschäftskritische Dienste, die geschützt oder zuerst wiederhergestellt werden müssen
- Vertrags- oder Kundennummer und erreichbare technische Kontaktperson

### Exit-Kriterien

- Angriff bestätigt oder mit belastbarer Begründung ausgeschlossen
- Ebene und betroffene Ziele bekannt
- ISP oder Schutzdienstleister informiert
- Erstes Lagebild dokumentiert

## Phase 2: Eindämmen

### Ziele

- Kritische Dienste stabilisieren
- Angriffsfläche und Auswirkungen verkleinern
- Parallele Angriffe nicht übersehen

### Maßnahmen

- Schutzdienst oder CDN auf Notfallregeln, Challenge-Modus und Rate Limits umstellen.
- Beim ISP Filterung oder Umleitung über Scrubbing veranlassen.
- Eigene Infrastruktur mit Rate Limits, Connection-Limits und gezielten Merkmalssperren schützen.
- Teure Funktionen wie Suche, Login oder Exporte begrenzen oder hinter Challenge legen.
- Eine statische Notfallseite über CDN oder unabhängige Statusseite bereitstellen.
- Origin-IP schützen; wenn nötig Origin wechseln und nur Verkehr vom Schutzdienst zulassen.
- Bei Cloud-Autoscaling Budgetgrenzen und Obergrenzen prüfen.
- Bei Angriffen auf autoritativen DNS sekundäre Zonen oder robusten DNS-Anbieter aktivieren.
- Kritische interne Dienste bei Bedarf über alternative Wege bereitstellen.
- Blackholing nur als letztes Mittel anwenden.

### Nicht tun

- Keine dauerhaften pauschalen Sperren ohne Dokumentation
- Security-Monitoring nicht zurückfahren
- Angreifer nicht öffentlich provozieren

### Exit-Kriterien

- Kritische Dienste wieder erreichbar oder stabil degradiert
- Schutzmaßnahmen aktiv und dokumentiert
- Monitoring auf Begleitangriffe läuft weiter

## Phase 3: Untersuchen

### Ziele

- Angriffsverlauf verstehen
- Begleitende Angriffe erkennen oder ausschließen
- Ursachen wirksamer machen als nur situative Abwehr

### Maßnahmen

- Verlauf dokumentieren: Beginn, Spitzen, Wellen, Methodikwechsel und Reaktionszeiten.
- Prüfen, ob parallel verdächtige Anmeldungen, Admin-Aktionen, Malware-Befunde oder Datenabflüsse auftraten.
- Ermitteln, wie Origin-IP oder Zielinfrastruktur bekannt wurden.
- Bekennerschreiben, Hacktivisten-Kanäle und Branchenwarnungen auswerten.
- Prüfen, ob der Angriff auf Verfügbarkeit beschränkt war oder als Ablenkung diente.

### Exit-Kriterien

- Angriffsverlauf dokumentiert
- Begleitangriff ausgeschlossen oder an anderes Playbook übergeben
- Erkenntnisse für dauerhafte Schutzmaßnahmen festgehalten

## Phase 4: Beseitigen

DDoS hinterlässt in der Regel keine Persistenz. Beseitigung bedeutet hier, die Bedingungen zu schließen, die den Angriff wirksam gemacht haben.

### Maßnahmen

- Origin-Exposition beheben
- Bewährte WAF- und Rate-Limit-Regeln dauerhaft übernehmen
- Kapazitätsengpässe und Single Points of Failure adressieren
- Schutzregeln, Routing oder DNS-Design verbessern

### Exit-Kriterien

- Origin nur noch kontrolliert erreichbar
- Sinnvolle Notfallregeln in Dauerkonfiguration übernommen
- Bekannte Schwachstellen im Betriebsmodell adressiert

## Phase 5: Wiederanlauf

### Ziele

- Notfallmaßnahmen kontrolliert zurückfahren
- Wiederaufflammen früh erkennen

### Maßnahmen

- Notfallregeln schrittweise zurücknehmen und Wirkung überwachen.
- Erhöhte Bereitschaft für mindestens 72 Stunden nach letztem Angriffsverkehr aufrechterhalten.
- Statusseite aktualisieren.
- Interne und externe Stakeholder erst nach stabiler Phase entwarnen.

### Exit-Kriterien

- 72 Stunden ohne erneuten Angriffsverkehr oder stabile Abwehr
- Notfallregeln geprüft zurückgenommen oder bewusst beibehalten
- Kommunikation aktualisiert

## Phase 6: Nachbereitung

### Maßnahmen

- Zeit bis Erkennung, Eskalation und wirksamer Abwehr messen.
- Prüfen, ob Notfallkontakte, Verträge und Eskalationswege aktuell und erreichbar waren.
- Vertraglichen Schutz hinsichtlich Bandbreite, L7-Abwehr und Reaktionszeiten bewerten.
- Runbooks, Schutzregeln und Kommunikationsvorlagen aktualisieren.
- DDoS-Ablauf regelmäßig üben.

## Zu sichernde Beweise

- Monitoring-Grafiken und Rohdaten zu Bandbreite, PPS, Anfragen und Fehlerraten
- NetFlow-, Firewall- und Load-Balancer-Statistiken
- Webserver-, CDN- und WAF-Logs mit Beispielanfragen
- Kommunikation mit ISP, Carrier und Schutzdienstleister inklusive Ticketnummern
- Erpressungsnachrichten mit vollständigen Headern
- Timeline und Entscheidungsprotokoll

## Kommunikations-Checkliste

- Statusseite früh, sachlich und ohne Täter-Spekulation aktualisieren
- Kundenservice mit abgestimmten Antworten versorgen
- Intern kommunizieren, welche Dienste betroffen sind und welche Alternativen bestehen
- Bei Erpressung keine öffentliche Aussage über Forderungen ohne Abstimmung mit Legal und Polizei

### Vorlage: Statusmeldung

> Wir beobachten derzeit eine erhöhte Last auf [Dienst], die zu Verzögerungen und Ausfällen führen kann. Unser Team und unsere Dienstleister arbeiten an der Abwehr. Nach aktuellem Kenntnisstand liegen keine bestätigten Hinweise auf eine Beeinträchtigung der Vertraulichkeit oder Integrität vor; die Untersuchung läuft. Nächstes Update: [Uhrzeit].

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

- Ein DDoS ohne spürbare Auswirkung ist in der Regel nicht meldepflichtig. Ein erheblicher Ausfall wichtiger Dienste kann es nach NIS2 und ISG sein.
- Ein reiner Verfügbarkeitsangriff ist meist keine Datenschutzverletzung. Die DSGVO-Bewertung wird nötig, wenn Daten nicht verfügbar waren, auf die Betroffene angewiesen sind, oder wenn ein Begleitangriff festgestellt wird.
- **Strafanzeige:** Deutschland: Zentrale Ansprechstelle Cybercrime (ZAC) des jeweiligen Landeskriminalamts; Österreich: Bundeskriminalamt bzw. jede Polizeidienststelle; Schweiz: Polizei des Kantons.
- SLAs gegenüber Kunden und Ansprüche gegenüber ISP, CDN oder Schutzdienstleister prüfen.

## Quick-Response-Checkliste

- [ ] Angriff oder Störung bewertet
- [ ] Ebene bestimmt: volumetrisch, Protokoll oder Anwendung
- [ ] Betroffene Dienste und Ziele erfasst
- [ ] Schutzdienst oder CDN aktiviert beziehungsweise verschärft
- [ ] ISP informiert, wenn die Leitung betroffen ist
- [ ] Origin-Exposition geprüft
- [ ] Security-Monitoring auf Begleitangriffe aktiv
- [ ] Statusseite und Kundenservice informiert
- [ ] Erpressungsnachricht gesichert, nicht gezahlt, Behördenweg geprüft
- [ ] Erhöhte Bereitschaft für 72 Stunden geplant

## Präventive Kontrollen

- DDoS-Schutz für alle geschäftskritischen öffentlichen Dienste
- Dokumentierte und getestete Notfallkontakte von ISP, Carrier und Schutzdienstleister
- Origin-Server nur über Schutzdienst erreichbar
- Robuster DNS-Anbieter mit sinnvoller Architektur und TTL-Strategie
- Rate Limits für teure Funktionen und Logins
- Unabhängige Statusseite
- Priorisierte Liste kritischer Dienste mit Verantwortlichen
- Regelmäßige Übungen für DDoS-Abläufe
- Schutz hochprivilegierter Vertrauenskonten für DNS, CDN und Zertifikate

## Kompakter Einstieg

Dieser Block ist kein verkürztes Ersatz-Playbook. Er dient als schneller Einstieg; für die Bearbeitung des Vorfalls gelten alle Abschnitte dieses Dokuments.

**DDoS-Angriff**  
Reaktion auf Überlastungsangriffe und DDoS-Erpressung: Angriff von Störung unterscheiden, Ebene bestimmen, ISP und Schutzdienstleister mit belastbaren Daten einschalten, kritische Dienste priorisieren und auf Begleitangriffe achten.

**Kernphasen:** Erkennen, Eindämmen, Untersuchen, Beseitigen, Wiederanlauf, Nachbereitung.

## Referenzen

- NIST SP 800-61 Rev. 3
- CISA, FBI, MS-ISAC: Understanding and Responding to Distributed Denial-of-Service Attacks
- BSI-Empfehlungen zur Abwehr von DDoS-Angriffen
- ENISA Threat Landscape zu Availability Threats
- MITRE ATT&CK: T1498, T1499

## Disclaimer

Dieses Playbook ist eine praktische Community-Ressource. Es ersetzt keine Rechtsberatung, keine Vorgaben Ihrer Dienstleister und keine organisationsspezifische Incident-Response-Planung.
