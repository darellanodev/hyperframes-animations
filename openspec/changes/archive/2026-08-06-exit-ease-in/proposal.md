## Why

La fase de salida del brazo usa actualmente desaceleración (ease-out): el brazo arranca la salida a máxima velocidad y frena. El usuario quiere lo contrario, que la salida acelere de menos a más (ease-in), quedando la animación de salida como la imagen especular de la entrada.

## What Changes

- Se añade la función de easing `easeInCubic` (t³) en ambas composiciones.
- La fase de salida pasa a usar aceleración progresiva (ease-in) en lugar de desaceleración (ease-out).
- La fase de entrada y la fase de hold se mantienen sin cambios.
- El delta spec documenta la aceleración progresiva en los escenarios de fase de salida.

## Capabilities

### New Capabilities

(ninguna)

### Modified Capabilities

- `robotic-arm-overlay`: Se modifica el escenario de fase de salida para documentar la aceleración progresiva (ease-in).

## Impact

- `arm_display_left/index.html`
- `arm_display_right/index.html`
- `openspec/changes/exit-ease-in/specs/robotic-arm-overlay/spec.md` (delta spec)
