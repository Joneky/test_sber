# Тестовое задание.
## Часть 1

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
