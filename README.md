# Тестовое задание.
## Часть 1
### 1. Разработать структуру базы данных для сервиса выдачи автомобилей в аренду. Предоставитьсхему БД с указанием таблиц (поля, тип полей), связей, ключей и т.д. Основные сущности (можнодобавлять по необходимости): клиенты, автомобили (свободен, арендован, на ТО и проч.), заказы (подготовлен, выполняется, закрыт, отменен и проч.) и проч.

<div align='center'>
<img src='/images/26.png' width='800'>
</div>

```
-- Таблица "Клиенты" (Customers)
CREATE TABLE Customers (
    customer_id INT PRIMARY KEY,
    first_name VARCHAR(255),
    last_name VARCHAR(255),
    email VARCHAR(255),
    phone_number VARCHAR(20)
);

-- Таблица "Автомобили" (Cars)
CREATE TABLE Cars (
    car_id INT PRIMARY KEY,
    brand VARCHAR(255),
    model VARCHAR(255),
    status VARCHAR(50),
    registration_number VARCHAR(20)
);

-- Таблица "Заказы" (Orders)
CREATE TABLE Orders (
    order_id INT PRIMARY KEY,
    customer_id INT,
    car_id INT,
    order_status VARCHAR(50),
    start_date timestamp without time zone,
    end_date timestamp without time zone,
    FOREIGN KEY (customer_id) REFERENCES Customers(customer_id),
    FOREIGN KEY (car_id) REFERENCES Cars(car_id)
);

-- Таблица "Платежи" (Payments)
CREATE TABLE Payments (
    payment_id INT PRIMARY KEY,
    order_id INT,
    amount DECIMAL(10, 2),
    payment_date timestamp without time zone,
    FOREIGN KEY (order_id) REFERENCES Orders(order_id)
);

-- Таблица "Техобслуживание" (Maintenance)
CREATE TABLE Maintenance (
    maintenance_id INT PRIMARY KEY,
    car_id INT,
    maintenance_date timestamp without time zone,
    description TEXT,
    FOREIGN KEY (car_id) REFERENCES Cars(car_id)
);

```
### 2. Разработать и описать (на русском языке) логику функционирования сервиса.

Регистрация клиента:
- Новые клиенты регистрируются в системе, предоставляя свои персональные данные, такие как имя, фамилия, адрес электронной почты и номер телефона.
- Система генерирует уникальный идентификатор для каждого клиента (customer_id) и сохраняет информацию в таблице "Клиенты".

Управление автомобилями:
- Администратор системы добавляет информацию о доступных автомобилях, включая марку, модель, статус (свободен, арендован, на техобслуживании и т.д.) и регистрационный номер.
- Система присваивает уникальный идентификатор (car_id) каждому автомобилю и сохраняет данные в таблице "Автомобили".

Оформление заказа:
- Клиент выбирает доступный автомобиль и указывает дату и время начала и окончания аренды.
- Система создает новый заказ, присваивает уникальный идентификатор (order_id), связывает его с клиентом и выбранным автомобилем.
- Статус заказа устанавливается как "подготовлен".

Подтверждение заказа:
- Система проверяет доступность выбранного автомобиля на указанные даты.
- Если автомобиль доступен, статус заказа изменяется на "выполняется", и начинается период аренды.

Завершение заказа:
- По истечении срока аренды клиент возвращает автомобиль.
- Система обновляет статус заказа на "закрыт" и рассчитывает сумму платежа на основе длительности аренды.
- Создается запись о платеже в таблице "Платежи".

Техобслуживание:
- Если автомобиль требует технического обслуживания, создается запись в таблице "Техобслуживание" с указанием даты и времени, а также описанием работ.
- Статус автомобиля изменяется на "на техобслуживании".

Отмена заказа:
- В случае отмены заказа клиентом, система изменяет статус заказа на "отменен".
- Если заказ был подготовлен, автомобиль снова становится доступным для аренды.

Отчеты и статистика:
- Система предоставляет возможность администратору просматривать отчеты и статистику по выполненным заказам, доходам, наличию автомобилей и другим ключевым показателям.

