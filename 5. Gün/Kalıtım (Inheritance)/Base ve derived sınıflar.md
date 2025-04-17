# C# Temel ve Türetilmiş Sınıflar: Parametreler ve Kalıtım Rehberi

## İçindekiler
1. [Giriş](#giriş)
2. [Temel Sınıflar (Base Classes)](#temel-sınıflar-base-classes)
3. [Türetilmiş Sınıflar (Derived Classes)](#türetilmiş-sınıflar-derived-classes)
4. [Yapıcı Metodlar ve Parametreler](#yapıcı-metodlar-ve-parametreler)
5. [Metod Geçersiz Kılma (Method Overriding)](#metod-geçersiz-kılma-method-overriding)
6. [Parametreli Metodlar ve Kalıtım](#parametreli-metodlar-ve-kalıtım)
7. [Temel Sınıf Üyelerine Erişim](#temel-sınıf-üyelerine-erişim)
8. [Sealed Sınıflar ve Metodlar](#sealed-sınıflar-ve-metodlar)
9. [Örnek Uygulamalar](#örnek-uygulamalar)
10. [İyi Uygulama Önerileri](#İyi-uygulama-önerileri)

## Giriş

C# programlama dilinde nesne yönelimli programlamanın (OOP) en temel kavramlarından biri kalıtımdır. Kalıtım, bir sınıfın başka bir sınıftan özelliklerini ve davranışlarını miras almasını sağlar. Bu süreçte, miras veren sınıfa "temel sınıf" (base class) veya "üst sınıf" (parent class), miras alan sınıfa ise "türetilmiş sınıf" (derived class) veya "alt sınıf" (child class) denir.

Bu belge, C# dilinde temel ve türetilmiş sınıflar arasındaki ilişkiyi, özellikle parametreler ve kurucu metodlar açısından detaylı bir şekilde incelemektedir.

## Temel Sınıflar (Base Classes)

Temel sınıf, kalıtım hiyerarşisinde daha üst seviyede bulunan ve özelliklerini alt sınıflara aktaran sınıftır.

### Temel Sınıf Tanımlama

```csharp
public class BaseClass
{
    // Alanlar (Fields)
    protected int _id;
    private string _name;
    
    // Özellikler (Properties)
    public string Name
    {
        get { return _name; }
        set { _name = value; }
    }
    
    // Yapıcı metodlar (Constructors)
    public BaseClass()
    {
        // Parametresiz yapıcı metod
        _id = 0;
        _name = "Varsayılan";
    }
    
    public BaseClass(int id, string name)
    {
        // Parametreli yapıcı metod
        _id = id;
        _name = name;
    }
    
    // Metodlar
    public virtual void Display()
    {
        Console.WriteLine($"ID: {_id}, Name: {_name}");
    }
}
```

### Erişim Belirleyiciler (Access Modifiers)

Temel sınıfta, erişim belirleyicileri üyelerin görünürlüğünü kontrol eder:

- **public**: Herhangi bir kod tarafından erişilebilir.
- **private**: Sadece tanımlandığı sınıf içinden erişilebilir.
- **protected**: Sadece tanımlandığı sınıf ve ondan türeyen sınıflar içinden erişilebilir.
- **internal**: Aynı assembly içindeki kodlar tarafından erişilebilir.
- **protected internal**: Aynı assembly içindeki kodlar veya türetilmiş sınıflar tarafından erişilebilir.

## Türetilmiş Sınıflar (Derived Classes)

Türetilmiş sınıf, bir temel sınıftan miras alan ve onun özelliklerini ve davranışlarını genişleten sınıftır.

### Türetilmiş Sınıf Tanımlama

```csharp
public class DerivedClass : BaseClass
{
    // Yeni alanlar
    private double _salary;

    // Yapıcı metodlar
    public DerivedClass() : base()
    {
        // Parametresiz yapıcı metod
        _salary = 0.0;
    }
    
    public DerivedClass(int id, string name, double salary) : base(id, name)
    {
        // Parametreli yapıcı metod
        _salary = salary;
    }
    
    // Yeni özellikler
    public double Salary
    {
        get { return _salary; }
        set { _salary = value; }
    }
    
    // Metod geçersiz kılma (Method Overriding)
    public override void Display()
    {
        // Temel sınıftaki metodu çağırma
        base.Display();
        Console.WriteLine($"Salary: {_salary}");
    }
}
```

Türetilmiş sınıflar, temel sınıflardan miras alır ve:

1. Temel sınıftaki özellikleri ve metodları kullanabilir.
2. Gerektiğinde temel sınıftaki metodları geçersiz kılabilir (override).
3. Yeni özellikler ve metodlar ekleyebilir.

## Yapıcı Metodlar ve Parametreler

Yapıcı metodlar (constructors), bir sınıf örneği oluşturulduğunda çağrılan özel metodlardır. Kalıtım hiyerarşisinde, yapıcı metodların çağrılma sırası ve parametrelerin aktarılması önemlidir.

### Temel Kurallar

1. Türetilmiş sınıf bir örneği oluşturulduğunda, önce temel sınıfın yapıcı metodu çalıştırılır.
2. Türetilmiş sınıfın yapıcı metodundan temel sınıfın yapıcı metoduna parametre aktarmak için `base` anahtar kelimesi kullanılır.

### Örnekler

```csharp
// Temel Sınıf
public class Animal
{
    public string Name;
    public int Age;

    public Animal()
    {
        Name = "Bilinmeyen";
        Age = 0;
    }

    public Animal(string name, int age)
    {
        Name = name;
        Age = age;
    }
}

// Türetilmiş Sınıf
public class Dog : Animal
{
    public string Breed;

    // Parametresiz yapıcı metod, temel sınıfın parametresiz yapıcısını çağırır
    public Dog() : base()
    {
        Breed = "Bilinmeyen";
    }

    // Parametreli yapıcı metod, temel sınıfın parametreli yapıcısını çağırır
    public Dog(string name, int age, string breed) : base(name, age)
    {
        Breed = breed;
    }

    // Kısmi parametreli yapıcı metod
    public Dog(string breed) : base()
    {
        Breed = breed;
    }
}
```

### Örnek Kullanım

```csharp
// Parametresiz yapıcı
Dog dog1 = new Dog();
// Sonuç: Name = "Bilinmeyen", Age = 0, Breed = "Bilinmeyen"

// Tam parametreli yapıcı
Dog dog2 = new Dog("Karabaş", 3, "Golden Retriever");
// Sonuç: Name = "Karabaş", Age = 3, Breed = "Golden Retriever"

// Kısmi parametreli yapıcı
Dog dog3 = new Dog("Husky");
// Sonuç: Name = "Bilinmeyen", Age = 0, Breed = "Husky"
```

## Metod Geçersiz Kılma (Method Overriding)

Temel sınıfta `virtual` olarak işaretlenmiş metodlar, türetilmiş sınıflarda `override` anahtar kelimesi ile geçersiz kılınabilir.

### Örnek

```csharp
public class Person
{
    public string Name { get; set; }
    
    public Person(string name)
    {
        Name = name;
    }
    
    public virtual void Introduce(string greeting)
    {
        Console.WriteLine($"{greeting}, benim adım {Name}.");
    }
}

public class Student : Person
{
    public string SchoolName { get; set; }
    
    public Student(string name, string schoolName) : base(name)
    {
        SchoolName = schoolName;
    }
    
    public override void Introduce(string greeting)
    {
        // Base sınıfın metodunu çağırma
        base.Introduce(greeting);
        Console.WriteLine($"{SchoolName} okulunda öğrenciyim.");
    }
}
```

### Kullanım

```csharp
Person person = new Person("Ahmet");
person.Introduce("Merhaba");
// Çıktı: "Merhaba, benim adım Ahmet."

Student student = new Student("Ayşe", "Ankara Üniversitesi");
student.Introduce("Selam");
// Çıktı: 
// "Selam, benim adım Ayşe."
// "Ankara Üniversitesi okulunda öğrenciyim."
```

## Parametreli Metodlar ve Kalıtım

Türetilmiş sınıflar, temel sınıfta tanımlanan parametreli metodları geçersiz kılabilir, genişletebilir veya aynen kullanabilir.

### Metod Gizleme (Method Hiding)

Bazen türetilmiş sınıfta, temel sınıftaki bir metodu geçersiz kılmak yerine gizlemek isteyebiliriz. Bu durumda `new` anahtar kelimesi kullanılır.

```csharp
public class BaseClass
{
    public virtual void Method(int x)
    {
        Console.WriteLine($"BaseClass.Method: {x}");
    }
}

public class DerivedClass : BaseClass
{
    // Temel sınıftaki metodu gizler
    public new void Method(int x)
    {
        Console.WriteLine($"DerivedClass.Method: {x * 2}");
    }
    
    // Yeni bir aşırı yüklenmiş metod ekler
    public void Method(int x, int y)
    {
        Console.WriteLine($"DerivedClass.Method: {x + y}");
    }
}
```

### Kullanım

```csharp
BaseClass baseObj = new BaseClass();
baseObj.Method(10); // Çıktı: "BaseClass.Method: 10"

DerivedClass derivedObj = new DerivedClass();
derivedObj.Method(10); // Çıktı: "DerivedClass.Method: 20"
derivedObj.Method(10, 20); // Çıktı: "DerivedClass.Method: 30"

// Polimorfik kullanım - temel sınıf referansı türetilmiş sınıf nesnesini işaret ediyor
BaseClass polyObj = new DerivedClass();
polyObj.Method(10); // Çıktı: "BaseClass.Method: 10" (new kullanıldığından)
```

## Temel Sınıf Üyelerine Erişim

Türetilmiş sınıflar, temel sınıfın `public` ve `protected` üyelerine doğrudan erişebilir. Temel sınıfın `private` üyelerine ise erişemezler.

```csharp
public class BaseClass
{
    public int PublicField = 1;
    protected int ProtectedField = 2;
    private int PrivateField = 3;
    
    public void PublicMethod()
    {
        Console.WriteLine("BaseClass.PublicMethod");
    }
    
    protected void ProtectedMethod()
    {
        Console.WriteLine("BaseClass.ProtectedMethod");
    }
    
    private void PrivateMethod()
    {
        Console.WriteLine("BaseClass.PrivateMethod");
    }
}

public class DerivedClass : BaseClass
{
    public void AccessBaseMembers()
    {
        // Public üyelere erişim
        Console.WriteLine(PublicField); // Doğrudan erişim
        PublicMethod(); // Doğrudan erişim
        
        // Protected üyelere erişim
        Console.WriteLine(ProtectedField); // Doğrudan erişim
        ProtectedMethod(); // Doğrudan erişim
        
        // Private üyelere erişim
        // Console.WriteLine(PrivateField); // Derleme hatası
        // PrivateMethod(); // Derleme hatası
        
        // Base anahtar kelimesi ile açık erişim
        base.PublicMethod();
    }
}
```

### Temel Sınıf Referansı ile Erişim

Aynı zamanda, türetilmiş sınıf nesnelerine temel sınıf referansı üzerinden erişildiğinde, polimorfizm kuralları geçerli olacaktır.

```csharp
BaseClass baseRef = new DerivedClass();

// Sadece temel sınıfta tanımlanan üyelere erişilebilir
baseRef.PublicMethod(); // Geçerli

// Türetilmiş sınıfa özgü üyelere erişmek için dönüşüm gerekir
((DerivedClass)baseRef).AccessBaseMembers(); // Geçerli, tür dönüşümü ile
```

## Sealed Sınıflar ve Metodlar

Bir sınıf `sealed` anahtar kelimesi ile işaretlenirse, bu sınıftan miras alınamaz. Benzer şekilde, `sealed` bir metod geçersiz kılınamaz.

```csharp
public class BaseClass
{
    public virtual void Method1()
    {
        Console.WriteLine("BaseClass.Method1");
    }
    
    public virtual void Method2()
    {
        Console.WriteLine("BaseClass.Method2");
    }
}

public class DerivedClass : BaseClass
{
    // Bu metod artık daha fazla geçersiz kılınamaz
    public sealed override void Method1()
    {
        Console.WriteLine("DerivedClass.Method1");
    }
    
    // Bu metod türetilmiş sınıflarda geçersiz kılınabilir
    public override void Method2()
    {
        Console.WriteLine("DerivedClass.Method2");
    }
}

// Sealed sınıf - bundan miras alınamaz
public sealed class FinalClass
{
    public void Method()
    {
        Console.WriteLine("FinalClass.Method");
    }
}

// Aşağıdaki sınıf tanımı derleme hatası verir
// public class AttemptToDerive : FinalClass { }
```

## Örnek Uygulamalar

### Şekil Hiyerarşisi Örneği

```csharp
public abstract class Shape
{
    protected string _name;
    
    public Shape(string name)
    {
        _name = name;
    }
    
    // Soyut metod, tüm türetilmiş sınıflar tarafından uygulanmalıdır
    public abstract double CalculateArea();
    
    // Sanal metod, türetilmiş sınıflar tarafından geçersiz kılınabilir
    public virtual void Display()
    {
        Console.WriteLine($"Şekil: {_name}");
        Console.WriteLine($"Alan: {CalculateArea()}");
    }
}

public class Circle : Shape
{
    private double _radius;
    
    public Circle(double radius) : base("Daire")
    {
        _radius = radius;
    }
    
    public override double CalculateArea()
    {
        return Math.PI * _radius * _radius;
    }
    
    public override void Display()
    {
        base.Display();
        Console.WriteLine($"Yarıçap: {_radius}");
    }
}

public class Rectangle : Shape
{
    private double _width;
    private double _height;
    
    public Rectangle(double width, double height) : base("Dikdörtgen")
    {
        _width = width;
        _height = height;
    }
    
    public override double CalculateArea()
    {
        return _width * _height;
    }
    
    public override void Display()
    {
        base.Display();
        Console.WriteLine($"Genişlik: {_width}, Yükseklik: {_height}");
    }
}
```

### Kullanım

```csharp
// Soyut sınıftan nesne oluşturulamaz
// Shape shape = new Shape("Şekil"); // Derleme hatası

Circle circle = new Circle(5);
circle.Display();
// Çıktı:
// Şekil: Daire
// Alan: 78.54...
// Yarıçap: 5

Rectangle rectangle = new Rectangle(4, 6);
rectangle.Display();
// Çıktı:
// Şekil: Dikdörtgen
// Alan: 24
// Genişlik: 4, Yükseklik: 6

// Polimorfik kullanım
Shape shape = new Circle(3);
Console.WriteLine($"Alan: {shape.CalculateArea()}"); // Çıktı: Alan: 28.27...
```

### Banka Hesabı Örneği

```csharp
public class BankAccount
{
    protected decimal _balance;
    public string AccountNumber { get; private set; }
    
    public BankAccount(string accountNumber, decimal initialBalance)
    {
        AccountNumber = accountNumber;
        _balance = initialBalance;
    }
    
    public virtual void Deposit(decimal amount)
    {
        if (amount <= 0)
        {
            throw new ArgumentException("Yatırılan miktar pozitif olmalıdır.");
        }
        
        _balance += amount;
        Console.WriteLine($"{amount:C} yatırıldı. Yeni bakiye: {_balance:C}");
    }
    
    public virtual bool Withdraw(decimal amount)
    {
        if (amount <= 0)
        {
            throw new ArgumentException("Çekilen miktar pozitif olmalıdır.");
        }
        
        if (amount > _balance)
        {
            Console.WriteLine("Yetersiz bakiye!");
            return false;
        }
        
        _balance -= amount;
        Console.WriteLine($"{amount:C} çekildi. Yeni bakiye: {_balance:C}");
        return true;
    }
    
    public decimal GetBalance()
    {
        return _balance;
    }
}

public class SavingsAccount : BankAccount
{
    private decimal _interestRate;
    
    public SavingsAccount(string accountNumber, decimal initialBalance, decimal interestRate)
        : base(accountNumber, initialBalance)
    {
        _interestRate = interestRate;
    }
    
    public void ApplyInterest()
    {
        decimal interest = _balance * _interestRate;
        _balance += interest;
        Console.WriteLine($"Faiz uygulandı: {interest:C}. Yeni bakiye: {_balance:C}");
    }
}

public class CheckingAccount : BankAccount
{
    private decimal _overdraftLimit;
    
    public CheckingAccount(string accountNumber, decimal initialBalance, decimal overdraftLimit)
        : base(accountNumber, initialBalance)
    {
        _overdraftLimit = overdraftLimit;
    }
    
    public override bool Withdraw(decimal amount)
    {
        if (amount <= 0)
        {
            throw new ArgumentException("Çekilen miktar pozitif olmalıdır.");
        }
        
        // Limit aşımı kontrolü
        if (amount > _balance + _overdraftLimit)
        {
            Console.WriteLine("Limit aşımı!");
            return false;
        }
        
        _balance -= amount;
        
        if (_balance < 0)
        {
            Console.WriteLine($"Uyarı: Hesap eksi bakiyede! Bakiye: {_balance:C}");
        }
        else
        {
            Console.WriteLine($"{amount:C} çekildi. Yeni bakiye: {_balance:C}");
        }
        
        return true;
    }
}
```

### Kullanım

```csharp
BankAccount basicAccount = new BankAccount("123456", 1000);
basicAccount.Deposit(500); // Çıktı: 500,00 ₺ yatırıldı. Yeni bakiye: 1.500,00 ₺
basicAccount.Withdraw(2000); // Çıktı: Yetersiz bakiye!

SavingsAccount savingsAccount = new SavingsAccount("654321", 2000, 0.05m);
savingsAccount.Deposit(1000); // Çıktı: 1.000,00 ₺ yatırıldı. Yeni bakiye: 3.000,00 ₺
savingsAccount.ApplyInterest(); // Çıktı: Faiz uygulandı: 150,00 ₺. Yeni bakiye: 3.150,00 ₺

CheckingAccount checkingAccount = new CheckingAccount("987654", 500, 1000);
checkingAccount.Withdraw(1200); // Çıktı: Uyarı: Hesap eksi bakiyede! Bakiye: -700,00 ₺
checkingAccount.Withdraw(1000); // Çıktı: Limit aşımı!
```

## İyi Uygulama Önerileri

1. **Miras Hiyerarşisini Basit Tutun**: Derin bir miras hiyerarşisi oluşturmaktan kaçının. Genellikle 2-3 seviyeden daha derin olmayan bir hiyerarşi daha kolay anlaşılır ve bakımı daha kolaydır.

2. **Soyut Sınıfları ve Arayüzleri Etkin Kullanın**: Ortak davranışı zorlamak için soyut sınıfları ve arayüzleri kullanın. Soyut sınıflar, türetilmiş sınıflara bazı varsayılan davranışlar sağlayabilir.

3. **'is-a' İlişkisini Doğru Uygulayın**: Kalıtım, 'is-a' (bir tür) ilişkisini temsil eder. Örneğin, "Kedi bir Hayvandır" ilişkisi için kalıtım uygunken, "Araba bir Motora sahiptir" ilişkisi için kompozisyon daha uygundur.

4. **Liskov Substitution Prensibini Takip Edin**: Türetilmiş sınıflar, temel sınıfların yerine geçebilir olmalıdır. Bu, türetilmiş sınıfların davranışlarının, temel sınıfın davranışlarıyla tutarlı olması gerektiği anlamına gelir.

5. **Geçersiz Kılma Yerine Kompozisyonu Düşünün**: Bazen kalıtım yerine kompozisyon (bir sınıfın başka bir sınıfı üye olarak içermesi) daha iyi bir çözüm olabilir. Bu, daha esnek ve bakımı daha kolay bir kod yapısı sağlayabilir.

6. **Temel Sınıf Yapıcılarını Dikkatli Tasarlayın**: Temel sınıfların yapıcıları, türetilmiş sınıfların ihtiyaçlarını karşılayacak şekilde esnek olmalıdır.

7. **Protected Üyeleri Akıllıca Kullanın**: Protected üyeler, temel sınıfın uygulama detaylarını türetilmiş sınıflara açarken, diğer kodlardan gizler. Bu dengeyi korumak önemlidir.

8. **Sealed Sınıfları ve Metodları Gerektiğinde Kullanın**: Bir sınıfın veya metodun daha fazla türetilmesini veya geçersiz kılınmasını önlemek için `sealed` anahtar kelimesini kullanın.

9. **Miras Hiyerarşisini Dokümante Edin**: Miras hiyerarşinizin davranışını ve amacını açıkça belgelendirin. Bu, kodunuzu başkalarının anlamasını ve bakımını kolaylaştırır.

10. **Test Etmeyi Unutmayın**: Temel ve türetilmiş sınıflarınızın davranışlarını test edin. Özellikle polimorfik davranışların beklendiği gibi çalıştığından emin olun.
