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
 ```ssh -i "keypair" ec2-user@Ip```

- where keypair is the name of your keypair, if you are running an ubuntu image, replace ec2-user with ubuntu, then your Ip.

### STEP 3 - Install docker
- Update the Amazon Linux 2 package index:

```sudo amazon-linux-extras update```

- Install the Docker package:

```sudo amazon-linux-extras install docker```

- Start the Docker service:

```sudo service docker start```

### STEP 4 - Clone the laravel repo
Run the command below to clone the laravel repo
```
git clone https://github.com/f1amy/laravel-realworld-example-app.git
cd laravel-realworld-example-app
```

## Now Lets get to work
### STEP 5:
We are going to create a folder named docker. Use the command below
```mkdir docker
cd docker
# create a dockerfile
touch Dockerfile
# create a file named entrypoint.sh. you can name it anything you like, but note that we will be referencing it in our dockerfile
touch entrypoint.sh
# You can go out of the folder, we are going to come back to it
cd ..
```

#### STEP 6:
Now lets delete the content of doccker-compose.yml file that came with the app and write ours.
We are going to start by building the database:
Add the following code to your docker-compose.yml file:
```
version '3.9' 
services:  
  mysql:
    image: mysql:8.0
    ports:
      - 3306:3306
    environment:
      - MYSQL_DATABASE=${DB_DATABASE}
      - MYSQL_USER=${DB_USERNAME}
      - MYSQL_PASSWORD=${DB_PASSWORD}
      - MYSQL_ROOT_PASSWORD=${DB_PASSWORD}
    volumes:
      - db-data:/var/lib/mysql
  redis:
    image: redis:alpine
    command: redis-server --appendonly yes --requirepass "${REDIS_PASSWORD}" 
    ports:
      - 6379:6379
 volumes:
   db-data: ~
```
- The <b>version</b> in a docker-compose.yml file is important because it specifies the version of the Docker Compose file format being used. This version determines the syntax and features that are available to you in the file. Different versions of Docker Compose have different capabilities and requirements, so specifying the correct version ensures that your docker-compose.yml file will work as expected with the version of Docker Compose that you are using. Additionally, using a specific version can help ensure compatibility and predictable behavior if you share your docker-compose.yml file with others who may be using different versions of Docker Compose.
- The <b>services</b> section in a docker-compose.yml file is important because it defines the individual components of your application and how they should be run as containers. Each service represents a separate container that runs a specific application or component of your application. By defining these services, you can control how each component is built, how it connects to other components, and how it is deployed and run.Having clear definitions for each service in your docker-compose.yml file makes it easier to manage and scale your application, as you can make changes to individual services without affecting the rest of your application.  
- The <b>images</b> under a service in a docker-compose.yml file specifies the Docker image that the service will use to build its container. The image acts as a blueprint for the container, providing all the necessary components, libraries, and configurations that the service needs to run.
- The <b>ports</b> configuration in a docker-compose.yml file is used to specify how a service's container should be exposed to the host and other containers. It maps ports within the container to ports on the host, allowing the container to receive incoming traffic on the specified port.
- The <b>environment</b> configuration in a docker-compose.yml file is used to specify environment variables for a service's container. Environment variables provide a way to pass configuration values to a container at runtime, allowing you to customize the behavior of a service without having to rebuild the image. Here, we are referencing our .env file variables.
- The volumes configuration in a docker-compose.yml file is used to specify how data and files should be stored and managed between containers and the host. Volumes provide a way to persist data outside of the container's file system, allowing data to persist even if the container is deleted or recreated.

Save the code above and lets build our database. Use the command below to build:
```
docker-compose up --build -d
```
Note: -d will run the command in a detached mode. You should see something like this when you run you command:

