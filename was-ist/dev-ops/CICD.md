# DevOps

## Pipeline

Eine Pipeline ist eine Abfolge von Schritten, die vor dem Ausliefern einer Software automatisiert ausgeführt werden. Im TypeScript Ökosystem ist das mindestens: `npm install`, `npm lint`, `npm test` und `npm build`. Es können aber noch weitere Schritte ausgeführt werden, die notwendig sind, um die Software zu überprüfen und auszuliefern. Zum Beispiel das Erstellen von Docker-Images oder das Ausführen von Security-Checks.

Die Pipeline kann ein lokales Skript sein, das auf dem eigenen Rechner ausgeführt wird, oder ein Cloud-Dienst wie GitHub Actions, GitLab CI/CD oder Azure DevOps. Bei einem Dienst wird die Pipeline meistens über eine spezielle YAML-Datei konfiguriert und bei jedem Push in das Repository automatisch entsprechend ausgeführt. Der Dienst pullt anschließend den aktuellen Codestand des Repos und führt die definierten Schritte in einer eigenen (Container-)Umgebung aus.

Das Ergebnis der Pipeline ist entweder ein erfolgreich gebautes Artefakt, das ausgeliefert werden kann, oder ein Fehler, der behoben werden muss.

## Deployment

Ein Deployment ist der Prozess, bei dem eine Software auf einem Server oder in der Cloud bereitgestellt wird. Beispielsweise wird ein reines Frontend in der Regel zuvor gebaut, der entstandene Build-Ordner wird dann auf einen Webserver hochgeladen. Ein Backend wird in der Regel als Docker-Image gebaut und dann auf einem Server oder in der Cloud via Docker gepullt, gestartet und bereitgestellt.

## Continuous Integration (CI)

Continuous Integration bedeutet, dass der Code eines Repositories durch eine Pipeline automatisch geprüft (z.B. gelintet, gebaut und getestet) wird, sobald Änderungen eingecheckt werden.

Das hilft, Fehler beim Zusammenspiel von Code-Änderungen frühzeitig zu erkennen und zu beheben. Der Code wird dabei nicht ausgeliefert. Es wird quasi nur der vorangehende Teil der Deployment-Pipeline ausgeführt, um zu sehen, ob man ihn ausliefern könnte.

## Continuous Delivery / Deployment (CD)

Continuous Deployment bedeutet, dass Software nach dem erfolgreichen Durchlaufen der Pipeline automatisch auch ausgeliefert wird.

Der Unterschied zwischen Delivery und Deployment liegt darin, dass bei Continuous Delivery die Software nach dem erfolgreichen Durchlaufen der Pipeline bereitgestellt werden kann, aber nicht automatisch ausgeliefert wird. Der Nutzer stößt den Auslieferungsprozess des vorliegenden Artefakts manuell an (z.B. durch einen Button).

Bei Continuous Deployment hingegen wird die Software nach dem erfolgreichen Durchlaufen der Pipeline automatisch ausgeliefert, ohne weiteres Zutun des Nutzers.

## Eigene Erfahrungen

Produkte mit denen ich zuletzt Pipelines angelegt habe, um ein Deployment zu automatisieren, sind zum Beispiel:

- GitHub Actions
- GitLab CI/CD
- Eigenes lokales Bash-Skript

Im Fall einer ImmoScout24-Extension liefert die Deployment Pipeline in GitHub Actions ein neues GitHub Release automatisch in alle gängigen Browser-Stores aus.
