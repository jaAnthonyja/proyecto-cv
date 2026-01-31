# 📦 ¿Qué es requirements.txt y Cómo Instalar Dependencias?

## 🎯 ¿Qué es requirements.txt?

**requirements.txt** es un archivo de texto que lista **todas las librerías/paquetes** que necesita tu proyecto Python para funcionar.

```
Es como una "lista de compras" para tu proyecto:

Tu Proyecto
└─ Necesita:
   ├─ Django (para crear la web)
   ├─ Pillow (para procesar imágenes)
   ├─ ReportLab (para generar PDFs)
   ├─ Gunicorn (servidor web)
   └─ ... (más paquetes)
   
requirements.txt = Archivo que lista TODO esto
```

---

## 📝 Contenido de requirements.txt

Aquí está el archivo de tu proyecto:

```
Django>=4.2              # Framework web
gunicorn                 # Servidor producción
psycopg2-binary          # Conexión a PostgreSQL
dj-database-url          # Leer URLs de BD
python-dotenv            # Cargar variables .env
whitenoise               # Servir archivos estáticos
django-storages[azure]   # Almacenamiento Azure
azure-storage-blob       # Azure Blob Storage
Pillow                   # Procesamiento de imágenes
PyPDF2                   # Manipulación de PDFs
requests                 # Peticiones HTTP
reportlab                # Generar PDFs
```

### ¿Qué significa cada cosa?

```
Django>=4.2
│      │
│      └─→ >= significa "versión 4.2 o superior"
└─→ Nombre del paquete a instalar
```

---

## 🚀 Paso a Paso: Llevar Proyecto a Otra Computadora

### Paso 1: Preparar el Proyecto (Tu computadora actual)

**✅ Ya tienes hecho:**
- `requirements.txt` ← Archivo con lista de dependencias
- `manage.py` ← Comando Django
- `cv/` ← Código de la app
- `.env` ← Variables de configuración

### Paso 2: Copiar Proyecto a Nueva Computadora

#### Opción A: USB o Email (Simple)
```bash
# En tu computadora:
1. Copia la carpeta completa "django"
2. Pega en USB o comprime a ZIP
3. Envía a otra computadora

# En la nueva computadora:
1. Descomprimir si está en ZIP
2. Copiar carpeta a ubicación deseada
```

#### Opción B: GitHub (Profesional - Recomendado)
```bash
# En tu computadora (primera vez):
cd "C:\Users\HP\Downloads\django (13)\django"

# Inicializar repositorio Git
git init

# Crear archivo .gitignore
echo "db.sqlite3" > .gitignore
echo ".env" >> .gitignore
echo "*.pyc" >> .gitignore
echo "__pycache__/" >> .gitignore

# Preparar para subir
git add .
git commit -m "Initial commit"

# Crear repositorio en GitHub y subir
git remote add origin https://github.com/tuusuario/django-cv.git
git branch -M main
git push -u origin main

# En la nueva computadora:
git clone https://github.com/tuusuario/django-cv.git
cd django-cv
```

### Paso 3: Instalar Dependencias (Lo IMPORTANTE)

```bash
# En la nueva computadora, abre terminal/PowerShell

# Navega a la carpeta del proyecto
cd "C:\ruta\donde\copiaste\la\carpeta\django"

# Verifica que existe requirements.txt
dir requirements.txt  # Debe mostrar el archivo

# INSTALA LAS DEPENDENCIAS
pip install -r requirements.txt

# ¡Listo! Ahora tienes todo instalado
```

---

## 💾 ¿Qué NO Debe Copiarse?

Estos archivos **NO necesitas copiar** (se crean automáticamente):

```
❌ db.sqlite3          → Base de datos (se crea con migrate)
❌ __pycache__/        → Archivos compilados Python
❌ *.pyc              → Archivos compilados
❌ venv/              → Entorno virtual (se crea nuevo)
❌ .env (opcional)    → Archivo de variables (crear uno nuevo)
❌ media/             → Archivos subidos (se crea)
❌ staticfiles/       → Archivos estáticos compilados
```

### ¿Por qué no copiarlos?

