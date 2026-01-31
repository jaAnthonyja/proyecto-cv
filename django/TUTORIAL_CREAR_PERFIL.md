# 👤 Tutorial: Crear tu Primer Perfil

## 🎯 Objetivo
Crear un perfil completo que se muestre en la página de inicio con toda tu información personal.

---

## 📋 Requisitos Previos

✅ Django corriendo:
```bash
python manage.py runserver
```

✅ Base de datos migrada:
```bash
python manage.py migrate
```

---

## 🚀 Método 1: Panel de Admin (Recomendado)

### Paso 1: Acceder al Admin
```
1. Abre navegador
2. Ve a: http://127.0.0.1:8000/admin/
3. Inicia sesión (si te pide credenciales, crea un superuser)
```

### Crear Superuser (si no tienes)
```bash
python manage.py createsuperuser
# Ingresa:
# Username: admin
# Email: admin@example.com
# Password: 123456 (o la que quieras)
```

### Paso 2: Ir a Datospersonales
```
1. En admin, ve a: Cv → Datospersonales
2. Haz clic en "Agregar Datospersonales"
```

### Paso 3: Llenar el formulario

#### 📝 Información Personal Obligatoria

```
Nombres: Juan
Apellidos: Pérez García
```

#### 📷 Foto de Perfil (Opcional)
```
Foto perfil: [Subir archivo JPG o PNG]
```

#### 📝 Descripción (Opcional)
```
Descripcionperfil: 
"Ingeniero de Software con 5 años de experiencia en desarrollo 
web con Django y React. Apasionado por la tecnología y la 
educación. Resido en Medellín, Colombia."
```

#### 🌍 Datos Personales
```
Nacionalidad: Colombiano
Lugar nacimiento: Medellín
Fecha nacimiento: 1995-05-20
Número cedula: 1234567890
Sexo: H (H=Hombre, M=Mujer)
```

#### 🔗 Contacto
```
Sitio web: https://tuwebsite.com
Teléfono: +57 1 23456789
Email: tu@email.com
```

#### 🔐 Configuración
```
✅ Perfil activo: MARCAR ESTO (fundamental)
✅ Permitir impresión: MARCAR (para PDF)
```

### Paso 4: Guardar
```
Haz clic en "GUARDAR" abajo a la derecha
```

### Paso 5: Verificar
```
1. Ve a http://127.0.0.1:8000/
2. ¡Deberías ver tu perfil!
```

---

## 🔧 Método 2: Shell de Django

### Paso 1: Abrir Shell
```bash
python manage.py shell
```

### Paso 2: Importar Modelo
```python
from cv.models import Datospersonales
```

### Paso 3: Crear Perfil Mínimo
```python
perfil = Datospersonales.objects.create(
    nombres="Juan",
    apellidos="Pérez",
    nacionalidad="Colombiano",
    perfilactivo=True,
    permitir_impresion=True
)

print(f"✅ Perfil creado con ID: {perfil.idperfil}")
```

### Paso 4: Crear Perfil Completo
```python
perfil = Datospersonales.objects.create(
    # Datos básicos
    nombres="Juan",
    apellidos="Pérez García",
    descripcionperfil="Ingeniero de Software con 5 años de experiencia",
    
    # Información personal
    nacionalidad="Colombiano",
    lugarnacimiento="Medellín",
    fechanacimiento="1995-05-20",
    numerocedula="1234567890",
    sexo="H",
    
    # Contacto
    sitioweb="https://juanperez.com",
    
    # Configuración
    perfilactivo=True,
    permitir_impresion=True
)

print(f"✅ Perfil '{perfil.nombres}' creado exitosamente")
```

### Paso 5: Salir del Shell
```python
exit()
```

---

## 📸 Subir Foto (Shell Avanzado)

### Opción 1: Desde el Admin (más fácil)
```
1. En admin, edita el perfil
2. Haz clic en "Foto perfil"
3. Elige archivo
4. Guarda
```

