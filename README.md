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
