# UNIVERSIDAD DE LA GUAJIRA

## Informe de instalación y configuración de motores de bases de datos

**Estudiante:** Miguel Ángel Paredes Noya  
**Asignatura:** Estructura de Datos 2  
**Entorno:** WSL 2 + Ubuntu + Docker + Docker Compose  
**Fecha:** Septiembre de 2026

> **Nota:** Este informe se organiza siguiendo la guía del docente para la creación de cuatro motores de base de datos con Docker Compose: MySQL, PostgreSQL, MS SQL Server y Oracle XE, bajo `~/ia-lab-anterior/services/motores-bd/`.

---

## 1. Introducción

En este informe se presenta el proceso realizado para instalar, configurar y verificar diferentes motores de bases de datos utilizando un entorno virtualizado mediante WSL 2, Ubuntu y Docker.

El trabajo se realizó con el propósito de disponer de un entorno local en el que fuera posible ejecutar varios sistemas gestores de bases de datos de manera independiente mediante contenedores. Los motores utilizados fueron MySQL, PostgreSQL, Microsoft SQL Server y Oracle XE.

También se realizó la configuración de persistencia de datos, redes Docker y conexiones mediante DBeaver, con el fin de comprobar que los motores pudieran ser utilizados desde una herramienta gráfica de administración.

---

## 2. Objetivos

### 2.1 Objetivo general

Instalar y configurar diferentes motores de bases de datos utilizando WSL 2, Ubuntu, Docker y Docker Compose, comprobando posteriormente su funcionamiento y conexión mediante DBeaver.

### 2.2 Objetivos específicos

- Preparar un entorno de trabajo utilizando WSL 2 y Ubuntu.
- Instalar y comprobar Docker y Docker Compose.
- Crear una estructura organizada para los servicios de bases de datos.
- Configurar MySQL 8.0.
- Configurar PostgreSQL 17.
- Configurar Microsoft SQL Server 2022.
- Configurar Oracle XE.
- Utilizar Docker Compose para administrar los contenedores.
- Configurar almacenamiento persistente para los datos.
- Crear una red Docker para facilitar la comunicación entre servicios.
- Comprobar el estado de los contenedores.
- Realizar conexiones mediante DBeaver.
- Documentar los problemas encontrados y las soluciones aplicadas.

---

## 3. Tecnologías utilizadas

| Tecnología | Uso |
|---|---|
| Windows | Sistema operativo principal |
| WSL 2 | Subsistema para ejecutar Linux |
| Ubuntu | Distribución Linux utilizada |
| Docker | Ejecución de los motores mediante contenedores |
| Docker Compose | Administración de los servicios |
| DBeaver | Administración y conexión a las bases de datos |
| MySQL 8.0 | Motor de base de datos |
| PostgreSQL 17 | Motor de base de datos |
| SQL Server 2022 | Motor de base de datos |
| Oracle XE | Motor de base de datos |

---

## 4. Preparación del entorno

### 4.1 Verificación de WSL

El primer paso fue comprobar que WSL estuviera instalado y que la distribución de Ubuntu utilizara la versión 2.

Se trabajó desde Windows y posteriormente se ingresó al terminal de Ubuntu.

La estructura general del proyecto quedó ubicada en:

```bash
~/ia-lab-anterior/
```

Dentro de esta carpeta se creó la estructura:

```text
ia-lab/
└── services/
    └── motores-bd/
        ├── mysql/
        ├── postgres/
        ├── mssql/
        ├── oracle/
        └── proces.md
```

Además, se utilizó una carpeta para almacenar los datos persistentes:

```text
~/ia-lab-anterior/data/
```

---

## 5. Instalación y comprobación de Docker

Después de preparar Ubuntu se instaló Docker y se comprobó que el servicio estuviera disponible.

La versión comprobada de Docker fue:

```text
Docker 29.8.0
```

También se comprobó Docker Compose:

```text
Docker Compose v5.5.1
```

Para verificar Docker se utilizó:

```bash
docker --version
docker compose version
docker ps
```

El comando `docker ps` permitió comprobar que Docker estaba funcionando correctamente.

También se verificó que el usuario pudiera utilizar Docker sin tener que escribir `sudo` en cada comando.

---

## 6. Organización de los motores

Para mantener organizado el laboratorio se creó una carpeta independiente para cada motor:

```bash
cd ~/ia-lab-anterior/services/motores-bd
```

La estructura utilizada fue:

```text
motores-bd/
├── mysql/
│   ├── docker-compose.yml
│   └── .env
├── postgres/
│   └── docker-compose.yml
├── mssql/
│   └── docker-compose.yml
├── oracle/
│   └── docker-compose.yml
└── proces.md
```

Esta organización permite administrar cada motor de manera independiente.

---

# 7. MySQL — MySQL 8.0

## 7.1 Imagen utilizada

Para MySQL se utilizó la imagen oficial:

```text
mysql:8.0
```

El contenedor se configuró con el nombre:

```text
mysql-server
```

La base de datos utilizada durante las pruebas fue:

```text
tecnogua
```

---

## 7.2 Variables de entorno

Para evitar colocar directamente las contraseñas dentro del archivo `docker-compose.yml`, se utilizó un archivo `.env`.

Ejemplo de la configuración:

```env
MYSQL_ROOT_PASSWORD=********
MYSQL_DATABASE=tecnogua
TZ=America/Bogota
```

La contraseña utilizada debe mantenerse privada y no debe publicarse en GitHub.

---

## 7.3 Configuración de Docker Compose

La configuración de MySQL utiliza la imagen `mysql:8.0`, un volumen para conservar los datos y una red Docker.

La estructura general utilizada fue:

```yaml
services:
  mysql:
    image: mysql:8.0
    container_name: mysql-server
    restart: unless-stopped

    environment:
      MYSQL_ROOT_PASSWORD: ${MYSQL_ROOT_PASSWORD}
      MYSQL_DATABASE: ${MYSQL_DATABASE}
      TZ: ${TZ}

    ports:
      - "3306:3306"

    volumes:
      - ~/ia-lab-anterior/data/mysql:/var/lib/mysql

    networks:
      - ia-lab-network

networks:
  ia-lab-network:
    external: true
```

> Las contraseñas y otros datos sensibles no deben incluirse en el repositorio público.

---

## 7.4 Inicio del contenedor

Para iniciar MySQL se ingresó a su carpeta:

```bash
cd ~/ia-lab-anterior/services/motores-bd/mysql
```

Luego se ejecutó:

```bash
docker compose up -d
```

Para comprobar el estado:

```bash
docker ps
```

También se revisaron los registros cuando fue necesario:

```bash
docker logs mysql-server
```

Finalmente, el contenedor quedó funcionando correctamente.

---

## 7.5 Prueba desde MySQL

Se ingresó al cliente de MySQL dentro del contenedor:

```bash
docker exec -it mysql-server mysql -u root -p
```

Después de ingresar la contraseña se realizó la comprobación:

```sql
SHOW DATABASES;
```

Entre las bases de datos apareció:

```text
tecnogua
```

También se comprobó la base de datos seleccionada:

```sql
SELECT DATABASE();
```

El resultado correspondió a:

```text
tecnogua
```

Esto permitió confirmar que MySQL estaba instalado y que la base de datos se había creado correctamente.

---

## 7.6 Problema presentado durante la configuración de MySQL

Durante el proceso se presentó un problema con los archivos de datos de MySQL. Después de varios intentos de reinicio, el contenedor entró en un ciclo de errores relacionados con InnoDB.

Se realizó una copia de seguridad del directorio de datos:

```text
~/ia-lab-anterior/data/mysql-backup
```

También se intentó realizar un proceso de recuperación utilizando un contenedor temporal, pero la recuperación no permitió iniciar correctamente el sistema de almacenamiento de InnoDB.

