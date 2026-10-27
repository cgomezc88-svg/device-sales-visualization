# Plan de Arquitectura e Implementación: device-sales-visualization

## 1. Estructura del DOM HTML (`index.html`)

La aplicación residirá en un único archivo `index.html` en la raíz del proyecto. Para permitir la ejecución directa mediante `file://` (sin necesidad de servidor web HTTP local), los datos iniciales, estilos CSS y lógica JavaScript se encuentran completamente embebidos.

La disposición maquetada establece el gráfico de barras de Google Charts en la parte superior y el gráfico multidimensional de burbujas de D3.js v7 en la parte inferior dentro de la misma página, garantizando espacio suficiente y holgura visual para una lectura cómoda sin superposiciones de elementos.

```html
<!DOCTYPE html>
<html lang="es">
<head>
  <meta charset="UTF-8">
  <meta name="viewport" content="width=device-width, initial-scale=1.0">
  <title>Dashboard de Ventas de Dispositivos</title>
  <style>
    /* Estilos CSS embebidos (Layout responsive, colores, leyendas superiores, tooltip, modales) */
    :root {
      --color-smartphone-a: #2B78E4;
      --color-smartphone-b: #E65100;
      --color-tablet-x: #27AE60;
      --bg-color: #f4f6f9;
      --text-color: #333333;
      --card-bg: #ffffff;
    }

    body {
      font-family: Arial, sans-serif;
      background-color: var(--bg-color);
      color: var(--text-color);
      margin: 20px;
    }

    .dashboard-container {
      display: flex;
      flex-direction: column;
      gap: 30px;
      max-width: 1000px;
      margin: 0 auto;
    }

    .chart-card {
      background: var(--card-bg);
      padding: 20px;
      border-radius: 8px;
      box-shadow: 0 2px 8px rgba(0,0,0,0.1);
    }

    .chart-description {
      font-size: 0.9em;
      color: #555555;
      margin-bottom: 15px;
      font-weight: bold;
    }

    #tooltip {
      position: absolute;
      padding: 8px 12px;
      background: rgba(255, 255, 255, 0.95);
      color: #000000;
      border: 1px solid #ccc;
      border-radius: 4px;
      pointer-events: none;
      font-size: 12px;
      box-shadow: 0 2px 5px rgba(0,0,0,0.2);
      z-index: 1000;
    }

    .hidden {
      display: none !important;
    }
  </style>

  <!-- CDNs Oficiales -->
  <script src="[https://www.gstatic.com/charts/loader.js](https://www.gstatic.com/charts/loader.js)"></script>
  <script src="[https://cdnjs.cloudflare.com/ajax/libs/d3/7.9.0/d3.min.js](https://cdnjs.cloudflare.com/ajax/libs/d3/7.9.0/d3.min.js)"></script>
</head>
<body>
  <header>
    <h1>Visualización Comercial de Dispositivos</h1>
  </header>

  <!-- Panel de Controles Globales -->
  <section id="controls" aria-label="Controles del Dashboard">
    <button id="update-btn">Actualizar</button>
    <div id="loading-indicator" class="hidden" role="status">Cargando datos...</div>
  </section>

  <!-- Mensaje o Modal para Outliers / Errores de Calidad -->
  <dialog id="outlier-modal">
    <div class="modal-content">
      <h3>Notificación de Calidad de Datos</h3>
      <p id="outlier-message"></p>
      <button id="close-modal-btn">Cerrar</button>
    </div>
  </dialog>

  <main class="dashboard-container">
    <!-- Visualización 1 (Superior): Google Charts (Barras Agrupadas / Columnas) -->
    <section class="chart-card">
      <h2>Ventas por mes</h2>
      <!-- Contenedor del gráfico de barras de Google Charts -->
      <div id="google-chart-container" style="width: 100%; height: 420px;"></div>
    </section>

    <!-- Visualización 2 (Inferior): D3.js v7 (Gráfico de Burbujas Multidimensional) -->
    <section class="chart-card">
      <h2>Análisis Multidimensional de Productos (D3.js)</h2>
      <!-- Texto descriptivo explicativo requerido para lectura de dimensiones -->
      <p class="chart-description">
        Eje x = Ventas, Eje y = Ingresos, Tamaño = Precio, Izquierda = enero, Derecha = febrero
      </p>
      <div id="chart">
        <svg viewBox="0 0 900 550" preserveAspectRatio="xMidYMid meet">
          <g id="scatter-plot-group">
            <g id="legend-group" transform="translate(0, 10)"></g>
            <g id="x-axis-group"></g>
            <g id="y-axis-group"></g>
            <g id="bubbles-group"></g>
          </g>
        </svg>
      </div>
    </section>
  </main>

  <!-- Tooltip Contextual de D3.js (Posición absoluta) -->
  <div id="tooltip" class="hidden" aria-hidden="true"></div>

  <script>
    // Lógica en JavaScript Vanilla (Dataset interno, Google Charts, D3.js v7, Math.random() y Eventos)
  </script>
</body>
</html>

```

