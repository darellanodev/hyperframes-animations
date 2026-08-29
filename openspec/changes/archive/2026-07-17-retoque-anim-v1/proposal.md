## Why

El brazo robótico renderizado en el cambio anterior tiene dos problemas de composición visual: (1) aparece centrado horizontalmente en lugar de ocupar la franja izquierda (640px), y (2) la altura del brazo excede el campo de visión vertical, cortando el panel superior. Esto impide usar el overlay tal como está en YouTube.

## What Changes

- Ajustar posición horizontal del armGroup de x=0 a x=-3.5 para que ocupe la franja izquierda
- Cambiar cámara: FOV de 28° a 36°, posición z de 7.5 a 9, lookAt de (0,0,0) en lugar de seguir al brazo
- El resto de la composición (geometrías, materiales, animación por fases, textura, variables) no cambia

## Capabilities

### New Capabilities
- *(ninguna)*

### Modified Capabilities
- `robotic-arm-overlay`: se modifican los requisitos de dimensionamiento y posición de cámara para que el brazo quede correctamente en el tercio izquierdo y completamente visible

## Impact

- Solo se modifica `arm_display_left/index.html` (parámetros de cámara y posición del armGroup)
- No afecta otras composiciones ni la estructura del proyecto
