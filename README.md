# 📊 Device Sales Visualization Dashboard

> **Repositorio Oficial:** [https://github.com/cgomezc88-svg/device-sales-visualization.git](https://github.com/cgomezc88-svg/device-sales-visualization.git)[cite: 1, 2]  
> **Asignatura:** Herramientas de Visualización de Datos Masivos  
> **Programa:** Maestría en Análisis y Visualización de Datos Masivos[cite: 7]  

---

## 🚀 Descripción General del Proyecto
El presente repositorio contiene el desarrollo de un **dashboard interactivo y adaptable** enfocado en el análisis comercial de dispositivos (*Smartphone A*, *Smartphone B*, *Tablet X*) durante los meses de enero y febrero. La solución está construida bajo una arquitectura ligera de **archivo único (`index.html`)**, ejecutándose de forma completamente autónoma en el navegador (*Client-Side Only*) sin requerir empaquetadores (Webpack/Vite) ni servidores web locales[cite: 5, 6].

El sistema integra dos visualizaciones complementarias sincronizadas:
1. **Visualización Superior (Google Charts Library):** Gráfico de columnas agrupadas para la comparación directa de ventas mensuales por categoría de producto[cite: 5, 6].
2. **Visualización Inferior (D3.js v7):** Gráfico multidimensional de burbujas interactivo que analiza simultáneamente **ventas (Eje X)**, **ingresos (Eje Y)**, **precio (Tamaño/Radio)**, **producto (Color)** y **mes temporal (Opacidad)**[cite: 5, 6].

---

## 🛠️ Metodología de Desarrollo: Spec-Driven Development (SDD)
El desarrollo de este sistema no se realizó mediante codificación improvisada, sino rigurosamente gobernado bajo la metodología **Spec-Driven Development (SDD)** en colaboración con Inteligencia Artificial[cite: 4, 5, 6]:
* **Gobernanza (`AGENTS.md` y `docs/constitution.md`):** Establecimiento de reglas innegociables (archivo único, CDNs oficiales, nomenclatura en inglés *camelCase* e interfaz en español, además de políticas de calidad de datos)[cite: 4, 5, 6].
* **Especificación Funcional (`specs/`):** Redacción de requisitos bajo la notación formal **EARS** (*Ubicuidad*, *Evento*, *Estado*) para tooltips, escalas y transiciones[cite: 5, 6].
* **Puntos de Control y Alineación:** Auditorías iterativas entre el humano y la IA para alinear nombres de archivos, paleta de colores fija y el filtrado por Rango Intercuartílico (IQR)[cite: 4, 5, 6].
* **Planificación y Tareas (`plan.md` y `tasks.md`):** Desglose secuencial de 10 tareas modulares ejecutadas y validadas paso a paso[cite: 4, 5, 10].

---

## 🔬 Explicación del Código mediante la Descomposición Forzada del MIT
Para comprender con rigor técnico la arquitectura y lógica del código fuente, se aplicó la metodología analítica de **Descomposición Forzada del MIT**, desglosando el sistema en 5 capas lógicas independientes[cite: 4, 5, 6]:

### Layer 1: El Contenedor y la Infraestructura DOM
* **Google Charts:** Aloja su contenedor neutral `<div id="google-chart-container"></div>` en la sección superior[cite: 5, 6].
* **D3.js v7:** Se estructura mediante un elemento vectorial `<svg viewBox="0 0 900 550" preserveAspectRatio="xMidYMid meet">` dentro de `<div id="chart">` para garantizar responsividad total[cite: 5, 6].

### Layer 2: El Lienzo Gráfico y Estructuración Espacial
* **Google Charts:** Gestiona de forma interna sus márgenes, ejes y leyendas[cite: 5, 6].
* **D3.js v7:** Aplica la convención de márgenes explícita de D3, creando grupos vectoriales aislados (`<g>`) para el eje X (`#x-axis-group`), eje Y (`#y-axis-group`), burbujas (`#bubbles-group`) y la leyenda superior (`#legend-group`), previniendo cualquier solapamiento[cite: 5, 6].

### Layer 3: Fuente de Datos, Normalización y Filtro IQR
* **Datos Embebidos y Simulación:** Los datos residen en la constante `initialDataset` dentro del script y se actualizan dinámicamente mediante la función `Math.random()` al presionar el botón «Actualizar»[cite: 5, 6].
* **Limpieza y Filtrado Estadístico:** Se procesa el Rango Intercuartílico ($IQR = Q3 - Q1$) para detectar y omitir valores atípicos, notificando al usuario mediante una ventana modal (`#outlier-modal`)[cite: 5, 6]. Los valores nulos se normalizan a `0`[cite: 5, 6].

### Layer 4: Escalas y Mapeo Multidimensional
* **Google Charts (Columnas):** Mapea el eje X categórico (*Mes*) y el eje Y cuantitativo (*Ventas*) con barras diferenciadas por producto[cite: 5, 6].
* **D3.js v7 (Burbujas):** Utiliza traductores matemáticos precisos[cite: 5, 6]:
  * **Eje X:** `d3.scaleLinear()` para ventas[cite: 5, 6].
  * **Eje Y:** `d3.scaleLinear()` para ingresos[cite: 5, 6].
  * **Radio ($r$):** `d3.scaleSqrt()` para el precio, asegurando un tamaño mínimo visible de $r \ge 8\text{px}$[cite: 5, 6].
  * **Color:** `d3.scaleOrdinal()` con paleta corporativa fija (*Smartphone A*: `#0098CD`, *Smartphone B*: `#DB8100`, *Tablet X*: `#00566C`)[cite: 5, 6].
  * **Opacidad Temporal:** Enero con transparencia al 60% (`opacity: 0.6`) y Febrero con opacidad completa (`1.0`)[cite: 5, 6].

### Layer 5: Motor de Renderizado, Transiciones e Interactividad
* **Google Charts:** Redibuja dinámicamente el diagrama de columnas[cite: 5, 6].
* **D3.js v7:** Implementa el patrón *Data-Join* (enter-update-exit) con transiciones animadas de 800ms (`.transition().duration(800)`)[cite: 5, 6]. Los eventos de cursor (`mouseover`, `mousemove`, `mouseout`) despliegan un *tooltip* contextual flotante con formato monetario y numérico en español[cite: 5, 6].

---

## 📦 Instrucciones de Ejecución Local
Al ser un desarrollo autónomo de archivo único, no requiere instalación de dependencias ni servidores locales:
1. Clona el repositorio en tu equipo:
   ```bash
   git clone [https://github.com/cgomezc88-svg/device-sales-visualization.git](https://github.com/cgomezc88-svg/device-sales-visualization.git)