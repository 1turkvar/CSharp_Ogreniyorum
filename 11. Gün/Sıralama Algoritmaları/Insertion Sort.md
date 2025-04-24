# C# Insertion Sort Algoritması

## İçindekiler
1. [Giriş](#giriş)
2. [Algoritmanın Çalışma Prensibi](#algoritmanın-çalışma-prensibi)
3. [C# Implementasyonu](#c-implementasyonu)
4. [Zaman ve Yer Karmaşıklığı](#zaman-ve-yer-karmaşıklığı)
5. [Avantajları ve Dezavantajları](#avantajları-ve-dezavantajları)
6. [Kullanım Alanları](#kullanım-alanları)
7. [Geliştirmeler ve Varyasyonlar](#geliştirmeler-ve-varyasyonlar)
8. [Diğer Sıralama Algoritmaları ile Karşılaştırma](#diğer-sıralama-algoritmaları-ile-karşılaştırma)

## Giriş

Insertion Sort (Eklemeli Sıralama), basit ve etkili bir sıralama algoritmasıdır. Günlük hayatta kart oyunlarında elimizdeki kartları sıralarken kullandığımız yönteme benzer bir mantık kullanır. Bu algoritmada, dizinin sıralanmış ve sıralanmamış kısımları vardır. Başlangıçta sıralanmış kısım sadece ilk elemandan oluşur ve her adımda sıralanmamış kısımdan bir eleman alınarak sıralanmış kısımda doğru pozisyona yerleştirilir.

## Algoritmanın Çalışma Prensibi

Insertion Sort algoritması aşağıdaki adımları izler:

1. Dizinin ilk elemanının zaten sıralanmış olduğu varsayılır.
2. Dizinin ikinci elemanı alınır ve sıralanmış kısımda doğru konuma yerleştirilir.
3. Üçüncü eleman alınır ve sıralanmış kısımda doğru konuma yerleştirilir.
4. Bu işlem, dizinin tüm elemanları sıralanana kadar devam eder.

Bir elemanı sıralanmış kısımda doğru konuma yerleştirirken, eleman sıralanmış kısmın sonundan başlanarak karşılaştırılır ve gerekirse elemanlar sağa kaydırılır.

### Görsel Örnek

Aşağıda bir diziyi Insertion Sort ile sıralama adımları gösterilmiştir:

```
Başlangıç dizisi: [5, 2, 4, 6, 1, 3]

1. Adım: [5, 2, 4, 6, 1, 3] -> [2, 5, 4, 6, 1, 3]
   (2, sıralanmış kısımda doğru yerine yerleştirildi)

2. Adım: [2, 5, 4, 6, 1, 3] -> [2, 4, 5, 6, 1, 3]
   (4, sıralanmış kısımda doğru yerine yerleştirildi)

3. Adım: [2, 4, 5, 6, 1, 3] -> [2, 4, 5, 6, 1, 3]
   (6 zaten doğru yerde olduğu için değişiklik yok)

4. Adım: [2, 4, 5, 6, 1, 3] -> [1, 2, 4, 5, 6, 3]
   (1, sıralanmış kısımda en başa yerleştirildi)

5. Adım: [1, 2, 4, 5, 6, 3] -> [1, 2, 3, 4, 5, 6]
   (3, sıralanmış kısımda doğru yerine yerleştirildi)
```

Sonuç olarak, dizi tamamen sıralanmış olur: [1, 2, 3, 4, 5, 6]

## C# Implementasyonu

Aşağıda C# dilinde Insertion Sort algoritmasının implementasyonu verilmiştir:

```csharp
using System;

class InsertionSort
{
    // Int dizileri için Insertion Sort metodu
    public static void Sort(int[] arr)
    {
        int n = arr.Length;
        for (int i = 1; i < n; i++)
        {
            int key = arr[i];
            int j = i - 1;
            
            // key'den büyük olan elemanları bir pozisyon sağa kaydır
            while (j >= 0 && arr[j] > key)
            {
                arr[j + 1] = arr[j];
                j = j - 1;
            }
            
            // key'i doğru pozisyona yerleştir
            arr[j + 1] = key;
        }
    }

    // Generic tip için Insertion Sort metodu
    public static void Sort<T>(T[] arr) where T : IComparable<T>
    {
        int n = arr.Length;
        for (int i = 1; i < n; i++)
        {
            T key = arr[i];
            int j = i - 1;
            
            // key'den büyük olan elemanları bir pozisyon sağa kaydır
            while (j >= 0 && arr[j].CompareTo(key) > 0)
            {
                arr[j + 1] = arr[j];
                j = j - 1;
            }
            
            // key'i doğru pozisyona yerleştir
            arr[j + 1] = key;
        }
    }
    
    // Diziyi ekrana yazdıran yardımcı metod
    public static void PrintArray<T>(T[] arr)
    {
        foreach (var item in arr)
        {
            Console.Write(item + " ");
        }
        Console.WriteLine();
    }
    
    // Kullanım örneği
    public static void Main()
    {
        // Int dizisi örneği
        int[] arr = { 12, 11, 13, 5, 6 };
        Console.WriteLine("Sıralanmamış dizi:");
        PrintArray(arr);
        
        Sort(arr);
        Console.WriteLine("Sıralanmış dizi:");
        PrintArray(arr);
        
        // String dizisi örneği (generic metod kullanarak)
        string[] names = { "Zeynep", "Ali", "Mehmet", "Ayşe", "Can" };
        Console.WriteLine("\nSıralanmamış isimler:");
        PrintArray(names);
        
        Sort(names);
        Console.WriteLine("Sıralanmış isimler:");
        PrintArray(names);
    }
}
```

## Zaman ve Yer Karmaşıklığı

### Zaman Karmaşıklığı
- **En İyi Durum**: O(n) - Dizi zaten sıralı olduğunda
- **Ortalama Durum**: O(n²)
- **En Kötü Durum**: O(n²) - Dizi ters sıralı olduğunda

### Yer Karmaşıklığı
- **Toplam**: O(1) - Ekstra yer kullanmadan yerinde (in-place) sıralama yapar

## Avantajları ve Dezavantajları

### Avantajları
1. **Basit Yapı**: Anlaşılması ve uygulanması kolaydır.
2. **Yerinde Sıralama**: Ekstra bellek alanı gerektirmez.
3. **Stabil Sıralama**: Aynı değere sahip elemanların sırası değişmez.
4. **Küçük Dizilerde Verimli**: Küçük veri kümeleri için hızlı çalışır.
5. **Online Algoritma**: Veri parça parça geldikçe sıralamaya devam edebilir.

### Dezavantajları
1. **Yavaş Performans**: Büyük veri kümeleri için O(n²) karmaşıklığı nedeniyle yavaş çalışır.
2. **Ölçeklenebilirlik**: Büyük veri kümeleri için uygun değildir.

## Kullanım Alanları

Insertion Sort şu durumlarda tercih edilebilir:

1. **Küçük Veri Kümeleri**: Az sayıda eleman içeren dizilerin sıralanmasında
2. **Neredeyse Sıralı Diziler**: Büyük ölçüde sıralı olan dizilerde çok hızlı çalışır
3. **Sınırlı Bellek**: Bellek kısıtlaması olan ortamlarda (yerinde sıralama özelliği nedeniyle)
4. **Online Sıralama**: Veriler parça parça geliyorsa ve her yeni veri geldiğinde sıralama yapılması gerekiyorsa
5. **Diğer Algoritmaların Parçası Olarak**: Hybrid sıralama algoritmalarında küçük alt dizileri sıralamak için kullanılabilir

## Geliştirmeler ve Varyasyonlar

### Binary Insertion Sort
Sıralanmış kısımda doğru konumu bulmak için ikili arama (binary search) kullanır. Bu, karşılaştırma sayısını azaltır ancak kaydırma işlemleri aynı kalır.

```csharp
public static void BinaryInsertionSort(int[] arr)
{
    int n = arr.Length;
    for (int i = 1; i < n; i++)
    {
        int key = arr[i];
        int insertPos = BinarySearch(arr, 0, i - 1, key);
        
        // Elemanları sağa kaydır
        for (int j = i - 1; j >= insertPos; j--)
        {
            arr[j + 1] = arr[j];
        }
        
        // Key'i doğru pozisyona yerleştir
        arr[insertPos] = key;
    }
}

// İkili arama ile eklenecek pozisyonu bul
private static int BinarySearch(int[] arr, int left, int right, int key)
{
    if (right <= left)
        return (key > arr[left]) ? (left + 1) : left;
    
    int mid = (left + right) / 2;
    
    if (key == arr[mid])
        return mid + 1;
    
    if (key > arr[mid])
        return BinarySearch(arr, mid + 1, right, key);
    
    return BinarySearch(arr, left, mid - 1, key);
}
```

### Shell Sort
Insertion Sort'un bir varyasyonu olan Shell Sort, dizideki elemanları belirli aralıklarla karşılaştırarak performansı iyileştirir.

## Diğer Sıralama Algoritmaları ile Karşılaştırma

| Algoritma | En İyi Durum | Ortalama Durum | En Kötü Durum | Yer Karmaşıklığı | Stabil mi? |
|-----------|-------------|---------------|--------------|----------------|-----------|
| Insertion Sort | O(n) | O(n²) | O(n²) | O(1) | Evet |
| Bubble Sort | O(n) | O(n²) | O(n²) | O(1) | Evet |
| Selection Sort | O(n²) | O(n²) | O(n²) | O(1) | Hayır |
| Merge Sort | O(n log n) | O(n log n) | O(n log n) | O(n) | Evet |
| Quick Sort | O(n log n) | O(n log n) | O(n²) | O(log n) | Hayır |
| Heap Sort | O(n log n) | O(n log n) | O(n log n) | O(1) | Hayır |

## Sonuç

Insertion Sort, basit ve etkili bir sıralama algoritmasıdır. Küçük veri kümeleri ve neredeyse sıralı diziler için iyi performans gösterir. Ancak büyük veri kümeleri için O(n²) zaman karmaşıklığından dolayı verimli değildir. Bu durumlar için Merge Sort, Quick Sort veya Heap Sort gibi daha etkili algoritmalar tercih edilmelidir.

Insertion Sort'un basitliği ve yerinde sıralama yapabilmesi, belirli kullanım senaryolarında hala tercih edilmesini sağlamaktadır. Özellikle küçük dizilerin sıralanması veya hibrit sıralama algoritmalarının bir parçası olarak kullanılması yaygındır.
