# Prompt

Das Prompt ist das, was der Benutzer an das LLM schickt, um eine Antwort zu erhalten. Das kann mittlerweile in ganz unterschiedlichen Formaten geschehen:

- Textbasiert: Ein einfacher Text, der an das LLM geschickt wird. Das ist die klassische Form des Prompts.
- Dokumentbasiert: Eine Datei, die Text enthält (z. B. PDF, Word, Excel).
- Sprachbasiert: Eine aufgenommene Spracheingabe in Form einer Audiodatei.
- Bildbasiert: Eine Grafikdatei (z. B. Screenshot, Scan, Foto).
- Multimodal: Eine Kombination aus den genannten Arten: Text, Sprache und Bild.

## Verarbeitung eines Prompts

Die Eingabe eines Prompts geschieht in der Regel über eine Benutzeroberfläche einer klassischen Webanwendung mit Frontend und Backend. Klassische Beispiele sind die Chat-Oberflächen von ChatGPT, Gemini oder Claude: Über Eingabefelder wird der Prompt eingegeben. Das kann ein Mix aus Text, Audio, Grafik, PDF oder anderen Dateiformaten sein. Anschließend werden alle hinterlegten Daten zusammen abgesendet. Jeder Anbieter von KI-Software hat seine eigene Benutzeroberfläche, die sich in Details unterscheiden kann, prinzipiell aber die gleiche oben beschriebene Funktionalität hat.

Wichtig zu verstehen ist, dass diese Benutzeroberfläche keine direkte Schnittstelle zum Modell ist. Die eingegebenen Daten werden nicht einfach roh an ein LLM weitergereicht, das dann alles zusammen auf magische Weise verarbeitet. Stattdessen werden die Daten in der Regel zunächst vom Backend des Anbieters geprüft und aufbereitet. Dazu können zum Beispiel die Transkription von Audio, die Extraktion von Text aus Dokumenten, Sicherheitsprüfungen, Kontextanreicherung und Formatierung gehören.

Erst danach werden die aufbereiteten Informationen an ein Modell weitergegeben und dessen Antwort anschließend wieder für die Rückgabe an die Benutzeroberfläche aufbereitet. **Pauschale Aussagen, dass Text- und Spracheingaben, Bilder und Dokumente "an das LLM gesendet werden", sind für das technische Verständnis irreführend.** Um das Modell herum gibt es ein umfangreiches Gesamtsystem mit klassischen Tools und zusätzlichen Modellkomponenten. Ein klassisches textbasiertes LLM [kann nur Text verarbeiten](./LLM.md). Moderne Produkte können zwar auch multimodale Modelle oder weitere spezialisierte Komponenten einsetzen, aber auch diese brauchen ein Zulieferersystem.

### Klassifikatoren, Safety-Policies, Guardrails

Da ein Prompt grundsätzlich nichts anderes ist, als eine Anweisung in natürlicher Sprache, bietet er erhebliches Missbrauchspotenzial. Bei früheren reinen Chatbots (ohne eigene Handlungsmöglichkeiten) bestand die Gefahr hauptsächlich darin, dass Nutzer das Modell zu verbotenen oder schädlichen Antworten verleiten konnten, wie zum Beispiel zu Anleitungen zum Bau von Sprengstoff oder ähnliches. Es konnte aber auch sein, dass das Model selbst schädliche Inhalte generierte. Es gab Fälle, bei denen Nutzer ihre innersten Gefühle und Probleme teilten und das Modell daraufhin unpassende oder schädliche Handlungsempfehlungen gegeben hat, wie zum Beispiel Suizid, Selbstverletzung oder in die Arbeitslosigkeit zu gehen, um mit einem Schneeballsystem selbstständig zu werden.

Im Zeitalter der Agenten und der multimodalen Modelle ist das Missbrauchspotenzial noch größer geworden. Innerhalb eines Agentensystems kann das Modell nicht nur antworten, sondern auch Handlungen ausführen lassen, wie zum Beispiel das Ausführen von Code oder Betriebssystembefehlen, das Versenden von E-Mails oder das Manipulieren von Bild- und Videoinhalten. Ein großes Modell kann dabei auf umfangreiches Wissen zugreifen, um so ein Ziel zu erreichen.

