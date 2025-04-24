# Merge Sort Algoritması ve C# Uygulaması

## İçindekiler
1. [Merge Sort Nedir?](#merge-sort-nedir)
2. [Algoritmanın Çalışma Prensibi](#algoritmanın-çalışma-prensibi)
3. [Algoritmanın Zaman ve Alan Karmaşıklığı](#algoritmanın-zaman-ve-alan-karmaşıklığı)
4. [C# İmplementasyonu](#c-implementasyonu)
5. [Adım Adım Örnek](#adım-adım-örnek)
6. [Avantajlar ve Dezavantajlar](#avantajlar-ve-dezavantajlar)
7. [Diğer Sıralama Algoritmaları ile Karşılaştırma](#diğer-sıralama-algoritmaları-ile-karşılaştırma)
8. [Gerçek Dünya Uygulamaları](#gerçek-dünya-uygulamaları)

## Merge Sort Nedir?

Merge Sort, 1945 yılında John von Neumann tarafından icat edilen karşılaştırma tabanlı, stabil bir sıralama algoritmasıdır. Bu algoritma "böl ve yönet" (divide and conquer) paradigmasını kullanır - büyük bir problemi daha küçük alt problemlere böler, her bir alt problemi çözer ve sonra sonuçları birleştirir.

## Algoritmanın Çalışma Prensibi

Merge Sort'un çalışma mantığı şu adımlara dayanır:

1. **Bölme (Divide)**: Sıralanacak dizi ortadan ikiye bölünür.
2. **Fethetme (Conquer)**: Her iki alt dizi özyinelemeli (recursive) olarak Merge Sort ile sıralanır.
3. **Birleştirme (Merge)**: Sıralanmış iki alt dizi, sıralı bir dizi oluşturmak üzere birleştirilir.

Temel durumda (alt dizinin uzunluğu 1 veya 0 olduğunda), alt dizi zaten sıralı kabul edilir.

## Algoritmanın Zaman ve Alan Karmaşıklığı

### Zaman Karmaşıklığı:
- **En İyi Durum**: O(n log n)
- **Ortalama Durum**: O(n log n)
- **En Kötü Durum**: O(n log n)

Merge Sort'un en büyük avantajlarından biri, performansının giriş verisinin düzeninden bağımsız olmasıdır. Dizinin başlangıçtaki durumu ne olursa olsun, algoritma her zaman aynı zaman karmaşıklığında çalışır.

### Alan Karmaşıklığı:
- **En Kötü Durum**: O(n)

Merge Sort, sıralama işlemi sırasında ekstra bellek alanı gerektirir. Bu, büyük veri setleri üzerinde çalışırken bir dezavantaj olabilir.

## C# İmplementasyonu

Aşağıda, Merge Sort algoritmasının C# dilinde bir implementasyonu bulunmaktadır:

```csharp
using System;

public class MergeSort
{
    // Ana Merge Sort fonksiyonu
    public static void Sort(int[] arr)
    {
        if (arr == null || arr.Length <= 1)
            return;
            
        int[] temp = new int[arr.Length];
        MergeSortInternal(arr, temp, 0, arr.Length - 1);
    }
    
    // Özyinelemeli Merge Sort fonksiyonu
    private static void MergeSortInternal(int[] arr, int[] temp, int left, int right)
    {
        if (left < right)
        {
            // Ortayı bul
            int middle = left + (right - left) / 2;
            
            // Sol ve sağ alt dizileri özyinelemeli olarak sırala
            MergeSortInternal(arr, temp, left, middle);
            MergeSortInternal(arr, temp, middle + 1, right);
            
            // Sıralanmış alt dizileri birleştir
            Merge(arr, temp, left, middle, right);
        }
    }
    
    // İki sıralı alt diziyi birleştiren fonksiyon
    private static void Merge(int[] arr, int[] temp, int left, int middle, int right)
    {
        // Sol ve sağ alt dizilerin sınırlarını belirle
        int leftStart = left;
        int leftEnd = middle;
        int rightStart = middle + 1;
        int rightEnd = right;
        
        // Birleştirme işlemi için geçici dizinin indeksi
        int tempIndex = left;
        
        // İki alt diziyi karşılaştır ve geçici diziye birleştir
        while (leftStart <= leftEnd && rightStart <= rightEnd)
        {
            if (arr[leftStart] <= arr[rightStart])
            {
                temp[tempIndex] = arr[leftStart];
                leftStart++;
            }
            else
            {
                temp[tempIndex] = arr[rightStart];
                rightStart++;
            }
            tempIndex++;
        }
        
        // Sol alt dizide kalan elemanları ekle
        while (leftStart <= leftEnd)
        {
            temp[tempIndex] = arr[leftStart];
            leftStart++;
            tempIndex++;
        }
        
        // Sağ alt dizide kalan elemanları ekle
        while (rightStart <= rightEnd)
        {
            temp[tempIndex] = arr[rightStart];
            rightStart++;
            tempIndex++;
        }
        
        // Geçici dizideki birleştirilmiş elemanları orijinal diziye kopyala
        for (int i = left; i <= right; i++)
        {
            arr[i] = temp[i];
        }
    }
    
    // Diziyi ekrana yazdırmak için yardımcı fonksiyon
    public static void PrintArray(int[] arr)
    {
        foreach (int num in arr)
        {
            Console.Write(num + " ");
        }
        Console.WriteLine();
    }
    
    // Örnek kullanım
    public static void Main(string[] args)
    {
        int[] arr = { 12, 11, 13, 5, 6, 7 };
        Console.WriteLine("Sıralanmamış dizi:");
        PrintArray(arr);
        
        Sort(arr);
        
        Console.WriteLine("Sıralanmış dizi:");
        PrintArray(arr);
    }
}
```

Bu implementasyonda, ana `Sort` fonksiyonu çağrıldığında, algoritma dizinin tam bir kopyasını oluşturmak için geçici bir dizi oluşturur ve ardından özyinelemeli olarak diziyi sıralamak için `MergeSortInternal` fonksiyonunu çağırır. `Merge` fonksiyonu, sıralanmış iki alt diziyi birleştirerek sıralı bir dizi oluşturur.

## Adım Adım Örnek

Örnek bir diziyi sıralamak için Merge Sort algoritmasının nasıl çalıştığını adım adım inceleyelim:

Dizi: [38, 27, 43, 3, 9, 82, 10]

1. **Bölme Aşaması**:
   - Dizi ikiye bölünür: [38, 27, 43, 3] ve [9, 82, 10]
   - [38, 27, 43, 3] ikiye bölünür: [38, 27] ve [43, 3]
   - [38, 27] ikiye bölünür: [38] ve [27]
   - [38] ve [27] tek elemanlı, sıralı kabul edilir.
   - [43, 3] ikiye bölünür: [43] ve [3]
   - [43] ve [3] tek elemanlı, sıralı kabul edilir.
   - [9, 82, 10] ikiye bölünür: [9] ve [82, 10]
   - [9] tek elemanlı, sıralı kabul edilir.
   - [82, 10] ikiye bölünür: [82] ve [10]
   - [82] ve [10] tek elemanlı, sıralı kabul edilir.

2. **Birleştirme Aşaması**:
   - [38] ve [27] birleştirilir: [27, 38]
   - [43] ve [3] birleştirilir: [3, 43]
   - [27, 38] ve [3, 43] birleştirilir: [3, 27, 38, 43]
   - [9] ve [10, 82] birleştirilir: [9, 10, 82]
   - [3, 27, 38, 43] ve [9, 10, 82] birleştirilir: [3, 9, 10, 27, 38, 43, 82]

Son Sonuç: [3, 9, 10, 27, 38, 43, 82]

## Avantajlar ve Dezavantajlar

### Avantajlar:
1. **Kararlı Performans**: Merge Sort, her durumda O(n log n) zaman karmaşıklığı ile çalışır.
2. **Stabilite**: Eşit değerlere sahip elemanların göreli sırası değişmez.
3. **Paralel İşleme**: Algoritma, çoklu işlemci ortamlarında paralelleştirilebilir.
4. **Büyük Veri Setleri**: Dış bellek sıralaması için iyi bir seçimdir.

### Dezavantajlar:
1. **Ekstra Bellek Kullanımı**: O(n) ekstra alan gerektirir.
2. **Küçük Diziler**: Küçük dizilerde, ek maliyet nedeniyle Insertion Sort gibi daha basit algoritmalar daha verimli olabilir.
3. **Cache Performansı**: Bellek erişim modeli, bazı durumlarda cache performansını olumsuz etkileyebilir.

## Diğer Sıralama Algoritmaları ile Karşılaştırma

| Algoritma     | En İyi Durum | Ortalama Durum | En Kötü Durum | Alan Karmaşıklığı | Stabilite |
|---------------|--------------|----------------|---------------|-------------------|-----------|
| Merge Sort    | O(n log n)   | O(n log n)     | O(n log n)    | O(n)              | Evet      |
| Quick Sort    | O(n log n)   | O(n log n)     | O(n²)         | O(log n)          | Hayır     |
| Heap Sort     | O(n log n)   | O(n log n)     | O(n log n)    | O(1)              | Hayır     |
| Insertion Sort| O(n)         | O(n²)          | O(n²)         | O(1)              | Evet      |
| Bubble Sort   | O(n)         | O(n²)          | O(n²)         | O(1)              | Evet      |

## Gerçek Dünya Uygulamaları

Merge Sort, birçok gerçek dünya uygulamasında kullanılır:

1. **Veritabanı Sistemleri**: Büyük veri setlerini sıralamak için.
2. **Dosya Sistemleri**: Dış bellek sıralama işlemleri için.
3. **Paralel Sistemler**: İş yükünü eşit olarak dağıtmak için.
4. **Tersine Çift Sayımı**: Bir dizideki tersine çiftleri saymak için.
5. **Doğal Dil İşleme**: Metin parçalarını birleştirmek ve sıralamak için.

Merge Sort, özellikle büyük veri setleri ve kararlı sıralama gerektiren uygulamalarda tercih edilen bir algoritmadır. Ekstra bellek kullanımına rağmen, her durumda öngörülebilir performans sunması büyük bir avantajdır.
