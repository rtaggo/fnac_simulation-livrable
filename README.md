# fnac-simulation

# Galigeo Expérience

[GaligeoEnterprise_G21_SP3-eXperience.zip](http://download.galigeo.com/Fnac/GaligeoEnterprise_G21_SP3-eXperience.zip)

[GaligeoEnterprise_G21_SP3-eXperience.zip.MD5](http://download.galigeo.com/Fnac/GaligeoEnterprise_G21_SP3-eXperience.zip.MD5)

# Application de Simulation
## Pré requis

1.  [Node.js](https://nodejs.org/en/) pour la dernière version
    - [Anciennes Versions](https://nodejs.org/en/download/releases/)
1.  Postgres version 9.4 minium

## Installation

1.  Télécharger l'application

[fnac-simulation.zip](https://github.com/rtaggo/fnac_simulation-livrable/raw/main/fnac_simulation.zip)

1.  Décompresser le fichier `fnac_simulation.zip` et aller dans le répertoire `fnac_simulation`:

        cd fnac_simulation

### Variables d'environnement

Afin de faire fonctionner l'application, il est nécessaire que des variables d'environnement soient définies.

Renseigner les valeurs dans le fichier .env

```yaml
NODE_ENV=production
PORT=5000
HTTPS_PORT=8443
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
POSTGRES_DATABASE_NAME=fnac
POSTGRES_HOST=localhost
POSTGRES_PORT=5432
MYTRAFFIC=
MYTRAFFIC_API_KEY=
MYTRAFFIC_MODE=
```

| Variables                 | Explication                                         | Exemple (Production)             |
| ------------------------- | --------------------------------------------------- | -------------------------------- |
| `NODE_ENV`                | Environnement d'exécution                           | production                       |
| `PORT`                    | Port de l'application web                           | 5000                             |
| `HTTPS_PORT`              | Port pour l'HTTPS                                   | 8443                             |
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
| `LOGS_PATH`               | Chemin absolute des logs (par défault: logs)        | c:\logs                          |
| `MYTRAFFIC`               | Activer ou non MyTraffic (consommation de crédit)   | enabled                          |
| `MYTRAFFIC_API_KEY`       | Clé API My Traffic (cf Galigeo)                     | XXXXXXX                          |
| `MYTRAFFIC_MODE`          | mode de donnée my traffic                           | production                       |

### Mise en place du modèle de données

Lancer le script suivant:

    run_install.bat

Cette commande va créer les tables suivantes dans le schéma `SCHEMA_TABLE`:

- `ggo_simulation`: table contenant les projets de simulation (avec un index `uniq_idx_id_simulation`)
- `ggo_simulation_zones`: table contenant pour une simulation, les zones de chalandises et les IRIS associés
- `ggo_simulation_concurrence`: table contenant la concurrence associée à la simulation en cours
- `ggo_simulation_cannibalisation`: table de la cannibalisation rempli par le modèle de simulation
- `ggo_simulation_job`: table contenant la liste des jobs de calcul de simulation

Elle teste aussi l'existence des tables socio-démographiques.

Un résumé est affiché à la fin du processus.

## Lancement de l'application

Lancer le script:

        run_simulation_app.bat

## Création d'un service Windows

> :warning: cette partie est à titre indicative et Galigeo n'assure aucun support.

Il est possible de créer un service windows base sur le script `run_simulation_app.bat`.

Se référer à l'exemple présenté sur ce site:

[Deploying a Node.js application on Windows IIS using a reverse proxy](https://alex.domenici.net/archive/deploying-a-node-js-application-on-windows-iis-using-a-reverse-proxy)
