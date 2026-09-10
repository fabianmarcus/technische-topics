# Google Cloud

## Firebase

Die meisten meiner privaten Projekte mache ich mit Firebase. Die Einrichtung ist kaum Aufwand und man kann quasi direkt loslegen.

Firebase ist eine Plattform von Google, die verschiedene Dienste für die Entwicklung von Web- und Mobilanwendungen bereitstellt. Sie bietet unter anderem SDKs, Datenbanken, Authentifizierung, Hosting und Cloud-Funktionen aus eine Hand.

Mit Firebase ist es möglich, schnell und einfach Anwendungen zu entwickeln, ohne sich um die Infrastruktur kümmern zu müssen. Man initiert via Firebase CLI Tools ein Projekt, wählt die gewünschten Dienste aus und kann direkt loslegen. Im Hintergrund kümmert sich Firebase um die Bereitstellung und Skalierung der benötigten Ressourcen wie serverless Functions, Datenbanken und Hosting.

## Firestore

Firestore ist eine objektorientierte NoSQL-Datenbank von Firebase, die für die Speicherung und Synchronisierung von Daten in Echtzeit entwickelt wurde. Sie ermöglicht es Entwicklern, strukturierte Daten in Form von Dokumenten und Sammlungen zu speichern und auf diese Daten von verschiedenen Plattformen aus zuzugreifen.

Über Snapshots und Listener können Entwickler in Echtzeit auf Änderungen in der Datenbank reagieren und die Benutzeroberfläche entsprechend aktualisieren.

## Firebase Functions

Firebase Functions sind serverlose Funktionen, die in der Google Cloud ausgeführt werden. Sie können auf Ereignisse aus Firebase-Diensten (`on`-Trigger) reagieren oder auf gewöhnliche HTTP-Anfragen via URL. Sie lassen sich leicht anlegen, da sie nur als TypeScript-Funktionen definiert werden müssen. Die Provisionierung erfolgt automatisch über die Firebase CLI, die die Funktionen automatisch in die Cloud deployed. Das Backend steht quasi on demand zur Verfügung. Über das Firebase Backend oder die Google Cloud Console können die Funktionen verwaltet, überwacht und konfiguriert werden.

Damit auch der Coding Assistent die Funktionen so anlegt, wie ich mir das vorstelle, habe ich mir einen Copilot Skill erstellt, der sich beim Code Generieren automatisch um Architektur und Versionierung kümmert. So kann ich bedenkenlos deployen und bei Problemen mit einem Rollback auf ein vorheriges Frontend Release zurücksetzen: [Copilot Skill für Firebase Functions Architektur und Versionierung](https://github.com/fabianmarcus/copilot/blob/main/skills/firebase-functions-architect/SKILL.md)
