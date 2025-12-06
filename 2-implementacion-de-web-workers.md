# Desarrollo Web de Alto Rendimiento. [[Soporte](https://www.linkedin.com/in/oscarlizarragag/)]
## ⚙️ Implementación de Web Workers

Los **Web Workers** permiten que el código JavaScript se ejecute en un **hilo separado** en segundo plano, **sin bloquear** el hilo principal de la interfaz de usuario (UI). Esto es fundamental para mantener la aplicación web **receptiva** mientras se ejecutan tareas intensivas como cálculos complejos, procesamiento de datos pesados o *fetching* de grandes archivos.

### 1\. El Flujo de Trabajo

La comunicación entre el hilo principal (donde está el DOM y la UI) y el Web Worker se realiza mediante **mensajes** a través de los métodos `postMessage()` y `onmessage`.

  * **Hilo Principal (UI):** Crea el Worker, le envía datos y espera la respuesta.
  * **Worker (Segundo Plano):** Escucha la solicitud, ejecuta la tarea y devuelve el resultado.

### 2\. Ejemplo de Código Práctico

Imagina que necesitas calcular la secuencia de Fibonacci para un número muy grande. Hacer esto en el hilo principal bloquearía la UI por varios segundos.

#### Paso 1: El Archivo del Worker (`fibo_worker.js`)

Este archivo se ejecuta en el hilo secundario. No tiene acceso directo al DOM ni a la ventana (`window`).

```javascript
// fibo_worker.js
// La función recursiva de cálculo (intensiva)
function calcularFibonacci(n) {
    if (n <= 1) return n;
    return calcularFibonacci(n - 1) + calcularFibonacci(n - 2);
}

// Escucha el mensaje enviado por el hilo principal
self.onmessage = function(event) {
    const numero = event.data; // El dato enviado es el número N
    
    // Ejecuta la tarea intensiva
    const resultado = calcularFibonacci(numero);
    
    // Devuelve el resultado al hilo principal
    self.postMessage(resultado);
};
```

#### Paso 2: El Script del Hilo Principal (`main.js`)

Este script se ejecuta en tu página HTML y gestiona la UI.

```javascript
// main.js
const numeroAProcesar = 40; // Un número que tomará tiempo

// 1. Crear una nueva instancia del Web Worker
const fiboWorker = new Worker('fibo_worker.js');

// 2. Escuchar la respuesta del Worker
fiboWorker.onmessage = function(event) {
    const resultado = event.data;
    console.log(`✅ El resultado de Fibonacci(${numeroAProcesar}) es: ${resultado}`);
    
    // Habilitar un botón o actualizar la UI
    document.getElementById('estado').textContent = 'Tarea completada.';
    
    // Importante: Terminar el Worker cuando ya no se necesite
    fiboWorker.terminate();
};

// 3. Manejar errores
fiboWorker.onerror = function(error) {
    console.error('❌ Error en el Web Worker:', error.message);
};

// 4. Enviar los datos para que el Worker comience
fiboWorker.postMessage(numeroAProcesar);

// Mientras el Worker calcula, esta línea se ejecuta inmediatamente (UI no bloqueada)
document.getElementById('estado').textContent = 'Calculando... La UI está libre para interacción.';
```

### 3\. Consideraciones Profesionales

  * **Sin Acceso al DOM:** Recuerda que los Workers no pueden acceder directamente a la interfaz (elementos del DOM o el objeto `window`). Toda interacción debe ser a través de `postMessage()`.
  * **Transferable Objects:** Para enviar grandes cantidades de datos (como `ArrayBuffer`s) de manera más eficiente, utiliza la sobrecarga de `postMessage()` que permite **transferir la propiedad** del objeto. Esto evita la costosa clonación del dato:
    ```javascript
    // Hilo principal
    worker.postMessage(miArrayBuffer, [miArrayBuffer]);
    ```
  * **Shared Workers:** Si necesitas que varios *scripts* (por ejemplo, pestañas de un navegador) compartan el mismo Worker, puedes usar **Shared Workers** en lugar de Workers dedicados.

