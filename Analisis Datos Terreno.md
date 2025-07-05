 <img src="https://www.digea.usach.cl/digea/site/artic/20230110/imag/foto_0000000620230110165150/LOGO_DIGEA_MAIN_01.png" height='100px'>

# Territorio, medioambiente y Sustentabilidad - 1er Semestre 2025
## Trabajo Final - Analisis datos terreno

Este Python Notebook es referecial.
<p> Este análisis es sólo una guia para que uds. puedan hacer un buen análisis🤓.
<p>No quedarse solo con este análisis, pueden hacer sus propios gráficos y mapas📊🗺️.

#### Conexión a ArcGIS Online


```python
import arcgis.features as af
from arcgis.gis import GIS
import pandas as pd

gis = GIS("home")
```

#### Pueden Revisar el Dashboard realizado


```python
from IPython.display import display, HTML

display(HTML("""
<a href="https://geo-usach.maps.arcgis.com/apps/dashboards/2fa1627265b649df9b27c63feda31775" target="_blank">Link a Dashboard Datos Terreno</a>
"""))
```



<a href="https://geo-usach.maps.arcgis.com/apps/dashboards/2fa1627265b649df9b27c63feda31775" target="_blank">Link a Dashboard Datos Terreno</a>


### Esta sección convierte un [servicio de mapas web](https://geo-usach.maps.arcgis.com/home/item.html?id=b7ca7d8e28de4020bf9ec8c32d7afa28) en un DataFrame para hacer Ingeniería de Datos, esto lo van a ver en sus ramos de programación 


```python
def featurelayer_to_sedf(flayer: af.layer.FeatureLayer) -> pd.core.frame.DataFrame:
    try:
        sedf = pd.DataFrame.spatial.from_layer(flayer)
        return sedf
    except Exception as e:
        print(f"Could not convert feature layer to Spatially Enabled DataFrame (SeDF) because of the following error: {e}.")

sdf = featurelayer_to_sedf(item.layers[0])
```

Aqui se analizan todos los campos (columnas) y cuantos registros (filas) no nulas tiene


```python
sdf.info()
```

    <class 'pandas.core.frame.DataFrame'>
    RangeIndex: 23 entries, 0 to 22
    Data columns (total 28 columns):
     #   Column      Non-Null Count  Dtype         
    ---  ------      --------------  -----         
     0   OBJECTID    23 non-null     Int64         
     1   ObjectID_1  0 non-null      Float64       
     2   Fecha_Mues  23 non-null     datetime64[ns]
     3   Latitud     23 non-null     Float64       
     4   Longitud    23 non-null     Float64       
     5   Este_UTM    23 non-null     Float64       
     6   Norte_UTM   23 non-null     Float64       
     7   Altura      23 non-null     Float64       
     8   Región      23 non-null     string        
     9   Provincia   23 non-null     string        
     10  Comuna      23 non-null     string        
     11  Instrument  23 non-null     string        
     12  Sunlight    16 non-null     string        
     13  Temperatur  7 non-null      Float64       
     14  Moisture    16 non-null     string        
     15  pH          7 non-null      Float64       
     16  HCOH        10 non-null     Float64       
     17  TVOC        10 non-null     Float64       
     18  PM25        10 non-null     Float64       
     19  PM10        10 non-null     Float64       
     20  CO          10 non-null     Float64       
     21  CO2         10 non-null     Float64       
     22  AQI         10 non-null     Float64       
     23  Humi        6 non-null      Float64       
     24  Temp_2      6 non-null      Float64       
     25  Temperat_1  14 non-null     Float64       
     26  Velocidad   14 non-null     Float64       
     27  SHAPE       23 non-null     geometry      
    dtypes: Float64(19), Int64(1), datetime64[ns](1), geometry(1), string(6)
    memory usage: 5.6 KB
    

## Hipótesis 1 
<p>El Gradiente de la Calidad del Aire en Relación con la Altitud y la Intervención Antrópica.
Se anticipa que las zonas de mayor altitud presentarán menores concentraciones de PM2.5, PM10, CO, CO2, HCHO y TVOC, lo que resultará en un mejor AQI, en contraste con las áreas de menor altitud con mayor huella humana, debido a las diferencias en los patrones de dispersión atmosférica y las fuentes de emisión.


