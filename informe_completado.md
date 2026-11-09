# Ruta 14 · Residuos Sólidos — Informe completado y revisado

> **Cómo leer este archivo**
>
> El texto del informe va completo y en orden. Entre párrafos hay notas de revisión con tres marcas:
>
> - **✅ Bien:** la parte está correcta y se puede dejar como está.
> - **✏️ Modificar:** hay algo que conviene corregir o precisar, y se dice qué y dónde.
> - **➕ Agregado:** texto nuevo que no estaba en el original (sobre todo donde decía *completar…*).
>
> Al pasarlo a Word, borrar las notas, aplicar alineación justificada (Ctrl + J) a todo el cuerpo y actualizar el índice. Las cifras salen de los notebooks `01` a `04` de la carpeta `notebooks/`.

---

**UNIVERSIDAD MAYOR DE SAN ANDRÉS**
**FACULTAD DE CIENCIAS PURAS Y NATURALES**
**CARRERA DE INFORMÁTICA**

**Ruta 14 – Residuos Sólidos**
**Optimizar rutas y campañas de separación**

Estudiante: Robert Josue Huanca Clares
Asignatura: Visualización de Datos (DAT-382)
Docente: M.Sc. Elizabeth Pommier Gallo
Gestión: II – 2026
Fecha: 30 de octubre de 2026

La Paz – Bolivia

> **✅ Bien:** la portada tiene todo lo que pide el temario (título, estudiante, ruta, asignatura, docente, gestión y fecha).
>
> **✏️ Modificar:** el subtítulo dice "Optimizar rutas", pero en la metodología se aclara que las fuentes no traen recorridos de camiones ni horarios. Es mejor dejar el nombre de la ruta tal como lo asignó la docente y agregar debajo una línea como: *"Análisis territorial de la recolección y la separación de residuos en los distritos comunitarios de Nueva York (2015–2026)"*. Así el título no promete algo que el proyecto no hace.

---

## Índice

- 1\. Introducción
- 2\. Antecedentes
    - 2.1. Situación actual del fenómeno analizado
    - 2.2. Experiencias, datos y estudios previos relacionados
    - 2.3. Necesidad de utilizar datos, indicadores y visualizaciones
- 3\. Planteamiento del problema
    - 3.1. Descripción de la problemática
    - 3.2. Consecuencias de la falta de información
    - 3.3. Formulación de la pregunta
- 4\. Objetivos
    - 4.1. Objetivo general
    - 4.2. Objetivos específicos
- 5\. Marco conceptual, normativo y metodológico
    - 5.1. Visualización de datos, percepción y narrativa visual
    - 5.2. Autores: Bertin, Cleveland y McGill, Munzner, Few, Tufte, Ware y Shneiderman
    - 5.3. Normas ISO/IEC e IEEE obligatorias
    - 5.4. Marcos: CRISP-DM, modelo anidado de Munzner, PDCA, ISO 31000 y otros pertinentes
- 6\. Metodología de Desarrollo
    - 6.1. Comprensión del problema
    - 6.2. Obtención y comprensión de datos
    - 6.3. Proceso ETL
    - 6.4. Análisis, diseño visual, evaluación y despliegue
- 7\. Datos y proceso ETL
    - 7.1. Fuentes de datos, licencia y condiciones de uso
    - 7.2. Diccionario y estructura de los datos
    - 7.3. Datos en bruto y datos procesados
    - 7.4. Reglas de limpieza, transformación, integración y carga
    - 7.5. Evidencia del proceso ETL
- 8\. Análisis exploratorio de datos (EDA)
    - 8.1. Descripción de la estructura y tipos de variables
    - 8.2. Valores faltantes, duplicados, inconsistencias y datos atípicos
    - 8.3. Distribuciones, frecuencias, medidas descriptivas y tendencias
    - 8.4. Relaciones, segmentaciones, correlaciones y patrones iniciales
    - 8.5. Visualizaciones de datos en bruto
    - 8.6. Interpretación de hallazgos preliminares y decisiones de limpieza o transformación
- 9\. Referencias
- Acta de alcance individual


> **✏️ Modificar:** en el índice original faltaba la sección 8 (EDA) y las referencias. El título de la sección 5 debe decir "Marco conceptual, normativo y metodológico" y no solo "Marco".

---

## 1. Introducción

La gestión de los residuos sólidos es uno de los servicios urbanos más visibles y, al mismo tiempo, uno de los más difíciles de planificar. Cada día una ciudad tiene que decidir cuántos camiones salen, a qué zonas van, qué materiales recogen por separado y en qué barrios conviene reforzar las campañas de reciclaje. Para tomar esas decisiones no basta con saber cuánto se recoge en total: hace falta conocer cómo se reparte esa cantidad entre los distritos, qué parte corresponde a materiales reciclables y cómo ha cambiado con el paso de los años.

La visualización de datos ayuda justamente en ese punto. Una tabla con miles de filas no muestra a simple vista qué distritos se alejan del resto, pero un mapa coloreado o un gráfico de barras ordenado sí lo hace. Bien diseñadas, estas representaciones permiten comparar zonas, reconocer patrones y detectar cambios que en los datos crudos pasarían desapercibidos, y por eso son un apoyo directo para quien tiene que priorizar recursos.

En este proyecto se analiza la recolección de residuos en los 59 distritos comunitarios de la ciudad de Nueva York entre enero de 2015 y septiembre de 2026. Se consideran tres fracciones: basura común; papel; y metal, vidrio, plástico y cartones de bebidas (MGP, por sus siglas en inglés). Se eligió Nueva York porque su Departamento de Saneamiento (DSNY) publica registros mensuales por distrito y por tipo de material, algo que no se encontró disponible para La Paz.

Como solución se construye un proceso de extracción, transformación y carga (ETL) en Python que descarga los datos oficiales, los limpia, los une con la información geográfica y de población, y calcula un indicador de separación. Con esos datos preparados se elaboran mapas y gráficos que más adelante formarán parte de un tablero en Power BI. El propósito es que el personal encargado de planificar la recolección pueda ver con claridad dónde se separa menos y dónde la recolección pesa más, y usar esa información para decidir hacia qué distritos dirigir las campañas.

> **✅ Bien:** la introducción cumple los tres puntos del temario (tema y contexto, importancia de la visualización y síntesis de la solución).
>
> **✏️ Modificar:**
> - Decía "basura común, papel y otros". Conviene nombrar la tercera fracción como **MGP** desde el principio y explicar la sigla una vez; "otros" es ambiguo y en el resto del documento se usa MGP.
> - Decía "59 distritos". Es más preciso decir **distritos comunitarios** (*community districts*), que son la unidad que usa el DSNY. Nueva York también tiene 5 *boroughs* y no hay que confundirlos.
> - La primera versión de la introducción no decía *por qué* Nueva York. Se agregó una frase corta con la razón.
>
> **➕ Agregado:** el último párrafo ahora dice para quién es la solución y qué decisión apoya, lo que conecta con el acta de alcance.

---

## 2. Antecedentes

### 2.1. Situación actual del fenómeno analizado

La generación de residuos crece al ritmo de las ciudades. El Banco Mundial estimó que en 2016 el mundo produjo unos 2.010 millones de toneladas de residuos sólidos municipales y que, si no cambian las tendencias, la cifra llegaría a 3.400 millones hacia 2050 (Kaza et al., 2018). Gran parte de ese volumen podría reciclarse, pero solo se aprovecha si se separa en el origen y si el servicio de recolección está organizado para recibirlo por separado.

En Bolivia, la Ley N.º 755 de Gestión Integral de Residuos (2015) establece la separación en origen y el aprovechamiento como parte de las obligaciones de los gobiernos municipales. En La Paz, el tema ganó visibilidad tras el deslizamiento del relleno sanitario de Alpacoma en enero de 2019, que obligó a buscar un nuevo sitio de disposición final y puso en evidencia la necesidad de reducir lo que llega al relleno. Sin embargo, al buscar datos para este proyecto, no se encontró información pública que tenga el nivel de detalle necesario: cantidades por distrito, por tipo de residuo y por mes.

