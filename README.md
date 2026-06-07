# tp8# TP8 — Sistema de Fixtures: Estructuración Semántica Avanzada
**Laboratorio de Programación · 6° G · 2026**

---

## Descripción del proyecto

Este trabajo práctico extiende el proyecto web de los TPs anteriores incorporando semántica HTML5 avanzada y un sistema multi-página completo. Se desarrolló un **Sistema de Gestión de Fixtures** para un torneo escolar, compuesto por cuatro secciones navegables: Panel de Control, Cargar Partido, Ver Fixture y Registro de Participante.

El objetivo fue reemplazar el uso genérico de `<div>` por etiquetas HTML5 con significado semántico real (`<header>`, `<nav>`, `<main>`, `<section>`, `<article>`, `<aside>`, `<footer>`), y lograr que el sistema funcione de forma completamente interactiva: los partidos cargados en el formulario aparecen en la tabla del fixture en tiempo real.

---

## Archivos entregados

| Archivo | Descripción |
|---|---|
| `tp08-fixtures.html` | Sistema completo (4 secciones en un único archivo HTML) |
| `estilos.css` | Hoja de estilos compartida TP6 + TP7 + nuevos estilos TP8 |
| `README_TP8.md` | Este informe |

> El sistema se implementó como una **Single Page Application (SPA)**: las cuatro "páginas" conviven en un único archivo HTML y se muestran/ocultan con JavaScript, simulando la navegación multi-archivo sin necesidad de un servidor.

---

## Semántica HTML5 aplicada

Este es el requerimiento central del TP8. Cada etiqueta tiene un propósito semántico concreto y está presente en las cuatro secciones del sistema.

### `<header>`
Encabezado principal del sitio. Contiene el título del sistema y el subtítulo institucional. Idéntico en las cuatro páginas para dar coherencia visual y estructural.

```html
<header>
  <h1>Sistema de Fixtures</h1>
  <p>Laboratorio de Programación · 6° G · 2026</p>
</header>
```

### `<nav>`
Barra de navegación global que conecta las cuatro secciones. Usa `text-transform: uppercase` y `cursor: pointer` en CSS (requerimiento heredado del TP7). El link de la sección activa recibe la clase `.activo` que lo resalta en dorado con borde inferior.

```html
<nav id="nav-principal">
  <a onclick="irA('dashboard')" class="activo">🏠 Panel de Control</a>
  <a onclick="irA('alta')">➕ Cargar Partido</a>
  <a onclick="irA('fixture')">📅 Ver Fixture</a>
  <a onclick="irA('registro')">📝 Registro</a>
</nav>
```

### `<main>`
Área de contenido central de cada sección. Todo el contenido visible de cada "página" vive dentro de su propio `<main>`, separando claramente la estructura del sitio del contenido navegable.

### `<section>`
Agrupa contenido temáticamente relacionado dentro de cada `<main>`. En el Panel de Control hay tres `<section>`: estadísticas, novedades y accesos rápidos. En Ver Fixture, la tabla de partidos y el aside conviven dentro de una sección principal.

### `<article>`
Cada novedad del torneo en el Panel de Control está marcada como `<article>`: una unidad de contenido independiente y autocontenida que podría distribuirse por separado. Se usaron cuatro artículos: fixture confirmado, resultados de cuartos, actualización de reglamento y próximo partido.

```html
<article>
  <div class="art-meta">📅 03 de junio de 2026 · Administración</div>
  <h3>🗓️ Fixture de Semifinales Confirmado</h3>
  <p>Se confirman los cruces de la instancia de Semifinal...</p>
</article>
```

### `<aside>`
Panel lateral en la sección Ver Fixture con información complementaria: tabla de posiciones dinámica, reglamento del torneo y avisos institucionales. El `<aside>` no es el contenido principal de la página sino información de soporte que lo enriquece.

### `<footer>`
Pie de página idéntico en las cuatro secciones con datos del TP, la materia y el año.

---

## Estructura de las secciones

### Panel de Control
- Tarjetas de estadísticas que se actualizan en tiempo real: total de partidos cargados, jugados y pendientes.
- Cuatro `<article>` con novedades del torneo.
- Accesos directos a las otras secciones del sistema.

### Cargar Partido
Formulario completo con `method="GET"` y `autocomplete="off`.

| Campo | Tipo HTML | Propósito |
|---|---|---|
| Equipo Local | `<select>` | Selección del equipo local |
| Equipo Visitante | `<select>` | Selección del equipo visitante |
| Fecha | `type="date"` | Fecha del encuentro con calendario nativo |
| Hora | `type="time"` | Horario con selector nativo |
| Sede | `type="text"` | Cancha o estadio asignado |
| Árbitro | `type="text"` | Árbitro designado (opcional) |
| Instancia | `type="radio"` | Fase del torneo (mismo `name` = excluyentes) |
| Observaciones | `<textarea>` | Notas adicionales |
| Sistema | `type="hidden"` | Metadato interno invisible |

