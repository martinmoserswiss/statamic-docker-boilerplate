# Statamic Docker Boilerplate

Boilerplate für ein Statamic-Projekt mit PHP-FPM in Docker. `docker-compose.yml` und `.env.example` liegen im Projekt-Root, weitere Docker-Dateien unter `docker/`.

## Voraussetzungen

Die Zielumgebung ist eine Unix-VM mit Root-Zugriff und folgenden installierten Tools:

- `git`
- `docker` und `Docker Compose`
- `caddy`

## Projektstruktur

```text
.
├── .env.example
├── docker-compose.yml
├── app/                # Statamic projekt
├── docker/
│   ├── default.conf    # Nginx config im Container    
│   └── php/            # Dockerfile und php.ini
└── README.md
```

## Setup

### 1. URL auf die passende VM umleiten

Lege beim DNS-Provider A-Records für Root-Domain und `www` auf die IP deiner VM.

Beispiel:

- `my-project.com` -> `203.0.113.10`
- `www.my-project.com` -> `203.0.113.10`

### 2. Template klonen

```bash
git clone git@github.com:martinmoserswiss/statamic-nginx-docker-boilerplate.git cms
cd cms
```

### 3. Template Repo entfernen

```bash
rm -rf .git
```

### 4. Boilerplate-.env erstellen

Alle boilerplate-spezifischen Werte werden in `.env` im Projekt-Root gepflegt.

```bash
cp .env.example .env
```

Passe danach die Werte in `.env` an.

### 5. Caddy Site File erstellen

1. Erstelle auf der VM eine Caddy-Site für die Domain. Z.B. `cp m0s.ch.caddy zoko.ch.caddy`

1. Passe entsprechend den Inhalt der Datei an.

1. Lade die Caddy Konfiguration mit `docker exec caddy caddy reload --config /etc/caddy/Caddyfile` nach.

### 6. Statamic-Projekt clonen

Das Repository wird nach `app/` geklont:

```bash
git clone <STATAMIC_REPOSITORY> app
```

Beispiel:

```bash
git clone git@github.com:martinmoserswiss/zoko.git app
```

### 7. Statamic-.env erstellen und Werte anpassen

```bash
cp app/.env.example app/.env
```

Passe die Werte an:

- `APP_NAME`
- `APP_URL`
- optional `STATAMIC_LICENSE_KEY`
- optional `STATAMIC_PRO_ENABLED`

### 8. Docker Compose kontrollieren

Die Standardwerte kommen aus `.env`. In der Regel reicht es, nur diese Datei zu pflegen.

### 9. Container starten

```bash
docker compose up -d
```

### 10. Im Container Dependencies installieren

```bash
composer install
npm install
npm run build
php artisan key:generate
```

## Betrieb

Container stoppen:

```bash
docker compose down
```

Container starten:

```bash
docker compose up -d
```

Statamic-Cache leeren:

```bash
php artisan view:clear
php artisan config:clear
php artisan cache:clear
```