```python
import matplotlib.pyplot as plt
import seaborn as sns

# Renombrar columnas para facilitar su uso
df = sdf.rename(columns={
    'Altura': 'Altitud',
    'PM10': 'PM10',
    'PM25': 'PM2.5',
    'CO': 'CO',
    'CO2': 'CO2',
    'HCOH': 'HCHO',
    'TVOC': 'TVOC',
    'AQI': 'AQI',
    'Humi': 'Humedad_Aire',
    'Moisture': 'Humedad_Suelo',
    'pH': 'pH',
    'Temperatur': 'Temp_Suelo',
    'Temperat_1': 'Temp_Aire',
    'Velocidad': 'Viento',
    'Sunlight': 'Luz_Solar'
})

# Convertir a tipo numérico las columnas claves
cols_to_numeric = [
    'Altitud', 'PM2.5', 'PM10', 'CO', 'CO2', 'HCHO', 'TVOC', 'AQI', 
    'Humedad_Aire','pH', 'Temp_Suelo', 'Temp_Aire', 'Viento'
]
df[cols_to_numeric] = df[cols_to_numeric].apply(pd.to_numeric, errors='coerce')

# Eliminar filas completamente vacías en variables claves
df_filtered = df.dropna(subset=['Altitud', 'PM2.5', 'PM10', 'CO', 'CO2', 'HCHO', 'TVOC', 'AQI'])

# Crear gráficos para Hipótesis 1: Gradiente de calidad del aire con altitud
sns.set(style="whitegrid")
plt.figure(figsize=(12, 8))
for i, contaminante in enumerate(['PM2.5', 'PM10', 'CO', 'CO2', 'HCHO', 'TVOC'], start=1):
    plt.subplot(2, 3, i)
    sns.scatterplot(data=df_filtered, x='Altitud', y=contaminante)
    sns.regplot(data=df_filtered, x='Altitud', y=contaminante, scatter=False, color='red')
    plt.title(f'{contaminante} vs Altitud')
    plt.xlabel('Altitud (m)')
    plt.ylabel(contaminante)

plt.tight_layout()
plt.suptitle('Hipótesis 1: Relación entre Altitud y Calidad del Aire', fontsize=16, y=1.03)
plt.show()
```


    
![png](Analisis%20Datos%20Terreno_files/Analisis%20Datos%20Terreno_13_0.png)
    


### Hipótesis 2: 
<p>La Influencia de la Proximidad a Grandes Centros de Actividad Humana en la Contaminación Atmosférica.
Aquellas localidades más próximas a concentraciones urbanas o puntos de alta actividad logística exhibirán mayores niveles de PM2.5, PM10, CO, CO2, HCHO y TVOC, traduciéndose en un AQI más elevado (indicando peor calidad del aire), en comparación con entornos menos densamente poblados o intervenidos.


```python
import numpy as np

# Crear un índice aproximado de urbanización basado en la distancia a un punto urbano central
# Usaremos una coordenada representativa del centro urbano de Santiago
centro_urbano = {'lat': -33.4489, 'lon': -70.6693}

# Calcular distancia euclidiana aproximada (en grados, sin transformar a kilómetros)
df['Dist_Urbano'] = np.sqrt((df['Latitud'] - centro_urbano['lat'])**2 + 
                            (df['Longitud'] - centro_urbano['lon'])**2)

# Filtrar datos con las variables necesarias
df_proximidad = df.dropna(subset=['Dist_Urbano', 'PM2.5', 'PM10', 'CO', 'CO2', 'HCHO', 'TVOC', 'AQI'])

# Crear gráficos para Hipótesis 2: Contaminación vs proximidad a centros urbanos
plt.figure(figsize=(12, 8))
for i, contaminante in enumerate(['PM2.5', 'PM10', 'CO', 'CO2', 'HCHO', 'TVOC'], start=1):
    plt.subplot(2, 3, i)
    sns.scatterplot(data=df_proximidad, x='Dist_Urbano', y=contaminante)
    sns.regplot(data=df_proximidad, x='Dist_Urbano', y=contaminante, scatter=False, color='red')
    plt.title(f'{contaminante} vs Distancia al Centro Urbano')
    plt.xlabel('Distancia Aproximada al Centro Urbano')
    plt.ylabel(contaminante)

plt.tight_layout()
plt.suptitle('Hipótesis 2: Proximidad a Centros Urbanos y Calidad del Aire', fontsize=16, y=1.03)
plt.show()
```


    
![png](Analisis%20Datos%20Terreno_files/Analisis%20Datos%20Terreno_15_0.png)
    


### Hipótesis 3: 
<p>La Humedad del Aire y del Suelo en Función de la Altitud y la Proximidad a Cuerpos de Agua Extensos.
La humedad del aire (HUMI) y la humedad del suelo (Moisture) serán significativamente inferiores en los sectores de mayor elevación, mientras que las áreas cercanas a grandes masas de agua mostrarán consistentemente una mayor humedad ambiental, independientemente de su altitud.


```python
moisture_map = {
    'DRY+': 0,
    'DRY': 1,
    'NOR': 2,
    'WET': 3,
    'WET+': 4
}
df['HS_Ordinal'] = df['Humedad_Suelo'].map(moisture_map)

```