```
db.sqlite3
├─ Puede tener datos viejos
└─ Se crea automáticamente con: python manage.py migrate

venv/ (Entorno virtual)
├─ Contiene paquetes compilados para TU sistema
├─ Puede no funcionar en otra computadora
└─ Se crea nuevo con: python -m venv venv
```

---

## 🔧 Instalación Detallada en Nueva Computadora

### Requisito Previo: Tener Python Instalado

```bash
# Verifica que tienes Python
python --version

# Si no muestra versión, descarga de https://www.python.org/
# Asegúrate de marcar "Add Python to PATH" durante instalación
```

### Pasos Completos

#### 1️⃣ Navega a la carpeta del proyecto

```bash
# Windows PowerShell
cd "C:\Users\TuUsuario\Desktop\django"

# Linux/Mac
cd ~/Desktop/django
```

#### 2️⃣ Crea un entorno virtual (Recomendado)

```bash
# Windows
python -m venv venv
venv\Scripts\activate

# Linux/Mac
python3 -m venv venv
source venv/bin/activate

# Resultado: Verás (venv) al inicio de la terminal
# (venv) C:\Users\TuUsuario\Desktop\django>
```

#### 3️⃣ Instala los requerimientos

```bash
# Comando principal
pip install -r requirements.txt

# Espera a que termine (puede tomar 2-5 minutos)
# Verás barras de progreso y "Successfully installed..."
```

#### 4️⃣ Crea la base de datos

```bash
# Aplicar migraciones
python manage.py migrate

# Resultado: "Operations to perform: Apply all migrations..."
```

#### 5️⃣ Crea usuario admin (opcional pero recomendado)

```bash
python manage.py createsuperuser

# Te pedirá:
# Username: admin
# Email: admin@example.com
# Password: tu_contraseña
```

#### 6️⃣ ¡Inicia el servidor!

```bash
python manage.py runserver

# Verás:
# Starting development server at http://127.0.0.1:8000/
```

#### 7️⃣ Accede en navegador

```
http://127.0.0.1:8000/      → Tu sitio
http://127.0.0.1:8000/admin → Panel administrador
```

---

## 📊 Visual: Flujo de Instalación

```
COMPUTADORA ORIGINAL              NUEVA COMPUTADORA
│                                 │
├─ django/                        ├─ COPIA LA CARPETA
│  ├─ cv/                         │  ├─ cv/
│  ├─ django_portfolio/           │  ├─ django_portfolio/
│  ├─ manage.py                   │  ├─ manage.py
│  ├─ requirements.txt ◄──────────┤  ├─ requirements.txt ✓
│  ├─ .env (opcional) ◄───────────┤  ├─ .env (crear nuevo)
│  └─ db.sqlite3 ✗ (NO COPIAR)    │  └─ (no existe aún)
│                                 │
└─ (Tu sistema está listo)        └─ pip install -r requirements.txt
                                    └─ python manage.py migrate
                                      └─ ✅ Listo para usar
```

---

## 🎯 Ejemplo Completo: Paso a Paso

### Scenario: Llevar proyecto a laptop

