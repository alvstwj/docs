# Self-Hosting Guide for Discuit

Welcome to the self-hosting guide for Discuit! Here, you'll find step-by-step instructions on how to set up and run Discuit on your own server.

::: warning
This guide is designed specifically for users running Linux and macOS systems. Please note that **Windows is not officially supported** for self-hosting Discuit. If you choose to attempt installation on Windows, you will be doing so at your own risk and without official support. We recommend that Windows users consider using virtualization tools like [WSL (Windows Subsystem for Linux)](https://learn.microsoft.com/en-us/windows/wsl/install) to create a more compatible environment for hosting Discuit.
:::

## Cloning the Repository

Install [Git](https://git-scm.com/) and clone the repository by running the following commands in your terminal:

```shell
git clone https://github.com/discuitnet/discuit.git && cd discuit
```

## Running Locally

### Prerequisites

Before you begin, make sure you have the following installed on your system:

- [Go (1.21 or higher)](https://go.dev/)
- [MariaDB](https://mariadb.org/)
- [Redis](https://redis.io/)
- [Node.js and NPM](https://nodejs.org/en/download/package-manager)
- [libvips](https://libvips.github.io/libvips/install.html) (for image transformations)

### 1. Setting Up the Development Environment

#### Create MariaDB Database

```shell
# Open MariaDB CLI
mariadb -u root -p --binary-as-hex

# Create a database named discuit (you may use a different name)
create database discuit;

# Exit MariaDB CLI
exit;
```

### 3. Configure Discuit

Create a file named `config.yaml` in the root directory and copy the contents of [`config.default.yaml`](https://github.com/discuitnet/discuit/blob/main/config.default.yaml) into it. Modify `config.yaml` with your desired configurations.

### 4. Build and Run Discuit

```shell
./build.sh           # Build frontend and backend
./discuit migrate run   # Run migrations
./discuit serve     # Start the server
```

## Running with Docker

::: warning Limitations
Currently the Docker image is not suitable for production use due to limitations in the configuration. It also only supports AMD64 architecture.
:::

### Prerequisites

Before you begin, make sure you have the following installed on your system:

- [Docker](https://docs.docker.com/engine/install/)

### 1. Build the Docker Image

```shell
docker build -t discuit .
```

### 2. Run the Docker Container

```shell
docker run -d --name discuit \
-v discuit-db:/var/lib/mysql \
-v discuit-redis:/var/lib/redis \
-v discuit-images:/app/images \
-p 8080:80 discuit
```

### 3. Accessing Discuit

After the container starts, you can access Discuit by navigating to `http://localhost:8080` on your web browser.

### 4. Stopping and Starting the Container

```shell
docker stop discuit     # Stop the container
docker start discuit    # Start the container again
```
