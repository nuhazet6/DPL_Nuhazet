
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
Para recoger el valor de los parámetros uso una serie de inputs en el html. El código html sería el siguiente:
``` 
<!DOCTYPE html>
<html lang="en">
<head>
    <meta charset="UTF-8">
    <meta name="viewport" content="width=device-width, initial-scale=1.0">
    <title>images</title>
    <script type="text/javascript" src="./images.js"></script>
</head>
<body>
    <h1>Nginx + ngx_small_light</h1> 
    
    <form method="POST">
        <label for="size">Tamaño:</label><br>
        <input type="text" id="size" name="tamaño"><br>
        <label for="border">Ancho del borde:</label><br>
        <input type="text" id="border" name="borde"><br><br>
        <label for="border_color">Color del borde:</label>
        <input type="color" id="border_color" name="color-borde" value="#000000"><br><br>
        <label for="sharpen">Enfoque:</label>
        <input type="text" id="sharpen" name="sharpen"><br><br>
        <label for="blur">Desenfoque:</label>
        <input type="text" id="blur" name="desenfoque">  
        <input type="button" value="Generar" onclick="get_images()" />
    </form>
    <h2>Imágenes</h2>
    <div id="imgs"></div>
</body>
</html>
```
Y el código js sería el siguiente:
``` 
function get_images(num_images=20){
    let size = document.getElementById("size").value;
    let border = document.getElementById("border").value;
    let border_color = document.getElementById("border_color").value.slice(1);
    let sharpen = document.getElementById("sharpen").value;
    let blur = document.getElementById("blur").value;
    let div_imgs = document.getElementById("imgs");
    let nginx_get = `?dw=${size.padStart(1,'0')}&dh=${size.padStart(1,'0')}&bh=${border.padStart(1,'0')}&bw=${border.padStart(1,'0')}&bc=${border_color.padEnd(6,'0')}&sharpen=${sharpen.padStart(1,'0')}&blur=${blur.padStart(1,'0')}`
    if(div_imgs.hasChildNodes()){
        
        div_imgs.childNodes.forEach(image => {
            image.src = `img/image${image.id}.jpg${nginx_get}`
        });
    }
    else{
    for(let i = 1; i < num_images; i++){
        img = document.createElement('img');
        img_indx = `${i}`.padStart(2,'0');
        img.id = img_indx;
        img.src = `img/image${img_indx}.jpg${nginx_get}`;
        div_imgs.appendChild(img);
    }
    }
}
```
De la forma en la que está hecho, si no hay contenido en el div "imgs" se crean los elementos img, que contendrán las rutas con los parámetros correspondientes a las imágenes sirviendo dichas imágenes modificadas por el módulo. En caso de que haya contenido, se sustituye la propiedad src de cada elemento img para asignar la ya mencionada ruta.

##### Dockerizando
Para crear la imagen de docker vamos a usar un Dockerfile. Es una especie de receta para instalar todo lo necesario en una imagen de docker. 
Primero como en la práctica anterior creamos una estructura para el docker:



#### ***Conclusiones***. <a name="id5"></a>
Es interesante saber instalar y utilizar módulos con nginx, en concreto sobre este módulo llama la atención cómo se pueden servir imágenes modificadas sin necesidad de alterar las propias imágenes originales. 
Por otra parte, también es interesante ver cómo podemos facilitar la instalación del módulo creando un dockerfile o una imagen docker la cual podríamos compartir con gente para que pudieran acceder más gente a dicho módulo y así agrandar una posible comunidad y soporte del mismo.  
