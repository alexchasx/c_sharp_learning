# Конспект базового синтаксиса языка программирования C#


#### 1. Структура программы

Базовая структура программы на C#:

* **Пространства имён** (`namespace`) — организуют код в логические блоки.
* **Классы** (`class`) — основные строительные блоки программы.
* **Методы** — функции внутри класса. Главный метод — `Main`, точка входа в программу.

Пример:
```csharp
using System;

namespace MyApp
{
    class Program
    {
        static void Main(string[] args)
        {
            Console.WriteLine("Hello, World!");
        }
    }
}
```

#### 2. Комментарии

Используются для пояснения кода и игнорируются компилятором:
* Однострочный: `// комментарий`.
* Многострочный: `/* комментарий */`.

#### 3. Идентификаторы и ключевые слова

* **Идентификаторы** — имена переменных, классов, методов и т. д. Должны начинаться с буквы или подчёркивания, могут содержать буквы, цифры и подчёркивания. Чувствительны к регистру.
* **Ключевые слова** — зарезервированные слова с особым значением (например, `int`, `string`, `if`, `for`).

#### 4. Переменные и типы данных

**Основные типы данных:**
* `int` — целые числа.
* `double` — числа с плавающей точкой двойной точности.
* `char` — одиночный символ.
* `string` — строка символов.
* `bool` — логический тип (`true`/`false`).
* `decimal` — десятичные числа высокой точности (для финансовых расчётов).

**Объявление и инициализация переменных:**
```csharp
int age = 25;
string name = "Alice";
bool isActive = true;
```

#### 5. Константы

Значения, которые не могут быть изменены после инициализации:
```csharp
const double PI = 3.14159;
```

#### 6. Операторы

* **Арифметические:** `+`, `-`, `*`, `/`, `%`.
* **Сравнения:** `==`, `!=`, `>`, `<`, `>=`, `<=`.
* **Логические:** `&&` (И), `||` (ИЛИ), `!` (НЕ).
* **Присваивания:** `=`, `+=`, `-=`, `*=`, `/=`.

#### 7. Управляющие конструкции

**Условные операторы:**
* `if-else`:
```csharp
if (age >= 18)
{
    Console.WriteLine("Взрослый");
}
else
{
    Console.WriteLine("Ребёнок");
}
```
* `switch`:
```csharp
switch (dayOfWeek)
{
    case "Monday":
        Console.WriteLine("Понедельник");
        break;
    case "Tuesday":
        Console.WriteLine("Вторник");
        break;
    default:
        Console.WriteLine("Другой день");
        break;
}
```

**Циклы:**
* `for`:
```csharp
for (int i = 0; i < 5; i++)
{
    Console.WriteLine(i);
}
```
* `while`:
```csharp
int i = 0;
while (i < 5)
{
    Console.WriteLine(i);
    i++;
}
```
* `do-while`:
```csharp
int j = 0;
do
{
    Console.WriteLine(j);
    j++;
} while (j < 5);
```
* `foreach`:
```csharp
string[] names = { "Alice", "Bob", "Charlie" };
foreach (string name in names)
{
    Console.WriteLine(name);
}
```

#### 8. Массивы

Коллекции элементов одного типа:
```csharp
int[] numbers = { 1, 2, 3, 4, 5 };
string[] words = new string[3];
words[0] = "Hello";
```

#### 9. Методы

Блоки кода с именем, которые можно вызывать многократно:
```csharp
static int Add(int a, int b)
{
    return a + b;
}

// Вызов метода
int result = Add(5, 3); // result = 8
```

#### 10. Ввод и вывод

* Вывод: `Console.WriteLine()` или `Console.Write()`.
* Ввод: `Console.ReadLine()`.

Пример:
```csharp
Console.Write("Введите имя: ");
string userName = Console.ReadLine();
Console.WriteLine($"Привет, {userName}!");
```

#### 11. Обработка исключений

Конструкция `try-catch` для обработки ошибок:
```csharp
try
{
    int number = int.Parse(Console.ReadLine());
}
catch (FormatException)
{
    Console.WriteLine("Неверный формат числа!");
}
finally
{
    Console.WriteLine("Блок finally выполняется всегда.");
}
```