# Curso de Fundamentos de Microscopia de Fluoresencia y Confocal UPCH

Bienvenidos al taller de análisis de bioimagenes de la UPCH. Estas 4 horas prácticas te introducen a la cuantificación, segmentación y análisis usan Napari, Napari-assistant con pyclesperanto y herramientas de análisis de imagenes mediante python.

## Partes del taller

Este taller estará dividido en 2 partes (1 teórica y 1 práctica), comenzando con la parte teórica:

## Módulo 1: Fundamentos teóricos (60 min · 0:00–1:00 h)

- **Recurso:** ejecutar napari siguiendo la instrucción de [instalación de Napari](./installation_napari.md)
 
- **Lectura/discusión** (20 min): ¿Qué es el análisis de bioimagen cuantitativo y por qué reemplaza el conteo manual en publicaciones actuales?
- **Tipos de microscopía en esta clase** (15 min): IHC con cromógeno DAB, cámara Neubauer con exclusión trypan blue, confocal z-stack
- **Napari como entorno de trabajo** (15 min): layers (imagen, etiquetas, shapes, puntos), plugins, flujo GUI + consola, escala px/µm
- **Conceptos clave** (10 min): deconvolución de color, umbralización, segmentación, cuantificación, validación contra referencia manual

---
 
*— Pausa 10 min —*
 
---

## Módulo 2: Microglía — IHC / DAB (50 min · 1:10–2:00 h)
 
### Segmentación (30 min)
 
- **Carga de imagen** (5 min): abrir archivo CZI con `bioio` / `napari-aicsimageio`; verificar escala (`layer.scale`)
- **ROI manual** (5 min): dibujar región de interés con el Shapes layer; convertir a máscara binaria desde consola
- **Deconvolución de color** (10 min): separar canal DAB con `skimage.color.separate_stains` + `hdx_from_rgb`; mostrar histograma antes/después
- **Umbralización y filtrado** (10 min): Otsu dentro del ROI → filtro de tamaño → etiquetado de componentes conexos (`skimage.measure.label`)

### Análisis y validación (20 min)
 
- **Métricas por célula** (10 min): exportar área, intensidad media y centroide con `napari-skimage-regionprops`
- **Validación** (10 min): comparar conteo automático vs. referencia manual con scatter plot; discutir fuentes de error
- **Cerrar imagen** antes de pasar al siguiente módulo para liberar RAM

---
 
*— Pausa 10 min —*
 
---
 
## Módulo 3: PBMC — Cámara Neubauer (40 min · 2:10–2:50 h)
 
### Segmentación (25 min)
 
- **Carga de imagen** (5 min): abrir imagen de Neubauer; ajustar contraste y escala
- **Preprocesamiento** (10 min): `top_hat_sphere` para suprimir fondo variable → `median_sphere` para suavizar ruido
- **Detección de células** (10 min): `voronoi_otsu_labeling` aplicado a la **imagen original** (no preprocesada) — el preprocesamiento solo mejora detección de máximos locales; aplicar máscara manual de cuadrantes → `exclude_labels_outside_size_range`

> **Nota conceptual importante:** las células muertas teñidas con trypan blue son **mínimos** locales de intensidad, no máximos. `voronoi_otsu` detecta máximos → invierte la imagen si necesitas clasificar células muertas por separado.
 
### Análisis y validación (15 min)
 
- **Viabilidad** (8 min): `mean_intensity_map` para clasificar células por intensidad (vivas = alta intensidad, muertas = baja intensidad si trypan blue visible)
- **Validación** (7 min): conteo automático vs. manual; discutir distribución unimodal si viabilidad >95 %
- **Cerrar imagen**

---

## Módulo 4: Giro dentado + introducción 3D (30 min · 2:50–3:20 h)
 
### Segmentación (20 min)
 
- **Giro dentado** (12 min): ROI manual sobre la región del hipocampo → deconvolución DAB → StarDist (`2D_versatile_he`) como alternativa a Otsu para morfología neuronal compleja
- **Demo 3D** (8 min): cargar z-stack con `bioio` → navegar slices → proyección MIP (`np.max(stack, axis=0)`) → mencionar que el mismo pipeline 2D se puede extender con `voronoi_otsu_labeling` en 3D

### Análisis (10 min)
 
- Exportar métricas de neuronas con `regionprops_table`
- Comparar conteo StarDist vs. manual; discutir ventajas sobre umbralización simple
- **Cerrar imágenes**
---
 
## Módulo 5: Buenas prácticas — guardar .tif para publicación (20 min · 3:40–4:00 h)
 
- **Metadata esencial** (8 min): resolución (µm/px), canal, bit depth, sistema de coordenadas — todo debe quedar en el archivo
- **Guardar con tifffile** (7 min): diferencia entre TIFF plano y OME-TIFF; cómo preservar metadata del microscopio

---
 
## Resumen de tiempos
 
| Módulo | Contenido | Tiempo |
|--------|-----------|--------|
| 1 | Fundamentos teóricos | 60 min |
| — | Pausa | 10 min |
| 2 | Microglía: segmentación + análisis | 50 min |
| — | Pausa | 10 min |
| 3 | PBMC: segmentación + análisis | 40 min |
| 4 | Giro dentado + demo 3D + análisis | 30 min |
| 5 | Buenas prácticas .tif + cierre | 20 min |
| **Total** | | **4:00 h** |

---

## Lecture Materials

Download the presentation slides and additional resources from **[Zenodo](https://zenodo.org/records/18717816)**.

## Sample Data

Download example images from Zenodo:
- **3D image**: `Lund.tif` - Full 3D stack for this workshop

https://zenodo.org/records/17986091

---

### Bad segmentation
- Try different preprocessing methods:
  - Use different thresholding methods (Otsu, Li, Isodata)
  - Adjust erosion/dilation parameters
  - Increase/decrease White Top Hat radius
  - Apply Gaussian smoothing before thresholding (available in assistant)

### Memory issues with large stacks
- Open only the region of interest (ROI)
- Downsampling spatial dimensions
- Process slices individually and recombine

## Further Learning

- [Napari Documentation](https://napari.org)
- [scikit-image Filters](https://scikit-image.org/docs/stable/api/skimage.filters.html)
- [napari-assistant](https://github.com/napari-assistant/napari-assistant)
- [Full Light-Sheet Workshop](https://bruvellu.github.io/light-sheet-image-analysis-workshop-2026/)

## References

- Cuenca, M., et al. - Practical segmentation workshop
- Data from: https://zenodo.org/records/17986091
