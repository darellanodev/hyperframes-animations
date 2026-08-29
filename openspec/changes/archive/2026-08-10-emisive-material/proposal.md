## Why

En el render final, la imagen del panel del brazo robótico se ve opaca y sin nitidez, como un "decal" impreso en vez de una pantalla encendida. La causa es que la textura se aplica como `map` en un `MeshStandardMaterial` con metalness, de modo que las luces de la escena la atenúan y generan brillos especulares; además el muestreo de textura actual (sin mipmaps, sin anisotropía, sin `colorSpace` sRGB) degrada la nitidez cuando el panel está en ángulo.

## What Changes

- El panel frontal SHALL mostrar la imagen como **material emisivo auto-iluminado** (`color` negro, `emissive` blanco, `emissiveMap`, `emissiveIntensity: 1`), de modo que la imagen se vea tal cual, como una pantalla encendida, sin ser sombreada por las luces de la escena.
- El muestreo de textura SHALL conservar mipmaps (no forzar `LinearFilter`), habilitar anisotropía máxima soportada por el renderer y fijar `colorSpace = SRGBColorSpace` para reproducir los colores reales de la imagen.
- El material de respaldo (fallback, cuando falla la carga de imagen) SHALL usar también el mismo esquema emisivo para mantener consistencia visual.
- El cambio se aplica a ambos brazos (`arm_display_right` y `arm_display_left`), que son composiciones espejo.
- No hay cambios de comportamiento en la animación, las variables de composición ni la resolución de la imagen. La imagen se usa tal cual (siempre cuadrada 1:1, el panel también es cuadrado), sin redimensionar ni recortar.

## Capabilities

### New Capabilities

_Ninguna._

### Modified Capabilities

- `robotic-arm-overlay`: la forma en que el panel frontal muestra la imagen cambia de textura iluminada (`map` sombreado por luces) a pantalla emisiva auto-iluminada, incluyendo el caso de fallback y los parámetros de muestreo de textura.

## Impact

- Código afectado: `arm_display_right/index.html` y `arm_display_left/index.html` (callback de `TextureLoader` y creación del material del panel, incluyendo el fallback).
- Sin cambios en: API, dependencias (sigue usando `three@0.181.2` desde CDN), geometrías, animación, variables de composición, ni imágenes fuente.
- La especificación `robotic-arm-overlay` se modifica en el requisito "Panel frontal muestra imagen personalizable".
