# Подробный конспект о символьных типах данных в C#

#### 1. Основной символьный тип `char`

**Тип `char`** — примитивный тип данных для хранения одного символа Unicode. Занимает 2 байта в памяти (16 бит), что позволяет представлять символы в кодировке UTF‑16.

Является псевдонимом для структуры `System.Char`.

**Объявление и инициализация:**
```csharp
char letter = 'A';
char digit = '5';
char symbol = '@';
char unicodeChar = '\u00A9'; // символ © (Unicode)
```

#### 2. Способы задания значений типа `char`

1. **Символьный литерал** (в одинарных кавычках):
   ```csharp
   char c1 = 'X';
   char c2 = '7';
   char c3 = ' '; // пробел
   ```
2. **Escape‑последовательности:**
   * `\'` — одинарная кавычка;
   * `\\` — обратный слеш;
   * `\n` — новая строка;
   * `\t` — табуляция;
   * `\r` — возврат каретки.
   ```csharp
   char quote = '\'';
   char backslash = '\\';
   char tab = '\t';
   ```
3. **Unicode‑последовательности** (`\uXXXX`, где X — шестнадцатеричная цифра):
   ```csharp
   char copyright = '\u00A9';  // ©
   char euro = '\u20AC';       // €
   char russianLetter = '\u0410'; // А (кириллица)
   ```
4. **Шестнадцатеричный код** (`\x...`):
   ```csharp
   char hexChar = '\x41'; // 'A'
   ```

#### 3. Основные операции с типом `char`

* **Арифметические операции** (символы преобразуются в числовые коды):
  ```csharp
  char a = 'A';
  char nextChar = (char)(a + 1); // 'B'
  int code = 'Z'; // 90 (числовой код символа)
  ```
* **Сравнение символов** (по числовым кодам):
  ```csharp
  bool isEqual = 'A' == 'B';     // false
  bool isLess = 'A' < 'Z';       // true
  bool isGreater = 'a' > 'A';   // true (строчные идут после заглавных)
  ```
* **Приведение типов:**
  ```csharp
  char c = 'A';
  int asInt = c;        // 65
  char fromInt = (char)66; // 'B'
  ```

#### 4. Вспомогательные методы структуры `char`

Структура `System.Char` предоставляет множество статических методов для работы с символами:

* **Проверка категорий символов:**
  * `char.IsLetter(c)` — буква;
  * `char.IsDigit(c)` — цифра;
  * `char.IsWhiteSpace(c)` — пробел или табуляция;
  * `char.IsPunctuation(c)` — знак препинания;
  * `char.IsUpper(c)` / `char.IsLower(c)` — верхний/нижний регистр.
  ```csharp
  Console.WriteLine(char.IsDigit('5'));     // true
  Console.WriteLine(char.IsLetter('A'));   // true
  Console.WriteLine(char.IsWhiteSpace('\t')); // true
  ```
* **Преобразование регистра:**
  * `char.ToUpper(c)`;
  * `char.ToLower(c)`.
  ```csharp
  Console.WriteLine(char.ToUpper('a')); // 'A'
  Console.WriteLine(char.ToLower('Z')); // 'z'
  ```
* **Анализ Unicode‑категорий:**
  * `char.GetUnicodeCategory(c)` — возвращает категорию символа (заглавная буква, цифра и т. д.).

#### 5. Escape‑последовательности

Специальные комбинации для представления непечатаемых или специальных символов:

| Последовательность | Значение |
|----------------|----------|
| `\'` | Одинарная кавычка |
| `"` | Двойная кавычка |
| `\\` | Обратный слеш |
| `\0` | Нулевой символ (null) |
| `\a` | Звуковой сигнал |
| `\b` | Забой (backspace) |
| `\f` | Перевод страницы |
| `\n` | Новая строка |
| `\r` | Возврат каретки |
| `\t` | Горизонтальная табуляция |
| `\v` | Вертикальная табуляция |

#### 6. Unicode и кодировки

C# использует Unicode (UTF‑16) для представления символов, что позволяет работать с:
* латиницей;
* кириллицей;
* иероглифами;
* эмодзи и специальными символами.

**Примеры:**
```csharp
char emoji = '\U0001F600'; // 😀 (расширенный Unicode)
char chinese = '\u4E2D';      // 中 (китайский иероглиф)
```

#### 7. Nullable‑версия (`char?`)

Тип `char?` (или `Nullable<char>`) может принимать значение `null` в дополнение к обычным символам:
```csharp
char? nullableChar = null;
nullableChar = 'X';

if (nullableChar.HasValue)
{
    Console.WriteLine($"Символ: {nullableChar.Value}");
}
else
{
    Console.WriteLine("Значение не задано");
}
```

#### 8. Отличия от строк

* **`char`** — один символ, занимает 2 байта, одинарные кавычки (`'A'`).
* **`string`** — последовательность символов, может быть пустой (`""`), двойные кавычки (`"Hello"`).

**Преобразование между `char` и `string`:**
```csharp
char c = 'X';
string fromChar = c.ToString(); // "X"
string str = "Hello";
char firstChar = str[0];        // 'H'
```

#### 9. Практические примеры использования

**Пример 1: проверка ввода пользователя**
```csharp
Console.Write("Введите один символ: ");
char input = Console.ReadKey().KeyChar;
Console.WriteLine();

if (char.IsDigit(input))
{
    Console.WriteLine("Вы ввели цифру");
}
else if (char.IsLetter(input))
{
    Console.WriteLine("Вы ввели букву");
}
else
{
    Console.WriteLine("Вы ввели специальный символ");
}
```

**Пример 2: обработка строки посимвольно**
```csharp
string text = "Hello123!";
foreach (char c in text)
{
    if (char.IsLetter(c))
    {
        Console.Write($"{c} — буква, ");
    }
    else if (char.IsDigit(c))
    {
        Console.Write($"{c} — цифра, ");
    }
    else
    {
        Console.Write($"{c} — спецсимвол, ");
    }
}
// Вывод: H — буква, e — буква, l — буква, ... 1 — цифра, 2 — цифра, ... ! — спецсимвол
```

**Пример 3: шифр Цезаря (сдвиг букв)**
```csharp
char EncryptChar(char c, int shift)
{
    if (char.IsLetter(c))
    {
        char baseChar = char.IsUpper(c) ? 'A' : 'a';
        int position = c - baseChar;
        int newPosition = (position + shift) % 26;
        return (char)(baseChar + newPosition);
    }
    return c;
}

Console.WriteLine(EncryptChar('A', 3)); // 'D'
Console.WriteLine(EncryptChar('z', 1)); // 'a'
```

---

#### Краткий итог

* `char` — основной тип для хранения одного символа Unicode (2 байта);
* поддерживает литералы, escape‑последовательности и Unicode‑коды;
* позволяет выполнять арифметические операции и сравнения;
* имеет богатый набор вспомогательных методов для анализа и преобразования;
* существует nullable‑версия (`char?`);
* отличается от `string`, но легко преобразуется в неё и обратно;
* полностью поддерживает Unicode, включая эмодзи и символы разных языков.

Хотите, я раскрою какой‑либо раздел подробнее или приведу дополнительные примеры?