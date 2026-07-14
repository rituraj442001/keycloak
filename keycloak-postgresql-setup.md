# Keycloak 26.7.0 with PostgreSQL Setup

## Prerequisites

* Keycloak 26.7.0
* PostgreSQL running on localhost:5432
* PostgreSQL user: `postgres`
* PostgreSQL database: `keycloak`

## Create Database

```sql
CREATE DATABASE keycloak;
```

## Start Keycloak in Development Mode

For local development and testing:

```bash
bin/kc.sh start-dev \
  --db=postgres \
  --db-url=jdbc:postgresql://localhost:5432/keycloak \
  --db-username=postgres \
  --db-password='Root@1234'
```

### Access Keycloak

```
http://localhost:8080
```

## Start Keycloak in Production Mode (HTTP Enabled)

Production mode requires HTTPS by default. For local environments where HTTPS is not required, enable HTTP explicitly:

```bash
bin/kc.sh start \
  --http-enabled=true \
  --hostname=localhost \
  --db=postgres \
  --db-url=jdbc:postgresql://localhost:5432/keycloak \
  --db-username=postgres \
  --db-password='Root@1234'
```

## Start Keycloak in Production Mode (HTTPS)

```bash
bin/kc.sh start \
  --https-certificate-file=/path/server.crt \
  --https-certificate-key-file=/path/server.key \
  --db=postgres \
  --db-url=jdbc:postgresql://localhost:5432/keycloak \
  --db-username=postgres \
  --db-password='Root@1234'
```

## Verify Configuration

```bash
bin/kc.sh show-config
```

## Common Error

### Error

```text
Key material not provided to setup HTTPS.
Please configure your keys/certificates...
```

### Cause

The `start` command runs Keycloak in production mode, which requires HTTPS by default.

### Solution

Use one of the following:

* `start-dev` for local development
* `--http-enabled=true` for local production testing
* Configure HTTPS certificates for a production deployment

## Environment Variable Configuration

```bash
export KC_DB=postgres
export KC_DB_URL=jdbc:postgresql://localhost:5432/keycloak
export KC_DB_USERNAME=postgres
export KC_DB_PASSWORD='Root@1234'

bin/kc.sh start-dev
```

## Verify PostgreSQL Connectivity

List available databases:

```bash
psql -U postgres -l
```

Connect to the Keycloak database:

```bash
psql -U postgres -d keycloak
```

## Notes

* PostgreSQL is fully supported by Keycloak 26.7.0.
* The PostgreSQL JDBC driver is bundled with Keycloak.
* `start-dev` is recommended for local development.
* HTTPS should be configured for production deployments.
