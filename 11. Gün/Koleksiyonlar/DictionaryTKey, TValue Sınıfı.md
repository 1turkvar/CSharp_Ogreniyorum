# C# Dictionary<TKey, TValue> Sınıfı

Dictionary<TKey, TValue> sınıfı, C# programlama dilinde anahtar-değer çiftlerini depolamak için kullanılan güçlü bir koleksiyon yapısıdır. Anahtar ve değerlerin güçlü bir şekilde tiplendirildiği bu veri yapısı, verilere hızlı erişim sağlar ve birçok uygulamada yaygın olarak kullanılır.

## İçindekiler
1. [Genel Bakış](#genel-bakış)
2. [Dictionary Oluşturma](#dictionary-oluşturma)
3. [Temel Operasyonlar](#temel-operasyonlar)
4. [Dictionary Özellikleri](#dictionary-özellikleri)
5. [Performans Özellikleri](#performans-özellikleri)
6. [İterasyonlar ve Döngüler](#i̇terasyonlar-ve-döngüler)
7. [Dictionary Metodları](#dictionary-metodları)
8. [Örnek Kullanım Senaryoları](#örnek-kullanım-senaryoları)
9. [İyi Uygulamalar ve İpuçları](#i̇yi-uygulamalar-ve-i̇puçları)
10. [Yaygın Hatalar ve Çözümleri](#yaygın-hatalar-ve-çözümleri)

## Genel Bakış

Dictionary<TKey, TValue> sınıfı, System.Collections.Generic namespace'i içerisinde bulunan ve IDictionary<TKey, TValue> arayüzünü uygulayan bir koleksiyon türüdür. Sınıf, her bir anahtarın benzersiz olduğu anahtar-değer çiftlerini depolayan bir hash tablosu yapısı kullanır.

**Önemli Özellikler:**
- Her anahtar benzersiz olmalıdır
- Anahtarlar null olamaz (reference tipler için)
- Değerler null olabilir (nullable tipler veya reference tipler için)
- Elemanlara erişim O(1) karmaşıklığındadır (ortalama durum)
- Generic olduğu için farklı veri tipleriyle kullanılabilir

## Dictionary Oluşturma

```csharp
// Boş bir Dictionary oluşturma
Dictionary<string, int> yaşlar = new Dictionary<string, int>();

// İlk değerleri belirterek oluşturma
Dictionary<string, int> puanlar = new Dictionary<string, int>()
{
    { "Ali", 85 },
    { "Ayşe", 92 },
    { "Mehmet", 78 }
};

// Alternatif sintaks (C# 3.0 ve sonrası)
Dictionary<string, int> puanlar2 = new Dictionary<string, int>()
{
    ["Ali"] = 85,
    ["Ayşe"] = 92,
    ["Mehmet"] = 78
};

// C# 9.0 ve sonrasında "new()" sintaksı
Dictionary<string, int> yaşlar2 = new();

// Kapasiteyi belirterek oluşturma
Dictionary<string, string> ülkeBaşkentleri = new Dictionary<string, string>(50);

// Özel bir IEqualityComparer ile oluşturma (Büyük/küçük harf duyarsız)
Dictionary<string, int> büyükKüçükHarfDuyarsız = new Dictionary<string, int>(StringComparer.OrdinalIgnoreCase);
```

## Temel Operasyonlar

### Eleman Ekleme

```csharp
Dictionary<int, string> öğrenciler = new Dictionary<int, string>();

// Add metodu ile ekleme
öğrenciler.Add(101, "Ahmet Yılmaz");
öğrenciler.Add(102, "Zeynep Kaya");

// İndeksleyici ile ekleme
öğrenciler[103] = "Mustafa Demir";
```

### Elemana Erişim

```csharp
// İndeksleyici ile erişim
string öğrenci = öğrenciler[101]; // "Ahmet Yılmaz"

// TryGetValue ile güvenli erişim
if (öğrenciler.TryGetValue(104, out string bulunanÖğrenci))
{
    Console.WriteLine($"Öğrenci: {bulunanÖğrenci}");
}
else
{
    Console.WriteLine("Öğrenci bulunamadı.");
}
```

### Eleman Güncelleme

```csharp
// Var olan anahtara yeni değer atama
öğrenciler[101] = "Ahmet Can Yılmaz";

// Kontrol ederek güncelleme
if (öğrenciler.ContainsKey(102))
{
    öğrenciler[102] = "Zeynep Ak";
}
```

### Eleman Silme

```csharp
// Belirli bir anahtara sahip elemanı silme
öğrenciler.Remove(103);

// Belirli bir koşula sahip elemanları silme (LINQ ile - .NET Framework 4.0+)
var silinecekler = öğrenciler.Where(x => x.Value.Contains("Can")).Select(x => x.Key).ToList();
foreach (var anahtar in silinecekler)
{
    öğrenciler.Remove(anahtar);
}

// Tüm elemanları silme
öğrenciler.Clear();
```

## Dictionary Özellikleri

```csharp
Dictionary<string, double> ürünFiyatları = new Dictionary<string, double>()
{
    { "Laptop", 10000.0 },
    { "Telefon", 5000.0 },
    { "Tablet", 3000.0 }
};

// Eleman sayısını alma
int elemanSayısı = ürünFiyatları.Count; // 3

// Dictionary içerisinde belirli bir anahtarın varlığını kontrol etme
bool laptopVarMı = ürünFiyatları.ContainsKey("Laptop"); // true
bool televizyonVarMı = ürünFiyatları.ContainsKey("Televizyon"); // false

// Dictionary içerisinde belirli bir değerin varlığını kontrol etme
bool değerVarMı = ürünFiyatları.ContainsValue(5000.0); // true

// Tüm anahtarları alma
ICollection<string> anahtarlar = ürünFiyatları.Keys; // "Laptop", "Telefon", "Tablet"

// Tüm değerleri alma
ICollection<double> değerler = ürünFiyatları.Values; // 10000.0, 5000.0, 3000.0
```

## Performans Özellikleri

Dictionary<TKey, TValue> sınıfı, arka planda hash tablosu kullandığı için aşağıdaki performans karakteristiklerine sahiptir:

| İşlem | Ortalama Durum | En Kötü Durum |
|-------|----------------|---------------|
| Arama | O(1) | O(n) |
| Ekleme | O(1) | O(n) |
| Silme | O(1) | O(n) |

En kötü durum, çok fazla hash çakışması olduğunda ortaya çıkar. Bu durum genellikle kötü bir hash fonksiyonu veya çok büyük boyutlu dictionary'ler için söz konusu olabilir.

## İterasyonlar ve Döngüler

```csharp
Dictionary<string, string> başkentler = new Dictionary<string, string>()
{
    { "Türkiye", "Ankara" },
    { "Almanya", "Berlin" },
    { "Fransa", "Paris" }
};

// foreach ile tüm anahtar-değer çiftlerini dolaşma
foreach (KeyValuePair<string, string> başkent in başkentler)
{
    Console.WriteLine($"{başkent.Key} ülkesinin başkenti {başkent.Value}'dir.");
}

// var kullanımı ile daha kısa yazım
foreach (var başkent in başkentler)
{
    Console.WriteLine($"{başkent.Key}: {başkent.Value}");
}

// Sadece anahtarlar üzerinde döngü
foreach (string ülke in başkentler.Keys)
{
    Console.WriteLine($"Ülke: {ülke}");
}

// Sadece değerler üzerinde döngü
foreach (string şehir in başkentler.Values)
{
    Console.WriteLine($"Başkent: {şehir}");
}

// LINQ ile filtreleme ve işleme
var avrupaÜlkeleri = başkentler.Where(b => b.Key == "Almanya" || b.Key == "Fransa");
foreach (var ülke in avrupaÜlkeleri)
{
    Console.WriteLine($"Avrupa ülkesi: {ülke.Key}");
}
```

## Dictionary Metodları

### Temel Metodlar

| Metod | Açıklama |
|-------|----------|
| `Add(TKey, TValue)` | Dictionary'ye yeni bir anahtar-değer çifti ekler |
| `Clear()` | Dictionary'deki tüm anahtar-değer çiftlerini siler |
| `ContainsKey(TKey)` | Belirtilen anahtarın Dictionary'de olup olmadığını kontrol eder |
| `ContainsValue(TValue)` | Belirtilen değerin Dictionary'de olup olmadığını kontrol eder |
| `Remove(TKey)` | Belirtilen anahtara sahip çifti siler |
| `TryGetValue(TKey, out TValue)` | Belirtilen anahtarı güvenli bir şekilde almaya çalışır |

### İlave Metodlar (.NET 6+)

| Metod | Açıklama |
|-------|----------|
| `TryAdd(TKey, TValue)` | Anahtar zaten yoksa ekler ve başarı durumunu döndürür |
| `GetValueOrDefault(TKey)` | Anahtarı bulursa değeri, bulamazsa default değeri döndürür |
| `EnsureCapacity(int)` | Dictionary'nin kapasitesini belirtilen değere yükseltir |
| `TrimExcess()` | Dictionary'nin kapasitesini eleman sayısına düşürür |

## Örnek Kullanım Senaryoları

### 1. Frekans Sayacı

```csharp
string metin = "Merhaba dünya, merhaba C# programlama!";
Dictionary<char, int> karakterSayıları = new Dictionary<char, int>();

foreach (char c in metin)
{
    if (karakterSayıları.ContainsKey(c))
    {
        karakterSayıları[c]++;
    }
    else
    {
        karakterSayıları[c] = 1;
    }
}

foreach (var karakter in karakterSayıları.OrderByDescending(k => k.Value))
{
    Console.WriteLine($"'{karakter.Key}' karakteri {karakter.Value} kez geçiyor");
}
```

### 2. Önbellek (Cache) Yapısı

```csharp
public class ÖnBellekYönetici<TKey, TValue>
{
    private readonly Dictionary<TKey, TValue> _önbellek = new Dictionary<TKey, TValue>();
    
    public TValue Getir(TKey anahtar, Func<TKey, TValue> veriYükleyici)
    {
        if (!_önbellek.TryGetValue(anahtar, out TValue değer))
        {
            değer = veriYükleyici(anahtar);
            _önbellek[anahtar] = değer;
        }
        
        return değer;
    }
    
    public void Temizle()
    {
        _önbellek.Clear();
    }
}

// Kullanım örneği
var önbellek = new ÖnBellekYönetici<int, string>();
string veri = önbellek.Getir(1, id => $"ID {id} için veritabanından yüklenen veri");
```

### 3. Arama Motoru İndeksi

```csharp
Dictionary<string, List<int>> kelimeİndeksi = new Dictionary<string, List<int>>(StringComparer.OrdinalIgnoreCase);

void BelgeyiİndeksleV(string belgeİçeriği, int belgeID)
{
    string[] kelimeler = belgeİçeriği.Split(new[] { ' ', ',', '.', '!', '?' }, StringSplitOptions.RemoveEmptyEntries);
    
    foreach (string kelime in kelimeler)
    {
        if (!kelimeİndeksi.ContainsKey(kelime))
        {
            kelimeİndeksi[kelime] = new List<int>();
        }
        
        if (!kelimeİndeksi[kelime].Contains(belgeID))
        {
            kelimeİndeksi[kelime].Add(belgeID);
        }
    }
}

List<int> KelimeAra(string arananKelime)
{
    if (kelimeİndeksi.TryGetValue(arananKelime, out List<int> sonuçlar))
    {
        return sonuçlar;
    }
    
    return new List<int>();
}
```

## İyi Uygulamalar ve İpuçları

1. **Kapasite Ayarlama**: Eğer Dictionary boyutunu önceden biliyorsanız, performans için kapasite belirterek başlatın:
   ```csharp
   Dictionary<string, int> büyükSözlük = new Dictionary<string, int>(10000);
   ```

2. **Güvenli Erişim**: `TryGetValue()` metodunu kullanarak anahtarın olmaması durumunda istisnalardan kaçının:
   ```csharp
   if (sözlük.TryGetValue(anahtar, out değer))
   {
       // değer ile işlem yap
   }
   ```

3. **Özel Anahtarlar**: Kendi sınıflarınızı anahtar olarak kullanıyorsanız, `GetHashCode()` ve `Equals()` metodlarını doğru şekilde uygulayın:
   ```csharp
   public class Kişi
   {
       public string Ad { get; set; }
       public string Soyad { get; set; }
       
       public override bool Equals(object obj)
       {
           if (obj is Kişi diğerKişi)
           {
               return Ad == diğerKişi.Ad && Soyad == diğerKişi.Soyad;
           }
           return false;
       }
       
       public override int GetHashCode()
       {
           return (Ad + Soyad).GetHashCode();
       }
   }
   ```

4. **Thread Güvenliği**: Dictionary thread güvenli değildir. Çoklu thread ortamında ConcurrentDictionary kullanmayı düşünün:
   ```csharp
   ConcurrentDictionary<string, int> threadGüvenliSözlük = new ConcurrentDictionary<string, int>();
   ```

5. **Bellek Optimizasyonu**: Büyük Dictionary nesnelerinde kullanılmayan alanı serbest bırakmak için `TrimExcess()` metodunu kullanın:
   ```csharp
   sözlük.TrimExcess();
   ```

## Yaygın Hatalar ve Çözümleri

1. **KeyNotFoundException**: Var olmayan bir anahtara indeksleyici ile erişmeye çalışıldığında oluşur.
   ```csharp
   // Hataya açık:
   string değer = sözlük["varolmayanAnahtar"]; // KeyNotFoundException fırlatır
   
   // Güvenli alternatif:
   if (sözlük.TryGetValue("varolmayanAnahtar", out string değer))
   {
       // değer ile çalış
   }
   ```

2. **Tekrarlı Anahtar Hatası**: Aynı anahtarı iki kez eklemeye çalışmak.
   ```csharp
   // Hataya açık:
   sözlük.Add("anahtar", "değer1");
   sözlük.Add("anahtar", "değer2"); // ArgumentException fırlatır
   
   // Güvenli alternatif:
   if (!sözlük.ContainsKey("anahtar"))
   {
       sözlük.Add("anahtar", "değer");
   }
   else
   {
       sözlük["anahtar"] = "yeniDeğer"; // Güncelleme
   }
   
   // Veya .NET 6+ için:
   sözlük.TryAdd("anahtar", "değer");
   ```

3. **Null Anahtar Hatası**: Reference türü anahtarlar için null kullanmak.
   ```csharp
   // Hataya açık:
   Dictionary<string, int> d = new Dictionary<string, int>();
   d.Add(null, 1); // ArgumentNullException fırlatır
   
   // Çözüm:
   // Nullable değer türleri ile kullanın veya null kontrolü yapın
   string anahtar = GetPotentiallyNullString();
   if (anahtar != null)
   {
       d.Add(anahtar, 1);
   }
   ```

4. **İterasyon Sırasında Değişiklik**: Dictionary üzerinde döngü içinde değişiklik yapmak.
   ```csharp
   // Hataya açık:
   foreach (var item in sözlük)
   {
       if (someCondition)
       {
           sözlük.Remove(item.Key); // InvalidOperationException fırlatır
       }
   }
   
   // Güvenli alternatif:
   var silinecekAnahtarlar = sözlük.Where(x => someCondition(x))
                                 .Select(x => x.Key)
                                 .ToList();
   foreach (var key in silinecekAnahtarlar)
   {
       sözlük.Remove(key);
   }
   ```

---

Bu dokümantasyon, C# Dictionary<TKey, TValue> sınıfının temel kullanımını, özelliklerini ve yaygın senaryolarını kapsamaktadır. Dictionary sınıfı, anahtar-değer çiftlerini verimli bir şekilde yönetmek isteyen her C# geliştiricisi için temel bir veri yapısıdır.

Dictionary yapısı, C#'ın en çok kullanılan koleksiyon türlerinden biridir ve doğru kullanıldığında uygulama performansınızı ve kod kalitesini önemli ölçüde artırabilir.

C# Dictionary sınıfı hakkında sorularınız var mı? Başka bir konuda yardımcı olabilir miyim?