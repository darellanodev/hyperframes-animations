## ADDED Requirements

### Requirement: Composición renderiza brazo robótico 3D con Three.js
La composición SHALL renderizar un brazo robótico 3D con Three.js en el tercio derecho de un frame 1920×1080, con fondo transparente.

#### Scenario: Dimensiones y posición correctas
- **WHEN** se renderiza el frame inicial
- **THEN** el canvas Three.js SHALL cubrir 1920×1080
- **THEN** el armGroup SHALL estar en x=+3.5 (centro visual del tercio derecho)
- **THEN** la cámara SHALL tener FOV 36°, position.z=9, lookAt (0,0,0)

#### Scenario: Renderizado con alpha
- **WHEN** se renderiza cualquier frame
- **THEN** el fondo SHALL ser transparente (WebGLRenderer con alpha:true, setClearColor(0,0,0,0))
- **THEN** el formato de salida SHALL ser WebM VP9 con canal alpha

#### Scenario: Iluminación correcta
- **WHEN** se renderiza cualquier frame
- **THEN** la escena SHALL tener DirectionalLight, AmbientLight, HemisphereLight y rim light
- **THEN** las geometrías SHALL verse iluminadas con estilo metálico/brillante

### Requirement: Brazo con geometrías primitivas estilizadas
El brazo SHALL construirse con geometrías primitivas de Three.js formando un brazo robótico estilizado (futurista elegante).

#### Scenario: Segmentos del brazo
- **WHEN** se inspecciona la escena
- **THEN** SHALL contener: base (torus + cilindro), pilar (cilindro), brazo inferior (box), junta esférica (esfera + torus ring), brazo superior (box), muñeca (esfera), soporte (cilindro), panel frontal (box con textura)
- **THEN** el panel frontal SHALL medir 2.6×1.7 unidades 3D
- **THEN** el bezel del panel SHALL medir 2.72×1.82 unidades 3D
- **THEN** los materiales SHALL usar MeshStandardMaterial con colores blanco/gris/azul, roughness 0.05-0.5, metalness 0.15-0.9

#### Scenario: Orientación invertida
- **WHEN** se inspecciona el brazo en fase hold
- **THEN** la base SHALL estar arriba (y≈+2.8)
- **THEN** el panel frontal SHALL estar abajo (y≈-1.4)
- **THEN** el armGroup SHALL estar en y≈+1.0

### Requirement: Panel con textura de imagen
El panel frontal SHALL mostrar una textura configurable por variable, con fallback generado proceduralmente.

#### Scenario: Carga de textura personalizada
- **WHEN** se proporciona imageUrl válido
- **THEN** el panel SHALL cargar la imagen como textura con TextureLoader
- **THEN** el material del panel SHALL usar map con la textura cargada (roughness 0.1, metalness 0.2)

#### Scenario: Fallback sin imagen
- **WHEN** no se proporciona imageUrl
- **THEN** el panel SHALL mostrar una textura procedural con grid + círculo + indicador

#### Scenario: Fallback en error de carga
- **WHEN** imageUrl falla al cargar
- **THEN** SHALL usar la textura de fallback procedural
- **THEN** SHALL emitir warning en consola

### Requirement: Animación en tres fases
El brazo SHALL animarse en 3 fases secuenciales: entry (entrada desde arriba), hold (idle), exit (salida hacia arriba).

#### Scenario: Fase de entrada (entry)
- **WHEN** time < entryDuration
- **THEN** armGroup.position.y SHALL transicionar de +8 a +1.0 con easeOutCubic
- **THEN** armGroup.rotation.y SHALL transicionar de -PI/2 a 0

#### Scenario: Fase de hold
- **WHEN** time entre entryDuration y entryDuration+holdDuration
- **THEN** armGroup.position.y SHALL mantenerse en +1.0
- **THEN** armGroup.rotation.y SHALL oscilar sinusoidalmente con amplitud idleAmplitude
- **THEN** armGroup.rotation.z SHALL oscilar sinusoidalmente con amplitud idleAmplitude*0.3 y frecuencia 0.25

#### Scenario: Fase de salida (exit)
- **WHEN** time > entryDuration+holdDuration
- **THEN** armGroup.position.y SHALL transicionar de +1.0 a +8 con easeOutCubic
- **THEN** armGroup.rotation.y SHALL transicionar de 0 a -PI/2

### Requirement: Sistema de variables
La composición SHALL soportar las siguientes variables configurables.

#### Scenario: Variables definidas
- **WHEN** se inspecciona la composición
- **THEN** SHALL existir variable imageUrl (string, default "")
- **THEN** SHALL existir variable entryDuration (number, default 2.5)
- **THEN** SHALL existir variable holdDuration (number, default 5)
- **THEN** SHALL existir variable exitDuration (number, default 2.5)
- **THEN** SHALL existir variable idleAmplitude (number, default 0.02)