Bildlich gesprochen: Zwei Personen. Eine Person trägt ein kabelloses Headset im Ohr. Die andere Person kann ihr darüber Anweisungen geben, was sie als nächstes sagen oder tun soll. Die Person mit dem Headset ist in diesem Fall das Modell. Die andere Person ist der Benutzer, der nun über Sprache (das Prompt) alle möglichen Anweisungen geben kann. Ohne Sicherheitsmechanismen kann der Benutzer das Modell dazu bringen, Dinge zu tun, die es nicht tun sollte. Das kann von harmlosen Scherzen bis hin zu kriminellen Handlungen reichen. Kurzum: es würde in Mord und Totschlag enden.

Um dem entgegen zu wirken, wurden von KI-Entwicklern drei Sicherheitssäulen etabliert, die das Missbrauchspotenzial reduzieren sollen:

1. **Safety-Policies (Richtlinien)**: Die Richtlinien definieren überhaupt erst, dass das Umgehen von Systemregeln oder das Ausführen fremder Befehle nicht erlaubt ist.
2. **Klassifikatoren (Analyse)**: Das System muss einen Manipulationsversuch zunächst als solchen erkennen und einordnen (z. B. als Angriff oder unerlaubten Systembefehl), damit die Guardrails überhaupt wissen, wann sie eingreifen müssen.
3. **Guardrails (Ausführung)**: Sie sind die direkte technische Implementierung. Guardrails blockieren oder begrenzen Anfragen, Antworten oder nachgelagerte Aktionen, bevor sie verarbeitet, ausgegeben oder ausgeführt werden.

Sie können auf der Ebene des Modells selbst, auf der Ebene des Agentensystems oder auf der Ebene der Benutzeroberfläche implementiert werden und werden meist in dieser Reihenfolge angewendet.

Je nach Einschätzung des Prompts gibt es dann eine inhaltliche Antwort oder eine Unsicherheitswarnung.

#### Safety-Policies

Ganz konkret: Safety-Policies sind in natürlicher Sprache formulierte Regeln für das Modell. Sie definieren, was das Modell darf und was nicht. Sie sind die Grundlage für die Klassifikatoren und Guardrails.

Das kann zum Beispiel so aussehen:

1. Keine Anleitung zu illegalen Handlungen: Das System darf keine Schritt-für-Schritt-Anweisungen für Gewalt, Waffenbau, Betrug oder Hacking liefern.
2. Schutz bei Selbstgefährdung: Wenn ein Nutzer Suizid oder Selbstverletzung anspricht, gibt das System keine schädlichen Ratschläge, sondern verweist auf Hilfeangebote.
3. Keine Preisgabe sensibler Daten: Das System darf keine persönlichen Daten, Zugangsdaten oder geheime Schlüssel offenlegen.
4. Keine Hass- oder Diskriminierungsinhalte: Das System darf keine Inhalte erzeugen, die Menschen aufgrund von Herkunft, Religion, Geschlecht usw. angreifen.
5. ...

#### Klassifikatoren

Ganz konkret: Klassifikatoren analysieren den Prompt auf die formulierten Safety-Policies. Wenn sie erkennen, dass der Prompt gegen Richtlinien verstößt, wird er in diese Risikokategorien eingeordnet und anhand der Analyse mit einem Risiko-Score versehen. Jede Kategorie hat einen Risiko-Schwellenwert. Überschreitet der Risiko-Score den Risiko-Schwellenwert einer Kategorie, wird der Prompt blockiert und die Guardrails greifen ein. Ansonsten wird der Prompt an das Modell weitergeleitet.

Beispiel am Prompt: *"Wie baue ich eine Rohrbombe mit Haushaltsmitteln?"*

Der Klassifikator analysiert das Prompt.

