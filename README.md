# ![Laravel RealWorld Example App](.github/readme/logo.png)

[![RealWorld: Backend](https://img.shields.io/badge/RealWorld-Backend-blueviolet.svg)](https://github.com/gothinkster/realworld)
[![Tests: status](https://github.com/f1amy/laravel-realworld-example-app/actions/workflows/tests.yml/badge.svg)](https://github.com/f1amy/laravel-realworld-example-app/actions/workflows/tests.yml)
[![Coverage: percent](https://codecov.io/gh/f1amy/laravel-realworld-example-app/branch/main/graph/badge.svg)](https://codecov.io/gh/f1amy/laravel-realworld-example-app)
[![Static Analysis: status](https://github.com/f1amy/laravel-realworld-example-app/actions/workflows/static-analysis.yml/badge.svg)](https://github.com/f1amy/laravel-realworld-example-app/actions/workflows/static-analysis.yml)
[![License: MIT](https://img.shields.io/badge/License-MIT-yellowgreen.svg)](https://opensource.org/licenses/MIT)

> Example of a PHP-based Laravel application containing real world examples (CRUD, auth, advanced patterns, etc) that adheres to the [RealWorld](https://github.com/gothinkster/realworld) API spec.

This codebase was created to demonstrate a backend application built with [Laravel framework](https://laravel.com/) including RESTful services, CRUD operations, authentication, routing, pagination, and more.

We've gone to great lengths to adhere to the **Laravel framework** community style guides & best practices.

For more information on how to this works with other frontends/backends, head over to the [RealWorld](https://github.com/gothinkster/realworld) repo.

## How it works

The API is built with [Laravel](https://laravel.com/), making the most of the framework's features out-of-the-box.

The application is using a custom JWT auth implementation: [`app/Jwt`](./app/Jwt).

## PROJECT DETAILS
- Write a docker file and docker compose file for the services involved in laravel
- Mount a volume to write
- a. A realtime log entry
- b. mysql data

## Requirements
- Laravel app repo( you can fork it or clone it here https://github.com/f1amy/laravel-realworld-example-app.git)
- A server with Docker installed
-

## Steps to Dockerizing the app
The repository comes with a docker-compose.yml file but we will be writing our own dockerfile and docker-compose.yml file.

### STEP 1 - Create an Ec2 instance/server
Follow the steps on [create ec2 instance](https://docs.aws.amazon.com/AWSEC2/latest/UserGuide/EC2_GetStarted.html)n to create an instance. Meanwhile you can also run docker on your desktop or using wsl ubuntu. Here I will be using an ec2 server which I have running already.

### STEP 2 - ssh into your server(I am using amazon linux 2 server)
Use the command below to ssh into your server
> ssh -i "keypair" ec2-user@Ip

- where keypair is the name of your keypair, if you are running an ubuntu image, replace ec2-user with ubuntu, then your Ip.

### STEP 3 - Install docker
- Update the Amazon Linux 2 package index:

> sudo amazon-linux-extras update

- Install the Docker package:

> sudo amazon-linux-extras install docker

- Start the Docker service:

> sudo service docker start

### STEP 4 - Clone the laravel repo
Run the command below to clone the laravel repo
> git clone https://github.com/f1amy/laravel-realworld-example-app.git
> cd laravel-realworld-example-app

## Now Lets get to work
### STEP 5:
We are going to create a folder named docker. Use the command below
> mkdir docker
> cd docker
#### create a dockerfile
> touch Dockerfile
#### create a file named entrypoint.sh. you can name it anything you like, but note that we will be referencing it in our dockerfile
> touch entrypoint.sh
#### You can go out of the folder, we are going to come back to it
> cd ..

#### STEP 6:
Now lets delete the content of doccker-compose.yml file that came with the app and write ours.
We are going to start by building the database:
Add the following code to your docker-compose.yml file:
    












### Installation

Clone the repository and change directory:

    git clone https://github.com/f1amy/laravel-realworld-example-app.git
    cd laravel-realworld-example-app



### Run PHPStan static analysis

    sail php ./vendor/bin/phpstan

### OpenAPI specification (not ready yet)

Swagger UI will be live at [http://localhost:3000/api/documentation](http://localhost:3000/api/documentation).

For now, please visit the specification [here](https://github.com/gothinkster/realworld/tree/main/api).

## Contributions

Feedback, suggestions, and improvements are welcome, feel free to contribute.

## License

The MIT License (MIT). Please see [`LICENSE`](./LICENSE) for more information.
