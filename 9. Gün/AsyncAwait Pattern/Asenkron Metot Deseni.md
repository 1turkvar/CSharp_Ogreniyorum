# C# Asenkron Programlama Rehberi

## İçindekiler
1. [Giriş](#giriş)
2. [Asenkron Programlamanın Temelleri](#asenkron-programlamanın-temelleri)
3. [async ve await Anahtar Kelimeleri](#async-ve-await-anahtar-kelimeleri)
4. [Task ve Task\<T> Sınıfları](#task-ve-taskt-sınıfları)
5. [Asenkron Metot Desenleri](#asenkron-metot-desenleri)
6. [Hata Yönetimi](#hata-yönetimi)
7. [İptal Mekanizması](#i̇ptal-mekanizması)
8. [Paralel Programlama ile İlişkisi](#paralel-programlama-ile-i̇lişkisi)
9. [Asenkron Akış Kontrolleri](#asenkron-akış-kontrolleri)
10. [Best Practices ve Optimizasyon](#best-practices-ve-optimizasyon)
11. [Gerçek Dünya Örnekleri](#gerçek-dünya-örnekleri)
12. [Sonuç](#sonuç)

## Giriş

Modern uygulamalar, kullanıcı arayüzü duyarlılığını korurken, uzun süren işlemleri verimli bir şekilde gerçekleştirebilmelidir. C# asenkron programlama modeli, bu tür işlemleri ana uygulama iş parçacığını bloke etmeden gerçekleştirmenize olanak tanır.

Asenkron programlama şu durumlarda özellikle faydalıdır:
- Ağ istekleri (HTTP çağrıları)
- Veritabanı işlemleri
- Dosya sistemi işlemleri
- Uzun süren hesaplamalar
- I/O tabanlı işlemler

## Asenkron Programlamanın Temelleri

### Senkron vs Asenkron

**Senkron (Geleneksel) Model:**
```csharp
public string GetWebContent()
{
    using (var client = new WebClient())
    {
        // Ana iş parçacığı burada bloke olur
        return client.DownloadString("https://example.com");
    }
}
```

**Asenkron Model:**
```csharp
public async Task<string> GetWebContentAsync()
{
    using (var client = new WebClient())
    {
        // Ana iş parçacığı bloke olmaz
        return await client.DownloadStringTaskAsync("https://example.com");
    }
}
```

### Asenkron Programlama Modelleri Tarihçesi

C# asenkron programlama üç farklı modelden geçmiştir:

1. **APM (Asynchronous Programming Model)** - Begin/End desenleri
   ```csharp
   IAsyncResult beginResult = webClient.BeginDownloadString(url, null, null);
   string result = webClient.EndDownloadString(beginResult);
   ```

2. **EAP (Event-based Asynchronous Pattern)** - Event temelli
   ```csharp
   webClient.DownloadStringCompleted += (s, e) => { string result = e.Result; };
   webClient.DownloadStringAsync(url);
   ```

3. **TAP (Task-based Asynchronous Pattern)** - Modern yaklaşım
   ```csharp
   Task<string> task = webClient.DownloadStringTaskAsync(url);
   string result = await task;
   ```

Modern C# programlama, Task-tabanlı Asenkron Desen (TAP) kullanmaktadır ve bu dokümanda odak noktamız olacaktır.

## async ve await Anahtar Kelimeleri

C# 5.0 ile tanıtılan `async` ve `await` anahtar kelimeleri, asenkron programlamayı basitleştirmiştir.

### async Anahtar Kelimesi

`async` anahtar kelimesi bir metodun asenkron olduğunu belirtir ve metodun içinde `await` kullanılmasına izin verir.

```csharp
public async Task<int> CalculateAsync()
{
    // Metot içeriği
}
```

Asenkron metotlar genellikle şu dönüş tiplerinden birine sahiptir:
- `Task<T>`: Asenkron olarak T tipinde bir değer döndürür
- `Task`: Asenkron olarak işlem yapar, değer döndürmez
- `void`: Asenkron event handler için (tavsiye edilmez)
- `ValueTask<T>` / `ValueTask`: Performans optimizasyonu için (C# 7.0+)
- `IAsyncEnumerable<T>`: Asenkron akış için (C# 8.0+)

### await Anahtar Kelimesi

`await` anahtar kelimesi, bir Task'in tamamlanmasını beklemek ve sonucunu almak için kullanılır. İşlemin tamamlanmasını beklerken ana iş parçacığını bloke etmez.

```csharp
int result = await CalculateAsync();
```

`await` çalıştığında:
1. Eğer asenkron işlem tamamlanmışsa, kod normal akışına devam eder
2. Eğer asenkron işlem devam ediyorsa, çağıran metodu askıya alır ve kontrolü çağıran metoda geri döndürür
3. İşlem tamamlandığında, kod kaldığı yerden devam eder

## Task ve Task\<T> Sınıfları

`Task` ve `Task<T>` sınıfları, asenkron işlemlerin temelini oluşturur.

### Task Sınıfı

`Task` sınıfı, herhangi bir değer döndürmeyen asenkron işlemleri temsil eder.

```csharp
public async Task WriteToFileAsync(string text)
{
    await File.WriteAllTextAsync("file.txt", text);
}
```

### Task\<T> Sınıfı

`Task<T>` sınıfı, T tipinde bir değer döndüren asenkron işlemleri temsil eder.

```csharp
public async Task<string> ReadFromFileAsync()
{
    return await File.ReadAllTextAsync("file.txt");
}
```

### Task Oluşturma Yöntemleri

**1. Task.Run**
İş parçacığı havuzunda bir işi çalıştırmak için:

```csharp
Task.Run(() => {
    // CPU-bound işlem
    for (int i = 0; i < 1000000; i++) {
        // Hesaplama
    }
});
```

**2. TaskCompletionSource**
Manuel olarak bir Task'ı tamamlamak için:

```csharp
var tcs = new TaskCompletionSource<string>();

// Başka bir yerde veya thread'de:
tcs.SetResult("Completed");

// Task'i kullanmak için:
string result = await tcs.Task;
```

**3. Task.FromResult**
Hemen tamamlanan bir Task için:

```csharp
Task<int> task = Task.FromResult(42);
```

**4. Task.Delay**
Belirli bir süre beklemek için:

```csharp
await Task.Delay(1000); // 1 saniye bekle
```

## Asenkron Metot Desenleri

### 1. Asenkron Metot İsimlendirme Kuralları

Asenkron metotlar genellikle "Async" son eki ile isimlendirilir:

```csharp
public async Task<string> GetDataAsync()
{
    return await _httpClient.GetStringAsync("https://api.example.com/data");
}
```

### 2. Asenkron Akış

Async/await kodunuzu okunması kolay, sıralı kod gibi yazmanıza olanak tanır:

```csharp
public async Task ProcessDataAsync()
{
    // Adım 1
    var data = await FetchDataAsync();
    
    // Adım 2
    var processedData = await TransformDataAsync(data);
    
    // Adım 3
    await SaveDataAsync(processedData);
}
```

### 3. Yaygın Asenkron Desenler

**Sıralı Asenkron İşlemler:**
```csharp
public async Task SequentialAsync()
{
    await Task1Async();
    await Task2Async();
    await Task3Async();
}
```

**Paralel Asenkron İşlemler:**
```csharp
public async Task ParallelAsync()
{
    Task task1 = Task1Async();
    Task task2 = Task2Async();
    Task task3 = Task3Async();
    
    await Task.WhenAll(task1, task2, task3);
}
```

**İlk Tamamlananı Bekleme:**
```csharp
public async Task<int> GetFirstResultAsync()
{
    Task<int> task1 = LongProcessAsync();
    Task<int> task2 = QuickProcessAsync();
    
    // İlk tamamlanan task'in sonucunu döndürür
    return await Task.WhenAny(task1, task2).Result;
}
```

**Asenkron Yerel Fonksiyonlar (C# 7.0+):**
```csharp
public async Task ProcessAsync()
{
    // Yerel asenkron fonksiyon
    async Task<int> HelperAsync()
    {
        await Task.Delay(100);
        return 42;
    }
    
    int result = await HelperAsync();
}
```

**ConfigureAwait Kullanımı:**
```csharp
// UI thread'ine dönmez (kitaplıklarda yaygın)
await DoWorkAsync().ConfigureAwait(false);
```

## Hata Yönetimi

Asenkron metotlarda hata yönetimi, normal try-catch blokları kullanılarak yapılabilir:

```csharp
public async Task TryOperationAsync()
{
    try
    {
        await SomeOperationThatMightFailAsync();
    }
    catch (HttpRequestException ex)
    {
        // HTTP hatalarını ele al
        Logger.LogError(ex);
    }
    catch (Exception ex)
    {
        // Diğer hataları ele al
        Logger.LogError(ex);
        throw; // Yeniden fırlat
    }
    finally
    {
        // Temizleme kodu
    }
}
```

### AggregateException

Birden çok Task'i beklerken, hatalar bir `AggregateException` içinde toplanır:

```csharp
public async Task HandleMultipleExceptionsAsync()
{
    var task1 = Task.Run(() => { throw new ArgumentException(); });
    var task2 = Task.Run(() => { throw new InvalidOperationException(); });
    
    try
    {
        // WhenAll tüm istisnaları toplar
        await Task.WhenAll(task1, task2);
    }
    catch (AggregateException ex)
    {
        foreach (var innerEx in ex.InnerExceptions)
        {
            Console.WriteLine($"Hata: {innerEx.Message}");
        }
    }
    catch (Exception ex)
    {
        // await kullanırken genellikle ilk istisna direkt olarak fırlatılır
        Console.WriteLine($"İlk hata: {ex.Message}");
    }
}
```

## İptal Mekanizması

Asenkron işlemlerin iptal edilmesi için `CancellationToken` kullanılır:

```csharp
public async Task<string> DownloadAsync(string url, CancellationToken cancellationToken)
{
    using (var httpClient = new HttpClient())
    {
        // Token iptal edilirse, bir OperationCanceledException fırlatılır
        return await httpClient.GetStringAsync(url, cancellationToken);
    }
}
```

### CancellationTokenSource İle İptal

```csharp
public async Task DemoWithCancellationAsync()
{
    // İptal kaynağı oluştur
    using (var cts = new CancellationTokenSource())
    {
        // Belirli bir süre sonra iptal et
        cts.CancelAfter(TimeSpan.FromSeconds(5));
        
        try
        {
            string result = await DownloadAsync("https://example.com", cts.Token);
            Console.WriteLine(result);
        }
        catch (OperationCanceledException)
        {
            Console.WriteLine("İndirme işlemi iptal edildi");
        }
    }
}
```

### Manuel İptal Kontrolü

```csharp
public async Task<int> LongCalculationAsync(CancellationToken cancellationToken)
{
    int result = 0;
    
    for (int i = 0; i < 1000000; i++)
    {
        // Periyodik olarak iptal kontrolü yap
        if (i % 1000 == 0)
        {
            cancellationToken.ThrowIfCancellationRequested();
            // Ya da:
            // if (cancellationToken.IsCancellationRequested)
            //     return -1; // veya throw new OperationCanceledException();
        }
        
        result += i;
    }
    
    return result;
}
```

## Paralel Programlama ile İlişkisi

Asenkron programlama ve paralel programlama farklı kavramlardır:

- **Asenkron programlama**: İşlemlerin eşzamanlı olarak ilerlemesini sağlar, ana amacı bloklamayı önlemektir
- **Paralel programlama**: İşleri birden çok CPU çekirdeğinde aynı anda çalıştırarak performansı artırmayı amaçlar

Ancak bu iki yaklaşım birlikte kullanılabilir:

```csharp
public async Task ProcessItemsAsync(List<int> items)
{
    // Parallel.ForEach paralel çalışır, ancak ana thread'i bloke eder
    Parallel.ForEach(items, item => {
        ProcessItem(item); // Senkron işlem
    });
    
    // Paralel ve asenkron
    await Task.WhenAll(items.Select(item => ProcessItemAsync(item)));
    
    // Sınırlı paralellik
    var options = new ParallelOptions { MaxDegreeOfParallelism = 4 };
    await Parallel.ForEachAsync(items, options, async (item, token) => {
        await ProcessItemAsync(item);
    });
}
```

## Asenkron Akış Kontrolleri

### ValueTask Kullanımı

`ValueTask<T>`, Task'ların neden olduğu performans yükünü azaltmak için kullanılır:

```csharp
public ValueTask<int> GetValueAsync()
{
    if (_cachedValue.HasValue)
    {
        // Değer zaten mevcut - Task oluşturmaya gerek yok
        return new ValueTask<int>(_cachedValue.Value);
    }
    
    // Değeri asenkron olarak yükle
    return new ValueTask<int>(LoadValueAsync());
}
```

### IAsyncEnumerable\<T> (C# 8.0+)

Asenkron veri akışları için kullanılır:

```csharp
public async IAsyncEnumerable<string> GetLinesAsync(string file)
{
    using StreamReader reader = File.OpenText(file);
    string line;
    
    while ((line = await reader.ReadLineAsync()) != null)
    {
        yield return line; // Her satırı üretir
    }
}

// Kullanımı:
await foreach (string line in GetLinesAsync("large-file.txt"))
{
    Console.WriteLine(line);
}
```

## Best Practices ve Optimizasyon

### 1. async void Kullanımından Kaçının

`async void` metotlar, istisnalar yakalamayı zorlaştırır ve test edilmesi daha zordur.

```csharp
// Kötü
public async void SaveData() { ... }

// İyi
public async Task SaveDataAsync() { ... }
```

### 2. ConfigureAwait Kullanımı

Kitaplık geliştiricileri, UI thread bloklamalarını önlemek için `ConfigureAwait(false)` kullanmalıdır:

```csharp
public async Task<int> LibraryMethodAsync()
{
    // UI context'e geri dönmeye gerek yok
    var data = await FetchDataAsync().ConfigureAwait(false);
    var result = await ProcessDataAsync(data).ConfigureAwait(false);
    return result;
}
```

### 3. İşlemleri Başlatın ve Sonra Bekleyin

Birden çok asenkron işlemi başlatıp sonra beklerseniz, bunlar paralel çalışabilir:

```csharp
// İyi: İşlemler paralel başlar
Task<Data> dataTask = _repository.GetDataAsync();
Task<Settings> settingsTask = _repository.GetSettingsAsync();

Data data = await dataTask;
Settings settings = await settingsTask;
```

### 4. Task.WhenAny ve Task.WhenAll Kullanımı

```csharp
// Tüm görevlerin tamamlanmasını bekler
Task<int>[] tasks = new[]
{
    Task1Async(),
    Task2Async(),
    Task3Async()
};

int[] results = await Task.WhenAll(tasks);
```

```csharp
// İlk tamamlanan görevi işler
Task<ResponseData> primaryTask = GetFromPrimarySourceAsync();
Task<ResponseData> backupTask = GetFromBackupSourceAsync();

Task<ResponseData> completedTask = await Task.WhenAny(primaryTask, backupTask);
ResponseData result = await completedTask;
```

### 5. Asenkron Metotları Senkron Çağırmaktan Kaçının

```csharp
// Kötü: Deadlock riski yaratır
string result = GetDataAsync().Result;
string resultWait = GetDataAsync().Wait();

// İyi: Her zaman await kullanın
string result = await GetDataAsync();
```

## Gerçek Dünya Örnekleri

### 1. Web API İsteği

```csharp
public class WeatherService
{
    private readonly HttpClient _httpClient;
    
    public WeatherService(HttpClient httpClient)
    {
        _httpClient = httpClient;
    }
    
    public async Task<WeatherData> GetWeatherAsync(string city)
    {
        string url = $"https://api.example.com/weather?city={city}";
        
        try
        {
            string json = await _httpClient.GetStringAsync(url);
            return JsonSerializer.Deserialize<WeatherData>(json);
        }
        catch (HttpRequestException ex)
        {
            throw new WeatherServiceException("Hava durumu alınamadı", ex);
        }
    }
}
```

### 2. Veritabanı İşlemi

```csharp
public class CustomerRepository
{
    private readonly string _connectionString;
    
    public CustomerRepository(string connectionString)
    {
        _connectionString = connectionString;
    }
    
    public async Task<Customer> GetCustomerAsync(int id)
    {
        using (var connection = new SqlConnection(_connectionString))
        {
            await connection.OpenAsync();
            
            using (var command = connection.CreateCommand())
            {
                command.CommandText = "SELECT * FROM Customers WHERE Id = @Id";
                command.Parameters.AddWithValue("@Id", id);
                
                using (var reader = await command.ExecuteReaderAsync())
                {
                    if (await reader.ReadAsync())
                    {
                        return new Customer
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
}
```

### 3. Dosya İşlemleri

```csharp
public class DocumentService
{
    public async Task<Document> ReadDocumentAsync(string path)
    {
        string content = await File.ReadAllTextAsync(path);
        
        return new Document
        {
            Content = content,
            Length = content.Length,
            Path = path,
            LastRead = DateTime.Now
        };
    }
    
    public async Task SaveDocumentAsync(Document document)
    {
        // Birden fazla asenkron işlemi sırayla yapma
        await File.WriteAllTextAsync(document.Path, document.Content);
        await UpdateDocumentIndexAsync(document);
        await NotifyUserAsync(document.OwnerId);
    }
}
```

### 4. UI Uygulaması (WPF)

```csharp
public partial class MainWindow : Window
{
    private async void SearchButton_Click(object sender, RoutedEventArgs e)
    {
        SearchButton.IsEnabled = false;
        ProgressBar.Visibility = Visibility.Visible;
        
        try
        {
            // UI thread'i bloke etmeden arama yap
            var results = await _searchService.SearchAsync(SearchBox.Text);
            ResultsListView.ItemsSource = results;
        }
        catch (Exception ex)
        {
            MessageBox.Show($"Arama sırasında hata oluştu: {ex.Message}");
        }
        finally
        {
            SearchButton.IsEnabled = true;
            ProgressBar.Visibility = Visibility.Collapsed;
        }
    }
}
```

### 5. Asenkron Stream Örneği (C# 8.0+)

```csharp
public class LogAnalyzer
{
    public async IAsyncEnumerable<LogEntry> AnalyzeLogsAsync(string path)
    {
        await using FileStream stream = File.OpenRead(path);
        using StreamReader reader = new StreamReader(stream);
        
        string line;
        while ((line = await reader.ReadLineAsync()) != null)
        {
            if (line.Contains("ERROR"))
            {
                LogEntry entry = ParseLogEntry(line);
                yield return entry;
            }
        }
    }
    
    private LogEntry ParseLogEntry(string line)
    {
        // Log satırını ayrıştır ve LogEntry nesnesine dönüştür
        return new LogEntry { /* ... */ };
    }
}

// Kullanımı:
public async Task ProcessErrorLogsAsync()
{
    await foreach (var entry in _analyzer.AnalyzeLogsAsync("app.log"))
    {
        Console.WriteLine($"Error at {entry.Timestamp}: {entry.Message}");
        
        // Gerekirse işlemi iptal et
        if (entry.Severity > 5)
            break;
    }
}
```

## Sonuç

C# asenkron programlama modeli, modern uygulamaların duyarlılığını ve ölçeklenebilirliğini artırmak için güçlü bir araçtır. Async/await deseni, karmaşık asenkron kodu basit ve okunabilir hale getirerek geliştirme sürecini önemli ölçüde kolaylaştırır.

Bu rehber, C# asenkron programlamasının temel kavramlarını ve ileri düzey tekniklerini kapsamaktadır. Bu bilgileri kullanarak daha verimli, duyarlı ve ölçeklenebilir uygulamalar geliştirebilirsiniz.

Unutmayın ki asenkron programlama, performans ve kullanıcı deneyimini iyileştirmenin anahtarıdır, ancak doğru bir şekilde uygulanması önemlidir. Bu rehberde belirtilen best practice'leri takip ederek, asenkron kodun zorluklarından kaçınabilir ve tüm avantajlarından yararlanabilirsiniz.
