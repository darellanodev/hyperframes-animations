## MODIFIED Requirements

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

El brazo SHALL tener un panel frontal cuadrado que muestre una imagen cargada mediante Three.js TextureLoader, con `image.png` como imagen por defecto.

#### Scenario: Carga exitosa de imagen
- **WHEN** se proporciona imageUrl válida
- **THEN** la imagen SHALL aplicarse como textura al panel frontal
- **AND** la textura SHALL mapearse correctamente (aspect ratio preservado o ajustado)
- **AND** una imagen cuadrada (1:1) SHALL mapearse sin distorsión sobre el panel cuadrado

#### Scenario: Fallback de textura
- **WHEN** la URL de imagen falla (error de carga o CORS)
- **THEN** el panel SHALL mostrar un color sólido neutro (#e0e0e0)
- **AND** SHALL dibujar un patrón geométrico decorativo como placeholder

### Requirement: Variables de composición personalizables

La composición SHALL exponer variables para personalización mediante data-composition-variables.

#### Scenario: Variables definidas
- **WHEN** se inspecciona el HTML
- **THEN** SHALL existir variable imageUrl (string, imagen del panel, default "image.png")
- **AND** SHALL existir variable entryDuration (number, segundos para fase de entrada, default 2.5)
- **AND** SHALL existir variable holdDuration (number, segundos para fase hold, default 5.0)
- **AND** SHALL existir variable exitDuration (number, segundos para fase salida, default 2.5)
- **AND** SHALL existir variable idleAmplitude (number, amplitud de oscilación idle, default 0.02)
