# C# Method Overriding (Ezme) Detaylı İnceleme

## İçindekiler
1. [Giriş](#giriş)
2. [Method Overriding Nedir?](#method-overriding-nedir)
3. [Method Overriding vs Method Hiding](#method-overriding-vs-method-hiding)
4. [Method Overriding Kullanımı](#method-overriding-kullanımı)
5. [Virtual, Override ve New Anahtar Kelimeleri](#virtual-override-ve-new-anahtar-kelimeleri)
6. [Soyut Sınıflar ve Metotlar](#soyut-sınıflar-ve-metotlar)
7. [Sealed Metotlar](#sealed-metotlar)
8. [Base Anahtar Kelimesi](#base-anahtar-kelimesi)
9. [Örnek Uygulamalar](#örnek-uygulamalar)
10. [En İyi Pratikler](#en-iyi-pratikler)
11. [Yaygın Hatalar](#yaygın-hatalar)
12. [Sonuç](#sonuç)

## Giriş

C# nesne yönelimli bir programlama dilidir ve nesne yönelimli programlamanın temel kavramlarından biri olan polimorfizmi (çok biçimlilik) destekler. Method Overriding (Metot Ezme), polimorfizmin önemli bir uygulamasıdır ve C# dilinde kalıtım (inheritance) ile birlikte kullanılır. Bu belge, C#'ta method overriding kavramını, kullanımını ve ilgili konuları detaylı bir şekilde incelemektedir.

## Method Overriding Nedir?

Method Overriding (Metot Ezme), türetilmiş bir sınıfın (derived class), temel sınıftan (base class) miras aldığı bir metodu yeniden tanımlamasıdır. Bu, aynı metot imzasına (method signature) sahip olan ancak farklı bir implementasyona sahip bir metot oluşturmak anlamına gelir.

Method overriding sayesinde, türetilmiş sınıflar kendi özel davranışlarını tanımlayabilir ve aynı zamanda temel sınıfın arayüzüne (interface) bağlı kalabilir. Bu, polimorfik davranışın temelidir ve run-time'da (çalışma zamanında) hangi metot uygulamasının çağrılacağına karar verilir.

## Method Overriding vs Method Hiding

C#'ta metotları yeniden tanımlamanın iki farklı yolu vardır:

1. **Method Overriding (Metot Ezme)**: `virtual` ve `override` anahtar kelimeleri kullanılır. Çalışma zamanında nesnenin gerçek tipine göre çağrılacak metot belirlenir.

2. **Method Hiding (Metot Gizleme)**: `new` anahtar kelimesi kullanılır. Derleme zamanında referans tipine göre çağrılacak metot belirlenir.

Örnek:

```csharp
class BaseClass
{
    public virtual void Display()
    {
        Console.WriteLine("BaseClass'tan Display metodu");
    }
}

class DerivedClass : BaseClass
{
    // Method Overriding (Metot Ezme)
    public override void Display()
    {
        Console.WriteLine("DerivedClass'tan Display metodu - override");
    }
}

class AnotherDerivedClass : BaseClass
{
    // Method Hiding (Metot Gizleme)
    public new void Display()
    {
        Console.WriteLine("AnotherDerivedClass'tan Display metodu - new");
    }
}
```

Farkı görmek için:

```csharp
BaseClass bc1 = new DerivedClass();
BaseClass bc2 = new AnotherDerivedClass();

bc1.Display(); // "DerivedClass'tan Display metodu - override"
bc2.Display(); // "BaseClass'tan Display metodu"
```

## Method Overriding Kullanımı

Method overriding için temel gereksinimler:

1. Temel sınıftaki metodun `virtual`, `abstract` veya `override` anahtar kelimesiyle işaretlenmiş olması gerekir.
2. Türetilmiş sınıftaki metodun `override` anahtar kelimesi ile işaretlenmesi gerekir.
3. Her iki metodun da aynı imzaya (aynı ad, aynı parametre sayısı ve tipleri) sahip olması gerekir.
4. Her iki metodun da aynı erişim belirleyicisine (access modifier) sahip olması gerekir (veya türetilmiş sınıftaki metot daha erişilebilir olabilir).
5. Her iki metodun da aynı dönüş tipine sahip olması gerekir (C# 9.0 ile getirilen kovaryant dönüş tipleri istisnadır).

## Virtual, Override ve New Anahtar Kelimeleri

### Virtual

`virtual` anahtar kelimesi, bir metodun türetilmiş sınıflarda override edilebileceğini belirtir. Bir metodu override edilebilir yapmak için temel sınıfta bu anahtar kelimeyi kullanmak gerekir.

```csharp
public class Animal
{
    public virtual void MakeSound()
    {
        Console.WriteLine("Hayvan sesi çıkarıyor...");
    }
}
```

### Override

`override` anahtar kelimesi, türetilmiş bir sınıfta temel sınıftan gelen virtual, abstract veya override edilmiş bir metodu yeniden tanımlamak için kullanılır.

```csharp
public class Dog : Animal
{
    public override void MakeSound()
    {
        Console.WriteLine("Hav hav!");
    }
}
```

### New

`new` anahtar kelimesi, bir metodun temel sınıftaki metodu gizlemesini (hiding) sağlar. Bu, overriding yapmaktan farklıdır çünkü polimorfik davranış göstermez.

```csharp
public class Cat : Animal
{
    public new void MakeSound()
    {
        Console.WriteLine("Miyav!");
    }
}
```

## Soyut Sınıflar ve Metotlar

Soyut (abstract) sınıflar ve metotlar, method overriding kavramıyla yakından ilişkilidir.

### Abstract Sınıflar

`abstract` anahtar kelimesiyle işaretlenen bir sınıf, doğrudan örneklenemez ve genellikle türetilmiş sınıflara bir şablon sunmak için kullanılır. Soyut sınıflar, soyut metotlar içerebilir.

```csharp
public abstract class Shape
{
    public abstract double CalculateArea();
    
    public virtual void Display()
    {
        Console.WriteLine("Bu bir şekildir.");
    }
}
```

### Abstract Metotlar

`abstract` anahtar kelimesiyle işaretlenen bir metot, uygulama içermez ve türetilmiş sınıflar tarafından uygulanması gerekir. Soyut metotlar aynı zamanda varsayılan olarak `virtual`'dır.

```csharp
public class Circle : Shape
{
    public double Radius { get; set; }
    
    // Abstract metodu override etmek zorunludur
    public override double CalculateArea()
    {
        return Math.PI * Radius * Radius;
    }
    
    // Virtual metodu override etmek zorunlu değildir
    public override void Display()
    {
        Console.WriteLine($"Bu bir daire, yarıçapı: {Radius}");
    }
}
```

## Sealed Metotlar

`sealed` anahtar kelimesi, bir override edilmiş metodun daha fazla override edilmesini engellemek için kullanılır. Bir metodu sealed yapmak için, metot aynı zamanda override edilmiş olmalıdır.

```csharp
public class Animal
{
    public virtual void MakeSound()
    {
        Console.WriteLine("Hayvan sesi çıkarıyor...");
    }
}

public class Dog : Animal
{
    public sealed override void MakeSound()
    {
        Console.WriteLine("Hav hav!");
    }
}

public class GermanShepherd : Dog
{
    // Hata! 'MakeSound' metodu sealed olduğu için override edilemez
    // public override void MakeSound() { ... }
}
```

## Base Anahtar Kelimesi

`base` anahtar kelimesi, türetilmiş sınıftan temel sınıftaki üyelere erişmek için kullanılır. Override edilmiş bir metot içinde, temel sınıfın metodunu çağırmak için kullanılabilir.

```csharp
public class Animal
{
    public virtual void MakeSound()
    {
        Console.WriteLine("Hayvan sesi çıkarıyor...");
    }
}

public class Dog : Animal
{
    public override void MakeSound()
    {
        // Önce temel sınıfın MakeSound metodunu çağır
        base.MakeSound();
        
        // Sonra kendi implementasyonunu ekle
        Console.WriteLine("Hav hav!");
    }
}
```

## Örnek Uygulamalar

### Örnek 1: Şekil Hiyerarşisi

```csharp
public abstract class Shape
{
    public abstract double CalculateArea();
    public abstract double CalculatePerimeter();
    
    public virtual void Display()
    {
        Console.WriteLine($"Alan: {CalculateArea()}, Çevre: {CalculatePerimeter()}");
    }
}

public class Circle : Shape
{
    public double Radius { get; set; }
    
    public Circle(double radius)
    {
        Radius = radius;
    }
    
    public override double CalculateArea()
    {
        return Math.PI * Radius * Radius;
    }
    
    public override double CalculatePerimeter()
    {
        return 2 * Math.PI * Radius;
    }
    
    public override void Display()
    {
        Console.WriteLine($"Daire - Yarıçap: {Radius}");
        base.Display();
    }
}

public class Rectangle : Shape
{
    public double Width { get; set; }
    public double Height { get; set; }
    
    public Rectangle(double width, double height)
    {
        Width = width;
        Height = height;
    }
    
    public override double CalculateArea()
    {
        return Width * Height;
    }
    
    public override double CalculatePerimeter()
    {
        return 2 * (Width + Height);
    }
    
    public override void Display()
    {
        Console.WriteLine($"Dikdörtgen - Genişlik: {Width}, Yükseklik: {Height}");
        base.Display();
    }
}
```

Kullanımı:

```csharp
Shape circle = new Circle(5);
Shape rectangle = new Rectangle(4, 6);

circle.Display();
// Çıktı:
// Daire - Yarıçap: 5
// Alan: 78.53981633974483, Çevre: 31.41592653589793

rectangle.Display();
// Çıktı:
// Dikdörtgen - Genişlik: 4, Yükseklik: 6
// Alan: 24, Çevre: 20
```

### Örnek 2: Çalışan Hiyerarşisi

```csharp
public class Employee
{
    public string Name { get; set; }
    public string Id { get; set; }
    public double BaseSalary { get; set; }
    
    public Employee(string name, string id, double baseSalary)
    {
        Name = name;
        Id = id;
        BaseSalary = baseSalary;
    }
    
    public virtual double CalculateMonthlySalary()
    {
        return BaseSalary;
    }
    
    public virtual void DisplayDetails()
    {
        Console.WriteLine($"Çalışan: {Name}, ID: {Id}");
        Console.WriteLine($"Aylık Maaş: {CalculateMonthlySalary():C}");
    }
}

public class Manager : Employee
{
    public double Bonus { get; set; }
    
    public Manager(string name, string id, double baseSalary, double bonus)
        : base(name, id, baseSalary)
    {
        Bonus = bonus;
    }
    
    public override double CalculateMonthlySalary()
    {
        return BaseSalary + Bonus;
    }
    
    public override void DisplayDetails()
    {
        Console.WriteLine($"Yönetici: {Name}, ID: {Id}");
        Console.WriteLine($"Temel Maaş: {BaseSalary:C}, Bonus: {Bonus:C}");
        Console.WriteLine($"Toplam Aylık Maaş: {CalculateMonthlySalary():C}");
    }
}

public class Developer : Employee
{
    public int OvertimeHours { get; set; }
    public double HourlyRate { get; set; }
    
    public Developer(string name, string id, double baseSalary, int overtimeHours, double hourlyRate)
        : base(name, id, baseSalary)
    {
        OvertimeHours = overtimeHours;
        HourlyRate = hourlyRate;
    }
    
    public override double CalculateMonthlySalary()
    {
        return BaseSalary + (OvertimeHours * HourlyRate);
    }
    
    public override void DisplayDetails()
    {
        base.DisplayDetails(); // Temel sınıfın metodunu çağır
        Console.WriteLine($"Fazla Mesai Saatleri: {OvertimeHours}, Saat Ücreti: {HourlyRate:C}");
        Console.WriteLine($"Fazla Mesai Ücreti: {(OvertimeHours * HourlyRate):C}");
    }
}
```

Kullanımı:

```csharp
Employee emp = new Employee("Ahmet Yılmaz", "E001", 5000);
Manager mgr = new Manager("Ayşe Kaya", "M001", 8000, 2000);
Developer dev = new Developer("Mehmet Demir", "D001", 6000, 20, 100);

emp.DisplayDetails();
// Çıktı:
// Çalışan: Ahmet Yılmaz, ID: E001
// Aylık Maaş: ₺5,000.00

mgr.DisplayDetails();
// Çıktı:
// Yönetici: Ayşe Kaya, ID: M001
// Temel Maaş: ₺8,000.00, Bonus: ₺2,000.00
// Toplam Aylık Maaş: ₺10,000.00

dev.DisplayDetails();
// Çıktı:
// Çalışan: Mehmet Demir, ID: D001
// Aylık Maaş: ₺8,000.00
// Fazla Mesai Saatleri: 20, Saat Ücreti: ₺100.00
// Fazla Mesai Ücreti: ₺2,000.00

// Polimorfizm örneği
Employee[] employees = { emp, mgr, dev };
Console.WriteLine("\nTüm Çalışanların Maaşları:");
foreach (Employee e in employees)
{
    Console.WriteLine($"{e.Name}: {e.CalculateMonthlySalary():C}");
}
// Çıktı:
// Tüm Çalışanların Maaşları:
// Ahmet Yılmaz: ₺5,000.00
// Ayşe Kaya: ₺10,000.00
// Mehmet Demir: ₺8,000.00
```

## En İyi Pratikler

1. **Amaçlı Kullanım**: Method overriding'i sadece gerektiğinde kullanın. Aynı davranışı türetilmiş tüm sınıflarda paylaşmak istiyorsanız, temel sınıfta implementasyonu sağlamak daha iyidir.

2. **Temel Sınıf Metodunu Çağırma**: Override edilmiş metot içinde, gerektiğinde `base` anahtar kelimesiyle temel sınıfın metodunu çağırabilirsiniz. Bu, kodun yeniden kullanılabilirliğini artırır.

3. **Liskov Substitution Prensibi**: Override edilmiş metotlar, temel sınıfın metodunun davranışını değiştirmemeli, sadece genişletmelidir. Bu, türetilmiş sınıf nesnelerinin temel sınıf referansları aracılığıyla sorunsuz kullanılabilmesini sağlar.

4. **Anlamlı İsimlendirme**: Override edilen metotların isimleri, her zaman temel sınıftaki metotların isimleriyle aynı olmalıdır, ancak bu isimlerin ne yaptıklarını açıkça belirtmesi önemlidir.

5. **Soyut Sınıf vs. Arayüz**: Eğer sadece bir metodu override etmek istiyorsanız, soyut sınıf yerine arayüz (interface) kullanmayı düşünün. Ancak varsayılan bir implementasyon sağlamanız gerekiyorsa, soyut sınıf daha uygundur.

## Yaygın Hatalar

1. **Override ve New Karıştırma**: `override` yerine `new` kullanmak, polimorfik davranışı bozar ve beklenmeyen sonuçlara yol açabilir.

```csharp
// Yanlış kullanım (new ile)
public class DerivedClass : BaseClass
{
    public new void MyMethod() // Base.MyMethod'u gizler, ancak override etmez
    {
        // kod
    }
}

// Doğru kullanım (override ile)
public class DerivedClass : BaseClass
{
    public override void MyMethod() // Base.MyMethod'u override eder
    {
        // kod
    }
}
```

2. **Virtual Olmayan Metodu Override Etmeye Çalışma**: Temel sınıfta `virtual` olarak işaretlenmemiş bir metodu override edemezsiniz.

```csharp
public class BaseClass
{
    public void MyMethod() // virtual değil
    {
        // kod
    }
}

public class DerivedClass : BaseClass
{
    // Derleme hatası! MyMethod temel sınıfta virtual değil
    public override void MyMethod()
    {
        // kod
    }
}
```

3. **Metot İmzası Uyuşmazlığı**: Override eden metot, temel sınıftaki metodun imzasıyla tam olarak eşleşmelidir.

```csharp
public class BaseClass
{
    public virtual void MyMethod(int x)
    {
        // kod
    }
}

public class DerivedClass : BaseClass
{
    // Derleme hatası! İmza eşleşmiyor
    public override void MyMethod(string x)
    {
        // kod
    }
}
```

4. **Erişilebilirlik Uyuşmazlığı**: Override eden metot, temel sınıftaki metottan daha az erişilebilir olamaz.

```csharp
public class BaseClass
{
    public virtual void MyMethod()
    {
        // kod
    }
}

public class DerivedClass : BaseClass
{
    // Derleme hatası! protected, public'ten daha az erişilebilir
    protected override void MyMethod()
    {
        // kod
    }
}
```

## Sonuç

Method overriding (ezme), C#'ta nesne yönelimli programlamanın güçlü bir özelliğidir ve polimorfizm için temel oluşturur. Doğru bir şekilde kullanıldığında, uygulamanızın esnekliğini ve bakım yapılabilirliğini artırır. Temel sınıfların bir şablon sağlamasına ve türetilmiş sınıfların bu şablonu kendi özel davranışlarıyla genişletmesine olanak tanır.

Bu belge, C#'ta method overriding kavramını, kullanımını ve ilgili konuları kapsamlı bir şekilde ele almıştır. Doğru anahtar kelimelerin kullanılması, temel sınıf metodunun çağrılması ve yaygın hataların önlenmesi gibi en iyi pratiklere dikkat ederek, method overriding'i projelerinizde etkili bir şekilde kullanabilirsiniz.
