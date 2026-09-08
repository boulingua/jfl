---
title: "JLPT ↔ GER — Zuordnung und ihre Grenzen"
page_type: appendix
author: S. Le Boulanger
date: 2026-09-07
description: Warum dieser Kurs jede Einheit doppelt etikettiert — mit GER-Deskriptor-IDs und mit einer JLPT-Bande —, wie die Zuordnung begründet ist und wo sie nicht trägt.
tags: ["jlpt", "ger", "referenz"]
---

Japanischunterricht spricht JLPT, das Curriculum dieses Kurses spricht GER.
Beide Systeme werden im Kurs geführt, aber sie sind nicht gleichrangig: die
**GER-Deskriptor-ID ist verbindlich**, die **JLPT-Bande ist ein Etikett**.
Dieser Anhang begründet, warum.

## Die beiden Systeme messen Verschiedenes

Der **Gemeinsame europäische Referenzrahmen** beschreibt Handlungsfähigkeit.
Seine Deskriptoren sind Kann-Beschreibungen — „kann einfache Fragen zur Person
stellen und beantworten“ — und decken Rezeption, Produktion, Interaktion,
Mediation sowie sprachliche, soziolinguistische und pragmatische Kompetenzen
ab. Produktion und Interaktion, also Sprechen und Schreiben, sind darin
gleichberechtigte Gegenstände.

Das **Nihongo Nōryoku Shiken** (JLPT) ist ein Einstufungstest mit fünf Stufen,
N5 bis N1, wobei N5 die niedrigste ist. Geprüft wird in drei Bereichen:
Sprachwissen (Wortschatz und Grammatik), Lesen und Hören. Der Test enthält
**keinen Sprech- und keinen Schreibteil**; er ist vollständig im
Mehrfachwahlformat gehalten. Die Gesamtpunktzahl beträgt 180; die Grenze zum
Bestehen liegt bei 80 Punkten für N5, 90 für N4 und 95 für N3, jeweils
zusätzlich zu Mindestpunktzahlen pro Bereich.

Daraus folgt der zentrale Vorbehalt: **eine JLPT-Stufe kann nie einer
GER-Stufe entsprechen, weil sie die Hälfte dessen nicht misst, was die
GER-Stufe verlangt.** Wer N4 besteht, hat damit nichts über die eigene
Sprechfähigkeit nachgewiesen; A2 verlangt sie ausdrücklich.

## Die Zuordnung dieses Kurses

Trotzdem ist eine Zuordnung sinnvoll, weil sie den Wortschatz- und
Kanji-Umfang steuert, an dem sich japanische Lehrwerke, Wörterbücher und
Lernsoftware ausrichten. Der Kurs verwendet die folgende Zuordnung:

| GER-Stufe | JLPT-Bande | Rolle im Kurs | Kanji (Richtwert, inoffiziell) |
|---|---|---|---|
| Pre-A1 | — | Schriftstufe; JLPT setzt Kana voraus, statt sie zu prüfen | ca. 40 |
| A1 | N5 | Vollständige Stufe | ca. 100 |
| A2 | N4 | Vollständige Stufe | ca. 300 |
| B1 | N4–N3 | Vollständige Stufe; die Bandengrenze liegt innerhalb von B1 | ca. 650 |
| B2 | N2 | **Nicht Bestandteil dieses Kurses** — erklärte Lücke | — |
| C1 | N1 | **Nicht Bestandteil dieses Kurses** — erklärte Lücke | — |

Die Kanji- und Wortschatzrichtwerte sind ausdrücklich **inoffiziell**. Seit
der Revision des Tests 2010 veröffentlichen die Veranstalter keine
verbindlichen Wort- und Zeichenlisten mehr; die kursierenden Zahlen sind
Rekonstruktionen aus früheren Listen und aus der Praxis. Sie werden hier als
Planungsgröße benutzt, nicht als Norm.

## Wo die Zuordnung bricht

