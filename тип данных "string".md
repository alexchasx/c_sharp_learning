# Подробный конспект о типе `string` в C#

#### 1. Основы типа `string`

**Тип `string`** — неизменяемый (immutable) ссылочный тип для хранения последовательности символов Unicode (UTF‑16). Является псевдонимом для класса `System.String`.

**Ключевые особенности:**
* **Неизменяемость:** после создания строки её содержимое нельзя изменить — любые операции создают новую строку.
* **Ссылочный тип:** хранится в управляемой куче (heap).
* **Автоматическое управление памятью:** сборщик мусора (GC) освобождает память.
* **Поддержка Unicode:** может содержать любые символы Unicode, включая эмодзи и символы разных языков.

**Объявление и инициализация:**
```csharp
string empty = "";
string message = "Hello, World!";
string unicode = "Привет, мир! 🌍";
string nullString = null;
```

#### 2. Способы создания строк

1. **Строковые литералы** (в двойных кавычках):
   ```csharp
   string s1 = "Обычная строка";
   string s2 = "С \"кавычками\" внутри";
   ```
2. **Дословные литералы** (`@"..."`) — игнорируют escape‑последовательности:
   ```csharp
   string path = @"C:\Users\Documents\file.txt"; // без экранирования слешей
   string multiline = @"Многострочная
   строка без \n";
   ```
3. **Конструктор `string`:**
   * из массива символов:
     ```csharp
     char[] chars = { 'H', 'e', 'l', 'l', 'o' };
     string fromArray = new string(chars);
     ```
   * повторение символа:
     ```csharp
     string repeated = new string('*', 5); // "*****"
     ```
4. **Статические методы:**
   * `string.Empty` — пустая строка;
   * `string.Concat()` — конкатенация;
   * `string.Join()` — объединение с разделителем.

#### 3. Основные операции со строками

* **Конкатенация (`+`):**
  ```csharp
  string first = "Hello";
  string second = "World";
  string result = first + " " + second; // "Hello World"
  ```
* **Интерполяция строк** (`$"..."`):
  ```csharp
  string name = "Alice";
  int age = 25;
  string info = $"Name: {name}, Age: {age}"; // "Name: Alice, Age: 25"
  ```
* **Форматирование** (`string.Format`):
  ```csharp
  string formatted = string.Format("Name: {0}, Age: {1}", name, age);
  ```
* **Сравнение строк:**
  * `==`/`!=` — сравнение значений;
  * `string.Equals()` — с указанием правил сравнения;
  * `CompareTo()` — лексикографическое сравнение.
  ```csharp
  bool areEqual = "hello" == "Hello"; // false (с учётом регистра)
  bool ignoreCase = string.Equals("hello", "Hello", StringComparison.OrdinalIgnoreCase); // true
  ```

#### 4. Методы класса `string`

**Поиск и анализ:**
* `Contains()` — содержит ли строка подстроку;
* `StartsWith()`/`EndsWith()` — начинается/заканчивается ли строка на подстроку;
* `IndexOf()`/`LastIndexOf()` — позиция первого/последнего вхождения;
* `Substring()` — извлечение подстроки.

**Преобразование регистра:**
* `ToUpper()`;
* `ToLower()`.

**Модификация (создание новых строк):**
* `Trim()`/`TrimStart()`/`TrimEnd()` — удаление пробелов;
* `Replace()` — замена подстрок;
* `Insert()` — вставка в позицию;
* `Remove()` — удаление части строки.

**Разделение и объединение:**
* `Split()` — разбиение на массив строк;
* `Join()` — объединение массива строк.

**Проверка:**
* `IsNullOrEmpty()` — `null` или пустая;
* `IsNullOrWhiteSpace()` — `null`, пустая или только пробелы.

#### 5. Escape‑последовательности в строках

Специальные символы внутри строк:
* `\"` — двойная кавычка;
* `\\` — обратный слеш;
* `\n` — новая строка;
* `\t` — табуляция;
* `\r` — возврат каретки;
* `\uXXXX` — Unicode‑символ.

