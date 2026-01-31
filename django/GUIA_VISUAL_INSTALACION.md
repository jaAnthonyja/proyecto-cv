# 🚀 Guía Visual: Instalar en Otra Computadora

## 📊 Diagrama del Proceso

```
┌──────────────────────────────────────────────────────────────────┐
│         LLEVAR PROYECTO A OTRA COMPUTADORA - PROCESO             │
└──────────────────────────────────────────────────────────────────┘

COMPUTADORA 1 (Tu PC Actual)          COMPUTADORA 2 (Otra PC)
│                                     │
├─ Tu proyecto funcionando            ├─ Python instalado
│  ├─ django/                         │
│  ├─ cv/                             │
│  ├─ requirements.txt ◄──────────────┤─ COPIA ESTO
│  └─ ... (más archivos)              │
│                                     │
└─ Punto de inicio                    └─ Punto de destino


PASOS DE INSTALACIÓN:

PASO 1: COPIAR
┌──────────────────────────────────────────────────────────┐
│  Copia carpeta "django" completa                         │
│  Método: USB, Email, Google Drive, o GitHub              │
│  ├─ ✅ Copiar: cv/, django_portfolio/, manage.py, etc.  │
│  ├─ ✅ Copiar: requirements.txt (IMPORTANTE)             │
│  └─ ❌ NO copiar: db.sqlite3, venv/, __pycache__        │
└──────────────────────────────────────────────────────────┘
              │
              ▼
PASO 2: ABRIR TERMINAL
┌──────────────────────────────────────────────────────────┐
│  En nueva computadora:                                   │
│  cd C:\Users\TuUsuario\Desktop\django                   │
│  dir requirements.txt  ✓ (debe existir)                 │
└──────────────────────────────────────────────────────────┘
              │
              ▼
PASO 3: CREAR ENTORNO VIRTUAL
┌──────────────────────────────────────────────────────────┐
│  python -m venv venv                                     │
│  venv\Scripts\activate  (Windows)                        │
│  source venv/bin/activate  (Linux/Mac)                  │
│                                                          │
│  Resultado: (venv) aparece en terminal ✓                │
└──────────────────────────────────────────────────────────┘
              │
              ▼
PASO 4: INSTALAR PAQUETES
┌──────────────────────────────────────────────────────────┐
│  pip install -r requirements.txt                         │
│                                                          │
│  Instalará:                                             │
│  ├─ Django (framework web)                              │
│  ├─ Pillow (imágenes)                                   │
│  ├─ ReportLab (PDF)                                     │
│  ├─ ... (9 paquetes más)                                │
│                                                          │
│  Espera 2-5 minutos... ⏳                                │
└──────────────────────────────────────────────────────────┘
              │
              ▼
PASO 5: CREAR BASE DE DATOS
┌──────────────────────────────────────────────────────────┐
│  python manage.py migrate                                │
│                                                          │
│  Crea: db.sqlite3 (automáticamente)                     │
│  Crea: Todas las tablas de datos                        │
└──────────────────────────────────────────────────────────┘
              │
              ▼
PASO 6: CREAR USUARIO ADMIN (OPCIONAL)
┌──────────────────────────────────────────────────────────┐
│  python manage.py createsuperuser                        │
│                                                          │
│  Ingresa:                                               │
│  ├─ Username: admin                                     │
│  ├─ Email: admin@example.com                            │
│  └─ Password: tu_contraseña                             │
└──────────────────────────────────────────────────────────┘
              │
              ▼
PASO 7: INICIAR SERVIDOR
┌──────────────────────────────────────────────────────────┐
│  python manage.py runserver                              │
│                                                          │
│  Verás: "Starting development server at                 │
│         http://127.0.0.1:8000/"                        │
└──────────────────────────────────────────────────────────┘
              │
              ▼
       ✅ ¡LISTO! 🎉
       
Abre navegador y accede:
├─ http://127.0.0.1:8000/        (Tu sitio)
└─ http://127.0.0.1:8000/admin   (Panel administrador)
```

---

## 📦 ¿Qué es requirements.txt?

