# Phishing- & Business-Email-Compromise-Playbook

## Zusammenfassung

Dieses Playbook bietet ein operatives, community-orientiertes Incident-Response-Verfahren für vermutete oder bestätigte Phishing-, Credential-Theft-, Mailbox-Kompromittierungs- und Business-Email-Compromise-(BEC)-Vorfälle.
Es hilft Incident Respondern, laufenden Missbrauch zu stoppen, Identitäten und Kommunikationskanäle abzusichern, Beweise zu sichern und geschäftliche Auswirkungen wie Datenabfluss oder betrügerische Zahlungen zu minimieren.

Das Playbook folgt einem standardisierten IR-Ablauf: Detect & Triage, Contain, Investigate, Eradicate, Recovery und Review. Es richtet sich an Security-Teams, IT-Administratoren, SOC-Analysten, Berater sowie kleine und mittlere Organisationen.

> Hinweis: Dies ist keine Rechtsberatung und kein Ersatz für spezialisierte forensische Arbeit. Passen Sie es an Ihren Mail-, Identity- und Endpoint-Stack, an rechtliche Verpflichtungen und an interne Rollen an.

## Zweck

- Einen strukturierten Reaktionsprozess für Phishing- und BEC-Vorfälle bereitstellen.
- Schnelle Validierung, Eindämmung und Beweissicherung ermöglichen.
- Den Umfang über Mail, Identität, Cloud, Endpunkte und Geschäftsprozesse hinweg bestimmen.
- Angreiferzugänge und unsichere Konfigurationen entfernen.
- Einen vertrauenswürdigen Konten- und Kommunikationszustand wiederherstellen.
- Dokumentierte Lessons Learned und nachverfolgte Remediation erzeugen.

## Anwendungsbereich / Wann verwenden

Verwenden Sie dieses Playbook, wenn einer oder mehrere der folgenden Punkte vorliegen:

- Ein Benutzer meldet eine verdächtige E-Mail oder einen verdächtigen Link.
- Ein Benutzer hat auf einen mutmaßlichen Phishing-Link geklickt oder einen Anhang geöffnet.
- Zugangsdaten wurden möglicherweise auf einer mutmaßlichen Phishing-Seite eingegeben.
- Eine Mailbox zeigt ungewöhnliche Anmeldeaktivität.
- Unerwartete MFA-Prompts oder Bestätigungen werden gemeldet.
- Verdächtige Inbox-Regeln, Weiterleitungen oder Delegationen werden gefunden.
- Unautorisierte E-Mails wurden von einem legitimen Konto versendet.
- Lieferanten- oder Kundenkontakte melden Nachrichten, die scheinbar aus Ihrer Organisation stammen.
- Betrügerische Zahlungsanweisungen, Gehaltsumleitungen oder Änderungen an Lieferanten-Bankdaten werden festgestellt.
- Eine kompromittierte Mailbox wird für Identitätsmissbrauch verwendet.

Dies deckt klassische E-Mail-Phishing-Angriffe und moderne Cloud-/Identity-Missbrauchsfälle ab (OAuth-Consent-Abuse, Token-/Session-Missbrauch), bei denen nicht zwingend Malware beteiligt ist.

## Schweregrad

Standard-Schweregrad: **Hoch**.

Auf **Kritisch** eskalieren, wenn einer der folgenden Punkte zutrifft:

- Privilegierte/Admin-/Executive-/Finance-/HR-/Systemkonten sind betroffen.
- Die Mailbox wird verwendet, um intern oder extern weitere Phishing-E-Mails zu versenden.
- Zahlungsbetrug oder bestätigter finanzieller Schaden ist eingetreten oder unmittelbar wahrscheinlich.
- Mehrere Konten, Tenants oder Cloud-Identitäten sind betroffen.
- Anzeichen für Datenzugriff, Exfiltration oder Missbrauch sensibler Kommunikation liegen vor.
- Persistenter Zugriff über OAuth, Weiterleitungen, Inbox-Regeln oder Tokens bleibt nach einem Passwortwechsel bestehen.

## Incident-Ziele (erste Stunden)

1. Laufenden Missbrauch stoppen.
2. Identitäten, Sessions und Kommunikationskanäle absichern.
3. Beweise sichern und eine belastbare Timeline führen.
4. Umfang, initialen Angriffsvektor und Business-Impact bestimmen.
5. Finanzielle, datenschutzrechtliche und operative Schäden minimieren.
6. Den Vorfall mit nachverfolgter Remediation und Lessons Learned abschließen.

