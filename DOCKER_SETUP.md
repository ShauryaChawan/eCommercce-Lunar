# Docker Setup for Lunar eCommerce

This guide will help you run the Lunar eCommerce application using Docker.

## Prerequisites

- Docker Desktop installed on your system
- Docker Compose (usually comes with Docker Desktop)

## Quick Start

### 1. Build and Start Containers

```bash
docker-compose up -d --build
```

This will:
- Build the Laravel application image
- Start MySQL database
- Start the application server
- Start the queue worker

### 2. Set Up Application Key

First time setup - generate application key:

```bash
docker-compose exec app php artisan key:generate
```

### 3. Copy Environment File

```bash
# Windows PowerShell
Copy-Item .env.docker .env

# Linux/Mac
cp .env.docker .env
```

Or manually copy `.env.docker` to `.env` and update `APP_KEY` with the generated key.

### 4. Run Database Migrations

```bash
docker-compose exec app php artisan migrate --force
```

### 5. Optional: Seed Database

```bash
docker-compose exec app php artisan db:seed
```

### 6. Access Application

Open your browser and navigate to:
```
http://localhost:8000
```

## Common Commands

### View Logs
```bash
# All services
docker-compose logs -f

# Specific service
docker-compose logs -f app
docker-compose logs -f db
docker-compose logs -f queue
```

### Stop Containers
```bash
docker-compose down
```

### Stop and Remove Volumes (Clean Reset)
```bash
docker-compose down -v
```

### Restart Services
```bash
docker-compose restart
```

### Run Artisan Commands
```bash
docker-compose exec app php artisan [command]
```

### Run Composer Commands
```bash
docker-compose exec app composer [command]
```

### Access Container Shell
```bash
docker-compose exec app bash
```

### Clear Cache
```bash
docker-compose exec app php artisan cache:clear
docker-compose exec app php artisan config:clear
docker-compose exec app php artisan view:clear
```

## Troubleshooting

### Permission Issues
```bash
docker-compose exec app chown -R www-data:www-data /var/www/html/storage
docker-compose exec app chmod -R 755 /var/www/html/storage
```

### Database Connection Issues
- Ensure the database container is running: `docker-compose ps`
- Check database logs: `docker-compose logs db`
- Verify environment variables in `.env` match those in `docker-compose.yml`

### Rebuild Containers
```bash
docker-compose down
docker-compose build --no-cache
docker-compose up -d
```

## Production Considerations

For production deployment:

1. Update `.env` with production values
2. Set `APP_DEBUG=false`
3. Use strong passwords for database
4. Configure proper mail settings
5. Set up SSL/TLS certificates
6. Configure proper backup strategy for database volume
7. Consider using managed database services
8. Add Redis for better caching and queue performance

## Ports

- **8000**: Laravel Application
- **3306**: MySQL Database

## Volumes

- `dbdata`: Persistent MySQL database storage
- Application files are mounted from host for development

## Network

All services communicate through the `lunar-network` bridge network.
