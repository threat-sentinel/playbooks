# Playbook: Cloud-Tenant-Kompromittierung

## Zusammenfassung

Dieses Playbook strukturiert die Reaktion auf die vermutete oder bestätigte Kompromittierung eines Cloud-Tenants: Microsoft 365, Entra ID und Azure, AWS, Google Workspace und Google Cloud, ein zentraler Identity Provider wie Okta sowie angebundene SaaS-, MDM-, EDR-, SIEM- und Backup-Plattformen.

Ein kompromittierter Tenant ist kein gewöhnlicher Kontovorfall. Wenn Angreifer administrative Rollen, Föderation, OAuth-Anwendungen, Workload-Identitäten, Logging oder Recovery beeinflussen können, steht das Vertrauensmodell der gesamten Umgebung infrage. Die Reaktion findet zudem häufig in derselben Management-Plane statt, die der Angreifer möglicherweise kontrolliert.

Der Kern dieses Playbooks: **Zuerst klären, ob dem Kontrollraum noch zu trauen ist. Dann Benutzer, Anwendungen, Daten und Workloads kontrolliert wiederherstellen.**

Konkrete Befehle, Portale und Logquellen stehen in den Plattformanhängen am Ende dieses Dokuments:

- [Anhang A: Microsoft 365, Entra ID und Azure](#anhang-a-microsoft-365-entra-id-und-azure)
- [Anhang B: AWS Organizations und IAM](#anhang-b-aws-organizations-und-iam)
- [Anhang C: Google Workspace und Google Cloud](#anhang-c-google-workspace-und-google-cloud)
- [Anhang D: Okta](#anhang-d-okta)

## Zweck

- Administrative Kontrolle über den Tenant bewahren oder zurückgewinnen
- Aktiven Angreiferzugriff stoppen, ohne Beweise zu vernichten
- Logs sichern, bevor sie ablaufen oder manipuliert werden
- Persistenz in Identitäten, Anwendungen, Richtlinien und Workloads finden und entfernen
- Entscheiden, ob dem Tenant nach der Bereinigung noch zu trauen ist
- Dienste kontrolliert und in der richtigen Reihenfolge wiederherstellen

## Anwendungsbereich

Verwenden Sie dieses Playbook, wenn mindestens einer der folgenden Punkte zutrifft:

- Unerlaubte administrative Aktivität in einem Cloud-Tenant wird entdeckt.
- Ein Global Administrator, Root-Konto, Super Admin oder eine andere privilegierte Rolle ist kompromittiert.
- Neue Admin-Konten, Rollen, Service Principals, App-Registrierungen oder OAuth-Grants erscheinen unerwartet.
- Conditional Access, MFA, SSO, Föderation, Audit-Logging oder Aufbewahrung werden unerwartet verändert.
- Sicherheitswerkzeuge, SIEM-Anbindungen oder Alarmierung werden deaktiviert oder umgangen.
- Ungewöhnlicher Zugriff auf Postfächer, Cloud-Speicher, Datenbanken, Workloads oder Management-APIs wird beobachtet.
- Cloud-Ressourcen werden erstellt, gelöscht, verschlüsselt oder als Angreiferinfrastruktur genutzt, etwa für Cryptomining.
- Massendownloads, Postfach-Exporte oder öffentliche Freigaben deuten auf Datenabfluss hin.
- Break-Glass-Konten, Backup- oder Recovery-Systeme im Tenant zeigen Auffälligkeiten.
- Administrative Kontrolle ist teilweise oder vollständig verloren.
- Ein Dritter, der Cloud-Anbieter oder ein Ransomware-Akteur meldet Zugriff auf Ihre Cloud-Umgebung.

**Abgrenzung:**

- Ein einzelnes übernommenes Benutzerkonto ohne administrative Rechte ist primär eine Identitätskompromittierung. Dieses Playbook übernimmt, sobald Rollen, Richtlinien, Anwendungen oder tenantweite Einstellungen betroffen sind.
- Hybride Identitäten und lokale Verzeichnisdienste gehören zum Scope, sobald sie Vertrauen, Konten oder Synchronisierung mit dem Tenant teilen.
- Bei Verschlüsselung oder Zerstörung in großem Umfang führt zusätzlich das Ransomware-Playbook.

## Schweregrad

Standard-Schweregrad: **Kritisch**.

Sofort an Management und gegebenenfalls Cloud-Anbieter eskalieren, wenn:

- höchste Rollen betroffen sind: Global Administrator, Root, Super Admin, Tenant- oder Organisationsadministrator
- der Identity Provider, Föderation oder Verzeichnissynchronisierung manipuliert wurde
- Logging, Aufbewahrung oder Sicherheitswerkzeuge verändert oder deaktiviert wurden
- Backup, Recovery, Key Management oder Secrets Management betroffen sind
- destruktive Aktionen laufen: Löschungen, Purge, Verschlüsselung, Ressourcen- oder Tenant-Wipe
- administrative Kontrolle verloren ist

## Incident-Ziele

1. Administrative Kontrolle sichern oder zurückgewinnen.
2. Logs und Beweise sichern, bevor sie ablaufen oder verändert werden.
3. Aktiven Angreiferzugriff und laufenden Schaden stoppen.
4. Erstzugang, Rechte, Persistenz und betroffene Daten bestimmen.
5. Entscheiden, ob der Tenant bereinigt oder in einer sauberen Umgebung neu aufgebaut wird.
6. Dienste kontrolliert wiederherstellen und ein erneutes Eindringen früh erkennen.

## Zentrale Reaktionsprinzipien

- **Nicht im Kontrollraum des Angreifers verhandeln.** Koordination über einen Kanal, der nicht vom kompromittierten Tenant abhängt; Administration nur von sauberen Admin-Geräten.
- **Logs zuerst.** Viele Cloud-Logs sind ohne Export nur Tage oder wenige Wochen verfügbar. Fehlende Logs sind kein negativer Befund.
- **Deaktivieren vor Löschen.** Angreiferobjekte erst nach Sicherung von Konfiguration, Credentials, Berechtigungen und Audit-Spuren entfernen, außer sie richten aktiv Schaden an.
- **Passwort-Reset ist keine Eindämmung.** Sitzungen, Refresh Tokens, App-Secrets, Zertifikate, Schlüssel und Vertrauensstellungen müssen eigens behandelt werden.
- **MFA ist kein Sauberkeitsnachweis.** Gestohlene Sitzungen, OAuth-Grants und Workload-Identitäten umgehen MFA.
- **Nie nur ein Recovery-Zugang.** Mindestens zwei unabhängige, geprüfte Break-Glass-Zugänge sichern, bevor weitreichende Änderungen erfolgen.
- **Lesend erfassen, mit Freigabe ändern.** Eindämmende Änderungen erst nach Beweissicherung und dokumentierter Entscheidung, im Vier-Augen-Prinzip.
- **Zeit konsistent erfassen.** Timeline, Log-Exporte und Entscheidungen in UTC dokumentieren.

## Kontakte und Eskalation

Diese Angaben organisationsspezifisch vorbelegen und offline verfügbar halten:

- Incident-Lead on call
- Cloud-, Tenant- und Identitätsadministration
- Notfallweg zum Cloud-Anbieter: Supportvertrag, Kundennummer, verifizierte Telefonnummer
- Anwendungs-, Daten- und Backup-Verantwortliche
- Externe Forensik und Incident-Response-Dienstleister
- Cyberversicherung
- Meldestellen: BSI (DE), zuständiges CSIRT (AT), BACS (CH)
- Management, Legal, Datenschutz und Kommunikation

## Die ersten 15 Minuten

- Incident-Lead benennen, Timeline und Entscheidungsprotokoll starten.
- Koordination auf einen Kanal außerhalb des Tenants verlegen.
- Prüfen, ob administrative Kontrolle besteht und ob die Break-Glass-Konten funktionieren und unverändert sind.
- Nur von sauberen Admin-Geräten arbeiten.
- Sign-in-, Audit-, Admin- und Aktivitätslogs exportieren, beginnend mit den kurzlebigsten Quellen.
- Sitzungen und Refresh Tokens bestätigt kompromittierter Konten widerrufen und die Konten sperren.
- Verdächtige Admin-Konten, Rollen, Service Principals und Apps deaktivieren, nicht löschen.
- Prüfen, ob gerade Daten abfließen, gelöscht oder verschlüsselt werden.
- Management, Legal und Datenschutz vorwarnen.

## Rollen und Verantwortlichkeiten

### Incident-Lead

- Steuert die Reaktion und priorisiert Maßnahmen.
- Gibt Eindämmung, Beseitigung und Wiederherstellung frei.
- Führt Timeline, Lagebild und Entscheidungsprotokoll.

### Cloud- und Tenant-Administration

- Prüft Tenant-Konfiguration, Rollen, Richtlinien und Ressourcen.
- Widerruft Sitzungen und entfernt unerlaubten Zugriff.
- Steuert den Kontakt zum Cloud-Anbieter auf technischer Ebene.

### Identität und Zugriff

- Prüft Anmeldungen, MFA, Conditional Access, SSO, Föderation und Verzeichnissynchronisierung.
- Entfernt Persistenz in Identitäten und stellt das Vertrauensmodell wieder her.

### SOC und Forensik

- Sichert Logs, Konfigurations-Snapshots und Artefakte.
- Korreliert Identitäts-, Cloud-, Mail-, SaaS- und Endpunkt-Telemetrie.
- Rekonstruiert Erstzugang, Angreiferpfad und Persistenz.

### Anwendungs-, Daten- und Backup-Verantwortliche

- Bewerten Datenzugriff, Integrationen und geschäftliche Auswirkungen.
- Schützen Backups vor Löschung und Manipulation und testen die Wiederherstellung.

### Legal, Datenschutz und Kommunikation

- Bewerten Melde-, Vertrags- und Versicherungspflichten.
- Steuern interne und externe Kommunikation.

### Management und Krisenstab

- Gibt weitreichende Maßnahmen frei, etwa tenantweite Sperren oder den Neuaufbau in einer sauberen Umgebung.
- Stellt Ressourcen bereit und steuert Business Continuity.

### Cloud-Anbieter und externe Incident Response

- Unterstützen bei Kontrollverlust, Kontowiederherstellung, Log-Abruf und erweiterter Analyse.
- Werden über den vertraglich vereinbarten, verifizierten Weg eingebunden.

## Entscheidungsmodell

### Regel 1: Ist dem Tenant noch zu trauen?

**Neuaufbau in einer sauberen Umgebung erwägen**, wenn:

- administrative Kontrolle verloren war und nicht vollständig nachvollziehbar ist, was der Angreifer in dieser Zeit tat
- Logging während der Kompromittierung deaktiviert, gelöscht oder lückenhaft war
- Break-Glass-Konten, Föderation oder SSO-Vertrauensstellungen manipuliert wurden
- mehrere unbekannte privilegierte Identitäten, Service Principals oder Workload-Identitäten gefunden wurden
- Backup-, Recovery- oder Sicherheitskontrollen manipuliert wurden
- nicht nachweisbar ist, dass die Persistenz des Angreifers entfernt wurde

**Bereinigung im bestehenden Tenant** ist nur vertretbar, wenn:

- administrative Kontrolle durchgehend bestand oder lückenlos zurückgewonnen wurde
- die Logs für den gesamten Zeitraum vollständig und unverändert vorliegen
- alle Änderungen des Angreifers identifiziert und rückgängig gemacht werden können
- Identity Provider, Föderation und Break-Glass-Zugänge nachweislich vertrauenswürdig sind

Ein Neuaufbau wird gestaffelt geplant: Identität, Logging, Sicherheit und Backup zuerst. Der kompromittierte Tenant bleibt für die Untersuchung erhalten, soweit möglich. Bei der Migration keine Identitäten, Berechtigungen, Apps oder Secrets ungeprüft übernehmen.

### Regel 2: Selbst handeln oder den Cloud-Anbieter einschalten?

**Anbieter sofort über den verifizierten Support-Weg einschalten**, wenn:

- administrative Kontrolle oder Root-, Super-Admin- bzw. Global-Admin-Zugang verloren ist
- Logs benötigt werden, die im Tenant nicht mehr verfügbar sind
- der Verdacht besteht, dass die Plattform selbst oder ein Anbieterprozess ausgenutzt wurde
- Ressourcen, Abrechnung oder der Tenant als Ganzes gesperrt oder gelöscht zu werden drohen

**Mit eigenen Mitteln weiterarbeiten**, wenn:

- mindestens zwei vertrauenswürdige Admin-Zugänge gesichert sind
- die benötigten Logs exportiert vorliegen
- die Eindämmung mit eigenen Rechten vollständig umsetzbar ist

### Regel 3: Föderation, Synchronisierung oder Identity Provider abschalten?

**Vertrauensstellung entziehen oder abschalten**, wenn:

- Föderationszertifikate, Token-Signaturschlüssel oder Vertrauensstellungen verändert wurden
- der Identity Provider oder der Synchronisierungsserver selbst kompromittiert ist
- über die Föderation Token für beliebige Benutzer ausgestellt werden könnten

**Unter verstärkter Beobachtung weiterbetreiben**, wenn:

- keine Manipulation an Föderation, Synchronisierung oder Signaturschlüsseln erkennbar ist
- ein Abschalten den Zugang für die gesamte Organisation ohne Ersatzweg unterbrechen würde
- Incident-Lead und Management die Risikoübernahme dokumentiert freigeben

Vor dem Abschalten einen Ersatz-Anmeldeweg für Administratoren sicherstellen, etwa Cloud-only-Notfallkonten.

### Regel 4: Gezielt oder tenantweit widerrufen?

**Tenantweit Sitzungen widerrufen und Zugänge beschränken**, wenn:

- privilegierte Rollen, Föderation oder Token-Signaturschlüssel kompromittiert sind
- der Umfang der betroffenen Konten nicht bestimmbar ist
- Token-Diebstahl in größerem Umfang wahrscheinlich ist

**Gezielt bei betroffenen Konten und Anwendungen**, wenn:

- die betroffenen Identitäten klar abgegrenzt sind
- keine tenantweiten Einstellungen verändert wurden
- ein tenantweiter Widerruf unverhältnismäßigen Betriebsausfall verursachen würde

### Regel 5: Wann andere Playbooks aktivieren?

- **Identitätskompromittierung parallel** für betroffene Einzelkonten und deren Sitzungen.
- **Datenpanne parallel**, wenn personenbezogene, regulierte oder vertrauliche Daten abgerufen, exportiert oder offengelegt worden sein könnten.
- **Ransomware-Playbook führend**, wenn Daten oder Ressourcen in großem Umfang verschlüsselt oder gelöscht werden.
- **Lieferkette parallel**, wenn der Zugang über einen Dienstleister, eine Drittanbieter-App oder eine CI/CD-Integration erfolgte.
- **Insider-Vorfall**, wenn eine berechtigte Person ihre Rechte vorsätzlich missbraucht hat.

## Lage- und Maßnahmenmatrix

| Lagebild | Mindestmaßnahme |
|---|---|
| Privilegiertes Konto kompromittiert | Sitzungen widerrufen, Konto sperren, Rollen- und Richtlinienänderungen im Zeitraum prüfen |
| Identity Provider, Föderation oder Sync manipuliert | Regel 3 anwenden, alle privilegierten Identitäten und Vertrauensstellungen prüfen, tenantweit widerrufen |
| Bösartige OAuth-App oder Consent-Missbrauch | App deaktivieren, Grants widerrufen, Zugriffe der App auf Mail, Dateien und APIs prüfen, gleiche Muster im Tenant suchen |
| Offengelegte Keys, Secrets oder Zertifikate | Erst deaktivieren, Nutzung im Log prüfen, Ersatz bereitstellen, dann rotieren und löschen |
| Datenabfluss | Öffentliche Links und externes Teilen sperren, Zugriffe aussetzen, Zugriffslogs sichern, Datenpannen-Playbook |
| Destruktive Aktivität | Automatisierung stoppen, Ressourcen sperren, Backups, Snapshots und Aufbewahrung schützen |
| Logging oder Sicherheitswerkzeuge manipuliert | Logs extern sichern, Konfiguration wiederherstellen, Lücken für Regel 1 dokumentieren |
| Kontrollverlust | Cloud-Anbieter nach Regel 2 einschalten, externe Incident Response hinzuziehen |

## Typische Persistenz im Tenant

- Neue oder veränderte Admin-Konten, Rollenzuweisungen und privilegierte Gruppenmitgliedschaften
- Neue Service Principals, App-Registrierungen und Workload-Identitäten
- Zusätzliche Client-Secrets, Zertifikate, API-Keys oder Access Keys an bestehenden Identitäten
- Bösartige OAuth-Grants und Admin-Consents
- Ausnahmen in Conditional Access, deaktivierte MFA-Anforderungen, neue Authentisierungsmethoden
- Veränderte Föderationsvertrauensstellungen, SSO-Konfigurationen oder Identity-Provider-Routing
- Neue Gastkonten und externe Identitäten
- Automatisierung: Runbooks, Funktionen, geplante Jobs, CI/CD-Credentials, Hooks und Workflows
- Postfach-Weiterleitungen, Transport- und Posteingangsregeln, Delegationen
- Veränderte Logging-, Aufbewahrungs-, Backup- oder Löschschutz-Einstellungen

## Wiederherstellungspriorität

1. Kommunikation außerhalb des Tenants und Koordination der Reaktion
2. Vertrauenswürdiger administrativer Zugang und Break-Glass-Konten
3. Identity Provider und privilegierter Zugriff
4. Logging, Audit-Aufbewahrung und SIEM-Anbindung
5. Sicherheitskontrollen und Alarmierung
6. Backup- und Recovery-Schutz
7. Mail, Kollaboration und zentrale SaaS-Dienste
8. Geschäftskritische Anwendungen und Datenbestände
9. Cloud-Workloads und Infrastruktur
10. Drittintegrationen und Automatisierung
11. Nicht kritische Dienste

## Phase 1: Erkennen und Triage

### Ziele

- Tenant-Kompromittierung bestätigen und Ebene bestimmen
- Feststellen, ob administrative Kontrolle betroffen ist
- Beweise sichern, bevor sie ablaufen oder verändert werden

### Maßnahmen

- Ersten Alarm, Entdeckungszeit und den frühesten bekannten verdächtigen Vorgang dokumentieren.
- Betroffene Tenants, Subscriptions, Accounts, Projekte, Workspaces und SaaS-Instanzen bestimmen.
- Anmeldungen privilegierter Konten prüfen: Ort, Gerät, Client, Anonymisierungsdienste, ungewöhnliche User-Agents.
- MFA-Aufforderungen, MFA-Methodenänderungen und neue Geräteregistrierungen prüfen.
- Änderungen an Conditional Access, SSO, Föderation, Rollen und Gruppen prüfen.
- Neue Apps, Service Principals, Workload-Identitäten, OAuth-Grants, Secrets, Zertifikate und Keys prüfen.
- Postfachzugriffe, Weiterleitungen, eDiscovery- und Exportaktivität prüfen.
- Speicherzugriffe, Freigabelinks, öffentliche Zugriffe und Massendownloads prüfen.
- Änderungen an Compute, Netzwerk, IAM, Logging, Aufbewahrung, Backup und Sicherheitswerkzeugen prüfen.
- Abrechnungs- und Ressourcenspitzen sowie ungewöhnliche Regionen prüfen.
- Vollständigkeit und Vertrauenswürdigkeit der Logs bewerten.

### Erste Klassifizierung

- **Benutzerebene:** einzelne Konten ohne administrative Rechte
- **Admin-Ebene:** privilegierte Rollen oder tenantweite Einstellungen betroffen
- **Anwendungs- und Workload-Ebene:** Service Principals, OAuth-Apps, Workload-Identitäten, Keys
- **Identity-Provider-Ebene:** Föderation, SSO, Synchronisierung oder Signaturschlüssel betroffen
- **Datenebene:** Zugriff, Export, Abfluss oder Löschung von Daten
- **Kontrollverlust:** administrative Kontrolle teilweise oder vollständig verloren

### Exit-Kriterien

- Vorläufige Klassifizierung dokumentiert
- Kurzlebige Logs exportiert und gesichert
- Status der administrativen Kontrolle und der Break-Glass-Konten bekannt
- Erste Hypothese zu Erstzugang und Umfang dokumentiert

## Phase 2: Eindämmen

### Ziele

- Angreiferzugriff stoppen
- Administrative Kontrolle sichern
- Destruktive Aktivitäten und Datenabfluss verhindern
- Logging, Sicherheit, Backup und Recovery schützen

### Maßnahmen

- Break-Glass-Konten prüfen, absichern und mindestens zwei unabhängige Admin-Zugänge sicherstellen.
- Sitzungen und Refresh Tokens betroffener Konten widerrufen, Konten sperren; Umfang nach Regel 4.
- Unerlaubte Rollen und Gruppenmitgliedschaften entfernen.
- Unerlaubte Konten, Gäste, Service Principals, Workload-Identitäten und Apps deaktivieren.
- Bösartige OAuth-Grants und App-Consents widerrufen.
- Offengelegte Credentials, Keys, Zertifikate und Secrets deaktivieren und anschließend kontrolliert rotieren.
- Manipulierte MFA- und Conditional-Access-Richtlinien wiederherstellen, riskante Ausnahmen entfernen.
- Öffentlichen Zugriff auf exponierte Speicher und Anwendungen sperren.
- Verdächtige Automatisierung, Skripte, Runbooks, Funktionen und Pipelines pausieren.
- Audit-Logs, Aufbewahrung, SIEM-Anbindung und Alarmierung schützen.
- Backups, Snapshots und Recovery-Vaults vor Löschung schützen.
- Bei Kontrollverlust den Cloud-Anbieter nach Regel 2 einschalten.

### Nicht tun

- Nicht von einer möglicherweise kompromittierten Admin-Workstation aus arbeiten.
- Angreiferobjekte nicht löschen, bevor Beweise gesichert sind, außer sie richten aktiv Schaden an.
- Nicht blind alle Secrets rotieren, ohne Abhängigkeiten zu kennen.
- Kritische Service Principals, Synchronisierung oder Föderation nicht ohne Ersatzweg abschalten.
- Sich für die Wiederherstellung nicht auf ein einziges Admin-Konto verlassen.
- Logs nicht trauen, solange nicht geprüft ist, ob Logging oder Aufbewahrung manipuliert wurden.
- Keine Incident-Details über Mail, Chat oder Dokumente im kompromittierten Tenant austauschen.

### Exit-Kriterien

- Kein aktiver Angreiferzugriff mehr erkennbar
- Mindestens zwei vertrauenswürdige Admin-Zugänge gesichert
- Logging, Backup und Sicherheitswerkzeuge geschützt
- Laufender Datenabfluss und destruktive Aktivität gestoppt

## Phase 3: Untersuchen

### Ziele

- Erstzugang, erlangte Rechte und Persistenz bestimmen
- Betroffene Daten, Ressourcen und Integrationen bestimmen
- Entscheidung nach Regel 1 vorbereiten

### Maßnahmen

- Alle administrativen Aktionen im Kompromittierungszeitraum rekonstruieren.
- Erstellung und Änderung von Benutzern, Gruppen, Rollen, Richtlinien und Anwendungen prüfen.
- Management-API-Aktivität, auch von Service Principals und Workload-Identitäten, auswerten.
- Postfach-, Datei-, Speicher- und Datenbankzugriffe sowie Exporte prüfen.
- Änderungen an Netzwerk, Firewalls, DNS, Routing und Private Endpoints prüfen.
- Key Vault, KMS, Secrets Manager, Backup, Snapshots und Aufbewahrung prüfen.
- CI/CD, Automatisierungskonten, Runbooks und Serverless-Funktionen prüfen.
- Gastkonten, externe Identitäten und Föderationsvertrauensstellungen prüfen.
- Admin-Aktivität in MDM, EDR und anderen Sicherheitswerkzeugen prüfen.
- Vom Angreifer erzeugte Ressourcen und Abrechnungsanomalien identifizieren.
- Nach der Persistenzliste oben systematisch suchen.

### Mögliche Erstzugänge

- Phishing, Token-Diebstahl oder Infostealer auf einem Admin-Gerät
- Password Spraying, Credential Stuffing, schwache oder fehlende MFA, Legacy Authentication
- Offengelegte Passwörter, API-Keys, Access Keys oder CI/CD-Secrets
- Bösartiger OAuth-Consent oder kompromittierte Drittanbieter-Integration
- Kompromittierter Service Principal oder kompromittierte Workload-Identität
- Fehlkonfigurierte Föderation oder SSO
- Verwundbare, aus dem Internet erreichbare Anwendung
- Insider-Missbrauch oder kompromittierter Dienstleister

### Bereiche für die Bewertung der Auswirkungen

- Identity Provider und Management-Plane
- Mail, Kollaboration und SaaS-Anwendungen
- Datei- und Objektspeicher, Datenbanken und Data Warehouses
- Compute, Container, Kubernetes und Serverless
- CI/CD, Secrets Management und Schlüssel
- Backup, Recovery, Monitoring und Logging
- Sicherheitswerkzeuge und Netzwerkkonfiguration
- Abrechnung, Subscriptions, externe Benutzer und Drittintegrationen

### Exit-Kriterien

- Plausibler Erstzugang und Angreiferpfad dokumentiert
- Persistenz und betroffene Identitäten, Daten und Ressourcen benannt
- Entscheidung nach Regel 1 getroffen und dokumentiert
- Übergabe an Datenpannen-, Identitäts- oder Ransomware-Playbook erfolgt, falls erforderlich

## Phase 4: Beseitigen

### Ziele

- Angreiferzugriff und Persistenz vollständig entfernen
- Vertrauenswürdige Tenant-Konfiguration wiederherstellen
- Erstzugang schließen

### Maßnahmen

- Unerlaubte Benutzer, Gäste, Gruppen, Rollen, Service Principals, Apps und Workload-Identitäten nach Sicherung entfernen.
- Bösartige OAuth-Grants und delegierte Berechtigungen entfernen.
- Unerlaubte Secrets, Zertifikate und Keys entfernen; alle offengelegten Credentials rotieren.
- Sitzungen, Refresh Tokens und Anwendungs-Tokens erneut widerrufen.
- Sichere MFA-, Conditional-Access-, Föderations- und SSO-Konfiguration wiederherstellen; nicht freigegebene Ausnahmen entfernen.
- Logging, Audit, Aufbewahrung, Alarmierung und SIEM-Anbindung wiederherstellen.
- Sicherheitswerkzeuge erneut aktivieren und ihre Funktion prüfen.
- Schädliche Automatisierung, Deployment-Artefakte und vom Angreifer erzeugte Ressourcen entfernen.
- Ausgenutzte Workloads patchen; für die Administration genutzte, kompromittierte Endpunkte neu aufsetzen.
- Bei Entscheidung für den Neuaufbau: gestaffelt nach der Wiederherstellungspriorität in die saubere Umgebung migrieren.

### Exit-Kriterien

- Persistenz entfernt oder Neuaufbau abgeschlossen
- Alle offengelegten Credentials rotiert
- Logging und Sicherheitskontrollen aktiv und geprüft
- Erstzugang geschlossen

## Phase 5: Wiederanlauf

### Ziele

- Sicheren Tenant-Betrieb wiederherstellen
- Erneutes Eindringen früh erkennen

### Maßnahmen

- Dienste in der Reihenfolge der Wiederherstellungspriorität freigeben.
- Bestätigen, dass administrative Kontrolle gesichert und Break-Glass-Konten überwacht sind.
- Bestätigen, dass Conditional Access, MFA, Logging, Aufbewahrung, Sicherheitswerkzeuge und Backup-Schutz aktiv sind.
- Betroffene Workloads nur aus vertrauenswürdigen Quellen wiederherstellen.
- Prüfen, ob Drittintegrationen weiterhin benötigt werden und sicher sind.
- Verstärkte Überwachung für 30 bis 90 Tage, je nach Schweregrad.

### Verstärkte Überwachung

- Anmeldungen von Administratoren und von bekannter Angreiferinfrastruktur
- Neue Rollenzuweisungen, Service Principals, App-Registrierungen, Secrets und Zertifikate
- OAuth-Consents
- Änderungen an Conditional Access, MFA, Föderation und SSO
- Änderungen an Logging, Aufbewahrung, Backup und Sicherheitswerkzeugen
- Massendownloads, Exporte, Postfachsuchen und öffentliche Freigaben
- Spitzen bei Erstellung oder Löschung von Ressourcen

### Exit-Kriterien

- Kritische Dienste stabil in Betrieb
- Überwachung ohne neue Auffälligkeiten
- Restrisiken dokumentiert und zugewiesen

## Phase 6: Nachbereitung

### Maßnahmen

- Nachbesprechung innerhalb von zehn Geschäftstagen durchführen.
- Timeline, Erstzugang, Persistenz, betroffene Daten und getroffene Entscheidungen dokumentieren.
- Modell für Identität und privilegierten Zugriff überprüfen.
- OAuth-Consent und Application Governance überprüfen.
- Rechte von Service Principals und Workload-Identitäten überprüfen.
- Logging, Aufbewahrung und Monitoring-Abdeckung überprüfen.
- Backup- und Recovery-Resilienz überprüfen und testen.
- Zugänge von Dienstleistern und Drittanbietern überprüfen.
- Erkennungsregeln, Playbooks und Härtungsbaselines aktualisieren.

### Leitfragen

- Wie kam der Angreifer hinein, und warum hat MFA das nicht verhindert?
- Waren privilegierte Konten und Break-Glass-Konten ausreichend geschützt?
- Waren Apps, Service Principals oder Workload-Identitäten überprivilegiert?
- Reichten die Logs, um die Aktivität zu rekonstruieren, und waren sie vor Manipulation geschützt?
- War dem Tenant nach der Bereinigung noch zu trauen?
- Welche Kontrolle hätte den Schaden am stärksten begrenzt?

## Zu sichernde Beweise

- **Identität:** Anmeldungen (auch fehlgeschlagene und nicht interaktive), MFA- und Conditional-Access-Ergebnisse, Risikoerkennungen, Passwort- und MFA-Änderungen, Geräteregistrierungen, Rollen- und Gruppenänderungen
- **Tenant-Konfiguration:** Snapshots von Conditional Access, MFA-Richtlinien, SSO und Föderation, Synchronisierung, Audit- und Aufbewahrungseinstellungen, externem Teilen, Gastzugriff, Admin-Rollen, Break-Glass-Konten
- **Anwendungen und Workload-Identitäten:** App-Registrierungen, Service Principals, OAuth-Grants, Admin-Consents, Secrets, Zertifikate, Keys, Managed Identities, Workload-Identity-Föderation
- **Cloud-Ressourcen:** Aktivitätslogs der Management-Plane, Erstellung, Änderung und Löschung von Ressourcen, Speicher- und Datenbankzugriffe, Key-Vault- bzw. KMS-Logs, Netzwerkänderungen, Kubernetes-, Serverless- und CI/CD-Logs, Backup-Logs, Abrechnungsdaten
- **SaaS und Kollaboration:** Postfach-Audit, Message Trace, Weiterleitungs- und Transportregeln, eDiscovery und Inhaltssuchen, Dateizugriffe, Freigabelinks, Exporte
- **Sicherheitswerkzeuge:** SIEM-, XDR-, EDR-, CSPM-, DLP- und CASB-Alarme, Hinweise auf deaktivierte Kontrollen
- **Entscheidungen:** Timeline, Entscheidungsprotokoll, Tickets beim Cloud-Anbieter, Kommunikation mit externer Incident Response, Behörden und Betroffenen, Begründung von Melde- oder Nicht-Meldeentscheidungen

Logquellen, Exportwege und Beispielbefehle je Plattform stehen in den Anhängen A bis D.

## Kommunikations-Checkliste

### Intern

- Reaktionsteam, Cloud- und Identitätsadministration, SOC, Forensik und Management über den Kanal außerhalb des Tenants informieren.
- Anwendungs-, Daten- und Backup-Verantwortliche einbinden.
- Helpdesk auf Anmelde- und Zugriffsprobleme vorbereiten.
- Eine einzige verlässliche Quelle für Lageupdates führen, sachlich und versioniert.

### Extern

- Cloud-Anbieter nur über verifizierte Support-Wege kontaktieren.
- Cyberversicherung und externe Incident Response nach Vertrag einbinden.
- Kunden, Lieferanten oder Partner informieren, wenn deren Daten oder Zugänge betroffen sein könnten.
- Externe Mitteilungen mit Legal, Datenschutz und Kommunikation abstimmen und aufbewahren.

### Vorlage: interne Mitteilung

> Wir untersuchen derzeit eine mögliche Kompromittierung unserer Cloud-Umgebung. Bitte nehmen Sie bis auf Weiteres keine nicht freigegebenen Änderungen an tenantweiten Einstellungen vor, löschen Sie keine verdächtigen Objekte und besprechen Sie Details nicht über Mail, Chat oder Dokumente in dieser Umgebung. Koordination ausschließlich über [Kanal außerhalb des Tenants].

### Vorlage: externe Stellungnahme

> Wir untersuchen einen Sicherheitsvorfall mit möglicher unberechtigter Aktivität in unserer Cloud-Umgebung. Maßnahmen zur Eindämmung und Bewertung laufen, einschließlich der Prüfung administrativer Zugriffe, der Sicherheitskontrollen und einer möglichen Offenlegung von Daten. Wir informieren Betroffene nach den geltenden Pflichten, sobald belastbare Erkenntnisse vorliegen.

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

- Die Kompromittierung eines Cloud-Tenants mit Mail, Dateien oder Kundendaten betrifft fast immer personenbezogene Daten. Die Datenschutzbewertung früh beginnen, nicht erst nach Abschluss der Analyse.
- Der Cloud-Anbieter ist in der Regel Auftragsverarbeiter (Art. 28 DSGVO). Liegt die Ursache bei ihm, muss er Sie nach Art. 33 Abs. 2 DSGVO unverzüglich informieren; die Meldepflicht gegenüber der Aufsichtsbehörde bleibt bei Ihnen.
- Ein Kontrollverlust über zentrale Identitäts- und Kollaborationsdienste ist häufig ein erheblicher Sicherheitsvorfall im Sinne von NIS2 und ISG.
- Verträge mit Kunden und Partnern sowie die Cyberversicherung enthalten oft eigene, kürzere Meldefristen.

## Quick-Response-Checkliste

### Sofort

- [ ] Incident-Lead, Timeline und Entscheidungsprotokoll gestartet
- [ ] Koordination außerhalb des Tenants
- [ ] Nur saubere Admin-Geräte im Einsatz
- [ ] Administrative Kontrolle und Break-Glass-Konten geprüft
- [ ] Kurzlebige Logs exportiert
- [ ] Sitzungen und Tokens betroffener Konten widerrufen
- [ ] Verdächtige Konten, Rollen und Apps deaktiviert

### Umfang

- [ ] Betroffene Tenants, Accounts, Projekte und SaaS-Plattformen bestimmt
- [ ] Betroffene Benutzer, Administratoren, Apps und Workload-Identitäten bestimmt
- [ ] Manipulation von Logging und Sicherheitskontrollen geprüft
- [ ] Datenzugriff, Abfluss und destruktive Aktionen geprüft

### Eindämmung und Beseitigung

- [ ] Unerlaubte Rollen, Apps, Grants und Credentials entfernt
- [ ] Offengelegte Secrets und Keys rotiert
- [ ] MFA, Conditional Access, Föderation und Logging wiederhergestellt
- [ ] Backups und Recovery geschützt
- [ ] Entscheidung Bereinigung oder Neuaufbau dokumentiert

### Danach

- [ ] Dienste nach Priorität wiederhergestellt
- [ ] Verstärkte Überwachung für 30 bis 90 Tage eingerichtet
- [ ] Meldepflichten bewertet
- [ ] Maßnahmen bis zum Abschluss verfolgt

## Präventive Kontrollen

### Identität und privilegierter Zugriff

- Phishing-resistente MFA, mindestens für Administratoren
- Getrennte Admin-Konten, Privileged Access Workstations und Just-in-Time-Rechte
- Mindestens zwei geschützte und überwachte Break-Glass-Konten
- Legacy Authentication deaktiviert, Conditional Access angewendet
- Regelmäßige Prüfung der Admin-Rollen

### Anwendungen und OAuth

- Benutzer-Consent einschränken, Admin-Consent-Workflow nutzen
- OAuth-Grants, Service Principals und App-Berechtigungen regelmäßig prüfen
- Managed Identities oder Workload-Identitäten statt langlebiger Secrets
- Ungenutzte Apps und Integrationen entfernen

### Tenant-Konfiguration, Logging und Recovery

- Sichere Tenant-Baseline, Änderungen an Richtlinien überwacht, wo möglich als Code
- Audit-, Identitäts-, Postfach- und Datenzugriffs-Logging aktiv und an ein SIEM außerhalb des Tenants weitergeleitet
- Alarme bei Rollenzuweisungen, neuen Apps und Credentials, OAuth-Consents, Logging-Änderungen, Massendownloads und Löschungen
- Backups gegen Löschung geschützt, unabhängige Recovery-Pfade, regelmäßig getestete Wiederherstellung

## Kompakter Einstieg

Dieser Block ist kein verkürztes Ersatz-Playbook. Er dient als schneller Einstieg; für die Bearbeitung des Vorfalls gelten alle Abschnitte dieses Dokuments und der passende Plattformanhang.

**Cloud-Tenant-Kompromittierung**  
Reaktion auf kompromittierte Cloud-Tenants und Identity Provider: administrative Kontrolle sichern, Logs vor dem Ablauf exportieren, Persistenz in Rollen, Apps und Richtlinien entfernen und entscheiden, ob dem Tenant noch zu trauen ist — mit Anhängen für Microsoft, AWS, Google und Okta.

**Kernphasen:** Erkennen, Eindämmen, Untersuchen, Beseitigen, Wiederanlauf, Nachbereitung.

## Referenzen

- NIST SP 800-61 Rev. 3
- CISA: Microsoft Expanded Cloud Logs Implementation Playbook
- CISA Advisory AA21-008A: Detecting Post-Compromise Threat Activity in Microsoft Cloud Environments
- Microsoft Entra ID: Revoke user access in an emergency
- MITRE ATT&CK: T1078.004, T1098.001, T1528, T1538, T1619, T1567.002
- CIS Critical Security Controls und CIS Benchmarks für Cloud-Anbieter
- Cloud Security Alliance: Cloud Controls Matrix

## Anhang A: Microsoft 365, Entra ID und Azure

Befehle enthalten Platzhalter. Aus einer sauberen Admin-Workstation und mit einem geprüften Response-Konto ausführen. Zeiten als UTC im Format `2026-10-01T00:00:00Z` einsetzen.

### Einsatzregeln

- Vor Änderungen Zeitfenster, Tenant-ID, ausführendes Konto und Begründung dokumentieren.
- Sign-in-, Directory-Audit-, Unified-Audit- und Azure-Activity-Daten so früh wie möglich exportieren.
- Benutzer-, Service-Principal- und Managed-Identity-Aktivität getrennt prüfen.
- Ein Passwort-Reset ersetzt weder Session-Widerruf noch die Rotation von App-Secrets, Zertifikaten oder föderierten Vertrauensstellungen.
- Gastkonten werden in ihrem Heimat-Tenant authentisiert; `revokeSignInSessions` im eigenen Tenant beendet deren Sitzungen im Heimat-Tenant nicht.

### Wichtige Logquellen

| Quelle | Worauf achten |
|---|---|
| Entra: interaktive und nicht interaktive Anmeldungen | Ungewöhnliche IPs, Geräte, Conditional-Access-Ergebnis, Client-Typ, Legacy Auth |
| Entra: Anmeldungen von Service Principals | Neue App-Nutzung, ungewöhnliche Ressourcen, Missbrauch von Secrets oder Zertifikaten |
| Entra: Anmeldungen von Managed Identities | Nutzung außerhalb der erwarteten Azure-Ressourcen |
| Entra: Directory Audit | Rollen, Benutzer, Gruppen, MFA, Apps, Consent, Föderation, Conditional Access |
| Microsoft 365: Unified Audit Log | Exchange, SharePoint, OneDrive, Teams, Entra und weitere Dienste |
| Exchange Online: Mailbox Audit, Admin Audit, Message Trace | Postfachzugriffe, Regeln, Weiterleitungen, Delegationen, Transportänderungen |
| Purview: Audit und eDiscovery | Exporte, Suchen, Aufbewahrung, DLP- und Compliance-Aktivität |
| Defender: Incidents, Alerts, Advanced Hunting | Korrelation über Identität, Endpunkt, Mail und Cloud |
| Azure: Activity Log | Änderungen an Subscriptions und Ressourcen (Control Plane) |
| Azure: Resource Logs | Key Vault, Storage, SQL, Firewall und weitere Data-Plane-Logs |

Aufbewahrung ohne Export: Entra-Anmelde- und Audit-Logs 7 Tage ohne Lizenz, 30 Tage mit Entra ID P1/P2; Unified Audit Log 180 Tage mit Audit Standard; Azure Activity Log 90 Tage. Der Abruf von Anmeldelogs über Microsoft Graph setzt Entra ID P1/P2 voraus, sonst Export im Portal. Fehlende Daten sind kein negativer Befund.

### Kontext und Identität (lesend)

```powershell
Connect-MgGraph -Scopes "AuditLog.Read.All","Directory.Read.All","Policy.Read.All","Application.Read.All","RoleManagement.Read.Directory"
Get-MgContext
Get-MgOrganization | Select-Object Id,DisplayName,VerifiedDomains

$Start = [datetime]"<START_UTC>"
$End   = [datetime]"<END_UTC>"
$User  = "user@example.com"
```

### Anmeldungen und Directory Audit exportieren

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

Microsoft Graph paginiert große Ergebnisse. `-All` bewusst verwenden und den Export auf Vollständigkeit, Fehler und Dateigröße prüfen.

### Rollen, Anwendungen und Consent prüfen

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

Besonders prüfen: kürzlich erstellte Apps, neue Credentials, tenantweiter Admin-Consent, ungewöhnliche Redirect-URIs, nicht verifizierte Herausgeber und Rechte wie `Mail.Read`, `Files.Read.All`, `Directory.ReadWrite.All` oder `RoleManagement.ReadWrite.Directory`.

### Conditional Access und Föderation sichern

```powershell
Get-MgIdentityConditionalAccessPolicy -All |
  ConvertTo-Json -Depth 30 |
  Set-Content -Encoding utf8 "entra-conditional-access.json"

Get-MgDomainFederationConfiguration -DomainId "example.com" |
  ConvertTo-Json -Depth 20 |
  Set-Content -Encoding utf8 "entra-federation-example.com.json"
```

Zusätzlich Authentication Methods Policy, Named Locations, Cross-Tenant Access, PIM-Konfiguration, Break-Glass-Konten und Verzeichnissynchronisierung dokumentieren.

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

`ReturnLargeSet` liefert unsortiert und ist pro Session begrenzt. In Zeitfenster aufteilen, wiederholt abrufen und Anzahl sowie Duplikate kontrollieren. Für große Untersuchungen die Audit-Suche in Purview verwenden.

### Exchange Online prüfen

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

Auch Transportregeln, Connectoren, Remote Domains, Journaling, Delegationen, eDiscovery-Exporte und neue Postfächer prüfen.

### Azure Control Plane und Ressourcen

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

Resource Logs liegen je nach Diagnostic Setting in Log Analytics, Storage oder Event Hub. Für Key Vault, Storage und Datenbanken prüfen, ob Data-Plane-Logging im relevanten Zeitraum überhaupt aktiv war.

### Eindämmung (nur mit Freigabe)

#### Sitzungen eines Benutzers widerrufen

```powershell
Revoke-MgUserSignInSession -UserId $User
```

Die Wirkung kann verzögert eintreten. Danach das Konto sperren, Authentisierungsmethoden prüfen und je nach Befund Passwort und registrierte Methoden kontrolliert zurücksetzen.

#### Konto vorübergehend sperren

```powershell
Update-MgUser -UserId $User -AccountEnabled:$false
```

#### Verdächtigen Service Principal deaktivieren

```powershell
Update-MgServicePrincipal -ServicePrincipalId "<OBJECT_ID>" -AccountEnabled:$false
```

Nicht löschen, bevor Konfiguration, Credentials, Besitzer, Grants, App-Role-Zuweisungen und Audit-Spuren gesichert sind.

### Kritische Suchthemen

- Rollenaktivierung und dauerhafte Rollenzuweisungen
- Änderung oder Ausnahmen von Conditional-Access-Richtlinien
- Neue oder geänderte Authentisierungsmethoden
- Domain-Föderation, Cross-Tenant Access und Verzeichnissynchronisierung
- App-Registrierungen, Service Principals, Consent und neue Credentials
- Postfachregeln, Weiterleitungen, Delegationen und eDiscovery-Exporte
- Änderungen an Audit, Aufbewahrung, Defender, Sentinel und Diagnostic Settings
- Key-Vault-Zugriffe, Secret-Abrufe, Storage-Downloads und SAS-Erstellung
- Deployments, VM Extensions, Automation Accounts und Runbooks

### Offizielle Referenzen

- [Microsoft Graph: Entra Audit Logs](https://learn.microsoft.com/en-us/graph/api/resources/azure-ad-auditlog-overview)
- [Microsoft Graph: Anmeldesitzungen widerrufen](https://learn.microsoft.com/en-us/graph/api/user-revokesigninsessions)
- [Entra-Aktivitätslogs mit Microsoft Graph analysieren](https://learn.microsoft.com/en-us/entra/identity/monitoring-health/howto-analyze-activity-logs-with-microsoft-graph)
- [Search-UnifiedAuditLog](https://learn.microsoft.com/en-us/powershell/module/exchange/search-unifiedauditlog)
- [Azure Activity Log](https://learn.microsoft.com/en-us/azure/azure-monitor/platform/activity-log)
- [Microsoft Incident Response Playbooks](https://learn.microsoft.com/en-us/security/operations/incident-response-playbooks)

## Anhang B: AWS Organizations und IAM

AWS-Abfragen sind meist Account- und Region-spezifisch. Globale Dienste schreiben ihre Ereignisse dennoch in bestimmte Regionen. Alle relevanten Accounts und aktivierten Regionen prüfen. Zeiten als UTC im Format `2026-10-01T00:00:00Z` einsetzen.

### Einsatzregeln

- Die Reaktion bevorzugt aus einem dedizierten Security- oder Log-Archive-Account durchführen.
- Management Account, delegierte Administratoren und Mitgliedskonten getrennt betrachten.
- CloudTrail-Konfiguration und zentrale Logkopien schützen, bevor verdächtige Principals verändert werden.
- Bei temporären Credentials (`ASIA…`) den ausstellenden STS-Vorgang und den `sessionIssuer` ermitteln; nur nach dem sichtbaren Access Key zu suchen reicht nicht.
- Access Keys zunächst deaktivieren und erst nach der Beweissicherung löschen. Rollen-Sitzungen erfordern andere Maßnahmen als langlebige Keys von IAM-Benutzern.

### Wichtige Logquellen

| Quelle | Abdeckung und Grenzen |
|---|---|
| CloudTrail Event History | 90 Tage Management Events pro Account und Region; keine Data Events, nur ein Filterattribut, keine organisationsweite Sicht |
| Organization Trail / CloudTrail Lake | Zentrale, längerfristige und accountübergreifende Suche; Abdeckung, Selektoren und Aufbewahrung prüfen |
| CloudTrail Data Events | S3, Lambda und weitere Data-Plane-Aktivität; nur vorhanden, wenn konfiguriert |
| GuardDuty | Anomalien aus CloudTrail, VPC Flow, DNS und optionalen Schutzplänen; ein Finding ist ein Ausgangspunkt, keine vollständige Beweislage |
| AWS Config | Ressourcen- und Konfigurationshistorie; Recorder, Regionen und Ressourcentypen prüfen |
| IAM Access Analyzer | Externe und ungenutzte Zugriffe; keine Timeline |
| VPC Flow Logs, Route 53 Resolver Query Logs | Netzwerk- und DNS-Metadaten; nur falls zuvor aktiviert |
| S3 Access Logs / CloudTrail S3 Data Events | Objektzugriffe; je Bucket unterschiedlich konfiguriert |
| Security Hub / Detective | Aggregation und Korrelation von Findings; regions- und organisationsabhängig |

### Kontext und Organisationsinventar

```bash
aws sts get-caller-identity > aws-caller-identity.json
aws organizations describe-organization > aws-organization.json
aws organizations list-accounts --output json > aws-accounts.json
aws organizations list-delegated-administrators --output json > aws-delegated-admins.json
aws organizations list-policies --filter SERVICE_CONTROL_POLICY --output json > aws-scps.json
```

Für jeden Account dokumentieren: ID, Name, OU, Status, verantwortliche Person, Break-Glass-Weg, zentrale Logziele und aktivierte Regionen.

### CloudTrail-Konfiguration prüfen

```bash
aws cloudtrail describe-trails --include-shadow-trails --region <REGION> > cloudtrail-trails-<REGION>.json
aws cloudtrail get-trail-status --name <TRAIL_ARN_OR_NAME> --region <REGION> > cloudtrail-status.json
aws cloudtrail get-event-selectors --trail-name <TRAIL_ARN_OR_NAME> --region <REGION> > cloudtrail-event-selectors.json
aws cloudtrail list-event-data-stores --region <REGION> > cloudtrail-lake-stores.json
```

Prüfen: Multi-Region, Organization Trail, Log File Validation, KMS, Ziel-Bucket, Object Lock, Lifecycle, Data-Event-Selektoren sowie Änderungen an Trail oder Bucket Policy.

### Event History exportieren

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

Die CLI paginiert automatisch, solange `--no-paginate` nicht gesetzt ist. Dateigröße, Zeitraum und Fehler trotzdem prüfen.

### CloudTrail Lake: Beispielabfrage

```bash
QUERY_ID=$(aws cloudtrail start-query \
  --query-statement "SELECT eventTime,eventSource,eventName,awsRegion,sourceIPAddress,userAgent,userIdentity FROM <EVENT_DATA_STORE_ID> WHERE eventTime >= '<START>' AND eventTime <= '<END>'" \
  --region <REGION> \
  --query 'QueryId' --output text)

aws cloudtrail get-query-results --query-id "$QUERY_ID" --region <REGION> > cloudtrail-lake-results.json
```

Zeiten hier im Format `2026-10-01 00:00:00`. Syntax, verfügbare Felder und Kosten vor breiten Abfragen prüfen. Ergebnisse paginieren und Original-Logs unverändert aufbewahren.

### IAM-Inventar und Credentials

```bash
aws iam get-account-summary > iam-account-summary.json
aws iam get-account-authorization-details > iam-authorization-details.json
aws iam generate-credential-report
aws iam get-credential-report --query Content --output text | base64 --decode > iam-credential-report.csv
aws iam list-saml-providers > iam-saml-providers.json
aws iam list-open-id-connect-providers > iam-oidc-providers.json
```

Der Credential Report enthält keine service-spezifischen Credentials. Für jeden verdächtigen Benutzer zusätzlich:

```bash
aws iam list-access-keys --user-name <USER> > iam-user-access-keys.json
aws iam list-service-specific-credentials --user-name <USER> > iam-service-credentials.json
aws iam list-mfa-devices --user-name <USER> > iam-user-mfa.json
aws iam list-signing-certificates --user-name <USER> > iam-user-signing-certificates.json
```

### Rollen und Vertrauensbeziehungen

```bash
aws iam list-roles --output json > iam-roles.json
aws iam get-role --role-name <ROLE_NAME> > iam-role-<ROLE_NAME>.json
aws iam list-attached-role-policies --role-name <ROLE_NAME> > iam-role-attached-policies.json
aws iam list-role-policies --role-name <ROLE_NAME> > iam-role-inline-policies.json
```

Besonders prüfen: neue Trust Principals, fremde Account-IDs, fehlende `ExternalId`, Wildcards, Web-Identity- und OIDC-Provider (etwa GitHub), Session Tags und ungewöhnlich lange Session-Dauer.

### Findings und Ressourcen

```bash
aws guardduty list-detectors --region <REGION> > guardduty-detectors.json
aws guardduty list-findings --detector-id <DETECTOR_ID> --region <REGION> > guardduty-finding-ids.json
aws guardduty get-findings --detector-id <DETECTOR_ID> --finding-ids <ID1> <ID2> --region <REGION> > guardduty-findings.json

aws configservice describe-configuration-recorder-status --region <REGION> > config-recorder-status.json
aws resourcegroupstaggingapi get-resources --region <REGION> > tagged-resources.json
```

Ergänzend EC2, Lambda, KMS, Secrets Manager, SSM, ECR, ECS/EKS, S3, CloudFormation und Billing auf neu erstellte oder geänderte Ressourcen prüfen.

### Kritische CloudTrail-Ereignisse

- `ConsoleLogin`, `CreateAccessKey`, `UpdateAccessKey`, `DeleteAccessKey`
- `CreateUser`, `CreateRole`, `UpdateAssumeRolePolicy`
- `Attach*Policy`, `Put*Policy`, `CreatePolicyVersion`, `SetDefaultPolicyVersion`
- `AssumeRole`, `AssumeRoleWithSAML`, `AssumeRoleWithWebIdentity`, `AssumeRoot`
- `DeactivateMFADevice`, `DeleteVirtualMFADevice` und jede Root-Aktivität
- `StopLogging`, `DeleteTrail`, `UpdateTrail`, Änderungen an Event Selectors
- `PutBucketPolicy`, `PutBucketAcl`, `PutBucketLogging`, `PutBucketVersioning`
- `DisableKey`, `ScheduleKeyDeletion`, Änderungen an Key Policies und Grants
- `GetSecretValue`, `GetParameter`, `SendCommand`, `StartSession`
- `CreateFunction`, `UpdateFunctionCode`, `RunInstances`, `CreateStack`, `ExecuteChangeSet`
- Änderungen an Organizations, SCPs und delegierten Administratoren

### Eindämmung (nur mit Freigabe)

#### Access Key eines IAM-Benutzers deaktivieren

```bash
aws iam update-access-key \
  --user-name <USER> \
  --access-key-id <ACCESS_KEY_ID> \
  --status Inactive
```

#### Explizites Deny als kurzfristige Quarantäne

Eine Quarantäne-Policy stoppt einen Principal schneller als das unkoordinierte Entfernen vieler Berechtigungen. Policy vorab testen, Break-Glass ausschließen und den Rückweg dokumentieren.

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

#### Aktive Sitzungen einer Rolle widerrufen

Bereits ausgestellte STS-Credentials bleiben bis zu ihrem Ablauf gültig. Die Konsole bietet dafür „Revoke active sessions“ (fügt die Policy `AWSRevokeOlderSessions` an); per CLI dieselbe Wirkung mit einem Deny für alle vor dem Zeitpunkt ausgestellten Tokens:

```bash
aws iam put-role-policy \
  --role-name <ROLE_NAME> \
  --policy-name AWSRevokeOlderSessions \
  --policy-document file://revoke-older-sessions.json
```

`revoke-older-sessions.json` (Zeitpunkt = jetzt, UTC):

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

Danach die Ursache behandeln: Trust Policy, Permission Policies, Session Policies, SCPs und Resource Policies gemeinsam bewerten, sonst holt sich der Angreifer neue Sitzungen.

### Root und Account Recovery

- Root-E-Mail, MFA, Telefonnummer und alternative Kontakte prüfen.
- Unerwartete Root-Aktivität über CloudTrail und GuardDuty untersuchen.
- AWS Support über den vereinbarten Notfall- bzw. Enterprise-Support-Weg einschalten.
- In AWS Organizations prüfen, ob Root-Credentials zentral verwaltet werden oder kurzlebiger Root-Zugriff genutzt wurde; `AssumeRoot` und die Folgeaktivität korrelieren.

### Offizielle Referenzen

- [CloudTrail Event History und Einschränkungen](https://docs.aws.amazon.com/awscloudtrail/latest/userguide/view-cloudtrail-events.html)
- [IAM Credential Report](https://docs.aws.amazon.com/IAM/latest/UserGuide/id_credentials_getting-report.html)
- [Temporäre Credentials einer Rolle widerrufen](https://docs.aws.amazon.com/IAM/latest/UserGuide/id_roles_use_revoke-sessions.html)
- [Kompromittierte AWS-Credentials behandeln](https://docs.aws.amazon.com/guardduty/latest/ug/compromised-creds.html)
- [Privilegierte Root-Sitzungen in CloudTrail verfolgen](https://docs.aws.amazon.com/IAM/latest/UserGuide/cloudtrail-track-privileged-tasks.html)
- [AWS Security Incident Response Guide](https://docs.aws.amazon.com/security-ir/latest/userguide/what-is-security-ir.html)

## Anhang C: Google Workspace und Google Cloud

Google Workspace und Google Cloud teilen Identitäten, haben aber unterschiedliche Audit- und Administrationsflächen. Beide Seiten untersuchen. Zeiten als UTC im Format `2026-10-01T00:00:00Z` einsetzen.

### Einsatzregeln

- Super-Admin-Konten, privilegierte Gruppen, delegierte Admin-Rollen und Break-Glass-Konten zuerst sichern.
- Passwortwechsel und das Zurücksetzen der Sign-in-Cookies immer gemeinsam durchführen.
- Workspace-Logs über „Audit and investigation“ bzw. das Security Investigation Tool exportieren; API-Verfügbarkeit und Aufbewahrung hängen von Edition und Datenquelle ab.
- In Google Cloud werden Admin-Activity- und System-Event-Logs immer geschrieben; Data-Access-Logs meist nur, wenn sie vorher aktiviert wurden (Ausnahme unter anderem BigQuery).
- Service-Account-Keys, Workload Identity Federation und domainweite Delegation getrennt von Benutzerkonten untersuchen.

### Logquellen in Google Workspace

- Admin- und Benutzer-Log-Events, einschließlich Anmeldung und Kontoaktivität
- OAuth- und Token-Log-Events
- Drive-Log-Events und externe Freigaben
- Gmail-Log-Events und Email Log Search
- SAML, Context-Aware Access und Access Evaluation
- Groups, Geräte, Chrome und Endpunktaktivität
- Regeln, DLP, Vault, Takeout und Datenmigration
- Alert Center und Security Investigation Tool

Verfügbare Datenquellen und Reaktionsmöglichkeiten unterscheiden sich nach Workspace-Edition und Admin-Rechten.

### Workspace Reports API

Die Reports API liefert Aktivitäten je Anwendung. Das Token wird ohne Echo eingelesen, damit es nicht in der Shell-History landet:

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

Antworten mit `nextPageToken` vollständig paginieren. Tokens nicht in Tickets oder Artefaktdateien speichern. Für wiederholte Abrufe einen kontrollierten OAuth-Client mit möglichst wenigen Scopes verwenden.

### Workspace im Admin-Portal untersuchen

Pfad: **Reporting → Audit and investigation** bzw. **Security → Security center → Investigation tool**.

- Neue Super Admins und delegierte Admin-Rollen
- Änderungen an Passwort, Wiederherstellungsdaten, 2-Step Verification und Passkeys
- Verdächtige Session Cookies und ungewöhnliche Anmeldungen
- Neue OAuth-Clients, Grants und hochprivilegierte Scopes
- Domainweite Delegation und Marketplace-Anwendungen
- Änderungen an SAML/SSO und Context-Aware Access
- Gmail-Weiterleitungen, Delegationen, Routing und Compliance-Regeln
- Drive-Massendownloads, externe Freigaben, Eigentumsübertragungen und Löschungen
- Google Takeout, Vault-Exporte und Datenmigrationen
- Mitgliedschaften in privilegierten oder vertraulichen Gruppen

### Eindämmung in Workspace (nur mit Freigabe)

1. Konto gegebenenfalls sperren.
2. Passwort zurücksetzen.
3. Sign-in-Cookies separat zurücksetzen.
4. 2-Step-Verification- und Passkey-Registrierungen prüfen, unbekannte Methoden entfernen.
5. App-Passwörter und OAuth-Tokens widerrufen.
6. Weiterleitungen, Delegationen, Filter, Gruppen und Drive-Freigaben prüfen.
7. Bei SSO auch die Sitzung beim Identity Provider und das Quellkonto behandeln.

Ein Passwort-Reset allein beendet nicht zuverlässig jeden bestehenden Zugriff.

### Logquellen in Google Cloud

| Log | Zweck |
|---|---|
| Admin Activity | Änderungen an Konfiguration und Metadaten; immer geschrieben |
| Data Access | Lesen und Schreiben von Daten; meist explizit zu aktivieren |
| System Event | Von Google-Systemen ausgelöste Änderungen |
| Policy Denied | Durch Sicherheitsrichtlinien abgelehnte Zugriffe |
| VPC Flow Logs, Cloud DNS Logs, Firewall Rules Logging | Netzwerk- und DNS-Metadaten; nur wenn aktiviert |
| Security Command Center | Findings und Korrelation |
| Access Transparency | Zugriffe durch Google-Personal; editionsabhängig |

### Cloud-Kontext und IAM exportieren

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

Die Projekt-IAM-Policy zeigt keine geerbten Bindings. Organisation, Ordner, Projekte und relevante Ressourcen gemeinsam erfassen; IAM Deny, Principal Access Boundaries und Organization Policies zusätzlich prüfen.

### Cloud Audit Logs exportieren

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

Für Data-Access-Logs ist `roles/logging.privateLogViewer` oder ein gleichwertiges Recht nötig. Prüfen, ob Logs in andere Buckets, Projekte, BigQuery oder ein SIEM geleitet werden.

### Logging-Konfiguration sichern

```bash
gcloud logging sinks list --organization <ORG_ID> --format=json > gcp-org-log-sinks.json
gcloud logging sinks list --project <PROJECT_ID> --format=json > gcp-project-log-sinks.json
gcloud logging buckets list --location=global --project <PROJECT_ID> --format=json > gcp-log-buckets.json
gcloud logging exclusions list --project <PROJECT_ID> --format=json > gcp-log-exclusions.json
```

Änderungen an Sinks, Exclusions, Aufbewahrung, CMEK und Writer Identities sind besonders relevant.

### Workload-Identitäten und Persistenz

- Neu erstellte Service Accounts und vom Benutzer verwaltete Keys
- `roles/iam.serviceAccountTokenCreator`, `roles/iam.serviceAccountUser` und Key-Admin-Rechte
- Workload Identity Pools, Provider, Attribute Mappings und Vertrauen zu OIDC oder AWS
- Domainweite Delegation für Service Accounts
- Cloud Functions, Cloud Run, GKE und Build-Pipelines mit privilegierten Service Accounts
- Zugriffe auf Secret Manager, KMS, Storage, BigQuery und Logging
- Organization Policies, VPC Service Controls und Access Context Manager

### Eindämmung in Google Cloud (nur mit Freigabe)

#### Service-Account-Key deaktivieren

```bash
gcloud iam service-accounts keys disable <KEY_ID> \
  --iam-account <SERVICE_ACCOUNT_EMAIL> \
  --project <PROJECT_ID>
```

Erst deaktivieren, Abhängigkeiten beobachten, Ersatz bereitstellen und danach kontrolliert löschen. Bei Workload Identity oder kurzlebigen Tokens die ausstellende Vertrauensbeziehung behandeln.

#### IAM-Bindings ändern

Vor jeder Änderung die aktuelle Policy mit ETag sichern. Breite `set-iam-policy`-Operationen können parallele Änderungen überschreiben oder den eigenen Recovery-Zugang entfernen. Änderungen im Vier-Augen-Prinzip durchführen.

### Offizielle Referenzen

- [Google Workspace Investigation Tool: Datenquellen](https://support.google.com/a/answer/11482175)
- [Admin SDK Reports API: activities.list](https://developers.google.com/workspace/admin/reports/reference/rest/v1/activities/list)
- [Verdächtige Session Cookies untersuchen](https://support.google.com/a/answer/14211409)
- [Passwort und Sign-in-Cookies zurücksetzen](https://support.google.com/a/answer/33319)
- [Google Cloud Audit Logs](https://cloud.google.com/logging/docs/audit)
- [Data-Access-Audit-Logs konfigurieren](https://cloud.google.com/logging/docs/audit/configure-data-access)

## Anhang D: Okta

Okta Classic Engine und Identity Engine unterscheiden sich in einzelnen Abläufen. API-Aufrufe gegen die eigene Engine und die aktuelle Okta-Dokumentation prüfen. Zeiten als UTC im Format `2026-10-01T00:00:00Z` einsetzen.

### Einsatzregeln

- Wenn Okta der zentrale Identity Provider ist, Reaktionskanal und Admin-Anmeldung nicht allein von der möglicherweise kompromittierten Okta-Organisation abhängig machen.
- System Log sowie Admin-, App- und Richtlinienkonfiguration vor Änderungen lesend exportieren.
- Eingeschränkte OAuth-2.0-Management-Tokens langlebigen SSWS-Tokens vorziehen.
- Benutzersitzungen, OAuth-Tokens, gemerkte Geräte und Sitzungen in Zielanwendungen sind getrennte Ebenen.
- Das Sperren eines Okta-Benutzers beendet Okta-Sitzungen, aber nicht zwingend Sitzungen, die eine Zielanwendung selbst verwaltet.

### Wichtige Quellen

- System Log und System Log API
- Admin-Rollen und Rollenzuweisungen
- Änderungen an Benutzern, Gruppen und Gruppenregeln
- Applications, App-Zuweisungen und Sign-on Policies
- Authorization Servers, OAuth-Clients, Grants und Scopes
- Identity Providers, Routing Rules und Föderation
- Authenticators, Enrollment und Faktor-Resets
- Network Zones, Behavior Detection und Global Session Policy
- API-Tokens, OAuth-Service-Apps und Device Assurance
- Event Hooks, Inline Hooks und Workflows

### Variablen vorbereiten

```bash
OKTA_DOMAIN='example.okta.com'
read -rs OKTA_IR_TOKEN && export OKTA_IR_TOKEN
SINCE='<START_UTC>'
UNTIL='<END_UTC>'
```

`read -rs` liest das Token ohne Echo und ohne Eintrag in der Shell-History. Header für SSWS-Tokens: `Authorization: SSWS <token>`; für OAuth: `Authorization: Bearer <token>`.

### System Log exportieren

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

Weitere Seiten über den `Link`-Header mit `rel="next"` abrufen. `published`, `uuid`, `eventType`, Actor, Client, Target, Outcome, Debug Context und Transaction korrelieren.

### Gezielte Suchen im System Log

```bash
# Ereignisse eines Benutzers
curl -sS --get -H "Authorization: Bearer ${OKTA_IR_TOKEN}" -H 'Accept: application/json' \
  --data-urlencode "since=${SINCE}" --data-urlencode "until=${UNTIL}" \
  --data-urlencode 'filter=actor.alternateId eq "user@example.com"' \
  "https://${OKTA_DOMAIN}/api/v1/logs" > okta-user-events.json

# Zugriffe auf die Admin-Konsole
curl -sS --get -H "Authorization: Bearer ${OKTA_IR_TOKEN}" -H 'Accept: application/json' \
  --data-urlencode "since=${SINCE}" --data-urlencode "until=${UNTIL}" \
  --data-urlencode 'filter=eventType eq "user.session.access_admin_app"' \
  "https://${OKTA_DOMAIN}/api/v1/logs" > okta-admin-console-access.json

# Freitextsuche nach IP, Domain oder Objekt-ID
curl -sS --get -H "Authorization: Bearer ${OKTA_IR_TOKEN}" -H 'Accept: application/json' \
  --data-urlencode "since=${SINCE}" --data-urlencode "until=${UNTIL}" \
  --data-urlencode 'q=<IP_OR_DOMAIN_OR_OBJECT_ID>' \
  "https://${OKTA_DOMAIN}/api/v1/logs" > okta-search.json
```

### Benutzer, Gruppen, Anwendungen und Grants inventarisieren

```bash
for EP in users groups apps idps; do
  curl -sS -H "Authorization: Bearer ${OKTA_IR_TOKEN}" -H 'Accept: application/json' \
    "https://${OKTA_DOMAIN}/api/v1/${EP}?limit=200" > "okta-${EP}-page-001.json"
done

curl -sS -H "Authorization: Bearer ${OKTA_IR_TOKEN}" -H 'Accept: application/json' \
  "https://${OKTA_DOMAIN}/api/v1/users/<USER_ID>/grants" > okta-user-grants.json
```

Alle Endpunkte vollständig paginieren. Client-Secrets werden nicht zwingend im Klartext zurückgegeben; Metadaten und letzte Änderungen trotzdem sichern.

### Kritische Themen im System Log

- Admin-Anmeldungen, neue Admin-Rollen und Rechteausweitung
- Aktivierung, Sperrung, Passwort- und Faktor-Resets von Benutzern
- Neue oder geänderte Authenticators und Enrollment Policies
- Neue Anwendungen, geänderte App-Credentials und App-Zuweisungen
- Änderungen an Identity Providers, Routing Rules, SAML und OIDC
- Neue API-Tokens, OAuth-Service-Apps und ungewöhnliche Nutzung der Management-API
- Network Zones, Trusted Origins, Global Session Policy und MFA-Umgehung
- Gruppenregeln und -mitgliedschaften mit nachgelagerten Rechten
- System-Log-Streaming, Event Hooks, Inline Hooks und Workflows

### Eindämmung (nur mit Freigabe)

#### Alle Okta-Sitzungen und OAuth-Tokens eines Benutzers widerrufen

```bash
curl -sS -X DELETE -H "Authorization: Bearer ${OKTA_IR_TOKEN}" -H 'Accept: application/json' \
  "https://${OKTA_DOMAIN}/api/v1/users/<USER_ID>/sessions?oauthTokens=true&forgetDevices=true"
```

Sitzungen, die Web- oder Native-Apps selbst verwalten, bleiben davon unberührt. Zielanwendungen separat behandeln.

#### Alle Grants eines Benutzers widerrufen

```bash
curl -sS -X DELETE -H "Authorization: Bearer ${OKTA_IR_TOKEN}" -H 'Accept: application/json' \
  "https://${OKTA_DOMAIN}/api/v1/users/<USER_ID>/grants"
```

#### Benutzer sperren

```bash
curl -sS -X POST -H "Authorization: Bearer ${OKTA_IR_TOKEN}" -H 'Accept: application/json' \
  "https://${OKTA_DOMAIN}/api/v1/users/<USER_ID>/lifecycle/suspend"
```

Vorher Profil, Gruppen, Rollen, Faktoren, Sitzungen, Grants und relevante System-Log-Ereignisse sichern. Break-Glass- und Dienstkonten nicht ohne geprüften Recovery-Weg sperren.

### Prüfung vor dem Wiederanlauf

- Mindestens zwei unabhängige Super-Admin-Zugänge mit phishing-resistenter MFA
- Admin-Zugriff nur aus kontrollierten Netzwerkzonen und Geräten
- Keine unbekannten Identity Providers, Routing Rules, Trusted Origins oder Hooks
- Admin-Rollen und privilegierte Gruppen nur mit erwarteten Mitgliedern
- API-Tokens und OAuth-Service-Apps inventarisiert und rotiert
- Sign-on Policies, Global Session Policy und Authenticators geprüft
- Sitzungen in nachgelagerten SaaS-Anwendungen und Provisioning-Connectoren separat geprüft
- System Log vollständig in ein externes SIEM oder unveränderliches Archiv exportiert

### Offizielle Referenzen

- [Okta System Log Query](https://developer.okta.com/docs/reference/system-log-query/)
- [Okta Core API](https://developer.okta.com/docs/reference/core-okta-api/)
- [OAuth-Tokens widerrufen](https://developer.okta.com/docs/guides/revoke-tokens/)
- [Okta Management API: Berechtigungen](https://developer.okta.com/docs/api/openapi/okta-management/guides/permissions)

## Disclaimer

Dieses Playbook ist eine praktische Community-Ressource. Es ersetzt keine Rechtsberatung, forensische Expertise, Anforderungen der Cyberversicherung, Unterstützung durch den Cloud-Anbieter oder organisationsspezifische Incident-Response-Planung. Die Befehle in den Anhängen sind Beispiele; Syntax und Verfügbarkeit ändern sich mit den Plattformen und sind vor dem Einsatz zu prüfen.
