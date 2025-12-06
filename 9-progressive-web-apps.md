# Desarrollo Web de Alto Rendimiento. [[Soporte](https://www.linkedin.com/in/oscarlizarragag/)]

## 1. ⚙️ Progressive Web Apps (PWAs)

Las PWAs son el estándar de oro para las aplicaciones web modernas, permitiendo que un sitio se sienta y funcione como una aplicación nativa. El conocimiento experto de PWAs requiere dominar dos APIs cruciales construidas sobre HTML5:

* **Service Workers:** Son la espina dorsal de las PWAs. Un experto debe saber cómo registrar, instalar y activar un Service Worker. Son scripts que se ejecutan en segundo plano, separados de la página principal, permitiendo:
    * **Caché Avanzado:** Controlar peticiones de red y cachear recursos para acceso instantáneo, permitiendo la funcionalidad **offline**.
    * **Sincronización en Segundo Plano:** Enviar datos al servidor cuando la conexión vuelve.
    * **Notificaciones Push:** Recibir notificaciones incluso cuando el navegador está cerrado.
    * 
* **Web App Manifest:** Un archivo JSON (`manifest.json`) que proporciona información sobre la PWA (nombre, iconos, pantalla de inicio, color del tema) para que el navegador pueda presentarla al usuario como una aplicación instalable.

---

## 2. 📡 Comunicación en Tiempo Real

Para construir aplicaciones interactivas modernas (chats, juegos, *dashboards* en vivo), necesitas dominar los mecanismos de comunicación bidireccional de alto rendimiento:

* **WebSockets:** Proporcionan un **canal de comunicación dúplex completo** (bidireccional) a través de una única conexión TCP. A diferencia de las peticiones HTTP tradicionales (que se cierran después de cada respuesta), WebSockets mantienen la conexión abierta, lo cual es esencial para reducir la latencia y el *overhead* para la transmisión constante de pequeños paquetes de datos.
    * Un experto sabe cuándo usar WebSockets versus *polling* o *Server-Sent Events* (SSE).
* **WebRTC (Web Real-Time Communication):** Permite la comunicación de **audio, video y datos genéricos *peer-to-peer*** (de igual a igual) sin necesidad de *plugins*. Es la base para construir videoconferencias directamente en el navegador.

---

## 3. 📂 Almacenamiento Avanzado del Lado del Cliente

Un profesional sabe cómo elegir el mecanismo de almacenamiento más adecuado para cada caso de uso.

| API | Propósito | Limitaciones Clave | Caso de Uso Experto |
| :--- | :--- | :--- | :--- |
| **LocalStorage** | Clave/Valor simple, persistente. | Síncrono, pequeño (5-10MB), solo strings. | Configuraciones de usuario, tokens ligeros. |
| **SessionStorage** | Clave/Valor simple, solo durante la sesión. | Síncrono, pequeño (5-10MB), solo strings. | Datos de carrito de compra temporales. |
| **IndexedDB** | Almacén de objetos, transaccional. | Asíncrono, más complejo de usar, grande (cientos de MB). | Caché masivo de datos estructurados, datos offline de PWAs. |
| **Cache API** | Almacenamiento de parejas Request/Response. | Usado principalmente por Service Workers. | Caché de recursos de red (CSS, JS, imágenes). |

---

## 4. 🗂️ Módulos de JavaScript Nativos (`<script type="module">`)

HTML5 fue unificado con JavaScript ES6 para permitir la modularización directa en el navegador, simplificando las herramientas de *bundling* complejas:

* **Uso:** Al usar `<script type="module">`, puedes usar las sentencias `import` y `export` directamente en el navegador.
* **Comportamiento:** Los módulos son **diferidos** por defecto (no bloquean el renderizado) y se ejecutan en **modo estricto** (`'use strict';`). Además, el código solo se **ejecuta una vez** (al contrario que los scripts tradicionales), incluso si se importa varias veces.

Dominar estos conceptos te moverá del nivel de desarrollador avanzado al de **arquitecto web** que toma decisiones informadas sobre la pila tecnológica y la experiencia de usuario.