# MCP

MCP steht für Model Context Protocol.

MCP ist das HTTP für KI-Modelle (LLMs). Es standardisiert die Kommunikation zwischen LLMs und Agenten, die diese Modelle nutzen. Es definiert ein Protokoll, das es Agenten ermöglicht, mit verschiedenen LLMs zu interagieren, ohne sich um die spezifischen Implementierungen der einzelnen Modelle kümmern zu müssen.

Das Protokoll definiert standardisierte Endpunkte und Datenformate für die Kommunikation zwischen Agenten und LLMs. Ähnlich zu HTTP, das standardisierte Methoden (GET, POST, PUT, DELETE) und Statuscodes definiert, definiert MCP standardisierte Anfragen und Antworten für die Interaktion mit LLMs.

## Tool Calling

Tool Calling ist ein Konzept, das LLMs ermöglicht, den zuständigen Agenten anzuweisen, für ihre Antwort Informationen aus externen Tools oder APIs abzurufen und an das LLM zurückzugeben. Das LLM kann so auf aktuellere Daten zugreifen, die es selbst noch nicht kennt.

Der Agent teilt dem LLM am Anfang der Interaktion mit, welche Tools oder APIs er aufrufen kann, und das LLM kann dann entscheiden, welche Tools es für die Beantwortung der Anfrage benötigt. Das LLM kann auch mehrere Tools in einer Anfrage kombinieren, um eine umfassendere Antwort zu generieren.

Bittet das LLM den Agenten, ein Tool aufzurufen, ruft der Agent die für das Tool hinterlegte Funktion mit den erforderlichen Parametern auf, die er vom LLM erhalten hat. Das Ergebnis geht für die weitere Verarbeitung zurück ans LLM.

Beispielsweise kann ein LLM eine Anfrage erhalten, um das Wetter in einer bestimmten Stadt zu überprüfen. Das LLM kennt die Antwort aufgrund der Aktualität des Wetters nicht, da das Modell auf älteren Trainingsdaten basiert. Stattdessen kann das LLM den zuständigen Agenten anweisen, eine Wetter-API aufzurufen und die aktuellen Wetterdaten abzurufen. Der Agent ruft die API auf, erhält die Daten und sendet sie zurück an das LLM, das die Daten in seine Textantwort einfügt und dem Benutzer präsentiert.

Die vom Agenten aufgerufene Funktion kann ein schlanker Adapter für einen API-, DB- oder SDK-Call sein. Bei kleinen Tools kann sie aber auch die konkrete Implementierung einer direkten Logik beinhalten - z.B. Berechnungen, Mappings oder Formatierungen. Zu komplex sollte die Funktion aber nicht werden. Best practice ist, sie schlank zu halten und komplexe Logik in einen Dienst auszulagern.

### Praktisches Beispiel

Low Level Code Beispiel, um einen wirklichen Eindruck davon zu bekommen, wie Tool Calling in der Praxis funktioniert. Das Beispiel nutzt Gemini, die anderen Anbieter funktionieren analog.

```ts
import { GoogleGenAI } from "@google/genai";

const ai = new GoogleGenAI({ apiKey: process.env.GEMINI_API_KEY! });

const functionDeclarations = [
  {
    name: "get_weather",
    description: "Liefert Wetterdaten fuer eine Stadt",
    parameters: {
      type: "OBJECT",
      properties: {
        city: { type: "STRING", description: "Name der Stadt" }
      },
      required: ["city"]
    }
  },
  {
    name: "get_air_quality",
    description: "Liefert Luftqualitaetsdaten fuer eine Stadt",
    parameters: {
      type: "OBJECT",
      properties: {
        city: { type: "STRING", description: "Name der Stadt" }
      },
      required: ["city"]
    }
  }
];

type FunctionCall = { name: string; args?: Record<string, unknown> };

async function executeTool(name: string, args: Record<string, unknown> = {}) {
  if (name === "get_weather") {
    return { city: args.city, tempC: 24, condition: "sonnig" };
  }
  if (name === "get_air_quality") {
    return { city: args.city, aqi: 42, level: "gut" };
  }
  throw new Error(`Unknown tool: ${name}`);
}

// SDK-kompatibles Auslesen von Function Calls (je nach Version unterschiedlich)
function extractFunctionCalls(response: any): FunctionCall[] {
  if (Array.isArray(response.functionCalls) && response.functionCalls.length > 0) {
    return response.functionCalls;
  }

  const parts = response?.candidates?.[0]?.content?.parts ?? [];
  const calls = parts
    .filter((p: any) => p.functionCall)
    .map((p: any) => p.functionCall);

  return calls;
}

export async function runGeminiToolLoop(userPrompt: string) {
  const contents: any[] = [
    { role: "user", parts: [{ text: userPrompt }] }
  ];

  for (let step = 0; step < 10; step++) {
    const response = await ai.models.generateContent({
      model: "gemini-1.5-pro",
      contents,
      config: {
        tools: [{ functionDeclarations }]
      }
    });

    const functionCalls = extractFunctionCalls(response);

    // Keine Tool-Anforderung mehr -> finale Antwort
    if (functionCalls.length === 0) {
      const finalText =
        response.text ??
        response?.candidates?.[0]?.content?.parts
          ?.map((p: any) => p.text)
          ?.filter(Boolean)
          ?.join("\n") ??
        "";
      return finalText;
    }

    // Modellnachricht + Tool-Antworten in den Verlauf eintragen
    for (const call of functionCalls) {
      const args = (call.args ?? {}) as Record<string, unknown>;
      const toolResult = await executeTool(call.name, args);

      contents.push({
        role: "model",
        parts: [{ functionCall: { name: call.name, args } }]
      });

      contents.push({
        role: "user",
        parts: [
          {
            functionResponse: {
              name: call.name,
              response: toolResult
            }
          }
        ]
      });
    }
  }

  throw new Error("Maximale Anzahl an Tool-Iterationen erreicht.");
}

// Beispielaufruf
runGeminiToolLoop(
  "Gib mir Wetter und Luftqualitaet fuer Berlin und sage, ob Joggen sinnvoll ist."
)
  .then((answer) => console.log(answer))
  .catch((err) => console.error(err));
```

## Eigene Erfahrungen

Wie viele andere habe ich MCP mit Tool Calling wahrscheinlich schon tausend Mal genutzt, ohne es zu wissen; der Vorgang läuft für Nutzer ja komplett transparent ab. Als Entwickler damit gearbeitet habe ich allerdings noch nicht. Mir war aber wichtig, das Prinzip zu verstehen. Vielleicht baue ich irgendwann mal ein kleines Projekt, um Tool Calling in der Praxis zu testen.
