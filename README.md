# Zeitkritikalität kommunaler KRITIS

**Eine Business Impact Analyse nach BSI-Standard 200-4, umgebaut für den Bevölkerungsschutz.**

Wenn Strom, Wasser oder Kommunikation in einer Kommune ausfallen, oder wenn Anlagen geräumt werden müssen: Welche Infrastruktur braucht zuerst Aufmerksamkeit? Dieses Verfahren beantwortet das, ohne nach Eintrittswahrscheinlichkeiten zu fragen. Es fragt nur, **wie lange ein Ausfall tragbar ist, bevor der Schaden für die Bevölkerung untragbar wird.**

![Priorisierung nach Zeitkritikalität](abbildungen/priorisierung.png)

## Die Idee

Die Business Impact Analyse (BIA) aus dem BSI-Standard 200-4 findet in Unternehmen die zeitkritischen Prozesse. Ihr Maßstab ist der Fortbestand des Unternehmens: Geld, Recht, Ruf.

Für den Bevölkerungsschutz bleibt die Mechanik gleich, nur der Maßstab ändert sich:

| | Klassische BIA | Diese Variante |
|---|---|---|
| Betrachtet | Geschäftsprozesse eines Unternehmens | KRITIS-Sektoren, -Branchen und -Dienstleistungen einer Kommune |
| Schutzgut | Fortbestand des Unternehmens | Versorgung und Sicherheit der Bevölkerung |
| Schadenskategorien | Finanzen, Recht, Reputation | Leib und Leben, Umwelt, Sachwerte, öffentliche Sicherheit und Ordnung, abhängige KRITIS |
| Ergebnis | Maximal tolerierbare Ausfallzeit je Prozess | Maximal tolerierbare Ausfallzeit (MTPD) je Branche → Reihenfolge |

**Warum nicht die klassische Risikoanalyse?** Für komplexe KRITIS-Ausfälle gibt es keine belastbaren Eintrittswahrscheinlichkeiten. Seltene, schwere Ereignisse würden dadurch kleingerechnet. **Warum nicht FMEA?** Sie braucht Kenntnisse über jede technische Komponente und vergleicht keine Branchen untereinander. **Warum nicht nur GIS?** Räumliche Nähe heißt noch nicht funktionale Abhängigkeit.

## Schritt für Schritt

Das Werkzeug: [`werkzeug/bia_zeitkritikalitaet.xlsx`](werkzeug/bia_zeitkritikalitaet.xlsx), benötigt Excel 2019 oder neuer (Funktion `WENNS`/`IFS`).

### Schritt 1: Schutzgüter festlegen

Welche Folgen eines Ausfalls zählen? Vorgeschlagen sind fünf Schutzgüter, angelehnt an das Kriterium „Qualität“ der BBK-Methode zur Identifizierung kritischer Infrastrukturen:

1. Schutz von Leib und Leben
2. Umwelt
3. Sachwerte
4. Öffentliche Sicherheit und Ordnung
5. Kritische Infrastruktur (Abhängigkeit): Wie stark reißt der Ausfall andere Sektoren mit?

*In der Excel:* Blatt **DB**, Spalte D. Die Namen werden automatisch in die anderen Blätter übernommen.

### Schritt 2: Schadensklassen je Schutzgut beschreiben

Jedes Schutzgut bekommt fünf Stufen von 1 (unbedeutend) bis 5 (sehr hoch), jeweils mit einer konkreten Beschreibung. So bewerten verschiedene Personen nach demselben Maßstab.

![Schutzgüter und Schadensklassen](abbildungen/schutzgueter_schadensklassen.png)

*In der Excel:* Blatt **KRITIS_Schutzgüter**.

### Schritt 3: Untragbarkeitsniveau festlegen

Ab welcher Schadensklasse ist ein Zustand für die Kommune nicht mehr hinnehmbar? Vorgeschlagen ist **Klasse 3** (z. B. Verletzte mit stationärer Behandlung, spürbare Störung der öffentlichen Ordnung).

*In der Excel:* Blatt **DB**, Zelle **D3**. Alle Ausfallzeiten rechnen mit diesem Wert.

### Schritt 4: Zeitschwellen festlegen

Fünf Zeitfenster, die unterschiedliche Grade der Dringlichkeit trennen:

| Zeitfenster | Bedeutung |
|---|---|
| < 30 Minuten | schon kurze Unterbrechungen schaden erheblich |
| < 12 Stunden | Wiederherstellung innerhalb eines Einsatz- oder Tageszyklus nötig |
| < 24 Stunden | |
| < 72 Stunden | |
| < 7 Tage | |

### Schritt 5: Jede Branche bewerten

Für jede Branche und jedes Schutzgut wird gefragt: *Welche Schadensklasse ist erreicht, wenn die Branche so lange ausfällt?* Das sind 5 Schutzgüter × 5 Zeitfenster, also 25 Werte je Zeile. Die Werte steigen meist mit der Zeit.

*In der Excel:* Blatt **KRITIS_Sektoren**, Spalten E bis AC. Die Liste der Sektoren, Branchen und Dienstleistungen ist vorbereitet und kann um eigene Anlagen ergänzt werden. Die Felder färben sich von Grün (1) bis Rot (5). In Spalte AE steht die Begründung, damit die Bewertung nachvollziehbar bleibt.

### Schritt 6: Maximal tolerierbare Ausfallzeit ablesen

Die Excel bestimmt die MTPD je Zeile nach dem **Prinzip der strengsten Restriktion**: Das erste Zeitfenster, in dem *irgendein* Schutzgut das Untragbarkeitsniveau erreicht, ist die tolerierbare Ausfallzeit der ganzen Branche. Bleiben alle Werte darunter, steht dort „Keine MTPD“.

*In der Excel:* Spalte **AD**, wird automatisch berechnet.

### Schritt 7: Priorisieren und vertiefen

Branchen mit der kürzesten MTPD kommen zuerst. Für sie folgt eine Detailanalyse der konkreten Anlagen, zusammen mit Betreibern und der unteren Katastrophenschutzbehörde. Beim Stromnetz sind das zum Beispiel die Spannungsebenen, Umspannwerke, Trafostationen, Redundanzen und alternativen Einspeisungen. Daraus werden Schutzmaßnahmen abgeleitet.

## Beispiel: Energie und Wasser

Diese Bewertungen sind in der Excel bereits eingetragen:

| Branche | MTPD | Begründung (gekürzt) |
|---|---|---|
| Strom: Übertragung/Verteilung | **< 12 h** | BOS-Kommunikation, medizinische Heimgeräte und Intensivstationen müssen bis dahin autark versorgt sein |
| Öffentliche Wasserversorgung | **< 12 h** | |
| Strom: Erzeugung | < 24 h | kurzfristig über das Verbundnetz ausgleichbar, danach sinkt die Netzstabilität |
| Kanalisation | < 24 h | |
| Gas | < 72 h | der Leitungsdruck puffert, danach fällt die Wärmeversorgung aus |
| Fernwärme | < 72 h | die Gebäude kühlen langsam aus, kritisch vor allem in Pflegeeinrichtungen |
| Kläranlagen | < 72 h | |
| Mineralöl | < 7 Tage | große dezentrale Vorräte |

## Grenzen

- Das Verfahren bleibt auf Ebene der Branchen und ersetzt keine Analyse einzelner Anlagen.
- Einheitliche Zeitschwellen und die strengste Restriktion sind bewusst konservativ. Manche Bereiche werden dadurch eher zu hoch eingestuft.
- Redundanzen, lokale Netzstrukturen und Notstrom vor Ort sind nicht berücksichtigt. Sie gehören in Schritt 7.
- Die Bewertungen sind Einschätzungen. Am besten bewerten mehrere Personen unabhängig voneinander und gleichen die Ergebnisse ab.

## Herkunft

Entstanden in der Hausarbeit „Zeitkritikalität bei der Evakuierung kritischer Anlagen“ im Modul Kritische Infrastrukturen und Bevölkerungsschutz (Prof. Dr. Alexander Fekete), Institut für Rettungsingenieurwesen und Gefahrenabwehr, TH Köln, 2026. Schutzgüter und Excel-Werkzeug sind gemeinsam mit Hendrik Platz entwickelt. Mehr zur Arbeit: [Steckbrief in RIW-Projekte](https://github.com/2708Tom/RIW-Projekte/tree/main/2026-07_KRITIS_Zeitkritikalitaet-Evakuierung).

Grundlagen: BSI-Standard 200-4 Business Continuity Management (2023); BBK: Schutz Kritischer Infrastrukturen – Identifizierung in sieben Schritten (2019).
