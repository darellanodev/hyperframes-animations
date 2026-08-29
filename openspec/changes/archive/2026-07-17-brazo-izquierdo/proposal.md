## Why

Crear un overlay con un brazo robótico 3D (Three.js) con panel de imagen para videos de YouTube, renderizado con fondo transparente para superponer sobre la grabación del presentador. El brazo aparece en la franja izquierda (1/3 del frame) con una imagen didáctica personalizable, haciendo los videos más atractivos visualmente sin distraer del presentador al centro.

## What Changes

- Nueva composición HyperFrames standalone con canvas Three.js para renderizar un brazo robótico 3D
- Modelado del brazo con geometrías primitivas (cilindros, cajas) con estilo futurista elegante
- Panel frontal que muestra una imagen personalizable como textura
- Pipeline de animación: entrada desde abajo (2.5s) → hold frontal con idle suave (5s) → salida hacia abajo (2.5s)
- Soporte de transparencia (WebM con canal alfa) para superposición en editor de video
- Variables de composición para personalizar URL de imagen y duración

## Capabilities

### New Capabilities
- `robotic-arm-overlay`: Composición HyperFrames con brazo robótico 3D y panel de imagen didáctico, renderizada con fondo transparente para superposición en videos de YouTube

### Modified Capabilities
- (ninguna - proyecto nuevo)

## Impact

- Archivos nuevos: `index.html` (composición HyperFrames con Three.js)
- Assets de imagen cargados dinámicamente por URL
- Dependencia: Three.js vía CDN o bundle local
- Output: WebM con canal alfa para edición de video
