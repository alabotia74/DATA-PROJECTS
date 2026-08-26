# Predicción de Popularidad en Spotify con Machine Learning & NLP

Este proyecto implementa un flujo completo de **Data Science** para predecir el score de popularidad de canciones en Spotify a partir de sus atributos acústicos (bailabilidad, energía, valencia, etc.) y metadatos (géneros y artistas).

## Aspectos Clave del Proyecto
* **Dataset:** ~114,000 registros de canciones de Spotify con atributos de audio y metadatos.
* **Procesamiento de Texto:** Extracción y vectorización de metadatos utilizando `TfidfVectorizer` / `CountVectorizer`.
* **Modelamiento:** Evaluación y comparación de regresores individuales y ensambles (`VotingRegressor`, `RandomForestRegressor`, `ExtraTreesRegressor`, `XGBoost`, `LightGBM`).
* **Métricas de Evaluación:** Optimización basada en RMSE, MAE y $R^2$.

## Tecnologías Utilizadas
* **Lenguaje:** Python
* **Librerías:** Pandas, NumPy, Scikit-Learn, XGBoost, LightGBM, Matplotlib, Seaborn
