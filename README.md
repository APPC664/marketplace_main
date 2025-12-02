# nombre del proyecto Marketplace


# Signup y Login en Forms.py
```Python
from django import forms
from django.contrib.auth.forms import UserCreationForm, AuthenticationForm
from django.contrib.auth.models import User


from .models import Item


class LoginForm(AuthenticationForm):
    username = forms.CharField(widget=forms.TextInput(
        attrs={
            'placeholder': 'Tu usuario',
            'class': 'form-control'
        }
    ))


    password = forms.CharField(widget=forms.PasswordInput(
        attrs={
            'placeholder': 'password',
            'class': 'form-control'
        }
    ))


class SignupForm(UserCreationForm):
    class Meta:
        model = User
        fields = ('username', 'email', 'password1', 'password2')


    username = forms.CharField(widget=forms.TextInput(
        attrs={
            'placeholder': 'Tu Usuario',
            'class': 'form-control'
        }
    ))


    email = forms.CharField(widget=forms.EmailInput(
        attrs={
            'placeholder': 'Tu Email',
            'class': 'form-control'
        }
    ))


    password1 = forms.CharField(widget=forms.PasswordInput(
        attrs={
            'placeholder': 'Password',
            'class': 'form-control'
        }
    ))


    password2 = forms.CharField(widget=forms.PasswordInput(
        attrs={
            'placeholder': 'Repite Password',
            'class': 'form-control'
        }
    ))
```




# Funciones en views.py
```python
from django.shortcuts import render
from .models import Item, Category
from django.shortcuts import get_object_or_404
from .forms import SignupForm



# Create your views here.
def home(request):
    items = Item.objects.filter(is_sold=False)
    categories=Category.objects.all()




    context = {
        'items':items,
        'categories': categories
    }
    return render(request, 'store/home.html', context)




def contact(request):
    context={
        'msg' : 'Quieres otros productos contactame!'
    }




    return render(request, 'store/contact.html', context)




def detail(request, pk):
    item = get_object_or_404(Item, pk=pk)
    print("item: ", item)
    related_items = Item.objects.filter(category=item.category, is_sold=False).exclude(pk=pk)[0:3]


    context={
        'item': item,
        'related_items': related_items
    }




    return render(request, 'store/item.html', context)


def register (request):
    if request.method == 'POST':
        form = SignupForm(request.POST)


        if form.is_valid():
            form.save()
            return redirect('login')
            else:
                form = SignupForm()
                    context = {
                        'form' : form
                    }
            return render (request, 'store/signup.html', context)
```


# Login, register urls.py
```Python
from django.urls import path
from django.contrib.auth import views as auth_views
from.views import contact, detail, register
from .forms import LoginForm


urlpatterns = [
    path('contact/',contact, name='contact' ),
    path('register/', register, name='register'),
    path('login/', auth_views.LoginView.as_view(template_name='store/login.html'),
          authentication_form= LoginForm),
    path('detail/<int:pk>/', detail, name='detail'),
]
```


# Templates templates/store login, signup
```html
{% extends 'store/base.html' %}




{% block title %}Registro| {% endblock %}




{% block content %}
<div class="row p-4">
    <div class="col-6 bg-light p-4">
        <h4 class="mb-6 text-center">Registro</h4>
        <hr>
        <form action="."method="POST">
            {% csrf_token %}
            <div class="form-floating mb-3">
                <h6>Username:</h6>
                {{form.Username}}
            </div>
            <div class="form-floating mb-3">
                <h6>Email:</h6>
                {{form.email}}
            </div>
            <div class="form-floating mb-3">
                <h6>Password:</h6>
                {{form.password1}}
            </div>
            <div class="form-floating mb-3">
                <h6>Repite Password:</h6>
                {{form.password2}}
            </div>




            {% if form.errors or form.non_field_errors%}
            <div class="mb-4 p-6 bg-danger">
                {% for field in form%}
                    field.errors
                {% endfor%}
                {{ form.non_field_errors }}
            </div>
                {% endif%}




                <button class="btn btn-primary mb-6">Register</button>
        </form>
    </div>
</div>




{% endblock %}
```


```html
{% extends 'store/base.html' %}


{% block title %}Registro| {% endblock %}


{% block content %}
<div class="row p-4">
    <div class="col-6 bg-light p-4">
        <h4 class="mb-6 text-center">Registro</h4>
        <hr>
        <form action="." method="POST">
            {% csrf_token %}
            <div class="form-floating mb-3">
                <h6>Username:</h6>
                {{form.username}}
            </div>
            <div class="form-floating mb-3">
                <h6>Email:</h6>
                {{form.email}}
            </div>
            <div class="form-floating mb-3">
                <h6>Password:</h6>
                {{form.password1}}
            </div>
            <div class="form-floating mb-3">
                <h6>Repite Password:</h6>
                {{form.password2}}
            </div>


            {% if form.errors or form.non_field_errors %}
                <div class="mb-4 p-6 bg-danger">
                    {% for field in form %}
                        fields.errors
                    {% endfor %}
                    {{ form.non_field_errors }}
                </div>
            {% endif %}


            <button class="btn btn-primary mb-6">Register</button>
        </form>
    </div>
</div>
{% endblock %}
```





Submódulo: 
Construye aplicaciones web


Nombre del trabajo: Práctica Evaluatoria Parcial 2


