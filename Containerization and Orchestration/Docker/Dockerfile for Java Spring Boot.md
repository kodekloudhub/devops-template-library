# Dockerfile for Java, Spring Boot

This Dockerfile offers a fundamental configuration for Dockerizing Spring Boot applications. It utilizes the OpenJDK:17-alpine image as the base, ensuring a lightweight and efficient container environment. With this setup, you can easily package your Spring Boot application for deployment. Once built and executed, your application will be accessible on port 8080, providing a seamless starting point for Spring Boot development within Docker.

## Dockerfile

```dockerfile
FROM openjdk:17-alpine
ADD target/*.jar app.jar
EXPOSE 8080
ENTRYPOINT ["java","-jar","app.jar"]
```


## How to Use

1. **Prepare Your Application**: Ensure you are in the project root directory files in your project directory that contains `/target` directory and `Dockerfile` file.

2. **Build the Docker Image**: Run the following command in your terminal, replacing `spring-app` with your desired image name and `0.0.1` is desired version of image.

    ```
    docker build -t spring-app:0.0.1 .
    ```

3. **Run Your Container**: To start your container and make your application accessible on port 8080, use the command below.

    ```
    docker run -p 8080:8080 spring-app:0.0.1
    ```
