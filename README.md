# Documento de Diseño — CV Web Personal

**Alumno:** Manuel Felipe Millán Peláez  
**Módulo:** Lenguaje de Marcas y Sistemas de Gestión de Información (1º DAM)  
**Centro:** CPR Daniel Castelao, Vigo · 2026

---

## 1. Esquema de páginas

El sitio está compuesto por **5 páginas HTML** interconectadas mediante un menú de navegación común:

| Archivo           | Título          | Contenido                                                                                                                |
| ----------------- | --------------- | ------------------------------------------------------------------------------------------------------------------------ |
| `index.html`      | Inicio / Perfil | Hero con foto, presentación personal, grid de acceso rápido a secciones e intereses.                                     |
| `experience.html` | Experiencia     | Historial laboral (2 tarjetas) y proyectos académicos (CV Web, Gestor de Boletines).                                     |
| `education.html`  | Formación       | Grid de 2 columnas: Formación Reglada (CFGS DAM, Bachillerato) y Complementaria (Autodidacta, CS50).                     |
| `skills.html`     | Competencias    | Grid de 3 columnas con barras de nivel, tecnologías, SS.OO. e habilidades interpersonales. Sección adicional de idiomas. |
| `contact.html`    | Contacto        | Grid de 2 columnas: datos de contacto (email, GitHub, localidad) + formulario.                                           |

---

## 2. Uso de Flexbox y CSS Grid

### Flexbox — alineación unidimensional

Se ha usado Flexbox para componentes de interfaz donde la distribución es en **una sola dimensión** (fila o columna):

- **`.hero-section`** (`index.html`): coloca la foto (`flex-shrink: 0`) y el bloque de texto (`flex: 1`) en fila, con `align-items: center`. En móvil cambia a `flex-direction: column`.
- **`.main-nav`** (todas): en móvil (≤768px) el grid de la nav se convierte en Flexbox con `justify-content: space-between` para separar logo y controles.
- **`.nav-menu`** (todas): alinea los enlaces de navegación en fila con `gap` responsivo.
- **`.nav-controls`** (todas): agrupa el toggle de tema, el selector de idioma y el botón hamburguesa en fila, alineados a la derecha (`justify-self: end`).
- **`.skill-bars`** (`skills.html`): apila las barras de nivel en columna (`flex-direction: column`) con `gap` uniforme entre ellas.
- **`.skill-bar`** (`skills.html`): alinea nombre, pista de progreso y porcentaje en fila con `align-items: center`. La pista usa `flex: 1` para ocupar el espacio disponible.
- **`.contact-info`** (`contact.html`): apila los ítems de contacto en columna con `gap`.
- **`.contact-form`** (`contact.html`): apila los grupos del formulario en columna.
- **`.footer-links`** (todas): alinea los enlaces del footer en fila, centrados con `flex-wrap` para móvil.
- **`.cv-entry`** (experiencia/formación): `flex-direction: column` con `flex: 1` en la descripción, para que todas las tarjetas de una fila tengan la misma altura.

### CSS Grid — estructuración bidimensional

Se ha reservado Grid para **cuadrículas de contenido** donde importa el control de filas y columnas a la vez:

- **`.main-nav`** (todas, escritorio): `grid-template-columns: minmax(0, 1fr) auto minmax(0, 1fr)` — 3 columnas simétricas que centran el menú exactamente en el medio independientemente del ancho del logo o los controles.
- **`.quicklinks-grid`** (`index.html`): `repeat(2, 1fr)` — cuadrícula 2×2 de acceso rápido a las secciones.
- **`.interests-grid`** (`index.html`): `repeat(auto-fit, minmax(180px, 1fr))` — tarjetas de intereses que se adaptan automáticamente al ancho disponible.
- **`.entries-grid`** (`experience.html`): `repeat(auto-fit, minmax(270px, 1fr))` — tarjetas de experiencia/proyectos en 2 columnas en escritorio, 1 en móvil.
- **`.edu-grid`** + **`.edu-col`** (`education.html`): `repeat(2, 1fr)` con **CSS Subgrid** (`grid-template-rows: subgrid`). Cada `.edu-col` hereda las filas del padre, logrando alineación cruzada entre las dos columnas (h2 con h2, tarjeta 1 con tarjeta 1, tarjeta 2 con tarjeta 2).
- **`.skills-grid`** (`skills.html`): `repeat(3, 1fr)` — 3 tarjetas de competencias técnicas; la última (habilidades interpersonales) ocupa `grid-column: 1 / -1` para extenderse al ancho completo.
- **`.lang-skills-grid`** (`skills.html`): `repeat(auto-fit, minmax(130px, 1fr))` — tarjetas de idiomas adaptables.
- **`.contact-layout`** (`contact.html`): `1fr 1.6fr` — datos de contacto a la izquierda, formulario (más ancho) a la derecha.

---

## 3. Lo más difícil y cómo lo resolví

Siendo honesto, Juan: lo que más me comió la cabeza fui yo mismo. Soy perfeccionista, cabezota y muy curioso — podía haber hecho algo mucho más simple y cumplir el enunciado igual, pero quería hacer algo chulo. Dicho esto, hubo retos técnicos reales.

**El tema y el selector de idioma sin JavaScript** fueron lo más elaborado. La primera implementación usaba el combinador `~` (hermano general): el checkbox estaba antes del `.nav-menu` en el DOM y `#theme-toggle:checked ~ .main-content` seleccionaba los elementos siguientes. Funcionaba en algunos casos, pero el combinador `~` solo alcanza hermanos posteriores en el mismo nivel, así que en cuanto un elemento quedaba anidado diferente o en otro contenedor, dejaba de funcionar. Tuve que restructurar el HTML varias veces para que los selectores llegaran donde necesitaba, y llegó un punto en que era insostenible.

Por suerte, encontré a **`:has()`**: en lugar de navegar hacia adelante desde el input, se consulta al `body` si _contiene_ un input marcado — `body:has(#theme-toggle:checked)` — y desde ahí se puede llegar a cualquier elemento de la página sin importar su posición en el DOM. Para el tema redefine las variables CSS del `:root`; para el idioma alterna `display` entre los `<span class="lang-XX">` de cada texto. Un cambio de enfoque que simplificó todo considerablemente.

**El footer flotante** fue otro clásico. En páginas con poco contenido el footer se quedaba a mitad de la pantalla en lugar de pegarse al fondo (algo que me producía mucho TOC). La solución es el patrón _sticky footer_ con Flexbox: `body` recibe `display: flex; flex-direction: column; min-height: 100dvh`, `.main-content` recibe `flex: 1` para crecer y ocupar todo el espacio sobrante, y `.main-footer` recibe `flex-shrink: 0` para no comprimirse nunca. Sin posicionamiento absoluto, sin trucos raros — pura lógica de flex.

**La alineación del grid de formación** fue el reto más técnico. El grid plano (h2 + artículos como hijos directos del grid padre) funcionaba en escritorio, pero al colapsar a una columna en móvil el orden resultante era: h2 Reglada → h2 Complementaria → CFGS → Autodidacta → Bachillerato → Cursos, que no tiene ningún sentido. Envolver cada columna en un `<div class="edu-col">` resolvió el orden en móvil, pero entonces las tarjetas entre columnas dejaron de alinearse en altura. La solución final fue **CSS Subgrid**: cada `.edu-col` usa `grid-row: span 3; grid-template-rows: subgrid`, heredando las filas del grid padre. Así el padre controla la altura de cada fila para ambas columnas a la vez, y la alineación es perfecta en escritorio sin romper el orden en móvil.
