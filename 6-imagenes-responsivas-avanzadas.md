# Desarrollo Web de Alto Rendimiento. [[Soporte](https://www.linkedin.com/in/oscarlizarragag/)]

## 🖼️ Imágenes Responsivas Avanzadas: `<picture>` y `srcset`

La optimización de imágenes responsivas se trata de permitir que el **navegador** elija el archivo de imagen más apropiado, en lugar de que tú le digas exactamente cuál usar. Esto asegura una carga más rápida (menor consumo de datos) y mejor renderizado.

### 1\. El Atributo `srcset` para `<img />`

El atributo `srcset` es la técnica más común y sencilla para ofrecer diferentes **resoluciones** de la misma imagen, permitiendo al navegador elegir la mejor opción basada en la densidad de píxeles (DPR) de la pantalla y el ancho de la *viewport*.

#### A. Definición de la Lista de Imágenes

El `srcset` contiene una lista de imágenes candidatas separadas por comas, donde cada una define:

1.  La **URL** del archivo de imagen.
2.  Un **descriptor** de ancho (`w`) o un descriptor de densidad de píxel (`x`).

#### B. Uso con Descriptor de Ancho (`w`)

Este es el método más flexible y se usa junto con el atributo **`sizes`**.

```html
<img 
    srcset="imagen-peque.jpg 480w,
            imagen-media.jpg 800w,
            imagen-grande.jpg 1200w" 
    sizes="(max-width: 600px) 100vw, 
           (max-width: 900px) 50vw,
           33vw"
    src="imagen-media.jpg" 
    alt="Una descripción de la imagen">
```

  * **`srcset`:** Le dice al navegador: "Tengo estas tres imágenes, con anchos nativos de 480, 800 y 1200 píxeles".
  * **`sizes`:** Le dice al navegador: "En viewports pequeñas (hasta 600px) la imagen debe ocupar el 100% del ancho de la vista (`100vw`). En viewports medianas (hasta 900px), el 50%, y en viewports grandes, el 33%".
  * **Decisión del Navegador:** El navegador calcula el ancho de la imagen en CSS (basado en `sizes`), lo compara con las imágenes disponibles en `srcset` y su densidad de píxeles, y elige el archivo más eficiente.

### 2\. El Elemento `<picture>` para Dirección de Arte y Formato

El elemento `<picture>` es un **contenedor** que permite aplicar la **dirección de arte** (cambiar completamente la imagen, no solo la resolución) y manejar diferentes **formatos** (ej. WebP) basado en *media queries*.

#### A. Dirección de Arte (Cambio de Recorte)

Esto es útil si necesitas mostrar una imagen recortada para móviles y una imagen completa para escritorios.

```html
<picture>
    <source media="(max-width: 799px)" srcset="cuerpo-entero-recorte.jpg">
    <source media="(min-width: 800px)" srcset="cuerpo-entero-completa.jpg">
    <img src="cuerpo-entero-completa.jpg" alt="Persona en la escena">
</picture>
```

#### B. Formatos Modernos (WebP, AVIF)

Permite aprovechar formatos de compresión superior como WebP o AVIF, mientras proporciona un *fallback* a formatos más antiguos (como JPEG) para navegadores que no los soportan.

```html
<picture>
    <source type="image/webp" srcset="imagen.webp">
    <source type="image/jpeg" srcset="imagen.jpg">
    <img src="imagen.jpg" alt="Descripción accesible">
</picture>
```

El navegador comprueba las etiquetas `<source>` en orden y utiliza la primera que **soporta** y que coincide con la *media query*. Si ninguna coincide, utiliza el `<img />` final.

### Conclusión para Expertos

La implementación profesional de imágenes no se limita a un simple `<img>`. Se centra en:

1.  **Priorizar `srcset` con `sizes`:** Para resolver el problema de la **densidad de píxeles y el tamaño de la viewport** (el navegador toma la decisión).
2.  **Usar `<picture>`:** Para resolver la **dirección de arte** (cambio de imagen o recorte) y el **soporte de formatos** (tú tomas la decisión basada en reglas).

