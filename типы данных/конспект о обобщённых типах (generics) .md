# Подробный конспект об обобщённых типах (generics) в языке C#

#### 1. Основы обобщённых типов

**Обобщения (generics)** — механизм C#, позволяющий создавать типы и методы, работающие с различными типами данных без потери типобезопасности.

**Ключевые особенности:**
* обеспечивают типобезопасность на этапе компиляции;
* устраняют необходимость приведения типов (`cast`);
* повышают производительность за счёт отсутствия упаковки/распаковки (`boxing/unboxing`);
* поддерживают ограничения на типы (`constraints`);
* позволяют создавать повторно используемый код.

#### 2. Объявление обобщённых типов

**Синтаксис:**
```csharp
class ИмяКласса<T> { /* ... */ }
struct ИмяСтруктуры<T1, T2> { /* ... */ }
interface ИмяИнтерфейса<T> { /* ... */ }
delegate TResult ИмяДелегата<T, TResult>(T arg);
```

**Примеры:**
```csharp
// Обобщённый класс
class Stack<T>
{
    private T[] items;
    private int count;
}

// Обобщённая структура
struct Pair<T1, T2>
{
    public T1 First;
    public T2 Second;
}

// Обобщённый метод
T GetDefault<T>() => default(T);
```

#### 3. Использование обобщённых типов

**Создание экземпляров:**
```csharp
Stack<int> intStack = new Stack<int>();
Stack<string> stringStack = new Stack<string>();
Pair<int, string> pair = new Pair<int, string>();
```

**Передача типов-параметров:**
* можно использовать любые типы: примитивные, классы, структуры, интерфейсы, делегаты;
* типы-параметры указываются в угловых скобках `<>`.

#### 4. Обобщённые методы

**Объявление:**
```csharp
public T GetFirstElement<T>(List<T> list)
{
    return list.Count > 0 ? list[0] : default(T);
}
```

**Вызов:**
```csharp
var numbers = new List<int> {1, 2, 3};
int firstNumber = GetFirstElement(numbers); // T выводится автоматически

var names = new List<string> {"Alice", "Bob"};
string firstName = GetFirstElement(names);
```

**Явное указание типа:**
```csharp
int result = GetFirstElement<int>(intList);
```

#### 5. Ограничения на параметры типа (constraints)

Ограничения задаются с помощью `where`.

**Основные виды ограничений:**

1. **Базовый класс:** `where T : SomeClass`
2. **Интерфейс:** `where T : ISomeInterface`
3. **Класс (`class`):** `where T : class` — только ссылочные типы
4. **Структура (`struct`):** `where T : struct` — только значимые типы
5. **Конструктор без параметров:** `where T : new()`
6. **Другой параметр типа:** `where T : U`

**Пример с ограничениями:**
```csharp
class Repository<T> where T : class, new()
{
    public List<T> GetAll()
    {
        T item = new T(); // разрешено благодаря new()
        // ...
        return new List<T>();
    }
}
```

**Несколько ограничений одновременно:**
```csharp
void Process<T>(T item) where T : IComparable<T>, new()
{
    T newItem = new T();
    // ...
}
```

#### 6. Обобщённые коллекции

В .NET Framework есть множество встроенных обобщённых коллекций:

* `List<T>` — динамический массив;
* `Dictionary<TKey, TValue>` — словарь (хэш‑таблица);
* `Queue<T>` — очередь (FIFO);
* `Stack<T>` — стек (LIFO);
* `HashSet<T>` — множество уникальных элементов;
* `LinkedList<T>` — двусвязный список.

**Примеры использования:**
```csharp
List<int> numbers = new List<int> {1, 2, 3, 4, 5};
Dictionary<string, int> ages = new Dictionary<string, int>
{
    {"Alice", 25},
    {"Bob", 30}
};
Queue<string> queue = new Queue<string>();
queue.Enqueue("First");
queue.Enqueue("Second");
```

#### 7. Вариативность обобщений (variance)

**Ковариация (`out`):**
* применяется к возвращаемым типам;
* обозначается ключевым словом `out`;
* позволяет использовать более конкретный тип.

**Контравариация (`in`):**
* применяется к параметрам;
* обозначается ключевым словом `in`;
* позволяет использовать более общий тип.

**Пример:**
```csharp
interface ICovariant<out T> { }
interface IContravariant<in T> { }

ICovariant<object> obj = new MyCovariant<string>(); // ковариация
IContravariant<string> str = new MyContravariant<object>(); // контравариация
```

#### 8. Статические члены в обобщённых классах

**Особенности:**
* статические поля и методы специфичны для каждой комбинации параметров типа;
* каждый `MyClass<int>` и `MyClass<string>` имеют свои статические члены.

**Пример:**
```csharp
class Counter<T>
{
    public static int Count;

    public Counter()
    {
        Count++;
    }
}

Counter<int> c1 = new Counter<int>(); // Count = 1
Counter<int> c2 = new Counter<int>(); // Count = 2
Counter<string> s1 = new Counter<string>(); // Count = 1 (для string)
```

#### 9. Обобщённые делегаты

**Встроенные обобщённые делегаты:**
* `Action<T1, ..., T16>` — выполняют действие, не возвращают значение;
* `Func<T1, ..., TResult>` — выполняют действие и возвращают результат;
* `Predicate<T>` — предикат (возвращает `bool`).

**Пример использования:**
```csharp
Func<int, int, int> add = (a, b) => a + b;
Predicate<string> isLong = s => s.Length > 10;
Action<string> print = s => Console.WriteLine(s);
```

#### 10. Практические примеры использования

**Пример 1: обобщённый класс‑контейнер**
```csharp
class Box<T>
{
    public T Content { get; set; }

    public void SetContent(T item) => Content = item;

    public T GetContent() => Content;
}

// Использование
Box<int> intBox = new Box<int>();
intBox.SetContent(42);

Box<string> stringBox = new Box<string>();
stringBox.SetContent("Hello");
```

**Пример 2: обобщённый метод поиска**
```csharp
static int FindIndex<T>(T[] array, T value) where T : IEquatable<T>
{
    for (int i = 0; i < array.Length; i++)
    {
        if (array[i].Equals(value))
            return i;
    }
    return -1;
}

// Использование
int[] numbers = {10, 20, 30, 40};
int index = FindIndex(numbers, 30); // возвращает 2
```

**Пример 3: обобщённый репозиторий**
```csharp
class Repository<T> where T : Entity, new()
{
    private List<T> _items = new List<T>();

    public void Add(T item) => _items.Add(item);

    public T FindById(int id) =>
        _items.FirstOrDefault(x => x.Id == id);
}

class Entity
{
    public int Id { get; set; }
}
```

#### 11. Нюансы и подводные камни

**Важные нюансы:**
* обобщения инстанцируются во время выполнения (runtime);
* каждый уникальный набор параметров типа создаёт новый тип;
* нельзя использовать операторы `==` и `!=` с `T` без ограничений;
* статические конструкторы выполняются для каждой комбинации типов;
* рефлексия с обобщениями требует особого подхода.

**Типичные ошибки:**
```csharp
// Ошибка: нельзя использовать == с T без ограничений
bool AreEqual<T>(T a, T b) => a == b; // ошибка компиляции

// Решение: использовать Equals
bool AreEqual<T>(T a, T b) => EqualityComparer<T>.Default.Equals(a, b);
```
