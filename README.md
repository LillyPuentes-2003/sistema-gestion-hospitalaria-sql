# Hospital Backend Relacional - PostgreSQL

Base de datos relacional para sistema integral de gestión hospitalaria.

## 📊 Características

- **Modelo normalizado (3FN)** con 9 tablas principales
- **800+ registros sintéticos** generados con Python/Faker
- **Datos reales** de parametrización (CIE-10, EPS, especialidades)
- **Transacciones ACID** para operaciones críticas
- **Roles y permisos** granulares (DCL)

## 🚀 Inicio Rápido

### Requisitos
- Docker
- Docker Compose

### Instalación

```bash
# Copiar variables de entorno
cp .env.example .env

# Levantar PostgreSQL
docker-compose up -d

# Verificar que esté corriendo
docker-compose ps

# Acceder a PostgreSQL
docker exec -it hospital-postgres psql -U admin -d hospital
```

### Ejecutar Scripts de Inicialización

```bash
# Opción 1: Script automático de inicialización
docker exec -i hospital-postgres psql -U admin -d hospital < init.sql

# Opción 2: Paso a paso
docker exec -i hospital-postgres psql -U admin -d hospital < db/DDL/01_create_tables.sql
docker exec -i hospital-postgres psql -U admin -d hospital < db/DDL/02_indexes.sql
# ... etc
```

## 📁 Estructura del Proyecto
