#  Despliegue de Oracle Database 19c en Docker  

## Descripción  
Esta documentación detalla los pasos para instalar y configurar Oracle Database 19c en un entorno Docker sobre Ubuntu. Se incluyen configuraciones de red, descarga de imágenes, creación de contenedores y conexión desde Oracle SQL Developer.  

## Requisitos  
- **Sistema operativo:** 

  - Ubuntu con Docker instalado  
- **Tarjetas de red:**

  - `enp0s3` (Adaptador Puente): Para obtener una IP dentro de la misma red que el host físico.  
  - `enp0s8` (Anfitrión): Para tener una red independiente para otros servicios.  
  
- **Software necesario:**  
  - Docker  
  - Git  
  - Oracle Database 19c  

---

## Instalación y Configuración  

### 1. Descargar las imágenes de Oracle
##### Clonar el repositorio de imágenes de Oracle:  
`git clone` https://github.com/oracle/docker-images.git

##### Esto descargará todo en la ruta: 
`/home/user/imagesdocker/docker-images`

---

### 2. Descargar y copiar Oracle Database 19c
##### Descargar la version 19.3.0 de Oracle:
[https://www.oracle.com/es/database/technologies/oracle19c-windows-downloads.html](https://www.oracle.com/es/database/technologies/oracle19c-linux-downloads.html

---

### 3. Crear la imagen de Docker
##### Ejecutar el script de construcción en la carpeta:
`cd /home/user/imagesdocker/docker-images/OracleDatabase/SingleInstance/dockerfiles/
./buildContainerImage.sh` 
`-v 19.3.0 -e -t oracledb`

##### Parámetros
`-v 19.3.0` - Especifica la versión

`-e` - Genera una imagen de Enterprise Edition

`-t oracledb` - Nombra la imagen como "oracledb"

---

### 4. Crear el contenedor
##### Comando para ejecutar el contenedor:

`docker run -it --name oracledb -p 1521:1521 -p 5500:5500 \
-e ORACLE_PDB=TESTDB -e ORACLE_PWD= [contraseña] \`
`-v oracledata:/opt/oracle/oradata oracledb`

##### Parámetros
`-it` - Permite visualizar el proceso en la terminal

`--name oracledb` - Nombre del contenedor

`-p 1521:1521` y `-p 5500:5500` - Pueros expuestos

`-e ORACLE_PDB=TESTDB` - Nombre de la base de datos

`-e ORACLE_PWD= [contraseña]` - Contraseña para el role SYS

`-v oracledata:/opt/oracle/oradata` - Crea un volumen llamado "oracledata"

##### Para iniciar el contenedor:
`docker start oracledb`

---
## Conectarse a Oracle SQL Database
### 1. Descargar Oracle SQL Develover
##### Link para descargar Oracle SQL Database:
https://www.oracle.com/database/sqldeveloper/

---

### 2. Obtener IP del contenedor
##### Obtener la IP 
`ifconfig` - Usar la IP de enp0s8 para conectarse a Oracle SQL Developer

---

### 3. Verificar si el puerto está abierto
##### Desde la terminal de Ubuntu ejecutar:
`telnet [IP de enp0s8] 1521`

---

### 4. Configurar SQL Developer
##### Conexión SYS (administrador)
###### Nueva conexión
`Name` - OracleDockerSYS

`Usuario` - sys

`Password` [constraseña]

`Nombre del Host` - [IP de enp0s8]

`Puerto` - 1521

`SID` - ORCLCDB

##### Una vez con la conexión establecida, cambiar la sesión a la base de datos TESTDB:
`ALTER SESSION SET CONTAINER=TESTDB;`

---

###  5. Crear un usuario y conceder permisos
##### Crear usuario
`CREATE USER [user] IDENTIFIED BY [contraseña];`
##### Conceder permisos
`GRANT CONNECT, RESOURCE TO [user];`
###### Nueva conexión con el usuario creado
`Name` - OracleDockerUser

`Usuario` - [user]

`Password` [constraseña]

`Nombre del Host` - [IP de enp0s8]

`Puerto` - 1521

`SID` - TESTDB

---

## Recursos adicionales
##### Guía en GitHub:
[Guía de GitHub](https://github.com/shazforiot/Oracle-Database-19c-on-Docker/blob/main/Steps "Guía de GitHub")
##### Video tutorial:
[Video Tuorial](https://www.youtube.com/watch?v=xY0Y1tWm8D4)

---

###### David Gutiérrez
###### Eder Aira
