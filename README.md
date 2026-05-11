# Innovatech Inventory

Aplicación de gestión de inventario y tickets para Innovatech Chile.

## Arquitectura

- **Frontend**: React + Vite → EC2 pública (puerto 80)
- **Backend**: Node.js + Express → EC2 privada (puerto 3001)
- **Base de datos**: MySQL 8.0 → EC2 privada (puerto 3306)

## Requisitos

- Docker Desktop
- Docker Compose
- Git

## Levantar localmente

1. Clonar el repositorio:

```bash
git clone https://github.com/vareeth227/innovatech-inventory.git
cd innovatech-inventory
```

2. Crear archivo de variables de entorno:

```bash
cp .env.example .env
```

3. Levantar todos los servicios:

```bash
docker-compose up --build
```

4. Abrir en el navegador:

- Frontend: http://localhost
- Backend: http://localhost:3001/health

## Variables de entorno

| Variable              | Descripción                |
| --------------------- | -------------------------- |
| `MYSQL_DATABASE`      | Nombre de la base de datos |
| `MYSQL_USER`          | Usuario de MySQL           |
| `MYSQL_PASSWORD`      | Contraseña de MySQL        |
| `MYSQL_ROOT_PASSWORD` | Contraseña root de MySQL   |
| `CORS_ORIGIN`         | URL del frontend permitida |
| `VITE_API_URL`        | URL del backend            |

## Persistencia

Se usa **named volume** (`mysql_data`) para MySQL. Los datos persisten al reiniciar contenedores porque el volumen vive fuera del contenedor en el host de Docker.

## Pipeline CI/CD

El pipeline se activa con push en la rama `deploy`:

```bash
git checkout deploy
git merge main
git push origin deploy
```

Esto ejecuta automáticamente:

1. Build de imágenes Docker
2. Push a Docker Hub
3. Deploy en EC2 correspondiente

## Secrets requeridos en GitHub

| Secret               | Descripción                  |
| -------------------- | ---------------------------- |
| `DOCKERHUB_USERNAME` | Usuario Docker Hub           |
| `DOCKERHUB_TOKEN`    | Token Docker Hub             |
| `EC2_FRONTEND_HOST`  | IP pública EC2 frontend      |
| `EC2_BACKEND_HOST`   | IP pública EC2 backend       |
| `EC2_SSH_KEY`        | Clave SSH privada EC2        |
| `DB_HOST`            | IP privada EC2 base de datos |
| `MYSQL_DATABASE`     | Nombre base de datos         |
| `MYSQL_USER`         | Usuario base de datos        |
| `MYSQL_PASSWORD`     | Contraseña base de datos     |
| `CORS_ORIGIN`        | URL pública del frontend     |
