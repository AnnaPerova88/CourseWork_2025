# CourseWork_2025

# Курсовая работа: Предсказание IC50, CC50 и SI по структуре молекул  👨‍🔬 Автор: Анна Перова
Магистратура МИФИ | Направление: Прикладная математика и информатика  📅 Июнь 2025


🎯 Цель исследования

Разработка моделей для прогнозирования ключевых биомедицинских показателей химических соединений:

* IC50 — ингибирующая концентрация (подавляет 50% активности);
* CC50 — цитотоксическая концентрация (вызывает 50% клеточной гибели);
* SI (Selectivity Index) — показатель терапевтической широты (отношение CC50 / IC50).

Модели машинного обучения позволяют определить перспективность соединения ещё до лабораторных тестов, сокращая затраты на синтез и первичное тестирование.

📦 Данные

* Предоставлены числовые и категориальные признаки молекул;
* Обнаружены пропуски, выбросы, перекосы в распределении (особенно у SI);
* Применено логарифмирование SI для нормализации (log10(SI)).

📋 Построенные задачи

| №  | Задача                           | Тип            | Цель                                               |
|----|----------------------------------|----------------|----------------------------------------------------|
| 1  | Регрессия IC50                  | Регрессия      | Прогноз ингибирующей концентрации                  |
| 2  | Регрессия CC50                  | Регрессия      | Прогноз токсичности                                |
| 3  | Регрессия log(SI)               | Регрессия      | Прогноз терапевтической широты                     |
| 4  | Классификация IC50 > median     | Классификация  | Разделение по эффективности                        |
| 5  | Классификация CC50 > median     | Классификация  | Разделение по токсичности                          |
| 6  | Классификация SI > median       | Классификация  | Разделение по селективности                        |
| 7  | Классификация SI > 8            | Классификация  | Перспективность соединения по SI                  |

🧠 Мотивация

SI отражает баланс эффективности и безопасности соединения;

* Классический порог SI ≥ 10 считается золотым стандартом, но может быть слишком строгим;
* Предлагается гибкая система оценки с порогами SI ≥ 8 и SI > median;
* Для интерпретации результатов предложена визуально понятная шкала “токсикологического светофора”.

🧰 Методы и этапы

* Предобработка данных (удаление выбросов, заполнение NaN);
* Логарифмирование SI;
* Построение регрессионных моделей: CatBoost, HistGradientBoosting, RandomForest, XGBoost;
* Построение классификаций: CatBoostClassifier, Logistic Regression, KNN;
* Анализ метрик: MAE, RMSE, R², Accuracy, F1, ROC AUC.

📊 Регрессия: основные результаты

| Показатель | Модель               | MAE   | RMSE  | R²    |
|------------|----------------------|--------|--------|--------|
| IC50       | CatBoostRegressor    | ~8.6   | ~21.7  | ~0.15  |
| CC50       | HistGradientBoosting | ~141   | ~1173  | ~0.005 |
| log(SI)    | CatBoostRegressor    | ~9.9   | ~21.7  | ~0.15  |

⚠️ Низкое R² объясняется биологической шумностью. Тем не менее, модели дают приближенную, полезную на скрининге оценку.

Эффект логарифмирования SI

| Метрика | Без логарифма | С логарифмом | Улучшение        |
|---------|----------------|---------------|------------------|
| MSE     | 1,376,795.50   | 473.03        | ✅ ~×2900         |
| RMSE    | 1173.37        | 21.75         | ✅ намного ниже   |
| MAE     | 141.54         | 9.86          | ✅ точность ↑     |
| R²      | 0.0046         | 0.1480        | ✅ структура найдена |

✅ Классификация: основные результаты

| Целевая задача     | Модель              | Accuracy | ROC AUC |
|--------------------|---------------------|----------|---------|
| IC50 > median      | CatBoostClassifier  | ~0.76    | ~0.83   |
| CC50 > median      | HistGradientBoosting| ~0.78    | ~0.81   |
| SI > median        | CatBoostClassifier  | ~0.75    | ~0.85   |
| SI > 8             | HistGradientBoosting| ~0.80    | ~0.84   |

Классификация SI > 8 выбрана как основной биомаркер для предварительного отбора.

🚦 "Токсикологический светофор"

| Цвет | log(SI)              | Интерпретация               |
|------|----------------------|-----------------------------|
| 🟢   | > 1.0                | Высокая селективность       |
| 🟡   | 0.5 ≤ log(SI) ≤ 1.0  | Пограничные значения        |
| 🔴   | < 0.5                | Высокий риск, нежелательно  |

Основано на подходе "Traffic Light Classification" (Gore et al., Green Chem., 2013), адаптировано под ML и биомедицину.

📌 Краткие выводы
* 📈 Логарифмирование SI значительно улучшило качество регрессионной модели (снижение MSE почти в 2900 раз);
* 🧠 Регрессия позволяет оценить численные значения биомаркеров (IC50, CC50, SI), полезные на этапе первичного скрининга;
* ✅ Классификация позволяет выделять перспективные соединения по критериям SI ≥ median и SI ≥ 8 — оба метода показали высокую точность;
* 🚦 Визуальная шкала ("токсикологический светофор") делает результаты интерпретируемыми для прикладных специалистов;
* 💡 Результаты могут использоваться в виртуальном скрининге для приоритезации соединений в фармакологических исследованиях.