---

## 2. Esquema del Conjunto de Datos JSON (Embebido en `index.html`)

Para garantizar el funcionamiento autónomo sin necesidad de servidor web HTTP (`file://`), los datos residen directamente dentro del archivo `index.html` bajo el esquema de una constante JS interna:

```javascript
const initialDataset = [
  { "product": "Smartphone A", "month": "Enero", "sales": 120, "revenue": 24000, "price": 200 },
  { "product": "Smartphone B", "month": "Enero", "sales": 85, "revenue": 25500, "price": 300 },
  { "product": "Tablet X", "month": "Enero", "sales": 60, "revenue": 18000, "price": 400 },
  { "product": "Smartphone A", "month": "Febrero", "sales": 150, "revenue": 30000, "price": 200 },
  { "product": "Smartphone B", "month": "Febrero", "sales": 90, "revenue": 27000, "price": 300 },
  { "product": "Tablet X", "month": "Febrero", "sales": 70, "revenue": 21000, "price": 400 }
];

```

### Reglas de Simulación Dinámica y Limpieza de Datos:

1. **Simulación Dinámica (`Math.random()`):** Al presionar `#update-btn`, una función JavaScript recomputará aleatoriamente las variables cuantitativas (`sales`, `revenue`, `price`) dentro de un rango realista y re-renderizará ambas gráficas simultáneamente.
2. **Nulos / Undefined:** Se normalizan a `0` antes del procesamiento.
3. **Outliers (Filtro IQR):** Se evalúa el Rango Intercuartílico $[Q1 - 1.5 \times IQR, Q3 + 1.5 \times IQR]$. Los registros fuera de este rango se omiten en D3.js y despliegan la notificación informativa en `#outlier-modal`.

---

## 3. Selección de Escalas, Ejes y Mapeo en D3.js v7

Para representar el gráfico multidimensional de burbujas en `#chart`:

### Mapeo Visual y Paleta Fija por Producto

* **Smartphone A:** Azul (`#2B78E4` / `#0098CD`)
* **Smartphone B:** Naranja (`#E65100` / `#DB8100`)
* **Tablet X:** Verde (`#27AE60` / `#2EC4B6`)
* **Transparencia por Mes:**
* *Enero:* Opacidad del **60%** (`opacity: 0.6`).
* *Febrero:* Opacidad completa (`opacity: 1.0`).



### Escalas de D3.js

* **Eje X (Ventas - Continua):** `d3.scaleLinear()`
* **Dominio:** `[0, d3.max(data, d => d.sales) * 1.15]` (ajustado dinámicamente).
* **Rango:** `[margin.left, width - margin.right]`


* **Eje Y (Ingresos - Continua):** `d3.scaleLinear()`
* **Dominio:** `[0, d3.max(data, d => d.revenue) * 1.15]`
* **Rango:** `[height - margin.bottom, margin.top + legendHeight]`


* **Radio de la Burbuja (Precio - Continua):** `d3.scaleSqrt()`
* **Dominio:** `[d3.min(data, d => d.price), d3.max(data, d => d.price)]`
* **Rango:** `[8, 25]` (garantizando $r \ge 8\text{px}$).


* **Color Categórico (Producto - Fijo):** `d3.scaleOrdinal()`
* **Dominio:** `["Smartphone A", "Smartphone B", "Tablet X"]`
* **Rango:** `["#2B78E4", "#E65100", "#27AE60"]`



### Ejes y Leyenda

* **Eje X:** `d3.axisBottom(xScale).ticks(6).tickFormat(d3.format(",d"))`
* **Eje Y:** `d3.axisLeft(yScale).ticks(6).tickFormat(d => "$" + d3.format(",d")(d))`
* **Grupo de Leyenda Superior (`#legend-group`):** Renderizado en la parte superior del SVG con espacio amplio para evitar colisiones con los ejes o las burbujas.

---

## 4. Estrategia de Animaciones y Transiciones de D3.js

Para dar cumplimiento al patrón **Enter-Update-Exit** y garantizar sincronización dinámica tras el clic en `#update-btn`:

1. **Duración y Easing:** `.transition().duration(800).ease(d3.easeCubicOut)`
2. **Ciclo de Vida de Selecciones:**
* **Enter:** Nuevas burbujas se instancian con radio `r = 0` y opacidad `0`, transicionando suavemente a sus coordenadas `(cx, cy)` y radio proyectado, aplicando opacidad `0.6` para *Enero* y `1.0` para *Febrero*.
* **Update:** Burbujas existentes transicionan simultáneamente actualizando posiciones `cx`, `cy` y radio `r` conforme al recálculo de datos por `Math.random()`.
* **Exit:** Elementos removidos reducen su radio a `0` y opacidad a `0` con `.transition().duration(400).remove()`.
* **Ejes:** `#x-axis-group` e `#y-axis-group` ajustan sus escalas de forma fluida invocando `.transition().duration(800).call(...)`.
