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

## ⚙️ Instalación de Fiji

Fiji es portable — no requiere instalador tradicional ni permisos de administrador.

1. **Descarga** la versión para tu sistema operativo desde la página oficial: [fiji.sc/#download](https://fiji.sc/#download) (Windows, macOS o Linux, 64-bit).
2. **Descomprime** el `.zip` (Windows/Linux) o monta el `.dmg` (macOS) en una carpeta con permisos de escritura — evita rutas como `Archivos de Programa` en Windows, ya que Fiji necesita escribir en su propia carpeta al actualizar/instalar plugins. Se recomienda algo simple como `C:\Fiji.app` o `~/Fiji.app`.
3. **Ejecuta** el binario:
   - Windows → `ImageJ-win64.exe`
   - macOS → abre `Fiji.app` (si macOS bloquea la app por venir de un desarrollador no identificado: `Preferencias del Sistema → Privacidad y Seguridad → Abrir de todos modos`)
   - Linux → `./ImageJ-linux64` (dale permisos de ejecución si hace falta: `chmod +x ImageJ-linux64`)
4. **Actualiza Fiji** apenas lo abras por primera vez: `Help → Update...` → `Update all` → reinicia Fiji. Esto asegura que tengas los plugins de bioimage analysis (BioFormats, MorphoLibJ, etc.) en su última versión — varias funciones que se usan en la clase (lectura de formatos propietarios como `.nd2`/`.czi`, LUTs específicas) dependen de que el updater haya corrido al menos una vez.
5. **Verifica la instalación** abriendo una imagen de muestra: `File → Open Samples → Boats (356K)` — si carga y puedes ver el histograma con `Ctrl+H`, tu instalación está lista para la práctica.

> **Nota:** Fiji corre sobre Java, y ya trae su propio runtime de Java empaquetado — no necesitas instalar Java por separado.

---

## 🐍 Instalación de Napari (vía Pixi)

El entorno de Napari se maneja con **[Pixi](https://pixi.sh/)**, un gestor de paquetes que resuelve automáticamente todas las dependencias (Python, napari, plugins) sin que tengas que instalar nada manualmente con `pip`/`conda`.

1. **Instala Pixi** siguiendo la guía oficial: [pixi.sh](https://pixi.sh/) (un solo instalador, funciona en Windows/macOS/Linux).
2. **Descarga el repositorio** desde GitHub: botón `Code → Download ZIP`.
3. **Extrae el ZIP** en una carpeta local llamada `napari` para tener acceso fácil (por ejemplo `C:\napari` en Windows).
4. **Abre una terminal** y navega a esa carpeta:
   ```bash
   cd C:\napari
   ```
5. **Instala el entorno** (esto resuelve y descarga todos los paquetes — puede tardar varios minutos la primera vez):
   ```bash
   pixi install --all
   ```
6. **Abre napari.** Este repositorio tiene **tres entornos independientes**, según qué flujo de trabajo quieras usar — elige el que corresponda:

   | Entorno | Para qué sirve | Comando |
   |---|---|---|
   | `assistant` | Segmentación clásica (thresholding, filtros, watershed) con `napari-assistant` / `pyclesperanto` | `pixi run -e assistant assistant` |
   | `cellpose` | Segmentación por deep learning con **CellPose** (PyTorch) | `pixi run -e cellpose napari` |
   | `stardist` | Segmentación por deep learning con **StarDist** (TensorFlow) | `pixi run -e stardist stardist` |

   Cada vez que quieras volver a abrir napari, solo repite el paso 4 (`cd C:\napari`) y el comando de la tabla correspondiente al entorno que necesitas — no hace falta repetir `pixi install` salvo que el `pixi.toml` haya cambiado.

---

## 🔬 Flujo de segmentación clásica en Napari (entorno `assistant`)

Este es el pipeline que se usa en la clase para pasar de una imagen cruda a una tabla de datos cuantificados, usando el widget **napari-assistant**. Sigue siempre este orden — cada paso condiciona al siguiente:

### 1. Remove Background (Eliminación de fondo)
Corrige iluminación desigual o fondo fluorescente inespecífico antes de segmentar. Es un paso obligatorio: si no se elimina el fondo, la fluorescencia difusa actúa como puente de luz y el algoritmo de labeling termina fusionando células vecinas en un solo objeto.

- **`top_hat_sphere`** — elemento estructurante esférico que borra todo lo más grande que el radio configurado. **Úsalo con:** células aisladas y redondeadas (PBMCs, somas de microglía) — al ser esférico, filtra el fondo de forma uniforme en todas direcciones.
- **`top_hat_box`** — igual que el anterior pero con una matriz cuadrada en vez de esférica. **Úsalo con:** tejidos densos y compactos (ej. capa granular del giro dentado en hipocampo), muy eficiente contra variaciones locales de luz en campo claro (DAB).

### 2. Filter / Noise Removal (Filtrado de ruido)
Elimina el ruido de fondo generado por la cámara/sensor antes de que el software intente detectar bordes.

- **`gaussian_blur`** — desenfoque matemático que atenúa variaciones de alta frecuencia. **Úsalo con:** conectar estructuras finas (ramificaciones de microglía) o suavizar ruido digital de fotos de smartphone. **Cuidado:** un sigma muy alto fusiona células vecinas.
- **`median_filter`** — reemplaza cada píxel por la mediana de sus vecinos. **Úsalo con:** ruido tipo sal-y-pimienta (píxeles aislados blancos/negros), preservando bordes mucho mejor que el filtro Gaussiano.

### 3. Binarize (Binarización)
Convierte la imagen en escala de grises en una máscara binaria (foreground/background).

- **`threshold_otsu`** — umbral global automático que minimiza la varianza intra-clase del histograma. **Úsalo con:** separación general de células brillantes sobre fondo oscuro, cuando el histograma es claramente bimodal.
- **`threshold_voronoi`** — combina una binarización preliminar con una separación basada en diagramas de Voronoi. **Úsalo con:** casos donde Otsu simple no alcanza porque hay variación local de intensidad entre células.

### 4. Label (Segmentación por instancia)
Asigna un ID único a cada objeto detectado — este es el paso que convierte "manchas blancas" en "objetos contables".

- **`voronoi_otsu_labeling`** — el algoritmo más usado del curso. Combina dos filtros Gaussianos con dos parámetros clave:
  - `spot_sigma` — detecta los centros/núcleos; controla qué tan separados deben estar dos picos de brillo para contarse como individuos distintos.
  - `outline_sigma` — define los contornos; subirlo desenfoca más y junta más células.
  **Úsalo con:** conteo de PBMCs o microglía, porque separa automáticamente cúmulos que se tocan, sin intervención manual.
- **`connected_component_labeling`** — agrupa píxeles blancos en contacto directo bajo el mismo ID. **Úsalo solo si** tus células están 100% aisladas — si dos se tocan, las fusiona en un solo objeto gigante (la trampa más común para quien recién empieza).
- **`local_minima_seeded_watershed`** — usa mínimos locales como semillas para delimitar bordes (algoritmo de cuenca hidrográfica). **Úsalo con:** estructuras con membranas densas o muy bien marcadas.

### 5. Process Labels (Limpieza de máscaras)
- **`exclude_labels_out_of_size_range`** — filtra por tamaño máximo/mínimo, eliminando basura o artefactos gigantes.
- **`exclude_labels_on_edges`** — elimina objetos cortados por el borde de la imagen, para no sesgar las estadísticas de tamaño hacia abajo.

### 6. Measure Labels (Cuantificación)
`regionprops` (scikit-image) extrae área, intensidad, posición y conteo total de cada objeto etiquetado, en una tabla exportable directo a CSV/Excel.

> **Tip:** una vez armado tu pipeline, usa el botón **"Generate code"** del napari-assistant para exportarlo como script de Python — así queda documentado y es 100% reproducible sin tener que repetir los clics manualmente en la próxima imagen.

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
