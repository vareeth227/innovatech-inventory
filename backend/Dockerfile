# ETAPA 1: instalar dependencias
FROM node:22-alpine AS deps
WORKDIR /app
# Copia solo package.json para aprovechar cache de Docker
COPY package.json .
RUN npm install --omit=dev

# ETAPA 2: imagen final liviana
FROM node:22-alpine AS runner
WORKDIR /app
# Copia dependencias desde etapa anterior
COPY --from=deps /app/node_modules ./node_modules
# Copia el código fuente
COPY src ./src
COPY package.json .
# Crea usuario sin privilegios para seguridad
RUN adduser -D appuser
USER appuser
# Puerto que expone el backend
EXPOSE 3001
# Comando para iniciar
CMD ["node", "src/server.js"]