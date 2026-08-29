## MODIFIED Requirements

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
