# CFMO – UPCH: Análisis de Bioimágenes
 
Material de clase del módulo de **post-análisis de imágenes** del *Curso de Fundamentos en Microscopía de Fluorescencia y Confocal (CFMO)*, dictado en la Universidad Peruana Cayetano Heredia (UPCH) como parte del módulo práctico organizado por **LABI (Latin American Bioimaging)**.

Link del Canva: https://canva.link/lq0xm50hyxlsrln
 
> Adaptado del material original de **KHIPUX – Quito**, por **Agustín Corbat** ([acorbat.github.io](https://acorbat.github.io)).
 
---
 
## 📋 Contenido de la clase
 
La sesión recorre el flujo completo de análisis de bioimágenes, desde los fundamentos de imagen digital hasta segmentación por deep learning:
 
1. **Fundamentos de imagen digital**
   Qué es una imagen digital, de dónde vienen las imágenes (sensor → fotón → electrón → ADC), tamaño de píxel vs. resolución óptica, bit-depth (1-bit a 32-bit) e histogramas.
2. **Cuantificación básica en Fiji**
   Canales, brillo/contraste (y por qué nunca usar *Apply*), LUTs, representaciones aptas para daltonismo, dimensiones de una imagen (2D–5D) y deconvolución de color en RGB.
3. **Segmentación clásica en Napari**
   Pipeline completo con `napari-assistant` / `pyclesperanto`:
   - **Remove background** — `top_hat_sphere` vs. `top_hat_box`
   - **Filtrado de ruido** — `gaussian_blur` vs. `median_filter`
   - **Binarización** — `threshold_otsu` vs. `threshold_voronoi`
   - **Labeling** — `voronoi_otsu_labeling`, `connected_component_labeling`, `local_minima_seeded_watershed`
   - **Limpieza de máscaras y medición** — `regionprops`, exportación a CSV/Excel
4. **Metadatos y formatos de archivo**
   Metadata experimental/de adquisición/analítica, formatos propietarios (ND2, LIF, CZI...) vs. abiertos (TIFF, HDF5, OME-TIFF, OME-Zarr) y criterios para elegir uno u otro.
5. **Cuantificación de labels**
   Medidas de forma, intensidad, textura y relaciones espaciales entre objetos segmentados.
6. **Segmentación por Machine Learning y Deep Learning**
   Machine learning clásico vs. deep learning, **StarDist** (Schmidt et al., MICCAI 2018) vs. **CellPose** (Stringer et al., Nat. Methods 2021), y el ecosistema de modelos pre-entrenados (bioimage.io, ZeroCostDL4Mic, Ilastik, DeepLabCut).
---
 
## 🎯 Objetivos de aprendizaje
 
Al finalizar la sesión, el estudiante podrá:
 
- Distinguir entre percepción visual subjetiva y cuantificación objetiva de imágenes.
- Ejecutar un flujo de segmentación clásica reproducible en Napari (background removal → filtrado → binarización → labeling → medición).
- Elegir el algoritmo correcto según el tipo de muestra (células aisladas vs. tejido denso; objetos convexos vs. irregulares).
- Guardar y reutilizar workflows como código, para garantizar reproducibilidad.
- Entender cuándo migrar de segmentación clásica a modelos de deep learning (StarDist / CellPose).
---
 
## 🛠️ Herramientas usadas
 
| Herramienta | Uso en la clase |
|---|---|
| [Fiji / ImageJ](https://fiji.sc/) | Cuantificación básica, manejo de canales y LUTs |
| [Napari](https://napari.org/) | Visualización multidimensional y segmentación |
| [napari-assistant](https://github.com/haesleinhuepf/napari-assistant) / [pyclesperanto](https://github.com/clEsperanto/pyclesperanto_prototype) | Pipeline de segmentación clásica GPU-acelerado |
| [StarDist](https://github.com/stardist/stardist) | Segmentación de núcleos por polígonos estrella-convexos |
| [CellPose](https://github.com/MouseLand/cellpose) | Segmentación generalista por flow fields |
| [bioimage.io](https://bioimage.io/) | Repositorio de modelos pre-entrenados |
 
---
 
## 📁 Estructura del repositorio
 
```
├── CFMO_UPCH.pdf        # Slides de la clase
├── workflows/           # Workflows exportados de napari-assistant (Generate code)
└── README.md
```
 
---
 
## 📚 Referencias clave
 
- Schmidt, U., Weigert, M., Broaddus, C., Myers, G. *Cell Detection with Star-Convex Polygons.* MICCAI (2018).
- Stringer, C., Wang, T., Michaelos, M., Pachitariu, M. *Cellpose: a generalist algorithm for cellular segmentation.* Nature Methods 18, 100–106 (2021).
- Haase, R. et al. *A Hitchhiker's guide through the bio-image analysis software universe.* FEBS Letters (2022).
- Haase, R., Royer, L.A., Steinbach, P. et al. *CLIJ: GPU-accelerated image processing for everyone.* Nature Methods 17, 5–6 (2020).
- Swedlow, J. et al. *OME-TIFF / Open Microscopy Environment Data Model.* Genome Biology (2005).
---
 
## 👤 Contacto
 
**Fabricio Chimoy Ayala**
📧 fabricio.chimoy.ayala@gmail.com
💻 [github.com/FabricioNoise-tif](https://github.com/FabricioNoise-tif)
 
**Agustín Corbat** (material original)
📧 acorbat@df.uba.ar
💻 [acorbat.github.io](https://acorbat.github.io) · [@acorbat.bsky.social](https://bsky.app/profile/acorbat.bsky.social)
 
---
 
*Material dictado en el marco de CFMO Montevideo 2026, curso de fundamentos en microscopía óptica organizado por LABI.*