### Opción 2: Desde Shell
```python
from cv.models import Datospersonales
from django.core.files.base import ContentFile
from PIL import Image
import io

# Obtener perfil existente
perfil = Datospersonales.objects.get(idperfil=1)

# Opción A: Desde archivo local
with open("C:\\Users\\HP\\Pictures\\mifoto.jpg", "rb") as f:
    perfil.foto_perfil.save("foto.jpg", ContentFile(f.read()))

perfil.save()
print("✅ Foto subida")

# Opción B: Desde URL
import requests
url = "https://example.com/foto.jpg"
response = requests.get(url)
perfil.foto_perfil.save("foto.jpg", ContentFile(response.content))
perfil.save()
print("✅ Foto descargada y subida")
```

---

## ✏️ Editar Perfil Existente

### Desde Admin
```
1. Ve a http://127.0.0.1:8000/admin/
2. Cv → Datospersonales
3. Haz clic en el perfil a editar
4. Cambia los datos
5. Guarda
```

### Desde Shell
```python
from cv.models import Datospersonales

# Obtener perfil
perfil = Datospersonales.objects.get(idperfil=1)

# Editar campos
perfil.nombres = "Carlos"
perfil.apellidos = "López"
perfil.descripcionperfil = "Descripción nueva"
perfil.perfilactivo = True
perfil.permitir_impresion = True

# Guardar cambios
perfil.save()

print(f"✅ Perfil actualizado: {perfil.nombres}")
```

---

## 📊 Ver Todos los Perfiles

### En Admin
```
1. Ve a http://127.0.0.1:8000/admin/
2. Cv → Datospersonales
3. Verás lista de todos los perfiles
```

### En Shell
```python
from cv.models import Datospersonales

# Ver todos
for p in Datospersonales.objects.all():
    print(f"{p.idperfil} - {p.nombres} ({p.nacionalidad})")

# Contar
total = Datospersonales.objects.count()
print(f"Total de perfiles: {total}")

# Ver solo activos
activos = Datospersonales.objects.filter(perfilactivo=True)
print(f"Perfiles activos: {activos.count()}")

# Ver solo uno
perfil = Datospersonales.objects.get(idperfil=1)
print(f"Perfil: {perfil.nombres} {perfil.apellidos}")
```

---

## 🚀 Activar/Desactivar Perfil

### Cambiar Perfil Activo

```python
from cv.models import Datospersonales

# Obtener perfil
perfil = Datospersonales.objects.get(idperfil=1)

# Activar este perfil
perfil.perfilactivo = True
perfil.save()

print(f"✅ Perfil {perfil.idperfil} ACTIVADO")

# Ahora desactivar todos los demás
Datospersonales.objects.exclude(idperfil=1).update(perfilactivo=False)
print("✅ Otros perfiles desactivados")
```

### Verificar Cuál está Activo
```python
activo = Datospersonales.objects.filter(perfilactivo=True).first()
if activo:
    print(f"Perfil activo: {activo.nombres} (ID: {activo.idperfil})")
else:
    print("❌ No hay perfil activo")
```

---

## 🔒 Controlar Acceso a Impresión

### Permitir Impresión
```python
perfil = Datospersonales.objects.get(idperfil=1)
perfil.permitir_impresion = True
perfil.save()
print("✅ PDF habilitado")
```

### Bloquear Impresión
```python
perfil = Datospersonales.objects.get(idperfil=1)
perfil.permitir_impresion = False
perfil.save()
print("✅ PDF bloqueado")
```

---

## 🧪 Verificación Completa

### Checklist Post-Creación

```python
from cv.models import Datospersonales

# 1. ¿Existe perfil con ese ID?
try:
    perfil = Datospersonales.objects.get(idperfil=1)
    print("✅ Perfil existe")
except:
    print("❌ Perfil no existe")

# 2. ¿Tiene nombre?
if perfil.nombres:
    print(f"✅ Nombre: {perfil.nombres}")
else:
    print("❌ Sin nombre")

# 3. ¿Está activo?
if perfil.perfilactivo:
    print("✅ Perfil activo")
else:
    print("❌ Perfil inactivo")

# 4. ¿Permite impresión?
if perfil.permitir_impresion:
    print("✅ Impresión habilitada")
else:
    print("❌ Impresión bloqueada")

# 5. ¿Tiene foto?
if perfil.foto_perfil:
    print(f"✅ Foto: {perfil.foto_perfil.url}")
else:
    print("⚠️ Sin foto")

# 6. ¿Ve la página?
print("\nVe a: http://127.0.0.1:8000/")
```

