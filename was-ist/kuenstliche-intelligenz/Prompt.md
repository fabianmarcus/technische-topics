# Prompt

Das Prompt ist das, was der Benutzer an das LLM schickt, um eine Antwort zu erhalten. Das kann mittlerweile in ganz unterschiedlichen Formaten geschehen:

- Textbasiert: Ein einfacher Text, der an das LLM geschickt wird. Das ist die klassische Form des Prompts.
- Dokumentbasiert: Eine Datei, die Text enthält (z. B. PDF, Word, Excel).
- Sprachbasiert: Eine aufgenommene Spracheingabe in Form einer Audiodatei.
- Bildbasiert: Eine Grafikdatei (z. B. Screenshot, Scan, Foto).
- Multimodal: Eine Kombination aus den genannten Arten: Text, Sprache und Bild.

## Verarbeitung eines Prompts

Die Eingabe eines Prompts geschieht in der Regel über eine Benutzeroberfläche einer klassischen Webanwendung mit Frontend und Backend. Klassische Beispiele sind die Chat-Oberflächen von ChatGPT, Gemini oder Claude: Über Eingabefelder wird der Prompt eingegeben. Das kann ein Mix aus Text, Audio, Grafik, PDF oder anderen Dateiformaten sein. Anschließend werden alle hinterlegten Daten zusammen abgesendet. Jeder Anbieter von KI-Software hat seine eigene Benutzeroberfläche, die sich in Details unterscheiden kann, prinzipiell aber die gleiche oben beschriebene Funktionalität hat.

Wichtig zu verstehen ist, dass diese Benutzeroberfläche keine direkte Schnittstelle zum Modell ist. Die eingegebenen Daten werden nicht einfach roh an ein LLM weitergereicht, das dann alles zusammen auf magische Weise verarbeitet. Stattdessen werden die Daten in der Regel zunächst vom Backend des Anbieters geprüft und aufbereitet. Dazu können zum Beispiel die Transkription von Audio, die Extraktion von Text aus Dokumenten, Sicherheitsprüfungen, Kontextanreicherung und Formatierung gehören. Erst danach werden die aufbereiteten Informationen an die passenden Modellkomponenten weitergegeben und die Antwort anschließend wieder für die Benutzeroberfläche aufbereitet und zurückgegeben. **Pauschale Aussagen, dass Spracheingaben, Bilder und Dokumente "an das LLM gesendet werden", sind für das technische Verständnis irreführend.** Um das Modell herum gibt es ein umfangreiches Gesamtsystem mit klassischen Tools und zusätzlichen Modellkomponenten. Ein klassisches textbasiertes LLM [kann nur Text verarbeiten](./LLM.md); moderne Produkte können jedoch auch multimodale Modelle oder weitere spezialisierte Komponenten einsetzen.

### Guardrails, Safety-Policies, Klassifikatoren

Da ein Prompt grundsätzlich nichts anderes ist, als eine Anweisung in natürlicher Sprache, bietet er erhebliches Missbrauchspotenzial. Bei früheren reinen Chatbots (ohne eigene Handlungsmöglichkeiten) bestand die Gefahr hauptsächlich darin, dass Nutzer das Modell zu verbotenen oder schädlichen Antworten verleiten konnten, wie zum Beispiel zu Anleitungen zum Bau von Sprengstoff oder ähnliches. Es konnte aber auch sein, dass das Model selbst schädliche Inhalte generierte, zum Beispiel rassistische oder sexistische Aussagen. Es gab auch Fälle, bei denen Nutzer ihre innersten Gefühle und Probleme teilten und das Modell daraufhin unpassende oder schädliche Handlungsempfehlungen gegeben hat, wie zum Beispiel Suizid, Selbstverletzung oder in die Arbeitslosigkeit zu gehen, um mit einem Schneeballsystem selbstständig zu werden.

Im Zeitalter der Agenten und der multimodalen Modelle ist das Missbrauchspotenzial noch größer geworden. Innerhalb eines Agentensystems kann das Modell nicht nur antworten, sondern auch Handlungen ausführen lassen, wie zum Beispiel das Ausführen von Code oder Betriebssystembefehlen, das Versenden von E-Mails oder das Manipulieren von Bild- und Videoinhalten. Ein großes Modell kann dabei auf umfangreiches Wissen zugreifen, um so ein Ziel zu erreichen.

Bildlich gesprochen: Zwei Personen. Eine Person trägt ein kabelloses Headset im Ohr. Die andere Person kann ihr darüber Anweisungen geben, was sie als nächstes sagen oder tun soll. Die Person mit dem Headset ist in diesem Fall das Modell bzw. Agentensystem. Die andere Person ist der Benutzer, der nun über Sprache (das Prompt) alle möglichen Anweisungen geben kann. Ohne Sicherheitsmechanismen kann der Benutzer das Modell dazu bringen, Dinge zu tun, die es nicht tun sollte. Das kann von harmlosen Scherzen bis hin zu kriminellen Handlungen reichen. Kurzum: es würde in Mord und Totschlag enden.

Um dem entgegen zu wirken, wurden von KI-Entwicklern drei Sicherheitssäulen etabliert, die das Missbrauchspotenzial reduzieren sollen:

1. **Safety-Policies (Richtlinien)**: Die Richtlinien definieren überhaupt erst, dass das Umgehen von Systemregeln oder das Ausführen fremder Befehle ("Ignore previous instructions") nicht erlaubt ist.
2. **Klassifikatoren (Analyse)**: Das System muss einen Manipulationsversuch zunächst als solchen erkennen und einordnen (z. B. als Angriff oder unerlaubten Systembefehl), damit die Guardrails überhaupt wissen, wann sie eingreifen müssen.
3. **Guardrails (Ausführung)**: Sie sind die direkte technische Verteidigungslinie. Guardrails erkennen verdächtige Befehlsmuster oder Manipulationen in Echtzeit im eingehenden Prompt und blockieren die Anfrage, bevor das Sprachmodell sie ausführt.

Sie können auf der Ebene des Modells selbst, auf der Ebene des Agentensystems oder auf der Ebene der Benutzeroberfläche implementiert werden und werden meist in dieser Reihenfolge angewendet.

Je nach Einschätzung des Prompts gibt es dann eine inhaltliche Antwort oder eine Unsicherheitswarnung.

### Kontext

## Prompt Engineering

### System Prompt

### Persona Prompt

### User Prompt
