## Why

Necesito un overlay de brazo robótico en el tercio derecho de la pantalla para YouTube, simétrico al arm_display_left existente pero invertido verticalmente y con animación de entrada desde arriba. Sirve para composiciones donde el contenido principal está en el centro y se necesita acompañamiento visual en el lado derecho.

## What Changes

- Nuevo proyecto HyperFrames independiente `arm_display_right/` con composición Three.js de brazo robótico
- Ocupa el tercer tercio derecho (columnas 1281-1920 de 1920×1080) con fondo transparente (WebM alpha)
- Brazo con base arriba y panel abajo (invertido respecto al izquierdo)
- Animación: entra desde arriba (y=+8 → y=+1.0) con rotación -90°→0°, hold con idle sinusoidal, sale hacia arriba (y=+1.0 → y=+8) con rotación 0→-90°
- Misma geometría, materiales, luces, panel 2.6×1.7, bezel 2.72×1.82 y sistema de variables que arm_display_left
- Sin dependencia de arm_display_left — proyecto completamente autónomo

## Capabilities

### New Capabilities
- `robotic-arm-overlay`: Brazo robótico 3D en el tercio derecho con Three.js, fondo transparente, panel con textura de imagen, animación entry→hold→exit

### Modified Capabilities


## Impact

- Nuevo proyecto en `arm_display_right/` dentro de `tmp1/`
- Sin cambios en `arm_display_left/` — conviven como proyectos independientes
- Sin cambios en `openspec/specs/robotic-arm-overlay/` (arm_display_right es un proyecto nuevo, no una modificación del spec existente)
