# Constitución del Proyecto: device-sales-visualization

## 1. Reglas Innegociables del Proyecto (Project Guiding Principles)

### A. Estructura de Archivo Único y Carga Autónoma (Client-Side Only)
* **Ubicación y Despliegue Autónomo:** La aplicación completa residirá exclusivamente en la raíz del proyecto dentro del archivo `index.html`. No requerirá de un servidor web local ni HTTP para su despliegue ni funcionamiento; debe ser ejecutable directamente al abrir el archivo en cualquier navegador.
* **Estilos y Scripts Embebidos:** Los estilos (CSS) deberán integrarse mediante la etiqueta `<style>` y la lógica de programación (JavaScript) mediante la etiqueta `<script>`.
* **Prohibición de Empaquetadores:** No se implementarán empaquetadores ni compiladores de código (Webpack, Vite, Parcel, Babel) ni entornos de servidor (Node.js/NPM).

### B. Gestión de Dependencias Externas:
* **Librería D3.js:** El único medio autorizado para cargar la librería **D3.js** es mediante su CDN oficial en versión v7 (`<script src="https://cdnjs.cloudflare.com/ajax/libs/d3/7.9.0/d3.min.js"></script>`).
* **Librería Google Charts:** De igual manera, las librerías secundarias (Google Charts) deberán cargarse vía CDN oficial (`https://www.gstatic.com/charts/loader.js`).
* **Limitante en Código:** El código de las librerías externas no deberá ser transcrito ni copiado localmente dentro de `index.html`.

### C. Estándar Idiomático y Convenciones de Código:
* **Nombres de Variables y Funciones:** Se escribirán estrictamente en inglés usando formato *camelCase* (ejemplo: `dataset`, `margin`, `renderScatterPlot`, `updateData`, `generateRandomData`).
* **Comentarios y Textos de la Interfaz:** Todos los comentarios descriptivos del código, títulos, etiquetas de ejes, descripciones explicativas, mensajes de la interfaz de usuario y herramientas emergentes (*tooltips*) deben estar redactados en **español**.

### D. Diseño Adaptable, Espacioso y Accesible:
* **Escalabilidad SVG:** Todas las visualizaciones basadas en SVG (D3.js) deben declararse obligatoriamente con los atributos `viewBox` y `preserveAspectRatio` para garantizar un comportamiento escalable e interactivo.
* **Maquetación y Holgura Visual:** Ambas visualizaciones contendrán espacio suficiente entre elementos, márgenes y ejes para garantizar un despliegue y lectura cómodos en una misma página vertical sin saturación ni superposiciones.
* **Accesibilidad Web:** Se deben aplicar principios de accesibilidad web (contraste de color, dimensiones táctiles accesibles y etiquetas semánticas).

---

## 2. Especificaciones Técnicas del Dashboard y Visualizaciones

### A. Gestión de Datos, Simulación y Eventos
* **Origen de Datos Embebido:** Los datos no se cargarán mediante peticiones a archivos externos (`.js` o `.json` externos), sino que estarán integrados directamente dentro del archivo `index.html` bajo el esquema de conjunto de datos JSON.
* **Actualización y Simulación Dinámica:** El botón interactivo «Actualizar» simulará valores métricos nuevos (ventas, ingresos, precio) en tiempo real mediante la función `Math.random()`. Al activarse, recomputará el dataset y actualizará ambas visualizaciones simultáneamente sin recargar la página.
* **Tratamiento de Datos e Integridad:** Si se detectan valores atípicos fuera del Rango Intercuartílico ($[Q1 - 1.5 \times IQR, Q3 + 1.5 \times IQR]$), se omitirán de la representación de D3.js notificando al usuario. Los valores nulos o no definidos se normalizarán a `0`.

### B. Especificaciones Visuales y Disposición Estricta

1. **Visualización Superior: Gráfico de Barras Agrupadas / Columnas**
   * **Disposición:** Se renderiza en primer lugar en la parte superior del dashboard con espacio y márgenes suficientes para una lectura holgada.
   * **Ejes:** 
     * **Eje X:** Mes (*Enero*, *Febrero*).
     * **Eje Y:** Unidades vendidas.
   * **Paleta de Colores por Producto:**
     * **Smartphone A:** Azul (`#0098CD` / `#2B78E4`).
     * **Smartphone B:** Naranja (`#DB8100` / `#E65100`).
     * **Tablet X:** Verde (`#00566C` / `#2EC4B6` / `#27AE60`).
   * **Identificación y Leyenda:** La identificación del producto por color debe colocarse en la **parte superior del gráfico**, garantizando márgenes adecuados para que **nunca se superponga ni al eje Y ni a las barras**.

2. **Visualización Inferior: Gráfico de Burbujas Multidimensional (D3.js v7)**
   * **Disposición:** Se renderiza directamente debajo del gráfico de barras con espacio suficiente para su lectura cómoda dentro de la misma página.
   * **Mapeo de Dimensiones y Mapeo Temporal:**
     * **Eje X:** Ventas.
     * **Eje Y:** Ingresos.
     * **Tamaño (Radio):** Precio (con radio mínimo $r \ge 8\text{px}$).
     * **Posición Temporal:** Izquierda = *Enero*, Derecha = *Febrero*.
   * **Transparencia por Mes:**
     * **Enero:** La burbuja del producto se renderizará con una transparencia/opacidad del **60%** (`opacity: 0.6`).
     * **Febrero:** Opacidad completa (`opacity: 1.0`).
   * **Leyenda y Texto Explicativo:**
     * Debe incluir una descripción en texto en la parte superior para facilitar la lectura:
       `Eje x = Ventas, Eje y = Ingresos, Tamaño = Precio, Izquierda = enero, Derecha = febrero`
     * Debe incluir la identificación de producto por color en la **parte superior del gráfico**, asegurando que **nunca se superponga ni a los ejes ni a las burbujas**.
     * **Interactividad:** Eventos `mouseover` / `mouseout` para desplegar un *tooltip* contextual con formato numérico en español.