# Informe de Consultas SQL y Estructura de la Base de Datos

Se ejecutó la consulta `SHOW TABLES;` para identificar las tablas disponibles en la base de datos y comprobar su estructura general.

![Estructura general de la base de datos y SHOW TABLES](input_file_0.png)

```sql
USE lecturaabierta;
SHOW TABLES;
```

---

## Estructura de las Tablas (`DESCRIBE`)

### Tabla: `autor`
```sql
DESC autor;
```
![Estructura de la tabla autor](input_file_1.png)

### Tabla: `categoria`
```sql
DESCRIBE categoria;
```
![Estructura de la tabla categoria](input_file_2.png)

### Tabla: `ejemplar`
```sql
DESCRIBE ejemplar;
```
![Estructura de la tabla ejemplar](input_file_3.png)

### Tabla: `lector`
```sql
DESCRIBE lector;
```
![Estructura de la tabla lector](input_file_4.png)

### Tabla: `libro_autor`
```sql
DESCRIBE libro_autor;
```
![Estructura de la tabla libro_autor](input_file_5.png)

### Tabla: `multa` / `prestamo`
```sql
DESCRIBE prestamo;
```
![Estructura de la tabla prestamo](input_file_6.png)

### Tabla: `reserva`
```sql
DESCRIBE reserva;
```
![Estructura de la tabla reserva](input_file_7.png)

### Tabla: `sede`
```sql
DESCRIBE sede;
```
![Estructura de la tabla sede](input_file_8.png)

---

## Consultas de Selección y Filtrado

### 1. Mostrar los 100 lectores
```sql
SELECT * FROM lector;
```
![Mostrar todos los lectores](input_file_9.png)

### 2. Contar los lectores
```sql
SELECT COUNT(*) AS total_lectores
FROM lector;
```
![Contar lectores](input_file_10.png)

### 3. Mostrar solamente algunas columnas
Esto permite demostrar que podemos seleccionar columnas específicas en lugar de mostrar toda la tabla.

```sql
SELECT id, nombre
FROM lector;
```
![Seleccionar columnas id y nombre](input_file_11.png)

### 4. Ordenar los lectores
Esto muestra los lectores ordenados alfabéticamente por nombre.

```sql
SELECT id, nombre
FROM lector
ORDER BY nombre ASC;
```
![Ordenar lectores alfabéticamente](input_file_12.png)

### 5. Buscar lectores activos
Esta consulta muestra únicamente los lectores activos.

```sql
SELECT id, nombre, is_active
FROM lector
WHERE is_active = 1;
```
![Buscar lectores activos](input_file_13.png)

### 6. Buscar lectores inactivos
Esta consulta permite identificar los lectores que se encuentran inactivos.

```sql
SELECT id, nombre, is_active
FROM lector
WHERE is_active = 0;
```
![Buscar lectores inactivos - Parte 1](input_file_14.png)
![Buscar lectores inactivos - Parte 2](input_file_15.png)

### 7. Contar lectores activos
```sql
SELECT COUNT(*) AS lectores_activos
FROM lector
WHERE is_active = 1;
```
![Contar lectores activos](input_file_16.png)

### 8. Buscar por nombre
La consulta utiliza `LIKE` para filtrar los registros cuyo nombre comienza con una letra determinada (en este caso, la letra 'A').

```sql
SELECT id, nombre
FROM lector
WHERE nombre LIKE 'A%';
```
![Buscar lectores que inician con A](input_file_17.png)

### 9. Mostrar fecha de creación de los lectores
```sql
SELECT id, nombre, created_at
FROM lector;
```
![Mostrar fecha de creación de los lectores](input_file_18.png)

### 10. Ordenar por fecha de creación
Ordena los lectores desde el registro con la fecha de creación más reciente hasta el más antiguo.

```sql
SELECT id, nombre, created_at
FROM lector
ORDER BY created_at DESC;
```
![Ordenar lectores por fecha de creación descendentemente](input_file_19.png)

### 11. Mostrar solo 10 lectores
Se utiliza `LIMIT` para mostrar únicamente los primeros 10 registros de la tabla lector.

```sql
SELECT id, nombre, created_at
FROM lector
LIMIT 10;
```
![Mostrar los primeros 10 lectores](input_file_20.png)

### 12. Mostrar los 10 lectores más recientes
La consulta ordena los registros por fecha de creación de forma descendente y muestra solamente los 10 más recientes.

```sql
SELECT id, nombre, created_at
FROM lector
ORDER BY created_at DESC
LIMIT 10;
```
![Mostrar los 10 lectores más recientes](input_file_21.png)

### 13. Combinar `WHERE`, `ORDER BY` y `LIMIT`
Se filtran los lectores activos, se ordenan alfabéticamente por nombre y se muestran los primeros 10 registros.

```sql
SELECT id, nombre, is_active, created_at
FROM lector
WHERE is_active = 1
ORDER BY nombre ASC
LIMIT 10;
```
![Combinar WHERE, ORDER BY y LIMIT](input_file_22.png)

### 14. Buscar nombres que contengan una letra
Aquí `%a%` significa que buscamos nombres que tengan la letra 'a' en cualquier posición.

```sql
SELECT id, nombre
FROM lector
WHERE nombre LIKE '%a%';
```
![Buscar nombres que contienen la letra a](input_file_23.png)

### 15. Contar lectores agrupados por estado
```sql
SELECT is_active, COUNT(*) AS cantidad
FROM lector
GROUP BY is_active;
```
![Contar lectores agrupados por estado](input_file_24.png)

### Consulta de comprobación general
Esta consulta nos dará en una sola fila:
* Total de lectores.
* Cantidad de lectores activos.
* Cantidad de lectores inactivos.

```sql
SELECT
    COUNT(*) AS total_lectores,
    SUM(is_active = 1) AS lectores_activos,
    SUM(is_active = 0) AS lectores_inactivos
FROM lector;
```
![Consulta de comprobación general](input_file_25.png)

---

## Verificación de DDL y Relaciones (`SHOW CREATE TABLE`)

### 16. Ver las relaciones de lector
Esta consulta nos permite ver cómo está definida la tabla y si tiene claves primarias, índices o claves foráneas.

```sql
SHOW CREATE TABLE lector;
```
![SHOW CREATE TABLE lector](input_file_26.png)

### 17. Revisar préstamo
Esta es especialmente importante porque `prestamo` debería relacionarse con `lector` y `ejemplar`.

```sql
SHOW CREATE TABLE prestamo;
```
![SHOW CREATE TABLE prestamo](input_file_27.png)

### 18. Revisar libro
```sql
SHOW CREATE TABLE libro;
```
![SHOW CREATE TABLE libro](input_file_28.png)

### 19. Revisar ejemplar
```sql
SHOW CREATE TABLE ejemplar;
```
![SHOW CREATE TABLE ejemplar](input_file_29.png)

### 20. Revisar libro_autor
```sql
SHOW CREATE TABLE libro_autor;
```
![SHOW CREATE TABLE libro_autor](input_file_30.png)

---

## Consulta Avanzada

### Subconsulta
```sql
SELECT id, nombre, created_at
FROM lector
WHERE id IN (
    SELECT id
    FROM lector
    WHERE is_active = 1
)
ORDER BY nombre ASC;
```