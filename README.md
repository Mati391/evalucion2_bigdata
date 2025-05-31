# evalucion2_bigdata
Integrantes: Matias Sanchez y Jonathan Avila
# Proyecto de Análisis de Avistamientos de OVNIs

#Creacion del DataSet
Nombre del archivo: `ufo_sighting.csv`
Tamaño: 12.9 MB  
Registros: 25.699  
Columnas: 11  

1. Se accede a **Google Cloud Platform** y se ingresa al servicio Cloud Storage.
2. Se crea un nuevo bucket con el nombre:  
   `jonathan_avila-yuliana_benitez-matias_sanchez`.
   - Tipo de ubicación: Multiregión (us)
   - Clase de almacenamiento: Standard
   - Control de acceso: No público
3. Una vez creado, se accede al bucket y se sube el archivo `ufo_sighting.csv`.
   ![Creacion de bucket y subida csv](https://github.com/user-attachments/assets/bb05351a-6d51-4e77-afe8-db6b6e136355)
4- Verificación del archivo `ufo_sighting.csv` en el bucket**
Después de subir el archivo al bucket, se visualiza en el panel de objetos de Google Cloud Storage para confirmar:
![data set ovni](https://github.com/user-attachments/assets/0c422a73-9f72-4fc7-a580-adbd64093058)

#Limpieza y transformacion de datos en Cloud Dataprep
Una vez cargado el dataset, se utiliza Cloud Dataprep para preparar los datos antes del análisis

1. Eliminación de valores faltantes** en las columnas clave:
`Date_time`, `state/province`, `country`, `UFO_shape`, `description`

2. Renombramiento de columna:
`state/province` → `state_province`

3. Validación del esquema final:
![limpieza de datos](https://github.com/user-attachments/assets/78eb8e9c-3004-4721-b27c-b39f9bc6eda3)

flujo completo del dataset desde su carga hasta su disponibilidad en BigQuery:
![flujo dataset analytic](https://github.com/user-attachments/assets/d45aeacc-b303-4d08-8670-019309d69e65)

#Tabla de avistamientos

1-Después de ejecutar la receta de limpieza en Cloud Dataprep, La imagen muestra la tabla final ya disponible para consultas SQL.
![tabla de avistamientos y confirmacion de datos](https://github.com/user-attachments/assets/f9387969-c49e-4a47-aefd-4e2cce81c7d2)

#Configuracion de permisos
Se asignan las cuentas y servicios dentro del proyecto de google cloud, Esta configuración garantiza que las herramientas tengan los accesos necesarios para operar correctamente.
![permisosdatafrod](https://github.com/user-attachments/assets/76452845-4b72-437e-9694-faaf2042e70d)

#Analisis de datos

1- Primero se realizo un grafico de torta con las formas mas comunes de avistamientos
Datos mostrados:
Formas de UFO más comunes en EE.UU. y su frecuencia:
Light: 5,440 avistamientos (54.4%)
Triangle: 2,574 (25.74%)
Circle: 2,483 (24.83%)
Fireball: 2,117 (21.17%)
Other: 1,880 (18.8%)
![image](https://github.com/user-attachments/assets/a31e9378-bfd3-46e2-b44b-70ed533649af)
Interpretación:
Los objetos luminosos ("light") son con diferencia los más reportados, representando más de la mitad de todos los avistamientos.
Las formas geométricas definidas (triángulo, círculo) son las siguientes más comunes.
El gráfico muestra solo los 5 tipos principales, excluyendo categorías menos frecuentes.
Consulta SQL usada:
SELECT 
  UFO_shape,
  COUNT(*) AS SightingsFROM 
  `ufo_dataset.ufo_sightings`WHERE 
  country = 'us'GROUP BY 
  UFO_shapeORDER BY 
  Sightings DESCLIMIT 5;

2- El segundo grafico corresponde a uno de barras con los avisos por año
![image](https://github.com/user-attachments/assets/4cb2443f-8009-4d9c-a9c7-f86cfbd826cf)
Interpretacion:
Interpretación:
Los años 2012 y 2013 fueron picos en reportes de avistamientos.
Hay una tendencia general de aumento hacia 2012-2013, con algunos altibajos.
Los datos muestran solo parte del conjunto total (11 de 63 registros anuales mostrados).
Consulta SQL usada:
SELECT 
  EXTRACT(YEAR FROM DATETIME(Date_time)) AS Year,
  COUNT(*) AS SightingsFROM 
  `ufo_dataset.ufo_sightings`WHERE 
  country = 'us'GROUP BY 
  YearORDER BY 
  Sightings DESC;

3- El tercer grafico es un mapa de avistamientos ordenados de mayor a menor
![image](https://github.com/user-attachments/assets/86ae55de-b404-499e-a1ae-d25ac1e25429)


 
