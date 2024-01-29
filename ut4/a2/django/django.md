# ÍNDICE

+ [Material empleado](#id1)
+ [Desarrollo](#id2)

# ***Material empleado***. <a name="id1"></a>

- Python
- Nginx
- PostgreSQL + datos
- pgAdmin
- CertBot

## ***Desarrollo***. <a name="id2"></a>
### Entorno  
```
python -m venv --prompt travelroad .venv
```  
```
source .venv/bin/activate
```  
```
pip install django
```
Proyecto: 
```
django-admin startproject main .
```  
```
./manage.py startapp places
```  
```
nano main/settings.py
```  
Cambiar la zona:  
```
...
INSTALLED_APPS = [
    'django.contrib.admin',
    'django.contrib.auth',
    'django.contrib.contenttypes',
    'django.contrib.sessions',
    'django.contrib.messages',
    'django.contrib.staticfiles',
    # Nueva línea ↓
    'places.apps.PlacesConfig',
]
...
```  
```
pip install psycopg2-binary
```  
```
nano main/settings.py
```  
Cambiar la zona:  
```
DATABASES = {
    'default': {
        'ENGINE': 'django.db.backends.postgresql',
        'NAME': 'travelroad',
        'USER': 'travelroad_user',
        'PASSWORD': 'dpl0000',
        'HOST': 'localhost',
        'PORT': 5432,
    }
}
```  
Modelos:
```
nano places/models.py
```  
Contenido:  
```
from django.db import models

class Place(models.Model):
    name = models.CharField(max_length=255)
    visited = models.BooleanField()

    class Meta:
        # ↓ necesario porque ya partimos de una tabla creada ↓
        db_table = "places"

    def __str__(self):
        return self.name
```  
```
nano places/views.py
```  
Contenido:  
```
from django.http import HttpResponse
from django.template import loader

from .models import Place


def index(request):
    template = loader.get_template('places/index.html')    
    return HttpResponse(template.render(request))

def wished(request):
    wished = Place.objects.filter(visited=False)
    template = loader.get_template('places/wished.html')
    context = {
        'wished': wished,
    }
    return HttpResponse(template.render(context, request))
def visited(request):
    visited = Place.objects.filter(visited=True)
    template = loader.get_template('places/visited.html')
    context = {
        'visited': visited,
    }
    return HttpResponse(template.render(context, request))
```  
Plantillas:  
```
mkdir -p places/templates/places
```  
```
nano places/templates/places/index.html
```  
Contenido:  
```
<h1>My Travel Bucket List</h1>
<a href="/wished">Places I'd Like to Visit</a>
<a href="/visited">Places I've Already Been To</a>
<p>Powered by Django</p>
```  
```
nano places/templates/places/wished.html
```  
Contenido:  
```
<h1>Places I'd Like to Visit</h1>

<ul>
  {% for place in wished %}
  <li>{{ place }}</li>
  {% endfor %}
</ul>
<a href="../">Back Home</a>
```  
```
nano places/templates/places/visited.html
```  
Contenido:  
```
<h1>Places I've Already Been To</h1>

<ul>
  {% for place in visited %}
  <li>{{ place }}</li>
  {% endfor %}
</ul>
<a href="../">Back Home</a>
```  
URLs:  
```
nano places/urls.py
```  
Contenido:  
```
from django.urls import path

from . import views

app_name = 'places'

urlpatterns = [
    path('', views.index, name='index'),
    path('wished/', views.wished, name='wished'),
    path('visited/', views.visited, name='visited'),
]
```  
Main/urls:  
```
nano main/urls.py
```  
Contenido:  
```
from django.contrib import admin
from django.urls import path
from django.urls import include, path

urlpatterns = [
    path('admin/', admin.site.urls),
    # NUEVA LÍNEA ↓
    path('', include('places.urls', 'places')),
]
```  
Parametrizando la configuración:  
```
pip install prettyconf
```  
```
nano main/settings.py
```  
Contenido:  
```
...
from pathlib import Path
# ↓ Nueva línea
from prettyconf import config
# ↑ Nueva línea
...
DEBUG = config('DEBUG', default=True, cast=config.boolean)
ALLOWED_HOSTS = config('ALLOWED_HOSTS', default=[], cast=config.list)
...
DATABASES = {
    'default': {
        'ENGINE': 'django.db.backends.postgresql',
        'NAME': config('DB_NAME'),
        'USER': config('DB_USERNAME'),
        'PASSWORD': config('DB_PASSWORD'),
        'HOST': config('DB_HOST', default='localhost'),
        'PORT': config('DB_PORT', default=5432, cast=int)
    }
}
...
```  
Fichero .env para las credenciales de desarrollo con la siguiente estructura:  
```
DEBUG=0
# ↓ Dominio en el que se va a desplegar la aplicación
ALLOWED_HOSTS=localhost
DB_NAME='travelroad'
DB_USERNAME='travelroad_user'
DB_PASSWORD='dpl10000'
```  
Especificar requerimientos:  
```
nano requirements.txt
```  
```

```  
```

```  
```

```  
```

```  
```

```  