Por esa razón se trabaja con los datos del Department of Sanitation of New York (DSNY). Nueva York tiene reciclaje obligatorio desde la Ley Local 19 de 1989 y el DSNY publica cada mes cuántas toneladas recoge en cada uno de sus 59 distritos comunitarios, separadas en basura común, papel y MGP. En el periodo de estudio, la ciudad recogió cerca de tres millones de toneladas de basura común al año, y el papel y el MGP juntos representaron alrededor del 17 % de lo recolectado en esas tres fracciones. Aunque el contexto es distinto al paceño, estos datos permiten desarrollar y probar el análisis y las visualizaciones propuestas.

> **✅ Bien:** la idea central (La Paz no tiene datos desagregados y por eso se usa Nueva York) está clara.
>
> **➕ Agregado:** contexto global (Banco Mundial), normativa boliviana (Ley 755), el caso Alpacoma y datos básicos de Nueva York. El temario pide "situación actual del fenómeno", y el original solo hablaba de la falta de datos, no del fenómeno en sí.
>
> **✏️ Revisar:** si se quiere poner una cifra de toneladas diarias de La Paz, sacarla de un informe oficial del GAMLP o del INE y citarla; aquí no se puso ninguna para no inventar datos.

### 2.2. Experiencias, datos y estudios previos relacionados

Durante la búsqueda de información se revisaron primero fuentes nacionales. El Instituto Nacional de Estadística (INE) de Bolivia publica estadísticas de recolección de residuos por ciudad y por procedencia, y existen diagnósticos municipales sobre la gestión de residuos en La Paz. Ninguna de estas fuentes ofrece una base de datos mensual descargable que permita comparar tipos de residuos por distrito.

Como alternativa se identificaron dos fuentes de Nueva York: los registros mensuales del DSNY y los datos históricos de población por distrito del Department of City Planning (DCP). Juntas permiten relacionar la recolección con la ubicación y el tamaño poblacional de cada distrito. El propio DSNY usa estos datos para calcular su *tasa de desvío* (*diversion rate*), es decir, la parte de lo recolectado que no va al relleno porque se recicla o se composta, y la reporta en sus informes anuales.

| Alternativa | Aporte | Límite | Decisión |
|---|---|---|---|
| INE – Bolivia | Estadísticas de recolección por ciudad y procedencia | No hay una serie mensual por distrito con papel y MGP | Se usa solo como contexto |
| DSNY + DCP, Nueva York | Recolección mensual por distrito y población histórica | Contexto y población distintos a La Paz | Se usa como caso de aplicación |

Esta decisión permite trasladar a La Paz la forma de analizar y visualizar, pero no las cifras ni las conclusiones particulares de Nueva York. Tampoco se encontró evidencia de que alguna institución paceña tenga registros con el mismo detalle; para aplicar el tablero en La Paz habría que solicitarlos formalmente y comprobar su cobertura y calidad.

> **✅ Bien:** la tabla de alternativas es una buena forma de justificar la elección de la fuente.
>
> **✏️ Modificar:** en la columna "Decisión" de INE decía "No tiene datos suficientes", que repite el límite. Se cambió por lo que realmente se decide hacer con la fuente.
>
> **➕ Agregado:** el concepto de tasa de desvío del DSNY, que es el antecedente directo del indicador que se calcula después.

### 2.3. Necesidad de utilizar datos, indicadores y visualizaciones

Los datos de Nueva York se eligieron porque están estructurados y tienen el detalle suficiente para construir indicadores comparables. Sin embargo, los datos por sí solos no responden la pregunta del proyecto. Una cifra de toneladas totales favorece a los distritos grandes y castiga a los pequeños, así que hace falta un indicador relativo, como la proporción de papel y MGP sobre el total recolectado, para comparar distritos de distinto tamaño en igualdad de condiciones.

Una vez calculado el indicador, la visualización hace que las diferencias se vean. Un mapa muestra si los distritos con menor separación están juntos o dispersos; un gráfico de barras ordena los valores para comparar con precisión; y una serie temporal muestra si la situación mejora o empeora. Ninguna de estas preguntas se responde bien leyendo una tabla.

Los resultados no se pueden trasladar a La Paz, pero la metodología sí. Si en el futuro se consiguen registros locales con el detalle necesario, el mismo proceso ETL y el mismo tablero podrían reutilizarse cambiando solo la fuente.

> **✅ Bien:** la idea de que la metodología es transferible y las cifras no.
>
> **➕ Agregado:** la justificación de *por qué* se necesita un indicador relativo y no toneladas absolutas, y para qué sirve cada tipo de gráfico. Esto es lo que el temario pide en "necesidad de utilizar datos, indicadores y visualizaciones".

---

## 3. Planteamiento del problema

### 3.1. Descripción de la problemática

En Nueva York, cada distrito genera cantidades muy distintas de residuos y separa los reciclables en proporciones también distintas. Para entender esas diferencias hay que mirar varios factores a la vez: cuántas toneladas se recogen, de qué tipo, cuánta gente vive en el distrito y dónde está ubicado.

Cuando esa información se revisa por separado, es difícil ver la relación entre la cantidad recolectada y las características de cada distrito. Un distrito puede recoger mucha basura simplemente porque tiene más habitantes, y otro puede recoger poco papel porque tiene menos comercios u oficinas. Si no se consideran estos factores, las comparaciones se vuelven engañosas.

Además, la recolección cambia con el tiempo: hay meses con más residuos, años con eventos extraordinarios como la pandemia de 2020, y cambios en los programas de reciclaje. Sin una vista conjunta, territorial y temporal, no es posible distinguir un problema persistente de un pico ocasional.

> **✅ Bien:** la descripción es clara y no adelanta la solución, como pide el temario.
>
> **✏️ Modificar:** se agregaron ejemplos concretos (más habitantes, menos comercios, pandemia) para que el problema se entienda mejor sin ser técnico.

### 3.2. Consecuencias de la falta de información

Sin información organizada, integrada y confiable, es difícil saber qué distritos recogen más residuos y cuáles separan menos. Esto tiene consecuencias prácticas: las campañas de reciclaje pueden terminar repartidas de forma uniforme en toda la ciudad, cuando lo eficiente sería concentrarlas donde la separación es más baja; y los recursos de recolección pueden asignarse por costumbre y no según la carga real de cada zona.

Por otra parte, comparar distritos sin tener en cuenta su población o su tamaño puede llevar a conclusiones equivocadas. Un distrito con muchas toneladas de papel no necesariamente separa mejor; puede que simplemente genere más residuos en general. Interpretaciones de este tipo pueden llevar a premiar o descuidar zonas por motivos que no tienen que ver con su desempeño real.

> **✅ Bien.**
>
> **➕ Agregado:** consecuencias concretas para la planificación (campañas mal dirigidas, recursos asignados por costumbre).

### 3.3. Formulación de la pregunta

¿Qué diferencias existen en la recolección de basura común, papel y MGP entre los distritos comunitarios de Nueva York entre enero de 2015 y septiembre de 2026, y qué distritos requieren mayor atención en la separación y recolección de residuos?

> **✅ Bien:** la pregunta delimita lugar, periodo y variables, y no adelanta la solución.
>
> **✏️ Modificar:** cambiar "papel y otros" por "papel y MGP" y "durante el periodo de enero de 2015 a septiembre de 2026" por "entre enero de 2015 y septiembre de 2026" (más directo).

---

## 4. Objetivos

### 4.1. Objetivo general

Implementar una herramienta de visualización de datos que permita identificar qué distritos comunitarios de Nueva York requieren mayor atención en la separación y recolección de residuos, a partir del tonelaje mensual registrado entre enero de 2015 y septiembre de 2026.

> **✅ Bien:** empieza con verbo en infinitivo y responde a la pregunta del problema.

### 4.2. Objetivos específicos

1. Analizar el problema, las fuentes de información disponibles y las necesidades del personal encargado de la planificación municipal.
2. Diseñar y ejecutar un proceso ETL que integre los datos por distrito y mes, verificando su calidad y conservando su procedencia.
3. Desarrollar indicadores, mapas y gráficos que permitan comparar los distritos y observar los cambios en la recolección de residuos.
4. Verificar la consistencia de los resultados y evaluar si la herramienta es fácil de usar, accesible y útil para consultar la información.

