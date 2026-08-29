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

## REMOVED Requirements

*(ninguno)*
