# ÍNDICE

+ [Material empleado](#id1)
+ [Desarrollo](#id2)

# ***Material empleado***. <a name="id1"></a>

- Nginx
- PostgreSQL + datos
- pgAdmin
- CertBot

## ***Desarrollo***. <a name="id2"></a>
### Instalación 
``` curl -fsSL https://deb.nodesource.com/setup_current.x | sudo -E bash ```  
``` sudo apt install -y nodejs ```  
``` sudo npm install -g express-generator ```  
### Aplicación
estructura base con pug como motor de plantillas (en mi caso me situé en la carpeta que quería el proyecto y puse . como ruta para que se generara en dicha carpeta, en otro caso se tendría que indicar el nombre del proyecto en vez de poner .) :  
``` express --view=pug . ```  
Instalamos el gestor y algunas dependencias:  
``` npm install ```  
``` npm install pg ```  
``` npm install dotenv ```  
Creamos un .env.example con la estructura de la cadena de conexión a la base de datos y copiamos dicho archivo en un .env reemplazando por los datos sensibles.  
```
echo 'PSQL_CONNECTION=postgresql://ejemplo_usuario:ejemplo_contraseña@localhost:puerto' > .env.example
 ```  
creamos el archivo con para la conexión con la base de datos  
``` mkdir config && nano config/database.js ```  
con el siguiente contenido:  
![imagen1](img/1.png)   
modificamos las rutas:  
``` nano routes/index.js ```  
con el siguiente contenido:  
![imagen2](img/2.png)  
creamos la vista principal:  
``` nano views/index.pug ```  
con el siguiente contenido:  
![imagen3](img/3.png)  
creamos la vista wished:  
``` nano views/wished.pug ```  
contenido:  
![imagen4](img/4.png)  
creamos la vista visited:  
``` nano views/visited.pug ```  
contenido:  
![imagen5](img/5.png)  
Instalamos un gestor de procesos  
```  sudo npm install -g pm2 ```  
lanzamos un proceso en background (estando en la carpeta de la aplicación)  
``` pm2 start ./bin/www --name travelroad_express ```  
creamos un travelroad_express.conf en /etc/nginx/conf.d/ :  
``` sudo nano /etc/nginx/conf.d/travelroad.conf```  
Contenido:  
![imagen6](img/6.png)  
script de despliegue (deploy.sh):  
```
#!/bin/bash

ssh arkania "
  cd travelroad_express
  git pull
  npm install
  pm2 restart travelroad_express --update-env
"
```  
Permisos de ejecución al scripts:  
``` chmod +x deploy.sh ```  
Ahora clonamos el repositorio en la máquina de producción(deberá tener todo instalado anteriormente y ejecutado el pm2 start), creamos un .env a raiz del .env.example con las correspondientes credenciales, ejecutamos el script desde la máquina de desarrollo y finalmente vamos a configurar el .conf:  
```
server {
    server_name express.nuhazet.arkania.es;

    location / {
        proxy_pass http://localhost:3000;  # socket TCP
    }
}
```  
Ponemos el certbot(seleccionamos el número que corresponda):  
```  sudo certbot --nginx ```  
Creamos un www_travelroad_express.conf:  
```
server{
  listen 80;
  server_name www.express.nuhazet.arkania.es;
  return 301 https://express.nuhazet.arkania.es$request_uri;
}
```
aplicamos el cerbot al numero que corresponda:
``` sudo certbot --nginx ```  
Resultado final:  
![imagen7](img/7.png)  
y nos redirecciona a:  
![imagen8](img/8.png)  
