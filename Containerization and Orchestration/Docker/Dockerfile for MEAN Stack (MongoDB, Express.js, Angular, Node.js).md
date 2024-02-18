# Dockerfile for MEAN Stack (MongoDB, Express.js, Angular, Node.js)

This Dockerfile provides a foundational setup for deploying MEAN stack web applications. It leverages the official Node.js image to install dependencies and prepare your application for execution. By building and running this Docker image, your application will be accessible on port 3000, offering a quick start to MEAN stack development.

## Dockerfile

```dockerfile
FROM node:14
WORKDIR /usr/src/app
COPY package*.json ./
RUN npm install
COPY . .
EXPOSE 3000
CMD ["npm", "start"]
```


## Sample Files

### `index.js`

```
const http = require('http');
  
const server = http.createServer((req, res) => {
  res.statusCode = 200;
  res.setHeader('Content-Type', 'text/plain');
  res.end('Hello World\n');
});

const PORT = process.env.PORT || 3000;
server.listen(PORT, () => {
  console.log(`Server running on port ${PORT}`);
});

```


### `package.json`

```
{
  "name": "simple-node-app",
  "version": "1.0.0",
  "description": "A simple Node.js application",
  "main": "index.js",
  "scripts": {
    "start": "node index.js"
  },
  "author": "KodeKloud",
  "license": "ISC"
}

```

## How to Use

1. **Prepare Your Application**: Ensure you have `index.js` and `package.json` files in your project directory. These files should be customized to fit your application's requirements.

2. **Build the Docker Image**: Run the following command in your terminal, replacing `my-mean-app` with your desired image name.

    ```
    docker build -t my-mean-app .
    ```

3. **Run Your Container**: To start your container and make your application accessible on port 3000, use the command below.

    ```
    docker run -p 3000:3000 my-mean-app
    ```