Nombres de los alumnos:
Bravo Luis Salvador
Garcia Arenas Pedro Daniel
Morales Garcia Axel Ovandi
Perez Perez Christian Alexander
Salgado Ruelas Victor Ernesto
Grupo
5AMPG 

Índice
Introduccion……………………………………………………………2
Comandos utilizados …………………………………………….3
Diagrama…………………………………………………………………7
Explicacion de archivos…………………………………………8
Código de los archivos…………………………………………13
Ejecucion del proyecto…………………………………………23
Archivos Actualizados………………………………………….25
.Forms.py……………………………………………………………25
.Views.py actualizado …………………………………………………31
.Explicacion de el decorados de loqgin_required………34
.Urls.py actualizado……………………………………………………35
.Actualizacionde store/templates…………………………….36
.Ejecucion del proyecto……………………………………………….45
Conclusion……………………………………………………………………..50












2. Introducción 
Django es un framework de programación hecho con el lenguaje Python. Sirve para crear páginas y aplicaciones web de una manera más rápida, sencilla y mejor organizada. Se usa mucho porque incluye cosas que normalmente se tendrían que hacer desde cero, como la conexión con bases de datos, la seguridad, el manejo de usuarios y hasta un panel de administración que ayuda a controlar la página. Esto ayuda a no tener que programar todo eso porque ya viene preparado desde antes.

1.- Es veloz: Gracias a que viene con varias cosas incluidas, Django permite desarrollar proyectos grandes en menos tiempo. 

2.- Es ordenado: Tiene una buena estructura, esto ayuda a que el código sea más limpio y fácil de entender, cosa que ayuda mucho cuando es un proyecto donde trabajan muchas personas o también ayuda cuando necesitas modificar algo del código más tarde, ya que todo está bien organizado.

3.- Es seguro: Django tiene varias medidas de seguridad, por ejemplo, te protege contra ataques comunes como la inyección de SQL o el robo de datos. Esto ayuda a que los programadores se enfoquen en el desarrollo del código y no sé preocupen por este tipo de cosas.

4.- Comunidad activa: Django cuenta con una gran comunidad que te puede ayudar ya que comparten tutoriales, ejemplos de códigos o también soluciones a distintos errores, cosas que facilita mucho a la hora de empezar a utilizarla o incluso con un poco de experiencia.








    
3. Comandos Utilizados

md
El comando md significa “make directory” en español es crear directorio. Este sirve para crear una carpeta nueva.
Se utiliza de la siguiente forma: 
C:\Users\Usuario> md NombreDeLaCarpeta
Para crear la carpeta de forma correcta y en el directorio correcto debe de estar dentro de la carpeta donde quieres que se encuentré, por ejemplo la carpeta NombreDeLaCarpeta  esta dentro de la carpeta llamada  Cris


cd
El comando cd significa “change directory”  en español es cambiar directorio
Se utiliza de la siguiente forma:
C:\Users\Usuario> cd NombreDeLaCarpeta
Despues de ingresar el comando te mostrara de la siguiente forma:
C:\Users\Usuario\NombreDeLaCarpeta>
para ingresar a la carpeta debes de estar primero dentro de la carpeta que guarda la carpeta que quieres acceder, como por ejemplo la carpeta NombreDeLaCarpeta estaba dentro de Usuario.


dir
El comando dir  significa “directory listing”  en españo es lista de directorios. Este sirve para mostrar el contenido de una carpeta como archivos, subcarpetas sus tamaños y las fechas de modificaciones. Se utiliza de la siguiente forma
C:\Users\Usuario\NombreDeLaCarpeta> dir     

python -m venv venv
El comando python -m venv venv  sirve para crear un entorno virtual en Python.
Se utiliza de la siguiente forma:
C:\Users\Usuario\NombreDeLaCarpeta> python -m venv venv
Se compone de tres partes:
 1-python Este llama el interprete de Python instalado en el sistema
2. -m venv Este crea el entorno virtual ya que usa el módulo venv el cual está incluido en Python
3. venv Este es el nombre de la carpeta dondese almacena el entorno virtual. El nombre es opcional
venv\Scripts\activate
El comando venv\Scripts\activate sirve para activar el entorno virtual de Python que creamos anteriormente donde el venv es la carpeta donde se utilizara el entorno virtual. Se utiliza de la siguiente forma
C:\Users\Usuario\NombreDeLaCarpeta> venv\Scripts\activate
despues de este comando nos aparecera de la siguiente manera
(venv) C:\Users\Usuario\NombreDeLaCarpeta> 
pip
Este comando es el gestor de paquetes de Python, el cual instala, desinstala, enlista paquetes, etc. Se utiliza de la siguientes maneras:
pip install django
El comando  pip install django instala el framework Django, que sirve para desarrollar aplicaciones web en Python, este se instala en el entorno actual, en nuestro caso se instalará en el entorno virtual. Se utiliza de la siguient forma:
(venv) C:\Users\Usuario\NombreDeLaCarpeta> pip install Django

pip install pillow
El comando pip install pillow sirve para instalar pillow  el cual es una biblioteca el cual sirve para trabajar con imagenes en Python, las cuales se puedan abrir, modificar y guardar.
Se utiliza de la siguiente manera 
(venv) C:\Users\Usuario\NombreDeLaCarpeta> pip install pillow

