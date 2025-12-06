# Desarrollo Web de Alto Rendimiento. [[Soporte](https://www.linkedin.com/in/oscarlizarragag/)]
## 🔗 History API: Navegación sin Recarga

La API se centra en manipular la pila del historial del navegador (los botones de "Atrás" y "Adelante") a través de los objetos `history` del navegador.

### 1\. El Método Clave: `pushState()`

El método `history.pushState()` te permite agregar una nueva entrada al historial del navegador sin disparar una recarga del documento.

#### Sintaxis

```javascript
history.pushState(state, title, url);
```

| Parámetro | Descripción |
| :--- | :--- |
| **`state`** | Un objeto JavaScript que representa el **estado** asociado con la nueva entrada del historial. Puedes recuperar este objeto más tarde en el evento `popstate`. |
| **`title`** | (Generalmente ignorado por los navegadores) El título que deseas asociar con la nueva entrada del historial. |
| **`url`** | La nueva URL de la página. El navegador **muestra** esta URL en la barra de direcciones, pero **no navega** realmente a ella. |

### 2\. Ejemplo Práctico: Navegación de Contenido

Imagina un sitio con dos secciones (`/seccion1` y `/seccion2`).

#### Paso 1: JavaScript para el Cambio de Estado (`history_spa.js`)

Este código simula la carga de contenido y la actualización del historial.

```javascript
// history_spa.js

function cargarContenido(url) {
    let titulo, contenidoHTML;

    // 1. Simulación de lógica de ruteo y carga de datos
    if (url.includes('seccion2')) {
        titulo = 'Sección Dos';
        contenidoHTML = '<h2>Bienvenido a la Sección 2</h2><p>Aquí hay contenido cargado dinámicamente.</p>';
    } else {
        titulo = 'Sección Uno';
        contenidoHTML = '<h2>Página Principal</h2><p>Este es el contenido inicial de la Sección 1.</p>';
    }

    // 2. Actualizar el DOM (la UI)
    document.getElementById('contenido-principal').innerHTML = contenidoHTML;
    document.title = titulo;

    // 3. Crear el objeto de estado y la URL para el historial
    const estado = { url: url, titulo: titulo };

    // 4. Agregar la entrada al historial. ¡No hay recarga de página!
    history.pushState(estado, titulo, url);

    console.log(`✅ Estado agregado. URL actual: ${url}`);
}

// Escuchador de eventos para los enlaces
document.addEventListener('DOMContentLoaded', () => {
    document.getElementById('nav-links').addEventListener('click', (e) => {
        if (e.target.tagName === 'A') {
            e.preventDefault(); // Detener la navegación normal (recarga)
            const nuevaURL = e.target.getAttribute('href');
            cargarContenido(nuevaURL);
        }
    });

    // Inicializar el estado de la URL actual al cargar la página por primera vez
    history.replaceState({ url: location.pathname, titulo: document.title }, document.title, location.pathname);
});
```

#### Paso 2: Manejo de los Botones "Atrás" y "Adelante" (`popstate`)

Cuando el usuario hace clic en los botones del historial del navegador (Atrás/Adelante), se dispara el evento **`popstate`**. Aquí es donde debes usar la información almacenada en el objeto `state`.

```javascript
// history_spa.js (continuación)

window.addEventListener('popstate', (e) => {
    // e.state contiene el objeto 'state' que guardamos con pushState()
    if (e.state) {
        const { url, titulo } = e.state;
        
        // Cargar el contenido basado en el estado recuperado
        let contenidoHTML;
        if (url.includes('seccion2')) {
            contenidoHTML = '<h2>Sección Dos (Desde Atrás)</h2><p>Contenido restaurado por popstate.</p>';
        } else {
            contenidoHTML = '<h2>Sección Uno (Desde Atrás)</h2><p>Contenido restaurado por popstate.</p>';
        }

        document.getElementById('contenido-principal').innerHTML = contenidoHTML;
        document.title = titulo;

        console.log(`⏪ Navegación por popstate a: ${url}`);
    }
});
```

### 3\. Métodos Adicionales

  * **`history.replaceState(state, title, url)`:**
      * Funciona como `pushState()`, pero **reemplaza** la entrada actual del historial en lugar de añadir una nueva. Es útil cuando se desea actualizar el estado de la URL actual (por ejemplo, al aplicar un filtro en una lista) sin abarrotar el historial.
  * **`history.back()`, `history.forward()`, `history.go(delta)`:**
      * Simulan los clics en los botones del navegador. `history.go(-1)` es equivalente a `history.back()`.

### 4\. Consideraciones Profesionales

  * **Fallback para Recargas:** Cuando un usuario recarga la página en una URL generada con `pushState()` (ej. `/seccion2`), el servidor **debe** poder manejar esa ruta y servir la página correcta. Si el servidor solo conoce la raíz (`/`), la página fallará. Esto se resuelve a menudo con la configuración de *routing* en el lado del servidor o mediante el uso del *hash* (`#`) en la URL (un enfoque más antiguo).
  * **Objeto `state`:** Sé diligente sobre qué información guardas en el objeto `state`. Solo necesitas los datos mínimos para poder recrear la vista cuando se dispare el evento `popstate`.

-----

