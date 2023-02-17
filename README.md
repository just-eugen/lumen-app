# Install Lumen with Composer

First, make sure you have Composer installed by running the following command in your terminal:

- ```composer -v```
- or
- ```composer --version```

Install Lumen by running:

- ```composer create-project --prefer-dist laravel/lumen lumen-app```

# Dockerfile

Create a file named Dockerfile in your project's root directory:

```
FROM php:8.2.3-fpm-alpine

WORKDIR /var/www/html/

RUN php -r "readfile('http://getcomposer.org/installer');" | php -- --install-dir=/usr/bin/ --filename=composer

COPY . .

RUN composer install
```

# docker-compose.yml

Now, create a docker-compose.yml file in your project’s root directory: 

```
version: '3.5'

services:
  lumen:
    ports:
      - "8000:8000"
    volumes:
      - .:/var/www/html
      - /var/www/html/vendor/
    build: .
    command: php -S lumen:8000 -t public
    restart: always
```

Before building and running our Lumen project on Docker, it is useful to check that you have your machine’s 8000 port free for use. You can check that by running:

```docker ps```

If one of your docker containers is listening on 8000, stop/kill that container or modify the port configs in you docker-compose.yml file.

You can now remove the /vendor/ folder from your project’s directory and run:

```docker-compose up --build```

Now, you can visit localhost:8000 and start creating an amazing API/microservice!
