# Lovelace Céramique

Plateforme web pour un atelier de céramique — projet collectif Ada Tech School (CDA).

Stack actuelle (cours 1) : PHP 8.5 · PostgreSQL 17 · Docker Compose.

## Prérequis

- Docker Desktop (ou Docker Engine + Compose v2)
- Git
- Un compte GitHub avec accès au dépôt

```bash
docker info
```

## Installation

1. Cloner le dépôt

```bash
git clone <url-du-depot> lovelace-ceramique
cd lovelace-ceramique
```

2. Créer le fichier d’environnement (**obligatoire** — `.env` n’est pas versionné)

```bash
cp .env.example .env
```

Sans ce fichier, Postgres peut refuser de démarrer (`POSTGRES_PASSWORD` manquant).

3. Lancer la stack

```bash
docker compose up --build
```

4. Ouvrir http://localhost:8080

Tu dois voir la version PHP et « Connexion PostgreSQL OK ».

5. Ouvrir pgAdmin : http://localhost:5050

Identifiants pgAdmin (voir aussi `.env`) :

- Email : `ada@example.com`
- Mot de passe : `ada_dev`

Puis enregistrer le serveur PostgreSQL (Register → Server) :

| Champ | Valeur |
|-------|--------|
| Name | Lovelace |
| Host name/address | `postgres` |
| Port | `5432` |
| Maintenance database | `lovelace` |
| Username | `lovelace` |
| Password | `lovelace_dev` |

Important : le host est `postgres` (nom du service Docker), pas `localhost`.

Arrêt : `docker compose down`

### Dépannage

Erreur `data directory was initialized by PostgreSQL version 16` (ou autre version), ou `password authentication failed for user "lovelace"` : le volume Docker a été créé avec d’anciennes données / un autre mot de passe. Réinitialiser (données locales perdues) :

```bash
docker compose down -v
cp .env.example .env
docker compose up --build
```

Erreur `not a directory` / mount de `default.conf` (souvent Windows) : Docker a créé un **dossier** à la place du fichier. Supprimer ce dossier fantôme, récupérer le fichier Git, puis rebuild :

```bash
# PowerShell
Remove-Item -Recurse -Force .\docker\nginx\default.conf -ErrorAction SilentlyContinue
git checkout -- docker/nginx/default.conf
docker compose down
docker compose up --build
```

## Contribution

- Toujours partir de `main` à jour : `git switch main && git pull`
- Créer une branche : `git switch -c feature/ma-feature`
- Une branche = une feature ou un fix
- PR obligatoire avant merge sur `main` (1 approbation minimum)
- Convention de commits : [Conventional Commits](https://www.conventionalcommits.org/) (`feat:`, `fix:`, `docs:`, `refactor:`, `chore:`)

### Convention de nommage des branches

| Préfixe | Usage | Exemple |
|---------|--------|---------|
| `feature/` | nouvelle fonctionnalité | `feature/catalogue-pieces` |
| `fix/` | correction de bug | `fix/validation-email` |
| `docs/` | documentation | `docs/readme-installation` |
| `refactor/` | restructuration | `refactor/separation-services` |
| `chore/` | config, deps, tooling | `chore/mise-a-jour-dependances` |

Exemples :

1. Story « Afficher le catalogue des pièces » → `feature/catalogue-pieces`
2. Story « Corriger l’erreur de connexion API » → `fix/connexion-postgres`
3. Story « Documenter le choix PostgreSQL » → `docs/adr-choix-bdd`

## Équipe

À compléter par l’équipe projet :

- … — …
- … — …
- … — …
