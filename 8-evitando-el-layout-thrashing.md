# Desarrollo Web de Alto Rendimiento. [[Soporte](https://www.linkedin.com/in/oscarlizarragag/)]

## 🎨 Evitando el Layout Thrashing (Recálculo Forzado)

### 1\. ¿Qué es el Layout Thrashing?

El *Layout Thrashing* es un problema de rendimiento que ocurre cuando el código JavaScript **fuerza** al navegador a realizar el cálculo de diseño (**Layout**) antes de que sea el momento óptimo para hacerlo.

El navegador funciona en un ciclo: lee el DOM, calcula todos los estilos, luego calcula el diseño (`Layout`), y finalmente pinta (`Paint`) y compone. Este ciclo debe ser asíncrono y por lotes.

El *thrashing* ocurre cuando:

1.  **Escribes (Modificas)** el DOM o los estilos (ej. `elemento.style.width = '200px'`).
2.  Inmediatamente después, **Lees** una propiedad de diseño que requiere el valor actualizado (ej. `elemento.offsetWidth`).

Al leer la propiedad, el navegador se da cuenta de que el diseño está "sucio" (fue modificado en el paso 1) y debe **detenerse y recalcular todo el diseño de forma síncrona** antes de devolver el valor. Si esto ocurre en un bucle, el rendimiento se desploma.

### 2\. Propiedades que Desencadenan la Lectura

Cualquier propiedad que el navegador necesite para calcular la **posición o el tamaño** de un elemento puede forzar un Layout.

| Propiedades de Lectura Comunes que Forzan Layout |
| :--- |
| `element.offsetLeft`, `element.offsetTop`, `element.offsetWidth`, `element.offsetHeight` |
| `element.clientLeft`, `element.clientTop`, `element.clientWidth`, `element.clientHeight` |
| `element.scrollWidth`, `element.scrollHeight`, `element.scrollTop`, `element.scrollLeft` |
| `window.getComputedStyle()` |

### 3\. La Regla de Oro para Evitar el Thrashing

El principio profesional para el rendimiento es simple:

> **Agrupar las Lecturas y Agrupar las Escrituras.**

**Nunca** alternes entre Lectura (Read) y Escritura (Write) dentro de un mismo bloque de código, especialmente en un bucle.

#### ❌ Código Malo (Thrashing)

```javascript
// Iteración que causa Layout Thrashing
for (let i = 0; i < 100; i++) {
    // 1. Escritura (Write)
    elementos[i].style.width = '10px'; 
    
    // 2. Lectura (Read): Fuerza al navegador a recalcular el layout para obtener offsetWidth
    const anchoActual = elementos[i].offsetWidth; 
    
    // 3. Escritura (Write)
    elementos[i].style.height = anchoActual + 'px';
} 
```

#### ✅ Código Bueno (Optimizado)

```javascript
// Paso 1: Agrupar todas las Lecturas
const anchos = [];
for (let i = 0; i < 100; i++) {
    anchos.push(elementos[i].offsetWidth); // Lecturas agrupadas
}

// Paso 2: Agrupar todas las Escrituras
for (let i = 0; i < 100; i++) {
    // Escrituras agrupadas (el navegador solo recalcula el layout una vez)
    elementos[i].style.width = '10px';
    elementos[i].style.height = anchos[i] + 'px';
}
```

### 4\. Soluciones Avanzadas (requestAnimationFrame)

Cuando se trabaja con animaciones basadas en JavaScript, debes asegurar que las actualizaciones del DOM se realicen justo antes del siguiente repintado del navegador. La herramienta estándar para esto es **`requestAnimationFrame()`**.

`requestAnimationFrame()` garantiza que tu código de **escritura** se ejecute en el momento más óptimo del ciclo de renderizado del navegador, minimizando la posibilidad de colisionar con operaciones de lectura forzada.

```javascript
function actualizarAnimacion() {
    // 1. Lectura: Obtenemos el estado actual
    const x = elemento.getBoundingClientRect().left;

    // 2. Escritura: Modificamos el DOM
    elemento.style.transform = `translateX(${x + 10}px)`;

    // Llamamos a la siguiente iteración antes del próximo repintado
    requestAnimationFrame(actualizarAnimacion); 
}

requestAnimationFrame(actualizarAnimacion);
```

