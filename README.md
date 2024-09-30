# my-fullstack-project

https://ma.opsteam.space/api - backend
https://ma.opsteam.space/frontend - frontend
https://ma.opsteam.space/ - phpmyadmin

# Instructions for Deploying the Project on a New Server

This document provides instructions for deploying the project on a new server using Docker and Docker Compose.

## Prerequisites

Before starting the deployment, ensure that you have the following installed on your server:

- [Docker](https://docs.docker.com/get-docker/)
- [Docker Compose](https://docs.docker.com/compose/install/)
- [Certbot](https://certbot.eff.org/instructions) (for obtaining SSL certificates)

## Deployment Steps

### 1. Install Docker and Docker Compose

Run the following commands to install Docker and Docker Compose:

```bash
# Update the system
sudo apt update

# Install Docker
sudo apt install -y docker.io

# Add the user to the docker group
sudo usermod -aG docker $USER

# Install Docker Compose
sudo apt install -y docker-compose
```

### 2. Obtain SSL Certificates

Get SSL certificates using Certbot:

```bash
sudo certbot certonly --standalone -d test.com
```

Copy the certificates to the `~/certs` directory:

```bash
mkdir -p ~/certs
sudo cp /etc/letsencrypt/live/test.com/fullchain.pem ~/certs/fullchain.pem
sudo cp /etc/letsencrypt/live/test.com/privkey.pem ~/certs/privkey.pem
```

### 3. Configure Nginx

In your `docker-compose.yml` file, configure Nginx to use the obtained certificates:

```yaml
nginx:
    image: nginx:alpine
    ports:
      - "80:80"
      - "443:443"
    volumes:
      - ./nginx.conf:/etc/nginx/nginx.conf
      - ~/certs:/etc/letsencrypt
```

### 4. Install Dependencies with Composer

Run the command to install dependencies in the container:

```bash
docker-compose exec backend composer install
```

### 5. Migrate the Database

Run the database migration:

```bash
docker-compose exec backend php artisan migrate
```

### 6. Start the Project

Run the project using Docker Compose:

```bash
docker-compose up -d
```

### 7. Access the Project

After successfully starting, you can access the project at: [https://test.com](https://test.com)

## Notes

- Ensure that the DNS records for the domain `test.com` point to the IP address of your server.
- In case of errors, check the Docker logs using:

```bash
docker-compose logs
```

