# Informe de Instalación - Motores de Base de Datos (IA Lab)

**Servidor:** Ubuntu / WSL2
**Ruta Base:** `~/ia-lab/services/motores-bd/`

## 1. Motores Desplegados
- **MySQL 8.0**: Puerto `3306` | Usuario `admin` / `admin123` | BD `tecnogua`
- **PostgreSQL 17**: Puerto `5433` | Usuario `admin` / `admin123` | BD `ialab`
- **SQL Server 2022**: Puerto `1433` | Usuario `sa` / `Admin123*`
- **Oracle XE**: Puerto `1521` | Usuario `SYSTEM` / `Admin123*` | BD `XE`

## 2. Estado de los Contenedores

```text
NAMES                 STATUS                          PORTS
oracle-xe             Restarting (1) 17 seconds ago   
sqlserver-container   Restarting (1) 16 seconds ago   
ia-postgres           Up 8 minutes                    0.0.0.0:5433->5432/tcp, [::]:5433->5432/tcp
mysql-server          Up 9 minutes                    0.0.0.0:3306->3306/tcp, [::]:3306->3306/tcp, 33060/tcp
```
