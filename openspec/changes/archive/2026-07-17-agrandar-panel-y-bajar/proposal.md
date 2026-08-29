## Why

El brazo robótico actual queda demasiado alto en el frame — el panel frontal queda cerca del borde superior en vez de centrado verticalmente. Además, el panel es pequeño (0.66×0.42 unidades 3D) y no aprovecha el espacio disponible en la franja izquierda.

## What Changes

- Bajar el brazo robótico ~1 unidad en Y durante la fase de hold (posición centrada verticalmente)
- Agrandar el panel frontal del brazo de 0.66×0.42 a 1.3×0.85 unidades 3D
- Ajustar posición de la geometría del panel para que quede centrado en el brazo inferior
- Ajustar proporciones de la escena para que el panel quede a media altura del frame

## Capabilities

### New Capabilities

Ninguna.

### Modified Capabilities

- `robotic-arm-overlay`: Se modifican dimensiones del panel frontal y posición vertical del brazo en fase de hold. Se renombra y actualiza escenario "Dimensiones y posición correctas" para incluir nueva altura del panel.

## Impact

- `arm_display_left/index.html`: ajustes en geometría Three.js (tamaño panel, posición armGroup en Y)
