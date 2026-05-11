# ETAPA 1: construir la app React
FROM node:22-alpine AS builder
WORKDIR /app
# Copia package.json para instalar dependencias
COPY package.json .
RUN npm install
# Copia el código fuente
COPY . .
# Construye la app para producción
RUN npm run build

# ETAPA 2: servir con nginx liviano
FROM nginx:alpine AS runner
# Copia el build generado al servidor nginx
COPY --from=builder /app/dist /usr/share/nginx/html
# Copia configuración personalizada de nginx
COPY nginx.conf /etc/nginx/conf.d/default.conf
# Puerto que expone el frontend
EXPOSE 80
CMD ["nginx", "-g", "daemon off;"]