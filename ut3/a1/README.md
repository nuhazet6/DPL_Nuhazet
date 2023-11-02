
<center>

# UT3-A1: Implantación de arquitecturas web


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

Práctica sobre la utilización de módulos de nginx

#### ***Objetivos***. <a name="id2"></a>

Desplegar una aplicación web que haga uso del módulo de Nginx "ngx_small_light"

#### ***Material empleado***. <a name="id3"></a>

- Nginx
- Docker
- ngx_small_light

#### ***Desarrollo***. <a name="id4"></a>
##### Configuración del archivo .conf
Creamos un archivo .conf en la ruta /etc/nginx/conf.d/ con la configuración deseada, en este caso la mía es la siguiente:

``` 
server {
	server_name images.nuhazet.me;
	root /usr/share/nginx/html/ut3-te1;
	index index.html;

	location /img {
		small_light on;
		small_light_getparam_mode on;
	}
}
```
Elegimos como server_name images.nuhazet.me, ponemos como raíz del proyecto /usr/share/nginx/html/ut3-te1 y habilitamos el módulo small_light en modo get para la localización /img dentro del proyecto.


#### ***Conclusiones***. <a name="id5"></a>
