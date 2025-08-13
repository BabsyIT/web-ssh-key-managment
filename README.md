# 🔐 GitHub-Native SSH Key Management

Vollständig serverlose SSH-Key-Verwaltung über GitHub Actions.

## 🚀 Features

- ✅ **Serverless** - Läuft komplett auf GitHub Actions
- ✅ **Kostenlos** - Nutzt GitHub Free Tier
- ✅ **Automatisch** - Sync alle 5 Minuten
- ✅ **Zentral** - Eine Konfiguration für alle Server
- ✅ **Sicher** - GitHub OAuth + SSH Keys
- ✅ **Dashboard** - Live Status über GitHub Pages

## 📊 Dashboard

**Live Dashboard:** https://stefan-ffr.github.io/web-ssh-key-managment

## ⚙️ Setup

### 1. SSH Private Key hinzufügen

Erstelle einen SSH-Key für Server-Zugriff:

```bash
# SSH-Key erstellen
ssh-keygen -t ed25519 -f ~/.ssh/github_actions -N ""

# Public Key auf alle Server kopieren
ssh-copy-id -i ~/.ssh/github_actions.pub root@your-server.com

# Private Key als GitHub Secret hinzufügen
gh secret set SSH_PRIVATE_KEY < ~/.ssh/github_actions
```

### 2. Konfiguration anpassen

Bearbeite `config/users.json`:

```bash
# Mit GitHub Editor
gh browse config/users.json

# Oder lokal
code config/users.json
```

**Wichtig:** 
- Ändere die Beispiel-Server-IP zu deinen echten Servern
- Füge weitere Benutzer hinzu
- Teste die Konfiguration

### 3. GitHub Pages aktivieren

```bash
# Pages automatisch aktivieren
gh api repos/:owner/:repo --method PATCH \
  -f has_pages=true \
  -f pages.source.branch=gh-pages
```

### 4. Ersten Sync starten

```bash
# Workflow manuell triggern
gh workflow run ssh-key-sync.yml

# Status verfolgen
gh run watch
```

## 🎯 Verwendung

### Neuen Benutzer hinzufügen

1. Bearbeite `config/users.json`
2. Commit & Push
3. GitHub Actions startet automatisch
4. Check Dashboard für Status

### Sync-Status prüfen

```bash
# Letzte Workflow-Läufe
gh run list --workflow=ssh-key-sync.yml

# Logs anzeigen
gh run view --log
```

### Dashboard öffnen

```bash
# Dashboard im Browser öffnen
gh browse --settings

# Oder direkt:
# https://stefan-ffr.github.io/web-ssh-key-managment
```

## 🔧 Erweiterte Konfiguration

### Slack-Benachrichtigungen

```bash
# Slack Webhook als Secret hinzufügen
gh secret set SLACK_WEBHOOK --body "https://hooks.slack.com/services/..."
```

### Sudo-Rechte verwalten

```json
{
  "github_user": "username",
  "local_user": "localname",
  "sudo_access": "full|limited|none",
  "sudo_commands": [
    "/usr/bin/systemctl restart nginx",
    "/usr/bin/docker *"
  ],
  "groups": ["sudo", "docker", "www-data"]
}
```

### Sync-Intervall ändern

Bearbeite `.github/workflows/ssh-key-sync.yml`:

```yaml
on:
  schedule:
    - cron: '*/10 * * * *'  # Alle 10 Minuten
```

## 📊 Monitoring

- **Dashboard**: Live-Status aller Hosts und Benutzer
- **GitHub Issues**: Automatische Sync-Reports
- **GitHub Actions**: Workflow-Logs und Historie
- **Slack**: Optional Benachrichtigungen

## 🔒 Sicherheit

- SSH Private Key als GitHub Secret gespeichert
- Keine Passwörter - nur SSH-Key-Authentifizierung
- Sudo-Rechte granular konfigurierbar
- Vollständige Audit-Logs in GitHub

## 🆘 Troubleshooting

### SSH-Verbindung fehlgeschlagen

```bash
# Key-Berechtigung prüfen
ssh -i ~/.ssh/github_actions root@your-server.com

# Known Hosts prüfen
ssh-keyscan -H your-server.com
```

### Konfiguration ungültig

```bash
# JSON validieren
jq . config/users.json

# Schema prüfen
jq '.users[].github_user' config/users.json
```

### Workflow-Fehler

```bash
# Logs anzeigen
gh run view --log

# Re-run failed jobs
gh run rerun --failed
```

## 🎉 Nächste Schritte

1. ✅ Server-IPs in `config/users.json` anpassen
2. ✅ SSH Private Key als Secret hinzufügen
3. ✅ Ersten Sync durchführen
4. ✅ Dashboard checken
5. ✅ Team-Mitglieder hinzufügen

**→ Happy SSH Key Management! 🚀**