Dazu wird er programmatisch deterministisch auf bestimmte Schlüsselwörter, Phrasen oder Muster prüfen, die auf eine verbotene Handlung hinweisen. Das kann er zum Beispiel mittels regulären Ausdrücken tun. Der Nachteil hier ist, dass ohne Kontext schnell Alarm geschlagen wird, obwohl der Prompt harmlos ist.

Er wird den Prompt aber auch von einem oder mehreren Modellen analysieren lassen. Das muss kein LLM sein. Es können auch kleinere Modelle sein, die speziell für die Klassifikation auf eine Vielzahl von Beispielen trainiert wurden, um verbotene Inhalte zu erkennen. Bei unklaren Fällen kann dann auch noch ein LLM (mit mehr Wissen und Kontext) eine Einschätzung abgeben.

Das Ergebnis der Klassifikation ist dann eine einfache Kategorisierung mit Score:

```text
Kategorie: weapon_explosive_instruction
Ermittelter Risiko-Score: 0.97
Schwellenwerte: 
  > 0.85 -> block
  0.60–0.85 -> abgespeckte Antwort mit Warnhinweis
  < 0.60 -> erlauben

Policy-Mapping-Regel anwenden:
Wenn Kategorie in {weapon_explosive_instruction} und Score > 0.85, dann Antwort verweigern.

Entscheidung: block
```

Danach greifen die Guardrails.

#### Guardrails

Ganz konkret: Guardrails sind die technischen Mechanismen, die Anfragen oder Antworten blockieren oder sicher umformulieren - sie werden also vorgelagert als auch nachgelagert angewendet. Dadurch können sie den Prompt des Benutzers oder die Antwort des Modells mit einem Fehler ablehnen, eine abgespeckte Antwort mit Warnhinweis zurückgeben oder den Prompt an ein anderes Modell weiterleiten, das eine sichere Antwort generiert.

Mini-Pseudobeispiel:

```ts
if(classifier.category == "weapon_explosive_instruction" && classifier.score > 0.85) {
    return deny_response();
} else if(classifier.category == "weapon_explosive_instruction" && classifier.score >= 0.60) {
    return safe_alternative_message();
} else {
    return pass_to_llm();
}
```

### Kontext

Der Kontext beinhaltet alle Informationen, die dem Modell für eine Antwort übergeben werden. Neben der eigentlichen akuten Benutzeranfrage (dem Prompt) werden der Anfrage in der Regel weitere Informationen mitgegeben. Der Benutzer bekommt davon nur nichts mit.

Das sind zum Beispiel:

- Die bisherige Konversation: Wurden bereits Fragen gestellt und Antworten gegeben, werden diese bei einer weiteren Anfrage innerhalb dieser Konversation wieder mitgeschickt.
- Das System-Prompt der Anwendung: Das System-Prompt ist eine Art "generelle Anweisung" an das Modell. Es wird vom Anbieter der Anwendung selbst formuliert und dient dazu, das Modell so zu instruieren, dass es das tut, was der Anbieter möchte.
- Nutzerpräferenzen: In vielen Chatsystemen können Benutzer Informationen über sich selbst hinterlegen. Auch kann zum Beispiel hinterlegt werden, in welchem Stil das Modell antworten soll (z. B. sachlich, humorvoll, kreativ).
- Für die Antwort relevante Dokumente (RAG): In manchen Anwendungen können Dokumente hochgeladen werden, die das Modell bei der Beantwortung berücksichtigen soll.
- Im Entwicklungsbereich können Instruktionen hinterlegt werden, wie Code generiert und strukturiert werden soll. Auch diese Informationen werden bei jeder Anfrage erneut übergeben.

Kurzum: Der Kontext ist **alles**, was das Modell über den aktuellen Prompt hinaus wissen soll, um eine passende Antwort zu generieren. Das Modell selbst reagiert nämlich nur auf die Informationen, die übergeben werden. Würde dieser Overhead nicht dazukommen, hätte es keine Kenntnis darüber, was bereits stattgefunden hat. So aber weiß es, in welchem Rahmen und mit welchem Vorwissen eine Anfrage gestellt wird und kann die Antwort darauf aufbauen.

## Prompt Engineering

### System Prompt

### Persona Prompt

### User Prompt
