
<center>

# UT4-A1: Implantación de arquitecturas web


</center>

***Nombre:*** Nuhazet Correa Torres
***Curso:*** 2º de Ciclo Superior de Desarrollo de Aplicaciones Web.

### ÍNDICE

+ [Introducción](#id1)
+ [Objetivos](#id2)
+ [Material empleado](#id3)
+ [Desarrollo](#id4)
+ [Conclusiones](#id5)


#### ***Introducción***. <a name="id1"></a>

Práctica sobre el despliegue de una aplicación con PostgreSQL y Laravel

#### ***Objetivos***. <a name="id2"></a>

Instalar y configurar PostgreSQL y pgAdmin, desarrollar una aplicación para mostrar los datos y crear un script que actualice la máquina de producción con los cambios realizados en desarrollo

#### ***Material empleado***. <a name="id3"></a>

- Nginx
- [Datos empleados](https://github.com/sdelquin/dpl/blob/main/ut4/files/places.csv)
- Python

#### ***Desarrollo***. <a name="id4"></a>
##### Instalación PostgreSQL
![imagen1](img/2.png)
###### Configuración
Primero iniciamos el cliente psql con el usuario habilitado por defecto para acceder a la administración de la base de datos:  
![imagen2](img/2.png)  
Con el usuario predeterminado creamos un usuario específico para asignarlo únicamente a una base de datos y salimos del cliente psql para volver a iniciar sesión más adelante con el usuario en cuestión:  
![imagen3](img/3.png)  
Iniciamos sesión con el usuario travelroad_user Y creamos una tabla donde pondremos los datos:  
![imagen4](img/4.png)  
###### Carga de Datos
Descargamos los datos, los copiamos con copy a la base de datos y comprobamos ls datos entrando y usando SELECT:  
![imagen5](img/5.png)  
###### pgAdmin
####### Dependencias
Aseguramos que la ruta a los binarios de las herramientas ejecutables en línea de comandos que instala Python está en el PATH e instalamos Python:  
![imagen6](img/6.png)  
[Instalación Python](https://github.com/sdelquin/edubase/blob/main/docs/python-install-linux.md)  
![imagen7](img/7.png)  
####### Instalación
Creamos las carpetas de trabajo con los permisos adecuados, creamos un entorno virtual de Python (lo activamos) e instalamos el paquete pgadmin4:  
![Imagen8](img/8.png)  
Ahora lanzamos el script de configuración en el que tendremos que dar credenciales para una cuenta "master":  
![Imagen9](img/9.png)  
Para poder lanzar el servidor pgAdmin en modo producción y con garantías, necesitaremos hacer uso de un procesador de peticiones WSGI denominado gunicorn:  
![Imagen10](img/10.png)
Levantamos el servidor pgAdmin utilizando gunicorn:
![Imagen11](img/11.png)


#### ***Conclusiones***. <a name="id5"></a>

