# Vagrant with Docker Kit

This mini project contains Docker images wrapped in a Vagrant environment. For installation you should perform the following steps:

1. Clone the repository and initialize the configuration file:
```bash
$ git clone https://github.com/mickgeek/vagrant-with-docker-kit.git
$ cd vagrant-with-docker-kit
$ git submodule init
$ git submodule update
```
2. Copy `.env.example` to `.env` and `docker/.env.example` to `.env`, then edit they if necessary:
```bash
$ cp .env.example .env
$ cp docker/.env.example docker/.env
```
3. Create and configure Vagrant machine:
```bash
$ vagrant up
```
4. Check correct work of the web server by address: http://app.local.
