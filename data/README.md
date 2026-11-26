# Datos

Esta carpeta va vacía en el repositorio. Pon aquí los tres archivos con estos nombres exactos para que el cuaderno funcione.

## 1. `dengue_semanal_2007_2025.csv`: casos de dengue

**Fuente:** Sivigila, Instituto Nacional de Salud (INS). **Los datos se deben solicitar al Sivigila / INS.**

**Construcción:**

- Los microdatos traen un registro por paciente.
- Se cuentan los casos por año y semana epidemiológica de notificación, **sumando los tres eventos de dengue**:
  - **210:** dengue
  - **220:** dengue grave
  - **580:** mortalidad por dengue
- El periodo va de **2007-SE01 a 2025-SE53** (992 semanas).
- La semana epidemiológica va de domingo a sábado. La semana 1 es la que contiene el 4 de enero.

> Hasta 2009 los códigos 210 y 220 se llamaban "dengue clásico" y "dengue hemorrágico". Como se suman los tres eventos, el total es comparable en todo el periodo.

**Formato:**

| Columna | Descripción |
| :--- | :--- |
| `ANO` | Año epidemiológico |
| `SEMANA` | Semana epidemiológica (1 a 53) |
| `casos_210` | Casos del evento 210 (dengue) |
| `casos_220` | Casos del evento 220 (dengue grave) |
| `casos_580` | Casos del evento 580 (mortalidad por dengue) |
| `casos` | Suma de los tres eventos: es la serie que se pronostica |
| `fecha_inicio_semana` | Fecha del domingo en que empieza la semana (AAAA-MM-DD) |

```
ANO,SEMANA,casos_210,casos_220,casos_580,casos,fecha_inicio_semana
2007,1,568,111,1,680,2006-12-31
```

## 2. `oni_cpc_v6_2005_2025.csv`: índice ONI de El Niño

**Fuente:** NOAA Climate Prediction Center, ONI versión 6, años 2005 a 2025 (público): https://www.cpc.ncep.noaa.gov/products/analysis_monitoring/enso/oni/v6/index.php

**Formato:** una fila por año y una columna por trimestre móvil.

```
year,DJF,JFM,FMA,MAM,AMJ,MJJ,JJA,JAS,ASO,SON,OND,NDJ
2005,0.6,0.6,0.4,0.4,0.3,0.1,0.0,-0.1,-0.1,-0.3,-0.6,-0.8
```

## 3. `clima_nasa_power_7_ciudades_2006_2025.csv`: clima local

**Fuente:** NASA POWER, datos diarios del 2006-01-01 al 2025-12-31 (público): https://power.larc.nasa.gov/

**Ciudades:** Cali, Ibagué, Bucaramanga, Villavicencio, Cartagena, Neiva y Barranquilla. Se usan las coordenadas de cada ciudad.

**Variables:**

- `T2M`: temperatura a 2 m, en °C;
- `RH2M`: humedad relativa a 2 m, en %;
- `PRECTOTCORR`: precipitación, en mm/día.

**Formato:** formato largo, una fila por ciudad y día. El nombre de la ciudad va en minúsculas y sin tildes.

```
ciudad,fecha,T2M,RH2M,PRECTOTCORR
neiva,2006-01-01,20.1,77.52,0.44
```

Valores posibles de `ciudad`: `cali`, `ibague`, `bucaramanga`, `villavicencio`, `cartagena`, `neiva`, `barranquilla`.
