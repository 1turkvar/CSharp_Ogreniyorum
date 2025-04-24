# C# HashSet Detaylı İnceleme

## İçindekiler
1. [Giriş](#giriş)
2. [HashSet Nedir?](#hashset-nedir)
3. [HashSet Oluşturma](#hashset-oluşturma)
4. [Temel Özellikleri ve Metodları](#temel-özellikleri-ve-metodları)
5. [HashSet Küme İşlemleri](#hashset-küme-işlemleri)
6. [Performans Özellikleri](#performans-özellikleri)
7. [HashSet vs List vs Dictionary](#hashset-vs-list-vs-dictionary)
8. [HashSet Örnek Kullanım Senaryoları](#hashset-örnek-kullanım-senaryoları)
9. [HashSet ile İlgili İyi Pratikler](#hashset-ile-ilgili-iyi-pratikler)
10. [Sonuç](#sonuç)

## Giriş

.NET Framework ve .NET Core içerisinde bulunan `HashSet<T>` sınıfı, benzersiz elemanlar içeren yüksek performanslı bir koleksiyon türüdür. Bu belge, C# dilinde HashSet'in kullanımı, avantajları, dezavantajları ve uygulama örneklerini kapsamlı bir şekilde incelemektedir.

## HashSet Nedir?

`HashSet<T>`, tekrarlayan öğeler içermeyen bir koleksiyon sınıfıdır. Matematikteki "küme" kavramına benzer şekilde, HashSet bir eleman grubunu benzersiz bir şekilde tutmak için tasarlanmıştır. `System.Collections.Generic` namespace'i altında bulunur ve .NET Framework 3.5 ile birlikte tanıtılmıştır.

HashSet'in en önemli özellikleri:
- Tekrarlayan öğeler içermez (her eleman benzersizdir)
- Elemanlar belirli bir sıraya göre tutulmaz (sırasızdır)
- Elemanları aramak, eklemek ve çıkarmak için hash tablosu kullanır, bu da O(1) karmaşıklığı sağlar
- Küme işlemleri (birleşim, kesişim, fark, alt küme kontrolü) için optimize edilmiştir

## HashSet Oluşturma

HashSet çeşitli şekillerde oluşturulabilir:

```csharp
// Boş bir HashSet oluşturma
HashSet<int> numbers = new HashSet<int>();

// Elemanlarla başlatma
HashSet<string> fruits = new HashSet<string> { "elma", "armut", "muz" };

// Başka bir koleksiyondan oluşturma
List<char> charList = new List<char> { 'a', 'b', 'c', 'a' };
HashSet<char> uniqueChars = new HashSet<char>(charList); // Sadece 'a', 'b', 'c' içerir

// IEqualityComparer ile özel karşılaştırma mantığı sağlama
HashSet<string> caseInsensitiveSet = new HashSet<string>(StringComparer.OrdinalIgnoreCase);
caseInsensitiveSet.Add("Hello");
caseInsensitiveSet.Add("hello"); // Eklenmez çünkü case-insensitive olarak aynı kabul edilir
```

## Temel Özellikleri ve Metodları

### Temel Özellikler

```csharp
HashSet<int> set = new HashSet<int> { 1, 2, 3, 4, 5 };

// Count - eleman sayısını döndürür
int count = set.Count; // 5

// Comparer - HashSet'in kullandığı IEqualityComparer<T> nesnesini döndürür
IEqualityComparer<int> comparer = set.Comparer;
```

### Ekleme ve Çıkarma İşlemleri

```csharp
HashSet<string> colors = new HashSet<string>();

// Add - bir eleman ekler, zaten varsa false döndürür
bool added = colors.Add("kırmızı"); // true döner, eklendi
bool addedAgain = colors.Add("kırmızı"); // false döner, zaten vardı

// Remove - elemanı siler, başarılıysa true döndürür
bool removed = colors.Remove("kırmızı"); // true döner
bool removedAgain = colors.Remove("kırmızı"); // false döner, eleman yok

// Clear - tüm elemanları temizler
colors.Clear();
```

### Arama ve Kontrol İşlemleri

```csharp
HashSet<int> numbers = new HashSet<int> { 10, 20, 30, 40, 50 };

// Contains - eleman var mı kontrolü
bool exists = numbers.Contains(30); // true
bool notExists = numbers.Contains(60); // false

// TryGetValue - değeri bulup referans olarak döndürür
int value = 25;
if (numbers.TryGetValue(value, out int actualValue))
{
    // Eleman bulunamadı, actualValue değiştirilmedi
}

value = 30;
if (numbers.TryGetValue(value, out actualValue))
{
    // Eleman bulundu, actualValue = 30
}
```

## HashSet Küme İşlemleri

HashSet, klasik küme işlemlerini destekler:

```csharp
HashSet<int> set1 = new HashSet<int> { 1, 2, 3, 4, 5 };
HashSet<int> set2 = new HashSet<int> { 3, 4, 5, 6, 7 };

// UnionWith - Birleşim işlemi (set1'e set2'nin elemanlarını ekler)
set1.UnionWith(set2);
// set1 şimdi: { 1, 2, 3, 4, 5, 6, 7 }

// IntersectWith - Kesişim işlemi (set1'de sadece set2 ile ortak elemanlar kalır)
set1 = new HashSet<int> { 1, 2, 3, 4, 5 };
set1.IntersectWith(set2);
// set1 şimdi: { 3, 4, 5 }

// ExceptWith - Fark işlemi (set1'den set2'nin elemanlarını çıkarır)
set1 = new HashSet<int> { 1, 2, 3, 4, 5 };
set1.ExceptWith(set2);
// set1 şimdi: { 1, 2 }

// SymmetricExceptWith - Simetrik fark (her iki kümenin de sadece diğerinde bulunmayan elemanları)
set1 = new HashSet<int> { 1, 2, 3, 4, 5 };
set1.SymmetricExceptWith(set2);
// set1 şimdi: { 1, 2, 6, 7 }
```

### Küme İlişki Kontrolleri

```csharp
HashSet<int> set1 = new HashSet<int> { 1, 2, 3 };
HashSet<int> set2 = new HashSet<int> { 1, 2, 3, 4, 5 };
HashSet<int> set3 = new HashSet<int> { 6, 7, 8 };

// IsSubsetOf - Alt küme kontrolü
bool isSubset = set1.IsSubsetOf(set2); // true

// IsSupersetOf - Üst küme kontrolü
bool isSuperset = set2.IsSupersetOf(set1); // true

// IsProperSubsetOf - Öz alt küme kontrolü (alt küme ve eşit değil)
bool isProperSubset = set1.IsProperSubsetOf(set2); // true

// IsProperSupersetOf - Öz üst küme kontrolü (üst küme ve eşit değil)
bool isProperSuperset = set2.IsProperSupersetOf(set1); // true

// Overlaps - En az bir ortak eleman kontrolü
bool overlaps = set1.Overlaps(set2); // true
bool noOverlap = set1.Overlaps(set3); // false

// SetEquals - Aynı elemanlardan oluşma kontrolü
bool equals = set1.SetEquals(new HashSet<int> { 3, 2, 1 }); // true
```

## Performans Özellikleri

HashSet'in zaman karmaşıklığı:

| İşlem | Ortalama Durum | En Kötü Durum |
|-------|---------------|--------------|
| Add   | O(1)         | O(n)         |
| Remove| O(1)         | O(n)         |
| Contains | O(1)      | O(n)         |
| UnionWith | O(n)     | O(n)         |
| IntersectWith | O(n) | O(n)         |
| ExceptWith | O(n)    | O(n)         |

En kötü durum performansı, hash fonksiyonunun tüm değerler için aynı hash değerini döndürmesi gibi nadir durumlarda ortaya çıkar. Bu durumda, HashSet içsel olarak bağlı liste gibi davranır.

## HashSet vs List vs Dictionary

| Özellik | HashSet | List | Dictionary |
|---------|---------|------|------------|
| Benzersiz Elemanlar | Evet | Hayır | Anahtar benzersiz |
| Sıralı Koleksiyon | Hayır | Evet | Hayır |
| İndeks ile Erişim | Hayır | Evet | Anahtar ile |
| Arama Performansı | O(1) | O(n) | O(1) |
| Ekleme Performansı | O(1) | O(1)/O(n) | O(1) |
| Küme İşlemleri | Evet | Hayır | Hayır |
| İki Yönlü İlişki | Hayır | Hayır | Evet |

Ne zaman kullanmalı:
- **HashSet**: Benzersiz elemanlar ve hızlı arama gerektiren küme işlemleri için
- **List**: Sıralı, tekrarlı elemanlar gerektiğinde ve indeks ile erişim önemliyse
- **Dictionary**: Anahtar-değer ilişkisi gereken durumlarda

## HashSet Örnek Kullanım Senaryoları

### 1. Tekrarlanan Elemanları Kaldırma

```csharp
List<string> rawData = new List<string> { "ankara", "istanbul", "izmir", "ankara", "bursa", "istanbul" };
HashSet<string> uniqueCities = new HashSet<string>(rawData);

// uniqueCities: { "ankara", "istanbul", "izmir", "bursa" }
```

### 2. Hızlı Arama ve Üyelik Kontrolü

```csharp
HashSet<string> validUsernames = new HashSet<string>(StringComparer.OrdinalIgnoreCase)
{
    "admin", "user123", "moderator", "guest"
};

string username = "Admin";
if (validUsernames.Contains(username))
{
    // Kullanıcı adı geçerli (büyük-küçük harf duyarsız)
}
```

### 3. Kesişim Bulma (Ortak Elemanları Bulma)

```csharp
HashSet<int> aliceNumbers = new HashSet<int> { 1, 3, 5, 7, 9 };
HashSet<int> bobNumbers = new HashSet<int> { 2, 3, 5, 7, 11 };

HashSet<int> commonNumbers = new HashSet<int>(aliceNumbers);
commonNumbers.IntersectWith(bobNumbers);
// commonNumbers: { 3, 5, 7 }
```

### 4. Sözcük Frekansı Analizi

```csharp
string text = "Bu bir örnek metin. Bu metin örnek olarak kullanılacak.";
string[] words = text.Split(new[] { ' ', '.', ',', '!', '?' }, StringSplitOptions.RemoveEmptyEntries);

HashSet<string> uniqueWords = new HashSet<string>(words, StringComparer.OrdinalIgnoreCase);
Console.WriteLine($"Benzersiz kelime sayısı: {uniqueWords.Count}");
```

### 5. Graf Algoritmaları (Ziyaret Edilen Düğümleri Takip Etme)

```csharp
public void DepthFirstSearch(Node startNode)
{
    HashSet<Node> visited = new HashSet<Node>();
    DFSRecursive(startNode, visited);
}

private void DFSRecursive(Node node, HashSet<Node> visited)
{
    if (node == null || visited.Contains(node))
        return;
        
    visited.Add(node);
    Console.WriteLine($"Düğüm ziyaret edildi: {node.Value}");
    
    foreach (var neighbor in node.Neighbors)
    {
        DFSRecursive(neighbor, visited);
    }
}
```

## HashSet ile İlgili İyi Pratikler

1. **Doğru Eşitlik Karşılaştırıcı Kullanımı**

Özel nesnelerle çalışırken, `GetHashCode()` ve `Equals()` metotlarını düzgün şekilde override etmeniz veya uygun bir `IEqualityComparer<T>` uygulaması sağlamanız önemlidir.

```csharp
public class Person
{
    public string Name { get; set; }
    public int Age { get; set; }
    
    public override bool Equals(object obj)
    {
        if (obj is Person other)
            return Name == other.Name && Age == other.Age;
        return false;
    }
    
    public override int GetHashCode()
    {
        return (Name, Age).GetHashCode();
    }
}
```

2. **Küme İşlemlerinin Doğru Kullanımı**

Mevcut bir HashSet'i değiştirmek istemiyorsanız, işlem sonucunu yeni bir HashSet'e aktarın:

```csharp
HashSet<int> set1 = new HashSet<int> { 1, 2, 3, 4, 5 };
HashSet<int> set2 = new HashSet<int> { 3, 4, 5, 6, 7 };

// Orijinal set1'i koruyarak birleşim bulma
HashSet<int> union = new HashSet<int>(set1);
union.UnionWith(set2);
```

3. **Kapasite Yönetimi**

Büyük veri setleriyle çalışırken, başlangıç kapasitesini belirlemek performansı artırabilir:

```csharp
// 10000 elemanlı bir koleksiyon için
HashSet<int> largeSet = new HashSet<int>(10000);
```

4. **Thread-Safe Kullanım**

HashSet thread-safe değildir. Eşzamanlı erişim gerektiren durumlarda kilitleme mekanizmaları kullanın:

```csharp
HashSet<string> sharedSet = new HashSet<string>();
object lockObj = new object();

// Güvenli ekleme
lock (lockObj)
{
    sharedSet.Add("item");
}
```

Ya da `ConcurrentDictionary<T, byte>` kullanarak thread-safe bir benzersiz koleksiyon oluşturun:

```csharp
ConcurrentDictionary<string, byte> concurrentSet = new ConcurrentDictionary<string, byte>();
concurrentSet.TryAdd("item", 0); // İkinci değer önemsiz
```

## Sonuç

HashSet, C#'ta benzersiz elemanlarla çalışmak ve küme işlemleri gerçekleştirmek için güçlü ve verimli bir koleksiyondur. Doğru kullanıldığında, arama, ekleme ve çıkarma işlemlerinde neredeyse sabit zamanlı performans sunarak, tekrarlayan verilerin elenmesi, hızlı üyelik kontrolü ve matematiksel küme işlemleri için ideal bir seçenektir.

HashSet'in sıra garantisi olmadığını ve null değerleri kabul ettiğini (referans tipleri için) unutmayın. Eğer sıralı bir benzersiz koleksiyona ihtiyacınız varsa, `SortedSet<T>` kullanmayı düşünebilirsiniz, ancak bu sınıf O(log n) performans sunar.

C#'ta HashSet, Modern ve performans odaklı uygulamalar geliştirirken, veri yapınızın gereksinimlerine göre HashSet'in avantajlarından yararlanabilirsiniz.
