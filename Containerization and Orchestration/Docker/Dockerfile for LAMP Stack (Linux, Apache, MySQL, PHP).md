LAMP Stack Dockerfile

This Dockerfile sets up a LAMP stack in a Docker container, making it ideal for running PHP web applications. It uses the official PHP image with Apache and installs MySQL extensions, ensuring effortless connections to MySQL databases. Simply add your PHP code to the src directory, build, and run the container to quickly deploy your application.

Features

PHP 7.4 with Apache: Uses the official PHP 7.4 image equipped with Apache.
MySQL Extensions Installed: Includes mysqli, pdo, and pdo_mysql extensions for PHP, facilitating easy connections to MySQL databases.
Quick Deployment: Add your PHP code to the src directory and deploy your application with ease.

Dockerfile
FROM php:7.4-apache
RUN docker-php-ext-install mysqli pdo pdo_mysql
COPY src/ /var/www/html/
EXPOSE 80

Usage Instructions

Prepare Your PHP Application:
Ensure your PHP application code is placed inside a src directory at the same location as this Dockerfile.
Build the Docker Image:
Execute the following command in the terminal, substituting my-lamp-app with your desired image name:

docker build -t my-lamp-app .

