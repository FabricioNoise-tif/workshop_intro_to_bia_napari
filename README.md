# Curso de Fundamentos de Microscopia de Fluoresencia y Confocal UPCH

Bienvenidos al taller de análisis de bioimagenes de la UPCH. Estas 4 horas prácticas te introducen a la cuantificación, segmentación y análisis usan Napari, Napari-assistant con pyclesperanto y herramientas de análisis de imagenes mediante python.

## Partes del taller

Este taller estará dividio en 2 partes (1 teórica y 1 práctica), comenzando con la parte teórica:

### Sección 1: Fundamentos de segmentación (2 hours)
- **Lectura teórica** (30 minutos): Principios de segmentación de imágenes, métodos de thresholding, and operaciones morfológicas
- **Practical: Napari-based Segmentation** (90 minutes): Hands-on segmentation using Napari with the segmentation assistant
  - See [Napari Segmentation Guide](./segmentation_napari.md) for detailed instructions

### Section 2: Advanced Segmentation Methods (2 hours)
- **Theoretical Lecture** (30 minutes): Deep learning-based segmentation and CellPose overview
- **Practical: CellPose Segmentation** (90 minutes): Practical application of CellPose for automated cell segmentation
  - See [CellPose Segmentation Guide](./segmentation_cellpose.md) for detailed instructions

## Lecture Materials

Download the presentation slides and additional resources from **[Zenodo](https://zenodo.org/records/18717816)**.

## Environment Setup

### Prerequisites

Install [Pixi](https://pixi.sh/) - a package manager for Python environments.

### Installation

1. Clone or download this repository
2. Open a terminal in the repository directory
3. Install the environment:
   ```bash
   pixi install --all
   ```

### Download data

Downloaded the dataset with `pixi run download-data`.

## Running the Workshop

### Start Napari with the Assistant Plugin

```bash
pixi run assistant
```

This launches Napari with the segmentation assistant panel that guides you through the workflow.

## Sample Data

Download example images from Zenodo:
- **3D image**: `Lund.tif` - Full 3D stack for this workshop

https://zenodo.org/records/17986091

## Tips & Tricks

- **2D/3D Toggle**: Click the button in the lower-left corner to switch between 2D slicing and 3D rendering
- **Orthogonal Views**: Click the axis order button to see XY, XZ, and YZ planes simultaneously
- **Adjusting Parameters**: Parameters like erosion radius should be tuned based on your image
  - Larger radius = more aggressive erosion (better separation, but may lose small objects)
  - Smaller radius = conservative erosion (keeps more detail, but may not separate adequately)
- **Memory**: Working with large 3D stacks can be memory-intensive. Consider downsampling or working with subsets if needed

## Troubleshooting

### Napari won't launch
- Ensure all dependencies are properly installed: `pixi install`
- Try clearing the pixi cache: `pixi clean cache`

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