Debido a este problema se decidió realizar una instalación limpia de MySQL, conservando previamente la copia de seguridad.

Posteriormente se inició nuevamente el contenedor con una estructura de datos limpia y se comprobó que MySQL funcionara correctamente.

Este proceso permitió identificar la importancia de realizar copias de seguridad antes de modificar o eliminar los directorios de datos de un motor de base de datos.

---

# 8. PostgreSQL — PostgreSQL 17

## 8.1 Imagen utilizada

Para PostgreSQL se descargó y utilizó:

```text
postgres:17
```

La configuración se realizó mediante Docker Compose.

La carpeta correspondiente es:

```bash
~/ia-lab-anterior/services/motores-bd/postgres
```

---

## 8.2 Configuración general

La estructura utilizada para PostgreSQL contempla:

- Imagen PostgreSQL 17.
- Nombre independiente para el contenedor.
- Variables de entorno para usuario, contraseña y base de datos.
- Puerto para conexión externa.
- Volumen para persistencia.
- Red `ia-lab-network`.

Ejemplo:

```yaml
services:
  postgres:
    image: postgres:17
    container_name: ia-postgres
    restart: unless-stopped

    environment:
      POSTGRES_USER: postgres
      POSTGRES_PASSWORD: ********
      POSTGRES_DB: tecnogua

    ports:
      - "5433:5432"

    volumes:
      - ~/ia-lab-anterior/data/postgres:/var/lib/postgresql/data

    networks:
      - ia-lab-network

networks:
  ia-lab-network:
    external: true
```

---

## 8.3 Inicio y comprobación

Desde la carpeta del servicio:

```bash
cd ~/ia-lab-anterior/services/motores-bd/postgres
```

Se puede iniciar el servicio mediante:

```bash
docker compose up -d
```

Luego se verifica:

```bash
docker ps
```

Para revisar los registros:

```bash
docker logs ia-postgres
```

---

# 9. MS SQL Server — SQL Server 2022

## 9.1 Imagen utilizada

Para Microsoft SQL Server se utilizó:

```text
mcr.microsoft.com/mssql/server:2022-latest
```

El contenedor se configuró para trabajar con SQL Server 2022.

La carpeta correspondiente es:

```bash
~/ia-lab-anterior/services/motores-bd/mssql
```

---

## 9.2 Configuración general

La configuración contempla:

- SQL Server 2022.
- Aceptación de la licencia.
- Contraseña del usuario administrador.
- Puerto `1433`.
- Persistencia de datos.
- Red Docker.

Ejemplo:

```yaml
services:
  mssql:
    image: mcr.microsoft.com/mssql/server:2022-latest
    container_name: sqlserver-container
    restart: unless-stopped

    environment:
      ACCEPT_EULA: "Y"
      MSSQL_SA_PASSWORD: "********"

    ports:
      - "1433:1433"

    volumes:
      - ~/ia-lab-anterior/data/mssql:/var/opt/mssql

    networks:
      - ia-lab-network

networks:
  ia-lab-network:
    external: true
```

---

## 9.3 Inicio y comprobación

Se ingresa a la carpeta:

```bash
cd ~/ia-lab-anterior/services/motores-bd/mssql
```

Se inicia el servicio:

```bash
docker compose up -d
```

Y se verifica:

```bash
docker ps
```

Los registros pueden revisarse mediante:

```bash
docker logs sqlserver-container
```

---

# 10. Oracle — Oracle XE

## 10.1 Imagen utilizada

Para Oracle se utilizó la imagen:

```text
gvenzl/oracle-xe
```

Esta imagen permite ejecutar Oracle Database Express Edition mediante Docker.

La carpeta correspondiente es:

```bash
~/ia-lab-anterior/services/motores-bd/oracle
```

---

## 10.2 Configuración general

La configuración contempla:

- Oracle XE.
- Puerto `1521` para conexión de base de datos.
- Puerto `8080` para la interfaz web cuando corresponde.
- Persistencia de datos.
- Red Docker.

