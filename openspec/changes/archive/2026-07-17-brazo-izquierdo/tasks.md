## 1. Scaffold proyecto HyperFrames

- [ ] 1.1 Inicializar proyecto HyperFrames con `npx hyperframes init arm_display_left`
- [ ] 1.2 Establecer dimensiones de composición: 1920x1080, background transparente
- [ ] 1.3 Verificar lint y validate pasan en proyecto vacío

## 2. Escena Three.js

- [ ] 2.1 Crear canvas Three.js con alpha:true, setClearColor(0,0,0,0), viewport cámara para 640px/1920
- [ ] 2.2 Modelar base del brazo (torus) y brazo inferior (box) con MeshStandardMaterial blanco/gris mate
- [ ] 2.3 Modelar articulación (sphere) y brazo superior (box)
- [ ] 2.4 Modelar panel frontal (box) con UV mapping para textura de imagen
- [ ] 2.5 Configurar iluminación: DirectionalLight + AmbientLight + HemisphereLight
- [ ] 2.6 Configurar cámara para que el brazo quede centrado en la franja izquierda (0 a 640px X)

## 3. Textura de imagen y fallback

- [ ] 3.1 Implementar TextureLoader con URL desde variable imageUrl
- [ ] 3.2 Crear fallback visual con CanvasTexture con patrón geométrico cuando la URL falla
- [ ] 3.3 Manejar errores CORS y timeout de carga

## 4. Sistema de animación

- [ ] 4.1 Registrar timeline en window.__timelines[compositionId] con paused:true
- [ ] 4.2 Implementar fase de entrada 2.5s: translateY desde abajo + rotateY 90°→0°
- [ ] 4.3 Implementar fase de hold 5s: posición frontal + oscilación idle suave (rotateY ±0.02)
- [ ] 4.4 Implementar fase de salida 2.5s: translateY hacia abajo + rotateY 0°→90°
- [ ] 4.5 Integrar con hf-seek para seekeabilidad determinista

## 5. Variables y personalización

- [ ] 5.1 Declarar data-composition-variables en el HTML raíz
- [ ] 5.2 Implementar variables: imageUrl, entryDuration, holdDuration, exitDuration, idleAmplitude
- [ ] 5.3 Validar que las variables afectan correctamente duraciones y comportamiento

## 6. Validación y render

- [ ] 6.1 Ejecutar `npx hyperframes lint` (0 errores)
- [ ] 6.2 Ejecutar `npx hyperframes validate` (0 errores)
- [ ] 6.3 Ejecutar `npx hyperframes snapshot --at` en hitos clave (entrada, hold, salida)
- [ ] 6.4 Renderizar draft: `npx hyperframes render --quality draft --format webm`
- [ ] 6.5 Verificar transparencia (píxeles alpha cero fuera del brazo)
- [ ] 6.6 Renderizar entrega: `npx hyperframes render --quality high --format webm`
