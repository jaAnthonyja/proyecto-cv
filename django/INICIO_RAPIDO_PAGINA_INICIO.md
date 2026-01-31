# 🚀 Guía de Inicio Rápido - Página de Inicio

## ¿Qué es la página de inicio?

Es el **punto central** de tu portafolio digital. Muestra:
- ✅ Tu foto y datos personales
- ✅ Navegación a 7 secciones del CV
- ✅ Contador rápido de elementos
- ✅ Botón para descargar CV en PDF

---

## 📍 ¿Por dónde empieza el código?

### 1️⃣ **Usuario entra al sitio**
```
http://127.0.0.1:8000/  ← Aquí
```

### 2️⃣ **Django busca en cv/urls.py**
```python
path("", views.home, name="home")  ← Encuentra esto
```

### 3️⃣ **Se ejecuta cv/views.py**
```python
def home(request):
    # Aquí comienza la lógica
```

### 4️⃣ **Se renderiza cv/templates/home.html**
```django-html
<!-- Aquí va el HTML que ves en navegador -->
```

---

## 🔄 Proceso en 4 pasos

### Paso 1: Obtener datos del perfil
```python
perfil = Datospersonales.objects.filter(perfilactivo=True).first()
```
**Trae:**
- Nombre, apellido, foto
- Descripción, datos personales
- Información de contacto

### Paso 2: Contar elementos activos
```python
counts = {
    "cursos": perfil.cursos.filter(activarparaqueseveaenfront=True).count(),
    "experiencias": perfil.experiencias.filter(...).count(),
    ...
}
```
**Resultado:** Números que aparecen en el panel derecho

### Paso 3: Verificar permisos
```python
permitir_impresion = bool(perfil and perfil.permitir_impresion)
```
**Determina:** Si se muestra o no el botón "Descargar PDF"

### Paso 4: Renderizar template
```python
return render(request, "home.html", {
    "perfil": perfil,
    "permitir_impresion": permitir_impresion,
    "counts": counts,
})
```
**Resultado:** HTML que ve el usuario

---

## 📊 Estructura Visual

```
┌─────────────────────────────────────────────────────────────┐
│         PÁGINA DE INICIO - home.html                        │
├─────────────────────────────────────────────────────────────┤
│                                                             │
│  [FOTO]  Nombre Completo                 │ Resumen Rápido  │
│          Descripción perfil              │                 │
│                                          │ 5 Cursos        │
│  • Nacionalidad: XX                      │ 3 Experiencias  │
│  • Lugar: XX                             │ 2 Reconoc.      │
│  • Nacimiento: XX                        │ 1 Prod. Acad.   │
│  • Cédula: XX                            │ 2 Prod. Lab.    │
│  • Sexo: XX                              │ 3 Venta         │
│  • Web: [Enlace]                         │                 │
│                                          │ Impresión: ✅   │
│  NAVEGACIÓN (7 secciones)                │                 │
│  ┌──────────────────────────────────┐    └─────────────────┘
│  │ 🧍 Datos personales              │
│  │ 🎓 Cursos realizados             │
│  │ 🛠️ Experiencia laboral            │
│  │ 📘 Productos académicos          │
│  │ 💼 Productos laborales           │
│  │ 🏅 Reconocimientos               │
│  │ 🏷️ Venta garage                  │
│  └──────────────────────────────────┘
│
└─────────────────────────────────────────────────────────────┘
```

---

## 🗄️ Base de Datos: Tabla Datospersonales

```
Tabla: Datospersonales
┌─────────────────────────────────────────────────┐
│ idperfil .............. Identificador (PK)     │
│ nombres ............... "Juan"                 │
│ apellidos ............. "Pérez"                │
│ foto_perfil ........... archivo.jpg            │
│ descripcionperfil ...... "Ingeniero con..."    │
│ nacionalidad ........... "Colombiano"          │
│ lugarnacimiento ........ "Medellín"            │
│ fechanacimiento ........ "1990-05-15"          │
│ numerocedula ........... "1234567890"          │
│ sexo ................... "H" o "M"            │
│ sitioweb ............... "https://..."        │
│ perfilactivo ........... True / False  ⭐     │
│ permitir_impresion ..... True / False          │
└─────────────────────────────────────────────────┘
```