## Zentrale Reaktionsprinzipien

- Behandeln Sie vermuteten Identitätsmissbrauch als Identity-First-Thema: Prüfen Sie Sessions/Tokens, bevor Sie von einer reinen Passwortlösung ausgehen.
- Trennen Sie bestätigte Fakten von Hypothesen; dokumentieren Sie Entscheidungen in der Timeline.
- Sichern Sie volatile Beweise vor destruktiver Remediation.
- Zentralisieren Sie die Kommunikation und binden Sie Finance bei BEC-Risiken früh ein.
- Beziehen Sie bei Cloud-Identitäten Tokens, OAuth-Consents und App-Permissions mit ein.
- Verlassen Sie sich nicht auf einen Passwortwechsel allein, solange persistente Sessions und Consents nicht invalidiert wurden.
- Suchen Sie tenantweit schnell nach weiteren betroffenen Benutzern.

---

## Erste 15 Minuten (Sofortmaßnahmen)

- Incident-Response-Lead / Incident Commander aktivieren.
- Eine Incident-Timeline und ein Entscheidungsprotokoll starten (wer, was, wann, warum).
- Das betroffene Konto bzw. die betroffene Mailbox, das Gerät und das gemeldete Phishing-Objekt identifizieren.
- Feststellen, ob Links angeklickt, Anhänge geöffnet oder Zugangsdaten eingegeben wurden.
- Verdächtige Sessions/Tokens widerrufen, wenn eine Kompromittierung wahrscheinlich ist.
- Die Originalnachricht(en) mit vollständigen Headern, URLs, Anhang-Hashes und Screenshots sichern.
- Nach ähnlichen Nachrichten im Tenant suchen.
- Finance / Legal / Management sofort benachrichtigen, wenn Zahlungen, Payroll oder Executive-Impersonation betroffen sind.
- Mailbox-Regeln, Weiterleitungen, Delegationen und OAuth-App-Consents prüfen.
- Eindämmungsmaßnahmen vorbereiten (E-Mails quarantänisieren, URLs blockieren, tenantweite Suche).

Nicht tun:
- Die Originalnachricht löschen, bevor Beweise gesichert wurden.
- Davon ausgehen, dass der Vorfall nur den meldenden Benutzer betrifft.
- Passwörter zurücksetzen, ohne Sessions/Tokens und Persistenz zu berücksichtigen.
- Mailbox-Inhalte vertrauen, bevor Regeln, Delegationen und Sent Items geprüft wurden.
- Root Cause, Datenexposition oder finanziellen Schaden extern kommunizieren, bevor dies verifiziert ist.

---

## Rollen & Verantwortlichkeiten

- Incident Commander: Gesamtkoordination, Schweregradentscheidungen, Eskalation an das Management.
- Technical Lead: Koordiniert E-Mail-, Identity-, Endpoint-, Netzwerk- und Cloud-Responder sowie die technische Hypothese.
- Identity & Access Lead: Untersucht Session-/Token-Missbrauch, MFA, OAuth-Consents und führt Kontomaßnahmen durch.
- Email / Collaboration Lead: Analysiert Header/Regeln/Weiterleitungen, quarantänisiert/entfernt Nachrichten.
- Endpoint/Security Analyst: Prüft Endpoint-Telemetrie auf Malware- oder Credential-Theft-Artefakte.
- Forensics Lead: Sichert und validiert Beweise sowie den Angriffsweg.
- Communications Lead: Koordiniert interne/externe Kommunikation und verhindert inkonsistente Aussagen.
- Legal / Privacy / Compliance: Prüft regulatorische und Meldepflichten.
- Finance / Business Owner: Prüft/stoppt Zahlungsprozesse und validiert Lieferanten- oder Bankdatenänderungen.
- Externe IR / Versicherung / Strafverfolgung: Wird gemäß Richtlinie und Vorfallschwere eingebunden.

---

## Entscheidungsmodell (schnelle Regeln)

