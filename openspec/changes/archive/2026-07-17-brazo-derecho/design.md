## Context

El proyecto arm_display_left ya implementa un overlay de brazo robótico en el tercio izquierdo de 1920×1080 con Three.js + HyperFrames, WebM alpha. Ahora se necesita un proyecto independiente arm_display_right con un brazo simétrico pero invertido verticalmente (base arriba, panel abajo) y animación de entrada desde arriba. No comparten código — cada proyecto es autónomo con su propio index.html, package.json y render.

## Goals / Non-Goals

**Goals:**
- Overlay de brazo robótico 3D en el tercio derecho (columnas 1281-1920 de 1920×1080)
- Fondo transparente (WebM VP9 con alpha)
- Animación 3 fases: entry (2.5s) → hold (5s) → exit (2.5s) = 10s total
- Panel frontal que muestra textura de imagen (configurable por variable)
- Brazo invertido: base arriba, panel abajo
- Entrada desde arriba descendiendo
- Rotación de despliegue en sentido contrario (-90°→0°)
- Mismo sistema de variables que el izquierdo (imageUrl, entryDuration, holdDuration, exitDuration, idleAmplitude)

**Non-Goals:**
- No incluir el brazo izquierdo en este proyecto
- No refactorizar código compartido entre proyectos
- No añadir nuevos tipos de animación más allá de entry/hold/exit
- No ocupar el tercio central

## Decisions

- **Three.js con alpha:true**: Misma configuración que arm_display_left. WebGLRenderer con alpha, setClearColor(0,0,0,0) para fondo transparente.
- **Importmap vía CDN**: Three.js v0.181.2 desde jsdelivr con importmap, misma versión que el proyecto existente.
- **hf-seek como driver de animación**: Sin GSAP timeline (data-no-timeline). La función renderAt(time) consulta window.__hfThreeTime y el evento hf-seek para determinar la animación.
- **Cámara desde la derecha**: camera.position.x positivo (3.2) para que el brazo se vea de frente pero enmarcado en el tercio derecho.
- **armGroup.x=+3.5**: Espejo horizontal del izquierdo (-3.5). Posiciona el brazo en el centro visual del tercio derecho.
- **Base arriba y=+2.8, panel abajo y=-1.4**: Espejo vertical completo del brazo izquierdo.
- **Entry y: +8 → +1.0, rot -PI/2 → 0**: Entra desde arriba desplegándose. La rotación negativa hace que se "abra" hacia el centro de la pantalla.
- **Panel 2.6×1.7, bezel 2.72×1.82**: Mismas medidas duplicadas que el brazo izquierdo actual.
- **easeOutCubic**: Misma función de easing para entrada y salida.

## Risks / Trade-offs

- [Duplicación de boilerplate] Al ser proyectos independientes, el código Three.js (luces, materiales, helpers) se duplica. Aceptado porque la simplicidad y el aislamiento valen más que la reutilización.
- [Mismas coordenadas Y que izquierdo pero invertidas] Si el izquierdo cambia sus coordenadas Y base, el derecho necesitaría ajuste manual. Bajo riesgo porque son proyectos separados.
- [Cámara espejada] La cámara mira a (0,0,0) desde x=+3.2 en vez de x=-3.2. La perspectiva del brazo derecho será simétricamente opuesta a la del izquierdo.
