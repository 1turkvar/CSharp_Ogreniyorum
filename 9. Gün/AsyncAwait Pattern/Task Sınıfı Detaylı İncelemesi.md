# C# Task Sınıfı Detaylı İncelemesi

## İçindekiler
- [Giriş](#giriş)
- [Task Sınıfı Temelleri](#task-sınıfı-temelleri)
- [Task Oluşturma Yöntemleri](#task-oluşturma-yöntemleri)
- [Task'ları Bekleme](#taskları-bekleme)
- [Task'ların Durumlarını İzleme](#taskların-durumlarını-i̇zleme)
- [Hata Yönetimi](#hata-yönetimi)
- [Task Zincirleme](#task-zincirleme)
- [Task Koordinasyonu](#task-koordinasyonu)
- [Task\<TResult> Sınıfı](#tasktresult-sınıfı)
- [async ve await ile Task Kullanımı](#async-ve-await-ile-task-kullanımı)
- [Task Factory](#task-factory)
- [Task Cancelation](#task-cancelation)
- [Task vs Thread](#task-vs-thread)
- [En İyi Uygulamalar](#en-i̇yi-uygulamalar)
- [Performans İpuçları](#performans-i̇puçları)
- [Yaygın Hatalar ve Çözümleri](#yaygın-hatalar-ve-çözümleri)
- [Gerçek Dünya Örnekleri](#gerçek-dünya-örnekleri)

## Giriş

Task sınıfı, .NET Framework 4.0 ile birlikte tanıtılan Task Parallel Library (TPL) parçası olarak geliştirilmiştir. Asenkron operasyonların temsilini ve kontrolünü sağlayan bu sınıf, modern C# programlamada özellikle kullanıcı arayüzlerinde duyarlılığı korumak, I/O işlemlerini verimli bir şekilde yönetmek ve çok çekirdekli işlemcilerde paralel işlem yapmak için çok önemlidir.

Task yapısı, thread'lere göre daha yüksek seviyeli bir soyutlama sağlar ve .NET tarafından yönetilen bir iş birimi olarak düşünülebilir. Task'lar, thread havuzunu etkin bir şekilde kullanır ve birden fazla işlemin asenkron olarak çalıştırılmasını kolaylaştırır.

## Task Sınıfı Temelleri

Task sınıfı, `System.Threading.Tasks` namespace'i altında bulunur ve `IAsyncResult` arayüzünü uygular. Bir Task, asenkron olarak yürütülen bir işlemi temsil eder.

Temel özellikleri:

- **Status**: Task'ın mevcut durumunu gösterir (Created, WaitingForActivation, WaitingToRun, Running, RanToCompletion, Canceled, Faulted)
- **IsCompleted**: Task'ın tamamlanıp tamamlanmadığını belirtir
- **IsCanceled**: Task'ın iptal edilip edilmediğini belirtir
- **IsFaulted**: Task'ın hata ile sonuçlanıp sonuçlanmadığını belirtir
- **Exception**: Task içinde oluşan istisnayı içerir (AggregateException olarak sarmalanır)
- **Id**: Task'ın benzersiz tanımlayıcısı
- **CreationOptions**: Task oluşturulurken belirlenen seçenekler

```csharp
using System;
using System.Threading.Tasks;

class Program
{
    static void Main()
    {
        // Basit bir Task oluşturma
        Task myTask = new Task(() => {
            Console.WriteLine("Task çalışıyor...");
            // İşlem yapılıyor
        });
        
        // Task'ı başlatma
        myTask.Start();
        
        // Task'ın tamamlanmasını bekleme
        myTask.Wait();
        
        Console.WriteLine("Task tamamlandı.");
    }
}
```

## Task Oluşturma Yöntemleri

Task'ları farklı yollarla oluşturabilirsiniz:

### 1. Constructor ile Oluşturma

```csharp
Task task = new Task(() => Console.WriteLine("Task çalışıyor"));
task.Start(); // Start metodu çağrılmadıkça task başlamaz
```

### 2. Task.Run ile Oluşturma

```csharp
// Doğrudan çalışmaya başlayan task
Task task = Task.Run(() => Console.WriteLine("Task çalışıyor"));
```

### 3. Task.Factory.StartNew ile Oluşturma

```csharp
Task task = Task.Factory.StartNew(() => Console.WriteLine("Task çalışıyor"));
```

### 4. Task.FromResult ile Tamamlanmış Task Oluşturma

```csharp
Task<int> completedTask = Task.FromResult(42);
```

### 5. TaskCompletionSource ile Kontrollü Task Oluşturma

```csharp
TaskCompletionSource<string> tcs = new TaskCompletionSource<string>();
Task<string> task = tcs.Task;

// Daha sonra tamamlama
tcs.SetResult("İşlem sonucu");
```

## Task'ları Bekleme

Task'ların tamamlanmasını beklemek için çeşitli yöntemler vardır:

### Wait() Methodu

```csharp
Task task = Task.Run(() => {
    // Uzun süren bir işlem
    System.Threading.Thread.Sleep(2000);
});

// Bloklamalı bekleme (bu çağrı, task tamamlanana kadar thread'i bloklar)
task.Wait();
```

### WaitAll() ve WaitAny() Methodları

```csharp
Task task1 = Task.Run(() => System.Threading.Thread.Sleep(2000));
Task task2 = Task.Run(() => System.Threading.Thread.Sleep(1000));

// Tüm task'ların tamamlanmasını bekle
Task.WaitAll(task1, task2);

// VEYA

// Herhangi bir task'ın tamamlanmasını bekle
int completedTaskIndex = Task.WaitAny(task1, task2);
```

### Timeout ile Bekleme

```csharp
Task task = Task.Run(() => System.Threading.Thread.Sleep(5000));

// Maksimum 2 saniye bekle
bool completed = task.Wait(2000);
if (!completed)
{
    Console.WriteLine("Task 2 saniye içinde tamamlanmadı");
}
```

### ContinueWith Kullanımı

```csharp
Task task = Task.Run(() => {
    // İşlem
});

Task continuation = task.ContinueWith(completedTask => {
    Console.WriteLine("İlk task tamamlandı, devam edilecek işlem yapılıyor");
});
```

## Task'ların Durumlarını İzleme

Task'ların farklı durum değerleri şunlardır:

- **Created**: Task oluşturuldu ama henüz zamanlanmadı
- **WaitingForActivation**: Task'ın başlatılması bekleniyor (genelde continuations için)
- **WaitingToRun**: Task zamanlandı ama henüz çalışmaya başlamadı
- **Running**: Task şu anda çalışıyor
- **RanToCompletion**: Task başarıyla tamamlandı
- **Canceled**: Task iptal edildi
- **Faulted**: Task bir hata ile sonuçlandı

```csharp
Task task = Task.Run(() => {
    for (int i = 0; i < 5; i++)
    {
        Console.WriteLine($"Çalışıyor: {i}");
        System.Threading.Thread.Sleep(1000);
    }
});

// Task durumunu izleme
while (!task.IsCompleted)
{
    Console.WriteLine($"Task durumu: {task.Status}");
    System.Threading.Thread.Sleep(500);
}

Console.WriteLine($"Son durum: {task.Status}");
```

## Hata Yönetimi

Task içinde oluşan istisnalar, task tamamlanana kadar yakalanmaz. Bu istisnalar, task'ın Exception özelliğinde saklanır.

```csharp
Task task = Task.Run(() => {
    throw new InvalidOperationException("Task içinde bir hata oluştu");
});

try
{
    task.Wait(); // Bu noktada istisna yeniden fırlatılacak
}
catch (AggregateException ae)
{
    foreach (var ex in ae.InnerExceptions)
    {
        Console.WriteLine($"Yakalanan hata: {ex.Message}");
    }
}

// Alternatif: Task.Exception özelliğini kontrol etme
if (task.IsFaulted)
{
    Console.WriteLine($"Task hatası: {task.Exception.InnerException.Message}");
}
```

## Task Zincirleme

Task'ları birbirine bağlamak için `ContinueWith` metodunu kullanabilirsiniz:

```csharp
Task<int> task1 = Task.Run(() => {
    Console.WriteLine("İlk task çalışıyor");
    return 42;
});

Task<string> task2 = task1.ContinueWith(antecedent => {
    Console.WriteLine($"İkinci task çalışıyor, önceki sonuç: {antecedent.Result}");
    return $"Sonuç: {antecedent.Result * 2}";
});

Console.WriteLine(task2.Result); // "Sonuç: 84"
```

Farklı durumlara göre devam etme:

```csharp
Task task = Task.Run(() => {
    if (DateTime.Now.Second % 2 == 0)
        throw new Exception("Çift saniye hatası");
});

// Sadece başarılı tamamlanırsa çalışır
task.ContinueWith(t => {
    Console.WriteLine("Task başarıyla tamamlandı");
}, TaskContinuationOptions.OnlyOnRanToCompletion);

// Sadece hata durumunda çalışır
task.ContinueWith(t => {
    Console.WriteLine($"Task hata ile sonuçlandı: {t.Exception.InnerException.Message}");
}, TaskContinuationOptions.OnlyOnFaulted);

// Sadece iptal durumunda çalışır
task.ContinueWith(t => {
    Console.WriteLine("Task iptal edildi");
}, TaskContinuationOptions.OnlyOnCanceled);
```

## Task Koordinasyonu

Birden fazla task'ı koordine etmek için kullanabileceğiniz yöntemler:

### Task.WhenAll

Tüm task'ların tamamlanmasını bekler ve tüm sonuçları toplar:

```csharp
Task<int>[] tasks = new Task<int>[3];
tasks[0] = Task.Run(() => { System.Threading.Thread.Sleep(2000); return 1; });
tasks[1] = Task.Run(() => { System.Threading.Thread.Sleep(1000); return 2; });
tasks[2] = Task.Run(() => { System.Threading.Thread.Sleep(3000); return 3; });

// Tüm task'ların tamamlanmasını asenkron olarak bekle
Task<int[]> whenAllTask = Task.WhenAll(tasks);

// Tüm sonuçları al
int[] results = await whenAllTask;
Console.WriteLine(string.Join(", ", results)); // "1, 2, 3"
```

### Task.WhenAny

İlk tamamlanan task'ı bekler:

```csharp
Task<string>[] tasks = new Task<string>[3];
tasks[0] = Task.Run(async () => { await Task.Delay(2000); return "Task 1"; });
tasks[1] = Task.Run(async () => { await Task.Delay(1000); return "Task 2"; });
tasks[2] = Task.Run(async () => { await Task.Delay(3000); return "Task 3"; });

// İlk tamamlanan task'ı bekle
Task<Task<string>> whenAnyTask = Task.WhenAny(tasks);
Task<string> firstCompletedTask = await whenAnyTask;

string result = await firstCompletedTask;
Console.WriteLine($"İlk tamamlanan: {result}"); // "Task 2"
```

## Task\<TResult> Sınıfı

Task\<TResult>, bir değer döndüren Task türüdür:

```csharp
Task<int> calculationTask = Task.Run(() => {
    Console.WriteLine("Hesaplama yapılıyor...");
    System.Threading.Thread.Sleep(2000);
    return 42;
});

// Sonucu alma (bloklamalı)
int result = calculationTask.Result;
Console.WriteLine($"Sonuç: {result}");

// Veya asenkron olarak
int asyncResult = await calculationTask;
Console.WriteLine($"Asenkron sonuç: {asyncResult}");
```

## async ve await ile Task Kullanımı

C# 5.0 ile gelen `async` ve `await` anahtar kelimeleri, Task tabanlı asenkron programlamayı çok daha kolay hale getirmiştir:

```csharp
public async Task<string> DownloadWebPageAsync(string url)
{
    using (HttpClient client = new HttpClient())
    {
        Console.WriteLine("Web sayfası indiriliyor...");
        string content = await client.GetStringAsync(url);
        Console.WriteLine("İndirme tamamlandı!");
        return content;
    }
}

// Kullanımı
public async Task ProcessDataAsync()
{
    string content = await DownloadWebPageAsync("https://example.com");
    Console.WriteLine($"İndirilen içerik uzunluğu: {content.Length} karakter");
}
```

Birden fazla asenkron işlemi paralel yürütme:

```csharp
public async Task ProcessMultipleRequestsAsync()
{
    Task<string> task1 = DownloadWebPageAsync("https://example.com");
    Task<string> task2 = DownloadWebPageAsync("https://example.org");
    
    // Paralel olarak çalışıyor
    
    // İlk tamamlananı bekle
    Task<string> firstCompleted = await Task.WhenAny(task1, task2);
    string firstResult = await firstCompleted;
    Console.WriteLine($"İlk tamamlanan: {firstResult.Substring(0, 50)}...");
    
    // Diğerini bekle
    string secondResult = await (firstCompleted == task1 ? task2 : task1);
    Console.WriteLine($"İkinci tamamlanan: {secondResult.Substring(0, 50)}...");
}
```

## Task Factory

TaskFactory, çeşitli özelliklere sahip Task oluşturmak için kullanışlı bir sınıftır:

```csharp
// Varsayılan factory
TaskFactory factory = new TaskFactory();
Task task = factory.StartNew(() => Console.WriteLine("Task çalışıyor"));

// Özelleştirilmiş factory
CancellationTokenSource cts = new CancellationTokenSource();
TaskFactory customFactory = new TaskFactory(
    cts.Token,
    TaskCreationOptions.LongRunning,
    TaskContinuationOptions.None,
    TaskScheduler.Default);

Task customTask = customFactory.StartNew(() => {
    Console.WriteLine("Özel task çalışıyor");
});
```

Bağlı task'lar oluşturma:

```csharp
Task parent = Task.Factory.StartNew(() => {
    Console.WriteLine("Ana task başladı");
    
    // Bağlı task'lar oluşturma
    Task.Factory.StartNew(() => {
        Console.WriteLine("Bağlı task 1");
        System.Threading.Thread.Sleep(1000);
    }, TaskCreationOptions.AttachedToParent);
    
    Task.Factory.StartNew(() => {
        Console.WriteLine("Bağlı task 2");
        System.Threading.Thread.Sleep(2000);
    }, TaskCreationOptions.AttachedToParent);
    
    Console.WriteLine("Ana task işlemleri tamamlandı");
});

// Ana task'ın ve tüm bağlı task'ların tamamlanmasını bekler
parent.Wait();
Console.WriteLine("Tüm işlemler tamamlandı");
```

## Task Cancelation

Task'ların iptali için CancellationToken kullanımı:

```csharp
CancellationTokenSource cts = new CancellationTokenSource();
CancellationToken token = cts.Token;

Task task = Task.Run(() => {
    for (int i = 0; i < 100; i++)
    {
        // İptal isteği kontrol ediliyor
        token.ThrowIfCancellationRequested();
        
        Console.WriteLine($"İşlem {i}");
        System.Threading.Thread.Sleep(100);
    }
}, token);

// 2 saniye sonra iptal et
System.Threading.Thread.Sleep(2000);
cts.Cancel();

try
{
    task.Wait();
}
catch (AggregateException ae)
{
    if (ae.InnerException is OperationCanceledException)
    {
        Console.WriteLine("İşlem iptal edildi");
    }
}
```

## Task vs Thread

Task ve Thread arasındaki temel farklar:

| Özellik | Task | Thread |
|---------|------|--------|
| Soyutlama Seviyesi | Yüksek seviye | Düşük seviye |
| Asenkron Destek | Doğal olarak asenkron | Manuel olarak yönetilmeli |
| Sonuç Döndürme | Task\<T> ile doğrudan | Manuel olarak yönetilmeli |
| İptal Mekanizması | Entegre CancellationToken | Manuel olarak yönetilmeli |
| Hata Yönetimi | Exception propagation | Try-catch gerektirir |
| Havuz Kullanımı | Otomatik thread pool | Manuel thread oluşturma |
| Zincirleme | ContinueWith ile kolay | Manuel olarak yönetilmeli |
| Performans | Düşük maliyetli | Daha yüksek maliyetli |

```csharp
// Thread örneği
Thread thread = new Thread(() => {
    Console.WriteLine("Thread çalışıyor");
});
thread.Start();
thread.Join(); // Tamamlanmasını bekle

// Task örneği
Task task = Task.Run(() => {
    Console.WriteLine("Task çalışıyor");
});
task.Wait(); // Tamamlanmasını bekle
```

## En İyi Uygulamalar

Task kullanırken dikkat edilmesi gereken en iyi uygulamalar:

1. **ConfigureAwait(false) Kullanımı**: UI thread'ini bloklamaktan kaçınmak için:

```csharp
public async Task SomeLibraryMethodAsync()
{
    // UI thread ihtiyacı olmayan işlemlerde ConfigureAwait(false) kullanın
    await Task.Delay(1000).ConfigureAwait(false);
    
    // Bu kod, orijinal sync context'e dönmeden devam eder
    DoMoreWork();
}
```

2. **async void Yerine async Task Kullanımı**: Hata yönetimi ve test edilebilirlik için:

```csharp
// Kötü - istisnalar yakalanmaz
public async void BadAsyncMethod()
{
    await Task.Delay(1000);
    throw new Exception("Bu istisna yakalanmayacak!");
}

// İyi - istisnalar yakalanabilir
public async Task GoodAsyncMethod()
{
    await Task.Delay(1000);
    throw new Exception("Bu istisna yakalanabilir");
}
```

3. **İptal Desteği Eklenmesi**:

```csharp
public async Task ProcessDataAsync(CancellationToken cancellationToken = default)
{
    for (int i = 0; i < 100; i++)
    {
        // Her adımda iptal kontrolü
        cancellationToken.ThrowIfCancellationRequested();
        
        await Task.Delay(100, cancellationToken);
    }
}
```

4. **Uygun Task Oluşturma Yöntemini Seçme**:

```csharp
// CPU-bound işlemler için Task.Run
Task cpuBoundTask = Task.Run(() => PerformComplexCalculation());

// I/O-bound işlemler için asenkron metotlar
Task ioBoundTask = File.ReadAllTextAsync("file.txt");
```

5. **Gereksiz Task Oluşturmaktan Kaçınma**:

```csharp
// Kötü - gereksiz Task sarmalama
public Task<int> BadMethod()
{
    return Task.FromResult(42);
}

// İyi - zaten sonuç biliniyor
public int GoodMethod()
{
    return 42;
}

// Asenkron işlem gerektiren durumda
public async Task<int> AsyncMethod()
{
    await Task.Delay(100);
    return 42;
}
```

## Performans İpuçları

Task kullanımında performans iyileştirmeleri:

1. **ValueTask Kullanımı**: Bazı durumlarda Task yerine daha verimli:

```csharp
public ValueTask<int> GetValueAsync()
{
    // Önbellekte değer varsa, yeni bir Task oluşturmadan dön
    if (_cache.TryGetValue("key", out int value))
    {
        return new ValueTask<int>(value);
    }
    
    // Yoksa asenkron olarak al
    return new ValueTask<int>(LoadValueAsync());
}

private async Task<int> LoadValueAsync()
{
    // Yükleme işlemi
    await Task.Delay(1000);
    int result = 42;
    _cache["key"] = result;
    return result;
}
```

2. **Task Pooling**: Sürekli task oluşturmak yerine havuz kullanımı:

```csharp
// ObjectPool veya benzer bir mekanizma kullanarak
// TaskCompletionSource nesnelerini yeniden kullanma örnekleri
```

3. **Asenkron Akış İşleme (C# 8.0+)**:

```csharp
public async IAsyncEnumerable<int> GenerateNumbersAsync()
{
    for (int i = 0; i < 100; i++)
    {
        await Task.Delay(100);
        yield return i;
    }
}

// Kullanımı
await foreach (var number in GenerateNumbersAsync())
{
    Console.WriteLine(number);
}
```

## Yaygın Hatalar ve Çözümleri

Task kullanımında sık karşılaşılan hatalar:

1. **Deadlock Durumları**:

```csharp
// Problem: UI thread'de .Result kullanımı
private void Button_Click(object sender, EventArgs e)
{
    // BU DEADLOCK YARATABİLİR!
    string result = DoSomethingAsync().Result;
}

private async Task<string> DoSomethingAsync()
{
    await Task.Delay(1000);
    return "Sonuç";
}

// Çözüm: async-await kullanımı
private async void Button_Click(object sender, EventArgs e)
{
    string result = await DoSomethingAsync();
}
```

2. **İstisnaların Kaybedilmesi**:

```csharp
// Problem: Task'lar beklenmiyor
Task.Run(() => {
    throw new Exception("Bu istisna yakalanmayacak!");
});

// Çözüm: Task'ları bekleyin veya observe edin
Task task = Task.Run(() => {
    throw new Exception("Bu istisna yakalanabilir");
});

try {
    task.Wait();
}
catch (AggregateException ae) {
    Console.WriteLine($"Hata: {ae.InnerException.Message}");
}

// Veya:
TaskScheduler.UnobservedTaskException += (sender, e) => {
    Console.WriteLine($"Gözlemlenmeyen task istisnası: {e.Exception.Message}");
    e.SetObserved(); // İstisnayı işlenmiş olarak işaretle
};
```

3. **async Main Fonksiyonu (C# 7.1+)**:

```csharp
// Program başlangıcında asenkron kod çalıştırma
static async Task Main(string[] args)
{
    await DoSomethingAsync();
}
```

## Gerçek Dünya Örnekleri

Task'ların gerçek dünya uygulamalarına örnekler:

### Paralel Dosya İşleme

```csharp
public async Task ProcessFilesAsync(string[] filePaths)
{
    // Her dosya için bir task oluştur
    List<Task> fileTasks = new List<Task>();
    
    foreach (string path in filePaths)
    {
        fileTasks.Add(ProcessFileAsync(path));
    }
    
    // Tüm dosya işlemlerinin tamamlanmasını bekle
    await Task.WhenAll(fileTasks);
    Console.WriteLine("Tüm dosyalar işlendi");
}

private async Task ProcessFileAsync(string path)
{
    string content = await File.ReadAllTextAsync(path);
    // İşleme
    await File.WriteAllTextAsync($"{path}.processed", ProcessContent(content));
}

private string ProcessContent(string content)
{
    // İçerik işleme mantığı
    return content.ToUpper();
}
```

### Web API İstekleri

```csharp
public async Task<List<ApiResult>> FetchMultipleApisAsync()
{
    using (HttpClient client = new HttpClient())
    {
        // Farklı API'lere paralel istekler
        Task<string> weatherTask = client.GetStringAsync("https://api.weather.com/data");
        Task<string> newsTask = client.GetStringAsync("https://api.news.com/latest");
        Task<string> stocksTask = client.GetStringAsync("https://api.stocks.com/prices");
        
        // Tüm API yanıtlarını bekle
        await Task.WhenAll(weatherTask, newsTask, stocksTask);
        
        // Sonuçları işle
        List<ApiResult> results = new List<ApiResult>
        {
            ParseWeather(await weatherTask),
            ParseNews(await newsTask),
            ParseStocks(await stocksTask)
        };
        
        return results;
    }
}
```

### Asenkron UI Güncelleme

```csharp
private async void SearchButton_Click(object sender, EventArgs e)
{
    // UI güncellemesi
    SearchButton.Enabled = false;
    StatusLabel.Text = "Aranıyor...";
    ProgressBar.Visible = true;
    
    try
    {
        // Asenkron işlem
        List<SearchResult> results = await SearchDatabaseAsync(SearchTextBox.Text);
        
        // UI güncelleme
        ResultsListBox.Items.Clear();
        foreach (var result in results)
        {
            ResultsListBox.Items.Add(result);
        }
        
        StatusLabel.Text = $"{results.Count} sonuç bulundu";
    }
    catch (Exception ex)
    {
        StatusLabel.Text = $"Hata: {ex.Message}";
    }
    finally
    {
        // UI son durum
        SearchButton.Enabled = true;
        ProgressBar.Visible = false;
    }
}
```

### Zamanlayıcı ile Düzenli Task Çalıştırma

```csharp
private CancellationTokenSource _cts;

public async Task StartPeriodicUpdateAsync()
{
    _cts = new CancellationTokenSource();
    
    try
    {
        while (!_cts.Token.IsCancellationRequested)
        {
            // Düzenli çalışan işlem
            await UpdateDataAsync();
            
            // 5 dakika bekle
            await Task.Delay(TimeSpan.FromMinutes(5), _cts.Token);
        }
    }
    catch (OperationCanceledException)
    {
        // İptal durumu normal
    }
}

public void StopPeriodicUpdate()
{
    _cts?.Cancel();
}
```

Bu belge, C# Task sınıfının temel özelliklerini ve kullanım senaryolarını detaylı olarak açıklamaktadır. Modern C# uygulamalarında asenkron programlama, kullanıcı duyarlılığı ve verimli kaynak kullanımı için Task sınıfının etkin kullanımı büyük önem taşımaktadır.