pip list
El comando pip list sirve para mostrar una lista de los paquetes instalados en el entorno, en nuestro caso nos mostrará los paquetes instalados en el entorno virtual.
Se utiliza de la siguiente manera:
(venv) C:\Users\Usuario\NombreDeLaCarpeta> pip list


django-admin startproject marketplace_main
El comando django-admin startproject marketplace_main  sirve para crear un nuevo proyecto Django desde cero el cual tendra el nombre de marketplace_main, el nombre es el que uno quiera.
 Se utiliza de la siguiente manera:
(venv) C:\Users\Usuario\NombreDeLaCarpeta> django-admin startproject marketplace_main
tendras que acceder a tu proyecto con el comando cd  ademas que tambien podras ver el codigo en Visual Studio
python manage.py runserver
El comando python manage.py runserver  sirve para iniciar el servidor el cual estamos creando en Django, lo que permite visualizar el servidor. despues de poner el comando debes de buscar la siguiente url en tu navegador web http://127.0.0.1:8000/  .
Se utiliza de la siguiente manera: 
(venv) C:\Users\Usuario\NombreDeLaCarpeta> python manage.py runserver

code .
El comando code .  sirve para abrir Visual Studio si es que lo tienes descargado en tu dispositivo. al colocar el comando automaticamente se abrira Visual Studio con todos los archivos de tu servidor en el cual podras agregar, modificar y eliminar codigo. Se utiliza de la siguiente manera: 
(venv) C:\Users\Usuario\NombreDeLaCarpeta> code .

python manage.py createsuperuser 
El comando python manage.py create superuser  sirve para crear un administrador en el proyecto de Django, este administrador puede gestionar modelos, usuarios, permisos y más de la interfaz web.
 Se utiliza de la siguiente forma: 
venv) C:\Users\Usuario\NombreDeLaCarpeta> python  manage.py create superuser
despues de escribirlo te pedira que le proporciones un nombre para el administrador, un correo, una contraseña aunque esta no se visualice en el cmd.


python manage.py startapp store
El comando python manage.py startapp store sirve para crear una nueva aplicacion Django con el nombre store  dentro de nuestro proyecto, este se mostrara en Visual Studio.
Se utiliza de la siguiente forma:
(venv) C:\Users\Usuario\NombreDeLaCarpeta> python manage.py runserver

python manage.py makemigrations
Este comando sirve para preparar los cambios en los modelos para que estén en la base de datos, el cual crea archivos de migracion que dicen como debe de ser la estructura de la base de datos.
Se utiliza de la siguiente forma 
(venv) C:\Users\Usuario\NombreDeLaCarpeta> python manage.py makemigrations
python manage.py migration
El comando python manage.py migration sirve para aplicar las migraciones en la base de datos.
Se utiliza de la siguiente forma
(venv) C:\Users\Usuario\NombreDeLaCarpeta> python manage.py migration


   4. 





5.Explicacion de los siguientes archivos:
settigs.py

En un proyecto Django, el archivo settings.py es uno de los más importantes puesto que este contiene la configuración del proyecto y se encarga de definir el proyecto esto mediante sus 8 funciones principales las cuales son:

1.-Configuración principal del proyecto.
En esta sección se encuentra la información principal del proyecto como la ruta base del proyecto, la clave usada por django para la seguridad del proyecto y la lista de dominios o IPs permitidas por la aplicación.

2.-Aplicaciones instaladas.
Aquí se registran todas las apps que se encuentran en el proyecto independientemente de si las creaste tu o django.

INSTALLED_APPS  =  [
    ' django.contrib.admin ' ,
    ' django.contrib.auth ' ,
    ' myapp ' ,   “ tu aplicación”
]

3.-Middlewares.
Los middlewares son los componentes que procesan las peticiones y respuestas desde las procedentes de la seguridad hasta manejar sesiones.

MIDDLEWARE  =  [
    ‘ django.middleware.security.SecurityMiddleware ' ,
    ' django.contrib.sessions.middleware.SessionMiddleware ' ,
]

4.Rutas principales.
Define el archivo que contiene las rutas (URLs) principales del proyecto.

ROOT_URLCONF  =  ' mi_proyecto.urls '

5.Plantilla(Templates).
Indica dónde buscar los archivos HTML y cómo se procesan.

TEMPLATES  =  [
    {
      ' DIRS ' : [ BASE_DIR  /  ' templates' ]  ,
    } ,
]

6.Base de datos
Define qué motor de base de datos se usa (SQLite, PostgreSQL, MySQL, etc.) y sus credenciales.

DATABASES  =  {
    ' default ' :  {
        ' ENGINE ' :  'django.db.backends.sqlite3 ' ,
        ' NAME ' :  BASE_DIR  /  ' db.sqlite3 ' ,
    }
}
.
7.Internacionalización y zona horaria
Configura el idioma y la zona horaria del proyecto.

LANGUAGE_CODE  =  ' es-mx '
TIME_ZONE  =  ' America/Mexico_City '

8.Archivos estáticos y multimedia
Define cómo se manejan las imágenes, CSS, JavaScript, etc

STATIC_URL  =  ' /static/ '
MEDIA_URL  =  ' /media/ '

urls.py

