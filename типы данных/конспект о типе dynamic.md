# Подробный конспект о типе `dynamic` в C#

#### 1. Основы `dynamic`

**`dynamic`** — специальный тип данных в C#, который **откладывает проверку типа до времени выполнения** (runtime), а не на этапе компиляции.

**Ключевые особенности:**
* проверка типов выполняется во время выполнения (late binding);
* обходит статическую проверку типов компилятора;
* позволяет работать с объектами, тип которых неизвестен на этапе компиляции;
* реализован через `System.Dynamic.DynamicObject`;
* связан с технологией **DLR** (Dynamic Language Runtime).

#### 2. Объявление и инициализация

**Синтаксис объявления:**
```csharp
dynamic variableName = value;
```

**Примеры:**
```csharp
dynamic name = "John";     // строка
dynamic number = 42;      // число
dynamic obj = new { Prop = "Value" }; // анонимный тип
dynamic list = new List<int> {1, 2, 3}; // коллекция
```

#### 3. Отличия от `var`

| `dynamic` | `var` |
|---------|---------|
| Проверка типа во время выполнения | Проверка типа на этапе компиляции |
| Динамическая типизация | Статическая типизация |
| Ошибки типов выявляются во время выполнения | Ошибки типов выявляются при компиляции |
| Пониженная производительность | Высокая производительность |
| Тип может меняться во время выполнения | Тип фиксируется при компиляции |

**Пример сравнения:**
```csharp
var staticVar = "text";    // тип string известен компилятору
dynamic dynamicVar = "text"; // тип определяется во время выполнения

staticVar = 10;        // ошибка компиляции
dynamicVar = 10;       // корректно, тип теперь int
```

#### 4. Операции с `dynamic`

**Все операции с `dynamic`-переменными выполняются во время выполнения:**

* **Арифметические операции:**
  ```csharp
  dynamic a = 5;
  dynamic b = 3;
  dynamic result = a + b; // 8 (во время выполнения)
  ```
* **Строковые операции:**
  ```csharp
  dynamic str1 = "Hello";
  dynamic str2 = "World";
  dynamic greeting = str1 + " " + str2; // "Hello World"
  ```
* **Вызов методов:**
  ```csharp
  dynamic obj = GetDynamicObject();
  var result = obj.SomeMethod(); // вызов во время выполнения
  ```
* **Доступ к свойствам:**
  ```csharp
  dynamic person = new { Name = "Alice", Age = 25 };
  Console.WriteLine(person.Name); // "Alice"
  Console.WriteLine(person.Age);  // 25
  ```

#### 5. Работа с объектами неизвестного типа

**Пример с COM‑объектами (Excel):**
```csharp
dynamic excel = Activator.CreateInstance(Type.GetTypeFromProgID("Excel.Application"));
excel.Visible = true;
excel.Workbooks.Add();
```

**Пример с JSON‑объектами:**
```csharp
string json = "{\"Name\": \"John\", \"Age\": 30}";
dynamic parsed = JsonConvert.DeserializeObject(json);
Console.WriteLine(parsed.Name); // "John"
Console.WriteLine(parsed.Age);  // 30
```

#### 6. Обработка ошибок

**Ошибки выявляются только во время выполнения:**
```csharp
dynamic obj = "I'm a string";

// Ошибки компиляции нет, но будет исключение во время выполнения:
try
{
    obj.NonExistentMethod(); // RuntimeBinderException
    int x = obj.NonExistentProperty; // RuntimeBinderException
}
catch (RuntimeBinderException ex)
{
    Console.WriteLine($"Ошибка: {ex.Message}");
}
```

**Распространённые исключения:**
* `RuntimeBinderException` — операция не поддерживается типом;
* `NullReferenceException` — попытка доступа к члену `null`-объекта;
* `MissingMemberException` — отсутствует запрашиваемый член.

#### 7. Взаимодействие с статическими типами

**Неявное преобразование:**
```csharp
dynamic d = 42;
int i = d; // корректно, d содержит int
string s = d.ToString(); // корректно
```

