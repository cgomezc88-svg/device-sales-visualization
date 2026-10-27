# Listado de Tareas de Implementación (tasks.md)

- [ ] **T1: Estructura base HTML5 con importación CDN de D3 v7 y Google Charts**
  - **Descripción:** Crear la estructura base del documento HTML5 autónomo en `index.html` e importar las librerías externas necesarias desde sus CDN oficiales (D3.js v7.9.0 y Google Charts Loader) para permitir su ejecución directa mediante el protocolo `file://` sin requerir servidor HTTP[cite: 5, 6].
  - **Requisito Funcional (RF) cubierto:** `[RF-UBI-01]`, `[RF-UBI-02]`, Criterio de Aceptación 1[cite: 5, 6].

- [ ] **T2: Estilos CSS y maquetación vertical responsiva**
  - **Descripción:** Definir las reglas CSS embebidas en `<style>` para estructurar el layout vertical de las visualizaciones (gráfico de barras superior y gráfico de burbujas inferior) dentro de `.dashboard-container`, maquetar la tarjeta `.chart-card`, el panel `#controls`, la etiqueta descriptiva de dimensiones `.chart-description`, el elemento flotante `#tooltip` y la ventana modal `#outlier-modal`[cite: 5, 6].
  - **Requisito Funcional (RF) cubierto:** `[RF-UBI-03]`, `[RF-EST-01]`, `[RF-EST-02]`, Casos Límite 5[cite: 5, 6].

- [ ] **T3: Estructura del dataset JSON embebido y función de simulación dinámica (`Math.random()`)**
  - **Descripción:** Definir la constante JavaScript `initialDataset` dentro de `<script>` con el esquema JSON de productos (*Smartphone A*, *Smartphone B*, *Tablet X*) y programar la función en JS para simular métricas cuantitativas (*ventas*, *ingresos*, *precio*) con `Math.random()`, normalizando valores nulos/undefined a `0` y filtrando atípicos mediante el Rango Intercuartílico (IQR)[cite: 5, 6].
  - **Requisito Funcional (RF) cubierto:** `[RF-UBI-01]`, `[RF-EVT-01]`, `[RF-EST-01]`, Casos Límite 1 y 2[cite: 5, 6].

- [ ] **T4: Creación del SVG base, márgenes y escalas D3.js con mapeo de Ventas en el Eje X**
  - **Descripción:** Configurar los atributos del SVG de D3.js (`viewBox="0 0 900 550"`, `preserveAspectRatio="xMidYMid meet"`), definir la convención de márgenes para la sección inferior y calcular las escalas dinámicas: `d3.scaleLinear` para **Ventas en el Eje X** (`[0, d3.max(data, d => d.sales) * 1.15]`), `d3.scaleLinear` para **Ingresos en el Eje Y**, `d3.scaleSqrt` con radio mínimo $r \ge 8\text{px}$ para precio, y `d3.scaleOrdinal` para la paleta de colores fija por producto (Azul `#2B78E4`, Naranja `#E65100`, Verde `#27AE60`)[cite: 5, 6].
  - **Requisito Funcional (RF) cubierto:** `[RF-UBI-03]`, `[RF-EST-02]`, `[RF-EST-04]`, Casos Límite 3[cite: 5, 6].

- [ ] **T5: Renderizado de ejes X e Y con formato numérico para Ventas y de moneda para Ingresos**
  - **Descripción:** Renderizar e integrar los ejes dinámicos X (`d3.axisBottom(xScale)`) asignado formalmente a las **Ventas** e Y (`d3.axisLeft(yScale)`) asignado a los **Ingresos** dentro de los grupos `#x-axis-group` e `#y-axis-group`, aplicando formatos numéricos en español (comas para unidades de ventas y formato de moneda `$USD` para ingresos)[cite: 5, 6].
  - **Requisito Funcional (RF) cubierto:** `[RF-EST-04]`, Criterios de Aceptación 3[cite: 5, 6].

- [ ] **T6: Renderizado de burbujas D3.js, transparencia por mes y animaciones de entrada**
  - **Descripción:** Mapear los datos procesados en `#bubbles-group` aplicando el patrón *Data-Join* / *Enter*, proyectando horizontalmente (`cx`) las burbujas mediante la escala del eje X de **Ventas**, renderizando las burbujas correspondientes al mes de *Enero* con transparencia al 60% (`opacity: 0.6`) y las de *Febrero* con opacidad completa (`opacity: 1.0`), acompañadas de transiciones animadas fluidas (`.transition()`, `.duration(800)`)[cite: 5, 6].
  - **Requisito Funcional (RF) cubierto:** `[RF-EVT-01]`, `[RF-EST-02]`, `[RF-EST-03]`, Criterio de Aceptación 3[cite: 5, 6].

- [ ] **T7: Implementación de eventos hover (`mouseover`, `mousemove`, `mouseout`) y Tooltip contextual**
  - **Descripción:** Programar los eventos interactivos de cursor sobre las burbujas SVG para acentuar el elemento activo y desplegar el contenedor `#tooltip` en la posición exacta del puntero (`pageX`, `pageY`) con fondo claro, texto negro y métricas formateadas en español (*ventas*, *ingresos*, *precio*)[cite: 5, 6].
  - **Requisito Funcional (RF) cubierto:** `[RF-EVT-02]`, `[RF-EVT-03]`[cite: 5, 6].

- [ ] **T8: Leyendas explicativas superiores de productos sin solapamientos**
  - **Descripción:** Construir los grupos de leyenda superior (`#legend-group`) tanto en el gráfico de barras de Google Charts como en el SVG de D3.js, ubicándolos en la parte superior con espacio suficiente para identificar visualmente cada producto por su color (*Smartphone A*: Azul, *Smartphone B*: Naranja, *Tablet X*: Verde) sin que se superpongan a los ejes, barras ni burbujas, e incluir el texto de descripción explícito: `Eje x = Ventas, Eje y = Ingresos, Tamaño = Precio, Izquierda = enero, Derecha = febrero`[cite: 5, 6].
  - **Requisito Funcional (RF) cubierto:** `[RF-EST-02]`, Criterios de Aceptación 2 y 3[cite: 5, 6].

- [ ] **T9: Integración del gráfico de barras agrupadas con Google Charts en la sección superior**
  - **Descripción:** Instanciar y renderizar el gráfico de columnas agrupadas (`google.visualization.ColumnChart`) en `#google-chart-container` al disparar `setOnLoadCallback`, configurando el eje X para los meses (*Enero*, *Febrero*), el eje Y para unidades vendidas y asignando la paleta de colores idéntica por producto con su leyenda superior sin traslapes[cite: 5, 6].
  - **Requisito Funcional (RF) cubierto:** `[RF-EVT-04]`, `[RF-EST-02]`, Criterio de Aceptación 2[cite: 5, 6].

- [ ] **T10: Sincronización global mediante el botón «Actualizar» y ciclo *Enter-Update-Exit***
  - **Descripción:** Vincular el evento `click` del botón `#update-btn` para reejecutar la función de simulación de datos con `Math.random()` y actualizar simultáneamente ambas visualizaciones (Google Charts y D3.js) aplicando el ciclo completo *Enter-Update-Exit* con transiciones de redibujado fluidas sin duplicar elementos DOM[cite: 5, 6].
  - **Requisito Funcional (RF) cubierto:** `[RF-EVT-01]`, `[RF-EST-04]`, Comportamiento No Deseado 2, Criterio de Aceptación 4[cite: 5, 6].