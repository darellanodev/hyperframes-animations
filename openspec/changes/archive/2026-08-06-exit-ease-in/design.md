## Context

Las composiciones `arm_display_left` y `arm_display_right` comparten el mismo código de animación en `renderAt()`: tres fases (entrada, hold, salida) interpoladas con funciones de easing. Actualmente tanto la entrada como la salida usan `easeOutCubic` (arranque rápido, desaceleración al final). El usuario quiere que la salida acelere de menos a más.

## Goals / Non-Goals

**Goals:**
- Cambiar únicamente el easing de la fase de salida a `easeInCubic` (velocidad creciente).
- Mantener la entrada, el hold y las posiciones/distancias exactamente como están.
- Mantener la rotación sincronizada con la traslación en la salida.

**Non-Goals:**
- No modificar la fase de entrada ni el hold.
- No añadir nuevas variables de composición.
- No tocar la duración de las fases (2.5 / 5.0 / 2.5).

## Decisions

1. **`easeInCubic` (`t³`) para la fase de salida.** Su velocidad (3t²) va de 0 a 3, aceleración de menos a más, que es lo solicitado. Se usa `Math.pow(t, 3)` para máxima legibilidad y coherencia con `easeOutCubic` existente (que ya usa `Math.pow`).
   - Alternativa considerada: `easeInQuad` (t²). Descartada porque el acelerado inicial sería más brusco y rompería la simetría visual con la entrada (cúbica).
2. **La rotación reutiliza el mismo `t`.** Como la rotación de salida ya se interpolaba con el mismo progreso que la traslación, reutilizar el valor easeado de `t` mantiene ambas en sincronía sin lógica adicional.
3. **Se define una función separada `easeInCubic`** junto a `easeOutCubic` en lugar de un parámetro, siguiendo el patrón existente del archivo (funciones de easing globales).

## Risks / Trade-offs

- [Visual] La salida arranca muy despacio (velocidad ~0 al inicio), lo que puede percibirse como un retraso antes de irse. → Mitigación: es el comportamiento solicitado; se valida visualmente con snapshot/render antes de confirmar.
- [Coherencia] Si en el futuro se cambia el easing de entrada, la salida quedará asimétrica. → Mitigación: aceptable, es el estado deseado.
