# Bundling

## Was ist ein Bundler (kurz & knapp)?

So wie ein Compiler den Quellcode in Maschinensprache übersetzt, übersetzt ein Bundler den Quellcode in ein Format, das von Browsern verstanden wird. Browser können nur JavaScript, CSS und HTML direkt interpretieren. Alles andere muss vorher in diese Formate übersetzt werden. Eine TypeScript-Anwendung kann vom Browser als solche nicht direkt ausgeführt werden. Ein Bundler übernimmt diese Aufgabe und kann dabei auch noch weitere Optimierungen durchführen.

Optimieren kann zum Beispiel bedeuten:

- Nur Code einbinden, der auch wirklich genutzt wird (Tree Shaking).
- Code optimieren, um die Performance zu verbessern (Code Optimization).
- Code minimieren, um die Größe der Dateien zu reduzieren (Minification).
- Code in kleinere Teile aufteilen, die nur bei Bedarf geladen werden (Code Splitting).
- Automatisch Sourcemaps erstellen, um die Fehlersuche zu erleichtern.
- Assets wie Bilder, Fonts oder Videos in das Bundle kopieren und dabei für die Nutzung optimieren.
- Und vieles mehr...

Am Ende entsteht ein `distribution`-Ordner, der die komplette optimierte und startbare Anwendung enthält. Dieser Ordner wird in der Regel als Artefakt in die CI/CD-Pipeline eingebunden und auf den Server oder in die Cloud deployt.

Dasselbe gilt für Node.js. Auch Node.js kann nur JavaScript direkt ausführen. TypeScript muss vorher in JavaScript übersetzt werden. Ein Bundler kann auch hier die Übersetzung übernehmen und den Code optimieren. Hier kann und muss aber nicht ganz so viel optimiert werden. Prinzipiell reicht es auch, den TypeScript-Code ohne Bundler mit `tsc` in JavaScript zu übersetzen.

## Produkte

Ein Auszug von Produkten, die heute in der Frontend-Entwicklung als Bundler genutzt werden. Ich habe nicht alle davon ausprobiert, das Prinzip ist aber bei allen ähnlich. Sie konkurrieren hauptsächlich in ihrer Geschwindigkeit und der Größe der Bundles. In einigen Fällen wurde auch nur ein Refactoring auf eine andere Sprache (z.B. Rust oder Go) durchgeführt, um die Performance zu verbessern und dann als neues Produkt auf den Markt gebracht.

Mit Webpack habe ich am meisten gearbeitet. Mittlerweile sind viele Starterpacks mit Vite vorkonfiguriert. Oft ist der Bundler auch schon im Framework integriert, wie z.B. bei Next.js. Dadurch wird die Nutzung transparent. Man hat nicht mehr viel mit dem Bundler zu tun, außer man muss ihn anpassen oder erweitern.

| Produktlink | Kurzbeschreibung | Eigene Erfahrung |
| --- | --- | --- |
| [Webpack](https://webpack.js.org/) | Etablierter, sehr flexibler Bundler mit großem Plugin-Ökosystem. | x |
| [Vite](https://vitejs.dev/) | Schneller Dev-Server und moderner Build-Tooling-Ansatz für Frontend-Projekte. | x |
| [Rollup](https://rollupjs.org/) | Fokus auf schlanke Bundles, häufig für Libraries und Packages genutzt. | x |
| [Turbopack](https://turbo.build/pack) | Neuer Hochleistungs-Bundler aus dem Next.js-Umfeld. | Transparent |
| [Rspack](https://rspack.rs/) | Webpack-ähnlicher Bundler mit Fokus auf Geschwindigkeit durch Rust. | - |
| [esbuild](https://esbuild.github.io/) | Extrem schneller Bundler und Transpiler, ebenfalls in Go geschrieben. | - |
| [Oxidation Compiler](https://oxc.rs/) | Moderne Toolchain in Rust mit Fokus auf Parsing, Linting und Compiler-Bausteine. | - |

## Eigene Erfahrungen

Bundler waren und sind für mich immer nur notwendiges Beiwerk. Gut, dass sie da sind, aber sie sollen eigentlich nur funktionieren. Ich habe nie einen eigenen Loader oder ein Plugin entwickelt. Allerdings habe ich mit Webpack in Selbstkonfiguration einiges angestellt. Webpack hat mir gut gefallen. Bei Vite und Rollup musste ich bislang nicht wirklich über die Grundeinstellungen hinaus.
