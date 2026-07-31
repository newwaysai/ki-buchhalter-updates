## v1.96.0 — Alte Irrtuemer ueber BuchhaltungsButler richtiggestellt

**Verbesserungen**
- **Ein Irrtum aus dem Juni ist korrigiert:** Bisher stand in meinen Unterlagen, dass sich Belege in
  BuchhaltungsButler grundsaetzlich nicht loeschen lassen. Das stimmt so nicht — **ungebuchte** Belege
  lassen sich sehr wohl loeschen und sogar wiederherstellen. Nur GEBUCHTE Belege sind endgueltig
  (das ist gesetzlich so gewollt). Der alte Befund beruhte auf einem technischen Fehler auf meiner
  Seite, nicht auf einer Sperre des Anbieters.
- **Widerspruch in der System-Vergleichstabelle beseitigt.** An einer Stelle stand, Kontoumsaetze
  liessen sich bei BuchhaltungsButler nur von Hand einlesen — zwei Zeilen darueber stand das
  Gegenteil, korrekt belegt. Jetzt sagt die Tabelle einheitlich: Einlesen geht automatisch.
- **Neue Zeile in allen Vergleichen: „Kann ein Fehl-Import wieder korrigiert werden?"** Das ist der
  wichtigste Unterschied zwischen den Systemen und fehlte bisher voellig. Bei sevDesk kann ich falsch
  eingelesene Kontoumsaetze selbst wieder entfernen. Bei BuchhaltungsButler nicht — dort bleibt ein
  Fehl-Import stehen, bis du ihn von Hand loeschst.
- **Kontoauszuege einlesen funktioniert jetzt auch mit BuchhaltungsButler.** Der Ablauf ist derselbe
  wie bei sevDesk: PDF rein, ich lese es aus (mit Saldo-Probe), lege ein Konto an und trage die
  Umsaetze ein. Neu dokumentiert samt der Sicherheitsregeln.

**Wichtig fuer dich, wenn du BuchhaltungsButler nutzt**
- **Kontoauszuege immer nur EINMAL und immer in ein frisches, leeres Konto einlesen.** Grund: In einem
  PDF-Kontoauszug steht keine Transaktionsnummer. Ohne sie kann ich drei echte gleich hohe Gebuehren
  am selben Tag nicht sicher von einem versehentlichen Doppel-Import unterscheiden. Ich pruefe deshalb
  vor jedem Einlesen alle Konten und breche ab, wenn die Umsaetze schon irgendwo liegen.

**Wissensstand:** 31.07.2026

---

## v1.95.0 — BuchhaltungsButler: jetzt schwarz auf weiss, was automatisch geht und was du klicken musst

**Neue Funktionen**
- **Wenn du BuchhaltungsButler nutzt, kann Bruno dort jetzt nachweislich die komplette
  Vorarbeit uebernehmen:** Belege hochladen (mit den Daten aus dem Beleg-Scan), buchen mit
  Sachkonto und Steuerschluessel, mit der passenden Kontobewegung verknuepfen — und alles davon
  auch wieder rueckgaengig machen. Jeder dieser Schritte wurde am 31.07.2026 an echten Daten
  ausprobiert, nicht nur in der Anleitung nachgelesen.
- **Kontoauszuege als PDF landen jetzt auch in BuchhaltungsButler.** Der Weg Kontoauszug-PDF →
  Kontobewegungen wurde mit echten Auszuegen durchgespielt: 18 Buchungen, auf den Cent genau,
  Verwendungszweck vollstaendig uebernommen.
- **Dreifacher Schutz vor doppelten Kontobewegungen.** Bruno prueft vor jedem Import, ob dieselben
  Umsaetze schon in einem anderen Konto liegen, und bricht dann ab statt doppelt zu importieren.
  Wichtig, weil sich Kontobewegungen in BuchhaltungsButler spaeter nicht mehr loeschen lassen.

**Verbesserungen**
- **Neue Uebersicht „Was macht Bruno, was machst du?"** fuer BuchhaltungsButler. Sie sagt dir in
  Alltagssprache, welche drei Dinge du weiterhin selbst in der Oberflaeche erledigst
  (Konten/Kontobewegungen loeschen, DATEV-Export, Umsatzsteuer-Voranmeldung) — und belegt jede
  Aussage mit dem, was die Schnittstelle tatsaechlich geantwortet hat.
- **Falsche Angabe korrigiert:** In der System-Vergleichstabelle stand, der Bankabgleich ginge bei
  BuchhaltungsButler nur von Hand. Das stimmt nicht — Kontobewegungen lassen sich einlesen und mit
  Belegen verknuepfen. Korrigiert, mit Beleg.
- **Klare Ansage zum Kontenrahmen:** Bruno liest deinen kompletten Kontenrahmen aus und erkennt, ob
  SKR03 oder SKR04 eingestellt ist — auch selbst angelegte Sachkonten sieht er. Falls doch mal ein
  Konto nicht passt, lehnt BuchhaltungsButler die Buchung sauber ab, statt sie still falsch zu
  verbuchen.

**Unter der Haube**
- Fuenf neue Pruefwerkzeuge fuer den BuchhaltungsButler-Anschluss (alle mit Probelauf als
  Voreinstellung, scharf nur auf ausdrueckliche Anweisung).
- Beleg-Kontingent des Tarifs wird erkannt: ist die Grenze erreicht, stoppt Bruno sofort mit
  klarer Meldung, statt weiter erfolglos hochzuladen.

**Wissensstand:** 31.07.2026

---

## v1.94.1 — Kleinkram in der Zugangsdaten-Datei bereinigt

**Verbesserungen**
- **Im Kopf deiner `.env` stand eine Notiz, die nicht fuer dich gedacht war** (ein interner Hinweis
  des Herstellers samt Datum). Sie ist raus — die Datei erklaert jetzt nur noch, was DU tun musst.
- Die Online-Setup-Anleitung (`newways.ai/ki-buchhalter/setup`) und die PDF dort zeigen wieder
  denselben Stand wie dein Ordner: eine `.env` statt zweier Dateien, die aktuellen Ordnernamen
  und die realistische Einrichtungszeit.

**Unter der Haube**
- Neue Selbstpruefung: die Auslieferung bricht ab, wenn in der Zugangsdaten-Datei interne Notizen
  stehen. Weil diese Datei unveraendert bei dir landet, ist dort jede Zeile Kundentext.

---

## v1.94.0 — Einrichtung entwirrt: eine `.env` mit allen Zeilen, statt zwei verwirrender Dateien

**Verbesserungen**
- **Deine Zugangsdaten-Datei ist jetzt vollstaendig.** Bisher lag im Ordner eine fast leere `.env`
  und daneben eine `.env.example` mit allen Zeilen — verwirrend, und die Zeile `SEVDESK_API_KEY=`
  fehlte genau dort, wo du sie brauchst. Jetzt gibt es **eine** Datei namens `.env`, mit allen
  Zeilen und leeren Werten. Du oeffnest sie, traegst deinen Key ein, fertig. Nichts kopieren,
  nichts umbenennen.
- **Der Screenshot in der Anleitung ist wieder da.** Die Setup-Anleitung zeigt dir, wo du in
  sevDesk deinen API-Key findest — das Bild dazu fehlte im Paket und war ein leerer Platzhalter.
- **`BRUNO_UPDATE_TOKEN` ist als „schon eingetragen" gekennzeichnet.** Diese Zeile ist bei
  Auslieferung bereits gefuellt (damit Bruno seine Updates selbst holen kann). Vorher stand da
  ein Wert ohne Erklaerung — jetzt weisst du, dass du ihn nicht anfassen musst.
- **Weniger Dateien, die nur verwirren.** Im Ordner des Beleg-Scrapers lag eine Vorlagen-Datei,
  in die du nie etwas eintragen musst (der Standard-Weg braucht dort keinen Key). Sie ist raus.
  Willst du das Belege-Lesen anders einstellen, richtet Bruno das im Onboarding fuer dich ein.
