# C# Temel Koleksiyonlar Kılavuzu

C# programlama dilinde koleksiyonlar, veri elemanlarını düzenli bir şekilde depolamak, yönetmek ve işlemek için kullanılan yapılardır. Bu kılavuzda, C#'taki en yaygın kullanılan koleksiyon türlerini detaylı bir şekilde inceleyeceğiz.

## İçindekiler

1. [List\<T\>](#listt)
2. [Dictionary\<TKey, TValue\>](#dictionarytkey-tvalue)
3. [HashSet\<T\>](#hashsett)
4. [Koleksiyonların Karşılaştırılması](#koleksiyonların-karşılaştırılması)
5. [Performans Değerlendirmesi](#performans-değerlendirmesi)
6. [Pratik Uygulamalar](#pratik-uygulamalar)

## List\<T\>

List\<T\>, C#'ta en yaygın kullanılan koleksiyon türlerinden biridir. System.Collections.Generic ad alanında bulunan List\<T\>, dinamik boyutlu bir dizidir ve belirli bir türdeki öğeleri sıralı bir şekilde saklar.

### Temel Özellikler

- **Dinamik Boyut**: Öğe eklendikçe veya çıkarıldıkça otomatik olarak boyutu değişir.
- **İndis Erişimi**: Dizi gibi, öğelere indisler kullanarak erişilebilir (0'dan başlar).
- **Tip Güvenliği**: Generic yapı sayesinde belirli bir türdeki öğeleri saklar.
- **LINQ Uyumluluğu**: LINQ sorguları ile kolayca filtrelenebilir ve işlenebilir.

### Kullanım Örnekleri

```csharp
// List oluşturma
List<int> sayilar = new List<int>();
List<string> isimler = new List<string> { "Ali", "Veli", "Ayşe" };

// Eleman ekleme
sayilar.Add(1);
sayilar.AddRange(new int[] { 2, 3, 4, 5 });

// Belirli bir konuma eleman ekleme
sayilar.Insert(2, 10); // 2. indekse 10 ekler

// Eleman silme
sayilar.Remove(3); // 3 değerini siler
sayilar.RemoveAt(0); // 0. indeksteki elemanı siler
sayilar.RemoveRange(0, 2); // 0. indeksten başlayarak 2 eleman siler

// Elemanları kontrol etme
bool varMi = sayilar.Contains(4); // 4 değeri var mı?
int indeks = sayilar.IndexOf(5); // 5 değerinin indeksi

// Listeyi temizleme
sayilar.Clear();

// Listeyi sıralama
sayilar.Sort();

// Listeyi ters çevirme
sayilar.Reverse();
```

### Önemli Yöntemler ve Özellikler

| Yöntem/Özellik | Açıklama |
|----------------|----------|
| `Add(T item)` | Listeye bir öğe ekler |
| `AddRange(IEnumerable<T> collection)` | Bir koleksiyondaki tüm öğeleri listeye ekler |
| `Insert(int index, T item)` | Belirli bir konuma öğe ekler |
| `Remove(T item)` | Belirtilen öğeyi listeden çıkarır |
| `RemoveAt(int index)` | Belirtilen indeksteki öğeyi çıkarır |
| `Contains(T item)` | Belirtilen öğenin listede olup olmadığını kontrol eder |
| `IndexOf(T item)` | Belirtilen öğenin indeksini döndürür |
| `Sort()` | Listeyi sıralar |
| `Count` | Listedeki öğe sayısını döndürür |
| `Capacity` | Listenin mevcut kapasitesini döndürür |
| `TrimExcess()` | Kullanılmayan kapasiteyi boşaltır |
| `ForEach(Action<T> action)` | Her öğe için belirtilen eylemi gerçekleştirir |
| `Find(Predicate<T> match)` | Belirtilen koşulu sağlayan ilk öğeyi bulur |
| `FindAll(Predicate<T> match)` | Belirtilen koşulu sağlayan tüm öğeleri bulur |

### İç Yapısı ve Çalışma Prensibi

List\<T\>, iç yapısında bir dizi (array) kullanır. Yeni öğeler eklendikçe, iç dizi yeterli kapasiteye sahip değilse, daha büyük bir dizi oluşturulur ve mevcut öğeler bu yeni diziye kopyalanır. Bu işlem, genellikle mevcut kapasiteyi iki katına çıkararak gerçekleştirilir.

```csharp
// List iç yapısı örneği
public class MinimalistList<T>
{
    private T[] _items;
    private int _size;
    private const int DefaultCapacity = 4;
    
    public MinimalistList()
    {
        _items = new T[DefaultCapacity];
        _size = 0;
    }
    
    public void Add(T item)
    {
        if (_size == _items.Length)
        {
            EnsureCapacity(_size + 1);
        }
        _items[_size++] = item;
    }
    
    private void EnsureCapacity(int min)
    {
        int newCapacity = _items.Length == 0 ? DefaultCapacity : _items.Length * 2;
        if (newCapacity < min) newCapacity = min;
        
        T[] newItems = new T[newCapacity];
        Array.Copy(_items, 0, newItems, 0, _size);
        _items = newItems;
    }
    
    // Diğer yöntemler...
}
```

## Dictionary\<TKey, TValue\>

Dictionary\<TKey, TValue\>, anahtar-değer çiftlerini saklayan ve anahtarlar üzerinden hızlı erişim sağlayan bir koleksiyondur. System.Collections.Generic ad alanında bulunur.

### Temel Özellikler

- **Anahtar-Değer Çiftleri**: Her öğe bir anahtar ve bir değer içerir.
- **Benzersiz Anahtarlar**: Aynı anahtar birden fazla kez kullanılamaz.
- **Hızlı Erişim**: Anahtarlar üzerinden O(1) karmaşıklığında erişim sağlar.
- **Tip Güvenliği**: Hem anahtarlar hem de değerler için belirli türler kullanılır.

### Kullanım Örnekleri

```csharp
// Dictionary oluşturma
Dictionary<string, int> yaslar = new Dictionary<string, int>();
Dictionary<int, string> numaralar = new Dictionary<int, string>
{
    { 1, "Bir" },
    { 2, "İki" },
    { 3, "Üç" }
};

// Eleman ekleme
yaslar.Add("Ali", 30);
yaslar["Veli"] = 25; // Bu şekilde de eklenebilir

// Eleman erişimi
int aliYas = yaslar["Ali"];

// Güvenli erişim
if (yaslar.TryGetValue("Mehmet", out int mehmetYas))
{
    Console.WriteLine($"Mehmet'in yaşı: {mehmetYas}");
}
else
{
    Console.WriteLine("Mehmet bulunamadı.");
}

// Eleman silme
yaslar.Remove("Ali");

// Eleman varlığını kontrol etme
bool aliVarMi = yaslar.ContainsKey("Ali");
bool otuzYasVarMi = yaslar.ContainsValue(30);

// Tüm anahtarları ve değerleri alma
var tumAnahtarlar = yaslar.Keys;
var tumDegerler = yaslar.Values;

// Dictionary içinde dolaşma
foreach (var kvp in yaslar)
{
    Console.WriteLine($"Ad: {kvp.Key}, Yaş: {kvp.Value}");
}

// Dictionary temizleme
yaslar.Clear();
```

### Önemli Yöntemler ve Özellikler

| Yöntem/Özellik | Açıklama |
|----------------|----------|
| `Add(TKey key, TValue value)` | Dictionary'ye anahtar-değer çifti ekler |
| `this[TKey key]` | Belirtilen anahtardaki değere erişir veya değer atar |
| `Remove(TKey key)` | Belirtilen anahtarı ve değerini çıkarır |
| `TryGetValue(TKey key, out TValue value)` | Güvenli erişim sağlar, başarı durumunu bool olarak döndürür |
| `ContainsKey(TKey key)` | Belirtilen anahtarın varlığını kontrol eder |
| `ContainsValue(TValue value)` | Belirtilen değerin varlığını kontrol eder |
| `Keys` | Tüm anahtarların koleksiyonunu döndürür |
| `Values` | Tüm değerlerin koleksiyonunu döndürür |
| `Count` | Dictionary'deki öğe sayısını döndürür |
| `Clear()` | Tüm öğeleri temizler |

### İç Yapısı ve Çalışma Prensibi

Dictionary\<TKey, TValue\>, iç yapısında hash tablosu kullanır. Anahtarlar, hash fonksiyonu kullanılarak bir hash değerine dönüştürülür ve bu hash değeri, değerin depolanacağı konumu belirler. Çakışma durumlarında, çakışan anahtarlar bir zincir yapısında saklanır.

```csharp
// Basitleştirilmiş Dictionary iç yapısı örneği
public class MinimalistDictionary<TKey, TValue>
{
    private struct Entry
    {
        public int HashCode;
        public int Next;
        public TKey Key;
        public TValue Value;
    }
    
    private Entry[] _entries;
    private int[] _buckets;
    private int _count;
    private const int DefaultCapacity = 4;
    
    public MinimalistDictionary()
    {
        _buckets = new int[DefaultCapacity];
        _entries = new Entry[DefaultCapacity];
        
        for (int i = 0; i < _buckets.Length; i++)
        {
            _buckets[i] = -1;
        }
    }
    
    public void Add(TKey key, TValue value)
    {
        // Hash değeri hesaplama
        int hashCode = key.GetHashCode() & 0x7FFFFFFF;
        int targetBucket = hashCode % _buckets.Length;
        
        // Çakışma kontrolü ve ekleme işlemi
        // ...
    }
    
    // Diğer yöntemler...
}
```

## HashSet\<T\>

HashSet\<T\>, benzersiz öğelerin bir koleksiyonudur. System.Collections.Generic ad alanında bulunur ve küme işlemlerini verimli bir şekilde gerçekleştirmeye odaklanır.

### Temel Özellikler

- **Benzersiz Öğeler**: Aynı öğe bir kez saklanır.
- **Hızlı Arama**: O(1) karmaşıklığında arama işlemi.
- **Küme İşlemleri**: Birleşim, kesişim, fark gibi küme işlemleri için optimize edilmiştir.
- **Sırasız**: Öğeler belirli bir sıra olmadan saklanır.

### Kullanım Örnekleri

```csharp
// HashSet oluşturma
HashSet<int> sayilar1 = new HashSet<int>();
HashSet<string> kelimeler = new HashSet<string> { "elma", "armut", "kiraz" };
HashSet<int> sayilar2 = new HashSet<int> { 1, 2, 3, 4, 5 };

// Eleman ekleme
sayilar1.Add(1);
sayilar1.Add(2);
sayilar1.Add(1); // Tekrar eden eleman eklenmez

// Birden fazla eleman ekleme
sayilar1.UnionWith(new int[] { 3, 4, 5 });

// Eleman silme
sayilar1.Remove(1);

// Eleman varlığını kontrol etme
bool ikinciBulunduMu = sayilar1.Contains(2);

// Küme İşlemleri
HashSet<int> kesisim = new HashSet<int>(sayilar1);
kesisim.IntersectWith(sayilar2); // Kesişim

HashSet<int> birlesim = new HashSet<int>(sayilar1);
birlesim.UnionWith(sayilar2); // Birleşim

HashSet<int> fark = new HashSet<int>(sayilar1);
fark.ExceptWith(sayilar2); // Fark

// Küme ilişkilerini kontrol etme
bool altKumeMi = sayilar1.IsSubsetOf(sayilar2);
bool ustKumeMi = sayilar1.IsSupersetOf(sayilar2);
bool kesisirMi = sayilar1.Overlaps(sayilar2);

// HashSet temizleme
sayilar1.Clear();
```

### Önemli Yöntemler ve Özellikler

| Yöntem/Özellik | Açıklama |
|----------------|----------|
| `Add(T item)` | HashSet'e bir öğe ekler, başarı durumunu bool olarak döndürür |
| `Remove(T item)` | Belirtilen öğeyi çıkarır, başarı durumunu bool olarak döndürür |
| `Contains(T item)` | Öğenin varlığını kontrol eder |
| `UnionWith(IEnumerable<T> other)` | Belirtilen koleksiyonla birleşim işlemi yapar |
| `IntersectWith(IEnumerable<T> other)` | Belirtilen koleksiyonla kesişim işlemi yapar |
| `ExceptWith(IEnumerable<T> other)` | Belirtilen koleksiyonla fark işlemi yapar |
| `SymmetricExceptWith(IEnumerable<T> other)` | Simetrik fark işlemi yapar |
| `IsSubsetOf(IEnumerable<T> other)` | Alt küme olup olmadığını kontrol eder |
| `IsSupersetOf(IEnumerable<T> other)` | Üst küme olup olmadığını kontrol eder |
| `Overlaps(IEnumerable<T> other)` | Kesişim olup olmadığını kontrol eder |
| `SetEquals(IEnumerable<T> other)` | İki kümenin eşit olup olmadığını kontrol eder |
| `Count` | HashSet'teki öğe sayısını döndürür |
| `Clear()` | Tüm öğeleri temizler |

### İç Yapısı ve Çalışma Prensibi

HashSet\<T\>, iç yapısında Dictionary\<T, object\> veya benzer bir hash tablosu kullanır. Öğeler, hash değerleri hesaplanarak saklanır ve böylece hızlı arama, ekleme ve silme işlemleri gerçekleştirilir.

```csharp
// Basitleştirilmiş HashSet iç yapısı örneği
public class MinimalistHashSet<T>
{
    private struct Slot
    {
        public int HashCode;
        public int Next;
        public T Value;
    }
    
    private int[] _buckets;
    private Slot[] _slots;
    private int _count;
    private const int DefaultCapacity = 4;
    
    public MinimalistHashSet()
    {
        _buckets = new int[DefaultCapacity];
        _slots = new Slot[DefaultCapacity];
        
        for (int i = 0; i < _buckets.Length; i++)
        {
            _buckets[i] = -1;
        }
    }
    
    public bool Add(T value)
    {
        // Hash değeri hesaplama
        int hashCode = (value != null) ? value.GetHashCode() & 0x7FFFFFFF : 0;
        int bucket = hashCode % _buckets.Length;
        
        // Eleman varlığını kontrol etme ve ekleme
        // ...
        
        return true; // Eğer eklendiyse
    }
    
    // Diğer yöntemler...
}
```

## Koleksiyonların Karşılaştırılması

Her koleksiyon türünün kendi güçlü yanları ve kullanım alanları vardır. Aşağıdaki tablo, üç koleksiyon türünün karşılaştırmasını sunar:

| Özellik | List\<T\> | Dictionary\<TKey, TValue\> | HashSet\<T\> |
|---------|-----------|----------------------------|--------------|
| Veri Yapısı | Sıralı liste | Anahtar-değer çiftleri | Benzersiz öğeler |
| Erişim | İndeks ile (O(1)) | Anahtar ile (O(1)) | Değer ile (O(1)) |
| Ekleme | O(1)* | O(1) | O(1) |
| Arama | O(n) | O(1) | O(1) |
| Silme | O(n) | O(1) | O(1) |
| Sıralama | Destekler | Desteklemez (manuel yapılabilir) | Desteklemez |
| Çift Öğe | İzin verir | Anahtarlar benzersiz olmalı | İzin vermez |
| Bellek Kullanımı | Düşük | Orta | Orta |
| Kullanım Durumu | Sıralı veri, sık erişim | Anahtar ile hızlı erişim | Benzersiz öğeler, küme işlemleri |

* Listenin sonuna ekleme genellikle O(1)'dir, ancak kapasite artırımı gerektiğinde O(n) olabilir.

## Performans Değerlendirmesi

Farklı koleksiyon türlerinin performansı, gerçekleştirilen işlemlere ve veri boyutuna bağlı olarak değişebilir. Aşağıda, yaygın işlemler için bir performans karşılaştırması bulunmaktadır:

### Ekleme İşlemi (1 milyon öğe)

```csharp
using System;
using System.Collections.Generic;
using System.Diagnostics;

class Program
{
    static void Main()
    {
        const int ElemanSayisi = 1_000_000;
        
        // List ekleme performansı
        var liste = new List<int>();
        var sw = Stopwatch.StartNew();
        for (int i = 0; i < ElemanSayisi; i++)
        {
            liste.Add(i);
        }
        sw.Stop();
        Console.WriteLine($"List ekleme: {sw.ElapsedMilliseconds}ms");
        
        // Dictionary ekleme performansı
        var sozluk = new Dictionary<int, int>();
        sw = Stopwatch.StartNew();
        for (int i = 0; i < ElemanSayisi; i++)
        {
            sozluk.Add(i, i);
        }
        sw.Stop();
        Console.WriteLine($"Dictionary ekleme: {sw.ElapsedMilliseconds}ms");
        
        // HashSet ekleme performansı
        var kume = new HashSet<int>();
        sw = Stopwatch.StartNew();
        for (int i = 0; i < ElemanSayisi; i++)
        {
            kume.Add(i);
        }
        sw.Stop();
        Console.WriteLine($"HashSet ekleme: {sw.ElapsedMilliseconds}ms");
    }
}
```

### Arama İşlemi

```csharp
// Arama performans testi
var rnd = new Random(42);
int aramaAdedi = 10_000;
int[] aranacaklar = new int[aramaAdedi];
for (int i = 0; i < aramaAdedi; i++)
{
    aranacaklar[i] = rnd.Next(0, ElemanSayisi);
}

// List arama
sw = Stopwatch.StartNew();
foreach (var aranan in aranacaklar)
{
    bool bulundu = liste.Contains(aranan);
}
sw.Stop();
Console.WriteLine($"List arama: {sw.ElapsedMilliseconds}ms");

// Dictionary arama
sw = Stopwatch.StartNew();
foreach (var aranan in aranacaklar)
{
    bool bulundu = sozluk.ContainsKey(aranan);
}
sw.Stop();
Console.WriteLine($"Dictionary arama: {sw.ElapsedMilliseconds}ms");

// HashSet arama
sw = Stopwatch.StartNew();
foreach (var aranan in aranacaklar)
{
    bool bulundu = kume.Contains(aranan);
}
sw.Stop();
Console.WriteLine($"HashSet arama: {sw.ElapsedMilliseconds}ms");
```

Örnek Sonuçlar:
- List ekleme: ~30-50ms
- Dictionary ekleme: ~150-250ms
- HashSet ekleme: ~150-220ms
- List arama: ~8000-12000ms
- Dictionary arama: ~1-5ms
- HashSet arama: ~1-5ms

Bu sonuçlar gösteriyor ki:
- List\<T\>, ekleme işlemlerinde en hızlı performansı sunar.
- Dictionary\<TKey, TValue\> ve HashSet\<T\>, arama işlemlerinde dramatik şekilde daha hızlıdır.
- Büyük veri setlerinde arama yapılacaksa, List\<T\> yerine hash tabanlı koleksiyonlar tercih edilmelidir.

## Pratik Uygulamalar

### List\<T\> Kullanım Senaryoları

```csharp
// Öğrenci listesi oluşturma ve sıralama
public class Ogrenci
{
    public int Id { get; set; }
    public string Ad { get; set; }
    public double Ortalama { get; set; }
}

List<Ogrenci> ogrenciler = new List<Ogrenci>
{
    new Ogrenci { Id = 1, Ad = "Ali", Ortalama = 85.5 },
    new Ogrenci { Id = 2, Ad = "Veli", Ortalama = 92.3 },
    new Ogrenci { Id = 3, Ad = "Ayşe", Ortalama = 78.9 }
};

// Ortalamaya göre sıralama
ogrenciler.Sort((a, b) => b.Ortalama.CompareTo(a.Ortalama));

// LINQ ile filtreleme
var basariliOgrenciler = ogrenciler.Where(o => o.Ortalama >= 85).ToList();
```

### Dictionary\<TKey, TValue\> Kullanım Senaryoları

```csharp
// Ürün veritabanı örneği
public class Urun
{
    public int Id { get; set; }
    public string Ad { get; set; }
    public decimal Fiyat { get; set; }
    public int Stok { get; set; }
}

Dictionary<int, Urun> urunler = new Dictionary<int, Urun>();

// Ürünleri ekleme
urunler.Add(1, new Urun { Id = 1, Ad = "Laptop", Fiyat = 8500, Stok = 45 });
urunler.Add(2, new Urun { Id = 2, Ad = "Telefon", Fiyat = 5200, Stok = 120 });
urunler.Add(3, new Urun { Id = 3, Ad = "Kulaklık", Fiyat = 750, Stok = 230 });

// ID ile hızlı ürün arama
if (urunler.TryGetValue(2, out Urun bulunanUrun))
{
    Console.WriteLine($"Ürün: {bulunanUrun.Ad}, Fiyat: {bulunanUrun.Fiyat}TL");
}

// Stok kontrolü ve güncelleme
void UrunSatisiYap(int urunId, int adet)
{
    if (urunler.TryGetValue(urunId, out Urun urun))
    {
        if (urun.Stok >= adet)
        {
            urun.Stok -= adet;
            Console.WriteLine($"{urun.Ad} ürününden {adet} adet satıldı. Kalan stok: {urun.Stok}");
        }
        else
        {
            Console.WriteLine("Yetersiz stok!");
        }
    }
    else
    {
        Console.WriteLine("Ürün bulunamadı!");
    }
}
```

### HashSet\<T\> Kullanım Senaryoları

```csharp
// Kelime frekansı analizi
string metin = "Bu bir örnek metin. Bu metin HashSet kullanımını göstermek için yazılmıştır.";
string[] kelimeler = metin.Split(new char[] { ' ', '.', ',' }, StringSplitOptions.RemoveEmptyEntries);

// Benzersiz kelimeleri bulma
HashSet<string> benzersizKelimeler = new HashSet<string>(kelimeler, StringComparer.OrdinalIgnoreCase);
Console.WriteLine($"Toplam kelime sayısı: {kelimeler.Length}");
Console.WriteLine($"Benzersiz kelime sayısı: {benzersizKelimeler.Count}");

// Ortak elemanları bulma
HashSet<int> kume1 = new HashSet<int> { 1, 2, 3, 4, 5 };
HashSet<int> kume2 = new HashSet<int> { 3, 4, 5, 6, 7 };

// Kesişim bulma
kume1.IntersectWith(kume2);
Console.WriteLine("Ortak elemanlar: " + string.Join(", ", kume1)); // 3, 4, 5
```

## Sonuç

C# koleksiyonları, farklı senaryolarda farklı avantajlar sunan güçlü veri yapılarıdır:

- **List\<T\>**: Sıralı, dinamik boyutlu listeler için idealdir.
- **Dictionary\<TKey, TValue\>**: Anahtar-değer ikililerini hızlı erişimle saklamak için kullanılır.
- **HashSet\<T\>**: Benzersiz öğeler kümesi ve küme işlemleri için optimize edilmiştir.

Doğru koleksiyon türünü seçmek, uygulamanızın performansını ve tasarımını önemli ölçüde etkileyebilir. Her türün kendine özgü avantajları ve kullanım alanları vardır. En iyi performans için, her senaryoda uygun koleksiyon türünü kullanmak önemlidir.