> **✅ Bien:** los cuatro objetivos siguen exactamente la estructura del temario (analizar, diseñar ETL, desarrollar visualizaciones, verificar).

---

## 5. Marco conceptual, normativo y metodológico

### 5.1. Visualización de datos, percepción y narrativa visual

**Conceptos sobre residuos sólidos**

- **Residuos sólidos urbanos (RSU):** materiales que se desechan en hogares, comercios e instituciones y que recoge el servicio municipal. En Nueva York, el DSNY recoge los residuos de viviendas e instituciones públicas; los comercios contratan empresas privadas, por lo que no aparecen en estos datos.
- **Fracción:** cada tipo de residuo que se recoge por separado. En este proyecto son tres: basura común (lo que va al relleno), papel y MGP (metal, vidrio, plástico y cartones de bebidas).
- **Separación en origen:** clasificar los residuos en el lugar donde se generan, antes de que pasen el camión. Es el primer paso para que algo se pueda reciclar; un material reciclable mezclado con basura común casi siempre termina en el relleno.
- **Tasa de desvío:** porcentaje de residuos que no va a disposición final porque se recicla o se aprovecha. El DSNY la usa como indicador principal de su programa de reciclaje.
- **Jerarquía de residuos:** orden de preferencia en la gestión: primero reducir, luego reutilizar, después reciclar y, al final, disponer. Medir cuánto se separa permite evaluar el tercer escalón.
- **Economía circular:** enfoque que busca mantener los materiales en uso el mayor tiempo posible. La separación en origen es la condición para que el papel o el plástico vuelvan a la cadena productiva.

**Conceptos sobre datos e indicadores**

- **Indicador:** medida que resume un fenómeno en un número comparable. Aquí se usa la **proporción de separación**:

  > Separación (%) = (papel + MGP) / (basura común + papel + MGP) × 100

  Se eligió un cociente y no toneladas absolutas porque permite comparar distritos de distinto tamaño. Es una aproximación a la tasa de desvío del DSNY, pero sin orgánicos, porque esa fracción tiene demasiados vacíos en la fuente (se verá en el EDA).
- **Granularidad:** nivel de detalle de cada registro. Aquí cada fila es un distrito en un mes. Esa granularidad es la que no se encontró para La Paz.
- **Clave de integración:** campo que permite unir dos tablas. El código `boro_cd` (por ejemplo, 306 = Brooklyn, distrito 6) une el tonelaje con el mapa y con la población.
- **ETL (extracción, transformación y carga):** proceso que toma datos de su fuente, los limpia y adapta, y los deja listos para analizar o visualizar.
- **Calidad de datos:** grado en que los datos sirven para el uso previsto. Se revisan cuatro dimensiones: completitud (no faltan datos), exactitud (los valores son correctos), consistencia (no se contradicen entre sí) y actualidad (están al día).
- **Medidas robustas:** la mediana y los cuartiles se ven menos afectados por valores extremos que la media. Se usan porque las toneladas por distrito tienen distribución asimétrica.

**Visualización de datos y percepción**

La visualización de datos es la representación gráfica de información para que una persona pueda comprenderla, compararla y tomar decisiones con ella (Munzner, 2014). Funciona porque el sistema visual humano procesa ciertas propiedades, como la posición, el largo o el color, mucho más rápido que la lectura de números.

En este proyecto la visualización debe responder tres preguntas:

- ¿Dónde existen diferencias entre los distritos?
- ¿Qué tan grandes son esas diferencias?
- ¿Cómo cambian con el tiempo?

Cada una se responde con un tipo de gráfico distinto: la primera con un mapa, la segunda con barras ordenadas y la tercera con una serie temporal.

**Mapas coropléticos y normalización.** Un mapa coroplético colorea cada área según un valor. Su regla principal es que el valor debe estar normalizado (un porcentaje o una tasa) y no ser un total, porque las áreas grandes o muy pobladas siempre aparecerían como "las peores". Por eso el mapa usa el porcentaje de separación y no las toneladas.

**Escalas de color y accesibilidad.** Para datos ordenados se usan escalas secuenciales, de un solo tono o perceptualmente uniformes como *viridis*, que se leen bien en blanco y negro y por personas con daltonismo (Harrower y Brewer, 2003). Las escalas divergentes (dos colores con un centro) se reservan para datos que tienen un punto medio con sentido, como una meta.

**Narrativa visual**

Contar una historia con datos significa ordenar los gráficos para guiar al lector desde la pregunta hasta la conclusión (Segel y Heer, 2010; Knaflic, 2015). En este proyecto la secuencia será: situación general → distritos que se alejan → evolución en el tiempo → distritos a revisar.

> **➕ Agregado:**
> - Conceptos de residuos sólidos (5.1.1) y de datos e indicadores (5.1.2): el marco conceptual original solo hablaba de visualización.
> - Conceptos de mapas coropléticos, normalización, escalas de color y narrativa visual, que el temario pide ("percepción y narrativa visual").

### 5.2. Autores: Bertin, Cleveland y McGill, Munzner, Few, Tufte, Ware y Shneiderman

Para diseñar los gráficos se toman como referencia los siguientes autores. De cada uno se explica brevemente qué propone y cómo se aplica en el proyecto.

**Variables visuales (Bertin, 1967).** Bertin propuso que todo gráfico se construye con un conjunto limitado de variables visuales: posición, tamaño, valor (claro u oscuro), textura, color, orientación y forma. No todas sirven para lo mismo: el *valor* sirve para mostrar orden (más o menos), mientras que el *color* (tono) sirve para distinguir categorías. Por eso, en el mapa la separación se representa con una escala que va de oscuro a claro y no con colores sin orden.

**Precisión de la percepción (Cleveland y McGill, 1984).** Estos autores midieron con qué exactitud las personas comparan valores según cómo se codifiquen. La posición sobre una escala común es la forma más precisa; el largo le sigue; el ángulo, el área y el color quedan al final. Esto justifica que el mapa coroplético (que usa color) se complemente con un gráfico de barras: el mapa muestra *dónde*, las barras muestran *cuánto*.

**Modelo anidado (Munzner, 2009).** Munzner divide el diseño de una visualización en cuatro niveles: (1) entender el problema del usuario, (2) traducirlo a datos y tareas, (3) elegir la forma visual y la interacción, y (4) implementarla. Si un nivel falla, los siguientes también fallan. Este modelo se usa para comprobar que cada gráfico responde a una necesidad real del planificador municipal.

**Paneles de control o tableros (Few, 2006).** Few define un tablero como una vista que reúne en una sola pantalla la información más importante para un objetivo. Recomienda pocos indicadores, sin adornos que distraigan, y que todo se entienda de un vistazo. El futuro tablero en Power BI seguirá ese criterio.

**Integridad gráfica y relación datos-tinta (Tufte, 1983).** Tufte plantea que la mayor parte de la "tinta" de un gráfico debe mostrar datos y no decoración (*data-ink ratio*), y que un gráfico no debe exagerar ni reducir las diferencias reales (*lie factor*). En la práctica: escalas que empiezan en cero en las barras, unidades y fuentes siempre visibles, y nada de efectos 3D ni sombras.

**Percepción preatentiva y Gestalt (Ware, 2021).** Ware explica que algunas propiedades visuales, como un color muy distinto o un tamaño mayor, se detectan antes de prestar atención conscientemente. También aplica los principios de la Gestalt: los elementos cercanos o del mismo color se perciben como un grupo. Se usa para resaltar los distritos prioritarios y para no depender únicamente del color, ya que cerca del 8 % de los hombres tiene algún tipo de daltonismo.

**Mantra de búsqueda visual (Shneiderman, 1996).** "Primero una vista general, luego acercar y filtrar, y después detalles a pedido". El tablero empezará con el mapa de toda la ciudad, permitirá filtrar por periodo y distrito, y mostrará el detalle de cada distrito al pasar el cursor.

