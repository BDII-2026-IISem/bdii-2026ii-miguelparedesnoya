**UNIVERSIDAD DE LA GUAJIRA**

**Informe de Base de Datos - Estructura de Datos 2**

**Datos del estudiante:**

Estudiante: Miguel Ángel Paredes Noya

Programa: Ingeniería de Sistemas

Asignatura: Estructura de Datos 2

Año: 2026

# 1. Introducción

El presente informe académico documenta el trabajo realizado durante el
corte actual en la asignatura de Estructura de Datos 2. Se detalla la
implementación, exploración y gestión de la base de datos
lecturaabierta, utilizando motores de bases de datos como MySQL 8.0 y
PostgreSQL 17 dentro de un entorno contenedorizado.

# 2. Objetivos

## 2.1 Objetivo general

Implementar y gestionar la base de datos lecturaabierta en múltiples
motores de bases de datos en un entorno Docker, documentando las
estructuras y evidencias del corte actual.

## 2.2 Objetivos específicos

\- Verificar la disponibilidad de los contenedores Docker (mysql-server
y ia-postgres).

\- Confirmar la estructura de 10 tablas en la base de datos
lecturaabierta.

\- Documentar las herramientas de gestión como DBeaver y terminales de
comandos.

\- Extraer e integrar diagramas ER comprobados del esquema de datos.

# 3. Entorno y herramientas utilizadas

Ubicación del proyecto: \~/ia-lab-anterior/services/motores-bd/

Contenedores activos comprobados:

\- mysql-server --- MySQL 8.0 --- puerto 3306

\- ia-postgres --- PostgreSQL 17 --- puerto 5433

\- sqlserver-container --- MS SQL Server 2022 --- puerto 1433

# 4. Base de datos lecturaabierta

La base de datos lecturaabierta cuenta con 10 tablas comprobadas en
MySQL y PostgreSQL:

  -----------------------------------------------------------------------
  Tabla                               Descripción
  ----------------------------------- -----------------------------------
  autor                               Información de autores

  categoria                           Categorías de libros

  ejemplar                            Ejemplares disponibles

  lector                              Información de lectores

  libro                               Información de libros

  libro_autor                         Relación entre libros y autores

  multa                               Multas asociadas a préstamos

  prestamo                            Préstamos realizados

  reserva                             Reservas de libros

  sede                                Sedes de la biblioteca
  -----------------------------------------------------------------------

# 5. Estructura comprobada de las tablas

Campos comprobados en MySQL:

autor: id (bigint, PK, auto_increment), nombre (varchar(150)),
descripcion (text), is_active (tinyint(1)), created_at (timestamp),
updated_at (timestamp).

lector: id (bigint, PK, auto_increment), nombre (varchar(150)),
descripcion (text), is_active (tinyint(1)), created_at (timestamp),
updated_at (timestamp).

reserva: id (bigint, PK, auto_increment), cliente_id (bigint, índice),
libro_id (bigint, índice), fecha_inicio (datetime), fecha_fin
(datetime), estado (varchar(50)), observaciones (text).

(Se mantienen 10 tablas estructuradas con llaves primarias
autoincrementables e índices).

# 6. Relaciones entre las tablas

# 6. Relaciones entre las tablas

Las relaciones de la base de datos lecturaabierta fueron verificadas
directamente en MySQL mediante la consulta de las claves foráneas
definidas en el esquema.

Las relaciones encontradas son las siguientes:

  ------------------------------------------------------------------------
  **Tabla**     **Columna**      **Tabla            **Columna
                                 relacionada**      relacionada**
  ------------- ---------------- ------------------ ----------------------
  ejemplar      libro_id         libro              id

  ejemplar      sede_id          sede               id

  libro         categoria_id     categoria          id

  libro_autor   principal_id     libro              id

  libro_autor   relacionado_id   autor              id

  multa         prestamo_id      prestamo           id

  prestamo      ejemplar_id      ejemplar           id

  prestamo      lector_id        lector             id

  reserva       cliente_id       lector             id

  reserva       libro_id         libro              id
  ------------------------------------------------------------------------

### Descripción de las relaciones

-   Un **libro** pertenece a una **categoría**, mediante
    libro.categoria_id.

-   Un **ejemplar** pertenece a un **libro**, mediante
    ejemplar.libro_id.

-   Un **ejemplar** está asociado a una **sede**, mediante
    ejemplar.sede_id.

-   La tabla libro_autor relaciona los **libros** con los **autores**,
    utilizando principal_id para el libro y relacionado_id para el
    autor.

-   Un **préstamo** está asociado a un **lector** mediante
    prestamo.lector_id.

-   Un **préstamo** corresponde a un **ejemplar** mediante
    prestamo.ejemplar_id.

-   Una **multa** está asociada a un **préstamo** mediante
    multa.prestamo_id.

-   Una **reserva** está asociada a un **lector** mediante
    reserva.cliente_id.

-   Una **reserva** está asociada a un **libro** mediante
    reserva.libro_id.

Estas relaciones permiten conectar las diferentes entidades del sistema
de gestión de la biblioteca y mantener la integridad referencial de los
datos.

# 7. Diagrama de la base de datos

A continuación se presentan los diagramas de entidad-relación exportados
directamente desde DBeaver para cada motor:

![](media/image1.png){width="5.5in" height="7.106194225721785in"}

**Figura 1. Diagrama de Entidad-Relación en MySQL (DBeaver)**

![](media/image2.png){width="5.989583333333333in"
height="5.436111111111111in"}

**Figura 2. Diagrama de Entidad-Relación en PostgreSQL (DBeaver)**

# 8. Implementación en MySQL

En MySQL 8.0 se comprobó la base de datos lecturaabierta y las 10 tablas
activas en el puerto 3306:

![](media/image3.png){width="5.5in" height="5.755417760279965in"}

**Figura 3. Estructura de tablas de lecturaabierta en MySQL (DBeaver -
tecnogua:3306)**

# 9. Implementación en PostgreSQL

En PostgreSQL 17 se confirmó la base lecturaabierta en el puerto 5433:

![](media/image4.png){width="5.5in" height="3.595469160104987in"}

**Figura 4. Conexión y base de datos lecturaabierta en PostgreSQL
(DBeaver - localhost:5433)**

# 10. Implementación en Microsoft SQL Server

Contenedor sqlserver-container activo en puerto 1433 (Evidencias de
tablas pendientes).

# 11. Comparación de los motores

MySQL 8.0 (Puerto 3306) vs PostgreSQL 17 (Puerto 5433). Ambos manejan la
misma estructura conceptual de 10 tablas para lecturaabierta.

# 12. Evidencias

Capturas integradas en las secciones 7, 8 y 9.

# 13. Problemas encontrados y soluciones

Ningún problema crítico durante la verificación de conectividad por
puerto.

# 14. Conclusiones

Se completó la verificación multomotor de la base lecturaabierta,
consolidando las evidencias visuales e infraestructura en Docker.
