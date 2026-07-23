# npm

[npm](https://www.npmjs.com/) ist der quasi-standard package manager für JavaScript. Es wird hauptsächlich für Node.js verwendet, kann aber auch in anderen Projekten eingesetzt werden.

Daneben gibt es noch andere package manager für JavaScript, wie [yarn](https://yarnpkg.com/) oder [pnpm](https://pnpm.io/), die ähnliche Funktionen bieten und ihre eigenen Stärken und Schwächen haben. [npm](https://www.npmjs.com/) ist jedoch der am weitesten verbreitete.

**Ich nutze hauptsächlich npm.** Es sei denn eine bestehende Codebase verwendet bereits einen anderen package manager.

Hier wird es nur um meine Learnings gehen, nicht um npm im Allgemeinen.

## Workspaces

Was mir bislang gar nicht so klar war ist, dass npm von Haus aus [Workspaces](https://docs.npmjs.com/cli/v9/using-npm/workspaces) unterstützt. Das bedeutet, dass man seine Codebasis bzw. sein Repository auf npm Basis in mehrere separate Projekte aufteilen kann. So kann man sich relativ einfach ein Monorepo aufbauen, das ein großes Projekt in Form mehrerer kleinerer Projekte organisiert. Jedes Projekt kann dabei seine eigenen Abhängigkeiten haben, die unabhängig von den anderen Projekten installiert werden. Ansonsten teilen sich die Projekte die Abhängigkeiten, die in der `package.json` im Hauptverzeichnis des Repos definiert sind.

### Einen Workspace erstellen

Um einen Workspace anzulegen, hinterlegt man in der `package.json` im Hauptverzeichnis des Repos die Eigenschaft `workspaces` mit den Pfaden zu den Unterprojekten als Array:

```json
{
  "name": "mein-monorepo",
  "workspaces": [
    "frontend", # Eindeutiger Name
    "backend", # Eindeutiger Name
    "packages/*" # Alle Unterordner im packages Ordner werden als Workspaces behandelt
  ]
}
```

Danach werden die Ordner `frontend` und `backend` angelegt. Das war's.

In beiden Ordnern kann nun über `npm init` eine eigene `package.json` angelegt und eine App aufgebaut werden. Beide Ordner laufen separat voneinander. Werden Abhängigkeiten in beiden Projekten benötigt, kann man diese in der `package.json` im Hauptverzeichnis des Repos definieren. npm installiert diese automatisch in beiden Projekten. Ansonsten definiert jedes Projekt seine eigenen Abhängigkeiten in der eigenen `package.json`.

Möchte man in der `package.json` im Hauptverzeichnis des Repos npm-Skripte definieren, die in einem der Unterprojekte ausgeführt werden sollen, kann man das mit dem Befehl `npm run <skript> --workspace=<projekt>` tun. Dabei ist `<projekt>` der Name des Unterprojekts, wie er in der `package.json` des Unterprojekts definiert ist.

*Wichtig:* Der Befehl `npm install` sollte im Hauptverzeichnis des Repos ausgeführt werden, da es sonst zu Auflösungsproblemen der Abhängigkeiten kommen kann.

Man kann sogar npm Packages auf diese Weise in einem Unterprojekt entwickeln und in einem anderen Unterprojekt als Abhängigkeit installieren und verwenden. Das wird in `Lokale packages` beschrieben.

## Lokale packages

In der Regel werden in einem Projekt einige npm Packages installiert und in der `dependencies`-Liste als Abhängigkeiten hinterlegt. Diese Packages stammen meistens aus der öffentlichen npm Registry. Über Workspaces ist es aber auch möglich, ein npm Package in einem Unterprojekt zu entwickeln und es in einem anderen Unterprojekt als Abhängigkeit zu installieren und zu verwenden. Das ist besonders praktisch beim Aufbau eines Monorepos, in dem mehrere Projekte den gleichen Code benötigen.

Ein lokales Package wird auch, wie oben beschrieben, als Workspace angelegt. Es wird also der Ordner `packages/mein-lokales-package` angelegt.

Da `mein-lokales-package` im Ordner `packages/` angelegt wurde, brauchen wir dafür keinen expliziten Workspace-Eintrag in der `workspaces`-Eigenschaft. Aufgrund der Wildcard-Angabe `packages/*` werden alle Unterordner im `packages/`-Ordner automatisch als Workspaces behandelt.

Wichtig für ein npm Package ist der Aufbau der `package.json`. Im Gegensatz zu einem normalen Projekt, das nur eine App enthält, muss ein npm Package in der `main`- oder `exports`-Eigenschaft auf die Datei(en) zeigen, die exportiert werden soll(en). In der Regel ist das die `index.js` als Einstiegspunkt des Kompilats im `dist`-Verzeichnis.

Ansonsten läuft die Entwicklung eines lokalen Packages wie die Entwicklung einer normalen App. Man kann Abhängigkeiten installieren, Skripte definieren und ausführen, Tests schreiben und ausführen usw.

```json
{
  "name": "mein-lokales-package",
  "version": "1.0.0",
  "type": "module",
  "main": "./dist/index.js",
  "types": "./dist/index.d.ts",
  "exports": {
    ".": {
      "import": "./dist/index.js",
      "types": "./dist/index.d.ts"
    }
  }
}
```

In der `package.json` des Unterprojekts, das das lokale Package verwenden soll, wird es dann als Abhängigkeit mit dem Pfad zum Unterprojekt angegeben:

```json
{
  "name": "frontend",
  "dependencies": {
    "mein-lokales-package": "file:../packages/mein-lokales-package"
  }
}
```

## npm package "concurrently"

Das das Terminal beim Aufrufen von npm Skripten in der Regel blockiert, hatte ich beim Entwickeln meistens mehrere Terminals offen, um mehrere Projekte gleichzeitig laufen zu lassen. Mit `concurrently` kann man das umgehen.

`concurrently` ist ein npm Package, das es ermöglicht, mehrere Skripte gleichzeitig auszuführen. Das ist besonders praktisch, wenn man mehrere Projekte in einem Monorepo hat, die gleichzeitig laufen sollen. So kann man z.B. das Frontend und das Backend gleichzeitig starten, ohne zwei separate Terminals öffnen zu müssen.

```json
{
  "name": "mein-monorepo",
  "scripts": {
    "start": "concurrently \"npm run start --workspace=frontend\" \"npm run start --workspace=backend\""
  }
}
```
