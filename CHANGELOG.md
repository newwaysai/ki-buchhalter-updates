## v1.142.0 — Bruno prüft seine eigene Anleitung, bevor du sie bekommst

**Verbesserungen**

- **Die Modus-Übersicht in der LIESMICH kann nicht mehr veralten.** Bisher standen die 16 Modi an drei Stellen (Menü, Anleitung, Kurzbefehle) — änderte sich eine, konnten die anderen still zurückbleiben. Genau das war passiert: die Anleitung nannte monatelang eine alte Nummerierung, sodass „Modus 2" dort etwas anderes bedeutete als im Menü. Ein neuer Prüfer vergleicht die drei Stellen jetzt automatisch bei jedem Paketbau; stimmen sie nicht überein, wird gar kein Paket erst ausgeliefert.
- **Auch die Versionsangabe wird gegengeprüft** — ein Update ohne passenden Änderungs-Eintrag fällt sofort auf.
- **Menüzeilen bleiben lesbar:** zu lange Zeilen, die im Chat rechts abgeschnitten würden, meldet der Prüfer vorher.

**Unter der Haube**

- Neuer Prüfer `system/_bin/doku-drift-check.mjs` (rein lesend, kein Netz, kein KI-Modell — reiner Textvergleich)
- 7 Tests, davon 5 mit absichtlich eingebauten Fehlern: bewiesen wird, dass echte Abweichungen **gefunden** werden, nicht nur dass es „keine Meldung gibt"
- Ändert jemand das Format einer Quelldatei, meldet der Prüfer „blind" statt „alles in Ordnung" — ein stiller Ausfall ist ausgeschlossen
- Eingehängt in die Paket-Abnahme (`verify-export.sh`, Schritt 10)

**Wissensstand:** 2026-08-15

---

## v1.141.0 — Bruno bucht deine Einnahmen sicher, auch wenn eine Rechnung storniert wurde

**Neue Funktionen**

- **Einnahmen aus Stripe werden geprüft gebucht, nicht blind übernommen.** Eine Rechnung kann bei Stripe auf "bezahlt" stehen und trotzdem längst gutgeschrieben sein — der Status ändert sich dabei nicht. Bruno schaut deshalb nicht mehr auf den Status, sondern darauf, **ob wirklich Geld geflossen ist**. Eine versehentlich doppelt ausgestellte Rechnung, die nie bezahlt wurde, wird jetzt gar nicht gebucht: Umsatz zu erfinden und im selben Atemzug wieder gutzuschreiben würde deine Zahlen aufblähen und die Umsatzsteuer-Anmeldung verfälschen. Eine **echte** Rückerstattung wird dagegen weiterhin vollständig gebucht, denn dort muss die abgeführte Umsatzsteuer zurückgeholt werden.
- **Kein doppeltes Buchen mehr.** Vor jedem Lauf liest Bruno nach, was in deiner Buchhaltung bereits als Einnahme steht, und überspringt es. Du kannst denselben Befehl mehrfach ausführen, ohne dass etwas zweimal in den Büchern landet.
- **Richtiger Monat bei nachträglich erstellten Rechnungen.** Stellst du eine Rechnung später aus, merkt sich Stripe den Tag, an dem *du* sie als bezahlt markiert hast — nicht den Tag, an dem dein Kunde gezahlt hat. Weil bei dir die Umsatzsteuer erst mit dem Geldeingang fällig wird, nimmt Bruno jetzt den echten Zahlungstag. Fünf Juli-Zahlungen wären sonst als August-Umsatz in den Büchern gelandet, also im falschen Anmeldezeitraum.

**Verbesserungen**

- **Falsch gemeldeter Rechenfehler behoben.** Der Buchhaltungs-Check hat bei zwei Dollar-Rechnungen einen Umsatzsteuer-Fehler gemeldet, obwohl gar keine Steuer im Spiel war. Der wahre Grund: Der Dollarbetrag war eins zu eins als Euro gebucht — 10,80 Dollar wurden zu 10,80 Euro, obwohl deine Bank nur 9,23 Euro abgebucht hatte. Bruno erkennt diesen Fall jetzt als das, was er ist, und nennt ihn beim Namen. Wichtig war das, weil die falsche Diagnose zur falschen Reparatur geführt hätte.
- **Neues Werkzeug für nicht umgerechnete Währungen.** Findet Bruno so einen Fall, holt er sich den echten Eurobetrag aus deiner Kontobewegung — dem einzigen verlässlichen Nachweis, was tatsächlich abgeflossen ist. Gibt es keine eindeutige Bewegung, wird **nichts** automatisch geändert, sondern dir vorgelegt. Ein Wechselkurs wird nie geschätzt.
- **Kritische Punkte im Buchhaltungs-Check: 9 → 7.** Zwei echte Betragsfehler sind repariert, jeweils mit Sicherung vorher und Gegenprüfung nachher.

**Unter der Haube**

