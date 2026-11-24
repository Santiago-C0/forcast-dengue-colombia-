# Del dato a la alerta: canal endémico y modelos ARIMA para el dengue en Colombia

Proyecto final de Series de Tiempo · Semillero de Machine Learning (feria científica).

El Instituto Nacional de Salud (INS) vigila el dengue con el **canal endémico**: una banda calculada con los casos de años anteriores que indica si los casos de cada semana son normales, están en alerta o en brote. El canal dice *cómo estamos*, pero no *cuántos casos vienen*.

Este proyecto replica el canal endémico del INS y evalúa si un modelo de la familia **ARIMA** (ARIMA, SARIMA, SARIMAX) puede complementarlo con un pronóstico a 4 semanas.

> **Estado:** los resultados de la comparación de modelos están en revisión con el docente.

## Integrantes

- [COMPLETAR: nombres del grupo]
- Docente: [COMPLETAR]

## Contenido del repositorio

| Archivo | Qué es |
| :--- | :--- |
| `Canal_endemico_y_ARIMA_dengue_proyecto_final.ipynb` | Cuaderno con todo el análisis, ya ejecutado (se ven tablas y gráficas sin correrlo) |
| `dengue_semanal_2019_2025.csv` | Casos semanales de dengue en Colombia, 2019–2025 (366 semanas) |
| `canal_ins_figura16_digitalizado.csv` | Valores del canal endémico del INS, leídos de la Figura 16 del boletín |
| `requirements.txt` | Paquetes de Python que usa el cuaderno |

## Datos

- **Fuente:** microdatos del Sivigila, Instituto Nacional de Salud (INS). https://portalsivigila.ins.gov.co/
- **Eventos:** 210 (dengue), 220 (dengue grave) y 580 (mortalidad por dengue), los mismos que suma el boletín del INS.
- **Serie:** casos notificados por semana epidemiológica, nivel nacional, de 2019-SE01 a 2025-SE53.
- **Fecha de descarga:** [COMPLETAR]
- **Canal de referencia:** Boletín Epidemiológico Semanal del INS, semana 38 de 2026 (Figura 16, página 31).

Los archivos originales del Sivigila (un registro por paciente) **no se incluyen** en el repositorio: son muy pesados (el de 2024 pasa de 200 MB) y no hacen falta para correr el cuaderno. `dengue_semanal_2019_2025.csv` solo tiene el conteo de casos por semana y no contiene datos de pacientes.

## Cómo ejecutar el cuaderno

**En Google Colab (recomendado)**

1. Abrir el archivo `.ipynb` en [Google Colab](https://colab.research.google.com/) (Archivo → Subir cuaderno).
2. Subir los dos archivos `.csv` a la carpeta `/content` (ícono de carpeta, a la izquierda).
3. Entorno de ejecución → Ejecutar todas. Tarda entre 6 y 8 minutos.

**En el computador**

```bash
pip install -r requirements.txt
jupyter notebook
```

El cuaderno y los dos `.csv` deben estar en la misma carpeta.

## Método

1. **Canal endémico (referencia institucional):** método de las medias geométricas de Bortman, validado contra la Figura 16 del boletín.
2. **Modelos de referencia:** ingenuo, ingenuo estacional e ingenuo con deriva.
3. **Modelos candidatos:** ARIMA, SARIMA (s = 52) y SARIMAX con términos de Fourier.
4. **Evaluación:** RMSE, MAE, MAPE y MASE en el periodo de prueba (2025), a 4 semanas (horizonte principal), 1 semana y año completo. Se repite con los años de prueba 2022 a 2025.
5. **Producto final:** pronóstico de 2026-SE01 a 2026-SE04, con intervalos del 80 % y del 95 %, ubicado sobre el canal endémico de 2026.

## Resultados principales

- La réplica del canal endémico coincide con la del boletín del INS con diferencias menores al 1 %.
- A 1 y 4 semanas, los modelos que usan los casos más recientes anticipan mejor que suponer "lo normal" (el valor esperado del canal).
- El canal y el modelo de pronóstico responden preguntas distintas y se complementan: el canal clasifica la situación actual y el modelo anticipa las próximas semanas.

El detalle de la comparación entre modelos está en la sección 5 del cuaderno.

## Limitaciones

- Un solo periodo (2019–2025) con una sola gran epidemia completa (2024).
- Los residuos de los modelos no son ruido blanco.
- No se usan variables climáticas.
- Los datos del portal están depurados; el boletín trabaja con cifras preliminares.

## Referencias

- Instituto Nacional de Salud. *Boletín Epidemiológico Semanal, semana 38 de 2026.*
- Bortman, M. (1999). Elaboración de corredores o canales endémicos mediante planillas de cálculo. *Revista Panamericana de Salud Pública*, 5(1).
- Las demás fuentes están citadas en la sección 3.2 del cuaderno.
