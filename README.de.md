# Artemis ERP — Warenwirtschafts- und Kassensystem

🌐 **[English](README.md) | [فارسی](README.fa.md) | [Deutsch](README.de.md) | [العربية](README.ar.md)**

**Eine Full-Stack-Anwendung zur Geschäftsverwaltung und als Kassensystem (POS) für kleine Unternehmen und Einzelhandelsgeschäfte, mit integriertem, datenbankgestütztem KI-Assistenten und Umsatzprognose.**

`JavaScript` · `HTML/CSS` · `Supabase` · `PostgreSQL` · `Capacitor` · `Gemini API`

![Artemis Dashboard](docs/screenshots/dashboard.png)

**Auf einen Blick:**
- Vollständiges Einzelhandels-POS/ERP-System (Verkauf, Lager, Rückgaben, Buchhaltung, Druck) — plus datenbankgestützter KI-Assistent und heuristische Umsatzprognose, nicht nur CRUD.
- Selbst gehostet auf einem eigenen Supabase-Projekt (Postgres + Auth + Realtime + Edge Functions); läuft sowohl als Web-App als auch als native Android-App über Capacitor.
- Über zwei Jahre im echten, täglichen Einsatz in einem realen Geschäft entwickelt und verfeinert — kein Tutorial-Projekt.

---

## Überblick

**Artemis** begann als Weg, teure kommerzielle Kassensoftware für ein echtes Einzelhandelsgeschäft — und deren Installationsprobleme — zu vermeiden, und wuchs über zwei Jahre täglichen Produktiveinsatzes Funktion für Funktion zu einer vollständigen Plattform für die Geschäftsführung heran: Verkauf, Lager, Kunden, Personal, Buchhaltung, Drucken, und ein KI-Assistent, der die Daten des Geschäfts tatsächlich kennt.

