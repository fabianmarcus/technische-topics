# React

## React Compiler

Der React Compiler ist in React 18 eingeführt worden. Er ist zurzeit noch optional, wird aber irgendwann Pflicht sein. Er ist bereits gut getestet und bei Meta produktiv im Einsatz.

Der Compiler ist im Prinzip ein Babel Plugin, das den eigentlichen Compiler ummantelt. Er läuft mit allen gängigen Build Tools und wird während der Build Time ausgeführt.

Die Aufgabe des Compilers ist es, die Performance und Stabilität von React-Anwendungen zu verbessern, indem er das Rerendern von Komponenten optimiert. Er analysiert den Code und erkennt, welche Teile einer Komponente sich ändern und welche nicht. Dadurch kann er unnötige Rerenderings vermeiden und die Performance der Anwendung verbessern.

Was also zuvor durch `useMemo` oder `useCallback` manuell optimiert werden musste, kann nun automatisch vom Compiler erledigt werden. Man muss es nicht mehr selbst schreiben, sondern kann die Variable oder Funktion einfach normal definieren.

Es sind zudem keine Änderungen an bestehendem Code notwendig, um den Compiler zu nutzen. Er optimiert nur da, wo es sinnvoll ist. Bestehende Memoization kann im Code belassen und weiterhin für spezielle Zwecke manuell genutzt werden.

Eine Flag `"compiler": "react"` in der `package.json` aktiviert den Compiler. In Vite ist er standardmäßig bereits aktiviert. Der Rest läuft transparent.

```json
{
  "name": "my-app",
  "version": "1.0.0",
  "compiler": "react"
}
```

## Lifecycle Methods

## State Management

### Custom Hook

### React Context

### Rerendering

## Client vs. Server Components

### Serverside Rendering (SSR)

### Clientside Rendering (CSR)

### "use client" Directive
