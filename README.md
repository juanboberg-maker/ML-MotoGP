# 🏍️ MotoGP Winner Prediction — Machine Learning Project

**Autor:** Juan Boberg  
**Bootcamp:** Ironhack Data Analytics  
**Fecha:** Mayo 2026

---

## Descripción del proyecto

Este proyecto aplica técnicas de Machine Learning supervisado para predecir si un piloto será el **máximo ganador de una temporada** en la categoría MotoGP™.

El dataset original contiene únicamente registros de ganadores de carrera desde 1949 hasta 2022, lo que impide predecir el resultado de una carrera completa (no hay información de los no-ganadores por carrera). Por ello, el problema se reformuló como una **clasificación binaria**: dado el historial acumulado de un piloto y sus victorias en una temporada concreta, ¿es el piloto que más carreras ganó ese año?

---

## Objetivo

> Clasificar si un piloto es el **top winner** de una temporada (`is_top_winner = 1`) o no (`is_top_winner = 0`).

---

## Datasets utilizados

| Archivo | Contenido |
|---------|-----------|
| `grand-prix-race-winners.csv` | Dataset principal: ganadores por carrera (3.083 registros, 1949–2022) |
| `riders-finishing-positions.csv` | Historial acumulado de posiciones por piloto |
| `circuit_data.csv` | Datos técnicos de circuitos |
| `constructure-world-championship.csv` | Campeonato de constructores |
| `grand-prix-events-held.csv` | Eventos celebrados |
| `riders-info.csv` | Información adicional de pilotos |
| `same-nation-podium-lockouts.csv` | Podios dominados por un mismo país |

Fuente: [Kaggle — MotoGP Dataset](https://www.kaggle.com/)

---

## Estructura del proyecto

```
motogp-ml/
│
├── MotoGP_ML_Juan_Boberg.ipynb   # Notebook principal con todo el análisis
├── archive (1)/                   # Datasets principales
├── archive (2)/                   # Dataset de circuitos
└── README.md
```

---

## Metodología

El proyecto sigue una estructura de 4 días:

**Día 1 — Selección y carga de datos**  
Definición del problema, exploración de los 7 datasets disponibles e identificación de las variables relevantes.

**Día 2 — Preparación y feature engineering**  
Limpieza de valores nulos, filtrado de la categoría MotoGP™ (871 registros), y construcción del dataset de modelado mediante agrupaciones, merges y creación de la variable objetivo `is_top_winner`. Dataset final: 304 registros.

**Día 3 — Entrenamiento de modelos base**  
Entrenamiento de KNN, Random Forest, AdaBoost y Gradient Boosting con parámetros por defecto para establecer una línea base de rendimiento.

**Día 4 — Optimización y validación**  
GridSearchCV con validación cruzada de 5 folds (scoring: recall), curva ROC-AUC, matriz de confusión y Stratified K-Fold Cross-Validation para confirmar la generalización del modelo.

---

## Resultados

| Modelo | Accuracy | Recall clase 1 | F1 clase 1 |
|--------|:--------:|:--------------:|:----------:|
| KNN (optimizado) | 0.84 | 0.80 | 0.71 |
| Random Forest (optimizado) | 0.85 | 0.73 | 0.71 |
| Gradient Boosting | 0.95 | 1.00 | 0.91 |
| **AdaBoost** ✅ | **0.97** | **1.00** | **0.94** |

**Modelo seleccionado: AdaBoost**  
Accuracy del 97%, recall perfecto en la clase minoritaria (0 falsos negativos) y AUC ~0.99.

---

## Conclusiones

Los modelos basados en boosting (AdaBoost y Gradient Boosting) superan claramente a KNN y Random Forest en este problema, especialmente en la detección de la clase minoritaria (top winners). AdaBoost alcanza un recall del 100% sobre los ganadores reales del conjunto de test, lo que significa que el modelo no pierde ningún campeón de temporada. La validación cruzada confirma que este resultado es consistente y no depende de un split concreto.

El principal reto del proyecto es el **desbalance de clases** (93.5% no-ganadores vs 6.5% ganadores), que los modelos boosting gestionan de forma natural al focalizar el aprendizaje en los ejemplos difíciles. Entre las limitaciones más relevantes destacan el tamaño reducido del dataset (304 registros), un posible data leakage en las features históricas acumuladas, y la ausencia de variables clave como condiciones climáticas o telemetría.

Como mejoras futuras se propone aplicar SMOTE para el desbalance, implementar feature engineering temporal para eliminar el data leakage, explorar XGBoost/LightGBM, e incorporar nuevas fuentes de datos que capturen el estado de forma actual del piloto.

---

## Tecnologías

- Python 3.14
- pandas, numpy
- scikit-learn (KNeighborsClassifier, RandomForestClassifier, GradientBoostingClassifier, AdaBoostClassifier, GridSearchCV, StratifiedKFold)
- matplotlib, seaborn

## Presentación 
https://canva.link/w41qxnh8ygrmyg9
