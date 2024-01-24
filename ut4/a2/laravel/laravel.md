# ÍNDICE

+ [Material empleado](#id1)
+ [Desarrollo](#id2)

# ***Material empleado***. <a name="id1"></a>

- Nginx
- PHP
- Composer
- PostgreSQL + datos
- pgAdmin
- CertBot

## ***Desarrollo***. <a name="id2"></a>

Creamos el proyecto con: composer create-project laravel/laravel travelroad_laravel (En mi caso lo cree dentro de un repositorio en el home de mi usuario y luego lo moví a /usr/share/nginx)  
Esto tendrá que generar un .gitignore y un .env con una app_key (En caso de que no, generarlo manualmente)
Cambiamos el .env quedando con el siguiente contenido:  

![imagen](img/1.png)  

Cambiamos los permisos del storage para el usuario nginx usando:  
``` sudo chgrp -R nginx storage bootstrap/cache ```  
``` sudo chmod -R ug+rwx storage bootstrap/cache ```
Cambiamos las vistas llendo a resources/views, dicho directorio tiene el siguiente contenido:  

![imagen](img/2.png)  

Las vistas tienen el siguiente contenido:  
travelroad.blade.php:  
![imagen](img/3.png)  
visited.blade.php:  
![imagen](img/4.png)  
wished.blade.php:  
![imagen](img/5.png)  

Volvemos a la raíz del proyecto y cambiamos routes/web.php con el siguiente contenido:  
![imagen](img/6.png)  

Creamos travelroad_laravel.conf en /etc/nginx/conf.d con el siguiente contenido:  
![imagen](img/7.png)  

Creamos un script para conectarse y actualizar la máquina de producción:
![imagen](img/8.png)  

En la máquina de producción bajamos y movemos a la ruta correspondiente el repositorio, creamos un .env a partir del .env.example modificando los valores por los que queramos usar en producción y generamos una app_key con ``` php artisan key:generate ```, creamos el .conf con el server_name "laravel.nuhazet.arkania.es", hacemos un cambio en desarrollo (añadimos - Changed al h1 de la vista travelroad) y ejecutamos el script. Nos conectamos para comprobar que todo funciona:  
![imagen](img/9.png)  

Ahora agregamos el certificado usando certbot ``` sudo certbot --nginx ``` y seleccionamos el número que corresponda a laravel.nuhazet.arkania.es. Luego creamos otro .conf para la redireccion www con el siguiente contenido:  
![imagen](img/10.png)  
y le agregamos el certbot ejecutando el mismo comando pero seleccionando el número correspondiente.
Quedaría tal que así:  
![imagen](img/11.png)  
![imagen](img/12.png)  
