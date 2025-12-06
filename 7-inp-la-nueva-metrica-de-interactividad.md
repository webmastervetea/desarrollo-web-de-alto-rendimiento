# Desarrollo Web de Alto Rendimiento. [[Soporte](https://www.linkedin.com/in/oscarlizarragag/)]
## ⏱️ INP (Interaction to Next Paint): La Nueva Métrica de Interactividad

INP es la métrica de Core Web Vitals que reemplaza a **FID (First Input Delay)**. Mientras que FID solo medía el tiempo que tardaba el navegador en empezar a procesar la **primera** interacción, INP mide la latencia de **todas las interacciones** de un usuario con la página (clics, toques y pulsaciones de teclado) y reporta el **peor** resultado (o el percentil 75) como el valor final.

### 1. ¿Qué Mide Exactamente INP?

INP mide el tiempo total desde que un usuario inicia una interacción hasta que el navegador pinta el **siguiente fotograma** que refleja visualmente el resultado de esa interacción (el "Next Paint").

Una interacción se compone de tres fases:
1.  **Input Delay (Retraso de entrada):** El tiempo que transcurre desde el inicio de la interacción hasta que el *handler* de eventos de JavaScript comienza a ejecutarse.
2.  **Processing Time (Tiempo de procesamiento):** El tiempo que tarda el *handler* de eventos de JavaScript en ejecutarse.
3.  **Presentation Delay (Retraso de presentación):** El tiempo que el navegador necesita para calcular los cambios de diseño (**Layout**), pintar (**Paint**) y componer (**Composite**) el nuevo fotograma en la pantalla.



### 2. Umbrales de Rendimiento

| Valor INP | Experiencia del Usuario |
| :--- | :--- |
| **Menos de 200 ms** | **Buena:** La aplicación es consistentemente rápida. |
| **Entre 200 ms y 500 ms** | **Necesita mejora** |
| **Más de 500 ms** | **Mala:** Los usuarios experimentan un retraso notable. |

### 3. Técnicas de Optimización para Expertos (INP)

La clave para optimizar el INP es reducir el **tiempo de bloqueo del hilo principal** de JavaScript.

#### A. Reducir el Tiempo de Procesamiento (Processing Time)

Esta fase se ve afectada por **tareas de JavaScript largas**.

* **Desglosar Tareas Largas (*Breaking Up Long Tasks*):** Si una función JavaScript tarda más de 50 ms en ejecutarse (considerada una "tarea larga"), utiliza técnicas como **`setTimeout`**, **`requestAnimationFrame`**, o **`postMessage`** para dividir el trabajo en fragmentos más pequeños, permitiendo que el navegador procese interacciones entre medio.
* **Priorizar la Entrada:** Utiliza la API de **Prioridad de la Entrada** (cuando esté disponible) o la librería **`scheduler.yield()`** (experimental) para ceder el control al hilo principal y permitir que se ejecuten las interacciones urgentes.

#### B. Optimizar el Retraso de Presentación (Presentation Delay)

Esta fase es el resultado de costosos cálculos de diseño (Layout).

* **Evitar el "Layout Thrashing":** El *layout thrashing* ocurre cuando lees repetidamente un valor de diseño (ej. `element.offsetWidth`) e inmediatamente lo cambias (ej. `element.style.width = '...'`) en un bucle. Esto obliga al navegador a recalcular el diseño forzosamente en cada iteración. Agrupa las lecturas y las escrituras.
* **Usar Propiedades Ligeras:** Siempre que sea posible, utiliza propiedades CSS que no fuercen el *Layout* ni el *Paint* completo, como **`transform`** y **`opacity`**, ya que son manejadas por el compositor y aceleradas por hardware.

#### C. Controlar el Retraso de Entrada (Input Delay)

* **Reducir Tareas en la Carga Inicial:** Si el hilo principal está ocupado analizando y compilando JavaScript durante la carga inicial, el `Input Delay` de la primera interacción será alto. Prioriza la carga de JavaScript y usa `defer` y `async` para evitar que bloquee el hilo.

### 4. Herramientas para Diagnosticar INP

* **Chrome DevTools (Performance Panel):** El panel de **Rendimiento** es esencial. Graba una interacción (un clic de usuario) y revisa la línea de tiempo. Identifica las **Tareas Largas** (marcadas con un triángulo rojo) que se ejecutan después de la interacción, ya que son las principales culpables de un INP alto.
* **PageSpeed Insights:** Muestra el valor de **INP de campo** (datos reales de usuarios) para tu sitio, dándote una imagen clara de la experiencia promedio.

Dominar INP significa escribir código JavaScript y CSS que interactúe con el DOM de manera eficiente y no acapare el hilo principal.

