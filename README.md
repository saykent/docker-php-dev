## A PHP application development environment with Docker
This is a repository for docker files to build a PHP application development environment. Anyone can use this as a template for building a docker image.

The docker compose file includes:
- PHP 7.4 with Xdebug 3.0.2
- Apache 2.4.x(on Ubuntu 20.04)
- MySQL 8.0

## phpdev: a shell script
phpdev is a shell script that make it easy to bring up and down the docker image. 

Also, the script can be used to:
- Bring up/down the containers including any docker-compose commands
- Run compose
- Log into the application server's shell which is a bash shell on Ubuntu 20.04

## How to use it
In most cases you need to customize some files for your own development environment.
For anyone who wants to look what this is like:
1. Download the repository's zip file or do `git clone` of the repository.
2. Run `./phpdev up`. This will build a docker image if it's the first time running the container.
3. Visit `http://localhost`
