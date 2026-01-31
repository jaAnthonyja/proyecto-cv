# ⚡ Comandos Útiles - Cheat Sheet

## 🚀 Comandos Básicos Django

### Iniciar Servidor
```bash
# Desarrollo local
python manage.py runserver

# En puerto específico
python manage.py runserver 8000

# Accesible desde otra máquina
python manage.py runserver 0.0.0.0:8000
```

### Base de Datos

#### Migraciones
```bash
# Ver estado
python manage.py showmigrations

# Crear nuevas migraciones
python manage.py makemigrations

# Crear específicamente para app cv
python manage.py makemigrations cv

# Ver SQL que se ejecutará
python manage.py makemigrations --dry-run

# Aplicar migraciones
python manage.py migrate

# Aplicar hasta migración específica
python manage.py migrate cv 0001

# Deshacer migración
python manage.py migrate cv 0001

# Ver migraciones aplicadas
python manage.py migrate --list
```

#### Backup y Restore
```bash
# Backup de BD
python manage.py dumpdata > backup.json
python manage.py dumpdata cv > cv_backup.json

# Restore de BD
python manage.py loaddata backup.json

# Limpiar datos (cuidado!)
python manage.py flush
```

### Usuario Admin

```bash
# Crear superuser
python manage.py createsuperuser

# Crear con variables
python manage.py shell
from django.contrib.auth.models import User
User.objects.create_superuser('admin', 'admin@example.com', 'password')

# Cambiar contraseña
python manage.py shell
from django.contrib.auth.models import User
user = User.objects.get(username='admin')
user.set_password('newpassword')
user.save()
```

### Shell de Django

```bash
# Entrar a shell
python manage.py shell

# Dentro del shell:
from cv.models import Datospersonales

# Crear
perfil = Datospersonales.objects.create(
    nombres="Juan",
    apellidos="Pérez",
    perfilactivo=True
)

# Leer
perfil = Datospersonales.objects.get(idperfil=1)
print(perfil.nombres)

# Listar
for p in Datospersonales.objects.all():
    print(p.nombres)

# Actualizar
perfil.nombres = "Carlos"
perfil.save()

# Eliminar
perfil.delete()

# Filtrar
activos = Datospersonales.objects.filter(perfilactivo=True)
print(activos.count())

# Consultas avanzadas
perfil = (Datospersonales.objects
    .filter(perfilactivo=True)
    .order_by("-idperfil")
    .first()
)

# Salir
exit()
```

---

## 📁 Gestión de Archivos

### Estáticos

```bash
# Recolectar archivos estáticos
python manage.py collectstatic

# Sin confirmación
python manage.py collectstatic --noinput

# Limpiar archivos
python manage.py collectstatic --clear

# Verificar integridad
python manage.py collectstatic --dry-run
```

### Media

```bash
# En shell Python
from django.core.files.base import ContentFile

perfil = Datospersonales.objects.get(idperfil=1)

# Subir desde archivo local
with open("ruta/a/foto.jpg", "rb") as f:
    perfil.foto_perfil.save("foto.jpg", ContentFile(f.read()))

# Subir desde URL
import requests
url = "https://ejemplo.com/foto.jpg"
response = requests.get(url)
perfil.foto_perfil.save("foto.jpg", ContentFile(response.content))

perfil.save()
```

---

## 🧪 Testing

### Ejecutar Tests

```bash
# Todos los tests
python manage.py test

# Tests específicos
python manage.py test cv

# Clase específica
python manage.py test cv.tests.DatospersonalesTest

# Método específico
python manage.py test cv.tests.DatospersonalesTest.test_crear_perfil

# Con verbose
python manage.py test -v 2

# Con Coverage
pip install coverage
coverage run --source='.' manage.py test
coverage report
coverage html  # Genera reporte HTML
```

### Crear Test

```python
# cv/tests.py
from django.test import TestCase
from .models import Datospersonales

class DatospersonalesTest(TestCase):
    def setUp(self):
        """Se ejecuta antes de cada test"""
        self.perfil = Datospersonales.objects.create(
            nombres="Test",
            apellidos="User"
        )
    
    def test_crear_perfil(self):
        """Verifica creación de perfil"""
        self.assertEqual(self.perfil.nombres, "Test")
    
    def test_perfil_activo(self):
        """Verifica perfil activo"""
        self.perfil.perfilactivo = True
        self.perfil.save()
        
        activo = Datospersonales.objects.filter(
            perfilactivo=True
        ).first()
        
        self.assertEqual(activo.idperfil, self.perfil.idperfil)
```

