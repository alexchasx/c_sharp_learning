# Подробный конспект о типе `enum` в C#


#### 1. Основы `enum`


**`enum`** (перечисление) — пользовательский значимый тип данных, который задаёт набор именованных констант целочисленного типа.

**Ключевые особенности:**
* является типом-значением (`value type`);
* наследуется от базового целочисленного типа (по умолчанию — `int`);
* все члены перечисления имеют уникальные значения;
* неизменяем — нельзя добавлять элементы во время выполнения.

#### 2. Объявление и синтаксис

**Базовый синтаксис:**
```csharp
enum ИмяПеречисления
{
    Константа1,
    Константа2,
    Константа3
}
```

**Пример:**
```csharp
enum DaysOfWeek
{
    Monday,
    Tuesday,
    Wednesday,
    Thursday,
    Friday,
    Saturday,
    Sunday
}
```

#### 3. Базовые типы перечисления

По умолчанию базовый тип — `int`. Можно явно указать другой целочисленный тип:

```csharp
// По умолчанию int
enum Colors { Red, Green, Blue }

// Явно указан byte
enum SmallEnum : byte { A, B, C }

// Другие возможные типы
enum LongEnum : long { Big1, Big2 }
enum ShortEnum : short { Small1, Small2 }
```

#### 4. Задание значений элементам

**Автоматическое присвоение:** значения начинаются с 0 и увеличиваются на 1:
```csharp
enum Status
{
    Pending,    // 0
    Approved,   // 1
    Rejected    // 2
}
```

**Явное присвоение значений:**
```csharp
enum HttpStatus
{
    OK = 200,
    NotFound = 404,
    ServerError = 500
}
```

**Смешанное присвоение:**
```csharp
enum Permissions
{
    None = 0,
    Read = 1,
    Write = 2,
    Execute = 4,  // можно пропускать значения
    Full = 7      // или задавать свои
}
```

#### 5. Работа с перечислениями

**Объявление и использование переменных:**
```csharp
DaysOfWeek today = DaysOfWeek.Monday;
Console.WriteLine(today);           // Monday
Console.WriteLine((int)today);    // 0
```

**Преобразование типов:**
```csharp
// int → enum
DaysOfWeek day = (DaysOfWeek)2; // Wednesday

// string → enum
string dayName = "Friday";
DaysOfWeek parsedDay = (DaysOfWeek)Enum.Parse(typeof(DaysOfWeek), dayName);

// Безопасный парсинг
if (Enum.TryParse<DaysOfWeek>("Monday", out DaysOfWeek result))
{
    Console.WriteLine($"Успешно распаршено: {result}");
}
```

#### 6. Флаги (`[Flags]` атрибут)

Для использования перечисления как набора битовых флагов применяется атрибут `[Flags]`.

**Объявление:**
```csharp
[Flags]
enum FilePermissions
{
    None = 0,
    Read = 1,      // 2^0
    Write = 2,     // 2^1
    Execute = 4,   // 2^2
    Delete = 8     // 2^3
}
```

**Использование:**
```csharp
FilePermissions userPermissions = FilePermissions.Read | FilePermissions.Write;

// Проверка наличия флага
if (userPermissions.HasFlag(FilePermissions.Read))
{
    Console.WriteLine("Есть право на чтение");
}

// Добавление флага
userPermissions |= FilePermissions.Execute;

// Удаление флага
userPermissions &= ~FilePermissions.Write;
```

#### 7. Вспомогательные методы класса `Enum`

**Основные статические методы:**

* `GetNames()` — возвращает массив имён элементов:
  ```csharp
  string[] names = Enum.GetNames(typeof(DaysOfWeek));
  ```
* `GetValues()` — возвращает массив значений элементов:
  ```csharp
  Array values = Enum.GetValues(typeof(DaysOfWeek));
  ```
* `IsDefined()` — проверяет, существует ли значение в перечислении:
  ```csharp
  bool exists = Enum.IsDefined(typeof(DaysOfWeek), "Monday");
  ```
* `Parse()` — преобразует строку в значение перечисления:
  ```csharp
  DaysOfWeek day = (DaysOfWeek)Enum.Parse(typeof(DaysOfWeek), "Tuesday");
  ```
* `TryParse()` — безопасный парсинг (не выбрасывает исключение):
  ```csharp
  if (Enum.TryParse<DaysOfWeek>("Wednesday", out DaysOfWeek wed))
  {
      Console.WriteLine(wed);
  }
  ```

#### 8. Практические примеры

**Пример 1: простое перечисление для статусов заказа**
```csharp
enum OrderStatus
{
    Created,
    Processing,
    Shipped,
    Delivered,
    Cancelled
}

OrderStatus currentStatus = OrderStatus.Processing;
Console.WriteLine($"Текущий статус: {currentStatus}");
```

**Пример 2: перечисление с числовыми кодами**
```csharp
enum ErrorCode
{
    Success = 0,
    FileNotFound = 100,
    AccessDenied = 101,
    OutOfMemory = 200
}

ErrorCode error = ErrorCode.FileNotFound;
Console.WriteLine($"Код ошибки: {(int)error}"); // 100
```

**Пример 3: флаги для прав доступа**
```csharp
[Flags]
enum UserRights
{
    None = 0,
    View = 1,
    Edit = 2,
    Delete = 4,
    Admin = View | Edit | Delete
}

UserRights user = UserRights.View | UserRights.Edit;
Console.WriteLine($"Права пользователя: {user}"); // View, Edit
```

#### 9. Особенности и рекомендации

**Рекомендации по именованию:**
* используйте единственное число для обычных перечислений (`DaysOfWeek`);
* используйте множественное число для флагов (`UserRights`).

**Лучшие практики:**
* указывайте явные значения для элементов, если они должны быть стабильными;
* используйте `[Flags]` только когда нужно комбинировать значения;
* добавляйте значение `None = 0` для флагов;
* избегайте «дырок» в значениях без необходимости;
* документируйте перечисления с нестандартными значениями.

**Нюансы:**
* значения перечисления не обязательно должны быть уникальными (хотя это не рекомендуется);
* можно использовать отрицательные значения;
* перечисления типобезопасны — компилятор не позволит присвоить значение другого перечисления;
* перечисления можно использовать в `switch`‑конструкциях.

#### 10. Ограничения

* нельзя наследовать от `enum`;
* нельзя создавать экземпляры перечисления с помощью `new`;
* базовый тип должен быть целочисленным (`byte`, `sbyte`, `short`, `ushort`, `int`, `uint`, `long`, `ulong`);
* нельзя определять методы внутри перечисления (но можно создавать методы расширения).

#### 11. Методы расширения для перечислений

Можно добавить функциональность с помощью методов расширения:
```csharp
public static class EnumExtensions
{
    public static string ToFriendlyString(this DaysOfWeek day)
    {
        switch (day)
        {
            case DaysOfWeek.Monday: return "Понедельник";
            case DaysOfWeek.Tuesday: return "Вторник";
            // ... остальные дни
            default: return day.ToString();
        }
    }
}

// Использование
DaysOfWeek today = DaysOfWeek.Monday;
Console.WriteLine(today.ToFriendlyString()); // "Понедельник"
```

---

#### Краткий итог

* `enum` — значимый тип для именованных констант;
* по умолчанию базируется на `int`, можно указать другой целочисленный тип;
* поддерживает явное и автоматическое присвоение значений;
* с атрибутом `[Flags]` позволяет комбинировать значения как битовые флаги;
* имеет богатый набор вспомогательных методов в классе `Enum`;
* типобезопасен и поддерживается компилятором;
* широко используется для представления наборов связанных констант.