**Явное преобразование:**
```csharp
dynamic unknown = GetUnknownValue();
if (unknown is string)
{
    string str = (string)unknown;
    Console.WriteLine($"Строка: {str}");
}
else if (unknown is int)
{
    int num = (int)unknown;
    Console.WriteLine($"Число: {num}");
}
```

#### 8. Использование в методах

**Параметры типа `dynamic`:**
```csharp
void ProcessDynamic(dynamic data)
{
    Console.WriteLine($"Тип: {data.GetType().Name}");
    Console.WriteLine($"Значение: {data}");
}

ProcessDynamic("text"); // Тип: String, Значение: text
ProcessDynamic(123);   // Тип: Int32, Значение: 123
```

**Возвращаемое значение:**
```csharp
dynamic CreateDynamicObject()
{
    return new { Id = 1, Name = "Test" };
}

dynamic obj = CreateDynamicObject();
Console.WriteLine(obj.Name); // "Test"
```

#### 9. Динамические объекты (`DynamicObject`)

Можно создавать собственные динамические объекты, наследуя от `DynamicObject`:

```csharp
class MyDynamicObject : DynamicObject
{
    private Dictionary<string, object> _properties = new();

    public override bool TryGetMember(GetMemberBinder binder, out object result)
    {
        return _properties.TryGetValue(binder.Name, out result);
    }

    public override bool TrySetMember(SetMemberBinder binder, object value)
    {
        _properties[binder.Name] = value;
        return true;
    }
}

// Использование:
dynamic obj = new MyDynamicObject();
obj.Name = "Custom";
obj.Value = 100;
Console.WriteLine(obj.Name);  // "Custom"
Console.WriteLine(obj.Value); // 100
```

#### 10. Практические примеры использования

**Пример 1: работа с внешними API**
```csharp
// Предположим, API возвращает JSON с неизвестной структурой
string apiResponse = GetApiResponse();
dynamic data = JsonConvert.DeserializeObject(apiResponse);

// Безопасный доступ к данным
if (data?.user?.profile?.name != null)
{
    Console.WriteLine($"Пользователь: {data.user.profile.name}");
}
```

**Пример 2: сценарии с плагинами**
```csharp
dynamic plugin = LoadPlugin();
if (plugin.CanExecute())
{
    plugin.Execute();
}
```

**Пример 3: динамическое построение объектов**
```csharp
dynamic config = new ExpandoObject();
config.Database = "MyDB";
config.Port = 5432;
config.UseSSL = true;

// Можно добавлять свойства динамически
config.AdditionalSettings = new ExpandoObject();
config.AdditionalSettings.Timeout = 30;
```

#### 11. Производительность

**Нюансы производительности:**
* операции с `dynamic` медленнее из‑за проверки типов во время выполнения;
* кэширование binder‑ов снижает накладные расходы при повторных вызовах;
* не подходит для высокопроизводительных участков кода.

**Сравнение производительности:**
```csharp
// Статический вызов (быстрый)
string str = "test";
int length1 = str.Length;

// Динамический вызов (медленнее)
dynamic dStr = "test";
int length2 = dStr.Length; // требуется разрешение во время выполнения
```

#### 12. Рекомендации по использованию

**Использовать `dynamic`, когда:**
* работаете с COM‑объектами (Office, Excel и т. д.);
* взаимодействуете с динамическими языками (.NET DLR);
* обрабатываете JSON/XML с неизвестной схемой;
* создаёте сценарии плагинов;
* используете отражение (reflection) для упрощения кода;
* разрабатываете DSL (предметно‑ориентированные языки).

**Избегать `dynamic`, когда:**
* тип известен на этапе компиляции;
* критична производительность;
* код должен быть максимально типобезопасным;
* проект имеет строгие требования к стабильности.

---

#### Краткий итог

* `dynamic` — тип с отложенной проверкой типов до времени выполнения;
* полезен для работы с динамическими данными и внешними системами;
* снижает типобезопасность, но увеличивает гибкость;
* имеет накладные расходы на производительность;
* требует аккуратного использования и обработки исключений;
* особенно полезен при интеграции с COM, JSON, XML и динамическими языками;
* альтернатива — отражение (`reflection`), но `dynamic` проще в использовании.
