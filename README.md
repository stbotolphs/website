# St Botolph's College CMS

This repository contains the code for St Botolph's College's CMS and Dockerfile
which can be used to package the CMS.

Environment variables which can be used to configure the container can be found
in the [settings module](botolphs/settings.py).

## Quickstart

> On OS X, some extra steps are required. See the "recipies" section below.

This section gives some guidance on how to spin up the CMS in development.
Docker and docker-compose must be installed. The application can then be started
via:

```console
$ docker-compose build --pull
$ docker-compose up
```

There is a convenience service which runs an initial database migration but an
initial superuser must be created manually. See the recipes section below.

Once started the following sites are available:

* http://localhost:8000/ - the site itself
* http://localhost:8001/ - an object store browser (credentials in
    [env/minio.env](env/minio.env))
* http://localhost:8002/ - an email inbox showing all sent mail
* http://localhost:8003/ - a web panel for the database (credentials in
    [env/db.env](env/db.env))

## Rebuilding

The docker-compose configuration *DOES NOT AUTO-RELOAD* when changes are made.
If you make changes to the CMS code you will need to restart as follows:

```console
$ docker-compose down  # Or Ctrl-C from the docker-compose up command
$ docker-compose up --build
```

## Testing

In development, you can test the following functionality:

* Adding an image to a page should succeed and the image should be stored and
    served from Object Storage. You can check this by visiting
    http://localhost:8001/minio/botolphs-cms/filer_public/.
* Adding a form to a page should succeed and should be able to send email. You
    can check this by visiting http://localhost:8002/ which provides a "fake
    inbox" showing all email sent by the site.

## Recipes

The following section provides some recipes and advice for common operations.

### Creating an initial admin user

See "Running management commands" below.

### Accessing the object store

We use minio to provide an S3-compatible object store for development. IT
Once the application is running, a browser is available at
http://localhost:8001/. Credentials can be found in [minio.env](env/minio.env).

### Accessing sent email

We use mailhog to run a development SMTP server and UI for viewing sent emails.
Once the application is running, the inbox can be accessed at
http://localhost:8002.

### Running management commands

Once the compose stack is up and running, management commands may be run in the
following way:

```console
$ docker-compose exec webapp ./manage.py [...]
```

For example, to create a new superuser:

```console
$ docker-compose exec webapp ./manage.py createsuperuser
```

### Running under OS X

The docker-compose configuration makes use of host networking which [has issues
under OS X](https://github.com/docker/for-mac/issues/1031). A work around is to
use [docker-machine](https://docs.docker.com/machine/get-started/). If
docker-machine is used, the S3 endpoint URL in [env/webapp.env](env/webapp.env)
needs to be modified to have the docker machine node's IP address in place of
localhost.