```
requirements.txt = Archivo de texto con LISTA de DEPENDENCIAS

┌─────────────────────────────────────┐
│        CONTENIDO DEL ARCHIVO        │
├─────────────────────────────────────┤
│ Django>=4.2                         │
│ gunicorn                            │
│ psycopg2-binary                     │
│ dj-database-url                     │
│ python-dotenv                       │
│ whitenoise                          │
│ django-storages[azure]              │
│ azure-storage-blob                  │
│ Pillow                              │
│ PyPDF2                              │
│ requests                            │
│ reportlab                           │
└─────────────────────────────────────┘

Cada línea = Un paquete a instalar

pip install -r requirements.txt
        ↓        ↓
    Comando    Archivo
    (Instalar) (Lista de paquetes)

Resultado: Todos los paquetes instalados ✅
```

---

## 🔄 Flujo Completo en Un Diagrama

```
MI COMPUTADORA                  → COPIA PROYECTO →    OTRA COMPUTADORA
┌──────────────────┐              (USB/Cloud)        ┌──────────────────┐
│ Proyecto activo  │                                 │ Python instalado │
│ ├─ cv/           │                 ✈️              │ ├─ Python.exe    │
│ ├─ manage.py     │                                 │ └─ pip            │
│ ├─ requirements. │  ╔═══════════════════════╗      │                  │
│ │   txt ◄────────┼─→║ requirements.txt     ║ ───→├─ requirements.txt│
│ └─ ...           │  ║ (IMPORTANTE)         ║      │ ├─ cv/           │
└──────────────────┘  ╚═══════════════════════╝      │ ├─ manage.py     │
                                                     │ └─ ...           │
                                                     └──────────────────┘
                                                            │
                                                            ▼
                                                     TERMINAL 1️⃣
                                                     pip install -r 
                                                     requirements.txt
                                                            │
                                                            ▼
                                                     TERMINAL 2️⃣
                                                     python manage.py
                                                     migrate
                                                            │
                                                            ▼
                                                     TERMINAL 3️⃣
                                                     python manage.py
                                                     runserver
                                                            │
                                                            ▼
                                                     ✅ FUNCIONA IGUAL
                                                     QUE EN ORIGINAL
```

---

## 📋 Comparativa: Qué Copiar y Qué No

```
┌─────────────────────────────────────────────────────────────────┐
│          ARCHIVO/CARPETA    │  COPIAR?  │    POR QUÉ?          │
├─────────────────────────────────────────────────────────────────┤
│ cv/                         │     ✅    │ Contiene el código    │
│ django_portfolio/           │     ✅    │ Configuración Django  │
│ manage.py                   │     ✅    │ Comando principal     │
│ requirements.txt            │     ✅    │ LISTA DE PAQUETES     │
│ .env                        │    ⚠️    │ Crear uno nuevo       │
│                             │          │                       │
│ db.sqlite3                  │     ❌    │ Se crea con migrate   │
│ venv/                       │     ❌    │ Se crea nuevo         │
│ __pycache__/                │     ❌    │ Archivos compilados   │
│ *.pyc                       │     ❌    │ Archivos compilados   │
│ staticfiles/                │     ❌    │ Se crea con collect   │
│ media/                      │    ⚠️    │ Solo si hay contenido │
└─────────────────────────────────────────────────────────────────┘
```

---

## ⚡ Los 7 Comandos Esenciales

```
1️⃣  NAVEGAR A CARPETA
    cd C:\Users\TuUsuario\Desktop\django
    
2️⃣  CREAR ENTORNO VIRTUAL
    python -m venv venv
    
3️⃣  ACTIVAR ENTORNO VIRTUAL
    venv\Scripts\activate  (Windows)
    source venv/bin/activate  (Linux)
    
    Resultado: (venv) en terminal ✓
    
4️⃣  INSTALAR PAQUETES (LA MÁS IMPORTANTE)
    pip install -r requirements.txt
    
    Espera hasta ver:
    "Successfully installed Django-4.2.x gunicorn..."
    
5️⃣  CREAR BASE DE DATOS
    python manage.py migrate
    
6️⃣  CREAR USUARIO ADMIN
    python manage.py createsuperuser
    
7️⃣  INICIAR SERVIDOR
    python manage.py runserver
    
    Luego abre navegador:
    http://127.0.0.1:8000/
```

---

## 🎨 Lo Que Instalas con requirements.txt

