# Итоговая контрольная работа по блоку специализация

## Используя команду cat в терминале операционной системы Linux, создать два файла Домашние животные (заполнив файл собаками, кошками, хомяками) и Вьючные животными заполнив файл Лошадьми, верблюдами и ослы), а затем объединить их. Просмотреть содержимое созданного файла. Переименовать файл, дав ему новое имя (Друзья человека).

```
cat > "Домашние животные"
собаки
кошки
хомяки

ctrl + D (для выхода из редактора и сохранения файла)

cat > "Вьючные животными"
лошади
верблюды
ослы

ctrl + D

cat "Домашние животные" "Вьючные животными" > "Друзья человека"

mv "Друзья человека" "Мои любимцы"

```

---

## Создать директорию, переместить файл туда.

```
mkdir Мои_животные

ls Мои_животные/ (проверка файла в директории)
```

---

## Подключить дополнительный репозиторий MySQL. Установить любой пакет из этого репозитория.

```
wget -c https://dev.mysql.com/get/mysql80-community-release-el7-3.noarch.rpm

sudo yum localinstall mysql80-community-release-el7-3.noarch.rpm

sudo yum update

sudo yum install mysql-server
```

---

## Установить и удалить deb-пакет с помощью dpkg.

```
wget https://dl.google.com/linux/direct/google-chrome-stable_current_amd64.deb (скачивание deb файла Google Chrome)

sudo dpkg -i google-chrome-stable_current_amd64.deb (Установка пакета)

sudo dpkg -r google-chrome-stable (Удаление пакета)

sudo dpkg -P google-chrome-stable (Полное удаление)
```

Нарисовать диаграмму, в которой есть класс родительский класс, домашние
животные и вьючные животные, в составы которых в случае домашних
животных войдут классы: собаки, кошки, хомяки, а в класс вьючные животные
войдут: Лошади, верблюды и ослы.

```
Животные
|
|----Домашние животные
|   |
|   |----Собаки
|   |
|   |----Кошки
|   |
|   |----Хомяки
|
|----Вьючные животные
|   |
|   |----Лошади
|   |
|   |----Верблюды
|   |
|   |----Ослы
```

---

## В подключенном MySQL репозитории создать базу данных “Друзья человека”

```
mysql -u tanor -p

CREATE DATABASE `Друзья человека`;

SHOW DATABASES;

exit;
```

---

## Создать таблицы с иерархией из диаграммы в БД

```
CREATE TABLE Животные (
id INT PRIMARY KEY,
тип VARCHAR(50) NOT NULL
);

CREATE TABLE Домашние*животные (
id INT PRIMARY KEY,
имя VARCHAR(50) NOT NULL,
возраст INT,
id*животного INT,
FOREIGN KEY (id_животного) REFERENCES Животные(id)
);

CREATE TABLE Вьючные*животные (
id INT PRIMARY KEY,
имя VARCHAR(50) NOT NULL,
возраст INT,
id*животного INT,
FOREIGN KEY(id_животного) REFERENCES Животные(id)
);

CREATE TABLE Собаки (
id INT PRIMARY KEY,
порода VARCHAR(50) NOT NULL,
id*домашнего*животного INT,
FOREIGN KEY (id*домашнего*животного) REFERENCES Домашние_животные(id)
);

CREATE TABLE Кошки (
id INT PRIMARY KEY,
порода VARCHAR(50) NOT NULL,
id*домашнего*животного INT,
FOREIGN KEY (id*домашнего*животного) REFERENCES Домашние_животные(id)
);

CREATE TABLE Хомяки (
id INT PRIMARY KEY,
вид VARCHAR(50) NOT NULL,
id*домашнего*животного INT,
FOREIGN KEY (id*домашнего*животного) REFERENCES Домашние_животные(id)
);

CREATE TABLE Лошади (
id INT PRIMARY KEY,
окрас VARCHAR(50) NOT NULL,
id*вьючного*животного INT,
FOREIGN KEY (id*вьючного*животного) REFERENCES Вьючные_животные(id)
);

CREATE TABLE Верблюды(
id INT PRIMARY KEY,
окрас VARCHAR(50) NOT NULL,
id*вьючного*животного INT,
FOREIGN KEY (id*вьючного*животного) REFERENCES Вьючные_животные(id)
);

CREATE TABLE Ослы (
id INT PRIMARY KEY,
окрас VARCHAR(50) NOT NULL,
id*вьючного*животного INT,
FOREIGN KEY (id*вьючного*животного) REFERENCES Вьючные_животные(id)
);

SHOW TABLES;

exit;
```

---

## Заполнить низкоуровневые таблицы именами(животных), командами которые они выполняют и датами рождения

