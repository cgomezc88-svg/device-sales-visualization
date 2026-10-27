# Guía de Operación para Agentes de IA (AGENTS.md)

Este documento establece el marco de gobernanza, la arquitectura técnica y las reglas estrictas de desarrollo que cualquier Agente de IA o asistente de código debe cumplir al trabajar en este proyecto.

---

## 1. Visión General y Stack Tecnológico

### Visión General
Página web local de uso personal (entorno monousuario) diseñada para el renderizado dinámico e interactivo de dashboards y gráficas. La aplicación opera exclusivamente del lado del cliente (*client-side*) y de forma autónoma sin necesidad de ejecutar un servidor web local (soporta ejecución directa vía protocolo `file://`).

### Stack Tecnológico Permitido
* **Estructura y Marcado:** HTML5 semántico.
* **Estilos:** CSS3 puro embebido (sin preprocesadores como SASS/LESS ni frameworks como Tailwind/Bootstrap).
* **Lógica:** JavaScript Vanilla (ES6+) embebido.
* **Librerías de Visualización:**
  * **Google Charts API:** Cargado mediante CDN oficial (`https://www.gstatic.com/charts/loader.js`).
  * **D3.js (v7):** Cargado mediante CDN oficial (`https://cdnjs.cloudflare.com/ajax/libs/d3/7.9.0/d3.min.js`).

### Restricciones Innegociables
1. **Archivo Único de Aplicación:** La aplicación web completa debe residir exclusivamente en la raíz del proyecto dentro del archivo `index.html`. Toda la hoja de estilos CSS debe ir contenida dentro de etiquetas `<style>`, la estructura de datos JSON en `<script type="application/json">` o constante JS embebida, y la lógica JavaScript en etiquetas `<script>`. No se permiten archivos `.js` o `.css` externos separados.
2. **Carga Directa sin Servidor:** Los datos iniciales deben cargarse directamente embebidos dentro de `index.html` siguiendo el esquema JSON predefinido. No se deben realizar peticiones `fetch()` asíncronas a archivos locales externos para garantizar que `index.html` funcione abriéndolo directamente en cualquier navegador web.
3. **Sin Entornos de Ejecución de Servidor ni Empaquetadores:** Prohibido el uso de Node.js, NPM, Webpack, Vite, Parcel, Rollup o Babel.
4. **Sin Integración Continua ni Contenedores:** Exclusión total de Docker, GitHub Actions, carpetas `.github/workflows/`, o scripts de despliegue CI/CD.

---

## 2. Estructura del Proyecto

El proyecto sigue un esquema estricto de separación de responsabilidades para un desarrollo guiado por especificaciones (*Spec-Driven Development*):

```text
.
├── AGENTS.md                  # Este archivo (Instrucciones para agentes de IA)
├── index.html                 # Aplicación completa (HTML, CSS <style>, Datos JSON y JS <script> embebidos)
├── docs/
│   └── constitution.md        # Reglas innegociables del proyecto y principios rectores
├── specs/                     # Especificaciones técnicas detalladas (notación EARS)
│   └── 001-d3-html-page/
│       ├── spec.md            # Requisitos funcionales (EARS) y casos límite
│       ├── plan.md            # Diseño técnico y arquitectura DOM
│       └── tasks.md           # Desglose de tareas atómicas

```

---

## 3. Especificaciones de Visualización y Comportamiento Dinámico

Cualquier cambio o desarrollo en `index.html` debe cumplir rigurosamente las siguientes especificaciones visuales y funcionales:

### 3.1. Simulación de Datos Dinámicos

* El botón **«Actualizar»** debe recargar y recomputar el dataset en tiempo real utilizando la función `Math.random()` para generar métricas simuladas de ventas, ingresos y precio.
* Al presionar el botón, ambas visualizaciones (Google Charts y D3.js) deben actualizarse simultáneamente en la pantalla con animaciones y transiciones fluidas.

### 3.2. Gráfico 1: Columnas/Barras Agrupadas (Google Charts / D3)

