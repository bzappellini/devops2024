# Dockerfile para React
FROM node:16

# Establecer el directorio de trabajo
WORKDIR /app

# Copiar los archivos de dependencias y luego instalarlas
COPY ./package.json ./
RUN npm install

# Copiar el código fuente de la aplicación React
COPY . /app

# Construir la aplicación para producción
RUN npm run build

# Instalar serve para servir el frontend en producción
RUN npm install -g serve

# Exponer el puerto donde se servirá la app
EXPOSE 3000

# Comando para servir la aplicación
CMD ["serve", "-s", "build", "-l", "3000"]
