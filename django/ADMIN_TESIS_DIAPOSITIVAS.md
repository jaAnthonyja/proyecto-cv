# Documentación: `cv/admin.py` — Estilo Tesis (Diapositivas)

---

# Diapositiva 1 — Portada

- **Título:** Documentación del archivo `cv/admin.py`
- **Autor:** (Nombre del estudiante)
- **Asignatura / Tesis:** Diseño e implementación de la interfaz administrativa del sistema CV
- **Fecha:** 30-01-2026

---

# Diapositiva 2 — Resumen Ejecutivo

- **Resumen:** Este documento analiza en detalle `cv/admin.py`, que configura el panel de administración de Django para los modelos del módulo `cv`. Se presenta: objetivos, metodología de diseño del admin, descripción de cada `ModelAdmin`, personalizaciones, implicaciones para la tesis y recomendaciones.

---

# Diapositiva 3 — Objetivos de la sección admin

- **Objetivo general:** Facilitar la gestión de contenidos (perfiles, cursos, experiencias, productos, reconocimientos y ventas) mediante una interfaz administrativa clara y usable.
- **Objetivos específicos:**
  - Registrar modelos relevantes en el admin de Django.
  - Configurar `list_display`, `list_editable`, `list_filter` y `search_fields` para eficiencia del gestor.
  - Personalizar títulos del sitio admin para una experiencia coherente.

---

# Diapositiva 4 — Metodología de análisis

- **Fuentes:** Código fuente `cv/admin.py`, `cv/models.py`, interfaces Django admin.
- **Método:** Revisión de código, mapeo de responsabilidades (cada clase `ModelAdmin` → comportamiento), y evaluación de usabilidad para tareas administrativas comunes.

---

# Diapositiva 5 — Estructura general del archivo

- **Imports:** `from django.contrib import admin` y modelos del módulo: `Datospersonales`, `Cursosrealizados`, `Experiencialaboral`, `Productosacademicos`, `Productoslaborales`, `Reconocimientos`, `Ventagarage`.
- **Personalización global del admin:** `admin.site.site_header`, `admin.site.site_title`, `admin.site.index_title`.
- **Registro por decorador:** Uso de `@admin.register(Model)` acompañado de la clase `ModelAdmin` correspondiente.

---

# Diapositiva 6 — Personalización global del panel admin

- `admin.site.site_header = "Panel de Administración"`
- `admin.site.site_title = "Panel Administrativo"`
- `admin.site.index_title = "Gestión del Sistema"`

- **Impacto:** Mejora la identificación del proyecto por parte de administradores y aportan profesionalismo al entorno de gestión.

---

# Diapositiva 7 — `DatospersonalesAdmin` (propósito)

- **Modelo objetivo:** `Datospersonales` (perfil principal del CV)
- **Atributos clave en admin:**
  - `list_display`: `idperfil, nombres, apellidos, perfilactivo, permitir_impresion`
  - `list_editable`: `perfilactivo, permitir_impresion` (edición rápida desde la lista)
  - `list_filter`: `perfilactivo, permitir_impresion`
  - `search_fields`: `nombres, apellidos, numerocedula`

---

# Diapositiva 8 — Análisis de `DatospersonalesAdmin`

- **Razonamiento:** El admin expone `permitir_impresion` para controlar generación de PDFs desde la interfaz pública; `perfilactivo` indica qué perfil mostrar.
- **Ventaja UX:** `list_editable` posibilita toggles rápidos sin entrar en la edición completa.
- **Consideraciones de tesis:** Discuta seguridad (quién puede activar `permitir_impresion`) y auditoría (registro de cambios si necesario).

---

# Diapositiva 9 — `CursosrealizadosAdmin` (y patrón común)

- **Campos visibles:** `nombrecurso, fechainicio, fechafin, totalhoras, perfil, activarparaqueseveaenfront, certificado_pdf, certificado_imagen`
- **Comportamiento repetido:** Uso de `list_editable` para `activarparaqueseveaenfront`; `list_filter` y `search_fields` para facilitar búsquedas.

---

# Diapositiva 10 — `ExperiencialaboralAdmin` y `ReconocimientosAdmin`

- **Experiencia laboral:** Campos relacionados a cargo, empresa y fechas; incluye certificados (PDF/imagen) y toggle `activarparaqueseveaenfront`.
- **Reconocimientos:** `tiporeconocimiento`, `fechareconocimiento`, `entidadpatrocinadora`; filtros por tipo y activación.

