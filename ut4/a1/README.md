
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
- Intalación y configuración PHP+nginx

#### ***Desarrollo***. <a name="id4"></a>
##### Instalación PostgreSQL
![imagen1](img/1.png)
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
###### Dependencias  
Aseguramos que la ruta a los binarios de las herramientas ejecutables en línea de comandos que instala Python está en el PATH e instalamos Python:  
![imagen6](img/6.png)  
[Instalación Python](https://github.com/sdelquin/edubase/blob/main/docs/python-install-linux.md)  
![imagen7](img/7.png)  
###### Instalación
Creamos las carpetas de trabajo con los permisos adecuados, creamos un entorno virtual de Python (lo activamos) e instalamos el paquete pgadmin4:  
![Imagen8](img/8.png)  
Ahora lanzamos el script de configuración en el que tendremos que dar credenciales para una cuenta "master", en caso de dar error instalar setuptools ``` pip install setuptools ``` :  
![Imagen9](img/9.png)  
Para poder lanzar el servidor pgAdmin en modo producción y con garantías, necesitaremos hacer uso de un procesador de peticiones WSGI denominado gunicorn:  
![Imagen10](img/10.png)
Levantamos el servidor pgAdmin utilizando gunicorn:
![Imagen11](img/11.png)  
###### Virtualhost en Nginx
Creamos el virtual host en nginx ``` sudo nano /etc/nginx/conf.d/pgadmin.conf ``` con el siguiente contenido:
![Imagen12](img/12.png)  
####### Demonizar el servicio
Creamos un servicio del sistema ``` sudo nano /etc/systemd/system/pgadmin.service ``` con el siguiente contenido:
![Imagen13](img/13.png)  
Recargamos los servicios para luego levantar pgAdmin y habilitarlo en caso de reinicio del sistema, por último comprobamos que el servicio está funcionando correctamente:
![Imagen14](img/14.png)  
Accedemos a pgadmin y registramos un servidor con el nombre TravelRoad
![Imagen15](img/15.png)  
![Imagen16](img/16.png)  
###### Acceso externo
Añadimos en el archivo ``` sudo nano /etc/postgresql/16/main/postgresql.conf ``` lo siguiente en la línea 64:  
![Imagen17](img/17.png)  
Y añadimos al final del archivo ``` sudo nano /etc/postgresql/16/main/pg_hba.conf ``` lo siguiente:  
![Imagen18](img/18.png)  
Reiniciamos el servicio de Postgre y comprobamos que el servicio ya está escuchando en todas las IPs, si no tenemos instalado netstat tendremos que instalarlo con ``` sudo netstat -napt | grep postgres | grep -v tcp6 ```:  
![Imagen19](img/19.png)  
##### Laravel
###### Instalación  
###### Composer
Instalamos Composer (se necesita PHP ``` sudo apt install php ```) y comprobamos version:  
![Imagen20](img/20.png)  
Instalamos ciertos módulos con: ``` sudo apt install -y php8.2-mbstring php8.2-xml \
php8.2-bcmath php8.2-curl php8.2-pgsql ```  
Creamos el proyecto: ```composer create-project laravel/laravel travelroad ``` (necesita zip y 7zip ``` sudo apt install zip 7zip ``` )  
Cambiamos el .env:  
![Imagen21](img/21.png)  
###### Configuración Nginx
Configuramos permisos para ciertas carpetas y el host de nginx para nuestra aplicación:  
```
sudo chgrp -R nginx storage bootstrap/cache
sudo chmod -R ug+rwx storage bootstrap/cache
sudo nano /etc/nginx/conf.d/travelroad.conf
```  
![Imagen22](img/22.png)  

mover instalación de framework a /usr/share/
#### ***Conclusiones***. <a name="id5"></a>