**Campos importantes:**
- `perfilactivo=True` → Este perfil se muestra en el sitio
- `permitir_impresion=True` → Se puede descargar PDF

---

## 🔗 Relaciones con otras tablas

```
Datospersonales (1) tiene muchos (M) Cursosrealizados
Datospersonales (1) tiene muchos (M) Experiencialaboral
Datospersonales (1) tiene muchos (M) Productosacademicos
Datospersonales (1) tiene muchos (M) Productoslaborales
Datospersonales (1) tiene muchos (M) Reconocimientos
Datospersonales (1) tiene muchos (M) Ventagarage
```

Ejemplo:
```python
perfil = Datospersonales.objects.get(idperfil=1)

# Acceder a cursos del perfil
cursos = perfil.cursos.all()  # Todos los cursos
cursos_activos = perfil.cursos.filter(activarparaqueseveaenfront=True)  # Solo activos
```

---

## 💾 ¿Cómo editar los datos?

### Opción 1: Panel de Admin (Recomendado)
```
1. Ve a http://127.0.0.1:8000/admin/
2. Inicia sesión
3. Ve a "Cv" → "Datospersonales"
4. Edita el perfil
5. Marca "perfilactivo" como True
6. Guarda cambios
```

### Opción 2: Shell de Django
```bash
python manage.py shell

# Crear un nuevo perfil
from cv.models import Datospersonales

perfil = Datospersonales.objects.create(
    nombres="Juan",
    apellidos="Pérez",
    descripcionperfil="Ingeniero de software",
    nacionalidad="Colombiano",
    sexo="H",
    perfilactivo=True,
    permitir_impresion=True
)

# Editar existente
perfil = Datospersonales.objects.get(idperfil=1)
perfil.nombres = "Carlos"
perfil.save()

# Ver todos
for p in Datospersonales.objects.all():
    print(p.nombres, p.apellidos)
```

---

## ⚙️ ¿Qué archivos cambiar si quiero modificar la página?

### Si quiero cambiar la **vista** (lógica)
📝 Editar: `cv/views.py`
```python
def home(request):
    # Aquí va tu lógica personalizada
```

### Si quiero cambiar el **HTML** (estructura)
📝 Editar: `cv/templates/home.html`
```django-html
<!-- Aquí va tu HTML personalizado -->
```

### Si quiero cambiar los **estilos** (CSS)
📝 Editar: `cv/static/css/cv_fix.css` o `ui_nav.css`
```css
/* Aquí van tus estilos */
```

### Si quiero cambiar la **ruta** (URL)
📝 Editar: `cv/urls.py`
```python
path("", views.home, name="home")  # Cambiar la cadena ""
```

### Si quiero cambiar la **estructura de datos**
📝 Editar: `cv/models.py`
```python
class Datospersonales(models.Model):
    # Aquí va tu estructura
```
✅ **IMPORTANTE:** Después de cambios en modelos, hacer:
```bash
python manage.py makemigrations
python manage.py migrate
```

---

## 🧪 Verificar que todo funciona

### 1. ¿Hay servidor corriendo?
```bash
python manage.py runserver
```

### 2. ¿Hay un perfil activo?
```bash
python manage.py shell
from cv.models import Datospersonales
print(Datospersonales.objects.filter(perfilactivo=True).count())
# Debe mostrar un número ≥ 1
```

### 3. ¿Renderiza correctamente?
```
Ve a http://127.0.0.1:8000/
¿Ves la página? ✅
¿Ves tu nombre y foto? ✅
¿Ves los 7 botones de secciones? ✅
¿Ves los contadores? ✅
```

---

## 🐛 Solución de problemas