La funcion urls.py tiene varios usos entre los cuales se destacan el mapear las URLs a vistas, lo cual permite al servidor dirigir con precisión a la vista o página que debe ejecutar dependiendo de la URLs mencionada, así mismo permite incluir URLs de otras aplicaciones las cuales se almacenan dentro del archivo principal y también es capaz de llevar a cabo la redirección de usuarios a otras vistas pero esto solo lo puede lograr si las vistas basadas en clases y no en funciones, así mismo esta función pueda dar nombre a cada URLs.
A continuación se mostraran ejemplos de las funciones mencionadas:

urlpatterns = [
path( 'home/' , views.home, name= 'home' ),
]

from django.urls import include, path
urlpatterns = [
path( 'blog/' , include( ' blog.urls ' ) ),
]

from django.views.generic import TemplateView
urlpatterns = [
path( ‘about/’ , TemplateView.as_view( template_name= 'about.html' ) , name= 'about' ),
]

< a href= " { % url 'home' % }" > Ir a la página de inicio < /a >



models.py 

El archivo models.py es uno de los archivos principales de la base de datos en Django.
Esta se encarga de definir cómo se almacenan, relacionan y manipulan los datos, conectando tu aplicación con el sistema de persistencia de una forma limpia, segura y en puro Python.
A continuación profundizaré más en las funciones principales de este archivo.

Definir la estructura de la base de datos:
El archivo models especifica los campos que tendrá cada tabla en la base de datos desde el nombre de esta hasta las restricciones de esta. Cada atributo dentro de la clase modelo
corresponde a una columna de la tabla en la base de datos y es necesario para su correcto funcionamiento.

Interacción con la base de datos:
Django maneja la creación, lectura, actualización y eliminación de datos a través del ORM
(Object-Relational Mapping) y en este caso el archivo models.py permite interactuar con la base de datos usando Python en lugar de escribir consultas SQL manualmente agilizando el progreso. 

Generar migraciones:
Una vez que un modelo se ha definido o modificado, Django puede generar migraciones las cuales son archivos que contienen las instrucciones para modificar la base de datos de acuerdo con los cambios realizados en los modelos. Esto facilita la sincronización entre la estructura del código y la base de datos.

Definir relaciones entre modelos:
Django permite definir relaciones entre los modelos utilizando campos especiales como
ForeignKey, OneToOneField y ManyToManyField. Esto permite crear bases de datos
relacionales, donde las tablas están conectadas de manera lógica y consistente.

Ejemplo de un modelo en Django:

from django.db import models

class  Post ( models.Model ) :
 	title  =  models.CharField ( max_length = 100 )
content  =  models.TextField ( )
created_at  =  models.DateTimeField ( auto_now_add = True )
def  __str__ ( self ) :

return  self.title


views.py

El archivo views.py se encarga de que la página o app se ejecute o funcione con normalidad cuando el usuario realiza una acción dentro del sitio web,app o solicita una vista nueva permitiendo que se pueda llevar a cabo el funcionamiento de esta, otra forma de verlas seria como los puentes entre los modelos y las plantillas, en este proceso también esencial el archivo urls.py ya que se encarga de mapear las urls a las vistas permitiendo que estas puedan se presenten con normalidad y precisión la página que se solicita.
Otras funciones con las que cuenta son las siguientes:
Recibir solicitudes: Cada vista recibe un objeto HttpRequest que contiene información sobre
la solicitud del usuario, como los datos del formulario, el método HTTP o las cookies.

Procesar la lógica del negocio: Aquí se toman decisiones sobre qué datos mostrar, cómo
validarlos o qué acciones realizar.

Interactuar con los modelos: Puede consultar, crear, actualizar o eliminar registros en la base de
datos mediante los modelos de Django.

Generar una respuesta: Devuelve al navegador del usuario una respuesta, que puede ser una
página HTML, un archivo JSON, una redirección u otro tipo de respuesta.
A continuación se presentan unos ejemplos de lo anteriormente mencionado:

from django.http import HttpResponse
from django.shortcuts import render

def saludo ( request ) :
return HttpResponse ( "¡Hola! Bienvenido a mi sitio web." )

def inicio ( request ) :
return render ( request, 'inicio.html ' )



folder templates/store 

En un proyecto Django, el folder templates/store se encarga de la organización de las plantillas HTML de una aplicación llamada store,no obstante, store no es un nombre obligatorio ya que este solo es el nombre que elegimos para nuestra aplicación por que le podemos colocar cualquier otro nombre dependiendo de la aplicación que deseemos crear, así mismo esta carpeta se encarga de renderizar las vistas y mantener un orden en la aplicación creada.




lógicamente la función add_item solo puede ser usada por usuarios que ya iniciaron sesión.
Y esto tiene sentido porque añadir un nuevo producto a store es algo que solo debe hacer un usuario registrado (o administrador).
Si alguien intenta entrar sin estar logueado, Django lo manda directamente al login.
su importancia radica en la función de que crea un nuevo producto y asigna created_by = request.user, o sea, el producto queda ligado al usuario que lo creó
Permitir esto sin login sería inseguro.
 El decorador evita que personas anónimas creen artículos o manipulen la base de datos.


6. Agregue el código por cada explicación de los archivos antes mencionados.
settings.py
"""
Django settings for marketplace_main project.


Generated by 'django-admin startproject' using Django 5.2.7.


For more information on this file, see
https://docs.djangoproject.com/en/5.2/topics/settings/


For the full list of settings and their values, see
https://docs.djangoproject.com/en/5.2/ref/settings/
"""


from pathlib import Path