### Regel 1 — Konto sperren oder beobachten?
Konto sofort einschränken oder sperren, wenn:
- Zugangsdaten auf einer Phishing-Seite eingegeben wurden.
- Verdächtige erfolgreiche Anmeldungen, Token-Missbrauch oder Kontoübernahme-Indikatoren vorliegen.
- Die Mailbox verdächtige Nachrichten versendet oder Manipulationen an Regeln/Weiterleitungen zeigt.
- Das Konto privilegiert, Executive- oder Finance-bezogen ist.

Beobachten nur, wenn:
- Kein erfolgreicher Missbrauch sichtbar ist UND eine Sperrung die Beweissicherung erheblich beeinträchtigen würde UND der Incident Commander zustimmt.

### Regel 2 — Reicht ein Passwort-Reset?
Nein, wenn:
- Sessions/Tokens weiterhin aktiv sind.
- OAuth-Consents / App-Permissions fortbestehen.
- Inbox-Regeln, Weiterleitungen oder Delegationen hinzugefügt wurden.
- Angreifer Persistenz oder sekundäre Zugriffspfade eingerichtet haben.

In diesen Fällen auch Sessions widerrufen, Tokens rotieren und App-Consents entfernen.

### Regel 3 — Ist Endpoint-Forensik erforderlich?
Ja, wenn:
- Ein Anhang geöffnet oder eine ausführbare Datei gestartet wurde.
- Browser-basierte Credential-Eingabe mit gespeicherten Sessions oder Zugangsdaten vermutet wird.
- Der Endpoint Hinweise auf Infostealer/Downloader zeigt.
- Der Benutzer lokale Administratorrechte oder Zugang zu sensiblen Ressourcen hat.

Nein (Fokus auf Cloud/Identity), wenn:
- Hinweise ausschließlich auf OAuth-/Session-Missbrauch oder Credential Submission ohne lokale Ausführung deuten.

### Regel 4 — Finanzprozess sofort stoppen?
Zahlungsprozesse sofort stoppen/anhalten, wenn:
- Zahlungsanweisungen, Lieferanten-Bankänderungen oder Payroll-Umleitungen verdächtig sind.
- Eine Freigabe ansteht oder kurz vor Ausführung steht.
- Kommunikationsverläufe manipulierte Anweisungen zeigen.

### Regel 5 — Können wir jetzt wiederherstellen?
Wiederherstellung nur, wenn:
1. Der Angriffsweg verstanden ist.
2. Konten, Sessions, Tokens, Weiterleitungen und OAuth-Consents bereinigt sind.
3. Keine Hinweise auf fortbestehenden Missbrauch vorliegen.
4. Monitoring und Kommunikation für Folgewellen aktiv sind.

---

## Identity-First-Fokus

Phishing/BEC-Vorfälle sind häufig Identitätsvorfälle. Priorisieren Sie:
- Erfolgreiche/fehlgeschlagene Anmeldungen, Standort-/Geräte-Anomalien.
- MFA-Ereignisse: Bestätigungen, Ablehnungen, Fatigue-Muster, neue Faktorregistrierungen.
- Session-/Refresh-Token-Aktivität und Widerrufe.
- Passwort-Resets und Änderungen an Recovery-Optionen.
- Privilegierte und geschäftskritische Konten (Executive, Finance, HR, Admin).
- OAuth-Consents, Enterprise-Apps und App-Permissions.

---

## Schutz von E-Mail und Kommunikation

Angreifer missbrauchen oft die Mailbox-Konfiguration, nicht nur Anmeldungen. Priorisieren Sie:
- Inbox-Regeln, Weiterleitungen und Delegationen.
- Tenantweite Suche nach ähnlichen Nachrichten.
- Entfernung/Quarantänisierung bösartiger Nachrichten aus Mailboxen (Beweise vorher sichern).
- Spoofing-/Reply-To-Anomalien und Header-Prüfungen.
- Externe Empfänger benachrichtigen, wenn die kompromittierte Mailbox bösartige Nachrichten gesendet hat.
- Nachrichtenverläufe auf Fraud-Indikatoren prüfen (Bankdatenänderungen, Rechnungsmanipulationen).

---

## Incident-Klassifizierung

Früh als einen oder mehrere der folgenden Typen klassifizieren:
- Verdächtige E-Mail
- Bestätigte Phishing-E-Mail
- Credential Submission
- Malware-Ausführung
- Account Takeover
- Mailbox-Kompromittierung
- Business Email Compromise (BEC)
- Versuchter Zahlungsbetrug
- Bestätigter finanzieller Schaden
- Datenzugriff / vermutete Datenpanne

