# Desarrollo Web de Alto Rendimiento. [[Soporte](https://www.linkedin.com/in/oscarlizarragag/)]
## 🚀 Core Web Vitals y Herramientas de Medición

Los Core Web Vitals son un conjunto de métricas definidas por Google que miden la experiencia del usuario, centrándose en la **velocidad de carga**, la **interactividad** y la **estabilidad visual** de la página.

Dominar estas métricas y saber cómo optimizarlas es crucial para un experto.

### 1\. Las Tres Métricas Clave

| Métrica | Objetivo Profesional | Enfoque de Optimización |
| :--- | :--- | :--- |
| **LCP (Largest Contentful Paint)** | **2.5 segundos o menos** | Mide el tiempo que tarda en renderizarse el **elemento de contenido principal** (imagen, bloque de texto grande) visible en la *viewport*. |
| **FID (First Input Delay)** | **100 milisegundos o menos** | Mide el tiempo desde que el usuario interactúa con la página (clic, toque) hasta que el navegador puede comenzar a **procesar** el evento. (Reemplazado en 2024 por **INP**). |
| **CLS (Cumulative Layout Shift)** | **0.1 o menos** | Mide la **estabilidad visual**. Cuantifica la cantidad de **movimiento inesperado** de los elementos de la página mientras se carga. |
| **INP (Interaction to Next Paint)** | **200 milisegundos o menos** | El sucesor de FID. Mide la latencia de **todas las interacciones** del usuario y reporta el peor resultado (o el percentil 75). |

### 2\. Optimización para LCP (Largest Contentful Paint)

El LCP suele ser el más difícil de optimizar y está muy ligado a la eficiencia de la estructura HTML y la carga de recursos.

  * **HTML Semántico y Optimizado:** Asegúrate de que el **elemento LCP** (a menudo una imagen o un titular principal) sea cargado lo antes posible.
  * **Priorización de la Imagen LCP:** Si el elemento LCP es una imagen, asegúrate de que **no** tenga el atributo `loading="lazy"` y utiliza `<link rel="preload" as="image" href="...">` en el `<head>` para darle la máxima prioridad.
  * **Minimizar Bloqueo:** Reduce el tiempo que el navegador pasa analizando CSS y JS. Mueve el CSS no crítico al final del documento y usa `defer` o `async` en los *scripts* de JavaScript.

### 3\. Optimización para CLS (Cumulative Layout Shift)

El CLS se resuelve asegurando que el navegador sepa **cuánto espacio** necesita cada elemento antes de que se cargue.

  * **Dimensiones de Imágenes:** Siempre incluye los atributos **`width`** y **`height`** en tus etiquetas `<img>`. Esto reserva el espacio necesario y evita que el contenido circundante se mueva cuando la imagen se cargue.
    ```html
    <img src="hero.jpg" width="1200" height="600" alt="Imagen principal">
    ```
  * **Espacio para Ads e Iframes:** Reserva el espacio para bloques de anuncios o *widgets* embebidos (como videos de YouTube o *iframes*) incluso si aún no se han cargado.

### 4\. Herramientas de Medición para Expertos

Un profesional no solo optimiza, sino que **mide consistentemente**.

#### A. Lighthouse (Integrado en DevTools)

  * **Uso:** Auditoría exhaustiva **basada en laboratorio** (simulada) que analiza Rendimiento, Accesibilidad, Mejores Prácticas y SEO.
  * **Enfoque Profesional:** Utiliza el informe para obtener una **lista de oportunidades de mejora** detallada, como "Eliminar recursos que bloquean el renderizado" y "Servir imágenes en formatos de próxima generación".

#### B. PageSpeed Insights

  * **Uso:** Muestra datos tanto de **Laboratorio** (Lighthouse) como de **Campo** (datos reales de usuarios de Chrome, o **CrUX**).
  * **Enfoque Profesional:** La métrica de **Campo** (Field Data) es más importante, ya que refleja la **experiencia real** de los usuarios. Un profesional siempre prioriza la optimización que mejora las métricas de campo.

#### C. DevTools (Pestaña Performance)

  * **Uso:** La herramienta de **debugging más profunda**. Permite grabar la actividad del navegador durante la carga y la interacción.
  * **Enfoque Profesional:** Utilízala para:
      * Identificar el **Time To Interactive (TTI)** y ver qué tareas de JavaScript están ocupando el hilo principal.
      * Analizar el **Flame Chart** (Gráfico de Llamadas) para ver exactamente qué funciones están consumiendo más tiempo y causando latencia (vital para INP/FID).

-----

