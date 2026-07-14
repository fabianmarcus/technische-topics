# Scrum

Scrum ist ein agiles Vorgehensmodell, das in der Softwareentwicklung eingesetzt wird. Es ist ein Rahmenwerk, das Teams dabei unterstützt, komplexe Projekte zu organisieren und zu managen. Scrum basiert auf iterativen und inkrementellen Prozessen, bei denen die Arbeit in festen Zeitabschnitten, sogenannten Sprints, durchgeführt wird.

**Nachfolgend eine Übersicht von Begrifflichkeiten samt kurzer Erklärung, die mir bei der Arbeit als Teil eines Scrum-Teams begegnet sind.**

## Meetings

- Planning
- Daily
- Refinement
- Review
- Retro

### Sprint Planning (am Anfang eines Sprints)

- Ein neuer Sprint wird in Jira angelegt.
- Der Sprint wird mit Tickets aus dem Backlog gefüllt (im Verhältnis zur Kappa des ganzen Teams oder pro Teammitglied).
- Der Sprint wird gestartet.

### Daily (täglich)

- Jeder erzählt, woran er arbeitet und ob es Probleme oder Fragen bei der Umsetzung gibt.
- Tägliches Abstimmen der Aufgaben.

### Ticket Refinement (wöchentlich)

- Im Backlog angelegte Tickets werden zusammen ausformuliert (falls noch nicht geschehen).
- Schätzbare Tickets werden geschätzt, damit sie im nächsten Planning entsprechend der Kappa verplant werden können.
- Bereits bearbeitete, aber noch nicht fertiggestellte Tickets werden erneut geschätzt, um die übrig gebliebenen Punkte zu ermitteln.

### Sprint Review (am Ende des Sprints)

- Abstimmung, was geschafft wurde.
- Berechnung der im Sprint geschafften Story-Punkte (Kappa für den nächsten Sprint).
- Überprüfung des Sprintziels.
- Vorstellung ausgerollter Features (falls vorhanden).

### Retrospective / Retro (am Ende eines Sprints)

- Überprüfung der Arbeitsweise.
- Benennung von Problemen, Schwierigkeiten, Verbesserungsvorschlägen usw.
- Benennung, was gut lief und beibehalten werden sollte.

## Begriffe

### Sprint

- Festgelegte Dauer zur Bearbeitung vereinbarter Tickets.
- Ein Sprint geht meistens 2 bis 3 Wochen.
- Für das Ende des Sprints wird ein Sprintziel festgelegt.
- Während eines Sprints werden keine anderen Tickets bearbeitet, es sei denn die Fertigstellung der vereinbarten Tickets geht schneller als erwartet, dann können Tickets nachgezogen werden.
- Wird ein Sprintziel bzw. werden die Sprint-Tickets während des Sprints obsolet, wird der Sprint abgebrochen und ein neuer definiert (nicht einfach Tickets aus dem Sprint raus und neue in den bestehenden Sprint reinschieben).

### Kappa

- Die Anzahl von Story-Punkten, die ein Team in einem Sprint schafft bzw. in den letzten Sprints geschafft hat.
- Sie ist im Planning die Referenz dafür, wie viele (geschätzte) Tickets in den Sprint kommen.
- Sie berechnet sich auch aus der zur Verfügung stehenden Zeit eines jeden Mitglieds über den nächsten Sprint.

### Epic

- Steht für ein Thema, das aus vielen Unteraufgaben besteht.
- Bündelt mehrere Tickets.
- Wird eventuell nie fertig, ansonsten wie Meilensteine.

### Board

- Unterteilung der Aufgaben nach Teammitgliedern und Status.
- Boards können immer anders aussehen, das Team einigt sich auf eine Struktur.
- Ein Beispiel wäre: todo, on hold / blocked, in progress, in review, done.
- Je nach Status wird das Ticket verschoben.

### Schätzung nach Story-Punkten

- Einzelnen Tickets werden auf Grundlage ihrer Beschreibung und der daraus hervorgehenden Komplexität Zahlen (Punkte) zugewiesen.
- Was die einzelnen Zahlen an Aufwand bedeuten, muss das Team vereinbaren.
- Es kann sich zum Beispiel anhand von Beispielen Komplexitätsreferenzen schaffen.
- Oder die Zahlen können für grobe zeitliche Aufwände stehen.
- Oder ganz was anderes.

## Ticketaufbau

Tickets werden meistens Stories genannt. Die Struktur sollte im Team als Template vereinbart werden.

### Beispielstruktur

- Problembeschreibung oder Feature-Wunsch aus Sicht desjenigen, dem die Umsetzung später zugutekommt.
- Beschreibung der Lösung oder Ausführung technischer Sachverhalte bezüglich der Umsetzung des Tickets.
- Auflisten der einzelnen Todos, die zur Umsetzung des Tickets notwendig sind (grob oder detailliert je nach Absprache).
- Definieren von Akzeptanzkriterien, wann ein Ticket als umgesetzt gilt.
- Definieren einer "Definition of Done", wann ein Ticket als komplett fertig gilt.
- Sonstige nützliche Notizen.