Ejemplo general:

```yaml
services:
  oracle:
    image: gvenzl/oracle-xe
    container_name: oracle-xe
    restart: unless-stopped

    ports:
      - "1521:1521"
      - "8080:8080"

    volumes:
      - ~/ia-lab-anterior/data/oracle:/opt/oracle/oradata

    networks:
      - ia-lab-network

networks:
  ia-lab-network:
    external: true
```

---

## 10.3 Inicio y comprobación

Se ingresa a:

```bash
cd ~/ia-lab-anterior/services/motores-bd/oracle
```

Se inicia:

```bash
docker compose up -d
```

Se comprueba:

```bash
docker ps
```

Y para consultar los registros:

```bash
docker logs oracle-xe
```

---

# 11. Red Docker

Para permitir la comunicación entre los servicios se planteó el uso de una red externa llamada:

```text
ia-lab-network
```

La red puede comprobarse con:

```bash
docker network ls
```

Si todavía no existe, puede crearse con:

```bash
docker network create ia-lab-network
```

Después, los servicios pueden asociarse a esta red desde Docker Compose.

La utilización de una red común permite que los contenedores puedan comunicarse entre ellos mediante sus nombres de servicio o nombres de contenedor, sin depender únicamente de las direcciones IP.

---

# 12. Persistencia de los datos

Una parte importante de la configuración fue utilizar volúmenes para que los datos no dependieran exclusivamente de la vida del contenedor.

La estructura utilizada fue:

```text
~/ia-lab-anterior/data/
├── mysql/
├── postgres/
├── mssql/
└── oracle/
```

De esta manera, los datos de cada motor se almacenan fuera del sistema de archivos interno del contenedor.

Esto permite que, al eliminar o recrear un contenedor, los datos puedan mantenerse siempre que el directorio persistente no sea eliminado.

---

# 13. Conexión mediante DBeaver

Después de configurar los motores se utilizó DBeaver para realizar conexiones desde una interfaz gráfica.

Para una conexión local desde Windows hacia los puertos publicados por Docker se utilizan los siguientes datos generales:

| Motor | Host | Puerto |
|---|---|---:|
| MySQL | localhost | 3306 |
| PostgreSQL | localhost | 5433 |
| SQL Server | localhost | 1433 |
| Oracle | localhost | 1521 |

Los usuarios y contraseñas dependen de la configuración realizada para cada motor.

---

## 13.1 Conexión a MySQL

En DBeaver se selecciona MySQL y se utilizan:

```text
Host: localhost
Port: 3306
Database: tecnogua
User: root
Password: contraseña configurada
```

Durante las pruebas se presentó inicialmente un problema de conexión relacionado con:

```text
Public Key Retrieval is not allowed
```

Este inconveniente se solucionó ajustando la configuración de conexión de MySQL en DBeaver.

Posteriormente se pudo realizar la conexión y acceder a la base de datos.

---

## 13.2 Conexión a PostgreSQL

Los datos generales son:

```text
Host: localhost
Port: 5433
Database: tecnogua
User: postgres
Password: contraseña configurada
```

La conexión debe comprobarse mediante el botón de prueba de DBeaver.

---

## 13.3 Conexión a SQL Server

Los datos generales son:

```text
Host: localhost
Port: 1433
Database: master
User: sa
Password: contraseña configurada
```

La conexión se realiza utilizando el controlador de SQL Server disponible en DBeaver.

---

## 13.4 Conexión a Oracle

Los datos generales dependen del servicio/SID configurado por la imagen de Oracle.

Como punto inicial se utiliza:

```text
Host: localhost
Port: 1521
User: usuario configurado
Password: contraseña configurada
```

En DBeaver se debe seleccionar el tipo de conexión correspondiente y especificar el servicio de Oracle utilizado por el contenedor.

---

# 14. Comandos principales utilizados

Durante el desarrollo del laboratorio se utilizaron comandos de Docker y Linux.

### Verificar Docker