Die Klassifizierung steuert Priorisierung und Eskalation.

---

## Phase 1 — Erkennen & Triage

Ziele:
- Vorfallsart bestätigen und feststellen, ob Missbrauch aktiv ist.
- Anfangs-Umfang und Hochrisikoauswirkungen bestimmen.
- Die am schnellsten verlorenen Beweise sichern.

Maßnahmen:
- Benutzerberichte, Gateway-/Quarantäne-Einträge, SIEM-, IdP- und Endpoint-Telemetrie sammeln.
- Originalnachricht(en) mit vollständigen Headern und Artefakten sichern.
- Klick-/Credential-/Anhangs-Interaktionen feststellen.
- Betroffene Konten, Mailboxen und Empfänger identifizieren.
- Anmelde-Logs, MFA-Ereignisse, Session-Aktivität und OAuth-Consent-Logs prüfen.
- Nach ähnlichen Nachrichten oder Kompromittierungsmustern suchen.
- Mailbox-Audit, Sent Items und Recoverable Items auf Angreiferaktivität prüfen.
- Dokumentieren, ob die Nachricht intern oder extern versendet wurde.
- Eine Incident-Timeline aufbauen und fortführen.

Exit-Kriterien:
- Vorfallsart validiert.
- Anfangs-Umfang identifiziert.
- Potenziell kompromittierte Konten markiert.
- Beweissicherung begonnen.

---

## Phase 2 — Eindämmen (Contain)

Ziele:
- Aktiven Missbrauch und weitere Interaktionen stoppen.
- Konten, Sessions und Mail-Flows sichern.
- Business-Impact minimieren.

Maßnahmen:
- Betroffene Konten vorübergehend einschränken oder deaktivieren.
- Sessions und Refresh-Tokens widerrufen.
- Passwörter zurücksetzen, wenn Credential Theft wahrscheinlich ist.
- Unautorisierte OAuth-Consents und Enterprise-Apps entfernen.
- Bösartige Inbox-Regeln, Weiterleitungen und Delegationen entfernen.
- Tenantweit suchen und bösartige Nachrichten, soweit möglich, quarantänisieren/entfernen.
- Bösartige Domains, URLs, IPs, Hashes auf Gateway- und Netzwerkebene blockieren.
- Ausgehende E-Mails kompromittierter Konten vorübergehend einschränken, wenn sie Missbrauch versenden.
- Finance einbinden, um riskante Zahlungen zu pausieren und Änderungen zu verifizieren.
- Endpunkte isolieren, wenn Anhangs-/Lokalartefakte vermutet werden.

Nicht tun:
- Nicht davon ausgehen, dass ein Passwortwechsel allein genügt.
- Nicht nur die meldende Mailbox betrachten, wenn eine tenantweite Ausbreitung möglich ist.
- Beweise nicht löschen, bevor sie gesichert wurden.
- Zahlungsfreigaben nicht fortsetzen, wenn BEC-Risiko ohne Out-of-Band-Verifikation besteht.

Exit-Kriterien:
- Laufender Missbrauch reduziert oder gestoppt.
- Kompromittierte Konten oder Kanäle gesichert.
- Nachrichtenausbreitung technisch begrenzt.
- Zuständige Geschäftsverantwortliche informiert (Finance, Legal).

---

## Phase 3 — Untersuchen (Investigate)

Ziele:
- Angriffspfad und vollständigen Umfang bestimmen.
- Persistenz und Folgewirkungen identifizieren.
- Datenzugriff und Fraud-Risiko bewerten.

Maßnahmen:
- Header, SPF/DKIM/DMARC, Reply-To, Redirect-Ketten und Anhangsartefakte analysieren.
- Anmelde-Logs auf Geolocation-Anomalien, Gerätewechsel oder riskante Anmeldungen prüfen.
- MFA-Ereignisse (Bestätigungen, Ablehnungen, ungewöhnliche Muster) untersuchen.
- Session-/Token-Verhalten und Refresh-Aktivität auswerten.
- OAuth-/Consent-Ereignisse und App-Permissions analysieren.
- Mailbox-Audit-Logs (Send-As/Send-on-behalf, Löschungen, Suchen) untersuchen.
- Empfänger identifizieren, die die Nachricht erhalten, geöffnet oder beantwortet haben.
- Nach verwandten Nachrichten in Tenants und externen Domains suchen.
- Prüfen, ob Datenablagen (OneDrive, SharePoint, File Shares) zugegriffen wurden.
- Endpoint-Telemetrie prüfen, wenn lokale Kompromittierung vermutet wird.

