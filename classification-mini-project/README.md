# Порівняння класичних моделей класифікації

Мініпроєкт практичних робіт 4-7 з дисципліни "Розпізнавання образів і машинне навчання".

Виконав: Конопля Іван, група ІН-42/1.

## Набір даних

Heart Disease (Cleveland), UCI Machine Learning Repository:
https://archive.ics.uci.edu/dataset/45/heart+disease

Файл `data/heart_disease.csv`: 303 об'єкти, 13 вхідних ознак, цільова змінна `target`
(0 - ознаки серцевого захворювання відсутні, 1 - наявні; початкові значення 1-4 об'єднано в клас 1).

## Структура

- `data/heart_disease.csv` - набір даних;
- `notebooks/PR04_Konoplia_I.ipynb` - наскрізний Notebook практичних робіт 4-7;
- `models/best_classifier.joblib` - фінальний Pipeline;
- `reports/cv_model_comparison.csv` - порівняння моделей за крос-валідацією;
- `reports/final_test_metrics.csv` - фінальні test-метрики;
- `README.md` - опис проєкту.

Результати будуть додані після виконання практичних робіт 4-7.
