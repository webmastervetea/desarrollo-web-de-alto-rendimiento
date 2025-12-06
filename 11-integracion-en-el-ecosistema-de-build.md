# Desarrollo Web de Alto Rendimiento. [[Soporte](https://www.linkedin.com/in/oscarlizarragag/)]
## 7. 🚀 Integración en el Ecosistema de *Build*

Un profesional no solo escribe HTML5, sino que entiende cómo su código es transformado por las herramientas modernas de *build* para producción.

### A. Dominio de los Formatos Modulares

Aunque HTML5 soporta módulos nativos (`<script type="module">`), la mayoría de los proyectos usan *bundlers* (empaquetadores) para optimizar el código.

* **Uso de `import` y `export` (ESM):** Entender que las herramientas como **Webpack**, **Rollup** o **Vite** analizan estas sentencias para crear el árbol de dependencias, lo cual permite la siguiente técnica.
* **Tree Shaking:** Saber cómo estructurar el código (tanto JS como CSS) para que el *bundler* pueda eliminar el código muerto (*dead code*) que no se está utilizando. Esto resulta en archivos finales más pequeños y un mejor **LCP** y **INP**.

### B. Optimización del HTML en el *Build*

El HTML final que llega al navegador no es el que escribiste.

* **Minificación y Compresión:** Entender que herramientas como **HTMLMinifier** eliminan espacios en blanco, comentarios y etiquetas innecesarias. El servidor luego comprime el archivo con **Gzip** o **Brotli** antes de enviarlo.
* **Inlining de Recursos Críticos:** Un experto sabe configurar el *build* para inyectar el **CSS Crítico** directamente en el `<head>` del HTML. Esto es crucial para la puntuación del **LCP** y para evitar el *Flash of Unstyled Content* (FOUC), permitiendo que el navegador renderice la parte superior de la página inmediatamente.
* **Hashing de Archivos:** Las herramientas de *build* añaden un *hash* (identificador único) al nombre de los archivos CSS/JS (ej., `app.f34a8b.js`). Esto permite establecer una política de **caché de larga duración** (`Cache-Control: max-age=31536000`) sin preocuparse de que los usuarios viejos carguen código obsoleto.

### C. Server-Side Rendering (SSR) y Static Site Generation (SSG)

Para la máxima velocidad y SEO, el HTML se debe generar antes de que el navegador lo solicite.

* **SSR:** Generar la primera vista de la aplicación en el servidor. Esto proporciona un HTML completamente formado (ideal para el **SEO** y el **LCP**) y luego el JavaScript toma el control en el cliente (*hidratación*).
* **SSG:** Generar todo el HTML de la página **durante el *build***. Esto es lo más rápido posible, ya que solo se sirve un archivo estático.

Un experto no solo domina HTML5, sino que también sabe cómo las **herramientas y las técnicas de *build*** maximizan el rendimiento y la mantenibilidad de ese HTML5 en un entorno de producción a gran escala.

---

Estos siete puntos (Semántica, Rendimiento, Accesibilidad, APIs, Arquitectura, Seguridad, y Ecosistema de Build) cubren el espectro completo del desarrollo web con HTML5 al nivel de un experto y profesional.