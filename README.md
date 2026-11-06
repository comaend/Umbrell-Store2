# StreamFlix Community App Store

Selbst gehostete Streaming-Plattform im Stil von Netflix für UmbrelOS (Raspberry Pi ARM64).

## Repository-Struktur

```text
streamflix-community-store/          <- Repository-Wurzel (auf GitHub hochladen)
├── README.md
├── LICENSE
├── umbrel-app-store.yml             <- Store-Metadaten (id + name)
└── streamflix-streamflix/           <- App-Ordner (Name muss der App-ID entsprechen)
    ├── umbrel-app.yml               -> App-Manifest (Umbrel erkennt die App daran)
    ├── docker-compose.yml           -> Container-Konfiguration
    ├── Dockerfile                   -> PHP 8.2 + Nginx + FFmpeg (ARM64)
    ├── health.php
    ├── README.md                    -> Installationsanleitung (in der App)
    ├── docker/
    │   └── nginx.conf
    ├── app/                         -> PHP-Anwendung (Frontend, API, Admin)
    ├── database/                    -> SQLite-Schema
    ├── scripts/                     -> Medien-Scanner, Backup, Cleanup
    └── database/migrations/
```

## Wichtige Hinweise vor dem Upload

1. **GitHub-Benutzername ersetzen**: Öffne `streamflix-streamflix/umbrel-app.yml` und ersetze jeden Vorkommen von `DEIN_GITHUB_BENUTZERNAME` durch deinen tatsächlichen GitHub-Benutzernamen. Dadurch werden App-Icon und Galeriebilder korrekt geladen.

2. **App-Icon und Galerie (optional)**: Lege unter `streamflix-streamflix/` folgende Bilder ab:
   - `icon.png` (App-Icon im Store)
   - `1.png`, `2.png`, `3.png` (Galerie-Screenshots)
   Aktualisiere die URLs in `umbrel-app.yml` entsprechend.

3. **Repository-Wurzel**: Die ZIP darf keine zusätzliche überflüssige Ordnerebene enthalten. Der Inhalt von `streamflix-community-store/` muss direkt im Wurzelverzeichnis des GitHub-Repos landen.

## Installation auf UmbrelOS

1. Repository als ZIP herunterladen und entpacken.
2. Alle Dateien auf GitHub hochladen (neues Repository, z. B. `streamflix-community-store`).
3. UmbrelOS öffnen → App Store → Community App Stores.
4. GitHub-Repository-URL einfügen (z. B. `https://github.com/DEIN_GITHUB_BENUTZERNAME/streamflix-community-store`).
5. Den neuen Community Store öffnen und **StreamFlix** auswählen.
6. Auf **Installieren** klicken. Umbrel erstellt und startet die Container automatisch.
7. Nach der Installation StreamFlix über die Umbrel-App-Übersicht öffnen.
8. Erster Administrator-Account erstellen (Registrierung).
9. Im Admin-Panel (`/admin`) Medienordner hinzufügen und die Bibliothek scannen.

## Medienordner einbinden

Ein Container kann nicht automatisch auf beliebige Host-Verzeichnisse zugreifen. Binde deine Medienordner schreibgeschützt ein:

- Über UmbrelOS: Speicher-Einstellungen → USB-Festplatte / Netzwerkspeicher einbinden.
- Oder trage in `docker-compose.yml` weitere Bind-Mounts ein, z. B.:
  ```yaml
  volumes:
    - ${APP_DATA_DIR:-./data}/data:/var/www/html/data:z
    - /mnt/hdd/Movies:/media/Movies:ro,z
    - /mnt/hdd/Series:/media/Series:ro,z
  ```
  Danach App in Umbrel neu starten.

Im Admin-Panel unter **Medienordner** die Pfade (z. B. `/media/Movies`, `/media/Series`) registrieren und einen Scan starten.

## Support

Probleme und Feature-Requests über die GitHub-Issues des Repositories.