| Autor | Concepto | Aplicación en el proyecto |
|---|---|---|
| Bertin | Variables visuales | Escala de valor (oscuro a claro) para ordenar proporciones en el mapa |
| Cleveland y McGill | Jerarquía de precisión perceptual | Complementar el mapa con barras sobre un eje común para comparar con precisión |
| Munzner | Modelo anidado | Separar necesidad del usuario, tarea, forma visual e implementación |
| Few | Diseño de tableros | Pocos indicadores, sin adornos, todo visible en una pantalla |
| Tufte | Relación datos-tinta e integridad gráfica | Mantener escalas, unidades y fuentes; evitar 3D y distorsiones |
| Ware | Percepción preatentiva y Gestalt | Contraste y agrupación para resaltar; no depender solo del color |
| Shneiderman | Vista general, filtro y detalle | Tablero con mapa general, filtros de periodo y distrito, y detalle al pasar el cursor |
| Brewer | Escalas de color | Escala secuencial apta para daltonismo (viridis) |
| Segel y Heer, Knaflic | Narrativa visual | Orden de los gráficos desde la pregunta hasta la conclusión |

> **✅ Bien:** la tabla de autores ya relacionaba cada autor con una aplicación concreta, que es lo más importante.
>
> **➕ Agregado:** una explicación breve de *qué propone* cada autor antes de decir cómo se aplica. Sin esto, la tabla queda como una lista de nombres sin justificar.
>
> **✏️ Modificar:** en Bertin decía "usar una escala de color"; técnicamente la variable que ordena es el **valor** (claridad), no el tono. Se corrigió.

### 5.3. Normas ISO/IEC e IEEE obligatorias

Las normas se usan como guía para definir controles en cada etapa del proyecto. No se busca una certificación, sino tomar de cada una los criterios que aplican a un proyecto de datos de este tamaño.

| Norma | Tema | Cómo se aplica en el proyecto |
|---|---|---|
| ISO 14001:2026 | Sistemas de gestión ambiental | Usar el indicador de separación como medida de desempeño ambiental y proponer su seguimiento periódico (mejora continua) |
| ISO 37120:2018 | Indicadores de ciudades sostenibles | Esta norma incluye indicadores de residuos sólidos, como el porcentaje de residuos reciclados; respalda que la proporción de separación sea un indicador urbano reconocido |
| ISO/IEC 25012:2008 | Modelo de calidad de datos | Revisar completitud, exactitud, consistencia y actualidad en el ETL (duplicados, nulos, negativos, meses faltantes) |
| ISO/IEC 25010:2023 | Calidad del producto de software | Evaluar la usabilidad y la adecuación funcional del tablero |
| ISO/IEC 27001:2022 | Seguridad de la información | Controlar el origen de los archivos, guardar los datos originales sin modificar y documentar la descarga |
| ISO/IEC 27701:2025 | Gestión de la privacidad | Confirmar que los datos son agregados por distrito y no contienen información personal |
| ISO 9241-210:2019 | Diseño centrado en las personas | Diseñar el tablero a partir de las necesidades del personal de planificación y probarlo con ellos |
| ISO/IEC 40500:2012 (WCAG 2.0) | Accesibilidad web | Contraste suficiente, textos alternativos y no usar el color como único medio para transmitir información |
| ISO 31000:2018 | Gestión del riesgo | Identificar, evaluar y tratar los riesgos del proyecto (ver 5.4) |
| IEEE 1012-2024 | Verificación y validación | Verificar que el ETL produce los datos esperados y validar que las visualizaciones responden la pregunta |

**Normativa de contexto en Bolivia:** la Ley N.º 755 de Gestión Integral de Residuos (2015) y su reglamento establecen la separación en origen y el aprovechamiento de residuos. Se menciona porque es el marco en el que se aplicaría la metodología en La Paz.

> **✅ Bien:** las seis normas originales son pertinentes.
>
> **➕ Agregado:**
> - **ISO 31000**, que el temario pide de forma explícita y no estaba. Su aplicación se desarrolla en 5.4.
> - **ISO 37120**, que justifica el indicador de separación desde una norma de ciudades.
> - **ISO/IEC 25010** y **ISO/IEC 40500**, porque el objetivo 4 habla de usabilidad y accesibilidad y no había norma que lo respaldara.
> - La columna "cómo se aplica": la lista original solo decía de qué trata cada norma.
>
> **✏️ Revisar:** confirmar en el catálogo de ISO e IEEE el año vigente de ISO 14001 y de IEEE 1012 antes de entregar.

### 5.4. Marcos: CRISP-DM, modelo anidado de Munzner, PDCA, ISO 31000 y otros pertinentes

Se combinan varios enfoques que se complementan: uno organiza el proyecto de datos, otro controla el diseño visual, otro asegura la mejora continua y el último gestiona los riesgos.

**CRISP-DM** (*Cross-Industry Standard Process for Data Mining*; Chapman et al., 2000). Es el estándar más usado para proyectos de datos y tiene seis fases: comprensión del negocio, comprensión de los datos, preparación, modelado, evaluación y despliegue. Se eligió porque sus primeras fases coinciden con lo que pide el temario en las semanas 1 y 2. En este proyecto, la fase de "modelado" se reemplaza por el cálculo de indicadores y el diseño visual, ya que no se entrena ningún modelo predictivo.

**Modelo anidado de Munzner** (2009). Se usa como lista de control del diseño: para cada gráfico se verifica qué necesidad del usuario atiende, qué tarea realiza (comparar, ubicar, ver tendencias), qué forma visual se eligió y por qué.

**Ciclo PDCA** (*Planificar, Hacer, Verificar, Actuar*; Deming). Se aplica en cada iteración: se planifica una regla de limpieza o un gráfico, se implementa, se verifica el resultado y se corrige si hace falta. Es también el ciclo que usa ISO 14001, lo que conecta la metodología con el marco normativo.

**ISO 31000** aporta el proceso de gestión de riesgos (identificar, analizar, evaluar y tratar), que se aplica de forma transversal a todo el proyecto. Los riesgos principales son:

| Riesgo | Probabilidad | Impacto | Tratamiento |
|---|---|---|---|
| Los datos incluyen residuos de instituciones y no solo de hogares | Alta | Medio | Aclararlo en cada gráfico y no hablar de "separación de los hogares" |
| Población desactualizada (último censo de la fuente: 2010) | Alta | Medio | Usarla solo como contexto, no calcular toneladas por habitante actuales |
| Leer una separación baja como "mal servicio" | Media | Alto | Presentar la priorización como exploratoria, a validar con personal municipal |
| Cambios en la fuente o en la página de descarga | Baja | Medio | Guardar copia local de los datos en bruto y sus metadatos |
| Trasladar cifras de Nueva York a La Paz | Media | Alto | Aclarar en el informe que solo se transfiere la metodología |

**Cómo se combinan los marcos en el tiempo:**

| Fase CRISP-DM | Semana | Nivel de Munzner | Entregable |
|---|---|---|---|
| Comprensión del negocio | 1 | Necesidad del usuario | Problema, objetivos, acta de alcance |
| Comprensión de los datos | 2 | Datos y tareas | Notebook 01 y EDA (notebook 03) |
| Preparación de los datos | 2 | Datos y tareas | Notebook 02 (ETL) |
| Modelado → indicadores y diseño visual | 3 en adelante | Forma visual | Notebook 04, mapas y gráficos |
| Evaluación | posterior | Implementación | Pruebas con usuarios |
| Despliegue | posterior | Implementación | Tablero en Power BI |

> **✅ Bien:** los tres enfoques originales (CRISP-DM, Munzner y PDCA) son los adecuados.
>
> **➕ Agregado:** una explicación de qué es cada uno y por qué se elige, ISO 31000 con su tabla de riesgos (el temario la incluye en esta sección) y una tabla que muestra cómo se combinan los enfoques en el tiempo.

---

## 6. Metodología de Desarrollo

El proyecto se desarrolla en cuatro etapas, que siguen las fases de CRISP-DM y van desde entender el problema hasta poner el tablero a disposición de los usuarios.

### 6.1. Comprensión del problema

Se define qué información necesita el personal de planificación, qué decisión se quiere apoyar y cuál es el alcance. El análisis se hace por distrito y por mes. No se incluyen rutas de camiones ni horarios, porque las fuentes disponibles no tienen esa información.

