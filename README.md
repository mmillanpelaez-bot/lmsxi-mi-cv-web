# **Documento de Diseño: Currículum Vitae Web**

**Alumno:** Manuel Felipe Millán Peláez  
**Módulo:** Lenguaje de Marcas (1º DAM)

## **1\. Esquema de Páginas**

El sitio web está compuesto por 5 páginas principales, enlazadas mediante un menú de navegación común estructurado semánticamente:

- **`index.html`(Inicio / Perfil):** Página principal con la presentación (Hero section), enlaces rápidos a otras secciones y un resumen de intereses.
- **`education.html`(Formación):** Detalla la formación académica reglada y complementaria (autodidacta, cursos).
- **`experience.html`(Experiencia):** Contiene el historial laboral y los proyectos desarrollados en el ámbito académico.
- **`skills.html` (Competencias):** Muestra de forma visual las habilidades técnicas (lenguajes, herramientas), competencias transversales (soft skills) y el nivel de idiomas.
- **`contact.html` (Contacto):** Integra un formulario funcional (vía `mailto:`) y enlaces a redes profesionales como GitHub.

## **2\. Uso de Flexbox y CSS Grid**

Para conseguir un diseño _Mobile First_ y completamente adaptativo, he combinado ambas tecnologías según la necesidad estructural:  
**Flexbox (Alineación unidimensional):** Lo he utilizado principalmente para componentes de interfaz y alineaciones internas.

- **Cabecera y Navegación (`.main-nav`, `.nav-controls`):** Para separar el logo a la izquierda y agrupar los botones de idioma/tema a la derecha usando `justify-content: space-between` y `align-items: center`.
- **Pie de página (`.footer-links`):** Para alinear horizontalmente los enlaces de contacto y centrar el contenido.
- **Sección Hero (`.hero`):** Para alinear la foto de perfil junto con la presentación vertical y horizontalmente.

**CSS Grid (Estructuración bidimensional):** Lo he reservado para las cuadrículas de contenido y maquetaciones a gran escala.

- **Tarjetas de Inicio (`.quicklinks-grid`):** Crea un mosaico de 2x2 en escritorio que colapsa a 1 columna en dispositivos móviles.
- **Habilidades (`.skills-grid`):** Distribuye los grupos de competencias en columnas asimétricas (`grid-template-columns: 2fr 1fr`), aprovechando mejor el espacio horizontal de la pantalla.
- **Formación (`.education-grid`):** Divide la sección en dos grandes bloques (Formación Reglada y Complementaria) que se apilan semánticamente en resoluciones pequeñas.

## **3\. Mayor dificultad y solución implementada**

El mayor reto fue **implementar un sistema de cambio de idioma y un modo oscuro/claro sin utilizar JavaScript**.  
Para solucionarlo, utilicé una técnica basada en el estado de etiquetas `<input>` ocultas (tipo _radio_ y _checkbox_) enlazadas a etiquetas `<label>` visibles. Aprovechando el selector relacional avanzado **`:has()`** en CSS sobre la etiqueta `<html>`, logré modificar las variables de color (Custom Properties) para el tema, y alternar la propiedad `display` de los distintos `<span>` de idiomas en tiempo real dependiendo del _input_ que el usuario marque.  
_(Nota técnica: Para el renderizado óptimo de la web se ha utilizado un SVG Sprite (`<use>`). Debido a las políticas de seguridad CORS de los navegadores actuales, se recomienda visualizar el proyecto mediante un servidor local como Live Server para garantizar la correcta carga de los iconos)._
