# Dockerfile for LAMP Stack (Linux, Apache, MySQL, PHP)

This Dockerfile configures a LAMP stack within a Docker container, making it perfect for running PHP web applications. It utilizes the official PHP image that comes with Apache and includes MySQL extensions to ensure seamless database connectivity for your applications. Simply add your PHP code to the `src` directory, build, and run the container for a quick and easy deployment.

## Dockerfile Content

```dockerfile
FROM php:7.4-apache
RUN docker-php-ext-install mysqli pdo pdo_mysql
COPY src/ /var/www/html/
EXPOSE 80
```

## How to Use

1. **Prepare Your PHP Application**: Place your application's PHP code within a directory named `src`.

2. **Build the Docker Image**: Execute the following command in your terminal, substituting `my-lamp-app` with your preferred name for the Docker image:
    ```
    docker build -t my-lamp-app .
    ```

3. **Run Your Container**: Start your container with the command below, which maps the container's port 80 to port 80 on your host, allowing you to access the application through `http://localhost`:
    ```
    docker run -p 80:80 my-lamp-app
    ```