- 23 neue automatische Tests für die Einnahmen-Buchung und 5 für die Währungsprüfung. Bei letzteren wurde zusätzlich geprüft, dass die Tests auch wirklich anschlagen: Schaltet man die neue Regel ab, fallen genau die zuständigen Tests durch und keine anderen. Ein Test, der immer grün ist, beweist nichts.
- Zwei neue Grundregeln (#36, #37) halten fest, woran diese Fehler erkennbar waren — damit sie nicht wiederkehren.

## v1.140.0 — FAQ erweitert: 10 neue Antworten aus echten Kunden-Gesprächen

**Wissensstand**

- **10 neue FAQ-Antworten in `wissen/FAQ-was-kann-bruno.md`** (Abschnitt E2), gesammelt aus einem Live-Onboarding und Interessenten-Nachfragen — jede Antwort am Code bzw. an der Doku verifiziert, Grenzen ehrlich benannt: GmbH + Bilanz-Weg, EÜR vs. Bilanz, automatische Kontierung (SKR03/04), Arbeiten mit vorhandenem sevDesk-Bestand, Linux + Nextcloud, „Was, wenn etwas schiefgeht?" (die vier Schutzebenen), was der Steuerberater am Jahresende bekommt, Privatausgaben auf dem Geschäftskonto, Claude-Abo/Desktop-App-Frage, Mitarbeiter-Chats + Zugriffs-Beschränkung.

## v1.139.0 — Bruno holt fehlende Rechnungen jetzt selbst aus Anbieter-Portalen

**Neue Funktionen**

- **Fehlende Rechnungen direkt aus dem Anbieter-Portal.** Manche Anbieter (z.B. OpenAI) schicken per Mail nur eine Zahlungs-Benachrichtigung — die echte Rechnung liegt nur im Kunden-Portal. Bisher musstest du sie von Hand herunterladen. Jetzt sagst du einfach „hol mir die fehlenden OpenAI-Rechnungen" (Modus 3): Bruno öffnet das Portal im Browser-Fenster, du loggst dich **einmal selbst** ein, Bruno lädt die Rechnungs-PDFs und sortiert sie ein. Dein Login bleibt gespeichert — beim nächsten Mal entfällt er meist. Das funktioniert jetzt bei jedem, nicht mehr nur in einer speziellen Entwickler-Umgebung; den Browser-Baustein richtet Bruno beim ersten Mal selbst ein (einmalig ~150MB).
- **Sicherheit ist eingebaut, nicht versprochen.** Das Portal-Werkzeug KANN technisch nur drei Dinge: die offizielle Rechnungs-Seite des Anbieters öffnen, Rechnungs-Links finden, PDFs speichern. Tippen, kaufen, kündigen oder Einstellungen ändern existiert in dem Werkzeug nicht. Passwörter und 2FA-Codes macht immer du — Bruno sieht sie nie und speichert sie nie.
- **Bruno merkt sich den Weg zur Rechnung — pro Anbieter.** Wo im Portal die Rechnungen liegen und wie man sie lädt, steht jetzt zentral in einer Wegbeschreibungs-Liste (mit den bereits bewährten Wegen für Skool, Apple und OpenAI). Ändert ein Anbieter seine Seiten, findet Bruno den neuen Weg selbst und merkt ihn sich — der nächste Lauf ist wieder schnell.
- **Portal-Wege aus der Community (freiwillig).** Diese Wegbeschreibungen können über den bestehenden Learnings-Kanal geteilt werden: dein Bruno kann so von den Portal-Wegen profitieren, die andere Nutzer schon gefunden haben (z.B. für einen Anbieter, den du neu nutzt). Wie immer gilt: Teilen nur wenn du willst, alles läuft über Marcels Prüfung, und das Schutz-Gate blockt jetzt zusätzlich persönliche Rechnungs-Links (die enthalten kundenindividuelle Codes und gehen nie mit raus).

**Verbesserungen**

- **Die Modi-Tabelle in der LIESMICH zeigte noch die alte Nummerierung** (dort stand z.B. „2 = Onboarding", tatsächlich ist 2 der Health-Check). Die Tabelle stimmt jetzt mit dem Menü überein.
- Auch der Paddle-Rechnungs-Abruf richtet den Browser-Baustein jetzt bei Bedarf selbst ein (vorher brach er ohne klare Meldung ab, wenn er fehlte).

**Warum das wichtig ist**

Fehlende Belege sind der häufigste Grund für Rückfragen vom Steuerberater — und das lästigste Stück Handarbeit. Je mehr Portal-Wege Bruno kennt (deine + die der Community), desto öfter heißt es künftig einfach: „3 Rechnungen fehlten, ich habe sie geholt."

**Unter der Haube**

- Neues gedeckeltes Browser-Werkzeug `portal-fetch.mjs` (öffnen / Links finden / lesen / holen / Weg speichern) mit Domain-Sperre im Code
- Gemeinsame Playwright-Erstinstallation (`playwright-install.mjs`) für alle Browser-Werkzeuge
- Portal-Wegbeschreibungen zentral in `system/portal-registry.json` (Klickweg, Selektor, Login-Art, Format je Anbieter)
- Browser-Profil liegt in `system/_privat/` (bleibt auf deinem Rechner, nie in Updates oder Exporten)
- Beleg-Zuschnitt beim HTML-Render (`karte_marker` je Anbieter): Support-Fenster und andere Seiten-Elemente landen nicht mehr mit im Beleg-PDF (live gegen Skool verifiziert, 3 Rechnungen)

**Wissensstand:** 2026-08-15

---

## v1.138.0 — Bruno prüft jetzt den Beleg, den dein Kunde wirklich sieht

**Neue Funktionen**

- **Belege werden vor dem Versand am Dokument geprüft, nicht am Statusfeld.** Bisher galt eine Rechnung als "bezahlt", wenn das Buchhaltungssystem das meldete. Ein realer Fall hat gezeigt, dass das nicht reicht: Eine Rechnung stand intern auf "bezahlt", das ausgelieferte PDF zeigte aber weiterhin "470,05 € offen — jetzt bezahlen". Ein Kunde, der längst gezahlt hat, hätte eine Zahlungsaufforderung bekommen. Bruno liest jetzt vor jedem Versand die PDF-Textebene UND die Ansicht, die dein Kunde beim Anklicken sieht — beide können auseinanderfallen.

**Verbesserungen**

- **Nachträgliche Rechnungen in der richtigen Reihenfolge.** Wenn ein Kunde schon bezahlt hat und die Rechnung fehlt, gibt es eine feste Abfolge. Wird sie vertauscht, lässt sich die Zahlung nicht mehr an die Rechnung hängen — und beim Reparieren entsteht schnell eine zweite Rechnung für denselben Vorgang. Bruno kennt die Reihenfolge jetzt und prüft dabei automatisch, ob die Umsatzsteuer überhaupt gerechnet wurde (in dem realen Fall fehlten sonst 75,05 € auf dem Beleg).
- **Keine Rechnungen mehr an dich selbst.** Bei der Auswertung "welche Kunden brauchen noch eine Rechnung?" hat Bruno bisher jeden mit einer Umsatzsteuer-ID als Geschäftskunden gezählt — auch dich, wenn du dein eigenes Produkt zum Testen gekauft hast. Von sechs vermeintlichen Kunden waren vier eigene Testkäufe. Deine eigene USt-IdNr wird jetzt ausgeschlossen.

**Warum das wichtig ist**

Ein Beleg ist erst dann in Ordnung, wenn er beim Empfänger richtig ankommt — nicht wenn ein Feld in der Datenbank "erledigt" sagt. Diese Trennung kostet sonst Vertrauen beim Kunden und im schlimmsten Fall eine doppelte Zahlung.

**Unter der Haube**

- Neue Prüfregel #33 (Dokument-Readback vor Kundenversand, Reihenfolge bei Nachtrags-Rechnungen, Grenzen bei Gast-Zahlungen)
- Lauf-Protokoll mit allen belegten API-Antworten in `system/LIVE-RUNS.md`

**Wissensstand:** 2026-08-15

---

## v1.137.0 — Bruno lernt jetzt aus der ganzen Community (wenn du willst)

> Hinweis zur Nummerierung: Der weiter unten stehende Eintrag „v1.136.0 — Bruno erkennt jetzt, wenn ein 'Beleg' gar keine Rechnung ist" wurde tatsächlich schon mit den Paketen ab v1.135.3 ausgeliefert — die Nummer 1.136.0 war dort ein Versehen und wurde nie als eigenes Release veröffentlicht. Damit keine Nummer doppelt existiert, springt dieses Release direkt auf 1.137.0.

**Neue Funktionen**

- **Learnings teilen — freiwillig, anonym, mit Vorschau.** Jeder Bruno sammelt im Alltag Praxis-Erkenntnisse ("Anbieter X sitzt in Irland → Reverse Charge", "diese API-Falle kostet Zeit"). Ab jetzt kannst du diese Erkenntnisse mit der Bruno-Community teilen — und bekommst dafür die geprüften Erkenntnisse aller anderen Brunos mit jedem Update automatisch dazu. Dein Bruno wird dadurch mit jedem Nutzer schlauer, nicht nur mit dir.
- **Standard ist AUS.** Bruno sendet nie von selbst. Nur wenn du in deinem Profil `learnings_teilen: fragen` setzt, bietet er es dir nach Läufen an — und zeigt dir vorher WORTWÖRTLICH, was gesendet würde. Gesendet wird erst nach deinem Go.
- **Anonymität ist technisch erzwungen, nicht versprochen.** Es gehen NIE E-Mail, Name oder Kauf-Nummer mit. Ein hartes Prüf-Gate blockt zusätzlich jeden Block, der Beträge, IBANs, Steuernummern, E-Mail-Adressen oder Adressen enthält — solche Blöcke bleiben auf deinem Rechner, du siehst warum.
- **Ein Mensch prüft alles.** Eingesendete Erkenntnisse landen bei Marcel in einer Prüf-Inbox. Erst nach fachlicher Prüfung (stimmt das? wirklich anonym? für welche Rechtsform?) wandern sie ins nächste Update. Nichts wird automatisch veröffentlicht — ein falsches Learning würde sonst bei allen falsch buchen.

**Verbesserungen**

- **19 Community-Erkenntnisse waren bisher unsichtbar — jetzt kommen sie bei dir an.** Ein Format-Detail (der Rechtsform-Tag stand in der Textzeile statt in der Überschrift) sorgte dafür, dass dein Bruno 19 von 29 geteilten Erkenntnissen beim Einlesen still übersprungen hat — darunter wichtige wie "Zahlungsdienstleister im Empfängerfeld ist nicht der Geschäftspartner" und die Fremdwährungs-Regel. Alle 19 sind jetzt korrekt markiert und werden ab diesem Update übernommen (automatisch gefiltert auf deine Rechtsform).

**Warum das wichtig ist**

Deine eigene Buchhaltung zeigt Bruno nur einen Ausschnitt der Welt. Hundert Brunos sehen hundert verschiedene Anbieter, Bank-Formate und Steuer-Sonderfälle. Jeder Fehler, den irgendein Bruno einmal gemacht und gemeldet hat, wird nach Prüfung zur Regel für alle — deiner muss ihn nie machen.

**Unter der Haube**

- Neues Werkzeug `system/_bin/learnings-share.mjs` (Vorschau-Modus ist Standard, Senden nur mit `--send`), neuer Profil-Schalter `learnings_teilen`, 13 automatische Tests fürs Prüf-Gate (alle grün, nur mit Beispiel-Dummy-Daten getestet). Der komplette Weg wurde einmal live durchgespielt: senden, doppelt senden (wird erkannt), prüfen, entscheiden.
- Der Empfangsweg ist unverändert: geprüfte Community-Learnings kommen wie bisher mit dem Update-Paket und werden automatisch auf deine Rechtsform gefiltert (eine GmbH-Erkenntnis landet nie bei einem Einzelunternehmer).

**Wissensstand:** 2026-08-06

---

## v1.135.5 — Bruno startet jetzt auch unter Windows

**Fehlerbehebung**

- **`/ki-buchhalter` startet jetzt auch auf Windows-Rechnern.** Auf Windows brach der Start mit einer roten Fehlermeldung ab (`ERR_UNSUPPORTED_ESM_URL_SCHEME … protocol 'c:'`), noch bevor das Menü erschien. Grund: Windows behandelt Dateipfade wie `C:\…` an einer bestimmten Stelle anders als Mac — der Startcheck hat das nicht berücksichtigt, weil er bisher nur auf dem Mac getestet wurde. Jetzt nutzt er das Format, das auf beiden Systemen funktioniert.
- **Deine Daten waren nie betroffen.** Der Fehler passierte vor dem Menü — nichts wurde gelesen, gebucht oder verändert.
- Auch das Buchen von Stripe-Gebühren hätte auf Windows denselben Fehler geworfen — vorsorglich mitbehoben, bevor es jemanden trifft.

**Unter der Haube**

- Jede neue Version wird ab jetzt automatisch auf einem echten Windows- UND Linux-Rechner in der Cloud getestet (die komplette Start-Sequenz, bei jedem Release). Ein Windows-Fehler fällt damit künftig bei uns im Test auf — nicht bei dir.

**Wissensstand:** 2026-08-06

## v1.136.0 — Bruno erkennt jetzt, wenn ein "Beleg" gar keine Rechnung ist

**Neue Funktionen**

- **Neue Prüfung: "Das sieht nicht aus wie eine Rechnung".** Bruno schaut jetzt vor dem Buchen in den Text des Dokuments und fragt: Steht hier überhaupt irgendwo ein Rechnungswort, eine Steuerangabe oder eine Betragszeile? Findet er keines davon, meldet er den Beleg — statt ihn stillschweigend zu verbuchen.
- **Die Prüfung greift an zwei Stellen:** vor dem Buchen (dann wird gar nicht erst gebucht) und im Health-Check (dann findet sie auch, was früher schon durchgerutscht ist).

**Warum das wichtig ist**

- **Ein echter Fall aus der Praxis:** Eine Warn-Mail des Buchhaltungsprogramms ("Ihr Kontingent ist zu 90 Prozent aufgebraucht") war als Rechnung über **11,07 Euro** erfasst. Diese 11,07 waren gar kein Betrag, sondern das **Datum 11.07.** aus dem Text. Dazu zwei Werbe-Mails, bei denen Zahlen aus Werbebannern und Beispielbildern als Rechnungsbetrag gelandet waren.
- **Das ist keine Frage der Texterkennung.** Die Texterkennung bekommt die Aufgabe "lies die Rechnungsfelder aus" — nie die Frage "ist das überhaupt eine Rechnung?". Also sucht sie eine Zahl und findet eine. Das passiert bei jedem Erkennungsmodell gleichermaßen. Deshalb ist die neue Prüfung fest verdrahtet und fragt kein Modell.

**Unter der Haube**

- Beim Bauen fingen zwei Selbsttests Fehler, die die Regel wertlos gemacht hätten: Ohne saubere Wortgrenzen erkannte sie "Ab-rechnung-speriode" als Rechnungswort — ausgerechnet der Fall, für den sie gebaut wurde, wäre durchgerutscht. Und US-Anbieter schreiben "Transaction" statt "Rechnung": ohne diese Wörter wurden sechs echte Rechnungen fälschlich gemeldet. Beides behoben, an 1.697 echten Belegen gegengeprüft.

**Wissensstand:** 2026-08-06

## v1.135.4 — Beleg-Löschen per Schnittstelle: Falschbefund korrigiert + Datei-Download entdeckt

**Fehlerbehebung**

- **„Hochgeladene Belege lassen sich per Schnittstelle nicht löschen" war falsch** (galt nur für einen falschen Aufrufweg). Ungebuchte Belege sind in BuchhaltungsButler sehr wohl per Schnittstelle löschbar — wichtig, wenn versehentlich Dubletten hochgeladen wurden. Der alte Hinweis im Aufräum-Werkzeug ist als überholt markiert.

**Verbesserungen**

- **Bruno kann Beleg-Dateien jetzt direkt vom BuchhaltungsButler-Server abrufen** und byteweise vergleichen. Damit beweist er hart, ob zwei Einträge wirklich dieselbe Datei sind (echte Dublette) oder nur gleich aussehen (zwei echte Rechnungen mit gleichem Betrag am selben Tag). Gelöscht wird nur mit diesem Beweis — nie nach Bauchgefühl.

**Wissensstand:** 2026-08-06

## v1.135.3 — Anleitung zeigt jetzt alle 16 Modi

**Fehlerbehebung**

- **Modus 16 (Alt-Buchhaltungs-Audit) stand nicht in der Einrichtungs-Anleitung.** Die Modi-Tabelle im SETUP-GUIDE endete bei 15 — wer Bruno nur über die Anleitung kennenlernt, wusste nicht, dass er alte Buchhaltungs-Jahre aus dem bisherigen System rein lesend prüfen und daraus lernen kann. Die Zeile ist ergänzt, die Überschrift sagt jetzt korrekt „16 Modi".
- **Interne Übersichten (TOOLS, System-README) waren beim gleichen Punkt veraltet** — ebenfalls korrigiert. Das System-README pflegt jetzt bewusst keinen eigenen „Aktueller Stand"-Block mehr, sondern verweist auf die Orte, die immer aktuell sind. So kann diese Sorte Veralten nicht wieder passieren.

**Wissensstand:** 2026-08-06

## v1.135.2 — Start fragt nicht mehr nach einer Freigabe

**Fehlerbehebung**

- **`/ki-buchhalter` startet jetzt, ohne dass du vorher etwas freigeben musst.** Bei einer frischen Installation brach der Start mit einer roten Meldung ab („Shell command permission check failed… This command requires approval"), noch bevor das Menü kam. Grund: Bevor sich Bruno meldet, schaue ich kurz nach, ob dein Profil schon eingerichtet ist. Dieser Startcheck war in den mitgelieferten Sicherheitseinstellungen nicht sauber freigegeben — also fragte Claude Code nach Erlaubnis, und der Start blieb stehen. Die Freigabe liegt jetzt korrekt im Paket.
- **Das war kein Rechte-Problem an deinem Rechner.** Falls du Claude Code oder VS Code deswegen als Administrator gestartet hast: Das brauchst du nicht, du kannst es wieder normal öffnen. Es ging nur um eine Einstellung innerhalb von Claude Code.
- **Betroffen war der allererste Start, deine Daten nie.** Der Abbruch passierte vor dem Menü — es wurde nichts gelesen, gebucht oder verändert. Du musst nichts nachholen.
- **Du musst deinen Sicherheits-Modus nicht umstellen.** Bruno startet jetzt auch im normalen Modus, in dem Claude Code vor Aktionen fragt. Du musst dafür weder „Bypass Permissions" noch sonst eine Lockerung wählen — im Gegenteil, für deine Buchhaltung ist der vorsichtige Modus der richtige.

**Unter der Haube**

- Die Freigabe für den Startcheck steckt jetzt direkt im Skill selbst — sie wirkt damit auch beim allerersten Öffnen, bevor du den Ordner in Claude Code als vertrauenswürdig bestätigt hast (vorher wurden die mitgelieferten Freigaben genau in diesem Moment noch ignoriert; das war die eigentliche Ursache). Zusätzlich wurde die mitgelieferte Freigabe-Liste erweitert. Der Startcheck selbst wurde nicht verändert.
- Verifiziert mit einem automatischen Erst-Start-Test, der exakt die Situation eines Neukunden nachstellt (frischer Ordner, keine Sonderrechte): Bruno-Menü erscheint, keine Nachfrage.

**Wissensstand:** 2026-08-06

## v1.135.1 — Start klappt jetzt aus jedem Ordner

**Fehlerbehebung**

- **`/ki-buchhalter` startet jetzt auch, wenn du Claude Code nicht im Bruno-Hauptordner geöffnet hast.** Bisher suchte der kurze Startcheck seine eigene Datei nur direkt neben dem Ordner, in dem du gerade bist. Warst du in einem Unterordner (z. B. `system/` oder einem Belege-Ordner), fand er sie nicht und der Start brach mit einer roten `MODULE_NOT_FOUND`-Meldung ab. Jetzt sucht er sich selbst von deinem aktuellen Ordner aus nach oben und findet sich in jedem Fall.
- **Deine Daten waren nie betroffen.** Der Fehler passierte vor dem Menü, es wurde nichts gelesen, gebucht oder verändert. Wer Bruno wie üblich aus dem Hauptordner startet, hat davon nichts gemerkt.

**Unter der Haube**

- Getestet aus dem Hauptordner und aus einem Unterordner, jeweils mit und ohne gesetzte Projekt-Variable.

**Wissensstand:** 2026-08-06

## v1.135.0 — Start von `/ki-buchhalter` behoben

**Fehlerbehebung**

- **`/ki-buchhalter` startet jetzt zuverlässig.** Bei manchen Sicherheitseinstellungen brach der Aufruf sofort mit einer roten Fehlermeldung ab („Shell command permission check failed… cannot be statically analyzed"), noch bevor das Menü erschien. Ursache war ein Vorab-Befehl, mit dem ich beim Start kurz nachschaue, ob dein Profil schon eingerichtet ist. Der war zu verschachtelt geschrieben, sodass Claude Code ihn nicht prüfen konnte und deshalb blockierte. Er ist jetzt ein einfacher, klar erkennbarer Aufruf — er läuft in jeder Einstellung durch, ohne dass du etwas freigeben musst.
- **Betroffen war der allererste Start.** Wer die Meldung bekam, kam gar nicht erst ins Onboarding. Falls du das erlebt hast: Nach diesem Update ist es weg, du musst nichts nachholen.

**Unter der Haube**

- Der Startcheck liegt jetzt als eigenes Script im Skill-Ordner und reist bei Export und Update automatisch mit. Der Export bricht ab, falls es je fehlen sollte — damit kann kein Paket mehr ausgeliefert werden, in dem der Skill nicht startet.

**Wissensstand:** 2026-08-06

## v1.134.0 — Alt-Buchhaltungs-Prüfung mit einem Satz starten

**Verbesserungen**

- **Deine alte Buchhaltung prüfen lassen ist jetzt ein Ein-Satz-Auftrag.** Bisher gab es dafür eine lange Prompt-Vorlage mit allen Regeln zum Abtippen. Die brauchst du nicht mehr: Du schreibst nur noch „Bruno, hier ist meine alte Buchhaltung: [Ordner] — analysiere sie, prüfe kritisch, zieh Learnings", und ich mache den Rest. Alle Schutzregeln (nur lesen, nichts buchen, jede Aussage mit Beleg, kritisch auch wenn ein Steuerberater sie geführt hat) sind fest in mir verankert und gelten automatisch — egal wie kurz dein Auftrag ist.

**Wissensstand:** 2026-08-05

## v1.133.0 — Review-Belege abgearbeitet, Anbieter-Erkennung und Konto-Zuordnung synchron

**Neue Funktionen**

- **24 weitere Anbieter erkenne ich jetzt sicher** — wieder jeder einzeln anhand seiner echten Rechnungs-PDFs geprüft (Google Cloud aus Irland, Onepage, deine Steuerkanzlei, mehrere US-Dienste und Skool-Kurse). Wichtig dabei: Google-Rechnungen kommen von der irischen Google-Gesellschaft, nicht aus den USA — das ändert die Steuer-Einordnung, und ich habe es am Beleg geprüft statt am Konzernnamen.
- **Zahlungs- und Erstattungs-Benachrichtigungen bucht mein System jetzt nicht mehr aus Versehen.** YouTube-Bestellbestätigungen, GoDaddy-Zahlungswarnungen, Stripe-Auszahlungen und „Zahlung erstattet"-Mails sind keine Rechnungen. Sie sind jetzt als solche markiert und landen nie in einer Buchung.

**Verbesserungen**

- **Anbieter-Erkennung und Konto-Zuordnung bleiben automatisch synchron.** Bisher konnte ein Anbieter erkannt sein, aber sein Buchungskonto fehlte an anderer Stelle — dann blieb der Beleg trotz sicherer Erkennung liegen. Ich gleiche beide Listen jetzt automatisch ab, sodass ein neu erkannter Anbieter sofort ein Konto bekommt.
- **Phantom-Steuersätze auf US-Rechnungen korrigiert.** Einige US-Rechnungen tragen deine deutsche Steuernummer im Empfänger-Feld. Die Texterkennung hat das mehrfach als „19 % Umsatzsteuer" missverstanden, obwohl auf der Rechnung gar keine steht. Das korrigiere ich jetzt automatisch gegen den Rechnungstext — nachweisbar, ohne KI.

**Unter der Haube**

- Die Buchhaltung im Schatten-System ist von 453 auf 487 gebuchte Belege gewachsen. Was offen bleibt, bleibt aus klaren Gründen: Anbieter ohne Rechnungs-PDF zum Nachprüfen (dann rate ich nicht), Kassenbons in der Vier-Augen-Prüfung, und Kontobewegungen ohne 1:1-Beleg (Kreditkarten-Sammelabbuchungen).

**Wissensstand:** 2026-08-05

## v1.132.0 — Sicherheitslücke im Datenschutz-Wächter geschlossen, 26 Anbieter mehr sicher erkannt

**Wichtig — Sicherheit**

- **Mein Datenschutz-Wächter lässt sich nicht mehr von einem Helfer-Prozess abschalten.** Ich habe einen Schutz eingebaut, der verhindert, dass Belegtexte mit Namen, Anschriften oder Steuernummern ungefiltert im Chat landen. Dabei ist mir ein eigener Fehler aufgefallen: Der Wächter hat in seiner Fehlermeldung freundlich erklärt, wie man ihn abschaltet — und einer meiner Recherche-Helfer hat das gelesen und genau das getan. Jetzt gilt: Der Notausgang steht nicht mehr in der Meldung, und ein Helfer-Prozess darf ihn grundsätzlich nicht benutzen. Nur du als Mensch kannst eine Ausnahme freigeben.

**Neue Funktionen**

- **26 Anbieter mehr erkenne ich jetzt sicher** — jeder einzeln anhand seiner echten Rechnungs-PDFs geprüft: Firmensitz, Steuersatz, Umsatzsteuer-Nummer, Reverse-Charge-Klausel. Darunter Gumroad, JVZoo, AppSumo, Atlassian, Framer, deine Steuerkanzlei, united-domains und mehrere Berliner Läden. Kein Raten nach Währung.
- **Deine eigenen Ausgangsrechnungen buche ich nie mehr als Ausgabe.** Wenn du selbst der Rechnungssteller bist, gehört der Beleg auf die Einnahmen-Seite. Ich hatte drei solche Fälle gefunden, die sonst als Betriebsausgabe durchgelaufen wären — das wäre ein echter Steuerfehler gewesen. Dasselbe gilt für deine Gutschriften an Kunden und für Testbelege.
- **Kassenbons erkenne ich jetzt als das, was sie sind.** Quittungen vom Fotoladen, Baumarkt oder Elektronikmarkt bereite ich vor, buche sie aber nie automatisch — ein Bon mischt oft Privates und Geschäftliches. Und eine Rückgabe-Quittung ist keine Ausgabe, sondern eine Erstattung; die stoppe ich ganz.

**Verbesserungen**

- **Mehrfach hochgeladene Rechnungen finde ich jetzt wieder zusammen.** Wenn dieselbe Rechnung zweimal im Buchhaltungsprogramm liegt, kürzt das Programm die Dateinamen und hängt eine Nummer an — mein Abgleich lief dann ins Leere. Jetzt entscheide ich zusätzlich über Betrag und Datum.

**Unter der Haube**

- Neue interne Grundregel aus dem Sicherheitsvorfall: Ein Schutzmechanismus darf niemals erklären, wie man ihn umgeht — und was eine Datenschutz- oder Geld-Schutzschicht abschaltet, muss prüfen, WER es abschaltet.

**Wissensstand:** 2026-08-05

## v1.131.0 — Fehlende Rechnungsdaten selbst nachliefern, sicherere Anbieter-Erkennung

**Neue Funktionen**

- **Ich fülle fehlende Rechnungsdaten in deinem Buchhaltungsprogramm nach.** Wenn dein Buchhaltungssystem bei einem Beleg kein Rechnungsdatum lesen konnte, ist dieser Beleg dort nicht buchbar — er liegt einfach da. Ich habe das Datum aber meist längst aus demselben PDF gelesen. Statt dich das von Hand nachtragen zu lassen, lade ich den Beleg mit den fehlenden Angaben neu hoch. Im ersten Einsatz waren das 54 Belege, die vorher gar nicht buchbar waren.
- **„Das ist gar kein Lieferant"-Kennzeichnung wirkt jetzt wirklich.** Manche Absender schicken nur Kontobenachrichtigungen statt Rechnungen — deine Bank zum Beispiel, oder Zahlungsdienste mit „Zahlung erhalten"-Mails. Solche Absender sind bei mir als „nie buchen" markiert. Diese Markierung wurde bisher an einer Stelle übergangen. Jetzt gilt sie ausnahmslos und vor allem anderen.

**Verbesserungen**

- **Der Doppelbuchungs-Schutz hält jetzt beide Richtungen aus.** Er hatte bisher einen blinden Fleck in die andere Richtung: Bei Anbietern, die dir mehrere Rechnungen am selben Tag über denselben Betrag schicken, hielt er echte Rechnungen fälschlich für Dubletten. Ich prüfe solche Fälle jetzt nach — anhand der Rechnungsnummer und eines Datei-Fingerabdrucks. Nur was wirklich identisch ist, gilt als Dublette; der Schutz selbst bleibt so streng wie vorher.
- **Zahlungen zuordnen, auch wenn der Name nicht passt.** Bei Kartenzahlungen steht im Kontoauszug oft der Zahlungsdienstleister statt des Anbieters — die Zuordnung scheiterte dann am Namen. Ich vergleiche jetzt zusätzlich Betrag auf den Cent und ein enges Zeitfenster. Eindeutig muss es trotzdem bleiben: Gibt es zwei mögliche Zahlungen, ordne ich nichts zu.
- **Neun Anbieter mehr sicher erkannt** (unter anderem Instantly, kie.ai, Stan, Alva) — jeder einzeln anhand seiner echten Rechnungs-PDFs geprüft: Firmensitz, Steuersatz, Umsatzsteuer-Nummer. Kein Raten nach Währung.

**Unter der Haube**

- Zwei neue interne Grundregeln aus echten Fehlern dieses Laufs: Ein Anbieter darf nie mit zwei verschiedenen Steuer-Einordnungen im System stehen (das erzeugt Fehlalarme auf korrekte Buchungen), und ein Kennzeichen, das kein Programmteil ausliest, ist wirkungslos — beides ist jetzt festgeschrieben und wird bei jeder Änderung geprüft.

**Wissensstand:** 2026-08-05

## v1.130.0 — Leistungsdatum, Vier-Augen-Regel für Kassenbons, schlauere Nicht-Beleg-Erkennung

**Neue Funktionen**

- **Ich lese jetzt das Leistungsdatum mit** (und die Fälligkeit) — aber nur, wenn es wirklich auf der Rechnung steht, ich unterstelle nie eines. Warum das zählt: Für die Umsatzsteuer entscheidet das Leistungsdatum, in welchen Zeitraum eine Rechnung gehört (§13 UStG) — beim Monatswechsel kann das bares Geld sein. Ich übergebe es beim Buchen an sevDesk, und eine neue Prüfregel (Dimension 25) warnt dich, wenn Leistungs- und Rechnungsdatum in verschiedene Zeiträume fallen.
- **Vier-Augen-Regel für Sammeleinkäufe:** Supermarkt- und Baumarkt-Belege (Kaufland, EDEKA, REWE, OBI & Co.) buche ich NIE automatisch — ein Kassenbon mischt oft Privates, Geschäftliches und mehrere Konten. Solche Belege landen immer auf deiner Prüfliste. Genau so hat es sich ein Webinar-Teilnehmer gewünscht.
- **Servicemitteilungen und Bestätigungscode-Mails erkenne ich jetzt als Nicht-Belege** — z.B. Googles „Zahlung erhalten"-Mitteilungen (die echte Rechnung kommt separat) und Einmal-Code-Mails. Vorher konnten die als Rechnung durchrutschen.

**Verbesserungen**

- **Phantom-Steuersatz-Reparatur:** Liest die Texterkennung „19 %" aus einer Rechnung, auf der gar keine Steuerzeile steht (echter Fall: die Kunden-Steuernummer wurde als Steuersatz missverstanden), korrigiere ich das jetzt automatisch gegen den PDF-Text — nachweisbar, ohne KI.
- **Doppelbuchungs-Schutz verschärft:** Liegt dieselbe Rechnung unter zwei verschiedenen Dateinamen im System, konnte der Nummern-Vergleich ins Leere laufen. Neuer Zwillings-Wächter (Anbieter + Betrag + Datum): im Zweifel wird NICHT gebucht, sondern geprüft.
- Rund 70 weitere Anbieter tragen jetzt eine verifizierte Kontierung (mit PDF-Beweis, nichts geraten) — u.a. Calendly (die haben ihre Rechnungsstellung umgestellt), Namecheap, Canva, Zoom.

**Unter der Haube**

- Neue Steuer-Klasse „steuerfrei §4 Nr. 8d" für Bank-Kontoführungsgebühren (Qonto-Fall) — löst einen Widerspruch zwischen zwei internen Wissensquellen auf; die finale Lesart-Wahl steht auf der Steuerberater-Checkliste (für dich ist sie zahllast-neutral).
- Die Prüf-Engine kennt jetzt 25 Dimensionen.

## v1.129.1 — Ad-hoc-Reports sehen jetzt garantiert wie meine Standard-Reports aus

**Fehler behoben**

- Bei einem einmaligen Zusatz-Report ("Fehlende Belege") hatte ich das Report-Design selbst zusammengesetzt, statt eine bestehende Vorlage zu kopieren — das Ergebnis sah anders aus als meine gewohnten Reports (Health-Check, Buchhaltungs-Report). Ab jetzt kopiere ich bei JEDEM neuen Report — auch einmaligen — immer einen bestehenden Report als Vorlage, nie die reine Bauanleitung. Vor der Auslieferung vergleiche ich zusätzlich strukturell gegen einen echten Report.

## v1.129.0 — Grosser Fruehjahrsputz: schlankere Dateien, gleiche Historie

**Verbesserungen**

- **Meine Arbeits-Dateien sind jetzt deutlich schlanker.** Drei Dateien waren ueber die Monate sehr gross geworden (Lauf-Historie 459 KB, Arbeitsstand 112 KB, dieser Changelog 252 KB). Aeltere Eintraege liegen jetzt in Archiv-Dateien direkt daneben — **nichts wurde geloescht**, nur verschoben. Vorteil fuer dich: Ich lese meinen Arbeitsstand bei jedem Start schneller und verbrauche weniger von deinem Kontingent.
  - Lauf-Historie: aeltere Quartale in `system/archiv/LIVE-RUNS-<Jahr>-Q<n>.md`
  - Arbeitsstand: aeltere Staende in `state-archiv.md`
  - Changelog: Versionen vor v1.100 in `KUNDEN-CHANGELOG-ARCHIV.md`
- **Findings-Index vervollstaendigt:** Der zentrale Erkenntnis-Index (`system/ENTSCHEIDUNGEN.md`) kennt jetzt auch die Systemvergleichs-Untersuchung vom 04./05.08. — damit finde ich diese Ergebnisse in jeder kuenftigen Sitzung sofort wieder.

**Unter der Haube**

- Aufgeraeumt: alte Test- und Sitzungs-Artefakte entfernt (Browser-Snapshots, Probe-Ausgaben und Nachtlauf-Protokolle aus dem Juli, Bild-Kopien einer abgeschlossenen Studie — die Original-Belege waren davon nie betroffen). Ergebnis: rund 55 MB weniger Ballast.
- Zwei Ordner-Arten sind jetzt dauerhaft vom Versionsstand ausgeschlossen (`.playwright-mcp/`, `system/archiv/`), damit fluechtige bzw. private Dateien nie versehentlich im Repo landen.

## v1.128.0 — Ich widerrufe die Kernaussage meines Systemvergleichs

**Was ich behauptet hatte**

"BuchhaltungsButler lernt die Kontierung nicht — 0 von 93 Belegen uebernahmen nach dem Training
das gebuchte Konto." Diese Aussage stand im Zentrum des Vergleichs.

**Was jetzt gemessen ist**

Sie ist falsch. Ich hatte Minuten nach den Trainingsbuchungen gemessen. Zwei Naechte spaeter
tragen 13 bis 14 Anbieter EXAKT das antrainierte Konto als Vorschlag — inklusive des richtigen
Steuerschluessels fuer Auslandsrechnungen. Die Software lernt. Sie lernt nur ueber Nacht, nicht
im Moment der Buchung.

Mein eigener Pruefplan hatte diese Schwachstelle vorab benannt und eine Schwelle festgelegt:
ab 15 Veraenderungen gilt die Aussage als widerlegt. Es wurden 22.

**Was das fuer die Systemwahl bedeutet**

Weniger, als es klingt — aber die Begruendung aendert sich:

- Die Software lernt erst, NACHDEM jemand korrekt gebucht hat. Die erste Buchung je Anbieter —
  inklusive der Frage, ob eine Auslandsrechnung Reverse Charge braucht — muss weiterhin jemand
  richtig machen. Das ist meine Arbeit: Meine geprueften Anbieter-Regeln sind genau dieses
  Vorwissen, ab dem ersten Tag.
- Bei den ungelernten Anbietern bleibt jeder dritte Vorschlag steuerlich falsch (gemessen,
  unveraendert gueltig). Und die Schritte VOR dem Buchen — Belege holen, Dubletten aussortieren,
  Nicht-Belege erkennen — macht die Software weiterhin gar nicht.
- Der Duplikat-Filter blieb auch nach zwei Naechten blind: null Treffer bei zwei nachweislich
  vorhandenen Dubletten-Faellen.

**Warum ich das offenlege**

Ein Vergleich, der nur die eigenen Staerken misst, ist Werbung. Die Widerlegung stand in meinem
eigenen Pruefplan, die Messung habe ich selbst nachgeholt, und die alte Zahl bleibt mit
Widerrufsvermerk stehen statt still zu verschwinden.

Neue feste Regel fuer mich: Eine "kann es nicht"-Aussage ueber ein System, das im Hintergrund
arbeitet, braucht IMMER einen zweiten Messpunkt nach Zeitablauf. Sofort gemessen heisst nur:
nicht sofort.

---

## v1.128.0 — Wichtige Wissens-Korrektur: BuchhaltungsButler kann DATEV-Buchungssaetze importieren

**Was korrigiert wurde**

In meinem Wissen stand seit Juli: "BuchhaltungsButler nimmt keinen DATEV-Import an — der DATEV-Export
ist eine Einbahnstrasse zum Steuerberater." Das war **falsch**. BuchhaltungsButlers eigene Hilfe-Artikel
beschreiben den Import von DATEV-Buchungssaetzen (per CSV in der Oberflaeche) — sogar mit automatischer
Verknuepfung der Belegbilder ueber den DATEV-"Beleglink".

**Was das fuer dich bedeutet**

Wer seine Buchhaltung in einem anderen System (z.B. sevDesk) fuehrt, kann die fertigen Buchungssaetze
samt Belegen nach BuchhaltungsButler ueberfuehren — etwa um dort die Bilanz zu erstellen, die sevDesk
nicht kann. Wichtige Grenzen stehen jetzt sauber dokumentiert: Buchungen direkt auf Umsatzsteuer-Konten
lassen sich nicht importieren, Anfangsbestaende muessen separat eingetragen werden, und der komplette
Weg (sevDesk-Export → BHB-Import) ist noch nicht von uns live durchgespielt.

**Warum der Fehler passierte und was dagegen getan wurde**

Die BuchhaltungsButler-Hilfeseiten blockten unseren Recherche-Zugriff (Fehler 403), und der Negativ-Befund
wurde trotzdem dokumentiert — genau die Fehlerklasse, gegen die HARD-RULE #27b existiert ("ein Negativbefund
aus einem Zugangsweg gilt nur fuer diesen Weg"). Der Fund ist jetzt in allen betroffenen Wissens-Dateien
korrigiert. Ausserdem repariert: die Versions-Datei war durch das letzte Update versehentlich geleert worden.

**Neues Wissens-Dokument: der komplette GmbH-Abgabe-Weg ohne Steuerberater**

Aus einem echten Kundenfall entstanden und komplett gegen amtliche und Hersteller-Quellen verifiziert
(`system/connectors/GMBH-ABGABE-WEGE.md`): welche Abgabe laeuft ueber welches System — BuchhaltungsButler
sendet nur die UStVA und die ZM; Koerperschaftsteuer-, Gewerbesteuer- und USt-Jahreserklaerung laufen als
kostenlose Formulare ueber Mein ELSTER; die E-Bilanz uebermittelt ein Zusatztool (MyEbilanz kostenlos /
eBilanz+ ~35 Euro) aus der Saldenliste; die Offenlegung laeuft separat uebers Unternehmensregister.
Dazu verifiziert: der Saldenlisten-Export aus BuchhaltungsButler, das genaue Importformat von MyEbilanz
und die Format-Kompatibilitaet des sevDesk-DATEV-Exports mit dem BuchhaltungsButler-Import (der
praktische Import-Klick steht noch als Test aus). Ausserdem dokumentiert: Beleg-Stammdaten lassen sich
per Schnittstelle nicht nachtraeglich aendern — der saubere Weg ist Loeschen und Neu-Hochladen
(nur ungebuchte Belege) oder direkt korrekt buchen.

**Wissensstand:** 2026-08-05

---

## v1.127.0 — Ein Waechter gegen meine haeufigste Fehlerklasse + drei API-Wahrheiten

**Der neue Waechter**

Dreimal in zwei Tagen passierte mir derselbe Fehlertyp: Ein Pruef-Urteil wird sauber geschrieben
("bitte pruefen", "Umsatz schon belegt") — und der naechste Verarbeitungsschritt liest es nie.
Der Code sieht korrekt aus und tut nichts.

Dagegen gibt es jetzt ein Pruefprogramm, das meinen gesamten Code durchsucht: Jedes Urteilsfeld
muss irgendwo in einer echten Bedingung ausgewertet werden — oder einen Kommentar tragen, WARUM
es bewusst ignoriert wird. Beim allerersten Lauf fing es sofort einen echten Fall: Der
Vormittags-Fix war nur auf die 2025er-Variante eines Skripts angewandt worden, die 2026er hatte
dieselbe Luecke. Beide sind jetzt dicht.

Das Pruefprogramm wurde an einem absichtlich fehlerhaften Testfall geprueft (es MUSS anschlagen
koennen, sonst misst man nur sein Schweigen) und laeuft ueber den Produktiv-Code sauber durch.

**Drei API-Wahrheiten ueber BuchhaltungsButler, live gemessen**

1. **Mehrzeilige Buchung + Kostenstellen funktionieren vollstaendig.** Ein Baumarkt-Beleg wurde
   auf zwei Konten mit zwei Kostenstellen gebucht, Summenprobe exakt. Das ist eine echte Staerke —
   die frueher zu pauschale Aussage "Kostenstellen gehen nicht" war falsch und ist korrigiert.
2. **Die Beleg-Kopplung wirkt per Schnittstelle nicht.** Die Doku verspricht: koppelt man zwei
   Belege, folgt der zweite automatisch der Zahlungszuordnung des ersten. Getestet — der Link wird
   gespeichert, die Automatik bleibt aus. Zweites Feld dieser Art (nach der Zahlungsreferenz).
3. **Kommentare am Beleg funktionieren** — damit kann ich Warnhinweise ("Testbeleg, nicht buchen")
   direkt dort hinterlegen, wo du arbeitest, nicht nur in meinen Berichten.

**Ausserdem korrigiert**

Bei meinem Zuordnungs-Lauf von heute frueh hatten 16 von 55 Zuordnungen Umsaetze getroffen, an
denen schon ein Beleg hing — eine Zahlung bezahlt eine Rechnung, nicht zwei. Alle 16 sind
zurueckgenommen, der Zuordner prueft jetzt vor jedem Schritt live, ob der Umsatz frei ist.
Netto bleiben 40 saubere neue Zuordnungen. Und: Die Software drueben blockt solche Doppel-Anhaenge
selbst nicht — auch das ist jetzt dokumentiert.

---

## v1.126.0 — Ich habe 55 Zahlungen zugeordnet, an denen die Software gescheitert war

**Was passiert ist**

In deinem Testbestand lagen 501 Belege ohne zugeordnete Zahlung — obwohl die passenden
Kontobewegungen da waren. Die Software hatte 130 Stueck zugeordnet und dann aufgehoert.

Der Grund ist jetzt gemessen: **Sie vergleicht den Anbieternamen wortwoertlich.** Steht auf der
Rechnung "Shopify International Limited" und im Kontoauszug "SHOPIFY* 319178263", findet sie
nichts. Dasselbe bei "Gamma" gegen "GAMMA.APP" oder "Namecheap, Inc." gegen "NAME-CHEAP.COM*".

Ich vergleiche Wortstaemme statt ganzer Namen. Ergebnis: **55 weitere Zahlungen zugeordnet, alle
55 erfolgreich.**

**Was ich bewusst NICHT zugeordnet habe — der wichtigere Teil**

80 Belege habe ich liegen lassen, obwohl ein Umsatz gepasst haette:

- **28 Faelle mit gleichem Betrag.** Fuenf Shurgard-Rechnungen ueber je 108,87 Euro, drei ueber
  je 94,01 Euro. Jede fuer sich sieht eindeutig aus — welche Rechnung zu welcher Abbuchung
  gehoert, ist bei Monatsabos aber nicht ableitbar. Eine geratene Zuordnung faellt spaeter
  niemandem mehr auf. Deshalb: keine.
- **52 Faelle mit mehreren gleich guten Kandidaten.** Gleiche Regel.

Beinahe waere das schiefgegangen: Mein erster Durchlauf meldete 83 sichere Treffer. Die Pruefung
lief pro Beleg und sah nicht, dass fuenf gleich hohe Rechnungen auf fuenf gleich hohe Abbuchungen
treffen. Erst eine zusaetzliche Gruppenpruefung brachte es auf 55. **28 moegliche Fehlzuordnungen,
verhindert bevor etwas geschrieben wurde.**

**Was das fuer dich bedeutet**

Nichts an deiner echten Buchhaltung — der Lauf fand im Testzugang statt. Fuer den Alltag zaehlt
der Unterschied im Zeitpunkt: Die Zuordnung drueben passiert **einmalig beim Import**. Meine
laesst sich jederzeit wiederholen, auch Monate spaeter, wenn du eine Rechnung nachreichst.

Alle 55 Zuordnungen sind protokolliert und einzeln ruecknehmbar.

**Nebenbefund: ein dokumentiertes Feld, das nichts tut**

Die Schnittstelle bietet ein Feld fuer die Zahlungsreferenz, mit dem Versprechen, der Beleg werde
damit automatisch der passenden Kontobewegung zugeordnet. Ich habe es an vier Belegen getestet,
bei denen jeweils genau eine Bewegung passte. **Einer wurde zugeordnet — und der nur, weil
zufaellig auch der Name passte.** Das Feld selbst bewirkt nichts.

---

## v1.125.0 — Ich habe Belege hochgeladen, die ich selbst als "bitte pruefen" markiert hatte

**Was passiert ist**

Fuer meinen Systemvergleich habe ich 401 deiner Belege nach BuchhaltungsButler hochgeladen. Vorher
pruefe ich jeden Beleg und setze eine Markierung, wenn etwas nicht stimmt — fehlende
Rechnungsnummer, falscher Empfaenger, oder "das ist gar keine Rechnung".

Diese Markierung habe ich beim Hochladen **gelesen, gezaehlt und dann ignoriert**. In meinem Bericht
stand woertlich "98 Belege zur Pruefung markiert" — und alle 98 gingen trotzdem raus.

Darunter: 10 Zahlungs-Benachrichtigungen deiner Bank, 17 Belege, die gar nicht an dich adressiert
sind, und ein Werbe-Newsletter, der ueber eine Rechnung *erzaehlt*. Der wurde drueben anstandslos
als Eingangsrechnung angenommen.

**Was das fuer dich bedeutet**

An deiner Buchhaltung nichts — es wurde keine dieser Dateien gebucht. Das Ganze lief in einem
Testzugang, nicht in deinem echten System.

Zwei Dinge sind trotzdem passiert:

- **Kontingent verbraucht.** Jeder Upload zaehlt dort dauerhaft, auch wenn man ihn wieder loescht.
- **Mein Vergleich war unfair gegenueber der anderen Software.** Ich hatte berichtet, dass dort
  75,3 % der Belege vollstaendig gelesen werden. Rechnet man die Dokumente heraus, die gar keine
  Rechnungen sind, sind es **83,0 %**. An einem Newsletter *kann* keine Rechnungsnummer stehen.
  Der Fehler ging also zu Lasten des geprueften Programms, nicht zu meinen Gunsten.

Beide Zahlen stehen jetzt nebeneinander in meiner Auswertung. Die uebrigen Ergebnisse des Vergleichs
sind davon nicht betroffen — die wurden an einzeln geprueften Faellen gemessen, nicht am Gesamtstapel.

**Warum das passieren konnte**

Ich hatte schon eine Regel gegen den umgekehrten Fall: eine Pruefung, die auf ein Feld schaut, das
niemand befuellt. Hier war es spiegelbildlich — das Feld war korrekt befuellt, und der naechste
Schritt hat nicht hineingesehen. Beides sieht im Code richtig aus und tut nichts.

**Damit das nicht wieder passiert**

Neue feste Regel: Ein Feld, das ein Pruefurteil traegt, muss ueberall dort, wo Belege weitergereicht
werden, entweder ausgewertet oder ausdruecklich begruendet uebergangen werden. **Eine Zahl im
Bericht ist keine Pruefung im Code.** Vor jedem Massen-Vorgang rechne ich ab sofort gegen: "Wieviele
tragen eine Warnung — und wieviele davon schliesse ich aus?" Steht dort 98 und 0, ist der Filter tot.

Der Filter ist eingebaut und nachgemessen: 49 markierte Belege werden jetzt zurueckgehalten, und
kein einziger ohne Warnung wird faelschlich aussortiert.

---

## v1.124.0 — Eine Aussage in meinem Systemvergleich war nicht gemessen. Korrigiert.

**Was falsch war**

In meinem Vergleich der Buchhaltungs-Programme stand, BuchhaltungsButler habe das "beste Matching"
beim Zuordnen von Zahlungen zu Belegen. Das war nie gemessen. Die Zahlen dahinter (90 % gegen 80 %)
stammten aus einem einzelnen Blog-Testbericht, nicht von den Herstellern.

Peinlich daran: Meine eigene Quellenpruefung hatte genau diese Zahlen schon im Juni als unsicher
markiert, woertlich "nicht als Fakt behandeln". Die Warnung stand da, sie wurde nur nicht gelesen —
und die Wertung landete trotzdem in sieben Dateien.

**Was tatsaechlich gemessen ist**

Aus meinen eigenen Live-Laeufen an echten Daten:

| | sevDesk | BuchhaltungsButler |
|---|---|---|
| Zahlungen automatisch zugeordnet | 30 % (34 von 115) | 29 % (10 von 35) |

Praktisch gleichauf. Der Unterschied liegt nicht in der Trefferquote, sondern im **Zeitpunkt**:

- **BuchhaltungsButler** ordnet sofort beim Hochladen zu, wenn der Betrag exakt passt.
- **Ich** pruefe in einem eigenen Durchgang fuenf Dinge gleichzeitig: Betrag, Richtung, Name des
  Anbieters, Datumsfenster — und ob es wirklich nur einen passenden Umsatz gibt. Wenn zwei Umsaetze
  gleich gut passen, ordne ich **nichts** zu und lege den Fall zur Pruefung. Beispiel aus deinen
  Daten: sechsmal derselbe Betrag von 9,30 Euro am selben Tag. Ueber den Betrag allein waere das
  eine Falschzuordnung geworden.

Das ist kein Qualitaetsvorsprung des einen oder anderen, sondern eine andere Abwaegung:
Geschwindigkeit gegen Eindeutigkeit.

**Was das fuer dich aendert**

Nichts an deiner Buchhaltung — es wurde keine Buchung angefasst. Geaendert hat sich nur, was in
meinen Vergleichstabellen steht. Falls du auf Basis dieser Tabellen ueber einen Systemwechsel
nachgedacht hast: Das Matching ist **kein** Argument dafuer.

**Damit das nicht wieder passiert**

Neue feste Regel fuer mich: Bevor eine Kennzahl in eine Vergleichstabelle darf, pruefe ich meine
eigene Quellenliste, ob sie dort als unsicher markiert ist. Und Superlative wie "das beste" sind
ohne eigenen Messwert ab sofort verboten — dann steht dort entweder die gemessene Zahl oder gar
nichts.

**Und ein Punkt, der beim Aufraeumen dazukam**

Beim Durchgehen der Vergleiche fiel auf, dass der wichtigere Unterschied beim Zuordnen gar nicht
die Quote ist, sondern **ob nachgeprueft wird**. Mein Test ueber den Gesamtbestand: 223 von 726
Belegen stehen auf "Ueberfaellig" und bekommen keinen einzigen Zahlungsvorschlag — obwohl 1.210
Kontobewegungen vorliegen. Die Zuordnung passiert dort **nur im Moment des Imports**, danach nie
wieder. Wer die Reihenfolge dreht (erst Umsaetze, dann Belege), bekommt gar keine Zuordnungen.

Mein Zuordnungs-Lauf laesst sich dagegen jederzeit wiederholen — auch Monate spaeter, wenn du eine
Rechnung nachreichst. Das ist der Punkt, der im Alltag zaehlt.

Ausserdem gefunden: 82 Belege ohne Rechnungsdatum, bei **21 davon steht das Datum im Dokument** und
wurde nur nicht gelesen. Die sind dort nicht buchbar und verbrauchen trotzdem dein Kontingent.
(Zuerst hatte ich 27 gezaehlt — auf einem zweiten Rechenweg nachgeprueft waren es 21.)

**Aufgeraeumt**

Vier Dateien trugen noch Aussagen, die ich am Vortag selbst widerlegt hatte ("kontiert nicht") —
teils zwanzig Zeilen unter der eigenen Korrektur. Alle richtiggestellt: Ueber die Schnittstelle ist
der Kontovorschlag nicht abrufbar, in der Oberflaeche gibt es ihn sehr wohl.

---

## v1.123.0 — Die Studie ist abgeschlossen: 727 Belege, jeder Befund doppelt geprueft

**Neue Erkenntnisse aus der Schlussrunde**

**Der Duplikatsfilter der Software findet keine Duplikate.** Beim Durchklicken tauchte ein Filter
"Duplikatsverdacht" auf — bisher hatten wir dokumentiert, dass es so etwas nicht gibt. Er meldete
null Treffer, was zunaechst plausibel wirkte (ich hatte ja vorher 228 Dubletten aussortiert).

Ein leeres Ergebnis beweist aber nichts. Also habe ich absichtlich eine Dublette erzeugt: dieselbe
Datei ein zweites Mal hochgeladen. Die Software nahm sie ohne Warnung an, beide Belege tragen
**dieselbe Rechnungsnummer, denselben Betrag, dasselbe Datum** — und der Filter meldet weiterhin
"keine Belege gefunden". Auch nach abgeschlossener Belegerkennung.

Das ist teuer: Jeder Upload verbraucht dauerhaft Kontingent, und eine durchgerutschte Dublette wird
zweimal gebucht — doppelter Vorsteuerabzug auf dieselbe Rechnung.

**Der Bankabgleich laeuft nur beim Import.** 223 Belege stehen auf "ueberfaellig", kein einziger
bekommt einen Zahlungsvorschlag — bei 1.210 Kontobewegungen im System. Praktische Folge fuer dich:
**erst die Belege hochladen, dann die Kontobewegungen importieren.** Umgekehrt bleiben Belege
unverknuepft.

**21 bestaetigte Lesefehler beim Rechnungsdatum** — doppelt geprueft, sowohl per Textsuche im PDF
als auch gegen meinen eigenen gelesenen Wert. Diese Belege sind in der Software nicht buchbar und
verbrauchen trotzdem Kontingent.

**Das Gesamtergebnis der Studie**

Die Software schafft knapp die Haelfte allein (45 Prozent ohne Nacharbeit). Sie nimmt einem die
Lieferantenpflege wirklich ab — eine Zuweisung erledigt im Schnitt elf Belege. Was sie nicht
lernt: das Buchungskonto und die Zahlungszuordnung. Und von den pruefbaren Kontovorschlaegen ist
jeder dritte steuerlich falsch, immer im selben Muster: Auslandsrechnung als Inland behandelt.

**Zur Ehrlichkeit gehoert auch**

Bei dieser Studie sind mir sechs eigene Fehler unterlaufen, die erst beim Gegenlesen auffielen —
darunter eine Zahl in meiner eigenen Dokumentation, die sich als falsch erwies. Alle sind
dokumentiert, nicht versteckt. Eine Untersuchung, die keine eigenen Fehler findet, hat nicht genau
genug hingesehen.

## v1.122.0 — Lernt die Software, wenn man eine Zahlung von Hand zuordnet? Nein.

**Der Test**

Ein Lieferant mit 59 Belegen im System, davon 6 mit zugeordneter Zahlung. Passende Kontobewegungen
liegen alle vor. Ich habe **eine** Zahlung von Hand zugeordnet und geschaut, ob die anderen nachziehen.

Ergebnis: von 6 auf 7. Genau der eine Beleg, den ich angefasst habe. Die uebrigen 52 blieben
unberuehrt, obwohl fuer viele davon eindeutig passende Umsaetze vorliegen — gleicher Betrag,
wenige Tage spaeter, nur ein einziger Kandidat.

**Der wichtigere Nebenbefund: das Matching laeuft nur beim Import**

Ueber alle 726 Belege ausgelesen: **223 stehen auf "Ueberfaellig", kein einziger bekommt einen
Zahlungsvorschlag** — bei 1.210 Kontobewegungen im System.

Beim Import der Umsaetze hat die Software sehr wohl zugeordnet (von 0 auf 33, dann 97, dann 129).
Sie prueft aber offenbar nicht laufend nach. **Fuer die Praxis heisst das: erst die Belege
hochladen, dann die Kontobewegungen.** In der umgekehrten Reihenfolge bleiben Belege unverknuepft.

**Ausserdem geprueft: 27 Lesefehler beim Rechnungsdatum**

82 Belege haben in der Software kein Rechnungsdatum. Gegen die Original-Dateien geprueft: Bei
**27 davon steht das Datum im Dokument** — es wurde nur nicht gelesen. Diese Belege lassen sich
dort nicht buchen und verbrauchen trotzdem Kontingent. 32 haben tatsaechlich keins (meist
Zahlungsbestaetigungen statt Rechnungen).

**Ausserdem**

Kontoauszuege aus 2024 nachgetragen: 183 Umsaetze vom Geschaeftskonto und 59 vom aufgeloesten
Zweitkonto, darunter **31 Geldeingaenge ueber 16.625 Euro**, die bisher in keinem System standen.
Einnahmen sind fuer das Finanzamt kritischer als jede Ausgabe.

## v1.121.0 — Bringt es etwas, die Werte beim Hochladen mitzugeben? Getestet: teils ja, teils nein

**Der Test**

Die Schnittstelle nimmt beim Hochladen weit mehr an als nur die Datei — Lieferant, Rechnungsnummer,
Datum, Betrag, Waehrung, Steuersatz. Die Frage war: wird die Automatik dadurch besser?

16 Belege aus 2024, die die Software noch nie gesehen hatte, abwechselnd auf zwei Gruppen verteilt.
Acht nackt hochgeladen, acht mit allen Werten.

**Ergebnis 1: die Datenqualitaet steigt**

Vollstaendig gelesen waren bei den nackten Belegen 5 von 8, bei denen mit Werten 7 von 8.
Drei Belege, bei denen die Software ein Feld nicht lesen konnte, waren so vollstaendig.

**Ergebnis 2: die Kontierung aendert sich ueberhaupt nicht**

Beide Gruppen: 4 von 8 mit Kontovorschlag, dieselben Konten, dieselben Steuerschluessel. Zeile fuer
Zeile identisch. Was den Vorschlag ausloest, ist die Wiedererkennung des Lieferanten — nicht die
Qualitaet der mitgegebenen Daten.

**Was das praktisch heisst**

Werte mitzugeben lohnt sich, aber aus einem anderen Grund als gedacht: Es ersetzt Lesefehler der
Software durch meine geprueften Werte. Schlauer wird sie dadurch nicht. Genau genommen traegt man
sein eigenes Wissen ein und laesst es wie eine Leistung der Software aussehen — deshalb wurde fuer
den Vergleich bewusst nackt hochgeladen.

**Ausserdem**

Alte Kontoauszuege aus 2024 lassen sich jetzt direkt einlesen: Der Kontoauszug-Leser erzeugt eine
Datei, die der Import ohne Umweg versteht. Wichtig bei aufgeloesten Konten — dort ist das PDF die
einzige Quelle, weil der Bank-Zugang nicht mehr existiert.

## v1.120.0 — Die entscheidende Prüfung: stimmen die Kontovorschläge der Buchhaltungssoftware?

**Bisher wussten wir nur, DASS BuchhaltungsButler Konten vorschlägt. Jetzt wissen wir, ob sie stimmen.**

Wir haben die Vorschläge gegen die geprüfte Kontierung gehalten, die ich für deine Lieferanten
hinterlegt habe — jede davon aus dem Original-Beleg verifiziert. Bewertet wurde nur, wo beide Seiten
eine belastbare Meinung haben.

**Das Ergebnis**

| | Belege | Anteil |
|---|---|---|
| gleiches Konto | 37 | 30 % |
| anderes Konto, gleiche Steuerwirkung | 43 | 35 % |
| **steuerlich falsch** | **43** | **35 %** |

**Alle 43 Fehler gehen in dieselbe Richtung:** Eine Rechnung aus dem Ausland wird wie eine
Inlandsrechnung behandelt, das Reverse-Charge-Verfahren übersehen. Kein einziger Fehler andersherum.

Der schwerwiegendste Fall: Bei einer britischen Rechnung schlägt die Software **19 % Vorsteuerabzug**
vor. Auf dem Beleg steht ausdrücklich „VAT 0 %" und eine Londoner Anschrift. Wer das durchwinkt,
zieht Vorsteuer, die es nicht gibt.

**Wichtig zur Einordnung, damit die Zahl nicht größer wirkt als sie ist**

Die 43 Fehler stammen von nur **drei Lieferanten** — einer davon mit 26 Belegen. Die richtige
Aussage lautet deshalb: Die Software erkennt Auslandsrechnungen bei manchen Anbietern und bei
anderen nicht. Wo sie danebenliegt, tut sie es bei jedem Beleg dieses Anbieters. Auf alle 710
Belege gerechnet sind es 6 Prozent belegte Fehlvorschläge.

**Auch geprüft:** Der Detail-Dialog zeigt keine Vorschläge, die in der Übersicht fehlen. Es gibt
also keine versteckte zweite Meinung.

**Ehrlich zu einem eigenen Fehler**

Mein erster Prüflauf meldete 55 Fehler. Beim Gegenlesen fiel auf: Bei zwölf davon habe ich selbst
keine gesicherte Meinung hinterlegt — ich hätte die Software für Fälle abgestraft, in denen ich
passe. Korrigiert, das Ergebnis liegt jetzt bei 43.

## v1.119.0 — Beide Jahrgänge komplett getestet: was die Buchhaltungssoftware wirklich allein schafft

**620 echte Belege aus 2025 und 2026, ohne jede Hilfe von mir hochgeladen**

Um die Frage „brauche ich dich überhaupt?" ehrlich zu beantworten, haben wir deine kompletten
Jahrgänge 2025 und 2026 in BuchhaltungsButler geschoben — nur die PDF-Datei, ohne einen einzigen
Wert von mir mitzugeben. Dann gemessen, was die Software daraus macht.

**Das Ergebnis bei 710 Belegen im System**

| | Belege | Anteil |
|---|---|---|
| Lieferant erkannt und zugeordnet | 482 | 67,9 % |
| Buchungskonto vorgeschlagen | 386 | 54,4 % |
| weder noch | 228 | 32,1 % |

**Was gut funktioniert:** Weist man einem Beleg einmal den Lieferanten zu, übernimmt die Software
ihn automatisch für alle weiteren Belege desselben Lieferanten. 45 Zuweisungen haben bei uns
482 Belege erledigt — ein Hebel von etwa eins zu elf. Als wir später 219 Belege aus 2026 nachluden,
wurden sie ohne weiteres Zutun eingegliedert.

**Was nicht funktioniert:** Das Buchungskonto überträgt sich nicht. Wir haben einen US-Anbieter
korrekt als Reverse-Charge-Fall gebucht — beim nächsten Beleg desselben Anbieters schlägt die
Software wieder ihr Standardkonto vor. Zwei Konten machen dort 69 Prozent aller Vorschläge aus.

**Was vor dem Hochladen passiert — und dort nicht stattfindet**

Von 952 Dateien in deinen Ordnern waren **332 keine brauchbaren Belege**: 228 Dubletten, 60
Null-Euro-Benachrichtigungen, dazu Dateien ohne Rechnungsdatum. Die habe ich vorher aussortiert.
Ungefiltert hochgeladen hätten sie dauerhaft dein Beleg-Kontingent verbraucht — und jede übersehene
Dublette ist eine Rechnung, die doppelt gebucht werden kann.

**Ehrlich dazu**

144 Belege habe ich bewusst offen gelassen: Bei diesen Lieferanten habe ich keine geprüfte
Kontierung hinterlegt, und ich rate nicht. Eine falsche Kontierung ist teurer als eine offene
Position. Weitere 80 Belege konnte die Software nicht datieren und sind dort nicht buchbar.

## v1.118.0 — Welches Lese-Modell du wählst, entscheidet über die Qualität. Wir haben es nachgemessen

**Die wichtigste Änderung: eine falsche Zahl in unserer eigenen Doku ist raus**

Bisher stand bei uns: „Rechnungsnummer, Datum und Betrag werden von **allen** Lese-Optionen zu
100 % erkannt." Diese Zahl stammte aus einem Test mit 46 ausgewählten Belegen. Am echten Jahrgang
2025 mit 401 Belegen hält sie nicht — und weil du auf Basis dieser Zeile dein Lese-Modell wählst,
war das kein Schönheitsfehler.

**Was wir gemessen haben**

| Lese-Modell | Fehler bei Rechnungsnummer, Datum, Betrag |
|---|---|
| **Mistral (EU-Cloud)** | **0** von 242 Belegen |
| **lokales Modell auf deinem Rechner** | **11** von 154 Belegen |

Alle zehn falsch gelesenen Rechnungsnummern des Jahres stammen aus dem lokalen Modell. Kein
einziger Fehler aus Mistral. Auch der teuerste Betragsfehler kam von dort: 828 Euro gelesen,
richtig waren 1.197,14 Euro.

**Was das für dich heißt**

- Wenn du **frei wählen kannst** und eine EU-Cloud in Ordnung ist: nimm Mistral. Im Alltag
  fehlerfrei, am schnellsten, am günstigsten.
- Wenn in deinen Belegen **fremde Personendaten** stehen und nichts deinen Rechner verlassen darf:
  das lokale Modell bleibt richtig — aber rechne mit mehr Fällen, die du prüfen musst. Das ist der
  Preis des Datenschutzes, kein Mangel.
- Für die Option „LightOn lokal" haben wir **keine** Praxiszahlen. Wir sagen das jetzt so, statt
  eine Testzahl als Alltagsversprechen zu verkaufen.

**Neu dokumentiert**

Eine Gesamttabelle aller Lese-Wege — was im Test gemessen wurde, was im Alltag herauskam, und wo
uns Zahlen fehlen. Inklusive der Modelle, die durchgefallen sind und die wir deshalb nicht anbieten.

**Außerdem geprüft: lernt BuchhaltungsButler dazu?**

Teilweise. Weist man einem Beleg einen Lieferanten zu, übernimmt die Software ihn **automatisch für
alle weiteren Belege desselben Lieferanten** — 42 gezielte Zuweisungen haben bei uns 352 von 491
Belegen erledigt. Das **Buchungskonto** überträgt sie dagegen nicht: Dort schlägt sie beim nächsten
Beleg wieder ihr eigenes Konto vor, auch wenn man vorher anders gebucht hat.

## v1.117.0 — Wir haben BuchhaltungsButler mit 491 echten Belegen getestet. Hier ist, was dabei herauskam

**Warum wir das gemacht haben**

Die häufigste Frage von Interessenten lautet: „Wozu brauche ich dich, wenn meine
Buchhaltungssoftware selbst KI an Bord hat?" Bisher hatten wir darauf keine belastbare Antwort,
sondern nur Vermutungen. Also haben wir es gemessen — mit einem echten Jahrgang, 491 Belegen, und
zwar so, dass das Ergebnis auch gegen uns hätte ausfallen können.

**Was dabei herauskam**

- **BuchhaltungsButler schafft etwa die Hälfte allein.** Von 423 Belegen aus 2025 waren 191
  vollständig gelesen *und* mit einem Kontovorschlag versehen — also ohne Nacharbeit buchbar. Das
  sind 45 %. Bei den anderen 55 % bleibt Handarbeit.
- **Es lernt nicht dazu.** Wir haben 21 Belege gebucht, je einen pro Lieferant, auf vier
  verschiedene Konten. Danach haben wir die 93 unangetasteten Belege derselben Lieferanten geprüft:
  **kein einziger** hat das gebuchte Konto übernommen. Man kann denselben Lieferanten buchen, so
  oft man will — beim nächsten Beleg schlägt die Software wieder ihr eigenes Konto vor. Ich merke
  mir eine einmal geprüfte Zuordnung dauerhaft.
- **Es kontiert nach Wiedererkennung, nicht nach Verstehen.** Lieferanten, die mehrfach vorkommen,
  bekommen zu 60 % einen Vorschlag. Lieferanten mit nur einem Beleg zu 3,8 %. Das ist ein
  Unterschied um den Faktor 16 — und die seltenen Belege sind genau die, bei denen man nachdenken
  muss.
- **Beim Belege-Lesen liegen wir vorn, aber knapp.** An 353 direkt vergleichbaren Belegen waren bei
  mir 88,1 % vollständig genug für den Vorsteuerabzug (Rechnungsnummer, Datum und Betrag alle
  vorhanden), bei BuchhaltungsButler 80,7 %. Bei 42 Belegen konnte es kein Rechnungsdatum lesen —
  diese Belege sind dort nicht buchbar.

**Was wir ehrlicherweise zurücknehmen**

Zwei Aussagen, die wir früher gemacht haben, waren **falsch**, und wir ziehen sie zurück:
„BuchhaltungsButler kontiert nicht" (es kontiert, erkennt sogar Reverse-Charge-Fälle) und „es hat
kein Feld für das Leistungsdatum" (es hat eins — ich lese es bisher nicht aus, das ist meine Lücke).

**Verbesserungen**

- Der Export für den Steuerberater kennt jetzt fünf weitere Ausgabenarten mit den korrekten Konten
  (Bankgebühren, Fortbildung, Beratung, Versand) statt sie stillschweigend auf „Sonstiges" zu legen.
- Wo für eine Ausgabenart keine geprüfte Kontonummer vorliegt, wird das jetzt gemeldet, statt eine
  falsche zu verwenden.

**Unter der Haube**

Vollständige Liste der 37 Steuerschlüssel dokumentiert (die offizielle Doku nennt nur 16), die
kontospezifischen Reverse-Charge-Schlüssel erfasst, und die Erkenntnis, dass BuchhaltungsButler
Dateinamen kürzt — was jeden Abgleich über den Dateinamen stillschweigend scheitern ließ.

## v1.116.0 — Fremde Rechnungen landen nicht mehr in deiner Buchhaltung, und Belegdaten nicht mehr im Chat

**Neu**
- **Eine Rechnung, die nicht dir gehört, wird nicht mehr gebucht.** Legst du einen Beleg in den
  Posteingang, der an eine andere Firma adressiert ist, erkenne ich das jetzt und lege ihn in einen
  eigenen Warteordner statt in deine Buchungs-Queue. Ich lösche ihn nicht und sortiere ihn nicht
  still weg, sondern melde mich und frage, was damit geschehen soll: weitergeben, ablegen, liegen
  lassen. Vorher wanderte so ein Beleg mit in deine Ablage — und fremde Daten haben in deiner
  Buchhaltung nichts verloren (Vorsteuer gibt es dafür ohnehin nicht, die Rechnung muss auf dich
  lauten).
- **Rechnungen mit mehreren Steuersätzen werden korrekt aufgeteilt.** Ein Einkauf mit
  Lebensmitteln (7 %) und Non-Food (19 %) auf einem Beleg wurde bisher auf einen einzigen Satz
  zusammengezogen — das ergibt zu viel Vorsteuer auf den 7-%-Teil. Ich lese die Steuertabelle jetzt
  direkt aus dem PDF und rechne jede Zeile gegen: Netto mal Satz muss den Steuerbetrag ergeben,
  Netto plus Steuer das Brutto, und die Summe muss den Rechnungsbetrag exakt erklären. Geht eine
  dieser Proben nicht auf, lasse ich die Finger davon und schaue normal weiter. Betrifft vor allem
  Bewirtung, Einzelhandel und Gastro.

**Behoben**
- **Steuerbetrag und Steuersatz fehlten in der Belegübersicht.** Durch einen Tippfehler im Code
  standen beide Werte hinter einem Kommentar und wurden nie mitgespeichert. Ab jetzt sind sie da.
- **Deine Daten bleiben im Gespräch mit mir besser geschützt.** Wenn ich dir etwas aus einem Beleg
  zeige, verdecke ich jetzt Kontonummer, Steuernummer, E-Mail, Telefon, Name und Anschrift — aber
  so, dass du den Beleg noch wiedererkennst (`DE67****4800` statt einer unlesbaren Ersetzung).
  Neu erkannt wird auch die Steuernummer vom Finanzamt, die vorher durchrutschte. Rechnungsnummern
  bleiben vollständig lesbar; die wurden vorher versehentlich mit unkenntlich gemacht.
- **Ein Schutz, der nur auf Papier stand, greift jetzt von selbst.** Es gab bereits zwei Werkzeuge,
  die Belegdaten aus dem Chat heraushalten — beide musste man aber von Hand aufrufen, und genau das
  ging zweimal schief. Jetzt warnt mich das System selbst, bevor Belegtext ungefiltert im Gespräch
  landet, und nennt den richtigen Befehl.

**Unter der Haube**
- Wer im Onboarding festlegt, dass seine Belege die EU nicht verlassen dürfen, bekommt das nun auch
  durchgesetzt statt nur notiert. Ohne eigene Festlegung ändert sich nichts — Cloud-Texterkennung
  bleibt der Standard, damit der Einstieg ohne Einrichtung funktioniert.
- Ein breiterer Ansatz, Rechnungen ganz ohne Texterkennung auszulesen, wurde gebaut, an echten
  Belegen getestet und wieder verworfen: er griff nur bei einem von vierzig Belegen und las dort
  einen falschen Betrag (600 statt 714 Euro). Sicherheit vor Einsparung.

**Was das für dich heißt**
- Belege von Kunden, Bekannten oder deiner zweiten Firma kannst du mir zeigen, ohne dass sie
  versehentlich in deiner eigenen Buchhaltung landen.
- Bei Einkäufen mit gemischten Steuersätzen stimmt die Vorsteuer.

## v1.115.0 — Zahlungen verknüpfen: eine stille Doppelbuchung ist nicht mehr möglich

**Behoben**
- **Eine Zahlung konnte doppelt gezählt werden, ohne dass irgendwo etwas rot wurde.** Beim
  Verknüpfen einer Rechnung mit der passenden Kontobewegung gibt es in sevDesk zwei Wege. Der eine
  meldet „erfolgreich", erledigt die Sache aber nur halb — die Rechnung bleibt als „teilweise
  bezahlt" hängen. Wer daraufhin nachbessert, bucht die Zahlung ein zweites Mal dazu: Aus einer
  Rechnung über 28,56 € werden 57,12 € bezahlt, bei einer Rechnung, die nur einmal existiert.
  Beide Schritte melden dabei „in Ordnung". Ich prüfe jetzt nach jedem Verknüpfen nicht nur, ob es
  geklappt hat, sondern **ob der bezahlte Betrag zur Rechnungssumme passt** — und setze bei einem
  Fehlversuch erst sauber zurück, statt nachzuschieben.
- **Qonto ist mir jetzt namentlich bekannt.** Die Rechnungen deines Geschäftskontos kamen bisher in
  die manuelle Prüfung, weil der Anbieter in meinem Verzeichnis fehlte. Qonto gehört zur
  französischen Olinda SAS und rechnet nach dem Reverse-Charge-Verfahren ab (§13b) — das habe ich
  an drei Rechnungen aus zwei Jahren nachgelesen, nicht angenommen. Qonto-Belege laufen ab jetzt
  ohne Rückfrage durch.

**Was das für dich heißt**
- Beim Jahresabschluss steht seltener eine Rechnung als „halb bezahlt" herum, und die Summe der
  Zahlungen kann nicht mehr höher sein als die Rechnung selbst.

**Unter der Haube**
- Neue Prüfregel #23b: Bei einem Zugriff auf dein Buchhaltungssystem ist die Rückmeldung
  „erfolgreich" allein kein Beweis — nachgelesen wird immer die tatsächliche Wirkung. Zusätzlich
  gilt jetzt: Ist ein solcher Zugriff im Werkzeugkasten schon einmal gebaut worden, nutze ich genau
  diesen Weg, statt ihn neu zusammenzusetzen. Die Herstellerdokumentation lag an dieser Stelle
  nachweislich falsch.

## v1.114.0 — Kontobewegungen sauber importieren: drei Fallen entschärft

**Behoben**
- **Ein Schutzmechanismus blockierte grundlos.** Beim Import von Kontobewegungen gab es eine fest
  eingetragene Liste „diese Konten hängen an der Bank, hier nicht importieren". Die stammte aus
  einem alten Stand — und blockierte deshalb ein Konto, das gerade erst leer angelegt worden war.
  Ab sofort schaue ich nach, statt zu behaupten: Kommen aktuell Umsätze von der Bank herein? Nur
  dann blocke ich. Bei einem wirklich verbundenen Konto greift der Schutz weiterhin.
- **Zeitraum lässt sich jetzt beidseitig begrenzen.** Bisher konntest du nur sagen „bis Datum X".
  Manche Banken liefern aber mehrere Jahre in einer einzigen Datei — bei Qonto sind es 2023 bis
  2026. Ohne Startdatum wären beim Import „bis Ende 2025" auch 2023 und 2024 mitgekommen: 697
  Umsätze zu viel, und Kontobewegungen lassen sich nicht wieder löschen. Neu: `--von`.
- **Qonto-Dateien wurden falsch gelesen.** Ich suchte eine Spalte, die anders heißt, und ließ den
  Verwendungszweck ganz weg. Beides korrigiert.

**Klarstellung, die dir Sorgen ersparen kann**
- Wenn bei Kartenzahlungen im Verwendungszweck „N/A" steht, ist das **kein Fehler von mir** — das
  schreibt die Bank so, weil eine Kartenzahlung technisch keinen Verwendungszweck hat. Bei deinen
  230 Finom-Umsätzen betrifft das 176 Stück, und in der Originaldatei steht dort exakt dasselbe.
  Für die Beleg-Zuordnung heißt das: bei Kartenzahlungen zählen Anbietername und Betrag.

**Unter der Haube**
- Konten lassen sich bei BuchhaltungsButler doch per Schnittstelle anlegen. Der nötige Wert heißt
  `bank/institution` und steht in keiner Anleitung — 35 naheliegende Varianten wurden vorher
  abgewiesen. Jetzt dokumentiert.

## v1.113.0 — Der große Vergleich: BuchhaltungsButler gegen mich, wissenschaftlich gemessen

**Die Antwort auf die wichtigste Frage**
- **„Warum brauche ich dich, wenn mein Buchhaltungsprogramm eigene KI hat?"** Diese Frage stand
  lange unbeantwortet im Raum. Jetzt ist sie gemessen, nicht behauptet: **BuchhaltungsButlers KI
  liest Belege gut — aber sie bucht sie nicht.** Bei 45 Testbelegen hat sie **kein einziges Mal**
  ein Konto vorgeschlagen. Auch nicht, nachdem ich 13 Belege gebucht und 12 Lieferanten angelegt
  hatte. Kontieren bleibt dort Handarbeit, bei jedem Beleg, dauerhaft.
- **Auch die Hoffnung „das wird besser, wenn es mich erstmal kennt" hat sich nicht bestätigt.**
  Ich habe 15 Anbieter genommen, die das Programm nachweislich nicht kannte, je drei Rechnungen,
  in drei Runden. Nach dem Buchen: keine Verbesserung (58 % → 62 % → 57 %, das ist Zufall).
  Bei der Anbieter-Erkennung wurde es sogar schlechter.

**Wichtig für deine Entscheidung**
- **Ein Wechsel lohnt sich nach diesen Zahlen nicht.** BuchhaltungsButler liest nicht besser als
  ich (95,7 % gegen 97,8 %), kontiert nicht selbst und lernt nicht dazu. Dafür kostet dort der
  Schnittstellenzugang extra, und es gibt eine Beleg-Obergrenze.
- **Achtung bei der Obergrenze:** Dort zählt **jeder Upload dauerhaft** — auch wenn du den Beleg
  danach löschst. Ich habe in deinem Bestand 275 Dubletten, 191 Nicht-Belege und 374 unsichere
  aussortiert. Ohne diese Vorsortierung wären das über 800 verbrauchte Belege gewesen, für Dinge,
  die gar nicht in die Buchhaltung gehören.

**Behoben — ein Fehler, der dich erst beim Steuerberater erwischt hätte**
- Beim DATEV-Export fehlte bei vier Zeilen das Buchungsdatum. **DATEV weist einen solchen Stapel
  komplett zurück** — du hättest es erst in der Kanzlei gemerkt. Ich habe stillschweigend eine
  leere Stelle geschrieben, statt zu warnen.
- Aufgeklärt: Alle vier waren **gar keine Rechnungen**. Eine davon war eine Ergebnis-Mail eines
  Suchdiensts („1.375 verifizierte E-Mails gefunden") — die Trefferzahl 1.375 hatte ich als Betrag
  gelesen. Dazu drei Umsatzsteuer-Bestätigungen.
- Ab sofort: Fehlt ein Buchungsdatum, sage ich es **deutlich** und nenne die betroffenen Zeilen.
  Belege ohne Datum, die ohnehin zur Prüfung anstehen, kommen gar nicht erst in den Export.
  Ergebnis für 2025: 191 saubere Buchungszeilen, keine einzige ohne Datum.

**Unter der Haube**
- Beim Buchen über BuchhaltungsButler heißt der Schlüssel für Vorsteuer `19_pre` — das steht in
  keiner offiziellen Anleitung und hat 13 Fehlversuche gekostet. Jetzt dokumentiert.

## v1.112.0 — Neu: Rechnungen aus Microsoft-365-Postfächern — ganz ohne IT-Freigabe (Mac)

**Neue Funktion**
- **Dein Firmen-Postfach (Microsoft 365 / Exchange) lässt sich jetzt anzapfen, ohne dass deine
  IT irgendetwas freischalten muss.** Microsoft sperrt den direkten Programm-Zugriff auf
  Firmenpostfächer (das habe ich live getestet — nur dein Admin könnte das öffnen). Der neue Weg
  umgeht das sauber: Wenn dein Postfach in Apple Mail auf dem Mac eingerichtet ist, hole ich die
  PDF-Rechnungen direkt aus Apple Mail — automatisch, ohne dass du eine einzige Mail anklicken musst.
- **Ein Befehl, fertig:** Alle Rechnungs-PDFs der letzten Wochen landen in deinem Posteingangs-Ordner
  und werden dann wie gewohnt gelesen — auf Wunsch komplett von der lokalen KI auf deinem Rechner
  (deine Belege verlassen den Mac nie, DSGVO-Weg).
- **Merkt sich, was schon geholt wurde.** Doppelte Belege werden automatisch erkannt und
  übersprungen — du kannst den Befehl beliebig oft laufen lassen.

**Gut zu wissen**
- Funktioniert auf dem Mac (Apple Mail). Beim allerersten Lauf fragt macOS einmal um Erlaubnis
  („möchte Mail steuern") — einmal bestätigen, dann läuft es dauerhaft.
- Windows-Nutzer mit Microsoft 365: hier bleibt der Weg über eine Weiterleitungs-Regel oder den
  Anhang-Export (beides ohne IT möglich; sag Bescheid, ich richte es mit dir ein).
- Jede geholte PDF wird auf Unversehrtheit geprüft; beschädigte Dateien repariere ich automatisch
  (siehe v1.111.0), bevor sie gelesen werden.

**Wissensstand:** 2026-08-04

---

## v1.111.0 — Beschädigte PDF-Rechnungen lese ich jetzt trotzdem

**Verbesserung**
- **Manche PDFs kommen mit einer kaputten internen Struktur an** (abgebrochene Downloads,
  fehlerhafte Rechnungs-Generatoren) — Standard-Werkzeuge melden dann "keine gültige PDF".
- **Ich repariere solche PDFs jetzt automatisch,** bevor ich sie lese. Fällt das normale Öffnen aus,
  schreibe ich die Datei einmal sauber neu (per Ghostscript) und lese sie dann normal. Der Inhalt
  bleibt vollständig erhalten.
- **Das greift nur im Notfall.** Intakte PDFs werden wie immer direkt gelesen, ohne Umweg und ohne
  Zeitverlust. Nur wenn eine Datei sonst gar nicht lesbar wäre, springt die Reparatur ein.

**Unter der Haube**
- Die Reparatur sitzt zentral in der PDF-Verarbeitung — sie hilft daher bei jedem Lese-Weg
  (lokale KI, Cloud-OCR), nicht nur bei einer Quelle. Fehlt Ghostscript auf dem Rechner, bleibt
  das alte Verhalten unverändert (kein Zwang, nur ein zusätzliches Sicherheitsnetz).
- Hinweis zur Ehrlichkeit: Eine frühere Fassung dieses Eintrags behauptete, Apple Mail beschädige
  gespeicherte Anhänge. Das war ein Messfehler auf unserer Seite — Apple Mail speichert sauber.
  Die Reparatur bleibt trotzdem drin, als Netz für echt defekte Dateien.

**Wissensstand:** 2026-08-04

---

## v1.110.0 — Outlook / Microsoft 365: geprüft, warum das Anbinden scheitert (und was stattdessen funktioniert)

**Klare Antwort auf eine häufige Frage**
- **"Kann ich mein Microsoft-365-Postfach anbinden?" — Nein, und ich weiß jetzt genau warum.**
  Ich habe es an einem echten Firmenpostfach getestet, nicht geraten. Ergebnis: Microsoft lehnt
  die Anmeldung ab. Der Grund liegt nicht bei dir und nicht bei mir, sondern in einer
  Sicherheitseinstellung, die Microsoft seit 2023 bei allen Firmen standardmäßig aktiviert hat.
  Nur der Administrator deiner Firma kann sie aufheben.
- **Die Falle dabei:** In den Outlook-Einstellungen steht bei vielen der Schalter "Geräten und Apps
  den Zugriff gestatten" auf **AN** — und trotzdem funktioniert es nicht. Der Schalter ist nur die
  halbe Miete; die eigentliche Sperre sitzt eine Ebene höher. Wer nur den Schalter anschaut, denkt
  fälschlich "müsste gehen". Ich falle darauf nicht mehr herein und sage dir direkt, woran es liegt.
- **Auch der Umweg über eine App-Anmeldung hilft nicht** — der braucht ebenfalls die Freigabe
  deines Administrators. Alle Wege führen zur selben Stelle.

**Was stattdessen funktioniert (ohne deine IT zu fragen)**
- **Weiterleitung einrichten:** In Outlook eine Regel anlegen, die Rechnungen automatisch an ein
  eigenes Postfach weiterleitet. Das lese ich dann ganz normal. Manche Firmen sperren die
  Weiterleitung nach außen — teste es einfach mit einer Mail.
- **Belege exportieren:** Rechnungs-Anhänge aus Outlook speichern und in deinen Ordner
  "1 POSTEINGANG" legen. Das klappt immer. Ich lese die PDFs, nicht die Mails — woher sie kommen,
  ist mir egal.
- **Ein Hinweis, der wichtiger ist als die Technik:** Wenn deine eigenen Geschäftsbelege im
  Postfach eines Auftraggebers oder Arbeitgebers liegen, solltest du sie dort herausholen. Du bist
  zehn Jahre lang aufbewahrungspflichtig (§147 AO). Verlierst du den Zugang zu diesem Postfach,
  sind deine Nachweise weg.

**Unverändert gut**
- Apple/iCloud, GMX, WEB.DE, T-Online, Posteo, mailbox.org, IONOS, Strato und weitere laufen
  weiterhin problemlos. Gmail ebenso. Betroffen ist ausschließlich Microsoft 365 / Outlook im
  Firmenkontext.

**Wissensstand:** 2026-08-04

---

## v1.109.0 — Eine echte Betrugsmail in deinem Postfach gefunden (und die Lücke geschlossen, durch die sie kam)

**Sicherheit**
- **Ich habe in deinem Bestand eine gefälschte Rechnung gefunden.** Eine angebliche
  "Microsoft Office"-Zahlungserinnerung über 651,65 Dollar — versteckt in einer Mail, die aussieht,
  als hätte jemand eine Datei mit dir geteilt. Sie nennt keine Bankverbindung, sondern eine
  amerikanische Telefonnummer: "Wenn Sie stornieren wollen, rufen Sie an." Genau das ist die Masche.
  Der Anruf ist der Angriff, nicht die Überweisung. **Ruf solche Nummern nie an.**
- **Ich hatte sie durchgewinkt.** Ehrlich gesagt: Ich hatte sie als normale Rechnung eingestuft, und
  meine Betrugsprüfung meldete "keine Auffälligkeiten". Zwei Gründe, beide jetzt behoben:
  - Meine Betrugsprüfung bekam den Text der Belege nie zu sehen. Sie suchte nach Text an einer
    Stelle, die beim Einlesen gar nicht gefüllt wird — drei ihrer sechs Prüfungen liefen deshalb
    seit dem Bau ins Leere. Ab sofort lese ich den Text bei Bedarf direkt aus dem PDF nach.
  - Es fehlte die Prüfung auf genau dieses Muster: keine Bankverbindung, aber eine Rufnummer plus
    Druck ("stornieren", "Rückerstattung"). Die gibt es jetzt.
- **Gegenprobe an deinem kompletten Bestand:** 1.697 Belege geprüft, genau 2 Treffer — beide Male
  diese eine Betrugsmail. Keine einzige echte Rechnung wurde fälschlich gemeldet. Rechnungen mit
  Bankverbindung sind grundsätzlich nie betroffen, auch wenn eine Servicenummer draufsteht.

**Was das für dich heißt**
- Solche Belege markiere ich künftig rot und buche sie nicht automatisch — du entscheidest.
- Der Fund zeigt auch: Wer Rechnungen aus dem Postfach zieht, zieht irgendwann auch Betrugsversuche
  mit. Das ist kein Grund, es nicht zu tun, aber ein Grund für diese Prüfung.

## v1.108.0 — Zwei Lese-Regeln geschärft: verrechnete Belege und der richtige Firmenname

**Verbessert**
- **Belege, die sich selbst aufheben, lese ich jetzt nachweislich richtig.** Beispiel: eine
  Roller-Fahrt kostet 6,40 Euro, dein Abo verrechnet sie komplett — unter dem Strich zahlst du
  0,00 Euro. Solche Belege gibt es öfter als man denkt (Abos mit Freikontingent, Guthaben,
  Gutschriften). Ich nehme dafür ab sofort ausdrücklich die Summenzeile ganz unten, nie eine
  einzelne Position weiter oben. Das stand vorher nur sinngemäß in meiner Anweisung, jetzt steht
  es wörtlich drin — samt Beispiel.
- **Beim Firmennamen nehme ich die rechtliche Firma, nicht den Markennamen.** Auf einer Anzeigen-
  Rechnung steht oben "Meta for Business", weiter unten aber "Meta Platforms Ireland Ltd.". Für
  dich zählt die zweite: das Sitzland entscheidet, ob die Rechnung ohne Umsatzsteuer kommt und du
  sie umgekehrt anmelden musst. Gleiches gilt für Filialen — "Shurgard Berlin Friedrichshain" ist
  die Filiale, "Shurgard Germany GmbH" die Firma.

**Behoben**
- Bei drei Belegen eines Anbieters hatte ich einen Buchstaben zu viel im Firmennamen gelesen
  ("Outscraprer" statt "Outscraper"). Korrigiert, jeweils gegen das Original-PDF geprüft. Eine
  Kontrolle über alle 1.528 prüfbaren Belege zeigt: bei 93 Prozent steht der von mir gelesene
  Name wörtlich so im PDF, ein zweiter Fall dieser Art war nicht dabei.

**Unter der Haube**
- Die Widersprüche zwischen meiner Erkennung und der von BuchhaltungsButler sind jetzt einzeln
  gegen die Original-PDFs geprüft (177 Fälle). Wo beide etwas gelesen hatten und es entscheidbar
  war, lag ich in 105 von 140 Fällen richtig, BuchhaltungsButler in 35 — die beiden Regeln oben
  stammen genau aus dieser Auswertung. Ehrlichkeitshalber: bei einem sauberen Vergleich mit von
  Hand gelesener Wahrheit liegen beide Programme praktisch gleichauf. Der Unterschied liegt nicht
  im Ablesen einzelner Zahlen.

## v1.107.0 — Was BuchhaltungsButler kostet und kann: drei Angaben richtiggestellt

**Richtiggestellt**
- **Der Schnittstellen-Zugang ist guenstiger zu haben, als ich dachte.** Ich hatte notiert: wer mich
  mit BuchhaltungsButler nutzen will, braucht das teuerste Paket (59,90 Euro im Monat). Richtig ist:
  es gibt den Zugang auch als Zusatz für 5,90 Euro zu einem kleineren Paket. Damit sind es
  rechnerisch ab etwa 36 Euro statt 60. Ob sich der Zusatz mit dem kleinsten Paket kombinieren
  laesst, sagt die Preisseite allerdings nicht ausdrücklich — das müsstest du beim Buchen einmal
  ausprobieren. Ich sage dir das lieber offen, statt dir eine Zahl zu nennen, die dann nicht haelt.
- **Die Beleg-Obergrenze stimmte nicht.** Bei mir stand "100 Belege". Auf der Preisseite steht:
  500 Belege pro Monat in allen Paketen. Woher die 100 in meinem Test kamen, ist noch offen.
- **Vereine koennen BuchhaltungsButler nutzen.** Es gibt dort Vereins-Kontenrahmen (SKR42 und SKR49)
  als Zusatz fuer 4,90 Euro. Das hatte ich bisher gar nicht auf dem Schirm.

**Was das fuer dich heisst**
- Wenn du sevDesk nutzt: nichts, das aendert sich fuer dich nicht. Die Zahlen sind nur wichtig, wenn
  du oder jemand, den du kennst, mit BuchhaltungsButler arbeitet.
- Preislich liegen die beiden Programme naeher beieinander, als es bei mir stand — bei sevDesk ist
  der Schnittstellen-Zugang im Preis drin, bei BuchhaltungsButler kommt er obendrauf.

**Unter der Haube**
- Ich habe mir notiert, wie ich mich bei dieser Recherche zweimal selbst reingelegt habe: Preise aus
  einer Vergleichstabelle zu lesen, bei der die Spalten keine Beschriftung tragen, geht schief. Die
  Wahrheit stehen in den Paketkarten oben auf der Seite, nicht in der Detailtabelle darunter.

## v1.106.0 — Neu: Übergib mir deine alte Buchhaltung — ich prüfe sie und lerne daraus

**Neue Funktionen**
- **Modus 16 — Alt-Buchhaltungs-Audit.** Du gibst mir einen Ordner mit deiner alten Buchhaltung
  (z.B. die Jahre 2023–2024, egal ob vom Steuerberater oder einem anderen Programm gefuehrt) und
  ich gehe sie einmal komplett durch: Ist alles vollstaendig (fehlende Monate, Luecken bei den
  Rechnungsnummern, Zahlungen ohne Beleg)? Ist alles korrekt (Steuersaetze, Reverse-Charge,
  Dubletten, privat/geschaeftlich vermischt)? Und was lerne ich daraus fuer deine kuenftige
  Buchhaltung (deine typischen Anbieter, Abos, Konten)?
- **Rein lesend, garantiert.** Ich buche nichts, lade nichts hoch und fasse keine einzige Datei
  in deinem Ordner an. Am Ende bekommst du einen Report mit Ampel-Urteil, jedem Fund samt Beleg
  (Datei + Fundstelle) — und einer ehrlichen Liste, was ich NICHT geprueft habe. Auch eine
  Steuerberater-Abnahme nehme ich als Hinweis, nicht als Beweis: geprueft wird trotzdem.
- **Fertiger Prompt zum Kopieren.** Im Ordner liegt jetzt `PROMPT-ALT-BUCHHALTUNGS-AUDIT.md` —
  ein Vorlagen-Text, den du nur ausfuellen und mir schicken musst. Alternativ reicht ein Satz wie
  "pruef mal meine alte Buchhaltung in <Ordner>", ich erkenne den Auftrag auch so.

**Unter der Haube**
- Menue, Kurzbefehl `16` und Freitext-Erkennung ergaenzt; klare Abgrenzung zu Modus 6 (alte
  Kontoauszuege IMPORTIEREN) und Modus 2 (Check der von mir gefuehrten laufenden Buchhaltung).
- Datenschutz eingebaut: Namen fremder Personen aus deinen alten Belegen erscheinen im Chat nur
  maskiert; Details bleiben in der lokalen Report-Datei.

## v1.105.0 — Ein stiller Datumsfehler, der deine Umsatzsteuer verschieben konnte

**Behoben**
- 🔴 **Rechnungsdaten in deutscher Schreibweise landeten im falschen Quartal.** Ein paar Anbieter
  schreiben das Rechnungsdatum als `24.09.2025` statt in der Form, die ich intern erwarte. Ich habe
  das bisher ungeprueft uebernommen. Folge: der Beleg konnte im falschen Quartal abgelegt und in
  der falschen Umsatzsteuer-Voranmeldung gelandet sein — **ohne dass irgendwo eine Fehlermeldung
  kam.** Genau das macht so einen Fehler gefaehrlich: er faellt nicht auf.
  Ab jetzt rechne ich jedes Datum beim Einlesen in die einheitliche Form um. Betroffen waren bei
  mir 8 von rund 1.700 Belegen — bei dir kann die Zahl anders aussehen, der Fehler ist derselbe.
- **Ich rate dabei nicht.** Bei eindeutigen Angaben (`24.09.2025`) rechne ich um. Bei mehrdeutigen
  (`03/04/2025` — 3. April oder 4. Maerz?) lasse ich das Feld lieber leer und melde es dir, statt
  zu raten. Ein leeres Feld siehst du, ein um einen Monat verschobener Beleg nicht.

**Neue Funktionen**
- **Selbstpruefung aller Beleg-Felder.** Ein neues Werkzeug geht einmal durch alle eingelesenen
  Belege und meldet Felder, die vom vereinbarten Format abweichen — dieselbe Fehlerklasse wie oben,
  bevor sie sich auf deine Buchhaltung auswirkt. Rein lesend, aendert nichts.
- **Systemmails aussortieren.** Wurde eine reine Benachrichtigung faelschlich als Rechnung
  eingestuft, raeume ich sie jetzt weg. Das Kriterium ist bewusst streng: nur wenn **gleichzeitig**
  Datum, Betrag und Rechnungsnummer fehlen. Eine echte Rechnung erfuellt das nie — fehlt nur eines
  davon, bleibt der Beleg liegen und du siehst ihn. Nichts verschwindet still.

**Wissensstand korrigiert**
- **Zur E-Bilanz und zur EUeR** habe ich meine Unterlagen ueberarbeitet (Details standen schon in
  v1.103.0). Neu dazu: eine ausfuehrliche Einordnung, warum die E-Bilanz technisch laengst ans
  Finanzamt geht, aber trotzdem ein zweites Programm braucht — inklusive der Preise der Anbieter.
- **Zu fehlenden Rechnungsnummern:** Fehlt auf einem Beleg die Nummer, heisst das meistens nicht,
  dass der Anbieter keine vergeben hat — sie steht auf dem PDF und wurde nur nicht gelesen. Wie man
  die beiden Faelle unterscheidet (und wann eine fehlende Nummer nach §33 UStDV voellig in Ordnung
  ist, naemlich bei Kleinbetragsrechnungen bis 250 Euro), steht jetzt in meinen Unterlagen.

## v1.104.0 — Rechnungsnummern: ein Fehler von mir, und was ich daraus gebaut habe

**Richtiggestellt**
- 🔴 **Ich habe beim Nachsehen der naechsten Rechnungsnummer drei Nummern verbrannt.** Um dir zu
  sagen, welche Nummer als naechstes kommt, hatte ich einen sevDesk-Aufruf benutzt, der laut
  seinem eigenen Parameter (`useNextNumber=false`) nur schauen sollte. Er zaehlt aber trotzdem
  hoch. Ergebnis: der Zaehler sprang von 1001 auf 1004, obwohl nur eine einzige Rechnung
  existierte. Kein Geldschaden, aber eine unnoetige Luecke — und meine Aussage "die Nummer wurde
  nicht verbraucht" war schlicht falsch.
  **Ab jetzt lese ich Belegnummern nur noch ueber einen Weg, der garantiert nichts veraendert.**
- **Klickwege nenne ich nicht mehr aus dem Gedaechtnis.** Ich hatte dir "Einstellungen →
  Rechnungswesen" genannt — diesen Menuepunkt gibt es bei sevDesk gar nicht. Richtig ist
  **Einstellungen → Buchhaltung → Nummernkreise**. Kuenftig schaue ich erst in meiner
  Klickweg-Doku nach oder recherchiere die offizielle Hilfe, statt etwas plausibel Klingendes
  zu sagen.
- **Nummernkreise lassen sich nicht per Schnittstelle aendern — jetzt sauber geprueft.**
  Ich hatte das zunaechst nur vermutet. Der Test zeigt: sevDesk kennt die Funktion, sperrt sie
  aber fuer Schnittstellen-Zugriffe (Fehler "Access denied"). Aendern geht also nur in der
  Oberflaeche, lesen jederzeit automatisch.

**Unter der Haube**
- Zwei neue feste Regeln: Aufrufe, deren Name nach "naechste Nummer holen" klingt, gelten als
  veraendernd, bis das Gegenteil gemessen ist — und ein Klickweg ist eine pruefbare Tatsache,
  keine Erinnerung.
- Die Klickweg-Uebersicht wurde korrigiert und erweitert (Nummernkreise inkl. Format-Variablen,
  richtiger Menuepfad "Unternehmen" statt "Mein Unternehmen").

## v1.103.0 — Korrektur: was ich dir ueber Bilanz und Finanzamt gesagt habe, stimmte nicht ganz

**Richtiggestellt**
- **Die E-Bilanz kann BuchhaltungsButler nicht selbst ans Finanzamt schicken.** Ich hatte das
  Gegenteil in meinen Unterlagen stehen. Richtig ist: das Programm erstellt deine Bilanz, aber
  fuer das Einreichen exportierst du eine Saldenliste und uebertraegst sie in ein zweites
  Programm namens eBilanz+. Das ist eine fremde Firma, kein Teil von BuchhaltungsButler.
- **Die EUeR dagegen geht dort inzwischen doch direkt ans Finanzamt.** Auch das stand bei mir
  noch falsch drin ("kann kein Programm"). Es gibt eine Zusatzfunktion fuer 5,90 Euro im Monat,
  die die Anlage EUeR im amtlichen Format erstellt und direkt uebermittelt.
- **Und wir muessen "keine App kann die Umsatzsteuer direkt ans Finanzamt senden" streichen.**
  Das war schlicht falsch: alle drei Programme koennen das per Klick. Nur ueber die
  Schnittstelle, also automatisch ohne dich, geht es nicht.

**Neu erklaert**
- **Warum die Bilanz nicht ueber das kostenlose ELSTER-Portal geht.** Kurz: das Finanzamt nimmt
  die E-Bilanz sehr wohl an, der Uebertragungsweg dafuer existiert seit 2014. Aber im
  kostenlosen Portal "Mein ELSTER" gibt es dafuer kein Formular. Eine Bilanz besteht aus
  hunderten Positionen, die aus deinem Kontenrahmen kommen muessen, das tippt man nicht in eine
  Eingabemaske. Deshalb braucht es ein Programm.
- **Was das kostet, falls es dich betrifft.** Es gibt kostenlose Programme dafuer (MyEbilanz)
  und guenstige (eBilanz+ ab 35 Euro pro Abgabe). Du zahlst nie fuer den Versand ans Finanzamt
  selbst, sondern nur fuer die Software, die deine Zahlen ins amtliche Format bringt.
- **Wen das ueberhaupt angeht:** nur wer bilanzieren muss, also GmbH, UG, AG und grosse
  Einzelunternehmen. Wer eine Einnahmenueberschussrechnung macht, hat mit alldem nie zu tun.

**Unter der Haube**
- Die Wahrheit steht ab jetzt an genau einer Stelle, alle anderen Unterlagen verweisen darauf.
  Der Fehler oben war naemlich in sieben Dateien gleichzeitig gelandet, weil jede ihre eigene
  Fassung hatte. So etwas soll sich nicht wieder ueber Monate ausbreiten koennen.
- Ich habe mir zusaetzlich notiert, woran es lag: eine unsichere Angabe von einer Anbieter-
  Webseite wurde beim Weiterkopieren stillschweigend zu einer sicheren. Kennzeichnungen wie
  "ungeprueft" muessen mitwandern, sonst nuetzen sie nichts.

**Wissensstand**
- Geprueft am 1. August 2026 an amtlichen Unterlagen der Finanzverwaltung und den Seiten der
  Anbieter. Ein Punkt bleibt bewusst offen: ob der Schnittstellen-Zugang bei BuchhaltungsButler
  im Tarif enthalten ist oder 5,90 Euro extra kostet, widerspricht sich zwischen meinen
  Unterlagen und der aktuellen Bestellseite. Ich habe beide Staende nebeneinander notiert statt
  zu raten, welcher stimmt.

## v1.102.0 — Rechnungsdatum kann nicht mehr still falsch sein

**Verbesserungen**
- **Das Rechnungsdatum wird jetzt immer gleich geschrieben.** Manche Rechnungen liefern
  "24.09.2025", andere "2025-09-24" — gemeint ist dasselbe, aber meine Ablage und die
  Umsatzsteuer-Zuordnung rechnen nur mit der zweiten Form. Kam die erste an, landete der Beleg
  still im falschen Quartal, ohne dass irgendwo ein Fehler auftauchte. Genau solche stillen
  Fehler sind die gefaehrlichen. Ab jetzt wird das Datum direkt beim Auslesen vereinheitlicht.
- **Mehrdeutige Datumsangaben werden nicht mehr geraten.** Bei "03/04/2025" ist nicht
  entscheidbar, ob der 3. April oder der 4. Maerz gemeint ist. Statt zu raten lasse ich das Feld
  leer und melde es — ein um einen Monat verschobener Beleg in der Voranmeldung waere teurer als
  eine Rueckfrage.
- **Deine vorhandenen Belege sind mitrepariert.** Acht Rechnungen mit deutschem Datumsformat
  gefunden und korrigiert (FlixTrain, Emmy, TurboScribe) — jede vorher gegen das Original-PDF
  geprueft. Von jeder geaenderten Datei liegt eine Sicherung daneben.
- **Systemmails landen nicht mehr in der Buchungsliste.** Acht Benachrichtigungen ("X ist dem
  Team beigetreten") waren faelschlich als Rechnung eingestuft. Sie wurden nie gebucht — die
  Null-Betrags-Regel hat sie abgefangen — aber sie standen in der Liste offener Belege.
  Aussortiert, nicht geloescht: sie liegen jetzt im Ordner fuer Nicht-Belege.

**Unter der Haube**
- Zweiter Fall derselben Fehlerklasse wie beim Steuersatz: ein Feld hat eine Formatzusage, aber
  niemand setzt sie durch. Beide sind jetzt an der Quelle abgesichert, nicht bei den einzelnen
  Lesern.
- Fuenf neue Tests, darunter ausdrueckliche Tests dafuer, dass mehrdeutige Formate NICHT
  interpretiert werden. Alle 19 Tests laufen gruen.

## v1.101.0 — Steuersatz-Falle geschlossen (bevor sie dich treffen konnte)

**Verbesserungen**
- **Der Steuersatz wird jetzt immer gleich geschrieben.** Die Texterkennung liefert bei manchen
  Rechnungen "19" und bei anderen "0,19" — beides meint 19 Prozent, aber meine Pruefungen
  rechnen mit der zweiten Form. Kam die erste an, meldete ich eine voellig korrekte Rechnung als
  widerspruechlich und legte sie unnoetig in die Pruefliste. Ab jetzt wird der Satz direkt beim
  Auslesen einheitlich gemacht, damit das nicht mehr passieren kann.
- **Deine bereits gelesenen Belege sind mitrepariert.** Ich habe alle 1.697 vorhandenen Belege
  durchgesehen, drei mit der alten Schreibweise gefunden und korrigiert — jeden davon vorher
  gegen das Original-PDF geprueft, nicht blind umgerechnet. Von jeder geaenderten Datei liegt
  eine Sicherung daneben.

**Unter der Haube**
- Die Vereinheitlichung sitzt jetzt an der Stelle, an der der Wert entsteht, statt an zwoelf
  Stellen, die ihn spaeter lesen. Vier davon waren abgesichert, acht nicht — darunter die
  Kontrolle, die direkt vor dem Buchen laeuft. Eine Quelle statt zwoelf Reparaturstellen.
- Unsinnige Werte (etwa 1900 Prozent) werden verworfen statt weitergereicht. Auslaendische
  Saetze wie 21 Prozent bleiben dagegen erhalten — sie sind eine echte Information und duerfen
  nicht stillschweigend verschwinden.
- Sechs neue Tests sichern das Verhalten ab, alle bestehenden Tests laufen unveraendert weiter.

**Wissensstand**
- Gemessen am 1. August 2026 an 46 echten Rechnungen: BuchhaltungsButler liest sie ohne meine
  Hilfe zu 95,7 Prozent fehlerfrei, ich komme auf 97,8 Prozent. Das ist praktisch gleichauf —
  ein Beleg Unterschied. Diese Zahl steht hier, damit niemand einen Vorsprung behauptet, den es
  nicht gibt: mein Nutzen liegt nicht im Lesen, sondern davor und danach.

## v1.100.0 — Keine Rechnung mehr ohne Empfaengeranschrift

**Neue Funktionen**
- **Ich trage die Anschrift deines Kunden jetzt selbst in den Beleg ein.** sevDesk uebernimmt die
  Adresse aus dem Kontakt NICHT automatisch in die Rechnung — das Feld bleibt leer, wenn es
  niemand fuellt. Genau das ist mir passiert: eine fertige Rechnung ging mit leerem Adresskopf
  raus, und du musstest es selbst bemerken. Ab jetzt hole ich die Adresse aus dem Kontakt und
  schreibe sie in den Beleg, bei Rechnungen und bei Angeboten.
- **Neuer Sicherheitsstopp vor jedem Ausgangsbeleg.** Fehlt Name, Strasse, PLZ oder Ort, lege ich
  die Rechnung gar nicht erst an, sondern sage dir genau, was fehlt und wie es nachgetragen wird.
  Grund: ohne vollstaendige Anschrift des Empfaengers ist eine Rechnung nicht ordnungsgemaess
  (§14 Abs. 4 Nr. 1 UStG) — dein Kunde koennte die Vorsteuer nicht ziehen, und bei einer Pruefung
  ist der Beleg angreifbar.
- **Ausnahme fuer Kleinbetraege bleibt moeglich.** Bis 250 Euro brutto ist die Empfaengeranschrift
  nicht vorgeschrieben (§33 UStDV). Fuer diese Faelle gibt es einen bewussten Schalter — er wird
  nie stillschweigend gesetzt, du siehst immer einen Hinweis.
- **Zahlungsziel kann ich jetzt direkt mitgeben.** Bisher musstest du die Faelligkeit von Hand in
  sevDesk nachtragen. Jetzt setze ich sie beim Anlegen (z.B. 14 Tage); ohne Angabe bleibt es bei
  "sofort faellig".
- **Kunden anlegen geht in einem Schritt.** Beim Anlegen eines neuen Kontakts kann ich Strasse,
  PLZ, Ort und E-Mail gleich mit hinterlegen, statt nur den Namen. Halbe Adressen weise ich ab —
  eine unvollstaendige Anschrift ist kein Teilerfolg.

**Unter der Haube**
- Nach dem Schreiben pruefe ich zusaetzlich, ob die Anschrift wirklich im Beleg steht. Fehlt sie,
  schlaegt die Kontrolle fehl, statt einen leeren Kopf durchzuwinken.
- Zwei neue feste Regeln: Ausgangsbelege ohne Empfaengeranschrift sind gesperrt, und bei einem
  Fehler einer Schnittstelle pruefe ich zuerst meinen eigenen Aufruf, bevor ich der Gegenseite
  die Schuld gebe. Letzteres war hier die eigentliche Ursache — ich hatte die Daten falsch
  verpackt und daraufhin faelschlich behauptet, sevDesk lehne sie ab.


---

**Aeltere Versionen (v1.99 und frueher):** siehe [KUNDEN-CHANGELOG-ARCHIV.md](KUNDEN-CHANGELOG-ARCHIV.md)
