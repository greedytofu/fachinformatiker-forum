

# Git

Die Grundlage eines CI/CD-Deployments ist GIT SCM. Bitte mache erst den Git-Kurs und stelle sicher, dass Du folgebde Punkt verstanden hast:

- Cloning
- Adding
- Committing
- Pushing
- Branching
- Merging
- Pull Request

Damit das SCM möglichst problemlos erfolgt, erstellen wir einen Key für die Authentifizierung mit Github:

```bash
ssh-keygen -t rsa -b 4096
```

Es werden in ~/.ssh zwei Schlüssel erstellt. Der Schlüssel id_rsa.pub wird bei Github eingefügt. Mein Profil (oben rechts), Settings und dann "SSH und GPG keys".

# Build Automation

Build Automation umfasst folgende Punkte:

- Compilieren
- Abhängigkeiten lösen
- Automatische Tests
- Packet bauen für die Verteilung


