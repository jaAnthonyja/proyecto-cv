# 📄 Documentación: Configuración de la Página de Inicio

## 📋 Resumen Ejecutivo

La página de inicio (`home.html`) es el punto central del proyecto. Es una aplicación tipo **Portafolio/CV Digital** que muestra:
- **Datos personales del usuario** (foto, nombre, descripción)
- **Resumen rápido** con contadores de secciones
- **Navegación** a todas las secciones del CV
- **Opción de impresión** a PDF

---

## 🏗️ Arquitectura General

```
Usuario → URL (/) → View: home() → Template: home.html → Respuesta HTML
```

### Flujo de Datos:

```
1. Usuario accede a http://127.0.0.1:8000/
   ↓
2. Django enruta a urls.py → path("", views.home, name="home")
   ↓
3. Se ejecuta la función view home()
   ↓
4. Se obtiene el perfil activo de la BD
   ↓
5. Se calculan contadores de cada sección
   ↓
6. Se renderiza template home.html con los datos
   ↓
7. El navegador muestra la página HTML
```

---

## 📍 Archivo: `cv/urls.py`

### Definición de Rutas

```python
from django.urls import path
from . import views

urlpatterns = [
    path("", views.home, name="home"),  # ← Página de inicio (raíz)
    
    # Secciones del CV
    path("datos-personales/", views.datos_personales, name="datos_personales"),
    path("cursos/", views.cursos, name="cursos"),
    path("experiencia/", views.experiencia, name="experiencia"),
    path("productos-academicos/", views.productos_academicos, name="productos_academicos"),
    path("productos-laborales/", views.productos_laborales, name="productos_laborales"),
    path("reconocimientos/", views.reconocimientos, name="reconocimientos"),
    path("venta-garage/", views.venta_garage, name="venta_garage"),
    
    # Generador de PDF
    path("imprimir/", views.imprimir_hoja_vida, name="imprimir_hoja_vida"),
]
```

**Explicación:**
- `path("", ...)` → Ruta vacía (raíz `/`)
- `name="home"` → Alias para referenciar en templates: `{% url 'home' %}`
- Las demás rutas son secciones del CV (información adicional)

---

## 🔧 Archivo: `cv/views.py` (Función `home()`)

### Código de la Vista

```python
def home(request):
    # 1. Obtener el perfil activo de la base de datos
    perfil = _get_perfil_activo()
    
    # 2. Verificar si se permite la impresión a PDF
    permitir_impresion = bool(perfil and perfil.permitir_impresion)
    
    # 3. Calcular contadores de cada sección
    counts = {
        "cursos": perfil.cursos.filter(activarparaqueseveaenfront=True).count() if perfil else 0,
        "experiencias": perfil.experiencias.filter(activarparaqueseveaenfront=True).count() if perfil else 0,
        "prod_acad": perfil.productos_academicos.filter(activarparaqueseveaenfront=True).count() if perfil else 0,
        "prod_lab": perfil.productos_laborales.filter(activarparaqueseveaenfront=True).count() if perfil else 0,
        "reconoc": perfil.reconocimientos.filter(activarparaqueseveaenfront=True).count() if perfil else 0,
        "venta": perfil.venta_garage.filter(activarparaqueseveaenfront=True).count() if perfil else 0,
    }
    
    # 4. Renderizar template con los datos
    return render(request, "home.html", {
        "perfil": perfil,
        "permitir_impresion": permitir_impresion,
        "counts": counts,
    })
```

### Desglose Paso a Paso

#### **Paso 1: Obtener el Perfil Activo**

```python
def _get_perfil_activo():
    return Datospersonales.objects.filter(perfilactivo=True).order_by("-idperfil").first()
```

**¿Qué hace?**
- Busca en la tabla `Datospersonales` todos los registros donde `perfilactivo=True`
- Los ordena por `idperfil` descendente (más reciente primero)
- Devuelve el primer resultado (o `None` si no hay)

**¿Por qué?**
- Solo hay UN perfil "activo" visible en el sitio
- Si no hay perfil activo, la página no muestra nada

#### **Paso 2: Permiso de Impresión**

```python
permitir_impresion = bool(perfil and perfil.permitir_impresion)
```

**¿Qué hace?**
- Verifica si existe el perfil Y si la columna `permitir_impresion` es `True`
- Devuelve un booleano que controla si aparece el botón "Descargar PDF"

