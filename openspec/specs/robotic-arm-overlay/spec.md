# Robotic Arm Overlay

## Purpose
Superposición de brazo robótico 3D para videos de YouTube, renderizada con Three.js sobre fondo transparente para incrustar en la franja izquierda o derecha del frame.

## Requirements

### Requirement: Composición renderiza brazo robótico 3D con Three.js

El sistema SHALL renderizar un brazo robótico 3D en la franja izquierda (640px) de un canvas de 1920x1080px, usando Three.js con renderer alpha para fondo transparente.

#### Scenario: Canvas transparente
- **WHEN** se inicializa el renderer Three.js
- **THEN** alpha channel SHALL estar habilitado
- **AND** setClearColor SHALL ser (0,0,0,0)

#### Scenario: Dimensiones y posición correctas (brazo izquierdo)
- **WHEN** la composición se renderiza con brazo izquierdo
- **THEN** el canvas SHALL ser 1920x1080
- **AND** el brazo SHALL ocupar la franja izquierda (0 a 640px en X)
- **AND** la cámara SHALL tener FOV 36°
- **AND** la cámara SHALL estar en posición z=9
- **AND** armGroup SHALL estar en x=-3.5
- **AND** la cámara SHALL hacer lookAt (0,0,0)
- **AND** armGroup SHALL estar en y=-1.0 durante la fase de hold

#### Scenario: Dimensiones y posición correctas (brazo derecho)
- **WHEN** la composición se renderiza con brazo derecho
- **THEN** el canvas THREE.js SHALL cubrir 1920×1080
- **AND** armGroup SHALL estar en x=+3.5 (centro visual del tercio derecho)
- **AND** la cámara SHALL tener FOV 36°
- **AND** la cámara SHALL estar en position.z=9
- **AND** la cámara SHALL hacer lookAt (0,0,0)

#### Scenario: Tamaño del panel frontal
- **WHEN** se construye el brazo
- **THEN** el panel frontal SHALL medir 1.7×1.7 unidades 3D (BoxGeometry)
- **AND** el bezel SHALL medir 1.82×1.82 unidades 3D

### Requirement: Brazo con geometrías primitivas estilizadas

El brazo SHALL estar compuesto por geometrías primitivas Three.js (CylinderGeometry, BoxGeometry, SphereGeometry) con Material estándar y luces suaves que generen un aspecto futurista elegante (blanco/gris mate).

#### Scenario: Segmentos del brazo
- **WHEN** se construye el brazo
- **THEN** SHALL tener al menos: base (torus), brazo inferior (box), articulación (sphere), brazo superior (box), panel (box)
- **AND** todos los materiales SHALL usar MeshStandardMaterial o MeshPhysicalMaterial
- **AND** el panel frontal SHALL medir 1.7×1.7 unidades 3D
- **AND** el bezel del panel SHALL medir 1.82×1.82 unidades 3D

#### Scenario: Orientación del brazo
- **WHEN** se inspecciona la escena
- **THEN** el brazo izquierdo SHALL tener la base abajo (y≈-2.8) y el panel arriba (y≈+1.4)
- **AND** el brazo derecho SHALL tener la base arriba (y≈+2.8) y el panel abajo (y≈-1.4)
- **AND** el armGroup del brazo derecho SHALL estar en y≈+1.0 durante la fase de hold

#### Scenario: Iluminación
- **WHEN** la escena se renderiza
- **THEN** SHALL tener al menos una DirectionalLight y una AmbientLight
- **AND** las luces SHALL producir sombras suaves y reflejos tenues

### Requirement: Panel frontal muestra imagen personalizable

El brazo SHALL tener un panel frontal cuadrado que muestre una imagen cargada mediante Three.js TextureLoader, con `image.png` como imagen por defecto. La imagen SHALL mostrarse como una pantalla encendida (material emisivo auto-iluminado) para que se vea tal cual, sin ser sombreada por las luces de la escena.

#### Scenario: Carga exitosa de imagen
- **WHEN** se proporciona imageUrl válida
- **THEN** la imagen SHALL aplicarse como `emissiveMap` al material del panel frontal
- **AND** el material del panel SHALL usar `color` negro, `emissive` blanco y `emissiveIntensity` 1 (auto-iluminado, sin componente difusa)
- **AND** una imagen cuadrada (1:1) SHALL mapearse sin distorsión sobre el panel cuadrado

