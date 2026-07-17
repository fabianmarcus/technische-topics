# Question & Answer (QA)

**Frage-Antwort-Runden mit Cope zum Thema KI.**

## Inhaltsverzeichnis

- [Wenn ich einem Modell die Aufgabe gebe, mir 3 Eigenschaften einer Tür zu nennen. Wie bestimmt es, dass es 3 Eigenschaften genannt hat und nicht zwei oder 4?](#wenn-ich-einem-modell-die-aufgabe-gebe-mir-3-eigenschaften-einer-tür-zu-nennen-wie-bestimmt-es-dass-es-3-eigenschaften-genannt-hat-und-nicht-zwei-oder-4)
- [Sammeln die großen LLM-Anbieter die Trainingsdaten selbst oder arbeiten sie mit den selben Trainingsdaten von einem Zulieferer?](#sammeln-die-großen-llm-anbieter-die-trainingsdaten-selbst-oder-arbeiten-sie-mit-den-selben-trainingsdaten-von-einem-zulieferer)

## Inhalt

### Wenn ich einem Modell die Aufgabe gebe, mir 3 Eigenschaften einer Tür zu nennen. Wie bestimmt es, dass es 3 Eigenschaften genannt hat und nicht zwei oder 4?

```text
Gute Frage. Kurz: Es „zählt“ nicht wie ein Programm mit sicherer Schleife, sondern folgt gelernten Mustern plus deinem Prompt.

Was passiert praktisch:

1. Dein Prompt setzt ein Muster  
`Nenne 3 Eigenschaften ...` aktiviert im Modell typische Antwortmuster wie nummerierte Listen mit drei Punkten.

2. Token-für-Token-Generierung  
Bei jedem Schritt wählt das Modell das nächste wahrscheinliche Token. Nach „1.“ ist „2.“ wahrscheinlich, nach „2.“ ist „3.“ wahrscheinlich, danach oft ein Abschluss statt „4.“.

3. Kein harter Zähler garantiert die 3  
Das Modell hat keine zwingende Kontrolllogik „stoppe exakt nach 3 Items“, außer wenn eine äußere Regel dazukommt.

Warum es trotzdem oft klappt:
- Solche Anweisungen sind im Training häufig vorgekommen.
- Listenformate sind stark gelernt.
- Gute Prompt-Formulierung erhöht die Treue.

Warum es manchmal trotzdem 2 oder 4 werden:
- Sampling/Temperatur kann das Muster aufweichen.
- Unklarer Prompt.
- Modell driftet im Verlauf.

Wie du es robuster machst:
- „Antworte mit **genau** 3 Stichpunkten.“
- „Wenn es mehr gäbe, nenne trotzdem nur 3.“
- Optional Formatvorgabe: JSON mit genau 3 Array-Elementen.
- Danach optional per nachgelagerter Prüfung validieren (bei kritischen Workflows).
```

### Sammeln die großen LLM-Anbieter die Trainingsdaten selbst oder arbeiten sie mit den selben Trainingsdaten von einem Zulieferer?

```text
Beides, aber überwiegend sammeln und kuratieren sie selbst.

Kurzbild aus der Praxis:

1. Eigene Datensammlungen  
Große Anbieter bauen eigene Pipelines für Web-Crawls, Lizenzdaten, Code, Dokumentation und proprietäre Quellen auf.

2. Lizenzierte Daten von Dritten  
Zusätzlich kaufen oder lizenzieren sie Datensätze, z. B. aus Verlagen, Bild-/Textarchiven, Spezialdomänen.

3. Öffentliche Standardquellen überschneiden sich  
Viele nutzen ähnliche öffentlich verfügbare Quellen. Daher gibt es Überschneidungen zwischen Modellen.

4. Nicht ein gemeinsamer „Zulieferer-Standarddatensatz“  
Es gibt keinen einzigen zentralen Lieferanten, von dem alle dasselbe Komplett-Set beziehen.

5. Unterschied entsteht durch Kuratierung  
Starke Unterschiede kommen durch Filterung, Deduplikation, Qualitätsregeln, Sicherheitsfilter, Mischung der Quellen und Trainingstaktik.

Also: Es gibt Überschneidungen bei öffentlichen Daten, aber kein einheitliches gemeinsames Trainingspaket für alle großen Anbieter.
```