Al presionar **REGISTRAR ENCUENTRO**, el partido se guarda en un array en memoria, aparece una confirmación personalizada y las estadísticas del dashboard se actualizan. El botón **RESTABLECER FORMULARIO** (`type="reset"`) limpia todos los campos.

### Ver Fixture
- `<table>` con columnas: número, partido, fecha, hora, sede, árbitro, instancia y estado.
- Los partidos cargados desde el formulario aparecen en la tabla en tiempo real.
- `<aside>` con tabla de posiciones que lista automáticamente los equipos que tienen partidos registrados.
- Reglamento del torneo y avisos institucionales en el aside.

### Registro de Participante
Formulario heredado del TP7, integrado al sistema:
- `type="text"` para nombre y apellido
- `type="email"` con validación nativa
- `type="password"` que oculta los caracteres
- `type="number"` con `min="6"` y `max="99"`
- `type="radio"` para categoría de álbum (excluyentes)
- `type="checkbox"` para temáticas de interés (múltiple)
- `<select>` para sección del álbum
- `readonly` para el ID de sesión (valor `100`, no editable)
- `type="hidden"` con metadato del sistema
- `<textarea>` con `</textarea>` cerrado en la misma línea

---

## Validaciones implementadas

Las validaciones operan en dos niveles:

**HTML5 nativo** (sin JavaScript):
- `required` bloquea el envío si el campo está vacío.
- `type="email"` verifica el formato de correo.
- `type="date"` y `type="time"` aseguran formatos válidos.
- `min` / `max` en campos numéricos limitan el rango aceptado.

**JavaScript** (complementaria, muestra mensajes en pantalla sin recargar):
- Verifica que local y visitante no sean el mismo equipo.
- Verifica que la contraseña tenga al menos 8 caracteres.
- Muestra mensajes de error en rojo o de confirmación en verde según el resultado.
- El botón reset oculta los mensajes de confirmación/error.

---

## Funcionalidad JavaScript

| Función | Descripción |
|---|---|
| `irA(pagina)` | Muestra la sección pedida, oculta las demás y actualiza el nav activo |
| `renderTablaFixture()` | Genera las filas `<tr>` dinámicamente con los partidos del array |
| `renderTablaPos()` | Arma la tabla de posiciones contando apariciones de cada equipo |
| `actualizarStats()` | Actualiza los contadores del dashboard (total, jugados, pendientes) |
| Listener `submit` (alta) | Valida, guarda en array y muestra confirmación |
| Listener `submit` (registro) | Valida y muestra confirmación con nombre del usuario |

---

## Estilos CSS — TP8

Los nuevos estilos se agregaron al final de `estilos.css`, respetando las variables de color del TP6 (`--dorado`, `--azul`, `--gris`, etc.).

| Selector | Propiedad clave | Efecto |
|---|---|---|
| `header` | `linear-gradient` + `border-bottom` | Encabezado con degradado azul y borde dorado |
| `nav a` | `text-transform: uppercase` | Texto en mayúsculas en todos los links |
| `nav a:hover` | `border-bottom-color` | Subrayado dorado al pasar el cursor |
| `article:hover` | `transform: translateY(-3px)` | Leve elevación al pasar el cursor |
| `aside` | `grid-template-columns: 1fr 270px` | Layout de dos columnas con panel lateral |
| `.t-fix th` | `font-family: 'Bangers'` | Encabezados de tabla con tipografía de álbum |
| `.bf-g/c/s/f` | colores por fase | Badges de color diferenciado por instancia |
| `.btn-submit:hover` | `font-size` aumentado | Hereda comportamiento TP6/TP7 |

---

## Cómo ejecutar

1. Colocar `tp08-fixtures.html` y `estilos.css` en la misma carpeta.
2. Abrir `tp08-fixtures.html` en cualquier navegador moderno.
3. Navegar entre las secciones usando la barra superior.
4. Probar el flujo completo: Cargar Partido → Ver Fixture (los partidos aparecen en la tabla).
5. No requiere servidor ni instalación.

---

## Relación con TPs anteriores

| TP | Aporte al proyecto actual |
|---|---|
| TP6 | Variables CSS, paleta de colores, tipografías, estructura base con `<div>` |
| TP7 | Formularios con `<label for>`, `required`, `type="email/password/number"`, `readonly`, `hidden`, botones submit/reset, validación JS |
| TP8 | Reemplazo de `<div>` por semántica HTML5, multi-página, `<article>`, `<aside>`, `<table>` dinámica |

---

*Laboratorio de Programación · 6° G · 2026*
