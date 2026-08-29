## Why

La imagen `image.png` (cuadrada) colocada en el ejemplo del brazo izquierdo no se muestra correctamente porque el panel frontal actual es rectangular (2.6×1.7). Al mapear una imagen cuadrada sobre una cara rectangular se produce distorsión. Se requiere un panel cuadrado que coincida con las proporciones de la imagen, además de que `image.png` sea la imagen por defecto en ambos brazos.

## What Changes

- El panel frontal del brazo pasa de rectangular a cuadrado: `BoxGeometry(2.6, 1.7, 0.05)` → `BoxGeometry(1.7, 1.7, 0.05)` en ambos brazos.
- El bezel del panel pasa a cuadrado: `BoxGeometry(2.72, 1.82, 0.015)` → `BoxGeometry(1.82, 1.82, 0.015)` en ambos brazos.
- La variable `imageUrl` cambia su valor por defecto de `""` a `image.png`, de modo que la imagen local se muestra por defecto en el panel.
- La imagen `image.png` se copia al proyecto `arm_display_right` para que ambos brazos la usen.
- La textura fallback (canvas) pasa a ser cuadrada (512×512) para coherencia con el panel cuadrado.

## Capabilities

### New Capabilities

- Ninguna: el change no introduce capacidades nuevas.

### Modified Capabilities

- `robotic-arm-overlay`: Se modifican las dimensiones del panel frontal y del bezel (paso de rectangular a cuadrado), se ajusta el mapeo de la imagen cuadrada y el valor por defecto de la variable `imageUrl`.

## Impact

- `arm_display_left/index.html`: geometrías del panel/bezel y default de `imageUrl`.
- `arm_display_right/index.html`: geometrías del panel/bezel y default de `imageUrl`.
- `arm_display_right/image.png`: nuevo archivo (copia de `arm_display_left/image.png`).
- `openspec/specs/robotic-arm-overlay/spec.md`: requisito de tamaño del panel frontal y default de `imageUrl`.