**Die Ränder passen nicht aufeinander.** Zwischen N4 und N3 liegt der größte
Sprung des gesamten Tests — an Wortschatz, an Kanji und an Textlänge. Der GER
kennt an dieser Stelle keinen entsprechenden Bruch: A2 geht in B1 gleitend
über. Deshalb steht B1 in der Tabelle oben auf **N4–N3** und nicht sauber auf
einer Bande.

**Rezeption läuft der Produktion davon.** Weil der JLPT nur rezeptive
Fähigkeiten prüft, erreichen Lernende regelmäßig eine JLPT-Stufe, deren
GER-Gegenstück sie im Sprechen deutlich verfehlen. Im Kurs wird dieser Abstand
nicht kleingerechnet: Sprech- und Schreibaufgaben sind an GER-Deskriptoren
gebunden, nicht an die JLPT-Bande.

**Die Schrift verzerrt die Skala nach unten.** Ein Lernender kann auf A1
sprachlich handlungsfähig sein und trotzdem an einem N5-Lesetext scheitern,
weil ihm dreißig Kanji fehlen. Genau dafür existiert in diesem Kurs die
Schriftstufe: sie nimmt die orthographische Last aus der A1-Stufe heraus, statt
sie mit der Sprechfähigkeit zu vermischen.

**Es gibt keine amtliche Gleichung.** Die Japan Foundation hat mit dem
*JF Standard for Japanese-Language Education* einen eigenen, ausdrücklich am
GER orientierten Rahmen vorgelegt — deshalb ist eine Zuordnung überhaupt
diskutierbar. Eine offizielle Umrechnungstabelle JLPT → GER gibt es aber
nicht. Die Tabelle oben ist eine redaktionelle Entscheidung dieses Kurses und
wird auch so ausgewiesen.

## Was daraus für den Kurs folgt

1. **Im Front Matter jeder Einheit** stehen beide Angaben: eine oder mehrere
   `curriculum.cefr_can_do`-Einträge, die auf IDs der Form
   `{LEVEL}.{DOMAIN}.{SCALE}.{SEQ}` aus `curriculum/levels/` auflösen, und
   ein JLPT-Tag.
2. **IDs werden nie lokal vergeben.** Fehlt eine benötigte Kann-Beschreibung,
   wird sie im Rahmenwerk ergänzt — nicht im Kurs erfunden.
3. **Prüfungen richten sich nach dem GER**, nicht nach dem JLPT-Format. Die
   Modellprüfungen dieses Kurses enthalten Sprechen und Schreiben und sind
   deshalb keine JLPT-Übungstests.
4. **Der JLPT-Tag steuert Auswahl, nicht Bewertung.** Er entscheidet, welche
   Kanji und welcher Wortschatz in einer Einheit vorkommen dürfen; bewertet
   wird gegen die Kann-Beschreibungen.

{{< callout type="warning" >}}
**Kein Zusammenhang mit dem Prüfungsanbieter.** Der JLPT wird von der Japan
Foundation und den Japan Educational Exchanges and Services veranstaltet.
Dieser Kurs ist eine private, offen lizenzierte Materialsammlung und steht in
keinem offiziellen Zusammenhang mit den Veranstaltern. Für Anmeldung,
Durchführung und Bewertung des JLPT sind ausschließlich diese zuständig:
<https://www.jlpt.jp/>.
{{< /callout >}}

## Quellen

- *Japanese-Language Proficiency Test* — Wikipedia contributors (Zugriff 2026-09-07), <https://en.wikipedia.org/wiki/Japanese-Language_Proficiency_Test>, Lizenz: CC BY-SA 4.0
- *JLPT — Japanese-Language Proficiency Test*, offizielle Website der Veranstalter (Zugriff 2026-09-07), <https://www.jlpt.jp/e/>
- *JF Standard for Japanese-Language Education* — The Japan Foundation (Zugriff 2026-09-07), <https://jfstandard.jpf.go.jp/en/>
- *Common European Framework of Reference for Languages: Companion Volume* — Council of Europe (2020); im Kurs mittelbar über das Rahmenwerk `boulingua-curriculum` verwendet, siehe dessen `docs/sources.md`.
