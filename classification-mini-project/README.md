# Порівняння класичних моделей класифікації

Мініпроєкт практичних робіт 4-7 з дисципліни "Розпізнавання образів і машинне навчання".

Виконав: Конопля Іван, група ІН-42/1.

[![Open In Colab](https://colab.research.google.com/assets/colab-badge.svg)](https://colab.research.google.com/github/Konoplyai/pattern-recognition-practice/blob/main/classification-mini-project/notebooks/PR04_Konoplia_I.ipynb)

## Постановка задачі

За клінічними характеристиками пацієнта потрібно визначити, чи належить він до класу
з наявністю ознак серцевого захворювання. Задача бінарної класифікації з навчанням
за розміченими даними. Позитивний клас: **1** (ознаки захворювання наявні).
Основна метрика, обрана до експериментів: **F1** для класу 1, додаткові: ROC AUC і Recall.

Результати навчального мініпроєкту не є медичним діагнозом.

## Набір даних

- Heart Disease (Cleveland), UCI Machine Learning Repository: https://archive.ics.uci.edu/dataset/45/heart+disease
- `data/heart_disease.csv`: 303 об'єкти, 13 вхідних ознак, 6 пропусків (`ca`: 4, `thal`: 2).
- Числові ознаки: age, trestbps, chol, thalach, oldpeak.
- Категоріальні ознаки: sex, cp, fbs, restecg, exang, slope, ca, thal.
- Цільова змінна: `target` (0: 164 об'єкти, 1: 139 об'єктів).

## Методика експерименту

- фіксований стратифікований train/test split 80/20, random_state=42 (242 / 61 об'єкт);
- підбір алгоритму і гіперпараметрів лише на train;
- єдина StratifiedKFold(n_splits=5, shuffle=True, random_state=42) для всіх моделей;
- одноразове фінальне оцінювання на test лише зафіксованої моделі.

## Підготовка даних

- SimpleImputer (median для числових, most_frequent для категоріальних);
- StandardScaler для числових ознак;
- OneHotEncoder(handle_unknown="ignore") для категоріальних ознак;
- ColumnTransformer;
- Pipeline (препроцесинг і класифікатор навчаються разом на train-частині кожного фолду).

## Досліджені моделі

- Logistic Regression (L1, L2, C = 0.01...100, зі стандартизацією і без неї);
- k-NN (k = 3...15, евклідова і мангеттенська відстань, зі стандартизацією і без неї);
- Gaussian Naive Bayes;
- LDA;
- QDA (reg_param = 0, 0.01, 0.1);
- SVM (linear і RBF, GridSearchCV за C і gamma).

## Порівняння за крос-валідацією

| Модель | Найкращі параметри | CV F1 | CV F1 std | CV Recall | CV ROC AUC |
|---|---|---:|---:|---:|---:|
| Logistic Regression | L1, C=100, liblinear | 0.8363 | 0.0127 | 0.8016 | 0.8974 |
| SVM | rbf, C=10, gamma=0.01 | 0.8270 | 0.0330 | 0.7830 | 0.8968 |
| k-NN | k=3, manhattan | 0.8175 | 0.0304 | 0.7917 | 0.8768 |
| LDA | solver=svd | 0.8128 | 0.0257 | 0.7656 | 0.8993 |
| QDA | reg_param=0.1 | 0.7962 | 0.0271 | 0.7743 | 0.8680 |
| GaussianNB | - | 0.7495 | 0.0215 | 0.8458 | 0.8773 |

QDA без регуляризації не навчається: коваріаційна матриця класу 0 вироджена через one-hot-ознаки.

Повна таблиця: `reports/cv_model_comparison.csv`.

## Фінальна модель і test-результат

Фінальна модель зафіксована до відкриття test: **Logistic Regression, L1, C = 100, solver = liblinear**.
Вона має найкращий CV F1 (0.8363), найменший розкид між фолдами (std 0.0127),
вищий recall за SVM і легко інтерпретується.

| Test Accuracy | Test Precision | Test Recall | Test F1 | Test ROC AUC |
|---:|---:|---:|---:|---:|
| 0.8689 | 0.8333 | 0.8929 | 0.8621 | 0.9578 |

Test-результат узгоджується з крос-валідацією (CV F1 = 0.8363 +/- 0.0127); невелике
перевищення пояснюється малим розміром тестової вибірки (61 об'єкт).
Після перегляду test модель не змінювалася.

Збережений Pipeline: `models/best_classifier.joblib` (препроцесинг + класифікатор).

## Запуск

Відкрити Notebook `notebooks/PR04_Konoplia_I.ipynb` у Google Colab і послідовно виконати всі комірки.
Дані завантажуються з цього репозиторію автоматично.
