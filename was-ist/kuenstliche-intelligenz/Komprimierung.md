# Modellkomprimierung

Wie in [LLM.md](./LLM.md) beschrieben, wird in Large Language Models nicht wirklich mit Texten gearbeitet. Texte werden in Tokens zerstückelt. Jedes Token bekommt eine eindeutige Zahl zugewiesen. Aus dieser Zahl wird ein Vektor (Embedding) berechnet.

In einem ursprünglich trainierten Large Language Model sieht ein Vektor dann ungefähr so aus: `[0.123456789, -0.345678912, 0.567891234, ...]`. Jede Zahl in diesem Vektor beschreibt eine Koordinate auf einer Dimension des mehrdimensionalen Vektorraums. Die hier angegebenen Zahlen sind recht präzise. Die Präzision der Zahlen des Vektors bestimmt, wie fein die Koordinate auf der Dimension beschrieben werden kann; sprich, es lässt sich genauer bestimmen, wie ähnlich sich zwei Konzepte (Worte) im Vektorraum sind.

Ein veranschaulichendes analoges Beispiel sind GPS-Koordinaten mit Längen- und Breitengrad, bei denen die Präzision der Zahlen über die Genauigkeit der Position auf Google Maps bestimmt. Eine Angabe mit weniger Nachkommastellen würde die Position ungenauer machen - Google Maps könnte die Position nur noch ungefähr mit einem größeren Kreis darstellen. Bei sehr vielen Nachkommastellen kann die Position möglicherweise mit einem kleinen Punkt vor der Haustür abgebildet werden.

Da präzisere Zahlen in großer Menge wesentlich mehr Speicherplatz benötigen, kann ein ausgiebig trainiertes Large Language Modell jedoch sehr groß werden - zum Teil mehrere Hundert Gigabyte. Da das insbesondere im lokalen Bereich schwer handhabbar ist, werden Verfahren wie *Downcasting* oder *Quantisierung* eingesetzt, um die Modellgröße zu reduzieren.

## Downcasting

Downcasting reduziert die Präzision der Modellparameter durch Rundung der Fließkommazahlen, um Speicher und Rechenleistung zu sparen.

Downcasting bedeutet vereinfacht gesagt, die Nachkommastellen der Zahlen in den Vektoren zu reduzieren. Das macht die genaue Koordinate eines Konzeptes (z. B. "Hund", "Katze", usw.) im Vektorraum etwas ungenauer, spart dafür aber in Masse ziemlich viel Speicherbedarf. Downcasting bewirkt demnach nicht, dass das Modell weniger weiß, sondern dass es sein Wissen semantisch schlechter in Relation setzen kann (es ist sich dann weniger sicher, ob Hund und Katze tatsächlich zwei Haustiere sind). Ein Downcasting von 16 Bit auf 8 Bit behält aber wohl einen Großteil der ursprünglichen Genauigkeit bei.

Kurzum: Downcasting = Wechsel von einer hohen Fließkommapräzision zu einer niedrigeren Fließkommapräzision (z. B. FP32 zu FP16 / BF16).

## Quantisierung

Quantisierung reduziert nicht nur die Nachkommastellen der Fließkommazahlen wie beim Downcasting, sondern wandelt sie durch Multiplikation mit einer geeigneten Skalierungszahl in Ganzzahlen (Integer) ohne Nachkommastellen um - z. B. INT8 oder INT4. Das reduziert nicht nur den Speicherbedarf, sondern beschleunigt auch die Berechnungen. Ganzzahlen können von passender Hardware wesentlich effizienter verarbeitet werden als Fließkommazahlen.

Vereinfacht sieht das so aus: Aus einem ursprünglichen Wert wie `0.13742` wird nicht einfach nur `0.137`, sondern z. B. ein Integer-Wert wie `35`. Die dafür verwendete Skalierung merkt sich das Modell, sodass die Fließkommazahlen bei Bedarf (zumindest näherungsweise) wieder rekonstruiert werden können. In der Praxis sind viele Verfahren gemischt: Gewichte liegen etwa als INT8 oder INT4 vor, während laufende Berechnungen weiterhin in FP16, BF16 oder teilweise FP32 verarbeitet werden.

Kurzum: Quantisierung = Wechsel von Fließkommazahlen zu Ganzzahlen mit niedriger Bit-Breite (z. B. FP16 zu INT8 / INT4).

## Zahlen & Fakten

### Vereinfachtes Beispiel

```text
Originalwert: 0.13742
Downcasting: 0.137 als FP16-Wert
Quantisierte Speicherung: 35 als INT8-Wert
Rekonstruierter Näherungswert: ungefähr 0.137
```

### Grundlegend zur Einordnung (von Gemini)

| Format | Bits pro Zahl | Speicher pro Zahl |
| --- | ---: | ---: |
| FP32 | 32 Bit | 4 Byte |
| FP16 / BF16 | 16 Bit | 2 Byte |
| INT8 | 8 Bit | 1 Byte |
| INT4 | 4 Bit | 0,5 Byte |

*INT = Integer, Ganzzahlen*
*FP = Floating Point, Fließkommazahlen*

### Typische Modellgrößen und deren Speicherbedarf (von Gemini)

| Modellgröße | FP32 | FP16 / BF16 | INT8 | INT4 |
| --- | ---: | ---: | ---: | ---: |
| 7B | ca. 28 GB | ca. 14 GB | ca. 7 GB | ca. 3,5 GB |
| 13B | ca. 52 GB | ca. 26 GB | ca. 13 GB | ca. 6,5 GB |
| 30B | ca. 120 GB | ca. 60 GB | ca. 30 GB | ca. 15 GB |
| 70B | ca. 280 GB | ca. 140 GB | ca. 70 GB | ca. 35 GB |
| 175B | ca. 700 GB | ca. 350 GB | ca. 175 GB | ca. 87,5 GB |

*B = Billion (engl.) bzw. Milliarde (dt.) Parameter.*

### Eine hilfreiche Faustregel (von Gemini)

- Kleines Cloud-Modell, z. B. 7B bis 13B: ca. 10-30 GB reine Gewichte, produktiv eher deutlich mehr.
- Mittleres Modell, z. B. 30B bis 70B: ca. 60-140 GB reine FP16-Gewichte, produktiv eher hunderte GB.
- Großes GPT-3-Klasse-Modell, z. B. 175B: ca. 350 GB reine FP16-Gewichte, produktiv über Replikate und Caches schnell im TB-Bereich.
- Moderne Spitzenmodelle: unbekannt, oft nicht öffentlich. Bei Mixture-of-Experts-Modellen kann die Gesamtgröße sehr groß sein, aber pro Anfrage wird nur ein Teil der Experten aktiv genutzt.