Dokumentationskategorien für den Umfang:
- Betroffene Konten & Mailboxen
- Betroffene Benutzer/Rollen
- Betroffene Empfänger & externe Parteien
- Tokens, Sessions, Apps, OAuth-Consents
- Finanz-/Prozessauswirkungen
- Zugegriffene Datenablagen und Dateien
- Betroffene Endpunkte

Exit-Kriterien:
- Plausibler Angriffspfad etabliert.
- Kompromittierte Konten, Sessions und Fehlkonfigurationen aufgeführt.
- Fraud-/Daten-Impact bewertet, um die Beseitigung zu planen.

---

## Phase 4 — Beseitigen (Eradicate)

Ziele:
- Angreiferzugänge und Persistenz entfernen.
- Bösartige Konfigurationen bereinigen und Eintrittsvektoren schließen.
- Vertrauenswürdige Zugriffshaltung wiederherstellen.

Maßnahmen:
- Kontrollierte Passwort-Resets durchführen.
- Aktive Sessions und Refresh-Tokens tenantweit für betroffene Benutzer invalidieren.
- Unautorisierte OAuth-Consents, Enterprise-Apps und App-Consents entfernen.
- Inbox-Regeln, Weiterleitungen, Delegationen und Reply-To-Manipulationen entfernen.
- Recovery-Methoden, MFA-Faktoren und Self-Service-Einstellungen prüfen und zurücksetzen.
- API-Tokens, App-Secrets und gespeicherte Zugangsdaten rotieren, wenn sie exponiert wurden.
- Endpunkte neu aufbauen oder remediieren, wenn lokale Kompromittierung nicht als vertrauenswürdig gilt.
- Gateway-/Tenant-/IdP-Schutz und Detection-Regeln härten.
- Zahlungsprüfprozesse anpassen, wenn BEC/Fraud beteiligt war.

Exit-Kriterien:
- Zugriffspfade entfernt.
- Mailbox- und Identity-Konfiguration bereinigt.
- Sessions/Tokens/App-Zugriff nicht mehr aktiv.
- Ausgleichsmaßnahmen implementiert.

---

## Phase 5 — Wiederherstellung (Recovery)

Ziele:
- Vertrauenswürdigen Zugriff und Kommunikationskanäle wiederherstellen.
- Geschäftsbetrieb sicher fortsetzen.
- Auf Wiederholung überwachen.

Recovery-Gate — NICHT wiederherstellen, bevor:
- Der Angriffspfad hinreichend verstanden ist.
- Konten, Sessions, Tokens und Weiterleitungs-/OAuth-Consents bereinigt sind.
- Aktive Missbrauchsindikatoren fehlen.
- Monitoring, Detection und Kommunikation für Folgewellen eingerichtet sind.

Maßnahmen:
- Konten kontrolliert wieder aktivieren.
- MFA neu etablieren oder stärken (für Hochrisiko-Benutzer möglichst phishing-resistente Optionen bevorzugen).
- Benutzer über sichere Wiederaufnahme und ggf. erzwungene Änderungen informieren (Passwörter, MFA).
- Partner, Kunden oder Lieferanten benachrichtigen, wenn sie betroffen waren.
- Das Monitoring auf erneute Angriffsindikatoren für 14–30 Tage erhöhen.
- Tenantweite Suchen nach verwandten Nachrichten oder Indikatoren fortsetzen.
- Endpunkte/Browser, wo relevant, validieren, bevor der Normalbetrieb wieder aufgenommen wird.

Exit-Kriterien:
- Konten und Kanäle gelten als vertrauenswürdig.
- Keine aktiven Missbrauchsindikatoren mehr vorhanden.
- Geschäftsprozesse stabil und Verantwortliche für Rest-Risiken benannt.

---

## Phase 6 — Review / Lessons Learned

Ziele:
- Root Cause und Kontrolllücken identifizieren.
- Detection, Prevention und Response verbessern.
- Beweise und Entscheidungen finalisieren.
- Wiederholungen verhindern.