**Пример:**
```csharp
string escaped = "Текст с \"кавычками\" и новой строкой:\nСледующая строка";
```

#### 6. Дословные строки (`@`‑литералы)

Позволяют записывать строки без экранирования специальных символов:
```csharp
string path = @"C:\Windows\System32"; // вместо "C:\\Windows\\System32"
string multiline = @"Первая строка
Вторая строка
Третья строка";
```
**Особенности:**
* двойные кавычки внутри строки удваиваются: `string quote = @"Он сказал: ""Привет!"""`;
* сохраняют все пробелы и переносы строк.

#### 7. Интерполяция строк (`$`‑строки)

Упрощает форматирование за счёт встраивания выражений в строку:
```csharp
int x = 10, y = 20;
string result = $"Сумма: {x + y}, произведение: {x * y}";
// "Сумма: 30, произведение: 200"
```
**Возможности:**
* форматирование внутри фигурных скобок: `{value:format}`;
* вызов методов: `{DateTime.Now:yyyy-MM-dd}`.

#### 8. StringBuilder для эффективной модификации

Класс `System.Text.StringBuilder` предназначен для многократного изменения строк без создания промежуточных объектов.

**Когда использовать:**
* при множественных операциях конкатенации в цикле;
* при построении больших строк динамически.

**Пример:**
```csharp
StringBuilder sb = new StringBuilder();
for (int i = 0; i < 1000; i++)
{
    sb.AppendLine($"Строка {i}");
}
string result = sb.ToString();
```
**Основные методы:**
* `Append()`/`AppendLine()` — добавление;
* `Insert()`/`Remove()` — вставка/удаление;
* `Replace()` — замена;
* `ToString()` — получение итоговой строки.

#### 9. Сравнение строк

**Важные нюансы:**
* оператор `==` сравнивает значения строк (не ссылки);
* по умолчанию сравнение учитывает регистр;
* для сравнения без учёта регистра используйте `StringComparison`:
  ```csharp
  string.Equals("hello", "HELLO", StringComparison.OrdinalIgnoreCase);
  ```
* лексикографическое сравнение: `str1.CompareTo(str2)`.

#### 10. Null и пустые строки

**Типы «пустых» строк:**
* `null` — ссылка не инициализирована;
* `""` (`string.Empty`) — пустая строка (длина 0);
* строка из пробелов — не считается пустой.

**Методы проверки:**
* `string.IsNullOrEmpty(str)` — `true` для `null` и `""`;
* `string.IsNullOrWhiteSpace(str)` — `true` для `null`, `""` и `"   "`.

#### 11. Практические примеры

**Пример 1: обработка пользовательского ввода**
```csharp
Console.Write("Введите имя: ");
string input = Console.ReadLine();

if (string.IsNullOrWhiteSpace(input))
{
    Console.WriteLine("Имя не может быть пустым!");
}
else
{
    string formattedName = input.Trim().ToUpper();
    Console.WriteLine($"Привет, {formattedName}!");
}
```

**Пример 2: разбор CSV‑строки**
```csharp
string csv = "John,Doe,25,Engineer";
string[] fields = csv.Split(',');

Console.WriteLine($"Имя: {fields[0]}");
Console.WriteLine($"Фамилия: {fields[1]}");
Console.WriteLine($"Возраст: {fields[2]}");
```

**Пример 3: построение SQL‑запроса**
```csharp
List<string> conditions = new List<string> { "age > 18", "status = 'active'" };
string whereClause = string.Join(" AND ", conditions);
string query = $"SELECT * FROM users WHERE {whereClause}";
// Результат: "SELECT * FROM users WHERE age > 18 AND status = 'active'"
```

---

#### Краткий итог

* `string` — неизменяемый ссылочный тип для работы с текстом;
* поддерживает Unicode и все языки мира;
* имеет богатый набор методов для поиска, анализа и модификации;
* интерполяция (`$`) и дословные строки (`@`) упрощают работу с текстом;
* для многократной модификации используйте `StringBuilder`;
*