#### Scenario: La imagen no es sombreada por las luces
- **WHEN** la escena se renderiza con las luces activas
- **THEN** la imagen del panel SHALL verse sin sombreado de las luces ni brillos especulares

#### Scenario: Muestreo de textura nítido
- **WHEN** la textura de la imagen se configura en el loader
- **THEN** el `minFilter` SHALL conservar mipmaps (default `LinearMipmapLinearFilter`, sin forzar `LinearFilter`)
- **AND** la anisotropía SHALL ser la máxima soportada por el renderer
- **AND** el `colorSpace` SHALL ser `SRGBColorSpace`

#### Scenario: Fallback de textura
- **WHEN** la URL de imagen falla (error de carga o CORS)
- **THEN** el panel SHALL mostrar un color sólido neutro (#e0e0e0)
- **AND** SHALL dibujar un patrón geométrico decorativo como placeholder
- **AND** el material del fallback SHALL usar también el esquema emisivo (misma configuración de pantalla auto-iluminada)

### Requirement: Animación por fases con control de tiempo

La composición SHALL ejecutar tres fases de animación secuenciales con duraciones configurables: entrada → hold → salida.

#### Scenario: Duración total
- **WHEN** la composición se renderiza
- **THEN** la duración total SHALL ser la suma de las tres fases
- **AND** el data-duration del root SHALL coincidir con ese total

#### Scenario: Animación determinista
- **WHEN** se reproduce la composición
- **THEN** la animación SHALL ser determinista (sin Math.random, Date.now, ni fuentes no-seckeables)
- **AND** el timeline SHALL registrarse en window.__timelines[compositionId]

#### Scenario: Fase de entrada (brazo izquierdo)
- **WHEN** t=0 hasta t=entryDuration y es brazo izquierdo
- **THEN** el brazo SHALL aparecer desde abajo (translación en Y de -8 a -1.0)
- **AND** SHALL rotar desde +90 grados a 0 grados

#### Scenario: Fase de entrada (brazo derecho)
- **WHEN** t=0 hasta t=entryDuration y es brazo derecho
- **THEN** el brazo SHALL aparecer desde arriba (translación en Y de +8 a +1.0)
- **AND** SHALL rotar desde -90 grados a 0 grados

#### Scenario: Fase de hold con idle
- **WHEN** t=entryDuration hasta t=entryDuration+holdDuration
- **THEN** el brazo SHALL permanecer frente a cámara
- **AND** SHALL tener un movimiento idle suave (oscilación sinusoidal de rotación Y con amplitud idleAmplitude y rotación Z con 0.3×idleAmplitude)

#### Scenario: Fase de salida (brazo izquierdo)
- **WHEN** t=entryDuration+holdDuration hasta t=totalDuration y es brazo izquierdo
- **THEN** el brazo SHALL descender (translación en Y de -1.0 a -8)
- **AND** SHALL rotar de 0 a +90 grados
- **AND** la aceleración SHALL ser progresiva (ease-in): la velocidad SHALL aumentar de lenta a rápida durante toda la fase de salida

#### Scenario: Fase de salida (brazo derecho)
- **WHEN** t=entryDuration+holdDuration hasta t=totalDuration y es brazo derecho
- **THEN** el brazo SHALL ascender (translación en Y de +1.0 a +8)
- **AND** SHALL rotar de 0 a -90 grados
- **AND** la aceleración SHALL ser progresiva (ease-in): la velocidad SHALL aumentar de lenta a rápida durante toda la fase de salida

### Requirement: Variables de composición personalizables

La composición SHALL exponer variables para personalización mediante data-composition-variables.

#### Scenario: Variables definidas
- **WHEN** se inspecciona el HTML
- **THEN** SHALL existir variable imageUrl (string, imagen del panel, default "image.png")
- **AND** SHALL existir variable entryDuration (number, segundos para fase de entrada, default 2.5)
- **AND** SHALL existir variable holdDuration (number, segundos para fase hold, default 5.0)
- **AND** SHALL existir variable exitDuration (number, segundos para fase salida, default 2.5)
- **AND** SHALL existir variable idleAmplitude (number, amplitud de oscilación idle, default 0.02)
