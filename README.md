# Universalizacion-de-Proyectos-y-Representaciones (2023)
## Reporte de Tomas Clandestinas 2019-2022 | PEMEX | Oficina de enlace de la subgerencia de gestión de derechos de uso y ocupación superficial (SGDUOS) Salamanca.

### Link al Dasboard: 

## Resumen del Proyecto

Una cuestión que requiere atención dentro de las oficinas donde los reportes de campo son vitales para llevar las operaciones, es la necesidad de tener consistencias en los datos a la hora de su captura, es decir, que sean del mismo tipo, estén en el mismo formato o 
que contengan la menor cantidad de errores de escritura, naturalmente en la practica esto es muy diferente, por una razón u otra, los datos capturados contienen una serie de inconsistencias que hacen que un análisis de estos sea muy inexacto y en muchas ocasiones
imposible de realizar. 
A finales de 2023 la oficina de SGDUOS Salamanca, requirió una forma de organizar y visualizar de una manera mas eficiente el registro o el estado de sus proyectos de legalización de ocupación superficial. compararlos con otros sectores y llevar un registro de ellos. Para esto desde 
una dependencia de la cual no se especificare el nombre,  se intento elaborar un proyecto de "universalización de proyectos" es decir recopilar y unificar todos los proyectos de legalización de ocupación superficial en un solo lugar. Aunque la intención e iniciativa era bastante
buena, la herramienta elegida no fue la mejor opción para una consolidación completa, (Microsoft Excel). Y no porque no fuese una herramienta potente y útil (como se describirá mas a detalle), si no por las implicaciones que acarrea casi por defecto su uso, que podría resumir en:

- la información ingresada y capturada, proviene de reportes de campo, muchas veces no se estandariza, ni se valida
- el esquema de las tablas u hojas de calculo no sigue un modelo único, muchas veces las columnas aportan información redundante o duplicada
- los tipos de datos a traves de las columnas vienen mezclados, ya que no existe una restricción de datos (data constraint)

En general, lo que se esperaría de hojas de datos cuya información proviene de reportes de campo.

Asi que para poder presentar un reporte ejecutivo acerca del estado y de los avances en materia de contratos de ocupación superficial, la información tuvo que ser primero extraída, claro, estandarizada y transformada.


objetivo: 
- Extraer la información de diversas hojas de calculo.
- Establecer un modelo canónico para las tablas que contendrá nuestro modelo, con el fin de que este modelo sea referencia para la información que debe extraerse.
- Normalizar y validar la información contenida.
- Crear una base de datos de la cual se puedan disponer los datos para la elaboración de un reporte ejecutivo.

## Arquitectura

## Raw/Source

La información disponible se encuentra en hojas de calculo que contienen información básica de los proyectos de legalización: nombre del propietario, corredor al que pertenecen, cadenamiento, estatus de legalización y fecha. El problema de facto que tiene es que como es información recopilada y capturada de reportes de campo, la información presenta inconsistencias. Y al elaborar hojas de calculo o algún documento que contenga esa información con el fin de almacenarla, esta no se normaliza o se valida.
Otra característica que tienen las hojas de cálculo, es que cada una contiene columnas que no están en un orden predeterminado, si bien lo que se busca es que todas contengan la misma información, muchas veces, las columnas están en diferente orden o incluso están duplicadas.

## Bronze

se establece e

Es por esto que antes de extraer la información, se determino el uso de un modelo canónico que se podría utilizar como base para la organización de la información en una base de datos, y asi definir las columnas a usar,
para esto simplemente se realizo una evaluación de las columnas con la mayor cantidad de información relevante, y que estuviese presente en la mayoria de las hojas de calculo.
Después de esto, se definió el tipo datos de cada columna, con el fin de restringir el tipo de dato que debe estar ahí. Al final, cada tabla puede ser guardada por separado o consolidada en una tabla maestra ya sea en formato xlsx o en formato parquet con el fin de poseer las tablas en estado "bronce" listas para las transformaciones que se requieran. en la herramienta que resulte mas adecuada

## Silver

En esta etapa, ya establecido el tipo de dato de cada columna, procedemos a la normalización y estandarización. Para la ubicación, no se registraron coordenadas geográficas, pero si un cadenamiento de los ductos en los tramos a legalizar. Uno de los principales retos y que vale la pena mencionar es que los reportes tenían información de cadenamiento tanto en tipo de dato "numérico" como "texto", a simple vista esto no supone un problema, puesto que se pensaría que simplemente cambiando el tipo de dato se solucionaria. Pero no es el caso, si la celda de Excel contiene un dato numérico de esta forma: 150.00+170.00 Excel pensara que lo que se busca es una suma  y al querer copiar este valor o querer transformar la columna a texto nos dara 320, siempre. Incluso utilizando pandas, o transformando la columna con fastparquet, nos sigue arrojando "320", la forma mas sencilla es usando el texto unicode en el mismo excel, y un "copiar y pegar" a un editor de texto...es una solución increíblemente simple y hasta ridícula, pero que hizo que esta tarea pudiese completarse. Otro de los problemas que fueron necesarios abordar para la normalización es el nombre de los municipios, ya que como se ha mencionado, al transcribir reportes de campo directamente a las hojas de calculo los errores de tipografia y ortografia son esperados. Por lo que es comun encontrar valores que se asemejan entre si, pero que no son correctos, estas situaciones se solucionan facilmente con el uso de tablas maestras, en el caso de municipios y poblados de México la fuente oficial y mas confiable son las bases de datos del INEGI. Fuera de eso el proceso de normalización no requiere algo "adicional" salvo las practicas mas comunes, estandarizar, nombres propios, acentos, remover espacios y caracteres no imprimibles, remover fechas imposibles como "1830" o el año "241".

## Gold

En esta capa los datos están listos para su consumo a nivel de presentación ejecutiva, y no requieren ninguna transformación, se elaboro el esquema estrella para la tabla facts y las de dimensiones, las columnas calculadas o medidas extras se establecieron para extraer información especifica, como la referente a solo los ductos que corresponden al area supervisada por SGDUOS Salamanca. 





