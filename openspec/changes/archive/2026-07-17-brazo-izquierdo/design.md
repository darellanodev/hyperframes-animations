## Context

Overlay de brazo robótico 3D para superponer sobre videos de YouTube (1920x1080). El presentador ocupa el centro; el brazo ocupa el tercio izquierdo (640px) y muestra imágenes didácticas en un panel frontal. Se renderiza con fondo transparente (WebM alpha) para composición en el editor de video.

## Goals / Non-Goals

**Goals:**
- Brazo robótico 3D estilizado con geometrías primitivas (cilindros, cajas, esferas) en estilo futurista elegante
- Panel frontal que muestre una imagen personalizable (cargada por URL)
- Animación: entrada desde abajo con rotación (2.5s) → hold frontal con idle suave (5s) → salida hacia abajo (2.5s)
- Fondo transparente para superposición directa en timeline de edición
- Variables de composición: imageUrl, duration, startDelay
- Composición HyperFrames con canvas Three.js registrado en hf-seek

**Non-Goals:**
- No interacción del usuario (click/hover)
- No audio
- No texto animado ni lower-thirds
- No brazo derecho (reservado para futuro)
- No soporte para múltiples imágenes simultáneas

## Decisions

| Decisión | Opción elegida | Alternativas | Razón |
|----------|---------------|--------------|-------|
| Runtime 3D | Three.js | CSS 3D, SVG | Three.js permite animación suave, iluminación realista, y alpha channel nativo |
| Geometría del brazo | Primitivas (CylinderGeometry, BoxGeometry) | Modelo glTF importado | Sin assets externos; tamaño exacto controlable; carga instantánea |
| Textura del panel | TextureLoader con imagen URL | Canvas 2D draw, video | Simplicidad; permite cualquier imagen remota |
| Animación | Three.js AnimationMixer + custom keyframes | GSAP animando mesh.position | Determinista, seekeable, integrado con hf-seek |
| Salida | Transparente (alpha: true, setClearColor(0,0,0,0)) | Chroma key verde | Sin spill en bordes; composición directa en NLE |
| Formato render | WebM con VP9 | PNG sequence, MOV ProRes | Buen equilibrio calidad/tamaño; alpha nativo; amplio soporte en editores |
| Layout | 640px izquierda de 1920x1080 | 480px, proporcional | División exacta en tercios; espacio suficiente para el brazo + panel |

## Risks / Trade-offs

| Riesgo | Mitigación |
|--------|-----------|
| Imagen no carga (URL rota o CORS) | Fallback a textura de color sólido + patrón geométrico; mostrar warning en consola |
| Three.js no soportado en render headless | HyperFrames usa Chromium headless con WebGL habilitado por defecto |
| Performance con alpha render | WebM VP9 alpha es más lento que opaco; usar draft para iterar, high solo para entrega final |
| Brazo se ve fuera del área izquierda | Camera Three.js configurada para que el brazo quede dentro de 640x1080; bounding box de seguridad |
| CORS bloquea textura de imagen | Documentar que la URL debe permitir CORS; recomendar Imgur, imgbb, o servidor propio |
