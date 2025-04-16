# C# Static ve Instance Üyeler

## İçindekiler
1. [Giriş](#giriş)
2. [Instance (Örnek) Üyeler](#instance-örnek-üyeler)
3. [Static (Statik) Üyeler](#static-statik-üyeler)
4. [Karşılaştırma](#karşılaştırma)
5. [Ne Zaman Hangisi Kullanılmalı?](#ne-zaman-hangisi-kullanılmalı)
6. [Yaygın Kullanım Desenleri](#yaygın-kullanım-desenleri)
7. [En İyi Uygulama Önerileri](#en-iyi-uygulama-önerileri)
8. [Örnekler](#örnekler)
9. [Özet](#özet)

## Giriş

C# nesne yönelimli bir programlama dilidir ve sınıflar bu paradigmanın temel yapı taşlarıdır. Sınıflar içerisinde iki temel üye türü bulunur: **instance (örnek) üyeler** ve **static (statik) üyeler**. Bu iki tür arasındaki farkı anlamak, C# ile etkili ve verimli kod yazmanın temel gerekliliklerinden biridir.

## Instance (Örnek) Üyeler

Instance üyeler, bir sınıfın her örneği (instance) için ayrı ayrı oluşturulan ve o örneğe özgü olan üyelerdir.

### Özellikleri

- Her nesne örneği için ayrı bir kopyası oluşturulur
- Nesnenin örneği oluşturulduktan sonra erişilebilir
- `this` anahtar sözcüğü ile erişilebilir
- Nesne örneği üzerinden çağrılırlar (örnek: `myObject.InstanceMethod()`)
- Instance üyeler, nesnenin durumunu (state) temsil eder

### Instance Üye Türleri

- **Instance Alanlar (Fields)**: Nesnenin durumunu saklayan değişkenler
- **Instance Özellikler (Properties)**: Alanları kapsülleyen ve erişim kontrolü sağlayan üyeler  
- **Instance Metotlar (Methods)**: Nesne ile ilgili işlemleri gerçekleştiren fonksiyonlar
- **Instance Olaylar (Events)**: Nesne durumundaki değişikliklere yanıt veren mekanizmalar
- **Instance Indexers**: Nesnelere dizi benzeri erişim sağlayan üyeler
- **Instance Yapıcılar (Constructors)**: Nesne oluşturulduğunda çalışan metotlar

### Kod Örneği

```csharp
public class Person
{
    // Instance alanlar
    private string name;
    private int age;
    
    // Instance özellikler
    public string Name
    {
        get { return name; }
        set { name = value; }
    }
    
    public int Age
    {
        get { return age; }
        set { age = value; }
    }
    
    // Instance yapıcı
    public Person(string name, int age)
    {
        this.name = name;
        this.age = age;
    }
    
    // Instance metot
    public string GetInfo()
    {
        return $"{Name} is {Age} years old.";
    }
    
    // Instance olay
    public event EventHandler AgeChanged;
    
    // Instance metot - olayı tetikleyen
    public void HaveBirthday()
    {
        age++;
        AgeChanged?.Invoke(this, EventArgs.Empty);
    }
}
```

## Static (Statik) Üyeler

Static üyeler, bir sınıfın tüm örnekleri için ortak olan ve sınıfa özgü olan üyelerdir. Bu üyeler sınıf düzeyinde var olur ve her bir nesne örneği için ayrı ayrı oluşturulmazlar.

### Özellikleri

- Tüm sınıf örnekleri için tek bir kopyası vardır
- Nesne örneği oluşturmadan doğrudan sınıf adı üzerinden erişilebilir
- Bellek kullanımı açısından daha verimlidir (tek bir kopya)
- Programın çalışma süresi boyunca bellekte kalır
- `static` anahtar sözcüğü ile tanımlanır
- Sınıf adı üzerinden çağrılırlar (örnek: `ClassName.StaticMethod()`)
- Static üyeler, sınıfın genel davranışlarını temsil eder

### Static Üye Türleri

- **Static Alanlar (Fields)**: Sınıfın tüm örnekleri için ortak değişkenler
- **Static Özellikler (Properties)**: Static alanları kapsülleyen ve erişim kontrolü sağlayan üyeler
- **Static Metotlar (Methods)**: Sınıfın tüm örnekleri için ortak işlemler
- **Static Olaylar (Events)**: Sınıf düzeyinde tepki veren mekanizmalar
- **Static Yapıcılar (Constructors)**: Sınıf ilk kez kullanıldığında bir kez çalışan özel metotlar
- **Static Sınıflar**: Sadece static üyeler içerebilen ve örneği oluşturulamamayan sınıflar

### Kod Örneği

```csharp
public class MathOperations
{
    // Static alan
    private static double pi = 3.14159;
    
    // Static özellik
    public static double Pi
    {
        get { return pi; }
    }
    
    // Static sayaç - tüm örnekler için ortak
    private static int operationCount = 0;
    
    // Static özellik - sayaca erişim
    public static int OperationCount
    {
        get { return operationCount; }
    }
    
    // Static metot
    public static double CalculateCircleArea(double radius)
    {
        operationCount++;
        return Pi * radius * radius;
    }
    
    // Static yapıcı - sınıf ilk kullanıldığında çalışır
    static MathOperations()
    {
        Console.WriteLine("MathOperations sınıfı ilk kez kullanılıyor.");
    }
    
    // Instance metot - ancak static alan kullanıyor
    public void ReportOperations()
    {
        Console.WriteLine($"Şu ana kadar {operationCount} işlem gerçekleştirildi.");
    }
}

// Tamamen static bir sınıf örneği
public static class Logger
{
    // Static metot
    public static void LogInfo(string message)
    {
        Console.WriteLine($"INFO: {message}");
    }
    
    // Static metot
    public static void LogError(string message)
    {
        Console.WriteLine($"ERROR: {message}");
    }
}
```

## Karşılaştırma

| Özellik | Instance Üyeler | Static Üyeler |
|---------|----------------|--------------|
| Tanımlama | Normal tanımlama | `static` anahtar sözcüğü ile |
| Erişim | Nesne örneği üzerinden | Sınıf adı üzerinden |
| Bellek | Her nesne için ayrı | Tüm sınıf için tek kopya |
| Ömür | Nesne hayatta olduğu sürece | Program çalıştığı sürece |
| `this` kullanımı | Kullanılabilir | Kullanılamaz |
| İlişki | Nesne durumuna bağlı | Nesne durumundan bağımsız |
| Soyutlama | Nesne davranışlarını temsil eder | Sınıf davranışlarını temsil eder |

## Ne Zaman Hangisi Kullanılmalı?

### Instance Üyeleri Şu Durumlarda Kullanın:

1. **Nesne durumunu temsil eden veriler için**
   - Örnekler: Bir kullanıcının adı, yaşı, e-posta adresi
   
2. **Her nesne için ayrı olması gereken davranışlar için**
   - Örnekler: Kullanıcının bilgilerini güncelleme, sipariş toplam tutarını hesaplama

3. **Nesnenin bireysel durumuna bağlı işlemler için**
   - Örnekler: Hesap bakiyesi kontrolü, oyun karakterinin can puanı kontrolü

4. **Çoklu nesne örneğinin farklı davranması gerektiğinde**
   - Örnekler: Farklı kullanıcıların farklı yetkileri olması

### Static Üyeleri Şu Durumlarda Kullanın:

1. **Yardımcı metotlar ve fonksiyonlar için**
   - Örnekler: Matematiksel hesaplamalar, dönüşüm işlemleri

2. **Tüm nesneler için ortak değerler için**
   - Örnekler: Sabitler, birimler, varsayılan değerler
   
3. **Global sayaçlar veya program genelinde takip edilen değerler için**
   - Örnekler: Toplam işlem sayısı, oluşturulan nesne sayısı

4. **Factory metotları (nesne oluşturma metotları) için**
   - Örnekler: `Create()`, `GetInstance()` gibi metotlar
   
5. **Nesne durumuna bağlı olmayan, yardımcı fonksiyonlar için**
   - Örnekler: String işleme, tarih formatlama metotları

6. **Uygulama genelinde tek örnek olması gereken nesneler için (Singleton deseni)**
   - Örnekler: Yapılandırma yöneticisi, log sistemi

## Yaygın Kullanım Desenleri

### 1. Yardımcı Sınıflar (Utility Classes)

Tamamen static üyelerden oluşan ve genellikle yaygın işlemleri gerçekleştirmek için kullanılan sınıflardır.

```csharp
public static class StringHelper
{
    public static string Capitalize(string input)
    {
        if (string.IsNullOrEmpty(input))
            return input;
            
        return char.ToUpper(input[0]) + input.Substring(1);
    }
    
    public static bool IsValidEmail(string email)
    {
        // E-posta doğrulama mantığı
        return email.Contains("@") && email.Contains(".");
    }
}
```

### 2. Factory Metotları

Nesne oluşturmak için kullanılan static metotlar:

```csharp
public class Product
{
    public string Name { get; set; }
    public decimal Price { get; set; }
    
    private Product() { }
    
    public static Product CreateBookProduct(string title, decimal price)
    {
        return new Product 
        {
            Name = $"Book: {title}",
            Price = price
        };
    }
    
    public static Product CreateElectronicsProduct(string name, decimal price)
    {
        return new Product
        {
            Name = $"Electronics: {name}",
            Price = price * 1.18m // KDV eklenmiş
        };
    }
}
```

### 3. Singleton Deseni

Uygulamada tek bir örneği olması gereken sınıflar için kullanılan bir tasarım desenidir:

```csharp
public class DatabaseConnection
{
    // Tek örnek için static alan
    private static DatabaseConnection instance;
    
    // Thread-safe erişim için kilitleme nesnesi
    private static readonly object lockObject = new object();
    
    // Private yapıcı - dışarıdan örnek oluşturulamaz
    private DatabaseConnection() 
    {
        // Bağlantı kurma işlemleri
    }
    
    // Örneğe erişim için static özellik
    public static DatabaseConnection Instance
    {
        get
        {
            // Çift kontrollü kilitleme (Double-check locking)
            if (instance == null)
            {
                lock (lockObject)
                {
                    if (instance == null)
                    {
                        instance = new DatabaseConnection();
                    }
                }
            }
            return instance;
        }
    }
    
    // Instance metot - veritabanı işlemleri
    public void ExecuteQuery(string query)
    {
        // Sorgu çalıştırma işlemleri
    }
}
```

### 4. Extension Metotları

Mevcut tiplere yeni fonksiyonellik eklemek için kullanılan static metotlar:

```csharp
public static class StringExtensions
{
    public static int WordCount(this string str)
    {
        if (string.IsNullOrEmpty(str))
            return 0;
            
        return str.Split(new[] { ' ', '\t', '\n' }, StringSplitOptions.RemoveEmptyEntries).Length;
    }
    
    public static string Truncate(this string str, int maxLength)
    {
        if (string.IsNullOrEmpty(str) || str.Length <= maxLength)
            return str;
            
        return str.Substring(0, maxLength) + "...";
    }
}
```

## En İyi Uygulama Önerileri

### Static Üyeler İçin Öneriler

1. **Tamamen bağımsız fonksiyonellik için static metotlar kullanın**
   - Metodun nesne durumuna bağlı olmadığından emin olun

2. **Static sınıfları sadece gerçekten gerektiğinde kullanın**
   - Test edilebilirliği zorlaştırabileceğini unutmayın

3. **Static yapıcıları dikkatli kullanın**
   - İlk çağrıda performans etkisi olabilir (lazy initialization teknikleri uygulayın)

4. **Thread-safety konularına dikkat edin**
   - Static üyeler tüm uygulama tarafından paylaşıldığı için eşzamanlılık sorunları olabilir

5. **Static üyeler aracılığıyla dış bağımlıkları yönetin**
   - Dependency Injection kullanarak bağımlılıkları daha iyi kontrol edin

### Instance Üyeler İçin Öneriler

1. **Nesneye özgü durum için instance alanlar kullanın**
   - Her nesne için ayrı olması gereken veriler

2. **Davranış nesne durumuna bağlıysa instance metot kullanın**
   - Nesnenin kendi verilerine dayalı hesaplamalar yapan metotlar

3. **Kapsülleme için private/protected alanlar ve public özellikler kullanın**
   - Veri gizleme ve validasyon için uygun erişim belirleyicileri seçin

4. **Interface'leri instance üyelerle uygulayın**
   - Polymorphism ve kod soyutlaması için

## Örnekler

### Örnek 1: Banka Hesabı

Hem instance hem de static üyelerin birlikte kullanımı:

```csharp
public class BankAccount
{
    // Static alan - tüm hesaplar için ortak
    private static decimal interestRate = 0.05m;
    private static int accountCounter = 0;
    
    // Instance alanlar - her hesap için ayrı
    private string accountNumber;
    private decimal balance;
    
    // Static özellik
    public static decimal InterestRate
    {
        get { return interestRate; }
        set { interestRate = value; }
    }
    
    // Instance özellikler
    public string AccountNumber => accountNumber;
    public decimal Balance => balance;
    
    // Static yapıcı
    static BankAccount()
    {
        Console.WriteLine("Banka hesap sistemi başlatıldı.");
    }
    
    // Instance yapıcı
    public BankAccount(decimal initialDeposit)
    {
        accountCounter++;
        accountNumber = $"ACC{DateTime.Now.Year}{accountCounter:D6}";
        balance = initialDeposit;
    }
    
    // Instance metot
    public void Deposit(decimal amount)
    {
        if (amount <= 0)
            throw new ArgumentException("Yatırılan miktar pozitif olmalıdır.");
            
        balance += amount;
    }
    
    // Instance metot
    public bool Withdraw(decimal amount)
    {
        if (amount <= 0)
            throw new ArgumentException("Çekilen miktar pozitif olmalıdır.");
            
        if (balance >= amount)
        {
            balance -= amount;
            return true;
        }
        return false;
    }
    
    // Instance metot - ancak static alan kullanıyor
    public decimal CalculateYearlyInterest()
    {
        return balance * interestRate;
    }
    
    // Static metot - toplam hesap sayısını döndürür
    public static int GetTotalAccountCount()
    {
        return accountCounter;
    }
    
    // Static factory metodu
    public static BankAccount CreateSavingsAccount(decimal initialDeposit)
    {
        if (initialDeposit < 100)
            throw new ArgumentException("Tasarruf hesabı için minimum 100 birim gereklidir.");
            
        var account = new BankAccount(initialDeposit);
        // Tasarruf hesabı için özel işlemler...
        return account;
    }
}
```

### Örnek 2: Logger Sınıfı (Tamamen Static)

```csharp
public static class Logger
{
    private static string logPath = "application.log";
    private static bool isInitialized = false;
    
    public static string LogPath
    {
        get { return logPath; }
        set 
        { 
            logPath = value;
            Initialize();
        }
    }
    
    // Static yapıcı
    static Logger()
    {
        Initialize();
    }
    
    private static void Initialize()
    {
        if (!isInitialized)
        {
            // Log dosyası hazırlama işlemleri
            isInitialized = true;
        }
    }
    
    public static void LogInfo(string message)
    {
        WriteLog($"INFO: {DateTime.Now}: {message}");
    }
    
    public static void LogError(string message, Exception ex = null)
    {
        string errorMessage = $"ERROR: {DateTime.Now}: {message}";
        if (ex != null)
            errorMessage += $" - Exception: {ex.Message}";
            
        WriteLog(errorMessage);
    }
    
    private static void WriteLog(string entry)
    {
        try
        {
            // Thread-safe log yazma
            lock (logPath)
            {
                File.AppendAllText(logPath, entry + Environment.NewLine);
            }
        }
        catch
        {
            // Log yazma hatası - konsola yazdırma yedek mekanizması
            Console.WriteLine($"LOG FAILED: {entry}");
        }
    }
}
```

### Örnek 3: Extension Metotları ile Genişletme

```csharp
public static class IntExtensions
{
    public static bool IsEven(this int number)
    {
        return number % 2 == 0;
    }
    
    public static bool IsPrime(this int number)
    {
        if (number <= 1) return false;
        if (number <= 3) return true;
        
        if (number % 2 == 0 || number % 3 == 0) return false;
        
        for (int i = 5; i * i <= number; i += 6)
        {
            if (number % i == 0 || number % (i + 2) == 0)
                return false;
        }
        
        return true;
    }
    
    public static int Factorial(this int number)
    {
        if (number < 0) throw new ArgumentException("Faktöriyel negatif sayılar için tanımlı değildir.");
        if (number <= 1) return 1;
        
        int result = 1;
        for (int i = 2; i <= number; i++)
        {
            result *= i;
        }
        
        return result;
    }
}

// Kullanım
public class Program
{
    public static void Main()
    {
        int number = 7;
        
        // Extension metotları nesne üzerinden çağrılır
        bool isEven = number.IsEven();  // false
        bool isPrime = number.IsPrime(); // true
        int factorial = number.Factorial(); // 5040
        
        Console.WriteLine($"{number} çift mi? {isEven}");
        Console.WriteLine($"{number} asal mı? {isPrime}");
        Console.WriteLine($"{number}! = {factorial}");
    }
}
```

## Özet

C#'ta static ve instance üyelerin farkını anlamak, programınızın yapısını ve davranışını optimum şekilde tasarlamanıza yardımcı olur:

1. **Instance üyeler**:
   - Her nesne için ayrı kopyalanan üyelerdir
   - Nesne durumunu ve davranışını tanımlar
   - Nesne örneği üzerinden erişilir
   - Polymorphism ve interface implementation için kullanılır

2. **Static üyeler**:
   - Sınıfın tüm örnekleri için ortak olan üyelerdir
   - Sınıf düzeyinde davranışları tanımlar
   - Sınıf adı üzerinden doğrudan erişilir
   - Utility metotları, factory metotları, singleton deseni gibi senaryolarda kullanılır

Doğru üye tipini seçmek, aşağıdaki faktörlere bağlıdır:
- Üyenin nesneye mi yoksa sınıfa mı ait olması gerektiği
- Üyenin nesne durumuna bağlı olup olmadığı
- Performans ve bellek verimliliği gereksinimleri
- Kod esnekliği, test edilebilirlik ve bakım kolaylığı

Her iki türün de avantajları ve dezavantajları vardır, bu nedenle her durumda en uygun çözümü seçmek için dikkatli düşünülmelidir.
