# DRLS Suite on Docker

## Version 0.2-alpha1

The goal of version 0.2 is to make it as easy as possible for anyone to start using/testing the DRLS Suite.

Currently, on the `master` branch, only the `keycloak` component is available.

We have tested this setup on
* Mac OS X 10.5 (Catalina), 10.14 (Mojave)

We are in the process of testing the following
* Windows 10 Home running WSL2 in VirtualBox
* Ubuntu 20.04

Use this suite for trying or testing the DRLS Suite and to report bugs to the development team. Once everything is ready, it will be moved to the official repository.

## Known bugs

- The following components are not yet available when you `docker-compose up`:
  1.  `crd`
  2.  `test-ehr`
  3.  `crd-request-generator`
  4.  `dtr`

- Windows 10

  - when running Windows 10 in VirtualBox on a Mac OS X machine, we are unable to run this suite due to VT-x issues in Docker for Windows. This is a [known incompatibility](https://docs.docker.com/docker-for-windows/install/#what-to-know-before-you-install) issue between VirtualBox and Docker for Windows and cannot be fixed.

We are in the process of resolving these problems. In the meantime, follow the instructions below in [Getting Started](#getting_started) whch contains workarounds.

## Getting Started

### Pre-Requisites

You need to have the following installed:

- Docker — [follow these instructions](http://docker.io) to install Docker for your OS.
- git — [follow these instructions](https://git-scm.com/book/en/v2/Getting-Started-Installing-Git) to install `git` for your OS

### Settin Up

The intent is that a single `docker-compose up` will bring up the entire DRLS suite. However, in the current version, you will need to install additional developer tools.

1. Clone this repository. It contains all the configurations you need to run the suite.

   1. Create a "DRLS base directory"; we will assume it is `~/drls` in the instructions below.

      `git clone https://github.com/hkong2/drls-docker ~/drls`

2. Start up the DRLS Suite.

   `docker-compose up`

   This will start up `keycloak` in the suite and will take about 2-3 minutes plus downloading the docker images if this is the first time you are running, or if there is an updated version.

   The remaining components will need to be run locally, using the directions that follow.

   You will see the console output from all the services mixed together in the console.

   Once the output for the init sequence for each app completes you will be able to reach these services via a browser:

| URL                            | `service_name`          | Description                                                           |
| ------------------------------ | ----------------------- | --------------------------------------------------------------------- |
| http://localhost:8180          | `keycloak`              | [Identity and Access Management](https://www.keycloak.org/about.html) |

- To see the logs of an individual service, run `docker-compose logs <service_name>` in a separate console

### Configurations and Customizations

The easiest way to run the DRLS Suite is to get the docker images from docker hub and orchestrate the images using the `docker-compose.yml` file included in the root project. Along with the `docker-compose.yml` file is a `.env` file. (Note this file may be invisible in your file system, you will need to do a `ls -a` to see it.) The `.env` file contains all the environment variables that you can change to customize the suite.

These 2 files provide the defaults for running the DRLS suite for a local development machine. If you choose, you can customize the suite in the `.env` file, or append the configuration before the command line, (e.g., `DRLS_NAMESPACE=XYZ docker-compose up`, where `XYZ` is your custom namespace)

The versions on docker hub follows the following conventions

- `alpha1` - most recent `0.2-alpha1` series (see [Roadmap](#roadmap))

## Stopping the DRLS Suite

To stop the entire DRLS suite and remove the stopped containers

1. in a separate console, run `docker-compose down`
2. in the console you used to run `test-ehr`, type `Ctrl-C`
   1. it may take a few seconds for the container to stop
   2. once you get the console prompt, the server has stopped
3. in the console you used to run `dtr`, type `Ctrl-C`
   1. it may take a few seconds for the container to stop
   2. once you get the console prompt, the server has stopped

## Diagnostics

## Information for Developers

### Docker-Compose Commands

In general, running `docker-compose <command>` will run the `<command>` and show its output while running the DRLS suite. For example,

- `docker-compose logs <service_name>` will show the `stdout` and `stderr` for the specified `<service_name>`
- `docker-compose ps` will show all containers currently running in the DRLS suite. (You can also use `docker ps` which will show all containers running in your system. And of course, running `ps`, will show all processes running in your system.)
- `docker-compose top` will show all running threads associated the DRLS suite
