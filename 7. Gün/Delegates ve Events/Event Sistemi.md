# C# Event Sistemi Rehberi

## İçindekiler
1. [Event Nedir?](#event-nedir)
2. [Delegeler ve Eventler Arasındaki İlişki](#delegeler-ve-eventler-arasındaki-i̇lişki)
3. [Basit Event Tanımlama](#basit-event-tanımlama)
4. [Event Handler (Olay İşleyici) Tanımlama](#event-handler-olay-i̇şleyici-tanımlama)
5. [Eventi Tetikleme (Raising)](#eventi-tetikleme-raising)
6. [Event Arguments Kullanımı](#event-arguments-kullanımı)
7. [Eventi Abonelikten Çıkarma](#eventi-abonelikten-çıkarma)
8. [Eventlerin Thread Safety Açısından Kullanımı](#eventlerin-thread-safety-açısından-kullanımı)
9. [Event Pattern Best Practices](#event-pattern-best-practices)
10. [Gerçek Dünya Örnekleri](#gerçek-dünya-örnekleri)
11. [Custom Event Accessor'lar](#custom-event-accessorlar)
12. [Özet](#özet)

## Event Nedir?

Event (olay), bir sınıfın başka sınıflara belirli bir olayın gerçekleştiğini bildirme mekanizmasıdır. C#'ta eventler, bir nesnenin durumundaki değişiklikleri diğer nesnelere bildirmek için kullanılan bir Publisher-Subscriber (Yayıncı-Abone) deseni uygular.

Bu modelde:
- **Publisher (Yayıncı)**: Eventi tanımlayan ve tetikleyen sınıftır.
- **Subscriber (Abone)**: Eventi dinleyen ve eventler tetiklendiğinde haberdar olan sınıftır.

## Delegeler ve Eventler Arasındaki İlişki

C#'ta eventler, delegelere dayanır. Bir event tanımlamadan önce, eventi temsil edecek bir delege tipini belirlememiz gerekir.

```csharp
// Delege tanımı
public delegate void MyEventHandler(object sender, EventArgs e);

// Event tanımı
public event MyEventHandler MyEvent;
```

.NET Framework, yaygın event senaryoları için önceden tanımlanmış delege tipleri sağlar:

1. **EventHandler**: Parametre almayan olaylar için
   ```csharp
   public event EventHandler MyEvent;
   ```

2. **EventHandler<TEventArgs>**: Özel veri taşıyan olaylar için
   ```csharp
   public event EventHandler<MyEventArgs> MyEvent;
   ```

## Basit Event Tanımlama

İşte basit bir event tanımlama örneği:

```csharp
public class Button
{
    // Event tanımı
    public event EventHandler Click;

    // Eventi tetikleyen metot
    public void OnClick()
    {
        // Eventi tetikle
        Click?.Invoke(this, EventArgs.Empty);
    }
}
```

Bu örnekte:
- `Button` sınıfı `Click` olayını tanımlar
- `OnClick` metodu eventi tetiklemek için kullanılır
- `Click?.Invoke` yapısı null check işlemi yaparak güvenli bir şekilde eventi tetikler

## Event Handler (Olay İşleyici) Tanımlama

Bir event'e abone olmak için event handler metodları şu şekilde tanımlanır:

```csharp
public class Program
{
    static void Main(string[] args)
    {
        Button button = new Button();
        
        // Eventi dinlemek için abone ol
        button.Click += Button_Click;
        
        // Eventi tetikle
        button.OnClick();
    }
    
    // Event handler metodu
    private static void Button_Click(object sender, EventArgs e)
    {
        Console.WriteLine("Butona tıklandı!");
    }
}
```

Lambda ifadeleri kullanarak da event handler tanımlayabilirsiniz:

```csharp
button.Click += (sender, e) => Console.WriteLine("Butona tıklandı!");
```

## Eventi Tetikleme (Raising)

Eventi tetiklemek (raise etmek), olayın gerçekleştiğini abonelere bildirmek anlamına gelir. Bu işlem şu şekilde yapılır:

```csharp
protected virtual void OnMyEvent(EventArgs e)
{
    // Null kontrol işlemi yaparak eventi tetikle
    MyEvent?.Invoke(this, e);
}
```

C# 6.0 ve üzeri sürümlerde null-conditional operatör (`?.`) kullanımı sayesinde, event'in null olup olmadığını kontrol etmek kolaylaşmıştır.

## Event Arguments Kullanımı

Özel veri taşıyan eventler oluşturmak için önce `EventArgs` sınıfından türetilen bir sınıf tanımlamanız gerekir:

```csharp
// Özel event argument sınıfı
public class MessageEventArgs : EventArgs
{
    public string Message { get; }
    
    public MessageEventArgs(string message)
    {
        Message = message;
    }
}

public class Messenger
{
    // Özel event args kullanan event
    public event EventHandler<MessageEventArgs> MessageReceived;
    
    public void ReceiveMessage(string message)
    {
        // Eventi tetikle ve özel veriyi geçir
        OnMessageReceived(new MessageEventArgs(message));
    }
    
    protected virtual void OnMessageReceived(MessageEventArgs e)
    {
        MessageReceived?.Invoke(this, e);
    }
}
```

Kullanımı:

```csharp
Messenger messenger = new Messenger();

// Eventi dinle
messenger.MessageReceived += (sender, e) => {
    Console.WriteLine($"Mesaj alındı: {e.Message}");
};

// Mesaj gönder
messenger.ReceiveMessage("Merhaba Dünya!");
```

## Eventi Abonelikten Çıkarma

Bir eventi dinlemeyi durdurmak için `-=` operatörü kullanılır:

```csharp
// Event handler tanımla
void Button_Click(object sender, EventArgs e)
{
    Console.WriteLine("Butona tıklandı!");
}

// Eventi dinle
button.Click += Button_Click;

// Eventi dinlemeyi durdur
button.Click -= Button_Click;
```

Bellek sızıntılarını önlemek için, özellikle nesne ömürlerinin farklı olduğu durumlarda eventlerden abonelikten çıkmak önemlidir.

## Eventlerin Thread Safety Açısından Kullanımı

Çoklu iş parçacığı (multithreading) ortamında, eventleri güvenli bir şekilde tetiklemek için ek önlemler almanız gerekebilir:

```csharp
public event EventHandler MyEvent;

protected virtual void OnMyEvent()
{
    // Yerel bir kopyaya atayarak thread safety sağlama
    EventHandler handler = MyEvent;
    
    if (handler != null)
    {
        handler(this, EventArgs.Empty);
    }
}
```

## Event Pattern Best Practices

C# eventlerini kullanırken izlemeniz gereken en iyi pratikler:

1. **İsimlendirme Kuralları**:
   - Event isimleri fiil veya fiil ifadeleri olmalıdır (örn. `Click`, `ValueChanged`)
   - Event handler metodları için "sender" ve "e" parametre isimlerini kullanın

2. **Koruma Metotları**:
   - Eventi tetiklemek için `OnEventName` formatında protected metotlar kullanın
   - Bu metotlar virtual olmalıdır (miras alan sınıfların override etmesine izin vermek için)

3. **Null Kontrolleri**:
   - Event tetiklemeden önce her zaman null kontrolü yapın

4. **Thread Safety**:
   - Çoklu iş parçacığı ortamında event triggering için yerel kopya kullanın

## Gerçek Dünya Örnekleri

### Windows Forms Uygulaması

Windows Forms uygulamalarında eventler yaygın olarak kullanılır:

```csharp
public partial class Form1 : Form
{
    public Form1()
    {
        InitializeComponent();
        
        // Button click olayını dinle
        button1.Click += Button1_Click;
    }
    
    private void Button1_Click(object sender, EventArgs e)
    {
        MessageBox.Show("Butona tıklandı!");
    }
}
```

### Custom Component Geliştirme

```csharp
public class CustomTextBox : TextBox
{
    // Özel event tanımlama
    public event EventHandler<TextChangedEventArgs> TextValidated;
    
    protected override void OnTextChanged(EventArgs e)
    {
        base.OnTextChanged(e);
        
        // Metin değiştiğinde doğrulama yap
        bool isValid = !string.IsNullOrEmpty(Text);
        
        // Özel eventi tetikle
        OnTextValidated(new TextChangedEventArgs(Text, isValid));
    }
    
    protected virtual void OnTextValidated(TextChangedEventArgs e)
    {
        TextValidated?.Invoke(this, e);
    }
}

public class TextChangedEventArgs : EventArgs
{
    public string NewText { get; }
    public bool IsValid { get; }
    
    public TextChangedEventArgs(string newText, bool isValid)
    {
        NewText = newText;
        IsValid = isValid;
    }
}
```

## Custom Event Accessor'lar

C#, eventler için özel erişim belirleyicileri (accessor) tanımlamanıza olanak tanır:

```csharp
private EventHandler _myEvent;

public event EventHandler MyEvent
{
    add
    {
        Console.WriteLine("Event'e abone olundu");
        _myEvent += value;
    }
    remove
    {
        Console.WriteLine("Event aboneliği kaldırıldı");
        _myEvent -= value;
    }
}

protected virtual void OnMyEvent()
{
    _myEvent?.Invoke(this, EventArgs.Empty);
}
```

Bu özellik, event aboneliklerini yönetirken özel davranışlar eklemenize olanak tanır.

## Özet

C# event sistemi:

1. **Publisher-Subscriber Pattern** üzerine kuruludur
2. **Delegeler** kullanılarak tanımlanır
3. **EventArgs** ile özel veri taşınabilir
4. **+=** ve **-=** operatörleri ile event abonelikleri yönetilir
5. **Thread Safety** için özel teknikler gerektirebilir
6. **Bellek Yönetimi** açısından dikkatli kullanılmalıdır

C# event sistemi, modern nesne yönelimli programlamada sınıflar arası iletişimi sağlamak için güçlü ve esnek bir yöntemdir. Bu rehberdeki konseptleri kullanarak, uygulamalarınızda daha modüler ve bakımı kolay kod yapıları oluşturabilirsiniz.
