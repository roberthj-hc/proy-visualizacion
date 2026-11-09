# Informe

Abrir `informe.pdf` para leer la entrega. Editar `informe.tex` para cambiar el texto.

`resultados.tex` se genera con `scripts/pipeline.py`: contiene los recuentos de la ejecución, no debe modificarse a mano. `figures/` guarda PNG y PDF reutilizables. `evidence/` guarda la validación de los notebooks.

Desde esta carpeta, compilar dos veces con:

```powershell
pdflatex -interaction=nonstopmode -halt-on-error informe.tex
```

Se requiere MiKTeX o TeX Live con español, geometry, graphicx, booktabs, tabularx, amsmath, xcolor e hyperref. No hace falta BibTeX: la bibliografía está incluida en el archivo principal. Para Overleaf, subir esta carpeta completa, elegir pdfLaTeX y establecer `informe.tex` como documento principal.

El alcance corresponde a las semanas 1 y 2. Las figuras y el ETL están ejecutados; la evaluación con usuarios y el tablero Power BI están identificados como actividades posteriores.
