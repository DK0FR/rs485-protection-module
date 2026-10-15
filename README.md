# rs485-protection-module – RS485 Protection Module

Überspannungs- und HF-Schutzmodul für RS485 und 24 V (Antennenrotor) – Projektzusammenfassung

Aufdruck auf der Platine: „A05 RS485 Protection Module / DH5WM 2026 / www.dk0fr.de“

Repository: https://github.com/DK0FR/rs485-protection-module · Fertigungsdaten: [Fertigung/JLCPCB_Rev4/rs485-protection-module_Rev4_JLCPCB.zip](Fertigung/JLCPCB_Rev4/rs485-protection-module_Rev4_JLCPCB.zip)

Stand: 15. Oktober 2026, Rev. 4. Die Platine ist geroutet und geprüft, die Gerberdaten sind bestellfertig. Die Schaltung ist berechnet, nicht gemessen.

## Ziel

Überspannungs- und HF-Schutz für einen Schrittmotor NEMA 17 mit integriertem Treiber (Treiberdaten unbekannt) an einem Antennenrotor. Der Motor hängt an 30 m ungeschirmter Steuerleitung (24 V DC und RS485, 2-Draht, bis 115 200 Bd) in der Nähe von KW-Antennen mit bis zu 750 W Sendeleistung. Zu schützen ist gegen eingekoppelte Überspannungen (Gewitter in der Nähe, nicht Direkteinschlag) und gegen HF-Einstreuung 1,8–30 MHz.

## Hardware

**Einbau:** Motor und Modul sitzen in einem geerdeten Vierkantrohr 50 × 50 mm. Das Modul liegt hinter dem Motor, die Steuerleitung geht über eine Hirschmann-Dose CA 6 GD (6 + PE) seitlich ins Rohr. Rohrlänge hinter dem Motor etwa 150 mm (10 mm Luft, 100 mm Platine, 40 mm Dose).

**Platine:** 100 × 38 mm, zweilagig, 36 Bauteile, vollständig geroutet. Vier M3-Löcher mit Metall-Abstandshaltern (5 mm) verbinden PE mit dem Rohr. Höchste Bauteile (F1 32,9 mm, L1 33,0 mm laut Datenblatt); bei 3 mm Wandstärke bleiben 4,4 mm Luft.

**Schutzkette 24 V** (Kabel → Motor): PTC-Sicherung MF-R800 → Varistor S14K30 → 10 nF/1 kV je Ader nach PE → Gasableiter 90 V (0 V gegen PE) → Gleichtaktdrossel 1 mH/7,5 A (Würth 744824801) → 10 nF nach PE → Gleichtaktdrossel 32 µH NiZn (Würth 744842632) → TVS SMCJ26A → 100 µF + 100 nF → 10 nF nach PE → Klemme. 0 V ist nicht fest geerdet, sondern über 10 nF, SMBJ15CA und 1 MΩ.

**Schutzkette RS485:** 1 nF/1 kV C0G je Ader nach PE → 3-poliger Gasableiter 90 V (Bourns 2036-09-SM) → 10 Ω Kohleschicht-MELF je Ader → Gleichtaktdrossel 2,2 mH (Würth 744272222) → 51 µH (Würth 744227) → TVS-Array SM712 → 1 nF nach PE → optionaler 120-Ω-Abschluss mit Steckbrücke → Klemme.

**Außerhalb der Platine:** Ringkern FT240-31 mit 12 Windungen (alle vier Adern gemeinsam) auf der Steuerleitung direkt vor dem Stecker, ein zweiter an der Stationsseite.

**Belegung:**

| Anschluss | Seite | Pins | Aufdruck oben (von oben nach unten) |
|---|---|---|---|
| Dose X1 | Kabel | 1 = +24 V, 2 = 0 V, 3 = A, 4 = B, PE = Rohr | – |
| J1 | Kabel | 1 = +24 V, 2 = 0 V | J1 +24V/0V |
| J3 | Kabel | 1 = A, 2 = B | J3 A/B |
| J2 | Motor | 1 = 0 V, 2 = +24 V | J2 +24V/0V |
| J4 | Motor | 1 = B, 2 = A, 3 = 0 V | J4 B/A/0V (senkrecht, von unten gelesen) |

Auf der Unterseite ist zusätzlich jeder Klemmenpin einzeln beschriftet.

**Drosselwicklungen (aus Würth-Datenblättern, nicht gemessen):**

| Drossel | Würth-Wicklungen | Phasenpunkte | Umsetzung |
|---|---|---|---|
| L1 744824801, L2 744842632 | 1–4 und 2–3 | 1, 2 | Footprint-Nummern weichen von Würth ab (1–2 / 4–3), die Lage der Wicklungen stimmt aber. Die Drosseln lassen sich nicht falsch einsetzen. |
| L3 744272222 (WE-SL5) | 1–2 und 4–3 | 1, 4 | Eigenes Symbol CMC_SL5 mit Würth-Nummern: A_R→4, A_MID→3, B_R→1, B_MID→2. Footprint um 90° gedreht. Ein Punkt im Bestückungsdruck markiert Würth-Pin 1 (Wicklungsanfang). |
| L4 744227 (WE-SL2) | 1–4 und 2–3 | 1, 2 | Symbol CMC_SL, passt. |

