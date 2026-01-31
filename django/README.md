# 📚 Documentación Completa del Proyecto Django CV

## 📖 Índice de Documentación

Este proyecto incluye una documentación completa dividida en 4 documentos:

1. **DOCUMENTACION_PAGINA_INICIO.md** ← Explicación técnica completa
2. **DIAGRAMAS_PAGINA_INICIO.md** ← Diagramas y flujos visuales  
3. **INICIO_RAPIDO_PAGINA_INICIO.md** ← Guía rápida para entender el concepto
4. **TUTORIAL_CREAR_PERFIL.md** ← Paso a paso para crear tu perfil
5. **README.md** ← Este archivo (resumen general)

---

## 🎯 ¿Qué es este proyecto?

**Django CV** es una aplicación web que te permite crear un **portafolio digital profesional** con:

✅ **Página de inicio** con tus datos personales
✅ **7 secciones** para organizar tu información
✅ **Sistema de activación/desactivación** de contenido
✅ **Generador de PDF** para descargar tu CV
✅ **Panel de administración** para gestionar contenido
✅ **Almacenamiento en la nube** (Azure)
✅ **Responsive design** (funciona en móvil)

---

## 🏗️ Estructura del Proyecto

```
django/
├── manage.py .......................... Comando principal de Django
├── db.sqlite3 ......................... Base de datos (desarrollo)
├── requirements.txt ................... Dependencias Python
├── procfile ........................... Configuración para deploy
│
├── cv/ ............................... APLICACIÓN PRINCIPAL
│   ├── models.py ..................... Estructura de datos (6 modelos)
│   ├── views.py ..................... Lógica de las páginas (8 vistas)
│   ├── urls.py ....................... Rutas (mapeo de URLs)
│   ├── admin.py ...................... Panel administrador
│   ├── apps.py ....................... Configuración de app
│   ├── tests.py ...................... Tests (pruebas)
│   ├── migrations/ ................... Historial de cambios BD
│   ├── templates/ .................... Archivos HTML
│   │   ├── base.html ................. Plantilla base
│   │   ├── home.html ................. Página de inicio
│   │   └── secciones/ ................ Páginas de cada sección
│   └── static/ ....................... Archivos CSS, JS, imágenes
│       └── css/ ....................... Estilos
│
├── django_portfolio/ ................. CONFIGURACIÓN DJANGO
│   ├── settings.py ................... Configuración general
│   ├── urls.py ....................... Rutas principales
│   ├── wsgi.py ....................... Servidor producción
│   └── asgi.py ....................... Servidor async
│
├── media/ ............................ Carpeta de uploads
│   └── perfiles/ ..................... Fotos de perfil
│
├── staticfiles/ ...................... Archivos estáticos compilados
│
└── DOCUMENTACION/ .................... 📚 ESTE DOCUMENTO

```

---

## 🗄️ Modelos de Base de Datos

### 1. **Datospersonales** (Perfil Principal)
```python
# Datos del usuario
- idperfil (PK)
- nombres
- apellidos
- foto_perfil (imagen)
- descripcionperfil
- nacionalidad
- lugarnacimiento
- fechanacimiento
- numerocedula
- sexo
- sitioweb
- perfilactivo (¿visible?)
- permitir_impresion (¿descargar PDF?)

# Relaciones (1 perfil → M items)
- cursos
- experiencias
- productos_academicos
- productos_laborales
- reconocimientos
- venta_garage
```

### 2. **Cursosrealizados** (Cursos)
```python
- idcursorealizado (PK)
- perfil (FK → Datospersonales)
- nombrecurso
- institucion
- descripcion
- fechainicio
- fechafin
- certificado_imagen
- certificado_pdf
- activarparaqueseveaenfront (¿mostrar?)
```

### 3. **Experiencialaboral** (Trabajo)
```python
- idexperiencialaboral (PK)
- perfil (FK)
- cargodesempenado
- nombrempresa
- descripcionfunciones
- fechainicio
- fechafin
- certificado_imagen
- activarparaqueseveaenfront
```

