
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

Desplegar una aplicación web con una configuración concreta que haga uso de algunas funcionalidades del módulo de Nginx "ngx_small_light"

#### ***Material empleado***. <a name="id3"></a>

- Nginx
- Docker
- ngx_small_light instalado y configurado en el nginx.conf

#### ***Desarrollo***. <a name="id4"></a>
##### Archivo .conf
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
##### Utilización del módulo
Para la correcta utilización del módulo es recomendable ir a la documentación del mismo, en este caso vamos a usar el módulo a través de peticiones get. La forma de trabajo para estas peticiones es construir una url donde la primera parte es la ruta hacia la imagen y la segunda es una serie de parámetros, por ejemplo:
``` /img/image01.jpg?dw=400&dh=400&bh=10&bw=10&bc=8ff0a4&sharpen=5x10&blur=1x10 ```
Los diferentes parámetros que se pueden usar se pueden consultar en la documentación del módulo.
Para construir la url por una parte he puesto una serie de labels

#### ***Conclusiones***. <a name="id5"></a>
