# Installation

## Pré-requis

1.  Installer [Node.js](https://nodejs.org/en/).
1.  Installer [git](https://git-scm.com/).

## Dépôt du projet

1.  Clôner le dépôt

        git clone https://github.com/rtaggo/fnac-simulation.git

1.  Changer de répertoire:

        cd fnac-simulation

1.  Installer les dépendances:

        npm install

## Variables d'environnement

Afin de faire fonctionner l'application, il est nécessaire que des variables d'environnement soient définies.

Copier le fichier d'exemple .env_fnac dans un fichier nommé .env

Renseigner les valeurs

```yaml
NODE_ENV=production
PORT=5000
MAPBOX_API_KEY=PUT_YOUR_MAPBOX_API_KEY_HERE
SCHEMA_TABLE='public'
SCHEMA_COMMON='public'
USER_MODE=galigeo
LOGIN_SERVER=GGO_CLOUD_LOGIN_SERVER if USER_MODE is galigeo
SIMULATION_MODE=postgres
SIMULATION_RUN=rscript
SIMULATION_RSCRIPT_PATH=PUT_ABSOLUTE_PATH_TO_R_SCRIPT
GEOCODER=galigeo
HERE_API_KEY=PUT_HERE_API_KEY (if GEOCODER is here)
GGO_REST_API_URL=https://ggo-rest-api.appspot.com
JWT_SECRET=PUTYOURSECRETPHRASE
JWT_EXPIRESIN='10 days'
JWT_TOKENNAME='ggofnac_simulation_accessToken'
POSTGRES_USER=
POSTGRES_PASSWORD=
POSTGRES_DATABASE_NAME=
POSTGRES_HOST=localhost
POSTGRES_PORT=5432
```

| Variables                 | Explication                                         | Exemple (Production)             |
| ------------------------- | --------------------------------------------------- | -------------------------------- |
| `NODE_ENV`                | Environnement d'exécution                           | production                       |
| `PORT`                    | Port de l'application web                           | 5000                             |
| `MAPBOX_API_KEY`          | Clé de l'API Mapbox pour la carte                   | XXXXXXX                          |
| `SCHEMA_TABLE`            | Schéma où les tables se trouveront                  | public                           |
| `SCHEMA_COMMON`           | Schéma où se trouvent les tables IRIS               | public                           |
| `USER_MODE`               | Indique comment sont gérés les utilisateurs         | galigeo                          |
| `LOGIN_SERVER`            | URL de l'application Galigeo                        | https://fnac.galigeo.com         |
| `SIMULATION_MODE`         | Indique comment sont gérées les simulations         | postgres                         |
| `SIMULATION_RUN`          | Indique comment est lancé le modèle R               | rscript                          |
| `SIMULATION_RSCRIPT_PATH` | Chemin du script R à lancer (obligatoire)           | chemin_absolut                   |
| `GEOCODER`                | Type de géocodeur                                   | galigeo                          |
| `GGO_REST_API_URL`        | URL du géocodeur Galigeo                            | GGO_REST_API_URL                 |
| `HERE_API_KEY`            | Clé here.com (obligatoire si le géocodeur est here) | XXXXXXX                          |
| `JWT_SECRET`              | clé de cryptage du token                            | 'unephrase.....'                 |
| `JWT_EXPIRESIN`           | Délai d'expiration du token                         | '10 days'                        |
| `JWT_TOKENNAME`           | Nom du token de sécurité                            | 'ggofnac_simulation_accessToken' |
| `POSTGRES_USER`           | Nom de l'utilisateur de la base PostGRES            | db_user                          |
| `POSTGRES_PASSWORD`       | Mot de l'utilisateur                                | db_user_passwor                  |
| `POSTGRES_DATABASE_NAME`  | Nom de la base de données                           | fnac                             |
| `POSTGRES_HOST`           | URL de la base de données                           | localhost                        |
| `POSTGRES_PORT`           | Port d'écoute de la base de données                 | 5432                             |

## Mise en place du modèle de données

Lancer la commande suivante:

    npm run setup_app

Cette commande va créer les tables suivantes dans le schéma `SCHEMA_TABLE`:

- `ggo_simulation`: table contenant les projets de simulation (avec un index `uniq_idx_id_simulation`)
- `ggo_simulation_zones`: table contenant pour une simulation, les zones de chalandises et les IRIS associés
- `ggo_simulation_concurrence`: table contenant la concurrence associée à la simulation en cours
- `ggo_simulation_cannibalisation`: table de la cannibalisation rempli par le modèle de simulation
- `ggo_simulation_job`: table contenant la liste des jobs de calcul de simulation

## Lancement de l'application

Sous Windows:

    run_simulation_app.bat

L'application sera lancé dans un terminal Windows et disponible dans un navigateur à l'adresse suivante où `PORT` est la valeur définit dans les variables d'environnement:

    http://localhost:PORT

## Création d'un service Windows

Se référer à l'exemple présenté sur ce site:

[Deploying a Node.js application on Windows IIS using a reverse proxy](https://alex.domenici.net/archive/deploying-a-node-js-application-on-windows-iis-using-a-reverse-proxy)