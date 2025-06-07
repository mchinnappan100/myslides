 

## 🐳 Introduction to Docker

Docker is an open-source platform that uses containerization to automate the deployment of applications inside lightweight, portable containers. These containers package an application and its dependencies, ensuring consistency across various environments.

---

## 🧱 Key Components

* **Docker Engine**: The core component that runs and manages containers.
* **Docker Images**: Read-only templates used to create containers.
* **Docker Containers**: Running instances of Docker images.
* **Docker Hub**: A cloud-based registry for sharing and managing Docker images.
* **Docker Compose**: A tool for defining and running multi-container applications.

---

## 🚀 Benefits of Docker

* **Portability**: Run containers consistently across different environments.
* **Efficiency**: Containers share the host OS kernel, making them lightweight and fast to start .
* **Isolation**: Applications run in isolated environments, reducing conflicts.
* **Scalability**: Easily scale applications by adding or removing containers.
* **CI/CD Integration**: Streamline development workflows with continuous integration and deployment.
---

## 🛠️ Basic Docker Commands

* `docker pull <image>`: Download an image from a registry.
* `docker build -t <tag> <path>`: Build an image from a Dockerfile.
* `docker run <options> <image>`: Create and start a container from an image.
* `docker ps`: List running containers.
* `docker stop <container>`: Stop a running container.
* `docker rm <container>`: Remove a stopped container.

---

## 🧪 Example: Dockerfile

A `Dockerfile` is a script containing instructions to build a Docker image.

```dockerfile
FROM node:14
WORKDIR /app
COPY . .
RUN npm install
EXPOSE 3000
CMD ["npm", "start"]
```



This Dockerfile creates an image for a Node.js application.

---

## 🧩 Docker Compose Example

Docker Compose allows you to define and manage multi-container applications. 

```yaml
version: '3'
services:
  web:
    image: nginx
    ports:
      - "8080:80"
  db:
    image: postgres
    environment:
      POSTGRES_PASSWORD: example
```



This `docker-compose.yml` file defines a web service using Nginx and a db service using PostgreSQL.

---

## 📚 Learn More

* [Docker Documentation](https://docs.docker.com/)
* [Docker Get Started Guide](https://docs.docker.com/get-started/)
* [Docker Compose Documentation](https://docs.docker.com/compose/)

 
