# C# Erişim Belirleyicileri

## İçindekiler
1. [Giriş](#giriş)
2. [Erişim Belirleyicileri Nedir?](#erişim-belirleyicileri-nedir)
3. [Erişim Belirleyici Türleri](#erişim-belirleyici-türleri)
   - [public](#public)
   - [private](#private)
   - [protected](#protected)
   - [internal](#internal)
   - [protected internal](#protected-internal)
   - [private protected](#private-protected)
4. [Örnek Senaryolar](#örnek-senaryolar)
5. [Sınıf Seviyesinde Erişim Belirleyicileri](#sınıf-seviyesinde-erişim-belirleyicileri)
6. [İç İçe Sınıflarda Erişim Belirleyicileri](#iç-içe-sınıflarda-erişim-belirleyicileri)
7. [Varsayılan Erişim Belirleyicileri](#varsayılan-erişim-belirleyicileri)
8. [En İyi Uygulamalar](#en-iyi-uygulamalar)
9. [Sonuç](#sonuç)

## Giriş

C# nesne yönelimli bir programlama dilidir ve kapsülleme (encapsulation), kalıtım (inheritance) ve çok biçimlilik (polymorphism) gibi nesne yönelimli programlama prensiplerini destekler. Erişim belirleyicileri, bu prensiplerin uygulanmasında önemli bir rol oynar ve kodun daha güvenli, anlaşılır ve yönetilebilir olmasını sağlar.

## Erişim Belirleyicileri Nedir?

Erişim belirleyicileri, bir sınıfın veya sınıf üyesinin (alanlar, özellikler, metodlar, olaylar vb.) diğer kod parçaları tarafından görünürlüğünü ve erişilebilirliğini belirler. Başka bir deyişle, kodunuzun hangi kısımlarının dışarıdan erişilebilir olduğunu ve hangilerinin gizli kalacağını kontrol eder.

Erişim belirleyicileri, kapsülleme kavramını uygulamanın temel bir yoludur. İyi tasarlanmış bir sınıf, gereksiz uygulama detaylarını gizler ve sadece dış dünyaya gerekli olan arayüzü sunar.

## Erişim Belirleyici Türleri

C#'ta altı farklı erişim belirleyici vardır:

### public

**public** erişim belirleyicisi, üyeye herhangi bir kısıtlama olmadan erişilebileceği anlamına gelir. Bir sınıf üyesi public olarak işaretlendiğinde, o üyeye projenin herhangi bir yerinden erişilebilir.

```csharp
public class User
{
    public string Username { get; set; }
    
    public void DisplayInfo()
    {
        Console.WriteLine($"Username: {Username}");
    }
}

// Kullanım
User user = new User();
user.Username = "johndoe"; // public alan olduğu için doğrudan erişilebilir
user.DisplayInfo(); // public metot olduğu için çağrılabilir
```

### private

**private** erişim belirleyicisi, üyeye sadece tanımlandığı sınıf içinden erişilebileceği anlamına gelir. Bir sınıf üyesi private olarak işaretlendiğinde, o üyeye sadece aynı sınıf içindeki kodlar erişebilir.

```csharp
public class User
{
    private string password; // Sadece User sınıfı içinden erişilebilir
    
    public void SetPassword(string newPassword)
    {
        // Password'e sınıf içinden erişebiliriz
        password = newPassword;
    }
    
    public bool ValidatePassword(string inputPassword)
    {
        return password == inputPassword;
    }
}

// Kullanım
User user = new User();
user.SetPassword("secret123");
// user.password = "hack"; // Hata! private alana dışarıdan erişilemez
bool isValid = user.ValidatePassword("secret123"); // public metot üzerinden dolaylı erişim
```

### protected

**protected** erişim belirleyicisi, üyeye sadece tanımlandığı sınıf ve bu sınıftan türetilen alt sınıflar içinden erişilebileceği anlamına gelir.

```csharp
public class Person
{
    protected string ssn; // Sadece Person sınıfı ve türetilen sınıflar erişebilir
    
    public void SetSSN(string value)
    {
        ssn = value;
    }
}

public class Employee : Person
{
    public void DisplaySSNLastFourDigits()
    {
        // ssn, Person sınıfından protected olarak tanımlandığı için
        // türetilen Employee sınıfı içinden erişilebilir
        if (ssn != null && ssn.Length >= 4)
        {
            Console.WriteLine($"Last four digits: {ssn.Substring(ssn.Length - 4)}");
        }
    }
}

// Kullanım
Employee emp = new Employee();
emp.SetSSN("123-45-6789");
emp.DisplaySSNLastFourDigits(); // "Last four digits: 6789"
// emp.ssn = "987-65-4321"; // Hata! protected alana dışarıdan erişilemez
```

### internal

**internal** erişim belirleyicisi, üyeye sadece aynı derleme (assembly) içinden erişilebileceği anlamına gelir. Farklı bir derleme içinden internal üyelere erişilemez.

```csharp
// Assembly1.dll
internal class DatabaseHelper
{
    internal void Connect(string connectionString)
    {
        // Veritabanı bağlantı kodu
    }
}

// Aynı derleme içinden kullanım
public class UserRepository
{
    public void SaveUser(User user)
    {
        DatabaseHelper helper = new DatabaseHelper(); // internal sınıfa aynı derleme içinden erişilebilir
        helper.Connect("connection-string"); // internal metoda aynı derleme içinden erişilebilir
    }
}

// Farklı bir derleme (Assembly2.dll) içinden kullanım
public class ExternalService
{
    public void DoSomething()
    {
        // DatabaseHelper helper = new DatabaseHelper(); // Hata! Farklı derleme içinden internal sınıfa erişilemez
    }
}
```

### protected internal

**protected internal** kombinasyonu, üyeye aynı derleme içinden veya türetilen sınıflar içinden erişilebileceği anlamına gelir. Bu, internal ve protected erişim belirleyicilerinin birleşimidir.

```csharp
// Assembly1.dll
public class BaseLogger
{
    protected internal void LogInternal(string message)
    {
        // Loglama işlemi
    }
}

// Aynı derleme içinden kullanım
public class FileLogger
{
    public void LogToFile(string message)
    {
        BaseLogger logger = new BaseLogger();
        logger.LogInternal(message); // Aynı derleme içinden protected internal metoda erişilebilir
    }
}

// Assembly2.dll (farklı derleme)
public class CustomLogger : BaseLogger
{
    public void CustomLog(string message)
    {
        LogInternal(message); // Farklı derlemede olsa bile türetilen sınıf içinden erişilebilir
    }
}

public class ExternalService
{
    public void Process()
    {
        BaseLogger logger = new BaseLogger();
        // logger.LogInternal("message"); // Hata! Farklı derleme içinden ve kalıtım ilişkisi olmadan erişilemez
    }
}
```

### private protected

**private protected** kombinasyonu, C# 7.2 ile tanıtıldı ve üyeye sadece aynı derleme içindeki türetilen sınıflar içinden erişilebileceği anlamına gelir.

```csharp
// Assembly1.dll
public class BaseComponent
{
    private protected void Initialize()
    {
        // Başlatma kodu
    }
}

// Aynı derleme içinden kalıtımla kullanım
public class SpecialComponent : BaseComponent
{
    public void Setup()
    {
        Initialize(); // Aynı derleme içinde ve kalıtım ilişkisi olduğu için private protected metoda erişilebilir
    }
}

// Assembly2.dll (farklı derleme)
public class ExternalComponent : BaseComponent
{
    public void Configure()
    {
        // Initialize(); // Hata! Farklı derleme içindeki türetilen sınıftan erişilemez
    }
}
```

## Örnek Senaryolar

### Senaryo 1: Basit Kapsülleme

```csharp
public class BankAccount
{
    private decimal balance; // Doğrudan erişimi engellemek için private
    
    public decimal Balance
    {
        get { return balance; }
        private set { balance = value; } // Sadece sınıf içinden değiştirilebilir
    }
    
    public void Deposit(decimal amount)
    {
        if (amount <= 0)
        {
            throw new ArgumentException("Deposit amount must be positive");
        }
        
        Balance += amount;
    }
    
    public bool Withdraw(decimal amount)
    {
        if (amount <= 0)
        {
            throw new ArgumentException("Withdrawal amount must be positive");
        }
        
        if (Balance >= amount)
        {
            Balance -= amount;
            return true;
        }
        
        return false;
    }
}

// Kullanım
BankAccount account = new BankAccount();
account.Deposit(1000);
// account.Balance = 1000000; // Hata! private set olduğu için dışarıdan değiştirilemez
bool success = account.Withdraw(500);
Console.WriteLine($"Current balance: {account.Balance}"); // 500
```

### Senaryo 2: Kalıtım ve Erişim Belirleyicileri

```csharp
public class Vehicle
{
    // Base class members with different access modifiers
    public string Make { get; set; }
    protected int Year { get; set; }
    private string vin;
    internal decimal BaseValue { get; set; }
    protected internal void RegisterVehicle() { /* ... */ }
    private protected void UpdateVIN(string newVIN) { vin = newVIN; }
    
    public Vehicle(string make, int year, string vin)
    {
        Make = make;
        Year = year;
        this.vin = vin;
    }
    
    public string GetVIN()
    {
        return vin;
    }
}

public class Car : Vehicle
{
    public string Model { get; set; }
    
    public Car(string make, string model, int year, string vin) : base(make, year, vin)
    {
        Model = model;
    }
    
    public void UpdateRegistration(string newVIN)
    {
        Year = DateTime.Now.Year; // protected üyeye erişilebilir
        // vin = newVIN; // Hata! private üyeye erişilemez
        UpdateVIN(newVIN); // private protected üyeye erişilebilir (aynı derleme ve kalıtım)
        RegisterVehicle(); // protected internal üyeye erişilebilir
        Console.WriteLine($"Updated {Make} {Model} registration for year {Year}");
    }
}

// Başka bir sınıftan kullanım
public class VehicleRegistry
{
    public void ProcessVehicle(Vehicle vehicle)
    {
        Console.WriteLine($"Processing {vehicle.Make}"); // public property erişilebilir
        // Console.WriteLine($"Year: {vehicle.Year}"); // Hata! protected property erişilemez
        // Console.WriteLine($"VIN: {vehicle.vin}"); // Hata! private field erişilemez
        Console.WriteLine($"VIN: {vehicle.GetVIN()}"); // public method üzerinden dolaylı erişim
        vehicle.BaseValue = 10000; // internal property aynı derleme içinden erişilebilir
        vehicle.RegisterVehicle(); // protected internal method aynı derleme içinden erişilebilir
        // vehicle.UpdateVIN("NEW123"); // Hata! private protected method erişilemez (kalıtım yok)
    }
}
```

## Sınıf Seviyesinde Erişim Belirleyicileri

Sınıflar için kullanılabilecek erişim belirleyicileri `public`, `internal` (varsayılan) ve bazı nested (iç içe) sınıflar için `private` ve `protected`'dır.

```csharp
// public sınıf - herhangi bir yerden erişilebilir
public class PublicClass { }

// internal sınıf - sadece aynı derleme içinden erişilebilir
internal class InternalClass { }

// Erişim belirleyicisi belirtilmemiş - varsayılan olarak internal
class DefaultClass { }

// İç içe sınıflarda private kullanımı
public class Container
{
    // private nested sınıf - sadece Container sınıfı içinden erişilebilir
    private class NestedPrivateClass { }
    
    // protected nested sınıf - Container ve türetilen sınıflar içinden erişilebilir
    protected class NestedProtectedClass { }
    
    public void UseNestedClass()
    {
        NestedPrivateClass privateInstance = new NestedPrivateClass();
        // ...
    }
}
```

## İç İçe Sınıflarda Erişim Belirleyicileri

İç içe (nested) sınıflar, bir sınıfın içinde tanımlanan sınıflardır ve özel erişim belirleyici kurallarına sahiptir.

```csharp
public class Outer
{
    private int outerPrivate = 10;
    
    // İç içe sınıf
    public class Inner
    {
        public void AccessOuter(Outer outer)
        {
            // Inner sınıfı, içinde tanımlandığı Outer sınıfının private üyelerine doğrudan erişemez
            // Console.WriteLine(outer.outerPrivate); // Hata!
            
            // Ancak parametre ile gelen bir Outer nesnesinin private üyelerine erişebilir
            Console.WriteLine(outer.outerPrivate); // Bu çalışır
        }
    }
    
    // Private iç içe sınıf
    private class PrivateInner
    {
        public void DoSomething()
        {
            Console.WriteLine("This can only be used inside Outer class");
        }
    }
    
    public void UsePrivateInner()
    {
        PrivateInner inner = new PrivateInner();
        inner.DoSomething();
    }
}

// Kullanım
Outer outer = new Outer();
Outer.Inner inner = new Outer.Inner(); // public iç içe sınıfa erişilebilir
inner.AccessOuter(outer);

// Outer.PrivateInner privateInner = new Outer.PrivateInner(); // Hata! private iç içe sınıfa dışarıdan erişilemez
```

## Varsayılan Erişim Belirleyicileri

C#'ta farklı yapı türleri için farklı varsayılan erişim belirleyicileri vardır:

1. **Sınıflar ve Yapılar (Classes & Structs)**: Varsayılan olarak `internal`
2. **Sınıf Üyeleri (Class Members)**: Varsayılan olarak `private`
3. **Arayüzler (Interfaces)**: Varsayılan olarak `internal`
4. **Enum'lar**: Varsayılan olarak `internal`
5. **Delegate'ler**: Varsayılan olarak `internal`

```csharp
// Bu sınıf varsayılan olarak internal
class DefaultClass
{
    // Bu alan varsayılan olarak private
    int defaultField;
    
    // Bu metot varsayılan olarak private
    void DefaultMethod() { }
}

// Bu arayüz varsayılan olarak internal
interface IDefaultInterface { }

// Bu enum varsayılan olarak internal
enum DefaultEnum { Value1, Value2 }

// Bu delegate varsayılan olarak internal
delegate void DefaultDelegate();
```

## En İyi Uygulamalar

Erişim belirleyicileri kullanırken izlenecek bazı en iyi uygulamalar:

1. **Minimum Erişim İlkesi**: Her üye için mümkün olan en kısıtlayıcı erişim belirleyiciyi kullanın. Örneğin, bir üye sadece sınıf içinde kullanılıyorsa `private` olmalıdır.

2. **Kapsülleme**: Doğrudan erişim yerine özellikler (properties) kullanarak alanları kapsülleyin:

   ```csharp
   // Kötü
   public string Name;
   
   // İyi
   private string name;
   public string Name
   {
       get { return name; }
       set { name = value; }
   }
   
   // Veya daha kısa (auto-property)
   public string Name { get; set; }
   
   // Sınırlı erişim için
   public string Name { get; private set; }
   ```

3. **İç İçe Sınıflar**: İç içe sınıfları, dış sınıfla yakından ilişkili ve diğer sınıfların doğrudan erişim gerektirmediği durumlarda kullanın.

4. **Test İçin Friend Assemblies**: Test edilebilirliği kolaylaştırmak için, test projelerine internal üyeleri açmak üzere `InternalsVisibleTo` özniteliğini kullanabilirsiniz:

   ```csharp
   // Assembly1.dll içinde:
   [assembly: InternalsVisibleTo("TestProject")]
   
   // Bu sayede TestProject, Assembly1.dll içindeki internal üyelere erişebilir
   ```

5. **İçerik ve Kullanım**: Sınıfın veya üyenin amacını ve kullanımını açıkça belirtmek için XML belgelendirme yorumları kullanın:

   ```csharp
   /// <summary>
   /// Kullanıcı bilgilerini temsil eden sınıf.
   /// </summary>
   public class User
   {
       /// <summary>
       /// Kullanıcının benzersiz kimliği.
       /// Bu değer doğrudan değiştirilemez.
       /// </summary>
       public int Id { get; private set; }
   }
   ```

## Sonuç

C#'ta erişim belirleyicileri, nesne yönelimli programlamanın temel ilkelerinden biri olan kapsüllemeyi uygulamanın güçlü bir yoludur. Doğru erişim belirleyicilerini kullanarak:

1. **Güvenliği artırabilirsiniz**: Hassas verilere ve işlemlere erişimi kısıtlayarak.
2. **Bakımı kolaylaştırabilirsiniz**: Uygulama detaylarını gizleyerek ve belirli bir arayüz sağlayarak.
3. **Kodunuzu daha sağlam hale getirebilirsiniz**: Nesnelerin geçersiz durumlara girmesini engelleyerek.
4. **Gelecekteki değişikliklere uyum sağlayabilirsiniz**: İç uygulamayı değiştirirken dış arayüzü koruyarak.

Erişim belirleyicileri hakkında bilgi sahibi olmak ve bunları etkin bir şekilde kullanmak, kaliteli ve sürdürülebilir C# kodu yazmanın önemli bir parçasıdır.