# Build paths inside the project like this: BASE_DIR / 'subdir'.
BASE_DIR = Path(__file__).resolve().parent.parent




# Quick-start development settings - unsuitable for production
# See https://docs.djangoproject.com/en/5.2/howto/deployment/checklist/


# SECURITY WARNING: keep the secret key used in production secret!
SECRET_KEY = 'django-insecure-jkt$q-_q&#^(xvz**z*ummfb_d*w7_fb!t&wuo@4=zbirr1=r_'


# SECURITY WARNING: don't run with debug turned on in production!
DEBUG = True


ALLOWED_HOSTS = []




# Application definition


INSTALLED_APPS = [
    'django.contrib.admin',
    'django.contrib.auth',
    'django.contrib.contenttypes',
    'django.contrib.sessions',
    'django.contrib.messages',
    'django.contrib.staticfiles',
   
    'store',
]


MIDDLEWARE = [
    'django.middleware.security.SecurityMiddleware',
    'django.contrib.sessions.middleware.SessionMiddleware',
    'django.middleware.common.CommonMiddleware',
    'django.middleware.csrf.CsrfViewMiddleware',
    'django.contrib.auth.middleware.AuthenticationMiddleware',
    'django.contrib.messages.middleware.MessageMiddleware',
    'django.middleware.clickjacking.XFrameOptionsMiddleware',
]


ROOT_URLCONF = 'marketplace_main.urls'


TEMPLATES = [
    {
        'BACKEND': 'django.template.backends.django.DjangoTemplates',
        'DIRS': [],
        'APP_DIRS': True,
        'OPTIONS': {
            'context_processors': [
                'django.template.context_processors.request',
                'django.contrib.auth.context_processors.auth',
                'django.contrib.messages.context_processors.messages',
            ],
        },
    },
]


WSGI_APPLICATION = 'marketplace_main.wsgi.application'




# Database
# https://docs.djangoproject.com/en/5.2/ref/settings/#databases


DATABASES = {
    'default': {
        'ENGINE': 'django.db.backends.sqlite3',
        'NAME': BASE_DIR / 'db.sqlite3',
    }
}




# Password validation
# https://docs.djangoproject.com/en/5.2/ref/settings/#auth-password-validators


AUTH_PASSWORD_VALIDATORS = [
    {
        'NAME': 'django.contrib.auth.password_validation.UserAttributeSimilarityValidator',
    },
    {
        'NAME': 'django.contrib.auth.password_validation.MinimumLengthValidator',
    },
    {
        'NAME': 'django.contrib.auth.password_validation.CommonPasswordValidator',
    },
    {
        'NAME': 'django.contrib.auth.password_validation.NumericPasswordValidator',
    },
]




# Internationalization
# https://docs.djangoproject.com/en/5.2/topics/i18n/


LANGUAGE_CODE = 'en-us'


TIME_ZONE = 'UTC'


USE_I18N = True


USE_TZ = True




# Static files (CSS, JavaScript, Images)
# https://docs.djangoproject.com/en/5.2/howto/static-files/


STATIC_URL = 'static/'
MEDIA_URL='media/'
MEDIA_ROOT = BASE_DIR / 'media'


# Default primary key field type
# https://docs.djangoproject.com/en/5.2/ref/settings/#default-auto-field


DEFAULT_AUTO_FIELD = 'django.db.models.BigAutoField'
URLS en marketplace_main
"""
URL configuration for marketplace_main project.


The `urlpatterns` list routes URLs to views. For more information please see:
    https://docs.djangoproject.com/en/5.2/topics/http/urls/
Examples:
Function views
    1. Add an import:  from my_app import views
    2. Add a URL to urlpatterns:  path('', views.home, name='home')
Class-based views
    1. Add an import:  from other_app.views import Home
    2. Add a URL to urlpatterns:  path('', Home.as_view(), name='home')
Including another URLconf
    1. Import the include() function: from django.urls import include, path
    2. Add a URL to urlpatterns:  path('blog/', include('blog.urls'))
"""
from django.contrib import admin
from django.urls import path, include
#agregar configuraciones para media
from django.conf import settings
from django.conf.urls.static import static
from store.views import home


urlpatterns = [
    path('', home, name='home'),
    path('admin/', admin.site.urls),
    path('store/', include('store.urls')),
] + static(settings.MEDIA_URL ,document_root= settings.MEDIA_ROOT)



URLS en store
from django.urls import path


from .views import contact, detail


urlpatterns = [
    path('contact/',contact, name='contact'),
    path('detail/<int:pk>/', detail, name='detail'),
]



models.py
from django.contrib.auth.models import User
from django.db import models


# Create your models here.
class Category(models.Model):
    name = models.CharField(max_length=225)


    class Meta:
        ordering = ('name', )
        verbose_name_plural = 'categories'


    def __str__(self):
        return self.name
   
class Item(models.Model):
    category = models.ForeignKey(Category, related_name="items", on_delete=models.CASCADE)
    name = models.CharField(max_length=225)
    description = models.TextField(blank=True, null=True)
    price = models.FloatField()
    image = models.ImageField(upload_to="item_images", blank=True, null=True)
    is_sold = models.BooleanField(default=False)
    created_by = models.ForeignKey(User,related_name='items', on_delete = models.CASCADE)
    created_at =models.DateTimeField(auto_now_add=True)


    def __str__(self):
        return self.name

