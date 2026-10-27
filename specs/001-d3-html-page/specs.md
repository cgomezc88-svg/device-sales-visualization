# Especificación Técnica: Visualización de Ventas (device-sales-visualization)

## Contexto y Objetivo
El objetivo de este componente es desarrollar un **dashboard interactivo, adaptable y autónomo** contenido exclusivamente en la raíz del proyecto dentro del archivo `index.html` para evaluar el desempeño comercial de los productos (*Smartphone A*, *Smartphone B*, *Tablet X*) durante los meses de enero y febrero[cite: 5, 6, 7].

La aplicación funcionará de forma 100% autónoma del lado del cliente sin requerir un servidor web local (compatible con el protocolo `file://`)[cite: 5, 6]. Los datos iniciales estarán embebidos directamente dentro de `index.html` siguiendo una estructura JSON[cite: 5, 6]. 

El dashboard integrará dos visualizaciones dispuestas en secuencia vertical[cite: 5, 6]:
1. **Visualización Superior: Gráfico de Barras Agrupadas / Columnas (Google Charts):** Representa las unidades vendidas por mes (*Enero*, *Febrero*) comparando los tres productos[cite: 5, 6]. La leyenda de identificación por color se ubicará en la parte superior sin solaparse con los ejes ni con las barras[cite: 5, 6].
2. **Visualización Inferior: Gráfico Multidimensional de Burbujas (D3.js v7):** Renderizado debajo del gráfico de barras dentro de la misma página[cite: 5, 6]. Analizará las dimensiones de *ventas* (eje X), *ingresos* (eje Y), *precio* (radio), *producto* (color) y posición temporal (*Enero* a la izquierda, *Febrero* a la derecha)[cite: 5, 6]. Las burbujas de *Enero* mostrarán una transparencia del 60% (`opacity: 0.6`)[cite: 5, 6]. Incluirá la leyenda superior de productos y un texto descriptivo explícito en la parte superior[cite: 5, 6].

Ambas visualizaciones se sincronizarán mediante el botón global «Actualizar», el cual recomputará métricas simuladas mediante `Math.random()`[cite: 5, 6].

---

## Requisitos Funcionales (RF) — Notación EARS

### Ubicuidad
- **[RF-UBI-01]** El sistema **almacenará** y gestionará el conjunto de datos de los productos (*Smartphone A*, *Smartphone B*, *Tablet X*) y sus métricas (*ventas*, *ingresos*, *precio*, *mes*) directamente embebido dentro de `index.html` en una estructura JSON interna[cite: 5, 6].
- **[RF-UBI-02]** El sistema **cargará** las librerías **Google Charts** (`https://www.gstatic.com/charts/loader.js`) y **D3.js v7** (`https://cdnjs.cloudflare.com/ajax/libs/d3/7.9.0/d3.min.js`) desde sus CDN oficiales dentro de `index.html`, sin empaquetadores locales ni dependencias externas[cite: 5, 6].
- **[RF-UBI-03]** El sistema **utilizará** un elemento SVG con los atributos `viewBox` y `preserveAspectRatio` para la visualización en D3.js y contenedores `<div>` adaptables para Google Charts, garantizando un despliegue espacioso y responsive en la misma página[cite: 5, 6].

### Evento
- **[RF-EVT-01]** **Cuando** el usuario haga clic en el botón «Actualizar», el sistema **generará** métricas simuladas mediante la función `Math.random()` y **redibujará simultáneamente** el gráfico de barras de Google Charts y el gráfico de burbujas de D3.js mediante transiciones animadas fluidas[cite: 5, 6].
- **[RF-EVT-02]** **Cuando** el usuario pase el cursor (`mouseover` / `mousemove`) sobre una burbuja del gráfico D3.js, el sistema **destacará** el elemento y **mostrará** un *tooltip* emergente en la posición exacta del cursor con fondo tono claro, texto negro, formato de número entero con comas en *ventas* y formato de moneda ($USD) en *ingresos* y *precio*[cite: 5, 6, 7].
- **[RF-EVT-03]** **Cuando** el usuario mueva el cursor fuera (`mouseout`) de la burbuja en D3.js, el sistema **ocultará** el *tooltip* y **restablecerá** los estilos originales del elemento SVG[cite: 5, 6, 7].
- **[RF-EVT-04]** **Cuando** la librería de Google Charts finalice su carga inicial (`setOnLoadCallback`), el sistema **instanciará** y **dibujará** automáticamente el gráfico de barras agrupadas[cite: 5, 6, 7].

### Estado
- **[RF-EST-01]** **Mientras** los datos se encuentren en proceso de renderizado o actualización, el sistema **mostrará** un indicador o mensaje de carga accesible dentro del dashboard[cite: 5, 7].
- **[RF-EST-02]** **Mientras** el dataset contenga registros válidos, el sistema **mantendrá** una paleta de colores fija e idéntica por producto (*Smartphone A*: `#0098CD`, *Smartphone B*: `#DB8100`, *Tablet X*: `#00566C`) en ambas visualizaciones, incluyendo sus leyendas superiores explicativas sin solapamientos[cite: 5, 6].
- **[RF-EST-03]** **Mientras** se renderice el gráfico de burbujas de D3.js, el sistema **aplicará** una opacidad del 60% (`opacity: 0.6`) a las burbujas del mes de *Enero* y opacidad completa (`opacity: 1.0`) a las del mes de *Febrero*[cite: 5, 6].
- **[RF-EST-04]** **Mientras** se recomputen las escalas ante un evento de actualización, el sistema **ajustará dinámicamente** el dominio de los ejes en D3.js al nuevo rango de datos válidos[cite: 5, 6, 7].

---

## Casos Límite y Manejo de Outliers
1. **Tratamiento de Outliers (Rango Intercuartílico - IQR):** Si un registro contiene valores numéricos atípicos fuera del rango $[Q1 - 1.5 \times IQR, Q3 + 1.5 \times IQR]$, el sistema lo omitirá de la representación de D3.js y desplegará un modal informativo que indique el motivo de omisión, la cantidad de valores excluidos y sus cifras correspondientes. Este modal sólo contendrá un botón para cerrarlo[cite: 5, 6, 7].
2. **Datos nulos o no definidos (`null` / `undefined`):** Si algún producto carece de valores en las métricas de *ventas*, *ingresos* o *precio*, el sistema reemplazará dichos valores por `0` antes de procesar las escalas[cite: 5, 6, 7].
3. **Valores negativos o iguales a cero:** Para el gráfico de burbujas de D3.js, los valores de radio calculados se mapearán a un tamaño mínimo visible de `r = 8px` para asegurar la interactividad sin distorsiones geométricas en el SVG[cite: 5, 6, 7].
4. **Lista de datos vacía (`[]`):** Si el dataset JSON está vacío, ambos contenedores pausarán el renderizado gráfico y mostrarán un mensaje claro de *"Sin datos disponibles"*[cite: 5, 6, 7].
5. **Pantallas pequeñas y móviles:** En resoluciones reducidas, los elementos gráficos y leyendas se reajustarán verticalmente garantizando holgura sin superponer ejes[cite: 5, 6, 7].

---

## Comportamiento No Deseado
- **Si** la estructura del JSON embebido contiene errores sintácticos de parseo, **entonces** el sistema **abortará** el dibujo de ambos gráficos y **desplegará** un mensaje de error visible e informativo en el contenedor principal de `index.html`[cite: 5, 6, 7].
- **Si** el usuario presiona el botón «Actualizar» repetidamente durante una transición activa en D3.js, **entonces** el sistema **cancelará** o **encadenará** las animaciones sin duplicar elementos SVG ni contenedores en el DOM[cite: 5, 6, 7].

---

## Criterios de Aceptación
1. **Despliegue Autónomo y Archivo Único:** La aplicación completa residirá **exclusivamente en la raíz dentro del archivo `index.html`** (con CSS `<style>`, datos JSON y JS `<script>` embebidos), ejecutándose directamente al abrir el archivo en cualquier navegador sin requerir servidor HTTP local (`file://`)[cite: 5, 6].
2. **Gráfico Superior de Barras (Google Charts):** 
   - Eje X = Mes (*Enero*, *Febrero*)[cite: 5, 6].
   - Eje Y = Unidades vendidas[cite: 5, 6].
   - Asignación de color: *Smartphone A* (Azul), *Smartphone B* (Naranja), *Tablet X* (Verde)[cite: 5, 6].
   - Identificación de producto por color en la parte superior del gráfico, con espaciado suficiente sin superponerse al eje Y ni a las barras[cite: 5, 6].
3. **Gráfico Inferior de Burbujas (D3.js v7):**
   - Dispuesto debajo del gráfico de barras en la misma página[cite: 5, 6].
   - Mapeo: Eje X = Ventas, Eje Y = Ingresos, Radio = Precio ($r \ge 8\text{px}$), Posición = Izquierda (*Enero*) y Derecha (*Febrero*)[cite: 5, 6].
   - Transparencia: Transparencia al 60% (`opacity: 0.6`) para *Enero* y opacidad completa para *Febrero*[cite: 5, 6].
   - Leyenda de color superior sin solaparse con ejes ni burbujas, junto a la descripción explícita: `Eje x = Ventas, Eje y = Ingresos, Tamaño = Precio, Izquierda = enero, Derecha = febrero`[cite: 5, 6].
4. **Sincronización Dinámica:** El botón «Actualizar» simula métricas con `Math.random()` y re-renderiza ambas visualizaciones simultáneamente con transiciones suaves[cite: 5, 6].
5. **Estándar Idiomático y Código:** Textos de interfaz, comentarios, descripciones, leyendas y tooltips estrictamente en **español**; código JavaScript con variables y funciones en **inglés** (*camelCase*)[cite: 5, 6].