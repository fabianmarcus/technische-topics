# SDKs wie Ollama, LlamaIndex und LangChain

Auszug einer Copilot Konversation...

## Sind Ollama SDK, LangChain und LlamaIndex ungefähr das Gleiche?

Nein, nur sehr grob im selben Themenfeld. Sie lösen unterschiedliche Ebenen des Problems.

**[Ollama SDK](https://ollama.com/)**  
Das SDK ist eine Schnittstelle zu Ollama. Damit sprichst du lokale Modelle an, die über Ollama laufen, zum Beispiel Llama, Mistral, Qwen oder Gemma. Es kümmert sich im Kern um Dinge wie:

- Modell aufrufen
- Chat-/Completion-Anfragen senden
- Streaming-Antworten lesen
- Embeddings über Ollama abrufen

Es ist eher ein **Client für einen Modell-Server**.

**[LangChain](https://www.langchain.com/)**  
LangChain ist ein Framework zum Bauen von LLM-Anwendungen. Es kann Ollama als Modellanbieter verwenden, aber auch OpenAI, Anthropic, Azure OpenAI, Hugging Face und andere. Es bietet Bausteine für:

- Prompt-Templates
- Chains und Workflows
- Tool-/Function-Calling
- Agenten
- Memory
- RAG-Pipelines
- Integrationen mit Vektor-Datenbanken, APIs und Dokumentenquellen

LangChain ist also eher ein **Orchestrierungsframework für LLM-Apps**.

**[LlamaIndex](https://gpt-index.readthedocs.io/en/latest/index.html)**  
LlamaIndex ist ebenfalls ein Framework für LLM-Anwendungen, aber mit stärkerem Fokus auf **Datenanbindung und Retrieval-Augmented Generation (RAG)**. Es hilft besonders bei:

- Dokumente laden
- Dokumente chunking/indexieren
- Embeddings erzeugen
- Vektorindizes verwalten
- relevante Textstellen wiederfinden
- Fragen über eigene Daten beantworten

LlamaIndex ist also eher ein **Daten- und Retrieval-Framework für LLMs**.

Kurz gesagt:

| Tool | Hauptrolle |
| --- | --- |
| Ollama SDK | Modelle über Ollama direkt ansprechen |
| LangChain | LLM-Workflows, Tools, Agenten und App-Logik bauen |
| LlamaIndex | Eigene Daten indexieren und für RAG nutzbar machen |

Ein Vergleich als Schichtenmodell:

```text
Deine App
  ↓
LangChain / LlamaIndex
  ↓
Ollama SDK oder anderer Provider-Client
  ↓
Ollama
  ↓
Lokales Modell
```

Du kannst sie also kombinieren. Zum Beispiel: LlamaIndex lädt und indexiert deine Dokumente, Ollama liefert lokal Embeddings und Antworten, und LangChain orchestriert einen komplexeren Agenten-Workflow.

Wenn du nur lokal ein Modell fragen willst, reicht oft das Ollama SDK. Wenn du Workflows, Tools oder Agenten brauchst, ist LangChain passender. Wenn du Fragen über eigene Dokumente beantworten willst, ist LlamaIndex oft der direktere Einstieg.

## Kann LangChain keine Antworten zu eigenen Dokumenten geben?

Doch, LangChain kann Antworten zu eigenen Dokumenten geben. Das ist ein klassischer RAG-Anwendungsfall, und LangChain unterstützt das sehr gut.

Der Unterschied ist eher der Schwerpunkt:

**LangChain kann RAG**, aber es ist breiter aufgestellt. Es bietet viele Bausteine für LLM-Apps: Chains, Agents, Tools, Prompting, Memory, Retriever, Vector Stores, Dokument-Loader und so weiter.

**LlamaIndex ist stärker auf RAG spezialisiert**. Dort stehen Datenaufnahme, Indexierung, Retrieval und Query Engines sehr zentral im Design.

Praktisch heißt das:

```text
Eigene Dokumente
  ↓
Loader
  ↓
Chunking
  ↓
Embeddings
  ↓
Vektorindex
  ↓
Retriever
  ↓
LLM-Antwort mit Kontext
```

Das geht mit beiden.

Mit LangChain würdest du zum Beispiel Dokumente laden, in Chunks teilen, Embeddings erzeugen, sie in einer Vektordatenbank speichern und dann per Retriever passende Textstellen zur Frage holen. Diese Textstellen werden dem LLM als Kontext gegeben.

Der Unterschied in einem Satz:

**LangChain ist ein allgemeines Framework für LLM-Anwendungen, das auch RAG kann. LlamaIndex ist ein Framework, das besonders stark auf RAG und Wissenszugriff über eigene Daten ausgerichtet ist.**

Eine grobe Faustregel:

| Ziel | Eher passend |
| --- | --- |
| Chatbot mit Tools, APIs, Agentenlogik, Workflows | LangChain |
| Fragen über PDFs, Markdown, Webseiten, interne Wissensdatenbanken | LlamaIndex |
| Beides kombiniert | Geht auch, beide können zusammenarbeiten |

Also: Ja, LangChain kann das. Ich würde es nicht als „LangChain kann keine Dokumente“ verstehen, sondern als „LlamaIndex wurde stärker um Dokumente und Retrieval herum gebaut“.

## Ist es wesentlich einfacher mit LlamaIndex RAG zu betreiben als mit LangChain?

Ja, meistens ist RAG mit **LlamaIndex spürbar einfacher**, wenn dein Hauptziel wirklich „eigene Dokumente rein, Fragen dazu raus“ ist.

LlamaIndex ist stärker um genau diesen Ablauf herum gebaut:

```text
Dokumente laden → indexieren → relevante Stellen finden → Antwort generieren
```

Viele Konzepte sind dort direkt als Hauptabstraktionen angelegt: `Document`, `Index`, `Retriever`, `QueryEngine`, `Node`, `Reader`. Dadurch fühlt sich ein einfacher RAG-Prototyp oft direkter an.

Bei **LangChain** geht RAG ebenfalls gut, aber du baust stärker aus generischen Bausteinen zusammen:

```text
Loader → Splitter → Embeddings → Vector Store → Retriever → Prompt → Chain → LLM
```

Das ist flexibler, aber anfangs oft etwas mehr Verkabelung.

**Faustregel:**

| Situation | Einfacher |
| --- | --- |
| Schnell RAG über lokale Dateien, PDFs, Markdown, Webseiten bauen | LlamaIndex |
| Datenaufnahme, Chunking, Indexing und Querying möglichst aus einer Hand | LlamaIndex |
| RAG ist nur ein Teil einer größeren App mit Tools, Agenten, APIs, Workflows | LangChain |
| Du willst sehr genau kontrollieren, wie die einzelnen Schritte zusammenspielen | LangChain |
| Du baust produktionsnahe LLM-Orchestrierung mit vielen Integrationen | LangChain |

Der wichtige Zusatz: „einfacher“ heißt nicht automatisch „besser“. LlamaIndex gibt dir für RAG einen bequemeren Start und gute höhere Abstraktionen. LangChain ist oft besser, wenn RAG nur ein Baustein in einer komplexeren Anwendung ist.

Kurz gesagt:

**Für reines RAG: LlamaIndex ist in der Regel einfacher.  
Für RAG plus Agenten, Tools und komplexe Workflows: LangChain kann passender sein.**

Mit Ollama im Hintergrund können beide arbeiten. Du könntest also lokal mit Ollama laufen und darüber entweder LlamaIndex oder LangChain setzen.