```bash
═══════════════════════════════════════════════════════════
PASO 1: COPIAR CARPETA
═══════════════════════════════════════════════════════════

# En computadora original:
# Copia: C:\Users\HP\Downloads\django (13)\django
# A USB, Google Drive, o GitHub

═══════════════════════════════════════════════════════════
PASO 2: PREPARAR NUEVA COMPUTADORA
═══════════════════════════════════════════════════════════

# En la laptop (nueva computadora)
# Pega la carpeta en: C:\Users\TuUsuario\Desktop\django

═══════════════════════════════════════════════════════════
PASO 3: ABRIR TERMINAL EN LA CARPETA
═══════════════════════════════════════════════════════════

# Opción 1: Click derecho → "Abrir terminal aquí"
# Opción 2: Abrir PowerShell y hacer:
cd C:\Users\TuUsuario\Desktop\django

# Verifica que estés en el lugar correcto:
dir requirements.txt
# Debe mostrar: requirements.txt ✓

═══════════════════════════════════════════════════════════
PASO 4: CREAR ENTORNO VIRTUAL
═══════════════════════════════════════════════════════════

python -m venv venv
venv\Scripts\activate

# Resultado: (venv) aparece en la terminal

═══════════════════════════════════════════════════════════
PASO 5: INSTALAR PAQUETES
═══════════════════════════════════════════════════════════

pip install -r requirements.txt

# Verá algo como:
# Collecting Django (from -r requirements.txt (line 1))
#   Using cached Django-4.2.x-py3-none-any.whl
# Collecting gunicorn (from -r requirements.txt (line 2))
# ...
# Successfully installed Django-4.2.x gunicorn-21.x ...

═══════════════════════════════════════════════════════════
PASO 6: MIGRAR BD
═══════════════════════════════════════════════════════════

python manage.py migrate

# Verá:
# Operations to perform:
#   Apply all migrations: admin, auth, contenttypes, cv, sessions
# Running migrations:
#   Applying contenttypes.0001_initial... OK
# ...

═══════════════════════════════════════════════════════════
PASO 7: CREAR ADMIN (OPCIONAL)
═══════════════════════════════════════════════════════════

python manage.py createsuperuser

# Ingresa datos:
Username: admin
Email: admin@example.com
Password: 12345678

═══════════════════════════════════════════════════════════
PASO 8: INICIAR SERVIDOR
═══════════════════════════════════════════════════════════

python manage.py runserver

# Verá:
# Watching for file changes with StatReloader
# Performing system checks...
# System check identified no issues (0 silenced)
# January 30, 2026 - 15:30:00
# Django version 4.2.27, using settings 'django_portfolio.settings'
# Starting development server at http://127.0.0.1:8000/
# Quit the server with CTRL-BREAK.

═══════════════════════════════════════════════════════════
¡LISTO! 🎉
═══════════════════════════════════════════════════════════

Abre navegador: http://127.0.0.1:8000/
```

---

## 🔧 ¿Qué hace cada paquete?

### Lista Completa

| Paquete | Función | ¿Necesario? |
|---------|---------|-----------|
| **Django** | Framework web principal | ✅ SÍ |
| **gunicorn** | Servidor para producción | ⚠️ Producción |
| **psycopg2-binary** | Conectar a PostgreSQL | ⚠️ Si usas PG |
| **dj-database-url** | Leer URL de BD del .env | ⚠️ Producción |
| **python-dotenv** | Cargar variables .env | ✅ SÍ |
| **whitenoise** | Servir archivos CSS/JS | ✅ SÍ |
| **django-storages** | Guardar en Azure/AWS | ⚠️ Si usas nube |
| **azure-storage-blob** | Azure Cloud Storage | ⚠️ Si usas Azure |
| **Pillow** | Procesar imágenes | ✅ SÍ |
| **PyPDF2** | Manipular PDFs | ⚠️ Si usas PDF |
| **requests** | Hacer peticiones HTTP | ✅ SÍ |
| **reportlab** | Generar PDFs | ✅ SÍ (importante) |

---

## 🚨 Errores Comunes y Soluciones

### Error: "Python no se reconoce"

```bash
# Problema: Python no está en PATH

# Solución 1: Desinstala e reinstala Python
# Marca "Add Python to PATH" durante instalación

# Solución 2: Usa la ruta completa
C:\Python313\python.exe --version

# Solución 3: En PowerShell como Admin
$env:Path += ";C:\Python313"
```

### Error: "pip no se reconoce"

```bash
# Solución
python -m pip install -r requirements.txt
```

### Error: "pip está desactualizado"

```bash
# Actualizar pip
python -m pip install --upgrade pip

# Windows PowerShell como Admin
python -m pip install --upgrade pip
```

### Error: "No se encuentra requirements.txt"

```bash
# Verificar que estés en carpeta correcta
dir
# Debe mostrar: requirements.txt

# Si no lo ves, navega a la carpeta correcta
cd C:\Users\...\django
```

### Error: "El entorno virtual no funciona"