### 6.2. Obtención y comprensión de datos

Se descargan las fuentes oficiales de NYC Open Data junto con sus metadatos y el diccionario de variables del DSNY. Antes de limpiar nada se revisa qué contiene cada archivo, qué significa cada columna y qué limitaciones declara la propia fuente (notebook `01_fuentes_y_descarga`).

### 6.3. Proceso ETL

Se convierten las fechas, se crea la clave de distrito, se selecciona el periodo, se revisan duplicados, nulos y valores negativos, se calcula el indicador y se unen las tablas con la información geográfica y de población. Al final se verifica que las uniones sean correctas y se guardan los resultados (notebook `02_limpieza_e_integracion`). El detalle de cada regla está en la sección 7.

### 6.4. Análisis, diseño visual, evaluación y despliegue

**Análisis y diseño visual.** Se explora la estructura y el comportamiento de los datos (notebook `03_analisis_exploratorio`), y con esos hallazgos se diseñan el mapa, las barras y la serie temporal (notebook `04_figuras_y_resultados`), siguiendo los criterios del marco conceptual.

**Evaluación y despliegue.** Se construirá el tablero en Power BI y el mapa final en QGIS. Luego se probará con usuarios para medir si encuentran la información que buscan, cuánto tardan y si entienden el indicador (ISO 9241-210). Con esos resultados se harán los ajustes finales y se publicará el tablero.

**Herramientas:**

| Herramienta | Uso | Justificación |
|---|---|---|
| Python en Google Colab | Descarga, ETL y EDA | Gratuito, reproducible y no requiere instalación |
| pandas y geopandas | Manejo de tablas y mapas | Librerías estándar para datos tabulares y geográficos |
| matplotlib | Gráficos del EDA | Control total del diseño de cada figura |
| QGIS | Mapa final | Software libre especializado en información geográfica |
| Power BI | Tablero interactivo | Permite filtros y detalle a pedido sin programar |

> **✅ Bien:** las etapas originales estaban bien planteadas.
>
> **✏️ Modificar:** el texto decía "cinco etapas" pero solo listaba cuatro, y faltaba **evaluación y despliegue**, que el temario pide explícitamente. Ahora son cuatro subsecciones que siguen exactamente el temario.
>
> **➕ Agregado:** la parte de evaluación y despliegue en 6.4, la referencia al notebook que corresponde a cada etapa y la tabla de herramientas con su justificación.

---

## 7. Datos y proceso ETL

### 7.1. Fuentes de datos, licencia y condiciones de uso

| Conjunto de datos | Institución | Formato | Uso en el proyecto |
|---|---|---|---|
| DSNY Monthly Tonnage Data (`ebb7-mvp5`) | Department of Sanitation (DSNY) | CSV | Toneladas mensuales de basura común, papel y MGP por distrito |
| Community Districts (`5crt-au7u`) | Department of City Planning (DCP) | GeoJSON | Límites geográficos de los distritos |
| New York City Population by Community Districts (`xi7c-iiu2`) | Department of City Planning (DCP) | CSV | Nombres de los distritos y población censal 1970-2010 |

Las tres fuentes son oficiales y se obtienen del portal NYC Open Data. Antes de usarlas se revisaron sus metadatos (institución responsable, fecha de actualización y descripción de columnas) y el diccionario oficial del DSNY.

**Licencia y condiciones de uso.** Los datos de NYC Open Data son públicos y pueden reutilizarse libremente bajo los Términos de Uso del portal, que piden citar la fuente y no presentar los datos como respaldados por la ciudad. No contienen información personal: todos los valores están agregados por distrito. En cada gráfico se indica "Fuente: DSNY y DCP; elaboración propia".

> **✅ Bien:** la tabla de fuentes.
>
> **➕ Agregado:** la institución, el código del conjunto en el portal (permite encontrarlo de nuevo) y un párrafo concreto sobre la licencia, que el temario pide y en el original solo se mencionaba de pasada.

### 7.2. Diccionario y estructura de los datos

| Variable | Fuente | Tipo | Descripción |
|---|---|---|---|
| `MONTH` | Tonelaje | Texto (`AAAA / MM`) | Mes y año del registro |
| `BOROUGH` | Tonelaje | Texto | Nombre del *borough* (una de las cinco grandes divisiones de la ciudad: Manhattan, Bronx, Brooklyn, Queens y Staten Island) |
| `BOROUGH_ID` | Tonelaje | Entero | Código del *borough* (1 a 5) |
| `COMMUNITYDISTRICT` | Tonelaje | Entero | Número del distrito comunitario dentro del *borough* |
| `REFUSETONSCOLLECTED` | Tonelaje | Decimal | Toneladas de basura común recolectadas en viviendas e instituciones |
| `PAPERTONSCOLLECTED` | Tonelaje | Decimal | Toneladas de papel separado en origen |
| `MGPTONSCOLLECTED` | Tonelaje | Decimal | Toneladas de metal, vidrio, plástico y cartones de bebidas separados en origen |
| `boro_cd` | Mapa / creada | Entero | Código de distrito (*borough* × 100 + distrito); clave para unir las tres tablas |
| `geometry` | Mapa | Polígono | Límites geográficos de cada distrito |
| `CD Name` | Población | Texto | Nombre del distrito comunitario (barrios que lo forman) |
| `2010 Population` | Población | Entero | Población registrada en el censo de 2010 |

**Variables creadas en el ETL:** `month` (fecha), `total` (suma de las tres fracciones) y `separacion_pct` (indicador de separación).

Los datos están en formato CSV y GeoJSON. Las cantidades se mantienen en la unidad original del DSNY, que son **toneladas cortas** (1 tonelada corta = 907,2 kg). No se convierten a toneladas métricas porque el indicador principal es un porcentaje y no cambia con la unidad.

> **✅ Bien:** el diccionario cubría todas las variables usadas.
>
> **✏️ Modificar:**
> - `BOROUGH` estaba descrito como "Nombre de la ciudad". Es un error: un *borough* es una división **dentro** de la ciudad de Nueva York (similar a un macrodistrito). Lo mismo para `BOROUGH_ID`.
> - Se aclaró que la unidad son **toneladas cortas** y por qué no se convierten. El texto original decía "sin realizar conversiones a toneladas métricas" sin explicar la razón.
>
> **➕ Agregado:** columna de tipo de dato, columna de fuente y las variables creadas en el ETL.

### 7.3. Datos en bruto y datos procesados

Los archivos originales se guardan sin cambios en `data/raw/`, para poder volver a la fuente en cualquier momento y comprobar que el procesamiento no alteró los valores. El archivo de tonelaje contiene **25.355 registros y 12 columnas**, con datos desde 1990 hasta octubre de 2026. El mapa tiene 71 polígonos y la tabla de población, 59 filas.

Los datos procesados se guardan en `data/processed/`. Al seleccionar el periodo de enero de 2015 a septiembre de 2026 quedaron **8.319 registros**, que corresponden exactamente a 59 distritos por 141 meses, sin ningún hueco. Separar las dos carpetas permite saber en todo momento qué información viene de la fuente y qué se generó en el procesamiento.

| | Datos en bruto | Datos procesados |
|---|---|---|
| Carpeta | `data/raw/` | `data/processed/` |
| Registros de tonelaje | 25.355 | 8.319 |
| Periodo | 1990 – octubre 2026 | enero 2015 – septiembre 2026 |
| Columnas de tonelaje | 12 (incluye 5 de orgánicos) | 10 (3 fracciones, indicador, nombre y población) |
| Archivos | `tonnage.csv`, `districts.geojson`, `population.csv`, metadatos y diccionario | `residuos_mensuales.csv`, `distritos_indicadores.geojson`, `poblacion_historica.csv`, `comparacion_ultimo_mes.csv` |

**¿Por qué termina en septiembre de 2026 si la fuente llega a octubre?** El periodo se fijó en el acta de alcance, cuando septiembre era el último mes publicado. Además, el mes más reciente de la fuente puede corregirse en publicaciones posteriores; trabajar con meses ya consolidados evita que las cifras cambien después de presentadas.