```python
import seaborn as sns
import matplotlib.pyplot as plt

# Filtrar datos válidos
df_valid = df.dropna(subset=['Altitud', 'HS_Ordinal'])

plt.figure(figsize=(8, 6))
sns.stripplot(data=df_valid, x='HS_Ordinal', y='Altitud', jitter=True)
sns.boxplot(data=df_valid, x='HS_Ordinal', y='Altitud', showcaps=True, boxprops={'facecolor':'None'})
plt.xticks(ticks=[0,1,2,3,4], labels=['DRY+', 'DRY', 'NOR', 'WET', 'WET+'])
plt.xlabel('Categoría de Humedad del Suelo')
plt.ylabel('Altitud (m)')
plt.title('Hipótesis 3: Relación entre Altitud y Humedad del Suelo')
plt.show()

```


    
![png](Analisis%20Datos%20Terreno_files/Analisis%20Datos%20Terreno_18_0.png)
    



```python
# Asegurar que las columnas son numéricas, convirtiendo errores en NaN (Not a Number)
df['Altitud'] = pd.to_numeric(df['Altitud'], errors='coerce')
df['Humedad_Aire'] = pd.to_numeric(df['Humedad_Aire'], errors='coerce')

# Eliminar filas donde falten los datos de interés
df_valid = df.dropna(subset=['Altitud', 'Humedad_Aire'])

# Creación del gráfico
plt.figure(figsize=(10, 6))
sns.regplot(data=df_valid, x='Altitud', y='Humedad_Aire',
            scatter_kws={'alpha': 0.5}, line_kws={'color': 'red'})
plt.title('Relación entre Altitud y Humedad del Aire')
plt.xlabel('Altitud (m)')
plt.ylabel('Humedad Relativa del Aire (%)')
plt.grid(True, linestyle='--', alpha=0.6)
plt.show()
```


    
![png](Analisis%20Datos%20Terreno_files/Analisis%20Datos%20Terreno_19_0.png)
    


### Hipótesis 4: 

<p>Variaciones en las Propiedades del Suelo (Temperatura y pH) Asociadas a la Altitud y la Cobertura del Terreno.
La temperatura del suelo (Temp) y el pH del suelo manifestarán diferencias notables entre los sitios de mayor altitud y las localidades de menor elevación con presencia de humedales o densa cobertura vegetal. Se espera que estas últimas exhiban una mayor estabilidad térmica y un pH más ligado a la dinámica biológica.


```python
# Filtrar datos válidos
df_clean = df.dropna(subset=['Altitud', 'Temp_Suelo', 'pH'])

# Gráficos
plt.figure(figsize=(12, 5))

# Temp del suelo vs Altitud
plt.subplot(1, 2, 1)
sns.scatterplot(data=df_clean, x='Altitud', y='Temp_Suelo')
sns.regplot(data=df_clean, x='Altitud', y='Temp_Suelo', scatter=False, color='red')
plt.title('Temperatura del Suelo vs Altitud')
plt.xlabel('Altitud (m)')
plt.ylabel('Temperatura del Suelo (°C)')

# pH vs Altitud
plt.subplot(1, 2, 2)
sns.scatterplot(data=df_clean, x='Altitud', y='pH')
sns.regplot(data=df_clean, x='Altitud', y='pH', scatter=False, color='red')
plt.title('pH del Suelo vs Altitud')
plt.xlabel('Altitud (m)')
plt.ylabel('pH del Suelo')

plt.tight_layout()
plt.suptitle('Hipótesis 4: Propiedades del Suelo en función de la Altitud', fontsize=16, y=1.05)
plt.show()
```


    
![png](Analisis%20Datos%20Terreno_files/Analisis%20Datos%20Terreno_21_0.png)
    


### Hipótesis 5: 

<p>La Relación entre la Elevación del Terreno, la Temperatura del Aire y los Patrones de Viento Dominantes.
Los puntos geográficos de mayor altitud registrarán una mayor velocidad del viento (Velocidad) y una menor temperatura del aire (TEMP) en comparación con las zonas más bajas y protegidas, debido a las características orográficas y la menor presencia de barreras físicas.


```python
# Filtrar datos válidos
df_viento = df.dropna(subset=['Altitud', 'Viento', 'Temp_Aire'])

# Gráficos
plt.figure(figsize=(12, 5))

# Altitud vs Velocidad del Viento
plt.subplot(1, 2, 1)
sns.scatterplot(data=df_viento, x='Altitud', y='Viento')
sns.regplot(data=df_viento, x='Altitud', y='Viento', scatter=False, color='red')
plt.title('Velocidad del Viento vs Altitud')
plt.xlabel('Altitud (m)')
plt.ylabel('Velocidad del Viento (m/s)')

# Altitud vs Temperatura del Aire
plt.subplot(1, 2, 2)
sns.scatterplot(data=df_viento, x='Altitud', y='Temp_Aire')
sns.regplot(data=df_viento, x='Altitud', y='Temp_Aire', scatter=False, color='red')
plt.title('Temperatura del Aire vs Altitud')
plt.xlabel('Altitud (m)')
plt.ylabel('Temperatura del Aire (°C)')

plt.tight_layout()
plt.suptitle('Hipótesis 5: Viento y Temperatura del Aire en función de la Altitud', fontsize=16, y=1.05)
plt.show()
```


    
![png](Analisis%20Datos%20Terreno_files/Analisis%20Datos%20Terreno_23_0.png)
    

