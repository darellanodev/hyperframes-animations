## 1. Preparar la imagen

- [ ] 1.1 Copiar `arm_display_left/image.png` → `arm_display_right/image.png`

## 2. Panel cuadrado en brazo izquierdo

- [ ] 2.1 Cambiar en `arm_display_left/index.html` la geometría del panel: `BoxGeometry(2.6, 1.7, 0.05)` → `BoxGeometry(1.7, 1.7, 0.05)`
- [ ] 2.2 Cambiar en `arm_display_left/index.html` la geometría del bezel: `BoxGeometry(2.72, 1.82, 0.015)` → `BoxGeometry(1.82, 1.82, 0.015)`
- [ ] 2.3 Cambiar en `arm_display_left/index.html` el default de `imageUrl` en `data-composition-variables`: `""` → `"image.png"`
- [ ] 2.4 Cambiar en `arm_display_left/index.html` el canvas de fallback de 512×384 a 512×512

## 3. Panel cuadrado en brazo derecho

- [ ] 3.1 Cambiar en `arm_display_right/index.html` la geometría del panel: `BoxGeometry(2.6, 1.7, 0.05)` → `BoxGeometry(1.7, 1.7, 0.05)`
- [ ] 3.2 Cambiar en `arm_display_right/index.html` la geometría del bezel: `BoxGeometry(2.72, 1.82, 0.015)` → `BoxGeometry(1.82, 1.82, 0.015)`
- [ ] 3.3 Cambiar en `arm_display_right/index.html` el default de `imageUrl` en `data-composition-variables`: `""` → `"image.png"`
- [ ] 3.4 Cambiar en `arm_display_right/index.html` el canvas de fallback de 512×384 a 512×512

## 4. Validar y renderizar

- [ ] 4.1 Ejecutar `openspec lint`
- [ ] 4.2 Ejecutar `npm run check` en `arm_display_left`
- [ ] 4.3 Ejecutar `npm run check` en `arm_display_right`
- [ ] 4.4 Ejecutar `npx hyperframes snapshot --at 3.5` en ambos proyectos y verificar visualmente que la imagen cuadrada llena el panel
- [ ] 4.5 Renderizar borrador y confirmar visualmente con el usuario
