# test-manager-library-3413974-infra

Infraestructura de contenedores para **Manager Library** (ADSO 3413974).

## Servicios

- **`db`**: PostgreSQL 16 (`postgres:16-alpine`), base de datos inicialmente vacía (Art. V de la Constitución, ADR-001), healthcheck configurado con `pg_isready`, persistencia en volumen `pgdata`.
- **`api`**: Backend en PHP 8.3 / Laravel 11, conectado al servicio `db`, ejecuta migraciones y expone la API REST en el puerto `8000`.
- **`app`**: Frontend en React (Vite / Nginx o Node), configurado para consumir `http://localhost:8000/api/v1` y expuesto en el puerto `5173`.
- **Red interna**: `library_net` (bridge).

## Requisitos

- Docker Engine 24+
- Docker Compose v2+

## Inicio del Entorno

1. Copiar el archivo de variables de entorno:
   ```bash
   cp .env.example .env
   ```
2. Levantar los servicios con un solo comando:
   ```bash
   docker compose up --build
   ```

3. Acceso a los servicios:
   - Frontend (React): `http://localhost:5173`
   - Backend API: `http://localhost:8000/api/v1`
   - Healthcheck API: `http://localhost:8000/api/v1/health`
   - PostgreSQL: `localhost:5432`

## Limpieza y Reinicio de la Base de Datos

Para detener y reiniciar con una base de datos limpia:
```bash
docker compose down -v
docker compose up --build
```

