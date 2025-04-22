# C# Asenkron Programlama Temel Kavramları

## İçindekiler
1. [Giriş](#giriş)
2. [Senkron ve Asenkron Programlama Arasındaki Farklar](#senkron-ve-asenkron-programlama-arasındaki-farklar)
3. [Asenkron Programlamanın Avantajları](#asenkron-programlamanın-avantajları)
4. [Task ve Task\<T> Sınıfları](#task-ve-taskt-sınıfları)
5. [async ve await Anahtar Kelimeleri](#async-ve-await-anahtar-kelimeleri)
6. [Asenkron Metot Tanımlama](#asenkron-metot-tanımlama)
7. [Asenkron İşlemleri İptal Etme](#asenkron-işlemleri-iptal-etme)
8. [Hata Yönetimi](#hata-yönetimi)
9. [Paralel Programlama ile Asenkron Programlama Arasındaki Farklar](#paralel-programlama-ile-asenkron-programlama-arasındaki-farklar)
10. [Yaygın Asenkron Desenler ve Pratikler](#yaygın-asenkron-desenler-ve-pratikler)
11. [Yaygın Hata ve Tuzaklar](#yaygın-hata-ve-tuzaklar)
12. [Örnek Uygulamalar](#örnek-uygulamalar)
13. [Kaynaklar ve İleri Okumalar](#kaynaklar-ve-ileri-okumalar)

## Giriş

Asenkron programlama, modern yazılım geliştirmenin önemli bir parçasıdır ve özellikle kullanıcı arayüzlerini duyarlı tutmak, sistem kaynaklarını verimli kullanmak ve yoğun I/O işlemlerini etkili bir şekilde yönetmek için kritik öneme sahiptir. C#, .NET Framework 4.5 ve sonraki sürümlerde geliştirilmiş ve kolaylaştırılmış asenkron programlama yetenekleri sunar.

Bu dokümanda, C# dilinde asenkron programlamanın temel kavramlarını, tekniklerini ve en iyi uygulamalarını inceleyeceğiz.

## Senkron ve Asenkron Programlama Arasındaki Farklar

### Senkron (Eşzamanlı) Programlama

Senkron programlamada, bir işlem tamamlanmadan diğerine geçilmez. Her işlem sırayla çalıştırılır:

```csharp
public void SenkronMetot()
{
    // İşlem 1
    var veri = DosyadanVeriOku(); // Bu işlem tamamlanana kadar bekler
    
    // İşlem 2
    VeriIsle(veri); // İşlem 1 tamamlandıktan sonra başlar
    
    // İşlem 3
    VeriyiKaydet(); // İşlem 2 tamamlandıktan sonra başlar
}
```

### Asenkron (Eşzamansız) Programlama

Asenkron programlamada, bir işlem başlatılır ve sonucu beklenmeden diğer işlemlere geçilebilir. İşlemin tamamlanması beklenirken diğer işlemler çalıştırılabilir:

```csharp
public async Task AsenkronMetot()
{
    // İşlem 1
    var veriTask = DosyadanVeriOkuAsync(); // İşlem başlatılır, sonuç beklemeden devam edilebilir
    
    // Başka işlemler yapılabilir...
    
    // İşlem 1'in sonucu gerektiğinde beklenebilir
    var veri = await veriTask;
    
    // İşlem 2
    await VeriIsleAsync(veri);
    
    // İşlem 3
    await VeriyiKaydetAsync();
}
```

## Asenkron Programlamanın Avantajları

1. **Uygulama Duyarlılığı**: Özellikle UI uygulamalarında, uzun süren işlemlerin arayüzü bloke etmesini önler.

2. **Kaynak Verimliliği**: I/O bağlantılı işlemlerde (ağ, dosya, veritabanı) CPU'nun boş bekleme süresi azalır.

3. **Ölçeklenebilirlik**: Sunucu uygulamalarında daha fazla eşzamanlı istek işleme kapasitesi sağlar.

4. **Daha İyi Kullanıcı Deneyimi**: Kullanıcıların beklemesini azaltarak daha akıcı bir deneyim sunar.

5. **Deadlock Riskini Azaltma**: Doğru kullanıldığında, senkron programlamada sık görülen kilitlenme sorunlarını azaltır.

## Task ve Task\<T> Sınıfları

C# asenkron programlamada, `Task` ve `Task<T>` sınıfları asenkron operasyonları temsil eder.

### Task Sınıfı

`Task` sınıfı, bir dönüş değeri olmayan asenkron işlemleri temsil eder:

```csharp
public Task VeriKaydetAsync()
{
    return Task.Run(() => 
    {
        // Veri kaydetme işlemleri
    });
}
```

### Task\<T> Sınıfı

`Task<T>` sınıfı, bir T tipinde dönüş değeri olan asenkron işlemleri temsil eder:

```csharp
public Task<List<Musteri>> MusterileriGetirAsync()
{
    return Task.Run(() => 
    {
        // Müşteri listesini getirme işlemleri
        return musteriListesi;
    });
}
```

### Task Oluşturma Yöntemleri

1. **Task.Run**: CPU bağımlı işleri arka planda çalıştırmak için kullanılır.

```csharp
Task<int> hesaplamaTask = Task.Run(() => KarmasikHesaplama());
```

2. **TaskCompletionSource**: Manuel olarak tamamlanabilen Task nesneleri için.

```csharp
var tcs = new TaskCompletionSource<string>();
// Daha sonra
tcs.SetResult("İşlem tamamlandı");
Task<string> task = tcs.Task;
```

3. **Task.FromResult**: Hazır bir değeri Task olarak döndürmek için.

```csharp
Task<int> task = Task.FromResult(42);
```

4. **Task.Delay**: Belirli bir süre beklemek için.

```csharp
await Task.Delay(1000); // 1 saniye bekle
```

## async ve await Anahtar Kelimeleri

`async` ve `await` anahtar kelimeleri, C#'ta asenkron programlamayı kolaylaştıran temel yapı taşlarıdır.

### async Anahtar Kelimesi

`async` anahtar kelimesi, bir metodun asenkron olduğunu belirtir. Bu anahtar kelimeyle işaretlenen bir metot, içinde `await` ifadelerini kullanabilir:

```csharp
public async Task<string> VeriGetirAsync()
{
    // Metot içeriği
}
```

### await Anahtar Kelimesi

`await` anahtar kelimesi, bir `Task` veya `Task<T>` nesnesinin tamamlanmasını beklemek için kullanılır. `await` kullanıldığında, metot o noktada yürütmeyi durdurur, kontrol çağıran metoda geri döner ve Task tamamlandığında yürütme await ifadesinden sonra devam eder:

```csharp
public async Task<string> VeriIsleAsync()
{
    var veri = await VeriGetirAsync(); // VeriGetirAsync tamamlanana kadar bekle
    return veri.ToUpper(); // Task tamamlandığında buradan devam et
}
```

## Asenkron Metot Tanımlama

Asenkron metotlar genellikle aşağıdaki kurallara uygun olarak tanımlanır:

1. Metot `async` anahtar kelimesiyle işaretlenir.
2. Dönüş türü genellikle `Task` veya `Task<T>` olur.
3. Metot adı genellikle "Async" son ekiyle biter.

```csharp
// Dönüş değeri olmayan asenkron metot
public async Task VeriIsleAsync()
{
    await Task.Delay(1000); // 1 saniye bekle
    // İşlemler
}

// Dönüş değeri olan asenkron metot
public async Task<List<Musteri>> MusterileriGetirAsync()
{
    await Task.Delay(1000); // 1 saniye bekle
    // İşlemler
    return musteriListesi;
}
```

### Asenkron Metotlarda Dönüş Değerleri

Asenkron bir metottan dönüş değeri döndürmek istiyorsanız, `Task<T>` kullanmalısınız:

```csharp
public async Task<int> ToplaAsync(int a, int b)
{
    await Task.Delay(100); // Simüle edilmiş işlem
    return a + b;
}
```

Çağıran tarafta, sonucu almak için await kullanılır:

```csharp
int sonuc = await ToplaAsync(5, 3);
Console.WriteLine(sonuc); // 8
```

## Asenkron İşlemleri İptal Etme

Asenkron işlemleri iptal etmek için `CancellationToken` kullanılır:

### CancellationToken Kullanımı

```csharp
public async Task VeriIndirAsync(string url, CancellationToken cancellationToken)
{
    using (HttpClient client = new HttpClient())
    {
        // İptal kontrolü
        cancellationToken.ThrowIfCancellationRequested();
        
        // Asenkron işlemi başlat ve iptal tokenını ilet
        var response = await client.GetAsync(url, cancellationToken);
        
        // Daha fazla işlem...
    }
}
```

### CancellationToken Oluşturma ve İptal Etme

```csharp
// CancellationTokenSource oluştur
var cts = new CancellationTokenSource();

try
{
    // Asenkron işlemi başlat ve token'ı ilet
    Task indirmeTask = VeriIndirAsync("https://example.com/data", cts.Token);
    
    // Belirli bir süre sonra veya bir tuşa basıldığında iptal et
    if (Console.KeyAvailable && Console.ReadKey().Key == ConsoleKey.Escape)
    {
        cts.Cancel();
    }
    
    await indirmeTask;
}
catch (OperationCanceledException)
{
    Console.WriteLine("İşlem iptal edildi.");
}
finally
{
    cts.Dispose(); // CancellationTokenSource'u temizle
}
```

## Hata Yönetimi

Asenkron programlamada hata yönetimi, senkron programlamaya benzer şekilde try-catch-finally blokları ile yapılır:

```csharp
public async Task AsenkronIslemAsync()
{
    try
    {
        await RiskliBirIslemAsync();
        // Normal işlem akışı
    }
    catch (SpecificException ex)
    {
        // Belirli bir hata türü için özel işleme
        Console.WriteLine($"Özel hata: {ex.Message}");
    }
    catch (Exception ex)
    {
        // Genel hata yakalama
        Console.WriteLine($"Hata oluştu: {ex.Message}");
        throw; // Hatayı yukarı ilet (gerekirse)
    }
    finally
    {
        // Her durumda çalışacak temizleme kodu
        // (kaynakları serbest bırakma vb.)
    }
}
```

### Yaygın Hata Tipleri

1. **OperationCanceledException**: İşlem iptal edildiğinde fırlatılır.
2. **TaskCanceledException**: Task iptal edildiğinde fırlatılır (OperationCanceledException'dan türetilmiştir).
3. **AggregateException**: Birden fazla Task çalıştırılırken hatalar oluştuğunda, tüm hataları içeren bir kapsayıcı olarak kullanılır.

### ConfigureAwait Kullanımı

`ConfigureAwait(false)` SynchronizationContext'e dönmeyi engeller ve performansı artırabilir:

```csharp
public async Task PerformansliMetotAsync()
{
    // SynchronizationContext'e dönme
    await Task.Delay(100).ConfigureAwait(false);
    
    // Bu kod artık orijinal context'te çalışmayacak
}
```

## Paralel Programlama ile Asenkron Programlama Arasındaki Farklar

### Asenkron Programlama
- **Amaç**: I/O bağımlı işlemlerin verimli bir şekilde yönetilmesi
- **Odak Noktası**: Kaynakların bloke edilmesini önlemek
- **Araçlar**: `async`, `await`, `Task`, `Task<T>`
- **Kullanım Alanları**: Ağ istekleri, dosya işlemleri, veritabanı sorguları

### Paralel Programlama
- **Amaç**: CPU bağımlı işlemleri birden fazla çekirdekte çalıştırarak performansı artırmak
- **Odak Noktası**: İşlemleri eş zamanlı olarak birden fazla iş parçacığında çalıştırmak
- **Araçlar**: `Parallel.For`, `Parallel.ForEach`, `Task.Run` ile ThreadPool
- **Kullanım Alanları**: Veri işleme, hesaplamalar, simülasyonlar

### Paralel Programlama Örneği

```csharp
public void ParalelVeriIsle(List<int> veriler)
{
    Parallel.ForEach(veriler, veri =>
    {
        // Her veri öğesi farklı bir thread'de işlenir
        var sonuc = KarmasikHesaplama(veri);
    });
}
```

## Yaygın Asenkron Desenler ve Pratikler

### 1. Asenkron İşlemleri Birleştirme

#### Sıralı İşlem

```csharp
public async Task SiraliIslemAsync()
{
    var veri = await VeriGetirAsync();
    var islenmisVeri = await VeriIsleAsync(veri);
    await VeriKaydetAsync(islenmisVeri);
}
```

#### Paralel İşlem

```csharp
public async Task ParalelIslemAsync()
{
    Task<Data> veri1Task = Veri1GetirAsync();
    Task<Data> veri2Task = Veri2GetirAsync();
    
    // İki işlemi paralel başlat ve her ikisinin de tamamlanmasını bekle
    await Task.WhenAll(veri1Task, veri2Task);
    
    // Sonuçları kullan
    var veri1 = await veri1Task;
    var veri2 = await veri2Task;
}
```

#### İlk Tamamlanan İşlemi Kullanma

```csharp
public async Task IlkTamamlananIslemiKullanAsync()
{
    Task<string> islem1 = VeriGetirAsync("kaynak1");
    Task<string> islem2 = VeriGetirAsync("kaynak2");
    
    // İlk tamamlanan işlemi al
    string sonuc = await Task.WhenAny(islem1, islem2)
                         .ContinueWith(t => t.Result.Result);
    
    Console.WriteLine($"İlk tamamlanan sonuç: {sonuc}");
}
```

### 2. Asenkron Akış İşleme (IAsyncEnumerable)

.NET Core 3.0 ve sonrasında, asenkron akışları işlemek için IAsyncEnumerable kullanılabilir:

```csharp
public async IAsyncEnumerable<int> AsenkronSayiUretAsync()
{
    for (int i = 0; i < 10; i++)
    {
        await Task.Delay(100); // Her sayı arasında bekleme
        yield return i;
    }
}

public async Task AsenkronAkisKullanAsync()
{
    await foreach (var sayi in AsenkronSayiUretAsync())
    {
        Console.WriteLine(sayi);
    }
}
```

### 3. ValueTask Kullanımı

Performans kritik durumlarda, heap tahsisini azaltmak için `ValueTask` kullanılabilir:

```csharp
public ValueTask<int> HizliAsenkronMetot(bool cacheDenKullan)
{
    if (cacheDenKullan)
    {
        // Hemen sonuç dönebiliyorsak, Task oluşturma maliyetinden kaçınırız
        return new ValueTask<int>(42);
    }
    
    // Gerçekten asenkron çalışması gerekiyorsa Task oluştur
    return new ValueTask<int>(YavasBirIslemAsync());
}
```

## Yaygın Hata ve Tuzaklar

### 1. async void Kullanımı

async void metotlar hata yönetimi sorunlarına yol açabilir ve test edilmesi zordur:

```csharp
// KÖTÜ KULLANIM
public async void HataliAsync()
{
    await Task.Delay(100);
    throw new Exception("Bu hata yakalanmayacak!");
}

// DOĞRU KULLANIM
public async Task DuzgunAsync()
{
    await Task.Delay(100);
    throw new Exception("Bu hata yakalanabilir");
}
```

async void yalnızca olay işleyicileri için kullanılmalıdır:

```csharp
// Olay işleyicide kullanımı kabul edilebilir
private async void Button_Click(object sender, EventArgs e)
{
    try
    {
        await DuzgunAsync();
    }
    catch (Exception ex)
    {
        // Hata işleme
    }
}
```

### 2. Deadlock Sorunları

UI uygulamalarında await kullanmadan .Result veya .Wait() çağrılması deadlock'a neden olabilir:

```csharp
// DEADLOCK'A YOL AÇABİLİR
public void DeadlockOrnegi()
{
    var task = AsenkronIslemAsync();
    var sonuc = task.Result; // Bu noktada kilitlenme olabilir
}

public async Task<int> AsenkronIslemAsync()
{
    await Task.Delay(1000);
    return 42;
}
```

### 3. await Kullanmayı Unutmak

Bir Task döndüren metodu await olmadan çağırmak, işlemin tamamlanmasını beklemez:

```csharp
// YANLIŞ - await unutulmuş
public async Task YanlisKullanimAsync()
{
    VeriKaydetAsync(); // İşlem başlar ama tamamlanması beklenmez
    Console.WriteLine("Veri kaydedildi"); // Bu yanlış olabilir!
}

// DOĞRU
public async Task DuzgunKullanimAsync()
{
    await VeriKaydetAsync(); // İşlem tamamlanana kadar bekle
    Console.WriteLine("Veri kaydedildi"); // Bu doğru
}
```

### 4. Using İfadelerini Düzgün Kullanmamak

IDisposable nesnelerle çalışırken using ifadelerini doğru kullanmak önemlidir:

```csharp
// YANLIŞ - using içinde await kullanıldığında potansiyel sorun
public async Task YanlisUsingAsync()
{
    using (var client = new HttpClient())
    {
        var response = await client.GetAsync("https://example.com");
        // client.Dispose() çağrılabilir ama garanti değil
    }
}

// DOĞRU - C# 8.0+ için using ifadesi
public async Task DuzgunUsingAsync()
{
    using var client = new HttpClient();
    var response = await client.GetAsync("https://example.com");
    // Metot sonunda client.Dispose() otomatik çağrılır
}
```

## Örnek Uygulamalar

### 1. Asenkron Web İsteği

```csharp
public async Task<string> WebSayfasiniGetirAsync(string url)
{
    using var client = new HttpClient();
    try
    {
        HttpResponseMessage response = await client.GetAsync(url);
        response.EnsureSuccessStatusCode();
        string icerik = await response.Content.ReadAsStringAsync();
        return icerik;
    }
    catch (HttpRequestException e)
    {
        Console.WriteLine($"İstek hatası: {e.Message}");
        throw;
    }
}

// Kullanım
public async Task WebSayfasiniGosterAsync()
{
    try
    {
        string icerik = await WebSayfasiniGetirAsync("https://example.com");
        Console.WriteLine(icerik.Substring(0, 100) + "...");
    }
    catch (Exception ex)
    {
        Console.WriteLine($"Hata: {ex.Message}");
    }
}
```

### 2. Asenkron Dosya İşlemleri

```csharp
public async Task<string> DosyaOkuAsync(string dosyaYolu)
{
    try
    {
        using FileStream fileStream = new FileStream(dosyaYolu, FileMode.Open, FileAccess.Read, FileShare.Read, 4096, true);
        using StreamReader reader = new StreamReader(fileStream);
        return await reader.ReadToEndAsync();
    }
    catch (Exception ex)
    {
        Console.WriteLine($"Dosya okuma hatası: {ex.Message}");
        throw;
    }
}

public async Task DosyaYazAsync(string dosyaYolu, string icerik)
{
    try
    {
        using FileStream fileStream = new FileStream(dosyaYolu, FileMode.Create, FileAccess.Write, FileShare.None, 4096, true);
        using StreamWriter writer = new StreamWriter(fileStream);
        await writer.WriteAsync(icerik);
    }
    catch (Exception ex)
    {
        Console.WriteLine($"Dosya yazma hatası: {ex.Message}");
        throw;
    }
}
```

### 3. Çoklu API İsteği

```csharp
public async Task<(Weather weather, News news)> GetDashboardDataAsync()
{
    // Paralel API istekleri
    Task<Weather> weatherTask = GetWeatherAsync();
    Task<News> newsTask = GetNewsAsync();
    
    // Tüm isteklerin tamamlanmasını bekle
    await Task.WhenAll(weatherTask, newsTask);
    
    // Sonuçları al
    Weather weather = await weatherTask;
    News news = await newsTask;
    
    return (weather, news);
}

private async Task<Weather> GetWeatherAsync()
{
    using var client = new HttpClient();
    var response = await client.GetAsync("https://api.weather.com");
    var json = await response.Content.ReadAsStringAsync();
    return JsonSerializer.Deserialize<Weather>(json);
}

private async Task<News> GetNewsAsync()
{
    using var client = new HttpClient();
    var response = await client.GetAsync("https://api.news.com");
    var json = await response.Content.ReadAsStringAsync();
    return JsonSerializer.Deserialize<News>(json);
}
```

## Kaynaklar ve İleri Okumalar

1. [Microsoft Docs: Asenkron Programlama](https://docs.microsoft.com/tr-tr/dotnet/csharp/programming-guide/concepts/async/)
2. [Asenkron Programlama En İyi Pratikleri](https://docs.microsoft.com/tr-tr/dotnet/standard/asynchronous-programming-patterns/task-based-asynchronous-pattern-tap)
3. [Task Paralel Kütüphanesi (TPL)](https://docs.microsoft.com/tr-tr/dotnet/standard/parallel-programming/task-parallel-library-tpl)
4. [ConfigureAwait Doğru Kullanımı](https://devblogs.microsoft.com/dotnet/configureawait-faq/)
5. [Asenkron Akışlar ve IAsyncEnumerable](https://docs.microsoft.com/tr-tr/dotnet/csharp/whats-new/csharp-8#asynchronous-streams)

---

Bu doküman C# dilinde asenkron programlama konusunda temel bir başlangıç sağlamaktadır. Daha ileri seviye konular için lütfen yukarıdaki kaynaklara başvurunuz.
