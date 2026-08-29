## 1. Ajustar cámara y posición del brazo

- [x] 1.1 Modificar cámara en `arm_display_left/index.html`: FOV 28° → 36°, position.z 7.5 → 9, lookAt armGroup → (0,0,0)
- [x] 1.2 Modificar armGroup.position.x de 0 a -3.5 en `arm_display_left/index.html`

## 2. Validar y renderizar

- [x] 2.1 Ejecutar `npx hyperframes lint` (0 errores esperados)
- [x] 2.2 Ejecutar `npx hyperframes validate` (sin errores de consola)
- [x] 2.3 Renderizar draft para verificar que el brazo aparece en la franja izquierda y completamente visible
