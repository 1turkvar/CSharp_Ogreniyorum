# C# Multicast Delegates - Kapsamlı Rehber

## İçindekiler
- [Delegate'ler Nedir?](#delegateler-nedir)
- [Multicast Delegate'ler](#multicast-delegateler)
- [Multicast Delegate'lerin Oluşturulması](#multicast-delegatelerin-oluşturulması)
- [Delegate Zinciri Yapısı](#delegate-zinciri-yapısı)
- [Delegate İşlemleri](#delegate-i̇şlemleri)
  - [Delegate Ekleme (+=)](#delegate-ekleme-)
  - [Delegate Çıkarma (-=)](#delegate-çıkarma--)
- [Çağrılma Sırası](#çağrılma-sırası)
- [Dönüş Değerleri](#dönüş-değerleri)
- [Multicast Delegate'lerin Avantajları](#multicast-delegatelerin-avantajları)
- [Olay (Event) İlişkisi](#olay-event-i̇lişkisi)
- [En İyi Kullanım Pratikleri](#en-i̇yi-kullanım-pratikleri)
- [Gerçek Dünya Örnekleri](#gerçek-dünya-örnekleri)
- [Yaygın Hatalar ve Çözümleri](#yaygın-hatalar-ve-çözümleri)
- [İleri Seviye Konular](#i̇leri-seviye-konular)

## Delegate'ler Nedir?

Delegate'ler, C# programlama dilinde metotları temsil eden ve onları birer nesne olarak işleyen yapılardır. Delegate'ler, metotlara referans olarak davranır ve bu referansları kullanarak metotları çağırabilirsiniz. Temel olarak, delegate'ler metotları parametre olarak geçirmenize, değişkenlerde saklamanıza ve dinamik olarak çağırmanıza olanak tanır.

Bir delegate tanımı şu şekilde yapılır:

```csharp
public delegate int MathOperation(int x, int y);
```

Yukarıdaki tanım, iki integer parametre alan ve integer döndüren metotları temsil eden bir delegate tanımıdır.

## Multicast Delegate'ler

Multicast delegate'ler, birden fazla metot referansını içerebilen ve bu metotları sırayla çağırabilen özel delegate türleridir. C#'ta tüm delegate'ler `System.MulticastDelegate` sınıfından türetilir ve bu nedenle tüm delegate'ler multicast özelliğine sahiptir.

Multicast delegate'lerin en önemli özelliği, bir delegate örneğine birden çok metot bağlayabilmeniz ve bu metotların hepsini tek bir çağrı ile sırayla çalıştırabilmenizdir.

## Multicast Delegate'lerin Oluşturulması

Bir multicast delegate oluşturmak için standart delegate tanımını kullanabilirsiniz:

```csharp
public delegate void NotifyHandler(string message);
```

Daha sonra bu delegate'e birden fazla metot ekleyebilirsiniz:

```csharp
public class NotificationSystem
{
    public void SendEmail(string message)
    {
        Console.WriteLine($"Email gönderiliyor: {message}");
    }
    
    public void SendSMS(string message)
    {
        Console.WriteLine($"SMS gönderiliyor: {message}");
    }
    
    public void LogMessage(string message)
    {
        Console.WriteLine($"Log kaydı oluşturuluyor: {message}");
    }
    
    public void TestMulticastDelegate()
    {
        // Delegate örneği oluşturuluyor
        NotifyHandler notifier = SendEmail;
        
        // Diğer metotları delegate'e ekleme
        notifier += SendSMS;
        notifier += LogMessage;
        
        // Tüm metotları tek bir çağrı ile çalıştırma
        notifier("Önemli bildirim");
    }
}
```

Yukarıdaki kod çalıştırıldığında, şu çıktıyı üretecektir:

```
Email gönderiliyor: Önemli bildirim
SMS gönderiliyor: Önemli bildirim
Log kaydı oluşturuluyor: Önemli bildirim
```

## Delegate Zinciri Yapısı

Multicast delegate'ler içerisinde, metot referansları bir zincir yapısında saklanır. Her bir delegate örneği, kendisine eklenmiş tüm metotların referanslarını muhafaza eder. Bu yapı, delegate zinciri (delegate chain) olarak adlandırılır.

```
notifier → SendEmail → SendSMS → LogMessage
```

Bu zincir yapısı sayesinde, delegate çağrıldığında tüm metotlar eklenme sırasına göre çağrılır.

## Delegate İşlemleri

### Delegate Ekleme (+=)

Bir multicast delegate'e yeni bir metot eklemek için `+=` operatörü kullanılır:

```csharp
notifier += SendSMS;
```

### Delegate Çıkarma (-=)

Bir multicast delegate'den bir metodu çıkarmak için `-=` operatörü kullanılır:

```csharp
notifier -= SendSMS;
```

Eğer çıkarılmak istenen metot delegate zincirinde birden fazla kez bulunuyorsa, sadece zincirleme sırasında bulunan en sondaki metot referansı kaldırılır.

## Çağrılma Sırası

Multicast delegate'lere eklenen metotlar, eklenme sıralarına göre çağrılır. İlk eklenen metot ilk çağrılır, son eklenen metot son çağrılır. Bu nedenle, metotların eklenme sırası önemlidir.

```csharp
NotifyHandler notifier = null;
notifier += Method1; // İlk çağrılacak
notifier += Method2; // İkinci çağrılacak
notifier += Method3; // Üçüncü çağrılacak
```

## Dönüş Değerleri

Multicast delegate'lerin önemli bir özelliği, dönüş değerleri ile ilgilidir. Eğer bir multicast delegate dönüş değeri olan bir metot türünü temsil ediyorsa, delegate çağrıldığında **sadece en son çağrılan metodun dönüş değeri** elde edilir. Önceki metotların dönüş değerleri kaybolur:

```csharp
public delegate int Calculator(int x, int y);

public class MathOperations
{
    public int Add(int x, int y) { return x + y; }
    public int Multiply(int x, int y) { return x * y; }
    
    public void TestMulticastReturns()
    {
        Calculator calc = Add;      // Dönüş değeri: x + y
        calc += Multiply;           // Dönüş değeri: x * y
        
        int result = calc(5, 3);    // result = 15 (5 * 3), Add metodu da çağrılır ama dönüş değeri kaybolur
        Console.WriteLine(result);  // Çıktı: 15
    }
}
```

Bu nedenle, dönüş değeri olan multicast delegate'leri kullanırken dikkatli olmak gerekir. Genellikle multicast delegate'ler `void` dönüş tipinde metotlar için daha uygundur.

## Multicast Delegate'lerin Avantajları

1. **Gevşek Bağlantı (Loose Coupling)**: Farklı bileşenler arasında doğrudan bağımlılık olmadan iletişim kurulabilir.
2. **Dinamik Metot Çağrıları**: Çalışma zamanında dinamik olarak metot listesi oluşturulabilir ve değiştirilebilir.
3. **Olay Tabanlı Programlama**: Olay tabanlı mimarilerde, olaylara çoklu işleyiciler (handlers) eklemek için idealdir.
4. **Kolayca Genişletilebilir**: Yeni metotlar, mevcut kodu değiştirmeden eklenebilir veya çıkarılabilir.
5. **Gözlemci Tasarım Deseni**: Gözlemci (Observer) tasarım deseninin implementasyonu için mükemmel bir yapıdır.

## Olay (Event) İlişkisi

C#'ta olaylar (events), multicast delegate'lerin üzerine inşa edilmiştir. Olaylar, multicast delegate'lerin özelleştirilmiş bir formudur ve abonelik (subscription) modeliyle çalışır:

```csharp
public class Publisher
{
    // Event tanımı (delegate tipi üzerine kurulu)
    public event NotifyHandler MessageReceived;
    
    public void ProcessMessage(string message)
    {
        // Event'i tetikleme
        MessageReceived?.Invoke(message);
    }
}

public class Subscriber
{
    public void Initialize(Publisher publisher)
    {
        // Event'e abone olma
        publisher.MessageReceived += HandleMessage;
    }
    
    public void HandleMessage(string message)
    {
        Console.WriteLine($"Mesaj alındı: {message}");
    }
}
```

Olaylar, multicast delegate'lerin aşağıdaki özelliklerini kısıtlar:
- Olay tanımlayan sınıf dışından `=` operatörü ile doğrudan atama yapılamaz, sadece `+=` ve `-=` operatörleri kullanılabilir.
- Olay tanımlayan sınıf dışından tetiklenemez (invoke edilemez).

## En İyi Kullanım Pratikleri

1. **Null Kontrolü**: Delegate çağırmadan önce her zaman null kontrolü yapın:
   ```csharp
   notifier?.Invoke("Mesaj");
   ```

2. **Thread Safety**: Multicast delegate'ler üzerinde yapılan işlemler atomik değildir. Çoklu thread ortamlarında delegate'leri manipüle ederken thread-safe teknikler kullanın.

3. **Dönüş Değerleri**: Dönüş değeri olan multicast delegate'leri dikkatli kullanın, sadece son çağrılan metodun dönüş değerini alırsınız.

4. **Exception Handling**: Bir multicast delegate içindeki bir metot exception fırlatırsa, sonraki metotlar çağrılmaz. Her metodu kendi içinde try-catch bloklarıyla koruyun:
   ```csharp
   foreach (NotifyHandler handler in notifier.GetInvocationList())
   {
       try
       {
           handler("Mesaj");
       }
       catch (Exception ex)
       {
           Console.WriteLine($"Hata: {ex.Message}");
       }
   }
   ```

5. **Delegate'leri Event Olarak Kullanın**: Birçok durumda, doğrudan delegate kullanmak yerine event anahtar kelimesini kullanmak daha güvenlidir.

## Gerçek Dünya Örnekleri

### GUI Olay İşleyicileri

Windows Forms veya WPF gibi GUI uygulamalarında, buton tıklama gibi olaylar multicast delegate'ler aracılığıyla yönetilir:

```csharp
// Buton tıklama olayına birden fazla işleyici eklenebilir
button1.Click += Button1_Click;
button1.Click += LogButtonClick;
button1.Click += UpdateStatistics;
```

### Logger Sistemi

Farklı loglama hedeflerini destekleyen bir logger sistemi:

```csharp
public delegate void LogHandler(string message, LogLevel level);

public class Logger
{
    private LogHandler _logHandlers;
    
    public void AddLogTarget(LogHandler handler)
    {
        _logHandlers += handler;
    }
    
    public void RemoveLogTarget(LogHandler handler)
    {
        _logHandlers -= handler;
    }
    
    public void Log(string message, LogLevel level)
    {
        _logHandlers?.Invoke(message, level);
    }
}

// Kullanım
Logger logger = new Logger();
logger.AddLogTarget(ConsoleLogger);
logger.AddLogTarget(FileLogger);
logger.AddLogTarget(DatabaseLogger);

logger.Log("Uygulama başlatıldı", LogLevel.Info);
```

### Bildirim Sistemi

Farklı bildirim kanallarını destekleyen bir sistem:

```csharp
public class NotificationSystem
{
    public delegate void NotificationHandler(string title, string message, NotificationPriority priority);
    
    private NotificationHandler _notifiers;
    
    public void Subscribe(NotificationHandler handler)
    {
        _notifiers += handler;
    }
    
    public void Unsubscribe(NotificationHandler handler)
    {
        _notifiers -= handler;
    }
    
    public void NotifyAll(string title, string message, NotificationPriority priority)
    {
        _notifiers?.Invoke(title, message, priority);
    }
}
```

## Yaygın Hatalar ve Çözümleri

### 1. Null Reference Exception

**Hata**: Delegate'i çağırmadan önce null kontrolü yapmamak.

**Çözüm**: Delegate'i çağırmadan önce her zaman null kontrolü yapın veya C# 6.0 ve üzeri sürümlerde null-conditional operatörünü (`?.`) kullanın:

```csharp
notifier?.Invoke("Mesaj");
```

### 2. Thread Safety Sorunları

**Hata**: Çoklu thread ortamında delegate manipülasyonu güvenli değildir.

**Çözüm**: Delegate atamalarını bir lock bloğu içinde yapın veya Interlocked.CompareExchange kullanın:

```csharp
private NotifyHandler _handler;
private readonly object _lock = new object();

public void AddHandler(NotifyHandler handler)
{
    lock (_lock)
    {
        _handler += handler;
    }
}
```

### 3. Dönüş Değerlerinin Kaybı

**Hata**: Multicast delegate'lerde sadece son metodun dönüş değeri korunur.

**Çözüm**: Dönüş değerlerini toplamak için explicit olarak her delegeyi çağırın:

```csharp
public List<int> GetAllResults(Calculator calc, int x, int y)
{
    List<int> results = new List<int>();
    
    foreach (Calculator operation in calc.GetInvocationList())
    {
        results.Add(operation(x, y));
    }
    
    return results;
}
```

### 4. Exception Yönetimi

**Hata**: Bir metot hata fırlattığında, diğer metotlar çağrılmaz.

**Çözüm**: Explicit olarak her delegeyi çağırıp try-catch bloklarıyla koruyun:

```csharp
public void SafeInvoke(NotifyHandler handler, string message)
{
    if (handler == null) return;
    
    foreach (NotifyHandler individualHandler in handler.GetInvocationList())
    {
        try
        {
            individualHandler(message);
        }
        catch (Exception ex)
        {
            Console.WriteLine($"Handler hatası: {ex.Message}");
            // Loglama veya diğer error handling işlemleri
        }
    }
}
```

## İleri Seviye Konular

### Func ve Action Delegate'leri

C# 3.0 ve üzeri sürümlerde, generic Func<> ve Action<> delegate türleri kullanılabilir:

```csharp
// 2 parametre alan ve int döndüren metotlar için delegate
Func<int, int, int> mathOperation = Add;
mathOperation += Multiply;

// 1 parametre alan ve değer döndürmeyen metotlar için delegate
Action<string> notifier = SendEmail;
notifier += SendSMS;
```

### Anonim Metotlar ve Lambda İfadeleri

Delegate'ler, anonim metotlar veya lambda ifadeleri ile de kullanılabilir:

```csharp
// Anonim metot
notifier += delegate(string message) {
    Console.WriteLine($"Anonim metot: {message}");
};

// Lambda ifadesi
notifier += (message) => Console.WriteLine($"Lambda: {message}");
```

### Async Delegate'ler

Async metotları delegate'lere bağlayabilirsiniz:

```csharp
public delegate Task AsyncNotifyHandler(string message);

public async Task SendEmailAsync(string message)
{
    await Task.Delay(100); // Email gönderme işlemini simüle ediyoruz
    Console.WriteLine($"Email gönderildi: {message}");
}

// Kullanım
AsyncNotifyHandler asyncNotifier = SendEmailAsync;
await asyncNotifier("Async bildirim");
```

### Custom Delegate Çağrısı (GetInvocationList)

Delegate'lerin `GetInvocationList()` metodu, bir multicast delegate içindeki tüm tekil delegate'lerin bir listesini döndürür. Bu, her metodu ayrı ayrı çağırmak ve sonuçlarını işlemek için kullanışlıdır:

```csharp
public delegate int Calculator(int x, int y);

public void ProcessAllResults(Calculator calc, int x, int y)
{
    foreach (Delegate del in calc.GetInvocationList())
    {
        Calculator singleCalc = (Calculator)del;
        int result = singleCalc(x, y);
        Console.WriteLine($"Sonuç: {result}");
    }
}
```

Multicast delegate'ler, C#'ta event tabanlı programlama için temel yapı taşlarından biridir ve doğru kullanıldığında kodunuzu daha modüler, esnek ve genişletilebilir hale getirebilir.
