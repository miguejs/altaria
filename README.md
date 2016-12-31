# Altaria
Personal site with  information about me, projects, and maybe posts build with phoenix as an API with [Furret](https://github.com/miguejs/furret) as front-end client

# Run Altaria using Docker

This guide will help you get started to setup the project using Docker, and keep using it as part of your development process.

## Table of contents
- [Installing Docker](#installing-docker)
- [Running Altaria](#running-Altaria)
- [Stop Altaria](#stop-Altaria)
- [Restoring DB](#restoring-db)
- [Debugging](#debugging)
- [Icalia Guides](#icalia-guides)

## Installing Docker

The first thing you need to do before start is to install `Docker`

[https://www.docker.com/products/docker](https://www.docker.com/products/docker)

If Docker was successfully installed, you should be good to go.

## Running Altaria

To run Altaria you can simple execute the following command inside the project directory:

```
% docker-compose up -d
```

That command will lift every service Altaria needs, such as the `rails server`, `postgres`.


It may take a while before you see anything, you can follow the logs of the containers with:

```
% docker-compose logs
```

Once you see an output like this:

```
altaria_1  | [info] Running App.Endpoint with Cowboy using http://localhost:4000
altaria_1  | 31 Dec 01:16:59 - info: compiling
altaria_1  | 31 Dec 01:17:00 - info: compiled 6 files into 2 files, copied 3 in 7.6 sec
altaria_1  | [info] GET /
altaria_1  | [debug] Processing by App.PageController.index/2
altaria_1  |   Parameters: %{}
altaria_1  |   Pipelines: [:browser]
altaria_1  | [info] Sent 200 in 276ms
altaria_1  | [error] backend port not found: :inotifywait
```

This means the project is up and running. It is worth mention that the `docker-compose` command will create the database if is not there yet.

## Stop Altaria

In order to stop Altaria as a whole you can run:

```
% docker-compose stop
```

This will stop every container, but if you need to stop one in particular, you can specify it like:

```
% docker-compose stop web
```

`web` is the service name located on the `docker-compose.yml` file, there you can see the services name and stop each of them if you need to.

## Restoring DB

You probably won't be working with a blank database, so once you are able to run Altaria you can restore the database, to do it, first stop all services:

```
% docker-compose stop
```

Then just lift up the `db` service:

```
% docker-compose up -d db
```

The next step is to login to the database container:

```
% docker exec -ti Altaria_db_1 bash
```

This will open up a bash session in to the database container.

Up to this point we just need to download or ask the project leader for a database dump and copy under `Altaria/db/dumps`, this directory is mounted on the container, so you will be able to restore it with:

```
root@a3f695b39869:/# bin/restoredb Altaria_dev db/dumps/<databaseDump>
```

If you want to see how this script works, you can find it under `bin/restoredb`

Once the script finishes its execution you can just exit the session from the container and lift the other services:

```
% docker-compose up -d
```

## Debugging

We know you love to use `debugger`, and who doesn't, that's why we put together a script to attach the `web` container service into your terminal session. 

What we mean by this, is that if you add a `debugger` or `binding.pry` on a part of the code, you can run:

```
% bin/attach web
```

This will display the logs from the rails app, as well as give you access to stop the execution on the debugging point as you would expect.

**Take note that if you kill this process you will kill the web service, and you will probably need to lift it up again.**

## Icalia Guides

Remember you can always rely on the Icalia Guides to a better development and
internal progamming style:

* [Rails Guides](https://github.com/IcaliaLabs/icalia_guides/tree/master/rails)