---

# Diapositiva 11 — `ProductosacademicosAdmin` y `ProductoslaboralesAdmin`

- **Productos académicos:** `nombreproducto`, `clasificador`, `imagenproducto`, certificados.
- **Productos laborales:** `nombreproducto`, `fechaproducto`, `imagenproducto`.
- **Notas de diseño:** Ambos usan `activarparaqueseveaenfront` para controlar visibilidad en front-end; `clasificador` actúa como tag/filter en admin.

---

# Diapositiva 12 — `VentagarageAdmin` (venta de artículos)

- **Campos:** `nombreproducto, estadoproducto, fecha, valordelbien, perfil, activarparaqueseveaenfront, foto_producto`
- **Uso:** Gestionar inventario de ventas dentro del CV; `estadoproducto` permite filtrar estado (disponible, vendido, etc.).

---

# Diapositiva 13 — Patrones repetidos y buenas prácticas

- Uso consistente de: `list_display`, `list_editable`, `list_filter`, `search_fields`.
- Beneficio: administración eficiente, mínima fricción para editar visibilidad y contenidos.
- Recomendación de tesis: incluir capturas de pantalla de admin y métricas de tiempo para tareas comunes.

---

# Diapositiva 14 — Consideraciones de seguridad y permisos

- Revisar permisos de acceso al admin (`is_staff`) y permisos por objeto cuando aplique.
- Para campos sensibles (ej. `permitir_impresion`) considerar restricciones: solo superusuarios o grupos concretos puedan modificarlos.

---

# Diapositiva 15 — Posibles extensiones (líneas de trabajo para la tesis)

- Añadir `Inlines` para mostrar relaciones directamente en `Datospersonales` (por ejemplo, cursos/experiencias embebidos).
- Implementar `actions` personalizados (ej. activar masivamente `activarparaqueseveaenfront`).
- Añadir `readonly_fields`, `fieldsets` y formularios personalizados para mejorar la edición.

---

# Diapositiva 16 — Ejemplo de `action` propuesto (diapositiva técnica)

- Título: "Action — Activar para front" — permitir seleccionar múltiples items y activar `activarparaqueseaenfront`.

```python
def activar_para_front(modeladmin, request, queryset):
    queryset.update(activarparaqueseveaenfront=True)
activar_para_front.short_description = "Activar items seleccionados para front"

class CursosrealizadosAdmin(admin.ModelAdmin):
    actions = [activar_para_front]

```

---

# Diapositiva 17 — Metodología de pruebas para admin (propuesta)

- Tests unitarios: verificar que `ModelAdmin` expone `list_display` esperado.
- Tests funcionales (Selenium/Playwright): flujos de edición rápida, filtros y búsqueda.
- Métrica: tiempo medio para activar un perfil y añadir un curso.

---

# Diapositiva 18 — Resultados esperados y evaluación

- Evaluar usabilidad con usuarios administrativos: tasa de completitud de tareas y satisfacción.
- Comparar versión sin personalizaciones vs con `list_editable` y `actions`.

---

# Diapositiva 19 — Conclusiones y recomendaciones

- `cv/admin.py` ofrece una configuración coherente y repetible ideal para la gestión de contenidos del CV.
- Recomendaciones:
  - Implementar permisos más finos para campos críticos.
  - Agregar `Inlines` y `actions` para eficientar la gestión masiva.
  - Documentar flujos administrativos y añadir capturas en la tesis.

---

# Diapositiva 20 — Referencias y anexos

- Código fuente: `cv/admin.py`, `cv/models.py`.
- Documentación Django Admin: https://docs.djangoproject.com/en/stable/ref/contrib/admin/
- Anexo A: fragmentos de código relevantes (incluir en apéndice de la tesis).

---

# Apéndice A — Fragmentos clave (resumen técnico)

- Registro global de títulos del admin:

```python
admin.site.site_header = "Panel de Administración"
admin.site.site_title = "Panel Administrativo"
admin.site.index_title = "Gestión del Sistema"
```

- Ejemplo de `ModelAdmin` típico (estructura):

```python
@admin.register(Model)
class ModelAdmin(admin.ModelAdmin):
    list_display = (...)
    list_editable = (...)
    list_filter = (...)
    search_fields = (...)
```

---

# Fin

_Archivo generado automáticamente: `ADMIN_TESIS_DIAPOSITIVAS.md`_
