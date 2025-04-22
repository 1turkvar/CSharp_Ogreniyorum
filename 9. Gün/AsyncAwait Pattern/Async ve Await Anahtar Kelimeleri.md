# C# Async ve Await Anahtar Kelimeleri Detaylı Rehberi

## İçindekiler
1. [Giriş](#giriş)
2. [Asenkron Programlama Nedir?](#asenkron-programlama-nedir)
3. [Async ve Await Anahtar Kelimeleri](#async-ve-await-anahtar-kelimeleri)
   - [Async Anahtar Kelimesi](#async-anahtar-kelimesi)
   - [Await Anahtar Kelimesi](#await-anahtar-kelimesi)
4. [Task ve Task\<T> Sınıfları](#task-ve-taskt-sınıfları)
5. [Asenkron Metodları Tanımlama ve Kullanma](#asenkron-metodları-tanımlama-ve-kullanma)
6. [Asenkron Programlama En İyi Uygulamaları](#asenkron-programlama-en-i̇yi-uygulamaları)
7. [Yaygın Senaryolar ve Örnekler](#yaygın-senaryolar-ve-örnekler)
8. [Asenkron Programlamada Hata Yönetimi](#asenkron-programlamada-hata-yönetimi)
9. [ConfigureAwait Metodu](#configureawait-metodu)
10. [Asenkron Programlamadaki Tuzaklar](#asenkron-programlamadaki-tuzaklar)
11. [Performans Hususları](#performans-hususları)
12. [Özet](#özet)

## Giriş

Modern uygulamalar, kullanıcı deneyimini iyileştirmek ve sistem kaynaklarını verimli kullanmak için asenkron programlama tekniklerinden yararlanır. C#'ta `async` ve `await` anahtar kelimeleri, asenkron programlamayı daha kolay ve anlaşılır hale getirmek için tasarlanmıştır. Bu rehber, bu anahtar kelimelerin derinlemesine anlaşılmasını sağlayacak ve C# uygulamalarınızda asenkron programlamayı nasıl uygulayacağınızı gösterecektir.

## Asenkron Programlama Nedir?

Asenkron programlama, işlemlerin eşzamanlı olarak yürütülmesine izin veren bir programlama paradigmasıdır. Geleneksel senkron programlamada, bir işlem tamamlanana kadar program akışı bekler. Ancak asenkron programlamada, bir işlem başlatılır ve tamamlanırken diğer işlemler devam edebilir.

Asenkron programlama şu durumlarda özellikle faydalıdır:

- I/O operasyonları (dosya okuma/yazma)
- Ağ istekleri (HTTP istekleri, veritabanı sorguları)
- Uzun süren hesaplamalar
- Kullanıcı arayüzü uygulamaları (UI'ın donmasını önlemek için)

## Async ve Await Anahtar Kelimeleri

### Async Anahtar Kelimesi

`async` anahtar kelimesi, bir metodun asenkron olduğunu belirtir. Bu anahtar kelime metot imzası içinde kullanılır ve şunları yapar:

- Derleyiciye metodun asenkron olduğunu belirtir
- Metodun içinde `await` anahtar kelimesinin kullanılmasına izin verir
- Metodun bir `Task` veya `Task<T>` döndürmesini sağlar

```csharp
public async Task<string> GetDataAsync()
{
    // Asenkron metot içeriği
}
```

Önemli noktalar:
- `async` anahtar kelimesi tek başına metodu asenkron yapmaz
- Metodun gerçekten asenkron olması için içinde en az bir `await` ifadesi bulunmalıdır
- `async void` tanımlamalarından genellikle kaçınılmalıdır (yalnızca olay yöneticileri için kullanılmalıdır)

### Await Anahtar Kelimesi

`await` anahtar kelimesi, asenkron bir işlemin tamamlanmasını beklemek için kullanılır. Bu anahtar kelime:

- Bir `Task` veya `Task<T>` nesnesinin sonucunu bekler
- Beklerken, kontrolü çağıran metoda geri verir (thread bloklanmaz)
- İşlem tamamlandığında, kodun kaldığı yerden devam etmesini sağlar

```csharp
public async Task<string> GetDataAsync()
{
    HttpClient client = new HttpClient();
    string result = await client.GetStringAsync("https://example.com/api/data");
    return result; // Bu satır sadece GetStringAsync tamamlandıktan sonra çalışır
}
```

## Task ve Task\<T> Sınıfları

Asenkron programlama, `Task` ve `Task<T>` sınıfları etrafında inşa edilmiştir:

- `Task`: Dönüş değeri olmayan asenkron operasyonları temsil eder
- `Task<T>`: T tipinde bir değer döndüren asenkron operasyonları temsil eder

Bu sınıflar, .NET'in görev tabanlı asenkron modelinin (Task-based Asynchronous Pattern - TAP) temelini oluşturur.

```csharp
// Değer döndürmeyen asenkron metot
public async Task ProcessDataAsync()
{
    await Task.Delay(1000); // 1 saniye bekle
    Console.WriteLine("İşlem tamamlandı");
}

// Değer döndüren asenkron metot
public async Task<int> CalculateResultAsync()
{
    await Task.Delay(1000); // 1 saniye bekle
    return 42;
}
```

## Asenkron Metodları Tanımlama ve Kullanma

Asenkron bir metot genellikle aşağıdaki kurallara uyar:

1. `async` anahtar kelimesi ile işaretlenir
2. Genellikle `Task` veya `Task<T>` döndürür
3. İsimlerinin sonunda "Async" son eki olur (konvensiyon)
4. En az bir `await` ifadesi içerir

```csharp
// Asenkron metot tanımı
public async Task<string> ReadFileAsync(string filePath)
{
    using (StreamReader reader = new StreamReader(filePath))
    {
        return await reader.ReadToEndAsync();
    }
}

// Asenkron metodun kullanımı
public async Task ProcessFileAsync()
{
    string content = await ReadFileAsync("example.txt");
    Console.WriteLine(content);
}
```

## Asenkron Programlama En İyi Uygulamaları

Asenkron programlama konusunda dikkat edilmesi gereken bazı önemli uygulamalar:

1. **Async all the way**: Asenkron metodlar senkron metodlardan çağrılmamalıdır, asenkron zinciri korunmalıdır.

```csharp
// Kötü: Asenkron metodu senkron olarak çağırmak
public string GetData()
{
    return GetDataAsync().Result; // Deadlock riski!
}

// İyi: Asenkron metodu asenkron olarak çağırmak
public async Task<string> GetDataWrapper()
{
    return await GetDataAsync();
}
```

2. **ConfigureAwait kullanımı**: Kütüphane geliştiricileri için, UI thread'ine geri dönmemeyi sağlamak amacıyla `ConfigureAwait(false)` kullanın:

```csharp
public async Task<string> LibraryMethodAsync()
{
    // Library code should use ConfigureAwait(false) to avoid potential deadlocks
    string result = await httpClient.GetStringAsync("https://example.com").ConfigureAwait(false);
    return result;
}
```

3. **Paralel işlemler için Task.WhenAll kullanımı**:

```csharp
public async Task ProcessMultipleItemsAsync(string[] items)
{
    var tasks = new List<Task>();
    
    foreach (var item in items)
    {
        tasks.Add(ProcessItemAsync(item));
    }
    
    await Task.WhenAll(tasks);
    Console.WriteLine("Tüm işlemler tamamlandı");
}
```

4. **async void'den kaçının**: Event handler'lar dışında async void kullanmaktan kaçının:

```csharp
// Kötü: async void
public async void ProcessData() // Hata yakalamak zor!
{
    await Task.Delay(1000);
}

// İyi: async Task
public async Task ProcessDataAsync()
{
    await Task.Delay(1000);
}
```

## Yaygın Senaryolar ve Örnekler

### HTTP İstekleri

```csharp
public async Task<string> GetWebContentAsync(string url)
{
    using (HttpClient client = new HttpClient())
    {
        return await client.GetStringAsync(url);
    }
}
```

### Dosya İşlemleri

```csharp
public async Task SaveDataAsync(string data, string filePath)
{
    using (StreamWriter writer = new StreamWriter(filePath))
    {
        await writer.WriteAsync(data);
    }
}
```

### Veritabanı İşlemleri

```csharp
public async Task<User> GetUserAsync(int userId)
{
    using (SqlConnection connection = new SqlConnection(connectionString))
    {
        await connection.OpenAsync();
        
        using (SqlCommand command = new SqlCommand("SELECT * FROM Users WHERE Id = @Id", connection))
        {
            command.Parameters.AddWithValue("@Id", userId);
            
            using (SqlDataReader reader = await command.ExecuteReaderAsync())
            {
                if (await reader.ReadAsync())
                {
                    return new User
                    {
                        Id = reader.GetInt32(0),
                        Name = reader.GetString(1),
                        Email = reader.GetString(2)
                    };
                }
                
                return null;
            }
        }
    }
}
```

### UI Uygulamaları (WPF)

```csharp
private async void Button_Click(object sender, RoutedEventArgs e)
{
    // UI thread'i bloklamadan işlem yapılır
    statusText.Text = "Veri yükleniyor...";
    
    try 
    {
        string data = await LoadDataAsync();
        resultTextBox.Text = data;
        statusText.Text = "Veri yüklendi!";
    }
    catch (Exception ex)
    {
        statusText.Text = "Hata: " + ex.Message;
    }
}
```

## Asenkron Programlamada Hata Yönetimi

Asenkron metodlarda hata yönetimi, try-catch blokları kullanılarak gerçekleştirilir:

```csharp
public async Task<string> TryGetDataAsync()
{
    try
    {
        HttpClient client = new HttpClient();
        return await client.GetStringAsync("https://example.com/api/data");
    }
    catch (HttpRequestException ex)
    {
        Console.WriteLine($"HTTP isteği başarısız: {ex.Message}");
        return null;
    }
    catch (Exception ex)
    {
        Console.WriteLine($"Genel hata: {ex.Message}");
        throw; // Yeniden fırlat
    }
}
```

Özel bir asenkron işlemi başarısız olduğunda yapılacak işlemler için `Task.ContinueWith` kullanılabilir:

```csharp
public async Task ProcessWithFallbackAsync()
{
    var task = GetDataAsync();
    
    // Hata durumunda yedek veri sağla
    var result = await task.ContinueWith(t => {
        if (t.IsFaulted)
            return "Yedek veri";
        else
            return t.Result;
    });
    
    Console.WriteLine(result);
}
```

## ConfigureAwait Metodu

`ConfigureAwait` metodu, asenkron işlemin tamamlandıktan sonra çağıran thread'e geri dönüp dönmeyeceğini kontrol eder:

```csharp
// ConfigureAwait(true) - varsayılan, orijinal context'e geri döner
await Task.Delay(1000); // UI uygulamalarda ana thread'e geri döner

// ConfigureAwait(false) - orijinal context'e geri dönmez
await Task.Delay(1000).ConfigureAwait(false); // Herhangi bir thread'de devam edebilir
```

Kütüphane geliştiricileri, deadlock sorunlarını önlemek için `ConfigureAwait(false)` kullanmalıdır. Uygulama geliştiricileri ise UI güncellemeleri için genelde varsayılan davranışı (UI thread'ine geri dönme) tercih ederler.

## Asenkron Programlamadaki Tuzaklar

### Deadlock Sorunu

```csharp
// Bu kod deadlock'a neden olabilir!
public void Button_Click(object sender, EventArgs e)
{
    var task = DoSomethingAsync();
    task.Wait(); // Deadlock riski!
}

public async Task DoSomethingAsync()
{
    await Task.Delay(1000); // Bu await, UI thread'ine geri dönmeyi bekler
    // Ama UI thread'i Wait() metodunda bloklandığı için deadlock oluşur
}
```

### Isınma Problemleri

```csharp
// Kötü: Her çağrıda yeni HttpClient oluşturulur
public async Task<string> BadHttpRequestAsync(string url)
{
    using (HttpClient client = new HttpClient()) // İstemci her seferinde oluşturulur!
    {
        return await client.GetStringAsync(url);
    }
}

// İyi: HttpClient statik olarak bir kez oluşturulur
private static readonly HttpClient _httpClient = new HttpClient();

public async Task<string> GoodHttpRequestAsync(string url)
{
    return await _httpClient.GetStringAsync(url);
}
```

### Beklenmeyen Görevler

```csharp
// Kötü: await eksik, görev beklenmiyor
public async Task ProcessFilesAsync(string[] filePaths)
{
    foreach (var path in filePaths)
    {
        ProcessFileAsync(path); // Hata: await eksik!
    }
    // Metodun bitmesi, tüm dosyaların işlendiği anlamına gelmez!
}

// İyi: await kullanımı
public async Task ProcessFilesCorrectlyAsync(string[] filePaths)
{
    foreach (var path in filePaths)
    {
        await ProcessFileAsync(path); // İyi: sırayla işlenir
    }
}

// Alternatif: Paralel işleme için
public async Task ProcessFilesParallelAsync(string[] filePaths)
{
    var tasks = filePaths.Select(path => ProcessFileAsync(path));
    await Task.WhenAll(tasks); // İyi: tümü paralel işlenir ve tamamlanmaları beklenir
}
```

## Performans Hususları

Asenkron programlama her durumda daha iyi performans anlamına gelmez. Dikkat edilmesi gereken noktalar:

1. **CPU-bound vs I/O-bound işlemler**:
   - I/O-bound işlemler (disk, ağ) için `async/await` idealdir
   - CPU-bound işlemler için `Task.Run` ve paralel işleme daha uygundur

```csharp
// I/O-bound işlem için async/await
public async Task<string> ReadFileAsync(string filePath)
{
    using (StreamReader reader = new StreamReader(filePath))
    {
        return await reader.ReadToEndAsync();
    }
}

// CPU-bound işlem için Task.Run
public async Task<int> ComputeIntensiveOperationAsync(int input)
{
    return await Task.Run(() => {
        // CPU yoğun işlem
        int result = 0;
        for (int i = 0; i < input; i++)
        {
            result += PerformComplexCalculation(i);
        }
        return result;
    });
}
```

2. **ValueTask için düşük bellek ayak izi**:

```csharp
// Task<T> yerine ValueTask<T> kullanımı
public ValueTask<int> GetValueAsync()
{
    if (_cachedValue.HasValue)
    {
        return new ValueTask<int>(_cachedValue.Value); // Heap allocasyonu yok
    }
    
    return new ValueTask<int>(LoadValueAsync());
}
```

3. **Asenkron operasyonlarda gereksiz Task oluşturmaktan kaçınma**:

```csharp
// Kötü: Her seferinde yeni Task nesnesi oluşturur
public Task<int> BadCachedValueAsync()
{
    if (_cache.TryGetValue("key", out int value))
    {
        return Task.FromResult(value); // Yeni bir Task nesnesi oluşturur
    }
    
    return FetchValueAsync();
}

// İyi: ValueTask kullanarak gereksiz Task oluşturmaktan kaçınır
public ValueTask<int> GoodCachedValueAsync()
{
    if (_cache.TryGetValue("key", out int value))
    {
        return new ValueTask<int>(value); // Task oluşmaz
    }
    
    return new ValueTask<int>(FetchValueAsync());
}
```

## Özet

`async` ve `await` anahtar kelimeleri, C# programlama dilinde asenkron programlamayı basitleştiren güçlü araçlardır. Bu anahtar kelimeler sayesinde:

1. Kullanıcı arayüzlerinin duyarlılığını artırabilirsiniz
2. Sistem kaynaklarını daha verimli kullanabilirsiniz
3. I/O işlemlerini bloklamadan gerçekleştirebilirsiniz
4. Karmaşık asenkron akışları senkron kod gibi yazabilirsiniz

Asenkron programlama, modern C# uygulamalarının önemli bir parçası haline gelmiştir ve özellikle web uygulamaları, ağ hizmetleri ve kullanıcı arayüzü uygulamaları için vazgeçilmezdir.

Ancak, doğru kullanılmadığında performans sorunlarına, deadlock'lara ve zor tespit edilebilen hatalara yol açabilir. Bu nedenle, asenkron programlama ilkelerini anlamak ve en iyi uygulamaları takip etmek önemlidir.

Bu rehberde, C#'ın asenkron programlama modelini, `async` ve `await` anahtar kelimelerinin nasıl çalıştığını ve bunları etkili bir şekilde nasıl kullanacağınızı öğrendiniz. Bu bilgilerle, daha verimli ve duyarlı uygulamalar geliştirebileceksiniz.
