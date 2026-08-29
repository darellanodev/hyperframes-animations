## Context

El proyecto consta de dos composiciones HyperFrames casi idénticas y espejadas (`arm_display_right/index.html`, `arm_display_left/index.html`), que renderizan un brazo robótico Three.js sobre fondo transparente (1920x1080). El panel frontal es un `BoxGeometry(1.7, 1.7, 0.05)` cuadrado que muestra `image.png` (imagen cuadrada 1:1, resolución variable: derecha 500x500, izquierda 1080x1080).

Estado actual (callback de `TextureLoader`, ~líneas 198-209 de cada archivo):

```js
tex.minFilter = THREE.LinearFilter;   // desactiva mipmaps
panel.material = new THREE.MeshStandardMaterial({ map: tex, roughness: 0.1, metalness: 0.2 });
```

Problemas:
1. El material con `map` es iluminado por las luces de la escena → la imagen se ve apagada/opaca (efecto decal) y el metalness 0.2 genera brillos especulares.
2. `minFilter = LinearFilter` elimina los mipmaps → aliasing/borrosidad al miniificar (el panel se ve en ángulo y con wobble idle).
3. Sin anisotropía (default 1) → pérdida de nitidez en superficies oblicuas.
4. Sin `colorSpace` (default `NoColorSpace` en three r152+) → colores apagados respecto a la imagen original.

## Goals / Non-Goals

**Goals:**
- La imagen del panel se muestra como una pantalla encendida: auto-iluminada, tal cual, sin sombras ni especulares de las luces de la escena.
- Nitidez: reproducción correcta de colores (sRGB), mipmaps activos y anisotropía máxima para vistas en ángulo.
- Consistencia entre ambos brazos y entre el caso de imagen cargada y el fallback.
- Mantener determinismo y la resolución/geometría actuales (la imagen siempre es cuadrada y se usa sin recortes ni reescalados).

**Non-Goals:**
- No se cambia la geometría, animación, variables de composición, resolución del render ni la fuente de imagen.
- No se introducen post-procesos (bloom/glow), tone mapping, ni nuevas dependencias.
- No se mejora la nitidez durante las fases de entrada/salida (panel casi de canto por perspectiva).

## Decisions

### D1. Material emisivo para el panel

Usar `MeshStandardMaterial` con el mapa como `emissiveMap`:

```js
new THREE.MeshStandardMaterial({
  color: 0x000000,
  emissive: 0xffffff,
  emissiveMap: tex,
  emissiveIntensity: 1,
  roughness: 0.2,
  metalness: 0
})
```

- `color` negro → sin componente difusa; la imagen sale solo por emisión → no la sombrean las luces.
- `metalness: 0` → sin brillos especulares sobre la imagen.
- **Alternativa considerada:** `MeshBasicMaterial` con `map` (también sin luces). Se descarta porque `emissiveMap`/`emissiveIntensity` dan control explícito del brillo "pantalla" y porque la especificación existente exige materiales estándar (`MeshStandardMaterial`/`MeshPhysicalMaterial`) para todos los segmentos.

### D2. Muestreo de textura correcto

En el callback del loader, antes de asignar el material:

```js
tex.colorSpace = THREE.SRGBColorSpace;                       // colores reales
tex.anisotropy = renderer.capabilities.getMaxAnisotropy();   // vistas en ángulo
// NO se asigna minFilter: queda el default LinearMipmapLinearFilter
```

- Mantener los mipmaps (no asignar `LinearFilter`) es clave para miniificar sin aliasing. three r181 usa WebGL2, que soporta mipmaps incluso en texturas no potencia de dos (500x500, 1080x1080).
- Anisotropía máxima soportada para eliminar el desenfoque al ver el panel inclinado (wobble idle + ángulo de cámara).
- `SRGBColorSpace` decodifica el mapa en el espacio correcto tanto para `map` como para `emissiveMap`.

### D3. Fallback también emisivo

El material del fallback (canvas de 512x512, `createFallbackTexture`) usa la misma configuración emisiva con `fallbackTex`, de modo que el panel mantenga el mismo aspecto "pantalla" si la imagen no carga.

### D4. Aplicar a ambos brazos

`arm_display_left/index.html` es un espejo de `arm_display_right/index.html` (cámara en x=-3.2, luces y base invertidas, etc.), pero el código del panel es idéntico. El cambio se aplica a los dos archivos con el mismo fragmento.

## Risks / Trade-offs

- [El panel emisivo puede verse más brillante que el resto de la escena] → Es el comportamiento buscado ("pantalla encendida"). Si se prefiere más sutil, `emissiveIntensity` se puede bajar (p. ej. 0.85) sin tocar nada más.
- [Los colores pueden verse distintos si la imagen fuente no es sRGB] → Las imágenes de usuario son sRGB por convención; `SRGBColorSpace` es el comportamiento correcto. Si hubiera casos lineales, se ajustaría puntualmente.
- [No mejora la nitidez en entrada/salida cuando el panel queda de canto] → Limitación de perspectiva, aceptada (no-goal).
- [Cambio estético no previsto al quitar el sombreado de luces sobre la imagen] → Mitigación: `emissiveIntensity: 1` reproduce la imagen prácticamente 1:1; revisar un snapshot en el hold antes de render final.

## Migration Plan

- Editar los dos `index.html` (callback del loader y bloque del fallback).
- Ejecutar `npm run check` en cada carpeta del brazo.
- Renderizar y comparar snapshots del hold contra `image.png` para validar nitidez y fidelidad de color.
- Rollback: revertir los dos archivos (cambio pequeño y localizado).