### 3. Описать (на русском языке) логику основных функций и реализовать эту логику в виде SQLскриптов:
#### a. Работа с заказом: создание, закрытие, отмена и т.д.
```
-- Добавление заказа.
INSERT INTO Orders (customer_id, car_id, order_status, start_date, end_date)
VALUES (1, 101, 'подготовлен', '2023-01-01 10:00:00', '2023-01-03 18:00:00');

-- Закрытие 
UPDATE Orders
SET order_status = 'закрыт'
WHERE order_id = 1;

-- Изменение 
UPDATE Orders
SET order_status = 'отменен'
WHERE order_id = 1;

-- Вывод первого заказа
SELECT *
FROM Orders
WHERE customer_id = 1;

-- Проверка заказов
SELECT *
FROM Orders
WHERE order_status IN ('выполняется', 'подготовлен');
```
#### b. Работа с клиентом: создание, редактирование, блокировка и т.д.

```
-- Создание нового клиента
INSERT INTO Customers (customer_id, first_name, last_name, email, phone_number)
VALUES (1, 'Иван', 'Иванов', 'ivan@example.com', '+7 (123) 456-7890');

-- Редактирование информации о клиенте
UPDATE Customers
SET email = 'ivan.ivanov@example.com'
WHERE customer_id = 1;

-- Блокировка клиента
UPDATE Customers
SET is_blocked = TRUE
WHERE customer_id = 1;

-- Разблокировка клиента
UPDATE Customers
SET is_blocked = FALSE
WHERE customer_id = 1;

-- Получение информации о клиенте
SELECT * FROM Customers WHERE customer_id = 1;
```

#### c. Работа с авто: добавление нового, вывод из эксплуатации, перевод из статуса в статус и т.д.

```
-- Добавление нового автомобиля
INSERT INTO Cars (car_id, brand, model, status, registration_number)
VALUES (101, 'Toyota', 'Camry', 'свободен', 'ABC123');

-- Вывод из эксплуатации автомобиля
UPDATE Cars
SET is_retired = TRUE
WHERE car_id = 101;

-- Перевод автомобиля из статуса в статус
UPDATE Cars
SET status = 'арендован'
WHERE car_id = 101;

-- Получение информации об автомобиле
SELECT * FROM Cars WHERE car_id = 101;
```

#### d. Сохранение и просмотр истории заказов

```
-- Сохранение заказа при создании заказа
INSERT INTO Orders (order_id, customer_id, car_id, order_status, start_date, end_date)
VALUES (1, 1, 101, 'подготовлен', '2023-01-01 10:00:00', '2023-01-03 18:00:00');

-- Сохранение заказа при закрытии заказа
INSERT INTO Orders (order_id, customer_id, car_id, order_status, start_date, end_date)
VALUES (1, 1, 101, 'закрыт', '2023-01-01 10:00:00', '2023-01-03 18:00:00');

-- Просмотр истории заказов для конкретного клиента
SELECT * FROM Orders WHERE customer_id = 1;

-- Просмотр истории заказов для конкретного автомобиля
SELECT * FROM Ordersy WHERE car_id = 101;
```

#### e. Финансовый отчет: за период времени сколько и какие автомобили были сданы в аренду, на сколько дней и стоимость аренды, с итоговой стоимостью всех авто.

```
-- Финансовый отчет за период времени
DECLARE @start_date DATETIME = '2023-01-01';
DECLARE @end_date DATETIME = '2023-01-31';

-- Общая стоимость аренды для каждого автомобиля
SELECT
    c.car_id,
    c.brand,
    c.model,
    COUNT(o.order_id) AS количество_аренд,
    SUM(DATEDIFF(DAY, o.start_date, o.end_date) + 1) AS общее_количество_дней,
    SUM(p.amount) AS общая_стоимость_аренды
FROM
    Cars c
LEFT JOIN
    Orders o ON c.car_id = o.car_id AND o.start_date BETWEEN @start_date AND @end_date
LEFT JOIN
    Payments p ON o.order_id = p.order_id
GROUP BY
    c.car_id, c.brand, c.model
ORDER BY
    общая_стоимость_аренды DESC;

-- Итоговая стоимость аренды всех автомобилей
SELECT
    SUM(p.amount) AS итоговая_стоимость_аренды
FROM
    Payments p
JOIN
    Orders o ON p.order_id = o.order_id AND o.start_date BETWEEN @start_date AND @end_date;

```

#### f. Отчет о состоянии автопарка: сколько и какие авто в каком состоянии находятся