views.py
from django.shortcuts import render
from .models import Item, Category
from django.shortcuts import get_object_or_404




# Create your views here.
def home(request):
    items = Item.objects.filter(is_sold=False)
    categories=Category.objects.all()




    context = {
        'items':items,
        'categories': categories
    }
    return render(request, 'store/home.html', context)




def contact(request):
    context={
        'msg' : 'Quieres otros productos contactame!'
    }




    return render(request, 'store/contact.html', context)




def detail(request, pk):
    item = get_object_or_404(Item, pk=pk)
    print("item: ", item)
    related_items = Item.objects.filter(category=item.category, is_sold=False).exclude(pk=pk)[0:3]


    context={
        'item': item,
        'related_items': related_items
    }




    return render(request, 'store/item.html', context)


7. Agregue la ejecución de lo que va del proyecto
Página principal

Página de contacto

página detail













Archivos actualizados
Forms.py
Este nos sirvió para crear un formulario personalizado en este caso fue para que el usuario inicie sesion, registre una cuenta y pueda editar objetos del modelo Item en la página web. Este funciona con las siguientes clases:
LoginForm
Este nos sirvió para crear un formulario donde el usuario llena sus datos para iniciar secion a la página web con una cuenta ya guardada en la base de datos. Este está basado en el formulario de login que trae Django.



SignupForm
Esto nos sirvió para que el usuario pueda crear una cuenta nueva en la página web, para que despues inicie sesion a la página web. El cual para iniciar sesion se necesita una contraseña, usuario y un correo para que se cree una nueva cuenta.



NewItemForm
Este nos sirvió para que el usuario pueda editar y crear objetos del modelo Item en la página web.






Su codigo:


from django import forms
from django.contrib.auth.forms import UserCreationForm, AuthenticationForm
from django.contrib.auth.models import User




from .models import Item




class LoginForm(AuthenticationForm):
    username = forms.CharField(widget=forms.TextInput(
        attrs={
            'placeholder': 'Tu usuario',
            'class': 'form-control'
        }
    ))




    password = forms.CharField(widget=forms.PasswordInput(
        attrs={
            'placeholder': 'password',
            'class': 'form-control'
        }
    ))




class SignupForm(UserCreationForm):
    class Meta:
        model = User
        fields = ('username', 'email', 'password1', 'password2')




    username = forms.CharField(widget=forms.TextInput(
        attrs={
            'placeholder': 'Tu Usuario',
            'class': 'form-control'
        }
    ))




    email = forms.CharField(widget=forms.EmailInput(
        attrs={
            'placeholder': 'Tu Email',
            'class': 'form-control'
        }
    ))




    password1 = forms.CharField(widget=forms.PasswordInput(
        attrs={
            'placeholder': 'Password',
            'class': 'form-control'
        }
    ))




    password2 = forms.CharField(widget=forms.PasswordInput(
        attrs={
            'placeholder': 'Repite Password',
            'class': 'form-control'
        }
    ))




class NewItemForm(forms.ModelForm):
    class Meta:
        model = Item
        fields = ('category', 'name', 'description', 'price', 'image',)


        widgets = {
            'category': forms.Select(attrs={
                'class': 'form-select'
            }),
            'name': forms.TextInput(attrs={
                'class': 'form-control'
            }),
            'description': forms.Textarea(attrs={
                'class': 'form-control',
                'style': 'height: 100px'
            }),
            'price': forms.TextInput(attrs={
                'class': 'form-control',
            }),
            'price': forms.TextInput(attrs={
                'class': 'form-control',
            }),
            'image': forms.FileInput(attrs={
                'class': 'form-control',
            }),
        }

























views.py actualización
Views.py (login(), logout_user(), detail(), add_item())

Antes el usuario no podía iniciar sesión ni registrarse.
Ahora se agregó el registro, logout y creación de items para usuarios registrados, haciendo la tienda más completa y funcional.

detail():
No cambió nada importante; solo limpiamos el código y quitamos cosas innecesarias.

logout_user():
Antes no existía. Ahora cerramos la sesión del usuario con logout(request) y lo redirigimos al home.

add_item():
Esta también es nueva. Protegimos la vista con @login_required para que solo usuarios autenticados puedan agregar productos.
Si el formulario se envía por POST, validamos los datos, asignamos created_by = request.user, guardamos y redirigimos al detalle del item.

register():
Nueva vista para registrar usuarios con SignupForm.
Si el formulario es válido, guardamos el usuario y lo mandamos al login.

Su codigo:

from django.shortcuts import render, get_object_or_404, redirect
from django.contrib.auth.decorators import login_required
from django.contrib.auth import logout


from .models import Item, Category


from .forms import SignupForm, NewItemForm




# Create your views here.
def home(request):
    items = Item.objects.filter(is_sold=False)
    categories = Category.objects.all()


    context = {
        'items': items,
        'categories': categories
    }
    return render(request, 'store/home.html', context)


def contact(request):
    context = {
        'msg': 'Quieres otros productos contactame!'
    }
    return render(request, 'store/contact.html', context)


def detail(request, pk):
    item = get_object_or_404(Item, pk=pk)
    related_items = Item.objects.filter(category=item.category, is_sold=False).exclude(pk=pk)[0:3]
    context={
        'item': item,
        'related_items': related_items
    }


    return render(request, 'store/item.html', context)


