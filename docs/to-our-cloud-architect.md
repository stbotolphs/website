# Instructions to Cloud Architect

Hello, We're the Computer Officers for St Botolph's College, Cambridge. We've
decided to re-architecture our website CMS as a Cloud Native application and
we're hoping you'd help us.

This repository contains our new CMS. It's a custom web application packaged as
a Docker container. We've tried hard to make the container stateless. For
example, the web application user cannot write to the application directory in
the container so we know that no state is being persisted on the filesystem.

We've put together a [testing document](./testing.md) which outlines the
functionality we need to have in the deployed site. If you can make a page like
the one shown in that document, you should be exercising all the functionality
we care about.

## What we've given you

The [README](../README.md) is aimed at developers but it should allow you to get
up and running with the application on your own machine. Most importantly, you
should be able to replicate the page in the [testing document](./testing.md) on
your own machine and verify the required functionality.

We've tried to structure the application in a Cloud Native way so we use
docker-compose when developing the site to mirror a container-based deployment.
We set up a local PostgreSQL database, S3 compatible Object Store and SMTP
server which the application uses so it should be fairly easy to swap them out
for ones deployed in the Cloud.

There's also a Dockerfile which we think should be suitable for production use
or, at least, only require some light modification.

## What we need from you

We'd like an *automated* way to deploy our application to the Cloud. We're
neutral on which cloud provider to use but we expect to use the following
services at a minimum:

* A hosted PostgreSQL database
* Object Storage
* Email sending
* Container hosting

They need not necessarily be all from the same provider but we would prefer that
as much of the deployment as possible is automated.

We already use [terraform](https://www.terraform.io/) here to deploy some Cloud
services so having an automated deployment based around that would be great.

## How we expect to get deliverables

Please open a pull request on this repository. We'd prefer additional
code/configuration to mostly be in a `deploy` directory in the repository
root but feel free to modify the application if necessary. Some files will
probably have to be in the repository root.

**Do not provide any secrets in the pull request.** If secrets are required, and
we could not regenerate them ourselves, please GPG encrypt them using our
[public
keys](https://guidebook.devops.uis.cam.ac.uk/en/latest/contact/public-keys/) and
email them to us.

## Minimal deliverables

We'd expect the following as minimal deliverables:

* Configuration for a tool such as [terraform](https://www.terraform.io/) which
    allows us to deploy one instance of our application in way which is entirely
    or mostly automated. Where deployment steps are not automated, clear
    instructions are needed along with a brief justification as to why that step
    cannot be automated.
* Documentation telling us how to deploy to the Cloud using your deployment
    configuration.
* The deployed application supports image uploads and creating forms which send
    email.
* The ability to deploy updates to the application.
* Documentation telling us how to deploy updates.

## Additional deliverables

We'd like the following as additional deliverables which should be provided if
possible:

* Either all secrets are generated automatically at deployment time or, if
    external secrets are required, a method by which we can generate them
    ourselves.
* The ability to deploy multiple instances of the application. For example, a
    test instance or a "next" instance to show future developments to
    stakeholders in the College. Each instance should have it's own database,
    object store, etc.
* Configuration for a CI/CD tool which will automatically deploy updates when
    they are committed to the `master` branch along with instructions on how we
    should set up the CI/CD tool.
* Automatic configuration of monitoring and alerting. At a minimum, automated
    alerting if the site is unreachable.
* An evaluation of the "vendor lock-in" of the deployment. If we wanted to
    change Cloud providers which services are likely to have no analogue from
    another provider.
