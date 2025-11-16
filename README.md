Pet ECG Quality Classification — Machine Learning for Veterinary Signal Analysis

Este proyecto desarrolla un modelo de Machine Learning capaz de clasificar automáticamente la calidad de un ECG veterinario (bueno / malo) utilizando características derivadas de la señal y metadatos del paciente.

El objetivo es asistir a profesionales veterinarios y a herramientas digitales en la detección temprana de registros ECG defectuosos, evitando diagnósticos basados en señales ruidosas o incompletas.

Contenido del repositorio
pet-ecg-quality-ml/
│
├── data/
│   ├── raw/               → dataset original (CSV)
│   ├── processed/         → features generadas
│
├── notebooks/
│   └── 01_model_training.ipynb
│
├── src/
│   ├── preprocessing.py
│   ├── feature_engineering.py
│   └── train_model.py
│
└── README.md

1. Objetivo del proyecto

Los registros ECG veterinarios pueden contener:

artefactos,

cortes,

ruido eléctrico,

pérdidas de contacto,

falsos picos.

Este proyecto aplica ML para detectar ECG de baja calidad antes del análisis clínico, permitiendo:

✔ evitar interpretaciones erróneas
✔ repetir la medición a tiempo
✔ mejorar flujos de trabajo en clínicas y dispositivos portátiles

2. Datos

El dataset incluye:

Metadatos del paciente:

peso

edad

raza

Señales derivadas:

.segments_hr → segmentos de ritmo cardiaco

.segments_br → segmentos respiratorios

.ecg_pulses → pulsos detectados

duración total de registro

Etiqueta:

bad_ecg → 0 = ECG bueno, 1 = ECG malo

No se utiliza el audio .wav en esta versión (fase futura del proyecto).

3. Ingeniería de características

A partir de los segmentos y pulsos se desarrollaron features tabulares:

Features cardiacas

hr_n_segments

hr_total_duration

hr_mean_value

Features respiratorias

br_n_segments

br_total_duration

br_mean_value

Actividad eléctrica

pulses_count

pulses_density

Metadata

duration (del ECG)

weight

age

breeds (one-hot encoded)

4. Modelado

Se construyó un pipeline completo de ML:

ColumnTransformer → Scaling/Imputation → OneHotEncoder → RandomForestClassifier


Se aplicó:

✔ train_test_split estratificado
✔ Búsqueda de hiperparámetros (RandomizedSearchCV)
✔ Métricas: Accuracy, F1, ROC-AUC

5. Resultados
Rendimiento final del mejor modelo:
Métrica	Resultado
Accuracy	0.81
ROC-AUC	0.887
F1 (ECG malo)	0.76
Confusion Matrix:

Verdaderos positivos = 66

Falsos positivos = 18

Falsos negativos = 24
(datos aproximados según la ejecución final)

Curva ROC

AUC ≈ 0.89, indicando excelente separabilidad entre ECG buenos y malos.

6. Interpretabilidad del modelo

Para entender qué señales utiliza el modelo, se analizaron:

✔ Feature importance del RandomForest
✔ Permutation importance (pérdida de AUC al permutar)

Variables más importantes

hr_total_duration

hr_n_segments

pulses_count

pulses_density

hr_mean_value

Las características cardiacas son las que más influyen.
Variables respiratorias tienen impacto moderado.
Las razas tienen una importancia baja (lo cual es positivo).

Interpretación clínica

ECG con muchos fragmentos → señal inestable

mayor duración de segmentos cardiacos → más irregularidades

más pulsos → mayor actividad/noise → peor calidad

densidad excesiva de pulsos → artefactos

En conjunto, el modelo aprende patrones coherentes con la fisiología y el ruido típico del ECG veterinario.

7. Líneas futuras

Este proyecto puede ampliarse significativamente:

1. Procesamiento del audio .wav

MFCCs

Espectrogramas MEL

CNN

Fusión audio + features tabulares

2. LightGBM / XGBoost

Modelos que suelen mejorar el rendimiento tabular.

3. Sistema en producción

endpoint FastAPI

dashboard Streamlit

pipeline CI/CD

8. Cómo ejecutar el proyecto
1. Clonar el repositorio
git clone https://github.com/insightiqx/pet-ecg-quality-ml.git
cd pet-ecg-quality-ml

2. Crear entorno
python -m venv .venv
.\.venv\Scripts\activate
pip install -r requirements.txt

3. Ejecutar JupyterLab
jupyter lab

4. Abrir el notebook:

notebooks/01_model_training.ipynb

Autor

Contact: insightiqx@gmail.com
Portfolio: github.com/insightiqx

