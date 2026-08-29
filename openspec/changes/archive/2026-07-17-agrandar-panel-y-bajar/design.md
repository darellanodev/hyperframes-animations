## Context

El brazo robótico actual se renderiza centrado verticalmente (armGroup.position.y=0 en hold), con panel frontal de 0.9×0.6 unidades ubicado en y=1.7 relativo al armGroup. Esto deja el panel cerca del borde superior del frame. Se necesita bajar el brazo y agrandar el panel para que quede a media altura y ocupe más espacio visual.

## Goals / Non-Goals

**Goals:**
- Panel centrado verticalmente en el frame durante la fase de hold
- Panel ~2× más grande (de 0.9×0.6 a 1.3×0.85 unidades)
- Brazo completo baja ~1 unidad en Y

**Non-Goals:**
- No cambiar FOV, posición Z de cámara, ni lookAt
- No modificar fases de animación ni timings
- No alterar otros segmentos del brazo (base, lowerArm, upperArm, joint)
- No cambiar iluminación ni materiales

## Decisions

1. **armGroup.position.y en hold: -1.0** — Baja el brazo completo 1 unidad. El panel pasa de y_abs=1.7 a y_abs=0.7, quedando ligeramente sobre el centro del frame (~0.7 unidades arriba del centro en FOV 36°, z=9). Esto logra el efecto visual de "panel a media altura".

2. **panel 1.3×0.85 (de 0.9×0.6)** — Incremento de ~2× en área. El panel ahora domina visualmente el brazo inferior. Bezel se escala proporcionalmente a 1.36×0.91.

3. **panel.position.y ajustado a 1.4** — Con el panel más grande, se baja ligeramente su posición relativa al armGroup (de 1.7 a 1.4) para que quede centrado verticalmente en el brazo inferior, manteniendo coherencia estética.

4. **Canvas de fallback texture se actualiza a 512×384** — El aspect ratio 4:3 se mantiene, solo se incrementa resolución para textura más grande.

## Risks / Trade-offs

- [Visual] Panel más grande podría obstruir otros elementos del brazo → mitigado manteniendo z-index visual (panel al frente del brazo, como está diseñado).
- [Layout] Al bajar el brazo, la base/torus podría quedar parcialmente fuera del frame inferior → mitigado: la base está en y=-1.6 relativo, con armGroup.y=-1.0 queda en y_abs=-2.6, aún visible con FOV 36° (visible range ~±2.9).