def register(request):
    if request.method == 'POST':
        form = SignupForm(request.POST)


        if form.is_valid():
            form.save()
            return redirect('login')
    else:
        form = SignupForm()


    context = {
        'form': form
    }


    return render(request, 'store/signup.html', context)


def logout_user(request):
    logout(request)


    return redirect('home')


@login_required
def add_item(request):
    if request.method == 'POST':
        form = NewItemForm(request.POST, request.FILES)


        if form.is_valid():
            item = form.save(commit=False)
            item.created_by = request.user
            item.save()


            return redirect('detail', pk=item.id)
    else:
        form = NewItemForm()
        context = {
            'form': form,
            'title': 'New Item'
        }


    return render(request, 'store/form.html', context)










Explicacón de el decorador @login_required
El decorador @login_required es una función especial de Django que sirve para proteger vistas.
 Su propósito es evitar que un usuario que no ha iniciado sesión pueda entrar a una página.
Cuando Django ve este decorador encima de una función si esta es:
Sí: entonces Django permite ejecutar la vista normalmente.
No: automáticamente lo redirecciona a la página de login.
Como no está iniciado sesion, no aparece la opcion de editar Items.











urls.py Actualización
En estas nuevas actualizaciones agregamos nuevas rutas para lograr que la pagina         funcionara añadiendo rutas a nuevos apartados como el registro o el permitir que el usuario regrese a la página principal tras haber entrado a una nueva pantalla o vista permitiendo que la página funcione como cualquier otra permitiendo al usuario navegar en esta sin problemas entre todos sus apartados.

from django.urls import path
from django.contrib.auth import views as auth_views
from .views import contact, detail, register, logout_user, add_item


from .forms import LoginForm


urlpatterns = [
    path('contact/', contact, name='contact'),
    path('register/', register, name='register'),
    path('login/', auth_views.LoginView.as_view(template_name='store/login.html', authentication_form=LoginForm), name='login'),
    path('logout/', logout_user, name='logout'),
    path('add_item/', add_item, name='add_item'),
    path('detail/<int:pk>/', detail, name='detail'),
]












Actualizaciones de store/templates
Item.html:
Su función consiste en mostrarle al usuario la página de detalles de algún producto en nuestra página principal la cuál es una tienda en línea.
Código:
{% extends 'store/base.html' %}




{% block title %}{{item.name}} | {% endblock %}




{% block content %}
<div class="container mt-4 mb-4 container mt-4 mb-4">
    <div class="row">
        <div class="col-4">
            <img src="{{ item.image.url}}" alt="" class="rounded" width="100%">
        </div>
        <div class="col-8 p-4 rounded bg-light">
            <h1 class="mb-4 text-center">
                {{ item.name}}
            </h1>
            <hr>
            <h4><strong>Precio ${{ item.price }}</strong></h4>
            <h4><strong>Vendedor ${{ item.created_by.username }}</strong></h4>
            {% if item.description %}
              <p>{{ item.description }}</p>
            {% endif %}
            <a href="" class="btn btn-dark">Contacte al Vendedor</a>
         
        </div>    
{% endblock %}

login.html:
Este apartado no es más que un apartado donde se muestra una pantalla para el inicio de sesión donde el usuario introduce sus datos solicitados
Código:
{% extends 'store/base.html' %}


{% block title %}Login| {% endblock %}


{% block content %}


<div class="row p-4 d-flex justify-content-center align-items-center">
    <div class="col-6 bg-light p-4">
        <h4 class="mb-6 text-center">Registro</h4>
        <hr>
        <form action="." method="POST">
            {% csrf_token %}
            <div class="form-floating mb-3">
                <h6>Username:</h6>
                {{form.username}}
            </div>
            <div class="form-floating mb-3">
                <h6>Password:</h6>
                {{form.password}}
            </div>




            {% if form.errors or form.non_field_errors %}
            <div class="mb-4 p-6 bg-danger text-white rounded">
                {% for field in form %}
                field.errors
                {% endfor %}
                {{ form.non_field_errors }}
            </div>
            {% endif %}
            <div class="d-flex justify-content-center align-items-center">
                <button class="btn btn-primary mb-6">Login</button>
            </div>
            <div class="d-flex justify-content-center align-items-center">
                <a href="{% url 'register' %}">¿No tienes cuenta? registrate aqui!</a>
            </div>
        </form>
    </div>
</div>






{% endblock %}


navigation.html:
Este código muestra un menú superior que contiene distintas opciones, sus botones varían dependiendo de si el usuario activo está autenticado o no.
código:
<nav class="navbar navbar-expand-lg bg-dark" data-bs-theme="dark">
    <div class="container-fluid">
        <a href="{% url 'home' %}" class="navbar-brand">Marketplace</a>
        <button class="navbar-toggler" type="button" data-bs-toggle="collapse" data-bs-target="#navbarNav" aria-controls="navbarNav" aria-expanded="false" aria-label="Toggle Navigation">
            <span class="navbar-toggler-icon"></span>
        </button>
        <div class="collapse navbar-collapse" id="navbarNav">
            <ul class="navbar-nav ms-auto">
                <li class="nav-item">
                    <a href="{% url 'home' %}" class="nav-link">
                        Home
                    </a>
                </li>
                <li class="nav-item">
                    <a href="{% url 'contact' %}" class="nav-link">
                        Contact
                    </a>
                </li>
               
                {% if request.user.is_authenticated %}
                    <li class="nav-item">
                        <a class="nav-link" href="{% url 'add_item'%}">Add Item</a>
                    </li>
                    <li class="nav-item">
                        <a href="{% url 'logout' %}" class="nav-link">
                            Logout
                        </a>
                    </li>
                {% else %}
                    <li class="nav-item">
                        <a href="{% url 'login' %}" class="nav-link">
                            Login
                        </a>
                    </li>
                    <li class="nav-item">
                        <a href="{% url 'register' %}" class="nav-link">
                            Register
                        </a>
                    </li>
                {% endif %}
            </ul>
        </div>
    </div>