#### **Paso 3: Contar Elementos**

```python
counts = {
    "cursos": perfil.cursos.filter(activarparaqueseveaenfront=True).count() if perfil else 0,
    ...
}
```

**¿Qué hace?**
- Por cada sección, cuenta cuántos elementos están **activados para verse en el front**
- Si no hay perfil, devuelve 0
- Estos números aparecen en el "Resumen rápido" de la derecha

**Tabla de Relaciones:**
| Campo en Datospersonales | Cuenta |
|---|---|
| `perfil.cursos.filter(...)` | Cursos realizados |
| `perfil.experiencias.filter(...)` | Experiencias laborales |
| `perfil.productos_academicos.filter(...)` | Productos académicos |
| `perfil.productos_laborales.filter(...)` | Productos laborales |
| `perfil.reconocimientos.filter(...)` | Reconocimientos |
| `perfil.venta_garage.filter(...)` | Artículos en venta |

---

## 🎨 Archivo: `cv/templates/home.html`

### Estructura HTML

```django-html
{% extends "base.html" %}
{% block title %}Inicio{% endblock %}

{% block content %}
<div class="home-shell home-shell--grid">
    
    {% if perfil %}
        <!-- COLUMNA IZQUIERDA: Perfil + Secciones -->
        <div class="home-left">
            
            <!-- SECCIÓN 1: Perfil -->
            <section class="home-profile">
                <div class="hp-left">
                    {% if perfil.foto_perfil %}
                        <img class="hp-avatar" src="{{ perfil.foto_perfil.url }}" alt="Foto">
                    {% endif %}
                </div>
                
                <div class="hp-right">
                    <h1 class="hp-name">{{ perfil.nombres }} {{ perfil.apellidos }}</h1>
                    {% if perfil.descripcionperfil %}
                        <p class="hp-desc">{{ perfil.descripcionperfil }}</p>
                    {% endif %}
                    
                    <div class="hp-grid">
                        {% if perfil.nacionalidad %}<div><b>Nacionalidad:</b> {{ perfil.nacionalidad }}</div>{% endif %}
                        {% if perfil.lugarnacimiento %}<div><b>Lugar:</b> {{ perfil.lugarnacimiento }}</div>{% endif %}
                        {% if perfil.fechanacimiento %}<div><b>Nacimiento:</b> {{ perfil.fechanacimiento }}</div>{% endif %}
                        {% if perfil.numerocedula %}<div><b>Cédula:</b> {{ perfil.numerocedula }}</div>{% endif %}
                        {% if perfil.sexo %}<div><b>Sexo:</b> {{ perfil.sexo }}</div>{% endif %}
                        
                        {% if perfil.sitioweb %}
                            <div>
                                <b>Web:</b>
                                <a class="btn-outline" href="{{ perfil.sitioweb }}" target="_blank">
                                    Abrir enlace
                                </a>
                            </div>
                        {% endif %}
                    </div>
                </div>
            </section>
            
            <!-- SECCIÓN 2: Navegación a Secciones -->
            <section class="home-sections">
                <h2 class="hs-title">Secciones</h2>
                <div class="hs-grid">
                    <a class="hs-card" href="{% url 'datos_personales' %}">🧍 Datos personales</a>
                    <a class="hs-card" href="{% url 'cursos' %}">🎓 Cursos realizados</a>
                    <a class="hs-card" href="{% url 'experiencia' %}">🛠️ Experiencia laboral</a>
                    <a class="hs-card" href="{% url 'productos_academicos' %}">📘 Productos académicos</a>
                    <a class="hs-card" href="{% url 'productos_laborales' %}">💼 Productos laborales</a>
                    <a class="hs-card" href="{% url 'reconocimientos' %}">🏅 Reconocimientos</a>
                    <a class="hs-card" href="{% url 'venta_garage' %}">🏷️ Venta garage</a>
                </div>
            </section>
        </div>
        
        <!-- COLUMNA DERECHA: Resumen Rápido -->
        <aside class="home-right">
            <div class="side-card">
                <div class="side-head">
                    <div class="side-title">Resumen rápido</div>
                    <div class="side-chip">CV</div>
                </div>
                
                <div class="side-metrics">
                    <div class="metric">
                        <div class="m-num">{{ counts.cursos }}</div>
                        <div class="m-lbl">Cursos</div>
                    </div>
                    <!-- ... más métricas ... -->
                </div>
            </div>
        </aside>
        
    {% else %}
        <!-- Si no hay perfil activo, mostrar mensaje -->
        <p>No hay perfil activo para mostrar.</p>
    {% endif %}
</div>
{% endblock %}
```

