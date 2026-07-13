## v1.42.2 — PDF-Beweis in der Vorbuchhaltung: OCR-Felder werden gegen das PDF verifiziert

**Verbesserungen**
- **PDF-Text-Beweis beim Einlesen:** Bevor ein Beleg in die Buchungs-Queue einsortiert wird, prüft Bruno deterministisch, ob Betrag und Rechnungsnummer aus der Texterkennung WIRKLICH so im PDF stehen und ob netto + Steuer = brutto aufgeht. Lesefehler (Zahlendreher, Login-Codes als Betrag) werden sofort markiert — nichts Falsches erreicht mehr die Buchhaltungs-App. Mahnungen/Proforma werden schon hier als „kein Beleg" getypt.
- Font-Verwechslungen der Texterkennung (1/I, 0/O) lösen keinen Fehlalarm aus. Getestet: 5/5 Fehler-Szenarien gefangen, 0 Fehlalarme auf 80 echten Belegen.

## v1.42.1 — §14-Gate: Vorsteuer nur mit vollständiger Rechnung

**Verbesserungen**
- **§14-Gate:** Wo Vorsteuer gezogen werden soll (Steuersatz > 0, Betrag > 250 €), prüft Bruno vor dem Buchen, ob die Rechnung eine Steuernummer/USt-ID des Ausstellers trägt (§14 Abs. 4 UStG) — fehlt sie, geht der Beleg in die Prüfliste („vollständige Rechnung anfordern") statt gebucht zu werden. Auslands-/Reverse-Charge-/Kleinbetragsrechnungen bleiben unberührt. Getestet mit Beispiel-Rechnungen, 0 Fehlalarme am Bestand.
- Mahnungs-Gate kennt jetzt auch Proforma-Rechnungen.
- Release-Kanal verweigert technisch Rückwärts-Versionen (Schutz bei parallelen Arbeitssträngen).

## v1.42.0 — Belege laufen jetzt bis „Gebucht" durch (Heilung + Ursache behoben)

**Neue Funktionen**
- **Automatische Heilung „teilweise bezahlt"-Hänger:** Manche voll bezahlten Belege blieben in sevDesk auf „teilweise bezahlt" stehen, obwohl die Zahlung komplett verknüpft war. Bruno hat die Ursache gefunden (ein bestimmter Buchungs-Typ der sevDesk-API schließt den Beleg nicht sauber ab) und ein Heil-Werkzeug gebaut: es erkennt alle Hänger, bucht sie sauber neu durch und prüft jeden einzelnen danach nach (Status „Gebucht", Bankumsatz „gebucht", interne Summen korrekt). Echte Teilzahlungen werden dabei NICHT angefasst — die warten zu Recht auf die Restzahlung und stehen jetzt mit Grund auf einer Warteliste.

**Verbesserungen**
- **Neue Verknüpfungen entstehen ab sofort direkt richtig:** Die Bank-Zuordnung nutzt jetzt immer den funktionierenden Buchungs-Typ — neue Belege landen direkt auf „Gebucht" statt auf halber Strecke.
- **Neue Selbstkontrolle:** Der Buchhaltungs-Check kennt den Fall jetzt als eigene Prüfung — sollte der Hänger je wieder auftauchen, schlägt Bruno sofort Alarm (und echte Teilzahlungen werden als normale Warteklasse grün angezeigt).

**Unter der Haube**
- bookAmount `type:'O'` statt `FULL_PAYMENT` in match-vouchers/match-clusters (empirisch: FULL_PAYMENT lässt Status 750 + falsches Accounting-Vorzeichen zurück) · neues Tool `durchbucher.mjs` (Dry-Run-Standard, Readback je Beleg, Audit-Log, Rollback dokumentiert) · Health-Check-Dimension 13 · API-Wissen in CAPABILITIES korrigiert.

## v1.41.0 — Sammel-Release (Konsolidierung zweier paralleler Arbeitsstränge)

**Hinweis:** bündelt alles bis v1.40.1 (u.a. Mahnungs-Gate) und v1.39.0 (Noise-Gate-Muster). Details darunter.

## v1.40.1 — Schutz: Mahnungen werden nie als Rechnung gebucht

**Verbesserungen**
- **Mahnungs-Gate:** Mahnungen und Zahlungserinnerungen tragen oft Rechnungsnummer + Betrag und sehen für die Texterkennung wie Rechnungen aus — sind aber keine (Pflichtangaben fehlen, kein Vorsteuerabzug). Bruno prüft jetzt jedes Dokument vor dem Buchen deterministisch auf Mahnungs-Signale: Treffer → Prüfliste mit dem Hinweis „Original-Rechnung beim Anbieter anfordern", nie gebucht. Getestet: Beispiel-Mahnung abgefangen, 80 echte gebuchte Rechnungen ohne Fehlalarm.

## v1.40.0 — Sammel-Release: alle Verbesserungen dieses Wochenendes in einem Paket

**Hinweis:** Dieses Release bündelt v1.33–v1.39 (zwei parallele Arbeitsstränge). Details in den Einträgen darunter — Highlights: Modus 17 (Kontoauszüge PDF/CSV), Link-Rechnungen automatisch holen, Postausgang-Scan (Gmail + IMAP), FYRST-Bank-Adapter, DATEV-Wegweiser, Prüfer-Härtungen.

## v1.39.0 — Weniger Fehlalarme, sauberere Eingangs-Queue

**Verbesserungen**
- **Mehr Nicht-Belege werden automatisch erkannt:** Testzeitraum-Erinnerungen, Sicherheitswarnungen und "Zahlungsdaten prüfen"-Mails landen nicht mehr als vermeintliche Belege in der Buchungs-Warteschlange (15 solcher Dateien wurden direkt mit aussortiert — reversibel).
- **Schlauere Selbstkontrolle:** Der unabhängige Kontroll-Blick nach jedem Buchungslauf bekommt jetzt automatisch die Aufschlüsselung mit, WARUM Umsätze noch offen sind — bekannte Warte-Fälle lösen keinen Fehlalarm mehr aus, echte Auffälligkeiten schon.

**Unter der Haube**
- Diagnose abgeschlossen, warum manche voll bezahlten Belege als "teilweise bezahlt" angezeigt werden (ein Vorzeichen in der internen Buchungssumme) — die automatische Heilung kommt im nächsten Update.

## v1.39.0 — DATEV-Übergabe an den Steuerberater: klarer Wegweiser

**Neues Wissen**
- **Welchen Export dein Steuerberater bekommt, ist jetzt sauber dokumentiert:** In sevDesk gibt es zwei Export-Arten — „Buchungsdaten + Belegbilder" (deine fertige Buchhaltung als DATEV-Buchungsstapel) und „Rechnungsdaten + Belegbilder" (nur Belege, der Steuerberater bucht selbst). Bruno empfiehlt für sein Setup immer die erste — deine Buchungsarbeit soll beim Steuerberater ankommen, nicht neu gemacht werden. Und: Belegbilder-Häkchen immer an, sonst bekommt die Kanzlei Buchungen ohne Nachweise.
- **Direktübertragung zu DATEV (Buchungsdatenservice) verstanden:** Statt Datei-Versand kann sevDesk direkt ins DATEV-Rechenzentrum übertragen. Die Einrichtung ist Sache der Steuerberater-Kanzlei (Bestellung + Registrierung) — Bruno sagt dir jetzt genau, was du deine Kanzlei fragen musst.

**Schutz vor einem teuren Klick**
- Im Export-Dialog gibt es einen Regler „Dokumente festschreiben" — festgeschriebene Buchungen sind unumkehrbar. Bruno warnt jetzt ausdrücklich: beim ersten Export AUS lassen, erst festschreiben wenn dein Steuerberater das Format abgenommen hat. (Festschreiben bleibt IMMER deine Entscheidung, nie Brunos.)

## v1.38.2 — Präzisere Fehlermeldung bei Dollar-Belegen

**Verbesserungen**
- Ein Dollar-Beleg ohne lesbares Datum meldete bisher irreführend "Wechselkurs fehlt". Jetzt sagt Bruno die echte Ursache ("Beleg ohne Datum") und nennt direkt das Reparatur-Werkzeug — spart unnötige Wiederholungs-Läufe.

## v1.38.1 — Klarere Zahlen im Bericht + präzisere Update-Zusage

**Verbesserungen**
- **Eigenüberträge immer getrennt:** Berichte weisen Überweisungen zwischen deinen eigenen Konten jetzt immer gesplittet aus — Entnahmen (Geld ging raus) getrennt von Einlagen (Geld kam rein) — und mischen sie nie mit echten Ausgaben in einer Summe.
- **Wechselkurse rückwirkend:** Bruno kennt jetzt auch EZB-Kurse ab Oktober 2024 — für alte Dollar-Belege, die erst später bezahlt wurden.

**Klarstellung (Lizenz/FAQ)**
- Präzisiert: Deine Update-Zusage gilt für dein gekauftes Paket. Eigenständige Zusatzprodukte (falls es sie künftig gibt) sind getrennte Angebote. Für Early-Bird-Käufer bleibt ausdrücklich alles wie versprochen (Lifetime-Updates aufs Paket).

## v1.38.0 — Ziel: fertig GEBUCHT, nicht nur zugeordnet

**Verbesserungen**
- **Durchbuchen-Regel:** Wenn ein Beleg alle Sicherheits-Prüfungen besteht (vollständig, korrekt gelesen, Betrag/Datum/Anbieter passen mehrfach geprüft zur Kontobewegung), bucht Bruno ihn jetzt konsequent bis zum Ende durch — statt auf halber Strecke ("zugeordnet") stehen zu bleiben. Ist er unsicher, macht er bewusst GAR nichts (auch keine halbe Zuordnung) und legt den Fall in die Prüfliste. Nur echte Teilzahlungen warten sichtbar mit Begründung.
- **Klarere Berichte:** Fehlende Belege erscheinen als To-do (📥), nicht mehr wie ein Fehler (🔴) — Rot ist ab jetzt echten Fehlern und dringenden Fristen vorbehalten. Die Konfidenz-Angabe zeigt zusätzlich eine grüne Zeile: was nachweislich sauber ist.

## v1.37.1 — Prüfer erkennt eigene Ausgangsrechnungen korrekt

**Verbesserungen**
- Der Health-Check hatte eine korrekt gebuchte eigene Ausgangsrechnung (Einnahme vom Kunden) fälschlich als Richtungsfehler gemeldet. Der Prüfer kennt diese Buchungs-Klasse jetzt — echte Fehlbuchungen werden weiterhin geflaggt.

## v1.37.0 — Kein Berechtigungs-Gefrickel mehr: Bruno bringt seine Erlaubnis mit

**Neue Funktionen**
- **Mitgelieferte Claude-Code-Berechtigung:** Der Bruno-Ordner enthält jetzt eine fertige Einstellungs-Datei (`.claude/settings.json`), die Brunos Buchhaltungs-Werkzeuge automatisch erlaubt. Du musst nichts einrichten und keinen Modus verstehen — Ordner öffnen, loslegen. (Falls Claude Code doch einmal fragt: einfach „Immer erlauben" klicken, steht auch in der LIESMICH.)

## v1.36.0 — Schutzregel: Auch Einzelbuchungen laufen durch alle Prüfungen

**Verbesserungen**
- **Kein Prüfungs-Bypass mehr:** Bisher konnten manuell angestoßene Einzelbuchungen an den automatischen Schutzprüfungen vorbeilaufen. Jetzt gilt: JEDE Buchung — auch die einzelne — wird vorher gegen die Fremdbetriebs-Sperrliste, den Rechnungs-Empfänger und den Bank-Anker geprüft (gibt es überhaupt eine passende Zahlung auf deinem Konto?). Genau so wurde ein Beleg entdeckt, der an ein altes, abgemeldetes Gewerbe adressiert und privat bezahlt war — er wäre fälschlich in der Firmen-Buchhaltung gelandet.

## v1.35.1 — Portal-Rechnungen sehen jetzt aus wie im Browser

**Verbesserungen**
- Wenn Bruno eine Rechnungsseite ohne PDF-Download als PDF sichert, wird sie ab jetzt originalgetreu gedruckt (mit Logo und Layout, wie du sie im Browser siehst) — statt als nackter Text-Ausdruck. Am steuerlichen Inhalt ändert sich nichts, bestehende Belege bleiben gültig.

## v1.35.0 — Kein Beleg entgeht mehr: Link-Rechnungen + Postausgang per IMAP

**Neue Funktionen**
- **Link-Rechnungen automatisch holen** (`--fetch-links`, Opt-in): Mails wie „Rechnung ansehen" ohne Anhang — Bruno lädt den Beleg hinter dem Link (PDF direkt oder geprüfte Rechnungs-Seite, lokal gerendert). Deterministisch, mit Sicherheits-Gattern (nur echte Rechnungs-Seiten, Größen-/Zeit-Limits, interne Adressen blockiert).
- **Postausgang-Scan jetzt auch für IMAP-Postfächer** (`--imap`): findet deine selbst verschickten Rechnungen in GMX, iCloud & Co. — der Gesendet-Ordner wird automatisch erkannt, egal wie er bei deinem Anbieter heißt.
- **Mehrere Postfächer:** zweites Konto als `IMAP2_USER`/`IMAP2_PASSWORD` in der `.env`, Aufruf mit `--imap-account=2`. (Noch ohne Live-Test — Rückmeldungen willkommen.)

## v1.35.0 — Private Zahlungen räumen sich aus der offenen Liste

**Neue Funktionen**
- **Privat markieren per API:** Vom Nutzer als privat erklärte Abbuchungen (z.B. Apple-Abos) markiert Bruno jetzt direkt in sevDesk als "Privat" — sie stehen nicht mehr ewig als offen da, und es ist sofort klar: dafür wird kein Beleg gebraucht. Jederzeit rückgängig machbar, jede Änderung wird protokolliert, nur unzugeordnete Umsätze werden angefasst.
- **Eingebaute Schutzgrenzen:** Zahlungsdienstleister-Auszahlungen (Stripe/PayPal) und Geldtransit lassen sich damit bewusst NICHT markieren — die sind nicht privat, sondern gehören sauber verrechnet. Und Geld-Eingänge prüft Bruno immer erst als mögliche Einnahme.

## v1.34.0 — Modus 17: Kontoauszüge importieren (PDF oder CSV) — schließt die 90-Tage-Lücke

**Neue Funktionen**
- **Modus 17 „Kontoauszüge importieren":** Alte oder aufgelöste Konten, Zeiträume älter als die ~90 Tage der Bankverbindung — Bruno liest PDF-Kontoauszüge (oder Bank-CSV) ein und importiert sie als eigenes Konto in dein Buchhaltungssystem. Deterministisch: kein Sprachmodell liest je einen Auszug, jeder Auszug muss den Saldo-Beweis auf den Cent bestehen, jeder Import wird feldgenau zurückgelesen.
- **Onboarding fragt jetzt aktiv nach Alt-Konten** — damit kein Konto (und keine Einnahme!) unentdeckt bleibt.

**Wissensbasis**
- Bank-Adapter: Qonto + FYRST/Postbank (beide live-verifiziert). Weitere Banken baut Bruno nach dokumentiertem Muster selbst — der Saldo-Beweis bleibt Pflicht.

## v1.34.0 — Weniger unnötige Rückfragen: der Kontoumsatz entscheidet, was auf deine Liste kommt

**Verbesserungen**
- **Bank-Anker-Regel:** Wenn dein Betrieb unbar arbeitet (der Normalfall), landet ein Dokument ohne jedes Zahlungs-Signal nicht mehr auf deiner Entscheidungsliste: Fremde Dokumente ohne Konto-Bezug (z.B. ein versehentlich mitgescanntes Angebot an jemand anderen) sortiert Bruno automatisch aus. Eigene Rechnungen, deren Abbuchung noch aussteht, warten geduldig. Wichtig: Einbehaltene Gebühren (Stripe/PayPal ziehen direkt vom Auszahlungsbetrag ab) erkennt Bruno weiterhin als echte Betriebsausgabe — die haben nie einen eigenen Kontoumsatz.
## v1.33.1 — Portal-Belege werden sofort geprüft, nicht erst später

**Verbesserungen**
- **Sofort-Scan bei Portal-Beschaffung:** Jeder aus einem Portal geladene Beleg wird SOFORT gelesen und geprüft (Betrag/Datum/Anbieter müssen zur Bank-Zahlung passen, für die er geholt wurde) — solange du noch eingeloggt bist. Kaputte Downloads (abgeschnittene Seiten, Fehlerseiten) werden sofort neu geladen statt Tage später aufzufallen, wenn ein neuer Login nötig wäre.
- **Klare Format-Regel:** Echtes Anbieter-PDF zuerst; gibt es keins, druckt Bruno die Rechnungsseite sauber als PDF (mehrseitig, mit Prüfung auf Vollständigkeit). Screenshots zählen NIE als Beleg.

## v1.33.0 — Kontoauszüge von FYRST/Postbank: PDF wird zu sauberen Daten

**Neue Funktionen**
- **FYRST-Adapter** im Kontoauszug-Leser: PDF-Kontoauszüge von FYRST (Postbank/DSL) werden deterministisch in CSV/JSON umgewandelt — ohne KI, ohne Ratespiel. Jeder Auszug muss den **Saldo-Beweis** bestehen (Anfangssaldo + alle Umsätze = Endsaldo, auf den Cent), sonst gibt es keine Ausgabe. Perfekt für alte/gekündigte Konten, wo nur noch PDFs existieren.

## v1.33.0 — Fehlende Belege jetzt auch direkt aus Anbieter-Portalen holen

**Neue Funktionen**
- **Portal-Beschaffung (Modus 6b):** Sag einfach "hol die Belege von <Anbieter>" — Bruno öffnet die Rechnungsseite des Portals in deinem Browser, DU loggst dich ein (Bruno tippt nie Passwörter oder Codes), Bruno lädt dann die fehlenden Rechnungs-PDFs herunter und verarbeitet sie automatisch. Er lädt nur herunter und klickt nichts, was etwas verändert. Voraussetzung: Browser-Steuerung in der Session (VSCode mit verbundenem Chrome); ohne sie bekommst du stattdessen eine Klick-Anleitung pro Portal.
- Bruno holt dabei gezielt nur, was laut Bank-Abgleich wirklich fehlt — keine Sammel-Downloads.

## v1.32.0 — Einnahmen-Check: Bruno findet deine selbst verschickten Rechnungen

**Neue Funktionen**
- **Postausgang-Scan** (`scan-ausgang.mjs`): Bruno durchsucht deinen Gesendet-Ordner (Gmail + jedes IMAP-Postfach) nach selbst verschickten Rechnungen — und deckt damit unverbuchte Einnahmen und Lücken im Rechnungs-Nummernkreis auf. Fehlende Einnahmen sind fürs Finanzamt kritischer als fehlende Ausgaben-Belege.
- Weiterleitungen („Fwd:/WG:") werden automatisch aussortiert — das sind Eingangs-Belege, keine eigenen Rechnungen.

## v1.32.0 — Transparentes Selbst-Verbessern + Steuerwissen für EÜR und USt-Jahreserklärung

**Neue Funktionen**
- **Klare Kennzeichnung im Auto-Review:** Nach jeder Aufgabe zeigt Bruno jetzt sichtbar getrennt, was er ✅ wirklich selbst korrigiert/verbessert hat (immer mit Ein-Satz-Begründung: welcher Vorteil, und warum ohne Risiko — z.B. reversibel oder nur Prüfregel schärfer), was 💡 nur ein Vorschlag ist (du entscheidest) und was 📝 nur notiert wurde. Gab es nichts zu verbessern, steht auch das explizit da. So siehst du auf einen Blick, was sich geändert hat — nichts passiert mehr "still".

**Wissensstand**
- **Anlage EÜR 2025 komplett kartiert:** Jede Zeile und Kennziffer des amtlichen Formulars (BMF-Muster vom 29.08.2025, byte-treu archiviert) ist jetzt den SKR03- und SKR04-Konten zugeordnet. Bruno kann damit die Einnahmenüberschussrechnung aus deinen Buchungsdaten vorbereiten.
- **USt-Jahreserklärung 2025 komplett kartiert:** Alle Kennzahlen des Formulars USt 2 A (BMF-Muster vom 09.12.2024) inklusive der neuen Kleinunternehmer-Regeln ab 2025 und der Reverse-Charge-Fälle (§13b) — mit eingebauter Plausibilitäts-Prüfregel.
- **Anlagenverzeichnis-Konzept:** sevDesk bietet keine Anlagen-Schnittstelle — Bruno hat ein Konzept, wie er dein Anlagenverzeichnis (AVEÜR) selbst aus deinen Kaufbelegen führt, inklusive der gesetzlichen Grenzen für Sofortabschreibung (800 €) und Sammelposten (250–1.000 €), direkt aus dem Gesetzestext verifiziert.
- Wichtig: Bruno bereitet vor und rechnet — abgegeben wird weiterhin nur von dir bzw. deinem Steuerberater.

## v1.31.0 — „Bruno, update dich": Updates lädt Bruno jetzt selbst

**Neue Funktionen**
- **Update-Direktkanal:** Mit einem Zugriffscode von Marcel (`BRUNO_UPDATE_TOKEN` in der `.env`) lädt Bruno neue Versionen selbst herunter — sag einfach „update dich". Kein GitHub-Konto, kein Anhang-Suchen. Der Mail-Weg bleibt als Fallback voll erhalten.
- **Prüfcode automatisch:** Jedes Paket wird gegen den mitveröffentlichten SHA256-Prüfcode verifiziert, bevor irgendetwas eingespielt wird.

**Verbesserungen**
- **Größere Umbauten sicher:** Update-Pakete können jetzt Migrations-Schritte mitbringen (z.B. Ordner-Umbenennungen). Schlägt einer fehl, wird das GESAMTE Update automatisch zurückgerollt — halbe Updates gibt es nicht.
- **Neue Bausteine installieren sich selbst:** Braucht ein Tool neue Software-Pakete, läuft `npm install` automatisch mit.

**Unter der Haube**
- Neues Skript `system/_bin/update-download.mjs` (privater Release-Kanal, hart validierte Server-Felder, kein Freitext erreicht je den Chat).
- `update-apply.mjs`: Migrations-Runner + npm-Nachzug.

## v1.30.1 — Klarheit für Lexware-Office-Nutzer: DATEV-Datei-Import geht dort nicht

**Wissensstand**
- **Verifiziert (offizielle Lexware-Hilfe):** Lexware Office (lexoffice) kann keine Buchungsdaten oder DATEV-Dateien importieren — DATEV funktioniert dort nur in Richtung Steuerberater (Export). Die Idee, Buchungen extern vorzubereiten und als DATEV-Datei einzuspielen, ist damit vom Tisch.
- Für Lexware-Office-Kunden gilt: Der einzige Automatik-Weg ist die Schnittstelle (ab Tarif XL). Bruno liest, kontiert und bucht darüber automatisch (~70–80% der Arbeit); nur der Bank-Abgleich-Klick und der Steuerberater-Export bleiben in der Lexware-Oberfläche.
- Brunos DATEV-Export-Datei bleibt wertvoll — ihr Ziel ist der Steuerberater bzw. DATEV selbst, nicht Lexware Office.

## v1.30.0 — Abo-Zahlungen in Dollar werden jetzt automatisch mit dem Konto verknüpft

**Neue Funktionen**
- **Dollar-Abos treffen ihre Abbuchung:** Monats-Abos in USD (z.B. Community-Tools) buchen auf dem Konto jeden Monat einen leicht anderen Euro-Betrag ab (Kartenkurs). Bruno paart Beleg und Abbuchung jetzt über den Wechselkurs-Korridor plus strenge Eindeutigkeits-Regeln — nie durch Raten.
- Der Nachtlauf holt die offiziellen EZB-Tageskurse jetzt automatisch zum Start (ein Abruf, ganze Historie).

**Verbesserungen**
- Jede Änderung an Brunos Buchungslogik zieht ab sofort automatisch eine Prüfung der Kontroll-Instanzen nach sich (neue feste Regel) — damit der Prüfer nie hinter der Buchhaltung zurückbleibt.
- Nachtlauf-Ergebnisse werden je Quartal archiviert (bessere Nachvollziehbarkeit).

## v1.29.0 — Empfänger-Wächter, ECB-Tageskurse und ein noch wachsamerer Nachtlauf

**Neue Funktionen**
- **Empfänger-Wächter (Guard):** Bei heiklen Anbietern (z.B. deiner Steuerkanzlei, die auch andere Firmen/Privates abrechnet) erzwingt Bruno jetzt per Code einen POSITIVEN Beweis im PDF, dass die Rechnung wirklich an DEIN Unternehmen geht — sonst Prüfschleife. Kein "sieht richtig aus" mehr.
- **Fremdfirmen-Schutz (Gate #7):** Jedes PDF wird vor dem Buchen lokal gegen deine Sperrliste geprüft (zweite Firma, aufgegebenes Gewerbe, Familien-Firma im selben Postfach). Treffer = Prüfschleife statt Buchung.
- **ECB-Tageskurse:** Dollar-Rechnungen werden jetzt mit dem offiziellen EZB-Tageskurs zum Rechnungsdatum umgerechnet (gesetzlich zulässig, §16 Abs. 6 UStG — präziser als der Monatsdurchschnitt). Ein Befehl aktualisiert die Kurse direkt von der EZB.
- **Onboarding-Frage 13:** Bruno fragt beim Einrichten aktiv nach weiteren Firmen, aufgegebenen Betrieben und Umzügen — der häufigste Grund für Fehlbuchungen in gemischten Postfächern wird damit von Anfang an abgefangen.

**Verbesserungen**
- Nachtlauf stoppt sofort, wenn ein zweites Programm während des Laufs die Anbieter-Zuordnungstabelle ändert (Kollisions-Schutz), und zeigt fremde Änderungen seit dem letzten Lauf an.
- Lese-Abfragen an sevDesk laufen ~2,5× schneller (gemessen, Schreib-Vorgänge bewusst unverändert vorsichtig).
- Beleg-Reparatur liest jetzt auch fehlende BETRÄGE lokal aus dem PDF nach (0,00-€-Belege).
- Nachtlauf kann gezielt nur ein Quartal nachbuchen (`--nur-upload`).
- Adress-Historie im Profil: nach einem Umzug erkennt Bruno alte Adressen auf alten Belegen als legitim (keine Fehlalarme mehr).

## v1.28.0 — Ordner 5 heißt jetzt „SONSTIGE BELEGE (nicht diese Firma)" — bereit für Zwei-Firmen-Setups

**Neue Funktionen**
- Ordner 5 ist jetzt der Sammelplatz für ALLES, was der E-Mail-Scraper findet, aber nicht in DIESE Buchhaltung gehört: `Einkommensteuer (privat absetzbar)/` (wie bisher, jahrweise) + `Firma 2 <Name>/` als **Übergabe-Postfach für eine zweite Firma** (Prinzip: ein Scraper liest alle Postfächer, aber jede Firma hat ihren eigenen KI-Buchhalter — der Zweit-Buchhalter holt seine Belege hier ab).
- Bestehende ESt-Belege sind umgezogen, GbR-/Fremd-Belege liegen jetzt ordentlich im Firma-2-Ordner statt in einem versteckten Aussortiert-Unterordner.

## v1.27.0 — Bruno kennt deine Betriebs-Historie (neue harte Regel #12)

**Neue Funktionen**
- **Multi-Signal-Zuordnung (Regel #12):** Ob ein Beleg wirklich zu DEINEM Unternehmen gehört (und nicht zu einem früheren Betrieb, einer GbR oder privat), entscheidet Bruno nie an einem einzelnen Merkmal — er gleicht Rechnungsempfänger, Leistungstext, Datum, Betrag, den Bank-Verwendungszweck UND deine Betriebs-Historie ab. Widersprechen sich die Signale, fragt er dich.
- Betriebs-Historie als eigene private Datei: welche Betriebe wann existierten und was das für die Buchung bedeutet (reist NICHT mit dem Produkt, ist Instanz-Wissen).

## v1.26.0 — Cent-genaue Buchungs-Härtung + Bruno repariert unlesbare Beleg-Felder selbst

**Fehlerbehebung (wichtig)**
- Manche Brutto-Beträge (z.B. 9,99 € oder 1.033,14 €) kann sevDesk rechnerisch nicht exakt aus Netto×Steuersatz bilden — die Buchung wäre 1 Cent daneben gewesen. Bruno erkennt diese Beträge jetzt VOR dem Buchen (mathematische Prüfung inkl. Nachbar-Netto-Suche) und legt sie zur bewussten Hand-Buchung in die Prüfschleife, statt eine Cent-falsche Buchung zu riskieren. Zweimal live abgefangen, beide Fehl-Entwürfe rückstandslos entfernt.

**Neue Funktionen**
- Neues Werkzeug `repariere-queue-felder`: fehlende Rechnungsnummern/-daten in der Buchungs-Warteschlange werden lokal aus dem PDF nachgelesen (deterministisch, kein KI-Upload) — erster Lauf reparierte 17 Felder und machte 3 Belege buchbar.

## v1.25.2 — Beleg-Prüfung schaut jetzt auch auf den Rechnungsempfänger

**Verbesserungen**
- **Empfänger-Prüfung:** Unter einem Anbieter-Namen (z.B. deiner Steuerkanzlei) können Rechnungen an DICH, an eine andere Firma oder privat liegen. Brunos Evidenz-Werkzeug zeigt jetzt zu jedem Beleg den Rechnungsempfänger aus dem PDF — gebucht wird nur, was an dein Unternehmen adressiert ist. (Real gefangen: 24 fremde/private Steuerberater-Rechnungen wären sonst als Betriebsausgabe gelandet — alle vor Freigabe entfernt, Prüfer bestätigt 0 Fehler.)
- Benachrichtigungs-Erkennung erweitert (Newsletter/Sharing/Werbe-Mails mit Schein-Beträgen wandern automatisch in den Müll-Ordner, Einnahme-Hinweise nie).

## v1.25.1 — Ankündigungs-Mails können sich nicht mehr als Rechnung tarnen

**Verbesserungen**
- **Belegnummer-Pflicht vor dem Buchen:** „Bevorstehende Rechnungsstellung"-Ankündigungen (z.B. von Domain-Anbietern) sehen für die Texterkennung wie Rechnungen aus, tragen aber keine Rechnungsnummer. Bruno bucht jetzt NUR noch Belege MIT Nummer (echte Rechnungen haben immer eine, §14 UStG). Der Cent-genaue Rückles-Check hatte den Fall gefangen — jetzt entsteht er gar nicht mehr.
- Gemischte Steuerberater-Rechnungen (eigene Firma / andere Firma / privat) erkennt Bruno am Rechnungsempfänger im PDF und bucht NIE pauschal — Unklares fragt er dich.

## v1.25.0 — Auto-Review nach jeder Arbeit + Steuerberater-Fristen + Gesundheits-Check in Sekunden

**Neue Funktionen**
- **Auto-Review:** Nach jedem Abschluss-Report prüft Bruno sich sichtbar selbst (4 Fragen: Recherche-Lücke? Unnötige Rückfrage? Wiederkehrendes Muster? Teuerster Schritt?). Verbesserungs-Ideen schlägt Bruno vor — DU entscheidest; nur bei absoluter Sicherheit (getestet, ohne Qualitätsverlust) setzt er sie selbst um. Qualität (sicheres Buchen) steht dabei IMMER über Token- oder Zeit-Ersparnis.
- **Steuerberater in den Stammdaten (neue Onboarding-Frage):** Ob du einen Steuerberater hast, ändert deine Abgabefristen (mit StB: Jahreserklärungen bis Ende Februar des zweiten Folgejahres, §149 Abs. 3 AO). Brunos Fristen-Ampel rechnet jetzt damit — kein falscher Juli-Alarm mehr.
- **Teilzahlungs-Verknüpfung ist jetzt festes Werkzeug** (`match-splits`): funktioniert für jeden Anbieter, der Monatsrechnungen in vielen Einzelbeträgen abbucht. Der Nachtlauf prüft automatisch (nur Melde-Modus — scharf erst nach Beweis pro Anbieter).

**Verbesserungen**
- **Gesundheits-Check ~50× schneller** (90 Sekunden → unter 2 Sekunden): Buchungspositionen werden in einem Rutsch geladen statt einzeln. Vor der Übernahme geprüft: identische Prüfergebnisse (Stichprobe 15/15 deckungsgleich).

## v1.24.0 — Bankgebühren-Rechnungen (viele kleine Abbuchungen) verknüpft Bruno jetzt automatisch

**Neue Funktionen**
- **Teilzahlungs-Verknüpfung:** Manche Anbieter (z.B. deine Bank Qonto) buchen eine Monatsrechnung als VIELE kleine Einzelbeträge ab. Bruno erkennt das Muster jetzt, prüft cent-genau, dass alle Abbuchungen eines Monats zusammen exakt die Rechnung ergeben, und verknüpft dann alle auf einmal (erster Echtlauf: 12 Monatsrechnungen mit 154 Kontobewegungen, 0 Fehler).
- Der Prüfer kennt dieses Muster ebenfalls: legitime Teilzahlungs-Splits werden grün markiert statt fälschlich als Fehler gemeldet — alles andere bleibt streng.

**Verbesserungen**
- **Der Nachtlauf (ein Befehl, kompletter Durchlauf) startet jetzt überall zuverlässig** — vorher konnte er abbrechen, wenn er nicht aus einer vorbereiteten Umgebung gestartet wurde (z.B. per Zeitplan). Jetzt findet er seine Einstellungen immer selbst.
- Der Gesundheits-Check schaut jetzt auch ins Beleg-Archiv (Ordner 3): gebuchte Belege wurden fälschlich als „PDF fehlt" gemeldet (102 Fehlalarme → 0).

**Unter der Haube**
- Rechnungs-Betrags-Korrektur an Entwürfen (OCR las bei einer Qonto-Rechnung nur die Abo-Zeile statt des Gesamtbetrags — gegen das PDF korrigiert), neue sevDesk-API-Erkenntnisse dokumentiert (Teilzahlungs-Typen, Vorzeichen-Konvention), Cluster-Verknüpfungen erscheinen jetzt im Prüf-Protokoll.

## v1.23.0 — Bruno trennt gemischte Postfächer jetzt selbst (privat / Gewerbe / Steuererklärung)

**Neue Funktionen**
- Neues Werkzeug `sortiere-gemischt`: löst den Trenn-Ordner eines gemischten Postfachs anhand einer Anbieter-Liste auf — Gewerbe-Belege wandern in die Buchungs-Warteschlange (mit Dubletten-Prüfung gegen den Bestand: gleiche Datei ODER gleiche Rechnungsnummer+Betrag), Steuererklärungs-Belege nach Ordner 5, Privates/Fremdes wird aussortiert (nie gelöscht). Nur EINDEUTIGE Anbieter werden automatisch einsortiert — alles Unklare bleibt liegen und wird dir als Liste vorgelegt. Probelauf zuerst, jede Bewegung im Index protokolliert.
- Erster echter Lauf (iCloud, 472 Belege): 124 → Gewerbe · 11 → Steuererklärung · 188 aussortiert · 146 warten auf deine Einordnung (siehe AUFGABEN-FUER-DICH.md).

## v1.22.0 — Neuer Ordner 5 für private Steuer-Belege + Tool heißt jetzt email-invoice-scraper

**Neue Funktionen**
- **Ordner „5 STEUERERKLÄRUNG (privat absetzbar)":** Belege, die nicht ins Gewerbe gehören, aber für deine private Einkommensteuer zählen können (Werbungskosten aus dem Job, Kranken-/Rentenversicherung, nicht erstattete Gesundheitskosten) — jahrweise sortiert in drei Kategorien. Bruno sammelt und sortiert hier nur; die steuerliche Bewertung bleibt beim Steuerberater. Beim Trennen gemischter Postfächer wandern ESt-Kandidaten automatisch hierher, GbR-/Privat-Belege werden aussortiert (nie gelöscht).
- Die Ablage-Regeln (welcher Beleg in welchen Ordner, inkl. Index-Datei) sind jetzt fest in Brunos Arbeitsanweisung verankert — vor jedem Lauf wird das Ziel geprüft.

**Verbesserungen**
- Der Beleg-Holer heißt jetzt ehrlich **`email-invoice-scraper`** (statt gmail-invoice-scraper) — er kann längst Gmail, 11 IMAP-Anbieter, eigene Mailserver und den Drop-Ordner. Alle Verweise umgestellt, Gmail- und IMAP-Weg nach der Umbenennung live nachgetestet.

## v1.21.0 — Deine To-do-Liste als feste Datei + Schutz vor Buchungen in alten Steuerjahren

**Neue Funktionen**
- **`AUFGABEN-FUER-DICH.md`**: deine persönliche To-do-Liste als Datei im Bruno-Ordner. Bruno hält sie nach jedem Lauf aktuell und beantwortet „Was steht für mich an?" jederzeit daraus. Da stehen nur Dinge drin, die wirklich DICH brauchen.
- **Buchungsjahre-Schutz:** Im Profil steht jetzt, welche Jahre gebucht werden dürfen. Abgeschlossene alte Steuerjahre fasst Bruno nicht mehr an — ein versehentlicher Lauf über 2023/2024 wurde erkannt und vollständig zurückgerollt, der Schutz verhindert das jetzt maschinell.
- Bankgebühren-Rechnungen (z.B. Qonto) bucht Bruno jetzt automatisch korrekt als steuerfreie Nebenkosten des Geldverkehrs.
- Klare Ordner-Wegweiser: Bruno prüft bei jedem Start, ob Dokumente am richtigen Platz im 1-2-3-4-Lebensweg liegen; der Posteingang ist nach dem Einlesen immer leer.

## v1.20.0 — Gemischte Postfächer sauber getrennt + Postfach-Abruf nochmal deutlich schneller

**Neue Funktionen**
- **Gemischte Postfächer** (privat + geschäftlich, z.B. dein privates iCloud): Belege landen jetzt in einem eigenen Unterordner in Ordner 2 — „<Anbieter> (privat-geschäftlich trennen)" (Flag `--gemischt`). Wichtig, weil die Jahresordner von Ordner 2 die Buchungs-Warteschlange sind: Nichts Privates kann versehentlich gebucht werden. Nach deiner Auswahl wandern nur die Geschäfts-Belege in die Jahresordner. Beide Ordner-Anleitungen (LIESMICH) erklären das jetzt.
- Belege landen damit garantiert IMMER in der 1/2/3/4-Ordnerstruktur — nie außerhalb.

**Verbesserungen**
- **Anhänge im Sammelpaket:** Bruno holt jetzt bis zu 25 Anhänge mit EINEM Server-Kommando statt einzeln (bei 200 PDFs: ~10 statt ~200 Anfragen). Jede so geladene Datei wurde Byte für Byte gegen zwei unabhängige Referenz-Wege geprüft (12/12 identisch) — plus eingebaute Echtheits-Prüfung (PDF-Signatur) mit automatischem Rückfall auf den bewährten Einzelweg.
- iCloud nutzt jetzt 3 parallele Verbindungen (live getestet); vorsichtigere Anbieter bleiben bei 2.

## v1.19.0 — Wichtiger Datenschutz-Fix bei der Beleg-Erkennung + schnellere Beleg-Analyse

**Fehlerbehebung (wichtig — Datenschutz)**
- Ein Kommentar in der Einstellungs-Datei konnte dazu führen, dass Bruno die Beleg-Erkennung **still über einen anderen KI-Anbieter** laufen ließ als den, den du im Onboarding gewählt hast (z.B. US-Cloud statt EU-Anbieter). Gefunden VOR einem großen Lauf, an zwei Stellen behoben und verifiziert — deine Anbieter-Wahl gilt jetzt garantiert, Kommentare in der Datei sind unschädlich.

**Verbesserungen**
- Beleg-Erkennung (EU-Anbieter Mistral) läuft jetzt mit 12 statt 8 Belegen parallel — live gemessen, 0 Fehler, keine Drosselung.
- Neue Arbeitsregel: Optimierungen an Brunos Werkzeugen passieren nie mitten in einem laufenden Durchlauf — nur davor/danach, und immer erst nach bestandener Qualitäts-Prüfung (Ergebnis-Vergleich alt gegen neu).

## v1.18.0 — Sichere Dubletten räumt Bruno selbst weg + Komplett-Durchlauf in unter 2 Minuten

**Neue Funktionen**
- Erkennt Bruno eine hundertprozentig sichere Doppel-Buchung (identische Datei, beide von ihm gebucht), räumt er sie bei Autonomie-Stufe „hoch" selbst weg — es bleibt immer die Buchung mit Bank-Verknüpfung stehen, jede Löschung wird protokolliert. Alles andere Löschen fragt weiterhin.
- Der Komplett-Durchlauf (buchen, Bank abgleichen, prüfen, Ordner spiegeln, Gesundheits-Check über BEIDE Jahre) läuft jetzt in unter 2 Minuten.

**Unter der Haube**
- Neuer API-Weg gefunden und belegt: gebuchte (nicht festgeschriebene) Belege lassen sich per resetToDraft zurücksetzen und dann löschen. Schnittstellen-Takt einstellbar (Sicherheitsnetz gegen Drosselung bleibt aktiv).

## v1.17.0 — Du bestimmst, wie selbstständig Bruno arbeitet (3 Stufen)

**Neue Funktionen**
- Neue Einstellung „Autonomie-Stufe": **hoch** (Empfehlung — Umkehrbares macht Bruno allein, gefragt wird nur bei Endgültigem) · **mittel** (Freigabe vor jedem Buch-Stapel) · **niedrig** (Freigabe vor jedem Schreiben). Wird im Onboarding kurz erklärt und abgefragt — und ist jederzeit mit einem Satz änderbar („Bruno, frag mich künftig immer").
- Wichtig: Die Stufe regelt nur, WANN Bruno fragt — nie, WAS geprüft wird. Alle Sicherheits-Prüfungen (unsichere Belege in die Prüfschleife, Rücklesen jeder Buchung, unabhängiger Prüfer) laufen bei jeder Stufe. Endgültiges (Festschreiben, Versand, Löschen, Zahlungen) fragt IMMER.
- Neue Schutz-Regel Nr. 11: Vor jeder Anbieter-Einstufung prüft Bruno, was unter dem Namen wirklich liegt (echte Rechnungen oder nur Banking-Mails?) — Lehre aus einem abgefangenen Fall, bei dem eine Kunden-Einnahme wie eine Ausgabe aussah.

**Verbesserungen**
- Fehler-Anzeige präziser: sicher übersprungene Belege (z.B. Dollar-Beleg ohne Monatskurs) zählen nicht mehr als „Fehler" — der Wächter schlägt nur noch bei echten Fehlern an.
- Namen von Privatpersonen werden jetzt auch in Dateinamen-Anzeigen im Chat maskiert.

## v1.16.0 — IMAP live-getestet, 3,6× schneller + mehrere Postfächer

**Verbesserungen**
- Der neue IMAP-Weg (v1.11.0) ist jetzt **live verifiziert** — echtes iCloud-Postfach: Verbindungstest, Beleg-Erkennung über 60 Mails, kompletter Abhol-Lauf und Wiederholungs-Lauf (erkennt zuverlässig, was schon da ist — nichts wird doppelt geholt).
- **3,6× schneller** beim Postfach-Scan (33s → 9s bei 60 Mails), ohne Qualitätsverlust: Bruno lädt zuerst nur das „Inhaltsverzeichnis" jeder Mail und holt große Anhänge erst, wenn sie wirklich gebraucht werden. Jede so geladene Datei wurde Byte für Byte gegen den alten Weg geprüft — identisch.
- **Mehrere Postfächer:** Ein zweites oder drittes Mail-Konto (auch verschiedene Anbieter) trägst du als weiteren Block in dieselbe Einstellungs-Datei ein und startest mit `--imap-account=2`.

**Unter der Haube**
- 2-Phasen-Fetch (ENVELOPE+BODYSTRUCTURE, Part-Downloads sha256-verifiziert), Verbindungs-Pool (Default 2, `IMAP_POOL`, automatischer Rückfall auf 1), Text-Part-Download mit Charset-Wächter; bei jeder Server-Eigenheit greift der Voll-Download-Fallback — es geht nie ein Beleg verloren.

## v1.15.0 — Deutlich schneller: Nachtlauf, Dedup-Cache, parallele Beleg-Analyse

**Neue Funktionen**
- **Nachtlauf:** Ein Befehl, kompletter Durchlauf — buchen, Bank abgleichen, prüfen, Ordner spiegeln, Gesundheits-Check. Ohne Zwischenstopps, mit hartem Stopp bei jedem echten Fehler. Macht weiterhin NUR Umkehrbares.
- **Dubletten-Abgleich gecacht:** Der Abgleich gegen dein Buchhaltungssystem wird nur noch einmal pro Stunde voll gezogen statt bei jedem Lauf — spart mehrere Minuten. Drei Sicherungen sorgen dafür, dass der Schutz dabei nie schwächer wird (Zeitlimit, Sofort-Nachtrag jeder Buchung, automatischer Voll-Abgleich im Zweifel).
- **Anbieter-Analyse parallel:** Die Beweis-Sammlung aus Beleg-PDFs (Sitzland, Steuer-Ausweis) läuft jetzt 3-4× schneller — rein lesend, gleiche Gründlichkeit.

## v1.14.0 — Report zeigt jetzt auch Fortschritt, Steuer-Wirkung, Fristen, Abos und Kosten

**Neue Funktionen**
- Der Abschluss-Report hat 5 neue Blöcke: 📈 Fortschritt seit letztem Lauf · 💶 was der Lauf steuerlich bewegt (Vorsteuer-Vorschau, klar als Vorschau gekennzeichnet) · 📅 Fristen-Ampel für deine Steuertermine · 🔁 Abo-Radar (wiederkehrende Ausgaben, mögliche Karteileichen) · 💸 was der Lauf an KI-/Schnittstellen-Kosten verursacht hat (in Cent, gemessen).
- Neue Arbeitsregel „erst machen, dann listen": Auf der To-do-Liste am Report-Ende steht nur noch, was wirklich DICH braucht (Logins, endgültige Aktionen, Steuerberater) — alles andere erledigt Bruno vorher selbst.

## v1.13.0 — Fester Abschluss-Report nach jeder Arbeit

**Neue Funktionen**
- Nach jedem Durchlauf bekommst du automatisch einen kompakten Report direkt im Chat: Kontoumsätze klassifiziert (Einnahmen, Ausgaben, Geldtransit), welche Rechnungen WIRKLICH fehlen (gegen dein Archiv abgeglichen — getrennt von "liegt vor, nur noch nicht gebucht"), wie sicher jede Buchung ist (mit Prüf-Beleg statt Bauchgefühl), Datenschutz-Status, Zeitaufwand und eine offene To-do-Liste am Ende. Auch kleine Aufgaben enden mit einem Mini-Report.

## v1.12.0 — Bruno arbeitet autonomer: Entwürfe + Bank-Verknüpfungen ohne Rückfrage

**Neue Funktionen**
- Neues Freigabe-Modell (auf Kundenwunsch umgestellt): Alles UMKEHRBARE macht Bruno selbstständig — Belege als Entwurf/gebucht anlegen und eindeutige Bank-Verknüpfungen setzen. Eine Rückfrage kommt nur noch bei Endgültigem: Rechnungsversand, Festschreiben, Löschen, Zahlungen.
- Bruno darf die Sicherheit einer Anbieter-Einstufung selbst erhöhen — aber nur mit dokumentierten Beweisen aus den Beleg-PDFs (Sitzland, USt-ID, Steuersatz) plus Bank-Gegenprobe. Widersprüche am Beleg schlagen immer die Automatik: der Beleg bleibt dann in der Prüfschleife.
- Gleichbetrags-Abos (z. B. mehrere 24-€-Monatsrechnungen) werden jetzt sicher zugeordnet, wenn die zeitliche Paarung eindeutig ist (taggenaue Monatslogik).

**Unter der Haube**
- Cluster-Zuordnung mit strenger Eindeutigkeits-Prüfung (gleiche Anzahl + alle im Datumsfenster, sonst mutual-nearest mit ≥3 Tagen Marge). Festschreiben (enshrine) bleibt grundsätzlich beim Menschen.

## v1.11.0 — Bruno liest jetzt jedes Postfach: GMX, WEB.DE, iCloud, T-Online & Co.

**Neue Funktionen**
- Bisher konnte Bruno Rechnungen nur aus Gmail holen. Jetzt kann er **jedes normale E-Mail-Postfach** lesen: GMX, WEB.DE, T-Online, iCloud, freenet, IONOS, Strato, Vodafone/Arcor, Yahoo, mailbox.org, Posteo — und auch deinen eigenen Firmen-Mailserver. Du gibst nur deine E-Mail-Adresse und ein Passwort an, den richtigen Server erkennt Bruno automatisch.
- Für jeden Anbieter sagt dir Bruno den genauen Klickweg (z.B. „bei GMX musst du IMAP einmalig freischalten" oder „bei iCloud brauchst du ein App-Passwort — so erzeugst du es").
- Vor dem ersten Lauf prüft ein Verbindungstest, ob alles stimmt — erst dann werden Belege geholt.

**Wichtig — deine Mails bleiben unangetastet**
- Bruno liest nur. Deine Mails bleiben ungelesen markiert, nichts wird verschoben, gelöscht oder verändert. Das ist technisch fest verdrahtet, nicht nur ein Versprechen.
- Datenschutz-Plus: bei deutschen Anbietern (GMX, WEB.DE, T-Online …) verlassen die Maildaten beim Abholen Deutschland nicht. Die Belegerkennung danach läuft wie immer über die Engine, die du im Onboarding gewählt hast.

**Unter der Haube**
- Neuer IMAP-Adapter (`src/imap.mjs`, imapflow + mailparser) mit gleichem Innenleben wie der Gmail-Weg: gleiche Beleg-Erkennung, gleiche Dubletten-Prüfung, gleiche Ablage. Aufruf: `--source=imap`. Alle 11 Anbieter-Voreinstellungen gegen offizielle Anleitungen + die Thunderbird-Serverdatenbank geprüft.

## v1.10.2 — Bankabgleich: Schutz vor Doppel-Verknüpfung

**Fehlerbehebung (wichtig)**
- Beim Verknüpfen von Belegen mit Kontoumsätzen konnte ein bereits verknüpfter Umsatz in einem späteren Lauf ein zweites Mal vergeben werden — eine Zahlung hing dann an zwei Rechnungen. Der Abgleich nimmt jetzt nur noch unverknüpfte Umsätze in die Auswahl. Der eine echte Fall wurde gefunden, aufgelöst und korrekt neu verknüpft.

**Unter der Haube**
- `match-vouchers.mjs` filtert Kandidaten auf Umsatz-Status „offen" (100). Nach-Kontrolle (`verify-after-match.mjs`) bestätigt: 0 kritische Funde.

## v1.10.1 — Schutz vor Falsch-Buchungen aus „Zahlung fehlgeschlagen"-Mails

**Fehlerbehebung (wichtig)**
- Manche Anbieter schicken Mails wie „deine Zahlung ist fehlgeschlagen". Diese Mails sehen für die Automatik wie Belege aus (Betrag + bekannter Anbieter) — sie wären als Ausgaben gebucht worden, obwohl nie Geld geflossen ist. Bruno prüft jetzt vor jeder Buchung zusätzlich: Ist das wirklich eine Rechnung, Gutschrift oder Quittung mit Belegnummer? Und steht der Beleg auf „bitte prüfen"? Alles andere geht in die manuelle Prüfschleife statt ins Buchungssystem.
- Bei einem echten Testlauf hat genau diese Prüfung 13 solcher Mails abgefangen, bevor irgendetwas gebucht wurde.

**Unter der Haube**
- Buchungs-Gate in `upload.mjs` um zwei Prüfungen erweitert (`needs_review`, Dokument-Typ). Deterministisch, kein KI-Urteil — gilt für alle Kunden-Setups.

## v1.10.0 — Gebuchte Belege wandern automatisch ins Archiv

**Neue Funktionen**
- Bruno gleicht den Buchungs-Status jetzt direkt mit deinem Buchhaltungssystem (sevDesk) ab: Belege, die dort schon gebucht und mit einem Kontoumsatz verknüpft sind, verschiebt er automatisch von "2 BELEGE (noch zu buchen)" nach "3 BELEGARCHIV (gebucht, fertig)". So siehst du ohne Nachschauen: was in Ordner 2 liegt, ist offen; was in Ordner 3 liegt, ist erledigt.
- Jede Verschiebung wird in einem Protokoll festgehalten (welcher Beleg, wann, zu welcher Buchung) — volle Nachvollziehbarkeit.
- Beim ersten Lauf sind so 37 bereits gebuchte Belege sauber ins Archiv gewandert.

**Wichtig — dein System bleibt die Wahrheit**
- sevDesk ist und bleibt maßgeblich: dort wird gebucht, verknüpft und geprüft. Bruno spiegelt diesen Stand nur lokal, damit du ihn am Ordner siehst — er bucht nichts von selbst. Vor jeder Verschiebung siehst du eine Vorschau, und rückgängig machen geht jederzeit.

## v1.9.0 — Du siehst jetzt am Ordner, was schon gebucht ist

**Neue Funktionen**
- Der Beleg-Lebensweg hat jetzt vier klare Stationen, an denen du den Status auf einen Blick erkennst — ganz ohne Nachfragen:
  - **`1 POSTEINGANG (zu sortieren)`** — was du selbst reinlegst (Foto/PDF), noch nicht gelesen.
  - **`2 BELEGE (gelesen, noch zu buchen)`** — gelesen und einsortiert, wartet aufs Buchen. Das ist deine offene Arbeit.
  - **`3 BELEGARCHIV (gebucht, fertig)`** — nach dem Buchen. Was hier liegt, ist erledigt.
  - **`4 DATEV-EXPORT`** — die fertige DATEV-Datei für deinen Steuerberater.
- Vorher gab es nur "Belegarchiv" — da war nicht sichtbar, welcher Beleg schon gebucht war und welcher nicht (das wusste nur das System). Jetzt zeigt dir der Ordner den Status: liegt ein Beleg in Ordner 2, ist er noch offen; liegt er in Ordner 3, ist er gebucht.

**Für Bestandskunden**
- Deine bisherigen Belege bleiben erhalten und werden automatisch weiter gefunden. Der frühere Ordner "2 BELEGARCHIV" heißt jetzt "2 BELEGE (gelesen, noch zu buchen)" (= deine offene Arbeit); "BELEGARCHIV" steht jetzt für den fertigen, gebuchten Stand (Ordner 3). Der DATEV-Export ist Ordner 4. Beim Update wird nichts überschrieben.

## v1.8.0 — Eingehende Zahlungen zuverlässiger geschützt

**Verbesserungen**
- Beim Aufraeumen des Postfachs erkennt Bruno jetzt anhand deiner Firmendaten (aus deinem Profil), ob eine "Zahlung erhalten"-Mail eine echte Einnahme von einem Kunden ist oder nur eine Umbuchung auf dein eigenes Konto. Echte Einnahmen werden nie versehentlich aussortiert, sondern dir zur Pruefung vorgelegt. Vorher war das an einen festen Namen gebunden — jetzt zieht Bruno deinen eigenen Namen automatisch aus deinem Profil.

**Unter der Haube**
- Ist noch kein Profil angelegt, geht Bruno auf Nummer sicher: jede eingehende Zahlung wird als Grenzfall zur Pruefung vorgelegt statt automatisch verschoben. Es kann nie eine Einnahme "verloren" gehen.

## v1.7.1 — Aufgeräumter Posteingang + Bruno merkt sich den Stand besser

**Verbesserungen**
- Der Posteingang-Ordner ("zu sortieren") bleibt jetzt wirklich sauber: sobald ein Beleg verarbeitet und ins Belegarchiv einsortiert ist, verschwindet die Kopie aus dem Posteingang (vorher blieb sie in einem "erledigt"-Unterordner liegen und blaehte den Posteingang auf). Es geht nichts verloren — der Beleg liegt sortiert im Archiv, und Doppel-Verarbeitung wird weiter zuverlaessig verhindert.

**Unter der Haube**
- Bruno haelt seinen Arbeitsstand jetzt konsequenter fest: nach jedem abgeschlossenen Schritt (Belege geholt, geprueft, korrigiert) notiert er "wo stehen wir / was ist offen". So weiss er nach einem Neustart sofort, was zuletzt lief und was als Naechstes ansteht.

## v1.7.0 — Schnellerer Gmail-Scan, Postfach-Ordnung, Selbstheilende Prüfung

**Neue Funktionen**
- Bruno markiert jetzt jede erfasste Rechnung direkt in deinem Gmail-Postfach mit einem Label (z.B. "KI-Buchhalter/2026"). So siehst du auf einen Blick, was schon in der Buchhaltung ist. Reversibel, es wird nichts geloescht.
- Neues Aufraeum-Werkzeug: versehentlich erfasste Nicht-Belege (Newsletter, Zahlungs-Benachrichtigungen) verschiebt Bruno in einen separaten Ordner, statt sie in der Buchhaltung zu lassen. Nichts wird geloescht, alles umkehrbar.

**Verbesserungen**
- Der Gmail-Scan ist deutlich schneller: Rechnungen aus dem Mailtext werden direkt gelesen statt Bild-fuer-Bild erkannt, mehrere Mails parallel verarbeitet, und ein zweiter Scan ueberspringt bereits Erfasstes sofort (Sekunden statt Minuten).
- Datumsfehler behoben: Rechnungen mit Datum in der Form "25/03/2026" werden jetzt korrekt gelesen (vorher gab es Verwechslungen Tag/Monat bei US-Format).

**Unter der Haube**
- Die Buchhaltungs-Pruefung (Health-Check) heilt sich selbst: ein Fehlalarm (eine Rechnung faelschlich als "falsch gebucht" gemeldet, weil ein Zahlungsbeleg einen OCR-Fehler hatte) wurde erkannt und die Pruefregel dauerhaft verbessert — die maßgebliche Rechnung wird jetzt dem Zahlungsbeleg vorgezogen.

## v1.6.0 — Weniger Handarbeit beim Belege-Sortieren

**Neue Funktionen**
- Bruno erkennt jetzt selbst, wenn eine E-Mail gar keine Rechnung ist (Bounce-Mails, Anfragen, Abo-Ankuendigungen "verlaengert sich bald") und sortiert sie automatisch aus, statt sie dir als "zu pruefen" vorzulegen. Beim Testlauf ueber ein ganzes Jahr fielen so 81 Nicht-Rechnungen von selbst raus.

**Verbesserungen**
- Der Gmail-Scan holt jetzt auch Rechnungen, deren Betreff nicht "Rechnung/Invoice" heisst (breiterer Scan) — deutlich vollstaendiger.
- Aussortierte Nicht-Rechnungen werden in einer Liste dokumentiert (was + warum), damit du es nachvollziehen kannst.

**Unter der Haube**
- Datenschutz haerter: sensible Belegdaten laufen bei jedem Scan-/Buch-Lauf nur noch lokal, nie in die Cloud-Log.
- OCR-Datumsfehler-Erkennung verbessert.

# KI-Buchhalter (Bruno)™ — Was ist neu?

> Dieses Changelog ist für dich als Nutzer: pro Version steht hier, was sich geändert hat.
> Deine installierte Version steht in `VERSION.md`. Update einspielen: Modus 14 (`/ki-buchhalter 14`).
> Format der Versionsnummer: `X.Y.Z` — Z = Fehlerbehebung, Y = neue Funktion/neues Wissen, X = grundlegende Änderung.

---

## v1.5.0 (2026-07-10) — Neue FAQ: die häufigsten Fragen beantwortet

### Neue Funktionen
- **`FAQ.md` — deine Fragen, ehrlich beantwortet.** Eine neue Datei direkt im Bruno-Ordner mit den häufigsten Fragen aus der Praxis: Was kostet der laufende Betrieb wirklich? Läuft Bruno komplett kostenlos lokal (ehrliche Antwort: das Beleg-Lesen ja, Brunos Denken braucht dein Claude-Abo)? Welche Hardware brauche ich? Geht das ohne Gmail? Was ist mit Windows, Stripe, PayPal, mehreren Firmen, Datenschutz und Updates? Alles kurz, mit klaren Grenzen statt Marketing.

### Wissensstand
- Wie v1.4.0.

---

## v1.4.0 (2026-07-10) — Klarere Ordner, verlässlicherer Export, weniger Prüf-Fälle

### Neue Funktionen
- **Aufgeräumte Ordner-Struktur — sofort verständlich, wo was liegt.** Statt der technischen Namen INPUT/OUTPUT hast du jetzt drei nummerierte Ordner, die von selbst die Reihenfolge zeigen:
  - **`1 POSTEINGANG (zu sortieren)`** — hier legst du von Hand Belege rein (Foto/PDF, Portal-Download, Kontoauszug). Alles noch Unverarbeitete.
  - **`2 BELEGARCHIV`** — dein sortiertes Beleg-Lager (nach dem Lesen), pro Jahr und Quartal. Aus diesem Ordner wird gebucht.
  - **`3 DATEV-EXPORT`** — die fertige DATEV-Datei für deinen Steuerberater.
  - In jedem Ordner liegt eine kurze `LIESMICH.txt`, die erklärt, wozu er da ist. Rechnungen aus Gmail landen wie bisher automatisch direkt im Belegarchiv — der Posteingang ist nur für das, was du selbst reinlegst.

### Verbesserungen
- **Weniger Belege im Prüf-Ordner:** Fehlte einem gescannten Beleg nur das Rechnungsdatum, musstest du bisher nachhelfen. Bruno liest das Datum jetzt zuverlässig direkt aus dem Beleg (bzw. aus dem Mail-Eingangsdatum) — im letzten Lauf haben so 30 Belege ihr Datum automatisch bekommen, keiner blieb offen.
- **Verlässlicherer DATEV-Export:** Der Export findet deine Belege jetzt unabhängig von der Ordner-Benennung und legt die DATEV-Datei sauber in den neuen `3 DATEV-EXPORT`-Ordner.
- **Zuverlässigere Beleg-Suche in Gmail:** Beim gezielten Nachsuchen (bestimmter Zeitraum) wird der Datumsfilter jetzt immer berücksichtigt — vorher konnte er in Kombination mit einer eigenen Suchanfrage übersprungen werden.

### Unter der Haube (ändert für dich nichts, macht künftige Updates aber sicherer)
- Die Ordnernamen liegen jetzt an einer einzigen zentralen Stelle im Code. Sollte je wieder etwas umbenannt werden, passiert das an einem Ort statt an dutzenden — weniger Fehlerquellen bei Updates.
- Beim Erstellen der Verkaufs-/Update-Pakete ist sichergestellt, dass deine echten Belege niemals mitkopiert werden (nur die leere Ordner-Struktur).

### Wissensstand
- Wie v1.3.0.

---

## v1.3.0 (2026-07-10) — Sicherheits-Doppelprüfung, weniger Handarbeit, Multi-Firma, günstigerer Top-OCR-Weg

### Neue Funktionen
- **Direkt-API-Anbindung an Claude (der beste OCR-Weg wird bezahlbar):** Rechnungen können jetzt direkt über die Anthropic-API gelesen werden (`ANTHROPIC_API_KEY` in der `.env` + Weg „anthropic" wählen). Live gemessen: **unter einem Cent pro Beleg bei ~3 Sekunden Lesezeit** — 700 Belege Startaufarbeitung ≈ 6,30 $ (vorher via Abo-Weg: ~20 Sekunden/Beleg). Mit API-Konto ist der AVV (Auftragsverarbeitungsvertrag, DSGVO-Pflichtvertrag) automatisch dabei.
- **Multi-Firma:** Du hast mehrere eigene Firmen und EIN Postfach sammelt alles? Bruno kann Belege jetzt automatisch je Firma trennen — nach harten Merkmalen (USt-ID, eindeutiger Firmenname), niemals nach Bauchgefühl. Was nicht eindeutig ist, landet sichtbar im Prüf-Ordner. Anleitung: `wissen/MULTI-FIRMA.md`. (Ohne Multi-Firma-Einrichtung ändert sich für dich NICHTS.)
- **Prüf-Ordner `_review/`:** Belege, die einen kurzen Blick brauchen, erscheinen jetzt als Verknüpfungen in einem eigenen Ordner in deiner Beleg-Ablage — Original bleibt am Platz, geklärte Fälle verschwinden beim nächsten Lauf von selbst.
- **Sicherheits-Doppelprüfung (Safety-Loop):** Beim datenschutz-maximalen Weg prüft eine zweite, unabhängige Instanz die gelesenen Felder — auf anonymisierten Daten, nie am Klartext. Unstimmigkeit → Beleg geht in den Prüf-Ordner statt still durchzurutschen.
- **Weniger Handarbeit beim Lesen:** Findet die Texterkennung Pflichtfelder nicht, versucht Bruno automatisch die nächste Engine DERSELBEN Datenschutz-Klasse (nie heimlich in die US-Cloud) und nimmt das beste Ergebnis. Im echten Lauf über 259 Belege: **0 manuelle Nacharbeiten**.

### Verbesserungen
- **Einfachere Schlüssel-Eingabe:** Die `.env`-Datei liegt ab dieser Version fertig im Ordner (nichts kopieren/umbenennen) — Bruno öffnet sie dir und sagt, in welche Zeile der Schlüssel gehört. Schlüssel-Werte werden nie im Chat angezeigt.
- **Onboarding-Tabelle mit ehrlichen Live-Preisen** (gemessen, nicht geschätzt) und klarer Kennzeichnung, welche Wege mit/ohne AVV DSGVO-konform sind.
- **Lokale Modell-Empfehlungen final ausgetestet** (80-Beleg-Vergleich): die Standard-Konfiguration bleibt die beste für deine Hardware-Klasse; ein größeres Modell brächte keine bessere Praxis-Qualität, wäre aber deutlich langsamer.

### Wissensstand
- Wie v1.2.0.

---

## v1.2.0 (2026-07-10) — Rechnungsempfänger-Erkennung: "Ist dieser Beleg überhaupt an dich?"

### Neue Funktion
- **Bruno erkennt jetzt, an WEN eine Rechnung adressiert ist.** Beim Sortieren deiner Belege (aus Gmail oder dem Ablage-Ordner) liest Bruno nicht mehr nur, WER die Rechnung stellt, sondern auch, an wen sie geht — und prüft das gegen deine Firmendaten. Jeder Beleg bekommt eine Richtung:
  - **Eingang** — an dich adressiert, normale Eingangsrechnung.
  - **Fremd** — an eine andere Firma adressiert (falsch im Postfach gelandet, Rechnung eines Dritten). Bruno markiert sie zur Prüfung, **sortiert sie aber nicht weg** — du siehst sie und entscheidest.
  - **Ausgang** — sieht nach deiner eigenen Ausgangsrechnung aus (die gehört auf die Einnahmen-Seite, nicht in die Vorsteuer).
  Nutzen: Ein falsch weitergeleiteter Beleg oder eine fremde Rechnung fließt dir nicht mehr versehentlich als Vorsteuer in die Buchhaltung. In einem echten Test über 121 Belege fand Bruno genau den einen Beleg heraus, der geprüft werden musste.
- **Zusätzliche Beleg-Felder:** Bruno zieht jetzt auch die USt-ID des Rechnungsstellers und — falls vorhanden — das Zahlungsdatum ("bezahlt am"). Beides hilft später beim Abgleich mit deinem Bankkonto (wann floss das Geld tatsächlich?).
- **USt-ID-Plausibilitätsprüfung:** Deutsche USt-IDs werden auf ihre Prüfziffer geprüft. Verliest die Texterkennung mal eine Ziffer, erkennt Bruno das als möglichen Lesefehler — statt den Beleg fälschlich als "fremd" abzustempeln.
- **Neue Prüfung im Gesundheits-Check (Modus 16):** meldet dir dauerhaft alle Belege, die nicht klar an dich adressiert sind, damit keiner untergeht.

### Verbesserungen
- **OCR-Auswahl im Onboarding jetzt als klare Tabelle:** Du wählst deinen Texterkennungs-Weg mit echten Zahlen vor Augen — Geschwindigkeit, Kosten und Datenschutz nebeneinander. Alle angebotenen Optionen sind geprüft und treffen die kaufentscheidenden Felder (Rechnungsnummer, Datum, Betrag) zu 100 %. Es ist eine Prioritäten-Wahl (schnell vs. maximal datensicher), keine Qualitäts-Wahl. Der schnellste Weg (Mistral, EU-Server) schafft ~100 Belege in ~25 Sekunden; der maximal datensichere Weg läuft komplett auf deinem Rechner.

### Wissensstand
- Wie v1.1.0.

---

## v1.1.0 (2026-07-09) — Stripe-Anbindung + Selbst-Setup-Verbesserungen

### Neue Funktionen
- **Stripe als Datenquelle (Modus 3):** Bruno kann jetzt deine Stripe-Zahlungsdaten lesen — Zahlungen (brutto), Stripe-Gebühren und Auszahlungen, mit eingebauter Rechen-Kontrolle. Rein lesend: der Connector KANN technisch nichts buchen, erstatten oder ändern.
- **Neues Steuerwissen „Stripe & Zahlungsanbieter richtig verbuchen":** die wichtigste Regel — die Stripe-Auszahlung auf deinem Bankkonto ist NICHT dein Umsatz (sie ist Umsatz MINUS Gebühren, gebündelt). Bruno splittet richtig: voller Erlös + Gebühren getrennt. Inkl. Klarheit, wo deine monatliche Stripe-Gebühren-Rechnung herkommt (Dashboard → Documents) und warum deine USt-ID im Stripe-Dashboard hinterlegt sein MUSS (sonst erstellt Stripe gar keine Rechnung).
- **Onboarding fragt jetzt nach Zahlungsanbietern** (Stripe/PayPal) und richtet den Stripe-Zugang mit ein.

### Verbesserungen
- **Selbst-Setup-Prinzip verankert:** Bruno liest beim Onboarding alles aus deinem Buchhaltungssystem aus, was lesbar ist (als Vorschlag zum Bestätigen), setzt was setzbar ist — und gibt dir für den Rest den exakten Klickweg statt vager Aufgaben. Neu ausgemessen (sevDesk): welche Firmen-Stammdaten per API gehen und welche nur im Tool klickbar sind.
- Kleinunternehmer-Hinweis bei Stripe-Gebühren: die Steuer-Einordnung kostet dich als Kleinunternehmer ggf. echtes Geld — Bruno bucht hier nicht per Default, sondern schickt dich einmal zum Steuerberater.

### Wissensstand
- Wie v1.0.0 (UStAE 2026-06-02) + §4 UStG (Steuerbefreiungen) neu importiert + Stripe-Modul (verifiziert 2026-07-09).

---

## v1.0.0 (2026-07-08) — Wissensstand: UStAE-Fassung 2026-06-02

Erste versionierte Ausgabe. Ab dieser Version kann Bruno Updates empfangen, ohne dass deine Einrichtung (Profil, API-Keys, Belege, eigene Vendor-Regeln) angefasst wird.

### Neue Modi / Features
- **Modus 14 — Update:** prüft auf Wunsch, ob eine neue Version existiert, und spielt Update-Pakete sicher ein (mit Sicherungspunkt + Rollback).
- **Modus 16 — Health-Check** (bisher Modus 14): unverändert in der Funktion, jetzt als Abschluss-Prüfer am Ende des Menüs.
- Versionsanzeige im Menü.

### Code-Fixes
- (Basis-Release — Historie beginnt hier)

### Steuerwissen (wissen/)
- UStAE komplett (432 Abschnitte, konsolidierte BMF-Fassung 2026-06-02, byte-treu verifiziert)
- 10 Gesetze als amtliche Quellen (UStG, EStG, AO, HGB, KStG, GewStG u.a.)
- SKR03/SKR04 offizielle Kontenrahmen

### Bekannte Grenzen
- Lokale OCR (Ollama/qwen): als Weg dokumentiert, Adapter in Arbeit (Roadmap)
