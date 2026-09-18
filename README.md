# 📚 Banco de Datos II — Motores de Bases de Datos

**Universidad de La Guajira**

**Estudiante:** Miguel Ángel Paredes Noya
**Asignatura:** Estructura de Datos 2
**Entorno:** WSL 2 + Ubuntu + Docker + Docker Compose
**Año:** 2026

---

## 📌 Descripción

Este repositorio contiene los archivos, configuraciones y documentación relacionados con el trabajo desarrollado en la asignatura **Estructura de Datos 2**.

El proyecto utiliza **Docker y Docker Compose** sobre **Ubuntu mediante WSL 2** para configurar diferentes motores de bases de datos.

Los motores utilizados son:

* 🐬 **MySQL 8.0**
* 🐘 **PostgreSQL 17**
* 🪟 **Microsoft SQL Server 2022**
* 🔶 **Oracle XE**

También se realizaron pruebas de conexión mediante **DBeaver** y se documentó el proceso de instalación y configuración.

---

## 🛠️ Tecnologías utilizadas

| Tecnología     | Uso                                            |
| -------------- | ---------------------------------------------- |
| WSL 2          | Entorno Linux sobre Windows                    |
| Ubuntu         | Sistema operativo utilizado                    |
| Docker         | Ejecución de los motores mediante contenedores |
| Docker Compose | Administración de los servicios                |
| DBeaver        | Administración y conexión a las bases de datos |
| Git            | Control de versiones                           |
| GitHub         | Almacenamiento del proyecto                    |

---

## 🗄️ Motores de Bases de Datos

| Motor                | Versión | Puerto |
| -------------------- | ------: | -----: |
| MySQL                |     8.0 |   3306 |
| PostgreSQL           |      17 |   5433 |
| Microsoft SQL Server |    2022 |   1433 |
| Oracle XE            |      XE |   1521 |

Las credenciales de los servicios se encuentran almacenadas en archivos `.env` y no forman parte del repositorio público.

---

## 📁 Estructura del proyecto

```text
bdii-2026ii-miguelparedesnoya/
│
├── services/
│   └── motores-bd/
│       ├── informe/
│       │   └── informe.md
│       │
│       ├── mysql/
│       │   ├── .env
│       │   └── docker-compose.yml
│       │
│       ├── postgres/
│       │   ├── .env
│       │   └── docker-compose.yml
│       │
│       ├── mssql/
│       │   ├── .env
│       │   └── docker-compose.yml
│       │
│       ├── oracle/
│       │   ├── .env
│       │   └── docker-compose.yml
│       │
│       ├── lectores_100_clientes_v2 (1).csv
│       └── proces.md
│
├── .gitignore
└── README.md
```

> Los archivos `.env` se encuentran excluidos de Git mediante `.gitignore` para evitar publicar información sensible.

---

## 📊 Datos de prueba

El proyecto contiene el archivo:

```text
lectores_100_clientes_v2 (1).csv
```

Este archivo contiene los datos utilizados durante las actividades realizadas con los motores de bases de datos.

---

## 📄 Documentación

La documentación principal se encuentra en:

```text
services/motores-bd/informe/informe.md
```

También se incluye:

```text
services/motores-bd/proces.md
```

con información relacionada con la instalación y configuración de los motores.

---

## 🐳 Docker Compose

Cada motor cuenta con su propio archivo `docker-compose.yml`.

Ejemplo:

```bash
cd services/motores-bd/mysql
docker compose up -d
```

Para consultar los contenedores:

```bash
docker ps -a
```

Para detener un servicio:

```bash
docker compose down
```

---

## 🔐 Seguridad

Los archivos `.env` contienen información de configuración y credenciales necesarias para los servicios.

Estos archivos están incluidos en `.gitignore` y **no deben subirse al repositorio público**.

---

## 👨‍💻 Autor

**Miguel Ángel Paredes Noya**

**Universidad de La Guajira — 2026**