### Desglose de Componentes

#### **1. Estructura CSS Grid**
```html
<div class="home-shell home-shell--grid">
```
- Usa CSS Grid para distribuir: **columna izquierda** (perfil) + **columna derecha** (resumen)

#### **2. Foto del Perfil**
```django-html
{% if perfil.foto_perfil %}
    <img class="hp-avatar" src="{{ perfil.foto_perfil.url }}" alt="Foto">
{% endif %}
```
- Si existe la imagen, la carga desde el servidor de archivos
- Si no existe, no renderiza nada

#### **3. Datos Personales Mostrados**
| Campo | Columna BD | Tipo |
|---|---|---|
| Nombre | `nombres + apellidos` | CharField |
| Foto | `foto_perfil` | ImageField |
| Descripción | `descripcionperfil` | TextField |
| Nacionalidad | `nacionalidad` | CharField |
| Lugar de Nacimiento | `lugarnacimiento` | CharField |
| Fecha de Nacimiento | `fechanacimiento` | DateField |
| Cédula | `numerocedula` | CharField |
| Sexo | `sexo` | CharField (H/M) |
| Sitio Web | `sitioweb` | URLField |

#### **4. Secciones del CV (Navegación)**
```django-html
<a class="hs-card" href="{% url 'datos_personales' %}">🧍 Datos personales</a>
```
- `{% url 'datos_personales' %}` → Django genera automáticamente la URL
- Equivale a `/datos-personales/`
- El emoji es solo visual

#### **5. Contadores en el Resumen Rápido**
```django-html
<div class="metric">
    <div class="m-num">{{ counts.cursos }}</div>
    <div class="m-lbl">Cursos</div>
</div>
```
- Muestra el número de cursos activos
- Calculado en la vista `home()`

---

## 🗄️ Base de Datos: Tabla `Datospersonales`

### Campos Utilizados en la Página de Inicio

```python
class Datospersonales(models.Model):
    idperfil = models.AutoField(primary_key=True)
    nombres = models.CharField(max_length=200)
    apellidos = models.CharField(max_length=200)
    foto_perfil = models.ImageField(upload_to="perfiles/", null=True, blank=True)
    descripcionperfil = models.TextField(blank=True)
    nacionalidad = models.CharField(max_length=100, blank=True)
    lugarnacimiento = models.CharField(max_length=200, blank=True)
    fechanacimiento = models.DateField(null=True, blank=True)
    numerocedula = models.CharField(max_length=20, blank=True)
    sexo = models.CharField(max_length=1, choices=SEXO_CHOICES, blank=True)
    sitioweb = models.URLField(blank=True)
    
    perfilactivo = models.BooleanField(default=False)  # ← Determina qué perfil mostrar
    permitir_impresion = models.BooleanField(default=True)  # ← Permite/bloquea descarga PDF
    
    # Relaciones (ForeignKey inversos)
    cursos = Relacion a tabla Cursosrealizados
    experiencias = Relacion a tabla Experiencialaboral
    productos_academicos = Relacion a tabla Productosacademicos
    productos_laborales = Relacion a tabla Productoslaborales
    reconocimientos = Relacion a tabla Reconocimientos
    venta_garage = Relacion a tabla Ventagarage
```

### Relaciones con Otras Tablas

```
Datospersonales (1) ←→ (M) Cursosrealizados
Datospersonales (1) ←→ (M) Experiencialaboral
Datospersonales (1) ←→ (M) Productosacademicos
Datospersonales (1) ←→ (M) Productoslaborales
Datospersonales (1) ←→ (M) Reconocimientos
Datospersonales (1) ←→ (M) Ventagarage
```

---

## 🚀 Flujo Completo de Ejecución

### 1️⃣ **Usuario entra al sitio**
```
GET http://127.0.0.1:8000/
```

### 2️⃣ **Django busca la URL coincidente**
```python
# cv/urls.py
path("", views.home, name="home")  # ← Coincide
```

