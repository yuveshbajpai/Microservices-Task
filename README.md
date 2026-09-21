## Microservices Task - Dockerized Microservices Application

## 1. Project Overview

This project demonstrates a microservices-based Node.js application consisting of four independent services:

- User Service
- Product Service
- Order Service
- Gateway Service

Each service runs in its own Docker container. Docker Compose is used to build, start, stop, and manage all services together.

All services are connected through a common Docker bridge network.

---

## 2. Technologies Used

- Node.js
- Express.js
- Docker
- Docker Compose
- REST APIs
- Git
- GitHub

---

## 3. Project Structure

Microservices-Task/
|
├── Microservices/
│ |
│ ├── gateway-service/
│ │ ├── app.js
│ │ ├── package.json
│ │ └── Dockerfile
│ |
│ ├── user-service/
│ │ ├── app.js
│ │ ├── package.json
│ │ └── Dockerfile
│ |
│ ├── product-service/
│ │ ├── app.js
│ │ ├── package.json
│ │ └── Dockerfile
│ |
│ ├── order-service/
│ │ ├── app.js
│ │ ├── package.json
│ │ └── Dockerfile
│ |
│ └── docker-compose.yml
|
└── README.md

## 4. Dockerfile and Docker Compose Creation

For this project, a separate Dockerfile was created for each microservice: User Service, Product Service, Order Service, and Gateway Service. Each Dockerfile is responsible for defining how the corresponding Node.js application is packaged into a Docker image. The Dockerfiles use node:20 as the base image because the application is built using Node.js. The WORKDIR /app instruction creates /app as the working directory inside the container. The COPY package\*.json ./ instruction copies the package files into the container, and RUN npm install installs all the dependencies required by the application. After installing the dependencies, COPY . . copies the application source code into the container. The EXPOSE instruction specifies the port on which the service runs, such as 3000 for the User Service, 3001 for the Product Service, 3002 for the Order Service, and 3003 for the Gateway Service. Finally, CMD ["node", "app.js"] starts the Node.js application when the container runs. This process allows each microservice to be packaged independently and run consistently inside its own container.

A docker-compose.yml file was then created inside the Microservices directory to manage all four containers together. Docker Compose eliminates the need to build and start each service separately. In the Compose file, each service is defined with its Docker build context, container name, port mapping, and network configuration. The build section specifies the directory containing the Dockerfile for each service, while container_name gives each container a readable name. The ports section maps the host machine's port to the corresponding container port, for example, "3000:3000" maps port 3000 on the host to port 3000 inside the User Service container. The Gateway Service also uses depends_on for the User, Product, and Order services so that Docker Compose starts those dependent services before starting the Gateway container. Finally, all four services are connected to a common Docker bridge network named microservices-network, which allows the containers to communicate with each other through the Docker network.

After creating the Dockerfiles and docker-compose.yml, the complete application can be built using docker compose build. Once the images are successfully created, all four services can be started together using docker compose up, or docker compose up -d to run them in the background. The running containers can be checked using docker compose ps. The individual services are available on ports 3000, 3001, 3002, and 3003, while the Gateway provides access through endpoints such as /api/users, /api/products, and /api/orders. This setup demonstrates how Docker containerizes individual microservices and how Docker Compose provides a simple way to build, network, and manage the complete microservices application.
