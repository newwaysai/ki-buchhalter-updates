## v1.214.0 — „Fehlt ein Kontoauszug?" wird jetzt gemessen statt geschätzt (2026-08-18)

### Verbesserungen

- **Neuer Befehl `konten-reichweite.mjs`** beantwortet die Frage „fehlt mir ein Kontoauszug?"
  mit einer Messung am Konto: Reicht der Bewegungsbestand bis heute? Fehlt dazwischen ein
  ganzer Monat? Nur wenn dort etwas klemmt, musst du wirklich einen Auszug besorgen.
- **Der Offenstands-Bericht sagt nicht mehr voreilig „Kontoauszug fehlt".** Bisher schloss er
  aus „zu diesem Beleg finde ich keine passende offene Kontobewegung" auf einen fehlenden
  Auszug. Das stimmt oft nicht — die Zahlung kann längst zugeordnet sein, in einer
  Sammelabbuchung stecken (z.B. 18,92 € für zwei Belege à 9,46 €), in Fremdwährung gelaufen
  oder nie erfolgt sein. Jetzt nennt der Bericht diese Gründe **und liefert die Konten-Messung
  direkt mit**, statt dich auf eine Suche zu schicken, die es nicht braucht.
- **Weniger Fehlalarme im Buchhaltungs-Check:** Die Prüfung „Bank-Kontinuität" schlug bisher
  auch bei Konten an, die gar keine Auszüge haben können — Verrechnungskonten und aufgelösten
  Konten. Bei einem geschlossenen Konto sind bewegungslose Monate der Normalfall, kein Loch.
  Im echten Bestand verschwinden dadurch 3 Dauer-Warnungen, ohne dass eine echte Lücke
  übersehen wird (echte Bankkonten werden unverändert streng geprüft).

### Unter der Haube

- Neue Prüfung mit 22 Fällen, in beide Richtungen abgesichert: Ein aufgelöstes Konto darf nicht
  ewig mahnen — ein echtes Bankkonto mit Lücke muss aber weiterhin sofort auffallen. Mit
  Sabotage-Probe abgenommen. Prüf-Sammellauf: 62 Prüfungen, alle grün.

## v1.213.0 — Der Stau-Wächter war nie eingeschaltet, jetzt nennt er auch die Ursache (2026-08-17)

### Verbesserungen

- **Ein Wächter, der seit Juli nie lief, ist jetzt aktiv.** Es gibt eine Prüfung, die meldet,
  wenn Belege zu lange ungebucht im Eingangsordner liegen. Sie war gebaut und getestet, wurde
  aber von keinem Programmteil aufgerufen — sie hat also nie etwas gemeldet. Jetzt läuft sie bei
  jeder Prüfung mit und fand sofort: 280 buchungsbereite Belege (ältester von Dezember 2024) und
  47 Belege, die über 45 Tage auf eine Entscheidung warten.
- **Neu: die Prüfung sagt jetzt auch, WORAN der Stau hängt** — nicht nur, dass etwas liegt.
  Zwei Ursachen erkennt sie automatisch: **89 Belege sind Scans ohne lesbaren Text** (dort laufen
  alle Textprüfungen ins Leere, sie brauchen einen Blick aufs Bild), und **47 Anbieter fehlen im
  Anbieter-Verzeichnis** (deren Belege werden bewusst nicht automatisch kontiert — geraten wird
  nie). Beides kann Bruno selbst abarbeiten, es steht jetzt nur nicht mehr unsichtbar im Ordner.
- Für beide Ursachen nennt der Bericht direkt den nächsten Handgriff, inklusive des Hinweises,
  dass Kleinbetragsbelege bis 250 Euro (Kassenbons, Parkscheine) gar keine Rechnungsnummer
  brauchen — die werden sonst leicht als unvollständig aussortiert.

## v1.212.0 — Vorschau-Liste brach bei Belegen ohne Betrag ab, Monatsordner bekommen eigene Lauf-Namen (2026-08-18)

### Verbesserungen