Maßnahmen:
- Innerhalb von 5–10 Geschäftstagen nach Stabilisierung eine strukturierte Nachbesprechung durchführen.
- Incident-Timeline und Entscheidungsprotokoll finalisieren.
- Angriffspfad, Nachrichtenmuster und Umfang bestätigen.
- Kontrollschwächen in Mail-Security, Identity, MFA, Conditional Access, Fraud-Workflows und Awareness identifizieren.
- Reaktionsleistung von Helpdesk, Security, Finance, Legal und Management bewerten.
- Detection-Regeln, Mailbox-Schutz und Tenant-Härtung aktualisieren.
- Zahlungsprüfungen und Lieferantenänderungsprozesse stärken.
- Dieses Playbook mit Lessons Learned aktualisieren und Remediation-Owner mit Fälligkeitsdaten benennen.

---

## Zu sichernde Beweise

- Originale Phishing-E-Mail(s) mit vollständigen Headern und Message-ID.
- Envelope Sender, Reply-To, extrahierte URLs, Redirect-Ketten und Anhangs-Hashes.
- Screenshots von Phishing-Seiten und belegte Formulareingaben, falls verfügbar.
- Anmelde-Logs, MFA-Logs und Passwort-Reset-Logs.
- Session- und Token-Logs, Refresh-Token-Ereignisse.
- Mailbox-Audit-Logs (Regeln, Weiterleitungen, Delegationen).
- OAuth-/Consent-Logs und Änderungen an Enterprise-Apps.
- Sent Items, Deleted Items und Recoverable Items.
- Quarantäne- und Message-Trace-Daten.
- Firewall-, Proxy-, DNS-, EDR- und Endpoint-Logs.
- Zahlungs-/Rechnungs- und Lieferantenänderungsdaten.
- Incident-Timeline und Entscheidungsprotokoll.

---

## Kommunikations-Checkliste

Intern:
- IR-Stakeholder benachrichtigen: IT, SOC, Helpdesk, Management, Legal, Finance, betroffene Geschäftsbereiche.
- Klare Benutzeranweisungen geben: Nachricht melden, Original nicht löschen, nicht auf verdächtige Anweisungen reagieren.
- Einen einzigen autoritativen Incident-Statuskanal pflegen.
- Dem Helpdesk einen FAQ-/Triage-Skript für Benutzerfragen geben.

Extern:
- Externe Empfänger informieren, wenn kompromittierte Konten zum Versand bösartiger Nachrichten genutzt wurden.
- Verifizierte Out-of-Band-Kanäle verwenden (nicht den kompromittierten Thread), um Partner/Kunden zu kontaktieren.
- Alle externe Kommunikation mit Legal und Communications abstimmen.
- Alle externen Nachrichtentwürfe als Beweise aufbewahren.
- Keine unbestätigten Aussagen zu Datenexposition oder finanziellem Schaden machen.
- Banken, Strafverfolgung oder Versicherer gemäß Richtlinie benachrichtigen, wenn Betrug eingetreten ist.

Vorgeschlagene kurze Benutzerwarnung:
> Es wurde eine verdächtige E-Mail-Kampagne festgestellt. Klicken Sie nicht auf Links, öffnen Sie keine Anhänge und antworten Sie nicht auf Nachrichten aus dieser Kampagne. Wenn Sie eine solche Nachricht erhalten oder damit interagiert haben, melden Sie dies sofort dem IT-/Security-Team und löschen Sie die E-Mail nicht.

---

## Rechtliche & regulatorische Aspekte

Phishing-/BEC-Vorfälle können vertragliche, regulatorische und Meldepflichten auslösen, wenn personenbezogene Daten, sensible Geschäftsdaten oder finanzielle Transaktionen betroffen sind. Legal/Privacy frühzeitig einbinden, wenn:

- Personenbezogene Daten oder Kundendaten möglicherweise zugegriffen wurden.
- Mailbox-Inhalte regulierte Daten enthalten.
- Betrügerische Zahlungen stattfanden oder Lieferanten-Bankdaten geändert wurden.
- Kompromittierte Konten genutzt wurden, um Kunden oder Partner zu imitieren.

Dieses Playbook ist keine Rechtsberatung; Beweise sichern und für Meldeentscheidungen an Counsel eskalieren.

---

## Quick-Response-Checkliste

