# Innovatech DevOps Frontend

Frontend desarrollado para el proyecto Innovatech Chile utilizando React y Vite.  
Este sistema forma parte de una arquitectura basada en microservicios, conectándose con servicios backend implementados con Spring Boot y desplegados mediante Docker.

---

# Tecnologías utilizadas

- React
- Vite
- JavaScript
- Axios
- TailwindCSS
- Docker
- Nginx
- Docker Compose
- GitHub Actions

---

# Descripción

La aplicación permite gestionar y visualizar información relacionada con:

- órdenes de compra
- órdenes de despacho
- seguimiento de entregas
- productos
- usuarios

El frontend se ejecuta dentro de un contenedor Docker utilizando Nginx para servir los archivos compilados en producción.

---

# Arquitectura

El frontend se comunica con los siguientes microservicios:

| Servicio | Puerto |
|---|---|
| Despachos | 8081 |
| Inventario | 8082 |
| Ventas | 8083 |

Todos los servicios son administrados mediante Docker Compose.

---

# Estructura del proyecto

```txt
front_despacho
├── public
├── src
├── package.json
├── vite.config.js
├── Dockerfile
├── .dockerignore
└── README.md
```

---

# Docker

El proyecto utiliza un Dockerfile multi-stage:

## Etapa 1
Compilación del proyecto usando Node.js.

## Etapa 2
Despliegue utilizando Nginx Alpine para servir el build generado por Vite.

---

# Dockerfile utilizado

```dockerfile
FROM node:20 AS build

WORKDIR /app

COPY package*.json ./

RUN npm install

COPY . .

RUN npm run build

FROM nginx:alpine

COPY --from=build /app/dist /usr/share/nginx/html

EXPOSE 80

CMD ["nginx", "-g", "daemon off;"]
```

---

# Construir imagen Docker

Desde la carpeta `front_despacho`:

```bash
docker build -t innovatech-frontend .
```

---

# Ejecutar contenedor individual

```bash
docker run -d --name innovatech-frontend -p 80:80 innovatech-frontend
```

Acceso:

```txt
http://localhost
```

---

# Ejecución con Docker Compose

El frontend se integra al stack completo desde el repositorio backend utilizando:

```bash
docker compose up -d
```

Servicios utilizados:

- innovatech-frontend
- innovatech-despachos
- innovatech-inventario
- innovatech-ventas
- innovatech-mysql

---

# Variables y comunicación

El frontend consume APIs REST desplegadas en contenedores Docker mediante la red:

```txt
innovatech-network
```

---

# Acceso a servicios

## Frontend

```txt
http://localhost
```

## Swagger Despachos

```txt
http://localhost:8081/swagger-ui/index.html
```

## Swagger Inventario

```txt
http://localhost:8082/swagger-ui/index.html
```

## Swagger Ventas

```txt
http://localhost:8083/swagger-ui/index.html
```

---

# Docker Compose

El sistema completo utiliza:

- contenedores independientes
- network bridge
- persistencia con volumes
- variables de entorno
- comunicación interna Docker

---

# GitHub Actions

El proyecto fue preparado para integración continua utilizando GitHub Actions y DockerHub.

Secrets utilizados:

```txt
DOCKERHUB_USERNAME
DOCKERHUB_TOKEN
```

---

# Consideraciones DevOps

El sistema fue diseñado considerando:

- contenerización completa
- arquitectura de microservicios
- separación frontend/backend
- persistencia de datos
- automatización CI/CD
- despliegue cloud-ready

---

# Autores

- Martín Díaz
- Diego Diaz
