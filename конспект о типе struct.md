# Подробный конспект о типе `struct` в C#

#### 1. Основы `struct`

**`struct`** — пользовательский значимый тип данных (value type), который позволяет группировать переменные разных типов в единую структуру.

**Ключевые особенности:**
* является **значимым типом** (хранится в стеке);
* копируется по значению (при присваивании создаётся копия);
* не поддерживает наследование (не может наследоваться от других классов или структур);
* может реализовывать интерфейсы;
* автоматически наследуется от `System.ValueType`;
* не может иметь конструктор без параметров (по умолчанию есть неявный);
* поля структуры не могут быть инициализированы при объявлении (кроме как в конструкторе).

#### 2. Объявление и синтаксис

**Базовый синтаксис:**
```csharp
struct ИмяСтруктуры
{
    // Поля
    // Методы
    // Конструкторы
    // Свойства
}
```

**Пример простой структуры:**
```csharp
struct Point
{
    public int X;
    public int Y;
}
```

#### 3. Создание и использование структур

**Объявление и инициализация:**
```csharp
// Способ 1: без конструктора
Point p1;
p1.X = 10;
p1.Y = 20;

// Способ 2: с конструктором
Point p2 = new Point(); // X=0, Y=0

// Способ 3: с пользовательским конструктором
Point p3 = new Point(5, 15);
```

#### 4. Конструкторы в структурах

**Важные правила:**
* компилятор автоматически создаёт конструктор без параметров, который инициализирует все поля значениями по умолчанию;
* нельзя явно определить конструктор без параметров;
* можно определять конструкторы с параметрами.

**Пример с конструктором:**
```csharp
struct Rectangle
{
    public int Width;
    public int Height;

    public Rectangle(int width, int height)
    {
        Width = width;
        Height = height;
    }

    public int Area() => Width * Height;
}

// Использование
Rectangle rect = new Rectangle(10, 20);
Console.WriteLine($"Площадь: {rect.Area()}"); // 200
```

#### 5. Поля и свойства

**Поля:**
* могут быть любого типа;
* по умолчанию имеют модификатор доступа `private`;
* рекомендуется делать поля `private` и предоставлять доступ через свойства.

**Свойства:**
* обеспечивают контролируемый доступ к полям;
* позволяют добавить логику валидации.

**Пример со свойствами:**
```csharp
struct Person
{
    private string _name;
    private int _age;

    public string Name
    {
        get => _name;
        set => _name = value ?? "Unknown";
    }

    public int Age
    {
        get => _age;
        set
        {
            if (value >= 0 && value <= 150)
                _age = value;
            else
                throw new ArgumentException("Возраст должен быть от 0 до 150");
        }
    }
}
```

#### 6. Методы в структурах

Структуры могут содержать методы:
```csharp
struct ComplexNumber
{
    public double Real;
    public double Imaginary;

    public ComplexNumber(double real, double imaginary)
    {
        Real = real;
        Imaginary = imaginary;
    }

    public double Magnitude() => Math.Sqrt(Real * Real + Imaginary * Imaginary);

    public ComplexNumber Add(ComplexNumber other) =>
        new ComplexNumber(Real + other.Real, Imaginary + other.Imaginary);
}
```

#### 7. Реализация интерфейсов

Структуры могут реализовывать интерфейсы:
```csharp
interface IPrintable
{
    void Print();
}

struct Book : IPrintable
{
    public string Title;
    public string Author;

    public void Print()
    {
        Console.WriteLine($"Книга: {Title} ({Author})");
    }
}

// Использование
Book book = new Book { Title = "C# Guide", Author = "John Doe" };
book.Print(); // "Книга: C# Guide (John Doe)"
```

#### 8. Модификаторы доступа

В структурах можно использовать модификаторы доступа:
* `public` — доступен из любого места;
* `private` — доступен только внутри структуры;
* `internal` — доступен в пределах сборки.

#### 9. Копирование по значению

**Важное свойство структур:** при присваивании создаётся полная копия объекта:
```csharp
Point p1 = new Point(10, 20);
Point p2 = p1; // Создаётся копия p1

p2.X = 100;
Console.WriteLine(p1.X); // 10 (не изменилось)
Console.WriteLine(p2.X); // 100
```

#### 10. Nullable‑структуры

Структуры по умолчанию не могут быть `null`. Для этого используется `Nullable<T>` или `T?`:
```csharp
Point? nullablePoint = null;

if (nullablePoint.HasValue)
{
    Console.WriteLine($"X: {nullablePoint.Value.X}");
}
else
{
    Console.WriteLine("Точка не определена");
}
```

#### 11. Вспомогательные методы

Структуры наследуют методы от `System.ValueType`:
* `Equals()` — сравнение значений;
* `GetHashCode()` — получение хэш‑кода;
* `ToString()` — строковое представление.

Можно переопределить `ToString()`:
```csharp
struct Time
{
    public int Hours;
    public int Minutes;

    public override string ToString() => $"{Hours:D2}:{Minutes:D2}";
}

Time t = new Time { Hours = 9, Minutes = 5 };
Console.WriteLine(t); // "09:05"
```

#### 12. Практические примеры использования

**Пример 1: структура для координат**
```csharp
struct Coordinate
{
    public double Latitude;
    public double Longitude;

    public Coordinate(double lat, double lon)
    {
        Latitude = lat;
        Longitude = lon;
    }

    public double DistanceTo(Coordinate other)
    {
        double dLat = other.Latitude - Latitude;
        double dLon = other.Longitude - Longitude;
        return Math.Sqrt(dLat * dLat + dLon * dLon);
    }
}
```

**Пример 2: структура для денежных значений**
```csharp
struct Money
{
    public decimal Amount;
    public string Currency;

    public Money(decimal amount, string currency)
    {
        Amount = amount;
        Currency = currency;
    }

    public override string ToString() => $"{Amount:C} {Currency}";
}
```

**Пример 3: структура с логикой**
```csharp
struct Fraction
{
    public int Numerator;
    public int Denominator;

    public Fraction(int num, int den)
    {
        if (den == 0) throw new ArgumentException("Знаменатель не может быть 0");
        Numerator = num;
        Denominator = den;
    }

    public double ToDouble() => (double)Numerator / Denominator;

    public static Fraction operator +(Fraction a, Fraction b) =>
        new Fraction(a.Numerator * b.Denominator + b.Numerator * a.Denominator,
                    a.Denominator * b.Denominator);
}
```

#### 13. Когда использовать структуры

**Рекомендуется использовать `struct`, когда:**
* тип представляет одно значение (как примитивные типы);
* экземпляр типа небольшой (обычно меньше 16 байт);
* тип неизменяемый (immutable);
* тип не будет часто упаковываться/распаковываться (boxing/unboxing);
* важна производительность (структуры хранятся в стеке).

**Типичные случаи использования:**
* геометрические примитивы (`Point`, `Rectangle`, `Vector`);
* денежные значения;
* даты и время;
* комплексные числа;
* другие простые структуры данных.

#### 14. Отличия от классов

| Критерий | `struct` | `class` |
|--------|---------|---------|
| Тип | Значимый (value type) | Ссылочный (reference type) |
| Хранение | Стек (обычно) | Куча (heap) |
| Копирование | По значению | По ссылке |
| Наследование | Не поддерживает | Поддерживает |
| Конструктор без параметров | Неявный, нельзя переопределить | Можно определить |
| Инициализация полей | Только в конструкторе | Можно при объявлении |
| `null