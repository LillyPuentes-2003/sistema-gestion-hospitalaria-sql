# Hospital SQL - PostgreSQL con Liquibase

Base de datos relacional para sistema integral de gestión hospitalaria, versionada con Liquibase.

## 📊 Características

- **PostgreSQL 16** como motor de base de datos
- **Liquibase** para control de versiones de BD
- **Arquitectura por capas SQL:** DDL, DML, DCL, TCL
- **9 tablas principales** normalizadas en 3FN
- **Roles y permisos** granulares
- **Scripts de rollback** para cada cambio
- **800+ registros** de datos de prueba

## 🗂️ Estructura del Proyecto

```text
hospital-sql/
├── changelog-master.yaml
├── 01_ddl/
│   ├── 0000changelog.yaml
│   ├── 00_extensions/
│   │   └── 0000changelog.yaml
│   ├── 01_schemas/
│   │   └── 0000changelog.yaml
│   ├── 02_types/
│   │   └── 0000changelog.yaml
│   ├── 03_tables/
│   │   └── 0000changelog.yaml
│   ├── 04_views/
│   │   └── 0000changelog.yaml
│   ├── 05_materialized_views/
│   │   └── 0000changelog.yaml
│   ├── 06_functions/
│   │   └── 0000changelog.yaml
│   ├── 07_procedures/
│   │   └── 0000changelog.yaml
│   ├── 08_triggers/
│   │   └── 0000changelog.yaml
│   └── 09_indexes/
│       └── 0000changelog.yaml
├── 02_dml/
│   ├── 0000changelog.yaml
│   ├── 00_inserts/
│   │   └── 0000changelog.yaml
│   ├── 01_updates/
│   │   └── 0000changelog.yaml
│   ├── 02_deletes/
│   │   └── 0000changelog.yaml
│   ├── 03_upserts/
│   │   └── 0000changelog.yaml
│   └── 04_patches/
│       └── 0000changelog.yaml
├── 03_dcl/
│   ├── 0000changelog.yaml
│   ├── 00_roles/
│   │   └── 0000changelog.yaml
│   ├── 01_grants/
│   │   └── 0000changelog.yaml
│   └── 02_policies/
│       └── 0000changelog.yaml
├── 04_tcl/
│   ├── 0000changelog.yaml
│   ├── 00_transaction_blocks/
│   │   └── 0000changelog.yaml
│   └── 01_manual_recoveries/
│       └── 0000changelog.yaml
├── 05_rollbacks/
│   ├── 01_ddl/
│   ├── 02_dml/
│   ├── 03_dcl/
│   └── 04_tcl/
├── docker/
│   └── liquibase/
│       └── Dockerfile
├── docs/
├── scripts/
├── docker-compose.yml
├── .env.example
├── liquibase.properties.example
├── .gitignore
├── .dockerignore
└── README.md
```

## 📊 Modelo de Datos

### Tablas Principales

1. **especialidades** - Especialidades médicas (Medicina General, Pediatría, etc.)
2. **medicos** - Registro de médicos con especialidad
3. **eps** - Entidades Promotoras de Salud
4. **pacientes** - Información de pacientes y afiliación
5. **citas** - Agendamiento de citas médicas
6. **diagnosticos_cie10** - Códigos CIE-10 (500+ diagnósticos)
7. **consultas** - Registro de atención médica
8. **medicamentos** - Inventario farmacéutico
9. **prescripciones** - Medicamentos formulados
---
### Relaciones

- 1 Especialidad → N Médicos
- 1 Médico → N Citas
- 1 Paciente → N Citas
- 1 EPS → N Pacientes
- 1 Cita → 1 Consulta
- 1 Consulta → 1 Diagnóstico CIE-10
- 1 Consulta → N Prescripciones
- 1 Medicamento → N Prescripciones

## 🚀 Inicio Rápido

### Requisitos
- Docker Desktop
- Docker Compose