- **Die Beleg-Vorschau bricht nicht mehr ab.** Lag in einem Monatsordner ein Dokument ohne
  erkennbaren Betrag (z.B. ein Anschreiben ohne Rechnungssumme), stoppte die Vorschau dort —
  und alle weiteren Belege dieses Ordners wurden gar nicht mehr angezeigt. Der Ordner sah
  dann leer aus, obwohl Belege drin lagen. Solche Dokumente stehen jetzt am Ende der Liste
  unter „Ohne Betrag" beim Namen, statt die Ausgabe abzuschneiden.
  🔴 **Buchungen waren davon nie betroffen** — der eigentliche Buchungsweg hat diese Belege
  schon immer sauber angehalten („OCR unvollständig"). Betroffen war nur die Vorschau.
- **Der Ordner-Schlüssel heißt jetzt, was er ist.** Seit die Belegordner monatlich geführt
  werden, passte der alte Name `BUCHHALTUNG_QUARTAL_DIR` nicht mehr zum Inhalt. Neuer Name:
  `BUCHHALTUNG_PERIODEN_DIR` (Monat oder Quartal).
  🔴 **Du musst nichts ändern:** der alte Name funktioniert unverändert weiter. Wer ihn in
  seiner Konfiguration stehen hat, merkt von der Umbenennung nichts.

### Unter der Haube

- **Jeder Monatslauf bekommt einen eigenen Namen.** Vorher hießen alle Läufe aus Monatsordnern
  gleich („x-qx"), weil der Name nur für Quartale gebaut war. Folge: Buchungen mehrerer Monate
  trugen dieselbe Kennzeichnung und ließen sich nicht mehr einzeln zurücknehmen. Jetzt heißt
  jeder Lauf nach seinem Monat. Unbekannte Ordner bekommen einen deutlich sichtbaren Hinweis
  statt eines Namens, der echt aussieht.
- Neue Prüfung mit 18 Fällen, die beides absichert — besonders, dass der alte Schlüsselname
  weiterhin greift. Prüf-Sammellauf: 61 Prüfungen, alle grün, mit Sabotage-Probe abgenommen.

## v1.211.0 — „Reist die Datei mit?" ist jetzt ein Befehl, Posteingang-Kontrolle wird Pflicht-Abschluss (2026-08-17)

### Verbesserungen

- **Neue Export-Vorprüfung** (`system/_bin/reist-mit.mjs`): beantwortet vor jedem Verschieben,
  Löschen oder Umbenennen einer Bruno-Datei die Frage „Reist sie mit dem Produkt?" — mit genau
  derselben Technik, die auch der echte Export nutzt (keine zweite, abweichende Logik, die
  auseinanderlaufen könnte). Gibt es keine Ausschlussliste (normale Kunden-Installation),
  lautet die Antwort vorsichtshalber immer „reist mit".
- **Der Beleg-Einlese-Ablauf (Modus 1a) endet jetzt immer mit der Posteingang-Kontrolle**
  aus v1.210.0: Bruno meldet nie „fertig", solange noch unverarbeitete Dateien im Posteingang
  liegen. Belege, die nicht zu deiner Firma gehören, wandern nach „5 SONSTIGE BELEGE".

### Unter der Haube

- Beide neuen Prüfungen laufen jetzt im automatischen Prüf-Sammellauf mit: 60 Prüfungen
  statt 58, alle grün. Jede wurde mit einer Sabotage-Probe abgenommen (Prüfung absichtlich
  kaputt gemacht → Test muss Alarm schlagen — hat er).

## v1.210.0 — Versions-Wächter erkennt Rücksetzungen + Posteingang-Kontrolle (2026-08-17)

### Verbesserungen

- **Der Versions-Wächter erkennt jetzt auch Rücksetzungen.** Bisher prüfte er nur, ob eine
  Versionsnummer einen Changelog-Eintrag hat — wurde die Nummer versehentlich auf einen älteren
  Stand zurückgesetzt, blieb das unsichtbar (real passiert bei parallel laufenden Arbeiten).
  Jetzt schlägt er an, sobald die eingetragene Version nicht die höchste ist. Der Vergleich
  rechnet mit echten Zahlen, damit auch Version 1.10 korrekt als neuer gilt als 1.9.
- **Neue Posteingang-Kontrolle** (`system/_bin/posteingang-check.mjs`): prüft nach jedem
  Beleg-Einlese-Lauf, dass der Posteingang wirklich leer ist — lose Dateien oder volle
  Unterordner werden gemeldet (nur Meldung, verschoben oder gelöscht wird nichts).

## v1.209.0 — Nachtlauf startet schlanker, jeder Prüf-Schritt bleibt nachlesbar (2026-08-17)

### Verbesserungen

- **Leere Beleg-Ordner werden im Komplett-Durchlauf übersprungen** (gemessen: ~25 von 32
  Perioden-Ordnern enthalten gerade nichts Buchbares — jeder kostete trotzdem einen
  Programmstart, zusammen rund eine Minute pro Lauf). Ordner mit Belegen laufen exakt wie
  vorher; die Prüftiefe ändert sich nicht, nur der Leerlauf entfällt.
- **Jeder Lauf-Abschluss wird jetzt zusätzlich als eigene Datei archiviert** (die neuesten 200).
  Vorher überschrieb in einem Komplett-Durchlauf jeder Schritt das Prüf-Protokoll des vorigen —
  der automatische Zweitprüfer sah am Ende nur den letzten (leeren) Schritt statt des ganzen
  Laufs. Jetzt ist jeder Schritt einzeln nachlesbar, auch Abbrüche.

## v1.208.0 — Rechnungen in Fremdwährung finden ihre Zahlung wieder (2026-08-18)

### Verbesserungen

- **Dollar-Rechnungen werden wieder ihrer Abbuchung zugeordnet.** Deine Bank bucht in Euro ab,
  die Rechnung steht in Dollar — 120,00 USD wurden als 115,94 € belastet. Bruno verglich bisher
  auf den Cent genau und fand deshalb *nie* eine passende Zahlung. Ergebnis: Belege blieben
  liegen, obwohl die Zahlung längst auf dem Konto stand. Bruno rechnet jetzt den Kurs zurück und
  erkennt die Abbuchung. Gemessen: **74 Belege von 24 Anbietern** haben dadurch wieder einen
  belegten Zahlungsnachweis (Namecheap, ElevenLabs, OpenRouter, Anthropic, Replit u.a.).
- **Anbieternamen werden robuster erkannt.** „Namecheap, Inc." auf der Rechnung und
  „NAME-CHEAP.COM* KL4Q" auf dem Kontoauszug gelten jetzt als derselbe Anbieter. Firmierungs-
  und Domain-Endungen (Inc., GmbH, .com) stören den Abgleich nicht mehr.
- **Überholte Prüf-Vermerke aufgeräumt.** Zehn Belege trugen noch einen Vermerk aus einer
  einmaligen Sonderprüfung im August und warteten seither auf eine Freigabe, die niemand mehr
  geben musste. Sie sind wieder im normalen Ablauf.

### Unter der Haube

- Der Kursabgleich ist **strenger** als der normale Betragsvergleich, nicht lockerer: Er greift
  nur bei Fremdwährung, verlangt zusätzlich einen passenden Anbieternamen, akzeptiert nur Kurse
  im gemessenen Korridor (0,80–0,98) und urteilt **gar nicht**, wenn mehrere Zahlungen in Frage
  kommen. Euro-Rechnungen werden unverändert auf den Cent genau geprüft.
- Doppelt-Buchen-Schutz erweitert: Ein Beleg gilt jetzt auch dann als vorhanden, wenn er unter
  einer *anderen* Belegnummer schon gebucht ist — erkannt über Originalbetrag und Datum. Genau
  dieser Fall trat auf (derselbe Beleg, zwei Nummern) und wurde abgefangen.
- 23 neue automatische Prüfungen sichern die Änderungen ab, darunter die Fälle, die
  **nicht** durchgehen dürfen (fremder Anbieter mit ähnlichem Namen, unplausibler Kurs,
  mehrdeutige Zuordnung, Geldeingang statt Ausgabe).

## v1.207.0 — Brunos Regelwerk: schlanker Kopf, gleiche Härte (2026-08-17)

### Verbesserungen

- **Das zentrale Regelwerk ist übersichtlicher geworden (Stufe 1 von 2):** Acht Regel-Komplexe,
  die längst von Code-Prüfern erzwungen werden (z.B. „liegt der Beleg schon da?",
  Offenstand-Messung, Versions-Wächter), stehen jetzt als kompakter Kernsatz mit Verweis auf
  den Prüfer. Die ausführlichen Herleitungen und Real-Fälle sind vollständig in die neue Datei
  `system/VERHALTENS-REGELN.md` umgezogen (reist mit dem Produkt mit). **Keine Regel wurde
  gelöscht oder abgeschwächt** — bewiesen per Wortlaut-Abgleich (22 Kernsätze unverändert),
  Sektions-Zählung (26 = 26) und Kanarien-Suite.
- **Neue Schutzregel:** Vor jedem Verschieben/Löschen einer Bruno-Datei wird geprüft, ob sie
  mit dem Produkt reist — verhindert, dass eine Datei still aus Kunden-Updates verschwindet.

### Unter der Haube

- Umbau in zwei getrennten, einzeln rückrollbaren Schritten (erst wortwörtliche Kopie, dann
  Kürzung). Stufe 2 (Verhaltens-Regeln mit Beobachtungs-Auftrag) folgt erst nach 1–2
  beobachteten Arbeitssitzungen ohne Auffälligkeit.

## v1.206.0 — Alte Belege wissen jetzt auch, ob sie Einnahme oder Ausgabe sind (2026-08-18)

### Verbesserungen

- **Nachträglich geklärt: 27 ältere Belege hatten keine Richtungsangabe.** Bei neuen Belegen
  passiert das längst automatisch beim Einlesen — Belege, die vorher erfasst wurden, hatten das
  Feld nicht. Darunter **20 eigene Ausgangsrechnungen über 7.256,70 €**, die sonst Kandidaten
  gewesen wären, versehentlich als Ausgabe mit Vorsteuer gebucht zu werden.
- **Nur ergänzt, nie überschrieben:** Wo bereits eine Richtung stand, blieb sie unangetastet.
  Wo kein sicheres Signal erkennbar war (330 Belege), bleibt das Feld **bewusst leer** — dann
  greift beim Buchen weiterhin die Sperre und fragt nach. Ein geratener Wert wäre schlechter
  als ein leerer: Er sähe geprüft aus.
- **Jede Änderung nachvollziehbar:** Zu jedem Beleg ist vermerkt, woher die Richtung stammt;
  von jeder geänderten Datei liegt eine Sicherungskopie daneben.
- **Zusätzlicher Wächter für Versionsnummern:** Wird die Versionsdatei versehentlich auf einen
  älteren Stand zurückgesetzt (passiert, wenn zwei Arbeitssitzungen parallel laufen), schlägt
  die Prüfung jetzt Alarm. Vorher fiel das nur zufällig auf. Sabotage-getestet.

## v1.205.0 — Ein Befehl von der Mail bis ins Archiv + ehrlicher Daten-Zeitstempel (2026-08-17)

### Neue Funktionen

- **`nachtlauf --mit-ingest`:** Der Komplett-Durchlauf kann jetzt optional das Beleg-Holen aus
  deinen Postfächern mit erledigen — ein Befehl von der Mail bis ins Belegarchiv. Welche
  Postfächer, bestimmst DU in deinem Profil (`ingest_quellen`); ohne Eintrag wird nichts
  gescannt — Bruno rät nie ein Postfach. Gemischte Postfächer landen weiterhin zuerst im
  Trenn-Ordner, nie direkt in der Buchungs-Queue.
- **Alters-Stempel für den Daten-Schnappschuss:** Der lokale sevDesk-Schnappschuss (der
  Auswertungen blitzschnell macht) trägt jetzt einen sekundengenauen Zeitstempel, und ein
  Prüfbefehl nennt sein Alter („Stand vor 12 Min"). So gilt ein alter Datenstand nie unbemerkt
  als aktuell. Grundsatz unverändert: der Schnappschuss dient nur dem schnellen FINDEN —
  gebucht und verknüpft wird ausschließlich gegen die Live-Daten deines Buchhaltungssystems.

## v1.204.0 — Bank-Abgleich sieht jetzt IMMER alle deine Konten (2026-08-17)

### Neue Funktionen

- **Der Beleg-Bank-Abgleich läuft jetzt standardmäßig über ALLE Konten in deinem
  Buchhaltungssystem** — Bankkonten, Kreditkarten, Stripe-/PayPal-Verrechnungskonten, auch
  aufgelöste Alt-Konten. Die Konten werden bei jedem Lauf live aus sevDesk gelesen: legst du
  (oder dein Bank-Feed) ein neues Konto an, ist es automatisch dabei — nichts zu konfigurieren.
  Vorher prüfte der Abgleich nur ein Standardkonto; Zahlungen auf anderen Konten blieben
  unbemerkt liegen (real: 4 Rechnungen à 500 €, deren Zahlungen auf einem aufgelösten Konto
  lagen — der Abgleich meldete „nichts gefunden").
- **Sicherheit unverändert:** Jede Verknüpfung wird ans Konto der jeweiligen Zahlung gebucht.
  Finden sich auf zwei Konten gleich gute Kandidaten, wird NICHT geraten, sondern zur Prüfung
  vorgelegt — diese Verwechslungs-Klasse war vorher unsichtbar, jetzt wird sie erkannt.
- Der automatische Nachtlauf nutzt das direkt: ein Abgleich-Schritt statt einer Konto-Schleife —
  schneller, gleiche Prüftiefe.

## v1.203.0 — Mehrere Rechnungen in EINER Mail werden nicht mehr verwechselt (2026-08-17)

### Verbesserungen

- **Schickt dir ein Anbieter mehrere Rechnungen in einer einzigen Mail** (z.B. nach einer
  Nachforderung: vier Monats-Rechnungen zum gleichen Abo-Preis), erkennt Bruno jetzt jede als
  eigene Rechnung. Vorher konnte die Dubletten-Erkennung sie für Kopien halten und nur eine
  behalten — real passiert: von 4 angeforderten Rechnungen à 500 € wären 3 im Dubletten-Ordner
  gelandet. Die Schutzfunktion für den Gegenfall (US-Anbieter legen Rechnung UND Quittung
  derselben Zahlung in eine Mail — das IST ein Paar) bleibt erhalten. 5 neue Wächter-Tests.
- **Alte Belege ohne „Beleg-Natur"-Prüfung werden jetzt beim Buchen nachgeprüft.** Belege, die
  vor Einführung der Natur-Prüfung eingelesen wurden, umgingen das Tor „ist das überhaupt eine
  Rechnung?" — so wurde eine Zahlungs-ANKÜNDIGUNG (5,90 €) als Rechnung gebucht, obwohl die
  echte Rechnung längst im System war. Jetzt beurteilt Bruno die Natur solcher Alt-Belege
  direkt vor dem Buchen aus dem Dokumententext nach — Ankündigungen und fehlgeschlagene
  Zahlungen gehen in die Prüfliste statt ins Buchungssystem.

## v1.202.0 — 59 längst gebuchte Belege lagen im Eingangsordner fest (2026-08-18)

### Verbesserungen

- **Belege, die schon gebucht und mit deinem Konto verknüpft waren, blieben im Eingangsordner
  liegen** — sie sahen dort wie offene Arbeit aus. Ursache: drei getrennte Lücken, alle gefunden
  und geschlossen. Ergebnis heute: **59 Belege korrekt ins Belegarchiv verschoben**, jeder mit
  Eintrag im Prüfprotokoll.
- **Lücke 1 — abweichende Nummern:** Manche Systeme führen einen Vorgang unter der
  Bestellnummer, während auf der Rechnung die Rechnungsnummer steht (beide stehen auf demselben
  Beleg). Bruno gleicht jetzt zusätzlich über die Bestellnummer ab — aber nur, wenn auch der
  **Betrag auf den Cent passt**. Zwei Signale, nie eins allein.
- **Lücke 2 — die Raute:** Steht die Nummer auf dem Beleg als „Rechnung #R91PW23", wurde die
  Raute mitgespeichert und der Beleg galt als unbekannt. **27 Belege** waren betroffen, einer lag
  nachweislich gebucht im Eingang. Wichtiger noch: derselbe Beleg hätte ein **zweites Mal**
  hochgeladen werden können — die Raute war eine stille Doppel-Buchungs-Lücke.
- **Lücke 3 — fehlende Verzeichnis-Einträge:** Einige Belege lagen als Datei da, ohne im
  Jahresverzeichnis zu stehen; für jede Abfrage waren sie unsichtbar. Neues Werkzeug trägt solche
  Einträge aus den vorhandenen Beleg-Daten nach — **nur ergänzend**, es ändert nie einen
  bestehenden Eintrag und behauptet nie selbst, etwas sei gebucht (das entscheidet allein der
  Abgleich mit deinem Buchhaltungssystem).
- **Nebenbuchungen werden auseinandergehalten:** Zu einem Verkauf gehören oft zwei Buchungen
  (Erlös + Zahlungsgebühr) mit derselben Nummer. Bruno ordnet Belege nie einer Gebührenbuchung
  zu; bleibt es mehrdeutig, ordnet er **gar nicht** zu.
- **21 neue Kontrollen** sichern das ab, darunter die Probe, dass ein Nachtrag niemals eine
  Dublette erzeugt.

## v1.201.0 — Brunos Schutzprüfungen kontrollieren sich jetzt gegenseitig (2026-08-18)

### Verbesserungen

- **Neue Kontrolle prüft, ob Brunos Sicherungen überhaupt angeschlossen sind.** Hintergrund: An
  einem Tag kamen drei Fälle zusammen, in denen eine Prüfung zwar existierte, aber nie ansprang —
  einmal, weil sie nur bei bereits auffälligen Belegen lief (**272 von 275 wurden nie geprüft**),
  einmal, weil sie ein Feld auslas, das gar nichts füllt.
- **Der Kern des Problems war nicht die einzelne Lücke, sondern das Muster:** Es gab längst eine
  Regel dagegen — geschriebene Regeln greifen aber nur, wenn jemand daran denkt. Diese Kontrolle
  ist deshalb kein Merksatz mehr, sondern läuft bei jedem Prüflauf automatisch mit und schlägt
  Alarm, sobald eine Sicherung ihren Anschluss verliert.
- **Selbst überprüft:** Die Kontrolle wurde bewusst sabotiert (ein Aufruf entfernt) — sie wurde
  sofort rot und danach wieder grün. Zusätzlich prüft sie sich selbst gegen beide Fehlerrichtungen,
  damit sie weder zu streng meckert noch alles stillschweigend durchwinkt.
- **Ausdrücklich keine neue Automatik beim Buchen:** Diese Kontrolle liest nur, sie entscheidet
  nichts und verschiebt oder verwirft keinen Beleg. Wo ein fachliches Urteil nötig ist (Ist dieser
  Beleg relevant? Passt dieses Konto?), bleibt es bei einer sichtbaren Rückfrage — Automatik würde
  dort Fehler unsichtbar machen statt verhindern.

## v1.200.0 — Zwei neue Wächter: Versionsnummern und Anleitungs-Stand (2026-08-17)

### Verbesserungen

- **Keine doppelten Versionsnummern mehr.** Ein neuer Prüfer stellt sicher, dass jede
  Bruno-Version genau EINEN Changelog-Eintrag hat und keine Nummer doppelt vergeben wird
  (das war diese Woche zweimal passiert, wenn zwei Arbeitssitzungen parallel liefen).
  Er meldet auch, wenn eine Version ohne Changelog-Eintrag existiert.
- **Die Anleitung kann nicht mehr heimlich veralten.** Ein zweiter Prüfer vergleicht die
  Setup-Anleitung in allen drei Formaten (Text, PDF, Vorschau-Seite) — sagen sie nicht
  dasselbe, schlägt er Alarm. Genau so war das „14 statt 16 Modi"-PDF wochenlang
  unbemerkt geblieben.

### Unter der Haube

- Beide Wächter laufen im Kanarien-Testlauf mit (jetzt 56 Prüfungen) und testen sich
  vor jedem Lauf selbst gegen bekannte Fehlerfälle.
- Der Versions-Prüfer fand beim ersten Lauf 17 historische Doppel-Nummern aus der
  Vergangenheit. Sie sind als bekannte Altfälle markiert (sichtbar, aber kein Alarm) —
  neue Doppel-Nummern werden ab sofort sofort gemeldet.

## v1.200.1 — Bruno erkennt Ankündigungen, und er schaut sich Bilder an (2026-08-18)

### Verbesserungen

- **„Ihre Rechnung kommt demnächst" wird nicht mehr als Rechnung gebucht.** Manche Anbieter
  schicken vor dem Einzug eine Vorab-Info („bevorstehende Rechnungsstellung", „wir berechnen
  demnächst"). Die sieht aus wie eine Rechnung — Betrag, Leistung, Datum — ist aber keine. Bruno
  hat so eine Mail einmal gebucht, und weil die echte Rechnung später ebenfalls kam, stand die
  Ausgabe doppelt in den Büchern. Erkennt er jetzt zuverlässig. Wichtig dabei: Eine echte
  Rechnung, die nebenbei den Einzugstermin nennt, läuft weiterhin normal durch.

- **Belege ohne Textebene werden zur Sichtprüfung markiert statt stillschweigend durchgewinkt.**
  Bei eingescannten oder abfotografierten Belegen kann Bruno den Text nicht maschinell lesen —
  damit liefen bisher alle automatischen Prüfungen (Mahnung, Vollständigkeit, Benachrichtigung)
  bei genau diesen Dokumenten ins Leere, ohne dass das jemand gemerkt hätte. Sie werden jetzt
  ausdrücklich als „Sichtprüfung nötig" ausgewiesen.

- **Klargestellt: Ein Screenshot einer Rechnung ist ein gültiger Beleg.** Wenn alle
  Pflichtangaben drauf sind (Aussteller, Anschriften, Steuernummern, Rechnungsnummer, Datum,
  Leistung, Betrag, Steuersatz), ist das Dokument in Ordnung — unabhängig davon, ob Software den
  Text auslesen kann. Bruno wertet fehlende Lesbarkeit nicht mehr als Beleg-Mangel.

### Unter der Haube

- Neue Grundregel #41: Bilddokumente werden gerendert und angesehen, bevor über sie geurteilt
  wird. Ergänzend: Bevor ein Steuersatz als falsch gebucht gilt, prüft Bruno den Bestand des
  Anbieters auf spätere Korrektur- oder Erstattungsbelege — ein Beleg kann überholt sein.
- `beleg-natur.mjs`: neuer Erkennungszweig für Ankündigungen; greift nur ohne Rechnungsnummer.
- Geprüft an 400 Belegen aus dem Bestand: 14 Treffer, alle korrekt, keine echte Rechnung gefangen.
- 56 automatische Selbsttests grün.

## v1.198.0 — Dein Ordner erklärt sich jetzt selbst (2026-08-17)

### Verbesserungen

- **Neue Orientierungs-Tabelle in der LIESMICH.** Direkt beim Einstieg siehst du jetzt in einer
  kurzen Tabelle, welche Datei wofür da ist — und welche drei du im Alltag wirklich brauchst
  (LIESMICH, deine Aufgaben-Liste, die nummerierten Beleg-Ordner). Der Rest ist als „Brunos
  Maschinenraum" markiert, damit du nichts davon lesen oder verstehen musst.

- **Frische Setup-Anleitung.** Die bebilderte Anleitung (PDF und Vorschau-Datei) nannte noch
  14 Modi — Bruno kann inzwischen 16. Beide sind jetzt neu erstellt und stimmen wieder mit
  dem überein, was Bruno wirklich kann.

### Unter der Haube

- Interne Arbeitsstand-Archive in den Maschinenraum (`system/archiv/`) verschoben — im
  Hauptordner liegt weniger herum. Für dich ändert sich nichts.

## v1.195.0 — Bruno nennt nichts mehr „offen", nur weil es so heißt (2026-08-18)

### Verbesserungen

- **Erledigte Zahlungen tauchen nicht mehr als To-do auf.** Wenn eine Monatsrechnung viele
  einzelne Abbuchungen abdeckt (z.B. Kontogebühren: eine Rechnung, fünfzehn kleine Beträge),
  blieben diese Bewegungen in einem Zwischen-Status stehen — obwohl die Rechnung längst
  vollständig bezahlt war. Bruno hat sie deshalb in jedem Bericht als offene Arbeit gezählt.
  Jetzt erkennt er den Unterschied und weist sie getrennt aus: „abgeschlossene Teilzahlung
  (kein To-do)". Sie verschwinden nicht heimlich — du siehst weiterhin, wie viele es sind.

- **Bruno prüft nach, bevor er dir Arbeit vorschlägt.** Auslöser war ein echter Fehlgriff: Er
  hatte empfohlen, 120 Buchungen „nur noch abzuschließen" — geprüft ergab sich, dass sie bereits
  abgeschlossen waren und jeder Versuch am System abgeprallt wäre. Neue Grundregel: Ein
  Status-Name ist kein Beweis. Bevor Bruno etwas als offen, fehlend oder doppelt bezeichnet,
  braucht er einen zweiten Beleg dafür — das Dokument, den Betrag oder eine Rückfrage ans System.

### Unter der Haube

- Neue Grundregel #40 („Ein Zustandsname ist kein Nachweis") mit vier belegten Beispielen aus
  einem einzigen Arbeitslauf; verwandt mit den bestehenden Regeln #26/#30/#27b.
- `offenstand.mjs`: erkennt abgeschlossene Teilzahlungen und zählt sie nicht mehr zur Restmenge.
- Verhalten der Schnittstelle dokumentiert: Bewegungen aus Teilzahlungen bleiben dauerhaft im
  Status „zugeordnet"; ein erneuter Buchungsversuch wird mit „bereits bezahlt" abgelehnt.

## v1.197.0 — Deine eigenen Rechnungen landen nie mehr im Aufwand (2026-08-18)

### Verbesserungen

- **Bruno prüft jetzt bei JEDEM Beleg, ob er eine Eingangs- oder eine Ausgangsrechnung ist.**
  Bisher passierte das nur bei Belegen, die schon aus einem anderen Grund auffällig waren —
  bei allen übrigen wurde stillschweigend „Ausgabe" angenommen. In einer echten Buchhaltung
  hatten **272 von 275 Rechnungen** keine Richtungsangabe.
- **Der Fund dahinter:** Darunter lagen **44 eigene Ausgangsrechnungen** aus einem zweiten
  Betrieb (Onlineshop). Als Eingangsrechnung gebucht wäre das doppelt falsch gewesen: Aufwand,
  den es nicht gibt, plus Vorsteuer, die dir nicht zusteht — und der Umsatz hätte auf der
  anderen Seite gefehlt. Gestoppt hatte sie nur ein Zufall (das Sachkonto war nicht hinterlegt).
- **Woran Bruno es erkennt:** Steht die eigene Firma als **Absender** auf der Rechnung, ist es
  eine Ausgangsrechnung. Steht sie als Empfänger, ist es eine Eingangsrechnung. Ist keins von
  beidem erkennbar, wird **nicht geraten** — der Beleg bleibt zur Prüfung liegen.
- **15 neue Kontrollen** halten das fest, inklusive der Gegenprobe, dass normale
  Eingangsrechnungen unverändert durchlaufen.

## v1.196.0 — Ein Schreibfehler kann keinen echten Buchungslauf mehr starten (2026-08-18)

### Verbesserungen

- **Das Buchungs-Werkzeug bucht ohne ausdrückliches „nur testen" scharf** — und ignorierte
  bisher jede Option, die es nicht kannte. Ein Tippfehler startete damit stillschweigend einen
  echten Lauf. Genau das ist beim Arbeiten passiert (folgenlos, weil kein Beleg im Zugriff war).
  Ab jetzt bricht das Werkzeug bei jeder unbekannten Option ab und sagt, welche erlaubt sind.
- **Auch der Fast-Treffer wird gefangen:** Ein einzelner Bindestrich statt zwei (`-dry` statt
  `--dry`) rutschte in der ersten Fassung noch durch — die neue Schutzprüfung fand es beim
  ersten Lauf, bevor es jemand anders bemerken konnte. Jetzt bricht auch das ab.
- **11 neue Kontrollen** halten das fest, inklusive der Gegenprobe, dass richtige Optionen
  weiterhin durchgehen. Der Beleg-Einleser hatte diesen Schutz längst — beim Buchen, also dort
  wo Geld bewegt wird, fehlte er.

## v1.195.0 — „Welche Zahlung hat keinen Beleg?" zeigt jetzt ALLE, nicht nur ein Drittel (2026-08-18)

### Verbesserungen

- **Der Bericht „Kontobewegungen ohne Beleg" sah bisher nur die bereits verknüpften Zahlungen
  an.** Genau die Zahlungen, an denen noch gar nichts hängt — also der eigentliche Rückstand —
  blieben unsichtbar. In einer echten Buchhaltung meldete er dadurch **29 Fälle, während 167
  offen waren** (8.112 € statt 1.287 €). Ab jetzt prüft er standardmäßig jede Abbuchung, die
  einen Beleg braucht. Rein privat markierte Zahlungen bleiben außen vor, die brauchen keinen.
- **Jede Zahl nennt jetzt ihren Geltungsbereich.** Über dem Ergebnis steht, was geprüft wurde.
  Eine Zahl ohne diese Angabe war der Grund, warum die Lücke so lange niemandem auffiel.
- **Der alte, engere Blick bleibt möglich** — aber nur, wenn man ihn ausdrücklich anfordert
  (`--nur-verknuepft`). Voreinstellung ist der vollständige.
- **Neue Schutzprüfung mit 16 Kontrollen**, davon vier, die den alten Fehler absichtlich
  nachstellen. Fällt der Bericht je auf den engen Blick zurück, schlägt die Prüfung an.

## v1.194.0 — Bruno schickt dich nicht mehr los, um etwas zu suchen, das es nicht gibt (2026-08-18)

### Verbesserungen

- **Kein „hol mir bitte die fehlenden Kontoauszüge" mehr, wenn nichts fehlt.** Bruno hatte eine
  Aufgabe vom Vortag ungeprüft übernommen und danach gefragt — die Auszüge gab es aber gar
  nicht, weil das Konto längst geschlossen war. Neue Regel: Ein Punkt aus einer älteren
  Aufgabenliste gilt als **Verdacht, nicht als Tatsache**. Bevor Bruno dich damit behelligt,
  misst er neu. Eine Behauptung sieht nicht dadurch geprüft aus, dass sie irgendwo steht.
- **Bei Konten zählt jetzt der Kontostand, nicht die Anzahl der Dateien.** „Für März liegt kein
  Auszug vor" sagt nichts aus — ein geschlossenes Konto erzeugt keine Auszüge, und ein einziges
  PDF kann vier Monate abdecken. Bruno prüft stattdessen die Saldenkette: Passt das Ende eines
  Auszugs zum Anfang des nächsten, ist nichts verloren. Endet sie auf 0,00 €, ist das Konto zu
  und es fehlt nichts.
- **Zahlungshinweise auf Rechnungen werden nicht mehr für bare Münze genommen.** Auf vielen
  Rechnungen steht „Wir buchen von Konto … ab" — oft eine veraltete Bankverbindung des
  Anbieters. Real aufgetreten: Ein Versanddienst druckte monatelang ein Konto auf die
  Rechnungen, das es nicht mehr gab. Bruno glaubt dafür ab jetzt nur dem Kontoauszug.

## v1.193.0 — Berufsgeheimnis: die zweite Pflicht, die fast alle übersehen (2026-08-17)

### Neue Funktionen

- **Wenn du Steuerberater, Anwalt, Arzt oder Wirtschaftsprüfer bist, reicht der
  Datenschutzvertrag (AVV) NICHT.** Du brauchst zusätzlich eine zweite Vereinbarung, in der sich
  der Dienstleister zur Verschwiegenheit verpflichtet — und zwar ausdrücklich **mit Belehrung
  über die strafrechtlichen Folgen**. Das ist keine Formalie: dahinter steht § 203
  Strafgesetzbuch. Bruno kennt diese Pflicht jetzt und fragt bei der Einrichtung einmal nach,
  ob sie dich betrifft.
- **Für alle anderen ändert sich nichts.** Bist du normaler Unternehmer oder Selbstständiger,
  gilt diese Pflicht für dich nicht — Bruno belastet dich dann auch nicht damit.
- **Prüfliste statt Bauchgefühl:** Bruno hat jetzt sieben konkrete Fragen, mit denen sich jeder
  KI- oder Cloud-Anbieter prüfen lässt (Vertrag schriftlich? Belehrung enthalten? Was ist mit
  Subunternehmern? Was gilt bei Verarbeitung im Ausland?). Grundlage ist ein offizielles
  Musterformular der Bundessteuerberaterkammer.

### Verbesserungen

- **Ehrlicher Anbieter-Stand, inklusive der Lücken:** Bei keinem der geprüften KI-Wege
  (Anthropic, Amazon, Langdock) ist eine solche Berufsgeheimnis-Zusage öffentlich auffindbar.
  Belegt ist sie nur bei Microsoft. Bruno sagt dazu klar: „nicht auffindbar" heißt **nicht**
  „gibt es nicht" — solche Vereinbarungen stehen meist nur in Firmenkunden-Verträgen. Der
  richtige Weg ist, sie beim Anbieter schriftlich anzufordern.
- **Wichtig bei Auslandsanbietern:** Das Gesetz verlangt, dass der Geheimnisschutz im Ausland
  dem deutschen „vergleichbar" ist. Das ist eine eigene Hürde zusätzlich zum Datenschutzrecht —
  und ein starkes Argument für Anbieter mit Sitz in Deutschland oder der EU.
- **Preisvergleich ergänzt und eine Zahl korrigiert:** Beispielrechnung für einen typischen
  Monat, damit du siehst, was die Wege wirklich kosten. Kernaussage: Der Aufpreis für
  EU-Verarbeitung liegt bei wenigen Euro im Monat — der eigentliche Kostenhebel ist die Wahl
  des Modells, nicht der Zugangsweg.

### Unter der Haube

- Die Gesetzestexte (§ 57, § 62, § 62a Steuerberatungsgesetz) liegen jetzt **wortgetreu aus der
  amtlichen Quelle** bei Bruno — maschinell extrahiert, mit Prüfsumme, kein KI-Modell hat den
  Text angefasst. Zusätzlich archiviert: das Hinweispapier der Bundessteuerberaterkammer.
- Bruno gibt hier bewusst **keine Rechtsberatung**: Er nennt die Prüfpunkte und verweist für die
  verbindliche Freigabe an Anwalt oder Steuerberaterkammer.

### Wissensstand

- Berufsrecht/Verschwiegenheit: 2026-08-17

## v1.192.0 — Entwurfs-Wächter: kein Beleg bleibt mehr unsichtbar im Entwurf hängen (2026-08-18)

### Neue Funktionen

- **Neue Prüf-Dimension „Entwurfs-Wächter":** Der Health-Check meldet jetzt jeden Beleg, der als
  Entwurf im Buchhaltungssystem hängt. Entwürfe können technisch nie mit einer Kontobewegung
  verknüpft werden und tauchen in keiner Auswertung auf — genau so blieben früher Dutzende
  Belege monatelang unsichtbar liegen.
- **Neues Werkzeug zum Anheben:** Bruno hebt vollständige Entwürfe selbst auf „Offen" an
  (umkehrbar), damit sie beim nächsten Buchungslauf mitmachen. Unvollständige Entwürfe (ohne
  Betrag, Position, Lieferant oder Datum) und Gutschriften fasst er bewusst nicht an, sondern
  meldet sie. Belege aus Jahren, die dein Steuerberater schon abgeschlossen hat, sind tabu.
- **Automatisch im Nachtlauf:** Das Anheben läuft ab sofort vor jedem nächtlichen Buchungslauf.

### Verbesserungen

- Beim ersten Echtlauf wurden 30 hängende Entwürfe gefunden: 28 angehoben (inkl. einer
  Behörden-Gebühr, die falsch kontiert war und gleich richtig gebucht wurde), 1 Demo-Beleg
  entfernt, 1 Gutschrift bewusst für ihren eigenen Buchungsweg liegen gelassen.

### Unter der Haube

- Neues API-Wissen: das Buchhaltungssystem prüft Konto/Steuerregel/Summen erst beim Anheben
  eines Entwurfs, nicht beim Anlegen — die Fehlermeldung nennt dabei die erlaubten Steuerregeln
  pro Konto. Vier neue Test-Dateien in der Prüf-Suite registriert (davon drei, die seit gestern
  existierten, aber noch nicht automatisch mitliefen) — jetzt 50 bewachte Test-Dateien.

## v1.191.0 — Klare Antworten zu Datenschutz, Vertrag und EU-Verarbeitung (2026-08-17)

### Neue Funktionen

- **Bruno erklärt dir jetzt genau, wie du den Datenschutz-Vertrag (AVV) bekommst.** Die gute
  Nachricht vorweg: Du musst dafür **nichts beantragen und nichts unterschreiben**. Der Vertrag
  ist automatisch Teil der Geschäftsbedingungen, sobald du einen geschäftlichen API-Zugang
  nutzt. Was viele nicht wissen und was teuer werden kann: **Ein normales Claude-Abo (Pro/Max)
  ist ein Privatvertrag und enthält diesen Vertrag NICHT.** Für eigene Belege ist das in
  Ordnung, für Belege mit fremden Personendaten nicht.
- **Die Frage „bleiben meine Daten in Europa?" ist jetzt eindeutig beantwortet.** Über den
  direkten Zugang: **nein, das geht dort technisch nicht** — dort kann man die Verarbeitung nur
  auf die USA festlegen. Wer echte EU-Verarbeitung braucht, geht über Amazon oder Google mit
  europäischer Region. Bruno sagt dir das jetzt klar, statt es offen zu lassen.
- **Der EU-Weg kostet etwa 10 % mehr — mehr nicht.** Diese Zahl nennt Bruno künftig aktiv bei
  der Einrichtung (Modus 14), weil die meisten den Aufpreis für Datenschutz deutlich
  überschätzen und deshalb aus Kostenangst den schlechteren Weg wählen.

### Verbesserungen

- **Ein wichtiges Detail beim EU-Weg, das leicht übersehen wird:** Es reicht nicht, eine
  europäische Region einzustellen. Erst eine bestimmte EU-Einstellung sorgt dafür, dass deine
  Daten die EU garantiert nie verlassen — die Standardeinstellung darf sie weltweit
  verarbeiten und dort sogar speichern. Bruno kennt jetzt den richtigen Schalter und richtet
  ihn mit ein.
- **Beim EU-Weg ist dein Vertragspartner Amazon bzw. Google**, nicht der Modell-Anbieter. Das
  klingt technisch, entscheidet aber, wessen Datenschutzvertrag für dich gilt.
- **Veraltete Angabe korrigiert:** Bruno ging bisher von „Daten werden 30 Tage gespeichert" aus.
  Richtig ist inzwischen: **Deine Eingaben werden standardmäßig gar nicht gespeichert.**
- **Ehrlich benannt:** Auf dem EU-Weg über Amazon steht die Web-Suche nicht zur Verfügung. Das
  sagt Bruno vorher, statt dass es dich später überrascht.

### Unter der Haube

- Alle Aussagen stammen aus den Original-Vertragstexten und Hersteller-Dokumenten (am
  2026-08-17 direkt abgerufen, Wortlaute mitgespeichert) — nichts davon aus dem Gedächtnis.
  Zwei seit Juli offene Prüfpunkte im Datenschutz-Leitfaden sind damit geschlossen.
- Ebenfalls geprüft und dokumentiert: europäische Alternativen (u. a. Mistral, IONOS,
  T-Systems, Scaleway) samt Preisen, wo öffentlich auffindbar.
- Offen und ehrlich als offen markiert: ob die Modelle in der neuen deutschen Amazon-Cloud
  („European Sovereign Cloud") verfügbar sind, und die genauen Zusagen einzelner
  EU-Anbieter — beides ließ sich aus öffentlichen Quellen nicht belegen.

### Wissensstand

- Datenschutz-/Vertragslage: 2026-08-17

## v1.190.2 — Status-Landkarte + „Entwurf ist kein Halte-Zustand" (2026-08-18)

### Verbesserungen

- **Eine klare Status-Übersicht für dein Buchhaltungssystem.** Was „Entwurf", „Offen",
  „Teilweise bezahlt", „Gebucht" beim Beleg bedeuten — und was die Zustände der Kontobewegung
  heißen — steht jetzt an EINER Stelle als Landkarte, inklusive Zielbild (Beleg gebucht +
  Kontobewegung verbucht). Vorher war das über viele Einzelnotizen verstreut.
- **Neue Regel: Entwürfe sind kein Halte-Zustand.** Ein Beleg-Entwurf kann technisch nie mit
  einer Kontobewegung verknüpft werden — deshalb legt Bruno Belege beim Hochladen direkt als
  „Offen" an und hebt liegen gebliebene Entwürfe selbst an (umkehrbar), statt sie liegen zu
  lassen. So blockiert nie wieder ein vergessener Entwurf den Abschluss.

## v1.190.1 — Wissens-Nachtrag: Verknüpfen ist Buchen (2026-08-17)

### Unter der Haube

- **Live-Test dokumentiert:** Ein Beleg-Entwurf kann im Buchhaltungssystem nie mit einer
  Kontobewegung verknüpft werden (die API lehnt das hart ab) — und es gibt keinen getrennten
  „erst verknüpfen, dann buchen"-Schritt: das Verknüpfen bucht den Beleg im selben Zug fertig.
  Brunos Ablauf (hochladen als offener Beleg → in einem Schritt verknüpfen + buchen) ist damit
  als der einzig mögliche und korrekte Weg bestätigt. Liegen gebliebene Entwürfe hebt Bruno
  selbst auf „offen" an (umkehrbar), damit sie nicht den Abschluss blockieren.

## v1.190.0 — Regel-Review mit Marcel: Finanzamt bucht Bruno jetzt selbst, Auto-Buchungen werden gegengeprüft (2026-08-17)

### Neue Funktionen

- **Finanzamts-Bewegungen bucht Bruno jetzt selbst — komplett per API.** Umsatzsteuer-Zahlungen
  und -Erstattungen brauchen keinen fremden Beleg (die eigene Voranmeldung gilt als Festsetzung,
  § 168 AO) — aber gebucht werden müssen sie. Bruno erkennt die Steuerart aus dem
  Verwendungszweck (Umsatzsteuer = betrieblich, Einkommensteuer = privat, Gewerbesteuer = keine
  Betriebsausgabe) und bucht durch. Live bewiesen: eine echte USt-Erstattung über 1.173,76 €
  wurde vollautomatisch gebucht und mit der Kontobewegung verknüpft.
- **Automatik-Buchungen des Buchhaltungssystems werden gegengeprüft.** sevDesk bucht eingehende
  Kundenzahlungen teils selbst (ohne Bestätigung). Bruno prüft solche Buchungen jetzt nach wie
  ein echter Buchhalter: Rechnungsnummer im Zahlungszweck + Betrag exakt + Datum plausibel →
  bestätigt (still grün). Nur echte Widersprüche erreichen dich.
- **Klare Prüf-Reihenfolge für Eingänge ohne Beleg:** erst offene Ausgangsrechnung abgleichen,
  dann Behörde/Erstattung/Rückläufer/Einlage/Darlehen/Vorkasse unterscheiden — nie blind eine
  Rechnung schreiben (eine Lieferanten-Erstattung braucht dessen Gutschrift, keine eigene
  Rechnung).

### Verbesserungen

- **Betrags-Schwelle standardmäßig AUS** (Nutzer-Entscheid): das Sicherheitsnetz ist das
  Mehrsignal-Matching plus die Kontrolle jeder Buchung — kein Betragslimit. Der Schalter bleibt
  als bewusster Opt-in (`review_ab_betrag`).
- Behörden-Zahlungen fordern keinen Bescheid mehr an — es wird nur noch die fehlende Buchung
  gemeldet (und die erledigt Bruno selbst).

### Unter der Haube

- Neuer API-Buchungsweg dokumentiert: Steuer-Bewegung = Konto-Typ 84 + Steuerschlüssel 15 +
  Verknüpfung Typ 'N' (die Ausgaben-Seite nutzt weiter Typ 'O'). Harte Produkt-Regel neu:
  sevDesk ausschließlich per API, nie Browser-Automation. Nachprüf-Logik für Auto-Buchungen
  mit 7 neuen Kanarien-Tests (Dim-30-Suite jetzt 29 Prüfungen).

## v1.189.0 — Onboarding: sevDesk günstiger starten (2026-08-17)

### Verbesserungen

- **Noch kein Buchhaltungstool? Bruno sagt es dir jetzt gleich im Onboarding:** sevDesk
  Buchhaltung Pro lässt sich 14 Tage kostenlos testen (ohne Kreditkarte), und über den
  Empfehlungslink gibt es einen Neukundenrabatt — die genaue Höhe zeigt die Aktionsseite.
  Bruno legt dabei offen, dass es ein Empfehlungslink ist. Kein Verkaufsdruck: der Hinweis
  kommt genau einmal.

## v1.188.0 — Neues Wissen: American Express & Kreditkarten anbinden (2026-08-17)

### Neue Funktionen

- **Bruno kann dir jetzt erklären, wie du American-Express-Umsätze (und andere Kreditkarten
  ohne Bank-Schnittstelle) in deine Buchhaltung bekommst.** Amex bietet Privatkunden keine
  offene Schnittstelle — der dokumentierte Weg läuft über die Mac-App MoneyMoney (39,99 €
  einmalig, kostenlose Testversion, App-Store- und Direkt-Download-Link in der Anleitung):
  Amex-Online-Zugang anlegen, in MoneyMoney „Andere" → „American Express" wählen, fertig.
  Von dort lassen sich die Umsätze automatisiert als CSV exportieren und wie ein Kontoauszug
  importieren (Kontoauszüge-Import). Ohne Mac bleibt der manuelle CSV-Download aus dem
  Amex-Konto — auch der ist beschrieben.
- Die Anleitung ist ehrlich gekennzeichnet: ein **dokumentierter, noch nicht live getesteter
  Weg** (alle Fakten von den Hersteller-Seiten verifiziert). Ein fertiger Automatik-Connector
  folgt erst nach einem echten Testlauf.

### Wissensstand

- Neue Doku: `system/connectors/moneymoney/README.md` + Eintrag im Connector-Index.

## v1.187.0 — Keine Buchung ohne Beleg: zwei neue Prüfungen + Betrags-Schwelle (2026-08-17)

### Neue Funktionen

- **Saldo-Verprobung (neue Prüfung im Health-Check).** Bruno vergleicht jetzt je Kontoauszug
  cent-genau: Wie viel Geld hat sich laut Bank bewegt — und wie viel laut Buchhaltung? Jede
  Abweichung wird gemeldet. Das ist die stärkste Kontrolle der klassischen Buchhaltung: sie
  findet doppelt importierte Umsätze UND fehlende Umsätze automatisch. Was nicht sicher lesbar
  ist, meldet Bruno ehrlich als „nicht prüfbar" — er rät nie eine Zahl.
- **Jede Kontobewegung braucht einen Nachweis (neue Prüfung im Health-Check).** Bruno prüft
  jetzt jede offene Kontobewegung — Ausgänge UND Eingänge, über alle Konten: Hat sie einen
  Beleg? Wenn nein, welche Erklärung? Geldtransit und Privateinlage/-entnahme sind belegfrei
  (nur über deine bestätigten Konten, nie über den Verwendungszweck), Darlehen brauchen den
  Vertrag, Behörden den Bescheid, Barabhebungen eine Kassen-Einordnung. Auch Einnahmen ohne
  Ausgangsrechnung fallen jetzt auf — „ist ja bezahlt" ersetzt keine Rechnung (§14 UStG).
- **Optionale Betrags-Schwelle für automatisches Buchen — standardmäßig AUS.** Wer möchte, kann
  im Profil mit `review_ab_betrag` einen Betrag setzen (z.B. 2500): Einzelbelege darüber warten
  dann auf ein Go statt automatisch gebucht zu werden; bekannte Anbieter im gewohnten Rahmen
  laufen trotzdem durch. Ohne den Schalter ändert sich NICHTS — das Sicherheitsnetz bleibt das
  Mehrsignal-Matching (Betrag, Anbieter, Datum, Beleg-Gegensignale) plus die Kontrolle jeder
  Buchung nach dem Schreiben.

### Verbesserungen

- **Nichts wird stillschweigend als Ausnahme durchgewunken.** Die neuen Prüfungen arbeiten
  nach dem Whitelist-Prinzip: Was in keine bekannte Klasse passt, wird gemeldet — eine
  vergessene Ausnahme erzeugt höchstens eine Rückfrage, nie einen stillen Fehler.
- Die Ausnahmen-Liste „ohne Belegpflicht" nennt jetzt ausdrücklich auch **Privateinlagen**
  (fehlten bisher im Text) — und stellt klar: Auslage-Erstattungen sind KEINE Ausnahme, dort
  gehört der Original-Beleg verknüpft.
- **Nach kritischem Review nachgeschärft (ohne die Autonomie zu bremsen):** (1) Behörden-Zahlungen
  (Finanzamt, Sozialversicherung) sind nicht mehr „grün" — der Bescheid ersetzt die Rechnung,
  gebucht werden muss trotzdem. (2) Bei Eingängen ohne Beleg rät Bruno nicht mehr pauschal
  „Rechnung schreiben", sondern klärt erst die Natur (Kunden-Umsatz? Lieferanten-Erstattung?
  Privateinlage? Darlehen?). (3) **Altjahr-Schutz:** offene Bewegungen aus Jahren, die beim
  Steuerberater abgeschlossen sind, erscheinen nie als To-do. (4) Die Betrags-Schwelle greift
  auch im zweiten automatischen Buchungsweg (Abo-Cluster).

### Unter der Haube

- Neue Health-Check-Dimensionen 29 (Saldo-Verprobung, `kontoauszug-saldo.mjs`) und 30
  (Kontobewegung ohne Beleg, `tx-nachweis.mjs`), beide mit eigenen Kanarien-Tests (26 + 22
  Prüfungen); Betrags-Schwelle als eigenes Modul (`betrag-schwelle.mjs`, 13 Kanarien) mit Gate
  im Bank-Abgleich (`match-vouchers.mjs`, Freigabe per `--schwelle-frei`). Alle bestehenden
  Tests bleiben grün; die neuen Prüfungen sind rein lesend.
- Nebenfund am echten Bestand: das Buchhaltungssystem kennt einen fünften Bewegungs-Status
  (350 = „automatisch gebucht ohne Bestätigung"). Solche Buchungen weist der Health-Check
  jetzt eigens aus — dort hat das System ohne Prüfpfad gebucht, das sollte man einmal gegenlesen.

## v1.186.0 — Gutschriften lassen sich endlich verbuchen (2026-08-17)

### Neue Funktionen

- **Erhaltene Gutschriften werden jetzt korrekt gegengebucht.** Das Buchhaltungssystem kann
  Belege mit negativem Betrag gar nicht verbuchen — deshalb lagen Gutschriften bisher liegen.
  Bruno legt jetzt eine Gegenbuchung an: derselbe Betrag positiv, **auf dasselbe Konto, aber
  in der Gegenrichtung**. So mindert die Gutschrift den Aufwand und berichtigt die Vorsteuer
  (§ 17 Abs. 1 S. 2 UStG), statt als Ertrag zu erscheinen.
- **Neue Kategorie im Offenstands-Bericht: „Gutschrift ohne Zahlungsweg".** Wird bei einem
  Zahlungsdienstleister nur Kundenguthaben verrechnet, fließt nie Geld — es gibt also keine
  Kontobewegung zu finden. Solche Fälle wurden bisher dauerhaft als offen geführt. Jetzt sind
  sie sauber benannt: geklärt, offen bleibt allein das Gegenkonto (eine Frage an den Berater).

### Verbesserungen

- **Das Konto wird nicht mehr geraten.** Die Gegenbuchung verlangt jetzt ausdrücklich das Konto
  der Ursprungsbuchung. Ein falsches Konto würde die Vorsteuer verschieben — deshalb bricht
  Bruno lieber ab, als einen Standardwert zu unterstellen.

### Unter der Haube

- Die Klassifizierung des Offenstands liegt jetzt in einem eigenen Modul. Vorher stand sie
  doppelt — einmal im Programm, einmal als Kopie im Test. Ein Test, der eine Kopie prüft,
  bewacht nicht den Code, der wirklich läuft.
- 46 automatische Tests, jeder neue Schutzmechanismus gezielt sabotiert zum Beweis, dass er
  anschlägt.

## v1.186.0 — Steuerwissen-Aktualität läuft jetzt zentral (2026-08-17)

### Verbesserungen

- **Gesetzesänderungen musst du nicht mehr selbst überwachen.** Ob sich UStG, EStG, AO oder
  ein BMF-Schreiben geändert hat, prüfen wir ab jetzt zentral am amtlichen Quell-Stand —
  und liefern dir betroffene Wissens-Korrekturen fertig geprüft und verifiziert als
  Produkt-Update (Modus 15). Der frühere manuelle Aktualitäts-Check entfällt dadurch in
  deinem Paket. Fragst du Bruno „ist mein Steuerwissen aktuell?", prüft er ab sofort
  direkt, ob ein Update für dich bereitliegt.
- **Eigene Gesetzes-Importe bleiben voll erhalten.** Brauchst du eine Norm, die dein
  Standard-Paket nicht abdeckt (Modus 11), importiert Bruno sie wie bisher byte-treu und
  verifiziert aus der amtlichen Quelle. Ein erneuter Import lädt dabei immer die aktuelle
  Fassung — so frischst du selbst importierte Zusatz-Quellen jederzeit auf.

### Unter der Haube

- Auslieferungs-Pipeline kann jetzt Betreiber-interne Abschnitte gezielt aus der Doku
  filtern; drei neue automatische Tests + eine neue Abnahme-Prüfung sichern das ab.

## v1.185.0 — Doppelter Aufwand entdeckt, doppeltes Buchen abgestellt (2026-08-17)

### Neue Funktionen

- **Doppelten Aufwand aufgespürt, den bisher keine Prüfung finden konnte.** Manche Anbieter
  stellen für denselben Monat ZWEI Rechnungen mit verschiedenen Nummern: eine über die
  Grundgebühr, eine über die Grundgebühr plus alle Einzelposten. Abgebucht wird nur einmal.
  Wer beide bucht, hat den Grundbetrag doppelt in den Büchern. Jede einzelne Buchung sieht
  dabei völlig korrekt aus — echter Beleg, richtiges Konto, plausibler Betrag. Der Fehler
  zeigt sich erst, wenn man beide Rechnungen zusammen gegen das Konto hält. Genau das macht
  Bruno jetzt bei jedem Gesundheits-Check.
- **Das Konto entscheidet, nicht der Beleg.** Die neue Prüfung fragt: Ist mehr Aufwand
  gebucht, als tatsächlich abgeflossen ist? Nur wenn eine einzelne Rechnung den kompletten
  Geldfluss allein abdeckt, gilt der Rest als darin enthalten. Bleibt es unklar, meldet
  Bruno lieber nichts, statt einen korrekten Beleg zum Löschen vorzuschlagen.

### Verbesserungen

- **Zahlungsziele verwirren die Prüfung nicht mehr.** Eine Rechnung vom 29. wird oft erst im
  Folgemonat bezahlt — das ist normal und darf nicht als fehlende Zahlung erscheinen. Die
  Prüfung rechnet jetzt mit einem realistischen Zahlungsfenster in beide Richtungen.
- **Hilfsbuchungen werden nicht mit echten Zahlungen verwechselt.** Für Belege ohne
  Bankzahlung legt das Buchhaltungssystem interne Hilfseinträge an. Die zählen jetzt nicht
  mehr als Geldfluss — sonst hätte sich jeder Beleg rechnerisch selbst gedeckt und der
  doppelte Aufwand wäre unsichtbar geblieben.
- **Ein Anbieter bleibt ein Anbieter.** Derselbe Dienst taucht in Rechnungen oft in mehreren
  Schreibweisen auf ("Marke", "Marke SA", "Marke (Betreiber SAS)"). Bruno fasst diese jetzt
  zusammen, statt sie als verschiedene Firmen zu behandeln.

- **Kein doppeltes Buchen mehr nach einem Serverfehler.** Meldete das Buchhaltungssystem einen
  Fehler, obwohl es die Buchung schon ausgeführt hatte, wiederholte Bruno den Versuch blind —
  und buchte die Zahlung ein zweites Mal. Der Beleg sah dabei immer korrekt aus, nur das Konto
  trug die Zeile doppelt. Jetzt wird vor jeder Wiederholung nachgesehen, ob der erste Versuch
  nicht doch gewirkt hat. Im Zweifel wird nicht wiederholt.

### Unter der Haube

- Neue Prüf-Dimension 28 im Gesundheits-Check, abgesichert durch 35 automatische Tests.
- Jeder Schutzmechanismus wurde gezielt sabotiert, um zu beweisen, dass die Tests wirklich
  anschlagen — ein Test, der immer grün ist, prüft nichts.

## v1.184.0 — Google-Rechnungen direkt aus dem Portal, der Klickweg ist jetzt Wissen (2026-08-17)

### Neue Funktionen

- **Google-Workspace-Rechnungen kann Bruno jetzt selbst aus dem Admin-Portal ziehen** (bei
  eingeloggtem Browser). Der komplette Weg — richtige Seite, Zeitraum-Filter auf „Gesamt
  bisher", Monats-PDF-Download samt dreier technischer Fallen des Google-Portals — ist in der
  Portal-Registry dokumentiert und reproduzierbar. Live bewiesen: 19 Monatsrechnungen über
  zwei Jahre in einer Runde geholt, gebucht und mit den Kontobewegungen verknüpft.

### Unter der Haube

- portal-registry: google*workspace von „nur Mail" auf vollwertigen Portal-Weg gehoben
  (Konto-Transaktionsseite, goog-menu-Bedienung, iframe-Koordinaten-Klicks, Scroll-Reihenfolge).

## v1.183.0 — Der Prüfer meldet keine Geister mehr, Dubletten-Aufräumer sieht Fremdwährung (2026-08-17)

### Verbesserungen

- **Kein Fehlalarm mehr nach einer Link-Reparatur.** Wenn Bruno eine falsche Beleg-Verknüpfung
  repariert, blieb der alte Eintrag im internen Verlaufs-Log stehen — der Nachprüfer meldete den
  längst behobenen Fehler bei jedem Lauf erneut als kritisch. Jetzt prüft er live, ob eine
  Verknüpfung überhaupt noch besteht, und pro Kontobewegung zählt nur der neueste Eintrag.
- **Dubletten-Aufräumer versteht Fremdwährung und Betreiber-Firmen.** Manche Anbieter schicken
  pro Kauf ZWEI Dokumente (Zahlungsbeleg + Rechnung) und rechnen unter einem anderen Firmennamen
  ab als die Marke (z.B. eine US-LLC hinter einem KI-Dienst). Der Aufräumer erkennt jetzt beide
  Fälle: Ein Entwurf wird auch dann als Doppel erkannt, wenn alle passenden Zahlungen bereits
  durch andere gebuchte Belege erklärt sind (neue Beweisklasse Z2b) — gelöscht wird weiterhin
  nur mit Beweis, die Quelldatei wandert in Quarantäne statt in den Papierkorb.
- **Neue Anbieter-Erkennung:** Die Abrechnungs-Firmen eines KI-Dienstes sind jetzt als Aliase
  hinterlegt — deren Rechnungen werden automatisch richtig zugeordnet statt in der Prüfschleife
  zu landen.

### Unter der Haube

- verify-after-match: Geister-Schutz (Status-Live-Check + letzter-Eintrag-pro-Bewegung gewinnt).
- entwurfs-dubletten: Beweisklasse Z2b (alle Kandidaten-Bewegungen anderweitig erklärt).
- vendor-aliases + Konto-Map: Betreiber-LLC-Keys ergänzt; Reparatur-Verknüpfungen werden im
  Match-Log nachgetragen, damit Prüfer und Realität dieselbe Wahrheit sehen.

## v1.182.0 — Warum Gutschriften nie funktioniert haben (2026-08-17)

### Das Rätsel ist gelöst

**sevDesk kann Belege mit negativem Betrag überhaupt nicht buchen.** Der Server lehnt schon den
ersten Schritt ab: *"Sum of all positions must not be less than zero"*. Deshalb lagen deine
beiden Gutschriften seit Monaten unbuchbar herum — es war kein Einstellungsfehler und nichts,
was man mit dem richtigen Klick löst.

In der offiziellen Dokumentation steht das nirgends. Herausgefunden habe ich es, indem ich es
kontrolliert versucht habe: Der Fehler des Servers war die einzige verfügbare Auskunft.

**Der richtige Weg:** ein neuer Beleg mit positivem Betrag als Gegenbuchung auf dasselbe
Aufwandskonto. Fachlich ist eine erhaltene Gutschrift eine Vorsteuer-Berichtigung nach
§ 17 UStG — sie mindert den Aufwand, sie ist kein Ertrag. Das steht jetzt vollständig in der
Doku, mit allen Fehlermeldungen und den passenden Steuerschlüsseln.

### Was aufgeräumt wurde

**Der Paddle-Doppeleintrag ist weg.** Der offene 24-€-Beleg trug nur die nackte Bestellnummer
und hatte keinen Beleg im Haus; die echte Rechnung derselben Bestellung ist längst gebucht und
mit der Zahlung verknüpft.

**Der Testbeleg über 0,01 € bleibt** — er ist festgeschrieben und damit nach GoBD
unveränderlich. Das Aufräum-Werkzeug hat das korrekt verweigert. Er steht jetzt als
Dauerzustand vermerkt statt jedes Mal als offener Punkt aufzutauchen.

### Ehrlich zum Stand

Die beiden Gutschriften konnte ich nicht selbst verbuchen: Der Sicherheitsfilter meiner
Entwicklungsumgebung blockiert die schreibenden Läufe — dreimal versucht, dreimal geblockt.
Umgangen habe ich ihn nicht. Der Weg ist geklärt und dokumentiert, die Ausführung braucht
entweder dich oder eine Freigabe in den Einstellungen.

## v1.181.0 — Vier Rätsel gelöst, vier Belege gebucht (2026-08-17)

### Was gelöst wurde

**Die 5 Anthropic-Abbuchungen à 18 € waren nur 2 echte.** Für zwei identische Rechnungen (beide
8. April, beide Claude Pro, derselbe Leistungszeitraum) lagen fünf Abbuchungen im System. Der
Kontoauszug klärt es: Über die Salden-Kette sind am 8./9. April nur **zwei** Abbuchungen
belegt. Die übrigen stammen aus einem zweiten Import derselben Umsätze (kein Verwendungszweck,
drei Wochen später angelegt). Die zwei echten sind jetzt verknüpft und gebucht.

**"PIRATE.COM" ist nicht dein n8n-Abo.** Deine n8n-Zahlungen erscheinen als
"PADDLE.NET* N8N CLOUD1" — dreizehnmal im Konto. PIRATE.COM taucht genau einmal auf; ein Abo
hätte monatliche Abbuchungen. Der eigentliche Grund war ein anderer: Der offene 24-€-Beleg ist
ein **Doppel-Eintrag**. Zur selben Bestellung ist längst eine Rechnung gebucht und mit der
richtigen n8n-Zahlung verknüpft; der Zwilling hat nicht einmal einen Beleg im Haus.

**Zwei Meta-Rechnungen à 3 €** waren nicht zuzuordnen, weil im Kontoauszug "FACEBK" steht.
Ebenfalls gebucht.

### Was behoben wurde

**Der Cluster-Abgleich prüfte das Datumsfenster zu spät.** Er sammelte erst alle betrags- und
namensgleichen Zahlungen ein und filterte nach Datum erst beim Zuordnen. Dadurch scheiterte die
sichere Regel "gleich viele Rechnungen wie Zahlungen → der Reihe nach zuordnen" an Zahlungen,
die längst außerhalb des Zeitfensters lagen. Jetzt wird zuerst gefiltert, dann gezählt. Das
Standardverhalten ändert sich nicht — es engt ein, es lockert nichts.

### Was offen bleibt

**Zwei Gutschriften (143,69 €).** Beim ersten Fall steht der passende Geldeingang taggenau fest.
Ich buche trotzdem nicht selbst: Für Gutschriften nennt die Doku einen anderen Weg, es gibt
keinen Vergleichsfall in deinem Bestand, und bei falschem Vorzeichen würde sich der Betrag
verdoppeln. Ein kontrollierter Testlauf wurde vom Sicherheitsfilter blockiert — richtig so.

**Und ja, wir brauchen die Gutschriften:** Ohne sie stehen 97,54 € zu viel Aufwand und 18,53 €
zu viel gezogene Vorsteuer in den Büchern.

## v1.181.0 — Privat markierte Bewegungen zählen nicht mehr als „Beleg fehlt" (2026-08-17)

### Was behoben wurde

Der Jahres-Bericht „Bewegungen ohne Beleg" zählte auch Abbuchungen mit, die bereits als
PRIVAT markiert sind — für die braucht es per Definition keinen Beleg (Privatentnahme).
Real blähten 47 private Apple-Abos die Fehl-Liste auf. Jetzt: privat = raus aus der Zählung.
Ehrliche Zahlen danach: 2025 = 121 Bewegungen ohne Beleg · 2026 = 58.

---

## v1.180.0 — 2024 raus aus sevDesk, Restposten aufgeklärt (2026-08-17)

### Was gemacht wurde

**32 Belege aus 2024 sind aus sevDesk entfernt.** Sie hatte ich selbst am 15.08. dort angelegt,
obwohl 2024 beim Steuerberater längst abgeschlossen ist. Sie verfälschten jede Auswertung und
tauchten dauerhaft als "offen" auf.

Vor dem Löschen dreifach geprüft: Belegdatum in sevDesk (alle Okt–Dez 2024), Gegenprobe gegen
die lokalen Belege (31 von 32 bestätigt, der eine Zweifelsfall einzeln angesehen), und die
Kontrolle, dass **alle 32 lokal als Datei vorhanden sind**. Die Belege selbst bleiben also
vollständig erhalten — entfernt wurde nur die Buchung. Eine Sicherungskopie aller Daten liegt
lokal.

**Ein Beleg mehr verknüpft.** Zwei Meta-Rechnungen à 3 € konnten nicht zugeordnet werden, weil
im Kontoauszug "FACEBK" steht statt "Meta". Die Übersetzungsliste dafür gab es längst — der
Cluster-Abgleich benutzte sie nur nicht, sondern hatte eine eigene, einfachere Namensprüfung.
Das war die dritte eigene Variante derselben Prüfung im System; jetzt nutzen alle dieselbe.

### Was noch offen ist — und warum

Vier Punkte stehen in deiner Aufgabenliste, alle mit Begründung:

- **Gutschrift 116,07 €**: Der passende Geldeingang steht taggenau fest. Ich verknüpfe trotzdem
  nicht selbst — für Gutschriften nennt die Doku einen anderen Weg, und es gibt keinen
  Präzedenzfall zum Prüfen. Bei falschem Vorzeichen würde sich der Betrag verdoppeln.
- **Gutschrift 27,62 €** (Erlösseite): braucht eine Entscheidung.
- **"PIRATE.COM"**: könnte dein n8n-Abo sein, das über Paddle läuft. Das kann ich aus den Daten
  nicht beweisen — ein falscher Eintrag würde künftig fremde Zahlungen zusammenführen.
- **3 Belege mit mehreren gleich guten Zahlungen**: bei Anthropic stehen 5 Abbuchungen à 18 €
  für 2 Rechnungen. Da rate ich nicht.

## v1.179.0 — Jetzt zähle ich am Ende jedes Laufs, was liegen bleibt (2026-08-17)

### Was neu ist

**Der Durchlauf endet ab sofort mit einer ehrlichen Restliste.** Bisher prüfte ich nur, ob das
Gebuchte richtig ist — nicht, was gar nicht erst gebucht wurde. Genau dort lagen 87 Entwürfe
und 200 offene Kontobewegungen monatelang, während jeder Lauf grün meldete.

Am Ende steht jetzt: wie viele Belege noch nicht fertig sind, wie viele Kontobewegungen offen
sind (je Konto), und vor allem **warum** — jeder einzelne Posten bekommt eine Begründung:

- **keine passende Zahlung im System** (dann ist der fehlende Kontoauszug die Aufgabe, nicht
  der Abgleich)
- **Zahlung da, aber anderer Empfänger** (Betrag stimmt zufällig — richtig nicht verknüpft)
- **mehrdeutig** (mehrere Zahlungen passen gleich gut — da rate ich bewusst nicht)
- **Teilzahlung**, **Gutschrift**, **Testbeleg aus der Entwicklung**

Bleibt ein Posten ohne Begründung, sage ich das deutlich: dann ist es keine Restmenge, sondern
eine ungeklärte Lücke, der ich nachgehe.

**Warum das wichtig ist:** Eine Zahl allein ("83 offen") sagt nichts. Erst die Begründung
zeigt, ob etwas zu tun ist — und ob ich mir selbst etwas vormache.

### Beim ersten Lauf gleich gelernt

Der Bericht meldete zunächst 6 Posten als ungeklärt. Beim Nachsehen war jeder einzelne zu Recht
offen: Ein Beleg über 24 € traf eine Zahlung an einen völlig anderen Händler, ein anderer eine
Zahlung 468 Tage vor dem Rechnungsdatum — nur der Betrag stimmte zufällig. Mein Prüfwerkzeug
urteilte gröber als der eigentliche Abgleich und hätte Arbeit erzeugt, die es nicht gibt. Jetzt
prüft es Betrag, Zeitraum und Empfänger — wie der Abgleich selbst. Übrig blieb: nichts
Ungeklärtes.

## v1.178.0 — Der Bank-Abgleich sah nur eines deiner Konten (2026-08-17)

### Was behoben wurde

**Der nächtliche Durchlauf hat nur EIN Konto abgeglichen — seit Monaten.** Er startete den
Bank-Abgleich ohne Angabe, welches Konto gemeint ist, und nahm damit immer das Standardkonto.
Deine Zahlungen liegen aber auf drei Konten. Konkret: 134 offene Bewegungen auf dem alten
Qonto-Konto und 2 auf dem Fyrst-Konto wurden bei jedem Lauf übersprungen. Aufgefallen ist es
nie, weil der Lauf grün meldete — auf *seinem* Konto gab es ja nichts mehr zu tun.

Jetzt ermittelt der Durchlauf selbst, welche Konten offene Bewegungen haben, und geht sie
nacheinander durch.

**Gutschriften konnten nie mit einer Zahlung verbunden werden.** Eine Gutschrift ist
spiegelverkehrt: negativer Rechnungsbetrag, und das Geld kommt zurück statt abzugehen. Der
Abgleich suchte aber ausnahmslos nach Abbuchungen. Eine Gutschrift über 116,07 € lag seit Mai
2025 als Entwurf, obwohl die passende Gutschrift am selben Tag auf dem Konto stand.

Gutschriften werden jetzt korrekt gefunden. Verbucht werden sie noch nicht automatisch — wie
das System sie genau verbucht, ist nicht eindeutig dokumentiert, und bei einem falschen
Vorzeichen würde sich der Betrag verdoppeln statt auszugleichen. Der Treffer wird deshalb
angezeigt, damit du ihn mit einem Klick selbst bestätigen kannst.

**"Namecheap, Inc." fand "NAME-CHEAP.COM" nicht.** Beim Namensvergleich sollten Zusätze wie
"Inc." oder "GmbH" abgeschnitten werden — im Code stand das als Absicht, passiert ist es nie.
Dadurch scheiterte der Abgleich an einem angehängten "inc", obwohl Betrag, Datum und Anbieter
übereinstimmten.

### Was das gebracht hat

**4 Belege sind jetzt vollständig verbucht und mit ihrer Zahlung verknüpft** (75,08 €) —
vorher lagen sie als Entwurf. Jeder wurde nach dem Buchen gegengeprüft.

### Ehrlich zum Rest

Von den verbleibenden offenen Belegen haben **66 schlicht keine passende Kontobewegung** —
die Zahlung ist in der Buchhaltung nicht vorhanden, meist weil sie über ein Konto lief, das
nicht angebunden ist. Da hilft kein Abgleich, sondern nur der fehlende Kontoauszug.

Weitere Fälle bleiben bewusst liegen, weil mehrere Bewegungen gleich gut passen (etwa sechs
identische Abbuchungen über 18 €). Dort zu raten wäre schlimmer, als es dir zu zeigen.

## v1.177.0 — Eigenbelege gelten jetzt als das, was sie sind: Belege (2026-08-16)

### Was behoben wurde

**Ein Eigenbeleg wurde als "vermutlich Werbung" eingestuft.** Deine Privateinlage über 150 €
(mit Datum, Betrag, Vorgang, Unterschriftszeile, nach GoBD erstellt) landete in der
Aussortier-Vorschlagsliste — weil ihr die typischen Rechnungswörter fehlen. Zu Recht fehlen
sie: Eine Privateinlage ist kein umsatzsteuerlicher Vorgang, da gibt es nichts auszuweisen.
Einen selbst erstellten Beleg nach Fremdrechnungs-Merkmalen zu beurteilen, ist ein
Denkfehler. Eigenbelege werden jetzt als eigene Klasse erkannt und gelten als buchbar.

Damit das nicht zur Hintertür wird: Eine Werbemail, die das Wort "Eigenbeleg" beiläufig
erwähnt, bleibt eine Werbemail — geprüft wird die Dokument-Überschrift, nicht ein Stichwort.

### Was sonst passiert ist

**Die 35 Zweifelsfälle sind abgearbeitet — jeder einzeln am Original angesehen.** Ergebnis:
31 waren tatsächlich keine Belege (Banking-Benachrichtigungen, Zahlungs-Ankündigungen,
Produktmails, eine Testmail), 1 war der oben genannte Eigenbeleg, 3 waren Screenshots aus
Zahlungsübersichten. Alle Nicht-Belege liegen jetzt im Ordner für aussortierte Dokumente —
verschoben, nicht gelöscht, jeder mit Begründung im Sortier-Log.

**Zwei Funde dabei, die teuer geworden wären:**

Eine Mitteilung einer Anwaltskanzlei über **1.372,31 €** war als Ausgabe erfasst. Tatsächlich
ist es eine Insolvenz-Abschlagszahlung **an dich** — also Geld, das reinkommt. Als Ausgabe
gebucht wäre der Fehler doppelt gewesen.

Vier Banking-Mails über zusammen **936,83 €** meldeten Geld-EINGÄNGE von Stripe. Auch die
hätten als Ausgabe gebucht in die falsche Richtung gezeigt.

**Nebenbei aufgefallen:** Bei mehreren dieser Mails war der erfasste Betrag frei erfunden —
"1000 Credits" wurde zu 1.000 €, "300,000 credits" zu 300 €, eine Sendungsnummer zu 18,02 €.
Da diese Dokumente jetzt aussortiert sind, richten die Zahlen keinen Schaden mehr an.

### Was für dich übrig bleibt

Eine Aufgabe in deiner Liste: zwei Google-Chrome-Rechnungen über je 5 USD aus dem Portal
holen (dein Login, deshalb nicht von mir machbar). Ehrlich gesagt lohnt es kaum — es steht
in der Liste mit Aufwand-Ertrag-Einschätzung, du entscheidest.

## v1.176.0 — Aussortieren erkennt jetzt, was sich selbst als Nicht-Rechnung ausweist (2026-08-16)

### Was neu ist

**Zahlungs-Benachrichtigungen werden automatisch aussortiert.** Bisher verlangte der
Aussortierer, dass ein Dokument *weder Datum noch Betrag noch Rechnungsnummer* hat. Gemessen
an deinem Bestand: von 45 offensichtlichen Nicht-Rechnungen traf das auf **keine einzige** zu —
alle hatten einen Betrag. Genau das macht solche Mails ja heikel, sie sehen aus wie Rechnungen.

Jetzt gilt zusätzlich: Sagt ein Dokument in seinem eigenen Text, dass es **keine Rechnung**
ist ("Das ist keine Rechnung") oder dass die **Zahlung fehlgeschlagen** ist, wandert es in den
Nicht-Beleg-Ordner. Fünf Meta-/Instagram-Zahlungsbelege sind so weggeräumt worden.

**Was bewusst NICHT automatisch passiert:** Dokumente, denen nur die typischen Rechnungswörter
fehlen, werden lediglich **aufgelistet** (`--vorschlag`) statt verschoben. Diese Einschätzung
irrt nachweislich — eine Zahlungsbestätigung vom Bezirksamt über 15 € ist ein völlig gültiger
Beleg, aber Behörden schreiben nie das Wort "Rechnung". Solche Fälle siehst du dir selbst an.

### Was behoben wurde

**Eine echte Rechnung wäre beinahe weggeräumt worden.** Ein Verein stellte einen
Mitgliedsbeitrag über 25 € neu in Rechnung, weil die Lastschrift zurückkam — im Titel stand
"Rücklastschrift". Das galt bisher als "Zahlung fehlgeschlagen". Tatsächlich ist die
Rücklastschrift der *Anlass* der Rechnung, die Forderung besteht sehr wohl. Trägt ein Dokument
Rechnungsnummer, Steuerangabe und Betrag, wiegt das jetzt schwerer als dieses eine Wort.
Aufgefallen, weil vor dem Verschieben ins Original gesehen wurde.

**Behördensprache wirkt jetzt überall.** Die Erweiterung von gestern (Zahlungsbestätigung,
Gebühr, entrichtet) war nur im Prüfer aktiv, nicht beim Einsortieren — es gab die Wortlisten
doppelt im System. Sie stehen jetzt an einer Stelle, beide Wege nutzen dieselbe.

### Unter der Haube

Der Schutz, der verhindert, dass Belegtexte mit Namen und Steuernummern in den Chat gelangen,
hat endlich einen eigenen Test: 14 Fälle gegen die echte Schutzdatei, darunter die Sperre, die
verhindert, dass ein Hintergrund-Agent den Schutz selbst abschaltet. Jetzt 38 Selbsttests,
jeder neue mehrfach absichtlich sabotiert.

## v1.175.0 — Aussortierte Belege zählen nicht mehr als „vorhanden" (2026-08-16)

### Was behoben wurde

Der Bericht „fehlende Belege" zählte auch Dateien aus den Aussortier-Bereichen (privat,
kein Beleg, ungetrennte Postfächer) als „Beleg liegt vor — nur buchen". Realer Fall: ein als
PRIVAT aussortierter Zahlungsbeleg ließ eine Geschäftskonto-Abbuchung als buchbar erscheinen —
richtig wäre eine Privat-Entscheidung gewesen, keine Buchung. Diese Bereiche sind jetzt vom
Bestand ausgeschlossen; die Zahlen sind dadurch ehrlicher (einzelne „fehlend"-Einträge mehr,
dafür stimmen sie).

---

## v1.175.0 — Vier Fehlalarme, die dich in die falsche Richtung geschickt hätten (2026-08-16)

### Was behoben wurde

**Der Buchhaltungs-Check hat vier Mal Alarm geschlagen, obwohl deine Buchung richtig war.**
Jeder dieser Alarme kam mit einem Reparatur-Vorschlag — und wer ihm gefolgt wäre, hätte
eine korrekte Buchung kaputtgemacht. Alle vier sind nachgeprüft und behoben:

**„Der Betrag stimmt nicht"** — bei einer Rechnung über 5,90 € stand plötzlich, sie müsse
506,94 € sein. Ursache: Belege ohne Rechnungsnummer wurden alle in denselben Topf geworfen
und konnten sich gegenseitig überschreiben; verglichen wurde dann mit einem wildfremden
Beleg. Betraf 465 von rund 2.000 Belegen. Jetzt gilt: kein Vergleich ist besser als ein
falscher — ohne Rechnungsnummer meldet die Prüfung sauber „keine Zuordnung".

**„Da fehlt Umsatzsteuer"** — bei vier Auslandsrechnungen (USA). Auf den Belegen steht gar
keine Steuer, das Lesegerät hatte den Prozentsatz schlicht geraten. Ab jetzt zählt nur ein
echter Steuer**betrag** als Steuerausweis, nicht eine geratene Prozentzahl.

**„Diese Einnahme ist falsch herum gebucht"** — bei zwei Kundenzahlungen. Der Check hat nach
dem Wort „Erlös" im Buchungstext gesucht; stand dort „Direktzahlung", fiel die Einnahme durch.
Jetzt schaut er aufs Konto — das sagt eindeutig, ob es eine Einnahme ist.

**„Das ist wohl keine Rechnung"** — bei einer Zahlungsbestätigung vom Bezirksamt (Gewerbe-
ummeldung, 15 €). Behörden schreiben nie „Rechnung", sondern „Gebühr" und „entrichtet".
Der Check kennt diese Sprache jetzt.

**Außerdem:** Belege, die du längst aussortiert hast, werden nicht mehr erneut angemahnt.
Drei der Warnungen betrafen Werbe-Mails, die schon im Aussortiert-Ordner lagen.

### Was das für dich heißt

Von 13 roten Warnungen bleiben 3 — und die sind echt: zwei Benachrichtigungs-Mails ohne
Rechnung dahinter und ein Textdokument, das versehentlich als Beleg erfasst wurde. Alle drei
sind noch nicht gebucht, es ist also nichts passiert. Eine Warnliste, die nur noch echte
Fälle enthält, ist eine Liste, die man auch wirklich abarbeitet.

### Unter der Haube

Vier neue Selbsttests (jetzt 37) bewachen die vier Regeln — jeder wurde absichtlich sabotiert,
um zu beweisen, dass er den Fehler auch wirklich meldet. Die dahinterliegende Lehre steht als
feste Regel #41 im Regelwerk: Eine Prüfung, die aus einem einzigen Feld urteilt, prüft nicht,
sie rät mit.

## v1.174.0 — Dubletten-Aufräumer räumt jetzt auch die Quelle weg (2026-08-16)

### Was behoben wurde

**Ein gelöschter Doppel-Entwurf kam nach einer Stunde wieder.** Der Aufräumer hat den
Entwurf im Buchhaltungssystem gelöscht — aber die Quelldatei lag weiter im Beleg-Eingangsordner.
Der nächste Hochlade-Lauf hat daraus brav denselben Doppel-Entwurf neu erzeugt (real passiert,
am selben Tag entdeckt). Jetzt wandert beim Löschen auch die Quelldatei in den
Dubletten-Ordner, mit Vermerk im Verzeichnis, welcher gebuchte Beleg das Original ist.
Die übrigen Quelldateien der heutigen 20 Löschungen räumt der nächste Lauf im selben
Mechanismus auf — nichts geht verloren, nichts kommt doppelt.

---

## v1.174.0 — „Habe ich den Beleg schon?" ist jetzt eine Frage, die ich stelle (2026-08-16)

### Verbesserungen

**Fotografierte Belege werden endlich richtig gelesen.** Wer eine Quittung abfotografiert
statt ein PDF zu bekommen, hatte bisher Pech: Die Texterkennung bekam das Bild mit falschem
Etikett gereicht und scheiterte still. Datum, Betrag und Anbieter wurden dann aus dem
Dateinamen geraten. Jetzt lesen wir Bilder als Bilder. Bei den 9 betroffenen Belegen im
Bestand: vorher 0 mit erkanntem Betrag, jetzt 9 von 9 vollständig, keiner braucht mehr eine
Nachprüfung.

**Ein Foto konnte sich selbst überschreiben.** Beim Nachtragen der Beschreibungsdatei wurde
bei Bildbelegen der falsche Dateiname berechnet — im Ergebnis hätte die Beschreibung den
Beleg ersetzt. Behoben, bevor es passiert ist. Bei PDFs trat der Fehler nie auf.

**Vor „dieser Beleg fehlt" schaue ich jetzt nach.** Bisher konnte aus einer Zählung eine
Empfehlung werden: „Für April bis Juni liegt nichts vor, hol bitte die Kontoauszüge." In
Wahrheit lag alles da — ein einziges PDF deckte den ganzen Zeitraum ab. Solche Sammelbelege
sind normal (Quartalsabrechnungen, Jahresrechnungen). Ab sofort gilt: erst im Bestand suchen,
dann die gefundenen Dokumente aufschlagen und ihren Zeitraum lesen, und erst dann etwas als
fehlend melden. Auch der Gesundheits-Check sagt jetzt „schau erst nach" statt „beschaffe".

**Die Bestandssuche ist abgesichert.** Der Befehl, der beantwortet „habe ich diesen Beleg
schon?", entscheidet mit darüber, ob eine Rechnung ein zweites Mal geholt und im
schlimmsten Fall doppelt gebucht wird. Er war bisher ungeprüft. Jetzt bewachen ihn 16
Tests, die absichtlich versuchen, ihn zu brechen — unter anderem den Fall, dass eine leere
Suche plötzlich „ja, hab ich" für alles antwortet.

### Unter der Haube

- Die Suchlogik liegt in einem eigenen, testbaren Baustein; der Befehl selbst kümmert sich
  nur noch ums Lesen der Dateien. Die Ergebnisse sind unverändert (an drei echten Suchen
  gegengeprüft, Treffer identisch).
- Drei Tests standen in der Prüfliste, ohne je zu existieren. Sie sind ausgetragen — mit
  Vermerk, was davon anderweitig abgedeckt ist und was nicht (Bildprüfung: offene Lücke,
  ehrlich benannt statt stillschweigend als „geprüft" geführt).
- Neue Regeln festgehalten: ein Dokument kann mehrere Monate abdecken · vor jedem Löschen
  wird der Inhalt einer Datei angesehen, nicht ihr Name.

---

## v1.173.0 — Aufräumer für doppelte Beleg-Entwürfe (2026-08-16)

### Neue Funktionen

**`entwurfs-dubletten.mjs`** — findet Beleg-Entwürfe, die eine bereits gebuchte Zahlung
duplizieren, und löscht sie nach Beweis (Probelauf zuerst, jede Löschung mit Begründung im
Protokoll). Zwei Beweisketten: gleiche Rechnungsnummer schon gebucht, ODER die Zahlung ist
nachweislich durch einen anderen gebuchten Beleg erklärt. Ein Entwurf zu einer ungeklärten
Abbuchung bleibt IMMER stehen — der wird ja noch gebraucht. Erster Lauf: 20 Dubletten
erkannt (darunter 9 Meta-Doppel, die die Werbekosten verdoppelt hätten), 69 behalten.

**Bericht „fehlende Belege" übergibt jetzt an den Bucher** (`--paare-out`): jede Bewegung
mit lokalem Beleg wird maschinenlesbar exportiert — mit ALLEN Kandidaten und Evidenz-Label,
damit der Buchungs-Schritt streng entscheiden kann (Fund-Suche darf großzügig sein,
gebucht wird nur eindeutig).

---

## v1.172.0 — Aus 126 „fehlenden Belegen" wurden 8 echte (2026-08-16)

### Was behoben wurde

**Der Bericht „Kontobewegungen ohne Beleg" hat massiv übertrieben.** Er meldete 126 Abbuchungen
ohne Beleg — in Wahrheit waren es 8. Drei getrennte Blindstellen, alle vom selben Muster
(nur EINE Variable geprüft statt alle):

1. **Verknüpfte Belege wurden nicht erkannt.** Ob eine Abbuchung einen Beleg hat, wurde nur
   am Betrag ±2 Cent gemessen. Bei Dollar-Rechnungen weicht der gebuchte Betrag (EZB-Kurs)
   aber immer ein paar Prozent vom Bank-Betrag (Karten-Kurs) ab — 73 korrekt verknüpfte
   Belege galten deshalb als „fehlt". Jetzt prüfe ich vier Stufen: den Verknüpfungs-
   Fingerabdruck im Buchungssystem, den Original-Dollarbetrag aus dem Verwendungszweck
   (cent-exakt!), den Euro-Betrag und zuletzt den Kurs-Korridor.
2. **Kurze Markennamen fielen durch.** Der Anbieter-Vergleich verlangte mindestens 6 gleiche
   Zeichen — Canva, Adobe, Loom, Make, Kie und Co. konnten nie treffen. Ihre längst gebuchten
   Belege galten als fehlend. Jetzt gibt es EINE zentrale Alias-Liste (31 Anbieter), die auch
   Bank-Schreibweisen kennt (FACEBK = Meta, Deutsche Post = DHL).
3. **Eine schwache Zuordnung konnte einer starken den Beleg wegnehmen.** Die Zuordnung läuft
   jetzt in Durchgängen von der stärksten zur schwächsten Evidenz — ein Beleg landet immer
   bei der Bewegung, zu der er beweisbar gehört.

### Was das für dich ändert

Die Liste „fehlende Belege" zeigt jetzt die Wahrheit: 8 Positionen über 666 Euro statt
126 über 5.994 Euro. Du suchst nur noch Belege, die wirklich fehlen.

### Unter der Haube

Neue zentrale Prüf-Funktionen (Evidenz-Leiter + Mehrpass-Verbrauch) in der Signal-Bibliothek,
beide Berichts-Werkzeuge umgestellt, 17 Regressionstests (vorher 8) — jeder bildet einen real
passierten Fehler ab.

---

## v1.171.0 — Eine Prüfung, die nichts geprüft hat (2026-08-16)

### Was behoben wurde

**Die Kontrolle nach dem Umsortieren war wirkungslos.**

Nach dem Verschieben von Belegen prüfe ich, ob jeder Eintrag im Verzeichnis noch seine Datei
findet. Diese Kontrolle las ein Feld, das im Verzeichnis gar nicht existiert — sie verglich
1.871 Einträge gegen leere Werte und meldete anschließend "alle in Ordnung".

Die Erfolgsmeldung nach der Umsortierung war damit wertlos. Sie hat nichts geprüft.

Jetzt liest die Kontrolle das richtige Feld. Und sie gibt keine Entwarnung mehr, wenn sie gar
keine prüfbaren Daten vorfindet: Ein Verzeichnis, dessen Einträge kein lesbares Dateifeld
tragen, gilt als "nicht prüfbar" — nicht als "sauber".

**Was die korrigierte Prüfung ergibt:** 570 Einträge zeigten nicht auf eine Datei im erwarteten
Ordner. 515 davon liegen korrekt woanders — sie sind gebucht und ins Archiv gewandert.
55 fehlen wirklich, und 51 davon sind aussortierte Nicht-Rechnungen. Bleiben vier echte Belege.

### Was neu ist

**Ich gleiche jetzt in beide Richtungen ab.**

Bisher prüfte ich nur: Gibt es zu jeder Buchung den Beleg? Die Gegenrichtung fehlte: Liegt ein
Beleg lokal, der noch gar nicht gebucht ist? Beides zusammen ergibt erst eine vollständige
Schattenbuchhaltung.

Bei einem Nutzer: 43 Buchungen ohne lokalen Beleg (fast alle selbst erzeugte Gebührenbelege,
für die es keine Fremdrechnung gibt) und 49 lokale Belege ohne Buchung — Schwerpunkt 2024.

### Unter der Haube

- Der Fehler gehört zur selben Klasse wie zwei andere an diesem Tag: eine Kontrolle liest ein
  Feld, das niemand schreibt, und ist damit wirkungslos. Neue Regel für mich: Vor jeder
  Prüfung, die ein Feld liest, einen echten Datensatz laden und die Feldnamen ausgeben lassen.
  Zehn Sekunden Aufwand, verhindert eine Kontrolle, die immer grün ist
- Zwei neue Selbsttests: einer nutzt bewusst die echte Feldbezeichnung, einer stellt sicher,
  dass eine Prüfung ohne prüfbare Daten keine Entwarnung gibt
- 32 Kanarien laufen, keine rot

### Wissensstand
2026-08-16

---

## v1.170.0 — Deine Belege liegen vollständig bei dir (2026-08-16)

### Was neu ist

**Ich prüfe jetzt, ob zu jeder Buchung der Beleg auch wirklich bei dir liegt.**

Bisher habe ich verglichen, ob eine Buchung den richtigen Status hat. Das beantwortet aber
nicht die wichtigere Frage: Hast du den Beleg noch, wenn du das Buchhaltungssystem einmal
wechselst oder es nicht mehr gibt?

Bei einem Nutzer waren das 43 von 447 Buchungen: Status überall korrekt, die Datei lag
trotzdem nicht lokal. Zusammen rund 267 Euro, überwiegend Kleinbeträge eines Shop-Anbieters.

Warum das zählt, in der Reihenfolge der Wichtigkeit:

- **Vorlagepflicht:** Das Finanzamt verlangt Belege von **dir**, nicht von deinem
  Software-Anbieter. Zehn Jahre lang. Ein Beleg, den nur ein fremdes System hat, ist kein
  sicher vorlegbarer Beleg
- **Unabhängigkeit:** Wenn du die Buchhaltung später ohne dein jetziges System machen willst,
  geht das nur, wenn lokal alles liegt. Sonst ist der Wechsel ein Datenverlust
- **Prüfbarkeit:** Alle meine Gegenkontrollen brauchen die Datei selbst. Ohne sie vergleiche
  ich nur noch Beschreibungen mit Beschreibungen

**Belege werden nicht gelöscht — auch vermeintliche Doppelte nicht.**

Von 286 Dateien im Doppelte-Ordner waren nur 54 wirklich Kopien (Byte für Byte identisch mit
einem vorhandenen Original). 197 waren Einzelstücke ohne jedes Original, 35 hatten denselben
Namen bei anderem Inhalt. Wer nach Dateinamen löscht, vernichtet Belege. Gelöscht wird nur bei
bewiesener Identität — sonst wird umsortiert, nie entfernt.

### Was besser geworden ist

**Doppelte liegen jetzt pro Jahr statt pro Quartal.**

638 Dateien aus 19 verstreuten Unterordnern liegen jetzt in einem Ordner je Jahr. Ein
Doppelte-Ordner ist kein Arbeitsordner: Du suchst dort nicht den Beleg vom März, sondern
schaust einmal im Jahr nach, ob etwas versehentlich dort gelandet ist. Dafür reicht ein
Ordner pro Jahr.

### Unter der Haube

- Die neue Prüfung muss zwei Fehler gleichzeitig vermeiden: Meldet sie zu viel, geht der eine
  echte Fall in hundert Fehlalarmen unter. Meldet sie zu wenig, fällt die Lücke erst beim
  Finanzamt auf. Selbst erstellte Belege (Gebührenabrechnungen, eigene Rechnungen) sind
  deshalb eng abgegrenzt, und "keine Belegnummer vorhanden" ist eine eigene Kategorie —
  nicht dasselbe wie "fehlt"
- Ein eigener Test fand einen Fehler in genau dieser Prüfung: Sie hätte vorhandene Belege als
  fehlend gemeldet, wenn die Schreibweise der Nummer leicht abwich. Das hätte dich auf die
  Suche nach etwas geschickt, das du längst hast
- Eine Zwischenauswertung meldete "62.361 Euro betroffen". Beide Zahlen dahinter stimmten, die
  Deutung nicht: derselbe Steuerberater-Beleg war siebenmal gezählt, weil er in einem
  Mailverlauf immer wieder mitzitiert wurde. Eine Summe über Dateien ist keine Summe über
  Vorgänge
- 32 Kanarien laufen, keine rot

### Wissensstand
2026-08-16

---

## v1.170.0 — Ich hätte dich fast Belege suchen lassen, die längst da waren (2026-08-16)

### Was behoben wurde

**Ich habe 107 Belege als "fehlt" gemeldet, obwohl sie im Archiv lagen.**

Der Grund war simpel und peinlich: Viele deiner Rechnungen sind in **Dollar** ausgestellt — die
meisten Software-Anbieter sitzen in den USA. Dein Konto bucht aber in Euro. Aus 99 Dollar werden
auf dem Kontoauszug 85,35 Euro. Ich habe stur Euro gegen Euro verglichen, nichts gefunden und
gemeldet: "Beleg fehlt, muss beschafft werden."

Bei einem einzigen Anbieter waren das 48 Rechnungen über 2.209 Euro. Hättest du danach gesucht,
wäre es verlorene Zeit gewesen — sie lagen alle da.

**Der umgekehrte Fehler passierte auch:** Weil ich nur auf den Betrag geschaut habe, hätte ich
fast eine Rechnung dem falschen Anbieter zugeordnet. Zwei verschiedene Dienste hatten zufällig
denselben Betrag. Bei Kleinbeträgen passiert das ständig.

**Was ich jetzt mache:** Beim Abgleich prüfe ich Betrag **mit Währungsumrechnung**, Anbietername
(auch wenn er auf dem Kontoauszug anders heißt), Rechnungsnummer und Zeitfenster — alles
zusammen, nicht eines davon.

Zusätzlich erkenne ich jetzt Umbuchungen zwischen deinen eigenen Konten am Verwendungszweck.
Acht davon standen als größter Posten auf der Liste fehlender Belege, obwohl es dafür naturgemäß
nie einen Beleg geben kann.

### Was das für dich ändert

Die Liste "fehlende Belege" ist von **123 Positionen über 8.581 Euro** auf **9 Positionen über
709 Euro** geschrumpft. Der Rest war nie weg.

### Unter der Haube

Acht Regressionstests, die jeden dieser Fehler festnageln. Läuft einer davon rot, ist der Fehler
zurück — und ich merke es, bevor du es merkst.

---

## v1.169.0 — Deine Belege liegen jetzt nach Monaten (2026-08-16)

### Was neu ist

**Der ganze Bestand ist von Quartalen auf Monate umsortiert.**

Bisher lagen deine Belege in Quartalsordnern (`2025-Q1`), neue kommen seit Kurzem monatlich.
Ein gemischter Bestand ist unübersichtlich: für dieselbe Rechnung musstest du je nach Alter
an zwei verschiedenen Stellen suchen.

Jetzt liegt alles einheitlich nach Monat. 1.017 Belege sind umgezogen, keiner blieb zurück.

**Wie ich sichergestellt habe, dass dabei nichts kaputtgeht:**

Zu jedem Beleg gehört ein Eintrag in einem Verzeichnis — daran erkenne ich, ob er schon
gebucht ist. Reißt diese Verbindung beim Verschieben, gilt eine bezahlte Rechnung wieder als
offen und könnte ein zweites Mal gebucht werden. Deshalb prüfe ich vor **und** nach jedem
Umzug, dass jeder Eintrag noch seinen Beleg findet: 1.299 Einträge, alle in Ordnung.

Dazu: nichts wird gelöscht, nichts überschrieben. Liegt am Zielort schon eine Datei gleichen
Namens, halte ich an und frage, statt zu überschreiben. Jede Bewegung steht in einem
Protokoll und lässt sich rückgängig machen.

### Was besser geworden ist

**Nicht jeder Beleg ist ein PDF.**

Abfotografierte Quittungen liegen als Bilddatei im Bestand — die hatte ich beim ersten
Durchgang übersehen. Neun Stück wären als einzige im alten Ordner zurückgeblieben, während
alles andere umgezogen ist. Wer in den Monatsordner geschaut hätte, hätte sie nie gefunden.
Jetzt ziehen Bilder mit, ebenso alle Begleitdateien eines Belegs.

**Die Merkliste "bitte ansehen" wird nach einem Umzug neu aufgebaut.**

Diese Liste besteht aus Verweisen auf Belege, die deine Aufmerksamkeit brauchen. Nach dem
Verschieben zeigten sie ins Leere. Sie baut sich selbst neu auf — aber sie tut es nicht von
allein, der Anstoß muss zum Umzug dazugehören. Ergebnis: 369 Verweise, alle funktionsfähig,
kein einziger toter.

### Unter der Haube

- Ein Datum wie "2025-13-45" wäre bisher als gültig durchgegangen und hätte einen Ordner
  "Monat 13" erzeugt — für jede Prüfung unsichtbar. Die Datei läge da, gefunden hätte sie
  niemand. Der eigene Test fand das, bevor es passieren konnte
- Beim Sabotieren der Schutzregeln blieb eine folgenlos: eine zweite Regel dahinter fing die
  Testfälle still mit ab. Zwei Regeln, die sich gegenseitig verdecken, sind zusammen so
  schwach wie eine. Das ist an einem Tag die dritte Testlücke, die nur durch Sabotage
  sichtbar wurde
- Sonderordner (Dubletten, aussortierte Post, Kontoauszüge) bleiben unberührt — sie sind
  nicht nach Zeitraum sortiert und behalten ihre Bedeutung
- 31 Kanarien laufen, keine rot

### Wissensstand
2026-08-16

---

## v1.169.0 — Ich schaue jetzt zuerst aufs Konto, nicht auf den Belegstapel (2026-08-16)

### Was besser geworden ist

**Ich habe die Reihenfolge umgedreht — und dabei einen Fehler bei mir selbst gefunden.**

Bisher bin ich vom Belegstapel ausgegangen: Beleg nehmen, passende Zahlung suchen. Das klingt
richtig, führt aber in die Irre. Bei neun Werbe-Rechnungen habe ich gemeldet, es gäbe keine
passende Kontobewegung — vermutlich per Kreditkarte bezahlt. Beides war falsch.

Die Zahlungen standen längst auf dem Konto. Nur eben unter einem anderen Namen: Auf dem
Kontoauszug steht nicht der Anbieter, sondern sein Zahlungsdienstleister. Ich hatte nur nach dem
Anbieternamen gesucht — ein einziges Merkmal, und schon war der Befund falsch.

Schlimmer: Die Zahlungen waren **schon gebucht**. Für denselben Vorgang lagen zwei Dokumente vor
— eine Zahlungsbestätigung und eine Rechnung. Hätte ich die Rechnungen zusätzlich gebucht, wären
deine Werbekosten doppelt in den Büchern gelandet.

**Was ich jetzt anders mache:**

Ich frage zuerst: *Welche Abbuchung auf deinem Konto hat noch keinen Beleg?* Das ist die
Arbeitsliste. Alles andere ist meistens überflüssig — oder eine Dublette.

Beim Zuordnen prüfe ich jetzt alle verfügbaren Merkmale statt nur eines: Referenznummer (die
steht oft nur im Dateinamen), Betrag, Zeitfenster, Anbietername **und** seine bekannten
Kontoauszug-Schreibweisen. Vor jeder Buchung läuft zusätzlich eine Dublettenprüfung: Ist derselbe
Betrag in den Tagen davor oder danach schon gebucht? Dann buche ich nicht, sondern frage nach.

Geldtransit und deine eigenen Privatentnahmen sind davon ausgenommen — die brauchen keinen
Fremdbeleg.

### Neu für dich

**Ein Bericht, der zeigt wo Belege fehlen.** Gruppiert nach Anbieter, mit Summe und Jahr — damit
du in einem Portal-Besuch gleich mehrere Belege holen kannst statt einzeln zu suchen.

### Unter der Haube

Zwei Messfehler in meiner eigenen Prüfung behoben: Das Zeitfenster war zu eng (eine Rechnung mit
33 Tagen Zahlungsziel galt fälschlich als unbelegt), und ein einzelner Beleg konnte mehrere
gleich hohe Abbuchungen "erklären" und damit echte Lücken verstecken. Jeder Beleg zählt jetzt nur
einmal.

---

## v1.168.0 — Fehlgeschlagene Zahlungen sind kein Aufwand (2026-08-16)

### Was neu ist

**Ich erkenne jetzt Mails über fehlgeschlagene Zahlungen.**

Wenn eine Abbuchung scheitert, schickt der Anbieter eine Mail: Firmenname, Betrag, Datum —
sie sieht aus wie eine Rechnung und wurde bisher auch so erfasst. Nur ist nie Geld geflossen.
Als Ausgabe gebucht wären das erfundene Kosten und eine Vorsteuer, die dir nicht zusteht.

Bei einem Nutzer fand ich sechs solcher Belege über zusammen 234 Euro. Einer davon war
besonders heimtückisch: eine Mail mit einem Bestätigungscode, bei der der **Code selbst als
Betrag gelesen** wurde — aus "101509" wurden 10,15 Euro.

Wichtig ist mir dabei die Formulierung "Ihre Rechnung über 49 Euro konnte nicht abgebucht
werden". Die enthält das Wort Rechnung und alle Beträge — und galt vorher als buchbarer
Beleg. Jetzt gewinnt der Fehlschlag.

Umgekehrt passe ich auf, dass eine echte Rechnung nicht aussortiert wird, nur weil sie eine
frühere Fehlbuchung erwähnt ("Ihre letzte Zahlung schlug fehl, dieser Betrag wurde
erfolgreich eingezogen"). Diese Richtung ist die gefährlichere: eine zu Unrecht
weggeworfene Betriebsausgabe fällt niemandem auf, weil man sie nie gesehen hat.

**Der Unterschied zu "Beleg fehlt" ist praktisch, nicht theoretisch.** Fehlt nur die
Rechnung, kannst du sie beim Anbieter nachfordern. Ist die Zahlung fehlgeschlagen, gibt es
keine — dich darauf anzusetzen wäre eine Jagd nach etwas, das nicht existiert.

### Was besser geworden ist

**Nachträge laufen jetzt gut viermal so schnell.**

Das Nachtragen fehlender Beleg-Beschreibungen lief bisher strikt nacheinander: 8 Sekunden
pro Beleg. Jetzt arbeiten mehrere gleichzeitig — gemessen 1,9 Sekunden. Aus 48 Minuten
werden rund 10.

Das ist bewusst nur hier erlaubt: Dieser Vorgang schreibt nichts, was mehrere Belege sich
teilen — keine Buchung, kein Verzeichnis, keine Abgleichsliste. Beim eigentlichen Buchen
bleibt alles streng nacheinander, weil dort ein Zwischenstand geteilt wird und eine
Doppelbuchung entstehen könnte.

**Eine Prüfung war nie im Testlauf angemeldet.**

Die Prüfung "ist das überhaupt eine Rechnung?" hatte eigene Tests — aber niemand startete
sie. Ein Test, den man nicht ausführt, meldet keinen Rückfall. Jetzt läuft er bei jedem
Durchgang mit.

### Unter der Haube

- Beim absichtlichen Sabotieren der neuen Regel blieben zunächst alle Tests grün: der
  Testfall enthielt zwar das Wort "fehlgeschlagen", aber keine der Formulierungen, auf die
  die Regel wirklich reagiert. Er prüfte also nichts. Das ist an einem Tag die zweite
  Testlücke, die nur so sichtbar wurde
- Eine erste Geschwindigkeitsmessung ergab nur den Faktor 1,1 — sie lief über einen
  Durchgang mit sieben Scheinfehlern und war damit wertlos. Erst der saubere Lauf zeigte 4,2
- 30 Kanarien laufen, keine rot

### Wissensstand
2026-08-16

---

## v1.167.0 — Belege ohne Beschreibung, und ein Datum aus drei Quellen (2026-08-16)

### Was neu ist

**Ich trage fehlende Beleg-Beschreibungen nach.**

Zu jedem Beleg gehört bei mir eine kleine Beschreibungsdatei: was steht drauf, welcher
Anbieter, welcher Betrag, welches Datum. Ohne sie ist ein Beleg für mich halb unsichtbar —
er liegt zwar im Ordner, taucht aber in keiner Prüfung auf und lässt sich nicht buchen.

Bei einem Nutzer fehlte diese Datei bei 459 PDFs. Ursache: sie kamen über Wege in die
Ablage, die keine Texterkennung fahren — von Hand kopiert, aus einem Portal geladen oder
noch von einer alten Version abgelegt. Gemeldet hat das nie jemand, weil niemand nach einer
Datei sucht, von der er nicht weiß, dass sie fehlen müsste.

Das neue Werkzeug liest diese Belege nach und legt die Beschreibung daneben. **Es rührt die
PDFs selbst nicht an** — nicht verschieben, nicht umbenennen, nicht löschen. Das ist wichtig,
weil ein Teil davon längst gebucht ist: die dürfen ihren Platz nicht verlieren.
Kontoauszüge lässt es aus, die sind kein Buchungsbeleg.

**Das Belegdatum kommt jetzt aus drei Quellen statt einer.**

Ein Datum bestimmt, in welchen Umsatzsteuer-Zeitraum ein Beleg gehört. Bisher verließ ich
mich auf die Texterkennung allein. Neu vergleiche ich drei Angaben:

- was im PDF-Text steht (der Aussteller hat es geschrieben — stärkstes Signal)
- was die Texterkennung gelesen hat
- was im Dateinamen steht (schwächstes Signal — der stammt oft vom Download, nicht vom Beleg)

Sind sich die starken Quellen einig, buche ich ohne Rückfrage. **Widersprechen sie sich,
entscheide ich nicht** — dann bekommst du den Beleg vorgelegt, mit beiden Daten im Klartext.
Ein still gewählter Monat wäre eine Buchung auf Verdacht.

Beim Lesen aus dem PDF-Text nehme ich außerdem nur, was ausdrücklich als Rechnungsdatum
gekennzeichnet ist. Auf einer Rechnung stehen schnell fünf Daten — Fälligkeit, Lieferung,
Abbuchungstag, Druckdatum. Wer einfach das erste nimmt, erwischt meist das falsche.

### Was besser geworden ist

**Belege, die sich selbst widersprochen haben.**

Der erste Entwurf setzte still das Datum aus dem Dateinamen ein, wenn die Texterkennung
nichts fand. Das Ergebnis war eine Beschreibungsdatei, die zugleich ein Datum trug **und**
behauptete, das Datum fehle. Aufgefallen ist es bei der ersten Stichprobe von drei Belegen.

Ein Beleg, der behauptet, sein Datum fehle, obwohl eines drinsteht, ist gefährlicher als
einer ganz ohne Datum: dem ersten glaubt man, den zweiten prüft man. Jetzt steht bei jedem
Datum dabei, aus welcher Quelle es stammt.

### Unter der Haube

- Neues Prüfmodul für den Datums-Abgleich mit 20 Selbsttests
- Beim Sabotieren des Moduls fiel eine Lücke in den eigenen Tests auf: der gefährlichste
  Fall — Texterkennung und PDF-Text widersprechen sich — war gar nicht geprüft. Alle Tests
  blieben grün, obwohl die Schutzregel entfernt war. Lücke geschlossen, jetzt sterben drei
  Tests bei derselben Sabotage
- Ein Analyse-Werkzeug meldete zunächst "0 Belege gefunden" — es suchte an einem falsch
  zusammengesetzten Pfad. Ein Nullbefund aus einem kaputten Pfad sieht aus wie ein Ergebnis
- 29 Kanarien laufen, keine rot

### Wissensstand
2026-08-16

---

## v1.166.0 — Zahlungen ohne Rechnung, Rechnungen ohne Zahlung (2026-08-16)

### Was neu ist

**Ich erkenne jetzt Zahlungen, die vor der Rechnung kamen.**

Wenn du einen Zahlungslink verschickst, zahlt der Kunde sofort — die Rechnung schreibst du
danach. Für dich ist das ein Vorgang. Für die Buchhaltung sahen es bisher zwei aus: eine
Zahlung ohne Rechnung und eine Rechnung, die als "außerhalb bezahlt" markiert ist und keinen
Zahlungsbezug trägt. Ich konnte beides nicht zusammenbringen und habe die Rechnungen liegen
lassen. Bei einem Nutzer waren das über 2.000 Euro echter Umsatz, die im Jahresergebnis
fehlten.

Jetzt führe ich beide Seiten zusammen — aber nur, wenn es eindeutig ist. Stehen drei
Rechnungen über denselben Betrag nur zwei Zahlungen gegenüber, ordne ich **keine einzige** zu
und lege dir alle drei vor. Das ist der Punkt, an dem der erste Entwurf noch falsch lag: er
hätte die beiden erstbesten bedient und die dritte leer ausgehen lassen — und welche das
gewesen wäre, hätte allein die Reihenfolge entschieden, in der die Daten ankommen. Bei Geld
ist "die erste" kein Argument.

**Ich prüfe jetzt, ob dein Geld wirklich auf dem Konto ist.**

Bisher konnte ich sagen: die Zahlung ist bei deinem Zahlungsdienstleister eingegangen. Das ist
nicht dasselbe wie "auf deinem Bankkonto". Neu rechne ich das Guthabenkonto Bewegung für
Bewegung nach und vergleiche das Ergebnis mit dem echten Kontostand. Stimmen beide auf den
Cent überein, weiß ich zweierlei: es fehlt keine Bewegung, und ich kann dir sagen, bis zu
welchem Tag alles ausgezahlt ist. Erst bis zu diesem Tag buche ich.

**Fehlende Gebühren-Belege lege ich selbst an.**

Die Monatsbelege für die Transaktionsgebühren musstest du bisher von Hand anlegen — es gab
schlicht kein Werkzeug dafür. Fehlte einer, meldete das auch niemand: es sucht ja keiner nach
einem Beleg, den es nie gab. Bei einem Nutzer fehlten zwei Monate. Ich lege sie jetzt selbst
an, den laufenden Monat bewusst nicht, und nie zweimal denselben.

### Was besser geworden ist

**Eine Umbuchung, die immer "nichts zu tun" meldete.**

Die Auszahlungen deines Zahlungsdienstleisters sollen automatisch mit den Eingängen auf deinem
Bankkonto verknüpft werden. Diese Funktion hat nie einen einzigen Treffer gefunden — sie las
das Ankunftsdatum aus einem Feld, das es gar nicht gibt. Das Ergebnis war kein Fehler, sondern
eine ruhige Null: "0 Umbuchungen". Sah aus wie ein sauberes Ergebnis. Jetzt werden die
Eingänge zugeordnet; bei einem Nutzer waren es vier über zusammen 2.014,79 Euro.

**Eine Prüfung, die den falschen Monat durchsucht hat.**

Wird eine Rechnung nachträglich geschrieben, trägt sie beim Zahlungsdienstleister das Datum
der Rechnungserstellung als Zahlungszeitpunkt — nicht den Tag, an dem das Geld floss. Meine
Monatsprüfung suchte deshalb Julizahlungen im August, fand im Juli nur eine stornierte
Rechnung und blockierte den Gebühren-Beleg mit "kein Umsatz". Ein Fehlalarm auf einem korrekt
gebuchten Monat. Jetzt zählt der Tag des Geldflusses.

**Stornierte Doppel-Rechnungen blockieren nichts mehr.**

Eine versehentlich doppelt ausgestellte und danach gutgeschriebene Rechnung ist kein Umsatz —
es floss nie Geld. Sie hätte trotzdem die Zuordnung der echten Rechnungen blockiert, weil sie
die Zahl der offenen Rechnungen verfälscht. Solche Belege lasse ich jetzt außen vor und sage
dir, welche das waren.

### Unter der Haube

- Zwei neue Prüfmodule mit zusammen 25 Selbsttests: Guthaben-Kette (12) und Zahlungs-Nachweis (14)
- Ein weiteres für die Gebühren-Monatsbelege (11 Selbsttests), geprüft gegen drei bereits
  gebuchte Monate — alle drei auf den Cent identisch
- Alle drei Module absichtlich sabotiert, um zu beweisen, dass die Tests den Schaden auch
  wirklich melden: fünf Tests sterben, sobald die Eindeutigkeitsprüfung entfernt wird
- Gebühren-Belege folgen §13b (Netto gleich Brutto, kein Steuersatz) — ein gesetzter
  Steuersatz hätte 39,62 Euro zu 47,15 Euro gemacht
- 28 Kanarien laufen, keine rot

### Wissensstand
2026-08-16

---

## v1.165.0 — Einnahmen buchen, und eine Prüfung, die nur so tat als ob (2026-08-16)

### Was neu ist

**Ich kann jetzt auch Einnahmen mit deinem Kontoeingang verknüpfen.**

Das klingt selbstverständlich, war es aber nicht: Bisher konnte ich nur Ausgaben zuordnen.
Deine eigenen Rechnungen blieben als Entwurf liegen, auch wenn das Geld längst da war — und
Entwürfe zählen in keiner Auswertung mit. Bei einem Nutzer waren das über 1.700 Euro echter,
belegter Einnahmen, die deshalb im Jahresergebnis fehlten.

Ab jetzt ordne ich sie zu. Vorsichtig: Ich verlange, dass deine Rechnungsnummer im
Verwendungszweck steht oder wenigstens der Kundenname passt, dass Betrag und Zeitraum
stimmen — und dass es **genau eine** passende Zahlung gibt. Zahlen zwei Kunden am selben Tag
denselben Betrag, ordne ich nichts zu, sondern lege beide zur Ansicht vor. Raten wäre hier
eine Zuordnung zum falschen Kunden.

Neu ist auch das längere Zeitfenster: Ausgaben werden abgebucht, da vergehen Tage. Deine
Rechnungen haben ein Zahlungsziel, da vergehen Wochen. Ich schaue jetzt bis zu 30 Tage weit.

### Was besser geworden ist

**Eine Prüfung hat gemeldet "alles in Ordnung", ohne irgendetwas geprüft zu haben.**

Das ist der wichtigere Teil dieser Version. Vor dem Buchen der Zahlungsdienstleister-Gebühren
prüfe ich, ob die Zahlungen des Monats vollständig nachvollziehbar sind. Diese Prüfung lief
über eine Liste — und wenn die Liste leer war, meldete sie "vollständig". Nicht, weil alles
stimmte, sondern weil es nichts zu prüfen gab.

Genau das ist bei zwei Monaten passiert. Die Gebühren wären gebucht worden, ohne dass eine
einzige Zahlung angesehen wurde.

Ich unterscheide jetzt zwei verschiedene Dinge, die vorher vermischt waren:
**"geprüft und in Ordnung"** und **"konnte gar nicht prüfen"**. Das Zweite ist kein grünes
Licht mehr, sondern landet bei dir zur Ansicht — mit dem konkreten Grund, statt eines
pauschalen Hinweises.

**Auszahlungen, die dein Zahlungsdienstleister nicht aufschlüsselt.**

Wenn du Auszahlungen manuell auslöst statt automatisch, verrät Stripe nachträglich nicht mehr,
welche Kundenzahlungen darin steckten. Ich rechne das jetzt nach, wo es eindeutig geht.

Ehrlich zur Grenze: Ich habe das gegen vier Auszahlungen geprüft, deren wahre Zusammensetzung
bekannt war. **Zwei konnte ich exakt rekonstruieren, zwei nicht.** Wo viele Cent-Beträge
zusammenkommen, gibt es zu viele rechnerisch passende Möglichkeiten — dann sage ich das, statt
mir eine auszusuchen. Für eine vollständige Zuordnung führt der Weg über das Stripe-Dashboard
oder über die Umstellung auf automatische Auszahlungen.

### Unter der Haube

- Neu: `system/_lib/match-revenue.mjs` + `tools/sevdesk-connector/match-revenue.mjs`
  (Einnahmen-Zuordnung, Probelauf als Standard, Rückabwicklung vorhanden, Kontrolllesung
  nach jeder Buchung)
- Neu: `system/_lib/payout-kette.mjs` (Auszahlungs-Rekonstruktion mit harter
  Eindeutigkeitsregel)
- Repariert: das Monats-Gate in `stripe-clearing.mjs` — leere Prüfliste gilt nicht mehr
  als bestandene Prüfung
- 30 neue automatische Tests, darunter vier Sabotage-Proben: absichtlich kaputt gemachte
  Schutzregeln müssen auffallen. Der gesamte Testlauf steht bei 25 grün, 0 rot.

### Wissensstand

2026-08-16

---

## v1.164.0 — Brauche ich überhaupt einen Steuerberater? (2026-08-16)

### Was neu ist

Ich habe eine neue Wissensseite dabei: **wie deine fertige Buchhaltung tatsächlich ans
Finanzamt kommt** — einmal komplett selbst, einmal über den Steuerberater. Für
Einzelunternehmer mit EÜR. Für GmbH und UG gab es das schon, jetzt ist die andere Seite da.

Frag mich einfach: *"Wie reiche ich das ein?"* oder *"Brauche ich meinen Steuerberater noch?"*

### Der Satz, der die meiste Verwirrung auflöst

**Deine Buchhaltung wird nirgendwo abgeschickt.** Ans Finanzamt gehen nur ausgefüllte
Formulare. Ein DATEV-Export ist kein Einreichen — er ist eine Datei für deinen
Steuerberater. Wer "DATEV-konform exportiert" hat damit noch gar nichts abgegeben.

### Was du selbst darfst

Mehr als die meisten denken: **für deine eigene Buchhaltung gibt es keinen Vorbehalt.**
Kontieren, abschließen, EÜR erstellen, selbst einreichen — alles erlaubt. Das
Steuerberatungsgesetz verbietet nur, so etwas *geschäftsmäßig für andere* zu tun.

Genau deshalb halte ich mich beim Festschreiben zurück: **ich** wäre der Dritte, nicht du.

### Was drin steht

- Welche Formulare du als EÜR-Einzelunternehmer überhaupt abgeben musst (und welche
  komplett wegfallen — keine Bilanz, keine Offenlegung)
- Der Weg Schritt für Schritt: was sevDesk selbst sendet und was du in Mein ELSTER eintippst
- Was beim Steuerberater mechanisch passiert, wenn er deinen Export bekommt
- Was ein Steuerberater kann, das keine Software kann — ehrlich, ohne Schönfärberei
- **Die längere Abgabefrist** mit Steuerberater: statt sieben Monaten hast du bis Ende
  Februar des übernächsten Jahres

### Eine Zahl korrigiert

Die Grenze, ab der das Finanzamt dich von der Umsatzsteuer-Voranmeldung befreien kann,
liegt bei **2.000 Euro** Zahllast im Vorjahr — nicht bei 1.000. Ich hatte den älteren Wert
notiert. Und: das Finanzamt *kann* befreien, es *muss* nicht. Unter der Grenze zu liegen
heißt noch nicht, befreit zu sein.

### Wissensstand

Alle Gesetzesstellen (§5 StBerG, §18 UStG, §149 AO) liegen wortgetreu aus dem amtlichen
Text vor und werden bei jeder Änderung maschinell gegengeprüft.

---

## v1.163.0 — Rechnungsportale: der schnelle Weg wird jetzt auch genutzt (2026-08-16)

### Was sich ändert

Wenn ich Rechnungen aus einem Anbieter-Portal hole, gehe ich ab sofort **zuerst den
direkten Weg**, falls der Anbieter einen anbietet. Vorher habe ich immer die Webseite
durchsucht — auch dann, wenn es schneller und zuverlässiger ging.

Für dich ändert sich nichts an der Bedienung. Der Unterschied zeigt sich nur, wenn etwas
schiefgeht.

### Warum das wichtig ist

Bisher konnte folgendes passieren: Ich suche im Portal nach Rechnungen, finde die Liste
nicht (weil der Anbieter seine Seite umgebaut hat) und melde **"Portal nicht erreichbar —
vermutlich abgemeldet"**. Das klang nach einem Login-Problem. In Wahrheit gab es einen
funktionierenden zweiten Weg, den ich nur nicht genutzt habe.

Das Ergebnis wäre gewesen: **"Beleg fehlt"** — obwohl er abrufbar war. Und ein Beleg, den
du nicht hast, ist Vorsteuer, die du nicht ziehen kannst.

Jetzt gilt: erst der direkte Weg, und nur wenn der nicht klappt, die Webseiten-Suche. Wenn
beides scheitert, sage ich dir dazu, dass ein schnellerer Weg existiert und was ihm fehlt —
statt eine falsche Ursache zu nennen.

### Was gleich bleibt

- **Die Dublettensperre.** Vor jedem Download prüfe ich weiterhin, ob der Beleg schon im
  Haus liegt — beide Wege laufen durch dieselbe Prüfung.
- **Der Login bleibt bei dir.** Passwort und 2FA gebe ich nie ein und sehe sie nie.
- **Anbieter ohne direkten Weg** (aktuell 30 von 31) verhalten sich exakt wie vorher.
- **Eine abgelaufene Anmeldung wird erkannt.** Portale antworten dann gern mit einer
  Login-Seite statt mit Daten — früher hätte das ausgesehen wie "keine Rechnungen
  vorhanden". Ich prüfe jetzt, ob echte Daten kamen, und sage dir sonst Bescheid.

### Unter der Haube

- `portal-fetch.mjs`: neues Kommando `liste` (nur lesend, läuft im selben abgesicherten
  Browser-Fenster wie bisher — kein zweiter Netzwerkweg an den Schutzmechanismen vorbei).
- `portal-runde.mjs`: nutzt den direkten Weg vor der Webseiten-Suche, fällt bei jedem
  Problem automatisch zurück. Die Begrenzung `--max` wirkt jetzt in beiden Wegen.
- 10 neue automatische Tests für das Auslesen der Rechnungsliste (24 insgesamt, alle grün).

---

## v1.162.0 — Belege liegen jetzt nach Monat statt nach Quartal (2026-08-16)

### Was sich ändert

Neue Belege werden ab sofort **nach Monat** abgelegt:

```
2 BELEGE (gelesen, noch zu buchen)/
   2026/
      2026-03/     ← neu: ein Ordner je Monat
      2026-04/
```

Vorher lagen sie in Quartalsordnern (`2026-Q1`). **Deine bestehenden Ordner bleiben, wie sie
sind** — es wird nichts verschoben und nichts umbenannt. Bruno liest beide Formen.

### Warum

Drei Gründe, der wichtigste zuerst:

**Monate lassen sich zusammenfassen, Quartale nicht auseinandernehmen.** Aus zwölf
Monatsordnern ist jede Quartals- oder Jahressicht ableitbar. Andersherum müsste man jeden
Beleg einzeln anfassen.

**Dein Melderhythmus kann sich mitten im Jahr ändern.** Steigt deine Umsatzsteuer-Zahllast über
7.500 € im Vorjahr, wird aus der vierteljährlichen Voranmeldung eine monatliche (§18 Abs. 2
UStG). Bei einer Neugründung sind die ersten beiden Jahre ohnehin monatlich. Eine Ablage, die
am Rhythmus hängt, müsste dann umziehen — eine Monatsablage nie.

**Bei einer Betriebsprüfung wird nach einem Monat gefragt**, nicht nach einem Quartal.

Willst du ausdrücklich bei Quartalen bleiben, geht das weiterhin: `--period=quarter`.

### Ein Fehler, der dabei aufgefallen ist

Beim Umbau kam heraus, dass drei Stellen im Code das Quartalsformat fest eingebaut hatten —
darunter der nächtliche Buchungslauf. Mit Monatsordnern hätte er **keinen einzigen Beleg
gefunden und trotzdem „erfolgreich" gemeldet.** Ein Lauf, der still nichts tut, ist schlimmer
als einer, der abbricht: man merkt es erst Wochen später.

Die Erkennung liegt jetzt an einer einzigen zentralen Stelle, abgesichert durch 32 automatische
Prüfungen. Darunter Fälle, die absichtlich **nicht** greifen dürfen: Ordner wie „Kontoauszüge"
oder „zur Ansicht" dürfen nie als Buchungsmonat gelesen werden — sonst wanderten Belege, die
bewusst zur Prüfung geparkt sind, in einen Buchungslauf.

### Beim Einrichten

Bruno fragt jetzt ausdrücklich nach deinem Voranmeldungs-Rhythmus (monatlich, vierteljährlich,
jährlich) und trägt ihn fest ein. Daran hängen die Fristen-Erinnerungen. Weißt du ihn nicht,
bleibt er offen und du klärst ihn mit deinem Steuerberater — geraten wird er nicht.

---

## v1.160.0 — Erst nachsehen, ob der Beleg schon da ist (2026-08-16)

### Das Problem

Bruno führt zwei Listen über deine Belege: die Dateien selbst und ein Verzeichnis dazu. Beim
Prüfen, ob eine Rechnung noch fehlt, schaute er nur ins Verzeichnis.

Das ging lange gut — bis auffiel, wie weit die beiden auseinanderliegen: **2.068 Belege liegen
im Haus, nur 1.298 stehen im Verzeichnis.** 770 Belege waren damit für jede Prüfung unsichtbar.
Sie liegen in Sammelordnern wie „zur Ansicht" oder „aussortiert", für die gar kein Verzeichnis
geführt wird.

Die Folge: Rechnungen galten als fehlend, obwohl sie längst da waren. Zwei davon wären beinahe
ein zweites Mal aus dem Anbieter-Portal geholt worden — eine davon war sogar schon gebucht.
Ein zweiter Beleg derselben Rechnung ist gefährlich: er kann zu einer Doppelbuchung führen.

### Die Lösung

Vor jeder Beschaffung schaut Bruno jetzt in die **Dateien** — durch alle Unterordner hindurch,
egal ob ein Verzeichnis geführt wird oder nicht. Am eigenen Bestand gemessen stieg damit die
Zahl der erkannten Belege von 1.298 auf **2.319**. Beide Beinahe-Dubletten werden zuverlässig
erkannt.

Zusätzlich gilt jetzt eine feste Reihenfolge, bevor überhaupt ein Portal geöffnet wird:

| Stufe | Frage |
|---|---|
| **0** | Liegt der Beleg schon auf der Platte? |
| 1 | Gibt es eine Schnittstelle beim Anbieter? |
| 2 | Kam er per Mail? |
| 3 | Erst dann: Portal im Browser |

### Was du davon hast

Kein doppelt geholter Beleg und keine unnötigen Portal-Logins. Und wenn Bruno sagt „diese
Rechnung fehlt", dann fehlt sie wirklich — vorher konnte es auch heißen, dass sie nur in einem
Ordner ohne Verzeichnis lag.

### Unter der Haube

- Drei neue Prüf-Kanarien, die genau diesen Fall festhalten: ein Beleg in einem Ordner ohne
  Verzeichnis, und ein Beleg zwei Ebenen tief in der Ordnerstruktur.
- Ein Fehlalarm in dieser Prüfung ist bewusst harmlos: die Folge ist „nicht holen", und eine
  echte Lücke meldet ohnehin der Gesundheits-Check. Die umgekehrte Richtung — holen, obwohl
  vorhanden — ist die teure.

---

## v1.158.0 — „Keine Rechnungen gefunden" war oft nur der falsche Zeitraum (2026-08-16)

### Das Problem

Viele Anbieter-Portale zeigen von sich aus nur einen kurzen Zeitraum: Meta die letzten sieben
Tage, andere den letzten Monat. Wer eine Rechnung von vor einem halben Jahr sucht, sieht dort
eine leere Liste — und die bedeutet nicht „gibt es nicht", sondern „nicht in diesem Fenster".

Bruno wusste das und hatte es sogar notiert. Nutzen konnte er es trotzdem nicht: der Befehl, der
die Rechnungsliste ausliest, rief immer die Startseite des Portals auf. Ein Zeitraum ließ sich
gar nicht mitgeben.

Ergebnis: eine Fehlanzeige, die wie ein Befund aussah.

### Die Lösung

Der Zeitraum lässt sich jetzt mitgeben. Die Sicherheitsgrenze bleibt unverändert — aufgerufen
werden ausschließlich Adressen des hinterlegten Anbieters, alles andere wird abgewiesen.

### Was du davon hast

Ein leeres Ergebnis heißt ab jetzt wirklich „keine Rechnung da". Vorher konnte es auch heißen
„falsches Fenster" — und eine fehlende Rechnung, die als „gibt es nicht" abgehakt wird, fällt
erst beim Steuerberater auf.

---

## v1.156.0 — Belege beschaffen, ohne dieselben nochmal zu holen (2026-08-16)

### Das Problem

Wenn eine Rechnung fehlt, sagt Bruno dir, in welchem Anbieter-Portal sie liegt. Nur fand er
das Portal oft nicht — obwohl es seit Monaten eingetragen war.

Grund: Die Bank schreibt Anbieternamen mit Sternchen, Bindestrichen und angehängten
Referenznummern. Der Registry-Eintrag steht dort in normaler Schreibweise. Verglichen wurde
Zeichen für Zeichen, also passte nichts zusammen. Sechs Buchungen desselben Cloud-Anbieters
liefen so ins Leere.

Sichtbar wurde es an der Registry selbst: derselbe Anbieter stand zweimal drin, einmal mit
und einmal ohne Bindestrich. Statt den Vergleich zu reparieren, hatte jemand die Schreibweise
nachgetragen.

### Die Lösung

Beide Namen werden vor dem Vergleich auf ihre Buchstaben und Ziffern reduziert. Aus
„NAME-CHEAP.COM* UYUWOP" und „namecheap" wird dieselbe Form — der Treffer sitzt.

Kurze Namen bleiben streng: „Apple" darf nicht in „Pineapple Studios" treffen, „DHL" nicht in
einem beliebigen Wort. Ein Falsch-Treffer wäre teurer als ein verpasster, denn er schickt die
Beschaffung auf ein **fremdes** Portal. Zwölf Prüfungen bestehen ausschließlich darin, genau
das zu verhindern.

Gemessen am eigenen Bestand: von 70 Kontobewegungen ohne Beleg finden jetzt **52** ihr Portal
statt vorher 23.

### Nur holen, was wirklich fehlt

Neu ist eine Portal-Runde, die vor dem Herunterladen abgleicht, welche Rechnung schon im Haus
ist. Am eigenen Bestand gemessen: 51 Rechnungen im Portal, 50 bereits im Archiv — genau **eine**
war zu holen. Vorher hätte das 51 Abrufe bedeutet.

Der Abgleich schaut dabei bewusst über **alle** Anbieter, nicht nur den gesuchten. Bei
Marktplätzen trägt ein Beleg oft den Händler statt des Rechnungsstellers — wird danach
gefiltert, gilt ein vorhandener Beleg als fehlend und wird ein zweites Mal geholt. Genau so
entstand am 15.08. eine Dublette.

### Drei neue Anbieter-Portale

Drei Anbieter aus deinen offenen Kontobewegungen haben jetzt einen Eintrag, jeder mit
belegter Adresse aus der offiziellen Hilfe des Anbieters. Fünf weitere bleiben bewusst leer:
dort ließ sich keine Adresse belegen, und geraten wird hier nicht — der Report sagt dann
ehrlich „Portal unbekannt".

### Unter der Haube

- 36 neue Prüf-Kanarien (24 für die Namensauflösung, 12 für den Abgleich).
- Rechnungsnummern liefen im Portal-Lauf ungefiltert über die Konsole und wären auch im
  stillen Modus im Chat gelandet. Dieselbe Fehlerklasse wie am 15.08. an anderer Stelle —
  jetzt geschlossen und mit einem Nachweis belegt.
- Report und Beschaffung nutzen dieselbe Auflösung. Vorher konnte der Report ein Portal
  anzeigen, das die Beschaffung nicht fand.

---

## v1.161.0 — Der Betrag steht nicht immer in einer Summenzeile (2026-08-16)

### Was dazugekommen ist

Die Erkennung „Datum als Betrag gelesen" (v1.159.0) suchte den richtigen Betrag nur in einer
Summen**zeile** — „SUMME: EUR 5,90". Vier Belege eines Anbieters nennen ihn aber nur im Fließtext:

```
Ihre neue Rechnung vom 23.03.2026 über 70,57 EUR liegt für Sie im Control-Center bereit.
Den offenen Betrag buchen wir am 28.03.2026 von Ihrem Konto ab.
```

Erfasst wurde `28.03` — wieder der Abbuchungstag. Der richtige Betrag stand im Satz darüber.

Bruno liest jetzt auch solche Formulierungen („Rechnung … über X EUR", „Betrag in Höhe von X EUR").
Wichtig dabei: die Zahl muss **sprachlich an „Rechnung" oder „Betrag" gebunden** sein. Eine
beliebige Zahl im Dokument wird nie zum Rechnungsbetrag erklärt — dafür gibt es eine eigene
Gegenprobe unter den Prüf-Kanarien.

### Die zwölf offenen Fälle sind durchgesehen

| Ergebnis | Anzahl |
|---|---:|
| Betrag deterministisch korrigierbar (neu erkannt) | 4 |
| **keine Rechnung** — Willkommens-Mail, Störungs-Info, Mahnungs-Hinweis, E-Mail-Verkehr | 8 |

Alle acht wurden vom Prüf-Gate ohnehin blockiert — fünf davon, obwohl sie **nicht** als
prüfbedürftig markiert waren. Genau dafür wurde das Gate diese Woche scharfgeschaltet.
Der Befund steht jetzt in jedem dieser Belege, damit sie in Berichten sichtbar sind statt
nur still zu scheitern.

### Ein Nebenbefund

Die vier korrigierten Belege sind **Benachrichtigungen**, keine Rechnungen: „liegt für Sie im
Control-Center bereit". Betrag und Fälligkeit stimmen, aber Rechnungsnummer und Steuerausweis
fehlen. Sie sind der Nachweis, *dass* eine Rechnung existiert — nicht die Rechnung selbst.
Entsprechend vermerkt, die echten Belege kommen aus dem Anbieter-Portal.

---

## v1.159.0 — Wenn der Abbuchungstag als Rechnungsbetrag gelesen wird (2026-08-16)

### Was passiert ist

Auf einer Hosting-Rechnung stand:

```
SUMME: EUR 5.90
Der Rechnungsbetrag in Höhe EUR 5,90 wird am 30.01.2025 per Lastschrift eingezogen.
```

Erfasst wurde als Betrag: **30,01** — der Abbuchungstag. Über acht Monate ergab das die Serie
30.01 / 30.03 / 30.04 / 30.05 / 30.06 / 30.07 / 30.08 / 30.09. Jeder „Betrag" war exakt der
Zahltag seines Monats. Statt 47,20 € standen 180,31 € in den Büchern, mit entsprechend
überhöhter Vorsteuer.

### Warum keine der bestehenden Prüfungen angeschlagen hat

Das ist der interessante Teil. Die Prüfungen waren da und haben korrekt gearbeitet:

- **Betrag im Dokument belegt?** Ja — 30.01 stand im Text. Als Datum.
- **Rechnen Netto, Steuer und Brutto zusammen?** Ja, in sich stimmig.
- **Ist das überhaupt eine Rechnung?** Ja, mit allen Pflichtangaben.

Jede Prüfung fragte „steht die Zahl im Dokument?". Keine fragte **„ist diese Zahl der Betrag
oder ein Datum?"**. Ein plausibler Wert an der falschen Stelle rutscht durch jedes Netz, das nur
auf Vorhandensein prüft.

### Die Lösung — an drei Stellen, nicht an einer

| Wann | Was passiert |
|---|---|
| **Beim Einlesen** | Der Beleg wird sofort zur Prüfung markiert, mit dem richtigen Betrag im Klartext daneben |
| **Vor dem Buchen** | Das Prüf-Gate blockiert — so ein Beleg kann nicht mehr gebucht werden |
| **Im Health-Check** | Bereits gebuchte Altfälle werden weiterhin gemeldet |

Der Test ist bewusst eng: Verdacht entsteht nur, wenn der Wert die Form Tag.Monat hat **und**
dieselbe Zahl im Text als Datum vorkommt **und** das Dokument einen abweichenden Betrag als
Summe ausweist. Ein echter Betrag von 12,05 € löst keinen Alarm aus, solange die Rechnung ihn
als Summe nennt — geprüft.

**Nichts wird automatisch überschrieben.** Der Betrag ist die zentrale Zahl einer Buchung; der
Beleg geht zur Prüfung, mit dem belegten Gegenwert. Korrigieren tut ein Mensch — auf Wunsch mit
einem Werkzeug, das den Ersatzwert ausschließlich aus der Summenzeile des PDFs zieht, nie aus
einer Schätzung.

### Was im Bestand gefunden wurde

1.653 Belege durchsucht:

- **8** hart belegte Fälle (alle derselbe Anbieter) — korrigiert, Sicherungskopie je Datei
- **6** davon lagen bereits als Entwurf im Buchhaltungssystem — entfernt
- **12** weitere Verdachtsfälle ohne auffindbare Summenzeile — zur Sichtprüfung markiert, **kein**
  geratener Ersatzwert
- **31** Treffer auf Kontobenachrichtigungen statt Rechnungen — ohne Buchungswirkung, unangetastet

### Neue Prüf-Kanarien

Siebzehn, darunter zwei Gegenproben: dass ein echter Betrag in Datumsform **nicht** verdächtigt
wird, und dass derselbe Dokumenttext je nach gelesenem Betrag unterschiedlich beurteilt wird —
sonst könnte ein Prüfer, der immer „unauffällig" sagt, alle Tests bestehen.

---

## v1.157.0 — Drei Schutzlücken, die beim Buchen aufgefallen sind (2026-08-16)

Gefunden bei einem gewöhnlichen Buchungslauf. Alle drei sind Fälle derselben Sorte: eine Prüfung
war vorhanden, hat aber nicht gewirkt. Das ist gefährlicher als eine fehlende Prüfung, weil der
Bericht trotzdem „geprüft" sagt.

### 1. Das Prüf-Gate rechnete, ohne die Buchung stoppen zu können

Vor jeder Buchung laufen fünf Grundregeln: Richtung, Lieferanten-Eindeutigkeit, Steuer-Stimmigkeit,
Pflichtfelder, Beträge. Ausgewertet wurde davon nur **eine** — die Betragsprüfung. Die anderen vier
liefen mit, ihr Ergebnis las niemand.

In der Praxis hat das nichts kaputtgemacht, weil davor schon andere Filter greifen. Aber die
Schranke selbst war offen. Jetzt stoppt **jede** verletzte Regel die Buchung, mit ihrer eigenen
Begründung im Protokoll.

### 2. Die Vorschau war milder als der Ernstfall

Der Probelauf brach ab, bevor das Prüf-Gate lief — er zeigte deshalb Belege als buchbar, die im
scharfen Lauf blockiert worden wären. Eine Vorschau, die strenger ist als die Wirklichkeit, ist
harmlos. Eine, die milder ist, verleitet zur Freigabe. Beide Läufe prüfen jetzt identisch.

### 3. „Nicht gelesen" galt als „0 Prozent"

Bei Rechnungen aus dem Ausland ohne deutsche Umsatzsteuer (Reverse Charge) muss die Rechnung
0 % ausweisen. Konnte die Texterkennung das Steuerfeld gar nicht lesen, wurde daraus intern eine
Null — und der Beleg sah aus wie ein sauber ausgewiesener Reverse-Charge-Fall.

Aufgefallen an einer Rechnung, auf der nur „Total $15.00" stand: keine Rechnungsnummer, keine
Steuerangabe, kein Pflichtbestandteil. Sie wäre als Vorsteuer-Fall durchgelaufen. Ein fehlender
Wert ist jetzt ein Grund zur Prüfung, kein Nachweis.

### Was du davon hast

Diese drei Lücken betreffen den Kern: **was überhaupt gebucht werden darf.** Sie zu schließen
heißt, dass die Prüfungen, die es ohnehin gibt, jetzt auch wirken.

---

## v1.156.0 — Doppelbuchung durch einen veralteten Zwischenspeicher (2026-08-16)

### Was passiert ist

Bei einem Buchungslauf wurden vier Belege ein zweites Mal angelegt. Einer der Zwillinge war
bereits gebucht und mit der Kontobewegung verknüpft.

### Warum

Damit ein Lauf nicht jedes Mal den gesamten Bestand neu lädt, merkt Bruno sich die bereits
gebuchten Rechnungsnummern für eine Stunde. Am selben Tag kam eine Erweiterung dazu: Belege
**ohne** Rechnungsnummer (Kassenbons, Portorechnungen) werden seither über Lieferant, Datum und
Betrag erkannt.

Der gespeicherte Stand stammte aber noch von davor — er kannte nur Rechnungsnummern. Da er
33 Minuten alt und damit „frisch genug" war, wurde er verwendet. Belege ohne Nummer liefen an der
Dublettenprüfung vorbei.

Die Stundenfrist schützt gegen **veraltete Daten**. Sie schützt nicht gegen einen **veralteten
Aufbau** der Daten. Genau diese Lücke hat zugeschlagen.

### Behoben

Der Zwischenspeicher trägt jetzt eine Versionsnummer. Passt sie nicht zum Programm, wird er
verworfen und der Bestand frisch geladen — sichtbar im Protokoll, nicht stillschweigend.
Der Unterschied ist messbar: statt 510 gemerkter Belege kennt der Lauf jetzt 996.

### Aufgeräumt

Die vier Doppelbuchungen wurden entfernt (alle noch im Entwurfsstadium, also spurlos löschbar).
Die Originale blieben unangetastet — geprüft und bestätigt, auch die bereits verknüpften.

### Neue Prüf-Kanarien

Neun Stück, darunter eine, die den Vorfall exakt nachstellt: frischer Zeitstempel, alter Aufbau.
Wäre der Schutz nicht wirksam, würde sie sofort rot.

---

## v1.155.0 — Die Kontobewegung entscheidet, ob es eine Ausgabe war (2026-08-16)

### Das Problem

Ob ein Beleg eine Ausgabe ist, stand bisher in einem Feld, das die Texterkennung gefüllt hat.
Konnte sie es nicht sicher lesen, schrieb sie „unklar" hinein — und „unklar" wurde beim Buchen
wie „Ausgabe" behandelt. Eine stille Annahme an der teuersten Stelle: bei der Richtung.

Im aktuellen Bestand trägt jeder dritte offene Beleg genau diesen Wert.

Drei Fälle, in denen die Annahme falsch ist: ein Anwaltsschreiben mit Betrag, bei dem Geld
**herein**kam. Eine eigene Ausgangsrechnung, die nach Eingangsrechnung aussieht. Ein Werbebrief,
dessen „Betrag" in Wahrheit ein Datum war — dazu gibt es gar keine Zahlung.

### Die Lösung

Vor jeder Buchung fragt Bruno jetzt das Konto. Ob Geld abgeflossen oder zugeflossen ist, steht
im Vorzeichen der Kontobewegung — da gibt es keinen Ermessensspielraum, keine Layout-Falle und
keine Sprachvariante.

Vier Ergebnisse, vier Konsequenzen:

| Was die Bank zeigt | Was Bruno tut |
|---|---|
| Geld abgeflossen | Ausgabe belegt — wird gebucht |
| Geld zugeflossen | **gestoppt** — das ist keine Ausgabe, egal was im Beleg steht |
| Ab- und Zufluss gleicher Höhe | **gestoppt** — nicht eindeutig, du siehst es dir an |
| gar keine Bewegung, Beleg unklar | **gestoppt** — kein einziges Signal, das die Richtung belegt |

Ein Beleg, dessen Feld schon eindeutig „Eingangsrechnung" sagt, läuft wie bisher — die Bank
bestätigt ihn dann zusätzlich, sichtbar im Prüfprotokoll.

### Was du davon hast

Eine falsche Richtung ist der teuerste Buchungsfehler: Vorsteuer, die dir nicht zusteht, oder
eine Einnahme, die als Kosten verschwindet. Beides fällt oft erst beim Steuerberater auf.
Dieser Prüfschritt fängt es davor ab — ohne dass du etwas tun musst.

### Unter der Haube

- Der Bank-Anker sitzt in der Regel HR#1 des Prüf-Gates, das vor **jedem** scharfen Buchen läuft.
- Fehlen die Kontobewegungen, sagt Bruno das laut, statt still auf die alte Annahme zurückzufallen.
- Neun neue Prüf-Kanarien, darunter eine, die absichtlich prüft, ob der Schutz überhaupt
  aufgerufen wird — genau das war vorher das Problem: das Modul existierte seit einem Tag,
  aber kein Buchungsweg fragte es.

### Zwei reparierte Wächter

- Eine Schutzprüfung (Modus-Nummern) stürzte seit gestern an einer toten Verknüpfung im
  Browser-Profil ab und bewachte ihre Regel damit gar nicht mehr. Ein abgestürzter Wächter
  meldet nichts — das sieht aus wie „alles in Ordnung".
- Dieselbe Prüfung hielt drei korrekte Code-Stellen für Fehler, weil sie Unterwege wie „3b"
  nicht kannte. Sie liest die gültigen Kennungen jetzt vollständig aus der Modus-Beschreibung.

---

## v1.154.0 — Der Name auf dem Kontoauszug ist nicht der auf der Rechnung (2026-08-16)

### Das Problem

Bei 11 offenen Belegen passten **Betrag und Datum** exakt zu genau einer Kontobewegung.
Zugeordnet wurde trotzdem nichts — allein weil Bank und Rechnung verschiedene Namen für
denselben Anbieter führen.

Die Rechnung nennt den Rechtsträger, der Kontoauszug die Marke oder den Zahlungsabwickler.
Ein irisches Unternehmen erscheint auf dem Konto unter seinem Produktnamen. Eine Holding
aus Singapur unter ihrer Domain. Ein Konzern unter einem Kürzel plus Referenznummer.

Kein Textvergleich findet das. Es ist Wissen, kein Muster.

### Die Lösung

Bruno kennt jetzt die Bank-Schreibweisen bekannter Anbieter. Jeder Eintrag stammt aus einem
tatsächlich beobachteten Paar aus Rechnung und Kontobewegung — nicht aus einer Vermutung.

Wichtig: Der Namensabgleich wird großzügiger, die Zuordnung nicht. Betrag, Datum, Richtung
und Eindeutigkeit gelten unverändert. Passen mehrere Kontobewegungen gleich gut, geht der
Fall weiterhin zur Sichtprüfung statt in eine Buchung.

### Was du davon hast

Belege, die vorher unzuordenbar liegenblieben, verknüpfen sich von selbst — ohne dass die
Prüfung lockerer wird.

### Zweiter Fund: der Abgleich lief gegen das falsche Konto

Der Zuordnungslauf meldete **null** Treffer. Ursache war kein Fehler in der Logik: Er lief
gegen ein Nebenkonto mit 79 offenen Umsätzen. Das Hauptkonto mit **261** offenen Umsätzen
war nie im Blick.

Daraus die Regel: Bei null Treffern zuerst prüfen, ob überhaupt die richtigen Daten geladen
sind — nicht die Zuordnungsregel lockern. Ein zu enger Ausschnitt sieht genauso aus wie ein
zu strenges Kriterium.

### Unter der Haube

- `system/_lib/bank-alias.mjs` mit 17 Kanarien, alle grün
- Der eigene Test fand dabei einen Fehler: ein kurzer Alias traf in einem fremden
  Firmennamen. Behoben durch Vergleich auf Wortebene.
- Eingebaut als Fallback in `match-vouchers.mjs` — der bewährte Pfad bleibt unberührt

## v1.153.0 — Die Bank entscheidet, ob Geld kam oder ging (2026-08-16)

### Was neu ist

Bruno prüft ab jetzt **vor** jeder Buchung die Kontobewegung, um die Richtung zu bestimmen:
Ist Geld abgeflossen oder zugeflossen? Das steht im Vorzeichen des Kontoumsatzes — da gibt
es keinen Auslegungsspielraum.

Vorher hat Bruno die Richtung aus dem Beleg selbst geschlossen. Das geht in den meisten
Fällen gut und in den gefährlichen Fällen schief.

### Der Fall, der das ausgelöst hat

Ein Schreiben einer Anwaltskanzlei, vierstelliger Betrag, Aktenzeichen, seriöser Absender.
Sah aus wie eine Rechnung. War aber die Mitteilung über eine **Auszahlung** — das Geld kam
herein, nicht heraus.

Als Ausgabe gebucht wäre das doppelt falsch gewesen: falsches Vorzeichen im Gewinn **und**
Vorsteuer auf einen Geldeingang gezogen.

Die Kontobewegung hat es sofort geklärt: der Umsatz stand mit **Plus** im Konto.

### Zwei weitere Fälle aus demselben Lauf

- **Eine eigene Rechnung** an einen Auslandskunden lag im Eingangsordner. Als Ausgabe gebucht
  hätte der Umsatz in der Umsatzsteuervoranmeldung gefehlt.
- **Ein Werbe-Newsletter** trug einen vierstelligen Betrag — die Texterkennung hatte eine Zahl
  aus einer Werbegrafik gelesen. Es gab keinen einzigen passenden Kontoumsatz.

### Was du davon hast

Ein Beleg wandert erst dann in die Buchung, wenn die Kontobewegung die Richtung bestätigt.
Wo Bruno sich nicht sicher ist — etwa wenn Ab- und Zufluss denselben Betrag haben — sagt er
das, statt zu raten.

### Ausserdem

- **Rechnungsprüfung für den Altbestand:** Die Prüfung, ob ein Dokument überhaupt eine Rechnung
  ist, gab es bisher nur für neu eingehende Belege. Jetzt lässt sie sich über den gesamten
  Bestand nachziehen — 504 Belege haben ein Urteil bekommen.
- **Fehler-Museum:** Alle bisherigen Fehlerklassen sind an einer Stelle nachlesbar
  (`system/FEHLER-MUSEUM.md`) — was schiefging, was es gekostet hätte, welche Regel daraus wurde.

### Unter der Haube

- `system/_lib/bank-richtung.mjs` mit 9 Kanarien, alle grün
- `system/_bin/natur-nachtrag.mjs` — Rechnungsprüfung für Altbestände
- `system/_bin/vision-review.mjs` — Sichtprüfung für Belege ohne lesbare Textebene

## v1.152.0 — Nachschlagewerk sagte „kenne ich nicht", obwohl der Eintrag da war (2026-08-16)

### Der Fehler

Bruno hat ein Nachschlagewerk für Lieferanten: Wer sitzt wo, wie wird die Steuer behandelt.
Fragt man dort nach einem **Kurznamen** — „openai", „kie", „hsp" — kam die Antwort
**„kenne ich nicht, bitte prüfen lassen"**. Obwohl alle drei längst eingetragen waren.

Der Grund: Das Nachschlagewerk suchte nur in eine Richtung. Es fand „openai" im vollständigen
Namen „OpenAI, LLC", aber nicht umgekehrt. Beim Buchen selbst hat es funktioniert — nur beim
Nachschlagen nicht.

### Warum das nicht harmlos war

Zwei Antworten auf dieselbe Frage. Bruno schlägt in **jeder** Sitzung dort nach — bei jeder
Steuerfrage, vor jeder Rückfrage an dich, bei jedem Prüfbefund. Das falsche „kenne ich nicht"
führte dazu, dass Belege zur Handprüfung gelegt wurden, die längst geklärt waren.

Es ist heute selbst passiert: Bruno meldete „vier Lieferanten fehlen im Nachschlagewerk,
deshalb hängen rund 196 Belege" — **alle vier waren drin.**

### Behoben

Das Nachschlagewerk sucht jetzt in **beide** Richtungen und sagt dazu, wenn du einen Kurznamen
getippt hast:

```
⚠️  Teiltreffer: "openai" ist ein Kurzname, der Registry-Key lautet vollständig:
    (weitere Keys mit "openai": openai opco, llc · openai ireland limited)
Treffer: openai, llc
```

So siehst du sofort, ob es mehrere Firmen mit ähnlichem Namen gibt — bei OpenAI und Google
ist das der Fall, und sie werden **steuerlich unterschiedlich behandelt**.

### Zwei weitere Funde beim Nachprüfen

**Ein Newsletter galt als Lieferant.** Absender „KIE AI Team" wurde wegen der Namensähnlichkeit
dem Anbieter „kie.ai" zugeordnet — ein Werbe-Newsletter hätte als Betriebsausgabe gebucht werden
können. Steht jetzt als „niemals buchen" drin.

**Ein „bitte prüfen" wurde stillschweigend übergangen.** Beim Google Chrome Web Store ist auf dem
Beleg nicht eindeutig, welche Google-Firma abrechnet. Genau dafür gibt es die Markierung „bitte
prüfen" — sie wurde jedoch übersprungen, und der Beleg lief über den allgemeinen Google-Eintrag
durch. Jetzt greift sie: Der Beleg wird dir vorgelegt statt automatisch gebucht.

### Neu im Nachschlagewerk

**Google Cloud EMEA Limited** (Dublin, Irland — §13b Reverse-Charge, mit USt-ID vom Beleg belegt)
und **Google Chrome Web Store** (bewusst zur Prüfung markiert).

### Für dich heißt das

Weniger Belege landen unnötig auf deinem Tisch — und die, die dort landen, gehören auch hin.

---

## v1.151.0 — Stripe: geklärt, was per Schnittstelle geht und was nicht (2026-08-16)

### Die Frage

Bei vier Zahlungseingängen von Stripe stand „Beleg fehlt". Naheliegender Verdacht: eine Rechnung
fehlt. **Stimmt nicht.**

### Die Antwort

Das sind **Auszahlungen an dich**, keine Einkäufe. Dafür stellt niemand eine Rechnung aus — ein
Zahlungsdienstleister überweist dir dein eigenes Geld. Alle vier ließen sich per Schnittstelle
**taggenau** deiner Bank zuordnen (398,34 / 200,00 / 492,83 / 923,62 €).

Deine **Ausgangsrechnungen** sind ebenfalls vollständig da: 15 Stück, alle mit Nummer und
PDF-Link — und alle bereits in deiner Buchhaltung erfasst.

### Eine Sache geht wirklich nicht — und warum

Welche Einzelzahlungen in einer Auszahlung stecken, verrät Stripe nur bei **automatischen**
Auszahlungen. Deine sind auf „manuell" eingestellt. Stripe sagt das im Fehlertext selbst:
*„Balance transaction history can only be filtered on automatic transfers, not manual."*

Drei Wege geprüft, alle drei bestätigen dasselbe — auch der Umweg über die Auswertungs-Schnittstelle,
deren Verknüpfungsspalte schlicht `automatic_payout_id` heißt.

**Der Fix ist eine Einstellung, kein Werkzeug:** Stellst du in Stripe auf automatische Auszahlung um,
löst sich jede künftige Auszahlung von selbst auf. Rückwirkend geht das nicht — die bisherigen
bleiben auf der Klärliste, wo sie korrekt einsortiert sind.

### Warum das wichtig ist

Ein „geht nicht" ist nur dann etwas wert, wenn der Grund bekannt ist. Sonst baut man beim nächsten
Mal einen Browser-Umweg für ein Problem, das keiner ist. Deshalb steht der Befund jetzt samt
Fehlertext und Gegenprobe in der Anbieter-Doku — inklusive der ausdrücklichen Notiz, **keinen**
Browser-Umweg dafür zu bauen.

### Unter der Haube

- `postReportRun()` im Stripe-Client: die **einzige** erlaubte Schreib-Operation, hart auf die
  Auswertungs-Adresse begrenzt. Ein Bericht erzeugt nur eine Auswertung — er bewegt kein Geld.
  Die Read-only-Zusage des Bausteins bleibt wörtlich gültig.
- Anbieter-Doku ergänzt: was geht (Auszahlungsliste, Beträge, Gebühren, Kundenrechnungen mit PDF),
  was nicht (Ketten-Auflösung bei manuellen Auszahlungen) — jeweils mit gemessenem Beleg.

## v1.150.0 — Der Rechnungscheck sitzt jetzt am Eingang (2026-08-16)

### Das Wichtigste in einem Satz

Ab sofort prüft Bruno bei **jedem** Dokument, ob es überhaupt eine Rechnung ist — **bevor** es in
deiner Buchungs-Queue landet, nicht erst Wochen später beim Gesundheits-Check.

### Warum das nötig war

Der Prüfer für „ist das wirklich eine Rechnung?" existierte schon. Er lief nur an der falschen
Stelle: **nach** dem Ablegen. Das Ergebnis konnte man messen — **129 Dokumente lagen als „Belege"
in deinem Ordner 2, die gar keine Rechnungen sind**: Support-Briefe, Newsletter, Werbe-Mails,
Statistik-Berichte, sogar eine Benachrichtigung über einen neuen Kontoauszug.

Allein von einem einzigen Anbieter waren es 47 Stück. Die haben deine Liste aufgebläht und bei
jeder Durchsicht Arbeit gemacht, die niemand brauchte.

### Was jetzt passiert

Jedes eingehende PDF wird auf Rechnungsmerkmale geprüft — Rechnungswort, Steuerangabe, Betragszeile,
Rechnungsnummer. Was nachweislich keins hat, landet direkt in `_kein-buchungsbeleg/<Jahr>/` statt in
deiner Buchungs-Queue. **Gelöscht wird nie etwas**, alles bleibt nachvollziehbar liegen.

Zwei Fälle werden bewusst **nicht** aussortiert, weil sie ein menschliches Urteil brauchen:

- **Rechnung ohne Nummer** — bei Auslandsleistungen mit Steuerschuldumkehr ist das korrekt.
  Der Vorsteuerabzug verlangt dort keinen Rechnungsbesitz (§ 15 Abs. 1 S. 1 Nr. 4 UStG).
- **Scan ohne Textebene** — nicht maschinell prüfbar, also auch nicht maschinell verurteilt.

Und: Ein Beleg mit erkanntem Betrag wird **nie** aussortiert. Der Betrag belegt die Rechnungsnatur.

### Zoom-Rechnungen kommen jetzt automatisch

Der Weg zu den Zoom-Rechnungs-PDFs war bisher offen — die Liste war lesbar, der Download nicht.
Jetzt läuft beides. 14 Rechnungen gefunden, 9 lagen schon bei dir, **5 fehlende wurden geholt**.

Der Umweg, der das gelöst hat, ist auch für andere Portale nützlich und steht dokumentiert:
Ein Einblende-Fenster („Quick Tour") lag über der Tabelle und schluckte jeden Klick — es sah aus
wie ein kaputter Knopf, war aber nur verdeckt.

### Aufgeräumt

- 129 Nicht-Belege aus der Buchungs-Queue nach `_kein-buchungsbeleg/` sortiert, mit Protokollzeile
  pro Dokument (was, wann, warum).
- Bei den betroffenen Google-Dokumenten vorher stichprobenartig ins PDF geschaut — vier von vier
  bestätigt: Support-Umfrage, Profil-Statistik, Glückwunsch-Mail, Konto-Löschbrief. Keine Rechnung.

### Unter der Haube

- Neues Eingangs-Gate in `sort.mjs` (`writeBeleg`), damit **alle** Wege es passieren — E-Mail,
  Drop-Ordner, Portal. Ein Prüfer an einer einzigen Stelle statt drei halbe.
- Canonical-JSON trägt zwei neue Felder: `beleg_natur` + `beleg_natur_grund`. Damit ist im Nachhinein
  belegbar, dass geprüft wurde — nicht nur, dass etwas nicht auffiel.
- 16 Regressionstests (`sort-natur-gate.test.mjs`), inklusive eines Tests gegen genau den Fehler,
  der beim ersten Live-Lauf auftrat: das Gate übersprang Belege mit Betrag komplett und trug deshalb
  gar kein Urteil ein — es lief unsichtbar. Jetzt wird immer geprüft, nur das Aussortieren ist an
  den Betrag gekoppelt.
- Zoom-Klickweg + PDF-Adresse in `portal-registry.json`, inklusive der Stolperfalle mit dem Overlay.

## v1.149.0 — 84 Belege gebucht, und zwei Fallen weniger (2026-08-15)

### Was passiert ist

In einem durchgehenden Lauf habe ich **84 Belege gebucht** und **17 davon direkt mit deinen
Kontobewegungen verknüpft** — ohne einen einzigen Fehler, jede Buchung nach dem Schreiben noch einmal
gegengelesen (Richtung, Steuersatz, Betrag).

Die offenen Bankbuchungen ohne Beleg sind dadurch gesunken: 2025 von 160 auf 149, 2026 von 89 auf 83.

### Zwei Fallen, die ich gefunden und geschlossen habe

**1. Eine Rechnung, die im richtigen Ordner liegt, ist noch nicht im System.**
Zehn Rechnungen aus einem Anbieter-Portal hatte ich direkt in den Quartalsordner gelegt — richtiger
Name, richtiger Platz, sauberes PDF. Beim Buchen tauchten sie trotzdem nicht auf, weil die *gelesene
Fassung* fehlte (die Datei mit Betrag, Datum, Steuersatz). Ohne die ist eine Rechnung für die
Buchhaltung unsichtbar — und es gibt keine Fehlermeldung, sie fehlt einfach.

Aufgefallen ist es nur durch einen Quervergleich. Für dich heißt das: Belege gehören immer durch den
Einlese-Schritt, auch wenn die Datei schon perfekt benannt am richtigen Platz liegt.

**2. Ein Anbieter muss an zwei Stellen bekannt sein, nicht an einer.**
Ich hatte den Werbe-Anbieter sauber als „irisches Unternehmen, Steuerschuld geht auf dich über"
eingetragen, und die Prüfung bestätigte das. Der Buchungslauf verweigerte trotzdem weiter mit
„Sitzland unklar". Grund: Es gibt zwei Nachschlagewerke — eines für die steuerliche Behandlung, eines
für das Buchungskonto. Der Buchungslauf fragt das zweite. Erst der Eintrag in **beiden** schaltete die
zehn Rechnungen frei.

Beides ist jetzt als Regel hinterlegt, damit es bei dir nicht passiert.

### Dubletten — die Prüfung, nach der du gefragt hast

- Beim Einlesen wurden mehrere Doppel automatisch abgefangen (2 von 25 bei der Bank, 2 von 11 beim
  Paketdienst) — erkannt am Datei-Fingerabdruck, also auch bei anderem Dateinamen.
- Beim Werbe-Anbieter gibt es **kein** Doppel-Risiko, obwohl derselbe Vorgang zweimal vorliegt: Die
  E-Mail trägt eine Transaktions-Nummer, die Rechnung eine Rechnungsnummer. Gebucht wird nur die
  Rechnung. 14 Vorgänge geprüft, 0 Doppelablagen.
- Der Gesundheits-Check meldet mehrere Kontoumsätze, die mehrfach im selben Konto stehen. Das kann
  eine echte Mehrfachzahlung sein oder ein Import-Doppel — das ist ohne Kontoauszug nicht
  entscheidbar, deshalb melde ich es als Hinweis statt es stillschweigend zu bereinigen.

### Sicherheit

Der Gesundheits-Check zeigt nach den 84 Buchungen **exakt dieselben Werte wie vorher** — meine
Buchungen haben also keinen einzigen neuen Mangel erzeugt. Die sieben kritischen Punkte stammen alle
aus dem Altbestand (Werbe-Mails und Hinweis-Mails, die früher als Rechnung erfasst wurden, zusammen
67,75 €). **Keiner davon ist gebucht** — die Prüfungen haben sie zuverlässig gestoppt.

### Wissensstand

2026-08-15
## v1.148.0 — Ich finde jetzt deutlich mehr Rechnungen in deinem Postfach (2026-08-15)

### Neue Funktionen

**Postfach-Suche findet Rechnungen, die vorher unsichtbar waren.**
Bisher habe ich in deinen E-Mails nur die **Betreffzeile** durchsucht. Das klingt harmlos, war aber eine
echte Lücke: Viele Anbieter schreiben gar kein Wort wie „Rechnung" oder „Invoice" in den Betreff, sondern
nur in den Text darunter. Deren Rechnungen habe ich schlicht nie gesehen — ohne jede Fehlermeldung.

Gemessen an einem echten Postfach, gleiches Jahr:

| Suche | gefundene Mails |
|---|---|
| nur Betreff (bisher) | 69 |
| Volltext (jetzt) | **88** |

Das sind **28 % mehr**. Ein Anbieter war komplett unsichtbar: 0 Treffer über den Betreff, obwohl 8 Mails
mit Rechnungsanhang vorlagen.

Falls dein Postfach dadurch zu viele Treffer liefert, kannst du mit `--nur-betreff` jederzeit auf die
alte, engere Suche zurückschalten.

**Neue Prüfung: Ist das überhaupt eine Rechnung?**
Ich schaue jetzt in jedes Dokument hinein, bevor es in deine Buchungsliste wandert — egal ob es aus dem
Postfach, aus einem Anbieter-Portal oder aus deinem Posteingangs-Ordner kommt.

Der Anlass war unangenehm konkret: Ein Stapel von 14 Dokumenten lag als „Belege" im Archiv. Auf **jedem
einzelnen** stand wörtlich „Das ist keine Rechnung" — es waren Zahlungs-Benachrichtigungen. Die echten
Rechnungen desselben Anbieters lagen unangetastet im Portal.

Ich prüfe deshalb ab sofort auf:
- Sagt das Dokument selbst, dass es **keine** Rechnung ist?
- Fehlen alle Rechnungsmerkmale (kein Rechnungswort, keine Steuerangabe, kein Betrag)? → wahrscheinlich
  Werbung oder eine Benachrichtigung
- Fehlt die Rechnungsnummer?
- Ist das Dokument nur ein Bild ohne lesbaren Text? → dann sage ich das ehrlich, statt es durchzuwinken

Findet die Stichprobe einen „ist keine Rechnung"-Hinweis, prüfe ich **den ganzen Stapel** statt nur eines
Dokuments — solche Sätze stehen fast immer in allen Dokumenten desselben Anbieters.

**Wichtig für deinen Vorsteuerabzug:** Ich bewerte nicht stur nach Checkliste. Bei Anbietern außerhalb der
EU (Reverse-Charge, § 13b UStG) verlangt das Gesetz gar keine klassische Rechnung — dort ist eine fehlende
Umsatzsteuer sogar korrekt. Ein solcher Beleg wird deshalb nicht mehr fälschlich als Mangel gemeldet. Die
abschließende Bewertung trifft weiterhin dein Steuerberater.

### Verbesserungen

**Vier Anbieter-Portale neu erschlossen** — die Klickwege sind gespeichert, künftige Abrufe gehen direkt:
- Ein Werbe-Anbieter: Die Rechnungen fand ich nur über die **Konto-Nummer, die auf dem Beleg selbst steht**
  — nicht durch Herumsuchen in den Konten-Umschaltern. Dazu ein direkter PDF-Link je Zahlung.
- Ein Domain-Anbieter: 23 Dokumente über einen direkten PDF-Link.
- Ein KI-Dienst: Hier gab es **gar keine Rechnungen**, weil im Konto das Rechnungs-Profil leer war. Ich
  habe es gefüllt (Firmenname, USt-IdNr., Anschrift) — seitdem stellt der Anbieter Rechnungen aus, und die
  bestehenden 7 kamen als Sammel-Download.
- Deine Bank: Auch bei einem **geschlossenen** Konto bleiben die Gebühren-Rechnungen abrufbar. Ich hatte
  das zuerst falsch eingeschätzt („Konto zu, also keine Belege") — tatsächlich lagen 37 Rechnungen bereit,
  nur hinter einem zweiten Reiter, den ich übersehen hatte.

**Doppelte Belege werden weiterhin zuverlässig abgefangen.** Beim Einlesen der Bank-Rechnungen waren 2 von
25 bereits im Archiv — sie wurden erkannt und nicht erneut abgelegt. Der Abgleich läuft über den
Datei-Fingerabdruck, also auch dann, wenn eine Datei anders heißt.

### Unter der Haube

- Neues Prüf-Modul für die Beleg-Natur, mit 20 automatischen Tests (darunter Sabotage-Fälle, die
  absichtlich versuchen, die Prüfung auszutricksen)
- Die Portal-Klickwege liegen maschinenlesbar in der Portal-Liste: URL, Selektor, PDF-Link-Muster,
  Stolperfallen und wann sie zuletzt geprüft wurden
- Neue harte Regel: Ein Stapel geholter Dokumente gilt erst dann als Belege, wenn mindestens eines
  inhaltlich gelesen wurde

### Wissensstand

2026-08-15

---
## v1.147.0 — Bruno holt Rechnungen aus Anbieter-Portalen und prüft, ob es überhaupt Rechnungen sind

**Neue Funktionen**

- **Beleg-Prüfung nach dem Download.** Bruno schaut jetzt in jedes aus einem Portal geholte PDF hinein, statt nur zu prüfen, ob eine Datei entstanden ist. Konkreter Fall: Ein Anbieter lieferte 23 saubere PDFs — alle trugen aber die Kopfzeile „Transactions", keine Umsatzsteuer-Angabe, keine Rechnungsnummer. Solche Dokumente landen nicht mehr in der Buchungs-Queue, sondern in einem eigenen Ordner mit Erklärung, was ihnen fehlt und was zu tun ist.
- **Die Einordnung richtet sich nach dem Steuerfall, nicht nach dem Aussehen.** Bei einem Anbieter aus den USA ist ein fehlender Umsatzsteuer-Ausweis korrekt — dort schuldest du die Steuer selbst (Reverse-Charge), und das Gesetz verlangt für den Vorsteuerabzug in diesem Fall keine förmliche Rechnung. Bruno unterscheidet das jetzt, statt jeden Beleg an derselben Checkliste zu messen. Die letzte Entscheidung bleibt beim Steuerberater.
- **Eine Fehlerseite wird nicht mehr als Rechnung gespeichert.** Ist die Anmeldung beim Anbieter abgelaufen, liefert das Portal statt der Rechnung eine Fehlermeldung. Die landete bisher als PDF im Posteingang — mit Erfolgsmeldung. Jetzt bricht Bruno ab, speichert nichts und sagt, dass die Anmeldung erneuert werden muss.

**Verbesserungen**

- **Bruno erkennt wieder, zu welchem Anbieter eine Abbuchung gehört.** Die Bank schreibt `GOOGLE*CLOUD 693C3W`, hinterlegt war `google*cloud` — ein Sternchen statt Leerzeichen genügte, und der Treffer blieb aus. Von 47 Abbuchungen ohne Rechnung wurden nur 23 zugeordnet, jetzt 33.
- **Es wird nur noch geholt, was wirklich fehlt.** Bruno liest die Rechnungsnummern im Portal, vergleicht sie mit deinem Ordner und lädt ausschließlich die Lücken. Am echten Beispiel: 51 Rechnungen im Portal, 50 lagen vor — genau eine war zu holen.
- **Zeitfilter werden beachtet.** Mehrere Portale zeigen von sich aus nur den letzten Monat. Eine leere Liste heißt dort nicht „keine Rechnungen" — Bruno zieht den Zeitraum jetzt selbst auf.
- **Keine Kontodaten mehr im Verlauf.** Eine Auswertung gab Anbieter, Beträge und Verwendungszwecke im Klartext aus, obwohl der Schutzschalter gesetzt war. Statt 89 solcher Zeilen erscheinen jetzt 6 reine Zählwerte; die Details bleiben lokal.

**Unter der Haube**

- Neue Bewertung `system/PORTAL-FETCH-BEWERTUNG.md`: was am Portal-Weg zuverlässig ist, was nicht, und wie er sich zu fertigen Rechnungs-Sammeldiensten verhält — mit den gemessenen Zahlen eines vollen Testtags.
- Klickwege, Selektoren, PDF-Link-Muster und Stolperfallen von vier Portalen sind maschinenlesbar hinterlegt, damit der nächste Lauf sie nicht neu suchen muss.
- 25 automatische Tests für die Anbieter-Zuordnung, darunter Fälle, die absichtlich **nicht** treffen dürfen. Zwei davon schlugen beim Bauen fehl und hatten recht.

**Warum das wichtig ist**

Ein Beleg, der nur so aussieht wie eine Rechnung, fällt erst bei der Betriebsprüfung auf. Und ein Anbieter, den die Zuordnung nicht findet, landet stillschweigend in der Kategorie „kein Portal bekannt" — die Rechnung holt dann niemand.

## v1.146.0 — Bruno zaehlt keine Mangel mehr, die keine sind

**Verbesserungen**

- **Bevor Bruno eine Fehlerquote nennt, prueft er, ob der Mangel ueberhaupt moeglich war.** Bei einer Nachkontrolle deiner Zahlungslinks stand im Befund „18 von 20 erzeugen keine Rechnung". Vier davon waren Abo-Links — und bei Abos erzeugt der Zahlungsanbieter die Rechnung ohnehin automatisch, ein Einschalten ist dort technisch gar nicht vorgesehen. Sie als Mangel zu zaehlen war schlicht falsch. Bruno prueft jetzt zuerst, ob das bemaengelte Merkmal fuer den jeweiligen Fall gilt, bevor eine Zahl in einen Bericht wandert.
- **Eine Ursache gilt nur fuer den Weg, auf dem gemessen wurde.** Im selben Vorgang war „die Zahlungslinks sind schuld" die Diagnose — die vier betroffenen Kundenzahlungen liefen aber gar nicht ueber einen Link. Alle Links umzustellen haette an den echten Faellen nichts geaendert. Bruno prueft jetzt gegen: Haette die geplante Massnahme die konkreten Faelle verhindert, die den Anlass gaben? Wenn nein, sucht er weiter, statt die Quote zu melden.
- **Ein leeres Datenfeld ist kein fehlender Beleg.** Fuenf aeltere Rechnungen galten als „ohne Steuer-ID des Ausstellers", weil das Feld in der Schnittstelle leer war. Im tatsaechlichen PDF steht die Nummer bei vier davon drin — der Anbieter zieht sie aus den Stammdaten. Nur eine einzige Rechnung ist wirklich betroffen. Bruno schaut bei solchen Aussagen jetzt ins Dokument, nicht nur in die Datenbank.

**Warum das wichtig ist**

Falsche Mangel-Zahlen sind teurer als sie aussehen: Sie erzeugen Arbeit, die niemand braucht, und lenken von der echten Ursache ab. In diesem Fall haetten vier Links umgestellt werden sollen, an denen nichts kaputt war — waehrend der Weg, ueber den die beanstandeten Zahlungen tatsaechlich liefen, unbeachtet geblieben waere.

## v1.145.0 — Bruno lädt keine Rechnung mehr herunter, die er schon hat

**Verbesserungen**

- **Beim Abgleich zählt die Rechnungsnummer, nicht der Anbietername.** Beim ersten scharfen Portal-Lauf holte Bruno eine Rechnung, die längst im Ordner lag — er hatte im Archiv nach „Skool" gesucht, abgelegt war sie aber unter dem Namen der Community („Claude Code Academy"). Bei Marktplätzen steht auf dem Beleg oft der Händler, nicht der Rechnungssteller. Jetzt vergleicht Bruno nur noch Rechnungsnummern, und die sind ohnehin eindeutig. Ergebnis am selben Bestand: vorher „1 fehlt" (falsch), jetzt „0 fehlt" (richtig).
- **Eine Fehlerseite wird nicht mehr als Rechnung gespeichert.** Wenn die Anmeldung beim Anbieter abgelaufen ist, liefert das Portal statt der Rechnung eine Fehlermeldung. Bisher landete die als PDF im Posteingang — mit Erfolgsmeldung. Jetzt prüft Bruno vor dem Speichern, ob überhaupt eine Rechnung auf der Seite steht, bricht sonst ab und sagt dir, dass die Anmeldung erneuert werden muss. Es wird nichts geschrieben.

**Warum das wichtig ist**

Beide Fehler wären still geblieben. Eine doppelt geholte Rechnung führt schlimmstenfalls zur Doppelbuchung, eine gespeicherte Fehlerseite zu einem Beleg ohne Inhalt in der Buchhaltung. Aufgefallen sind sie nur, weil nach dem Holen geprüft wurde, was tatsächlich in der Datei steht — nicht nur, ob eine entstanden ist.

## v1.144.0 — Bruno findet die Rechnungsportale jetzt wieder und holt nur, was wirklich fehlt

**Neue Funktionen**

- **Bruno erkennt jetzt, zu welchem Anbieter eine Abbuchung gehört — auch wenn die Bank kreativ schreibt.** Deine Bank schreibt `GOOGLE*CLOUD 693C3W`, ein andermal `Google CLOUD 6BXB75`, bei Namecheap `NAME-CHEAP.COM* UYUWOP`. Bruno verglich das bisher Zeichen für Zeichen mit seiner Portal-Liste, in der die Anbieter schlicht `google*cloud` und `namecheap` heißen. Ein Leerzeichen statt Sternchen genügte, und der Treffer blieb aus: Von 47 Abbuchungen ohne Rechnung erkannte er nur bei 23 den Anbieter, obwohl das Portal längst hinterlegt war. Allein sechs Google-Cloud-Buchungen fielen so durch. Jetzt werden Schreibweisen vor dem Vergleich vereinheitlicht — **33 statt 23 Treffer**, ohne einen einzigen falschen.
- **Neue Portal-Runde: Bruno holt nur noch, was wirklich fehlt.** Bisher musste jede Rechnung einzeln angefordert werden. Jetzt sieht Bruno im Portal nach, welche Rechnungsnummern es gibt, vergleicht sie mit deinem Beleg-Ordner und lädt ausschließlich die Lücken. Am echten Beispiel: 51 Rechnungen im Skool-Portal, 50 lagen bereits vor — **genau eine war zu holen**. Das ist der Normalfall, wenn regelmäßig gebucht wird.

**Verbesserungen**

- **Die Rechnungsnummer wird gelesen, ohne die Seite aufzubauen.** Für den Abgleich „habe ich schon?" genügt die Nummer aus dem Seitenquelltext — das dauert Millisekunden statt Sekunden pro Rechnung. Erst der Beleg, der tatsächlich fehlt, wird vollständig als PDF erzeugt.
- **Anbieternamen aus der Bank funktionieren direkt als Eingabe.** `P.SKOOL.COM/ZSXQF` findet jetzt den Skool-Eintrag, statt mit einer Fehlermeldung abzubrechen.
- **Ohne bekanntes Rechnungsnummern-Format bricht die Portal-Runde ab**, statt vorsichtshalber alles herunterzuladen. Bruno sagt dann klar, was ihm fehlt.
- **Eine doppelte Anbieter-Zeile entfernt.** Namecheap stand zweimal in der Liste, weil früher jede Schreibweise von Hand nachgetragen wurde. Das erledigt jetzt der Abgleich selbst.

**Unter der Haube**

- Die Zuordnung Anbieter → Portal liegt jetzt an einer einzigen Stelle. Vorher gab es sie doppelt: einmal im Bericht, einmal beim Abholen — mit unterschiedlichem Ergebnis. Der Bericht konnte „Portal unbekannt" anzeigen, obwohl eines hinterlegt war.
- **25 automatische Tests** mit echten Bank-Schreibweisen, darunter Fälle, die absichtlich **nicht** treffen dürfen. Zwei davon schlugen beim Bauen fehl und hatten recht: „Pineapple Studios" wurde als Apple erkannt, und ein Anbietername mit Punkt fand seinen Eintrag nicht. Beides ist behoben — ohne die Tests wäre es in Betrieb gegangen.

**Warum das wichtig ist**

Fehlende Rechnungen kosten Vorsteuer. Wenn Bruno das Portal nicht findet, landet der Anbieter in der Liste „Portal unbekannt" — und die Rechnung holt niemand. Der Fehler war unsichtbar, weil nichts rot wurde: Es fehlte einfach ein Treffer.

## v1.143.0 — Doppelte Kontobewegungen werden jetzt schon beim Import verhindert

**Neue Funktionen**

- **Der Import stoppt, bevor er dieselbe Zahlung ein zweites Mal anlegt.** Bisher verglich Bruno beim Einlesen eines Kontoauszugs Datum, Betrag und Empfänger. Steht dieselbe Zahlung im zweiten Export mit dem Buchungstag statt dem Wertstellungstag — bei Banken ein üblicher Unterschied von einem Tag — sah das wie eine neue Zahlung aus und wurde importiert. Genau so waren bei dir 20 doppelte Bewegungen entstanden. Jetzt prüft Bruno zusätzlich mit Datumstoleranz: gleicher Betrag, gleicher Empfänger, Datum ein paar Tage daneben → Warnung mit genauer Auflistung. Sieht es nach einem kompletten Doppel-Import aus, bricht er ab, **bevor** die erste Zeile geschrieben wird.
- **Vorsichtig statt übereifrig:** Bruno wirft solche Zeilen nie automatisch weg. Eine echte Zahlung fälschlich zu verwerfen wäre schlimmer als eine Dublette — die findet der Buchhaltungs-Check, eine fehlende Zahlung niemand. Du entscheidest.

**Verbesserungen**

- **Dubletten in laufenden Abos werden jetzt gefunden.** Die Prüfung erkannte doppelte Bewegungen nur, wenn ein Betrag genau zweimal im Bestand stand. Bei einem Monatsabo gibt es aber viele Zahlungen gleicher Höhe — dort blieben fünf echte Dubletten unentdeckt. Jetzt schaut Bruno innerhalb eines engen Zeitfensters; die monatlichen Abbuchungen selbst lösen weiterhin keinen Alarm aus.
- **Bei Unklarheit schweigt Bruno.** Liegen drei gleiche Beträge dicht beieinander, lässt sich nicht sagen, welche zwei zusammengehören. Solche Fälle meldet er nicht mehr als Dublette — falsch zeigen ist schlimmer als nicht zeigen.
- **20 doppelte Kontobewegungen bereinigt.** Jede einzelne vorher gegen deinen Original-Kontoauszug geprüft. Gelöscht wurde immer nur die überzählige Zeile, nie die mit dem Beleg — nach jeder Löschung hat Bruno gegengeprüft, dass der Beleg noch hängt. Sechs weitere Fälle waren echte Mehrfachzahlungen und blieben unangetastet.
- **Zwei Beträge korrigiert und wieder mit der Bank verknüpft** (Skool 115,43 → 100,85 €, Paddle 49,00 → 42,08 €). In beiden Fällen war ein Dollarbetrag als Euro gebucht.

**Unter der Haube**

- Neues Werkzeug, um einen einzelnen Beleg gezielt mit einer Kontobewegung zu verknüpfen — nötig nach jeder Betragskorrektur, mit sechs Sicherheitsprüfungen vorab.
- 25 automatische Tests für die Dublettenerkennung. Einer davon schlug bei der Änderung fehl und hatte recht: Er deckte auf, dass bei drei dicht beieinanderliegenden Zahlungen kein sicheres Urteil möglich ist.

## v1.142.0 — Bruno findet jetzt doppelt importierte Kontobewegungen und liegengebliebene Korrekturen

**Neue Funktionen**

- **Doppelte Kontobewegungen werden erkannt, auch wenn das Datum verrutscht ist.** Wird derselbe Kontoauszug zweimal eingelesen, kann dieselbe Zahlung zweimal in deiner Buchhaltung landen — einmal mit dem Wertstellungstag, einmal mit dem Buchungstag. Weil sich die Daten unterscheiden, sah der Buchhaltungs-Check das bisher nicht als Paar. Jetzt schon: Bruno vergleicht Betrag und Zahlungsempfänger unabhängig vom Datum und meldet ein Paar, wenn genau eine der beiden Zeilen mit einem Beleg verknüpft ist und die andere offen daneben liegt — das ist das typische Muster eines doppelten Imports. Bei deinem Bestand hat das **16 solcher Paare aus dem April** gefunden. Bruno sagt dir dabei immer, welche Zeile die überzählige ist und welche du auf keinen Fall löschen darfst, weil an ihr der Beleg hängt.
- **Korrekturen, die nie in der Buchhaltung ankamen, fallen jetzt auf.** Wird ein Beleg nachträglich repariert — etwa weil die Währung falsch erkannt wurde — hilft das nur, solange er noch nicht gebucht ist. War er schon gebucht, bleibt der alte Wert in den Büchern stehen, und die Reparatur verpufft still. Genau das war bei zwei Belegen passiert und monatelang unbemerkt geblieben. Bruno prüft jetzt bei jedem Check, ob eine Beleg-Reparatur zeitlich nach der Buchung liegt, und meldet es, wenn die Buchung den alten Wert trägt.

**Verbesserungen**

- **Das Reparatur-Werkzeug warnt jetzt selbst.** Wenn es einen Beleg korrigiert, der bereits gebucht ist, sagt es das ausdrücklich und nennt den nächsten Schritt — statt stillschweigend weiterzulaufen und den Eindruck zu erwecken, die Arbeit sei erledigt. Es fragt dafür deine Buchhaltung direkt ab; ist sie gerade nicht erreichbar, sagt es auch das ehrlich, statt Sicherheit vorzutäuschen.
- **Weniger Fehlalarme.** Die neue Prüfung meldete im ersten Anlauf 74 Fälle, von denen nur zwei echt waren — die übrigen waren längst korrekt umgerechnet. Ein Prüfer, der ständig grundlos ruft, wird ignoriert und ist damit wertlos. Nach der Nachschärfung bleiben **3 Meldungen**, davon 2 echte. Zusätzlich meldet Bruno denselben Beleg nur noch einmal, auch wenn er mehrfach in deinen Ordnern liegt.

**Unter der Haube**

- 22 neue automatische Tests für die beiden Prüfungen. Für beide wurde geprüft, dass sie auch wirklich anschlagen: Schaltet man eine Regel ab, fallen genau ihre Tests durch und keine anderen. Ein Test, der immer grün ist, beweist nichts.
- Der Buchhaltungs-Check hat jetzt 27 Prüfdimensionen.

## v1.143.0 — Dein eigener Bereich: Erweiterungen, die jedes Update überleben

**Neue Funktionen**

- **`system/custom/` — hier gehört alles hin, was du dir selbst dazubaust.** Eigene Skripte, eigene Auswertungen, Anbindungen an Dienste, die im Standard nicht vorgesehen sind. Der Ordner wird bei einem Update **nie** überschrieben. Eine mitgelieferte Anleitung darin erklärt, was hierher gehört und was nicht.
- **Klare Trennung der drei Bereiche**, damit nichts verloren geht: `system/custom/` = dein Code · `system/_privat/` = deine privaten Daten und Notizen · `PROFIL.md` = deine Einstellungen. Alles andere ist Produkt und wird bei einem Update ersetzt.
- Bisher gab es diesen Schutz nur als ungeschriebene Übung — ein Kunde hatte sich auf diesem Weg bereits eine eigene Erweiterung für American Express gebaut. Jetzt ist der Ort offiziell, dokumentiert und geprüft.

**Verbesserungen**

- **Die Antwortsammlung (FAQ) ist wieder auf dem aktuellen Stand.** Sie hatte drei Fähigkeiten als „fehlt noch" geführt, die längst eingebaut sind: das **Leistungsdatum** wird ausgelesen und geprüft, **Kostenstellen** sind nachweislich setzbar, und die **Belegabholung aus Anbieter-Portalen** ist allgemein nutzbar statt nur für einen einzelnen Anbieter.
- Ehrlicher formuliert an zwei Stellen: Die hinterlegten Anbieter-Portale sind ein **Nachschlagewerk** („wo liegt die Rechnung?") — die meisten Rechnungen kommen weiterhin per Mail. Und ob eine Zusatz-Software zum Rechnungssammeln damit überflüssig wird, hängt an deinen Anbietern; das wird nicht mehr pauschal behauptet.
- Neu erklärt: **American Express** (das Buchen ist unproblematisch, offen ist der Belegweg — die Sammelabrechnung ersetzt keine Händlerrechnung), **Skonto** und **Zahlungsziele** (beides wird nicht ausgelesen, mit der Begründung, was das praktisch bedeutet), sowie **Österreich und Schweiz** (das hinterlegte Steuerrecht ist deutsch).

**Unter der Haube**

- Der Update-Schutz für `system/custom/` ist **gemessen, nicht angenommen**: Ein Testlauf beweist, dass eine Produktdatei ersetzt und eine Kundendatei unangetastet bleibt. Dabei zeigte sich auch, dass die mitgelieferte Anleitung im Ordner von Updates ebenfalls nicht angefasst wird — sie kommt nur bei der Erstauslieferung mit. Das ist dokumentiert, damit es niemanden überrascht.
- Neue interne Regel (#27c) gegen eine wiederkehrende Fehlerklasse: Ein Eintrag in einer Datei belegt noch keine Fähigkeit, und eine nicht gefundene Datei belegt keine Abwesenheit. Beides hatte dazu geführt, dass Bruno seine eigenen Fähigkeiten teils zu vorsichtig, teils zu großzügig beschrieb.

**Wissensstand:** 2026-08-15

---

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
