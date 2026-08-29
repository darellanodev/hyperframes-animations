## MODIFIED Requirements

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
