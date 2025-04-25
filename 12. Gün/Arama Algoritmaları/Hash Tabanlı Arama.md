# C# Hash Tabanlı Arama

## İçindekiler
1. [Giriş](#giriş)
2. [Hash Fonksiyonları](#hash-fonksiyonları)
3. [C#'ta Hash Tabanlı Veri Yapıları](#cta-hash-tabanlı-veri-yapıları)
   - [Dictionary](#dictionary)
   - [HashSet](#hashset)
   - [ConcurrentDictionary](#concurrentdictionary)
4. [Kendi Hash Fonksiyonunuzu Oluşturma](#kendi-hash-fonksiyonunuzu-oluşturma)
   - [GetHashCode() Metodu](#gethashcode-metodu)
   - [IEqualityComparer Arabirimi](#iequalitycomparer-arabirimi)
5. [Hash Çakışmaları (Collision)](#hash-çakışmaları-collision)
   - [Çakışma Çözümleme Teknikleri](#çakışma-çözümleme-teknikleri)
6. [Performans Analizi](#performans-analizi)
7. [Örnek Uygulamalar](#örnek-uygulamalar)
   - [Basit Kelime Sayacı](#basit-kelime-sayacı)
   - [Özel Nesne İçin Hash Tabanlı Arama](#özel-nesne-için-hash-tabanlı-arama)
   - [Çakışma Yönetimi Örneği](#çakışma-yönetimi-örneği)
8. [En İyi Uygulamalar ve Tavsiyeler](#en-iyi-uygulamalar-ve-tavsiyeler)
9. [Sık Karşılaşılan Sorunlar ve Çözümleri](#sık-karşılaşılan-sorunlar-ve-çözümleri)
10. [Sonuç](#sonuç)

## Giriş

Hash tabanlı arama, büyük veri setlerinde hızlı arama yapmak için kullanılan etkili bir tekniktir. Geleneksel doğrusal arama algoritmaları O(n) zaman karmaşıklığına sahipken, hash tabanlı yapılar teorik olarak O(1) - sabit zamanlı - arama karmaşıklığı sunar.

Bu yapılar, anahtar-değer çiftlerini saklamak ve anahtarı kullanarak değere hızlı bir şekilde erişmek için ideal veri yapılarıdır. C# dili ve .NET platformu, hash tabanlı veri yapıları için zengin bir kütüphane sunar.

## Hash Fonksiyonları

Hash fonksiyonu, herhangi bir boyuttaki veriyi (anahtar) sabit boyutlu bir değere (hash değeri) dönüştüren bir fonksiyondur. İyi bir hash fonksiyonu aşağıdaki özelliklere sahip olmalıdır:

- **Deterministik**: Aynı giriş her zaman aynı çıktıyı üretmelidir.
- **Hızlı hesaplanabilir**: Hash değeri hesaplama işlemi hızlı olmalıdır.
- **Düzgün dağılım**: Hash değerleri, hash tablosunda düzgün dağılmalıdır.
- **Çakışma direnci**: Farklı girişler için aynı hash değeri üretme olasılığı düşük olmalıdır.

.NET çerçevesinde, her nesne varsayılan olarak bir `GetHashCode()` metodu içerir. Bu metot, nesnenin hash değerini döndürür ve hash tabanlı veri yapılarında kullanılır.

## C#'ta Hash Tabanlı Veri Yapıları

### Dictionary

`Dictionary<TKey, TValue>` sınıfı, C#'ta en yaygın kullanılan hash tabanlı veri yapısıdır. Anahtar-değer çiftlerini saklar ve anahtarlara göre hızlı arama yapar.

```csharp
// Dictionary örneği
Dictionary<string, int> ages = new Dictionary<string, int>();

// Eleman ekleme
ages.Add("Alice", 30);
ages["Bob"] = 25;  // Alternatif syntax

// Arama
if (ages.ContainsKey("Alice"))
{
    Console.WriteLine($"Alice'in yaşı: {ages["Alice"]}");
}

// Güvenli arama
if (ages.TryGetValue("Charlie", out int charlieAge))
{
    Console.WriteLine($"Charlie'nin yaşı: {charlieAge}");
}
else
{
    Console.WriteLine("Charlie bulunamadı.");
}

// Dolaşma
foreach (KeyValuePair<string, int> pair in ages)
{
    Console.WriteLine($"{pair.Key}: {pair.Value}");
}
```

### HashSet

`HashSet<T>` sınıfı, benzersiz elemanların bir koleksiyonunu temsil eder. Matematiksel küme işlemlerini destekler ve elemanlara hızlı erişim sağlar.

```csharp
// HashSet örneği
HashSet<string> fruits = new HashSet<string>();

// Eleman ekleme
fruits.Add("Elma");
fruits.Add("Muz");
fruits.Add("Portakal");

// Aynı elemanı tekrar eklemek etkisizdir
bool added = fruits.Add("Elma");  // added = false

// Hızlı arama
if (fruits.Contains("Muz"))
{
    Console.WriteLine("Muz bulundu!");
}

// Küme işlemleri
HashSet<string> moreFruits = new HashSet<string> { "Çilek", "Portakal", "Kivi" };

// Birleşim
fruits.UnionWith(moreFruits);  // Elma, Muz, Portakal, Çilek, Kivi

// Kesişim
fruits.IntersectWith(moreFruits);  // Sadece Portakal

// Fark
fruits.ExceptWith(moreFruits);  // Portakal kalkar
```

### ConcurrentDictionary

Çok iş parçacıklı uygulamalar için, `ConcurrentDictionary<TKey, TValue>` sınıfı thread-safe bir hash tabanlı sözlük sunar.

```csharp
// ConcurrentDictionary örneği
ConcurrentDictionary<string, int> concurrentAges = new ConcurrentDictionary<string, int>();

// Eleman ekleme
concurrentAges.TryAdd("Alice", 30);
concurrentAges["Bob"] = 25;  // Otomatik eklenir veya güncellenir

// Atomik güncelleme işlemleri
concurrentAges.AddOrUpdate("Alice", 
    addValue: 31,  // Anahtar mevcut değilse
    updateValueFactory: (key, oldValue) => oldValue + 1);  // Anahtar mevcutsa

// Thread-safe işlemler
Parallel.For(0, 100, i =>
{
    concurrentAges.AddOrUpdate("Counter", 1, (k, v) => v + 1);
});
```

## Kendi Hash Fonksiyonunuzu Oluşturma

### GetHashCode() Metodu

Özel bir sınıf için hash tabanlı yapılarda kullanılacaksa, `GetHashCode()` metodunu ve `Equals()` metodunu override etmeniz gerekir.

```csharp
public class Person
{
    public string FirstName { get; set; }
    public string LastName { get; set; }
    public DateTime BirthDate { get; set; }
    
    public override bool Equals(object obj)
    {
        if (obj == null || GetType() != obj.GetType())
            return false;
        
        Person other = (Person)obj;
        return FirstName == other.FirstName && 
               LastName == other.LastName && 
               BirthDate == other.BirthDate;
    }
    
    public override int GetHashCode()
    {
        // .NET Core 2.1+ ve .NET 5+ için HashCode yapısını kullanın
        return HashCode.Combine(FirstName, LastName, BirthDate);
        
        // Alternatif olarak manuel hesaplama:
        // unchecked
        // {
        //     int hash = 17;
        //     hash = hash * 23 + (FirstName?.GetHashCode() ?? 0);
        //     hash = hash * 23 + (LastName?.GetHashCode() ?? 0);
        //     hash = hash * 23 + BirthDate.GetHashCode();
        //     return hash;
        // }
    }
}
```

### IEqualityComparer Arabirimi

Sınıfınızın kodunu değiştiremiyorsanız veya özel karşılaştırma mantığı uygulamak istiyorsanız, `IEqualityComparer<T>` arabirimini uygulayabilirsiniz.

```csharp
public class PersonComparer : IEqualityComparer<Person>
{
    public bool Equals(Person x, Person y)
    {
        if (x == null && y == null)
            return true;
        if (x == null || y == null)
            return false;
        
        return x.FirstName == y.FirstName && 
               x.LastName == y.LastName;
        // BirthDate'i dikkate almıyoruz - sadece isim ve soyisim karşılaştırması
    }
    
    public int GetHashCode(Person obj)
    {
        if (obj == null)
            return 0;
        
        return HashCode.Combine(obj.FirstName, obj.LastName);
    }
}

// Kullanım:
Dictionary<Person, string> personMap = new Dictionary<Person, string>(new PersonComparer());
```

## Hash Çakışmaları (Collision)

Hash çakışması, iki farklı anahtarın aynı hash değerini üretmesi durumudur. Bu durum kaçınılmazdır çünkü hash fonksiyonu sonsuz sayıda potansiyel girişi sonlu sayıda çıkışa haritalandırır.

### Çakışma Çözümleme Teknikleri

.NET'in Dictionary ve HashSet sınıfları, çakışmaları çözmek için **ayrı zincirleme (separate chaining)** yöntemini kullanır. Bu yöntemde, aynı hash değerine sahip elemanlar bir bağlı listede saklanır.

C#'ta hash tabanlı veri yapılarının iç yapısı şu şekildedir:

1. Hash değeri hesaplanır.
2. Hash değeri, tablonun boyutuna modüler bölünür (hash % size) ve bir dizin elde edilir.
3. Bu dizindeki bağlı listede anahtar aranır.
4. Çakışma durumunda, bağlı listedeki tüm elemanlar `Equals()` metodu kullanılarak karşılaştırılır.

Bu nedenle, `GetHashCode()` ve `Equals()` metodlarının doğru şekilde uygulanması çok önemlidir.

## Performans Analizi

Hash tabanlı arama performansı genellikle mükemmeldir:

- **Arama, Ekleme, Silme**: O(1) (ortalama durum)
- **En Kötü Durum**: O(n) (tüm anahtarlar aynı hash değerini üretirse)

Büyük veri setleri için, hash tabanlı yapıların performansı diğer arama yöntemlerinden önemli ölçüde daha iyidir:

| İşlem | Array (doğrusal) | Sorted Array (ikili) | Dictionary (hash) |
|-------|-----------------|---------------------|-------------------|
| Arama | O(n)            | O(log n)            | O(1)              |
| Ekleme | O(1)           | O(n)                | O(1)              |
| Silme | O(n)            | O(n)                | O(1)              |

## Örnek Uygulamalar

### Basit Kelime Sayacı

```csharp
public static Dictionary<string, int> CountWords(string text)
{
    Dictionary<string, int> wordCount = new Dictionary<string, int>(StringComparer.OrdinalIgnoreCase);
    
    // Metni kelimelere böl
    string[] words = text.Split(new[] { ' ', '.', ',', ';', ':', '!', '?' }, 
                                StringSplitOptions.RemoveEmptyEntries);
    
    foreach (string word in words)
    {
        if (wordCount.ContainsKey(word))
        {
            wordCount[word]++;
        }
        else
        {
            wordCount[word] = 1;
        }
        
        // Alternatif olarak:
        // wordCount[word] = wordCount.TryGetValue(word, out int count) ? count + 1 : 1;
    }
    
    return wordCount;
}

// Kullanım
string text = "Hash tabanlı arama çok hızlıdır. Hash tabanlı veri yapıları C#'ta yaygın olarak kullanılır.";
Dictionary<string, int> counts = CountWords(text);

foreach (var pair in counts.OrderByDescending(p => p.Value))
{
    Console.WriteLine($"{pair.Key}: {pair.Value}");
}
```

### Özel Nesne İçin Hash Tabanlı Arama

```csharp
public class Student
{
    public int Id { get; set; }
    public string Name { get; set; }
    public string Department { get; set; }
    
    public override bool Equals(object obj)
    {
        if (obj == null || GetType() != obj.GetType())
            return false;
        
        Student other = (Student)obj;
        return Id == other.Id;
    }
    
    public override int GetHashCode()
    {
        return Id.GetHashCode();
    }
    
    public override string ToString()
    {
        return $"{Id} - {Name} ({Department})";
    }
}

public class StudentDatabase
{
    private Dictionary<int, Student> _studentsById;
    private Dictionary<string, HashSet<Student>> _studentsByDepartment;
    
    public StudentDatabase()
    {
        _studentsById = new Dictionary<int, Student>();
        _studentsByDepartment = new Dictionary<string, HashSet<Student>>(StringComparer.OrdinalIgnoreCase);
    }
    
    public void AddStudent(Student student)
    {
        // ID'ye göre ekle
        _studentsById[student.Id] = student;
        
        // Bölüme göre ekle
        if (!_studentsByDepartment.TryGetValue(student.Department, out HashSet<Student> students))
        {
            students = new HashSet<Student>();
            _studentsByDepartment[student.Department] = students;
        }
        
        students.Add(student);
    }
    
    public Student GetStudentById(int id)
    {
        if (_studentsById.TryGetValue(id, out Student student))
            return student;
        
        return null;
    }
    
    public IEnumerable<Student> GetStudentsByDepartment(string department)
    {
        if (_studentsByDepartment.TryGetValue(department, out HashSet<Student> students))
            return students;
        
        return Enumerable.Empty<Student>();
    }
}

// Kullanım
var db = new StudentDatabase();

db.AddStudent(new Student { Id = 1, Name = "Ali", Department = "Bilgisayar Mühendisliği" });
db.AddStudent(new Student { Id = 2, Name = "Ayşe", Department = "Elektrik Mühendisliği" });
db.AddStudent(new Student { Id = 3, Name = "Mehmet", Department = "Bilgisayar Mühendisliği" });

// ID ile arama
Student student = db.GetStudentById(2);
Console.WriteLine(student);  // 2 - Ayşe (Elektrik Mühendisliği)

// Bölüme göre arama
var compSciStudents = db.GetStudentsByDepartment("Bilgisayar Mühendisliği");
foreach (var s in compSciStudents)
{
    Console.WriteLine(s);  // 1 - Ali (Bilgisayar Mühendisliği), 3 - Mehmet (Bilgisayar Mühendisliği)
}
```

### Çakışma Yönetimi Örneği

```csharp
public class BadHashExample
{
    public string Value { get; set; }
    
    // Kötü bir hash fonksiyonu - tüm değerler için aynı hash değerini döndürür
    public override int GetHashCode()
    {
        return 42;  // Tüm nesneler için aynı değer! Kaçınılması gereken durum
    }
    
    public override bool Equals(object obj)
    {
        if (obj == null || GetType() != obj.GetType())
            return false;
        
        return Value == ((BadHashExample)obj).Value;
    }
}

// Performans testi
public static void HashCollisionTest()
{
    // Standart string dictionary - iyi hash fonksiyonu
    Dictionary<string, int> goodDict = new Dictionary<string, int>();
    
    // Kötü hash fonksiyonlu dictionary
    Dictionary<BadHashExample, int> badDict = new Dictionary<BadHashExample, int>();
    
    int itemCount = 10000;
    
    // Zamanlamayı ölç
    Stopwatch sw = new Stopwatch();
    
    // İyi hash fonksiyonu ile doldur
    sw.Start();
    for (int i = 0; i < itemCount; i++)
    {
        goodDict[$"Item{i}"] = i;
    }
    sw.Stop();
    Console.WriteLine($"Good hash fill time: {sw.ElapsedMilliseconds}ms");
    
    // Kötü hash fonksiyonu ile doldur
    sw.Restart();
    for (int i = 0; i < itemCount; i++)
    {
        badDict[new BadHashExample { Value = $"Item{i}" }] = i;
    }
    sw.Stop();
    Console.WriteLine($"Bad hash fill time: {sw.ElapsedMilliseconds}ms");
    
    // İyi hash fonksiyonu ile ara
    sw.Restart();
    for (int i = 0; i < itemCount; i++)
    {
        int value = goodDict[$"Item{i}"];
    }
    sw.Stop();
    Console.WriteLine($"Good hash lookup time: {sw.ElapsedMilliseconds}ms");
    
    // Kötü hash fonksiyonu ile ara
    sw.Restart();
    for (int i = 0; i < itemCount; i++)
    {
        int value = badDict[new BadHashExample { Value = $"Item{i}" }];
    }
    sw.Stop();
    Console.WriteLine($"Bad hash lookup time: {sw.ElapsedMilliseconds}ms");
}
```

## En İyi Uygulamalar ve Tavsiyeler

1. **GetHashCode() ve Equals() Uyumluluğu**: Eğer `GetHashCode()` metodunu override ederseniz, mutlaka `Equals()` metodunu da override edin. İki metot birbiriyle uyumlu olmalıdır - eşit nesneler aynı hash kodlarına sahip olmalıdır.

2. **Değişmezlik**: Mümkünse, hash kodu hesaplamasında kullanılan alanlar değişmez (immutable) olmalıdır. Bir nesnenin hash kodu değişirse ve nesne bir hash tablosunda bir anahtar olarak kullanılıyorsa, nesne kaybolabilir.

3. **Hash Fonksiyonu Performansı**: Hash fonksiyonu hesaplamalarınız hızlı olmalıdır. Karmaşık hesaplamalar, hash tabanlı veri yapılarının performans avantajlarını azaltabilir.

4. **Çakışmaları Azaltın**: İyi bir hash fonksiyonu, nesneleri hash tablosunda düzgün bir şekilde dağıtmalıdır. Çakışmaları minimize etmek için, hash kodlarınızın iyi dağılımlı olmasını sağlayın.

5. **Dictionary Kapasitesi**: Büyük Dictionary'ler için, başlangıç kapasitesini önceden belirlemek performansı artırabilir. Bu, yeniden boyutlandırma işlemlerini azaltır.

    ```csharp
    // Başlangıç kapasitesini belirle
    Dictionary<string, int> largeDict = new Dictionary<string, int>(initialCapacity: 100000);
    ```

6. **ReadOnlyDictionary Kullanımı**: İmmutable bir sözlük için `ReadOnlyDictionary<TKey, TValue>` sınıfını kullanın.

    ```csharp
    Dictionary<string, int> dict = new Dictionary<string, int>();
    // dict'i doldur...
    
    // Salt okunur sürüm oluştur
    var readOnlyDict = new ReadOnlyDictionary<string, int>(dict);
    ```

7. **StringComparer Belirtme**: String anahtarları için, uygun bir `StringComparer` kullanarak kültür ve büyük/küçük harf duyarlılığını kontrol edin.

    ```csharp
    // Büyük/küçük harf duyarsız sözlük
    Dictionary<string, int> caseInsensitiveDict = new Dictionary<string, int>(StringComparer.OrdinalIgnoreCase);
    ```

## Sık Karşılaşılan Sorunlar ve Çözümleri

1. **KeyNotFoundException**

    ```csharp
    // Problem: Olmayan bir anahtara erişim
    Dictionary<string, int> dict = new Dictionary<string, int>();
    int value = dict["nonexistent"];  // KeyNotFoundException fırlatır
    
    // Çözüm: TryGetValue kullanın
    if (dict.TryGetValue("nonexistent", out int result))
    {
        // Anahtar bulundu
        Console.WriteLine(result);
    }
    else
    {
        // Anahtar bulunamadı
        Console.WriteLine("Anahtar bulunamadı");
    }
    
    // Alternatif: ContainsKey kullanın
    if (dict.ContainsKey("nonexistent"))
    {
        Console.WriteLine(dict["nonexistent"]);
    }
    ```

2. **Değiştirilebilir Anahtarlar Sorunu**

    ```csharp
    // Problem: Değiştirilebilir anahtarlar
    public class MutableKey
    {
        public string Value { get; set; }
        
        public override int GetHashCode() => Value.GetHashCode();
        public override bool Equals(object obj) => obj is MutableKey other && Value == other.Value;
    }
    
    Dictionary<MutableKey, string> dict = new Dictionary<MutableKey, string>();
    var key = new MutableKey { Value = "original" };
    dict[key] = "data";
    
    // Anahtarı değiştir
    key.Value = "modified";
    
    // Artık anahtarı bulamayız
    Console.WriteLine(dict.ContainsKey(key));  // False
    
    // Çözüm: Değişmez (immutable) anahtarlar kullanın veya anahtarları değiştirmeyin
    ```

3. **Tekrarlayan Hash Hesaplamaları**

    ```csharp
    // Problem: Karmaşık nesnelerde tekrarlayan hash hesaplamaları
    class ComplexObject
    {
        public string[] Data { get; set; }
        
        public override int GetHashCode()
        {
            // Her seferinde yeniden hesaplama
            int hash = 17;
            foreach (var item in Data)
            {
                hash = hash * 23 + (item?.GetHashCode() ?? 0);
            }
            return hash;
        }
    }
    
    // Çözüm: Hash değerini önbelleğe al
    class OptimizedComplexObject
    {
        private int? _cachedHashCode;
        private string[] _data;
        
        public string[] Data
        {
            get => _data;
            set
            {
                _data = value;
                _cachedHashCode = null;  // Hash önbelleğini temizle
            }
        }
        
        public override int GetHashCode()
        {
            if (!_cachedHashCode.HasValue)
            {
                int hash = 17;
                foreach (var item in Data)
                {
                    hash = hash * 23 + (item?.GetHashCode() ?? 0);
                }
                _cachedHashCode = hash;
            }
            
            return _cachedHashCode.Value;
        }
    }
    ```

## Sonuç

Hash tabanlı arama, C#'ta büyük veri setlerinde hızlı arama yapmak için en etkili yöntemlerden biridir. Dictionary, HashSet ve ConcurrentDictionary gibi hash tabanlı veri yapılarını kullanarak, anahtar tabanlı hızlı arama, ekleme ve silme işlemleri gerçekleştirebilirsiniz.

Hash tabanlı veri yapılarını etkili bir şekilde kullanmak için, uygun GetHashCode() ve Equals() implementasyonları oluşturmanız ve çakışmaları en aza indirmeniz önemlidir. Bu yöntemleri doğru bir şekilde uygularsanız, O(1) - sabit zamanlı - arama performansının avantajlarından yararlanabilirsiniz.

Bu rehber, C# dili için hash tabanlı aramanın temel kavramlarını ve pratik uygulamalarını kapsamlı bir şekilde incelemiştir. Bu bilgileri kullanarak, daha verimli ve ölçeklenebilir uygulamalar geliştirebilirsiniz.
