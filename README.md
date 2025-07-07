# Local WordPress Farm

This project provides a simple yet powerful setup for running multiple, independent WordPress instances locally using Docker and Docker Compose. It's ideal for developers who need to work on several WordPress projects simultaneously without conflicts, or for testing different WordPress versions and configurations.

## Features

* **Multiple Independent WordPress Sites:** Run `wordpress-site-1` on port `8080` and `wordpress-site-2` on port `8090`. Each site operates independently.
* **Dedicated MySQL Databases:** Each WordPress instance has its own dedicated MySQL 8.0 database, preventing data conflicts.
* **Persistent Data:** All database data and WordPress files are persisted using Docker volumes, so your changes and content remain even after stopping and restarting containers.
* **Easy Setup:** Get your local WordPress development environment running with just a few commands.
* **Customizable:** Easily extendable to add more WordPress sites or modify configurations.

## Prerequisites

Before you start, make sure you have the following installed on your machine:

* [**Docker Desktop**](https://www.docker.com/products/docker-desktop/) (includes Docker Engine and Docker Compose)

## Getting Started

Follow these steps to get your WordPress farm up and running:

### 1. Clone the Repository

First, clone this repository to your local machine:

```bash
git clone [https://github.com/ankerod/local-wordpress-farm.git](https://github.com/ankerod/local-wordpress-farm.git)
cd local-wordpress-farm
````

### 2\. Configure Environment Variables

Create a `.env` file in the root directory of the project (where `docker-compose.yml` is located). This file will store sensitive information like database credentials.

Copy the example below and replace the placeholder values with your desired passwords and usernames. Make sure these values are strong and secure for production, but for local development, they can be simpler.

```
# .env file content

# Database 1 for wordpress-site-1
MYSQL_ROOT_PASSWORD1=your_root_password_site1
MYSQL_DATABASE1=wordpress1_db
MYSQL_USER1=wpuser1
MYSQL_PASSWORD1=your_wp_password1

# Database 2 for wordpress-site-2
MYSQL_ROOT_PASSWORD2=your_root_password_site2
MYSQL_DATABASE2=wordpress2_db
MYSQL_USER2=wpuser2
MYSQL_PASSWORD2=your_wp_password2

# WordPress 1 Configuration
WORDPRESS_DB_HOST1=db-wordpress-site-1:3306
WORDPRESS_DB_USER1=wpuser1
WORDPRESS_DB_PASSWORD1=your_wp_password1
WORDPRESS_DB_NAME1=wordpress1_db

# WordPress 2 Configuration
WORDPRESS_DB_HOST2=db-wordpress-site-2:3306
WORDPRESS_DB_USER2=wpuser2
WORDPRESS_DB_PASSWORD2=your_wp_password2
WORDPRESS_DB_NAME2=wordpress2_db
```

### 3\. Build and Run the WordPress Farm

Navigate to the project root directory in your terminal and run the following command to start all services:

```bash
docker compose up -d
```

  * The `-d` flag runs the containers in detached mode (in the background).
  * The first time you run this, Docker will download the necessary images and set up the databases, which might take a few minutes.

### 4\. Access Your WordPress Sites

Once the containers are up and running, you can access your WordPress sites in your web browser:

  * **WordPress Site 1:** [http://localhost:8080](https://www.google.com/search?q=http://localhost:8080)
  * **WordPress Site 2:** [http://localhost:8090](https://www.google.com/search?q=http://localhost:8090)

Follow the standard WordPress installation process for each site.

### 5\. Stop the WordPress Farm

To stop all running containers without removing their data (volumes will persist), run:

```bash
docker compose down
```

### 6\. Stop and Remove All Data

To stop all containers and remove all associated data (databases and WordPress files), which is useful for a clean start:

```bash
docker compose down --volumes
```

-----

### Project Structure

```
.
├── docker-compose.yml
├── .env                  # Your environment variables (create this file)
├── .gitignore            # Git ignore file (recommended)
├── README.md             # This file
├── wordpress-site-1/     # Local mount point for WordPress Site 1 files
└── wordpress-site-2/     # Local mount point for WordPress Site 2 files
```

The `wordpress-site-1/` and `wordpress-site-2/` directories will be created automatically by Docker when the containers are first started, and WordPress's core files will be populated within them.

## Troubleshooting

  * **"Error establishing a database connection"**:
      * Ensure your `.env` file is correctly configured, especially `WORDPRESS_DB_HOST`. Remember that inside the Docker network, MySQL runs on port `3306`.
      * Check Docker container logs: `docker compose logs db-wordpress-site-1` or `docker compose logs wordpress-site-1`.
      * Ensure you ran `docker compose down --volumes` for a clean start after major configuration changes.
  * **"Service 'db-wordpress-site-X' failed to build: ... must be a string"**:
      * Your Docker Compose version might be old. Update Docker Desktop to the latest version, which typically includes Docker Compose V2 (run `docker compose` instead of `docker-compose`).
  * **Git Push/Pull Issues**: If Git rejects your push, always `git pull origin main` first to integrate remote changes before pushing. Resolve any merge conflicts if they occur.

Feel free to contribute or suggest improvements\!
