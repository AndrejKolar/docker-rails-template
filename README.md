# docker-rails-template
Template for creating Docker containers for Ruby on Rails

Template is from the docker-compose [tutorial](https://docs.docker.com/compose/rails/).
Creates two docker containers, one for the app and one for the postresql database.

## Setup Docker

Install [VirtualBox](https://www.virtualbox.org)
Install [Docker](https://docs.docker.com/installation/mac/) and other tools


Init and start
```bash
$ docker-machine create --driver virtualbox default
```

Setup the docker host enviroment variable (ip address is from `docker-machine env default`)
```bash
$ export DOCKER_HOST=tcp://192.168.59.103:2375
```
Connect the shell to the default `default` machine
```bash
$ eval "$(docker-machine env default)"
```

Port forwarding should be setup in VirtualBox for the Docker image



## Build

Run
```bash
$ docker-compose run web rails new . --force --database=postgresql --skip-bundle
```

Uncomment line in the new `Gemfile` to setup the js framework
```bash
gem 'therubyracer', platforms: :ruby
```

Rebuild the project
```bash
$ docker-compose build
```

To connect the database replace the `database.yml` with:
```
development: &default
  adapter: postgresql
  encoding: unicode
  database: postgres
  pool: 5
  username: postgres
  password:
  host: db

test:
  <<: *default
  database: myapp_test
```

Boot the app:
```bash
docker-compose up
```

Create the database (in another terminal):
```bash
$ docker-compose run web rake db:create
```
