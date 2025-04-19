# C# Generic Koleksiyonlar Rehberi

## İçindekiler
1. [Giriş](#giriş)
2. [Generic Kavramı](#generic-kavramı)
3. [Generic Koleksiyonlar Nedir?](#generic-koleksiyonlar-nedir)
4. [Temel Generic Koleksiyonlar](#temel-generic-koleksiyonlar)
   - [List\<T>](#listt)
   - [Dictionary\<TKey, TValue>](#dictionarytkey-tvalue)
   - [HashSet\<T>](#hashsett)
   - [Queue\<T>](#queuet)
   - [Stack\<T>](#stackt)
   - [LinkedList\<T>](#linkedlistt)
   - [SortedList\<TKey, TValue>](#sortedlisttkey-tvalue)
   - [SortedDictionary\<TKey, TValue>](#sorteddictionarytkey-tvalue)
   - [SortedSet\<T>](#sortedsett)
5. [Concurrent Koleksiyonlar](#concurrent-koleksiyonlar)
   - [ConcurrentDictionary\<TKey, TValue>](#concurrentdictionarytkey-tvalue)
   - [ConcurrentQueue\<T>](#concurrentqueuet)
   - [ConcurrentBag\<T>](#concurrentbagt)
   - [ConcurrentStack\<T>](#concurrentstackt)
6. [ReadOnly Koleksiyonlar](#readonly-koleksiyonlar)
   - [ReadOnlyCollection\<T>](#readonlycollectiont)
   - [ReadOnlyDictionary\<TKey, TValue>](#readonlydictionarytkey-tvalue)
7. [Immutable Koleksiyonlar](#immutable-koleksiyonlar)
8. [Koleksiyon Performans Karşılaştırması](#koleksiyon-performans-karşılaştırması)
9. [Custom Generic Koleksiyon Oluşturma](#custom-generic-koleksiyon-oluşturma)
10. [LINQ ile Generic Koleksiyonlar](#linq-ile-generic-koleksiyonlar)
11. [En İyi Uygulama Önerileri](#en-iyi-uygulama-önerileri)
12. [Sık Yapılan Hatalar](#sık-yapılan-hatalar)

## Giriş

C# programlama dilinde koleksiyonlar, birden fazla veriyi tutmak, düzenlemek ve yönetmek için kullanılan veri yapılarıdır. Generic koleksiyonlar ise tip güvenliği (type safety) sağlayan, performans avantajları sunan ve kod tekrarını azaltan modern koleksiyon türleridir.

Bu rehberde, C# dilindeki generic koleksiyonların tüm yönlerini inceleyeceğiz. Temel türlerden ileri düzey kullanım senaryolarına kadar, C# programlama dilinizde veri yönetimini optimize etmenizi sağlayacak bilgileri bulacaksınız.

## Generic Kavramı

Generic'ler, C# 2.0 ile birlikte tanıtılan ve kodunuzu daha esnek, yeniden kullanılabilir ve tip güvenli hale getiren bir özelliktir. Generic'lerin temel avantajları:

- **Tip Güvenliği**: Derleme zamanında tür kontrolü sağlar, çalışma zamanı hatalarını azaltır.
- **Performans**: Boxing/unboxing işlemlerini ortadan kaldırarak performansı artırır.
- **Kod Tekrarını Azaltma**: Farklı türler için aynı mantığı tek bir kodla kullanabilme imkanı sağlar.
- **Yeniden Kullanılabilirlik**: Aynı kodu farklı veri tipleriyle kullanmayı mümkün kılar.

Örnek bir generic sınıf tanımı:

```csharp
public class GenericRepository<T> where T : class
{
    public void Add(T entity) { /* ... */ }
    public T GetById(int id) { /* ... */ }
    public IEnumerable<T> GetAll() { /* ... */ }
    public void Update(T entity) { /* ... */ }
    public void Delete(T entity) { /* ... */ }
}
```

## Generic Koleksiyonlar Nedir?

Generic koleksiyonlar, `System.Collections.Generic` namespace'i altında bulunan ve belirli bir veri tipi üzerinde çalışan koleksiyon sınıflarıdır. Generic olmayan (non-generic) koleksiyonlardan farklı olarak, generic koleksiyonlar yalnızca belirtilen türde nesneleri kabul eder ve döndürür, böylece tip dönüşümü gereksinimini ortadan kaldırır.

Generic koleksiyonların avantajları:

1. **Tip Güvenliği**: Yalnızca belirtilen türde öğelerin eklenmesine izin verir.
2. **Performans Artışı**: Boxing/unboxing işlemleri gerektirmez.
3. **Kod Okunabilirliği**: Kodunuzun amacı daha net anlaşılır.
4. **Daha İyi IntelliSense Desteği**: IDE'de daha iyi kod tamamlama ve ipuçları sağlar.

Generic olmayan koleksiyonlar (ArrayList, Hashtable, Queue, vb.) `object` tipinde çalışırken, generic koleksiyonlar belirli bir tip ile çalışır.

## Temel Generic Koleksiyonlar

### List\<T>

`List<T>`, dinamik bir dizi yapısıdır ve C#'ta en yaygın kullanılan generic koleksiyondur. ArrayList'in generic versiyonu olarak düşünülebilir.

**Özellikler ve Metodlar:**
- `Add()`, `AddRange()` - Eleman ekleme
- `Remove()`, `RemoveAt()`, `RemoveRange()` - Eleman silme
- `Contains()`, `IndexOf()`, `Find()`, `FindAll()` - Eleman arama
- `Sort()`, `Reverse()` - Sıralama işlemleri
- `Count` - Eleman sayısını öğrenme
- `Capacity` - Kapasite ayarlama

**Örnek Kullanım:**

```csharp
List<string> cities = new List<string>();
cities.Add("İstanbul");
cities.Add("Ankara");
cities.Add("İzmir");

// Tüm elemanları foreach ile dolaşma
foreach (string city in cities)
{
    Console.WriteLine(city);
}

// Lambda ile filtreleme
var filteredCities = cities.FindAll(c => c.StartsWith("İ"));

// Sıralama
cities.Sort();
```

**Performans Özellikleri:**
- Eleman ekleme ve sondan silme işlemleri O(1) (sabit süre) karmaşıklığındadır.
- Orta ya da baştan eleman silme işlemleri O(n) karmaşıklığındadır.
- Arama işlemleri O(n) karmaşıklığındadır.
- Dizinin kapasitesi dolduğunda yeniden boyutlandırma gerekir.

### Dictionary\<TKey, TValue>

`Dictionary<TKey, TValue>`, anahtar-değer çiftleri şeklinde veri depolayan bir koleksiyondur. Hashtable'ın generic versiyonudur.

**Özellikler ve Metodlar:**
- `Add()` - Anahtar-değer çifti ekleme
- `Remove()` - Anahtar ile eleman silme
- `ContainsKey()`, `ContainsValue()` - Anahtar veya değer kontrolü
- `TryGetValue()` - Güvenli değer alma
- `Keys`, `Values` - Anahtar ve değer koleksiyonlarına erişim
- İndeksleyici (`dictionary[key]`) ile değerlere erişim ve atama

**Örnek Kullanım:**

```csharp
Dictionary<string, int> population = new Dictionary<string, int>();
population.Add("İstanbul", 15462452);
population.Add("Ankara", 5503985);
population.Add("İzmir", 4367251);

// Değere erişim
int istanbulPopulation = population["İstanbul"];

// Güvenli değer alma
if (population.TryGetValue("Ankara", out int ankaraPopulation))
{
    Console.WriteLine($"Ankara nüfusu: {ankaraPopulation}");
}

// Tüm anahtar-değer çiftlerini dolaşma
foreach (KeyValuePair<string, int> city in population)
{
    Console.WriteLine($"{city.Key}: {city.Value}");
}

// Sadece anahtarları dolaşma
foreach (string cityName in population.Keys)
{
    Console.WriteLine(cityName);
}
```

**Performans Özellikleri:**
- Anahtar ile erişim, ekleme ve silme işlemleri ortalama O(1) karmaşıklığındadır.
- Hash çakışmaları durumunda performans düşebilir.
- Anahtarlar benzersiz olmalıdır.

### HashSet\<T>

`HashSet<T>`, benzersiz elemanları depolayan bir küme (set) veri yapısıdır. Matematikteki küme kavramını uygular.

**Özellikler ve Metodlar:**
- `Add()` - Eleman ekleme (aynı eleman tekrar eklenemez)
- `Remove()` - Eleman silme
- `Contains()` - Eleman kontrolü
- `UnionWith()`, `IntersectWith()`, `ExceptWith()` - Küme işlemleri
- `IsSubsetOf()`, `IsSupersetOf()` - Alt/üst küme kontrolü

**Örnek Kullanım:**

```csharp
HashSet<int> evenNumbers = new HashSet<int> { 2, 4, 6, 8, 10 };
HashSet<int> oddNumbers = new HashSet<int> { 1, 3, 5, 7, 9 };

// Birleşim
evenNumbers.UnionWith(oddNumbers);
// Şimdi evenNumbers kümesi: {2, 4, 6, 8, 10, 1, 3, 5, 7, 9}

// Yeni bir küme
HashSet<int> numbers = new HashSet<int> { 1, 2, 3, 4, 5 };
HashSet<int> otherNumbers = new HashSet<int> { 3, 4, 5, 6, 7 };

// Kesişim
numbers.IntersectWith(otherNumbers);
// Şimdi numbers kümesi: {3, 4, 5}
```

**Performans Özellikleri:**
- Ekleme, silme ve kontrol işlemleri ortalama O(1) karmaşıklığındadır.
- Küme işlemleri için etkin bir veri yapısıdır.
- Elemanların sırası korunmaz.

### Queue\<T>

`Queue<T>`, FIFO (First In, First Out - İlk Giren, İlk Çıkar) prensibiyle çalışan bir kuyruk veri yapısıdır.

**Özellikler ve Metodlar:**
- `Enqueue()` - Kuyruğa eleman ekleme
- `Dequeue()` - Kuyruktan eleman çıkarma (ve döndürme)
- `Peek()` - Kuyruktaki bir sonraki elemanı silmeden görme
- `Contains()` - Eleman kontrolü
- `Count` - Eleman sayısını öğrenme

**Örnek Kullanım:**

```csharp
Queue<string> printQueue = new Queue<string>();
printQueue.Enqueue("Document1.pdf");
printQueue.Enqueue("Photo.jpg");
printQueue.Enqueue("Report.docx");

while (printQueue.Count > 0)
{
    string nextItem = printQueue.Dequeue();
    Console.WriteLine($"Printing: {nextItem}");
}
```

**Performans Özellikleri:**
- Enqueue ve Dequeue işlemleri O(1) karmaşıklığındadır.
- Arama işlemleri O(n) karmaşıklığındadır.
- FIFO (ilk giren ilk çıkar) mantığına uygun uygulamalar için idealdir.

### Stack\<T>

`Stack<T>`, LIFO (Last In, First Out - Son Giren, İlk Çıkar) prensibiyle çalışan bir yığın veri yapısıdır.

**Özellikler ve Metodlar:**
- `Push()` - Yığına eleman ekleme
- `Pop()` - Yığından eleman çıkarma (ve döndürme)
- `Peek()` - Yığındaki üstteki elemanı silmeden görme
- `Contains()` - Eleman kontrolü
- `Count` - Eleman sayısını öğrenme

**Örnek Kullanım:**

```csharp
Stack<string> browserHistory = new Stack<string>();
browserHistory.Push("https://www.google.com");
browserHistory.Push("https://www.github.com");
browserHistory.Push("https://www.stackoverflow.com");

// Geri gitme işlemi
if (browserHistory.Count > 0)
{
    string currentPage = browserHistory.Pop();
    Console.WriteLine($"Current page: {currentPage}");
    
    if (browserHistory.Count > 0)
    {
        string previousPage = browserHistory.Peek();
        Console.WriteLine($"Going back to: {previousPage}");
    }
}
```

**Performans Özellikleri:**
- Push ve Pop işlemleri O(1) karmaşıklığındadır.
- Arama işlemleri O(n) karmaşıklığındadır.
- LIFO (son giren ilk çıkar) mantığına uygun uygulamalar için idealdir.

### LinkedList\<T>

`LinkedList<T>`, çift yönlü bağlı liste olarak çalışan bir koleksiyondur.

**Özellikler ve Metodlar:**
- `AddFirst()`, `AddLast()` - Başa veya sona eleman ekleme
- `AddBefore()`, `AddAfter()` - Belirtilen düğümün önüne veya arkasına ekleme
- `RemoveFirst()`, `RemoveLast()` - Baştaki veya sondaki elemanı silme
- `Remove()` - Belirtilen elemanı silme
- `Find()`, `FindLast()` - Eleman arama
- `First`, `Last` - İlk ve son düğümlere erişim

**Örnek Kullanım:**

```csharp
LinkedList<string> linkedList = new LinkedList<string>();
linkedList.AddLast("İstanbul");
linkedList.AddLast("Ankara");

// Başa eleman ekleme
linkedList.AddFirst("İzmir");

// Belirli bir konuma ekleme
LinkedListNode<string> ankaraNode = linkedList.Find("Ankara");
linkedList.AddAfter(ankaraNode, "Bursa");

// Tüm elemanları dolaşma
foreach (string city in linkedList)
{
    Console.WriteLine(city);
}
```

**Performans Özellikleri:**
- Listenin başına ve sonuna ekleme/silme işlemleri O(1) karmaşıklığındadır.
- Belirli bir konuma ekleme/silme işlemleri, konum biliniyorsa O(1), bilinmiyorsa O(n) karmaşıklığındadır.
- Arama işlemleri O(n) karmaşıklığındadır.
- Çok sayıda araya ekleme ve silme işlemi yapılacaksa List<T>'ye göre daha avantajlıdır.

### SortedList\<TKey, TValue>

`SortedList<TKey, TValue>`, anahtarlarına göre sıralanmış bir anahtar-değer koleksiyonudur.

**Özellikler ve Metodlar:**
- `Add()` - Anahtar-değer çifti ekleme
- `Remove()` - Anahtar ile eleman silme
- `ContainsKey()`, `ContainsValue()` - Anahtar veya değer kontrolü
- `IndexOfKey()`, `IndexOfValue()` - Anahtar veya değer indeksi alma
- `Keys`, `Values` - Anahtar ve değer koleksiyonlarına erişim
- İndeksleyici (`sortedList[key]`) ile değerlere erişim ve atama

**Örnek Kullanım:**

```csharp
SortedList<string, int> sortedScores = new SortedList<string, int>();
sortedScores.Add("Ahmet", 95);
sortedScores.Add("Zeynep", 98);
sortedScores.Add("Mehmet", 87);

// Otomatik olarak anahtarlara göre sıralanır (Ahmet, Mehmet, Zeynep)

foreach (KeyValuePair<string, int> score in sortedScores)
{
    Console.WriteLine($"{score.Key}: {score.Value}");
}
```

**Performans Özellikleri:**
- Ekleme ve silme işlemleri O(n) karmaşıklığındadır (sıralanmış durumu korumak için).
- Anahtar ile erişim işlemleri O(log n) karmaşıklığındadır (ikili arama).
- İndeks ile erişim işlemleri O(1) karmaşıklığındadır.
- Dictionary'ye göre daha yavaş, ancak sıralı veriler için kullanışlıdır.

### SortedDictionary\<TKey, TValue>

`SortedDictionary<TKey, TValue>`, anahtarlarına göre sıralanmış bir anahtar-değer koleksiyonudur. SortedList'e benzer, ancak farklı bir iç yapıya sahiptir.

**Özellikler ve Metodlar:**
- `Add()` - Anahtar-değer çifti ekleme
- `Remove()` - Anahtar ile eleman silme
- `ContainsKey()`, `ContainsValue()` - Anahtar veya değer kontrolü
- `TryGetValue()` - Güvenli değer alma
- `Keys`, `Values` - Anahtar ve değer koleksiyonlarına erişim
- İndeksleyici (`sortedDictionary[key]`) ile değerlere erişim ve atama

**Örnek Kullanım:**

```csharp
SortedDictionary<string, decimal> stockPrices = new SortedDictionary<string, decimal>();
stockPrices.Add("AAPL", 173.50m);
stockPrices.Add("GOOGL", 2830.72m);
stockPrices.Add("MSFT", 309.26m);

foreach (KeyValuePair<string, decimal> stock in stockPrices)
{
    Console.WriteLine($"{stock.Key}: ${stock.Value}");
}
```

**Performans Özellikleri:**
- Ekleme, silme ve arama işlemleri O(log n) karmaşıklığındadır.
- SortedList ile karşılaştırıldığında, sık ekleme/silme işlemleri için daha verimlidir.
- SortedList ise bellek kullanımı açısından daha verimlidir.

### SortedSet\<T>

`SortedSet<T>`, elemanları sıralı şekilde tutan ve benzersiz olan bir küme veri yapısıdır.

**Özellikler ve Metodlar:**
- `Add()` - Eleman ekleme (aynı eleman tekrar eklenemez)
- `Remove()` - Eleman silme
- `Contains()` - Eleman kontrolü
- `UnionWith()`, `IntersectWith()`, `ExceptWith()` - Küme işlemleri
- `Min`, `Max` - En küçük ve en büyük elemana erişim
- `GetViewBetween()` - Belirli bir aralıktaki elemanların görünümünü alma

**Örnek Kullanım:**

```csharp
SortedSet<int> sortedNumbers = new SortedSet<int> { 5, 2, 10, 1, 8 };
// sortedNumbers: {1, 2, 5, 8, 10}

// Aralık alma
SortedSet<int> subset = sortedNumbers.GetViewBetween(2, 8);
// subset: {2, 5, 8}

// Min ve Max değerleri
int minValue = sortedNumbers.Min; // 1
int maxValue = sortedNumbers.Max; // 10
```

**Performans Özellikleri:**
- Ekleme, silme ve arama işlemleri O(log n) karmaşıklığındadır.
- Sıralı ve benzersiz elemanlar gerektiren uygulamalar için idealdir.
- Kırmızı-siyah ağaç yapısı kullanılarak uygulanmıştır.

## Concurrent Koleksiyonlar

Concurrent koleksiyonlar, çok iş parçacıklı (multi-threaded) ortamlarda güvenli bir şekilde kullanılabilen koleksiyonlardır. `System.Collections.Concurrent` namespace'i altında bulunurlar.

### ConcurrentDictionary\<TKey, TValue>

`ConcurrentDictionary<TKey, TValue>`, çok iş parçacıklı ortamlarda güvenli bir şekilde kullanılabilen anahtar-değer çifti koleksiyonudur.

**Özellikler ve Metodlar:**
- `TryAdd()`, `TryUpdate()`, `TryRemove()` - Thread-safe ekleme, güncelleme ve silme
- `AddOrUpdate()` - Ekleme veya güncelleme
- `GetOrAdd()` - Anahtarı kontrol edip yoksa ekleme ve değeri döndürme
- Standart Dictionary yöntemlerine benzer diğer yöntemler

**Örnek Kullanım:**

```csharp
ConcurrentDictionary<string, int> concurrentDict = new ConcurrentDictionary<string, int>();

// Paralel olarak çalışan iş parçacıkları tarafından güvenle kullanılabilir
Parallel.For(0, 100, i =>
{
    string key = $"Key{i % 10}";
    concurrentDict.AddOrUpdate(key, 1, (k, oldValue) => oldValue + 1);
});

foreach (var item in concurrentDict)
{
    Console.WriteLine($"{item.Key}: {item.Value}");
}
```

### ConcurrentQueue\<T>

`ConcurrentQueue<T>`, çok iş parçacıklı ortamlarda güvenli bir şekilde kullanılabilen FIFO kuyruk yapısıdır.

**Özellikler ve Metodlar:**
- `Enqueue()` - Kuyruğa eleman ekleme
- `TryDequeue()` - Kuyruktan eleman çıkarmayı deneme
- `TryPeek()` - Kuyruktaki bir sonraki elemanı silmeden görmeyi deneme

**Örnek Kullanım:**

```csharp
ConcurrentQueue<string> taskQueue = new ConcurrentQueue<string>();

// Üretici iş parçacıkları
Parallel.For(0, 10, i =>
{
    taskQueue.Enqueue($"Task {i}");
});

// Tüketici iş parçacıkları
Parallel.For(0, 10, i =>
{
    if (taskQueue.TryDequeue(out string task))
    {
        Console.WriteLine($"Processing {task}");
    }
});
```

### ConcurrentBag\<T>

`ConcurrentBag<T>`, çok iş parçacıklı ortamlarda güvenli bir şekilde kullanılabilen, sıra garantisi olmayan bir koleksiyondur.

**Özellikler ve Metodlar:**
- `Add()` - Eleman ekleme
- `TryTake()` - Elemanı çıkarmayı deneme
- `TryPeek()` - Elemanı silmeden görmeyi deneme

**Örnek Kullanım:**

```csharp
ConcurrentBag<int> results = new ConcurrentBag<int>();

// Paralel hesaplama
Parallel.For(0, 100, i =>
{
    results.Add(i * i);
});

// Sonuçları toplama
int sum = 0;
while (results.TryTake(out int result))
{
    sum += result;
}
Console.WriteLine($"Sum: {sum}");
```

### ConcurrentStack\<T>

`ConcurrentStack<T>`, çok iş parçacıklı ortamlarda güvenli bir şekilde kullanılabilen LIFO yığın yapısıdır.

**Özellikler ve Metodlar:**
- `Push()`, `PushRange()` - Yığına eleman ekleme
- `TryPop()`, `TryPopRange()` - Yığından eleman çıkarmayı deneme
- `TryPeek()` - Yığındaki üstteki elemanı silmeden görmeyi deneme

**Örnek Kullanım:**

```csharp
ConcurrentStack<string> undoStack = new ConcurrentStack<string>();

// Birden çok iş parçacığı tarafından güvenli bir şekilde kullanılabilir
Parallel.For(0, 10, i =>
{
    undoStack.Push($"Action {i}");
});

string[] lastThreeActions = new string[3];
if (undoStack.TryPopRange(lastThreeActions, 0, 3) > 0)
{
    foreach (string action in lastThreeActions.Where(a => a != null))
    {
        Console.WriteLine($"Undoing: {action}");
    }
}
```

## ReadOnly Koleksiyonlar

ReadOnly koleksiyonlar, var olan bir koleksiyonun salt okunur görünümünü sağlar.

### ReadOnlyCollection\<T>

`ReadOnlyCollection<T>`, var olan bir listenin salt okunur bir görünümünü sağlar.

**Örnek Kullanım:**

```csharp
List<string> cities = new List<string> { "İstanbul", "Ankara", "İzmir" };
ReadOnlyCollection<string> readOnlyCities = new ReadOnlyCollection<string>(cities);

// readOnlyCities.Add("Bursa"); // Derleme hatası - Koleksiyon salt okunur
// Ancak asıl liste hala değiştirilebilir
cities.Add("Bursa");
// readOnlyCities artık "İstanbul", "Ankara", "İzmir", "Bursa" içerir
```

### ReadOnlyDictionary\<TKey, TValue>

`ReadOnlyDictionary<TKey, TValue>`, var olan bir sözlüğün salt okunur bir görünümünü sağlar.

**Örnek Kullanım:**

```csharp
Dictionary<string, int> ages = new Dictionary<string, int>
{
    { "Ahmet", 30 },
    { "Mehmet", 25 },
    { "Ayşe", 28 }
};

ReadOnlyDictionary<string, int> readOnlyAges = new ReadOnlyDictionary<string, int>(ages);

// readOnlyAges["Fatma"] = 22; // Derleme hatası - Koleksiyon salt okunur
// Ancak asıl sözlük hala değiştirilebilir
ages["Fatma"] = 22;
// readOnlyAges artık Fatma'yı da içerir
```

## Immutable Koleksiyonlar

Immutable (değiştirilemez) koleksiyonlar, oluşturulduktan sonra hiçbir şekilde değiştirilemeyen koleksiyonlardır. `System.Collections.Immutable` namespace'i altında bulunurlar.

**Temel Immutable Koleksiyonlar:**
- `ImmutableArray<T>`
- `ImmutableList<T>`
- `ImmutableDictionary<TKey, TValue>`
- `ImmutableHashSet<T>`
- `ImmutableQueue<T>`
- `ImmutableStack<T>`
- `ImmutableSortedDictionary<TKey, TValue>`
- `ImmutableSortedSet<T>`

**Örnek Kullanım:**

```csharp
// NuGet paketi gerektirir: System.Collections.Immutable
ImmutableList<string> immutableList = ImmutableList.Create<string>();
ImmutableList<string> newList = immutableList.Add("İstanbul").Add("Ankara").Add("İzmir");

// immutableList hala boştur
// newList "İstanbul", "Ankara", "İzmir" içerir

// Builder ile daha verimli oluşturma
ImmutableList<string>.Builder builder = ImmutableList.CreateBuilder<string>();
builder.Add("İstanbul");
builder.Add("Ankara");
builder.Add("İzmir");
ImmutableList<string> finalList = builder.ToImmutable();
```

## Koleksiyon Performans Karşılaştırması

| Koleksiyon Tipi          | Ekleme        | Silme         | Arama         | Erişim       | Diğer Özellikler               |
|--------------------------|---------------|---------------|---------------|--------------|--------------------------------|
| List\<T>                 | O(1) / O(n)*  | O(n)          | O(n)          | O(1)         | Dinamik dizi                   |
| Dictionary\<K,V>         | O(1)          | O(1)          | O(1)          | O(1)         | Hash tabanlı                   |
| HashSet\<T>              | O(1)          | O(1)          | O(1)          | -            | Benzersiz elemanlar            |
| Queue\<T>                | O(1)          | O(1)          | O(n)          | -            | FIFO yapısı                    |
| Stack\<T>                | O(1)          | O(1)          | O(n)          | -            | LIFO yapısı                    |
| LinkedList\<T>           | O(1)**        | O(1)**        | O(n)          | O(n)         | Çift yönlü bağlı liste         |
| SortedList\<K,V>         | O(n)          | O(n)          | O(log n)      | O(1)         | Sıralı, dizi tabanlı           |
| SortedDictionary\<K,V>   | O(log n)      | O(log n)      | O(log n)      | O(log n)     | Sıralı, ağaç tabanlı           |
| SortedSet\<T>            | O(log n)      | O(log n)      | O(log n)      | -            | Sıralı, benzersiz elemanlar    |

\* List için ekleme genellikle O(1)'dir, ancak kapasite artırımı gerektiğinde O(n) olur.  
\** LinkedList için ekleme ve silme, düğüm biliniyorsa O(1), bilinmiyorsa önce arama gerektiği için O(n)'dir.

## Custom Generic Koleksiyon Oluşturma

C#'ta kendi özel generic koleksiyonlarınızı oluşturabilirsiniz. Bunu yapmanın birkaç yolu vardır:

### 1. Generic Sınıf Oluşturma

```csharp
public class CircularBuffer<T>
{
    private T[] _buffer;
    private int _start;
    private int _end;
    private int _count;
    private readonly int _capacity;
    
    public CircularBuffer(int capacity)
    {
        _buffer = new T[capacity];
        _capacity = capacity;
        _start = 0;
        _end = 0;
        _count = 0;
    }
    
    public void Add(T item)
    {
        if (_count == _capacity)
        {
            // Dairesel tampon dolu ise, en eski öğeyi üzerine yazacak
            _buffer[_end] = item;
            _end = (_end + 1) % _capacity;
            _start = (_start + 1) % _capacity;
        }
        else
        {
            // Tampon dolu değilse, yeni öğe ekleyecek
            _buffer[_end] = item;
            _end = (_end + 1) % _capacity;
            _count++;
        }
    }
    
    public T Get(int index)
    {
        if (index < 0 || index >= _count)
        {
            throw new IndexOutOfRangeException();
        }
        
        int actualIndex = (_start + index) % _capacity;
        return _buffer[actualIndex];
    }
    
    public int Count => _count;
}
```

### 2. Var Olan Koleksiyonları Genişletme

```csharp
public class ObservableList<T> : List<T>
{
    public event EventHandler<T> ItemAdded;
    public event EventHandler<T> ItemRemoved;
    
    public new void Add(T item)
    {
        base.Add(item);
        ItemAdded?.Invoke(this, item);
    }
    
    public new bool Remove(T item)
    {
        bool result = base.Remove(item);
        if (result)
        {
            ItemRemoved?.Invoke(this, item);
        }
        return result;
    }
}
```

### 3. Collection\<T> Sınıfından Türetme

```csharp
public class UniqueList<T> : Collection<T>
{
    protected override void InsertItem(int index, T item)
    {
        if (Contains(item))
        {
            throw new ArgumentException("Koleksiyon zaten bu öğeyi içeriyor.");
        }
        
        base.InsertItem(index, item);
    }
    
    protected override void SetItem(int index, T item)
    {
        if (Items.IndexOf(item) != -1 && Items.IndexOf(item) != index)
        {
            throw new ArgumentException("Koleksiyon zaten bu öğeyi içeriyor.");
        }
        
        base.SetItem(index, item);
    }
}
```

### 4. IEnumerable\<T> ve ICollection\<T> Arayüzlerini Uygulama

```csharp
public class PriorityQueue<T> : IEnumerable<T>, ICollection<T> where T : IComparable<T>
{
    private readonly List<T> _items = new List<T>();
    
    public void Add(T item)
    {
        _items.Add(item);
        _items.Sort(); // Basit implementasyon - her ekleme işleminde sıralama yapılıyor
    }
    
    public T Peek()
    {
        if (_items.Count == 0)
            throw new InvalidOperationException("Kuyruk boş");
            
        return _items[0];
    }
    
    public T Dequeue()
    {
        if (_items.Count == 0)
            throw new InvalidOperationException("Kuyruk boş");
            
        T item = _items[0];
        _items.RemoveAt(0);
        return item;
    }
    
    // ICollection<T> implementasyonu
    public void Clear() => _items.Clear();
    public bool Contains(T item) => _items.Contains(item);
    public void CopyTo(T[] array, int arrayIndex) => _items.CopyTo(array, arrayIndex);
    public int Count => _items.Count;
    public bool IsReadOnly => false;
    public bool Remove(T item) => _items.Remove(item);
    
    // IEnumerable<T> implementasyonu
    public IEnumerator<T> GetEnumerator() => _items.GetEnumerator();
    System.Collections.IEnumerator System.Collections.IEnumerable.GetEnumerator() => _items.GetEnumerator();
}
```

## LINQ ile Generic Koleksiyonlar

LINQ (Language Integrated Query), koleksiyonlar üzerinde sorgulamalar yapmayı sağlayan güçlü bir özelliktir. Generic koleksiyonlarla birlikte kullanıldığında çok daha etkilidir.

### Temel LINQ Operatörleri

```csharp
List<Student> students = new List<Student>
{
    new Student { Id = 1, Name = "Ali", Grade = 85 },
    new Student { Id = 2, Name = "Ayşe", Grade = 92 },
    new Student { Id = 3, Name = "Mehmet", Grade = 78 },
    new Student { Id = 4, Name = "Zeynep", Grade = 95 },
    new Student { Id = 5, Name = "Can", Grade = 68 }
};

// Filtreleme
var highGradeStudents = students.Where(s => s.Grade >= 85);

// Sıralama
var sortedByGrade = students.OrderByDescending(s => s.Grade);

// Seçme ve Dönüştürme
var studentNames = students.Select(s => s.Name);

// İlk/Tek Eleman Bulma
var topStudent = students.OrderByDescending(s => s.Grade).First();
var student3 = students.SingleOrDefault(s => s.Id == 3);

// Gruplama
var groupedByGradeRange = students.GroupBy(s => s.Grade >= 85 ? "Başarılı" : "Geliştirilmeli");

// Agregasyon
var averageGrade = students.Average(s => s.Grade);
var sumOfGrades = students.Sum(s => s.Grade);
var totalStudents = students.Count();
```

### Method Zinciri ve Sorgular

LINQ, akıcı bir API ile zincirleme metodlar oluşturmayı destekler:

```csharp
// Method Zinciri Sözdizimi
var results = students
    .Where(s => s.Grade >= 70)
    .OrderBy(s => s.Name)
    .Select(s => new { s.Name, GradeLevel = s.Grade >= 90 ? "A" : "B" });

// Sorgu Sözdizimi
var query = from s in students
            where s.Grade >= 70
            orderby s.Name
            select new { s.Name, GradeLevel = s.Grade >= 90 ? "A" : "B" };
```

### Lazy Evaluation ve Ertelenmiş Yürütme

LINQ sorgularının çoğu, sorguyu tanımladığınızda değil, sonuçlar üzerinde yineleme yaptığınızda yürütülür:

```csharp
// Sorgu tanımlanır, ancak henüz yürütülmez
var query = students.Where(s => s.Grade > 80);

// List'e ekleme yapalım
students.Add(new Student { Id = 6, Name = "Emir", Grade = 88 });

// Sorgu şimdi yürütülür ve Emir de sonuçlarda yer alır
foreach (var student in query)
{
    Console.WriteLine(student.Name);
}

// Sonuçları hemen almak için ToList(), ToArray() gibi metodlar kullanılabilir
var immediateResults = students.Where(s => s.Grade > 80).ToList();
```

## En İyi Uygulama Önerileri

### 1. Doğru Koleksiyon Seçimi

- Basit liste işlemleri için `List<T>` kullanın.
- Anahtar-değer çiftleri için `Dictionary<TKey, TValue>` kullanın.
- Benzersiz elemanlar için `HashSet<T>` kullanın.
- FIFO (ilk giren ilk çıkar) işlemleri için `Queue<T>` kullanın.
- LIFO (son giren ilk çıkar) işlemleri için `Stack<T>` kullanın.
- Sık araya ekleme/silme işlemleri varsa `LinkedList<T>` kullanın.
- Sıralı veri yapıları için `SortedList<TKey, TValue>` veya `SortedDictionary<TKey, TValue>` kullanın.
- Çok iş parçacıklı uygulamalarda concurrent koleksiyonları kullanın.

### 2. Kapasite Yönetimi

```csharp
// Başlangıç kapasitesini ayarlama
List<int> numbers = new List<int>(10000);

// Dictionary için başlangıç kapasitesi ve yük faktörü
Dictionary<string, string> dict = new Dictionary<string, string>(10000, 0.8f);
```

### 3. Generic Kısıtlamalar (Constraints)

```csharp
// Referans tipleri için kısıtlama
public class Repository<T> where T : class { }

// Değer tipleri için kısıtlama
public class Calculator<T> where T : struct { }

// Belirli bir sınıftan türetme kısıtlaması
public class AnimalShelter<T> where T : Animal { }

// Constructor kısıtlaması
public class Factory<T> where T : new() { }

// Interface kısıtlaması
public class Sorter<T> where T : IComparable<T> { }

// Birden fazla kısıtlama
public class Repository<T> where T : class, IEntity, new() { }
```

### 4. İterasyon Optimizasyonu

```csharp
// foreach yerine for döngüsü kullanma (List için)
List<int> numbers = new List<int>();
for (int i = 0; i < numbers.Count; i++)
{
    // İndeks erişimi O(1) karmaşıklığındadır
}

// LINQ ile döngü yerine FirstOrDefault() kullanma
var item = list.FirstOrDefault(x => x.Id == 5);

// Birçok öğe eklerken veya çıkarırken kapasiteyi önceden ayarlama
List<int> numbers = new List<int>(10000);
for (int i = 0; i < 10000; i++)
{
    numbers.Add(i);
}
```

### 5. ReadOnly Koleksiyonlar

```csharp
// Public API'lerde dönüş değeri olarak readOnly koleksiyonları kullanma
public IReadOnlyList<Customer> GetCustomers()
{
    return _customers.AsReadOnly();
}

// Koleksiyonu değiştirme yetkisini sınırlama
public IReadOnlyDictionary<string, int> Scores => _scores;
```

## Sık Yapılan Hatalar

### 1. Uygun Olmayan Koleksiyon Seçimi

❌ **Yanlış:**
```csharp
// Sık arama işlemleri için List kullanımı
List<Customer> customers = new List<Customer>();
Customer customer = customers.FirstOrDefault(c => c.Id == "12345");
```

✅ **Doğru:**
```csharp
// Arama işlemleri için Dictionary kullanımı
Dictionary<string, Customer> customers = new Dictionary<string, Customer>();
if (customers.TryGetValue("12345", out Customer customer)) 
{
    // Customer bulundu
}
```

### 2. Koleksiyon Üzerinde İterasyon Sırasında Değişiklik Yapma

❌ **Yanlış:**
```csharp
// foreach döngüsü sırasında koleksiyondan eleman silme
foreach (var item in list)
{
    if (SomeCondition(item))
    {
        list.Remove(item); // InvalidOperationException fırlatır
    }
}
```

✅ **Doğru:**
```csharp
// Geçici bir liste oluşturma veya tersten iterasyon
var itemsToRemove = list.Where(item => SomeCondition(item)).ToList();
foreach (var item in itemsToRemove)
{
    list.Remove(item);
}

// Veya
for (int i = list.Count - 1; i >= 0; i--)
{
    if (SomeCondition(list[i]))
    {
        list.RemoveAt(i);
    }
}
```

### 3. Dictionary Anahtarlarının Değiştirilmesi

❌ **Yanlış:**
```csharp
// Dictionary'de anahtar olarak kullanılan mutable nesneler
Dictionary<Person, string> personAddresses = new Dictionary<Person, string>();
Person person = new Person { Id = 1, Name = "Ali" };
personAddresses.Add(person, "İstanbul");

// Person nesnesinin durumunu değiştirme
person.Name = "Veli";
// Artık bu kişiyi Dictionary'de bulamayız!
string address = personAddresses[person]; // Beklenmeyen davranış!
```

✅ **Doğru:**
```csharp
// Dictionary için değişmez (immutable) anahtarlar kullanma
Dictionary<int, string> personAddresses = new Dictionary<int, string>();
Person person = new Person { Id = 1, Name = "Ali" };
personAddresses.Add(person.Id, "İstanbul");

// Veya özel bir IEqualityComparer<T> uygulama
Dictionary<Person, string> personAddresses = new Dictionary<Person, string>(
    new PersonByIdComparer());
```

### 4. Generic Koleksiyonlar Yerine Non-Generic Koleksiyonlar Kullanma

❌ **Yanlış:**
```csharp
// ArrayList kullanımı
ArrayList list = new ArrayList();
list.Add("İstanbul");
list.Add(42); // Farklı tipteki nesneler eklenebilir
string city = (string)list[0]; // Cast gerekli
```

✅ **Doğru:**
```csharp
// Generic List<T> kullanımı
List<string> cities = new List<string>();
cities.Add("İstanbul");
// cities.Add(42); // Derleme hatası - tip güvenliği
string city = cities[0]; // Cast gerekli değil
```

### 5. Uygun Boyutlandırma Yapmama

❌ **Yanlış:**
```csharp
// Boyut ayarlamadan büyük miktarda veri ekleme
List<int> numbers = new List<int>();
for (int i = 0; i < 1000000; i++)
{
    numbers.Add(i); // Birçok yeniden boyutlandırma gerçekleşir
}
```

✅ **Doğru:**
```csharp
// Başlangıç kapasitesini ayarlama
List<int> numbers = new List<int>(1000000);
for (int i = 0; i < 1000000; i++)
{
    numbers.Add(i); // Yeniden boyutlandırma yok
}
```

### 6. Null Kontrolü Yapılmaması

❌ **Yanlış:**
```csharp
Dictionary<string, List<int>> groupedNumbers = new Dictionary<string, List<int>>();
groupedNumbers["even"].Add(2); // NullReferenceException fırlatır
```

✅ **Doğru:**
```csharp
Dictionary<string, List<int>> groupedNumbers = new Dictionary<string, List<int>>();
if (!groupedNumbers.ContainsKey("even"))
{
    groupedNumbers["even"] = new List<int>();
}
groupedNumbers["even"].Add(2);

// Veya daha temiz bir şekilde
if (!groupedNumbers.TryGetValue("even", out List<int> evenNumbers))
{
    evenNumbers = new List<int>();
    groupedNumbers["even"] = evenNumbers;
}
evenNumbers.Add(2);

// .NET 6+ için
groupedNumbers.TryAdd("even", new List<int>());
groupedNumbers["even"].Add(2);
```

## Sonuç

C# generic koleksiyonlar, tip güvenliği ve yüksek performans sağlayan güçlü veri yapılarıdır. Farklı senaryolar için tasarlanmış çeşitli koleksiyon türleri mevcuttur. Doğru koleksiyon türünü seçmek, uygulamanın performansını ve kod kalitesini önemli ölçüde etkileyebilir.

Bu rehberde incelediğimiz temel generic koleksiyonlar, concurrent koleksiyonlar, ReadOnly ve Immutable koleksiyonlar, özel koleksiyon oluşturma yöntemleri ve en iyi uygulama önerileri, C# uygulamalarınızda veri yönetimini optimize etmenize yardımcı olacaktır.

Generic koleksiyonların doğru kullanımı, kodunuzu daha güvenli, daha hızlı ve daha bakımı kolay hale getirecektir.
