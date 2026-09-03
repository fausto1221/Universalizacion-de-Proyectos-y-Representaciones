# Universalización de Proyectos y Representaciones (2023)

## Reporte Ejecutivo del estatus de los proyectos de legalización | PEMEX | Oficina de Enlace de la Subgerencia de Gestión de Derechos de uso y Ocupación Superficial (SGDUOS) Salamanca.

### Link al Dasboard:  https://app.powerbi.com/groups/me/reports/8a8fe389-7ab6-4998-a0e1-b798a3029161/1ec9113dac04ec2f12c0?experience=power-bi

## Resumen del Proyecto

Una cuestión que requiere atención dentro de las oficinas donde los reportes de campo son vitales para llevar las operaciones, es la necesidad de tener consistencias en los datos a la hora de su captura, es decir, que sean del mismo tipo, estén en el mismo formato o 
que contengan la menor cantidad de errores de escritura, naturalmente en la practica esto es muy diferente, por una razón u otra, los datos capturados contienen una serie de inconsistencias que hacen que un análisis de estos sea muy inexacto y en muchas ocasiones
imposible de realizar.  
A finales de 2023 la oficina de SGDUOS Salamanca, requirió una forma de organizar y visualizar de una manera mas eficiente el registro o el estado de sus proyectos de legalización de ocupación superficial. compararlos con otros sectores y llevar un registro de ellos. Para esto desde 
una dependencia de la cual no se especificare el nombre,  se intento elaborar un proyecto de "universalización de proyectos" es decir recopilar y unificar todos los proyectos de legalización de ocupación superficial en un solo lugar. Aunque la intención e iniciativa era bastante
buena, la herramienta elegida no fue la mejor opción para una consolidación completa, (Microsoft Excel). Y no porque no fuese una herramienta potente y útil (como se describirá mas a detalle), si no por las implicaciones que acarrea casi por defecto su uso, que podría resumir en:

- la información ingresada y capturada, proviene de reportes de campo, muchas veces no se estandariza, ni se valida
- el esquema de las tablas u hojas de calculo no sigue un modelo único, muchas veces las columnas aportan información redundante o duplicada
- los tipos de datos a través de las columnas vienen mezclados, ya que no existe una restricción de datos (data constraint)

En general, lo que se esperaría de hojas de datos cuya información proviene de reportes de campo.

Así que para poder presentar un reporte ejecutivo acerca del estado y de los avances en materia de contratos de ocupación superficial, la información tuvo que ser primero extraída, claro, estandarizada y transformada.


objetivo: 
- Extraer la información de diversas hojas de calculo.
- Establecer un modelo canónico para las tablas que contendrá nuestro modelo, con el fin de que este modelo sea referencia para la información que debe extraerse.
- Normalizar y validar la información contenida.
- Crear una base de datos de la cual se puedan disponer los datos para la elaboración de un reporte ejecutivo.

## Arquitectura

<img width="922" height="312" alt="universalizacion" src="https://github.com/user-attachments/assets/e90a1977-85c1-4d98-ac90-9b550dbde8b1" />


### Raw/Source

La información disponible se encuentra en hojas de calculo que contienen información básica de los proyectos de legalización: nombre del propietario, corredor al que pertenecen, cadenamiento, estatus de legalización y fecha. El problema de facto que tiene es que como es información recopilada y capturada de reportes de campo, la información presenta inconsistencias. Y al elaborar hojas de calculo o algún documento que contenga esa información con el fin de almacenarla, esta no se normaliza o se valida.
Otra característica que tienen las hojas de cálculo, es que cada una contiene columnas que no están en un orden predeterminado, si bien lo que se busca es que todas contengan la misma información, muchas veces, las columnas están en diferente orden o incluso están duplicadas.

### Bronze

Antes de extraer la información, se determino el uso de un modelo canónico que se podría utilizar como base para la organización de la información en una base de datos, y así definir las columnas a usar,
para esto simplemente se realizo una evaluación de las columnas con la mayor cantidad de información relevante, y que estuviese presente en la mayoría de las hojas de calculo.
Después de esto, se definió el tipo datos de cada columna, con el fin de restringir el tipo de dato que debe estar ahí. Al final, cada tabla puede ser guardada por separado o consolidada en una tabla maestra ya sea en formato xlsx o en formato parquet con el fin de poseer las tablas en estado "bronce" listas para las transformaciones que se requieran. en la herramienta que resulte mas adecuada

### Silver