* **Disposición:** Se ubica en la parte superior del dashboard con espacio suficiente para una lectura cómoda.
* **Ejes:**
* Eje X: Categórico por **Mes** (*Enero*, *Febrero*).
* Eje Y: Cuantitativo para **Unidades vendidas**.


* **Colores por Producto:**
* **Smartphone A:** Azul (`#0098CD` / `#2B78E4`)
* **Smartphone B:** Naranja (`#DB8100` / `#E65100`)
* **Tablet X:** Verde (`#00566C` / `#2EC4B6` / `#27AE60`)


* **Leyenda:** Debe incluirse la identificación de producto por color en la **parte superior del gráfico**, garantizando un margen/espaciado suficiente para que **nunca se superponga ni al eje Y ni a las barras**.

### 3.3. Gráfico 2: Gráfico Multidimensional de Burbujas (D3.js v7)

* **Ubicación y Disposición:** Debe colocarse **debajo del gráfico de barras**, en secuencia vertical con espacio suficiente para una visualización cómoda y clara dentro de la misma página sin saturar la vista.
* **Mapeo de Dimensiones:**
* **Eje X:** Ventas.
* **Eje Y:** Ingresos.
* **Tamaño (Radio):** Precio.
* **Posición Temporal:** Izquierda = *Enero*, Derecha = *Febrero*.


* **Leyenda y Descripción del Gráfico:**
* Incluir en la parte superior del gráfico un texto explicativo claro:
`Eje x = Ventas, Eje y = Ingresos, Tamaño = Precio, Izquierda = enero, Derecha = febrero`
* Incluir la identificación de productos por color en la **parte superior**, asegurando que no se superponga con los ejes ni con las burbujas.


* **Transparencia por Mes:**
* Las burbujas correspondientes al mes de **Enero** deben renderizarse con una transparencia (opacidad) del **60%** (`opacity: 0.6` o formato `rgba`).
* Las burbujas de **Febrero** se renderizarán con opacidad completa (`opacity: 1.0` o `0.85`).



---

## 4. Reglas de Operación para la IA

Cualquier Agente de IA que interactúe con este repositorio debe seguir el siguiente algoritmo de trabajo secuencial antes de proponer o escribir código:

### Protocolo de Actuación

1. **Paso 1: Consulta de Gobernanza (`docs/constitution.md`)**
* Lee y verifica los principios rectores en `docs/constitution.md`.
* Garantiza que la solución sea ejecutable de forma estática y autónoma sin necesidad de servidor HTTP local.


2. **Paso 2: Lectura de Especificaciones (`specs/`)**
* Revisa las especificaciones técnicas en `specs/` (notación EARS).
* Valida eventos, estados y los requisitos visuales de maquetación explicitados en la sección 3 de este documento.


3. **Paso 3: Mantenimiento y Edición en el Archivo Único (`index.html`)**
* Toda modificación debe realizarse dentro del archivo `index.html` en la raíz.
* Modifica reglas CSS exclusivamente dentro de `<style>`.
* Modifica la lógica JavaScript únicamente dentro de `<script>`. No agregues archivos `.js` o `.css` externos.


4. **Paso 4: Convenciones Idiomáticas y de Código**
* **Código y Sintaxis:** Nombres de variables, constantes, funciones, objetos y atributos **DEBEN estar strictly en inglés** en formato *camelCase* (ej. `updateRandomMetrics`, `renderBarChart`, `margin`, `svgContainer`).
* **Comentarios y UI:** Comentarios descriptivos, títulos, tooltips, etiquetas de ejes y textos instructivos de la interfaz **DEBEN estar redactados en español**.


5. **Paso 5: Diseños SVG Adaptables y Gestión de Márgenes en D3.js**
* Aplica siempre los atributos `viewBox` y `preserveAspectRatio` en los contenedores SVG.
* Utiliza la convención de márgenes explícita de D3 para separar adecuadamente el grupo de la leyenda superior (`#legend-group`) de las áreas de gráficos, ejes y burbujas, evitando cualquier tipo de colisión o solapamiento visual.
