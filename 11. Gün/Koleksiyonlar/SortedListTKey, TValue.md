Rehberi">
# C# SortedList<TKey, TValue> Rehberi

## İçindekiler
1. [Giriş](#giriş)
2. [SortedList<TKey, TValue> Nedir?](#sortedlisttkey-tvalue-nedir)
3. [SortedList ve Dictionary Arasındaki Farklar](#sortedlist-ve-dictionary-arasındaki-farklar)
4. [Temel Özellikler](#temel-özellikler)
5. [Temel Metodlar ve Özellikler](#temel-metodlar-ve-özellikler)
6. [SortedList<TKey, TValue> Oluşturma](#sortedlisttkey-tvalue-oluşturma)
7. [Örnekler](#örnekler)
   - [Temel Kullanım](#temel-kullanım)
   - [Özel Karşılaştırıcı Kullanımı](#özel-karşılaştırıcı-kullanımı)
   - [SortedList'i Listeye Dönüştürme](#sortedlisti-listeye-dönüştürme)
   - [SortedList'i Filtreleme](#sortedlisti-filtreleme)
8. [Performans Özellikleri](#performans-özellikleri)
9. [İyi Uygulamalar](#iyi-uygulamalar)
10. [Sık Karşılaşılan Sorunlar](#sık-karşılaşılan-sorunlar)
11. [Özet](#özet)

## Giriş

SortedList<TKey, TValue>, C# programlama dilinde .NET Framework içinde bulunan, anahtar-değer çiftlerini saklayan ve anahtarlara göre sıralanmış bir koleksiyon sınıfıdır. System.Collections.Generic namespace'i altında bulunur ve generic (genel) olarak tasarlandığı için farklı veri tipleriyle çalışabilir.

## SortedList<TKey, TValue> Nedir?

SortedList<TKey, TValue>, anahtar-değer çiftlerini saklayan ve bu çiftleri **anahtarlara göre sıralı** tutan bir koleksiyon yapısıdır. Bu koleksiyon:

- Anahtar-değer çiftlerini saklar
- Anahtarlar benzersiz olmalıdır (duplicate keys kabul edilmez)
- Anahtarlara göre otomatik olarak sıralanır
- Hem anahtar hem de indeks tabanlı erişim sağlar
- Dizi ve bağlantılı liste kombinasyonu olarak uygulanmıştır

SortedList, içerdiği verilere hem anahtar üzerinden hem de indeks üzerinden erişim imkanı tanır, bu da onu diğer koleksiyonlardan ayıran önemli bir özelliktir.

## SortedList ve Dictionary Arasındaki Farklar

| Özellik | SortedList<TKey, TValue> | Dictionary<TKey, TValue> |
|---------|--------------------------|--------------------------|
| Sıralama | Anahtarlara göre sıralı | Sırasız (hash tabanlı) |
| Bellek Kullanımı | Daha az bellek kullanır | Daha fazla bellek kullanır |
| Ekleme/Silme Performansı | Daha yavaş (O(n)) | Daha hızlı (O(1)) |
| Arama Performansı | Binary search ile hızlı (O(log n)) | Hash tabanlı çok hızlı (O(1)) |
| İndeks ile Erişim | Destekler | Desteklemez |
| İmplementasyon | Dizi tabanlı | Hash tabanlı |

## Temel Özellikler

- **Anahtar Sıralaması**: Anahtarlar, varsayılan olarak doğal sıralamalarına göre (artan sırada) sıralanır.
- **Benzersiz Anahtarlar**: Her anahtar koleksiyon içinde yalnızca bir kez bulunabilir.
- **İndeksleyiciler**: Hem anahtar hem de indeks kullanarak elemanlara erişilebilir.
- **Dinamik Boyut**: İhtiyaca göre boyutu değişebilir.
- **Type Safety**: Generic yapısı sayesinde tip güvenliği sağlar.
- **Özel Karşılaştırma**: IComparer<T> arayüzünü uygulayan özel karşılaştırıcılar kullanılabilir.

## Temel Metodlar ve Özellikler

### Özellikler

| Özellik | Açıklama |
|---------|----------|
| `Capacity` | Koleksiyonun alabileceği maksimum eleman sayısı |
| `Count` | Koleksiyondaki eleman sayısı |
| `Keys` | Anahtarların bir koleksiyonunu döndürür |
| `Values` | Değerlerin bir koleksiyonunu döndürür |
| `Item[TKey]` | Belirtilen anahtara sahip değeri alır veya ayarlar |
| `Item[int]` | Belirtilen indeksteki değeri alır veya ayarlar |

### Metodlar

| Metod | Açıklama |
|-------|----------|
| `Add(TKey, TValue)` | Belirtilen anahtar ve değeri koleksiyona ekler |
| `Remove(TKey)` | Belirtilen anahtara sahip elemanı kaldırır |
| `RemoveAt(int)` | Belirtilen indeksteki elemanı kaldırır |
| `Clear()` | Tüm elemanları kaldırır |
| `ContainsKey(TKey)` | Belirtilen anahtarın varlığını kontrol eder |
| `ContainsValue(TValue)` | Belirtilen değerin varlığını kontrol eder |
| `IndexOfKey(TKey)` | Belirtilen anahtarın indeksini döndürür |
| `IndexOfValue(TValue)` | Belirtilen değerin indeksini döndürür |
| `TryGetValue(TKey, out TValue)` | Belirtilen anahtara sahip değeri almaya çalışır |

## SortedList<TKey, TValue> Oluşturma

SortedList<TKey, TValue> sınıfını farklı şekillerde oluşturabilirsiniz:

```csharp
// Boş bir SortedList oluşturma
SortedList<string, int> bosSortedList = new SortedList<string, int>();

// Belirli bir başlangıç kapasitesi ile oluşturma
SortedList<string, int> kapasiteliSortedList = new SortedList<string, int>(100);

// Özel bir karşılaştırıcı ile oluşturma
SortedList<string, int> ozelKarsilastiriciSortedList = new SortedList<string, int>(StringComparer.OrdinalIgnoreCase);

// Başka bir IDictionary'den oluşturma
Dictionary<string, int> dict = new Dictionary<string, int>
{
    { "bir", 1 },
    { "iki", 2 }
};
SortedList<string, int> dictionarydenSortedList = new SortedList<string, int>(dict);
```

## Örnekler

### Temel Kullanım

```csharp
using System;
using System.Collections.Generic;

class Program
{
    static void Main()
    {
        // Yeni bir SortedList oluşturma
        SortedList<string, int> ogrenciPuanlari = new SortedList<string, int>();
        
        // Eleman ekleme
        ogrenciPuanlari.Add("Mehmet", 85);
        ogrenciPuanlari.Add("Ali", 92);
        ogrenciPuanlari.Add("Zeynep", 78);
        ogrenciPuanlari.Add("Canan", 95);
        
        // Koleksiyonu gezme - anahtar ve değerlere erişim
        Console.WriteLine("Öğrenci Puanları (Alfabetik Sıralı):");
        foreach (var ogrenci in ogrenciPuanlari)
        {
            Console.WriteLine($"{ogrenci.Key}: {ogrenci.Value}");
        }
        
        // İndeks ile erişim
        Console.WriteLine("\nİlk öğrenci: " + ogrenciPuanlari.Keys[0] + " - " + ogrenciPuanlari.Values[0]);
        
        // Anahtar kontrol
        if (ogrenciPuanlari.ContainsKey("Ali"))
        {
            Console.WriteLine("\nAli'nin puanı: " + ogrenciPuanlari["Ali"]);
        }
        
        // Değer almaya çalışma (güvenli yol)
        if (ogrenciPuanlari.TryGetValue("Deniz", out int puan))
        {
            Console.WriteLine("Deniz'in puanı: " + puan);
        }
        else
        {
            Console.WriteLine("\nDeniz isimli öğrenci bulunamadı.");
        }
        
        // Anahtar-değer çiftini güncelleme
        ogrenciPuanlari["Zeynep"] = 82;
        Console.WriteLine("\nZeynep'in güncellenen puanı: " + ogrenciPuanlari["Zeynep"]);
        
        // Eleman silme
        ogrenciPuanlari.Remove("Ali");
        
        // Güncel listeyi gösterme
        Console.WriteLine("\nGüncellenmiş Öğrenci Listesi:");
        foreach (KeyValuePair<string, int> ogrenci in ogrenciPuanlari)
        {
            Console.WriteLine($"{ogrenci.Key}: {ogrenci.Value}");
        }
    }
}
```

Çıktı:
```
Öğrenci Puanları (Alfabetik Sıralı):
Ali: 92
Canan: 95
Mehmet: 85
Zeynep: 78

İlk öğrenci: Ali - 92

Ali'nin puanı: 92

Deniz isimli öğrenci bulunamadı.

Zeynep'in güncellenen puanı: 82

Güncellenmiş Öğrenci Listesi:
Canan: 95
Mehmet: 85
Zeynep: 82
```

### Özel Karşılaştırıcı Kullanımı

Anahtarların sıralama mantığını özelleştirmek için özel bir karşılaştırıcı kullanabilirsiniz:

```csharp
using System;
using System.Collections.Generic;

class Program
{
    static void Main()
    {
        // Tersine sıralama için özel karşılaştırıcı ile SortedList oluşturma
        SortedList<string, int> tersStringSirali = new SortedList<string, int>(
            Comparer<string>.Create((a, b) => string.Compare(b, a)));
        
        tersStringSirali.Add("Elma", 10);
        tersStringSirali.Add("Portakal", 15);
        tersStringSirali.Add("Muz", 7);
        tersStringSirali.Add("Çilek", 20);
        
        Console.WriteLine("Tersine Alfabetik Sıralı Meyveler:");
        foreach (var meyve in tersStringSirali)
        {
            Console.WriteLine($"{meyve.Key}: {meyve.Value}");
        }
        
        // Büyük-küçük harf duyarsız karşılaştırıcı
        SortedList<string, string> buyukKucukHarfDuyarsiz = new SortedList<string, string>(
            StringComparer.OrdinalIgnoreCase);
        
        buyukKucukHarfDuyarsiz.Add("Abc", "Değer 1");
        buyukKucukHarfDuyarsiz.Add("DEF", "Değer 2");
        buyukKucukHarfDuyarsiz.Add("ghi", "Değer 3");
        
        // Aynı anahtarı farklı büyük-küçük harfle eklemeye çalışma
        try
        {
            buyukKucukHarfDuyarsiz.Add("abc", "Değer 4"); // Hata verecek
        }
        catch (ArgumentException ex)
        {
            Console.WriteLine("\nHata: " + ex.Message);
        }
        
        Console.WriteLine("\nBüyük-Küçük Harf Duyarsız Liste:");
        foreach (var item in buyukKucukHarfDuyarsiz)
        {
            Console.WriteLine($"{item.Key}: {item.Value}");
        }
    }
}
```

Çıktı:
```
Tersine Alfabetik Sıralı Meyveler:
Portakal: 15
Muz: 7
Elma: 10
Çilek: 20

Hata: An item with the same key has already been added. Key: abc

Büyük-Küçük Harf Duyarsız Liste:
Abc: Değer 1
DEF: Değer 2
ghi: Değer 3
```

### SortedList'i Listeye Dönüştürme

```csharp
using System;
using System.Collections.Generic;
using System.Linq;

class Program
{
    static void Main()
    {
        SortedList<int, string> numaralar = new SortedList<int, string>
        {
            { 5, "Beş" },
            { 1, "Bir" },
            { 10, "On" },
            { 3, "Üç" }
        };
        
        // Anahtarları listeye dönüştürme
        List<int> anahtarListesi = numaralar.Keys.ToList();
        Console.WriteLine("Anahtarlar:");
        foreach (int anahtar in anahtarListesi)
        {
            Console.WriteLine(anahtar);
        }
        
        // Değerleri listeye dönüştürme
        List<string> degerListesi = numaralar.Values.ToList();
        Console.WriteLine("\nDeğerler:");
        foreach (string deger in degerListesi)
        {
            Console.WriteLine(deger);
        }
        
        // KeyValuePair listesine dönüştürme
        List<KeyValuePair<int, string>> ciftListesi = numaralar.ToList();
        Console.WriteLine("\nAnahtar-Değer Çiftleri:");
        foreach (var cift in ciftListesi)
        {
            Console.WriteLine($"{cift.Key}: {cift.Value}");
        }
    }
}
```

Çıktı:
```
Anahtarlar:
1
3
5
10

Değerler:
Bir
Üç
Beş
On

Anahtar-Değer Çiftleri:
1: Bir
3: Üç
5: Beş
10: On
```

### SortedList'i Filtreleme

```csharp
using System;
using System.Collections.Generic;
using System.Linq;

class Program
{
    static void Main()
    {
        SortedList<string, int> urunler = new SortedList<string, int>
        {
            { "Laptop", 15000 },
            { "Telefon", 8000 },
            { "Tablet", 5000 },
            { "Monitör", 3500 },
            { "Klavye", 500 },
            { "Mouse", 300 }
        };
        
        // 5000 TL'den pahalı ürünleri filtreleme
        var pahaliUrunler = urunler.Where(x => x.Value > 5000);
        
        Console.WriteLine("5000 TL'den Pahalı Ürünler:");
        foreach (var urun in pahaliUrunler)
        {
            Console.WriteLine($"{urun.Key}: {urun.Value} TL");
        }
        
        // İsmi 'T' ile başlayan ürünleri filtreleme
        var tIleBaslayanlar = urunler.Where(x => x.Key.StartsWith("T"));
        
        Console.WriteLine("\n'T' ile Başlayan Ürünler:");
        foreach (var urun in tIleBaslayanlar)
        {
            Console.WriteLine($"{urun.Key}: {urun.Value} TL");
        }
        
        // Filtrelemeyi yeni bir SortedList'e çevirme
        SortedList<string, int> uygunFiyatliUrunler = new SortedList<string, int>();
        foreach (var urun in urunler.Where(x => x.Value <= 1000))
        {
            uygunFiyatliUrunler.Add(urun.Key, urun.Value);
        }
        
        Console.WriteLine("\n1000 TL ve Altındaki Ürünler (Yeni SortedList):");
        foreach (var urun in uygunFiyatliUrunler)
        {
            Console.WriteLine($"{urun.Key}: {urun.Value} TL");
        }
    }
}
```

Çıktı:
```
5000 TL'den Pahalı Ürünler:
Laptop: 15000 TL
Telefon: 8000 TL

'T' ile Başlayan Ürünler:
Tablet: 5000 TL
Telefon: 8000 TL

1000 TL ve Altındaki Ürünler (Yeni SortedList):
Klavye: 500 TL
Mouse: 300 TL
```

## Performans Özellikleri

SortedList<TKey, TValue>'in farklı işlemler için zaman karmaşıklıkları şu şekildedir:

| İşlem | Zaman Karmaşıklığı | Açıklama |
|-------|-------------------|----------|
| Ekleme (Add) | O(n) | Eleman eklerken, doğru konumu bulmak ve diğer elemanları kaydırmak için O(n) zaman gerekebilir |
| Erişim (Anahtar ile) | O(log n) | İkili arama (binary search) kullanılır |
| Erişim (İndeks ile) | O(1) | Dizi tabanlı erişim sağlar |
| Silme (Remove) | O(n) | Elemanı bulmak ve diğer elemanları kaydırmak gerekir |
| Arama (ContainsKey) | O(log n) | İkili arama kullanılır |

### Bellek Kullanımı

SortedList<TKey, TValue>, öğeleri saklamak için iki ayrı dizi kullanır: biri anahtarlar için, diğeri değerler için. Bu durum:

- Dictionary<TKey, TValue>'e göre daha az bellek kullanımı sağlar
- Büyük veri kümeleri için etkili bellek yönetimi sunar
- Seyrek kullanılan (sparse) veriler için uygun değildir

## İyi Uygulamalar

1. **Doğru Kullanım Senaryoları**:
   - Sıralanmış verilere ihtiyaç duyduğunuzda kullanın
   - Bellekte verimlilik önemli olduğunda tercih edin
   - Veri yapısına sık ekleme/silme işlemleri yapılmayacaksa kullanın

2. **Capacity Yönetimi**:
   - Başlangıçta yaklaşık boyutu biliyorsanız, Capacity özelliğini ayarlayarak performansı artırın
   - `TrimExcess()` metodunu kullanarak kullanılmayan kapasiteyi serbest bırakın

3. **Güvenli Erişim**:
   - Anahtar kontrolü için her zaman `ContainsKey()` veya `TryGetValue()` kullanın
   - Doğrudan anahtara erişim yerine güvenli yöntemleri tercih edin

4. **Performans İpuçları**:
   - Sık güncellemeler için SortedDictionary<TKey, TValue> kullanmayı düşünün
   - Çok büyük veri kümeleri için SortedList yerine SortedDictionary tercih edin

## Sık Karşılaşılan Sorunlar

1. **Duplike Anahtar Hatası**:

```csharp
// Hata Örneği
SortedList<string, int> liste = new SortedList<string, int>();
liste.Add("anahtar", 1);
liste.Add("anahtar", 2); // ArgumentException fırlatır
```

Çözüm:
```csharp
// TryGetValue kullanarak kontrol
if (!liste.ContainsKey("anahtar"))
{
    liste.Add("anahtar", 2);
}
// Veya güncelleme için
liste["anahtar"] = 2; // Varsa günceller, yoksa ekler
```

2. **Null Anahtar Kullanımı**:

```csharp
// Hata Örneği
SortedList<string, int> liste = new SortedList<string, int>();
liste.Add(null, 1); // ArgumentNullException fırlatır
```

Çözüm:
```csharp
// Null kontrolü yapma
string anahtar = null;
if (anahtar != null)
{
    liste.Add(anahtar, 1);
}
else
{
    // Alternatif işlem
    liste.Add("varsayılan", 1);
}
```

3. **Karşılaştırılamayan Anahtarlar**:

```csharp
// Özel sınıf, IComparable implemente etmeden kullanıldığında
class OzelSinif { public string Ad { get; set; } }

// Hata Örneği
SortedList<OzelSinif, string> liste = new SortedList<OzelSinif, string>();
liste.Add(new OzelSinif { Ad = "Test" }, "değer"); // ArgumentException fırlatır
```

Çözüm:
```csharp
// Özel karşılaştırıcı kullanma
class OzelSinifKarsilastirici : IComparer<OzelSinif>
{
    public int Compare(OzelSinif x, OzelSinif y)
    {
        return string.Compare(x.Ad, y.Ad);
    }
}

// Karşılaştırıcı ile kullanım
SortedList<OzelSinif, string> liste = new SortedList<OzelSinif, string>(new OzelSinifKarsilastirici());
liste.Add(new OzelSinif { Ad = "Test" }, "değer"); // Çalışır
```

## Özet

SortedList<TKey, TValue>, anahtarlara göre sıralanmış veri saklama ihtiyaçları için ideal bir koleksiyon yapısıdır. Özellikle:

- **Anahtarlara göre sıralanmış veri** gerektiğinde
- **Hem anahtar hem indeks erişimi** istendiğinde
- **Bellek kullanımının** önemli olduğu durumlarda
- **Daha çok okuma, daha az yazma/güncelleme** işlemi yapılan senaryolarda

Bu koleksiyon sınıfı, veri ekleme ve silme işlemleri sık yapılmayan, ancak sıralı erişim gerektiren durumlarda güçlü bir seçenektir. Dictionary ile karşılaştırıldığında daha az bellek kullanır, ancak anahtar ekleme ve silme işlemleri daha yavaştır.

Doğru kullanıldığında, SortedList<TKey, TValue> çeşitli veri saklama ve işleme gereksinimleri için etkili bir çözüm sunar.
