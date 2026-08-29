## ADDED Requirements

### Requirement: Composición renderiza brazo robótico 3D con Three.js

El sistema SHALL renderizar un brazo robótico 3D en la franja izquierda (640px) de un canvas de 1920x1080px, usando Three.js con renderer alpha para fondo transparente.

#### Scenario: Canvas transparente
- **WHEN** se inicializa el renderer Three.js
- **THEN** alpha channel SHALL estar habilitado
- **AND** setClearColor SHALL ser (0,0,0,0)

#### Scenario: Dimensiones correctas
- **WHEN** la composición se renderiza
- **THEN** el canvas SHALL ser 1920x1080
- **AND** el brazo SHALL ocupar la franja izquierda (0 a 640px en X)

### Requirement: Brazo con geometrías primitivas estilizadas

El brazo SHALL estar compuesto por geometrías primitivas Three.js (CylinderGeometry, BoxGeometry, SphereGeometry) con Material estándar y luces suaves que generen un aspecto futurista elegante (blanco/gris mate).

#### Scenario: Segmentos del brazo
- **WHEN** se construye el brazo
- **THEN** SHALL tener al menos: base (torus), brazo inferior (box), articulación (sphere), brazo superior (box), panel (box)
- **AND** todos los materiales SHALL usar MeshStandardMaterial o MeshPhysicalMaterial

#### Scenario: Iluminación
- **WHEN** la escena se renderiza
- **THEN** SHALL tener al menos una DirectionalLight y una AmbientLight
- **AND** las luces SHALL producir sombras suaves y reflejos tenues

### Requirement: Panel frontal muestra imagen personalizable

El brazo SHALL tener un panel frontal que muestre una imagen cargada por URL mediante Three.js TextureLoader.

#### Scenario: Carga exitosa de imagen
- **WHEN** se proporciona imageUrl válida
- **THEN** la imagen SHALL aplicarse como textura al panel frontal
- **AND** la textura SHALL mapearse correctamente (aspect ratio preservado o ajustado)

#### Scenario: Fallback de textura
- **WHEN** la URL de imagen falla (error de carga o CORS)
- **THEN** el panel SHALL mostrar un color sólido neutro (#e0e0e0)
- **AND** SHALL dibujar un patrón geométrico decorativo como placeholder

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

#### Scenario: Fase de entrada
- **WHEN** t=0 hasta t=entryDuration
- **THEN** el brazo SHALL aparecer desde abajo (translación en Y)
- **AND** SHALL rotar desde 90 grados (de perfil) a 0 grados (frente a cámara)

#### Scenario: Fase de hold con idle
- **WHEN** t=entryDuration hasta t=entryDuration+holdDuration
- **THEN** el brazo SHALL permanecer frente a cámara
- **AND** SHALL tener un movimiento idle suave (oscilación leve de rotación o elevación)

#### Scenario: Fase de salida
- **WHEN** t=entryDuration+holdDuration hasta t=totalDuration
- **THEN** el brazo SHALL descender hacia abajo (translación en Y inversa a entrada)
- **AND** SHALL rotar de vuelta a 90 grados

### Requirement: Variables de composición personalizables

La composición SHALL exponer variables para personalización mediante data-composition-variables.

#### Scenario: Variables definidas
- **WHEN** se inspecciona el HTML
- **THEN** SHALL existir variable imageUrl (string, URL de imagen)
- **AND** SHALL existir variable entryDuration (number, segundos para fase de entrada, default 2.5)
- **AND** SHALL existir variable holdDuration (number, segundos para fase hold, default 5.0)
- **AND** SHALL existir variable exitDuration (number, segundos para fase salida, default 2.5)
- **AND** SHALL existir variable idleAmplitude (number, amplitud de oscilación idle, default 0.02)