- Die Setup-Anleitung wurde entsprechend korrigiert — sie beschrieb noch den alten Weg
  („kopiere die Vorlage"), der so nicht mehr stimmt. Das gilt jetzt an **allen** Stellen: in der
  Text-Anleitung, in der bebilderten PDF-Anleitung und in der Key-Uebersicht, die Bruno beim
  Einrichten heranzieht. Vorher haette dir die PDF noch den alten Weg gezeigt.
- **Eine Anleitung weniger zum Suchen.** Von der Setup-Anleitung lagen drei Varianten desselben
  Inhalts im Ordner. Jetzt bleiben zwei: die **PDF** (bebildert, zum Lesen oder Ausdrucken) und die
  **Text-Fassung** zum Nachschlagen. Die LIESMICH verlinkt beide direkt.
- **Der Update-Hinweis auf neue Zugangsdaten-Zeilen funktioniert wieder.** Kommt in einer neuen
  Version eine optionale Zeile fuer die `.env` dazu, sagt Bruno dir das beim Update. Dieser Hinweis
  blieb bisher stumm, weil er in einer Datei nachsah, die es bei dir gar nicht gibt.

**Unter der Haube**
- Ein neues Kontroll-Tor bricht die Auslieferung ab, wenn die `.env` unbrauchbar waere (fehlende
  Pflicht-Zeile) oder noch eine Vorlagen-Datei im Paket liegt. Solche Fehler erreichen dich damit
  nicht mehr — sie fallen vorher auf.
- Der Setup-Screenshot ist jetzt strukturell gegen stilles Verschwinden gesichert (drei neue
  Selbsttests). Beim Paketieren konnte er bisher unbemerkt herausfallen, obwohl alle Pruefungen
  gruen meldeten — genau der Fehler, der die Anbieter-Zuordnung in v1.93.0 betraf.

---

## v1.93.0 — 71 erprobte Buchungs-Zuordnungen neu im Paket + Erfahrungsberichte wieder sauber lesbar

**Neue Funktionen**
- **Die Vendor-Kontenzuordnung ist jetzt Teil deines Pakets** (`tools/sevdesk-connector/vendor-konto-map.json`):
  71 gaengige Anbieter (Anthropic, OpenAI, Google, Vercel, Adobe, Meta u.v.m.) mit live verifizierter
  Buchungs-Zuordnung — welches Konto, welche Steuerbehandlung, woran man den Anbieter auf dem
  Kontoauszug erkennt. Bruno bucht bekannte Anbieter damit ab dem ersten Tag treffsicherer.
  Sie war fuer dich gedacht, fehlte aber durch einen Auslieferungsfehler in den Paketen der
  letzten Wochen — das ist behoben, und zwei neue Selbstpruefungen verhindern die Wiederholung.
- **Wichtig dabei:** Die Zuordnung wird NIE blind uebernommen. Widerspricht dein Beleg der
  Zuordnung (Anbieter hat z.B. das Sitzland gewechselt), gewinnt immer der Beleg und der Fall
  geht in die Pruefliste statt gebucht zu werden. Die Map traegt dazu jetzt einen eigenen
  Warnhinweis (Kontenrahmen SKR04-verifiziert; bei SKR03 vorher pruefen).
- **Neues Praxis-Wissen:** Wie manuell geschriebene Ausgangsrechnungen (aus dem Banking-Tool
  statt ueber den Zahlungsdienstleister) korrekt gebucht und mit dem Zahlungseingang verknuepft
  werden — inkl. der Regel, dass bei Ist-Besteuerung das Zahlungsdatum zaehlt, nicht das
  Rechnungsdatum (`system/LEARNINGS-SHARE.md`).

**Verbesserungen**
- **Die Erfahrungsberichte (Learnings) lesen sich wieder wie von Menschen geschrieben.** Eine zu
  grobe Text-Ersetzung hatte in frueheren Paketen Saetze verstuemmelt ("fuer der Nutzer",
  "mit die Zahlen begruendet") und an einer Stelle sogar den Sinn verdreht. Alle Fach-Inhalte
  waren davon nie betroffen — jetzt stimmt auch die Sprache wieder.
- Zwei defekte Dateien ohne Nutzen fuer dich wurden aus dem Paket entfernt (Einmal-Skripte des
  Herstellers; eine davon war durch die Anonymisierung nicht mehr lauffaehig).

**Unter der Haube**
- Die Auslieferung prueft sich jetzt selbst haerter: JEDE mitgelieferte Code-Datei wird vor dem
  Release auf Lauffaehigkeit geprueft (vorher nur Stichproben), und ein neues Kontroll-Tor
  verhindert, dass Produkt-Dateien beim Paketieren still verloren gehen.

**Wissensstand:** 2026-07-29

## v1.92.0 — Beim ersten Start fuehrt Bruno dich jetzt direkt in die Einrichtung

**Was sich fuer dich aendert**
Wer Bruno frisch installiert hatte und `/ki-buchhalter` eingab, bekam sofort das volle Menue mit
allen 15 Modi zu sehen — obwohl noch gar kein Profil eingerichtet war. Wer dann "1" tippte, startete
den Buchungs-Workflow, ohne dass Bruno wusste, mit wem er es zu tun hat: Rechtsform, Umsatzsteuer-
Status, Kontenrahmen, Buchhaltungssystem. Alles unbekannt. Das ist jetzt behoben.

- **Ohne eingerichtetes Profil zeigt Bruno kein Menue mehr**, sondern begruesst dich und schlaegt
  die Einrichtung vor (Modus 13, einmalig ein paar Minuten). Du kannst dir das volle Menue trotzdem
  anzeigen lassen — es ist eine Empfehlung, keine Sperre.
- **Zweite Absicherung:** Waehlst du ohne Profil einen Buchungs-Modus, startet dieser nicht mehr
  einfach los, sondern verweist auf die Einrichtung. Vorher lief er an.
- **Ein leeres Profil zaehlt jetzt als "nicht eingerichtet"** — vorher reichte es, dass die Datei
  ueberhaupt existierte.

**Unter der Haube**
Die Regel "erst einrichten, dann buchen" gab es schon; sie griff nur nie. Zwei Ursachen: Die
Pruefung, ob ein Profil existiert, meldete wegen eines Shell-Details nie einen Fehler — die Warnung
blieb also stumm. Und die Regel selbst stand im Skill an einer Stelle, die erst nach der Menue-
Ausgabe gelesen wurde. Beides ist korrigiert, die Pruefung laeuft jetzt als Erstes.

- **Personalisierte Ordnernamen entfallen.** Das ausgelieferte Paket heisst fuer alle gleich
  (`ki-buchhalter-bruno-v<version>`). Die Initialen im Ordnernamen liessen sich ohnehin in Sekunden
  umbenennen und haben nie etwas geschuetzt. Deine Lizenz-Kennzeichnung in `LIZENZ.md` bleibt.

## v1.91.1 — Selbsttests laufen jetzt mit Musterdaten statt echten Kontodaten

**Was sich fuer dich aendert**
Bruno bringt Selbsttests mit ("Kanarien"), die nach jedem Update pruefen, ob die Buchungs-Logik
noch stimmt. Zwei dieser Tests arbeiteten mit echten Konto- und Steuernummern als Beispieldaten.
Fuer die Pruefung selbst war das egal, sauber ist es trotzdem nicht: Testdaten werden mitgeliefert.

- **Alle Selbsttests nutzen jetzt Musterdaten** (Muster-IBANs, "Max Mustermann", Dummy-Steuernummern).
- **Die Pruefungen selbst sind unveraendert** — dieselben 13 bzw. 8 Testfaelle, alle bestehen weiterhin.
  Getauscht wurden ausschliesslich die Beispielwerte, nicht die Logik dahinter.
- Du merkst im Alltag nichts davon. Der Punkt ist Hygiene: in ausgeliefertem Code haben echte
  Kontonummern nichts verloren, auch nicht als Testbeispiel.

## v1.91.0 — Health-Check erkennt jetzt widerspruechliche Doppel-Belege

**Was sich fuer dich aendert**
Manche Rechnungen liegen doppelt vor (z.B. als "Invoice" und als "Receipt" derselben Zahlung) —
und in seltenen Faellen widerspricht sich dabei der erkannte Steuersatz, obwohl beide Dokumente
dieselbe Zahlung beschreiben. Das kann zu falscher Umsatzsteuer fuehren, wenn die falsche Datei
gebucht wird.

- **Neue Pruefung (Dimension 24):** findet automatisch, wenn zwei Dateien derselben Rechnung
  gleichen Betrag/gleiche Waehrung, aber unterschiedlichen Steuersatz zeigen.
- **13 solcher Faelle im Bestand gefunden** (Skool, Anthropic, Gumroad, Gamma, Replit) — jeweils
  mit klarer Anweisung, welche der beiden Dateien geprueft werden sollte.
- Ausfuehrlich getestet: 7 automatisierte Testfaelle, alle bestehen; keine Fehlalarme bei bereits
  korrekt erfassten Rechnungen.

## v1.90.0 — Higgsfield-Rechnung nachgefordert und gebucht

**Was sich fuer dich aendert**
Eine Rechnung war nur als Zahlungsbeleg vorhanden, nicht als vollstaendige Rechnung — fuer den
Vorsteuerabzug reicht das rechtlich nicht. Nach Nachforderung beim Anbieter (du hast dich eingeloggt,
die richtige Rechnung geholt) ist sie jetzt korrekt gebucht.

- **Higgsfield Inc. (262,80 EUR, 01.12.2025):** vollstaendige Rechnung ersetzt den alten Zahlungsbeleg,
  gebucht und mit der Kontobewegung verknuepft.

## v1.89.0 — Waehrung wird jetzt am Beleg geprueft, statt Euro anzunehmen

**Was sich fuer dich aendert**
Gestern kam heraus, dass 132 Dollar-Belege als Euro erfasst waren. Repariert war das schnell — die
Ursache aber nicht. Genau die ist jetzt behoben, und zwar an der Quelle: beim Auslesen des Belegs,
bevor irgendetwas gebucht wird.

**Warum es ueberhaupt passiert ist**
Zwei Dinge kamen zusammen:
1. In der Anweisung, mit der Bruno einen Beleg ausliest, stand bei der Waehrung der feste Wert
   "EUR" — bei jedem anderen Feld steht dort eine Beschreibung ("Firmenname des Rechnungsstellers",
   "Datum im Format JJJJ-MM-TT"). Nur die Waehrung war vorgegeben statt gefragt. Sie wurde also
   uebernommen, ohne den Beleg anzusehen.
2. An sechs weiteren Stellen im Ablauf wurde "Euro" nachtraeglich eingesetzt, wenn nichts erkannt
   wurde. Selbst eine ehrliche Luecke wurde so wieder zu einem falschen Wert.

**Was jetzt passiert**
- **Bruno wird nach der Waehrung gefragt, statt sie vorgesetzt zu bekommen** — mit dem klaren
  Hinweis, sie vom Beleg zu lesen und im Zweifel nichts einzutragen.
- **Gegenprobe am Dokument:** Unabhaengig davon zaehlt Bruno die Waehrungszeichen im PDF-Text.
  Steht dort nur "$", der Beleg wurde aber als Euro erfasst, wird das korrigiert — und dir
  angezeigt, nicht stillschweigend gemacht.
- **Mehrdeutiges wird nie geraten:** Nennt ein Beleg beide Waehrungen, landet er zur Ansicht bei
  dir. Nennt er gar keine, bleibt das Feld leer statt falsch gefuellt.
- **Kein Buchen ohne Waehrung:** Ein Beleg ohne erkannte Waehrung wird nicht mehr hochgeladen.
  Lieber wartet ein Beleg, als dass ein falscher Betrag in deiner Buchhaltung landet.

**Der Grundsatz dahinter:** Bei Geld ist ein falscher Standardwert schlimmer als eine Luecke. Die
Luecke faellt auf und wird gemeldet — der falsche Wert sieht aus wie ein geprueftes Ergebnis.

**Abgesichert durch neun Testfaelle**, darunter der reale Instantly-Beleg, die Gegenprobe mit
deutschen Euro-Rechnungen (kein Fehlalarm) und ein Test, der absichtlich den alten Zustand
herstellt — er muss fehlschlagen, sonst wuerde die Pruefung den historischen Fehler nicht fangen.

**Wissensstand:** 2026-07-27

---

## v1.88.0 — 7 Steuerberater-Rechnungen (2.294,62 EUR) haengengeblieben, jetzt gebucht

**Was sich fuer dich aendert**
7 Rechnungen deines Steuerberaters (HSP Steuer) waren seit Monaten haengengeblieben — sie waren
korrekt an dich adressiert, aber das System hatte den Empfaengernamen beim ersten Einlesen nicht
erkannt und darum vorsichtshalber zur Kontrolle geparkt. Nach Pruefung gegen das Original-PDF war
das ein reiner Lese-Fehler, kein echtes Problem.

- **Alle 7 gebucht und mit der Kontobewegung verknuepft** (03.2025 bis 08.2025), darunter die
  bisher groesste offene Rechnung ueberhaupt: 1.989,68 EUR vom 03.05.2025.
- Gesamtsumme: 2.294,62 EUR.

## v1.87.0 — Bonder-Rechnung per Waehrungskurs gefunden, AnyMailFinder-Vendor ergaenzt

**Was sich fuer dich aendert**
Eine Rechnung war ueber Monate nicht mit der Bankbewegung verknuepft, weil der Zahlungsdienstleister
auf dem Kontoauszug einen fremden Namen anzeigte. Ueber den Dollar-Euro-Umrechnungskurs zum
Zahltag liess sich die richtige Bewegung trotzdem eindeutig finden.

- **Gabriel Neuman Bonder (383,66 EUR):** Kontobewegung zeigte "SERVICIO RASTREO IOT" statt dem
  echten Namen — Zahlungsdienstleister-Effekt. Ueber den EZB-Tageskurs am Zahltag (448 USD x 0,8540)
  eindeutig bestaetigt und verknuepft. Die zweite Bonder-Rechnung (224,90 EUR) bleibt offen — keine
  passende Kontobewegung gefunden.
- **AnyMailFinder (3 Rechnungen, 88,10 EUR):** lagen lokal vor, waren aber nie in die Buchhaltung
  hochgeladen. Neu: der Anbieter rechnet zweigleisig ab — Credit-Kaeufe ohne Umsatzsteuer (Steuer
  im Ausland), das Monats-Abo dagegen MIT deutscher Umsatzsteuer. Beide Faelle sind jetzt automatisch
  korrekt erkennbar.

## v1.86.0 — 132 Dollar-Belege waren als Euro erfasst (Zeitbombe vor dem Buchen entschaerft)

**Was sich fuer dich aendert**
Rechnungen in US-Dollar wurden teilweise als Euro-Belege erfasst. Der Betrag stimmte, aber die
Waehrung war falsch etikettiert. Wird so ein Beleg gebucht, landet der Dollar-Betrag als Euro in
der Buchhaltung — Betriebsausgabe zu niedrig, Vorsteuer falsch.

- **Gefunden:** 132 Belege (Instantly, OpenAI, Airtable, Anthropic, OpenRouter, Namecheap, Loom u.a.)
- **Davon 107 noch ungebucht** — zusammen 4.314 $. Das war die eigentliche Gefahr: haetten wir sie
  gebucht, waere der Fehler erst entstanden. Alle korrigiert, bevor etwas passiert ist.
- **25 waren bereits gebucht** — dort wurde beim Buchen korrekt umgerechnet, es ging nur um das
  Etikett. Gesamtabweichung ueber alle Faelle: **1,90 €**, reine Wechselkurs-Differenzen.
- **14 mehrdeutige Faelle** (PDF nennt beide Waehrungen) wurden bewusst NICHT automatisch geaendert,
  sondern zur Ansicht gelistet.

**Damit es nicht wieder passiert: neue Pruefung "Waehrungs-Plausibilitaet"**
Bruno vergleicht ab sofort die erfasste Waehrung mit dem, was im PDF steht. Weicht es ab, ist das
ein kritischer Hinweis — und zwar **bevor** gebucht wird, nicht erst danach. Sechs Testfaelle
sichern die Pruefung ab, darunter der umgekehrte Fall (Euro-Beleg als Dollar erfasst) und der
Nachweis, dass korrekte Euro-Belege keinen Fehlalarm ausloesen.

**Warum es passieren konnte**
Die Belegerkennung setzte "Euro" als Standardwert, wenn die Waehrung nicht zweifelsfrei erkennbar
war. Bei Geld ist ein falscher Standardwert schlimmer als eine Luecke: Die Luecke faellt auf, der
falsche Wert sieht aus wie ein geprueftes Ergebnis.

**Nebenbei korrigiert:** Ein Instantly-Beleg trug einen erfundenen Steuersatz von 19 %, obwohl das
PDF gar keine Umsatzsteuer ausweist (US-Anbieter, Reverse-Charge). Das erzeugte einen Fehlalarm
gegen eine voellig korrekte Buchung.

**Ergebnis:** kritische Meldungen von 27 auf **2**.

**Wissensstand:** 2026-07-26

---

## v1.85.0 — 5 Belege trugen denselben falschen Betrag (OCR-Fehler von Anfang 2025 gefunden und korrigiert)

**Was sich fuer dich aendert**
5 Werbekosten-Belege desselben Tages hatten alle exakt denselben Betrag eingetragen, obwohl jeder Beleg
eigentlich einen anderen echten Betrag zeigt. Ursache: ein alter OCR-Fehler beim ersten Einlesen (nicht
der aktuelle Code) — vermutlich wurde der erste gelesene Betrag versehentlich fuer alle 5 Belege eines
Mail-Stapels uebernommen, statt jeden einzeln zu lesen.

- **Konkret gefunden + korrigiert:** 5 Meta-Werbekosten-Belege vom 05.02.2025, alle faelschlich mit
  5,02€ eingetragen. Die echten Betraege (per Beleg gegengelesen): 2,00€ / 2,00€ / 3,00€ / 3,00€ / 5,00€.
  Keiner davon war vorher schon final gebucht — nur der Entwurf wurde korrigiert, kein Risiko fuer
  bereits abgeschlossene Buchungen.
- **Systematisch nachgeprueft:** 11 weitere Verdachtsfaelle (gleicher Anbieter + gleicher Betrag am
  gleichen Tag) einzeln gegen die Original-Belege gegengelesen. Alle 11 waren in Ordnung — echte,
  unterschiedliche Rechnungen mit zufaellig gleicher Summe (z.B. wiederkehrende Monatsabos). Nur der
  eine Meta-Fall war tatsaechlich fehlerhaft.
- **Zusaetzliche Klarheit gewonnen:** diese 5 Meta-Belege sind offiziell Zahlungsbestaetigungen, keine
  Rechnungen (steht wortwoertlich so auf dem Beleg) — ein Vorsteuerabzug daraus waere ohnehin nicht
  sicher. Wird jetzt als Hinweis angezeigt, nicht mehr als "Beleg fehlt".

## v1.84.0 — Bestaetigte Einzelfaelle wurden gespeichert, aber nie beachtet

**Was sich fuer dich aendert**
Wenn du einen Pruef-Hinweis einmal geprueft und als unbedenklich bestaetigt hast, wird er in einer
Merkliste hinterlegt — damit er beim naechsten Mal nicht erneut als kritisch erscheint. Diese Liste
wurde zwar **geschrieben**, aber von der Pruefung **nie gelesen**. Bestaetigte Faelle blieben damit
fuer immer rot, egal wie gruendlich sie geprueft waren.

- **Jetzt:** Bestaetigte Einzelbelege werden korrekt auf gruen herabgestuft — mit deiner Begruendung
  sichtbar im Bericht. Nicht versteckt, sondern nachvollziehbar entschaerft.
- **Eng gefasst:** Die Bestaetigung gilt nur fuer die Pruefart, fuer die du sie erteilt hast. Taucht am
  selben Beleg ein *anderer* Fehler auf, bleibt er sichtbar.

**Konkret aufgeklaert: die 10 Qonto-Faelle**
Zehn Belege standen als "Status 750 trotz Vollzahlung" auf kritisch — bisher mit dem Hinweis
"technische Grenze, kosmetisch akzeptieren". Auf Nachfrage nachgerechnet: Qonto bucht die
Kontofuehrungsgebuehren nicht einmal im Monat ab, sondern in vielen Einzelbetraegen (Grundgebuehr
plus Cent-Betraege je Ueberweisung) und stellt am Monatsende **eine** Sammelrechnung. Die Summe der
Einzelabbuchungen ergibt in allen zehn Faellen **exakt auf den Cent** den Rechnungsbetrag —
13 Abbuchungen = 33,68 €, 17 Abbuchungen = 30,08 €, kein Cent Abweichung.

Das ist buchhalterisch voellig korrekt: eine Rechnung, viele Zahlungen. Der Status 750 ist die
richtige Antwort deines Systems darauf, kein Defekt. Alle zehn sind jetzt mit dem Rechenbeweis
dokumentiert.

**Ergebnis:** kritische Meldungen von 15 auf **0** — jede einzelne nachgewiesen, keine weggeklickt.

**Wissensstand:** 2026-07-25

---

## v1.83.0 — Dollar-Rechnungen wurden faelschlich als Buchungsfehler gemeldet

**Was sich fuer dich aendert**
Bei Rechnungen in Fremdwaehrung (Dollar-Abos wie Skool, Paddle, make) hat die Pruefung Alarm
geschlagen, obwohl alles korrekt gebucht war. Grund: Sie verglich den gebuchten Euro-Betrag mit dem
offiziellen Tageskurs der Europaeischen Zentralbank. Deine Bank rechnet aber mit ihrem **eigenen**
Kurs, der regelmaessig ein bis zwei Prozent abweicht — und genau dieser Betrag wurde abgebucht.

- **Konkret:** Eine Rechnung ueber 19,00 USD wurde mit 16,31 € gebucht. Nach EZB-Kurs waeren es
  13,73 € gewesen — das Verhaeltnis sah zufaellig aus wie ein bekannter Steuersatz-Fehler, und die
  Pruefung meldete faelschlich einen kritischen Buchungsfehler.
- **Jetzt:** Existiert eine echte Kontobewegung ueber genau den gebuchten Betrag, gilt die Buchung
  als bank-bestaetigt und wird gruen ausgewiesen — mit dem Nachweis im Bericht. Denn gebucht wird,
  was tatsaechlich geflossen ist.
- **Wirkung:** Die kritischen Meldungen sanken von 15 auf 10. Die verbleibenden 10 sind eine bekannte
  Grenze deines Buchhaltungssystems bei Teilzahlungen, kein Fehler.

**Wichtig:** Die Regel wurde nicht abgeschwaecht. Fehlt die passende Kontobewegung, schlaegt sie
weiterhin an — sie prueft jetzt nur gegen die Realitaet statt gegen einen Kurs, der nie gezahlt wurde.

**Wissensstand:** 2026-07-25

---

## v1.82.1 — DHL/Deutsche Post als selben Absender erkannt

Kleine Ergänzung zum vorigen Fix: DHL-Paketrechnungen werden bei dir vom Konzernkonto "Deutsche Post AG"
abgebucht, nicht von "DHL". Der Abgleich kennt jetzt beide als denselben Absender.

## v1.82.0 — Beleg-Abgleich erkannte Zahlungsdienstleister-Namen nicht

**Was sich fuer dich aendert**
Auf deiner Rechnung steht oft der Firmenname des Anbieters (z.B. "Meta for Business"), auf deinem
Kontoauszug aber der Name des Zahlungsabwicklers (z.B. "FACEBK") — beides dieselbe Zahlung, aber
unterschiedlich benannt. Der Abgleich hat nur nach dem Rechnungsnamen gesucht und deshalb nichts
gefunden, obwohl die passende Kontobewegung laengst da war.

- **Konkret gefunden:** 8 Meta/Facebook-Werbekosten-Rechnungen und mehrere Paddle-Rechnungen (Verkaufs-
  plattform heisst auf der Rechnung anders als der Zahlungsabwickler auf dem Konto) blieben deswegen
  als "offen" stehen. Bei ~40 % der noch offenen Faelle lag genau dieses Problem vor.
- **Jetzt:** Der Abgleich kennt Meta/Facebook als dasselbe Unternehmen und erkennt bei Plattform-Namen
  automatisch den Zahlungsabwickler dahinter (z.B. Domain-Endung wie ".com" vs. ".net" spielt keine
  Rolle mehr).
- **12 Belege sofort automatisch bzw. gezielt nachverknuepft** nach dem Fix.

## v1.81.0 (2) — Beleg-Abgleich uebersah bereits vorkategorisierte Kontobewegungen

**Was sich fuer dich aendert**
Manche Kontobewegungen sind von deiner Bank/sevDesk automatisch vorkategorisiert, aber noch keinem
Beleg zugeordnet. Der Abgleich hat diese komplett ignoriert — er hat nur nach ganz "frischen" (nicht
kategorisierten) Bewegungen gesucht. Ergebnis: Belege blieben "offen", obwohl die passende Zahlung
laengst im System war, nur eben schon leicht vorsortiert.

- **Konkret gefunden:** 13 Monats-Abo-Rechnungen (durchgehend derselbe Anbieter) hatten trotz exakt
  passendem Betrag und Datum keinen Treffer — die zugehoerigen Kontobewegungen waren alle in diesem
  vorkategorisierten Zwischenzustand.
- **Jetzt:** Der Abgleich erkennt auch vorkategorisierte, aber noch unverknuepfte Bewegungen als
  gueltige Kandidaten. Bereits fertig verbuchte Bewegungen bleiben weiterhin ausgeschlossen (Schutz
  vor Doppel-Verknuepfung bleibt bestehen).
- **11 Belege sofort automatisch nachverknuepft** nach dem Fix.

## v1.81.0 — Der Beleg-Abgleich suchte im falschen Konto (und sagte nichts)

**Was sich fuer dich aendert**
Wenn du mehrere Bankkonten hast — etwa weil du die Bank gewechselt hast — konnte der Abgleich
zwischen Belegen und Zahlungen ins Leere laufen: Er durchsuchte immer nur **ein** Konto. Fuer ein
zurueckliegendes Jahr war das oft das falsche. Das Ergebnis sah dann aus wie "es passt nichts
zusammen", obwohl er schlicht am falschen Ort gesucht hat. Ohne Fehlermeldung.

- **Konkret gefunden:** 214 von 218 offenen Zahlungen aus 2025 lagen auf einem inzwischen
  geschlossenen Konto, waehrend der Abgleich auf dem aktuellen Konto suchte. Deshalb bewegte sich
  dieser Rueckstand wochenlang nicht.
- **Jetzt:** Bruno bricht mit einer klaren Ansage ab, wenn das eingestellte Konto keine Zahlung aus
  dem gewaehlten Jahr enthaelt — inklusive Hinweis, wie du das richtige Konto waehlst. Ein stiller
  Leerlauf ist bei Geld nicht hinnehmbar.
- **Nebenbei behoben:** Die Angabe `--jahr` wurde vom Abgleich gar nicht ausgewertet.

**Neu: Offene-Posten-Liste auf Knopfdruck**
Die Uebersicht "welche Kontobewegung hat noch keinen Beleg" (`OFFENE-POSTEN.md`) verwies auf einen
Befehl zum Neuerzeugen — den es nie gab. Die Datei war handgepflegt und veraltete still. Jetzt gibt
es das Werkzeug wirklich: `node tools/sevdesk-connector/offene-posten.mjs` erzeugt die Liste
jederzeit frisch aus deinem Buchhaltungssystem, getrennt nach Jahr, Anbieter und Beschaffungsweg —
inklusive der Faelle, die gar keinen Beleg brauchen (eigene Umbuchungen, Zahlungsdienstleister).

**Wissensstand:** 2026-07-25

---

## v1.80.0 — Der Buchhaltungs-Report sieht jetzt aus wie gedacht

**Was sich fuer dich aendert**
Der Report aus Modus 4 („Wie steht meine Buchhaltung da?") war bisher eine schlichte Liste aus Zahlen und
Tabellen. Das abgenommene Design lag zwar schon im Produkt, wurde von diesem Report aber gar nicht genutzt.
Jetzt bekommst du dieselbe Uebersicht wie im Lauf-Report:

- **Aktions-Banner oben:** steht ganz oben, wenn wirklich etwas fuer dich zu tun ist — und bleibt weg,
  wenn nichts ansteht (kein erfundener Handlungsbedarf).
- **Drei Kennzahlen auf einen Blick:** gebuchte Belege, noch offene Belege, Kontoumsaetze ohne Beleg.
- **Sprungnavigation:** ein Klick zu Ausgaben, Belegen, Umsatzsteuer oder Konten — mit farbigem Punkt,
  der zeigt, wo etwas offen ist.
- **Grosse Uebersicht mit Quoten-Ring:** wie viel Prozent deiner Belege mit einem Kontoumsatz verknuepft
  sind, plus die wichtigsten Summen daneben.
- Die Produkt-Version steht jetzt im Report-Kopf, damit du spaeter siehst, mit welchem Stand er erzeugt wurde.

**Unter der Haube**
Der Report-Generator konnte diese Bausteine laengst — der Modus-4-Report hat sie nur nie aufgerufen.
Alle Zahlen stammen aus denselben live gelesenen sevDesk-Daten wie zuvor, es kommt keine zweite Wahrheit
dazu. Ein Anzeigefehler in den Kopfzeilen-Pills ist mit behoben.

## v1.79.0 — Rechnungen aus Anbieter-Portalen holen (wenn's keine Mail-Rechnung gibt)

**Was sich fuer dich aendert**
Manche Anbieter (OpenAI ist der haeufigste Fall) schicken bei Kartenzahlung nur eine Mail-Benachrichtigung
("Zahlung erfolgt"), aber KEINE echte Rechnung als Anhang. Die richtige Rechnung liegt dann nur in deinem
eigenen Kundenkonto im Browser. Bruno konnte solche Rechnungen bisher gar nicht automatisch finden.

- **Konkret gefunden:** 18 fehlende OpenAI-Rechnungen ueber 2025 und 2026 (ChatGPT-Plan + API-Nutzung),
  weil die Mails dazu keinen PDF-Anhang hatten.
- **Neu:** Bruno kann jetzt gezielt Rechnungen aus dem Kundenportal eines Anbieters holen — du navigierst
  einmal in deinem eigenen Browser zu deiner Rechnungs-Uebersicht, Bruno holt sich dann automatisch die
  PDFs. Kein Passwort wird dabei je an Bruno weitergegeben.
- **Fuer wen:** funktioniert bei jedem Anbieter, der Rechnungen ueber Stripe abwickelt (sehr verbreitet
  bei SaaS-Tools) — nicht nur OpenAI.
- Alle 18 nachgezogenen OpenAI-Rechnungen wurden direkt gebucht und mit deinen Kontobewegungen verknuepft.

**Unter der Haube**
Der Anbieter "OpenAI" fehlte komplett in der internen Steuer-Zuordnung (trotz frueher schon gebuchter
Belege) — jetzt ergaenzt (US-Firmensitz und Irland-Firmensitz getrennt, mit korrektem Reverse-Charge).

## v1.78.0 — Erloes-Rechnungen mit Unterstrich blieben faelschlich "offen"

**Was sich fuer dich aendert**
Rechnungen, die du selbst ausgestellt hast (z.B. an Kunden ueber dein Geschaeftskonto), zeigte Bruno
manchmal weiter als "noch nicht gebucht" an, obwohl sie laengst gebucht UND mit dem Kontoumsatz
verknuepft waren. Der Beleg blieb dadurch im falschen Ordner liegen statt im Archiv.

- **Konkret gefunden:** Rechnungsnummern mit Unterstrich (z.B. "RE2025_0003") wurden beim Abgleich
  mit deinem System anders geschrieben als im Original ("RE20250003" ohne Unterstrich) — fuer Bruno
  sahen das wie zwei verschiedene Rechnungen aus.
- **Wie es jetzt geprueft wird:** Der Abgleich ignoriert jetzt auch Unterstriche (wie schon bei
  Leerzeichen und Bindestrichen), erkennt beide Schreibweisen als dieselbe Rechnung.
- **Betroffen:** 1 Beleg sofort korrekt einsortiert nach dem Update.

## v1.77.0 — Kartenzahlungen galten faelschlich als "eigene Umbuchung" (Belege waren unsichtbar)

**Was sich fuer dich aendert**
Bruno hat bei manchen Kartenzahlungen faelschlich angenommen, du haettest nur Geld zwischen deinen
eigenen Konten verschoben — und sie deshalb von der Belegpflicht ausgenommen. Diese Ausgaben tauchten
dann nirgends mehr als "Beleg fehlt" auf, obwohl es ganz normale Betriebsausgaben waren. Betroffen
war jede Zahlung, bei der deine Bank deinen Namen als Karteninhaber in den Verwendungszweck schreibt
(bei Qonto zum Beispiel bei jeder Kartenzahlung).

- **Konkret gefunden:** 14 Ausgaben des Jahres 2025 an OpenAI, Facebook, vidIQ, Outscraper, Gumroad
  und Kie.ai galten als "eigene Umbuchung, kein Beleg noetig". Real waren es Betriebsausgaben —
  mit Vorsteuer, die dir sonst verloren gegangen waere.
- **Warum das passierte:** Bruno suchte deinen Namen im gesamten Text der Buchung, also auch im
  Verwendungszweck. Dort steht bei Kartenzahlungen aber der *Zahler* (du), nicht der Empfaenger.
- **Wie es jetzt geprueft wird:** Entscheidend ist, **wer das Geld bekommt** — nicht, welcher Name
  irgendwo im Text auftaucht. Eine Umbuchung gilt nur dann als deine eigene, wenn die Gegen-IBAN auf
  deiner bestaetigten Kontenliste steht oder der Empfaenger exakt dein hinterlegter Name ist. Passt
  nur der Name ohne IBAN, legt Bruno den Fall vor statt still zu entscheiden.
- **Du siehst jetzt mehr:** Zu jeder offenen Buchung stehen Empfaenger, Gegen-IBAN, Betrag,
  Verwendungszweck und die Begruendung des Urteils — damit du Grenzfaelle selbst beurteilen kannst.

**Unter der Haube**
Die Pruefung nutzt jetzt dasselbe geprüfte Sicherheits-Gate wie das Werkzeug zum Privat-Markieren
(`system/_lib/privat-gate.mjs`) statt einer eigenen, schwaecheren Logik — eine Wahrheit statt zwei.
14 neue Kanarien sichern das ab, darunter die fuenf realen Fehlfaelle und ein Sabotage-Test, der
beweist, dass die alte Logik durchgefallen waere. Zusaetzlich abgesichert: ein stiller
Konfigurations-Leerlauf (ein gross/klein geschriebenes Feld in deinem Profil wurde nicht gelesen,
wodurch die Pruefung pauschal nichts mehr entschied — ohne Fehlermeldung).

**Wissensstand:** 2026-07-25

---

## v1.76.0 — Belegerkennung: die Fehlerklasse "Wort statt Struktur" geschlossen + Kanarien-Sammellauf

**Was sich für dich ändert**
Nachtrag zur letzten Verbesserung: Dieselbe Ursache, aus der eine US-Quittung fälschlich in
"unklar" landete, steckte noch an zwei weiteren Stellen — überall dort, wo Bruno ein einzelnes
Wort im Text als Beweis wertete, statt auf die Stelle zu schauen, an der es steht.

- **"Rechnungsnummer: RE20250003"** stand als Verwendungszweck einer Überweisung mitten in einem
  Kontoauszug (Zeile 327) und wurde als Rechnungs-Merkmal gewertet. Bruno prüft die Rechnungsnummer
  jetzt nur noch im Kopfbereich eines Dokuments — dort steht sie bei echten Rechnungen ausnahmslos
  (an 71 von 71 geprüften Belegen in den ersten 19 Zeilen).
- **Deine eigene Umsatzsteuer-ID** steht im Briefkopf deiner eigenen Kontoauszüge und zählte als
  Rechnungs-Merkmal. Auf einer Eingangsrechnung zählt aber die ID des Absenders — deshalb wertet
  Bruno jetzt nur fremde IDs. Deine eigene ID liest er einmal aus dem Profil und blendet sie aus.

**Warum das wichtig ist**
Jeder dieser Fälle konnte einen Kontoauszug wie eine Rechnung aussehen lassen — und ein Kontoauszug,
der als Betriebsausgabe gebucht wird, ist ein echter Buchungsfehler. Die Änderung macht Bruno nicht
leichtgläubiger, sondern genauer.

**Neu: ein Befehl prüft alle Schutzregeln auf einmal**
Es gab 18 kleine Selbsttests ("Kanarien"), die jeweils eine Buchungs- oder Erkennungsregel absichern —
aber verstreut über fünf Ordner und nur von Hand einzeln zu starten. Jetzt bündelt sie
`system/_bin/kanarien.mjs` in einem Lauf, und der Nachtlauf ruft ihn automatisch VOR dem ersten
Buchen auf. Ist eine Schutzregel kaputt, siehst du es künftig, bevor gebucht wird — nicht danach.

**Unter der Haube**
Belegerkennung um 7 Kanarien erweitert (jetzt 13), plus ein neuer USt-ID-Leser, der echte
ID-Syntax von Fließtext trennt (getestet gegen 8 reale Formate). Regression auf dem Bestand:
818 Belege + 25 Kontoauszüge geprüft, keine Verschlechterung; alle Änderungen per Sabotage-Test
verifiziert (Schutz entfernt → Test schlägt fehl).

## v1.75.0 — Fehlende Rechnungen werden nicht mehr übersehen (wichtiger Fund)

**Was sich für dich ändert**
Bruno prüft für jede Kontobewegung, ob die passende Rechnung schon bei dir liegt. Dabei konnte er
sich bisher von einem ähnlichen Firmennamen täuschen lassen — und hat eine fehlende Rechnung dann
fälschlich als „ist schon da" abgehakt. Sie tauchte danach in keiner Liste mehr auf. Das ist behoben.

**Der konkrete Fall (in echten Daten gefunden)**
Eine Zahlung an **Claude.ai** wurde von einer Rechnung der **„Claude Code Academy"** (ein völlig
anderer Anbieter, ein Kurs) für erledigt erklärt — beide Namen fangen mit „Claude" an, und ein
gemeinsames Wort reichte dem Abgleich. Folge: **10 Zahlungen über 530,95 € galten als belegt,
obwohl die Rechnungen fehlten.** Nach dem Fix stehen sie wieder auf der Beschaffungsliste.

**Verbesserungen**
- **Strengerer Namensabgleich.** Ein einzelnes gemeinsames Wort genügt nicht mehr. Die Namen
  müssen darüber hinaus zusammenpassen — trägt die Gegenseite noch zwei eigenständige
  Namenswörter, ist es eine andere Firma. Schreibweisen wie „NAME-CHEAP.COM" ↔ „Namecheap"
  oder Zahlungs-Präfixe wie „LS* LEARNINGSUITE" werden weiterhin korrekt erkannt.
- **Neue Schutzprüfung.** 29 Testfälle sichern beide Fehlerrichtungen ab: Rechnungen dürfen weder
  fälschlich als vorhanden gelten (versteckte Lücke) noch fälschlich als fehlend (unnötige Sucherei).
  Wird die Regel je aufgeweicht, schlägt die Prüfung sofort an.
- **Rechnungen von Stripe-Seiten holen.** Viele Anbieter (u.a. Anthropic) stellen ihre Rechnungen
  über Stripe bereit. Bruno lädt dort jetzt das **echte Anbieter-PDF** herunter — kein Bildschirmfoto —
  und prüft danach automatisch, ob Betrag und Rechnungsnummer im Dokument lesbar sind. Ist das PDF
  leer oder abgeschnitten, wird es nicht einsortiert, sondern neu geholt.

**Unter der Haube**
- `wortanteilPasst`-Gate in `fehlende-rechnungen.mjs` + `canary-vendor-match.mjs` (29 Fälle,
  Sabotage-verifiziert: Gate entfernt → Prüfung schlägt fehl).
- Neu `tools/email-invoice-scraper/src/stripe-invoice-fetch.mjs` (eigener Weg statt `paddle-fetch.mjs`:
  Stripe-Seiten erreichen `networkidle` nie und liefen in einen Timeout).

**Wissensstand:** 2026-07-23

## v1.74.0 — Umzug von einer alten Version: deine Daten kommen automatisch mit

**Was sich für dich ändert**
Wenn du eine komplett neue Version bekommst statt eines Updates (das passiert bei großen
Sprüngen — sicherer, weil danach garantiert alles zusammenpasst), musstest du deine Sachen
bisher von Hand rüberkopieren. Ab jetzt macht das ein Befehl für dich.

**Neue Funktionen**
- **Daten-Umzug aus jeder alten Version.** Sag Bruno einfach, wo deine alte Installation liegt
  („Ich bin umgezogen, meine alte Version liegt hier: …") — er holt sich alles, was dir gehört:
  API-Keys, Firmenprofil, alle Belege, deine privaten Notizen, deine eigenen Anbieter-Regeln
  und deinen Arbeitsstand.
- **Alte Ordnernamen werden automatisch einsortiert.** Hießen deine Ordner früher `INPUT`,
  `OUTPUT` oder `2 BELEGARCHIV`, landen die Belege trotzdem am richtigen Platz in der heutigen
  Struktur. Egal wie alt deine Version ist.
- **Neue Anleitung `UMZUG-VON-ALTER-VERSION.md`** — erklärt den Umzug in zwei Minuten, mit
  Tabelle was mitkommt und was bewusst nicht.

**So sicher ist das**
- **Deine alte Installation wird nur gelesen, nie verändert.** Sie bleibt als Sicherung liegen,
  bis du selbst sagst, dass alles stimmt.
- **Trockenlauf ist die Voreinstellung.** Du siehst erst, was passieren würde. Geschrieben wird
  nur, wenn du es ausdrücklich startest.
- **Nichts wird überschrieben.** Was am Ziel schon existiert, bleibt. Läuft der Umzug zweimal,
  passiert beim zweiten Mal nichts.
- **Deine API-Keys werden zeilenweise zusammengeführt** — deine Keys kommen dazu, ohne dass der
  Update-Kanal der neuen Version verloren geht.
- **Deine eigenen Anbieter-Regeln gewinnen**, aber veraltete Regeln aus deiner alten Version
  überschreiben keine neueren Produkt-Regeln (z.B. geänderte Steuersätze).

**Unter der Haube**
- Getestet gegen zwei echte Kundenstände (eine sehr alte Installation mit der früheren
  3-Ordner-Struktur und eine neuere mit 4 Ordnern) — 16 Einzelprüfungen, alle bestanden:
  Keys, Profil, Steuernummer, Belege inklusive Unterordner, Kontoauszüge, private Notizen,
  Arbeitsstand und eigene Regeln kamen vollständig an.
- Die Liste „was gehört dem Kunden" stammt aus derselben Quelle wie beim normalen Update —
  es gibt keine zweite Wahrheit, die auseinanderlaufen könnte.

**Wissensstand:** unverändert (2026-07-23).

---

## v1.73.0 — Prüfungen erkennen Fremdwährung, Steuerbefreiungen und stille Ausfälle

**Was sich für dich ändert**

**Rechnungen in Dollar lösen keinen Fehlalarm mehr aus.** Bei Belegen in Fremdwährung rechnet
deine Bank mit ihrem eigenen Kurs ab, der leicht vom amtlichen EZB-Kurs abweicht. Der Prüfer
meldete solche Buchungen bisher als „Betrag stimmt nicht" — obwohl die Bank exakt den gebuchten
Betrag abgebucht hatte. Jetzt gilt: Weicht der Betrag nur geringfügig ab (höchstens 2,5 Prozent
und höchstens 1,50 Euro) UND gibt es eine Kontobewegung über genau den gebuchten Betrag, ist die
Buchung bestätigt statt beanstandet. Wichtig: Fehlt die passende Kontobewegung, bleibt die
Meldung bestehen — ein erfundener Betrag fällt weiterhin auf. Bei deinem letzten Check sind
dadurch vier von fünfzehn kritischen Meldungen als geklärt ausgewiesen worden.

**Steuerbefreiungen werden beim Einlesen erkannt.** Trägt ein Beleg einen Hinweis wie
„umsatzsteuerbefreit nach § 4 UStG", „VAT exempt", „Kleinunternehmer nach § 19 UStG" oder
„Reverse Charge", wird der Steuersatz jetzt auf null gesetzt — auch wenn anderswo auf dem Beleg
eine Prozentzahl steht. Das ist bares Geld: Ein Postbeleg über 10,49 Euro war mit 19 Prozent
erfasst, obwohl der Bon ausdrücklich die Steuerbefreiung nennt. Daraus wäre ein Vorsteuerabzug
entstanden, der dir nicht zusteht. Der wörtliche Wortlaut der Klausel wird mitgespeichert, damit
später nachvollziehbar bleibt, warum keine Steuer angesetzt wurde.

**Ausgefallene Texterkennung wird gemeldet statt verschwiegen.** Ist der Zugang zur bevorzugten
Texterkennung abgelaufen oder der Dienst gerade gestört, übernimmt automatisch die nächste —
das ist richtig so und schützt deine Daten weiterhin. Bisher geschah das aber lautlos, sodass
monatelang die schwächere Erkennung arbeiten konnte, ohne dass es jemandem auffiel. Jetzt gibt
es eine Meldung, bei abgelaufenen Zugangsdaten mit ausdrücklichem Hinweis darauf. Am Beleg wird
vermerkt, welche Erkennung ersatzweise gelaufen ist.

**Zwei neue Übersichten, die sich selbst aktualisieren**

- **Offene Posten** (`OFFENE-POSTEN.md`): Was ist offen, getrennt nach Jahr — Kontobewegungen
  ohne Beleg, Eingänge ohne Zuordnung, Belege ohne Zahlung. Dazu die Konto-Abdeckung: bis wann
  reichen die Daten je Konto? Das deckt Lücken auf, die sonst unsichtbar bleiben (Beispiel: ein
  Konto endet im Januar, Rechnungen laufen bis Juni — dann fehlen Auszüge).
- **Fehlende Rechnungen** (`FEHLENDE-RECHNUNGEN.md`): die Beschaffungsliste für eine Portal-Runde,
  nach Anbieter gruppiert, mit Zeitraum und Betrag. Monatsgenau geprüft — ein Anbieter erscheint
  auch dann, wenn nur ein einzelner Monat fehlt.

Beide werden bei jedem Lauf komplett neu aus deinem Buchhaltungssystem erzeugt. Sie sind eine
Lesehilfe, keine zweite Wahrheit: Bei Abweichung gilt immer das Buchhaltungssystem.

**Unter der Haube**

- Anbieter-Namen von Kontoauszug und Rechnung werden robuster zusammengeführt („NAME-CHEAP.COM*
  UJWFOK" und „Namecheap Inc" sind derselbe Anbieter). Getrennt bleiben bewusst verschiedene
  Landesgesellschaften desselben Konzerns, weil sie steuerlich unterschiedlich behandelt werden.
  Ohne diese Härtung hätte die Beschaffungsliste 116 Anbieter zu viel enthalten.
- Neue Schutzregel für Beleg-Bilder: Fotos und Scans von Belegen werden nur noch über den in
  deinem Profil eingestellten Anbieter gelesen, nie daran vorbei. Ist der nicht erreichbar,
  bricht die Prüfung ab, statt auf einen Anbieter außerhalb der EU auszuweichen.
- 60 neue automatische Tests sichern diese Regeln ab (Fremdwährung, Anbieter-Zuordnung,
  Datenschutz-Kette). Jede wurde durch absichtliches Kaputtmachen gegengeprüft.

## v1.72.0 — Belege werden zuverlässiger erkannt (Quittungen und Kontoabschlüsse)

**Was sich für dich ändert**
Wenn du Belege in den Posteingang legst, entscheidet Bruno zuerst: Ist das eine Rechnung oder
ein Kontoauszug? Zwei Sorten Dokumente landeten dabei bisher fälschlich in „unklar" und mussten
von dir von Hand einsortiert werden:

**Quittungen ohne Umsatzsteuer** (typisch bei US-Anbietern). Die schreiben „RECEIPT" statt
„Rechnungsnummer" und weisen keine deutsche Umsatzsteuer aus — es fehlte also jedes Merkmal,
an dem eine Rechnung erkennbar war. Kam dann noch eine lange Positionsliste dazu (etwa eine
Domain-Abrechnung mit zwölf Zeilen), sah das für Bruno aus wie eine Kontoumsatz-Tabelle.
Jetzt zählen Quittungs-Kopf und Bestellnummer als Beleg-Merkmale.

**Bank-Kontoabschlüsse** (der Quartals- oder Jahresabschluss mit Zinsen und Entgelten). Die
tragen oft gar nicht das Wort „Kontoauszug" und wurden deshalb nicht sicher als Bankdokument
erkannt. Verschärfend kam hinzu: im Kleingedruckten steht ein Satz über Umsatzsteuer, und den
hat Bruno als Rechnungs-Merkmal gewertet. Beides ist behoben — ein Umsatzsteuer-Hinweis zählt
jetzt nur noch als solcher, wenn tatsächlich ein Steuersatz oder Steuerbetrag dabeisteht.

**Warum das wichtig ist**
Ein Kontoauszug, der als Rechnung durchgeht, würde als Betriebsausgabe gebucht — das wäre ein
echter Buchungsfehler. Deshalb ist Bruno hier bewusst vorsichtig und fragt lieber nach. Diese
Änderung macht ihn nicht leichtgläubiger, sondern genauer: Er erkennt jetzt mehr Dokumente
sicher, ohne die Vorsicht aufzugeben.

**Unter der Haube**
Neue Kanarien-Tests (`src/statement-detect.test.mjs`, 6 Fälle) sichern beide Richtungen ab:
Quittung darf nie als Kontoauszug gelten, Kontoauszug nie als Rechnung. Gegenprobe auf dem
Bestand: 825 Belege und 25 Kontoauszüge geprüft, keine einzige Verschlechterung gegenüber
vorher; die Erkennung wurde per Sabotage-Test verifiziert (Fix entfernt → Test schlägt fehl).

## v1.71.0 — Neues Menü: 15 Modi in 4 Kategorien, neu sortiert und neu nummeriert

**Was sich für dich ändert**
Das Modus-Menü ist jetzt nach deinem Alltag sortiert statt nach Bau-Reihenfolge: Was du täglich
brauchst, steht oben — Einrichtung steht unten. Vier Kategorien (ALLTAG · ABSCHLUSS & PFLICHTEN ·
STEUERWISSEN · EINRICHTUNG), die wichtigsten Modi tragen ein ⭐. Die drei Teilschritte des
Full-Workflows sind jetzt als 1a/1b/1c direkt unter Modus 1 gruppiert. Alle Menüzeilen sind
kürzer — nichts wird mehr rechts abgeschnitten.

**🔴 Wichtig: Die Modus-Nummern haben sich geändert!**

| Alt | Modus | Neu |
|---|---|---|
| 1 | Full-Workflow | **1** |
| 3 | Belege scrapen → „Belege holen" | **1a** |
| 4 | Vorbuchhaltung | **1b** |
| 5 | Buchen → „Buchen + verknüpfen" | **1c** |
| 16 | Health-Check | **2** |
| 6 | Fehlende Belege | **3** |
| 9 | Report | **4** |
| 15 | Rechnung/Angebot | **5** |
| 17 | Kontoauszüge importieren → „Kontoauszüge-Import" | **6** |
| 8 | UStVA + Fristen | **7** |
| 7 | Export/Übergabe → „Export an den StB" | **8** |
| 18 | Festschreiben (GoBD) | **9** |
| 10 | Konsultieren → „St-Frage" | **10** (unverändert) |
| 13 | Steuerwissen R·I·C → „St-Wissen-Import" | **11** |
| 11 | Optimierungs-Check → „St-Optimierung" | **12** |
| 2 | Onboarding | **13** |
| 12 | DSGVO-Optimierung → „DSGVO-Härtung" | **14** |
| 14 | Update | **15** |

**Eingebauter Schutz:** Bruno nennt bei jeder Zahl-Eingabe zuerst den Modus-NAMEN
(„Modus 2 — Health-Check: …") und startet erst danach. Tippst du aus Gewohnheit eine alte
Nummer, siehst du sofort, dass ein anderer Modus startet, und kannst abbrechen.
Freitext funktioniert unverändert — „mach die Buchhaltung" findet immer den richtigen Modus.

**Unter der Haube**
- Über 100 Querverweise in Skill, Doku und Werkzeugen auf die neuen Nummern umgestellt —
  dabei auch zwei uralte Falsch-Verweise aus früheren Umbenennungen gefunden und repariert.
- Neuer Wächter (Kanarie): prüft ab jetzt automatisch, dass Modus-Nummern in Menü, Doku und
  Werkzeugen zusammenpassen — die Fehlerklasse „vergessener Alt-Verweis" kann nicht mehr
  unbemerkt liegen bleiben.
- Hinweis: Changelog-Einträge UNTER diesem hier nennen die alten Modus-Nummern (Tabelle oben
  übersetzt). Die Versionsnummer v1.69.0 wurde übersprungen (interne Nummern-Kollision zweier
  paralleler Verbesserungen — keine Funktion fehlt).

**Wissensstand:** unverändert (2026-07-23).

---

## v1.70.0 — Zweigleisige Anbieter richtig buchen + Doppel-Upload-Schutz + Stau-Wächter

**Das Problem, das wir gelöst haben**
Manche Anbieter (Anthropic, Paddle, Skool, OpenRouter, Google Cloud) stellen mal Rechnungen MIT
deutscher Umsatzsteuer aus, mal ohne (Reverse Charge) — je nachdem, ob deine USt-ID im Konto
hinterlegt ist. Bisher blieben all diese Belege sicherheitshalber liegen. Und: Belege konnten
unbemerkt monatelang im Eingangsordner festhängen, ohne dass eine Prüfung Alarm schlug.

**Neue Funktionen**
- **Beleg-gesteuerte Steuer-Varianten:** Ein Anbieter darf jetzt ZWEI hinterlegte Behandlungen
  haben. Bruno wählt die richtige pro Beleg — aber nur mit Beweis: der Steuerbetrag auf dem Beleg
  muss rechnerisch exakt zum Steuersatz passen, bzw. die Reverse-Charge-Klausel muss wörtlich in
  der PDF-Textebene stehen. Ohne Beweis bleibt der Beleg wie bisher in der Prüfschleife.
  Rechnungen mit ausländischer OSS-USt (ohne deutsche USt-ID des Ausstellers) werden brutto als
  Aufwand gebucht — ohne Vorsteuer-Abzug, denn der stünde dir dort nicht zu (§14/§15 UStG).
- **Neuer Stau-Wächter (Prüf-Dimension 22):** Der Health-Check meldet jetzt, wenn buchfertige
  Belege länger als 14 Tage oder Prüf-Fälle länger als 45 Tage unbearbeitet im Eingangsordner
  liegen. Genau diese Blindstelle hatte 300+ Belege monatelang unbemerkt liegen lassen.

**Verbesserungen**
- **Doppel-Upload-Schutz verschärft:** Manche Anbieter schicken zum selben Kauf Rechnung UND
  Quittung mit fast gleicher Nummer (z.B. mit „#" davor). Die Dublettenerkennung normalisiert
  Belegnummern jetzt strikt — solche Zwillinge werden erkannt statt doppelt gebucht.
- **Benachrichtigungs-Filter erweitert:** 8 neue Familien (Kontoauszug-Hinweise, 2-Faktor-Mails,
  Produkt-Updates, Team-Beitritte, Guthaben-Warnungen u.a.) werden als Nicht-Belege aussortiert.
- **Betrags-Erkennung:** Beträge mit Leerzeichen als Tausendertrenner („1 701,70") werden jetzt
  auch beim Feld-Heilen korrekt gelesen.

**Unter der Haube**
- Alle Änderungen mit Kanarien-Tests + Sabotage-Beweis abgesichert (14 neue Tests für die
  Varianten-Logik, 7 für den Stau-Wächter). Falsch kontierte Entwürfe des ersten Anlaufs wurden
  vollständig entfernt und korrekt neu angelegt (sevDesk-Konto 5960 erlaubt nur §13b-Steuerregeln —
  die Brutto-Variante bucht jetzt auf „laufende Lizenzgebühren", wie der bewährte Calendly-Fall).

**Wissensstand:** 2026-07-23

## v1.68.1 — Neuer Modus 18 „Festschreiben" (GoBD-Abschluss) + weniger unnötige Rückfragen + Menü aufgeräumt

**Das Problem, das wir gelöst haben**
Der Lebensweg eines Belegs endete im Menü scheinbar beim Export — dabei gehört zum GoBD-sauberen
Abschluss noch die Festschreibung (das endgültige Versiegeln geprüfter Buchungen). Und: Beim
Komplett-Lauf fragte Bruno „Wie weit soll ich gehen?", obwohl du ihm längst hohe Autonomie gegeben
hattest.

**Neue Funktionen**
- **Modus 18 — Festschreiben (GoBD-Abschluss):** Bruno prüft die Kandidaten IMMER frisch per
  Health-Check und fragt dann: „N Belege sind safe (gebucht + mit Kontobewegung verknüpft + ohne
  Prüf-Befund) — soll ich sie festschreiben, oder machst du es selbst?" Nach deinem Ja erledigt er
  es per API — kein Klicken in sevDesk nötig. Wichtig zu wissen: Festschreiben ist endgültig,
  Korrekturen gehen danach nur noch per Stornobuchung. Wir haben das scharf getestet — auch per
  API gibt es KEIN Zurück (das ist der Sinn der GoBD).
- **Neuer Schalter `festschreiben` im Profil:** `tor` (Standard — Bruno fragt einmal pro Periode) ·
  `auto` (Bruno schreibt selbst fest, aber nur nach abgegebener USt-Voranmeldung und ohne offene
  Befunde) · `nie` (Bruno zeigt nur die Liste). Deine Autonomie-Stufe allein löst NIE ein
  Festschreiben aus.
- **Report-Block „Festschreib-reif":** Jeder große Abschluss-Report zeigt jetzt, wie viele Belege
  fertig geprüft und bereit zum Versiegeln sind.
- ⚠️ **Klare Warnung zur Reihenfolge:** Für den DATEV-Export an deinen Steuerberater musst du
  NICHT festschreiben — es reicht, dass die Belege gebucht und mit der Kontobewegung verknüpft
  sind. Schreibst du vorher fest, kann dein Steuerberater seine Korrekturwünsche nur noch per
  Stornobuchung umsetzen. Richtige Reihenfolge: buchen → prüfen → Export an den Steuerberater
  (der Regler „Dokumente festschreiben" im sevDesk-Dialog bleibt dabei AUS) → seine Rückmeldung →
  erst dann festschreiben. Der Hinweis steht jetzt im Menü, in Modus 18 und in Modus 7 (Export).

**Verbesserungen**
- **Komplett-Lauf ohne unnötige Zwischenfrage:** Steht deine Autonomie auf „hoch", sagt Bruno nur
  noch an („Ich laufe komplett durch bis gebucht + verknüpft — sag stopp, wenn du weniger willst")
  und arbeitet durch. Die Sicherheitsprüfungen bleiben alle unverändert aktiv.
- **Menü aufgeräumt:** jede Modus-Beschreibung jetzt eine klare Zeile; E-Mail-Eingang korrekt als
  „Gmail + gängige Anbieter per IMAP" beschrieben (nicht mehr nur Gmail).
- **Doku an die Wirklichkeit angepasst:** Der DATEV-Export per Befehl existiert längst
  (`datev-export.mjs`) — die Anleitung behauptete das Gegenteil. Der Health-Check hat 20
  Prüfdimensionen (stand fälschlich „15"). Onboarding-Texte auf den aktuellen Stand gebracht.

**Unter der Haube**
- Scharf-Test mit Wegwerf-Beleg beweist: Nach dem Festschreiben blockt sevDesk Ändern (400),
  Zurücksetzen auf Entwurf (400 „Already enshrined") und Löschen (409). Ergebnis in der
  Wissensbasis (GoBD-Referenz) und den API-Notizen dokumentiert; eine ältere, ungetestete
  Log-Behauptung („Rückgängig möglich") wurde korrigiert.
- GoBD-Wissen erweitert: Warum Festschreiben Pflicht ist (§ 146 Abs. 4 AO, GoBD Rz 58) und wann
  der übliche Zeitpunkt ist (spätestens mit Abgabe der USt-Voranmeldung — Fachpraxis).

**Wissensstand**
- 2026-07-23

## v1.67.0 — Neue Prüfung „Das ist keine Rechnung" + Bank-Schiedsrichter + genaueres Betrags-Lesen

**Das Problem, das wir gelöst haben**
Manche Dokumente sehen aus wie Rechnungen, sind aber keine: Meta/Facebook verschickt z.B.
Zahlungsbestätigungen, auf denen wörtlich „Das ist keine Rechnung" steht. 14 solcher Dokumente
lagen bei uns in der Buchhaltung — für den Vorsteuerabzug reichen sie nicht, die echten Rechnungen
muss man separat im Werbekonto herunterladen. Das fiel bisher nur durch Zufall auf.

**Neue Funktionen**
- **Neue Prüfung 21 „Beleg-Natur":** Bruno liest bei jedem Beleg die PDF-Textebene (lokal, ohne KI)
  und warnt VOR dem Buchen, wenn der Beleg selbst sagt, dass er keine Rechnung ist. Zusätzlich weist
  der Health-Check jetzt ehrlich aus, wie viele Belege gescannte Bilder ohne Textebene sind — dort
  kann diese Prüfung nichts sehen (vorher wurden sie stillschweigend als sauber gezählt).
- **Bank-Schiedsrichter (2-von-3-Prinzip):** Wenn der gebuchte Betrag und der Betrag im PDF-Text
  sich widersprechen, stimmt jetzt automatisch die Bank ab: stützt eine Kontobewegung Cent-genau
  genau EINEN der beiden Werte, wird die Warnung entsprechend eingeordnet (wahrscheinlicher
  Fehlalarm bzw. wahrscheinlich echter Fehler). Es wird dabei nie etwas automatisch geändert oder
  gebucht — nur die Warnung wird schlauer. Kleinstbeträge unter 10 € stimmen nicht ab (zu leicht
  zufällig in jeder Bank vorhanden).

**Verbesserungen**
- Beträge mit Leerzeichen als Tausendertrenner („1 701,70") werden jetzt korrekt gelesen — vorher
  konnte das einen falschen Alarm auf eine korrekte Buchung auslösen (real bei einer Rechnung
  passiert). Auch geschützte Sonder-Leerzeichen werden erkannt.
- Der „Bitte ansehen"-Ordner (_review) wird nach jedem Buchungs-Abgleich automatisch neu aufgebaut —
  vorher konnten dort veraltete Verweise auf schon abgelegte Belege zurückbleiben (real: 78 Stück).
- Login-Benachrichtigungen der Bank („Es gab eine neue Anmeldung"), die fälschlich einen Betrag
  trugen, werden jetzt als Nicht-Belege erkannt und aus der Buchungs-Warteschlange geräumt.

**Unter der Haube**
- 32 neue Selbsttests (Kanarien) für alle neuen Prüfungen, jeweils mit Sabotage-Beweis (Fix
  entfernen → Test schlägt an). Unabhängiger Code-Review über alle Änderungen, Funde eingearbeitet.
- Benchmark an 38 Echtbelegen dokumentiert, warum die KI-Texterkennung der Leser bleibt und die
  PDF-Textebene der Kontrolleur: `system/research/PDFTOTEXT-ANALYSE-2026-07-23.md`.
- DSGVO-Doku ergänzt: die komplette Textebenen-Schicht läuft lokal (kein Datenabfluss, kein AVV
  nötig, bei jeder Engine-Wahl aktiv) + Widerspruchs-Leiter, wer Konflikte auflöst.

## v1.66.2 — Aufgeräumte Testdaten + neues Reparatur-Werkzeug für lückenhafte Belege

**Verbesserungen**
- Die internen Selbsttests (Kanarien) nutzen jetzt durchgehend neutrale Muster-Bankdaten
  (Max Mustermann, offizielle Beispiel-IBANs). Für dich ändert sich nichts — alle Tests
  laufen unverändert grün.

**Neue Funktion (unter der Haube)**
- Neues Werkzeug `fill-canonical-from-text.mjs`: füllt fehlende Beleg-Felder (Rechnungsnummer,
  Betrag, Währung, Steuersatz) direkt aus dem PDF-Text nach — ohne KI, rein deterministisch,
  und nur bei eindeutigen Treffern. Bestehende Werte werden nie überschrieben. Damit werden
  Belege, die der Health-Check bisher nicht prüfen konnte, wieder prüfbar.

## v1.66.1 — Stripe-Auszahlungen: manuelle Auszahlungen brechen den Lauf nicht mehr ab

**Was war**
Wenn du eine Auszahlung in Stripe von Hand auslöst (statt sie automatisch laufen zu lassen), verrät
Stripes Schnittstelle nicht, welche Kundenzahlungen darin stecken — das geht nur bei automatischen
Auszahlungen. Bruno brach an dieser Stelle komplett ab, wodurch auch alle anderen (automatischen)
Auszahlungen nicht mehr verarbeitet wurden.

**Was jetzt passiert**
- Manuelle Auszahlungen werden sauber übersprungen und mit Betrag und Datum auf die Klärliste
  gesetzt — nichts verschwindet still, der Rest läuft normal durch.
- Du bekommst den konkreten Weg genannt: entweder im Stripe-Dashboard nachsehen, welche Zahlungen
  enthalten sind, oder in Stripe auf automatische Auszahlungen umstellen.

## v1.66.0 — Durchgeleitete Zahlungen: wenn auf dem Kontoauszug der falsche Name steht

**Das Problem, das wir gelöst haben**
Auf deinem Kontoauszug steht bei einer Zahlung nicht immer der, mit dem du wirklich Geschäfte gemacht
hast. Beispiel aus der Praxis: zwei Zahlungseingänge waren als "STRIPE" ausgewiesen — im
Verwendungszweck stand aber "SHOPIFY AUSZAHLUNG". Es waren also Shop-Erlöse, die nur über den
Zahlungsdienstleister geflossen sind. Wer nur auf den Empfängernamen schaut, ordnet sie dem
Falschen zu. Bei uns waren das über 5.000 Euro.

**Was jetzt passiert**
- **Bruno erkennt durchgeleitete Zahlungen.** Steht als Empfänger ein Zahlungsdienstleister
  (Stripe, PayPal, Klarna, Mollie, Adyen, SumUp …), im Verwendungszweck aber ein Händler oder eine
  Plattform (Shopify, Amazon, Etsy, Digistore, elopage …), dann gilt: die Zahlung gehört
  wirtschaftlich dem Händler. Der Zahlungsdienstleister ist nur der Weg, nicht der Partner.
- **Falsche Zuordnung wird verhindert.** Eine Rechnung des Zahlungsdienstleisters kann sich so eine
  durchgeleitete Händler-Zahlung nicht mehr "schnappen". Nur ein Beleg des echten Händlers passt dazu.
- **Neue Prüfung im Health-Check** meldet jede solche Zahlung im Klartext: "nennt als Empfänger X,
  gehört laut Verwendungszweck aber Y". Noch nicht zugeordnete Fälle werden als Hinweis markiert,
  bereits zugeordnete nur zur Kontrolle angezeigt.

**Was du davon hast**
Erlöse aus einem Shop oder Marktplatz landen nicht mehr versehentlich beim Zahlungsdienstleister —
gerade wichtig, wenn du mehrere Geschäftsbereiche hast oder einen Shop betreibst.

## v1.65.0 — Schärfere Fehler-Erkennung: Tippfehler-Dubletten, Betrags-Gegenprüfung, fehlende Kontoauszüge

**Neue Funktionen**
- **Tippfehler-Dubletten werden jetzt erkannt.** Wenn die Texterkennung eine Rechnungsnummer minimal
  verliest (z.B. den Buchstaben I als Ziffer 1 oder O als 0), erkennt Bruno beim Buchen, dass es sich
  wahrscheinlich um dieselbe Rechnung handelt, die schon gebucht wurde — und legt sie zur Prüfung
  zurück statt sie doppelt zu buchen. Das verhindert genau die Art von Doppelbuchung, die sonst nur
  mühsam von Hand zu finden ist.
- **Betrags-Gegenprüfung gegen das Original-PDF (neue Health-Check-Prüfung).** Bruno vergleicht jetzt
  jeden gebuchten Betrag noch einmal direkt mit dem Text im Original-PDF — mit einer anderen Methode
  als die Texterkennung, die den Betrag ursprünglich gelesen hat. So fallen zwei Fehlerklassen auf:
  ein falsch übernommener Betrag und eine als Euro gebuchte Fremdwährungs-Rechnung (Dollar-Rechnung).
- **Fehlende Kontoauszüge fallen jetzt auf (neue Health-Check-Prüfung).** Findet Bruno bei einem
  Bankkonto einen ganzen Monat ohne einen einzigen Umsatz, meldet er das — meist heißt das, ein
  Kontoauszug wurde noch nicht eingelesen. So bleibt keine Lücke im Zahlungsnachweis unbemerkt.
- **Anbieter-Namen werden zusammengeführt.** Auf dem Kontoauszug steht oft ein anderer Name als auf
  der Rechnung (z.B. "FACEBK*..." statt "Meta", "P.SKOOL.COM/..." statt "Skool"). Bruno erkennt jetzt,
  dass das derselbe Anbieter ist, und findet die passende Bankbuchung zuverlässiger.

**Verbesserungen**
- Der Posteingang wird sauberer: Bruno erkennt jetzt anhand des Inhalts (nicht nur des Betreffs),
  wenn eine eingelesene Datei gar keine Rechnung ist (Ankündigung, Passwort-Mail, Warenkorb-Erinnerung),
  und legt sie beiseite — Zahlungseingänge werden dabei geschützt und nie aussortiert.
- Der Import-Prüfbericht meldet keinen Fehlalarm mehr, wenn bewusst bereinigte Doppelbuchungen fehlen.
  Bewusst gelöschte Dubletten werden jetzt dauerhaft protokolliert und als "kein Verlust" ausgewiesen.
- Klarere Info, wie du eigene Erkenntnisse (anonym) mit der Community teilen kannst — freiwillig,
  nichts verlässt automatisch deinen Rechner.

**Wissensstand**
- Stripe-Gebühren: direkt aus Stripes eigenen Daten bestätigt, dass Stripe keine Umsatzsteuer auf die
  Gebühr berechnet — für dich als Regelbesteuerer ist die steuerliche Behandlung damit ohne Zahllast-Effekt.

## v1.64.2 — Festschreiben per API funktioniert jetzt nachweisbar (+ Schutz-Lücke geschlossen)

**Was passiert ist**
- Beim ersten Festschreib-Lauf (18 Belege, mit deiner ausdrücklichen Freigabe) meldete Bruno zunächst
  "fehlgeschlagen" — dabei hatte alles funktioniert. Grund: sevDesk bestätigt das Festschreiben nicht
  in der Antwort, und Bruno prüfte danach ein Feld, das es gar nicht gibt (`enshrineDate` statt des
  echten Feldes `enshrined`). Die Kontrolle las also ins Leere.
- Beim Beheben fiel auf: **zwei Reparatur-Werkzeuge nutzten dasselbe falsche Feld als Schutz** — ihr
  eingebauter "festgeschriebene Belege nie anfassen"-Riegel war dadurch wirkungslos. Bisher folgenlos
  (es war noch nie etwas festgeschrieben), ab jetzt wäre es riskant gewesen.

**Was sich für dich ändert**
- Festschreiben über Bruno funktioniert jetzt mit echter Erfolgskontrolle (das richtige Feld wird
  gelesen). Es bleibt dabei: Bruno schreibt NIE ohne deine ausdrückliche Freigabe pro Liste fest.
- Die Schutz-Riegel in den Reparatur-Werkzeugen greifen jetzt wirklich: festgeschriebene Belege
  werden übersprungen, nie verändert.

**Unter der Haube**
- `durchbucher.mjs` + `durchbucher-a2.mjs`: Guard `enshrineDate` → `enshrined` korrigiert.
- `CAPABILITIES.md`: enshrine-Endpoint-Verhalten live-dokumentiert (leerer Response-Body, Readback
  über `enshrined`-Feld Pflicht).
- Kanarien-Test grün (7/7).

## v1.64.1 — Drei Funktionen standen nicht in deiner Anleitung (obwohl du sie hast)

**Was schiefgelaufen war**
- Die Anleitung (`SETUP-GUIDE.md`) listete 13 Modi und sprach von "den 14 Modi". Tatsächlich sind es
  **17**. Drei davon fehlten in der Tabelle komplett — du hattest sie die ganze Zeit, konntest sie aus
  der Anleitung aber nicht kennen:
  - **Modus 14 (Update)** — Bruno prüft und spielt neue Versionen selbst ein, deine Daten bleiben unangetastet.
  - **Modus 15 (Rechnung/Angebot schreiben)** — Ausgangsrechnungen und Angebote erstellen, prüfen, versenden.
  - **Modus 17 (Kontoauszüge importieren)** — alte Auszüge als PDF oder CSV nachträglich einlesen.
    Das ist der Modus, der die 90-Tage-Lücke schließt: Banken geben über die automatische Verbindung
    meist nur die letzten 90 Tage heraus — ältere Umsätze holst du damit trotzdem sauber rein.
- Der Tiefen-Check wurde mit "12 Prüfungen" angegeben. Es sind **17** — die drei neuesten prüfen
  doppelt importierte Kontoumsätze, deine Konto-Stammdaten (IBAN) und ob eine Prüfung mangels Daten
  ins Leere läuft.

**Was sich für dich ändert**
- Anleitung, FAQ und Werkzeug-Übersicht nennen jetzt alle dieselben, am Code geprüften Zahlen.
- An deiner Buchhaltung ändert sich **nichts** — es war reine Dokumentation. Die Funktionen liefen die
  ganze Zeit, sie standen nur nicht in deiner Anleitung.

**Unter der Haube**
- Neue Regel (`system/HARD-RULES.md` #18b): Zahlenangaben in der Doku (Modi, Prüf-Dimensionen) werden
  vor dem Schreiben am Code gezählt, nie aus dem Gedächtnis oder aus einer anderen Doku-Datei
  übernommen — sonst pflanzt sich ein alter Stand fort. Mit Prüfbefehlen hinterlegt.
- Korrigiert außerdem: ein falscher interner Verweis in `TOOLS.md` (Modus 1 ruft am Ende den
  Health-Check auf, Nummer 16 — dort stand noch die alte 14), die Dimensions-Zahl in Brunos eigener
  Arbeitsanweisung, und eine letzte veraltete Zeile in der internen Architektur-Notiz, die den
  Mail-Import fälschlich als "nur Gmail" beschrieb (IMAP für gängige Anbieter ist seit v1.11 gebaut).

**Wissensstand:** unverändert (2026-07-12) — reine Dokumentations-Korrektur, keine Auswirkung auf Buchungen oder Steuerlogik.

---

## v1.64.0 — Neues Report-Design für alle Berichte

**Neue Funktionen**
- Deine Berichte (Lauf-Report, Buchhaltungs-Report, Health-Check) haben ein neues, klareres Design
  bekommen. Ganz oben steht jetzt auf einen Blick, was Bruno erledigt hat und was noch DEINE Aktion
  braucht — mit einem Sprungmenü direkt zu den wichtigen Stellen.
- Neu oben im Bericht: ein farbiger "Auf einen Blick"-Bereich (grün/gelb/rot), ein Verifiziert-Ring
  (wie viel Prozent der Buchungen rückgeprüft sind) und eine kurze Kennzahlen-Reihe. Falls etwas von
  dir zu tun ist, klebt oben eine Hinweis-Leiste mit Sprung direkt zur Aufgabe.

**Verbesserungen**
- Angenehmer zu lesen: neue Schrift (Manrope für Text, Monospace-Ziffern für Zahlen), mehr Ruhe im
  Layout, klare Ampel-Farben statt bunt. Zahlen tragen weiterhin ihre Einheit, jede Kennzahl ihre
  Erklärung — nichts muss geraten werden.
- Druck/PDF sauber: A4-Layout, die Hinweis-Leiste und das Sprungmenü werden beim Drucken automatisch
  ausgeblendet.
- Der Bericht ist eine einzelne HTML-Datei ohne Internet-Abhängigkeit — öffnet offline und lässt sich
  ohne Zusatzsoftware speichern oder drucken.

**Unter der Haube**
- `system/_lib/report-html.mjs` (der Report-Baustein für ALLE Modi) auf das abgenommene Design aus dem
  Design-System umgestellt. Bestehende Berichte funktionieren unverändert weiter — die neuen Elemente
  (Hinweis-Leiste, Sprungmenü, Verifiziert-Ring) sind optional und erscheinen nur, wo sinnvoll.
- Das Ursprungs-Design liegt als Referenz in `system/_design-ref/` (nur Quelle, nicht Teil der
  ausgelieferten Berichte — diese bleiben eigenständige HTML-Dateien).

**Wissensstand:** unverändert (2026-07-12) — reine Design-Änderung, keine Auswirkung auf Buchungen oder Steuerlogik.

---

## v1.63.0 — Health-Check zeigte falsche Alarme + ein Status bleibt technisch bei "teilweise"

**Teil 1 — falsche Alarme bei Belegen mit zwei Dokumenten**
- Manche Anbieter schicken zu einem Kauf ZWEI PDFs mit derselben Rechnungsnummer: die Rechnung und
  eine separate Zahlungsbestätigung. Bei zwei dieser Belege hatte die automatische Texterkennung die
  Zahlungsbestätigung fälschlich als "Rechnung" eingestuft — einmal mit dem Betrag VOR einem Rabatt
  statt danach, einmal mit einem erfundenen Steuersatz auf einem Screenshot. Der Health-Check nahm
  diese falschen Werte als Vergleichsbasis und meldete drei Alarme, die es nicht gab.
- **Deine Buchungen selbst waren die ganze Zeit korrekt** — betroffen war nur die Anzeige im Prüfbericht.

**Was sich für dich ändert**
- Erscheinen künftig zwei Dokumente zu derselben Rechnungsnummer, prüft Bruno jetzt zusätzlich: zuerst,
  ob eines davon nur ein Bildschirmfoto ist (dann gewinnt das echte PDF), dann, welches zum tatsächlich
  gebuchten Betrag passt. Bleibt es trotzdem uneindeutig, wird nichts mehr geraten: der Beleg gilt dann
  als "nicht geprüft" statt einen falschen Alarm zu erzeugen.
- Insgesamt sind dadurch 8 falsche Alarme aus dem Prüfbericht verschwunden.

**Teil 2 — Status "750" bei manchen Abo-Rechnungen ist kein Fehler**
- 10 Qonto-Monatsrechnungen aus 2025 zeigten "Status 750 trotz Vollzahlung" als kritischen Alarm. Nach
  genauer Prüfung: Das ist bei Rechnungen, die durch VIELE kleine Einzelbuchungen beglichen werden
  (nicht durch eine einzige Zahlung), eine technische Grenze deines Buchhaltungssystems — der Status
  "vollständig gebucht" lässt sich dort nicht erreichen, obwohl bewiesen ist, dass exakt der richtige
  Betrag verbucht wurde. Ein Korrekturversuch (an einem Einzelfall live getestet) bestätigte das, ohne
  etwas zu beschädigen.
- Der Prüfbericht sagt das jetzt auch so: bei diesen Fällen steht "kosmetisch, kein Handlungsbedarf"
  statt eines falschen "das lässt sich reparieren".

**Unter der Haube**
- `system/_lib/health-check.mjs`: Canonical-Auswahl bei mehreren gleich-eingestuften Dokumenten pro
  Rechnungsnummer wählt jetzt zweistufig (echte PDF-Datei vor Bildschirmfoto, dann Betrag) statt
  "erster/letzter gewinnt". Empfehlungstext bei "Status 750" unterscheidet jetzt zwischen echt
  heilbaren und technisch nicht heilbaren Fällen.
- Zwei betroffene Beleg-Dateien selbst korrigiert (Betrag/Steuersatz gegen das Original-PDF berichtigt,
  Backups angelegt). Neues Diagnose-Werkzeug `durchbucher-a2.mjs` für künftige Fälle dieser Art.
- Kanarien-Test nach jeder Änderung grün (7/7) — bestehende Prüflogik nicht angefasst, nur die
  Mehrdeutigkeits-Auflösung gehärtet und ein Empfehlungstext präzisiert.

**Wissensstand:** unverändert 2026-07-12.

## v1.62.1 — Nachtrag: zwei weitere Stellen, die den Postfach-Import verschwiegen

**Was noch fehlte**
- Die Korrektur aus v1.62.0 hatte zwei Dateien übersehen: die **FAQ** („Der automatische Mail-Import braucht aktuell Gmail") und eine interne Wissensdatei („Bruno kann heute nur Gmail"). Beide stammten aus der Zeit vor dem Einbau.
- **Warum sie durchrutschten:** Gesucht wurde nach Stichwörtern wie „IMAP" oder „Gmail-only" — beide Sätze enthalten keins davon. Sie sagen dasselbe in normaler Sprache.

**Behoben**
- **FAQ:** „Ich habe kein Gmail — geht das?" wird jetzt richtig beantwortet, samt Anbieterliste, Hinweis auf den Verbindungstest und der Zusicherung, dass Bruno im Postfach ausschließlich liest.
- **Wissensdatei:** Der Rechercheteil vom 7. Juli bleibt unverändert erhalten (die Server-Adressen und Klickwege sind weiterhin gültig), bekommt aber oben einen klaren Aktualitäts-Hinweis. Recherche wird nicht umgeschrieben, sondern datiert.

**Damit das nicht wieder passiert**
- Die interne Prüfregel sucht ab jetzt nach der **Aussage** statt nach dem Stichwort — also nach Formulierungen wie „braucht", „nur", „ohne", „setzt voraus". Zusätzlich wird jede Kundendatei einzeln geöffnet: Dort steht so etwas in Alltagssprache, und genau dort richtet es Schaden an.

**Wissensstand:** unverändert 2026-07-12.

## v1.62.0 — Kein Gmail? Bruno konnte dein Postfach die ganze Zeit lesen

**Was passiert ist**
- Bruno liest seit Version 1.11 nicht nur Gmail, sondern auch **GMX, WEB.DE, T-Online, iCloud, Posteo, mailbox.org, IONOS, Strato, freenet, Arcor und Yahoo** direkt aus dem Postfach. Das ist gebaut, getestet und an einem echten Postfach verifiziert.
- **Nur: die Einrichtung hat es verschwiegen.** Wer beim Onboarding "ich habe kein Gmail" sagte, bekam die Antwort, es gebe keinen automatischen Postfach-Import, und wurde zum manuellen Ordner geschickt. Auch die Einrichtungs-FAQ sagte das. Ein Fehler in der Anleitung, nicht in der Software.
- Ursache: Ein alter, verworfener Anlauf hatte einen Hinweis hinterlassen, der nach dem Neubau nie korrigiert wurde.

**Was sich für dich ändert**
- **Beim Einrichten wird der Postfach-Zugang jetzt angeboten**, samt genauem Klickweg für deinen Anbieter (bei den meisten einmalig freischalten, bei iCloud und Yahoo ein App-Passwort erzeugen) und einem Verbindungstest vorab.
- **Bruno liest dabei ausschließlich.** Er markiert nichts, verschiebt nichts und löscht nichts in deinem Postfach — das ist technisch abgesichert, nicht nur zugesagt.
- **Mehrere Postfächer** gehen ebenfalls, auch bei verschiedenen Anbietern.
- Bei **Outlook/Microsoft 365** bleibt es ehrlich: Manche Konten sperren den Zugriff serverseitig. Bruno testet das, bevor du etwas einrichtest, statt es zu versprechen.
- Der Ordner zum Reinlegen von PDFs und Fotos bleibt zusätzlich nutzbar — als Ergänzung, nicht als Notlösung.

**Damit so etwas nicht wieder passiert**
- Neue interne Regel: Wird eine Funktion entfernt **oder neu gebaut**, gilt die Arbeit erst als fertig, wenn alle Stellen dasselbe sagen — die interne Beschreibung, der gesprochene Einrichtungstext, die FAQ und der Changelog. Widersprechen sich zwei Stellen, entscheidet der Code, nicht die ältere Notiz.
- Ehrlichkeits-Grenze festgeschrieben: Aussagen, die ein Kunde am eigenen System widerlegen kann, kommen nicht in die Unterlagen. Deshalb heißt es "Gmail plus alle gängigen Anbieter" und nicht "jedes Postfach".

**Unter der Haube**
- `CLAUDE.md`: Falschaussage ersetzt, Historie als Hinweis erhalten (damit der Punkt nicht erneut "aufgelöst" wird) · `SKILL.md` Onboarding 2.2: IMAP-Weg mit Klickweg, Health-Check und Read-only-Zusage statt Absage · `SETUP-GUIDE.md` FAQ korrigiert · neue HARD-RULE #18.

**Wissensstand:** unverändert 2026-07-12.

## v1.61.0 — Rechnungen mit mehreren Steuersätzen werden jetzt richtig aufgeteilt

**Warum das wichtig ist**
- Eine Hotelrechnung trägt zwei Steuersätze: 7 % auf die Übernachtung, 19 % auf Frühstück und Parkplatz. Dasselbe bei Catering, bei Druck plus Porto, bei vielen Restaurantrechnungen. Bruno hat solche Belege bisher mit **einem einzigen Steuersatz auf alles** gebucht.
- Was das kostet, am echten Beispiel (Hotelrechnung über 385,29 €): Mit 7 % auf alles hättest du **15,39 € Vorsteuer zu wenig** gezogen. Mit 19 % auf alles **12,60 € zu viel gemeldet** — und zu viel gemeldete Vorsteuer ist die unangenehmere Richtung, das ist eine unrichtige Voranmeldung.
- Richtig sind 18,90 € aus dem 7-%-Teil plus 15,39 € aus dem 19-%-Teil, zusammen **34,29 €**. Genau das bucht Bruno ab jetzt.

**Neue Funktionen**
- **Bruno erkennt beim Lesen einer Rechnung, welche Position welchen Steuersatz trägt**, und legt im Buchhaltungssystem pro Steuersatz eine eigene Buchungszeile an. Getestet mit einer Rechnung, die alle drei Sätze gleichzeitig trägt: 12,50 € Porto (0 %), 180 € Druck (7 %), 620 € Grafik (19 %) — alle drei landen sauber getrennt, Vorsteuer 130,40 €, Summe auf den Cent.
- **Es wird nichts geraten.** Steht der Steuersatz nicht an jeder Position, oder ist die Aufteilung unklar, bucht Bruno wie bisher mit einem Satz und meldet den Beleg zur Prüfung — statt sich eine Aufteilung auszudenken. Im Test wurde das an vier normalen Ein-Satz-Rechnungen gegengeprüft: keine erfundene Aufteilung.
- **Der Gesundheits-Check prüft solche Belege mit.** Er rechnet nach, ob die Summe aller Positionen zum Rechnungsbetrag passt, und fragt nach, wenn eine 0-%-Position neben normal besteuerten steht (steuerfrei oder Auslandsleistung? — das ist eine echte Steuerfrage, die dein Steuerberater entscheiden sollte).

**Ein Angebot wird nicht mehr versehentlich als Rechnung gebucht**
- Angebote tragen oft den Satz „Dies ist ein Angebot, keine Rechnung." Ausgerechnet das Wort „Rechnung" in dieser Verneinung hat Brunos Angebots-Erkennung ausgehebelt — **der Beleg, der am deutlichsten sagt, dass er keiner ist, rutschte durch.** Im Test kam ein Angebot über 5.355 € als ganz normale, buchbare Rechnung an.
- Ab jetzt zählt ein verneintes „keine Rechnung" nicht mehr als Rechnungs-Nachweis, und eine Angebotsnummer ohne jede Rechnungsnummer gilt immer als Angebot. An 120 echten Belegen gegengeprüft: keine einzige Änderung am bisherigen Verhalten, nur das Angebot wird zusätzlich abgefangen.

**Verbesserungen**
- **Die Vorschau vor dem Buchen zeigt jetzt den richtigen Betrag.** Bei der Hotelrechnung stand dort „288,90" — das war nur die erste Position, nicht die 385,29 € des Belegs. Gebucht und geprüft wurde immer korrekt, aber eine Vorschau, die einen falschen Betrag nennt, verleitet dazu, etwas freizugeben, das man nicht gesehen hat. Jetzt steht dort der Gesamtbetrag samt Aufschlüsselung.

**Wie das geprüft wurde**
- An acht eigens dafür gebauten Testrechnungen, echt durch die komplette Kette: lesen, einordnen, ins Buchhaltungssystem buchen, zurücklesen, wieder löschen. Die beiden Testbuchungen wurden anschließend restlos entfernt (Nachweis: Abruf liefert „nicht gefunden"), die Testeinträge in den Anbieterlisten zurückgenommen und per Prüfsumme bestätigt, dass der Ausgangszustand exakt wiederhergestellt ist.

**Unter der Haube**
- Zwei Stellen verwarfen das Positions-Feld still: `normalize()` in `ocr/index.mjs` (baut ein neues Objekt aus fester Feldliste) und `toCanonical()` in `sort.mjs`. Der komplette Mehrsatz-Pfad im Mapper, im Buchungs-Gate und im Readback existierte bereits — er wurde nie erreicht. Beweis vor dem Fix: 0 von 1.587 gespeicherten Belegen trugen das Feld. Positions-Filterung ist rein deterministisch (nur 0/7/19, mindestens zwei verschiedene Sätze, sonst `null`).

**Wissensstand:** unverändert 2026-07-12.

## v1.60.0 — Der Betrugsschutz ist jetzt aktiv (und deine Kontonummer korrigiert)

**Neue Funktionen**
- **Der Betrugsschutz aus der letzten Version läuft ab sofort automatisch mit.** Er war gebaut und getestet, hing aber noch nicht im Buchungsweg — also wirkungslos. Jetzt wird jeder Beleg vor dem Buchen geprüft.
- **Ein Verdacht blockiert unabhängig von den Buchungsregeln.** Das ist der Kern: Eine untergeschobene Rechnung ist buchungstechnisch völlig unauffällig — richtiger Betrag, richtige Steuer, richtige Richtung. Nur die Bankverbindung stimmt nicht. Im Test wurde genau so ein Beleg blockiert, obwohl alle Buchungsprüfungen grün waren.
- **Auch ein „trotzdem buchen" hebt den Betrugsverdacht nicht auf.** Wer eine Buchungsregel bewusst übergeht, meint die Buchungslogik — nicht „ich habe die Echtheit geprüft".

**Verbesserungen**
- **Deine Qonto-Kontonummer ist korrigiert** — und zwar automatisch. Bruno hatte zuvor behauptet, das ginge nur von Hand. Beim Nachtesten stellte sich heraus: Die Einschränkung gilt für Firmen-Stammdaten, nicht für Konten. Zwei Stellen fehlten, jetzt stimmt sie.
- **Der Schutz kennt jetzt deine Anbieter-Historie** (rund 1.500 Vergleichsbelege aus deinem Archiv). Ohne sie könnte die wichtigste Prüfung — „dieser Lieferant hatte immer eine andere Bankverbindung" — gar nicht anschlagen.
- **Fehlt die Historie, sagt Bruno es laut.** Beim ersten Einhängen fand er null Vergleichsbelege und hätte still weitergeprüft, ohne etwas finden zu können. Jetzt erscheint eine deutliche Warnung statt einer falschen Entwarnung.

**Wissensstand:** unverändert 2026-07-12.

## v1.59.0 — Bruno sagt dir am Ende, was als Nächstes dran ist

**Warum das wichtig ist**
- Bisher endete jeder Lauf mit einer Liste offener Punkte. Die sagt, was **offen** ist — nicht, was **zuerst** dran ist und warum. Bei zehn offenen Punkten war das Sortieren deine Arbeit, nicht deine Entscheidung.

**Neue Funktionen**
- **Jeder Bericht endet jetzt mit einer Empfehlung.** Höchstens drei Schritte, priorisiert, jeder mit einem Satz „warum jetzt" und einer Risiko-Einschätzung in normaler Sprache: umkehrbar? braucht es deine Freigabe? kann etwas verloren gehen? Dazu ausdrücklich, was **bewusst warten kann**.
- **„Nichts tun" ist eine gültige Empfehlung** — wenn der Zustand stabil ist, sagt Bruno das, statt Arbeit zu erfinden.
- Was Bruno selbst erledigen könnte, bietet er an („sag Bescheid, dann mache ich das") statt es dir als Aufgabe zu geben.

**Verbesserungen — zwei Regeln, die es zwar gab, die aber nicht griffen**
- **Erkenntnisse landen jetzt sofort in den Regeldateien.** Die Pflicht stand schon da, aber nur als Nebensatz — und wurde prompt einmal übersehen: vier von fünf Erkenntnissen wurden eingetragen, die fünfte blieb im Chat und wäre verloren gewesen. Jede Lehre nennt ab sofort die Datei, in der sie steht; fehlt die Angabe, gilt der Punkt als unfertig.
- **Vor dem endgültigen Löschen wird jetzt geprüft, ob eine Sicherung existiert.** Auch diese Regel stand bereits, war aber unbelegt: Das neue Aufräum-Werkzeug hatte keinen Rückweg — die Sicherung war reiner Zufall. Jetzt bricht das Werkzeug ab, wenn keine vollständige Sicherung vorliegt.

**Die dahinterliegende Einsicht**
- **Eine Regel, die nur im Fließtext steht, wird in langen Läufen übersehen** — nicht aus Nachlässigkeit, sondern weil nichts sie abfragt. Neue Regeln bekommen deshalb einen eigenen Auslöser („nach jedem X", „bevor Y läuft") und möglichst eine sichtbare Spur, an der man später prüfen kann, ob sie eingehalten wurde.
- Beide Erkenntnisse gelten nicht nur für Bruno und stehen deshalb zusätzlich in den übergreifenden Grundregeln.

**Unter der Haube**
- `CLAUDE.md`: Report-Block 12 (🧭 Empfehlung) + Eintragungs-Pflicht mit Fundstelle und Selbstprüfung · `docs/GRUNDREGELN.md` Cluster 6 (Learning ohne Fundstelle = unfertig · Nebensatz-Warnung) + Cluster 8 (Empfehlung auch nach Arbeits-Läufen, bisher nur nach Analysen) · `SKILL.md` auf 12 Blöcke · `dubletten-bereinigen.mjs`: Sicherungs-Gate vor Phase 2/3 + ehrliche Rollback-Lage im Kopf · `state.md` auf aktuellen Stand.

**Wissensstand:** unverändert 2026-07-12.

## v1.58.1 — Schutz vor untergeschobenen Rechnungen

**Warum das wichtig ist**
- Wenn Bruno dein Postfach nach Rechnungen durchsucht, findet er alles, was dort ankommt — auch das, was jemand dir absichtlich geschickt hat. Firmenname, Logo und eine echt aussehende Bankverbindung lassen sich besorgen. Landet so eine Rechnung unbemerkt in der Buchhaltung, sieht sie dort **vertrauenswürdig aus** (sie kommt ja aus deinem eigenen System) — und wird später bezahlt.

**Zuerst die Entwarnung**
- **Bruno überweist nichts.** Es gibt in seinem gesamten Code keine Funktion, die Geld bewegt. Der bekannteste Angriff — „Rechnung mit getauschter Kontonummer, Software zahlt automatisch" — ist hier nicht möglich, nicht weil eine Regel es verbietet, sondern weil die Fähigkeit fehlt.
- Der echte Schaden wäre indirekt: eine erfundene Rechnung, die **du** später in gutem Glauben bezahlst, plus falsch gezogene Vorsteuer.

**Neue Funktionen**
- **Prüfungen vor dem Buchen.** Die wichtigste: **Bekannter Lieferant, aber plötzlich eine andere Bankverbindung** → wird nie automatisch gebucht. Das ist das häufigste Muster bei manipulierten Rechnungen.
- Dazu: ungewöhnlich hoher Betrag für diesen Anbieter · Formulierungen, die zur schnellen Zahlung drängen · Absender-Domain passt nicht zum Anbieter · brandneuer Anbieter mit sofort hoher Summe.
- Bei Verdacht sagt Bruno auch, **wie** du prüfst: beim Anbieter über eine Nummer aus einem alten Beleg zurückrufen — niemals über die Kontaktdaten aus der verdächtigen Mail.

**Was ausdrücklich KEIN Verdacht ist**
- **Eine noch nicht bezahlte Rechnung.** Das war zwischenzeitlich als Warnsignal eingebaut und wurde auf Marcels Einwand wieder entfernt. „Beleg da, Zahlung noch nicht" ist völlig normal: Das Zahlungsziel läuft noch, man hat es schlicht noch nicht überwiesen, die Lastschrift kommt später, oder die Bankdaten reichen nicht weit genug zurück.
- Der Zustand lässt sich von einer echten offenen Rechnung nicht unterscheiden. **Ein Kriterium, das bei vielen korrekten Belegen anschlägt, ist kein Kriterium** — und ein Schutz, der ständig grundlos warnt, wird abgeschaltet und schützt danach gar nichts mehr.

**Für Betriebe, in denen die Buchhaltung nicht selbst bestellt**
- Neue Einstellung `beleg_freigabe`. Wer alles selbst beauftragt, erkennt eine erfundene Rechnung am Inhalt — dort meldet Bruno nur echte Auffälligkeiten.
- Wo aber jemand Belege verbucht, die **andere** beauftragt haben (angestellte oder externe Buchhaltung), fehlt der stärkste Schutz überhaupt: der Mensch, der sagt „das habe ich nie bestellt". Dann meldet Bruno eine Stufe früher und empfiehlt bei **jedem neuen Lieferanten** eine Bestätigung durch die bestellende Person.

**Kein Fehlalarm-Automat**
- Von 21 Tests sind acht bewusst **Gegenproben**: normale Preisschwankungen, Rechnungsdienstleister wie Paddle, ein neuer kleiner Anbieter, eine noch offene Rechnung — nichts davon darf Alarm auslösen. Ein Schutz, der ständig grundlos warnt, wird abgeschaltet und schützt dann gar nichts.
- Danach wurde jede Prüfung einzeln außer Kraft gesetzt, um nachzuweisen, dass sie wirklich greift.

**Dabei zwei eigene Fehler gefunden**
- Die Kontonummer-Erkennung las bei Nummern mit Leerzeichen das folgende Wort mit — der Vergleich hätte **nie** angeschlagen, die Kernprüfung wäre wirkungslos gewesen.
- Die Ausnahmeliste für Rechnungsversender enthielt das Wort „billing" — ein Angreifer hätte nur eine Domain mit „billing" registrieren müssen, um die Prüfung abzuschalten. Beides behoben.

**Unter der Haube**
- Neu `system/_lib/betrug-gate.mjs` (Signale S1–S5 + S7; S6 = wirkungslose Kontext-Notiz) · neu `tools/sevdesk-connector/canary-betrug.mjs` (21 Kanarien, sabotage-verifiziert) · HARD-RULE #17 dokumentiert · `PROFIL.md → beleg_freigabe`.

**Wissensstand:** unverändert 2026-07-12.

## v1.57.0 — „Grün" heißt jetzt wirklich geprüft, nicht nur „nichts gefunden"

**Warum das wichtig ist**
- Eine Prüfung braucht Werte. Fehlt der Betrag oder die Rechnungsnummer auf einem Beleg, konnte Bruno bisher nichts vergleichen — und meldete stillschweigend nichts. **Für dich sah das aus wie „alles in Ordnung".** In Wahrheit hieß es nur: „hier konnte ich gar nicht hinsehen."
- Der unangenehme Teil: Genau bei schlecht erkannten Belegen fehlen diese Felder am häufigsten. Die Prüfung war also ausgerechnet dort blind, wo sie am nötigsten gewesen wäre.

**Neue Funktionen**
- **Datenlücken werden jetzt gemeldet.** Bruno sagt dir, bei wie vielen Belegen ein prüfrelevantes Feld fehlt und welche Prüfung dadurch ausfällt — im Klartext: „Sie sind nicht geprüft-und-sauber, sondern ungeprüft."
- **An deinem Bestand gefunden** (1.321 Belege): Rechnungsnummer fehlt bei 34 %, Steuersatz bei 23 %, Bruttobetrag bei 12 %. Diese Belege liefen bisher unbemerkt durch.
- Gemeldet wird die **Quote**, nicht jeder einzelne Beleg — sonst wäre der Report unlesbar. Ab 20 % als Warnung, ab 5 % als Hinweis.
- Es ist ausdrücklich **kein Buchungsfehler**, sondern eine Frage der Beleg-Erkennung. Der Vorschlag lautet entsprechend: Stichprobe lesen, bei systematischem Ausfall die Texterkennung wechseln.

**Woher der Hinweis kam**
- Aus einem Befund in Marcels Buchhaltungs-App: Dort übersprang eine Prüfung Belege ohne Steuerwert — ein Beleg mit **„2026 €" als Nettobetrag** (in Wahrheit die Jahreszahl aus dem PDF) lief durch achtzehn Prüfungen, ohne dass etwas auffiel. Marcel fragte, ob daraus für Bruno etwas folgt. Es folgte.
- **Der Gedanke dahinter gilt überall:** Ein fehlender Wert ist kein „nichts zu prüfen", sondern ein „ich kann nicht prüfen".

**Verbesserungen**
- Eine Prüfung („Mixed-Tax") hatte bisher gar keinen hinterlegten Lösungsweg — beim Bauen aufgefallen und nachgetragen.

**Unter der Haube**
- Neu Dim 17 „Datenlücken" in `system/_lib/health-check.mjs` (Quoten-Meldung je Feld, Schwellen 5 %/20 %, Reparatur-Art `beleg`) · Muster übernommen vom bestehenden Blindfleck-Wächter aus Dim 12, der seit 2026-07-13 existierte, aber nie auf die übrigen Felder angewandt wurde · Reparatur-Weg für Dim 16 nachgetragen.

**Wissensstand:** unverändert 2026-07-12.

## v1.56.0 — Der Health-Check sagt dir jetzt auch, wie du es behebst

**Warum das wichtig ist**
- Bisher endete jeder Befund bei „hier ist ein Problem". Was man dagegen tun kann, stand nirgends — obwohl Bruno für fast jeden Fall längst ein Werkzeug hatte. Konkret: **neun Reparatur-Werkzeuge existierten, in vierzig Befunden wurde genau eines erwähnt.**
- Für dich hieß das: Du liest „6 Belege mit falscher Steuerregel" und weißt nicht, ob das zehn Minuten oder zwei Tage Arbeit sind, ob Bruno das kann oder du ranmusst.

**Neue Funktionen**
- **Jeder Befund sagt jetzt, wer ihn beheben kann.** Am Ende des Health-Checks stehen drei Gruppen:
  - 🔧 **Bruno kann das beheben** — mit dem Hinweis, dass immer erst ein Trockenlauf läuft
  - 👤 **Nur du kannst das** — etwa eine Kontonummer, die das Buchhaltungssystem nur per Hand ändern lässt
  - 📄 **Beleg fehlt** — kein Softwareproblem, die Rechnung muss beschafft werden
- **Endgültige Schritte sind als solche gekennzeichnet.** Wo etwas gelöscht wird, steht sichtbar „ENDGÜLTIG — nur nach ausdrücklicher Freigabe" dabei. Umkehrbares ist ebenfalls als umkehrbar markiert, damit du weißt, wo du gefahrlos zustimmen kannst.
- **Am echten Bestand:** 21 Befunde könnte Bruno selbst beheben, 19 brauchen dich, 2 brauchen eine Rechnung — und **kein einziger** blieb ohne bekannten Weg.

**Warum es kein eigener Menüpunkt geworden ist**
- Der erste Vorschlag war ein zusätzlicher Modus „Reparatur". **Marcels Einwand hat ihn verworfen:** Reparatur ist immer die Folge einer Prüfung, kein eigener Einstieg. Ein separater Menüpunkt hätte das Problem nur verlagert — wer einen Befund liest und keinen Hinweis auf eine Lösung sieht, findet auch nie in einen Reparatur-Modus.
- Deshalb hängt die Lösung jetzt direkt am Befund, wo du sie ohnehin liest.

**Verbesserungen**
- Der Health-Check bleibt wie bisher **rein lesend**. Er sagt, was möglich wäre — ausgeführt wird nur über die gewohnten Freigaben.
- **Neue Regel intern:** Eine neue Prüfung gilt erst als fertig, wenn ihr Reparaturweg hinterlegt ist. Sonst entstehen wieder Befunde ohne Ausweg.

**Unter der Haube**
- Neu `system/_lib/reparaturen.mjs` (Zuordnung Befund → Werkzeug, mit `art` automatisch/nutzer/beleg und `umkehrbar` ja/teilweise/nein; feine Fälle über `kind` statt `dim`) · `health-check.mjs` hängt sie an jedes Finding (`f.reparatur`) und zählt sie in `summary.reparierbar` · Chat-Report um die Sektion „Was sich davon beheben lässt" ergänzt.

**Wissensstand:** unverändert 2026-07-12.

## v1.55.0 — Bruno kann jetzt sehen, welcher Beleg an welcher Kontobewegung hängt

**Warum das wichtig ist**
- Wenn eine Rechnung mit einer Kontobewegung verknüpft ist, war diese Verbindung für Bruno bisher unsichtbar. Er konnte sie herstellen, aber nicht nachlesen. Das klingt harmlos, ist es aber nicht: Sobald man eine solche Verbindung löst — etwa beim Aufräumen doppelter Kontobewegungen — war sie **unwiederbringlich weg**. Man hätte anschließend raten müssen, welche Rechnung zu welcher Zahlung gehört.
- Am konkreten Fall: Von 127 verknüpften Kontobewegungen wären nur 37 sicher wiederherstellbar gewesen. Die übrigen 90 hättest du von Hand zuordnen müssen — bei gleichen Abo-Beträgen und Dollar-Rechnungen eine mühsame Rätselei.

**Neue Funktionen**
- **Bruno liest bestehende Verknüpfungen jetzt zuverlässig aus.** Ergebnis im Praxistest: **127 von 127** exakt aufgelöst, keine einzige Lücke, kein Raten über Beträge. Damit lassen sich Verknüpfungen gefahrlos lösen und danach wieder korrekt setzen.
- **Pflicht vor jedem Lösen:** Bruno sichert die bestehenden Verbindungen erst in eine Datei, bevor er eine davon anfasst. Selbst wenn zwischendrin etwas schiefgeht, ist die Zuordnung dokumentiert.

**Wie das gefunden wurde**
- Ehrlich gesagt: Bruno hatte vorher behauptet, es ginge nicht. Zehn verschiedene Wege ausprobiert, alle erfolglos, und daraus geschlossen, dass die Schnittstelle das nicht kann.
- Die Wahrheit stand seit fünf Tagen in Brunos eigener Dokumentation — er hatte nur eine von fünf Dateien durchsucht. **Marcels Nachfrage „hast du schon in der API-Dokumentation geschaut?" hat es aufgedeckt.**
- Der entscheidende Hinweis kam dann aus einer ganz normalen Suche im Buchhaltungsprogramm: Die Oberfläche benutzte die ganze Zeit den Weg, den Bruno für nicht existent erklärt hatte.
- **Regel daraus, fest eingebaut:** „Steht nicht in der Spezifikation" heißt nicht „geht nicht". Bevor Bruno künftig etwas für unmöglich erklärt, prüft er alle eigenen Unterlagen und sieht nach, welchen Weg die Oberfläche selbst nimmt.

**Verbesserungen**
- **Zwei Fehlalarme im eigenen Prüfvorgehen abgestellt.** Das Buchhaltungssystem meldet „erfolgreich" auch dann, wenn es eine Änderung stillschweigend verworfen hat. Bruno prüft deshalb ab jetzt nach jeder Änderung frisch nach, statt der Erfolgsmeldung zu glauben. Ebenso bei Filtern: Ein Filter, der zufällig plausibel aussieht, wird mit einem zweiten Wert gegengeprüft — genau so fiel auf, dass einer gar nicht funktioniert.

**Unter der Haube**
- `GET /CheckAccountTransaction?embed=log,log.object` liefert `log[].object` = der verknüpfte Voucher (undokumentiert, aus UI-Traffic; Readback bleibt Pflicht) · `system/connectors/sevdesk/CAPABILITIES.md` mit der vollständigen Negativ-Tabelle (10 getestete Wege) plus dem Nachtrag, der sie überholt · Sicherung `_probe-out/BACKUP-links-127-2026-07-18.json`.

**Wissensstand:** unverändert 2026-07-12.

## v1.54.0 — Der Health-Check findet doppelte Kontoumsätze und falsche Kontonummern

**Warum das wichtig ist**
- Die Vorprüfung aus v1.53.0 schützt ab jetzt vor doppelten Importen — aber nur bei NEUEN Importen. Was schon in den Büchern liegt, findet sie nicht. Und eine falsch hinterlegte Kontonummer fiel bisher überhaupt nirgends auf. Beides prüft der Health-Check (Modus 16) jetzt bei jedem Durchlauf mit.

**Neue Funktionen**
- **Doppelte Kontoumsätze werden gefunden.** Der Health-Check sucht denselben Umsatz in zwei verschiedenen Konten. Entscheidend ist dabei nicht die Menge, sondern die **Zeitgrenze**: Ein versehentlich doppelt eingelesener Zeitraum hört abrupt auf — echtes Geld, das zwischen deinen eigenen Konten hin und her fließt, ist über die Zeit verteilt. Nur der erste Fall wird als Fehler gemeldet, der zweite als harmlos eingestuft.
  - Derselbe Umsatz **mehrfach im selben Konto** gilt bewusst nur als Hinweis, nie als Fehler: Sechs identische Kleinbeträge am selben Tag sind bei manchen Anbietern völlig normal. Bruno sagt dir, wo du nachsehen kannst, statt Alarm zu schlagen.
  - Am Praxistest sofort gefunden: 548 doppelte Umsätze (Σ 44.881 €) im Zeitraum Januar bis Dezember 2025.
- **Kontonummern werden geprüft.** Für jedes Konto: Ist eine Kontonummer hinterlegt? Ist sie gültig (Prüfziffer)? Tragen zwei Konten versehentlich dieselbe? Gibt es Konten ganz ohne Umsätze? Ein Konto ohne Kontonummer ist dabei nur ein Hinweis — bei einer Kasse ist das normal.
  - Am Praxistest sofort gefunden: die zwei Stellen zu kurze Qonto-Kontonummer. Solange die falsch ist, kann jede Prüfung, die über sie läuft, gar nichts finden — ein blinder Fleck, der nirgends sichtbar wurde.

**Verbesserungen**
- **Der Health-Check wird nicht mehr stillschweigend blind.** Bisher hat er höchstens 2.000 Belege und 2.000 Kontoumsätze geladen — darüber hinaus hätte er den Rest einfach nicht geprüft und trotzdem grün gemeldet. Jetzt lädt er alles, seitenweise, und sagt es dir, falls doch je eine Grenze greift. Ein Prüfer, der blind wird, ohne es zu sagen, ist schlimmer als gar keiner.
- **Beim Einlesen von Kontoauszügen (Modus 17) erklärt Bruno vorab in einem Satz**, was er prüft, bevor er etwas schreibt. Keine zusätzliche Rückfrage — nur damit du weißt, was passiert.

**Sicherheit**
- 12 neue automatische Tests. Danach wurde jede der vier Prüfungen einzeln absichtlich außer Kraft gesetzt, um nachzuweisen, dass genau der zuständige Test das meldet und kein anderer.
- Dabei fiel auf, dass einer der Tests zunächst aus dem **falschen Grund** grün war: Der Fall scheiterte schon an einer früheren Bedingung und erreichte die eigentlich zu prüfende Stelle nie. Der Test wurde umgebaut, bis wirklich nur noch die Zeitgrenze über das Ergebnis entscheidet. Ein grüner Test, der nichts beweist, ist gefährlicher als gar keiner.
- Der Health-Check bleibt wie bisher **rein lesend** — er schlägt vor, er ändert nichts.

**Unter der Haube**
- `system/_lib/health-check.mjs`: neue Dimensionen 14 (`pruefeDoppelteUmsaetze`, Inhalts-Schlüssel Datum|Betrag|Partner, Trennschärfe über Zeitgrenze + Anteil ≥50 % + ≥20 Treffer) und 15 (`pruefeKontenStammdaten`); `ibanGueltig` wird aus `import-preflight.mjs` importiert statt dupliziert (eine Quelle für beide Prüfungen) · `tools/sevdesk-connector/health-check.mjs`: `getPaged()` ersetzt die harten 2000er-Limits bei Belegen und Kontoumsätzen, `GET /CheckAccount` neu (fällt bei fehlender Berechtigung lautlos weg, statt falsch zu entwarnen) · neu `tools/sevdesk-connector/canary-health-konten.mjs` (12 Kanarien) · SKILL Modus 16 auf 15 Dimensionen aktualisiert (war auf 11 stehengeblieben), Modus 17 um den Transparenz-Satz ergänzt.

**Wissensstand:** unverändert 2026-07-12.

## v1.53.0 — Kontoumsätze können nicht mehr doppelt oder ins falsche Konto importiert werden

**Warum das wichtig ist**
- Bei Marcel waren **555 Kontoumsätze aus 2025 doppelt** in der Buchhaltung (Σ 44.907 €). Ursache: eine Bank-Datei wurde in das falsche Konto eingelesen und Tage später nochmal in das richtige. Beide Konten sahen für sich plausibel aus, deshalb fiel es wochenlang nicht auf — es verfälschte aber Kontostand, Belegquote und Steuer-Vorschau. Diese Version macht genau das unmöglich.

**Neue Funktionen**
- **Vorprüfung vor jedem Einlesen von Kontoumsätzen.** Bevor auch nur eine Zeile geschrieben wird, beantwortet Bruno vier Fragen und bricht bei Bedarf ab:
  - **Gehört die Datei überhaupt zu diesem Konto?** Viele Bank-Exporte nennen die eigene Kontonummer in der Datei. Passt sie nicht zum gewählten Konto, ist Schluss. Das ist der einzige Schutz, der schon beim allerersten Einlesen greift.
  - **Wurde diese Datei schon einmal eingelesen?** Sind alle Zeilen bereits vorhanden, wird abgebrochen statt verdoppelt.
  - **Liegen diese Umsätze schon in einem anderen Konto?** Genau der Fall, der die 44.907 € verursacht hat.
  - **Holt die Bank diesen Zeitraum ohnehin selbst?** Bei verbundenen Konten liefert die Bank die letzten 90 Tage automatisch. Reicht deine Datei in dieses Fenster, warnt Bruno und nennt das passende Enddatum.
- **Dein Szenario ist abgedeckt:** erst 2025 exportieren, dann versehentlich 2025+2026 und beides einlesen. Der bereits vorhandene Teil wird erkannt und übersprungen, der neue Teil geht durch — mit Hinweis, ohne Abbruch.
- **Dubletten-Suche (`dubletten-report.mjs`).** Findet Kontoumsätze, die in zwei Konten doppelt liegen, und prüft selbst mit, ob es wirklich ein Fehl-Import war oder womöglich eine echte Doppelzahlung — im Zweifel rät es ab. Rein lesend; gelöscht wird nichts.

**Verbesserungen**
- **Qonto-Dateien lassen sich jetzt direkt einlesen**, ohne Umweg über eine vereinfachte Zwischendatei. Der Umweg hatte die Spalte mit der eigenen Kontonummer verworfen — also genau den Beweis, der den Fehl-Import verraten hätte.
- **Zwei stille Fehlerquellen beim Einlesen behoben:** Der Status „Abgerechnet" (Qonto) wurde bisher nicht als abgeschlossen erkannt, wodurch **jede** Zeile verworfen worden wäre — gemeldet als harmloses „0 neu". Und Datumsangaben mit Bindestrich und Uhrzeit (`11-02-2026 21:54:30`) wurden gar nicht gelesen. Beides ist zu, inklusive Schutz davor, `11-02` als 2. November statt 11. Februar zu verstehen.
- **Kontonummern werden auf Gültigkeit geprüft** (Prüfziffer). Dabei kam heraus, dass die Qonto-Kontonummer im Buchhaltungssystem zwei Stellen zu kurz hinterlegt ist — ein Fehler, der den Kontoabgleich an dieser Stelle dauerhaft blind gemacht hätte.

**Sicherheit**
- 16 automatische Tests, jede der fünf Schranken zusätzlich absichtlich außer Kraft gesetzt, um nachzuweisen, dass der zugehörige Test das auch meldet. Zum Schluss der Praxistest: die **echte** Datei von damals gegen das falsche Konto — Import gestoppt, keine Zeile geschrieben. Gegen das richtige Konto läuft dieselbe Datei sauber durch.
- Wenn du sicher bist, dass die Vorprüfung sich irrt, gibt es einen bewusst umständlichen Notausgang (`--trotzdem-importieren`). Versehentlich tippt den niemand.

**Unter der Haube**
- Neu `system/_lib/import-preflight.mjs` (Tore T1/T1a IBAN + Prüfziffer nach ISO 7064 mod-97 · T2 Ziel-Dublette · T3 Fremdkonto-Dublette · T4 API-Fenster; offline testbar) · neu `tools/sevdesk-connector/canary-import-preflight.mjs` (16 Kanarien, jedes Tor sabotage-verifiziert) · neu `tools/sevdesk-connector/dubletten-report.mjs` (read-only, Trennschärfe-Prüfung + JSON-Export) · `import-transactions.mjs`: Vorprüfung vor dem ersten Write, Qonto-Rohformat-Erkennung, eigene Konto-IBAN aus dem Dateikopf (nur wenn alle Zeilen dasselbe Konto nennen), Status-Synonyme, Datums-Parser für Bindestrich+Uhrzeit · PROFIL.md: Qonto-IBAN auf die prüfziffer-gültige 22-stellige Fassung korrigiert.

**Wissensstand:** unverändert 2026-07-12.

## v1.52.0 — Eigene Umbuchungen sicher erkennen (mit fünf Schutzschranken)

**Neue Funktionen**
- **Geld, das du zwischen deinen eigenen Konten hin und her schiebst, erkennt Bruno jetzt automatisch — und lässt es dich nicht mehr als „fehlenden Beleg" verfolgen.** Als Einzelunternehmer ist eine Umbuchung Geschäfts- ↔ Privatkonto eine Privatentnahme bzw. -einlage: dafür gibt es keine Rechnung und es braucht auch keine. Solche Bewegungen werden als „privat" gekennzeichnet (jederzeit rückgängig zu machen) und verschwinden aus deiner Liste offener Punkte. Deine Zahl „so viele Belege fehlen noch" wird damit endlich ehrlich.
- **Bruno findet dein Privatkonto selbst.** Statt dich nach der IBAN zu fragen, leitet er sie aus deinen eigenen Kontobewegungen ab und prüft sie sechsfach gegen: Bankleitzahl, Kontoinhaber-Name, Geldfluss in beide Richtungen, Verwendungszweck, Häufigkeit und Abgleich mit deinen Geschäftskonten. Erst wenn alle sechs passen, gilt das Konto als bestätigt.

**Verbesserungen (Sicherheit)**
- **Fünf Schutzschranken verhindern, dass je eine echte Betriebsausgabe versehentlich als „privat" verschwindet:**
  - **Rechtsform:** Bei GmbH/UG/AG ist die Automatik komplett gesperrt. Dort ist Geld zwischen Firma und Gesellschafter ein Darlehen bzw. eine verdeckte Gewinnausschüttung — das braucht einen Vertrag, keine schnelle Markierung.
  - **Namensgleichheit:** Der Name muss **exakt** stimmen. Heißt du „Thomas Müller" und ein Lieferant „Thomas Müller Bedachungen GmbH", bleibt dessen Rechnung unangetastet. (Vorher hätte eine Teil-Übereinstimmung genügt — genau diese Lücke ist jetzt zu.)
  - **Darlehen:** Steht im Verwendungszweck ein Hinweis auf Darlehen, Tilgung oder Gesellschafter, entscheidest du — nie die Automatik.
  - **Zahlungsdienstleister:** Auszahlungen von Stripe, PayPal, Mollie & Co. werden nie als privat behandelt; sie gehören in die Verrechnung deiner Verkäufe.
  - **Zwei-Signale-Regel:** Ein passender Name allein reicht nicht. Ohne zusätzliche Bestätigung durch die Kontonummer gibt es nur einen Vorschlag zum Ansehen, keine stille Buchung.
- **Bewiesen statt behauptet:** 19 automatische Sicherheitstests prüfen diese Schranken. Zusätzlich wurde jede Schranke absichtlich außer Kraft gesetzt, um nachzuweisen, dass der zugehörige Test das auch wirklich meldet. Dabei fielen zwei echte Schwächen auf, die vorher niemandem aufgefallen wären: ein Test, der auch bei kaputter Schranke „grün" gemeldet hätte, und eine zu breite Sperre, die 72 korrekt markierte Umbuchungen (34.341 €) blockiert hätte. Beides ist behoben.

**Unter der Haube**
- Neu `system/_lib/privat-gate.mjs` (Gates G0 Status · G1 Rechtsform · G2 Identität exakt/IBAN · G3 Darlehen · G4 PSP; offline testbar ohne API) · neu `tools/sevdesk-connector/canary-privat.mjs` (19 Kanarien, Sabotage-verifiziert) · `mark-tx-privat.mjs`: `--payee` ist nur noch Vorauswahl, das Urteil fällt das Gate; Notausgang `--ich-habe-jede-tx-selbst-geprueft` hebt ausschließlich das Identitäts-Gate auf, Rechtsform- und PSP-Sperre bleiben scharf · PROFIL.md neu: `privat_konten`, `privat_kontoinhaber`, `eigene_geschaeftskonten` (Geschäfts-IBANs selbst aus `GET /CheckAccount` gelesen) · PROFIL-Parser: Klammer-Kommentare werden vor dem `|`-Split entfernt (erzeugten sonst Pseudo-IBANs), Platzhalter `[ leer … ]` zählen als nicht gesetzt.

**Wissensstand:** unverändert 2026-07-12.

## v1.51.0 — Bild-Belege direkt beim Einlesen als PDF + noch robusterer Zahlungs-Schutz

**Verbesserungen**
- **Screenshot-/Foto-Belege werden jetzt schon beim Einlesen in ein PDF umgewandelt** — nicht erst als Reparatur beim Buchen. Ein abfotografierter oder gescreenshotteter Beleg (PNG/JPG) landet direkt als sauberes PDF in deiner Ablage, mit korrektem Namen. Damit ist der Buch-Lauf von vornherein abgesichert; die Zwischenlösung aus v1.50.0 wird gar nicht mehr gebraucht.
- **Der Schutz vor doppelt verknüpften Zahlungen greift jetzt auch bei bereits begonnenen Teilzahlungen.** Bisher erkannte Bruno eine Teilzahlungs-Kette (viele kleine Abbuchungen für eine Rechnung) nur, solange sie komplett offen war. War eine Kette schon halb bezahlt, konnte eine der übrigen Abbuchungen fälschlich einer anderen Rechnung zugeordnet werden. Das ist jetzt zu: Bruno erkennt auch angefangene Ketten und hält die restlichen Abbuchungen für die richtige Rechnung frei. Gegen den realen Fehlerfall geprüft — er würde jetzt verhindert, ohne eine einzige korrekte Zuordnung zu blockieren.

**Unter der Haube**
- `sort.mjs writeBeleg`: Bild-Belege (png/jpg/jpeg/tiff/webp) → PDF via `sips` beim Ablegen (Fallback: Bild bleibt bei sips-Fehler erhalten), `belegBasename` strippt jetzt Bild-Endungen (kein `.png.png`-Doppelsuffix mehr) · `match-vouchers.mjs findSplitReservedTx`: neuer Teil-Split-Zweig (status 750 oder 0<paid<gross) neben dem Voll-Split-Zweig, konservativ (reserviert nur, blockt nie), Unit-getestet gegen den 17.07.-Vorfall + 0 Über-Reservierung im Live-Dry.

**Wissensstand:** unverändert 2026-07-12.

## v1.50.0 — Screenshot-Belege werden sauber verarbeitet + bessere Kontoauszug-Erkennung

**Verbesserungen**
- **Screenshot-Belege (z. B. abfotografierte Abo-Rechnungen als PNG) werden jetzt zuverlässig gebucht:** Bisher konnte ein Bild-Beleg dazu führen, dass ein Buch-Lauf mittendrin abbrach, weil die passende PDF-Datei fehlte. Bruno wandelt Bild-Belege jetzt in ein PDF um, bevor er bucht — der Lauf läuft sauber durch. (Ein Sicherheits-Wächter hatte den Abbruch korrekt erkannt und gestoppt, bevor etwas Falsches passieren konnte.)
- **Mehr Kontoauszüge werden automatisch als solche erkannt:** Auszüge im Fyrst-/Postbank-Stil („Alter Saldo / Neuer Saldo") landen jetzt zuverlässig im Kontoauszug-Archiv statt in der „unklar"-Ablage. Geprüft: echte Rechnungen werden dadurch nicht fälschlich als Auszug einsortiert.
- **Besserer Dubletten-Schutz beim Buchen:** Manche Anbieter stellen pro Monat zwei Rechnungen aus (z. B. Qonto: eine Grundgebühr und eine Gesamtrechnung, in der die Grundgebühr schon enthalten ist) oder nummerieren identische Belege mal mit, mal ohne „#"-Präfix. Bruno erkennt solche Doppelerfassungen jetzt beim Gesundheits-Check und entfernt die Dublette (reversibel, mit Nachweis) — so zählt keine Ausgabe doppelt in Report und Umsatzsteuer.

**Unter der Haube**
- PNG→PDF-Konvertierung vor dem Upload (`sips`, GoBD-tauglich) · `statement-detect.mjs` Salden-Marker um „alter/neuer Saldo" + „Saldovortrag" erweitert (additiv, diff-getestet) · Health-Check Dim 6 fängt jetzt auch `#`-Präfix-Rechnungsnummern und inhaltsgleiche sha256-Dubletten über verschiedene Rechnungsnummern.

**Wissensstand:** unverändert 2026-07-12.

## v1.49.0 — Kontoauszüge landen automatisch im Archiv + Schutz vor doppelt verknüpften Zahlungen

**Neue Funktionen**
- **Kontoauszüge werden jetzt automatisch archiviert:** Bisher hat Bruno aus deinen Kontoauszügen zwar die Umsätze eingelesen, das PDF selbst blieb aber liegen — obwohl Kontoauszüge 10 Jahre aufbewahrt und bei einer Betriebsprüfung vorgelegt werden müssen (§147 AO). Neu wandert jeder Auszug an einen festen Platz: **Belegarchiv → `<Jahr>` → `Kontoauszüge`**. Alle Konten eines Jahres liegen zusammen in EINEM Ordner, getrennt von den Rechnungen — im Quartalsordner stehen die Belege (*was* wurde gekauft), im Kontoauszüge-Ordner der Zahlungsnachweis (*dass* gezahlt wurde). Bei einer Prüfung hast du beides sofort zur Hand.
- Der Ordner erkennt Dubletten am Inhalt, nicht am Dateinamen: derselbe Auszug wird nie doppelt abgelegt, auch wenn er anders heißt. Abgeschlossene Jahre (laut deinem Profil) bleiben unangetastet.

**Verbesserungen**
- **Schutz vor doppelt verknüpften Zahlungen:** Manche Anbieter (z. B. Qonto) begleichen eine Monatsrechnung nicht mit einer Abbuchung, sondern mit vielen kleinen — in Summe ergeben sie den Rechnungsbetrag. Solange so eine Kette noch unvollständig war, konnte der normale Bankabgleich eine dieser Teilzahlungen fälschlich einer *anderen* Rechnung zuordnen. Bruno erkennt solche Ketten jetzt und lässt die Finger davon (dafür ist ein eigener Abgleich zuständig). Der Schutz wurde gegen echte Daten geprüft: er hätte den realen Fehlerfall verhindert und blockiert dabei keine einzige korrekte Zuordnung.
- **Ehrliche Laufzeit statt Schätzung:** Bruno misst die Dauer eines Laufs jetzt, statt sie im Bericht zu schätzen.
- **Schneller bei gleicher Sorgfalt:** unabhängige Prüfschritte (z. B. mehrere Lieferanten-Rechnungen fachlich einordnen) laufen jetzt parallel statt nacheinander. Geprüft: identisches Ergebnis. Das eigentliche Buchen bleibt bewusst Schritt für Schritt — dort wäre Parallelbetrieb ein Risiko für Doppelbuchungen.

**Unter der Haube**
- Neu `tools/bank-statement-parser/archive-statements.mjs` (Inhalts-Erkennung statt Dateiname, sha256-Dedup, `buchungsjahre`-Guard, Dry-Run-Default, Idempotenz bewiesen) · Split-Guard `findSplitReservedTx` in `match-vouchers.mjs` (beide Kandidaten-Pfade, HARD-RULE #7f) · Speed-Standards + Nicht-Parallelisieren-Begründung in CLAUDE.md verankert.

**Wissensstand:** unverändert 2026-07-12.

## v1.48.2 — Export-Härtung (intern)

**Unter der Haube**
- `export.sh` schloss zwei neue Marcel-interne Dateien noch nicht aus (`tools/pennylane-connector/` mit echten Lieferanten-Adressen einer Testfirma, `AUFGABEN-FUER-DICH.md` als gitignorte, aber von rsync trotzdem kopierte private To-do-Liste) — beide jetzt explizit ausgeschlossen, Re-Export verifiziert sauber.

## v1.48.0 — Fehler-Logs werden automatisch zu Marketing-/Webinar-Stories

**Neue Funktionen**
- **Auto-Marketing-Extraktor:** Bruno protokolliert jeden Live-Lauf inkl. gefundener Fehler und ihrer Behebung ohnehin (`LIVE-RUNS.md`). Neu zieht ein Tool daraus automatisch die „echter Fehler → echter Fix"-Geschichten und bereitet sie als Story-Kandidaten mit Verkaufssatz-Rohling auf — belegbar, nicht behauptet. Ideal für Webinar, Sales-Page, Social. Rein deterministisch (kein KI-Text, keine erfundenen Zahlen); die finale Auswahl kuratiert weiterhin ein Mensch.

**Verbesserungen**
- **Angebote sauber abgelegt:** Ein Angebot/Kostenvoranschlag (kein Buchungsbeleg) wandert jetzt automatisch in einen eigenen Ordner-5-Bereich statt in die Buchungs-Queue — mit Erklärung, warum es nicht gebucht wird.

**Unter der Haube**
- `system/_bin/marketing-wins-sync.mjs` (LIVE-RUNS → `marketing-assets/wins-from-live-runs.md`, Delta-State gitignored) · ABLAGE-MATRIX (CLAUDE.md) um `document_type: offer` → Ordner 5 erweitert.

## v1.47.0 — Angebote werden nicht mehr als Rechnungen verbucht (DE + EN)

**Verbesserungen**
- **Angebot ≠ Rechnung — jetzt sicher getrennt:** Ein Angebot oder Kostenvoranschlag ist kein Buchungsbeleg (kein Geldfluss, keine Rechnungsnummer). Bruno erkennt Angebote jetzt zuverlässig an ihren Merkmalen — **auf Deutsch UND Englisch** (Angebot, Kostenvoranschlag, Quote, Quotation, Estimate, Offer, Proposal …) — und behandelt sie als „kein Beleg" statt sie versehentlich als Rechnung einzubuchen. Eine echte Rechnung, die nur ihre Angebotsnummer erwähnt, bleibt korrekt eine Rechnung.
- Bereinigt: 2 Angebote, die früher fälschlich als Rechnungs-Entwürfe gebucht waren, wurden entfernt.

**Unter der Haube**
- `src/ocr/index.mjs`: eigener `document_type: offer` (getrennt von `dunning`), zweisprachige OFFER/RECHNUNG-Regex mit Verb-Ausschluss („to quote") + „estimated delivery"-Guard · Buch-Gate-Whitelist schließt `offer` automatisch aus · neuer Test `test-offer-detection.mjs` (17 Angebote + 14 Nicht-Angebote, DE+EN, grün) · 12 echte PDF-Fixtures verifiziert.

## v1.46.0 — Bank-Landkarte: „Welcher Beleg fehlt für welche Zahlung?" auf einen Blick

**Neue Funktionen**
- **Abgleich-Landkarte (Bank ↔ Beleg):** Deine Kontobewegungen sind die wichtigste Datenquelle — Bruno stellt sie jetzt als beidseitige Landkarte dar, nach Anbieter gruppiert: (A) Bank-Bewegungen ohne Beleg, (B) Belege ohne passende Bank-Zahlung, (C) Verdacht auf ein Abbuchungskonto, das noch nicht verbunden ist. So fällt sofort auf, wenn z. B. „6 DHL-Rechnungen, aber keine einzige Abbuchung in der Buchhaltung" heißt: das Konto fehlt. Bruno liest dann selbst die Abbuchungs-IBAN vom Beleg (statt nachzufragen), erkennt die Bank und schlägt den Konto-Import vor.
- Der Status-Scan (Modus 1.1) nutzt diese Landkarte jetzt als ersten Blick.

**Unter der Haube**
- `abgleich-landkarte.mjs` (read-only, Vendor-Cluster + Konto je TX + Beleg-ohne-TX-Richtung) · SKILL 1.1 verweist darauf · Grundhaltung #3 + HR#12 gehärtet: Zahlungsweg/Abbuchungskonto steht auf dem Beleg → selbst lesen, nie fragen.

## v1.45.0 — Selbstschutz + optionale Zweitmeinung beim Bank-Abgleich

**Neue Funktionen**
- **Frühwarnung für interne Schnittstellen:** Bruno nutzt einige Buchhaltungs-Funktionen, die die Software offiziell nur im Fenster anbietet (z. B. die Auszahlungs-Umbuchung). Ändert der Anbieter so etwas im Hintergrund, würde es bisher erst beim nächsten Lauf auffallen. Jetzt prüft Bruno bei jedem Health-Check und Nacht-Lauf kurz, ob diese Funktionen noch erreichbar sind — und meldet einen Ausfall sofort, statt still weiterzumachen.
- **Optionale Zweitmeinung beim Bank-Abgleich (`--crosscheck`):** Auf Wunsch fragt Bruno vor dem Verknüpfen die eingebaute Zuordnungs-Automatik der Buchhaltungs-Software als zweite Meinung. Weicht sie ab, nimmt Bruno die betroffene Zahlung sicherheitshalber aus dem Lauf und legt sie zum Nachschauen. Wichtig und ehrlich: diese eingebaute Automatik ist im Test **deutlich ungenauer** als Bruno (sie ordnete eine Zahlung dem falschen Anbieter zu, nur weil der Betrag passte). Deshalb ist sie nur ein schwaches Zusatz-Signal — sie stupst Bruno zum Prüfen an, überstimmt ihn nie.

**Verbesserungen**
- **Kein versehentlicher Start mehr beim Beleg-Einlesen:** Ein Tippfehler oder unbekannter Zusatz beim Aufruf des Beleg-Einlesers startete bisher stillschweigend einen echten Lauf. Jetzt bricht das Tool bei unbekannten Optionen sauber ab und `--help` zeigt die Möglichkeiten — gelesen wird nur, wenn der Aufruf eindeutig ist.

**Unter der Haube**
- `endpoint-liveness.mjs` (read-only, in Nacht-Lauf Schritt 0a + Health-Check) · `matching-crosscheck.mjs` + `match-vouchers --crosscheck` (Status-Guard: nur offene Zahlungen, sevDesk = schwaches Zweitsignal, blockt nie den Rest) · `ingest-local.mjs` usageGate (Flag-Whitelist, --help/-h, Exit 2 bei Unbekanntem).

## v1.44.0 — Stornos/Gutschriften: Bruno erstellt die fehlenden Belege selbst per Stripe-API

**Neue Funktionen**
- **Fehlende Storno-Belege direkt aus Stripe:** Wurde eine Zahlung erstattet, fehlte bisher der Gutschrift-Beleg (nur im Dashboard klickbar). Bruno erstellt die Gutschrift jetzt per API — sicher gekoppelt an die BEREITS erfolgte Erstattung (es wird nie neues Geld bewegt), ohne Kunden-E-Mail, mit Vorschau vor jedem Schritt und dokumentiertem Rückgängig-Weg. Das PDF wird automatisch gezogen und GoBD-konform abgelegt.
- **Auszahlungs-Umbuchungen jetzt vollautomatisch:** Die Umbuchung der Stripe-Auszahlung vom Bankkonto aufs Verrechnungskonto galt bisher als „nur im sevDesk-Fenster klickbar". Bruno hat den internen Weg gefunden (durch Beobachten, was sevDesk selbst beim Klick macht) und erledigt jetzt auch diesen letzten Schritt selbst — mit Sicherheitsprüfung vor und nach jeder Umbuchung. Du musst gar nichts mehr klicken.
- **Erlös-Gutschriften jetzt buchbar:** sevDesk akzeptiert keine negativen Belege — Bruno kennt jetzt den korrekten Spiegel-Weg (gedrehte Richtung + positiver Betrag aufs Erlösschmälerungs-Konto). Rechnung und Storno heben sich in Buchhaltung und Umsatzsteuer exakt auf.

**Verbesserungen**
- Der Buchhaltungs-Check erkennt die Gutschrift-Konvention und schlägt bei korrekten Stornos keinen Fehlalarm mehr.

**Unter der Haube**
- `create-credit-note.mjs` (endpoint-gewhitelistetes POST, refund-Link statt refund_amount, Preview/void) · Dim-5-Kopplung · API-Fund dokumentiert (negative Voucher nicht registrierbar).

## v1.43.0 — Stripe-Einnahmen sauber verrechnet (Erlös + Gebühr + Auszahlung)

**Neue Funktionen**
- **Stripe-Verrechnung automatisch:** Stripe zahlt nie 1:1 aus — vom Kundenumsatz gehen erst die Gebühren ab, dann kommt ein Sammelbetrag aufs Konto. Bruno bildet das jetzt korrekt ab: Kundenumsätze und monatliche Gebühren-Eigenbelege laufen über ein eigenes Verrechnungskonto, die Bank-Auszahlung ist nur noch eine Umbuchung (kein „falscher Umsatz" mehr). Mit eingebauter Null-Kontrolle: jede Auszahlungs-Kette wird cent-genau gegen die Stripe-Daten geprüft — geht sie nicht auf, wird NICHT gebucht.
- **Sicherheits-Filter vor jeder Erlös-Buchung:** Bruno prüft je Rechnung, ob das Geld wirklich geflossen ist (Auszahlung angekommen?), ob voll bezahlt wurde und ob es eine Rückerstattung gab. Erstattete oder über Kunden-Guthaben verrechnete Rechnungen werden NICHT einfach als Umsatz gebucht, sondern landen mit Begründung auf deiner Klärliste — das verhindert überhöhte Umsätze und zu viel gemeldete Umsatzsteuer.

**Unter der Haube**
- Neues Tool `stripe-clearing.mjs` (Dry-Run-Standard, 4 Safe-Gates, Ketten-Null-Kontrolle als Abbruch-Invariante, Readback je Buchung, Audit-Log, bewiesener Rollback) · Gebühren-Eigenbelege pro Monat mit doppeltem Zahlen-Wächter · API-Wissen erweitert (Zahlungen auf Verrechnungskonto per API, Konten-Umbuchung nur im sevDesk-UI).

## v1.42.3 — Angebote und Kostenvoranschläge sind keine Belege

**Verbesserungen**
- Der PDF-Beweis und das Buchungs-Gate erkennen jetzt auch **Angebote/Kostenvoranschläge** (eindeutige Marker wie „Angebotsnummer") und halten sie aus der Buchhaltung heraus — realer Fund: zwei Angebote über 4.506 € waren als Rechnungs-Entwürfe gelandet.
- Voll-Audit über den gesamten Bestand (940 Belege): der GEBUCHTE Bestand ist zu 100 % sauber; 18 Auffälligkeiten in der ungebuchten Queue wurden markiert.

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