```
-- Добавление данных в таблицу Животные
INSERT INTO Животные (id, тип) VALUES
(1, 'Домашние животные'),
(2, 'Домашние животные'),
(3, 'Домашние животные'),
(4, 'Вьючные животные'),
(5, 'Вьючные животные'),
(6, 'Вьючные животные');

-- Добавление данных в таблицу Домашние*животные
INSERT INTO Домашние*животные (id, имя, возраст, id_животного) VALUES
(1, 'Рекс', 3, 1),
(2, 'Мурзик', 5, 2),
(3, 'Чарли', 1, 1),
(4, 'Барсик', 2, 2),
(5, 'Черчиль', 1, 3),
(6, 'Фред', 2, 3);

-- Добавление данных в таблицу Вьючные*животные
INSERT INTO Вьючные*животные (id, имя, возраст, id_животного) VALUES
(1, 'Лошадь 1', 5, 4),
(2, 'Лошадь 2', 6, 4),
(3, 'Верблюд 1', 4, 5),
(4, 'Осел 1', 3, 6);

-- Добавление данных в таблицу Собаки
INSERT INTO Собаки (id, порода, id*домашнего*животного) VALUES
(1, 'Немецкая овчарка', 1),
(2, 'Йоркширский терьер', 2),
(3, 'Джек Рассел терьер', 3);

-- Добавление данных в таблицу Кошки
INSERT INTO Кошки (id, порода, id*домашнего*животного) VALUES
(1, 'Персидская', 4),
(2, 'Сиамская', 5);

-- Добавление данных в таблицу Хомяки
INSERT INTO Хомяки (id, вид, id*домашнего*животного) VALUES
(1, 'Золотой', 6),
(2, 'Сирийский', 6);

-- Добавление данных в таблицу Лошади
INSERT INTO Лошади (id, окрас, id*вьючного*животного) VALUES
(1, 'Коричневый', 1),
(2, 'Белый', 2);

-- Добавление данных в таблицу Верблюды
INSERT INTO Верблюды (id, окрас, id*вьючного*животного) VALUES
(1, 'Серый', 3);

-- Добавление данных в таблицу Ослы
INSERT INTO Ослы(id, окрас, id*вьючного*животного) VALUES
(1, 'Серый', 4);
```

---

## Удалив из таблицы верблюдов, т.к. верблюдов решили перевезти в другой питомник на зимовку. Объединить таблицы лошади, и ослы в одну таблицу.

```
DELETE FROM Верблюды;

CREATE TABLE Лошади*и*ослы (
id INT PRIMARY KEY,
тип VARCHAR(50) NOT NULL,
имя VARCHAR(50) NOT NULL,
возраст INT,
окрас VARCHAR(50) NOT NULL
);

INSERT INTO Лошади*и*ослы (id, тип, имя, возраст, окрас)
SELECT id, 'Вьючные животные',имя, возраст, окрас FROM Лошади
UNION ALL
SELECT id, 'Вьючные животные', имя, возраст, окрас FROM Ослы;

    SELECT * FROM Лошади_и_ослы;
```

---

Создать новую таблицу “молодые животные” в которую попадут все
животные старше 1 года, но младше 3 лет и в отдельном столбце с точностью
до месяца подсчитать возраст животных в новой таблице

CREATE TABLE Молодые*животные (
id INT PRIMARY KEY,
тип VARCHAR(50) NOT NULL,
имя VARCHAR(50) NOT NULL,
возраст*в_месяцах INT NOT NULL
);

INSERT INTO Молодые*животные (id, тип, имя, возраст*в*месяцах)
SELECT id, тип, имя, TIMESTAMPDIFF(MONTH, дата*рождения, NOW()) AS возраст*в*месяцах
FROM Животные
WHERE TIMESTAMPDIFF(YEAR, дата_рождения, NOW()) BETWEEN 1 AND 3;

SELECT \* FROM Молодые_животные;

---

## Объединить все таблицы в одну, при этом сохраняя поля, указывающие на прошлую принадлежность к старым таблицам.

```
CREATE TABLE Общая*таблица (
id INT PRIMARY KEY,
тип VARCHAR(50) NOT NULL,
имя VARCHAR(50) NOT NULL,
возраст INT,
окрас VARCHAR(50),
порода VARCHAR(50),
вид VARCHAR(50),
id*животного INT,
id*домашнего*животного INT,
id*вьючного*животного INT
);

ALTER TABLE Общая*таблица ADD COLUMN id*животного INT;
ALTER TABLE Общая*таблица ADD COLUMN id*домашнего*животного INT;
ALTER TABLE Общая*таблица ADD COLUMN id*вьючного*животного INT;

INSERT INTO Общая*таблица (id, тип, имя, возраст, окрас, порода, вид, id*животного, id*домашнего*животного, id*вьючного*животного)
SELECT id, 'Животные', имя, возраст, NULL, NULL, NULL, id, NULL, NULL FROM Животные
UNION ALL
SELECT id, 'Домашние животные', имя, возраст, NULL, NULL, NULL, NULL, id, NULL FROM Домашние*животные
UNION ALL
SELECT id, 'Вьючные животные', имя, возраст, окрас, порода, вид, NULL, NULL, id FROM Вьючные*животные;

SELECT \* FROM Общая_таблица;
```