---

## 📊 Análisis y Debug

### Queries SQL

```bash
# Ver todas las consultas ejecutadas
python manage.py shell

from django.db import connection
from django.test.utils import override_settings

@override_settings(DEBUG=True)
def ver_queries():
    Datospersonales.objects.all()
    for query in connection.queries:
        print(query['sql'])

# Contar consultas
print(len(connection.queries))
```

### Logs y Errores

```bash
# Ver errores en consola (mientras runserver está abierto)
# Los errores aparecen automáticamente

# Guardar logs en archivo
# En settings.py:
LOGGING = {
    'version': 1,
    'handlers': {
        'file': {
            'class': 'logging.FileHandler',
            'filename': 'debug.log',
        },
    },
    'loggers': {
        'django': {
            'handlers': ['file'],
            'level': 'DEBUG',
        },
    },
}
```

### Profiling

```bash
# Ver tiempo de ejecución
pip install django-extensions

# En settings.py, agregar 'django_extensions'

# Usar en shell
python manage.py shell_plus

# Ver tiempo de query
from django.db import connection, reset_queries
reset_queries()

Datospersonales.objects.all()

print(len(connection.queries), "queries")
print(connection.queries[-1])  # Última query
```

---

## 🔧 Configuración

### Variables de Entorno

```bash
# Crear archivo .env
echo "DEBUG=1" > .env
echo "SECRET_KEY=mi-clave-secreta" >> .env
echo "DATABASE_URL=" >> .env

# Cargar en shell
python manage.py shell
import os
from dotenv import load_dotenv
load_dotenv()
print(os.getenv('DEBUG'))
```

### Settings

```bash
# Ver configuración actual
python manage.py shell
from django.conf import settings
print(settings.DEBUG)
print(settings.DATABASES)
print(settings.INSTALLED_APPS)
print(settings.MEDIA_ROOT)
```

---

## 🚢 Deploy

### Producción

```bash
# Preparar para producción
python manage.py check --deploy

# Recolectar estáticos
python manage.py collectstatic --noinput

# Comprimir estáticos
python manage.py compress --offline

# Crear backup
python manage.py dumpdata > backup_prod.json
```

### Gunicorn

```bash
# Instalar
pip install gunicorn

# Ejecutar
gunicorn django_portfolio.wsgi

# Con workers
gunicorn django_portfolio.wsgi -w 4

# Con bind
gunicorn django_portfolio.wsgi -b 0.0.0.0:8000

# Crear archivo gunicorn_config.py
workers = 4
bind = "0.0.0.0:8000"
timeout = 120
accesslog = "/var/log/gunicorn/access.log"
errorlog = "/var/log/gunicorn/error.log"

# Usar config
gunicorn -c gunicorn_config.py django_portfolio.wsgi
```

### Git

```bash
# Inicializar repo
git init
git add .
git commit -m "Initial commit"

# Ignorar archivos
echo "*.pyc" > .gitignore
echo "__pycache__/" >> .gitignore
echo ".env" >> .gitignore
echo "db.sqlite3" >> .gitignore
echo "media/" >> .gitignore

git add .gitignore
git commit -m "Add .gitignore"

# Push a repo
git remote add origin https://github.com/user/repo.git
git branch -M main
git push -u origin main
```

---

## 🐛 Debugging Común

### Problema: ModuleNotFoundError

```bash
# Verificar que estás en el directorio correcto
pwd  # Debe mostrar: .../django

# Verificar que estás en el entorno virtual
which python  # Debe mostrar: .../venv/bin/python

# Instalar requirements
pip install -r requirements.txt
```

### Problema: Database Error

```bash
# Verificar BD
python manage.py dbshell
.tables  # Listar tablas

# Recrear BD
rm db.sqlite3
python manage.py migrate

# Restaurar datos
python manage.py loaddata backup.json
```

### Problema: Static Files No Aparecen

```bash
# Recolectar
python manage.py collectstatic --noinput --clear

# Verificar settings
python manage.py shell
from django.conf import settings
print(settings.STATIC_ROOT)
print(settings.STATIC_URL)
```

### Problema: Port Already in Use

