## Context

El cambio anterior (arm_display_left) implementó un overlay de brazo robótico 3D con Three.js. La revisión del render reveló dos problemas de composición: (1) el brazo aparece centrado en lugar de ocupar la franja izquierda (640px), porque la cámara hace lookAt al armGroup que está en x=0; (2) el brazo es demasiado alto (4.92 unidades) para el FOV de 28° a z=7.5 (altura visible ~3.74 unidades), cortando el panel superior.

## Goals / Non-Goals

**Goals:**
- Reposicionar el brazo en la franja izquierda del canvas 1920x1080
- Asegurar que el brazo completo (incluyendo panel superior) sea visible
- Mantener la misma estructura de animación, geometrías, texturas y variables

**Non-Goals:**
- No se modifican las geometrías del brazo ni sus materiales
- No se cambian las fases de animación (entrada/hold/salida) ni las variables
- No se introducen nuevas funcionalidades

## Decisions

### Decisión 1: Camera FOV y posición
- **Opción elegida**: FOV 36°, posición z=9, lookAt (0,0,0)
- **Alternativa considerada**: Mantener FOV 28° y alejar cámara a z=11
- **Por qué**: FOV 36° proporciona una altura visible de ~5.84 unidades a z=9, suficiente para el brazo de 4.92 unidades con margen. La alternativa (z=11 con FOV 28°) daba altura visible ~5.48 unidades pero aleja demasiado la cámara, reduciendo el detalle visual del brazo.

### Decisión 2: Posición horizontal del brazo
- **Opción elegida**: armGroup.position.x = -3.5, cámara lookAt (0,0,0)
- **Alternativa considerada**: Mover cámara a x=-3.5 y hacer lookAt al brazo
- **Por qué**: Con cámara centrada y brazo desplazado, la proyección perspectiva es más natural. Mover la cámara lateralmente introduce distorsión de perspectiva no deseada en los bordes.

### Decisión 3: Margen de clip vertical
- Se añade margen de ~0.3 unidades arriba y abajo respecto a la altura total del brazo
- El panel superior del brazo termina en y≈2.0; la altura visible con FOV 36° a z=9 es ~5.84 unidades (±2.92 desde centro)

## Risks / Trade-offs

- **[Rendimiento]** El cambio de FOV y posición de cámara no afecta el costo de renderizado (misma geometría, mismos pases de luz)
- **[Regresión]** Si en el futuro se agregan segmentos al brazo que aumenten su altura total, habrá que recalcular FOV/distancia de cámara