> **✅ Bien:** las cifras (25.355 y 8.319) son correctas y coinciden con los notebooks.
>
> **➕ Agregado:** la tabla comparativa, la lista de archivos de cada carpeta y la justificación del corte en septiembre (la fuente ya tiene octubre, y un revisor podría preguntarlo).

### 7.4. Reglas de limpieza, transformación, integración y carga

| Paso | Qué se hizo | Justificación |
|---|---|---|
| Conversión de fechas | `MONTH` pasó de texto (`2026 / 10`) a fecha | Para ordenar, filtrar y agrupar por mes y año |
| Creación de la clave | `boro_cd = BOROUGH_ID × 100 + COMMUNITYDISTRICT` | Es el mismo código que usan el mapa y la población; sin él no se pueden unir |
| Selección del periodo | Enero 2015 a septiembre 2026 | Periodo reciente y completo; los vacíos de la fuente están antes de 2005 |
| Selección de variables | Solo basura común, papel y MGP | Los orgánicos tienen entre 85 % y 97 % de vacíos |
| Control de calidad | Se revisaron duplicados, distrito-mes repetidos, nulos, negativos y meses faltantes | Ninguno apareció en el periodo, así que no fue necesario eliminar ni imputar registros |
| Cálculo del indicador | Separación (%) = (papel + MGP) / total × 100 | Permite comparar distritos de distinto tamaño |
| Integración con población | Unión por `boro_cd`, muchos registros a un distrito | Agrega nombre y población; se verificó que ningún registro quedara sin nombre |
| Integración con el mapa | Unión por `boro_cd` del último mes, un polígono a un distrito | Los 12 polígonos sin datos (parques, aeropuertos) quedan vacíos y se muestran en gris |
| Carga | Se exportaron CSV y GeoJSON | Formatos que leen directamente Power BI y QGIS |

**Regla para agregar el indicador.** Cuando se calcula la separación de un año, un *borough* o todo el periodo, primero se suman las toneladas y después se divide. No se promedian los porcentajes mensuales, porque eso daría el mismo peso a un distrito pequeño que a uno grande.

> **✅ Bien:** los pasos originales (periodo, tipos, control de calidad, indicador y exportación) eran correctos.
>
> **✏️ Modificar:** en "Control de calidad" e "Indicador" decía "papel y otros"; cambiar por "papel y MGP".
>
> **➕ Agregado:** la creación de la clave, la selección de variables, las dos uniones (que el temario pide como "integración" y no estaban), la columna de justificación separada y la regla para agregar el indicador.

### 7.5. Evidencia del proceso ETL

El proceso se ejecutó en notebooks de Python, que pueden abrirse en Google Colab. Cada notebook muestra el código junto con su resultado, de modo que cualquier persona puede repetirlo y obtener las mismas cifras.

| Evidencia | Notebook | Qué se ve |
|---|---|---|
| Descarga de las fuentes | `01_fuentes_y_descarga` | Los seis archivos descargados y su tamaño |
| Metadatos y licencia | `01_fuentes_y_descarga` | Institución que publica cada conjunto y fecha de actualización |
| Diccionario oficial | `01_fuentes_y_descarga` | Definición de las tres fracciones según el DSNY |
| Conversión de fechas y clave | `02_limpieza_e_integracion` | Tabla con `MONTH` original, fecha convertida y `boro_cd` |
| Filtro del periodo | `02_limpieza_e_integracion` | 8.319 registros, 59 distritos, 141 meses |
| Control de calidad | `02_limpieza_e_integracion` | 0 duplicados, 0 nulos, 0 negativos, 8.319 de 8.319 combinaciones |
| Uniones | `02_limpieza_e_integracion` | 0 distritos sin nombre; 59 de 71 polígonos con dato |
| Archivos exportados | `02_limpieza_e_integracion` | Resumen final del proceso |

**Resultado del control de calidad:**

| Control | Resultado |
|---|---|
| Registros en bruto | 25.355 |
| Registros en el periodo | 8.319 |
| Distritos | 59 |
| Meses | 141 |
| Duplicados exactos | 0 |
| Distrito-mes repetidos | 0 |
| Valores nulos en las tres fracciones | 0 |
| Valores negativos | 0 |
| Combinaciones distrito-mes faltantes | 0 |
| Polígonos sin dato (no son distritos residenciales) | 12 |

> **➕ Agregado:** toda la sección (decía *completar…*).
>
> **✏️ Para la versión final:** insertar capturas de pantalla de las celdas indicadas en la tabla. Las más importantes son la celda de control de calidad y la del resumen final del notebook 02. Si se ejecuta en Colab, que se vea la barra de Colab en la captura, porque el acta dice que se usará Colab.

---

## 8. Análisis exploratorio de datos (EDA)

El análisis exploratorio se hizo en el notebook `03_analisis_exploratorio`. Se revisó primero el archivo en bruto completo (1990–2026), para entender la fuente y justificar las decisiones de limpieza, y después la tabla procesada (2015–2026).

### 8.1. Descripción de la estructura y tipo de variables

La tabla procesada tiene 8.319 filas y 10 columnas. Cada fila representa un distrito comunitario en un mes.

| Tipo de variable | Variables | Comentario |
|---|---|---|
| Temporal | `month` | 141 meses, de enero 2015 a septiembre 2026 |
| Categórica (nominal) | `BOROUGH`, `nombre` | 5 *boroughs* y 59 nombres de distrito |
| Categórica (identificador) | `boro_cd` | Aunque es un número, no se suma ni se promedia: es un código |
| Numérica continua | `basura`, `papel`, `mgp`, `total` | Toneladas cortas por distrito y mes |
| Numérica continua (proporción) | `separacion_pct` | Entre 0 y 100 |
| Numérica discreta | `poblacion_2010` | Un valor fijo por distrito |

Los 59 distritos se reparten así: Brooklyn 18, Queens 14, Manhattan 12, Bronx 12 y Staten Island 3.

En el archivo en bruto, las columnas de basura común no tienen vacíos, pero papel tiene un 9,6 % y MGP un 8,2 %. Las cinco columnas de orgánicos (residenciales, escolares, hojas, árboles de Navidad y otros) tienen entre 85 % y 97 % de vacíos, porque esos programas empezaron mucho después que el reciclaje.

> **➕ Agregado:** toda la sección (decía *completar…*). Evidencia: celdas de estructura del notebook 03.

### 8.2. Identificación de valores faltantes, duplicados, inconsistencias y datos atípicos

**Valores faltantes.** En el archivo en bruto, los vacíos de papel y MGP se concentran entre 1990 y 2004. El año 1990 está incompleto (89 registros) y entre 1991 y 1993 faltan cientos de registros de ambas fracciones. Entre 2003 y 2004 hay vacíos de MGP, que coinciden con el periodo en que la ciudad suspendió parte del reciclaje de vidrio y plástico. Desde 2005 no hay vacíos en las tres fracciones, y en el periodo de estudio tampoco.

**Duplicados.** No hay filas duplicadas ni combinaciones distrito-mes repetidas, ni en el archivo en bruto ni en el procesado.

**Inconsistencias.** Se verificó que todos los códigos de distrito del tonelaje existan en la tabla de población y en el mapa: ningún registro quedó sin nombre después de la unión. No se encontraron contradicciones. Fuera del periodo hay dos registros con papel o MGP igual a cero (diciembre de 2003 en Brooklyn 3 y julio de 1997 en Manhattan 2), que no afectan al análisis.

**Datos atípicos.** Se usó la regla del rango intercuartílico (IQR): un valor es atípico si está más de 1,5 veces el IQR por debajo del primer cuartil o por encima del tercero.

| Variable | Atípicos | % del total | Distrito con más atípicos |
|---|---|---|---|
| Basura común | 200 | 2,4 % | 407 (Flushing, Queens) |
| Papel | 277 | 3,3 % | 108 (Upper East Side, Manhattan) |
| MGP | 188 | 2,3 % | 412 (Jamaica, Queens) |
| Separación (%) | 46 | 0,6 % | 306 (Park Slope, Brooklyn) |

Los atípicos se repiten siempre en los mismos distritos, que son los más grandes o los que tienen hábitos muy marcados. Son valores reales, no errores de registro.