```bash
# Encontrar proceso
lsof -i :8000  # Linux/Mac

# Matar proceso
kill -9 <PID>

# Usar otro puerto
python manage.py runserver 8001
```

---

## 📦 Dependencias

### Instalar

```bash
# Desde archivo
pip install -r requirements.txt

# Paquete específico
pip install Django==4.2
pip install gunicorn
pip install psycopg2-binary

# Todos de directorios
pip install -e .
```

### Actualizar

```bash
# Actualizar un paquete
pip install --upgrade Django

# Actualizar todos
pip install --upgrade -r requirements.txt
```

### Generar requirements

```bash
# Desde entorno actual
pip freeze > requirements.txt

# Solo dependencias directas
pip install pipdeptree
pipdeptree --graph-output png > dependencies.png
```

### Entorno Virtual

```bash
# Crear
python -m venv venv

# Activar (Windows)
venv\Scripts\activate

# Activar (Linux/Mac)
source venv/bin/activate

# Desactivar
deactivate

# Eliminar
rm -rf venv  # Linux/Mac
rmdir /s venv  # Windows
```

---

## 🔍 Útiles

### Ver URLs

```bash
# Listar todas las URLs
python manage.py show_urls

# Filtrar
python manage.py show_urls | grep admin
```

### Generar Documentación

```bash
# HTML de admin
python manage.py graph_models cv > models.png
pip install django-extensions
pip install pydotplus

# Generar
python manage.py graph_models cv -o models.png

# Con todas las apps
python manage.py graph_models -a -o models.png
```

### Limpiar

```bash
# Limpiar archivos compilados
find . -name "*.pyc" -delete
find . -name "__pycache__" -delete

# Limpiar datos
python manage.py flush

# Limpiar migraciones (CUIDADO!)
python manage.py migrate cv zero
```

---

## 💡 Tips Útiles

### Performance

```bash
# Crear índice en BD
python manage.py shell
from django.db import connection
with connection.cursor() as cursor:
    cursor.execute("CREATE INDEX idx_perfilactivo ON cv_datospersonales(perfilactivo);")

# Ver consultas lentas
python manage.py runserver --debug-toolbar
```

### Desarrollo Rápido

```bash
# Auto-reload con Watchdog
pip install watchdog

# Mostrar tráfico HTTP
pip install django-silk

# Editor visual
pip install django-admin-interface
```

### Producción

```bash
# Monitorear
pip install sentry-sdk

# Cache
pip install django-redis

# Compresión
pip install django-compressor

# CORS
pip install django-cors-headers
```

---

## 📝 Cheat Sheet Rápido

```
DESARROLLO
$ python manage.py runserver          Iniciar servidor
$ python manage.py migrate            Aplicar cambios BD
$ python manage.py makemigrations     Crear migraciones
$ python manage.py shell              Entrar a Python
$ python manage.py test               Ejecutar tests

PRODUCCIÓN
$ python manage.py collectstatic      Recolectar estáticos
$ python manage.py check --deploy     Verificar seguridad
$ gunicorn django_portfolio.wsgi      Ejecutar servidor
$ python manage.py dumpdata > backup.json  Backup

USUARIO ADMIN
$ python manage.py createsuperuser    Crear admin
$ python manage.py changepassword     Cambiar contraseña

DEBUGGING
$ python manage.py dbshell            Entrar a BD directa
$ python manage.py show_urls          Ver URLs
$ python manage.py runserver 0.0.0.0:8000  Accesible desde red

GIT
$ git init                             Inicializar repo
$ git add .                            Preparar cambios
$ git commit -m "message"              Guardar cambios
$ git push origin main                 Subir a GitHub
```

---

## 🆘 Referencia Rápida

### URLs Locales
```
Página: http://127.0.0.1:8000/
Admin: http://127.0.0.1:8000/admin/
PDF: http://127.0.0.1:8000/imprimir/
```

### Archivo Importante
```
settings.py → Configuración general
models.py → Estructura de datos
views.py → Lógica
urls.py → Rutas
templates/ → HTML
static/ → CSS, JS, imágenes
```

### Estructura Carpetas
```
django/
├── cv/ → Aplicación principal
├── django_portfolio/ → Configuración
├── db.sqlite3 → Base de datos
└── manage.py → Comando Django
```

---

**Última actualización:** Enero 30, 2026
**Versión:** 1.0

