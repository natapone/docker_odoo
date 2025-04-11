# Odoo 18 Docker Setup

This repository contains a Docker Compose setup for running Odoo 18 with PostgreSQL.

## Prerequisites

- Docker
- Docker Compose

## Directory Structure

```
.
├── docker-compose.yml
├── config
│   └── odoo.conf
├── addons
│   └── custom_addons
├── data
│   ├── odoo
│   └── postgres
└── .env.example
```

## Setup

1. Clone this repository
2. Copy `.env.example` to `.env` and update the values if needed
3. Run the following command to start the containers:
   ```bash
   docker compose up -d
   ```

## Access

- Odoo Web Interface: http://localhost:8069
- Default login credentials for database creation:
  - Email: admin
  - Password: admin

## Custom Addons

Place your custom addons in the `addons/custom_addons` directory. They will be automatically loaded by Odoo.

## Maintenance

- To update Odoo: `docker compose pull && docker compose up -d`
- To view logs: `docker compose logs -f`
- To stop: `docker compose down`

## Security Notes

- Change the admin password in `config/odoo.conf`
- Update database credentials in production
- Don't commit sensitive data to version control
