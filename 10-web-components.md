# Desarrollo Web de Alto Rendimiento. [[Soporte](https://www.linkedin.com/in/oscarlizarragag/)]
## 5. 🧱 Web Components (Componentización Nativa)

Como experto, debes conocer la respuesta nativa de HTML5 al ecosistema de *frameworks* de componentes (React, Vue, Angular). Los **Web Components** permiten crear elementos HTML **reutilizables y encapsulados** para el desarrollo de sistemas de diseño y librerías de interfaz.

Los Web Components se basan en tres tecnologías principales:

1.  **Custom Elements:** Permite definir nuevas etiquetas HTML (ej. `<mi-tarjeta-usuario>`). Los expertos deben dominar la clase `HTMLElement` para definir su comportamiento y ciclo de vida (ej. `connectedCallback`, `attributeChangedCallback`).
2.  **Shadow DOM:** Proporciona **encapsulación** de estilos y estructura. El código CSS y la estructura HTML dentro del Shadow DOM están aislados del DOM principal de la página, evitando conflictos de estilos (**CSS *leaking***).
3.  **HTML Templates (`<template>` y `<slot>`):**
    * **`<template>`:** Contiene marcado HTML que **no se renderiza** inmediatamente, sino que sirve como plantilla para ser reutilizada por JavaScript.
    * **`<slot>`:** Elemento dentro del Shadow DOM que permite inyectar contenido externo del DOM principal dentro del componente, facilitando la **composición**. 

**Uso Experto:** Utilizar Web Components para crear componentes de diseño neutrales y compartibles que se puedan integrar en cualquier *framework* de JavaScript (o sin él).

---

## 6. 🛡️ Seguridad (Content Security Policy - CSP)

La seguridad es una responsabilidad central del profesional. Debes ir más allá de la validación de formularios y entender las políticas de defensa del lado del cliente.

* **Content Security Policy (CSP):** Una capa de seguridad crucial que ayuda a mitigar las vulnerabilidades de inyección de código, como los ataques **Cross-Site Scripting (XSS)**.
    * La CSP se implementa mediante un **encabezado de respuesta HTTP** (`Content-Security-Policy`) o una etiqueta `<meta>`.
    * Permite al experto definir **fuentes de contenido confiables** para *scripts*, estilos, imágenes y otros recursos.

#### Directivas Clave del CSP

| Directiva | Descripción | Ejemplo de Valor |
| :--- | :--- | :--- |
| **`default-src`** | Política de *fallback* para todos los tipos de recursos. | `'self'` (solo contenido del mismo origen) |
| **`script-src`** | Fuentes permitidas para archivos JavaScript. | `'self' https://cdn.com'` |
| **`style-src`** | Fuentes permitidas para hojas de estilo CSS. | `'self' 'unsafe-inline'` (debe evitarse) |
| **`connect-src`** | Endpoints a los que se puede conectar (Fetch, WebSockets, XHR). | `'self' wss://chat.com'` |

**Uso Experto:** Implementar una política CSP estricta que restrinja el uso de *scripts* y estilos en línea (`'unsafe-inline'`) y solo permita cargar recursos de dominios propios y CDN verificados.

Dominar estas dos áreas te da una base para la **arquitectura** de la aplicación, el **diseño de sistemas** y la **protección contra ataques**.