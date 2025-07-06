# Development Tools Docker Compose

This Docker Compose setup provides a comprehensive development environment with multiple database systems, caching solutions, message queues, and development tools.

## Services Included

### Databases
- **PostgreSQL 15** - Primary relational database
- **MySQL 8.0** - Alternative relational database
- **MongoDB 6.0** - NoSQL document database

### Caching & Memory
- **Redis 7.0** - In-memory data structure store
- **Memcached 1.6** - Distributed memory caching system

### Message Queue
- **RabbitMQ 3.11** - Message broker with management interface

### Development Tools
- **Adminer 4.8.1** - Database management tool
- **Mailpit** - Modern email testing tool with advanced features
- **SonarQube LTS** - Code quality and security analysis

## Quick Start

1. **Clone and navigate to the project directory**
   ```bash
   cd /path/to/your/project
   ```

2. **Copy the environment file**
   ```bash
   cp .env.example .env
   ```

3. **Start all services**
   ```bash
   docker-compose up -d
   ```

4. **Check service status**
   ```bash
   docker-compose ps
   ```

## Configuration

### Environment Variables

Copy `.env.example` to `.env` and customize the following variables:

#### Docker Configuration
- `RESTART_POLICY` - Container restart policy (default: `always`)
  - Options: `no`, `always`, `on-failure`, `unless-stopped`

#### PostgreSQL
- `POSTGRES_USER` - Database username (default: `postgres`)
- `POSTGRES_PASSWORD` - Database password (default: `changeme`)
- `POSTGRES_DB` - Database name (default: `sonarqube`)
- `POSTGRES_PORT` - Host port mapping (default: `5432`)

#### MySQL
- `MYSQL_DATABASE` - Database name (default: `cloud`)
- `MYSQL_USER` - Database username (default: `uid`)
- `MYSQL_PASSWORD` - Database password (default: `pwd`)
- `MYSQL_ROOT_PASSWORD` - Root password (default: `secret`)
- `MYSQL_PORT` - Host port mapping (default: `3306`)
- `TZ` - Timezone (default: `Asia/Ho_Chi_Minh`)

#### MongoDB
- `MONGO_ROOT_USER` - Root username (default: `root`)
- `MONGO_ROOT_PASSWORD` - Root password (default: `example`)
- `MONGO_PORT` - Host port mapping (default: `27017`)

#### Redis
- `REDIS_PASSWORD` - Redis password (default: `changeme`)
- `REDIS_PORT` - Host port mapping (default: `6379`)

#### RabbitMQ
- `RABBITMQ_USER` - Username (default: `guest`)
- `RABBITMQ_PASSWORD` - Password (default: `guest`)
- `RABBITMQ_PORT` - AMQP port (default: `5672`)
- `RABBITMQ_WEB_PORT` - Management UI port (default: `15672`)

#### Development Tools
- `ADMINER_PORT` - Adminer web interface port (default: `8048`)
- `MAILPIT_WEB_PORT` - Mailpit web interface port (default: `8025`)
- `MAILPIT_SMTP_PORT` - Mailpit SMTP port (default: `1025`)
- `MEMCACHED_PORT` - Memcached port (default: `11211`)
- `SONARQUBE_PORT` - SonarQube web interface port (default: `9000`)

## Service Access

### Web Interfaces
- **Adminer**: http://localhost:8048 - Database management
- **RabbitMQ Management**: http://localhost:15672 - Message queue management
- **Mailpit**: http://localhost:8025 - Email testing interface
- **SonarQube**: http://localhost:9000 - Code quality analysis
  - Default credentials: `admin/admin`
  - ⚠️ **Important**: Change the default password after first login

### Database Connections
- **PostgreSQL**: `localhost:5432`
- **MySQL**: `localhost:3306`
- **MongoDB**: `localhost:27017`
- **Redis**: `localhost:6379`
- **Memcached**: `localhost:11211`

### SMTP Testing
- **Mailpit SMTP**: `localhost:1025`

## Health Checks

