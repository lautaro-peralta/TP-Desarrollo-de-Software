# The Garrison System (TGS)

**Trabajo Práctico - Desarrollo de Software**  
**UTN FRRo - Grupo Shelby**

[![Materia](https://img.shields.io/badge/Materia-Desarrollo%20de%20Software-blue)]()
[![Universidad](https://img.shields.io/badge/Universidad-UTN%20FRRo-green)]()
[![Año](https://img.shields.io/badge/A%C3%B1o-2025-orange)]()

---

## 📋 Contenidos

- [🎯 Quick Start para Evaluadores](#-quick-start-para-evaluadores)
- [Sobre este Proyecto](#sobre-este-proyecto)
- [Estructura del Proyecto](#estructura-del-proyecto)
- [Tecnologías](#tecnologías)
- [Requisitos Previos](#requisitos-previos)
- [Configuración Inicial](#configuración-inicial)
- [Ejecución Local](#ejecución-local)
- [Usuarios de Prueba](#usuarios-de-prueba)
- [Testing](#testing)
- [Documentación](#documentación)
- [Troubleshooting](#troubleshooting)
- [Equipo](#equipo)

---

## 🎯 Quick Start para Evaluadores

Si solo quieres **probar rápidamente** el sistema (5-10 minutos):

### Opción 1: Producción (Recomendado - Sin instalación)
```bash
Acceder directamente: https://garrsys.vercel.app
Usar credenciales en la sección "Usuarios de Prueba" ↓
```

### Opción 2: Desarrollo Local (Con Docker)
```bash
# 1. Clonar con submódulos
git clone --recurse-submodules https://github.com/lautaro-peralta/GarrSYS.git
cd GarrSYS

# 2. Setup automático (requiere Docker)
make setup    # Linux/Mac
# O manualmente:
cd infra && docker compose up -d && cd ..

# 3. Backend
cd apps/backend && pnpm install && cp .env.example .env.development
# Editar .env.development: cambiar EMAIL_VERIFICATION_REQUIRED=false (ya lo dice)
pnpm start:dev

# 4. Frontend (en otra terminal)
cd apps/frontend && pnpm install && pnpm start

# 5. Cargar datos de prueba
make load-data    # Desde raíz del proyecto
```

**URLs locales:**
- Frontend: http://localhost:4200
- Backend API: http://localhost:3000
- Swagger Docs: http://localhost:3000/api-docs
- Health Check: http://localhost:3000/health

---

## Sobre este Proyecto

**The Garrison System** es un sistema integral de gestión y ventas ambientado en el Birmingham de los años 1920. Es un **Trabajo Práctico** de la materia *Desarrollo de Software* de la UTN FRRo que simula una red comercial con elementos de riesgo, corrupción y toma de decisiones estratégicas.

### Funcionalidades principales
- 🏪 Gestión de productos (legales e ilegales)
- 👥 Gestión de clientes, socios y distribuidores
- 📊 Sistema de ventas con trazabilidad
- 🗺️ Zonas de operación y control territorial
- 🚨 Autoridades y sistema de sobornos
- 📋 Consejo Shelby para decisiones estratégicas
- 🔐 Autenticación JWT con roles y permisos
- 📧 Verificación de email integrada

### Arquitetura
El proyecto utiliza **Git Submodules** para separar componentes:
- **Backend** (submódulo): API REST
- **Frontend** (submódulo): Aplicación SPA
- **Infraestructura** (este repo): Docker, scripts, documentación

---

## Estructura del Proyecto

```
GarrSYS/
├── apps/
│   ├── backend/              → Submódulo: API REST (Node.js + TypeScript)
│   │                         Endpoints, BD, lógica de negocio
│   └── frontend/             → Submódulo: SPA (Angular + TypeScript)
│                             Componentes, servicios, UI
├── infra/
│   └── docker-compose.yml    → PostgreSQL 16 y Redis 7
├── scripts/
│   ├── load-test-data.sh    → Script Unix: cargar datos de prueba
│   └── load-test-data.bat   → Script Windows: cargar datos de prueba
├── docs/
│   └── proposal.md          → Propuesta del proyecto
├── Makefile                 → Comandos simplificados
└── README.md               → Este archivo
```

### Información importante sobre Submódulos

Los submódulos del proyecto están en repositorios externos:
- **Backend:** https://github.com/lautaro-peralta/TGS-Backend
- **Frontend:** https://github.com/Tsplivalo/TGS-Frontend

**Para verificar estado de submódulos:**
```bash
git submodule status
# Output esperado:
#  abc1234... apps/backend (commit-hash)
#  def5678... apps/frontend (commit-hash)
```

---

## Tecnologías

### Backend
- **Runtime:** Node.js 18+
- **Lenguaje:** TypeScript
- **Framework:** Express.js
- **ORM:** MikroORM
- **Base de Datos:** PostgreSQL 16
- **Cache:** Redis 7
- **Autenticación:** JWT
- **Logger:** Pino
- **Documentación API:** Swagger/OpenAPI

### Frontend
- **Framework:** Angular 18+
- **Lenguaje:** TypeScript
- **Estilos:** SCSS
- **Proxy:** Proxy a Backend local

### Infraestructura Local
- **Contenedores:** Docker + Docker Compose
- **Control de Versiones:** Git con Submódulos

### Infraestructura Producción
- **Base de Datos:** Neon.tech (PostgreSQL 16 serverless)
- **Cache:** Redis Cloud (Redis 7)
- **Backend Hosting:** Render (Node.js containers)
- **Frontend Hosting:** Vercel (Edge Network)

### Testing & CI/CD
- **Backend Testing:** Jest con cobertura
- **Frontend Testing:** Karma + Jasmine con cobertura
- **Integración Continua:** GitHub Actions (workflows automáticos)
- **Documentación:** Markdown con versionado en repositorios

---

## Requisitos Previos

### Software Obligatorio
- **Node.js** >= 18 LTS
- **pnpm** >= 9 (gestor de paquetes)
- **Docker** >= 24 y **Docker Compose** >= 2
- **Git** (con soporte para submódulos)

### Verificar instalación
```bash
node --version       # v18+
pnpm --version       # 9+
docker --version     # 24+
docker compose version
git --version
```

### Recursos mínimos
- 2 GB RAM disponible
- 500 MB espacio en disco
- Puertos libres: 3000 (backend), 4200 (frontend), 5432 (PostgreSQL), 6379 (Redis)

---

## Configuración Inicial

### 1. Clonar el repositorio con Submódulos

```bash
git clone --recurse-submodules https://github.com/lautaro-peralta/GarrSYS.git
cd GarrSYS
```

**Si ya clonaste sin `--recurse-submodules`:**
```bash
git submodule update --init --recursive
```

### 2. Levantar Infraestructura (PostgreSQL + Redis)

```bash
cd infra
docker compose up -d
```

Verificar que los servicios estén corriendo:
```bash
docker compose ps
# Deberías ver:
# NAME       STATUS
# postgres   Up (healthy)
# redis      Up
```

### 3. Configurar Backend

```bash
cd ../apps/backend
pnpm install
cp .env.example .env.development
```

**Editar `.env.development` con estos valores:**

| Variable | Valor Por Defecto | Descripción |
|----------|-------------------|-------------|
| `NODE_ENV` | `development` | Ambiente |
| `PORT` | `3000` | Puerto API |
| `DB_HOST` | `localhost` | Host PostgreSQL |
| `DB_PORT` | `5432` | Puerto PostgreSQL |
| `DB_USER` | `postgres` | Usuario DB |
| `DB_PASSWORD` | `postgres` | Password DB |
| `DB_NAME` | `tpdesarrollo` | Nombre BD |
| `REDIS_ENABLED` | `true` | Habilitar caché |
| `REDIS_HOST` | `localhost` | Host Redis |
| `REDIS_PORT` | `6379` | Puerto Redis |
| `JWT_SECRET` | `dev-secret-key` | Clave JWT (cambiar en prod) |
| `EMAIL_VERIFICATION_REQUIRED` | `false` | Modo demo (sin verificación) |
| `SMTP_HOST` | `sandbox.smtp.mailtrap.io` | Server SMTP |
| `SMTP_PORT` | `2525` | Puerto SMTP |
| `SMTP_USER` | `tu-usuario` | Usuario Mailtrap |
| `SMTP_PASS` | `tu-password` | Password Mailtrap |

```env
# .env.development - Minimal para evaluadores
NODE_ENV=development
PORT=3000

# Database
DB_HOST=localhost
DB_PORT=5432
DB_USER=postgres
DB_PASSWORD=postgres
DB_NAME=tpdesarrollo

# Redis (opcional)
REDIS_ENABLED=false

# JWT
JWT_SECRET=dev-secret-key-change-in-production

# Email - Modo demo (sin verificación obligatoria)
EMAIL_VERIFICATION_REQUIRED=false
SMTP_HOST=sandbox.smtp.mailtrap.io
SMTP_PORT=2525
SMTP_USER=tu-usuario
SMTP_PASS=tu-password
```

### 4. Configurar Frontend

```bash
cd ../frontend
pnpm install
```

El frontend ya está preconfigurado con proxy al backend en `http://localhost:3000` mediante `proxy.conf.json`. **No requiere cambios adicionales.**

---

## Ejecución Local

### Orden recomendada

#### Paso 1: Verificar infraestructura Docker
```bash
cd infra
docker compose ps
```
Espera a que `postgres` esté "healthy" (puede tardar 10-15 segundos).

#### Paso 2: Iniciar Backend
```bash
cd apps/backend
pnpm start:dev
```

**Salida esperada:**
```
[11:25:33.123] INFO (server): Starting application initialization...
[11:25:33.456] INFO (server): Initializing critical services (email, scheduler)...
[11:25:34.789] INFO (server): Application initialization completed successfully
[11:25:34.890] INFO (server): Server running on http://localhost:3000/ [development]
```

**Endpoints de verificación:**
- Health: `curl http://localhost:3000/health`
- API Docs: http://localhost:3000/api-docs

#### Paso 3: Iniciar Frontend (nueva terminal)
```bash
cd apps/frontend
pnpm start
```

**Salida esperada:**
```
✔ Compiled successfully.
✔ Built successfully.

** Angular Live Development Server is listening on localhost:4200 **
```

**Acceder:** http://localhost:4200

---

## Usuarios de Prueba

Después de cargar los datos de prueba, puedes ingresar con estas credenciales:

| Rol | Email | Password | Funciones |
|-----|-------|----------|-----------|
| **Admin** | `thomas.shelby@shelbyltd.co.uk` | `password123` | Acceso total, gestión de usuarios |
| **Partner 1** | `arthur.shelby@shelbyltd.co.uk` | `password123` | Consejo, decisiones |
| **Partner 2** | `polly.gray@shelbyltd.co.uk` | `password123` | Consejo, decisiones |
| **Distributor 1** | `john.shelby@shelbyltd.co.uk` | `password123` | Ventas, inventario |
| **Distributor 2** | `michael.gray@shelbyltd.co.uk` | `password123` | Ventas, inventario |
| **Distributor 3** | `isaiah.jesus@shelbyltd.co.uk` | `password123` | Ventas, inventario |
| **Client 1** | `alfie@solomonsltd.co.uk` | `password123` | Compras |
| **Client 2** | `johnny@example.com` | `password123` | Compras |
| **Client 3** | `aberama@goldltd.com` | `password123` | Compras |
| **Authority 1** | `campbell@birminghampd.gov.uk` | `password123` | Inspecciones |
| **Authority 2** | `moss@birminghampd.gov.uk` | `password123` | Inspecciones |

**Datos de prueba incluidos:**
- 5 zonas de operación
- 10 productos (legales e ilegales)
- 12 usuarios (múltiples roles)
- 4 ventas de ejemplo
- 3 sobornos registrados

---

## Cargar Datos de Prueba

### Opción 1: Script automático (Recomendado)

Desde la **raíz del proyecto:**
```bash
# Linux/Mac/Git Bash
bash scripts/load-test-data.sh

# Windows (CMD o PowerShell)
scripts\load-test-data.bat

# O con Make
make load-data
```

### Opción 2: Comando directo

```bash
cd apps/backend
node scripts/seed-test-data.mjs
```

### Verificar que los datos se cargaron

```bash
# Acceder a Swagger
curl http://localhost:3000/api-docs

# O probar login
curl -X POST http://localhost:3000/auth/login \
  -H "Content-Type: application/json" \
  -d '{
    "email": "thomas.shelby@shelbyltd.co.uk",
    "password": "password123"
  }'
```

---

## Testing

### Backend

```bash
cd apps/backend

# Ejecutar todos los tests
pnpm test

# Tests con cobertura
pnpm test:cov

# Tests en modo watch (escucha cambios)
pnpm test:watch

# Tests específicos
pnpm test -- --testPathPattern=auth
```

### Frontend

```bash
cd apps/frontend

# Ejecutar tests unitarios
pnpm test

# Tests con cobertura
pnpm test:cov

# Build de producción (valida tipos)
pnpm build
```

### Verificación de tipos (TypeScript)

```bash
# Backend
cd apps/backend && pnpm type-check

# Frontend
cd apps/frontend && pnpm type-check
```

### CI/CD Automático

El proyecto incluye **workflows automáticos con GitHub Actions**:

- **Backend Tests:** Se ejecutan en cada push/PR al backend
- **Frontend Tests:** Se ejecutan en cada push/PR al frontend
- **Integración Completa:** Tests de full-stack cuando hay cambios en ambos
- **Deploy a Producción:** Automático desde branch principal

Consulta los workflows en: `.github/workflows/` de cada submódulo

---

## Documentación

### Documentación del Sistema

- **[Propuesta del Proyecto](docs/proposal.md)** - Requerimientos funcionales y alcance
- **[Swagger/OpenAPI](http://localhost:3000/api-docs)** - Documentación interactiva de endpoints (requiere backend corriendo)

### Documentación de Componentes

Cada submódulo tiene su propio README:

- **[README Backend](apps/backend/README.md)** - Arquitectura, endpoints, modelos
- **[README Frontend](apps/frontend/README.md)** - Componentes, servicios, estructura

### Principales Endpoints

**Autenticación**
- `POST /auth/register` - Registrar usuario
- `POST /auth/login` - Iniciar sesión
- `POST /auth/refresh` - Renovar token JWT

**Gestión de Productos**
- `GET /products` - Listar productos
- `POST /products` - Crear producto
- `GET /products/:id` - Obtener detalles
- `PUT /products/:id` - Actualizar producto

**Gestión de Ventas**
- `GET /sales` - Listar ventas
- `POST /sales` - Registrar venta
- `GET /sales/:id` - Detalles de venta

**Usuarios**
- `GET /users` - Listar usuarios
- `GET /users/:id` - Detalles de usuario
- `PUT /users/:id` - Actualizar usuario

**Ver todos los endpoints en Swagger:** http://localhost:3000/api-docs

---

## Troubleshooting

### Error: "Cannot connect to PostgreSQL"

**Síntomas:** Backend falla con `Error: connect ECONNREFUSED 127.0.0.1:5432`

**Soluciones:**
1. Verifica que Docker está corriendo y PostgreSQL está iniciado:
   ```bash
   docker compose ps
   docker compose logs postgres
   ```
2. Verifica que el puerto 5432 está libre:
   ```bash
   # Windows
   netstat -ano | findstr :5432
   
   # Linux/Mac
   lsof -i :5432
   ```
3. Verifica credenciales en `.env.development`:
   ```env
   DB_HOST=localhost
   DB_PORT=5432
   DB_USER=postgres
   DB_PASSWORD=postgres
   DB_NAME=tpdesarrollo
   ```
4. Si todo falla, recrea la infraestructura:
   ```bash
   cd infra
   docker compose down -v
   docker compose up -d
   ```

---

### Error: "Redis connection failed"

**Síntomas:** Backend logs muestran `Error: connect ECONNREFUSED 127.0.0.1:6379`

**Soluciones:**
1. **Opción A (Recomendado):** Desactiva Redis (es opcional):
   ```env
   REDIS_ENABLED=false
   ```
2. **Opción B:** Verifica que Redis está corriendo:
   ```bash
   docker compose ps redis
   docker compose logs redis
   ```

---

### Error: "Port 3000/4200 already in use"

**Síntomas:** `Error: listen EADDRINUSE: address already in use :::3000`

**Soluciones:**
1. Encuentra qué proceso usa el puerto:
   ```bash
   # Windows
   netstat -ano | findstr :3000
   
   # Linux/Mac
   lsof -i :3000
   ```
2. Cierra el proceso o cambia el puerto en `.env.development`:
   ```env
   PORT=3001
   ```
3. Para el frontend, puedes usar:
   ```bash
   ng serve --port 4201
   ```

---

### Error: "Submódulos vacíos" / "apps/backend no existe"

**Síntomas:** Las carpetas `apps/backend` y `apps/frontend` están vacías

**Soluciones:**
```bash
# Descargar submódulos
git submodule update --init --recursive

# O fuerza sincronización
git submodule sync --recursive
git submodule update --init --recursive
```

---

### Error: "CORS policy" en el frontend

**Síntomas:** Console muestra `Access to XMLHttpRequest blocked by CORS policy`

**Soluciones:**
1. Verifica que el backend está corriendo en `http://localhost:3000`
2. Verifica `proxy.conf.json` en el frontend:
   ```json
   {
     "/api": {
       "target": "http://localhost:3000",
       "pathRewrite": { "^/api": "" },
       "changeOrigin": true
     }
   }
   ```
3. Verifica variable en backend `.env.development`:
   ```env
   ALLOWED_ORIGINS=http://localhost:4200
   ```

---

### Error: "La base de datos está vacía"

**Síntomas:** Login falla aunque backend esté corriendo

**Solución:** Carga los datos de prueba:
```bash
make load-data
# O manualmente:
cd apps/backend && node scripts/seed-test-data.mjs
```

---

### Error: "Versión de Node.js incorrecta"

**Síntomas:** `Node.js v16 not supported` o similar

**Solución:**
```bash
# Verificar versión actual
node --version

# Si no tienes Node.js 18+, instálalo:
# Opción A: Desde nodejs.org
# Opción B: Con nvm (recomendado)
nvm install 18
nvm use 18
```

---

### Error: "pnpm not found"

**Síntomas:** `command not found: pnpm`

**Solución:**
```bash
npm install -g pnpm@latest
pnpm --version  # Verificar
```

---

### Limpiar todo y empezar de nuevo

Si tienes problemas persistentes:

```bash
# 1. Detener todo
cd infra
docker compose down -v

# 2. Limpiar backend
cd ../apps/backend
rm -rf node_modules dist
pnpm install

# 3. Limpiar frontend
cd ../frontend
rm -rf node_modules .angular dist
pnpm install

# 4. Levantar infraestructura de nuevo
cd ../../infra
docker compose up -d

# 5. Iniciar backend y frontend (en diferentes terminales)
cd ../apps/backend && pnpm start:dev
cd ../apps/frontend && pnpm start
```

---

### Comandos Útiles Rápidos

```bash
# Verificar estado de todo
make status

# Ver logs de Docker
docker compose logs -f

# Ejecutar migraciones de BD
cd apps/backend && pnpm mikro-orm migration:up

# Build de producción
cd apps/frontend && pnpm build

# Type checking
cd apps/backend && pnpm type-check
cd ../frontend && pnpm type-check
```

---

## Equipo

**Grupo Shelby - UTN FRRo**

| Nombre | Legajo | GitHub | Responsabilidades |
|--------|--------|--------|-------------------|
| **Peralta, Lautaro Martín** | 53483 | [@lautaro-peralta](https://github.com/lautaro-peralta) | **Líder del Proyecto** - Desarrollo Backend (API REST, Autenticación, BD), Infraestructura Local (Docker, PostgreSQL, Redis), Puesta en Producción (Neon.tech, Render, Vercel), Diseño Arquitectónico, Gestión de Submódulos y Coordinación General |
| **Splivalo, Tomas** | 51665 | [@Tsplivalo](https://github.com/Tsplivalo) | Frontend (Angular SPA), Debugging Backend, Integración Full-Stack, Optimización de Rendimiento |
| **Delprato, Luca** | 50215 | [@LucaDelpra](https://github.com/LucaDelpra) | Testing & QA, CI/CD (GitHub Actions), Automatización de Workflows |

---

## 🔗 Repositorios

- **Principal (Este):** [lautaro-peralta/GarrSYS](https://github.com/lautaro-peralta/GarrSYS)
- **Backend (Submódulo):** [lautaro-peralta/TGS-Backend](https://github.com/lautaro-peralta/TGS-Backend)
- **Frontend (Submódulo):** [Tsplivalo/TGS-Frontend](https://github.com/Tsplivalo/TGS-Frontend)

---

**Materia:** Desarrollo de Software | **Universidad:** UTN FRRo | **Año:** 2025