---

## Создать класс с Инкапсуляцией методов и наследованием по диаграмме.

```
class Animal:
def **init**(self, name, age):
self.\_name = name
self.\_age = age

    def get_name(self):
        return self._name

    def set_name(self, name):
        self._name = name

    def get_age(self):
        return self._age

    def set_age(self, age):
        self._age = age

class Dog(Animal):
def **init**(self, name, age, breed):
super().**init**(name, age)
self.\_breed = breed

    def get_breed(self):
        return self._breed

    def set_breed(self, breed):
        self._breed = breed

class Cat(Animal):
def **init**(self, name, age, color):
super().**init**(name, age)
self.\_color = color def get_color(self):
return self.\_color

    def set_color(self, color):
        self._color = color
```

---

## Написать программу, имитирующую работу реестра домашних животных. В программе должен быть реализован следующий функционал:

- Завести новое животное
- определять животное в правильный класс
- увидеть список команд, которое выполняет животное
- обучить животное новым командам
- Реализовать навигацию по меню

```
class Animal:
def **init**(self, name, age):
self.name = name
self.age = age
self.commands = ["Есть", "Пить", "Спать"]

    def add_command(self, command):
        self.commands.append(command)

    def execute_command(self, command):
        if command in self.commands:
            print(f"{self.name} выполняет команду: {command}")
        else:
            print(f"{self.name} не знает команду: {command}")

class Dog(Animal):
def **init**(self, name, age, breed):
super().**init**(name, age)
self.breed = breed
self.commands.extend(["Голос", "Сидеть", "Лежать"])

    def bark(self):
        print(f"{self.name}: Гав-гав!")

class Cat(Animal):
def **init**(self, name, age, color):
super().**init**(name, age)
self.color = color
self.commands.extend(["Мяукать", "Ловить мышей", "Драть мебель"])

    def meow(self):
        print("Мяу!")

# Функция для вывода списка команд

def print_commands(animal):
print(f"Список команд для {animal.name}:")
for command in animal.commands:
print(f"\t- {command}")

# Функция для обучения новым командам

def teach_command(animal):
new_command = input(f"Введите новую команду для {animal.name}: ")
animal.add_command(new_command)
print(f"Команда '{new_command}' добавлена для {animal.name}.")

# Функция для навигации по меню

def menu(registry):
while True:
print("Меню:")
print("1. Добавить новое животное")
print("2. Вывести список всех зарегистрированных животных")
print("3. Вывести список команд для животного")
print("4. Обучить животное новой команде")
print("5. Выход")
choice = input("Выберите опцию (1-5): ")
if choice == "1":
animal_type = input("Введите тип животного (собака/кошка): ")
name = input("Введите имя животного: ")
age = int(input("Введите возрастживотного: "))
if animal_type.lower() == "собака":
breed = input("Введите породу собаки: ")
animal = Dog(name, age, breed)
elif animal_type.lower() == "кошка":
color = input("Введите цвет кошки: ")
animal = Cat(name, age, color)
else:
print("Неверный тип животного.")
continue
registry.add_animal(animal)
print(f"{animal.name} успешно добавлен в реестр.")
elif choice == "2":
registry.list_all_animals()
elif choice == "3":
name = input("Введите имя животного: ")
animal = registry.find_animal_by_name(name)
if animal:
print_commands(animal)
elif choice == "4":
name = input("Введите имя животного: ")
animal = registry.find_animal_by_name(name)
if animal:
teach_command(animal)
elif choice == "5":
break
else:
print("Неверный выбор.")
continue

# Класс Registry

class Registry:
def **init**(self):
self.animals = []

    def add_animal(self, animal):
        self.animals.append(animal)

    def find_animal_by_name(self, name):
        for animalin self.animals:
            if animal.name == name:
                return animal
        print(f"{name} не найден в реестре.")

    def list_all_animals(self):
        for animal in self.animals:
            print(f"{animal.name} ({type(animal).__name__}) - Возраст: {animal.age}")
            if isinstance(animal, Dog):
                print(f"\tПорода: {animal.breed}")
            elif isinstance(animal, Cat):
                print(f"\tЦвет: {animal.color}")

# Пример использования

# Создаем экземпляры классов Dog, Cat и Registry

dog1 = Dog("Барсик", 3, "Дворняга")
cat1 = Cat("Мурзик", 5, "Серый")
registry = Registry()

# Добавляем животных в реестр

registry.add_animal(dog1)
registry.add_animal(cat1)

# Запускаем меню

menu(registry)
```

---
