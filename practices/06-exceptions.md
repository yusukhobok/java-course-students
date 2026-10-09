# Практика 6. Обработка исключений

## Выбор варианта

| Вариант | БОМ31ПРИ          | БОМ31ТВР         |
| ------- | ----------------- | ---------------- |
| 1       | Бруева А.В.       | Боярчук И.Ю.     |
| 2       | Быкова М.В.       | Виденкин Ю.Е.    |
| 3       | Верхозина В.Ф.    | Десятериков Р.В. |
| 4       | Галсанов Т.В.     | Дмитренко А.О.   |
| 5       | Гильмутдинов Т.С. | Иванов Р.А.      |
| 6       | Громова Т.В.      | Квязова Н.И.     |
| 7       | Данилушкина А.М.  | Коноплев В.А.    |
| 8       | Дерюшев А.Р.      | Косинов Н.А.     |
| 9       | Казаков П.Е.      | Кудрявский Н.С.  |
| 10      | Калоша Е.Д.       | Левочкин Д.Е.    |
| 11      | Круковский И.А.   | Митрейкин Д.В.   |
| 12      | Москвалев А.С.    | Николаев А.В.    |
| 13      | Пестряков Н.Е.    | Новиков А.С.     |
| 14      | Побежимов С.В.    | Одношевный Р.А.  |
| 15      | Подопригора Е.А.  | Олейников А.Н.   |
| 16      | Стрельникова С.Д. | Постол С.Г.      |
| 17      | Титов М.В.        | Романенко К.В.   |
| 18      | Хафизова А.А.     | Урбан А.В.       |
| 19      | Чернявский Г.М.   | Фролова А.Д.     |
| 20      | Черняховский Р.С. | Хоботов А.В.     |
| 21      | Кощей Д.          | Шаплов А.И.      |

## Общая постановка

1. Создайте **checked**-исключение варианта (имя в таблице).
2. Класс предметной области: конструктор/фабрика **или** бизнес-метод бросает это исключение при нарушении инварианта (для «занято»/«просрочено» — там, где нарушение фактически обнаруживается: `book()`, проверка при продаже и т.п.).
3. Метод разбора строки/числа: неверный формат → `IllegalArgumentException` (с cause при необходимости).
4. Консольное меню (цикл): команды варианта; ошибки не роняют программу.
5. Успешные операции дописывайте в `log.txt` (UTF-8, try-with-resources / `Files.writeString` + APPEND).
6. Режим debug с печатью stack trace.

## Варианты

| № | Исключение | Предметный класс / инвариант | Команды меню (минимум) |
|---|------------|------------------------------|-------------------------|
| 1 | `InvalidAgeException` | Person: age ∈ [0;150] | `create`, `parsePort`, `exit` |
| 2 | `InvalidPriceException` | Product: price > 0 | `addProduct`, `parseSku`, `exit` |
| 3 | `InvalidGpaException` | Student: gpa ∈ [0;5] | `enroll`, `parseYear`, `exit` |
| 4 | `InvalidDoseException` | Medicine: dose > 0 | `prescribe`, `parseCount`, `exit` |
| 5 | `InvalidSeatException` | Ticket: seat ∈ [1;100] | `book`, `parseFlight`, `exit` |
| 6 | `InsufficientFundsException` | Account: withdraw ≤ balance | `deposit`, `withdraw`, `exit` |
| 7 | `InvalidScoreException` | Match: score ≥ 0 | `addMatch`, `parseRound`, `exit` |
| 8 | `InvalidRatingException` | Movie: rating ∈ [0;10] | `addMovie`, `parseYear`, `exit` |
| 9 | `EmptyOrderException` | Order: items > 0 | `newOrder`, `parseTable`, `exit` |
| 10 | `OverweightException` | Parcel: weight ∈ (0;50] | `send`, `parseTrack`, `exit` |
| 11 | `RoomOccupiedException` | Booking: нельзя занять занятый | `book`, `free`, `exit` |
| 12 | `InvalidVinException` | Car: VIN длина 17 | `register`, `parseYear`, `exit` |
| 13 | `InvalidYearException` | Exhibit: year ≠ 0 разумный диапазон | `add`, `parseHall`, `exit` |
| 14 | `InvalidZipException` | Address: индекс 6 цифр | `setAddress`, `parseCode`, `exit` |
| 15 | `InvalidDistanceException` | Trip: distance > 0 | `ride`, `parseTariff`, `exit` |
| 16 | `ExpiredItemException` | Drug: expiry в будущем | `addLot`, `parseQty`, `exit` |
| 17 | `InvalidPlanException` | Membership: months ∈ [1;24] | `sign`, `parseType`, `exit` |
| 18 | `ExcessBaggageException` | Baggage: weight ≤ limit | `checkIn`, `parseTag`, `exit` |
| 19 | `InvalidLevelException` | Item: level ∈ [1;100] | `craft`, `parseRarity`, `exit` |
| 20 | `InvalidReadingException` | Reading: temp ∈ [-80;60] | `add`, `parseStation`, `exit` |
| 21 | `InvalidGradeException` | Grade: score ∈ [2;5] | `addGrade`, `parseSubject`, `exit` |

Для команд `parse*` используйте разбор числа/кода с обработкой `NumberFormatException`.

## Состав отчёта (печатный вид)

Отчёт сдаётся **в распечатанном виде**.

1. **Титульный лист** — дисциплина «Java-программирование»; номер и название практики; ФИО, группа, **номер варианта**; дата сдачи.
2. **Постановка задачи** — условие своего варианта (для практик 1–2 — номера задач из задачника и краткая суть каждой).
3. **Ход выполнения** — как решали задачу: структура программы, важные типы/классы/методы, при необходимости блок-схема или краткий алгоритм.
4. **Листинг программы** — исходный код (весь проект или ключевые классы/методы) моноширинным шрифтом, читаемый кегль.
5. **Результаты работы** — не менее 2–3 прогонов: входные данные и полученный вывод (текст консоли или скриншоты с читаемым текстом).

На защите дополнительно демонстрируется **работающая программа** (исходники + запуск).

