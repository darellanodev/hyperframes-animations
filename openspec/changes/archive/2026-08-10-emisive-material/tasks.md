## 1. Brazo derecho (`arm_display_right/index.html`)

- [x] 1.1 En el callback de `TextureLoader.load`, configurar la textura: `colorSpace = THREE.SRGBColorSpace` y `anisotropy = renderer.capabilities.getMaxAnisotropy()`; NO sobrescribir `minFilter` (dejar mipmaps por defecto)
- [x] 1.2 Sustituir el material del panel por el esquema emisivo: `{ color: 0x000000, emissive: 0xffffff, emissiveMap: tex, emissiveIntensity: 1, roughness: 0.2, metalness: 0 }`
- [x] 1.3 Aplicar el mismo esquema emisivo al material del fallback (`fallbackTex`) manteniendo el patrón de placeholder

## 2. Brazo izquierdo (`arm_display_left/index.html`)

- [x] 2.1 En el callback de `TextureLoader.load`, configurar la textura: `colorSpace = THREE.SRGBColorSpace` y `anisotropy = renderer.capabilities.getMaxAnisotropy()`; NO sobrescribir `minFilter` (dejar mipmaps por defecto)
- [x] 2.2 Sustituir el material del panel por el esquema emisivo: `{ color: 0x000000, emissive: 0xffffff, emissiveMap: tex, emissiveIntensity: 1, roughness: 0.2, metalness: 0 }`
- [x] 2.3 Aplicar el mismo esquema emisivo al material del fallback (`fallbackTex`) manteniendo el patrón de placeholder

## 3. Verificación

- [x] 3.1 Ejecutar `npm run check` en `arm_display_right` y `arm_display_left` y corregir errores si aparecen (Lint/Runtime/Motion/Contrast 0/0 en ambos; error de Layout pre-existente no relacionado con el cambio)
- [x] 3.2 Capturar un snapshot del frame de hold y comparar nitidez/fidelidad de color de la imagen del panel contra `image.png` en ambos brazos (verificado: contenido idéntico al origen, nitidez 89-91% de edge energy, brillo autoiluminado)
