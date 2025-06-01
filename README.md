
# Evaluación 2 - Big Data

**Integrantes:** Matías Sánchez, Jonathan Ávila, Yuliana Benitez

# Proyecto de Análisis de Avistamientos de OVNIs

## Creación del Dataset

**Nombre del archivo:** `ufo_sighting.csv`  
**Tamaño:** 12.9 MB  
**Registros:** 25.699  
**Columnas:** 11  

1. Se accede a Google Cloud Platform y se ingresa al servicio Cloud Storage.
2. Se crea un nuevo bucket con el nombre: `jonathan_avila-yuliana_benitez-matias_sanchez`.
   - Tipo de ubicación: Multiregión (us)
   - Clase de almacenamiento: Standard
   - Control de acceso: No público
3. Una vez creado, se accede al bucket y se sube el archivo `ufo_sighting.csv`.

![Creación de bucket y subida CSV](https://github.com/user-attachments/assets/bb05351a-6d51-4e77-afe8-db6b6e136355)

4. Verificación del archivo `ufo_sighting.csv` en el bucket:  
Después de subir el archivo al bucket, se visualiza en el panel de objetos de Google Cloud Storage para confirmar su correcto almacenamiento.

![Visualización del dataset](https://github.com/user-attachments/assets/0c422a73-9f72-4fc7-a580-adbd64093058)

## Limpieza y Transformación de Datos en Cloud Dataprep

Una vez cargado el dataset, se utiliza Cloud Dataprep para preparar los datos antes del análisis:

1. Eliminación de valores faltantes en las columnas clave:  
`Date_time`, `state/province`, `country`, `UFO_shape`, `description`

2. Renombramiento de columna:  
`state/province` → `state_province`

3. Validación del esquema final:  
- 100% de los valores válidos  
- 0% datos nulos  
- Total de filas procesadas: 21.568

![Limpieza de datos](https://github.com/user-attachments/assets/78eb8e9c-3004-4721-b27c-b39f9bc6eda3)

## Flujo de procesamiento de datos

Flujo completo del dataset desde su carga hasta su disponibilidad en BigQuery:

![Flujo de dataset](https://github.com/user-attachments/assets/d45aeacc-b303-4d08-8670-019309d69e65)

## Tabla de Avistamientos

Después de ejecutar la receta de limpieza en Cloud Dataprep, la imagen muestra la tabla final ya disponible para consultas SQL en BigQuery.

![Tabla de avistamientos y confirmación](https://github.com/user-attachments/assets/f9387969-c49e-4a47-aefd-4e2cce81c7d2)

## Configuración de Permisos

Se asignan las cuentas y servicios dentro del proyecto de Google Cloud. Esta configuración garantiza que las herramientas tengan los accesos necesarios para operar correctamente.

![Permisos IAM](https://github.com/user-attachments/assets/76452845-4b72-437e-9694-faaf2042e70d)

## Análisis de Datos

### 1. Gráfico de Torta – Formas más comunes de avistamientos

**Datos mostrados:**  

Formas de UFO más comunes en EE.UU. y su frecuencia:
- Light: 5,440 avistamientos (54.4%)
- Triangle: 2,574 (25.74%)
- Circle: 2,483 (24.83%)
- Fireball: 2,117 (21.17%)
- Other: 1,880 (18.8%)


![Gráfico de torta](https://github.com/user-attachments/assets/a31e9378-bfd3-46e2-b44b-70ed533649af)


**Interpretación:**  
Los objetos luminosos ("light") son con diferencia los más reportados, representando más de la mitad de todos los avistamientos.  
Las formas geométricas definidas (triángulo, círculo) son las siguientes más comunes.  
El gráfico muestra solo los 5 tipos principales, excluyendo categorías menos frecuentes.

**Consulta SQL utilizada:**
```sql
SELECT 
  UFO_shape,
  COUNT(*) AS Sightings
FROM 
  `ufo_dataset.ufo_sightings`
WHERE 
  country = 'us'
GROUP BY 
  UFO_shape
ORDER BY 
  Sightings DESC
LIMIT 5;
```

### 2. Gráfico de Barras – Avisos por Año

![Gráfico de barras](https://github.com/user-attachments/assets/4cb2443f-8009-4d9c-a9c7-f86cfbd826cf)

**Datos mostrados:**
- 2012: 2,626
- 2013: 2,424
- 2011: 1,763
- 2010: 1,597
- 2008: 1,498
- 2007: 1,446
- 2009: 1,360
- 2005: 1,272
- 2004: 1,239
- 2006: 1,110
- 2003: 1,083

**Interpretación:**  
Los años 2012 y 2013 fueron picos en reportes de avistamientos.  
Hay una tendencia general de aumento hacia 2012-2013, con algunos altibajos.  
Los datos muestran solo parte del conjunto total (11 de 63 registros anuales mostrados).

**Consulta SQL utilizada:**
```sql
SELECT 
  EXTRACT(YEAR FROM DATETIME(Date_time)) AS Year,
  COUNT(*) AS Sightings
FROM 
  `ufo_dataset.ufo_sightings`
WHERE 
  country = 'us'
GROUP BY 
  Year
ORDER BY 
  Sightings DESC;
```

### 3. Mapa de Avistamientos por Estado

El tercer gráfico representa un mapa con la distribución de avistamientos por estado en EE.UU., ordenado de mayor a menor.

![Mapa de avistamientos](https://github.com/user-attachments/assets/86ae55de-b404-499e-a1ae-d25ac1e25429)

**Interpretación:**  
Interpretación del gráfico de tabla y mapa:
1. Tabla de avistamientos por estado:
Muestra los 11 estados con mayor número de avistamientos.
Los tres estados con más avistamientos son:

- California: 3.561
- Washington: 1.555
- Florida: 1.544
