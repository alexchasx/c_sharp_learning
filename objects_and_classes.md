# Конспект синтаксиса C# для ООП

#### 1. Классы и объекты

**Класс** — шаблон для создания объектов. Объявляется с помощью ключевого слова `class`.

```csharp
class Person
{
    // Поля, свойства, методы
}
```

**Объект** — экземпляр класса, создаётся с помощью оператора `new`:

```csharp
Person person = new Person();
```

#### 2. Поля и свойства

* **Поля** — переменные, объявленные на уровне класса:
```csharp
private string name;
private int age;
```
* **Свойства** — механизм доступа к полям с контролем:
```csharp
public string Name
{
    get { return name; }
    set { name = value; }
}
// Автоматическое свойство:
public int Age { get; set; }
```

#### 3. Конструкторы

Специальные методы для инициализации объектов. Могут быть перегружены:

```csharp
// Конструктор по умолчанию
public Person() { }

// Конструктор с параметрами
public Person(string name, int age)
{
    this.name = name;
    this.age = age;
}
```

Инициализация с использованием конструкторов:
```csharp
Person p1 = new Person();
Person p2 = new Person("Alice", 25);
```

#### 4. Модификаторы доступа

Определяют видимость компонентов класса:
* `public` — доступен отовсюду;
* `private` — доступен только внутри класса (по умолчанию);
* `protected` — доступен в классе и его наследниках;
* `internal` — доступен в пределах сборки;
* `protected internal` — комбинация `protected` и `internal`.

#### 5. Инкапсуляция

Принцип сокрытия деталей реализации. Реализуется через:
* приватные поля;
* публичные свойства с контролируемым доступом;
* методы доступа.

Пример:
```csharp
class BankAccount
{
    private decimal balance;

    public decimal Balance
    {
        get { return balance; }
        private set { balance = value; }
    }

    public void Deposit(decimal amount)
    {
        if (amount > 0)
            balance += amount;
    }
}
```

#### 6. Наследование

Позволяет создавать новые классы на основе существующих:

```csharp
class Animal
{
    public string Name { get; set; }
    public virtual void MakeSound()
    {
        Console.WriteLine("Some sound");
    }
}

class Dog : Animal // Dog наследует Animal
{
    public override void MakeSound()
    {
        Console.WriteLine("Woof!");
    }
}
```

#### 7. Полиморфизм

Возможность объектов одного типа иметь разные реализации:
* **Переопределение методов** (`override`):
```csharp
public override void MakeSound() { /* новая реализация */ }
```
* **Перегрузка методов** (разные сигнатуры):
```csharp
public void Display(int number) { /* ... */ }
public void Display(string text) { /* ... */ }
```

#### 8. Абстрактные классы и методы

* **Абстрактный класс** не может быть инстанцирован, предназначен для наследования (`abstract`):
```csharp
abstract class Shape
{
    public abstract double CalculateArea();
}
```
* **Абстрактный метод** не имеет реализации в базовом классе, должен быть реализован в наследниках.

#### 9. Интерфейсы

Определяют контракт, который класс должен реализовать:

```csharp
interface IDrawable
{
    void Draw();
}

class Circle : IDrawable
{
    public void Draw()
    {
        Console.WriteLine("Drawing a circle");
    }
}
```

Класс может реализовывать несколько интерфейсов.

#### 10. Статические члены

Принадлежат классу, а не экземпляру:

```csharp
class MathHelper
{
    public static double PI = 3.14159;

    public static int Add(int a, int b)
    {
        return a + b;
    }
}

// Использование:
int result = MathHelper.Add(5, 3);
double pi = MathHelper.PI;
```

#### 11. Ключевое слово `this`

Ссылается на текущий экземпляр класса:

```csharp
public Person(string name)
{
    this.name = name; // this.name — поле класса, name — параметр
}
```

#### 12. Деструкторы

Вызываются сборщиком мусора перед удалением объекта (редко используются):

```csharp
~Person()
{
    // Очистка ресурсов
}
```

#### 13. Ключевые слова для ООП

* `class` — объявление класса;
* `interface` — объявление интерфейса;
* `new` — создание объекта;
* `override` — переопределение метода;
* `virtual` — объявление виртуального метода;
* `abstract` — объявление абстрактного класса/метода;
* `sealed` — запрет наследования от класса или переопределения метода.

#### 14. Пример полного класса с ООП-концепциями

```csharp
public class Employee : Person, IComparable<Employee>
{
    private string department;
    public decimal salary;

    // Конструктор
    public Employee(string name, int age, string department, decimal salary)
        : base(name, age)
    {
        this.department = department;
        this.salary = salary;
    }

    // Свойство с логикой
    public decimal Salary
    {
        get => salary;
        set => salary = value >= 0 ? value : 0;
    }

    // Переопределённый метод
    public override string ToString()
    {
        return $"{Name}, {Age} years, {department}, ${salary}";
    }

    // Реализация интерфейса
    public int CompareTo(Employee other)
    {
        return this.salary.CompareTo(other.salary);
    }
}
```

---

Этот конспект охватывает основные синтаксические конструкции C# для объектно‑ориентированного программирования. Если хотите, могу подробнее раскрыть какой‑либо раздел или привести дополнительные примеры!