En esta etapa, ya establecido el tipo de dato de cada columna, procedemos a la normalización y estandarización. Para la ubicación, no se registraron coordenadas geográficas, pero si un cadenamiento de los ductos en los tramos a legalizar. Uno de los principales retos y que vale la pena mencionar es que los reportes tenían información de cadenamiento tanto en tipo de dato "numérico" como "texto", a simple vista esto no supone un problema, puesto que se pensaría que simplemente cambiando el tipo de dato se solucionaría. Pero no es el caso, si la celda de Excel contiene un dato numérico de esta forma: 150.00+170.00 Excel pensara que lo que se busca es una suma  y al querer copiar este valor o querer transformar la columna a texto nos dará 320, siempre. Incluso utilizando pandas, o transformando la columna con fastparquet, nos sigue arrojando "320", la forma mas sencilla es usando el texto unicode en el mismo excel, y un "copiar y pegar" a un editor de texto...es una solución increíblemente simple y hasta ridícula, pero que hizo que esta tarea pudiese completarse. Otro de los problemas que fueron necesarios abordar para la normalización es el nombre de los municipios, ya que como se ha mencionado, al transcribir reportes de campo directamente a las hojas de calculo los errores de tipografía y ortografía son esperados. Por lo que es común encontrar valores que se asemejan entre si, pero que no son correctos, estas situaciones se solucionan fácilmente con el uso de tablas maestras, en el caso de municipios y poblados de México la fuente oficial y mas confiable son las bases de datos del INEGI. Fuera de eso el proceso de normalización no requiere algo "adicional" salvo las practicas mas comunes, estandarizar, nombres propios, acentos, remover espacios y caracteres no imprimibles, remover fechas imposibles como "1830" o el año "241".

### Gold

En esta capa los datos están listos para su consumo a nivel de presentación ejecutiva, y no requieren ninguna transformación, se elaboro el esquema estrella para la tabla "facts" y las de "dimensiones", las columnas calculadas o medidas extras se establecieron para extraer información especifica, como la referente a solo los ductos que corresponden al área supervisada por SGDUOS Salamanca. 


# Reporte Ejecutivo

## Primera Parte

Primero que nada, a el enlace de SGDUOS, le interesaba conocer el estatus de las legalizaciones de las que se tenia ya un contrato de ocupación superficial, los regímenes de propiedad mas comunes, lo que se había pagado en dichos contratos, una vista general de los corredores con mayor cantidad de proyectos de legalización y por supuesto el porcentaje de los estatus. Los datos arrojaron que el corredor de Salamanca presentaba una lata concentracion de proyectos y también era donde mas se estaba avanzando.

<img width="1371" height="780" alt="image" src="https://github.com/user-attachments/assets/ddd72521-4c4e-42e5-aaff-42a4c5b4c6ac" />

## Segunda Parte

Por lo que la segunda parte del proyecto se concentro en el área de SGDUOS Salamanca, de nuevo las métricas mas relevantes es conocer el avance en los proyectos de legalización, así como lo que queda pendiente y los municipios que requieren mas atención, todo esto con el fin de informarlo a la subgerencia.

<img width="1341" height="754" alt="image" src="https://github.com/user-attachments/assets/bcc5f708-d29e-4d4b-9310-48714018d853" />

## Tercera Parte

Por ultimo, otro de los requerimientos era la capacidad de localizar y visualizar de manera fácil y rápida el estatus actual del proyecto de legalización, para ello y a petición de la oficina de enlace, se estableció que la manera mas fácil de buscar, era por nombre o por el numero de contrato de ocupación superficial (cos), cuando ya esta legalizado. Una pregunta común y valida que surgió es ¿como distinguir entre diferente contratos con la misma persona? la respuesta es que el cadenamiento es distinto, un tramo puede recorrer varios metros en una propiedad pero cada tramo solo se puede legalizar una vez.

<img width="1350" height="759" alt="image" src="https://github.com/user-attachments/assets/621f9f37-abbf-48d4-a208-22336f59dee9" />

## Conclusiones
Si bien se utilizaron librerías de Python para examinar la calidad de los datos, no hizo falta un análisis exploratorio tan detallado o crear funciones para normalizar, ya que el problema principal eran los tipos de datos y los errores de tipografía, cosa que se podían realizar desde Excel, a su vez sirve para ejemplificar que en muchas oficinas, datos de suma importancia para las operaciones se guardan o residen en hojas de calculo.

## Tecnologías 

| Tecnología | Uso |
| --- | --- |
| Python/pandas | manipulación, perfilamiento y exploración de datos |
| Microsoft Excel | Normalización, Transformación |
| PostgreSQL | Base de datos (extracción de datos bronze y silver) |
| Parquet | Almacenamiento Intermedio |
| Power BI | Visualización |

## Autor 

Fausto G. Osorio Cruz










