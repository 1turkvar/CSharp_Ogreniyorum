# C# Event Handler Pattern

## İçindekiler
1. [Giriş](#giriş)
2. [Olaylar ve Delegeler](#olaylar-ve-delegeler)
3. [Event Handler Pattern Temel Bileşenleri](#event-handler-pattern-temel-bileşenleri)
4. [Event Handler Pattern Kullanımı](#event-handler-pattern-kullanımı)
5. [Standart Event Pattern](#standart-event-pattern)
6. [EventArgs Sınıfı](#eventargs-sınıfı)
7. [Event Handler Pattern Örneği](#event-handler-pattern-örneği)
8. [Event Handler Pattern'in Avantajları](#event-handler-patternin-avantajları)
9. [Custom Event Arguments](#custom-event-arguments)
10. [Thread-Safe Event Handling](#thread-safe-event-handling)
11. [Event Handler Pattern Best Practices](#event-handler-pattern-best-practices)
12. [İleri Seviye Konular](#i̇leri-seviye-konular)
13. [Özet](#özet)

## Giriş

C# Event Handler Pattern, bir nesnenin durum değişikliğini diğer nesnelere bildirmesine olanak tanıyan bir tasarım modelidir. Bu model, Publisher-Subscriber (Yayıncı-Abone) tasarım desenine dayanır ve .NET platformunun temel özelliklerinden biridir.

Bu desen sayesinde:
- Nesneler arasında gevşek bağlantı (loose coupling) sağlanır
- Kod modülerliği artar
- Yeniden kullanılabilirlik ve genişletilebilirlik gelişir
- Arayüz tabanlı programlama kolaylaşır

## Olaylar ve Delegeler

C#'ta Event Handler Pattern'in temelinde **delegeler** yer alır. Delegeler, metot referanslarını tutan ve C#'ın fonksiyonel programlama yeteneklerini destekleyen tiplerdir.

### Delege Tanımı

```csharp
// Basit bir delege tanımı
public delegate void MessageHandler(string message);
```

Delegeler, belirli bir imzaya sahip metotları işaret eden referanslardır. C#'taki olaylar (events), temelde delege örneklerinin özel bir türüdür.

### Olay (Event) Tanımı

```csharp
// Bir sınıf içinde olay tanımı
public event MessageHandler MessageReceived;
```

Olaylar, dışarıdan salt abonelik (subscribe) ve abonelikten çıkma (unsubscribe) işlemlerine izin veren, ancak doğrudan tetiklenemeyen delege örnekleridir.

## Event Handler Pattern Temel Bileşenleri

Event Handler Pattern, üç temel bileşenden oluşur:

1. **Publisher (Yayıncı)**: Olayı tanımlayan ve tetikleyen sınıf
2. **Subscriber (Abone)**: Olaya abone olan ve olay gerçekleştiğinde bildirim alan sınıf
3. **Event Arguments (Olay Argümanları)**: Olay hakkında detaylı bilgi taşıyan sınıf

## Event Handler Pattern Kullanımı

### 1. Delege Tanımı

```csharp
// Genel delege tanımı
public delegate void EventHandler(object sender, EventArgs e);
```

### 2. Yayıncı Sınıfta Olay Tanımı

```csharp
public class Publisher
{
    // Olay tanımı
    public event EventHandler SomethingHappened;

    // Olayı tetikleyen metot
    protected virtual void OnSomethingHappened(EventArgs e)
    {
        // Thread-safe kontrol
        EventHandler handler = SomethingHappened;
        if (handler != null)
        {
            handler(this, e);
        }
    }

    // Örnek işlevsellik
    public void DoSomething()
    {
        Console.WriteLine("Doing something...");
        
        // İşlem tamamlandığında olayı tetikle
        OnSomethingHappened(EventArgs.Empty);
    }
}
```

### 3. Abone Sınıfta Olay İşleyici Tanımı

```csharp
public class Subscriber
{
    private Publisher _publisher;

    public Subscriber(Publisher publisher)
    {
        _publisher = publisher;
        
        // Olaya abone ol
        _publisher.SomethingHappened += HandleSomethingHappened;
    }

    // Olay işleyici metodu
    private void HandleSomethingHappened(object sender, EventArgs e)
    {
        Console.WriteLine("Event was handled!");
    }

    // Abonelikten çıkma
    public void Unsubscribe()
    {
        _publisher.SomethingHappened -= HandleSomethingHappened;
    }
}
```

## Standart Event Pattern

C# ve .NET Framework'te standart event pattern şu şekildedir:

1. Olaylar `event` anahtar kelimesi ile tanımlanır
2. Olay handler'lar genellikle iki parametre alır: `object sender` ve `EventArgs e` (veya EventArgs'dan türetilmiş bir sınıf)
3. Olay adları genellikle geçmiş zaman fiil yapısındadır (örn. `ButtonClicked`, `PropertyChanged`)
4. Olayı tetiklemek için genellikle `On` önekli protected virtual metotlar kullanılır (örn. `OnButtonClicked`)

.NET Framework'teki çoğu olay, şu delege tiplerinden birini kullanır:

```csharp
// Parametre almayan olaylar için
public delegate void EventHandler(object sender, EventArgs e);

// Generic olay parametreleri için
public delegate void EventHandler<TEventArgs>(object sender, TEventArgs e);
```

## EventArgs Sınıfı

`EventArgs` sınıfı, olayla ilgili bilgileri taşımak için kullanılır. Basit olaylar için `EventArgs.Empty` kullanılabilir, ancak özel bilgi taşımak gerektiğinde `EventArgs`'tan türetilmiş sınıflar oluşturulur:

```csharp
public class MessageEventArgs : EventArgs
{
    public string Message { get; }
    public DateTime Timestamp { get; }

    public MessageEventArgs(string message)
    {
        Message = message;
        Timestamp = DateTime.Now;
    }
}
```

## Event Handler Pattern Örneği

Aşağıda, button click olayını simüle eden kapsamlı bir örnek bulunmaktadır:

```csharp
using System;

// Özel olay argümanları
public class ButtonClickEventArgs : EventArgs
{
    public int ClickCount { get; }
    public MouseButton Button { get; }

    public ButtonClickEventArgs(int clickCount, MouseButton button)
    {
        ClickCount = clickCount;
        Button = button;
    }
}

public enum MouseButton
{
    Left,
    Right,
    Middle
}

// Publisher sınıfı
public class Button
{
    private int _clickCount = 0;
    private string _name;

    // Olay tanımı (generic EventHandler kullanımı)
    public event EventHandler<ButtonClickEventArgs> ButtonClicked;

    public Button(string name)
    {
        _name = name;
    }

    // Olayı tetikleyen protected virtual metot
    protected virtual void OnButtonClicked(ButtonClickEventArgs e)
    {
        // Thread-safe kontrol
        EventHandler<ButtonClickEventArgs> handler = ButtonClicked;
        handler?.Invoke(this, e);
    }

    // Butona tıklama simülasyonu
    public void Click(MouseButton button = MouseButton.Left)
    {
        _clickCount++;
        Console.WriteLine($"Button '{_name}' was clicked with {button} button.");
        
        // Olayı tetikle
        OnButtonClicked(new ButtonClickEventArgs(_clickCount, button));
    }
}

// Subscriber sınıfı
public class ButtonClickHandler
{
    public void Subscribe(Button button)
    {
        // Lambda expression ile olaya abone olma
        button.ButtonClicked += (sender, e) => {
            Button clickedButton = sender as Button;
            Console.WriteLine($"Button click handled: Click count = {e.ClickCount}, Button = {e.Button}");
        };
    }
}

// Kullanım örneği
public class Program
{
    public static void Main()
    {
        // Publisher oluştur
        Button myButton = new Button("OK Button");
        
        // Subscriber oluştur ve abone et
        ButtonClickHandler handler = new ButtonClickHandler();
        handler.Subscribe(myButton);
        
        // Olayı tetikle
        myButton.Click();
        myButton.Click(MouseButton.Right);
    }
}
```

## Event Handler Pattern'in Avantajları

1. **Gevşek Bağlantı (Loose Coupling)**: Publisher, subscriber'ların kimler olduğunu veya ne yaptığını bilmez
2. **Modülerlik**: Bileşenler birbirlerinden bağımsız olarak geliştirilebilir
3. **Genişletilebilirlik**: Yeni subscriber'lar, publisher'ı değiştirmeden eklenebilir
4. **Kod Tekrarını Önleme**: Aynı olaya birden fazla bileşen tepki verebilir
5. **Temiz Kod**: Sorumlulukların net ayrımı ve daha düzenli kodlama yapısı sağlar

## Custom Event Arguments

Özel ihtiyaçlara yönelik olay argümanları şu şekilde oluşturulabilir:

```csharp
public class OrderProcessedEventArgs : EventArgs
{
    public string OrderId { get; }
    public decimal TotalAmount { get; }
    public OrderStatus Status { get; }
    public DateTime ProcessedDate { get; }

    public OrderProcessedEventArgs(string orderId, decimal totalAmount, OrderStatus status)
    {
        OrderId = orderId;
        TotalAmount = totalAmount;
        Status = status;
        ProcessedDate = DateTime.Now;
    }
}

public enum OrderStatus
{
    Processing,
    Completed,
    Failed,
    Cancelled
}
```

## Thread-Safe Event Handling

Çoklu iş parçacığı (multi-threading) ortamlarında güvenli olay işleme için:

```csharp
public event EventHandler<TEventArgs> MyEvent;

protected virtual void OnMyEvent(TEventArgs e)
{
    // Yerel bir değişkene referansı kopyalama
    EventHandler<TEventArgs> handler = MyEvent;
    
    // Null kontrolü ve çağrı
    if (handler != null)
    {
        handler(this, e);
    }
    
    // C# 6.0 ve sonrası için null-conditional operatör kullanımı
    // MyEvent?.Invoke(this, e);
}
```

## Event Handler Pattern Best Practices

1. **Olayları Thread-Safe Bir Şekilde Tetikleyin**: Yukarıdaki örnekte gösterildiği gibi, olay referansını yerel bir değişkene kopyalayın veya null-conditional operatörü kullanın.

2. **Olayları Protected Virtual Metotlar İle Tetikleyin**: Bu, türetilmiş sınıfların olay tetikleme davranışını özelleştirmesine olanak tanır.

```csharp
protected virtual void OnPropertyChanged(PropertyChangedEventArgs e)
{
    PropertyChanged?.Invoke(this, e);
}
```

3. **Olaylar İçin Anlamlı İsimler Kullanın**: Olaylar genellikle geçmiş zaman fiil olarak adlandırılır (örn. `ButtonClicked`, `PropertyChanged`).

4. **Abonelikten Çıkmayı Unutmayın**: Nesne yok edildiğinde olaylara olan aboneliği kaldırın, aksi takdirde hafıza sızıntıları oluşabilir.

```csharp
public void Dispose()
{
    // Olaylardan abonelikten çıkma
    _publisher.SomethingHappened -= HandleSomethingHappened;
}
```

5. **EventArgs'ı Yeniden Kullanın**: Basit olaylar için `EventArgs.Empty` kullanın; her seferinde yeni bir örnek oluşturmayın.

## İleri Seviye Konular

### 1. Olaylar ve Asenkron Programlama

.NET'te asenkron olay işleme:

```csharp
// Asenkron olay işleyici
private async void HandleEventAsync(object sender, EventArgs e)
{
    await Task.Delay(1000); // Asenkron işlem
    Console.WriteLine("Event handled asynchronously");
}
```

### 2. Weak Event Pattern

Hafıza sızıntılarını önlemek için zayıf referans olay işleyicileri:

```csharp
// WeakEventManager kullanımı (WPF)
WeakEventManager<Publisher, EventArgs>.AddHandler(publisher, 
    "SomethingHappened", HandleSomethingHappened);
```

### 3. Olay Aggregator Pattern

Merkezi olay yönetimi için:

```csharp
// Basit bir olay aggregator
public class EventAggregator
{
    // Olay koleksiyonu
    private Dictionary<string, List<Action<object>>> _events = 
        new Dictionary<string, List<Action<object>>>();

    // Abone olma
    public void Subscribe(string eventName, Action<object> handler)
    {
        if (!_events.ContainsKey(eventName))
            _events[eventName] = new List<Action<object>>();
            
        _events[eventName].Add(handler);
    }

    // Olay tetikleme
    public void Publish(string eventName, object data)
    {
        if (_events.ContainsKey(eventName))
        {
            foreach (var handler in _events[eventName])
            {
                handler(data);
            }
        }
    }
}
```

### 4. Generic Event Pattern

Tip güvenli event handling için generic yaklaşım:

```csharp
public class MessageBus<TMessage>
{
    public event EventHandler<MessageEventArgs<TMessage>> MessageReceived;

    protected virtual void OnMessageReceived(MessageEventArgs<TMessage> e)
    {
        MessageReceived?.Invoke(this, e);
    }

    public void SendMessage(TMessage message)
    {
        OnMessageReceived(new MessageEventArgs<TMessage>(message));
    }
}

public class MessageEventArgs<T> : EventArgs
{
    public T Message { get; }

    public MessageEventArgs(T message)
    {
        Message = message;
    }
}
```

## Özet

C#'taki Event Handler Pattern:

1. **Temel bileşenler**: Publisher, Subscriber ve Event Arguments
2. **İmplementasyon**: `event` anahtar kelimesi ve delegeler kullanılarak
3. **Standart yapı**: `EventHandler` ve `EventHandler<T>` delegeleri
4. **Avantajlar**: Gevşek bağlantı, modülerlik, genişletilebilirlik
5. **Best Practices**: Thread-safe tetikleme, protected virtual metotlar, anlamlı isimlendirme

Bu pattern, .NET ekosisteminde yaygın olarak kullanılır ve Windows Forms, WPF, ASP.NET, Xamarin ve MAUI gibi birçok framework'ün temelini oluşturur.

Event Handler Pattern, nesneler arasında etkin iletişim kurmanın ve temiz, bakımı kolay kod yazmanın güçlü bir yoludur. İyi tasarlanmış bir olay sistemi, uygulamanızın farklı bileşenleri arasında veri ve bildirim akışını organize etmenize yardımcı olur, bu da daha esnek ve sürdürülebilir uygulamalar oluşturmanıza olanak tanır.