### 4. **Productosacademicos** (Proyectos Académicos)
```python
- idproductoacademico (PK)
- perfil (FK)
- nombreproducto
- descripcion
- clasificador (Artículo, Tesis, etc.)
- imagenproducto
- certificado_imagen
- enlace_repositorio
- activarparaqueseveaenfront
```

### 5. **Productoslaborales** (Proyectos Trabajo)
```python
- idproductolaboral (PK)
- perfil (FK)
- nombreproducto
- descripcion
- tecnologias
- imagenproducto
- certificado_imagen
- enlace_github
- activarparaqueseveaenfront
```

### 6. **Reconocimientos** (Premios/Certificaciones)
```python
- idreconocimiento (PK)
- perfil (FK)
- tiporeconocimiento
- entidadpatrocinadora
- fecha
- certificado_imagen
- activarparaqueseveaenfront
```

### 7. **Ventagarage** (Artículos en Venta)
```python
- idventa (PK)
- perfil (FK)
- nombrenpProducto
- precio
- descripcion
- imagen
- fecha
- activarparaqueseveaenfront
```

---

## 📍 Rutas (URLs)

```
GET  /                           → home()                    Página de inicio
GET  /datos-personales/          → datos_personales()        Información personal
GET  /cursos/                    → cursos()                  Cursos realizados
GET  /experiencia/               → experiencia()             Experiencia laboral
GET  /productos-academicos/      → productos_academicos()    Productos académicos
GET  /productos-laborales/       → productos_laborales()     Productos laborales
GET  /reconocimientos/           → reconocimientos()         Reconocimientos
GET  /venta-garage/              → venta_garage()            Venta garage
GET  /imprimir/                  → imprimir_hoja_vida()      Descargar PDF
```

---

## 🔧 Configuración Importante

### Base de Datos
```python
# Desarrollo (local)
DATABASES = {
    "default": {
        "ENGINE": "django.db.backends.sqlite3",
        "NAME": "db.sqlite3",
    }
}

# Producción (Azure)
# DATABASE_URL = "postgresql://user:pass@host/db"
```

### Almacenamiento de Archivos
```python
# Desarrollo: Sistema de archivos local
MEDIA_ROOT = "media/"
MEDIA_URL = "/media/"

# Producción: Azure Blob Storage
STORAGES = {
    "default": {
        "BACKEND": "storages.backends.azure_storage.AzureStorage",
    }
}
```

### Apps Instaladas
```python
INSTALLED_APPS = [
    'django.contrib.admin',
    'django.contrib.auth',
    'django.contrib.contenttypes',
    'django.contrib.sessions',
    'django.contrib.messages',
    'django.contrib.staticfiles',
    'storages',      # Para Azure
    'cv',            # Nuestra aplicación
]
```

---

## 🚀 Instalación y Setup

### 1. Clonar/Descargar Proyecto
```bash
cd /ruta/del/proyecto
```

### 2. Crear Entorno Virtual (Opcional pero Recomendado)
```bash
python -m venv venv
# Windows:
venv\Scripts\activate
# Linux/Mac:
source venv/bin/activate
```

### 3. Instalar Dependencias
```bash
pip install -r requirements.txt
```

### 4. Configurar Variables de Entorno
Crear archivo `.env`:
```env
DEBUG=1
SECRET_KEY=tu-clave-secreta
DATABASE_URL=  # Dejar vacío para SQLite
AZURE_ACCOUNT_NAME=
AZURE_ACCOUNT_KEY=
```

### 5. Aplicar Migraciones
```bash
python manage.py migrate
```

### 6. Crear Superuser
```bash
python manage.py createsuperuser
```

### 7. Iniciar Servidor
```bash
python manage.py runserver
```

### 8. Acceder
```
Página: http://127.0.0.1:8000/
Admin: http://127.0.0.1:8000/admin/
```

---

## 📝 Uso Básico

### Crear tu Primer Perfil