## Entscheidungen

- **Dreistufige Gleichtaktsperre** (Ringkern, MnZn-Drossel, NiZn-Drossel), weil eine einzelne 51-µH-Drossel bei 1,8 MHz nur rund 580 Ω bringt; Ziel sind 2–5 kΩ über das ganze KW-Band.
- **Kondensatoren nach PE vor den Gasableitern**, damit die HF-Spannung beim Senden die 90-V-Ableiter nicht zündet; Kondensatoren zwischen den Drosselstufen gegen Resonanz.
- **Gasableiter nicht an +24 V** (Bogenspannung unter 24 V, würde am Netzteil nicht löschen), stattdessen Varistor zwischen +24 V und 0 V, Gasableiter nur 0 V gegen PE.
- **SMCJ26A** als engste sinnvolle Klemmung bei unbekannter Treibergrenze; Netzteil auf höchstens 24,5 V stellen.
- **PTC-Sicherung MF-R800 (8 A)** statt Feinsicherung auf Wunsch; 8 A wegen Derating im warmen Rohr. Sie löst erst ab ~16 A aus, schützt also nur mit einem kräftigen Netzteil.
- **10 Ω statt 47 Ω** in den RS485-Adern (Signalpegel am Abschluss ~86 %), impulsfeste Kohleschicht-MELF statt 0,25-W-Typen.
- **Bauform:** ursprünglich Lochraster mit handlötbarem SMD; seit Rev. 4 eigene Leiterplatte, dadurch entfällt der SOT-23-Adapter.
- **Netzklassen:** Kabelseite 1,5 mm Abstand (2 mm hätten an den 1206-Pads angeschlagen), 24-V-Bahnen 2 mm breit (≈17 K bei 5 A, 1 oz), PE 1 mm, Signale 0,5 mm. Muster mit führendem „*“, damit sie auch auf die Schaltplan-Netznamen mit „/“ passen.
- **Interne Netze benannt** (+24V_MID, 0V_MID, A_R, B_R, A_MID, B_MID, TERM), damit die Klassenzuordnung ein Netzlisten-Update übersteht.
- **PE-Flächen geteilt:** auf beiden Lagen PE_EIN (x 100–128, kabelseitig, H1/H2) und PE_AUS (ab x 148,5, im unteren Bereich ab x 158,5, H3/H4). Unter L1 und L3 kein Kupfer, damit der Spannungsabfall über die PE-Kupferfläche die erste Drosselstufe nicht umgeht. Verbunden nur über eine 1-mm-Brücke am oberen Rand (y = 81,3) und über das Rohr. Pads vollflächig angebunden, Verbindungs-Vias etwa alle 8 mm, Via-Paare an jedem PE-Kondensator.
- **Keine 0-V-Fläche**, weil 0 V bewusst nicht fest mit PE verbunden ist.
- **Bestückungsdruck oben:**
  - Alle Texte 1,0/0,15 mm (JLCPCB-Minimum), ohne Überlappungen mit Pads, Konturen oder Gehäusen.
  - Statt J1–J4 stehen Klemmen-Legenden in Pin-Reihenfolge.
  - Bezeichner von H1–H4 ausgeblendet.

## Status

- Bauteile sind bei Mouser bestellt (23 Positionen). Noch nicht bestellt: Hirschmann-Dose, Abstandshalter M3 × 5, Isolierfolie, Leiterplatte.
- KEMET C1206C102JDGACTU war nicht lieferbar; Ersatz Yageo CC1206JKNPOCBN102 (gleiche Bauform).
- **Prüfung am 15.10.2026 (letzter Lauf 19:29, nach der Umbenennung):**
  - ERC: 0 Fehler.
  - DRC: 0 offene Verbindungen, keine Abstandsfehler, keine Meldungen zum Bestückungsdruck. Ein Fehler ist bewusst akzeptiert: Die Bestückungsflächen von L1 und L3 überschneiden sich um 0,1 mm; die Gehäuse haben etwa 1 mm Abstand.
  - Schaltplan und Layout: alle 79 Pins identisch verbunden. Die gemeldeten Netzkonflikte betreffen nur die Schreibweise („/+24V_PROT“ gegen „+24V_PROT“).
- **Footprints gegen Datenblätter geprüft und korrigiert:**
  - RV1: Bohrung 0,8 → 1,1 mm (Draht 0,8 ± 0,05 mm)
  - L1, L2: Bohrung 1,2 → 1,3 mm (Würth-Empfehlung)
  - GDT1: Pads 1,8 → 1,3 × 5,6 mm (Bourns-Empfehlung)
  - L3: A und B lagen auf derselben Wicklung (Kurzschluss); behoben, siehe Tabelle oben
  - 4 Vias 0,5/0,3 → 0,6/0,3 mm (Restring für JLCPCB)
  - Bestätigt: GDT2 (1,6 × 5,6 mm, Raster 3,3 mm), F1 (Raster 10,2 mm, Draht 0,81 mm), C1 (10 × 12,5 mm, Raster 5 mm), L4, Klemmen, Standardbauformen