### 3️⃣ **Se ejecuta la vista `home()`**
```python
def home(request):
    # a) Consulta a la BD
    perfil = Datospersonales.objects.filter(perfilactivo=True).first()
    
    # b) Verifica permisos
    permitir_impresion = bool(perfil and perfil.permitir_impresion)
    
    # c) Cuenta elementos activos
    counts = {
        "cursos": perfil.cursos.filter(activarparaqueseveaenfront=True).count(),
        ...
    }
    
    # d) Renderiza template
    return render(request, "home.html", {...})
```

### 4️⃣ **Django renderiza `home.html`**
```django-html
<!-- Muestra el perfil si existe -->
{% if perfil %}
    <h1>{{ perfil.nombres }} {{ perfil.apellidos }}</h1>
    <!-- Más contenido... -->
{% endif %}
```

### 5️⃣ **Navegador renderiza HTML + CSS**
```
┌─────────────────────────────────────┐
│   PÁGINA DE INICIO DEL CV           │
├──────────────────┬──────────────────┤
│ Foto + Nombre    │   Resumen Rápido │
│ Descripción      │   6 Cursos       │
│ Datos básicos    │   3 Experiencias │
│ Secciones (7)    │   ...            │
│                  │   Botón PDF      │
└──────────────────┴──────────────────┘
```

---

## ⚙️ Configuración Importante

### 📍 `django_portfolio/settings.py`

#### **Base de Datos**
```python
# Si no hay DATABASE_URL, usa SQLite
DATABASES = {
    "default": {
        "ENGINE": "django.db.backends.sqlite3",
        "NAME": BASE_DIR / "db.sqlite3",
    }
}
```

#### **Apps Instaladas**
```python
INSTALLED_APPS = [
    "django.contrib.admin",
    "django.contrib.auth",
    "django.contrib.contenttypes",
    "django.contrib.sessions",
    "django.contrib.messages",
    "django.contrib.staticfiles",
    "storages",  # Para almacenar archivos en Azure
    "cv",        # ← Nuestra app
]
```

#### **Plantillas**
```python
TEMPLATES = [
    {
        "BACKEND": "django.template.backends.django.DjangoTemplates",
        "DIRS": [BASE_DIR / "templates"],  # Busca templates aquí
        "APP_DIRS": True,  # También busca en app/templates/
    }
]
```

#### **Archivos Estáticos**
```python
STATIC_URL = "/static/"
STATIC_ROOT = BASE_DIR / "staticfiles"
MEDIA_URL = "https://{AZURE_CUSTOM_DOMAIN}/{AZURE_CONTAINER}/"
```

---

## 🎯 Resumen de Configuración

| Aspecto | Ubicación | Descripción |
|---------|-----------|-------------|
| **URL de inicio** | `cv/urls.py` | `path("", views.home)` |
| **Lógica de página** | `cv/views.py` | `def home(request):` |
| **Plantilla HTML** | `cv/templates/home.html` | Estructura visual |
| **Modelo de datos** | `cv/models.py` | Tabla `Datospersonales` |
| **Relaciones** | `cv/models.py` | 6 tablas relacionadas |
| **Estilos CSS** | `cv/static/css/` | Grid, responsive |
| **Base de datos** | `db.sqlite3` | SQLite (desarrollo) |

---

## 🔍 Verificación

Para ver la página en acción:

1. **Inicia el servidor:**
   ```bash
   python manage.py runserver
   ```

2. **Accede en el navegador:**
   ```
   http://127.0.0.1:8000/
   ```

3. **Si no ves nada:**
   - Verifica que hay un registro en la BD con `perfilactivo=True`
   - Usa el admin de Django para crear/activar un perfil:
     ```
     http://127.0.0.1:8000/admin/
     ```

---

## 📌 Notas para la Tesis

**Este es un ejemplo de:**
- ✅ Arquitectura Django MTV (Model-Template-View)
- ✅ ORM de Django (consultas a BD)
- ✅ Plantillas dinámicas con contexto
- ✅ Relaciones de bases de datos (1-a-M)
- ✅ Validaciones y filtros de visualización
- ✅ Generación de PDFs con ReportLab

**Conceptos clave explicados:**
1. **Vista** → Lógica de negocio (home function)
2. **Modelo** → Estructura de datos (Datospersonales)
3. **Template** → Presentación (home.html)
4. **Routing** → Mapeo de URLs (urls.py)

