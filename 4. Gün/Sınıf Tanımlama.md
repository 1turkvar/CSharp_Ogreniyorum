# C# Sınıf Tanımlama Rehberi

## İçindekiler
1. [Sınıf Kavramı](#sınıf-kavramı)
2. [Temel Sınıf Tanımlama](#temel-sınıf-tanımlama)
3. [Erişim Belirteçleri](#erişim-belirteçleri)
4. [Üye Değişkenler ve Özellikler](#üye-değişkenler-ve-özellikler)
5. [Metodlar](#metodlar)
6. [Yapıcı Metodlar (Constructor)](#yapıcı-metodlar-constructor)
7. [Statik Üyeler](#statik-üyeler)
8. [Kalıtım (Inheritance)](#kalıtım-inheritance)
9. [Arayüzler (Interface)](#arayüzler-interface)
10. [Kapsülleme (Encapsulation)](#kapsülleme-encapsulation)
11. [İleri Düzey Konular](#i̇leri-düzey-konular)
12. [Örnekler](#örnekler)

## Sınıf Kavramı

Sınıf (Class), nesne yönelimli programlamanın temel yapı taşıdır. Nesnelerin özelliklerini ve davranışlarını tanımlayan bir şablondur. C#'ta sınıflar, veri ve bu veri üzerinde işlem yapan metodları bir araya getiren yapılardır.

Sınıflar:
- Verileri (alanlar, özellikler)
- Davranışları (metodlar)
- Olayları (events)
- Ve diğer üyeleri bir araya getirir.

## Temel Sınıf Tanımlama

En temel haliyle bir C# sınıfı şu şekilde tanımlanır:

```csharp
// Namespace içinde tanımlama (isteğe bağlı)
namespace MyApplication
{
    // Erişim belirteci ve sınıf adı
    public class Person
    {
        // Sınıf üyeleri buraya yazılır
    }
}
```

## Erişim Belirteçleri

C#'ta sınıfların ve üyelerinin görünürlüğünü belirleyen erişim belirteçleri:

| Belirteç | Açıklama |
|----------|----------|
| `public` | Tüm projeden erişilebilir |
| `private` | Sadece aynı sınıf içinden erişilebilir |
| `protected` | Aynı sınıf ve alt sınıflardan erişilebilir |
| `internal` | Aynı assembly (proje) içinden erişilebilir |
| `protected internal` | Aynı assembly veya türetilen sınıflardan erişilebilir |
| `private protected` | (C# 7.2+) Aynı assembly içindeki türetilen sınıflardan erişilebilir |

## Üye Değişkenler ve Özellikler

### Alanlar (Fields)

Alanlar, sınıfın veri depolayan üyeleridir:

```csharp
public class Person
{
    // Alan (Field) tanımlama
    private string _name;
    private int _age;
}
```

### Özellikler (Properties)

Özellikler, alanları kapsülleyen ve kontrollü erişim sağlayan üyelerdir:

```csharp
public class Person
{
    // Özel alan
    private string _name;

    // Tam özellik
    public string Name
    {
        get { return _name; }
        set { _name = value; }
    }

    // Auto-property (Otomatik özellik)
    public int Age { get; set; }

    // Salt okunur özellik
    public bool IsAdult => Age >= 18;

    // C# 9.0+ init-only özellik
    public string Id { get; init; }
}
```

## Metodlar

Metodlar, sınıfın davranışlarını tanımlayan fonksiyonlardır:

```csharp
public class Person
{
    public string Name { get; set; }
    public int Age { get; set; }

    // Metod tanımlama
    public void Introduce()
    {
        Console.WriteLine($"Merhaba, ben {Name}, {Age} yaşındayım.");
    }

    // Parametreli ve dönüş değeri olan metod
    public bool CanVote(int legalAge)
    {
        return Age >= legalAge;
    }
}
```

## Yapıcı Metodlar (Constructor)

Yapıcı metodlar, sınıftan nesne oluşturulduğunda çalışan özel metodlardır:

```csharp
public class Person
{
    // Özellikler
    public string Name { get; set; }
    public int Age { get; set; }

    // Parametresiz yapıcı metod
    public Person()
    {
        Name = "İsimsiz";
        Age = 0;
    }

    // Parametreli yapıcı metod
    public Person(string name, int age)
    {
        Name = name;
        Age = age;
    }
}
```

## Statik Üyeler

Statik üyeler sınıfın kendisine aittir, nesnelere değil:

```csharp
public class MathHelper
{
    // Statik alan
    public static double Pi = 3.14159;

    // Statik metod
    public static double CalculateCircleArea(double radius)
    {
        return Pi * radius * radius;
    }
}

// Kullanımı:
double area = MathHelper.CalculateCircleArea(5);
```

## Kalıtım (Inheritance)

Kalıtım, bir sınıfın başka bir sınıftan özelliklerini ve davranışlarını devralmasıdır:

```csharp
// Temel sınıf
public class Person
{
    public string Name { get; set; }
    public int Age { get; set; }

    public void Introduce()
    {
        Console.WriteLine($"Merhaba, ben {Name}");
    }
}

// Türetilmiş sınıf
public class Student : Person
{
    public int StudentId { get; set; }
    public string Department { get; set; }

    // Metod ezme (override)
    public new void Introduce()
    {
        Console.WriteLine($"Merhaba, ben {Name}, {Department} bölümünde öğrenciyim.");
    }
}
```

## Arayüzler (Interface)

Arayüzler, sınıfların uyması gereken sözleşmeleri tanımlar:

```csharp
// Arayüz tanımlama
public interface IMovable
{
    void Move(int x, int y);
    double Speed { get; set; }
}

// Arayüzü uygulayan sınıf
public class Car : IMovable
{
    public double Speed { get; set; }

    public void Move(int x, int y)
    {
        Console.WriteLine($"Araba {x},{y} konumuna {Speed} hızında hareket ediyor.");
    }
}
```

## Kapsülleme (Encapsulation)

Kapsülleme, verileri ve işlemleri bir araya getirip dış dünyadan koruma mekanizmasıdır:

```csharp
public class BankAccount
{
    // Kapsüllenmiş veri
    private decimal _balance;
    
    public string AccountNumber { get; private set; }

    public decimal Balance 
    { 
        get { return _balance; }
        private set { _balance = value; }
    }

    public BankAccount(string accountNumber, decimal initialBalance)
    {
        AccountNumber = accountNumber;
        _balance = initialBalance;
    }

    // Kontrollü erişim metodu
    public void Deposit(decimal amount)
    {
        if (amount <= 0)
            throw new ArgumentException("Miktar pozitif olmalıdır.");
        
        _balance += amount;
    }

    public bool Withdraw(decimal amount)
    {
        if (amount <= 0)
            throw new ArgumentException("Miktar pozitif olmalıdır.");
            
        if (_balance < amount)
            return false;
            
        _balance -= amount;
        return true;
    }
}
```

## İleri Düzey Konular

### Soyut Sınıflar (Abstract Class)

Doğrudan örneği oluşturulamayan, türetilen sınıflar için şablon görevi gören sınıflardır:

```csharp
public abstract class Shape
{
    // Soyut metod (alt sınıflar tarafından uygulanmak zorunda)
    public abstract double CalculateArea();
    
    // Normal metod
    public void Display()
    {
        Console.WriteLine($"Bu şeklin alanı: {CalculateArea()}");
    }
}

public class Circle : Shape
{
    public double Radius { get; set; }
    
    public Circle(double radius)
    {
        Radius = radius;
    }
    
    // Soyut metodun uygulanması
    public override double CalculateArea()
    {
        return Math.PI * Radius * Radius;
    }
}
```

### Sealed Sınıflar

Kalıtıma kapatılmış sınıflardır:

```csharp
public sealed class FinalClass
{
    // Bu sınıftan türetme yapılamaz
}
```

### Partial Sınıflar

Sınıf tanımını birden fazla dosyaya bölmeye yarar:

```csharp
// File1.cs
public partial class Customer
{
    public string Name { get; set; }
    public string Email { get; set; }
}

// File2.cs
public partial class Customer
{
    public void SendEmail(string message)
    {
        // Email gönderme kodu
    }
}
```

### Generic Sınıflar

Tip parametreli sınıflardır:

```csharp
public class Repository<T> where T : class
{
    private List<T> _items = new List<T>();
    
    public void Add(T item)
    {
        _items.Add(item);
    }
    
    public List<T> GetAll()
    {
        return _items;
    }
}

// Kullanımı:
var personRepo = new Repository<Person>();
personRepo.Add(new Person { Name = "Ali" });
```

## Örnekler

### Basit Bir Sınıf Örneği

```csharp
public class Book
{
    // Özellikler
    public string Title { get; set; }
    public string Author { get; set; }
    public int Pages { get; set; }
    private bool _isRead;
    
    // Constructor
    public Book(string title, string author, int pages)
    {
        Title = title;
        Author = author;
        Pages = pages;
        _isRead = false;
    }
    
    // Metodlar
    public void MarkAsRead()
    {
        _isRead = true;
        Console.WriteLine($"{Title} kitabı okundu olarak işaretlendi.");
    }
    
    public bool IsRead()
    {
        return _isRead;
    }
    
    public void DisplayInfo()
    {
        Console.WriteLine($"Kitap: {Title}");
        Console.WriteLine($"Yazar: {Author}");
        Console.WriteLine($"Sayfa Sayısı: {Pages}");
        Console.WriteLine($"Okundu mu: {(_isRead ? "Evet" : "Hayır")}");
    }
}
```

### Kalıtım ve Polimorfizm Örneği

```csharp
// Temel sınıf
public abstract class Animal
{
    public string Name { get; set; }
    
    public Animal(string name)
    {
        Name = name;
    }
    
    public abstract void MakeSound();
    
    public void Sleep()
    {
        Console.WriteLine($"{Name} uyuyor...");
    }
}

// Türetilmiş sınıf 1
public class Dog : Animal
{
    public Dog(string name) : base(name) { }
    
    public override void MakeSound()
    {
        Console.WriteLine($"{Name} havlıyor: Hav hav!");
    }
    
    public void Fetch()
    {
        Console.WriteLine($"{Name} topu getiriyor!");
    }
}

// Türetilmiş sınıf 2
public class Cat : Animal
{
    public Cat(string name) : base(name) { }
    
    public override void MakeSound()
    {
        Console.WriteLine($"{Name} miyavlıyor: Miyav!");
    }
    
    public void Climb()
    {
        Console.WriteLine($"{Name} tırmanıyor!");
    }
}

// Kullanımı
Animal animal1 = new Dog("Karabaş");
Animal animal2 = new Cat("Pamuk");

animal1.MakeSound();  // "Karabaş havlıyor: Hav hav!"
animal2.MakeSound();  // "Pamuk miyavlıyor: Miyav!"

// Tip dönüşümü
if (animal1 is Dog dog)
{
    dog.Fetch();  // "Karabaş topu getiriyor!"
}
```

### Arayüz Kullanım Örneği

```csharp
public interface IPayable
{
    decimal GetPaymentAmount();
}

public class Employee : IPayable
{
    public string Name { get; set; }
    public decimal Salary { get; set; }
    
    public Employee(string name, decimal salary)
    {
        Name = name;
        Salary = salary;
    }
    
    public decimal GetPaymentAmount()
    {
        return Salary;
    }
}

public class Invoice : IPayable
{
    public string PartNumber { get; set; }
    public int Quantity { get; set; }
    public decimal PricePerItem { get; set; }
    
    public Invoice(string partNumber, int quantity, decimal price)
    {
        PartNumber = partNumber;
        Quantity = quantity;
        PricePerItem = price;
    }
    
    public decimal GetPaymentAmount()
    {
        return Quantity * PricePerItem;
    }
}

// Kullanımı
IPayable[] payableItems = new IPayable[2];
payableItems[0] = new Employee("Ahmet Yılmaz", 5000M);
payableItems[1] = new Invoice("12345", 2, 100M);

foreach (var item in payableItems)
{
    Console.WriteLine($"Ödeme tutarı: {item.GetPaymentAmount():C}");
}
```

## Özet

C# sınıfları, nesne yönelimli programlamanın temel yapı taşlarıdır. Bu rehberde:

- Temel sınıf tanımlama ve üye değişkenler
- Özellikler ve metodlar
- Yapıcı metodlar
- Kalıtım ve arayüzler
- Kapsülleme ve diğer ileri düzey konular

üzerinde durduk. Bu bilgiler, C# ile nesne yönelimli programlama yaparken sağlam bir temel oluşturabilir.
