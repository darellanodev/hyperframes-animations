## 1. Ajustar tamaño del panel y bezel

- [x] 1.1 Cambiar BoxGeometry del panel de 0.9×0.6 a 1.3×0.85 en `arm_display_left/index.html`
- [x] 1.2 Cambiar BoxGeometry del bezel de 0.96×0.66 a 1.36×0.91 en `arm_display_left/index.html`

## 2. Bajar el brazo en fase de hold

- [x] 2.1 Cambiar armGroup.position.y durante hold de 0 a -1.0
- [x] 2.2 Ajustar panel.position.y de 1.7 a 1.4 y bezel.position.y de 1.7 a 1.4

## 3. Validar y renderizar

- [x] 3.1 Ejecutar `openspec lint && npx hyperframes lint`
- [x] 3.2 Ejecutar `npx hyperframes validate`
- [x] 3.3 Renderizar draft y verificar panel centrado verticalmente