---

## 📝 Campos Opcionales vs Obligatorios

### ✅ OBLIGATORIOS
- `nombres` (CharField)
- `apellidos` (CharField)

### ⚠️ OPCIONALES RECOMENDADOS
- `foto_perfil` (ImageField) - Para foto en página
- `descripcionperfil` (TextField) - Pequeña biografía
- `perfilactivo` (BooleanField) - ¡Muy importante!

### ❓ OPCIONALES INFORMATIVOS
- `nacionalidad`
- `lugarnacimiento`
- `fechanacimiento`
- `numerocedula`
- `sexo`
- `sitioweb`

### 🔒 OPCIONALES DE CONTROL
- `permitir_impresion` - Permite descargar PDF

---

## 🎨 Valores Válidos

### Sexo
```python
sexo = "H"  # Hombre
sexo = "M"  # Mujer
sexo = ""   # Vacío (sin especificar)
```

### Booleanos
```python
perfilactivo = True       # Visible
perfilactivo = False      # Oculto

permitir_impresion = True  # Se puede descargar PDF
permitir_impresion = False # No se puede descargar PDF
```

### Fechas
```python
fechanacimiento = "1995-05-20"  # YYYY-MM-DD
fechanacimiento = None           # Sin especificar
```

### URLs
```python
sitioweb = "https://www.ejemplo.com"
sitioweb = "https://miportafolio.com"
sitioweb = "https://github.com/usuario"
```

---

## 🐛 Problemas Comunes

### Problema: "No aparece en la página"

**Causa:** Perfil no está activo
```python
# Verificar
perfil = Datospersonales.objects.get(idperfil=1)
print(perfil.perfilactivo)  # ¿False?

# Solución
perfil.perfilactivo = True
perfil.save()
```

### Problema: "Foto no aparece"

**Causa:** Campo vacío o formato incorrecto
```python
# Verificar
print(perfil.foto_perfil)  # ¿None o vacío?

# Solución: Subir foto desde admin o shell
```

### Problema: "Datos no aparecen en página"

**Causa:** El servidor está en caché
```bash
# Solución 1: Refrescar navegador (Ctrl+F5)

# Solución 2: Reiniciar servidor
# Presiona Ctrl+C en terminal y ejecuta:
python manage.py runserver
```

### Problema: "Error de permisos en Admin"

**Causa:** Sin acceso de admin
```bash
# Crear superuser
python manage.py createsuperuser
```

---

## 📚 Referencia SQL (Para Ver en BD Directa)

```sql
-- Ver todos los perfiles
SELECT idperfil, nombres, apellidos, perfilactivo FROM cv_datospersonales;

-- Ver solo activos
SELECT idperfil, nombres FROM cv_datospersonales WHERE perfilactivo = 1;

-- Actualizar
UPDATE cv_datospersonales SET nombres = 'Carlos' WHERE idperfil = 1;

-- Activar uno, desactivar otros
UPDATE cv_datospersonales SET perfilactivo = 0;
UPDATE cv_datospersonales SET perfilactivo = 1 WHERE idperfil = 1;
```

---

## 🎯 Próximos Pasos

Una vez tengas un perfil creado:

1. **Ir a otras secciones** para agregar:
   - Cursos realizados
   - Experiencias laborales
   - Productos académicos
   - Productos laborales
   - Reconocimientos
   - Artículos en venta

2. **Personalizar estilos** en `cv/static/css/`

3. **Cambiar el template** en `cv/templates/home.html`

4. **Agregar funcionalidades** en `cv/views.py`