![mysql-redis](https://user-images.githubusercontent.com/99274632/217493408-e5c2a719-2366-44b5-a66f-73d55efd5d71.PNG)

Now lets check if we can see the containers. Use the command below to check for your containers
```
docker-compose ps
```
You should see something like this

![running](https://user-images.githubusercontent.com/99274632/217494400-cac33d87-c2c7-471f-a736-6ee8a151bf39.PNG)


# We have successfully built our database and redis. Lets install php(Note laravel is a php web framework so php is required)

### Add the following code to the top of your docker-compose file, after services. it should come before mysql

```
version: '3.9'
services:
  php:
    build:
      context: ./docker
      target: php
      args:
        - APP_ENV=${APP_ENV}
    environment:
      - APP_ENV=${APP_ENV}
      - CONTAINER_ROLE=myapp
    working_dir: /var/www
    volumes:
      - ./:/var/www
    ports:
      - 8000:8000
    depends_on:
      - mysql
      - redis
  mysql:
    image: mysql:8.0
    ports:
      - 3306:3306
    environment:
      - MYSQL_DATABASE=${DB_DATABASE}
      - MYSQL_USER=${DB_USERNAME}
      - MYSQL_PASSWORD=${DB_PASSWORD}
      - MYSQL_ROOT_PASSWORD=${DB_PASSWORD}
    volumes:
      - db-data:/var/lib/mysql
  redis:
    image: redis:alpine
    command: redis-server --appendonly yes --requirepass "${REDIS_PASSWORD}" 
    ports:
      - 6379:6379
volumes:
  db-data: ~
  
```
In the above code, we add php service to our docker-compose file. Let me explain:
The first line, "version: '3.9'" specifies the version of the Docker Compose file format. Note; only one version is required in a docker-compose file.
The "services" key refers to the list of services that will be run with Docker Compose. In this case, "php".
The "build" key within the "php" service defines the build context, target and arguments for building the Docker image for this service.
- The "context" key sets the build context to the "./docker" directory.(Remember the docker directory we created. We will be building the content of the Dockerfile which we will be writing shortly.
- The "target" key sets the target to "php".
- The "args" key sets an argument for the build process, with the argument being an environment variable named "APP_ENV" that is set to the value of the environment variable ${APP_ENV}. The APP_ENV is found in your .env file.You might want to change it from local to your IP address.
The working_dir is the directory where all command line operations will be executed from, by default.

### Lets write content in the Dockerfile
``` cd docker
vi Dockerfile```

Enter the code below, I will explain shortly
```
FROM php:8.1 as php

RUN apt-get update -y
RUN apt-get install -y unzip libpq-dev libcurl4-gnutls-dev
RUN docker-php-ext-install pdo pdo_mysql bcmath

RUN pecl install -o -f redis \
    && rm -rf /tmp/pear \
    && docker-php-ext-enable redis

WORKDIR /var/www

COPY . .

COPY --from=composer:2.3.5 /usr/bin/composer /usr/bin/composer

ENV PORT=8000
ENTRYPOINT [ "docker/entrypoint.sh" ]

```
## Explanation
This Dockerfile above builds a PHP image using the official PHP 8.1 base image. It then performs several operations:
    Updates the package index using apt-get update -y.
    Installs the packages unzip, libpq-dev and libcurl4-gnutls-dev using apt-get install -y.
    Installs the PHP extensions pdo, pdo_mysql and bcmath using docker-php-ext-install.
    Installs the Redis extension using pecl and enables it with docker-php-ext-enable.
    Sets the working directory to /var/www.
    Copies the files and directories from the build context to the working directory.
    Copies the Composer binary from the composer:2.3.5 image to /usr/bin/composer.
    Sets the PORT environment variable to 8000.
    Sets the entry point to docker/entrypoint.sh.
Remember we already referenced this file to be built under php service in our docker-compose file. Now lets write contents in our entrypoint.sh. This file will contain scripts to install composer and its dependencies and you can add any other code here. Add the following code to your docker/entrypoint.sh file.

```
#!/bin/bash
if [ ! -f "vendor/autoload.php" ]; then
composer install --no-progress --no-interaction
fi
if [ ! -f ".env" ]; then
	echo "Creating env file for env $APP_ENV"
	cp .env.example .env
else
	echo "env file exists"
fi

php artisan migrate
php artisan key:generate
php artisan config:clear
php artisan route:clear

php artisan serve --port=$PORT --host=0.0.0.0 --env=.env
exec docker-php-entrypoint "$@"

```
The above shell script is used to setup and run a Laravel PHP web application. The script checks if the vendor directory, which contains the Composer autoloader, exists. If it does not exist, it runs the composer install command to install the required dependencies. It then checks if the .env file exists. If it does not, it creates the .env file by copying the .env.example file.

The script then runs several Artisan commands to set up the Laravel application: <b>migrate</b> to run database migrations, <b>key:generate</b> to generate an encryption key, <b>config:clear</b> to clear the configuration cache, and <b>route:clear</b> to clear the route cache.

Finally, the script runs <b>php artisan serve</b> to start the PHP web server, listening on the port specified by the $PORT environment variable, and accessible from any IP address (0.0.0.0). The <b>exec</b> command is then used to run the docker-php-entrypoint script with the command line arguments passed to the script ("$@").
## Before running the command to build, Lets edit out routes/index.php file.
`vi routes/index.php` Uncomment the Routes block.

## Save and lets build and run our container with the command below
`docker-compose down && docker-compose up -d`

The above Docker Compose command stops and removes the containers, networks, and volumes created by the docker-compose up command and then starts the containers in the background. The docker-compose down command stops and removes the containers and any resources created by docker-compose up. The -d option for docker-compose up starts the containers in the background, so the logs are not displayed in the terminal.
If sucessful, you will see something like this(this, you will if you ran the above command without -d):
![without-d](https://user-images.githubusercontent.com/99274632/217508471-f67ae3d5-7a3f-4c4b-bd1f-457a20b5cc2d.PNG)

After building, you can check if all your containers are running by running `docker-compose ps`
You wil see something like this:

![containers](https://user-images.githubusercontent.com/99274632/217508946-18a17d43-a155-4c7e-970e-5dfd68a98e9f.PNG)

<strong>Now, to be able to view and access your application using your IP and mapped port, You will have to add the port 8000 to your instance security group, after which you can view on your browser by typing `Ipaddress:8000`. Replace Ipaddress with your public Ip. You should the page below:</strong>


![lrvel](https://user-images.githubusercontent.com/99274632/217511182-2d0b96db-0c72-4252-9075-27e5638fe5e9.PNG)

Api testing below:

![populated](https://user-images.githubusercontent.com/99274632/217511196-e7982cc3-0559-4e98-8523-455987a7bb6a.PNG)

There's more you can do with the application. You can even add a node service on your compose file. But this will be all for now.

# Thank you for reading!

Check out this Link for more explanation [Reference](https://www.youtube.com/watch?v=WahJ91Nrgn0)

## License

The MIT License (MIT). Please see [`LICENSE`](./LICENSE) for more information.
