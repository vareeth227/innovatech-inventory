## EP3 - Orquestación con ECS Fargate

### Arquitectura AWS

- **Cluster ECS:** `innovatech-cluster` (Fargate)
- **Frontend:** servicio ECS con ALB público `innovatech-alb-617625291.us-east-1.elb.amazonaws.com`
- **Backend:** servicio ECS con ALB interno `innovatech-backend-alb-17953016.us-east-1.elb.amazonaws.com:3001`
- **Base de datos:** RDS MySQL `innovatech-db.cetrfq3r5lka.us-east-1.rds.amazonaws.com`
- **Registro de imágenes:** ECR `691996033181.dkr.ecr.us-east-1.amazonaws.com`

### Pipeline CI/CD

El pipeline se activa con push en la rama `deploy`:

```bash
git checkout deploy
git merge main
git push origin deploy
```

Esto ejecuta automáticamente:

1. Login a ECR con credenciales AWS
2. Build de imagen Docker con variables de entorno
3. Push a ECR
4. Force new deployment en ECS

### Secrets requeridos en GitHub

| Secret                  | Descripción                 |
| ----------------------- | --------------------------- |
| `AWS_ACCESS_KEY_ID`     | Credencial AWS              |
| `AWS_SECRET_ACCESS_KEY` | Credencial AWS              |
| `AWS_SESSION_TOKEN`     | Token de sesión AWS Academy |
| `ECR_REGISTRY`          | URL del registro ECR        |
| `VITE_API_URL`          | URL del ALB del backend     |
