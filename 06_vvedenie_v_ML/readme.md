# `Проект:` Классификаиция клиентов телеком компании

## Описание проекта и Цель
Оператор мобильной связи выяснил, что многие клиенты пользуются архивными тарифами. Они хотят проанализировать поведение клиентов и предложить им один из двух новых тарифов.

**Цель:** Нужно построить модель для задачи классификации, которая выберет подходящий тариф. Построить модель с максимально большим значением accuracy (по крайней мере до 0.75).

## Описание датасета
В вашем распоряжении данные о поведении клиентов, которые уже перешли на эти тарифы. Данные уже предобработаны, т.к. обработка этого датасета производилась в одном из предыдущих спринтов "Статистический анализ данных"
## Постановка задач
* Открыть файл с данными и изучить его.
* Поделить исходные данные на обучающую, валидационную и тестовую выборки.
* Исследовать качество разных моделей, меняя гиперпараметры.
* Проверить качество модели на тестовой выборке и проверить модель на вменяемость.

## Применяемые библиотеки
`pandas` `scikit-learn` `random`
# Основные выводы
Данный проект - первый, касающийся именно применения Машинного Обучения. Хотя данные были готовы к выделению признаков, ввиду предобработки данных раннее в другом проекте с этим же датасетом, все же не лишним было изучить данные.
Изучив целевой признак, я удостоверился, что изучение и проверку данных нужно делать всегда - был выявлен дисбаланс классов (класс "1" занимает 70%).
Существует множество подходов, чтобы учесть дисбаланс классов. В моем случае на этапе разделения датасета на выборки был использован параметр stratify = y в функции train_test_split( ), где y – соответствующая целевая переменная.
Параметр работает следующим образом: если test_size=0.2, то в выделяемую выборку попадет по 20% из каждого кластера. Таким образом, соотношение классов сохранится таким же во всех выборках.

Разделив датасет на выборки, я приступил к обучению моделей.
В данном проекте были рассмотрены 3 модели классификации: `LogisticRegression`, `DecisionTreeClassifier` и `RandomForestClassifier`
Итак, мы знаем, что в общем и целом, если ранжировать 3 указанные модели по `accuracy_score` :
* Самое высокое качество (accuracy) у случайного леса;
* На втором месте — логистическая регрессия. Модель несложная, а значит, переобучение ей не грозит.
* Самое низкое качество предсказания у дерева решений. Если глубина примерно меньше четырёх, оно недообучается, когда больше — переобучается.
### Подбирая гиперпараметры и выявив лучшие, на практике мы действительно получили, что самое высокое качество имеет модель Случайного леса с aacuracy_score на тестовой выборке равной 0.82. При этом на втором месте оказалась модель Дерево решений и на последнем - Логистическая регрессия

Дополнительно были проверены модели через метрики качества: `recall` и `precision`. Расчет метрик показал и подтвердил, что лучшей моделью является RandomForest. 
* `recall_ score` = 0.525. То есть из всех правильных ответов он обнаружил 52,5%
* `precision_score` = 0.806. То есть из тех ответов, которые модель пометила маркером, действительно правильны из них 80.6%

Проверили также модель на вменяемость. Как правило для проверки на адекватность сравниваем метрики нашей модели с метриками какой-то очень примитивной модели (например, константной, которая всегда предсказывает один класс). Если наша модель показывает худшие метрики, то она не адекватна.