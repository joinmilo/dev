# wooportal.dev

All services started from a single repository

_Keep in mind this repository is for local development only and is not meant to be deployed on any production environment!_

## Requirements

1. [Docker](https://docs.docker.com/install/)
2. [Docker Compose](https://docs.docker.com/compose/install/)

## How to run it?

1. Clone the repository:

```
git clone https://github.com/wooportal/dev --recursive wooportal.dev
```

2. Shared folders:
   We are using shared folders to enable live code reloading. Without this, Docker Compose will not start: - Windows/MacOS: Add the cloned `wooportal.dev` directory to Docker shared directories (Preferences -> Resources -> File sharing). - Windows/MacOS: Make sure that in Docker preferences you have dedicated at least 5 GB of memory (Preferences -> Resources -> Advanced). - Linux: No action required, sharing already enabled and memory for Docker engine is not limited.

3. Go to the cloned directory:

```
cd wooportal.dev
```

4. Copy the `.env` file from the example :

```
cp .env.example .env
```

5. Ask for WOOPORTAL_LOCATION_KEY or create your own Bing Maps Key (see [Bing Maps Integration](https://www.bingmapsportal.com/))

6. Ask for firebase-credentials.json or create your own Firebase credential file (see [Firebase Docs](https://firebase.google.com/docs/cloud-messaging/android/client))

7. Copy firebase-credentials.json to server:

```
cp  /path/to/your/firebase-credentials.json server/src/main/resources/credentials/firebase-credentials.json
```

7. Copy media files to server storage

```
mkdir server/.storage
```

```
cp -a media/. server/.storage
```

8. In order to have a local db running, ask for SQL dump and place it into /db 

10. Build the stack:

```
docker-compose build --no-cache
```

11. Run the application:

```
docker-compose up
```

In case of server development you should run the server within the IDE and not within docker compose. In that case you can use the scale flag, e.g.:

```
docker-compose up --scale server=0
```

## Tools and URLs

After building and running the application following URLs are exposed:

- Client: http://localhost:8010
- Server: http://localhost:8011
- Database: http://localhost:8012
- Mail: http://localhost:8014

### GraphQL Playground

For testing the API you can reach the playground here:
http://localhost:8011/gui

### Media content

In order to retrieve any media content there are the following endpoints to retrieve them:

- http://localhost:8011/api/media/{id}: {id} should be replaced by the media id. This endpoint retrieves the item without downloading it immediately
- http://localhost:8011/api/media/download/{id}: {id} should be replaced by the media id. This endpoint retrieves the data and has header set to download it automatically.

## Specification and data model

The folder `specs` contains the datamodel and is a [Modelio](https://github.com/ModelioOpenSource/Modelio) project.

To open the project open Modelio and switch the workspace to `specs`.

## How to update the subprojects to the newest versions?

This repository contains newest stable versions.
When new release appear, pull new version of this repository.
In order to update all of them to their newest versions, run:

```
git submodule update --remote
```

During the development there might be changes in existing changelog files. Therefore the whole database need to be purged. To setup a clean database you have to purge the volume before starting the docker-comppose:

```
docker-compose down -v
```
