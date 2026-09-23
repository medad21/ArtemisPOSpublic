# Artemis — POS- und Warenwirtschaftssystem für den Einzelhandel

🌐 **[🇮🇷 فارسی](README.md) | [🇬🇧 English](README.en.md) | [🇩🇪 Deutsch](README.de.md) | [🇸🇦 العربية](README.ar.md)**

**Artemis** ist ein vollständiges, selbst gehostetes Kassen- und Filialverwaltungssystem, das über zwei Jahre hinweg im echten, täglichen Einsatz in einem realen Geschäft entwickelt wurde. Es begann als Weg, teure kommerzielle Kassensoftware und deren Installationsprobleme zu vermeiden, und wuchs Funktion für Funktion zu einer vollständigen Plattform für die Geschäftsführung heran: Verkauf, Lager, Kunden, Personal, Buchhaltung und Drucken — alles an einem Ort.

Es läuft als Web-App (nutzbar über jeden Browser, auf jedem Gerät) **und** als native Android-App, beide verbunden mit deinem eigenen [Supabase](https://supabase.com)-Projekt — das heißt, **du besitzt deine Daten vollständig**. Keine monatliche SaaS-Gebühr, keine Abhängigkeit von unseren Servern.

**Repository:** https://github.com/medad21/ArtemisPOS
**Kontakt:** bagheryane@gmail.com

## 🧩 Für Anpassung gebaut

Artemis ist nicht auf eine bestimmte Art von Geschäft festgelegt. Das Geschäft, für das dies gebaut wurde, betreibt nebenbei auch einen Druck-/Kopierservice, weshalb ein **Dienstleistungen**-Modul hinzugefügt wurde: Statt eines einfachen Artikelverkaufs kann jede Dienstleistung (ein Farbdruck, eine Bindearbeit, ein Kopierauftrag …) in ihre eigenen Kostenkomponenten aufgeteilt werden (Papier, Tinte/Toner, Arbeitslohn usw.), sodass die tatsächliche Gewinnspanne pro Auftrag getrennt von normalen Einzelhandelsverkäufen erfasst wird — mit eigener Berichterstattung, aufgeschlüsselt nach Dienstleistungsart und Tag. Das ist die Grundidee des gesamten Projekts: Jedes Modul wurde hinzugefügt, weil ein echtes, konkretes Bedürfnis entstand, und der Code ist einfach genug (reines HTML/JS, kein Build-Schritt), dass jeder ihn öffnen und einen Bereich an sein eigenes Geschäft anpassen kann.

## ✨ Funktionen

**Verkauf & Kasse**
- Schnelle Verkaufserfassung mit Barcode-Scan (kamerabasiert, keine zusätzliche Hardware nötig)
- Rabatte, mehrere Zahlungsarten (bar, Karte, Überweisung, auf Kredit)
- Kollegen-/Großhandelsverkauf für Geschäftskunden
- Ein separates **Dienstleistungen**-Modul für auftragsbasierte Arbeit (Drucken, Reparaturen, Sonderaufträge …) mit aufgeschlüsselten Kostenkomponenten und eigener Gewinnberichterstattung

**Lagerverwaltung**
- Vollständige Bestandsverfolgung mit Warnungen bei niedrigem Lagerbestand
- Artikelkategorien, Kardex (Ein-/Ausgangsverlauf) pro Artikel
- Erstellung und Druck scanbarer Barcode-Etiketten für Artikel ohne Barcode

**Rückgaben & Umtausch**
- Vollständige oder teilweise Rückgaben mit automatischer Bestands- und Gewinnanpassung
- Artikeltausch (Kunde tauscht gegen einen anderen Artikel), wobei nur die Preisdifferenz abgerechnet wird — vollständig nachverfolgt, keine manuelle Notlösung

**Kunden**
- Kundenprofile, Kaufhistorie, Schuldenverfolgung
- Treue-/Punkteprogramm mit VIP- und Vielkäufer-Stufen
- Massen-SMS an Schuldner, alle Kunden oder beliebige Nummern — mehrere Anbieter (Kavenegar, MeliPayamak, SMS.ir oder jede andere REST-API per Webhook)

**Mehrbenutzerfähig**
- Manager- und Mitarbeiterrollen mit getrennten Logins
- Nachverfolgung der Aktionen jedes Mitarbeiters zur Rechenschaftspflicht

**Buchhaltung**
- Bilanz, mehrere Kassen-/Bankkonten
- Ausgabenverfolgung, Eröffnungsbilanz, automatische Backups

**Drucken**
- Normale Drucker (A4/A5/A6) und Thermo-Bondrucker (58mm/80mm)
- Direkter Bluetooth-Druck aus der Android-App (kein Betriebssystem-Druckertreiber nötig)
- Vollständig anpassbarer Rechnungsinhalt und Betrag in Worten

**Alles Weitere**
- 4 Sprachen: Persisch, Englisch, Deutsch, Arabisch (mit vollständiger RTL-Unterstützung)
- Mehrwährungsfähig
- Dunkler/heller Modus
- Erinnerungen für Einkäufe und Scheckzahlungen

## 📸 Screenshots

*(Füge deine eigenen Screenshots unter `docs/screenshots/` mit den unten stehenden Dateinamen hinzu — sie erscheinen dann automatisch hier.)*

| | |
|---|---|
| **Verkauf & Kasse**<br>![Verkauf](docs/screenshots/sales.png) | **Lagerverwaltung**<br>![Lager](docs/screenshots/inventory.png) |
| **Rückgaben & Umtausch**<br>![Rückgaben](docs/screenshots/returns.png) | **Kundenclub & Treueprogramm**<br>![Kundenclub](docs/screenshots/customer-club.png) |
| **Berichte & Bilanz**<br>![Berichte](docs/screenshots/reports.png) | **Einstellungen**<br>![Einstellungen](docs/screenshots/settings.png) |

## 🚀 Erste Schritte

Artemis benötigt ein [Supabase](https://supabase.com)-Projekt (die kostenlose Stufe reicht zum Starten) zur Speicherung deiner Daten.

1. Erstelle ein kostenloses Supabase-Projekt.
2. Öffne die App (`index.html`) in einem Browser — du siehst einen Ersteinrichtungsbildschirm.
3. Führe im SQL-Editor deines Supabase-Projekts das Tabellenerstellungsskript aus (die App kann dieses nach der Verbindung selbst generieren — siehe „Einstellungen → SQL-Skript herunterladen“, oder nutze die Version in diesem Repository).
4. Gib die Projekt-URL und den anon/public-Schlüssel deines Supabase-Projekts im Einrichtungsbildschirm ein.

Das war's — deine Daten liegen vollständig in deinem eigenen Supabase-Projekt.

## 📱 Android-App

Artemis ist mit [Capacitor](https://capacitorjs.com) auch als native Android-App verpackt, mit nativen Funktionen wie kamerabasiertem Barcode-Scan, biometrischem Login und lokalen Benachrichtigungen. Siehe den Ordner `android/` und den GitHub-Actions-Workflow unter `.github/workflows/` für die Build-Pipeline.

## 🛠 Technologie-Stack

- Einzeldatei-Frontend aus HTML/CSS/JS — kein Build-Schritt nötig, um es im Browser auszuführen
- [Supabase](https://supabase.com) (Postgres + Auth + Realtime + Edge Functions) als Backend
- [Capacitor](https://capacitorjs.com) für den Android-App-Wrapper

## 📄 Lizenz

Siehe [LICENSE](LICENSE) für die Nutzungsbedingungen.

## 📬 Kontakt

Fragen, Anpassungswünsche oder Lizenzanfragen: **bagheryane@gmail.com**

---

*Entwickelt von Ehsan Bagheryan.*
