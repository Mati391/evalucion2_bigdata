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

![Flujo del dataset](img/flujo%20dataset%20analytic.jpeg)

flujo completo del dataset desde su carga hasta su disponibilidad en BigQuery:

