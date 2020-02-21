Symfony docker compose environment
==========================

Docker compose configuration for running Symfony 4 (PHP 7.2 / Mysql 5.7 / Nginx 14.0).

You can using it with nginx reverse proxy with https certifactes and etc.  
https://github.com/tim96/Reverse-proxy

# How to run

1. Clone the repository
    ```bash
    git clone https://github.com/tim96/docker-symfony.git tim96-docker-symfony
    ``` 

   After call this command, folder `tim96-docker-symfony` was created.

2. Check that docker/docker-commands already installed using next commands
    ```bash
    docker --version # docker -v
    # Output example: Docker version 18.06.1-ce, build e68fc7a

    docker-compose --version # docker-compose -v
    # Output example: docker-compose version 1.22.0, build f46880fe
    ```

    About Docker Community Edition (CE)  
    https://docs.docker.com/install/#server

    Articles how to install Docker Compose:  
    https://docs.docker.com/compose/install/

3. For configure path for symfony app using `SYMFONY_APP_PATH` environment variable. By default:  
   ```
   SYMFONY_APP_PATH=../symfony
   ```
   If you have Symfony project just set path for symfony folder here.
   If you don't have vendor folder for your project you can install it without running locally composer and php:
   Sometimes dependencies or Composer scripts require the availability of certain PHP extensions. 
   You can work around this by add `--ignore-platform-reqs --no-scripts`
   ```
   # Linux/MacOS
   docker run --rm -v $(pwd):/app composer install -vvv --ignore-platform-reqs --no-scripts
    
   # Windows Powershell:
   docker run --rm -v ${PWD}:/app --name symfony-image composer install -vvv --ignore-platform-reqs --no-scripts
    
   # Windows Terminal (cmd):
   docker run --rm -v %cd%:/app --name symfony-image composer  install -vvv --ignore-platform-reqs --no-scripts
   ```
   
   If you want to start a new project using Symfony 4, just call next command (Select one for your platform).
   You don't need locally composer or php to install new Symfony project.
   ```
   # Linux/MacOS
   docker run --rm -v $(pwd):/app --name symfony-image composer create-project symfony/website-skeleton symfony
    
   # Windows Powershell:
   docker run --rm -v ${PWD}:/app --name symfony-image composer create-project symfony/website-skeleton symfony
    
   # Windows Terminal (cmd):
   docker run --rm -v %cd%:/app --name symfony-image composer create-project symfony/website-skeleton symfony
   ```
   
   By default we are using `symfony/website-skeleton` 

4. After previous steps you should have two folders  
   ```
   tim96-docker-symfony
   symfony
   ``` 
   
5. Go to `tim96-docker-symfony` folder
   Need to configure `.env` file from `.env.dist` to set up environment parameters
   
6. Run `docker-compose build` for build everything

7. Run `docker-compose up` for run application or `docker-compose up -d` to run containers in background

8. Run `docker-compose` to check that all containers were started.

9. Open `localhost:8081`. By default we are using `NGINX_PORT=8081`. If you need to change port, just change value in `.env` file
