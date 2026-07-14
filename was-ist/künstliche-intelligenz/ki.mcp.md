# MCP

MCP steht für Model Context Protocol.

MCP ist das HTTP für KI-Modelle (LLMs). Es standardisiert die Kommunikation zwischen LLMs und Agenten, die diese Modelle nutzen. Es definiert ein Protokoll, das es Agenten ermöglicht, mit verschiedenen LLMs zu interagieren, ohne sich um die spezifischen Implementierungen der einzelnen Modelle kümmern zu müssen.

Das Protokoll definiert standardisierte Endpunkte und Datenformate für die Kommunikation zwischen Agenten und LLMs. Ähnlich zu HTTP, das standardisierte Methoden (GET, POST, PUT, DELETE) und Statuscodes definiert, definiert MCP standardisierte Anfragen und Antworten für die Interaktion mit LLMs.

## Tool Calling

Tool Calling ist ein Konzept, das LLMs ermöglicht, den zuständigen Agenten anzuweisen, für ihre Antwort Informationen aus externen Tools oder APIs abzurufen und an das LLM zurückzugeben. Das LLM kann so auf aktuellere Daten zugreifen, die es selbst noch nicht kennt.

Der Agent teilt dem LLM am Anfang der Interaktion mit, welche Tools oder APIs er aufrufen kann, und das LLM kann dann entscheiden, welche Tools es für die Beantwortung der Anfrage benötigt. Das LLM kann auch mehrere Tools in einer Anfrage kombinieren, um eine umfassendere Antwort zu generieren.

Bittet das LLM den Agenten, ein Tool aufzurufen, ruft der Agent die für das Tool hinterlegte Funktion mit den erforderlichen Parametern auf, die es vom LLM erhalten hat. Das Ergebnis geht für die weitere Verarbeitung zurück ans LLM.

Beispielsweise kann ein LLM eine Anfrage erhalten, um das Wetter in einer bestimmten Stadt zu überprüfen. Das LLM kennt die Antwort aufgrund der Aktualität des Wetters nicht, da das Modell auf älteren Trainingsdaten basiert. Stattdessen kann das LLM den zuständigen Agenten anweisen, eine Wetter-API aufzurufen und die aktuellen Wetterdaten abzurufen. Der Agent ruft die API auf, erhält die Daten und sendet sie zurück an das LLM, das die Daten in seine Textantwort einfügt und dem Benutzer präsentiert.

Die vom Agenten aufgerufene Funktion kann ein schlanker Adapter für einen API-, DB- oder SDK-Call sein. Bei kleinen Tools kann sie aber auch die konkrete Implementierung einer direkten Logik beinhalten - z.B. Berechnungen, Mappings oder Formatierungen. Zu komplex sollte die Funktion aber nicht werden. Best practice ist, sie schlank zu halten und komplexe Logik in einen Dienst auszulagern.