1. Ve a `http://127.0.0.1:8000/admin/`
2. Inicia sesión
3. Ve a `Cv → Datospersonales`
4. Clic en "Agregar Datospersonales"
5. Completa:
   - Nombres: Tu nombre
   - Apellidos: Tu apellido
   - Foto perfil: Sube una foto
   - ✅ Marca "Perfil activo"
6. Clic en "GUARDAR"

### Agregar Contenido (Cursos, Experiencias, etc.)

1. En admin, ve a `Cv → [Sección que quieras]`
2. Clic en "Agregar"
3. Completa los campos
4. ✅ Marca "Activar para que se vea en front"
5. Guarda

### Ver tu Portafolio
```
http://127.0.0.1:8000/
```

### Descargar PDF
```
http://127.0.0.1:8000/imprimir/
```

---

## 🎨 Personalización

### Cambiar Estilos (CSS)
Editar: `cv/static/css/cv_fix.css`
```css
/* Tus cambios de estilo aquí */
```

### Cambiar HTML (Template)
Editar: `cv/templates/home.html`
```django-html
<!-- Tu HTML personalizado aquí -->
```

### Agregar Funcionalidad (Python)
Editar: `cv/views.py`
```python
def home(request):
    # Tu lógica personalizada
    pass
```

### Cambiar Estructura (Base de Datos)
Editar: `cv/models.py`
```python
class MiModelo(models.Model):
    # Tus campos aquí
    pass

# Luego ejecutar:
# python manage.py makemigrations
# python manage.py migrate
```

---

## 🔐 Seguridad

### Variables Sensibles (En Producción)
```env
# NUNCA commitear estos valores
SECRET_KEY=algo-super-secreto
DATABASE_URL=postgresql://...
AZURE_ACCOUNT_KEY=...
```

### Permiso de Impresión
```python
# En admin, desmarcar "Permitir impresión"
# para que no se pueda descargar el PDF
permitir_impresion = False
```

### Perfil Privado
```python
# Desmarcar "Perfil activo"
# para ocultar completamente el portafolio
perfilactivo = False
```

---

## 📊 Tecnologías Usadas

| Tecnología | Versión | Uso |
|------------|---------|-----|
| **Django** | 4.2+ | Framework web |
| **Python** | 3.8+ | Lenguaje |
| **SQLite** | Built-in | BD desarrollo |
| **PostgreSQL** | 12+ | BD producción |
| **ReportLab** | - | Generación PDF |
| **Pillow** | - | Procesamiento imágenes |
| **Azure Storage** | - | Almacenamiento nube |
| **HTML5** | - | Estructura |
| **CSS3** | - | Estilos |
| **JavaScript** | - | Interactividad |

---

## 📦 Dependencias Principales

```
Django>=4.2
gunicorn              # Servidor producción
psycopg2-binary       # PostgreSQL
dj-database-url       # Variables BD
python-dotenv         # Variables de entorno
whitenoise            # Archivos estáticos
django-storages[azure] # Azure Storage
azure-storage-blob    # Blob Storage
Pillow                # Imágenes
PyPDF2                # Manipulación PDF
requests              # HTTP requests
reportlab             # Generación PDF
```

---

## 🐛 Debugging

### Ver Errores en Consola
```bash
# Cuando runserver está abierto
# Los errores aparecen automáticamente
```

### Ver Errores en Base de Datos
```python
python manage.py shell

from cv.models import Datospersonales
Datospersonales.objects.all()  # Si sale error, revisar BD
```

### Logs de SQL
```python
# En settings.py, agregar:
LOGGING = {
    'version': 1,
    'handlers': {
        'console': {
            'class': 'logging.StreamHandler',
        },
    },
    'loggers': {
        'django.db.backends': {
            'handlers': ['console'],
            'level': 'DEBUG',
        },
    },
}
```

---

## 🚀 Deploy (Producción)

### En Azure
```
1. Crear App Service en Azure
2. Conectar repositorio Git
3. Agregar variables de entorno en Azure
4. Configurar PostgreSQL
5. Deploy automático
```

