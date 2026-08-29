## ADDED Requirements

*(ninguno)*

## MODIFIED Requirements

### Requirement: Composición renderiza brazo robótico 3D con Three.js

El sistema SHALL renderizar un brazo robótico 3D en la franja izquierda (640px) de un canvas de 1920x1080px, usando Three.js con renderer alpha para fondo transparente.

#### Scenario: Canvas transparente
- **WHEN** se inicializa el renderer Three.js
- **THEN** alpha channel SHALL estar habilitado
- **AND** setClearColor SHALL ser (0,0,0,0)

#### Scenario: Dimensiones y posición correctas
- **WHEN** la composición se renderiza
- **THEN** el canvas SHALL ser 1920x1080
- **AND** el brazo SHALL ocupar la franja izquierda (0 a 640px en X)
- **AND** la cámara SHALL tener FOV 36°
- **AND** la cámara SHALL estar en posición z=9
- **AND** armGroup SHALL estar en x=-3.5
- **AND** la cámara SHALL hacer lookAt (0,0,0)
- **AND** armGroup SHALL estar en y=-1.0 durante la fase de hold

#### Scenario: Tamaño del panel frontal
- **WHEN** se construye el brazo
- **THEN** el panel frontal SHALL medir 1.3×0.85 unidades 3D (BoxGeometry)
- **AND** el bezel SHALL medir 1.36×0.91 unidades 3D

### Requirement: Brazo con geometrías primitivas estilizadas

El brazo SHALL estar compuesto por geometrías primitivas Three.js (CylinderGeometry, BoxGeometry, SphereGeometry) con Material estándar y luces suaves que generen un aspecto futurista elegante (blanco/gris mate).

#### Scenario: Segmentos del brazo
- **WHEN** se construye el brazo
- **THEN** SHALL tener al menos: base (torus), brazo inferior (box), articulación (sphere), brazo superior (box), panel (box)
- **AND** todos los materiales SHALL usar MeshStandardMaterial o MeshPhysicalMaterial
- **AND** el panel frontal SHALL medir 1.3×0.85 unidades 3D

#### Scenario: Iluminación
- **WHEN** la escena se renderiza
- **THEN** SHALL tener al menos una DirectionalLight y una AmbientLight
- **AND** las luces SHALL producir sombras suaves y reflejos tenues

## REMOVED Requirements

*(ninguno)*