| Problema | Causa | Solución |
|----------|-------|----------|
| "No hay perfil activo" | No existe perfil con `perfilactivo=True` | Crear/activar un perfil en admin |
| Foto no aparece | Campo `foto_perfil` vacío | Cargar foto en admin |
| Contadores muestran 0 | No hay elementos activos | Crear elementos y marcar `activarparaqueseveaenfront=True` |
| Error 500 | Tabla no existe o migraciones no aplicadas | `python manage.py migrate` |
| URL no funciona | Ruta incorrecta en urls.py | Verificar `cv/urls.py` y `django_portfolio/urls.py` |
| CSS no carga | Archivos static no recolectados | `python manage.py collectstatic` |

---

## 📚 Conceptos Clave para la Tesis

### 1. **MTV (Model-Template-View)**
```
Model (Datospersonales)  → Define estructura de datos
     ↓
View (home function)      → Procesa datos
     ↓
Template (home.html)      → Presenta datos al usuario
```

### 2. **ORM (Object-Relational Mapping)**
```python
# En lugar de SQL:
# SELECT * FROM Datospersonales WHERE perfilactivo=True

# Usamos:
Datospersonales.objects.filter(perfilactivo=True)
```

### 3. **Context (Contexto)**
```python
# Datos que pasan de view a template
context = {
    "perfil": perfil,
    "permitir_impresion": permitir_impresion,
    "counts": counts,
}
# En template: {{ perfil.nombres }}
```

### 4. **QuerySet (Conjunto de datos)**
```python
# Lazy evaluation (no ejecuta hasta que se necesita)
cursos = perfil.cursos.filter(...)  # NO ejecuta aún
count = cursos.count()              # AHORA ejecuta
```

### 5. **Relaciones de Base de Datos**
```python
# Uno a Muchos (1:M)
perfil.cursos.all()        # Acceso directo a todos los cursos
perfil.cursos.filter(...)  # Filtrar cursos
perfil.cursos.count()      # Contar cursos
```

---

## 📖 Referencia Rápida

### Comandos útiles
```bash
# Iniciar servidor
python manage.py runserver

# Entrar a shell Django
python manage.py shell

# Crear migraciones
python manage.py makemigrations

# Aplicar migraciones
python manage.py migrate

# Recolectar archivos estáticos
python manage.py collectstatic

# Ver todas las URLs
python manage.py show_urls
```

### Código common en views
```python
# Obtener un objeto
perfil = Datospersonales.objects.get(idperfil=1)

# Obtener todos
perfiles = Datospersonales.objects.all()

# Filtrar
activos = Datospersonales.objects.filter(perfilactivo=True)

# Contar
total = Datospersonales.objects.count()

# Ordenar
ordenado = Datospersonales.objects.order_by("-idperfil")

# Combinar
resultado = Datospersonales.objects.filter(perfilactivo=True).order_by("-idperfil").first()
```

### Código común en templates
```django-html
{# Mostrar variable #}
{{ perfil.nombres }}

{# Condicional #}
{% if perfil %}
    <p>Hay perfil</p>
{% else %}
    <p>No hay perfil</p>
{% endif %}

{# Bucle #}
{% for curso in cursos %}
    <p>{{ curso.nombrecurso }}</p>
{% endfor %}

{# URL reversa #}
<a href="{% url 'home' %}">Inicio</a>

{# Filtros #}
{{ perfil.nombres|upper }}
{{ perfil.fechanacimiento|date:"Y-m-d" }}
```

---

## 🎯 Resumen Final

| Aspecto | Ubicación | Función |
|---------|-----------|---------|
| **Ruta** | `cv/urls.py` | Mapea `/` a `views.home` |
| **Lógica** | `cv/views.py` | Obtiene datos y contadores |
| **HTML** | `cv/templates/home.html` | Renderiza la página |
| **Estilos** | `cv/static/css/` | Diseño visual |
| **Datos** | `db.sqlite3` + `Datospersonales` | Almacena información |

**Flow:** Usuario → Django → BD → Vista → Template → HTML → Navegador