### En Heroku
```
1. Instalar Heroku CLI
2. heroku login
3. heroku create
4. git push heroku main
5. heroku run python manage.py migrate
```

### En tu propio servidor
```
1. Instalar Python y pip
2. Clonar proyecto
3. Crear entorno virtual
4. Instalar dependencias
5. Configurar gunicorn + nginx
6. Configurar supervisor para mantener app viva
```

---

## 📈 Estadísticas del Proyecto

| Métrica | Valor |
|---------|-------|
| **Modelos** | 7 |
| **Vistas** | 8 |
| **URLs** | 9 |
| **Templates** | 8+ |
| **Campos BD** | 60+ |
| **Líneas de código** | 600+ |
| **Migraciones** | 18 |

---

## 🤝 Estructura de Datos Relacionales

```
Datospersonales (1)
    ↓
    ├─→ (M) Cursosrealizados
    ├─→ (M) Experiencialaboral
    ├─→ (M) Productosacademicos
    ├─→ (M) Productoslaborales
    ├─→ (M) Reconocimientos
    └─→ (M) Ventagarage
```

Ejemplo:
```python
perfil = Datospersonales.objects.get(idperfil=1)

# Acceder a datos relacionados
cursos = perfil.cursos.all()
experiencias = perfil.experiencias.all()
productos = perfil.productos_academicos.all()

# Filtrar
activos = perfil.cursos.filter(activarparaqueseveaenfront=True)

# Contar
cantidad = perfil.cursos.count()
```

---

## 🎓 Conceptos Django Utilizados

### MTV (Model-Template-View)
```
Model   → Define datos (cv/models.py)
Template → Presenta datos (cv/templates/)
View    → Procesa datos (cv/views.py)
```

### ORM (Object-Relational Mapping)
```python
# En lugar de SQL directo:
Datospersonales.objects.filter(perfilactivo=True)
# Es más seguro y fácil
```

### QuerySet (Conjunto de Datos)
```python
# Lazy evaluation
queryset = Datospersonales.objects.filter(...)  # No ejecuta
resultado = queryset.first()                     # AHORA ejecuta
```

### Context (Contexto)
```python
context = {"perfil": perfil, "counts": counts}
# En template: {{ perfil.nombres }}
```

### Validadores
```python
# Validar datos antes de guardar
cedula = models.CharField(validators=[cedula_10_digitos])
```

---

## 📚 Documentos Incluidos

### Para Aprender Rápido
→ Lee: `INICIO_RAPIDO_PAGINA_INICIO.md`

### Para Entender a Fondo
→ Lee: `DOCUMENTACION_PAGINA_INICIO.md`

### Para Ver Diagramas
→ Lee: `DIAGRAMAS_PAGINA_INICIO.md`

### Para Crear tu Perfil
→ Lee: `TUTORIAL_CREAR_PERFIL.md`

---

## 🆘 Soporte

### Problemas Comunes

**P: "No aparece la página"**
R: Verifica que hay perfil activo en admin

**P: "Error 500"**
R: Revisa la consola donde corre `runserver`

**P: "Cambios no se ven"**
R: Presiona Ctrl+F5 para refrescar caché

**P: "No puedo subir foto"**
R: Verifica permisos en carpeta `media/`

---

## 🎯 Próximos Pasos

1. ✅ Crear tu perfil
2. ✅ Agregar cursos y experiencias
3. ✅ Personalizar estilos CSS
4. ✅ Modificar templates HTML
5. ✅ Agregar funcionalidades Python
6. ✅ Configurar para producción
7. ✅ Desplegar a servidor

---

## 📞 Contacto y Ayuda

Para dudas específicas:
1. Revisa la documentación completa
2. Consulta los comentarios en el código
3. Abre issue en GitHub (si tienes repo)
4. Contacta al autor

---

## 📄 Licencia

Este proyecto es de código abierto. Úsalo libremente para tu portafolio.

---

## ✨ ¡Bienvenido!

Ahora tienes tu propio CV digital. 

**¡Haz que brille!** ✨

