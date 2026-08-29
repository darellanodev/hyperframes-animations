## Context

La composición `robotic-arm-overlay` renderiza un brazo robótico 3D con Three.js por proyecto (brazo izquierdo y brazo derecho). El panel frontal del brazo usa un `MeshStandardMaterial` con textura. La textura se resuelve con `TextureLoader` a partir de la variable `imageUrl` (default actual `""`), y en caso de error usa un canvas de fallback (512×384).

El usuario colocó `image.png` (cuadrada) en la raíz de `arm_display_left`. El panel actual mide 2.6×1.7, por lo que la imagen cuadrada se estira al mapearse sobre la cara rectangular (las UVs por defecto cubren toda la cara).

## Goals / Non-Goals

**Goals:**
- Panel frontal cuadrado (1.7×1.7) en ambos brazos para que la imagen cuadrada se mapee 1:1 sin distorsión.
- `image.png` sea la imagen por defecto del panel en ambos brazos.
- Copiar `image.png` al proyecto `arm_display_right`.
- Mantener el mecanismo de textura existente (variable `imageUrl` + `TextureLoader` + fallback).

**Non-Goals:**
- No se añade lógica de recorte/ajuste (cover/contain) vía canvas: con panel cuadrado e imagen cuadrada el mapeo 1:1 es suficiente.
- No se cambia la posición, orientación ni animación del brazo.
- No se sustituye el sistema de render (Three.js/WebM VP9 alpha).

## Decisions

1. **Panel cuadrado manteniendo la altura.** Panel `BoxGeometry(1.7, 1.7, 0.05)` y bezel `BoxGeometry(1.82, 1.82, 0.015)` conservando la posición actual. La altura (1.7) es el valor original del eje Y; el ancho se reduce de 2.6 a 1.7 para hacerlo cuadrado. Alternativa descartada: agrandar a 2.6×2.6 — modificaría el eje vertical y el bezel, y la imagen no necesita más tamaño.
2. **Default de `imageUrl` a `image.png`.** Al estar el archivo en la raíz del proyecto (junto a `index.html`), el servidor de preview/render lo sirve por HTTP same-origin (sin CORS) y la carga es determinista. Alternativa descartada: URL remota — requiere red en render y rompe la deterministridad.
3. **Fallback cuadrado 512×512.** Coherente con el panel cuadrado; solo es visible si la imagen falla.
4. **Cambio en la spec `robotic-arm-overlay`** como delta (MODIFIED Requirements) y no edición directa de la spec principal, siguiendo el flujo spec-driven.

## Risks / Trade-offs

- [Visual] El panel pierde ancho (de 2.6 a 1.7), lo que hace el panel más estrecho → Mitigación: la imagen cuadrada llena la cara sin franjas ni distorsión, manteniendo el protagonismo visual.
- [Comportamiento] Si `image.png` no existe en la raíz de algún proyecto, el `TextureLoader` falla y se muestra la textura de fallback → Mitigación: se copia `image.png` a ambos proyectos; el fallback garantiza que el panel nunca quede vacío.
- [Spec] Los valores 2.6×1.7 / 2.72×1.82 aparecen en dos escenarios de la spec principal → Mitigación: el delta re-emite las requirements completas con los nuevos valores.
