# Ruta 14 · Residuos sólidos

Proyecto integrador de Visualización de Datos (DAT-382). Autor: Robert Josue Huanca Clares.

## Contenido

- `notebooks/`: todo el código, en cuatro notebooks que se ejecutan en orden.
  1. `01_fuentes_y_descarga.ipynb`: descarga las fuentes de NYC Open Data y revisa sus metadatos.
  2. `02_limpieza_e_integracion.ipynb`: proceso ETL (fechas, periodo, control de calidad, indicador, uniones y exportación).
  3. `03_analisis_exploratorio.ipynb`: EDA (estructura, calidad, distribuciones, relaciones y hallazgos).
  4. `04_figuras_y_resultados.ipynb`: mapa, barras y serie mensual.
- `data/raw/`: archivos originales, sin modificar.
- `data/processed/`: CSV para Power BI y GeoJSON para QGIS.
- `docs/figures/`: figuras generadas por los notebooks.
- `informe_completado.md`: informe con las secciones completadas y observaciones de revisión.

## Cómo ejecutar

**Google Colab:** subir la carpeta del proyecto (o clonarla), abrir cada notebook y ejecutar todas las celdas.
Colab ya trae pandas, matplotlib, geopandas y requests.

**Local (Windows):**

```powershell
python -m venv .venv
.\.venv\Scripts\python.exe -m pip install -r requirements.txt
.\.venv\Scripts\jupyter.exe notebook
```

Si los archivos de `data/raw/` ya existen, el notebook 01 no los vuelve a descargar.

## Notas sobre el indicador

Separación (%) = 100 × (papel + MGP) / (basura común + papel + MGP).
En Power BI no promediar `separacion_pct`; usar
`100 * DIVIDE(SUM(papel) + SUM(mgp), SUM(basura) + SUM(papel) + SUM(mgp))`.