All critical services include health checks:
- **PostgreSQL**: `pg_isready` command
- **MySQL**: `mysqladmin ping` command
- **MongoDB**: MongoDB ping command
- **RabbitMQ**: `rabbitmq-diagnostics check_running`
- **Redis**: `redis-cli ping`
- **SonarQube**: HTTP status check on `/api/system/status`

## Data Persistence

The following volumes are created for data persistence:
- `postgres_data` - PostgreSQL data
- `mysql_data` - MySQL data
- `mongodb_data` - MongoDB data
- `rabbitmq_data` - RabbitMQ data
- `redis_data` - Redis data
- `sonarqube_conf` - SonarQube configuration
- `sonarqube_data` - SonarQube data
- `sonarqube_extensions` - SonarQube extensions
- `sonarqube_logs` - SonarQube logs
- `sonarqube_temp` - SonarQube temporary files

## Common Commands

### Start services
```bash
# Start all services
docker-compose up -d

# Start specific service
docker-compose up -d postgres

# Start with logs
docker-compose up
```

### Stop services
```bash
# Stop all services
docker-compose down

# Stop and remove volumes (⚠️ This will delete all data)
docker-compose down -v
```

### View logs
```bash
# View all logs
docker-compose logs

# View specific service logs
docker-compose logs postgres

# Follow logs
docker-compose logs -f
```

### Service management
```bash
# Restart a service
docker-compose restart postgres

# Check service status
docker-compose ps

# Execute commands in containers
docker-compose exec postgres psql -U postgres
docker-compose exec mysql8 mysql -u root -p
docker-compose exec mongodb mongosh
```

## Networking

All services are connected through the `dev_tools` bridge network, allowing inter-service communication using container names as hostnames.

Example connection strings from within containers:
- PostgreSQL: `postgresql://postgres:changeme@postgres:5432/sonarqube`
- MySQL: `mysql://uid:pwd@mysql8:3306/cloud`
- MongoDB: `mongodb://root:example@mongodb:27017`
- Redis: `redis://:changeme@redis:6379`

### Connecting External Services

To connect your application containers to the dev_tools network and access these services, add the following configuration to your `docker-compose.yml`:

```yaml
services:
  app:
    # ... your service configuration
    networks:
      - dev_tools

networks:
  dev_tools:
    external: true
```

**Important**: Make sure the dev_tools network is created first by running this development environment before starting your application containers.

Once connected, your application can access services using their container names:
- Database host: `postgres` (instead of `localhost`)
- Redis host: `redis` (instead of `localhost`)
- MySQL host: `mysql8` (instead of `localhost`)
- MongoDB host: `mongodb` (instead of `localhost`)
- RabbitMQ host: `rabbitmq` (instead of `localhost`)

## Troubleshooting

### Common Issues

1. **Port conflicts**
   - Check if ports are already in use: `netstat -tulpn | grep :PORT`
   - Modify port mappings in `.env` file

2. **Permission issues**
   - Ensure Docker daemon is running
   - Check user permissions for Docker

3. **Memory issues**
   - SonarQube requires at least 2GB RAM
   - Increase Docker memory limits if needed

4. **Service won't start**
   - Check logs: `docker-compose logs SERVICE_NAME`
   - Verify environment variables in `.env`
   - Check health check status

### Reset Everything
```bash
# Stop all services and remove volumes
docker-compose down -v

# Remove all images (optional)
docker-compose down --rmi all

# Start fresh
docker-compose up -d
```

## Security Notes

⚠️ **Important**: This setup is designed for development environments only.

- Default passwords are used - change them in production
- **SonarQube**: Default login is `admin/admin` - **must be changed after first login**
- Services are bound to localhost (127.0.0.1) for security
- Consider using Docker secrets for sensitive data in production

## Requirements

- Docker Engine 20.10+
- Docker Compose 2.0+
- At least 4GB RAM (recommended 8GB for SonarQube)
- Available ports: 5432, 3306, 27017, 6379, 11211, 5672, 15672, 8048, 8025, 1025, 9000

## Contributing

When adding new services:
1. Add environment variables to `.env.example`
2. Include health checks where applicable
3. Use the `dev_tools` network
4. Bind to localhost for security
5. Update this README with service information