```
pip install -r requirements.txt

Instala 12 Paquetes:

1. Django 4.2          ← Framework web (lo más importante)
2. gunicorn            ← Servidor para producción
3. psycopg2-binary     ← Conectar a PostgreSQL
4. dj-database-url     ← Leer URL de BD
5. python-dotenv       ← Cargar variables .env
6. whitenoise          ← Servir CSS/JS
7. django-storages     ← Guardar en nube
8. azure-storage-blob  ← Azure Cloud
9. Pillow              ← Procesar imágenes
10. PyPDF2             ← Manipular PDFs
11. requests           ← Peticiones HTTP
12. reportlab          ← Generar PDFs

TOTAL: ~50MB de paquetes
TIEMPO: 2-5 minutos
```

---

## 🔍 Verificación Paso a Paso

```
┌─ ¿PYTHON INSTALADO?
│  Command: python --version
│  Resultado: Python 3.8+ ✅
│
├─ ¿ESTOY EN CARPETA CORRECTA?
│  Command: dir requirements.txt
│  Resultado: requirements.txt (archivo existe) ✅
│
├─ ¿ENTORNO VIRTUAL ACTIVO?
│  Señal: (venv) aparece en terminal
│  Command: (venv) C:\Users\... > ✅
│
├─ ¿PAQUETES INSTALADOS?
│  Command: pip list
│  Resultado: Django 4.2, gunicorn, ... ✅
│
├─ ¿BASE DE DATOS CREADA?
│  Command: dir db.sqlite3
│  Resultado: db.sqlite3 (archivo existe) ✅
│
├─ ¿SERVIDOR FUNCIONA?
│  Command: python manage.py runserver
│  Resultado: "Starting dev server at..." ✅
│
└─ ¿SITIO ACCESIBLE?
   Abre navegador: http://127.0.0.1:8000/ ✅
   Deberías ver tu portafolio
```

---

## 💾 Estructura de Carpetas Después

```
Después de seguir todos los pasos:

django/  (Tu carpeta copiada)
├── cv/
│   ├── migrations/
│   ├── templates/
│   ├── static/
│   ├── models.py
│   ├── views.py
│   └── ... (más archivos)
│
├── django_portfolio/
│   ├── settings.py
│   ├── urls.py
│   └── ...
│
├── manage.py
├── requirements.txt  ← NECESARIO PARA INSTALAR
├── .env  ← CREAR NUEVO (IMPORTANTE)
│
├── venv/  ← SE CREA CON "python -m venv venv"
│   ├── Scripts/
│   ├── Lib/
│   └── ...
│
├── db.sqlite3  ← SE CREA CON "python manage.py migrate"
│
└── media/  ← PARA UPLOADS (fotos, etc)

✅ Listo para usar
```

---

## 🚨 Problemas Comunes al Instalar

```
PROBLEMA 1: "Python no se reconoce"
└─→ SOLUCIÓN: Desinstala e reinstala Python
   └─ Marca "Add Python to PATH" en instalador

PROBLEMA 2: "pip no se reconoce"
└─→ SOLUCIÓN: python -m pip install -r requirements.txt

PROBLEMA 3: "Error al instalar paquetes"
└─→ SOLUCIÓN: Actualiza pip
   python -m pip install --upgrade pip

PROBLEMA 4: "Módulo no encontrado después de instalar"
└─→ SOLUCIÓN: Activa el entorno virtual
   venv\Scripts\activate  (Windows)
   source venv/bin/activate  (Linux)

PROBLEMA 5: "requirements.txt no se encuentra"
└─→ SOLUCIÓN: Verifica que estés en carpeta correcta
   dir requirements.txt  (debe existir)
```

---

## 🎯 Resumen en 30 Segundos

```
OBJETIVO: Instalar proyecto en otra computadora

RESPUESTA RÁPIDA:

1. Copia carpeta del proyecto
2. Abre terminal en esa carpeta
3. Crea entorno: python -m venv venv
4. Activa entorno: venv\Scripts\activate
5. Instala paquetes: pip install -r requirements.txt
6. Crea BD: python manage.py migrate
7. Inicia servidor: python manage.py runserver
8. Abre navegador: http://127.0.0.1:8000/

¡Listo! 🎉
```

---

## 📞 Números a Recordar

| Concepto | Valor |
|----------|-------|
| Paquetes en requirements.txt | 12 |
| Tamaño de descargas | ~50MB |
| Tiempo de instalación | 2-5 minutos |
| Espacio en disco necesario | ~200MB |
| Versión mínima Python | 3.8 |
| Versión Django | 4.2+ |

---

**¡Ahora puedes compartir tu proyecto con cualquiera! 🚀**