Además, en la serie mensual se ven dos meses con valores fuera de lo común en muchos distritos a la vez: **febrero de 2015**, con menos basura de lo normal (febrero tiene menos días y ese invierno fue especialmente frío), y **abril de 2020**, que coincide con el confinamiento por la pandemia.

> **➕ Agregado:** toda la sección (decía *completar…*). Evidencia: celdas de calidad del notebook 03 y figura `eda_serie_bruta.png`.

### 8.3. Análisis de distribuciones, frecuencias, medidas descriptivas y tendencias

**Medidas descriptivas (registros distrito-mes, 2015–2026):**

| Variable | Media | Mediana | Desv. estándar | Mínimo | Máximo | Coef. variación | Asimetría |
|---|---|---|---|---|---|---|---|
| Basura común (t) | 4.229,5 | 4.008,0 | 1.612,9 | 1.009,8 | 10.413,9 | 38,1 % | 0,78 |
| Papel (t) | 446,0 | 403,2 | 215,0 | 76,6 | 1.516,0 | 48,2 % | 1,21 |
| MGP (t) | 417,7 | 392,5 | 178,7 | 83,2 | 1.254,6 | 42,8 % | 0,76 |
| Separación (%) | 17,4 | 17,1 | 5,4 | 3,2 | 35,4 | 30,9 % | 0,27 |

Las tres fracciones tienen **asimetría positiva**: la mayoría de los distritos recoge cantidades moderadas y unos pocos recogen mucho más. Por eso la media queda por encima de la mediana, y para comparar se prefiere la mediana. El papel es la fracción que más varía entre distritos (coeficiente de variación del 48 %), lo que sugiere que su separación depende mucho de cada zona. La separación, en cambio, tiene una distribución casi simétrica alrededor del 17 %.

**Tendencia anual (59 distritos):**

| Año | Basura común (t) | Papel (t) | MGP (t) | Separación |
|---|---|---|---|---|
| 2015 | 2.947.236 | 318.934 | 256.594 | 16,3 % |
| 2016 | 2.965.748 | 327.347 | 281.312 | 17,0 % |
| 2017 | 2.966.507 | 319.901 | 288.829 | 17,0 % |
| 2018 | 3.023.434 | 331.557 | 295.184 | 17,2 % |
| 2019 | 2.984.428 | 323.225 | 302.838 | 17,3 % |
| 2020 | 3.122.075 | 333.159 | 352.538 | 18,0 % |
| 2021 | 3.166.925 | 334.309 | 325.118 | 17,2 % |
| 2022 | 2.992.120 | 308.675 | 296.855 | 16,8 % |
| 2023 | 2.956.232 | 296.424 | 286.541 | 16,5 % |
| 2024 | 2.981.772 | 293.890 | 283.797 | 16,2 % |
| 2025 | 2.886.143 | 299.458 | 287.555 | 16,9 % |
| 2026* | 2.192.511 | 223.244 | 217.395 | 16,7 % |

\* Solo enero a septiembre.

La separación subió poco a poco de 16,3 % en 2015 a 17,3 % en 2019, tuvo su punto más alto en 2020 (18,0 %) y luego bajó hasta 16,2 % en 2024. El pico de 2020 coincide con la pandemia: más gente en casa generó más residuos domiciliarios, en especial envases (el MGP creció un 16 % ese año). En resumen, en once años la ciudad no ha logrado subir la separación de forma sostenida.

**Estacionalidad.** La basura común es más baja en febrero (unas 3.583 t promedio por distrito, en parte porque el mes es más corto) y más alta en junio (4.525 t). El papel tiene un pico claro en diciembre (527 t frente a unas 440 t el resto del año), que se explica por las cajas y envoltorios de las fiestas.

> **➕ Agregado:** toda la sección (decía *completar…*). Evidencia: figuras `eda_distribuciones.png`, `eda_tendencia_anual.png` y `eda_estacionalidad.png`.

### 8.4. Exploración de relaciones, segmentaciones, correlaciones y patrones iniciales

**Segmentación por *borough* (todo el periodo):**

| *Borough* | Distritos | Separación |
|---|---|---|
| Bronx | 12 | 13,4 % |
| Brooklyn | 18 | 15,6 % |
| Queens | 14 | 18,5 % |
| Staten Island | 3 | 18,8 % |
| Manhattan | 12 | 19,8 % |

El Bronx es el *borough* con menor separación y Manhattan el de mayor. La diferencia entre ambos es de más de seis puntos porcentuales, que es grande si se considera que el promedio de la ciudad está alrededor del 17 %.

**Segmentación por distrito (todo el periodo):**

| Menor separación | % | Mayor separación | % |
|---|---|---|---|
| 202 Hunts Point, Longwood | 4,8 | 306 Park Slope, Carroll Gardens | 30,0 |
| 110 Central Harlem | 8,2 | 101 Battery Park City, Tribeca | 28,5 |
| 316 Brownsville, Ocean Hill | 9,1 | 107 Upper West Side | 27,0 |
| 308 Crown Heights North | 9,2 | 106 Stuyvesant Town, Turtle Bay | 26,6 |
| 201 Melrose, Mott Haven | 10,6 | 302 Brooklyn Heights, Fort Greene | 25,5 |

La distancia entre el distrito que menos separa (4,8 %) y el que más separa (30,0 %) es de más de seis veces. Los distritos con menor separación están concentrados en el sur del Bronx, Harlem y el centro-este de Brooklyn, mientras que los de mayor separación están en el bajo y oeste de Manhattan y en el noroeste de Brooklyn. Este patrón territorial es justamente lo que el mapa debe mostrar.

**Correlaciones (Spearman).** Se usó el coeficiente de Spearman porque las variables no siguen una distribución normal y porque mide relaciones que no necesariamente son lineales.

| | Basura | Papel | MGP | Separación | Población 2010 |
|---|---|---|---|---|---|
| Basura | 1,00 | 0,43 | 0,58 | **−0,33** | **0,68** |
| Papel | | 1,00 | 0,75 | 0,60 | 0,56 |
| MGP | | | 1,00 | 0,43 | **0,77** |
| Separación | | | | 1,00 | **0,06** |

Tres patrones destacan:

- La población se relaciona fuertemente con las toneladas (0,68 con basura y 0,77 con MGP): los distritos más poblados recogen más, como era de esperar.
- La población **no** se relaciona con la separación (0,06). Es decir, que un distrito sea grande o pequeño no explica si separa más o menos. Esto confirma que el indicador relativo es el adecuado para comparar.
- La basura común tiene una relación negativa moderada con la separación (−0,33): los distritos que generan más basura tienden a separar menos.

> **➕ Agregado:** toda la sección (decía *completar…*). Evidencia: figuras `eda_borough.png` y `eda_relaciones.png`.
>
> **⚠️ Cuidado al redactar conclusiones:** las correlaciones no prueban causas. Se puede decir "los distritos con más basura tienden a separar menos", pero no "separan menos porque generan más basura".

### 8.5. Visualizaciones de datos en bruto

Se elaboraron seis gráficos exploratorios. Su objetivo no es presentar resultados finales sino entender los datos, por lo que tienen un diseño sencillo.

| Figura | Archivo | Qué muestra |
|---|---|---|
| Serie en bruto 1990–2026 | `eda_serie_bruta.png` | El tonelaje mensual de las tres fracciones en toda la historia de la fuente; la franja azul marca el periodo de estudio |
| Distribuciones | `eda_distribuciones.png` | Histogramas de basura, papel, MGP y separación, con la mediana marcada |
| Tendencia anual | `eda_tendencia_anual.png` | La separación de cada año completo |
| Estacionalidad | `eda_estacionalidad.png` | El promedio por mes del año para cada fracción |
| Separación por *borough* | `eda_borough.png` | Diagramas de caja que muestran la mediana, la dispersión y los atípicos de cada *borough* |
| Relación basura–separación | `eda_relaciones.png` | Un punto por distrito: volumen de basura frente a separación |

La serie en bruto es la más importante para justificar la limpieza: en ella se ven las caídas de papel y MGP antes de 2005, que coinciden con los años con vacíos, y la estabilidad de la serie a partir de esa fecha.