```bash
docker --version
docker compose version
docker ps
```

### Ver imágenes descargadas

```bash
docker images
```

### Iniciar un servicio

```bash
docker compose up -d
```

### Detener un servicio

```bash
docker compose down
```

### Ver contenedores

```bash
docker ps
docker ps -a
```

### Ver registros

```bash
docker logs NOMBRE_CONTENEDOR
```

### Entrar a un contenedor MySQL

```bash
docker exec -it mysql-server mysql -u root -p
```

### Ver redes

```bash
docker network ls
```

### Crear la red del laboratorio

```bash
docker network create ia-lab-network
```

### Ver imágenes

```bash
docker images
```

---

# 15. Verificación del laboratorio

La comprobación final debe realizarse verificando que los cuatro motores estén disponibles y que sus respectivos contenedores aparezcan correctamente mediante:

```bash
docker ps
```

La tabla esperada del laboratorio es:

| Motor | Contenedor | Puerto |
|---|---|---:|
| MySQL 8.0 | mysql-server | 3306 |
| PostgreSQL 17 | ia-postgres | 5433 |
| SQL Server 2022 | sqlserver-container | 1433 |
| Oracle XE | oracle-xe | 1521 |

Las capturas de pantalla de esta sección deben mostrar el resultado real obtenido en el terminal.

---

# 16. Evidencias del procedimiento

Las evidencias deben demostrar cada etapa solicitada en la guía del docente. Se recomienda agregar las capturas reales tomadas durante la instalación y configuración.

## 16.1 Requisitos previos

- [ ] WSL 2 funcionando.
- [ ] Ubuntu funcionando.
- [ ] Docker instalado.
- [ ] Docker Compose instalado.

**Captura:** versión de Docker y Docker Compose.

## 16.2 Estructura de carpetas

Debe demostrarse la estructura:

```text
~/ia-lab-anterior/
├── services/
│   └── motores-bd/
│       ├── mysql/
│       ├── postgres/
│       ├── mssql/
│       └── oracle/
└── data/
    ├── mysql/
    ├── postgres/
    ├── mssql/
    └── oracle/
```

**Captura:** comando `tree` mostrando la estructura.

## 16.3 Red Docker compartida

Comando utilizado:

```bash
docker network inspect ia-lab-network >/dev/null 2>&1 || docker network create ia-lab-network
```

Verificación:

```bash
docker network ls | grep ia-lab
```

**Captura:** resultado de la red `ia-lab-network`.

## 16.4 MySQL

Agregar evidencias de:

- `docker-compose.yml`
- `.env` sin mostrar contraseñas
- `docker compose up -d`
- contenedor funcionando
- conexión local
- creación del usuario propio con acceso remoto
- conexión remota
- backup
- conexión desde DBeaver

## 16.5 PostgreSQL

Agregar evidencias de:

- `docker-compose.yml`
- `.env` sin mostrar contraseñas
- `docker compose up -d`
- contenedor funcionando
- conexión local
- usuario propio con acceso remoto
- conexión remota
- backup
- conexión desde DBeaver

## 16.6 MS SQL Server

Agregar evidencias de:

- `docker-compose.yml`
- `.env` sin mostrar contraseñas
- `docker compose up -d`
- contenedor funcionando
- instalación de `mssql-tools`
- conexión local
- usuario propio con acceso remoto
- conexión remota
- backup
- conexión desde DBeaver

## 16.7 Oracle XE

Agregar evidencias de:

- `docker-compose.yml`
- `.env` sin mostrar contraseñas
- `docker compose up -d`
- contenedor funcionando
- conexión local
- usuario propio con acceso remoto
- conexión remota
- backup
- conexión desde DBeaver

## 16.8 Verificación final

La comprobación final debe mostrar los cuatro contenedores activos mediante:

```bash
docker ps
```

La tabla esperada es:

| Motor | Contenedor | Puerto |
|---|---|---:|
| MySQL 8.0 | mysql-server | 3306 |
| PostgreSQL 17 | ia-postgres | 5433 |
| SQL Server 2022 | sqlserver-container | 1433 |
| Oracle XE | oracle-xe | 1521 |

> Los nombres y puertos deben coincidir con la configuración realmente utilizada. No se deben colocar resultados que no hayan sido comprobados.

---

# 17. Problemas encontrados y soluciones

## 17.1 Problemas durante la descarga de imágenes

Durante el proceso se presentaron problemas temporales de conexión al descargar algunas imágenes de Docker. Se realizaron nuevos intentos hasta completar las descargas.

## 17.2 Problema de datos de MySQL

MySQL presentó errores relacionados con InnoDB después de reinicios y problemas con el directorio de datos.

Antes de realizar cambios se creó una copia:

```text
~/ia-lab-anterior/data/mysql-backup
```

La recuperación mediante un contenedor temporal no permitió recuperar correctamente el sistema de almacenamiento.

Finalmente se realizó una instalación limpia de MySQL y se comprobó nuevamente la creación de la base `tecnogua`.

## 17.3 Problema de conexión de DBeaver con MySQL

Al realizar la conexión se presentó el mensaje:

```text
Public Key Retrieval is not allowed
```

Se ajustaron las propiedades de la conexión en DBeaver y posteriormente se logró realizar la conexión correctamente.

---

# 18. Importancia de Docker en el laboratorio

Docker facilitó la instalación de los motores porque cada uno puede ejecutarse dentro de su propio contenedor.

Esto evita tener que instalar directamente todos los motores en Ubuntu y permite mantener separados sus archivos, configuraciones y dependencias.

Docker Compose también permite guardar la configuración de cada servicio en archivos `docker-compose.yml`, facilitando que los servicios puedan ser iniciados nuevamente mediante comandos sencillos.

---

# 19. Conclusiones

Durante el desarrollo del laboratorio se preparó un entorno utilizando WSL 2, Ubuntu y Docker para trabajar con diferentes motores de bases de datos.

Se trabajó con MySQL 8.0, PostgreSQL 17, Microsoft SQL Server 2022 y Oracle XE. Cada motor fue organizado en su propia carpeta y configurado mediante Docker Compose.

Uno de los aspectos más importantes del proceso fue la configuración de persistencia de datos, ya que permite separar la información de las bases de datos del ciclo de vida de los contenedores.

También se comprobó la utilidad de DBeaver como herramienta para administrar y conectar los diferentes motores desde una interfaz gráfica.

Durante la instalación de MySQL se presentó un problema relacionado con los archivos de InnoDB. Este inconveniente permitió aplicar un proceso de respaldo y posteriormente realizar una instalación limpia para recuperar el funcionamiento del servicio.

En conclusión, el laboratorio permitió adquirir experiencia práctica en la instalación, configuración, administración y conexión de diferentes sistemas gestores de bases de datos utilizando tecnologías de virtualización mediante contenedores.

---

# 20. Referencias

- Documentación oficial de Docker.
- Documentación oficial de Docker Compose.
- Documentación oficial de MySQL.
- Documentación oficial de PostgreSQL.
- Documentación oficial de Microsoft SQL Server.
- Documentación de Oracle Database.
- Documentación de DBeaver.

---

## Anexo: estructura final del proyecto

```text
ia-lab/
├── data/
│   ├── mysql/
│   ├── postgres/
│   ├── mssql/
│   └── oracle/
│
└── services/
    └── motores-bd/
        ├── mysql/
        │   ├── docker-compose.yml
        │   └── .env
        │
        ├── postgres/
        │   └── docker-compose.yml
        │
        ├── mssql/
        │   └── docker-compose.yml
        │
        ├── oracle/
        │   └── docker-compose.yml
        │
        └── proces.md
```

**Nota:** Las contraseñas, claves y demás credenciales deben mantenerse fuera del repositorio de GitHub. El archivo `.env` debe agregarse al `.gitignore`.
