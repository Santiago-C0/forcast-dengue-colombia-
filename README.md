# Del dato a la alerta: pronóstico del dengue en Colombia con modelos ARIMA

Pronóstico de los casos semanales de dengue en Colombia (2007–2025) con modelos de la familia ARIMA. El proyecto compara un modelo ingenuo, un ARIMA y varios SARIMAX (regresión con errores ARIMA) que incluyen el índice ONI de El Niño y el clima local. Además modela la varianza de los errores con un GARCH(1,1).

**Pregunta de pronóstico:** ¿cuántos casos de dengue se notificarán en Colombia en cada una de las próximas 4 semanas epidemiológicas? ¿Mejora ese pronóstico al incluir el fenómeno de El Niño y el clima local?

## Autores

- Jacobo Simancas Hernandez — Ciencia de Datos
- Santiago Cuesta Maza — Ciencia de Datos e Ingeniería de Sistemas
- Susana Rosales Castellar — Ciencia de Datos e Ingeniería de Sistemas
- Diego Jaraba Padilla — Ciencia de Datos e Ingeniería de Sistemas

Proyecto final de Series de Tiempo · Semillero de Machine Learning.

## Contenido del repositorio

| Archivo o carpeta | Descripción |
| :--- | :--- |
| `DENGUE_FINAL.ipynb` | Cuaderno con todo el análisis, ejecutado y con sus resultados |
| `DENGUE_FINAL.html` | El mismo cuaderno en HTML, para verlo en el navegador sin ejecutar nada |
| `figuras/` | Gráficas principales que genera el cuaderno |
| `data/` | Carpeta vacía donde van los datos (**no se incluyen**; ver abajo) |
| `requirements.txt` | Librerías de Python necesarias |

## Datos (no incluidos)

Los datos **no se suben a este repositorio**. Para ejecutar el cuaderno hay que conseguirlos y ponerlos en la carpeta `data/` con estos nombres:

| Archivo | Contenido | Cómo obtenerlo |
| :--- | :--- | :--- |
| `dengue_semanal_2007_2025.csv` | Casos semanales de dengue en Colombia, 2007-SE01 a 2025-SE53 | **Se deben solicitar al Sivigila** (Sistema Nacional de Vigilancia en Salud Pública, Instituto Nacional de Salud) |
| `oni_cpc_v6_2005_2025.csv` | Índice ONI de El Niño, 2005–2025 | Público: NOAA Climate Prediction Center |
| `clima_nasa_power_7_ciudades_2006_2025.csv` | Temperatura, humedad y precipitación diarias de 7 ciudades, 2006–2025 | Público: NASA POWER |

**Serie de casos de dengue.** La serie es la **suma de los tres eventos de dengue del Sivigila** por semana epidemiológica, de **2007 a 2025**:

- **210:** dengue
- **220:** dengue grave
- **580:** mortalidad por dengue

`data/README.md` explica el formato exacto de cada archivo (columnas y ejemplos).

## Cómo ejecutarlo

**En Google Colab**

1. Abre `DENGUE_FINAL.ipynb` en Colab.
2. En el panel de archivos crea la carpeta `data` y sube ahí los tres CSV.
3. Ejecuta todas las celdas (*Entorno de ejecución → Ejecutar todas*). La primera celda instala `pmdarima` y `arch`.

**En tu computador**

```bash
pip install -r requirements.txt
jupyter notebook DENGUE_FINAL.ipynb
```

La ejecución completa tarda entre 20 y 25 minutos, casi todo en la evaluación de la sección 5.2: 10 años de prueba, 2 horizontes y 7 modelos.

## Método, en resumen

1. **Análisis exploratorio:**
   - series de casos, ONI y clima;
   - descomposición STL;
   - correlación con el ONI rezagado;
   - estacionariedad con ADF y KPSS, y gráficos ACF y PACF.
2. **Modelos:**
   - **M0:** ingenuo, equivalente a un ARIMA(0,1,0).
   - **ARIMA:** elegido con `auto_arima`.
   - **M1 a M5:** SARIMAX con ONI, temperatura, humedad y precipitación. Los rezagos de cada variable se eligen por AICc.
3. **Evaluación fuera de la muestra:**
   - pronóstico con origen móvil a 4 y 12 semanas en 10 años de prueba (2016–2025);
   - RMSE, MAE y prueba de Diebold-Mariano (con la corrección de Harvey, Leybourne y Newbold) frente al ingenuo.
4. **Diagnóstico de residuos:** Ljung-Box, Jarque-Bera, Shapiro-Wilk y ARCH-LM.
5. **Volatilidad:** GARCH(1,1) sobre los residuos del modelo final, para corregir los intervalos de pronóstico.
6. **Pronóstico** de 2026-SE01 a 2026-SE04, con intervalos del 80 % y del 95 %.

## Resultado principal

- **El mejor modelo es M5,** un SARIMAX con ONI, temperatura, humedad y precipitación. Tiene el menor RMSE a 4 semanas en los 10 años de prueba (394.0 frente a 436.5 del ingenuo) y supera al ingenuo de forma significativa (Diebold-Mariano = −2.07, p = 0.039).
- **El ONI es la variable que aporta,** con un rezago de 17 semanas. El clima local casi no suma información una vez que el ONI está en el modelo.

## Fuentes

- Instituto Nacional de Salud. Sivigila, Sistema Nacional de Vigilancia en Salud Pública. https://portalsivigila.ins.gov.co/
- NOAA Climate Prediction Center. Oceanic Niño Index (ONI), versión 6. https://www.cpc.ncep.noaa.gov/products/analysis_monitoring/enso/oni/v6/index.php
- NASA Langley Research Center. POWER Project. https://power.larc.nasa.gov/
