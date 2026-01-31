# Diapositivas — Resumen general de los CSS en `cv/static/css`

---

# Diapositiva 1 — `cv_fix.css` (visión general)

- **Propósito general:** Establece el estilo principal del sitio CV: tipografías, paleta de colores, componentes principales (perfil, secciones, tarjetas, layout del CV clásico), y comportamientos responsivos.
- **Áreas cubiertas:** reset y base (`body`), contenedores (`.page-wrap`, `.home-shell`), layout de inicio (`.home-shell--grid`, `.home-profile`), tarjetas de secciones (`.home-sections`, `.hs-card`), panel lateral (`.home-right`, `.side-card`), componentes de CV clásico (`.cv-wrapper`, `.cv-left`, `.cv-right`), miniaturas y modales (certificados, PDF), y reglas para impresión.
- **Rol en la aplicación:** Es la hoja de estilos principal que “alimenta” la apariencia y disposición de la mayoría de las vistas del módulo `cv` — controla cómo se organizan y se ven los bloques de información del CV.

---

# Diapositiva 2 — `cv_fix.css` (detalles relevantes)

- **Selectores y patrones clave:** usa grid y flex para layouts, `.list_display`-style cards con sombras y transiciones para interacción visual. Clases específicas para secciones (ej.: `.hp-avatar`, `.hs-card`, `.side-chip`) facilitan modificaciones puntuales.
- **Responsive y accesibilidad:** incluye media queries (`@media (max-width: 900px)` etc.) que reconfiguran columnas a una sola columna, reducen paddings y adaptan imágenes; también contiene reglas `@media print` para ocultar controles web y ajustar tipografías para impresión.
- **Buenas prácticas y recomendaciones:** modularizar variables (colores/tamaños) en un archivo SASS/variables para facilitar temas; considerar uso de clases utilitarias para evitar duplicación; añadir focus styles para accesibilidad de teclado.

---

# Diapositiva 3 — `ui_nav.css` (visión general)

- **Propósito general:** Estilizar la barra de navegación superior (`.nav-top`) — color de fondo, comportamientos sticky, enlaces de navegación y botones de acción (imprimir, admin).
- **Áreas cubiertas:** contenedor sticky con degradado oscuro, marca (`.nav-brand`), grupos de enlaces (`.nav-links`), estilos de enlace con hover y variantes especializadas (`.nav-print`, `.nav-admin`).
- **Rol en la aplicación:** Proporciona la experiencia de navegación global y accesos rápidos (imprimir, administración), manteniéndose visible al hacer scroll gracias a `position: sticky`.

---

# Diapositiva 4 — `ui_nav.css` (detalles relevantes)

- **Detalles técnicos:** la barra usa un `linear-gradient` oscuro y sombra para contraste; los enlaces tienen padding y bordes redondeados para zonas táctiles cómodas; `z-index` alto evita que quede tapada por contenidos.
- **Interacción y estado:** hover ligero con fondo semitransparente mejora el feedback; variantes `.nav-print` y `.nav-admin` aplican bordes y fondo sutil para distinguir acciones importantes.
- **Recomendaciones:** añadir `aria-label` y roles en HTML, estilos para `:focus` visibles, y considerar colapsado responsivo (hamburger) si hay muchas entradas en `nav-links` para pantallas pequeñas.

---

_Archivo generado automáticamente: `CV_CSS_DIAPOSITIVAS.md`_
