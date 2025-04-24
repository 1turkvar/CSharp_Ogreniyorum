# C# LinkedList Detaylı Rehber

## İçindekiler
- [Giriş](#giriş)
- [LinkedList Nedir?](#linkedlist-nedir)
- [LinkedList vs. List](#linkedlist-vs-list)
- [LinkedList Sınıfı ve Özellikleri](#linkedlist-sınıfı-ve-özellikleri)
- [LinkedListNode Sınıfı](#linkedlistnode-sınıfı)
- [LinkedList Oluşturma ve Temel İşlemler](#linkedlist-oluşturma-ve-temel-i̇şlemler)
- [Düğüm Ekleme ve Çıkarma İşlemleri](#düğüm-ekleme-ve-çıkarma-i̇şlemleri)
- [LinkedList'te Gezinme](#linkedlistte-gezinme)
- [LinkedList Kullanım Örnekleri](#linkedlist-kullanım-örnekleri)
- [İleri Düzey Örnekler](#i̇leri-düzey-örnekler)
- [Performans Değerlendirmesi](#performans-değerlendirmesi)
- [En İyi Kullanım Senaryoları](#en-i̇yi-kullanım-senaryoları)
- [Sonuç](#sonuç)

## Giriş

C# programlama dilinde, veri yapıları verilerin organize edilmesi ve işlenmesi için kritik öneme sahiptir. LinkedList, özellikle dinamik veri manipülasyonu gerektiren senaryolarda önemli bir veri yapısıdır. Bu dokümanda, C#'taki LinkedList veri yapısını detaylı bir şekilde inceleyeceğiz.

## LinkedList Nedir?

LinkedList, her bir elemanın (düğüm/node) bir sonraki ve bir önceki elemana referans içerdiği çift yönlü bağlı bir liste yapısıdır. C#'ta, LinkedList<T> sınıfı System.Collections.Generic namespace'i altında bulunur ve generic (genel) bir koleksiyon yapısı sunar.

Çift yönlü bağlı liste olarak:
- Her bir düğüm, veriyi ve komşu düğümlere referansları tutar
- Her düğüm hem önceki hem de sonraki düğüme işaret eder
- Liste, başlangıç (First) ve bitiş (Last) düğümlerine direkt erişim sağlar

```
[NULL] <- [Önceki | Data | Sonraki] <-> [Önceki | Data | Sonraki] <-> [Önceki | Data | Sonraki] -> [NULL]
   ^                                                                                                  ^
   |                                                                                                  |
 First                                                                                               Last
```

## LinkedList vs. List

LinkedList ve List (ArrayList) arasındaki temel farklar:

| Özellik | LinkedList<T> | List<T> |
|---------|--------------|---------|
| Veri Yapısı | Çift yönlü bağlı liste | Dinamik dizi |
| Bellek Kullanımı | Her eleman için ekstra referans belleği | Daha verimli bellek kullanımı |
| Eleman Ekleme/Silme | O(1) sabit zaman (konum biliniyorsa) | O(n) doğrusal zaman (orta/baş) |
| Rastgele Erişim | O(n) doğrusal erişim | O(1) sabit zaman |
| Dizin Erişimi | Desteklemez (dizin yok) | Destekler |
| En İyi Kullanım | Sık ekleme/silme işlemleri | Sık okuma/erişim işlemleri |

## LinkedList Sınıfı ve Özellikleri

LinkedList<T> sınıfı, generic (genel) bir koleksiyon sınıfıdır ve şu özellikleri içerir:

```csharp
public class LinkedList<T> : System.Collections.Generic.ICollection<T>,
                              System.Collections.Generic.IEnumerable<T>,
                              System.Collections.ICollection,
                              System.Collections.IEnumerable,
                              System.Runtime.Serialization.ISerializable,
                              System.Runtime.Serialization.IDeserializationCallback
```

### Temel Özellikler

| Özellik | Açıklama |
|---------|----------|
| Count | LinkedList içindeki düğüm sayısını döndürür |
| First | İlk düğümü döndürür (LinkedListNode<T>) |
| Last | Son düğümü döndürür (LinkedListNode<T>) |

### Temel Metotlar

| Metot | Açıklama |
|-------|----------|
| AddFirst(T value) | Listenin başına yeni bir düğüm ekler |
| AddFirst(LinkedListNode<T> node) | Belirtilen düğümü listenin başına ekler |
| AddLast(T value) | Listenin sonuna yeni bir düğüm ekler |
| AddLast(LinkedListNode<T> node) | Belirtilen düğümü listenin sonuna ekler |
| AddAfter(), AddBefore() | Belirli bir düğümün önüne veya arkasına ekleme yapar |
| Clear() | Listedeki tüm düğümleri temizler |
| Contains(T value) | Belirtilen değerin listede olup olmadığını kontrol eder |
| Find(T value) | Belirtilen değeri içeren ilk düğümü bulur |
| FindLast(T value) | Belirtilen değeri içeren son düğümü bulur |
| Remove(T value) | Belirtilen değeri içeren ilk düğümü kaldırır |
| Remove(LinkedListNode<T> node) | Belirtilen düğümü kaldırır |
| RemoveFirst() | İlk düğümü kaldırır |
| RemoveLast() | Son düğümü kaldırır |

## LinkedListNode Sınıfı

LinkedListNode<T>, LinkedList<T> içindeki her bir düğümü temsil eden bir sınıftır:

```csharp
public sealed class LinkedListNode<T>
```

### LinkedListNode Özellikleri

| Özellik | Açıklama |
|---------|----------|
| Value | Düğümün değerini tutar veya ayarlar |
| Next | Sonraki düğüme referans (bir sonraki düğüm yoksa null) |
| Previous | Önceki düğüme referans (bir önceki düğüm yoksa null) |
| List | Düğümün ait olduğu LinkedList'e referans |

## LinkedList Oluşturma ve Temel İşlemler

### LinkedList Oluşturma

```csharp
// Boş bir LinkedList oluşturma
LinkedList<string> isimler = new LinkedList<string>();

// Var olan bir koleksiyondan LinkedList oluşturma
string[] isimDizisi = { "Ali", "Veli", "Mehmet", "Ayşe" };
LinkedList<string> isimListesi = new LinkedList<string>(isimDizisi);
```

### Temel İşlemler

```csharp
LinkedList<int> sayilar = new LinkedList<int>();

// Eleman ekleme
sayilar.AddLast(10);
sayilar.AddLast(20);
sayilar.AddFirst(5);  // Liste başına ekleme
                      
// Sonuç: 5 -> 10 -> 20

// Elemanları dolaşma
foreach (int sayi in sayilar)
{
    Console.WriteLine(sayi);
}

// Eleman arama
LinkedListNode<int> bulunanDugum = sayilar.Find(10);
if (bulunanDugum != null)
{
    Console.WriteLine($"Bulunan değer: {bulunanDugum.Value}");
}

// Eleman silme
sayilar.Remove(10);      // Değere göre silme
sayilar.RemoveFirst();   // İlk elemanı silme
sayilar.RemoveLast();    // Son elemanı silme

// Liste temizleme
sayilar.Clear();
```

## Düğüm Ekleme ve Çıkarma İşlemleri

### Belirli Konumlara Düğüm Ekleme

```csharp
LinkedList<string> dersler = new LinkedList<string>();

// Temel eklemeler
dersler.AddLast("Matematik");
dersler.AddLast("Fizik");
dersler.AddLast("Kimya");

// Belirli bir düğümün önüne veya arkasına ekleme
LinkedListNode<string> fizikDugumu = dersler.Find("Fizik");
if (fizikDugumu != null)
{
    dersler.AddBefore(fizikDugumu, "Biyoloji");  // Fizik'ten önce
    dersler.AddAfter(fizikDugumu, "İngilizce");  // Fizik'ten sonra
}

// Sonuç: Matematik -> Biyoloji -> Fizik -> İngilizce -> Kimya

// Node oluşturup ekleme
LinkedListNode<string> tarihDugumu = new LinkedListNode<string>("Tarih");
dersler.AddLast(tarihDugumu);
```

### Düğüm Çıkarma

```csharp
// Değere göre çıkarma
dersler.Remove("Fizik");

// Düğüme göre çıkarma
LinkedListNode<string> biyolojiDugumu = dersler.Find("Biyoloji");
if (biyolojiDugumu != null)
{
    dersler.Remove(biyolojiDugumu);
}

// İlk/son düğümü çıkarma
dersler.RemoveFirst();
dersler.RemoveLast();
```

## LinkedList'te Gezinme

LinkedList yapısı, düğümler arasında ileri ve geri gezinme imkanı sağlar.

```csharp
LinkedList<int> sayilar = new LinkedList<int>(new[] { 10, 20, 30, 40, 50 });

// İlk düğümden başlayarak ileri doğru gezinme
LinkedListNode<int> current = sayilar.First;
while (current != null)
{
    Console.WriteLine(current.Value);
    current = current.Next;
}

// Son düğümden başlayarak geriye doğru gezinme
current = sayilar.Last;
while (current != null)
{
    Console.WriteLine(current.Value);
    current = current.Previous;
}

// Belirli bir düğümden başlayarak ileri/geri gezinme
LinkedListNode<int> ortaDugum = sayilar.Find(30);
if (ortaDugum != null)
{
    // İleri doğru
    LinkedListNode<int> temp = ortaDugum;
    while (temp != null)
    {
        Console.WriteLine(temp.Value);
        temp = temp.Next;
    }
    
    // Geri doğru
    temp = ortaDugum;
    while (temp != null)
    {
        Console.WriteLine(temp.Value);
        temp = temp.Previous;
    }
}
```

## LinkedList Kullanım Örnekleri

### Örnek 1: Palindrom Kontrolü

```csharp
public static bool IsPalindrome<T>(LinkedList<T> list) where T : IEquatable<T>
{
    if (list.Count <= 1)
        return true;
        
    LinkedListNode<T> forward = list.First;
    LinkedListNode<T> backward = list.Last;
    
    while (forward != backward && forward.Previous != backward)
    {
        if (!forward.Value.Equals(backward.Value))
            return false;
            
        forward = forward.Next;
        backward = backward.Previous;
    }
    
    return true;
}

// Kullanım
LinkedList<char> text = new LinkedList<char>("radar".ToCharArray());
bool isPalindrome = IsPalindrome(text);  // true
```

### Örnek 2: Dairesel Kuyruk Uygulaması

```csharp
public class CircularQueue<T>
{
    private LinkedList<T> _items;
    private int _capacity;
    
    public CircularQueue(int capacity)
    {
        _items = new LinkedList<T>();
        _capacity = capacity;
    }
    
    public void Enqueue(T item)
    {
        if (_items.Count >= _capacity)
        {
            // Kapasiteye ulaşıldığında ilk elemanı çıkar
            _items.RemoveFirst();
        }
        
        _items.AddLast(item);
    }
    
    public T Dequeue()
    {
        if (_items.Count == 0)
            throw new InvalidOperationException("Kuyruk boş");
            
        T value = _items.First.Value;
        _items.RemoveFirst();
        return value;
    }
    
    public T Peek()
    {
        if (_items.Count == 0)
            throw new InvalidOperationException("Kuyruk boş");
            
        return _items.First.Value;
    }
    
    public int Count => _items.Count;
    public bool IsEmpty => _items.Count == 0;
}
```

## İleri Düzey Örnekler

### Örnek 3: İki LinkedList'i Birleştirme

```csharp
public static LinkedList<T> MergeSortedLists<T>(LinkedList<T> list1, LinkedList<T> list2) 
    where T : IComparable<T>
{
    LinkedList<T> result = new LinkedList<T>();
    
    LinkedListNode<T> node1 = list1.First;
    LinkedListNode<T> node2 = list2.First;
    
    while (node1 != null && node2 != null)
    {
        if (node1.Value.CompareTo(node2.Value) <= 0)
        {
            result.AddLast(node1.Value);
            node1 = node1.Next;
        }
        else
        {
            result.AddLast(node2.Value);
            node2 = node2.Next;
        }
    }
    
    // Kalan elemanları ekle
    while (node1 != null)
    {
        result.AddLast(node1.Value);
        node1 = node1.Next;
    }
    
    while (node2 != null)
    {
        result.AddLast(node2.Value);
        node2 = node2.Next;
    }
    
    return result;
}

// Kullanım
LinkedList<int> list1 = new LinkedList<int>(new[] { 1, 3, 5, 7 });
LinkedList<int> list2 = new LinkedList<int>(new[] { 2, 4, 6, 8 });
LinkedList<int> merged = MergeSortedLists(list1, list2);
// Sonuç: 1, 2, 3, 4, 5, 6, 7, 8
```

### Örnek 4: LinkedList'i Tersine Çevirme

```csharp
public static void ReverseLinkedList<T>(LinkedList<T> list)
{
    if (list.Count <= 1)
        return;
        
    LinkedListNode<T> current = list.First;
    while (current != null)
    {
        LinkedListNode<T> nextNode = current.Next;
        
        // Mevcut düğümü çıkarıp listenin başına ekle
        list.Remove(current);
        list.AddFirst(current);
        
        current = nextNode;
    }
}

// Kullanım
LinkedList<int> numbers = new LinkedList<int>(new[] { 1, 2, 3, 4, 5 });
ReverseLinkedList(numbers);
// Sonuç: 5, 4, 3, 2, 1
```

## Performans Değerlendirmesi

LinkedList'in performans özellikleri:

1. **Ekleme ve Silme İşlemleri**:
   - Başa/sona ekleme/silme: O(1) - sabit zaman
   - Ortaya ekleme/silme (konum biliniyorsa): O(1) - sabit zaman
   - Değere göre silme: O(n) - doğrusal zaman (önce arama gerekir)

2. **Arama İşlemleri**:
   - Eleman arama: O(n) - doğrusal zaman
   - İndeks ile erişim: Desteklenmez (dizin olmadığından)

3. **Bellek Kullanımı**:
   - Her eleman için ekstra bellek (önceki/sonraki referanslar)
   - Dizi tabanlı koleksiyonlara göre bellek kullanımı daha fazla

4. **İterasyon**:
   - İleri/geri yönde verimli gezinme

## En İyi Kullanım Senaryoları

LinkedList, aşağıdaki senaryolarda özellikle faydalıdır:

1. **Sık Ekleme/Silme**: Koleksiyonun ortasında sık ekleme/silme işlemleri yapılacaksa
2. **Çift Yönlü Gezinme**: Elemanlar arasında hem ileri hem geri gezinme gerekiyorsa
3. **Referansa Dayalı Erişim**: İndeks yerine referans tabanlı erişim gerekiyorsa
4. **Kuyruk/Yığın İmplementasyonları**: FIFO veya LIFO veri yapıları oluşturulacaksa
5. **Öncelik Kuyrukları**: Düzenlenmiş sıralı yapılar gerekiyorsa
6. **Cache Mekanizmaları**: LRU (en az kullanılan) cache yapıları oluşturulacaksa

LinkedList şu durumlarda **verimli değildir**:

1. Sık rastgele erişim gerekiyorsa
2. Bellek kullanımı kritik öneme sahipse
3. Dizin tabanlı erişim gerekiyorsa
4. İterasyon işlemleri daha sık yapılacaksa
5. Elemanlarda sıralama ön planda ise

## Sonuç

C#'taki LinkedList<T> sınıfı, çift yönlü bağlı liste yapısını güçlü ve esnek bir şekilde sunar. Özellikle sık ekleme/silme işlemleri yapılacak uygulamalarda, LinkedList doğru veri yapısı seçimi olabilir. Ancak, her veri yapısında olduğu gibi, kullanım senaryonuza en uygun yapıyı seçmek performans açısından önemlidir.

Doğru kullanıldığında, LinkedList<T> sınıfı, dinamik ve verimli veri manipülasyonu sağlayan güçlü bir araçtır. Bu dokümanda sunulan bilgi ve örnekler, C# LinkedList'i etkili bir şekilde kullanmanıza yardımcı olacaktır.