Es läuft als browserbasierte Web-App **und** als native Android-App (über Capacitor), beide gestützt auf ein selbst gehostetes [Supabase](https://supabase.com)-Projekt — jede Installation besitzt ihre Daten vollständig selbst.

> **Projektstatus:** Aktives persönliches Projekt, im täglichen Produktiveinsatz. Dieses Repository ist eine öffentliche Demo-/Portfolio-Version — siehe [Hinweis zum öffentlichen Repository](#hinweis-zum-öffentlichen-repository).

## Warum Artemis?

Das Projekt wurde um reale betriebliche Probleme herum aufgebaut, nicht als akademische CRUD-Übung. Funktionen wurden hinzugefügt, wenn ein konkreter, praktischer Bedarf entstand:

```text
Reales Geschäftsproblem → Workflow entwerfen → umsetzen → mit echten
Verkäufen testen → Grenzfälle finden → Zuverlässigkeit verbessern →
in das Gesamtsystem integrieren
```

Konkrete Beispiele aus diesem Code: Artikeltausch, bei dem nur die Preisdifferenz abgerechnet wird statt zwei vollständiger Transaktionen; eine Offline-Verkaufswarteschlange, die bei Verbindungsabbruch mitten im Verkauf keine doppelten Rechnungen erzeugen kann; eine austauschbare SMS-/Druck-Anbieterschicht statt fest codiertem einzelnen Anbieter; und eine Umsatzprognose, die sich anhand echter Verlaufsdaten zu Anlasstagen selbst anpasst, statt eines fest eingestellten Multiplikators.

---

## Kernfunktionen

**Verkauf & Kasse** — Kassiervorgang mit Barcode-Scan, Rabatte, Zahlung per Bar/Karte/Überweisung/Kredit, Großhandels-(Kollegen-)Verkauf, sowie ein separates Modul zur Auftragskalkulation für Dienstleistungen (z. B. Druck-/Kopieraufträge) mit aufgeschlüsselten Kostenkomponenten.

**Lagerverwaltung** — Bestandsverfolgung mit Warnungen bei niedrigem Lagerbestand, Kategorien, Kardex (Ein-/Ausgangsverlauf) pro Artikel, sowie Erstellung/Druck von Barcode-Etiketten für Artikel ohne vorhandenen Barcode.

**Rückgaben & Umtausch** — Vollständige oder teilweise Rückgaben mit automatischer Bestands- und Gewinnanpassung; Artikeltausch, bei dem nur die Preisdifferenz abgerechnet wird, vollständig nachverfolgt statt als manuelle Notlösung gehandhabt.

**Kundenverwaltung** — Profile, Kaufhistorie, Schuldenverfolgung, sowie Massen-SMS an Schuldner/alle Kunden/beliebige Nummern über eine austauschbare Anbieterschicht (Kavenegar, MeliPayamak, SMS.ir oder jede REST-API über einen generischen Webhook).

**Treueprogramm** — Punktebasierte Kundenbindung mit konfigurierbaren Sammel-/Einlöseraten und kaufbasierten Kundenstufen (Standard / aktiver Käufer / VIP).

**Buchhaltung & Berichte** — Bilanz, mehrere Kassen-/Bankkonten, Ausgabenverfolgung, Eröffnungsbilanz, Tages-/Gewinnberichte sowie Erinnerungen für Einkäufe und Scheckzahlungen.

**Drucken** — A4/A5/A6 sowie Thermo-Bondruck (58mm/80mm), direkter Bluetooth-Druck aus der Android-App, vollständig konfigurierbarer Rechnungsinhalt und Betrag in Worten.

**Mehrbenutzerfähig** — Manager-/Mitarbeiterrollen mit getrennten Logins und Nachverfolgung der Aktionen jedes Mitarbeiters.

**Lokalisierung** — Persisch, Englisch, Deutsch, Arabisch, mit vollständiger RTL-Unterstützung und mehreren Währungen.

**Web + Android** — Einzeldatei-Web-App (kein Build-Schritt nötig) sowie eine mit Capacitor verpackte Android-App mit kamerabasiertem Barcode-Scan, Fingerabdruck-Login und lokalen Benachrichtigungen.

---

## KI-gestützter Geschäftsassistent

Eine der zentralen Funktionen von Artemis ist ein Chat-Assistent, der um die tatsächlichen Daten des Geschäfts herum aufgebaut ist — kein generischer, nachträglich angehängter Chatbot.

### Zwei Persönlichkeiten

- **Artemis** — ein formellerer Assistent für Finanz- und Geschäftsanfragen.
- **Aria** — eine lockerere, gesprächigere Persönlichkeit für schnelle Alltagsfragen.

Jede hat ihr eigenes Farbschema, ihren eigenen Begrüßungsstil und Ton; das Umschalten zwischen beiden erfolgt sofort über das Chat-Widget.

### Eine hybride, dreischichtige Architektur

1. **Lokale Intent-Engine** — Begrüßungen, Danksagungen und andere häufige Muster werden direkt durch In-App-Logik erkannt und beantwortet, ohne Server-Anfrage.
2. **Direkte Datenbankabfragen** — Geschäftsfragen („Wie viel haben wir heute verkauft?“, „Wer sind die Top-Kunden?“, „Welche Produkte haben niedrigen Bestand?“) werden durch direkte Abfrage von Supabase/PostgreSQL vom Client aus beantwortet.
3. **KI-/serverseitige Verarbeitung (Gemini)** — Alles Offenere wird an eine Supabase Edge Function (`gemini-handler`) weitergeleitet, wodurch KI-Aufrufe und zugehörige Geheimnisse vollständig vom Client ferngehalten werden.

### Umsatzprognose

Der Assistent kann die kommende Woche oder den kommenden Monat anhand echter Verkaufshistorie prognostizieren — mit einem regelbasierten (heuristischen) Modell, nicht einem trainierten Machine-Learning-Modell:

- Erstellt eine **Basislinie pro Wochentag** aus der Verkaufshistorie.
- Lernt einen echten **Anlass-Multiplikator** für wiederkehrende Ereignisse (z. B. einen bestimmten Feiertag), indem tatsächliche vergangene Verkäufe an diesem Anlass mit der Wochentags-Basislinie verglichen werden, gewichtet so, dass jüngere Vorkommen stärker zählen als ältere — mit Rückgriff auf einen manuell konfigurierten Multiplikator, wenn noch nicht genug Verlaufsdaten vorliegen.
- Weist jedem Prognosetag ein **Vertrauensniveau** zu (hoch/mittel/niedrig/unklar), basierend darauf, wie viele historische Daten dahinterstehen, und kennzeichnet Tage mit geringem Vertrauen in der Ausgabe, statt jede Zahl mit gleicher Sicherheit darzustellen.
- Kann die Prognose zusätzlich danach aufschlüsseln, welche Artikel voraussichtlich dazu beitragen.

### Assistenten-Gedächtnis

Zwei Ebenen von Kontext: kurzfristiger Gesprächsverlauf für die aktuelle Sitzung sowie eine kleine Menge langfristiger, in der Datenbank gespeicherter Fakten, damit der Assistent nützlichen Kontext über Sitzungen hinweg beibehalten kann, statt jedes Mal von vorn zu beginnen.

### Schutz sensibler Daten

Finanz- und Bestandsabfragen können einen separaten Zugangscode erfordern, bevor der Assistent sie preisgibt — zusätzlich zu Supabases eigener Row Level Security und rollenbasierten Regeln.

---

## Technische Architektur

```text
                          Artemis
                              |
             +----------------+----------------+
             |                                 |
         Web / Browser                   Android-App
             |                                 |
             +----------------+----------------+
                              |
                           Supabase
                              |
       +----------------------+----------------------+
       |                      |                       |
   PostgreSQL              Auth (RLS)              Realtime
       |
   Edge Functions
       |
  Gemini-KI-Assistent · SMS-Gateway · Rechnungsscanner
```

**Frontend** — Einzeldatei-HTML5/CSS3/JavaScript, kein Build-Schritt, responsiv und RTL-fähig.
**Backend** — Supabase: PostgreSQL, Auth, Realtime, Edge Functions, Row Level Security.
**Mobil** — Mit Capacitor verpackte Android-App mit nativen Geräte-APIs.
**KI** — Google Gemini über eine dedizierte Edge Function, plus die oben beschriebene lokale Intent-/Prognoselogik.
**Automatisierung** — GitHub Actions für Android-Builds.

---

## Offline & Synchronisierung

Verkäufe können weiterhin erfasst werden, wenn die Verbindung abbricht. Ausstehende Verkäufe werden in einer lokalen Warteschlange gehalten, die über eine clientseitig generierte ID indiziert ist; wenn die Verbindung wiederhergestellt ist, wird die Warteschlange anhand dieser ID abgeglichen, sodass eine mitten in der Synchronisierung abgebrochene Verbindung nicht dieselbe Rechnung zweimal erzeugen kann.

---

## Sicherheit & Autorisierung

- Supabase-Authentifizierung und PostgreSQL Row Level Security (RLS)
- Rollenbasierte Zugriffsregeln (Manager vs. Mitarbeiter)
- Ein separater Zugangscode, der sensible Assistentenabfragen (Verkauf, Gewinn, Lager, Finanzdaten) absichert
- Serverseitige Edge Functions für KI-Aufrufe, SMS-Versand und Rechnungsscan, sodass Anbieter-Zugangsdaten den Client nie erreichen

> **Vor der Bereitstellung:** Richte dein eigenes Supabase-Projekt ein. Committe niemals Service-Role-Schlüssel, Passwörter, private API-Schlüssel oder echte Produktions-/Kundendaten in ein öffentliches Repository.

---

## Screenshots

Vollständige Bilder befinden sich in [`docs/screenshots/`](docs/screenshots/).

| | |
|---|---|
| **Kasse / Verkauf**<br>![Verkauf](docs/screenshots/sales.png) | **Lager**<br>![Lager](docs/screenshots/inventory.png) |
| **Rückgaben & Umtausch**<br>![Rückgaben](docs/screenshots/returns.png) | **Kunden & Treueprogramm**<br>![Kundenclub](docs/screenshots/customer-club.png) |
| **Berichte & Bilanz**<br>![Berichte](docs/screenshots/reports.png) | **Einstellungen**<br>![Einstellungen](docs/screenshots/settings.png) |
| **KI-Assistent**<br>![Assistent](docs/screenshots/assistant.png) | **Android-App**<br>![Android](docs/screenshots/android.png) |

---

## Erste Schritte

Artemis nutzt dein eigenes Supabase-Projekt zur Speicherung — du besitzt deine Daten vollständig.

1. Erstelle ein kostenloses [Supabase](https://supabase.com)-Projekt.
2. Öffne `index.html` in einem Browser — du siehst einen Ersteinrichtungsbildschirm.
3. Führe im SQL-Editor deines Supabase-Projekts das Tabellenerstellungsskript aus (die App kann es nach der Verbindung selbst generieren, über **Einstellungen → SQL-Skript herunterladen**).
4. Gib die Projekt-URL und den anon/public-Schlüssel deines Supabase-Projekts im Einrichtungsbildschirm ein.

Es ist kein Frontend-Build-Schritt erforderlich.

## Android-Build

Die Android-App ist mit [Capacitor](https://capacitorjs.com) verpackt. Siehe den Ordner `android/` und den GitHub-Actions-Workflow unter `.github/workflows/` für die Build-Pipeline. Native Funktionen umfassen kamerabasierten Barcode-Scan, biometrische Authentifizierung, lokale Benachrichtigungen und direkten Bluetooth-Thermodruck.

---

## Technische Highlights

- Tausch-Abrechnung, die nur die Preisdifferenz ausgleicht, statt zwei sich gegenseitig aufhebender Volltransaktionen
- Offline-Verkaufswarteschlange mit clientseitig generierten IDs zur Vermeidung doppelter Rechnungen bei Wiederverbindung
- Dreischichtige Assistenten-Architektur (lokaler Intent → direkte DB-Abfrage → serverseitige KI) statt jede Nachricht durch ein LLM zu leiten
- Umsatzprognose mit einem gelernten, nach Aktualität gewichteten Anlass-Multiplikator statt einem statischen, von Hand eingestellten
- Row Level Security und rollenbasierte Zugriffskontrolle durchgängig
- Austauschbare SMS- und Druck-Anbieterschichten (keine Anbieterbindung bei beiden)
- Vollständige RTL- und Mehrsprachenunterstützung (Persisch, Englisch, Deutsch, Arabisch)
- Android-Verpackung über Capacitor mit nativer Geräteintegration

---

## Repository-Struktur

```text
ArtemisPOSpublic/
├── index.html
├── docs/
│   └── screenshots/
├── README.md
├── README.fa.md
├── README.de.md
└── README.ar.md
```

Dieses öffentliche Repository ist eine Demonstrations-/Portfolio-Version, die die Anwendung und ihre Architektur zeigen soll.

## Hinweis zum öffentlichen Repository

Dies ist eine öffentliche Demo-Version eines privaten, aktiv genutzten Produktivsystems. Es enthält keine — und es dürfen keine — Service-Role-Schlüssel, private API-Schlüssel, Passwörter, echte Kundendaten, echte Finanzdaten oder sonstige Produktions-Zugangsdaten committet werden. Für einen echten Einsatz verbinde Artemis mit deinem eigenen Supabase-Projekt.

---

## Autor

**Ehsan Bagheryan** — Full-Stack-Entwickler mit Fokus auf praxisnahe Geschäftsanwendungen, Supabase/PostgreSQL, Offline-First-Workflows und KI-Integration.

- E-Mail: `bagheryane@gmail.com`
- GitHub: [@medad21](https://github.com/medad21)

## Lizenz

Siehe [LICENSE](LICENSE) für die Nutzungsbedingungen.