```bash
# Verificar activación
(venv) C:\Users\...  # ✓ Debe mostrar (venv)

# Si no lo ves, actívalo
venv\Scripts\activate  # Windows
source venv/bin/activate  # Linux/Mac
```

### Error: "Módulos no encontrados después de instalar"

```bash
# Solución: Verifica que entorno virtual está activo
(venv) C:\Users\...  # ✓ Debe mostrar (venv)

# Si no, actívalo nuevamente
venv\Scripts\activate
```

---

## 💡 Tips Útiles

### Crear archivo requirements.txt desde cero

Si por alguna razón necesitas recrear el archivo:

```bash
# En tu entorno virtual activado:
pip freeze > requirements.txt

# Esto crea archivo con todos los paquetes instalados
```

### Ver qué paquetes están instalados

```bash
# Ver lista instalada
pip list

# Ver solo paquetes de proyecto (sin dependencias)
pip list --not-required
```

### Actualizar todos los paquetes

```bash
# Actualizar uno
pip install --upgrade Django

# Actualizar todos
pip install --upgrade -r requirements.txt
```

### Desinstalar un paquete

```bash
pip uninstall nombre_paquete

# Desinstalar todos (¡CUIDADO!)
pip uninstall -r requirements.txt -y
```

---

## 🔐 Variables de Entorno (.env)

### Qué es

Archivo con información sensible que **NO debe subirse a GitHub**:

```env
DEBUG=1
SECRET_KEY=tu-clave-super-secreta
DATABASE_URL=  # Dejar vacío para SQLite
AZURE_ACCOUNT_NAME=
AZURE_ACCOUNT_KEY=
```

### Crear en nueva computadora

```bash
# Opción 1: Crear archivo manualmente
# Crea archivo: .env (en la carpeta principal)

# Opción 2: Desde terminal
echo "DEBUG=1" > .env
echo "SECRET_KEY=dev-secret-key-123" >> .env
echo "DATABASE_URL=" >> .env
```

---

## 📋 Checklist Final

Después de instalar, verifica:

```
INSTALACIÓN COMPLETA
├─ [ ] Python 3.8+ instalado (python --version)
├─ [ ] requirements.txt existe (dir requirements.txt)
├─ [ ] Entorno virtual activado ((venv) en terminal)
├─ [ ] Paquetes instalados (pip list)
├─ [ ] BD migrada (python manage.py migrate)
├─ [ ] Admin creado (python manage.py createsuperuser)
├─ [ ] Servidor inicia (python manage.py runserver)
├─ [ ] Página funciona (http://127.0.0.1:8000/)
├─ [ ] Admin accesible (http://127.0.0.1:8000/admin/)
└─ [ ] ¡Todo funciona! ✅
```

---

## 🎯 Resumen en Comandos

```bash
# TODO en orden:

# 1. Copiar carpeta a nueva computadora

# 2. Navegara la carpeta
cd "C:\ruta\al\proyecto\django"

# 3. Crear entorno virtual
python -m venv venv

# 4. Activar entorno
venv\Scripts\activate  # Windows
source venv/bin/activate  # Linux/Mac

# 5. Instalar paquetes
pip install -r requirements.txt

# 6. Migrar BD
python manage.py migrate

# 7. Crear admin (opcional)
python manage.py createsuperuser

# 8. Iniciar servidor
python manage.py runserver

# 9. Acceder en navegador
http://127.0.0.1:8000/
```

---

## 📞 Resumen Rápido

| Pregunta | Respuesta |
|----------|-----------|
| **¿Qué es requirements.txt?** | Archivo con lista de paquetes necesarios |
| **¿Debo copiar db.sqlite3?** | ❌ No, se crea con migrate |
| **¿Debo copiar venv/?** | ❌ No, se crea nuevo |
| **¿Cómo instalo paquetes?** | `pip install -r requirements.txt` |
| **¿Necesito internet?** | ✅ Sí, para descargar paquetes |
| **¿Cuánto tiempo toma?** | 2-5 minutos depende de conexión |
| **¿Se pueden cambiar paquetes?** | ✅ Sí, edita requirements.txt |

---

**¡Ahora puedes llevar tu proyecto a cualquier computadora! 🚀**

