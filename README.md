**Поставнока задачи**
***Описание проблемы***
Из «Бета-Банка» стали уходить клиенты. Каждый месяц. Немного, но заметно. Банковские маркетологи посчитали: сохранять текущих клиентов дешевле, чем привлекать новых. 
***Поставнока задачи***
Спрогнозировать, уйдёт клиент из банка в ближайшее время или нет 
***Инструменты реализации***
Были предоставлены исторические данные о поведении клиентов и расторжении договоров с банком. Необходимо было построить модель с предельно большим значением F1-меры. Требовалось довести метрику до 0.59. Проверить F1-меру на тестовой выборке.
Дополнительно требовалось измерять AUC-ROC, сравнивайте её значение с F1-мерой.
**Порядок решения**
1. Провели предобработку полученных статистичских данных: анализ пропусков, дубликатов, аномалий и т.д.
2. Провели исследование исходных данных. В результате исследования факторов можно сделать следующие выводы:
- клиенты из Германии в 2 раза чаще уходят из банка, чем клиенты из других стран;
- женщины чаще покидают банк, чем мужчины;
- примерно одинаковая доля клиентов ушедших и оставшихся имеют кредитные карты, вероятнее всего этот фактор не оказывает влияние на факт ухода клиента из банка;
- активные клиенты уходят из банка реже;
- клиенты, имеющие 2 продукта более надежны, чем те, у кого их 1,3 или 4 продукта;
- возраст влияет на вероятность ухода клиента из банка, по результатам статистического теста установлено, что клиенты, которые уходят из Банка более старшего возраста, чем те, которые остаются;
- по результатам статического теста установлено, что у клиентов, которые уходят из банка рейтинг ниже;
- баланс на счетах у клиентов которые покидают банк выше, чем у клиентов, которые остаются в банке (установлено результатом статистического теста);
- уровень заработной платы в 2 группах одинаковый и не влияет на факт ухода клиента (установлено результатом статистического теста);
- анализируя фактор продолжительности отношений с банком было отмечено, что доля ухода в зависимости от продолжительности отношений с банком примерно одинаковая, как для ушедших клиентов, так и для оставшихся с банком;
- среднего или высокого уровня корреляции между признаками не наблюдается.
3. Было выполнено кодирование переменных.
По результатам данного этапа:
- категориальные признаки мыли закодированы техникой OHE
- другие числовые признаки были масштабированы с применяем метода StandardScaler 
- исходная выборка была разделена на обучающую (60%), валидационную (20%) и тестовую (20%).
4. Обучение проведено моделями: Random Forest и  логистической регрессии. Метрика ROC-AUC для модели случайного леса на уровне 0.82 показывает, что модель отвечает лучше, чем случайным образом. В целом модель случайного леса демонстрирует необходимый уровень F1 на несбалансированных данных. Следующим шагом  устраненили дисбаланс. Было рассмотренно несколько методов борьбы с дисбалансом, каждый из них улучшал результаты по сравнениию с оценкой на не сбалансированных данных. При этом  методики балансирования классов и повышения размероности дали резульаты лучше, чем сокращение размерности. Также отметим, что модель случайного леса в каждом случае показывала свое превосходство над логистической регрессией. По результатам данного этапа отобрана лучшая модель (по максимальному значению F1 меры, которая будет проверена на тестовой выборке).

**Вывод**
1) По результатам исследования задачи  были установлены признаки, котоыре не влияют на факт ухода клиента из банка. Темне-менее часть признаков оставлена, так как не исключается возможность, что в совокупности с другими признаками эти признаки могут повлиять на итоговое решение.

2) Учитывая, что в выборке присутстовал сильный дисбаланс (80/ 20) результаты тестирования моделей не дали должного уровня F1 меры (по условиям задачи не менее 0.59)

3) Были рассмотрено несколько методов борьбы с дисбалансом, лушчими оказались метод взвешивания классов и увеличение размера обучающей выборки.

4) По результатам тестирования моделей установлено, что случайный лес дает результат намного выше, чем логистическая регрессия.

5) Наибольшее значение F1 меры и ROC-AUC на валидационной выборке показла модель случайного леса с взаешиванием классов. После чего был проверен оптимальный порог классификации и установлено, что для лучшией модели порог по умолчанию и явялется оптимальным при максимизации F1 меры

6) Лучшая найденная модель на валидационной выборке была проверена на тестовой и показала значение F1 меры на уровне 0.5978. В целом показатель ROC-AUC у моделей находится на довольно не плохом уровне и показывает, что модель прогнозировали не случайно и в большинстве случаев отвечали верно. 

7) Дополнительно проанализирована матрица ошибок и значение точности и полноты. было установлено, что модель лучше оценивает клиентов котоыре покидают банк, чем клиентов, котоыре остаются (Точность = 0.5483, Полнота = 0.6572).

8) Также проведена оценка важности признаков - тройка лидеров это возраст, количество проудктов и баланс. Тройка наименее значимых факторов проживание в Испании, наличие кредитной карты и мужской пол.
