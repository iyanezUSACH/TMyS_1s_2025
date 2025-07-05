<img src="https://www.digea.usach.cl/digea/site/artic/20230110/imag/foto_0000000620230110165150/LOGO_DIGEA_MAIN_01.png" height='100px'>

# Territorio, Medioambiente y Sustentabilidad - 1er Semestre 2025
## Trabajo Final - Caracterización de la Cuenca del Río Maipo

¡Bienvenidos al repositorio del Trabajo Final! Este espacio centraliza todo el material que necesitaremos para nuestro análisis de datos geoespaciales.

---
### ¿Qué es GitHub y por qué lo usamos? 🤔

Piensa en **GitHub** como una especie de "Google Drive para programadores". Es una plataforma en línea donde podemos guardar, compartir y colaborar en proyectos de código.

Lo usamos en este curso por tres razones clave:

1.  **Colaboración y Transparencia 🤝**: Nos permite trabajar en equipo de manera ordenada. Todos podemos ver las últimas versiones de los archivos, evitando la confusión de tener múltiples copias de un mismo documento.
2.  **Control de Versiones 🕒**: GitHub guarda un historial completo de todos los cambios. Si algo sale mal, podemos "viajar en el tiempo" y volver a una versión anterior que funcionaba.
3.  **Portafolio Profesional 🚀**: Aprender a usar GitHub es una habilidad muy valorada en el mundo profesional y académico.

---

### ¿Por qué es importante la Ciencia de Datos en nuestra área? 🌎

Como futuros ingenieros en Territorio y Medioambiente, su trabajo será entender y gestionar sistemas complejos. La **ciencia de datos** es una de las herramientas más poderosas para lograrlo.

* **Entender Sistemas Complejos**: El medioambiente es un sistema lleno de variables que interactúan (clima, geografía, actividad humana). La ciencia de datos nos permite tomar mediciones del mundo real (como las que hicimos en terreno) y encontrar patrones y relaciones que a simple vista son invisibles.

* **Tomar Decisiones Informadas**: En lugar de basar decisiones de planificación territorial o mitigación ambiental en la intuición, podemos usar datos para **validar hipótesis** y elegir la solución más efectiva. Este proyecto es un ejemplo práctico de ello.

* **Predecir y Prevenir Riesgos**: Analizando datos históricos y actuales, podemos construir modelos que nos ayuden a predecir el riesgo de desastres naturales como inundaciones o remociones en masa, permitiendo un diseño de infraestructura más resiliente.

* **Optimizar Recursos**: Desde la gestión del agua hasta la planificación de corredores biológicos, analizar datos nos ayuda a usar los recursos de manera más eficiente y a diseñar soluciones verdaderamente **sostenibles**.

En resumen, aprender a manejar datos les dará una ventaja fundamental para resolver los desafíos ambientales y territoriales del futuro.

---

### Sobre este Repositorio 📂

Este repositorio contiene los archivos para el análisis de los datos que levantamos en terreno, con el objetivo de **trazar y caracterizar la cuenca del Río Maipo**. El estudio se centró en dos puntos clave:

* **Cajón del Maipo**: Representando la cuenca alta y los procesos de origen.
* **Santo Domingo**: Representando la desembocadura y la zona baja de la cuenca.

En la carpeta **[Archivos📂](Archivos)** encontrarás:
* El **notebook de análisis** con el código y los gráficos.
* Los **datos consolidados** en formato `.xlsx`.
* Un archivo `requirements.txt` para replicar el entorno de trabajo.

En la carpeta **[Analisis Datos Terreno_files📂](Analisis%20Datos%20Terreno_files)** están los gráficos obtenidos por este análisis

Además completamos el análisis, accediendo al ArcGIS Dashboard 📊🗺️ copiando el siguiente enlace en tu navegador: 

``` python
https://geo-usach.maps.arcgis.com/apps/dashboards/2fa1627265b649df9b27c63feda31775
```
---
### Análisis de Datos 📝

El corazón de este proyecto es el análisis de datos, donde exploramos las hipótesis planteadas. El notebook completo, con todo el código, explicaciones y gráficos, se encuentra en el siguiente enlace:

➡️ **[Ver el Notebook de Análisis de Datos de Terreno](Analisis%20Datos%20Terreno.md)**

---
### Hipótesis del Estudio 🔬

A continuación, se presentan las cinco hipótesis que guiaron el levantamiento de datos y el posterior análisis.

### **Hipótesis 1: Gradiente de Calidad del Aire vs. Altitud y Actividad Humana**
> Se anticipa que las zonas de mayor altitud presentarán menores concentraciones de **PM2.5**, **PM10**, **CO**, **CO2**, **HCHO** y **TVOC**, lo que resultará en un mejor **AQI**, en contraste con las áreas de menor altitud con mayor huella humana.

* **Instrumento Clave**: Detector de Calidad de Aire

### **Hipótesis 2: Influencia de Centros Urbanos en la Contaminación**
> Aquellas localidades más próximas a concentraciones urbanas o puntos de alta actividad logística exhibirán mayores niveles de contaminantes (**PM2.5**, **PM10**, **CO**, **CO2**, etc.), traduciéndose en un **AQI** más elevado.

* **Instrumento Clave**: Detector de Calidad de Aire

### **Hipótesis 3: Humedad del Aire y Suelo vs. Altitud y Agua**
> La humedad del aire (**HUMI**) y la humedad del suelo (**Moisture**) serán significativamente inferiores en los sectores de mayor elevación, mientras que las áreas cercanas a grandes masas de agua mostrarán una mayor humedad ambiental.

* **Instrumentos Clave**: Detector de Calidad de Aire, Medidor de Suelo

### **Hipótesis 4: Propiedades del Suelo (Temperatura y pH) vs. Altitud y Cobertura**
> La temperatura del suelo (**Temp**) y el **pH** manifestarán diferencias notables entre sitios de mayor altitud y localidades de menor elevación con humedales o densa cobertura vegetal.

* **Instrumento Clave**: Medidor de Suelo

### **Hipótesis 5: Viento y Temperatura del Aire vs. Altitud**
> Los puntos geográficos de mayor altitud registrarán una mayor velocidad del **viento** y una menor **temperatura del aire** en comparación con las zonas más bajas y protegidas.

* **Instrumentos Clave**: Anemómetro, Detector de Calidad de Aire