Sofort:
- [ ] Incident Commander aktivieren.
- [ ] Timeline & Entscheidungsprotokoll starten.
- [ ] Betroffenes Konto/Mailbox identifizieren und Originalnachricht(en) sichern.
- [ ] Verdächtige Sessions/Tokens widerrufen.
- [ ] Finance / Legal / Management bei Bedarf benachrichtigen.

Umfang:
- [ ] Alle betroffenen Konten/Mailboxen identifizieren.
- [ ] Empfänger und Kommunikationsverläufe identifizieren.
- [ ] Inbox-Regeln/Weiterleitungen/Delegationen prüfen.
- [ ] Anmelde-, MFA- und Session-Aktivität prüfen.
- [ ] OAuth-/Consent-Änderungen prüfen.
- [ ] Datenzugriff und Fraud-Indikatoren bewerten.

Eindämmung:
- [ ] Konto/Konten einschränken oder deaktivieren.
- [ ] Sessions & Tokens widerrufen.
- [ ] Passwörter bei Bedarf zurücksetzen.
- [ ] OAuth-/App-Zugriff entfernen.
- [ ] Bösartige Nachrichten, soweit möglich, suchen und entfernen.
- [ ] Riskante Finanzprozesse absichern.

Untersuchung:
- [ ] Angriffspfad bestimmen.
- [ ] Umfang des Missbrauchs bestimmen.
- [ ] Persistenz & Mailbox-Manipulation identifizieren.
- [ ] Fraud- und Daten-Impact bewerten.
- [ ] Endpoint-Beteiligung bestimmen.

Wiederherstellung:
- [ ] Recovery-Gate validieren.
- [ ] Konto/Konten kontrolliert wieder aktivieren.
- [ ] MFA und Schutzmaßnahmen stärken.
- [ ] Stakeholder über korrigierte Kommunikation informieren.
- [ ] Auf Folgeangriffe überwachen.

Nach dem Vorfall:
- [ ] Timeline finalisieren.
- [ ] Root Cause bestätigen.
- [ ] Detection und Kontrollen aktualisieren.
- [ ] Playbook & Training aktualisieren.
- [ ] Remediation bis zum Abschluss nachverfolgen.

---

## Präventive Kontrollen

E-Mail-Sicherheit:
- SPF, DKIM und DMARC durchsetzen.
- Anti-Phishing-/Impersonation-Schutz und Sender-/URL-Rewrite einsetzen, wo sinnvoll.
- Anhänge sandboxen und verdächtige Dateien detonieren.
- Auto-Forwarding an externe Adressen einschränken.
- Neue Inbox-Regeln und Tenant-Mailflow-Regeln überwachen.
- Externe-Absender-Banner oder Warnhinweise angemessen einsetzen.

Identity-Sicherheit:
- MFA für alle Benutzer erzwingen; für Hochrisiko-Konten phishing-resistente Methoden bevorzugen.
- Legacy Authentication deaktivieren.
- Conditional Access anwenden.
- Impossible-Travel- und risky sign-ins überwachen.
- MFA-Fatigue-/Auto-Approve-Muster und neue Faktorregistrierungen überwachen.
- OAuth-App-Consents regelmäßig überprüfen.

Business-Prozess-Kontrollen:
- Out-of-Band-Verifikation für Lieferanten-Bankänderungen und Hochrisikozahlungen verlangen.
- Dual-Approval für signifikante Überweisungen einführen.
- Lieferanten-Bankänderungen über vertrauenswürdige Kanäle verifizieren.
- Sich bei dringenden Zahlungsanweisungen nicht allein auf E-Mail verlassen.
- Finance, HR und Executive Assistants auf BEC-Indikatoren schulen.
- Klare Eskalation für vermuteten Fraud etablieren.

---

## Website-Zusammenfassungsblock (kurz)

**Phishing & Business Email Compromise**  
Strukturiertes Response-Playbook für vermutete oder bestätigte Phishing-, Credential-Theft- und BEC-Vorfälle — von früher Erkennung und Eindämmung bis hin zu Identity-Härtung, sicherer Wiederherstellung und Lessons Learned.

Kernphasen: Detect (Triage), Contain, Investigate, Eradicate, Recovery, Review.

---

## Referenzen

- NIST SP 800-61: Computer Security Incident Handling Guide  
- CISA: Stopping the Attack Cycle at Phase One  
- Microsoft: Responding to a compromised email account  
- FBI: Business Email Compromise guidance  
- FBI IC3: BEC public advisories