</nav>


signup.html
Este muestra una página de registro de usuarios, este código está creado para mostrar un formulario donde el usuario se registra.
código: 
{% extends 'store/base.html' %}


{% block title %}Registro| {% endblock %}


{% block content %}
<div class="row p-4 d-flex justify-content-center align-items-center">
    <div class="col-6 bg-light p-4">
        <h4 class="mb-6 text-center">Registro</h4>
        <hr>
        <form action="." method="POST">
            {% csrf_token %}
            <div class="form-floating mb-3">
                <h6>Username:</h6>
                {{form.username}}
            </div>
            <div class="form-floating mb-3">
                <h6>Email:</h6>
                {{form.email}}
            </div>
            <div class="form-floating mb-3">
                <h6>Password:</h6>
                {{form.password1}}
            </div>
            <div class="form-floating mb-3">
                <h6>Repite Password:</h6>
                {{form.password2}}
            </div>


            {% if form.errors or form.non_field_errors %}
                <div class="mb-4 p-6 bg-danger rounded">
                    {% for field in form %}
                        <h5 class="text-white">
                            {{field.errors}}
                        </h5>
                       
                    {% endfor %}
                    {{ form.non_field_errors }}
                </div>
            {% endif %}


            <div class="d-flex justify-content-center align-items-center">
                <button class="btn btn-primary mb-6">Register</button>
            </div>
            <div class="d-flex justify-content-center align-items-center">
                <a href="{% url 'login' %}">¿Ya tienes cuenta? Accesa aqui!</a>
            </div>
        </form>
    </div>
</div>
{% endblock %}

form.html:
Es una plantilla que sirve para renderizar formularios en django, este tiene un soporte para la subida de archivos, manejo de errores y un título dinámico.
código:
{% extends 'store/base.html' %}


{% block title %} {{ title }} {% endblock %}


{% block content%}
    <h4 class="mb-4 mt-4">{{ title }}</h4>
    <hr>
    <form action="." method="POST" enctype="multipart/form-data">
        {% csrf_token %}
        <div>
       
            {{ form.as_p }}
        </div>


        {% if form.errors or form.non_field_errors %}
            <div class="mb-4 p-6 bg-danger">
                {% for field in form %}
                    {{ field.errors }}
                {% endfor %}


                {{ form.non_field_errors }}
            </div>
        {% endif %}


        <button class="btn btn-primary mb-6">Register</button>
    </form>
{% endblock%}


















Ejecución del proyecto














  


Conclusion
Para nuestro equipo este parcial se sintió bastante productivo, teníamos bastante tiempo sin estar en alguna clase que sea tan activa y que nos hiciera recordar lo que conlleva ser programador, aún así también nos hemos llevado nuevas conclusiones, para nuestro líder fue algo divertido pero frustrante que al momento de correr el programa tuviera un error, era algo frustrante cuando sucedía por algún error que jamás hubiésemos imaginado.

Por otra parte, un inconveniente que si nos atrasó bastante para seguir al día cada clase es que estamos hasta el fondo del aula y se nos dificulta seguir cada instrucción del profesor, por lo que hemos tenido que recurrir a varios compañeros y al profesor para entender mejor cada paso que hemos dado, pero aún así medio hemos entendido lo que cada clase ha querido aportar y por lo tanto seguir a los demás en el avance de el programa.

Por último, consideramos que hemos practicado de forma eficiente la creación y codificación de sectores para hacer nuestro programa aún más completo, aunque parezca algo complejo puede ser bastante sencillo si se hace todo con el suficiente conocimiento, aunque esperamos no seguir teniendo retrasos derivados de la baja visión que tenemos respecto a la pantalla del profesor.

Actualización 
En este parcial actualizamos el proyecto haciéndolo más funcional, ya que agregamos nuevas vistas o páginas como el login o inicio de sesión, un registro y una función para añadir items nuevos a la tienda permitiendo agrandar el catálogo y hacerlo cada vez más profesional y funcional.

También una actualización importante fue la que llevamos a cabo en el archivo urls.py mediante la cual agregamos nuevas rutas a la página permitiendo navegar a las nuevas vistas mencionadas anteriormente, así mismo permitimos una navegación completa y fluida al permitir al usuario regresar a la página principal desde cualquier página o vista.

Aparte de todo esto también investigamos cómo funcionaba toda la página al ver cómo se conectaban todos las vistas y páginas dentro del proyecto, así mismo investigamos los lenguajes de programación que utilizamos en nuestro proyecto como lo puede ser el lenguaje de programación markdown, permitiéndonos comprender mejor nuestro proyecto y todo lo que lo compone desde las conexiones entre todos los archivos y componentes de este como los lenguajes de programación involucrados en la creación de este.









