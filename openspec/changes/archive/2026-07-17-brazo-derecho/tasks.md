## 1. Inicializar proyecto

- [x] 1.1 Crear proyecto HyperFrames con `npx hyperframes init arm_display_right --example blank`
- [x] 1.2 Verificar lint 0 errores: `npm run check`

## 2. Implementar escena Three.js

- [x] 2.1 Configurar renderizador WebGL con alpha, setClearColor(0,0,0,0), cámara FOV 36° aspect 1920/1080, position (3.2, 0.3, 9), lookAt(0,0,0)
- [x] 2.2 Añadir luces: AmbientLight, DirectionalLight (key+fill+rim), HemisphereLight
- [x] 2.3 Crear geometría del brazo invertido: base arriba (y=+2.8), pilar, brazo inferior, junta, brazo superior, muñeca, soporte
- [x] 2.4 Crear panel frontal BoxGeometry(2.6, 1.7, 0.05) en y=-1.4 y bezel BoxGeometry(2.72, 1.82, 0.015) en y=-1.43
- [x] 2.5 Posicionar armGroup en x=+3.5

## 3. Implementar textura del panel

- [x] 3.1 Crear función createFallbackTexture con canvas 512×384 (grid + círculo + indicador)
- [x] 3.2 Implementar TextureLoader con fallback para imageUrl

## 4. Implementar animación

- [x] 4.1 Implementar easeOutCubic y función renderAt con 3 fases
- [x] 4.2 Entry: y de +8 a +1.0, rot de -PI/2 a 0, easeOutCubic
- [x] 4.3 Hold: y=+1.0, idle sinusoidal (y + idleAmp*sin, z + idleAmp*0.3*sin)
- [x] 4.4 Exit: y de +1.0 a +8, rot de 0 a -PI/2, easeOutCubic

## 5. Implementar sistema de variables

- [x] 5.1 Declarar data-composition-variables: imageUrl, entryDuration(2.5), holdDuration(5), exitDuration(2.5), idleAmplitude(0.02)
- [x] 5.2 Implementar readVar() para lectura de variables desde dataset + defaults

## 6. Verificar

- [x] 6.1 Ejecutar `npm run check` (lint + validate)
- [x] 6.2 Renderizar draft WebM: `npm run render -- --format webm --output out-draft.webm`
