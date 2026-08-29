## 1. Implementar easing de salida en arm_display_left

- [ ] 1.1 Añadir `function easeInCubic(t){ return t*t*t; }` junto a `easeOutCubic` en `arm_display_left/index.html`
- [ ] 1.2 Cambiar la interpolación de la fase de salida en `arm_display_left/index.html` de `easeOutCubic(localT)` a `easeInCubic(localT)`

## 2. Implementar easing de salida en arm_display_right

- [ ] 2.1 Añadir `function easeInCubic(t){ return t*t*t; }` junto a `easeOutCubic` en `arm_display_right/index.html`
- [ ] 2.2 Cambiar la interpolación de la fase de salida en `arm_display_right/index.html` de `easeOutCubic(localT)` a `easeInCubic(localT)`

## 3. Validar

- [ ] 3.1 Ejecutar `openspec validate --changes` y confirmar que pasa
- [ ] 3.2 Ejecutar `npm run check` en `arm_display_left` (el error de Layout sweep_static es ruido de baseline conocido)
- [ ] 3.3 Ejecutar `npm run check` en `arm_display_right` (el error de Layout sweep_static es ruido de baseline conocido)
- [ ] 3.4 Capturar snapshot en fase de salida (p. ej. --at 8.5) en ambos proyectos y confirmar visualmente con el usuario
