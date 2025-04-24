# C# SortedDictionary<TKey, TValue> Kılavuzu

SortedDictionary<TKey, TValue>, C# dilinde anahtarların sıralı olarak tutulduğu bir koleksiyon türüdür. Bu yapı, hem verileri anahtar-değer çiftleri şeklinde depolamanızı hem de bu verilere anahtarların sıralı hali üzerinden erişmenizi sağlar.

## İçindekiler
- Temel Özellikler
- SortedDictionary vs Dictionary
- SortedDictionary vs SortedList
- Kullanım Örnekleri
- Performans Değerlendirmesi
- En İyi Kullanım Senaryoları

## Temel Özellikler

SortedDictionary<TKey, TValue> sınıfı, System.Collections.Generic namespace'i altında bulunur ve şu özelliklere sahiptir:

- **Sıralı Anahtarlar**: Anahtarlar, varsayılan olarak artan sırada saklanır.
- **Benzersiz Anahtarlar**: Her anahtar yalnızca bir kez yer alabilir.
- **Hızlı Arama**: İkili arama ağacı (Red-Black Tree) yapısını kullanır.
- **Dinamik Boyut**: Eleman sayısı dinamik olarak artıp azalabilir.

## SortedDictionary vs Dictionary

| Özellik | SortedDictionary | Dictionary |
|---------|-----------------|------------|
| Sıralama | Anahtarlar sıralı | Anahtarlar sırasız |
| Yapı | İkili arama ağacı | Hash tablosu |
| Arama | O(log n) | O(1) (ortalama) |
| Ekleme/Silme | O(log n) | O(1) (ortalama) |
| Bellek | Daha az bellek | Daha fazla bellek |

## SortedDictionary vs SortedList

| Özellik | SortedDictionary | SortedList |
|---------|-----------------|------------|
| Yapı | İkili arama ağacı | Sıralı diziler |
| Ekleme/Silme | O(log n) | O(n) |
| Bellek | Daha fazla bellek | Daha az bellek |
| İndeksleyiciler | Sadece anahtar | Anahtar veya indeks |
| En iyi kullanım | Dinamik koleksiyonlar | Çoğunlukla statik koleksiyonlar |

## Kullanım Örnekleri

### Temel Kullanım

```csharp
// SortedDictionary oluşturma
SortedDictionary<int, string> siraliSozluk = new SortedDictionary<int, string>();

// Eleman ekleme
siraliSozluk.Add(3, "Üç");
siraliSozluk.Add(1, "Bir");
siraliSozluk.Add(2, "İki");

// Elemanlara erişim
string deger = siraliSozluk[2]; // "İki" değerini döndürür

// Tüm elemanları listeleme (anahtarlara göre sıralı olacaktır: 1, 2, 3)
foreach (KeyValuePair<int, string> item in siraliSozluk)
{
    Console.WriteLine($"Anahtar: {item.Key}, Değer: {item.Value}");
}
```

### Özel Karşılaştırıcı Kullanımı

```csharp
// String anahtarları büyük/küçük harf duyarsız şekilde sıralamak için
var buyukKucukHarfDuyarsiz = new SortedDictionary<string, int>(StringComparer.OrdinalIgnoreCase);

buyukKucukHarfDuyarsiz.Add("elma", 1);
buyukKucukHarfDuyarsiz.Add("ARMUT", 2);
buyukKucukHarfDuyarsiz.Add("Muz", 3);

// Anahtarlar karşılaştırıcıya göre sıralanacaktır
foreach (var item in buyukKucukHarfDuyarsiz)
{
    Console.WriteLine($"{item.Key}: {item.Value}");
}
```

### Özel Sınıf Anahtarları

```csharp
public class Ogrenci : IComparable<Ogrenci>
{
    public int OgrenciNo { get; set; }
    public string Ad { get; set; }

    public int CompareTo(Ogrenci other)
    {
        return this.OgrenciNo.CompareTo(other.OgrenciNo);
    }
    
    public override string ToString()
    {
        return $"{OgrenciNo} - {Ad}";
    }
}

// Kullanım
SortedDictionary<Ogrenci, float> notlar = new SortedDictionary<Ogrenci, float>();

notlar.Add(new Ogrenci { OgrenciNo = 103, Ad = "Ali" }, 85.5f);
notlar.Add(new Ogrenci { OgrenciNo = 101, Ad = "Ayşe" }, 92.0f);
notlar.Add(new Ogrenci { OgrenciNo = 102, Ad = "Mehmet" }, 78.5f);

// Öğrenciler öğrenci numarasına göre sıralanacaktır
foreach (var item in notlar)
{
    Console.WriteLine($"{item.Key}: {item.Value}");
}
```

## Performans Değerlendirmesi

SortedDictionary sınıfı, ikili arama ağacı (Red-Black Tree) yapısını kullandığı için:

- **Arama, Ekleme ve Silme**: O(log n) karmaşıklığına sahiptir.
- **Yineleme**: O(n) karmaşıklığında tüm koleksiyonu dolaşabilir.
- **Bellek Kullanımı**: Her düğüm için ekstra referanslar içerdiğinden normal Dictionary'den daha fazla bellek kullanır.

## En İyi Kullanım Senaryoları

SortedDictionary şu durumlarda tercih edilmelidir:

1. **Sıralı erişim gerektiğinde**: Anahtarlara sıralı erişim önemliyse.
2. **Dinamik koleksiyonlarda**: Sık ekleme/silme işlemleri yapılacaksa.
3. **Anahtar aralığı sorgulamalarında**: Belirli anahtar aralıklarına erişim gerektiğinde.

Örnek senaryo:

- Kelime frekansı analizi
- Sıralı olay zamanlaması
- Öncelik sırası gerektiren işlemler
- İsim veya ID'ye göre sıralanmış veri yapıları

SortedDictionary, koleksiyonun boyutu büyüdükçe ve sıralama önemli oldukça daha değerli hale gelir.