- **Gerberdaten für JLCPCB** (zuletzt erzeugt am 15.10.2026 um 19:29):
  - Liegen in `Fertigung/JLCPCB_Rev4/`.
  - Bestellung: 2 Lagen, 100 × 38 mm, 1,6 mm, HASL bleifrei, 1 oz (2 oz gäbe mehr Reserve).
  - Bohrungen: 1,0 mm (C1, JP1), 1,1 mm (F1, RV1), 1,3 mm (L1, L2, Klemmen), 3,2 mm (M3), Vias 0,3/0,4 mm.

## Offene Punkte

1. **Lieferbarkeit der Gasableiter** 2035-09-SM-RPLF und 2036-09-SM-RPLF bei Mouser prüfen. Für beide gibt es aktuelle Datenblätter (2035-xx-SM Rev. R 10/26, 2036-xx-SM Rev. AB 01/26). Das 2036.pdf ohne „-xx-SM“ beschreibt nur die bedrahteten Typen.
2. **Hirschmann-Dose:** Einbautiefe messen, die 40 mm sind geschätzt. Rohrwandstärke bestätigen.
3. **Einführrichtung der Klemmen** in der 3D-Ansicht prüfen.
4. **Vor Inbetriebnahme:** Messen, ob am Motor 0 V am Gehäuse liegt. Falls ja, darf 0 V am Netzteil in der Station nicht geerdet sein (sonst Gleichstrom über Erde, L1 sättigt). Motorstrom und Netzteilstrom sind unbekannt.
5. **Stationsseite:** mindestens zweiter Ringkern, besser ein zweites Modul.
6. Lagerbestand bei Mouser wurde nicht geprüft, die Bestellnummern zu 9 von 23 Positionen stammen aus dem Gedächtnis.
7. **Kosmetisch:** Bibliothekswarnungen (Projektbibliothek „Rotor“ nicht eingebunden, Footprints weichen von der Bibliothekskopie ab). Sie betreffen die Fertigung nicht.
8. **Bei Dateiänderungen außerhalb von KiCad** KiCad vorher komplett schließen, auch die Projektverwaltung. Sonst überschreibt KiCad die Änderungen beim nächsten Speichern. Das ist am 15.10.2026 zweimal passiert, einmal mit dem Board und einmal mit den Netzklassen-Mustern in der Projektdatei.

## Artefakte

| Artefakt | Inhalt | Stand |
|---|---|---|
| `README.md` | Diese Zusammenfassung (früher „Projektzusammenfassung Rotor-Schutzmodul.md“ bzw. „rs485-protection-module.md“) | 15.10.2026 |
| Seite „Rotor-Schutzmodul RS485/24 V“ (claude.ai-Artefakt) | Schaltplan 24 V und RS485, Bestückungsplan, Seitenansicht im Rohr, Netzklassen, Stückliste, Mouser-Bestellliste mit Kopierknopf, Prüf- und Aufbauhinweise | Rev. 4, Version 5 (vor dem Routing; L3-Wicklungen dort noch falsch angenommen) |
| `rs485-protection-module.kicad_sch` | Schaltplan, ein A3-Blatt, Symbole eingebettet (L3 mit eigenem Symbol CMC_SL5), Footprints und MPN-Felder zugewiesen, Dose X1 und Befestigungslöcher enthalten | Rev. 4, 15.10.2026 |
| `rs485-protection-module.kicad_pcb` | Platine 100 × 38 mm, geroutet, geteilte PE-Flächen auf beiden Lagen, überarbeiteter Bestückungsdruck, DRC sauber (bis auf die akzeptierte Überschneidung L1/L3) | Rev. 4, 15.10.2026 |
| `rs485-protection-module.kicad_pro` | Projektdatei mit Netzklassen (Muster mit „*“) | Rev. 4, 15.10.2026 |
| `Fertigung/JLCPCB_Rev4/rs485-protection-module_Rev4_JLCPCB.zip` | Gerber (Protel-Endungen, ohne X2), Excellon in mm, PTH und NPTH getrennt, Bohrkarte, Pastenlagen | Rev. 4, 15.10.2026 19:29 |
| `Mouser-Bestellliste-Rotor-Schutzmodul (1).xlsx` | 23 Positionen für den Mouser-BOM-Import (Spalte A Herstellernummer, B Menge), Blatt „Hinweise“ mit Ersatztyp und Prüfstatus | Rev. 4 |

Das Projekt hieß bis 15.10.2026 „rotor-schutzmodul“. Ältere Sicherungen in `rs485-protection-module-backups/` tragen noch den alten Namen.

Frühere Stände: Rev. 1 Lochraster-Entwurf, Rev. 2 Grundkorrekturen und HF-Filter, Rev. 3 PTC-Sicherung.
