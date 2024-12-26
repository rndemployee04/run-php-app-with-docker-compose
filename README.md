Certainly! Below is a `README.md` file with a tab-like structure to guide users through setting up the PHP application with Docker Compose for both **Linux** and **Windows**.

```markdown
# PHP Application with Docker Compose

This guide will help you set up and run a PHP application using Docker Compose on both **Linux** and **Windows** environments.

---

## **Table of Contents**

- [Prerequisites](#prerequisites)
- [Step 1: Create Project Structure](#step-1-create-project-structure)
- [Step 2: Create the `docker-compose.yml` File](#step-2-create-the-docker-composeyml-file)
- [Step 3: Prepare Environment Variables (Optional)](#step-3-prepare-environment-variables-optional)
- [Step 4: Run Docker Compose](#step-4-run-docker-compose)
  - [Linux Setup](#linux-setup)
  - [Windows Setup](#windows-setup)
- [Step 5: Stopping and Removing Containers](#step-5-stopping-and-removing-containers)
- [Troubleshooting](#troubleshooting)

---

## **Prerequisites**

Before you begin, ensure the following:

- **Docker** and **Docker Compose** installed on your machine.
  - [Install Docker on Linux](https://docs.docker.com/engine/install/)
  - [Install Docker Desktop on Windows](https://www.docker.com/products/docker-desktop)
- **Docker Desktop** configured to use **Linux containers** on Windows.
- **Linux** or **Windows Subsystem for Linux (WSL 2)** for running Docker on Windows.

---

## **Step 1: Create Project Structure**

Create a folder for your project and navigate into it:

```bash
mkdir php-docker-app
cd php-docker-app
```

Inside this folder, create the following structure:

```bash
php-docker-app/
  ├── docker-compose.yml
  └── src/  # PHP source code goes here
```

You can add a simple `index.php` file inside the `src/` directory:

```php
<!-- src/index.php -->
<?php
echo "Hello, Docker! This is PHP running inside a container.";
?>
```

---

## **Step 2: Create the `docker-compose.yml` File**

Create the `docker-compose.yml` file in the root of your project (`php-docker-app/`) with the following content:

```yaml
version: '3.8'

services:
  web:
    image: php:8.1-apache
    container_name: php-app
    ports:
      - "${PHP_PORT:-80}:80"  # Allows overriding the port with the PHP_PORT environment variable, default is 80
    volumes:
      - ./src:/var/www/html  # Mounts the source code directory to the web server
    environment:
      - PHP_DISPLAY_ERRORS=${PHP_DISPLAY_ERRORS:-On}  # Default to 'On' if not specified
      - PHP_ERROR_LOG=${PHP_ERROR_LOG:-/var/log/php/error.log}  # Default log path
    networks:
      - app-network
    restart: unless-stopped

  db:
    image: mysql:5.7
    container_name: php-db
    environment:
      MYSQL_ROOT_PASSWORD: ${MYSQL_ROOT_PASSWORD:-rootpassword}  # Default root password
      MYSQL_DATABASE: ${MYSQL_DATABASE:-app_db}  # Default database name
      MYSQL_USER: ${MYSQL_USER:-user}  # Default MySQL user
      MYSQL_PASSWORD: ${MYSQL_PASSWORD:-password}  # Default user password
    volumes:
      - db-data:/var/lib/mysql  # Persist database data between restarts
    networks:
      - app-network
    restart: unless-stopped

networks:
  app-network:
    driver: bridge

volumes:
  db-data:
    driver: local
```

---

## **Step 3: Prepare Environment Variables (Optional)**

You can create an optional `.env` file in the root directory (`php-docker-app/`) to define environment variables for customization.

Example `.env` file:

```ini
PHP_PORT=8080
MYSQL_ROOT_PASSWORD=securepassword
MYSQL_DATABASE=my_database
MYSQL_USER=my_user
MYSQL_PASSWORD=my_password
PHP_DISPLAY_ERRORS=Off
```

---

## **Step 4: Run Docker Compose**

### **Linux Setup**

1. **Build and Start the Containers**:

   Run the following command in the terminal (inside the `php-docker-app` directory):

   ```bash
   docker-compose up -d
   ```

   This will download the required Docker images (PHP, Apache, MySQL) and start the containers.

2. **Check the Running Containers**:

   You can check if the containers are running by executing:

   ```bash
   docker ps
   ```

3. **Access the PHP Application**:

   Open a browser and visit `http://localhost:80` (or another port if customized via `.env` or `PHP_PORT`).

---

### **Windows Setup**

1. **Install Docker Desktop** and ensure **Linux containers** are enabled (default).

   - You can switch between **Linux containers** and **Windows containers** in Docker Desktop by right-clicking the Docker icon in the taskbar and selecting "Switch to Linux containers".

2. **Build and Start the Containers**:

   Open **Command Prompt** or **PowerShell**, navigate to your project directory (`php-docker-app`), and run:

   ```powershell
   docker-compose up -d
   ```

3. **Check the Running Containers**:

   Use the following command to verify that the containers are running:

   ```powershell
   docker ps
   ```

4. **Access the PHP Application**:

   Open a browser and go to `http://localhost:80` (or the port specified in `.env`).

---

## **Step 5: Stopping and Removing Containers**

After testing, you can stop and remove the containers:

1. **Stop the Containers**:

   Run this command to stop and remove the containers:

   ```bash
   docker-compose down
   ```

2. **Remove Containers and Volumes (optional)**:

   If you want to remove the containers along with the associated volumes (which will delete all MySQL data):

   ```bash
   docker-compose down -v
   ```

---

## **Troubleshooting**

### 1. **Permission Issues (Linux)**

If you face permission issues with mounting volumes (e.g., `./src`), make sure the user running Docker has access to the files:

```bash
sudo docker-compose up -d
```

### 2. **File Mounting (Windows)**

If file mounts aren't working on Windows, ensure the directory you're mounting (e.g., `./src`) is within a directory Docker is allowed to share (like `C:/Users/...`).

### 3. **Accessing Containers**

If you need to access a running container's shell:

```bash
docker exec -it php-app bash
```

This will start a bash shell inside the running container, allowing you to debug or run commands directly in the container.

---

## **Conclusion**

With this guide, you can easily run a PHP application using Docker Compose on both **Linux** and **Windows** systems. By following the steps outlined here, you will have a portable setup for PHP and MySQL, with the ability to configure settings via environment variables for different environments (e.g., development, production).
```

---

### **Explanation of the Structure:**

- **Tabs for Linux and Windows**: The guide is split into sections, with specific instructions for **Linux** and **Windows** in the `Run Docker Compose` step.
- **Environment Variables**: Optional `.env` file usage allows easy configuration for different environments.
- **Cross-platform Notes**: All commands and steps are clearly indicated for both platforms, ensuring clarity for users on each system.

---