### Instalación

```bash
# 1. Clonar repositorio
git clone https://github.com/TU-USUARIO/hospital-sql.git
cd hospital-sql

# 2. Copiar variables de entorno
cp .env.example .env

# 3. Copiar configuración de Liquibase
cp liquibase.properties.example liquibase.properties

# 4. Levantar PostgreSQL
docker-compose up -d postgres

# 5. Validar changelog
docker-compose --profile tooling run --rm liquibase validate

# 6. Ver estado
docker-compose --profile tooling run --rm liquibase status

# 7. Aplicar cambios
docker-compose --profile tooling run --rm liquibase update
```

## 🔄 Comandos Liquibase

### Operaciones básicas

```bash
# Validar estructura de changelog
docker-compose --profile tooling run --rm liquibase validate

# Ver estado de migraciones
docker-compose --profile tooling run --rm liquibase status

# Aplicar todos los changesets pendientes
docker-compose --profile tooling run --rm liquibase update

# Ver SQL sin ejecutar
docker-compose --profile tooling run --rm liquibase update-sql

# Ver historial de cambios aplicados
docker-compose --profile tooling run --rm liquibase history
```

### Rollback

```bash
# Rollback del último changeset
docker-compose --profile tooling run --rm liquibase rollback-count --count=1

# Vista previa de rollback (sin ejecutar)
docker-compose --profile tooling run --rm liquibase rollback-count-sql --count=1

# Crear tag antes de cambios importantes
docker-compose --profile tooling run --rm liquibase tag --tag=v1.0.0

# Rollback hasta un tag específico
docker-compose --profile tooling run --rm liquibase rollback --tag=v1.0.0

# Rollback por fecha
docker-compose --profile tooling run --rm liquibase rollback-to-date "2026-05-11 10:00:00"
```

## 🔒 Seguridad

### Roles Implementados

- **admin** - Acceso completo a todas las tablas
- **medico** - Lectura completa, escritura en consultas y prescripciones
- **recepcionista** - Gestión de citas y datos de pacientes
- **farmaceutico** - Gestión de inventario y despacho de medicamentos
  
### Credenciales por Defecto
```text
Usuario: ariel5253

Password: ariel5253

Base de datos: hospital

Puerto: 5433
```
## 📈 Datos Incluidos

### Datos de Parametrización (Reales)
- 15 especialidades médicas
- 20 EPS registradas en Colombia
- 500+ códigos de diagnóstico CIE-10

### Datos Sintéticos (Generados para pruebas)
- 100+ pacientes
- 30+ médicos
- 300+ citas
- 200+ consultas
- 80+ medicamentos
- 150+ prescripciones

## 🧪 Verificación

### Conectar a PostgreSQL

```bash
# Desde Docker
docker exec -it hospital-postgres psql -U ariel5253 -d hospital

# Comandos útiles dentro de psql
\dt                    # Listar tablas
\d+ pacientes          # Describir tabla
SELECT COUNT(*) FROM pacientes;
\q                     # Salir
```

### Con cliente GUI

- **DBeaver / pgAdmin / DataGrip**
- Host: `localhost`
- Puerto: `5433`
- Base de datos: `hospital`
- Usuario: `ariel5253`
- Password: `ariel5253`

## 🔄 Reset Completo

```bash
# Detener y eliminar todo (¡cuidado! borra datos)
docker-compose down --volumes --remove-orphans

# Volver a crear desde cero
docker-compose up -d postgres
docker-compose --profile tooling run --rm liquibase update
```

## 📚 Documentación Adicional

- [Arquitectura de Capas SQL](docs/sql-layer-architecture.md)
- [Modelo Entidad-Relación](docs/MODELO_ER.md)
- [Diccionario de Datos](docs/DICCIONARIO_DATOS.md)

## 🤝 Colaboradores

- **ariel5253** - Permisos de lectura


Proyecto académico - CORHUILA 2026