```
-- Отчет о состоянии автопарка
SELECT
    car_id,
    brand,
    model,
    status
FROM
    Cars;

-- Подсчет количества автомобилей в каждом статусе
SELECT
    status,
    COUNT(*) AS количество_автомобилей
FROM
    Cars
GROUP BY
    status;
```

## Часть 2

### Выполнить на ВМ следующие инструкции:
a. Создайте файл file1 в текущей директории
<div align='center'>
<img src='/images/2.png' width='800'>
</div>
b. Создайте файл file2 в текущей директории
<div align='center'>
<img src='/images/2.png' width='800'>
</div>
c. Задайте файлу file2 время модификации - 1 января 1990 года, 0 часов, 0 минут, 0 секунд
<div align='center'>
<img src='/images/3.png' width='800'>
</div>
d. Задайте файлу file2 права доступа: rwxrw--—
<div align='center'>
<img src='/images/4.png' width='800'>
</div>
e. Создайте вложенные директории dir1/dir2/dir3/ в текущем каталоге
<div align='center'>
<img src='/images/5.png' width='800'>
</div>
f. Задайте цепочке вложенных директорий dir1/dir2/dir3/ права доступа: rwxrw--—
<div align='center'>
<img src='/images/6.png' width='800'>
</div>
g. Скопируйте файл file1 в директорию dir1/dir2/dir3/
<div align='center'>
<img src='/images/7.png' width='800'>
</div>
h. Переместите файл file2 в директорию dir1/dir2/dir3/
<div align='center'>
<img src='/images/8.png' width='800'>
</div>
i. С помощью специальных команд Linux определите тип каталога dir1
<div align='center'>
<img src='/images/9.png' width='800'>
</div>
j. С помощью специальных команд Linux выведите количество секунд, прошедших с 1 января 1970
года
<div align='center'>
<img src='/images/10.png' width='800'>
</div>
k. Очистите текущий каталог от созданных файлов и каталогов
<div align='center'>
<img src='/images/11.png' width='800'>
</div>

### 5. Выполнить на ВМ следующие инструкции:

a. Создайте каталог public в текущей директории
<div align='center'>
<img src='/images/12.png' width='800'>
</div>
b. Создайте каталог private в текущей директории
<div align='center'>
<img src='/images/12.png' width='800'>
</div>
c. Создайте в каталоге private скрытый каталог, имя может быть любым
<div align='center'>
<img src='/images/13.png' width='800'>
</div>
d. Создайте в каталоге public директорию с именем New folder
<div align='center'>
<img src='/images/14.png' width='800'>
</div>
e. Создайте в каталоге New folder (из предыдущего шага) файл file.txt
<div align='center'>
<img src='/images/15.png' width='800'>
</div>
f. Создайте мягкую ссылку soft-link-file.txt в той же директории New folder на файл file.txt
<div align='center'>
<img src='/images/16.png' width='800'>
</div>
g. Удалите все файлы и каталоги, созданные на предыдущих шагах
<div align='center'>
<img src='/images/17.png' width='800'>
</div>

### 6. Выполнить на ВМ следующие инструкции:

a. Выяснить, сколько оперативной памяти доступно на компьютере. Сколько из них заняты,
сколько свободны.
<div align='center'>
<img src='/images/18.png' width='800'>
</div>
b. Выясните, какие TCP порты открыты (прослушиваются). Адрес и порт должны отображаться в
цифровом виде. Отображение не интернет портов считается ошибкой.
<div align='center'>
<img src='/images/19.png' width='800'>
</div>
c. Выясните, какие порты открыты (прослушиваются). Адрес и порт должны отображаться в
цифровом виде. Отображение не интернет портов считается ошибкой.
<div align='center'>
<img src='/images/20.png' width='800'>
</div>
d. Создайте каталог dir в текущей директории
<div align='center'>
<img src='/images/21.png' width='800'>
</div>
e. Замерьте, сколько по времени выполняется следующая команда: Создание в каталоге dir 99
директорий с именами от sub_dir01 до sub_dir99
<div align='center'>
<img src='/images/22.png' width='800'>
</div>
<div align='center'>
<img src='/images/23.png' width='800'>
</div>
f. Выясните, сколько места на жёстком диске занимает каталог dir и всё его содержимое
<div align='center'>
<img src='/images/24.png' width='800'>
</div>
g. Удалите все файлы и каталоги, созданные на предыдущих шагах
<div align='center'>
<img src='/images/25.png' width='800'>
</div>
