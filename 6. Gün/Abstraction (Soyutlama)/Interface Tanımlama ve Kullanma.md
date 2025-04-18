# C# Interface Tanımlama ve Kullanma Rehberi

## İçindekiler
1. [Interface Kavramı](#interface-kavramı)
2. [Interface Tanımlama](#interface-tanımlama)
3. [Interface Uygulama](#interface-uygulama)
4. [Çoklu Interface Uygulamaları](#çoklu-interface-uygulamaları)
5. [Interface Miras Alma](#interface-miras-alma)
6. [Örtülü ve Açık Uygulama](#örtülü-ve-açık-uygulama)
7. [Default Interface Metodları](#default-interface-metodları)
8. [Interface vs Abstract Sınıflar](#interface-vs-abstract-sınıflar)
9. [En İyi Kullanım Pratikleri](#en-iyi-kullanım-pratikleri)
10. [Yaygın Senaryolar ve Örnekler](#yaygın-senaryolar-ve-örnekler)

## Interface Kavramı

Interface (arayüz), C# programlama dilinde bir sözleşme veya şablon olarak düşünülebilir. Bir interface, bir sınıfın uygulaması gereken metotları, özellikleri ve olayları tanımlar ancak bu üyelerin nasıl uygulanacağını belirtmez. Interfaceler, kodunuzu daha modüler, bakımı kolay ve genişletilebilir hale getirmeye yardımcı olur.

### Interface'in Temel Özellikleri:

- Sadece üye tanımlamalarını içerir, uygulama içermez (C# 8.0'dan önce)
- İçinde alan (field) tanımlanamaz
- Üye erişim belirteçleri (public, private vb.) içermez, tüm üyeler varsayılan olarak public'tir
- Bir sınıf birden fazla interface uygulayabilir
- Interfaceler birbirinden miras alabilir

## Interface Tanımlama

C#'ta bir interface `interface` anahtar kelimesi kullanılarak tanımlanır. İsimlendirme standardı olarak interface isimleri "I" harfi ile başlar.

```csharp
public interface IMyInterface
{
    // Metot tanımı
    void DoSomething();
    
    // Özellik tanımı
    string Name { get; set; }
    
    // Salt okunur özellik
    int Count { get; }
    
    // C# 8.0 ve sonrasında default uygulama
    void DefaultMethod()
    {
        Console.WriteLine("Bu bir varsayılan metot uygulamasıdır.");
    }
}
```

## Interface Uygulama

Bir sınıf bir interface'i uygulamak için sınıf adından sonra interface adını yazmanız ve interface'de tanımlanan tüm üyeleri uygulamanız gerekir.

```csharp
public class MyClass : IMyInterface
{
    private string _name;
    private int _count;
    
    // Interface metotlarını uygulama
    public void DoSomething()
    {
        Console.WriteLine("DoSomething metodu çağrıldı");
    }
    
    // Interface özelliklerini uygulama
    public string Name
    {
        get { return _name; }
        set { _name = value; }
    }
    
    public int Count
    {
        get { return _count; }
    }
}
```

## Çoklu Interface Uygulamaları

Bir sınıf birden fazla interface uygulayabilir:

```csharp
public interface ILoggable
{
    void Log(string message);
}

public interface IPrintable
{
    void Print();
}

public class Document : ILoggable, IPrintable
{
    public void Log(string message)
    {
        Console.WriteLine($"Log: {message}");
    }
    
    public void Print()
    {
        Console.WriteLine("Belge yazdırılıyor...");
    }
}
```

## Interface Miras Alma

Interfaceler birbirinden miras alabilir, böylece daha uzmanlaşmış interfaceler oluşturabilirsiniz:

```csharp
public interface IBasic
{
    void BasicMethod();
}

public interface IAdvanced : IBasic
{
    void AdvancedMethod();
}

// Bu sınıf hem BasicMethod hem de AdvancedMethod uygulamalıdır
public class AdvancedClass : IAdvanced
{
    public void BasicMethod()
    {
        Console.WriteLine("Basic method implementation");
    }
    
    public void AdvancedMethod()
    {
        Console.WriteLine("Advanced method implementation");
    }
}
```

## Örtülü ve Açık Uygulama

C#'ta interface üyeleri iki şekilde uygulanabilir:

### Örtülü Uygulama (Implicit Implementation)

```csharp
public class MyClass : IMyInterface
{
    public void DoSomething() // Örtülü uygulama
    {
        Console.WriteLine("DoSomething metodu çağrıldı");
    }
}
```

### Açık Uygulama (Explicit Implementation)

Açık uygulama, özellikle bir sınıf birden fazla interface uyguladığında ve bu interfacelerde aynı isimde metotlar olduğunda kullanışlıdır:

```csharp
public interface IFirst
{
    void Method();
}

public interface ISecond
{
    void Method();
}

public class MyClass : IFirst, ISecond
{
    // IFirst için açık uygulama
    void IFirst.Method()
    {
        Console.WriteLine("IFirst.Method çağrıldı");
    }
    
    // ISecond için açık uygulama
    void ISecond.Method()
    {
        Console.WriteLine("ISecond.Method çağrıldı");
    }
}
```

Açık uygulama kullanıldığında, metotlara erişmek için önce nesneyi ilgili interface tipine cast etmeniz gerekir:

```csharp
MyClass obj = new MyClass();
// obj.Method(); // Bu çalışmaz, çünkü Method açık uygulanmıştır

// Interface üzerinden erişim
((IFirst)obj).Method();  // "IFirst.Method çağrıldı" yazdırır
((ISecond)obj).Method(); // "ISecond.Method çağrıldı" yazdırır

// Alternatif syntax
IFirst firstObj = obj;
firstObj.Method();       // "IFirst.Method çağrıldı" yazdırır
```

## Default Interface Metodları

C# 8.0 ile birlikte, interface'lerde varsayılan uygulama (default implementation) tanımlama özelliği eklendi. Bu, interface'leri genişletmek ve geriye dönük uyumluluk sağlamak için kullanışlıdır:

```csharp
public interface ILogger
{
    void Log(string message);
    
    // Varsayılan uygulama içeren metot
    void LogError(string message)
    {
        Log($"ERROR: {message}");
    }
}

public class ConsoleLogger : ILogger
{
    public void Log(string message)
    {
        Console.WriteLine(message);
    }
    
    // LogError metodunu yeniden uygulamaya gerek yok, varsayılan uygulama kullanılacak
}
```

Default uygulama içeren metotlara erişmek için interface referansı üzerinden erişmeniz gerektiğini unutmayın:

```csharp
ConsoleLogger logger = new ConsoleLogger();
// logger.LogError("Hata!"); // Bu çalışmaz

ILogger iLogger = logger;
iLogger.LogError("Hata!"); // Bu çalışır
```

## Interface vs Abstract Sınıflar

Interface ve abstract sınıflar arasındaki temel farklar:

| Özellik | Interface | Abstract Sınıf |
|---------|-----------|---------------|
| Çoklu Miras | Bir sınıf birden fazla interface uygulayabilir | Bir sınıf sadece bir abstract sınıftan miras alabilir |
| Uygulama | Interface metotları için uygulama sağlanamaz (C# 8.0 öncesi) | Abstract sınıflar hem abstract hem de concrete metotlar içerebilir |
| Erişim Belirteçleri | Tüm üyeler varsayılan olarak public'tir | Farklı erişim belirteçleri kullanılabilir |
| Alanlar (Fields) | Alanlar tanımlanamaz | Alanlar tanımlanabilir |
| Yapıcılar (Constructors) | Yapıcılar tanımlanamaz | Yapıcılar tanımlanabilir |

## En İyi Kullanım Pratikleri

1. **İsimlendirme**: Interface isimlerini "I" öneki ile başlatın (örn. `IDisposable`)
2. **Tek Sorumluluk**: Her interface tek bir sorumluluğa odaklanmalıdır
3. **Küçük ve Odaklı**: Interface'leri küçük ve odaklı tutun, gerekirse birden fazla küçük interface kullanın
4. **Açık/Kapalı Prensibi**: Interface'leri kodunuzu genişletilebilir tutmak için kullanın
5. **İçe Bağımlılık Tersine Çevirme**: Üst seviye modüllerin alt seviye detaylara değil, soyutlamalara bağlı olmasını sağlayın

## Yaygın Senaryolar ve Örnekler

### Örnek 1: Repository Pattern

```csharp
public interface IRepository<T>
{
    T GetById(int id);
    IEnumerable<T> GetAll();
    void Add(T entity);
    void Update(T entity);
    void Delete(int id);
}

public class ProductRepository : IRepository<Product>
{
    private readonly List<Product> _products = new List<Product>();
    
    public Product GetById(int id)
    {
        return _products.FirstOrDefault(p => p.Id == id);
    }
    
    public IEnumerable<Product> GetAll()
    {
        return _products;
    }
    
    public void Add(Product entity)
    {
        _products.Add(entity);
    }
    
    public void Update(Product entity)
    {
        var index = _products.FindIndex(p => p.Id == entity.Id);
        if (index != -1)
            _products[index] = entity;
    }
    
    public void Delete(int id)
    {
        _products.RemoveAll(p => p.Id == id);
    }
}
```

### Örnek 2: Dependency Injection

```csharp
public interface INotificationService
{
    void SendNotification(string message);
}

public class EmailNotificationService : INotificationService
{
    public void SendNotification(string message)
    {
        // E-posta ile bildirim gönderme implementasyonu
        Console.WriteLine($"E-posta gönderiliyor: {message}");
    }
}

public class SmsNotificationService : INotificationService
{
    public void SendNotification(string message)
    {
        // SMS ile bildirim gönderme implementasyonu
        Console.WriteLine($"SMS gönderiliyor: {message}");
    }
}

public class NotificationManager
{
    private readonly INotificationService _notificationService;
    
    // Dependency Injection ile INotificationService alınıyor
    public NotificationManager(INotificationService notificationService)
    {
        _notificationService = notificationService;
    }
    
    public void Notify(string message)
    {
        _notificationService.SendNotification(message);
    }
}

// Kullanımı:
var emailService = new EmailNotificationService();
var manager = new NotificationManager(emailService);
manager.Notify("Merhaba!"); // E-posta ile gönderir

var smsService = new SmsNotificationService();
var smsManager = new NotificationManager(smsService);
smsManager.Notify("Merhaba!"); // SMS ile gönderir
```

### Örnek 3: Strategy Pattern

```csharp
public interface IPaymentStrategy
{
    void Pay(decimal amount);
}

public class CreditCardPayment : IPaymentStrategy
{
    private string _cardNumber;
    private string _name;
    
    public CreditCardPayment(string cardNumber, string name)
    {
        _cardNumber = cardNumber;
        _name = name;
    }
    
    public void Pay(decimal amount)
    {
        Console.WriteLine($"{_name} adına {_cardNumber} numaralı kredi kartı ile {amount:C} ödendi");
    }
}

public class PayPalPayment : IPaymentStrategy
{
    private string _email;
    
    public PayPalPayment(string email)
    {
        _email = email;
    }
    
    public void Pay(decimal amount)
    {
        Console.WriteLine($"{_email} PayPal hesabı ile {amount:C} ödendi");
    }
}

public class ShoppingCart
{
    private IPaymentStrategy _paymentStrategy;
    
    public void SetPaymentStrategy(IPaymentStrategy paymentStrategy)
    {
        _paymentStrategy = paymentStrategy;
    }
    
    public void Checkout(decimal amount)
    {
        _paymentStrategy.Pay(amount);
    }
}

// Kullanımı:
var cart = new ShoppingCart();

// Kredi kartı ile ödeme
cart.SetPaymentStrategy(new CreditCardPayment("1234-5678-9012-3456", "Ahmet Yılmaz"));
cart.Checkout(100.50m);

// PayPal ile ödeme
cart.SetPaymentStrategy(new PayPalPayment("ahmet@example.com"));
cart.Checkout(100.50m);
```

Bu rehberde C#'ta interfacelerin nasıl tanımlanacağını ve kullanılacağını, bunlarla ilgili başlıca kavramları ve yaygın kullanım senaryolarını öğrendiniz. Interface kullanımı, kod yapınızı daha esnek, genişletilebilir ve test edilebilir hale getirerek geliştirme sürecinizi iyileştirir.