> **➕ Agregado:** toda la sección (decía *completar…*).
>
> **✏️ Para la versión final:** insertar en el Word al menos la serie en bruto, las distribuciones y la figura por *borough*, con su título, número de figura y la nota "Fuente: DSNY; elaboración propia".

### 8.6. Interpretación de hallazgos preliminares y decisiones de limpieza o transformación

| Hallazgo | Decisión tomada |
|---|---|
| Papel y MGP tienen vacíos entre 1990 y 2004 | Trabajar con 2015–2026, un periodo reciente y completo |
| Los orgánicos tienen entre 85 % y 97 % de vacíos | Dejarlos fuera del indicador y aclararlo en los gráficos |
| No hay duplicados, nulos ni negativos en el periodo | No eliminar ni imputar registros |
| Los atípicos se concentran en distritos grandes y son valores reales | Conservarlos |
| Las toneladas tienen asimetría positiva | Usar mediana y cuartiles para comparar y para definir prioridades |
| La población se relaciona con las toneladas pero no con la separación | Comparar distritos con el porcentaje de separación, no con toneladas absolutas |
| La separación de la ciudad está estancada entre 16 % y 18 % | Mostrar la serie temporal en el tablero para que se vea el estancamiento |
| El año 2020 tiene un comportamiento distinto | Marcarlo en la serie y no usarlo solo para sacar conclusiones |
| Bronx y el centro-este de Brooklyn separan menos | Priorizarlos visualmente en el mapa y en el tablero |
| Hay 12 polígonos sin datos | Mostrarlos en gris con la leyenda "sin dato", no como valor cero |

Como resultado de estos hallazgos se definió un criterio inicial para identificar distritos a revisar: aquellos que, en el mismo mes, están a la vez en el 25 % con **más basura común** y en el 25 % con **menor separación**. En septiembre de 2026 cumplen esta condición siete distritos: **103** (Lower East Side, Chinatown), **110** (Central Harlem), **202** (Hunts Point, Longwood), **305** (East New York), **308** (Crown Heights North), **311** (Bensonhurst) y **312** (Borough Park). Este criterio es exploratorio: sirve para decidir dónde mirar primero, pero no demuestra que el servicio en esos distritos sea malo, y debe validarse con el personal municipal.

> **➕ Agregado:** toda la sección (decía *completar…*). Evidencia: celda final del notebook 03 y última celda del notebook 04.

---

## 9. Referencias

- Bertin, J. (1983). *Semiology of graphics: Diagrams, networks, maps* (W. J. Berg, Trad.). University of Wisconsin Press. (Obra original publicada en 1967).
- Chapman, P., Clinton, J., Kerber, R., Khabaza, T., Reinartz, T., Shearer, C. y Wirth, R. (2000). *CRISP-DM 1.0: Step-by-step data mining guide*. SPSS.
- Cleveland, W. S. y McGill, R. (1984). Graphical perception: Theory, experimentation, and application to the development of graphical methods. *Journal of the American Statistical Association, 79*(387), 531–554.
- Few, S. (2006). *Information dashboard design: The effective visual communication of data*. O'Reilly.
- Harrower, M. y Brewer, C. A. (2003). ColorBrewer.org: An online tool for selecting colour schemes for maps. *The Cartographic Journal, 40*(1), 27–37.
- Kaza, S., Yao, L., Bhada-Tata, P. y Van Woerden, F. (2018). *What a waste 2.0: A global snapshot of solid waste management to 2050*. Banco Mundial.
- Knaflic, C. N. (2015). *Storytelling with data: A data visualization guide for business professionals*. Wiley.
- Munzner, T. (2009). A nested model for visualization design and validation. *IEEE Transactions on Visualization and Computer Graphics, 15*(6), 921–928.
- Munzner, T. (2014). *Visualization analysis and design*. CRC Press.
- Segel, E. y Heer, J. (2010). Narrative visualization: Telling stories with data. *IEEE Transactions on Visualization and Computer Graphics, 16*(6), 1139–1148.
- Shneiderman, B. (1996). The eyes have it: A task by data type taxonomy for information visualizations. En *Proceedings of the IEEE Symposium on Visual Languages* (pp. 336–343).
- Tufte, E. R. (2001). *The visual display of quantitative information* (2.ª ed.). Graphics Press.
- Ware, C. (2021). *Information visualization: Perception for design* (4.ª ed.). Morgan Kaufmann.
- Estado Plurinacional de Bolivia. (2015). *Ley N.º 755 de Gestión Integral de Residuos*.
- NYC Department of Sanitation. (2026). *DSNY Monthly Tonnage Data* [Conjunto de datos]. NYC Open Data. https://data.cityofnewyork.us/d/ebb7-mvp5
- NYC Department of City Planning. (2026). *Community Districts* [Conjunto de datos]. NYC Open Data. https://data.cityofnewyork.us/d/5crt-au7u
- NYC Department of City Planning. (2024). *New York City Population by Community Districts* [Conjunto de datos]. NYC Open Data. https://data.cityofnewyork.us/d/xi7c-iiu2

> **➕ Agregado:** el documento original no tenía referencias. Todo autor y norma que se menciona en el texto debe aparecer aquí.

---

## Acta de alcance individual

**Estudiante:** Robert Josue Huanca Clares  **Fecha:** 30-10-2026
**Ruta:** 14 – Residuos Sólidos

**Decisión que se apoyará:**
Identificar qué distritos necesitan mayor atención en la separación de residuos y cuáles podrían requerir mejoras en el servicio de recolección. Para eso se analizará la cantidad de residuos recolectados, su tipo y cómo han cambiado estos valores en el tiempo. Con esa información se podrá priorizar los distritos donde se necesitan mejores estrategias de reciclaje y recolección.

**Usuario o audiencia:**
El proyecto está dirigido al personal encargado de planificar la recolección y el reciclaje en el servicio de aseo municipal. También puede ser útil para las juntas de vecinos, porque les permitirá conocer la situación de su distrito.

**Fuente(s), licencia y periodo:**
Se utilizarán datos reales del Departamento de Saneamiento de Nueva York (DSNY) y del Departamento de Planificación Urbana (DCP), disponibles públicamente en NYC Open Data y reutilizables bajo los Términos de Uso del portal. El análisis abarca de enero de 2015 a septiembre de 2026.

**Herramienta principal y evidencia ETL:**
Se utilizará Python en Google Colab para descargar, revisar, limpiar y preparar los datos; QGIS para trabajar los mapas; y Power BI para el tablero final.
Como evidencia se presentarán capturas de los notebooks en Google Colab: la descarga desde NYC Open Data, el control de calidad (duplicados, nulos, negativos y meses faltantes), las uniones con el mapa y la población, y los archivos exportados para QGIS y Power BI.

**Visual obligatorio de mi ruta:**
Un mapa que permita identificar los distritos con mayor y menor separación de residuos, acompañado de un gráfico de barras para comparar los valores con precisión y de una serie mensual que muestre cómo varían las cantidades de cada tipo de residuo.

**Norma focal y riesgo principal:**
Se tomará como referencia la norma ISO 14001, relacionada con la gestión ambiental, para usar la proporción de residuos separados como indicador de desempeño y proponer su seguimiento periódico. El principal riesgo es que los datos no representan solo la basura de los hogares, sino que incluyen también residuos de instituciones públicas; por eso los resultados se presentarán como separación del servicio municipal y no como comportamiento exclusivo de los hogares.

**Aprobación docente:**

_____________________________________________

> **✅ Bien:** el acta está completa y coherente con el resto del informe.
>
> **✏️ Modificar:**
> - En "Herramienta principal" decía que habría evidencias de "cómo se corrigieron datos incompletos o duplicados". El ETL encontró **cero** duplicados y nulos en el periodo, así que esa frase no se puede cumplir. Se reemplazó por la evidencia del *control* que se hizo.
> - En "Norma focal" la última oración ("Esto permitirá utilizar indicadores ambientales…") no se relacionaba con el riesgo que se acababa de mencionar. Se cambió por cómo se trata ese riesgo.
> - Se agregó al DCP como fuente, porque el mapa y la población vienen de ahí.
