# C# Bubble Sort Algoritması Detaylı Rehberi

## İçindekiler
1. [Bubble Sort Nedir?](#bubble-sort-nedir)
2. [Algoritmanın Çalışma Prensibi](#algoritmanın-çalışma-prensibi)
3. [C# ile Bubble Sort Uygulaması](#c-ile-bubble-sort-uygulaması)
4. [Algoritma Analizi](#algoritma-analizi)
5. [Algoritmanın Optimizasyonu](#algoritmanın-optimizasyonu)
6. [Karşılaştırmalı Analiz ve Kullanım Önerileri](#karşılaştırmalı-analiz-ve-kullanım-önerileri)
7. [Pratik Örnek ve Uygulamalar](#pratik-örnek-ve-uygulamalar)

## Bubble Sort Nedir?

Bubble Sort (Kabarcık Sıralama), en basit sıralama algoritmalarından biridir. Adını, dizi içinde büyük elemanların bir kabarcık gibi yükselerek sona doğru "yüzmesi" benzetmesinden alır. Algoritma, komşu elemanları karşılaştırarak ve gerektiğinde yer değiştirerek çalışır.

Bubble Sort algoritması öğrenilmesi kolay olmasıyla bilinir ve genellikle sıralama algoritmaları öğrenirken ilk öğretilen algoritma olarak karşımıza çıkar.

## Algoritmanın Çalışma Prensibi

Algoritma şu adımları izler:

1. Dizinin başından başlayarak, her bir elemanı komşusuyla karşılaştırır
2. Eğer elemanlar yanlış sıradaysa (örneğin artan sıralama için, sol eleman sağ elemandan büyükse), yerlerini değiştirir
3. Diziyi baştan sona tarar ve her tarama sonunda en büyük eleman dizinin sonuna yerleşir
4. Bu işlem, dizideki tüm elemanlar sıralanana kadar (n-1) kez tekrarlanır

Şematik gösterim:

```
İlk Durum: [5, 1, 4, 2, 8]

1. Geçiş:
   [5, 1, 4, 2, 8] → [1, 5, 4, 2, 8] (5 > 1 olduğu için yer değiştirme yapıldı)
   [1, 5, 4, 2, 8] → [1, 4, 5, 2, 8] (5 > 4 olduğu için yer değiştirme yapıldı)
   [1, 4, 5, 2, 8] → [1, 4, 2, 5, 8] (5 > 2 olduğu için yer değiştirme yapıldı)
   [1, 4, 2, 5, 8] → [1, 4, 2, 5, 8] (5 < 8 olduğu için yer değiştirme yapılmadı)

2. Geçiş:
   [1, 4, 2, 5, 8] → [1, 4, 2, 5, 8] (1 < 4 olduğu için yer değiştirme yapılmadı)
   [1, 4, 2, 5, 8] → [1, 2, 4, 5, 8] (4 > 2 olduğu için yer değiştirme yapıldı)
   [1, 2, 4, 5, 8] → [1, 2, 4, 5, 8] (4 < 5 olduğu için yer değiştirme yapılmadı)
   [1, 2, 4, 5, 8] → [1, 2, 4, 5, 8] (5 < 8 olduğu için yer değiştirme yapılmadı)

3. Geçiş: Hiçbir yer değiştirme yapılmadı, dizi sıralandı.
```

## C# ile Bubble Sort Uygulaması

Aşağıda C# dilinde Bubble Sort algoritmasının temel bir uygulaması verilmiştir:

```csharp
using System;

class BubbleSort
{
    // Temel Bubble Sort algoritması
    public static void Sort(int[] arr)
    {
        int n = arr.Length;
        
        for (int i = 0; i < n - 1; i++)
        {
            for (int j = 0; j < n - i - 1; j++)
            {
                // Komşu elemanları karşılaştır
                if (arr[j] > arr[j + 1])
                {
                    // Elemanların yerini değiştir
                    int temp = arr[j];
                    arr[j] = arr[j + 1];
                    arr[j + 1] = temp;
                }
            }
        }
    }

    // Diziyi ekrana yazdıran yardımcı metod
    public static void PrintArray(int[] arr)
    {
        foreach (var item in arr)
        {
            Console.Write(item + " ");
        }
        Console.WriteLine();
    }

    // Test için Main metodu
    static void Main(string[] args)
    {
        int[] arr = { 64, 34, 25, 12, 22, 11, 90 };
        
        Console.WriteLine("Sıralanmamış dizi:");
        PrintArray(arr);
        
        Sort(arr);
        
        Console.WriteLine("Sıralanmış dizi:");
        PrintArray(arr);
    }
}
```

## Algoritma Analizi

Bubble Sort algoritmasının performans analizi:

- **Zaman Karmaşıklığı**:
  - En İyi Durum: O(n) - Dizi zaten sıralı ise ve optimizasyon yapılmışsa
  - Ortalama Durum: O(n²)
  - En Kötü Durum: O(n²) - Dizi ters sıralı ise

- **Alan Karmaşıklığı**: O(1) - Sadece birkaç değişken kullanır, ekstra bellek alanı gerektirmez

- **Kararlılık (Stability)**: Kararlı bir algoritmadır. Aynı değere sahip elemanların göreceli sırası korunur.

## Algoritmanın Optimizasyonu

Temel Bubble Sort algoritması bazı durumlarda verimsiz olabilir. Aşağıdaki optimizasyonlar performansı artırabilir:

### 1. Erken Çıkış (Early Exit)

Eğer bir geçişte hiçbir yer değiştirme yapılmamışsa, dizi sıralanmış demektir ve algoritma sonlandırılabilir:

```csharp
public static void OptimizedSort(int[] arr)
{
    int n = arr.Length;
    bool swapped;
    
    for (int i = 0; i < n - 1; i++)
    {
        swapped = false;
        
        for (int j = 0; j < n - i - 1; j++)
        {
            if (arr[j] > arr[j + 1])
            {
                // Elemanların yerini değiştir
                int temp = arr[j];
                arr[j] = arr[j + 1];
                arr[j + 1] = temp;
                swapped = true;
            }
        }
        
        // Eğer iç döngüde hiç yer değiştirme yapılmadıysa, dizi sıralıdır
        if (!swapped)
            break;
    }
}
```

### 2. Sınır Optimizasyonu

Her geçişte, sıralamaya dahil edilecek elemanların sınırını güncelleme:

```csharp
public static void OptimizedSortWithBoundary(int[] arr)
{
    int n = arr.Length;
    int newn;
    
    do
    {
        newn = 0;
        
        for (int i = 1; i < n; i++)
        {
            if (arr[i - 1] > arr[i])
            {
                // Elemanların yerini değiştir
                int temp = arr[i - 1];
                arr[i - 1] = arr[i];
                arr[i] = temp;
                newn = i;
            }
        }
        
        n = newn;
    }
    while (newn != 0);
}
```

## Karşılaştırmalı Analiz ve Kullanım Önerileri

Bubble Sort'un diğer sıralama algoritmalarıyla karşılaştırılması:

| Algoritma | En İyi Durum | Ortalama Durum | En Kötü Durum | Alan Kullanımı | Kararlılık |
|-----------|--------------|----------------|---------------|----------------|------------|
| Bubble Sort | O(n) | O(n²) | O(n²) | O(1) | Evet |
| Selection Sort | O(n²) | O(n²) | O(n²) | O(1) | Hayır |
| Insertion Sort | O(n) | O(n²) | O(n²) | O(1) | Evet |
| Merge Sort | O(n log n) | O(n log n) | O(n log n) | O(n) | Evet |
| Quick Sort | O(n log n) | O(n log n) | O(n²) | O(log n) | Hayır |

**Bubble Sort Kullanım Önerileri**:

- ✅ **Kullanım için uygun olduğu durumlar**:
  - Dizi boyutu küçükse (genellikle <1000 eleman)
  - Dizi büyük oranda sıralıysa (optimizasyon yapılmış halde)
  - Öğretim amaçlı veya algoritma prensiplerini anlamak için
  - Bellek kullanımı kısıtlıysa (O(1) alan karmaşıklığı)

- ❌ **Kullanım için uygun olmadığı durumlar**:
  - Büyük veri kümeleri
  - Performans kritik uygulamalar
  - Gerçek zamanlı sistemler
  - Yüksek verimlilik gerektiren uygulamalarda

## Pratik Örnek ve Uygulamalar

### 1. Generic (Jenerik) Bubble Sort Uygulaması

Farklı veri tipleriyle çalışabilen jenerik bir uygulama:

```csharp
using System;
using System.Collections.Generic;

class GenericBubbleSort
{
    public static void Sort<T>(T[] arr) where T : IComparable<T>
    {
        int n = arr.Length;
        bool swapped;
        
        for (int i = 0; i < n - 1; i++)
        {
            swapped = false;
            
            for (int j = 0; j < n - i - 1; j++)
            {
                // IComparable arayüzü kullanarak karşılaştırma
                if (arr[j].CompareTo(arr[j + 1]) > 0)
                {
                    // Elemanların yerini değiştir
                    T temp = arr[j];
                    arr[j] = arr[j + 1];
                    arr[j + 1] = temp;
                    swapped = true;
                }
            }
            
            if (!swapped)
                break;
        }
    }
    
    static void Main(string[] args)
    {
        // Int dizisi örneği
        int[] intArray = { 64, 34, 25, 12, 22, 11, 90 };
        Sort(intArray);
        Console.WriteLine("Sıralanmış int dizisi:");
        foreach (var item in intArray)
            Console.Write(item + " ");
        Console.WriteLine();
        
        // String dizisi örneği
        string[] stringArray = { "Zebra", "Apple", "Banana", "Orange", "Kiwi" };
        Sort(stringArray);
        Console.WriteLine("Sıralanmış string dizisi:");
        foreach (var item in stringArray)
            Console.Write(item + " ");
        Console.WriteLine();
    }
}
```

### 2. Adımları Görselleştiren Bubble Sort

Algoritmanın adımlarını görselleştiren bir uygulama:

```csharp
using System;
using System.Threading;

class VisualBubbleSort
{
    public static void SortWithVisualization(int[] arr)
    {
        int n = arr.Length;
        
        for (int i = 0; i < n - 1; i++)
        {
            Console.WriteLine($"Geçiş #{i + 1}:");
            
            for (int j = 0; j < n - i - 1; j++)
            {
                // Mevcut durumu göster ve karşılaştırılan elemanları vurgula
                PrintArrayWithHighlight(arr, j, j + 1);
                
                if (arr[j] > arr[j + 1])
                {
                    // Elemanların yerini değiştir
                    int temp = arr[j];
                    arr[j] = arr[j + 1];
                    arr[j + 1] = temp;
                    
                    // Yer değiştirme sonrası durumu göster
                    Console.WriteLine("Elemanlar yer değiştirdi:");
                    PrintArrayWithHighlight(arr, j, j + 1);
                }
                else
                {
                    Console.WriteLine("Yer değiştirme gerekmedi.");
                }
                
                Thread.Sleep(500); // Görselleştirme için bekle
                Console.WriteLine();
            }
            
            Console.WriteLine($"Geçiş #{i + 1} sonunda en büyük {i + 1} eleman doğru konuma yerleşti.\n");
        }
        
        Console.WriteLine("Sıralama tamamlandı!");
    }
    
    private static void PrintArrayWithHighlight(int[] arr, int pos1, int pos2)
    {
        for (int i = 0; i < arr.Length; i++)
        {
            if (i == pos1 || i == pos2)
                Console.Write("[" + arr[i] + "] "); // Karşılaştırılan elemanları vurgula
            else
                Console.Write(arr[i] + " ");
        }
        Console.WriteLine();
    }
    
    static void Main(string[] args)
    {
        int[] arr = { 64, 34, 25, 12, 22, 11, 90 };
        
        Console.WriteLine("Orijinal dizi:");
        foreach (var item in arr)
            Console.Write(item + " ");
        Console.WriteLine("\n");
        
        SortWithVisualization(arr);
    }
}
```

### 3. Custom Comparer ile Bubble Sort

Özel karşılaştırıcı kullanarak sıralama:

```csharp
using System;
using System.Collections.Generic;

class CustomBubbleSort
{
    // Custom comparer ile bubble sort
    public static void Sort<T>(T[] arr, IComparer<T> comparer)
    {
        int n = arr.Length;
        bool swapped;
        
        for (int i = 0; i < n - 1; i++)
        {
            swapped = false;
            
            for (int j = 0; j < n - i - 1; j++)
            {
                // Custom comparer kullanarak karşılaştırma
                if (comparer.Compare(arr[j], arr[j + 1]) > 0)
                {
                    // Elemanların yerini değiştir
                    T temp = arr[j];
                    arr[j] = arr[j + 1];
                    arr[j + 1] = temp;
                    swapped = true;
                }
            }
            
            if (!swapped)
                break;
        }
    }
    
    // Örnek bir custom comparer - uzunluğa göre string sıralama
    public class StringLengthComparer : IComparer<string>
    {
        public int Compare(string x, string y)
        {
            return x.Length.CompareTo(y.Length);
        }
    }
    
    static void Main(string[] args)
    {
        string[] words = { "apple", "pear", "banana", "kiwi", "strawberry", "fig" };
        
        Console.WriteLine("Orijinal kelimeler:");
        foreach (var word in words)
            Console.Write(word + " ");
        Console.WriteLine();
        
        // Alfabetik sıralama (varsayılan string karşılaştırma)
        Sort(words, Comparer<string>.Default);
        Console.WriteLine("\nAlfabetik sıralanmış:");
        foreach (var word in words)
            Console.Write(word + " ");
        Console.WriteLine();
        
        // Uzunluğa göre sıralama (özel karşılaştırıcı)
        Sort(words, new StringLengthComparer());
        Console.WriteLine("\nUzunluğa göre sıralanmış:");
        foreach (var word in words)
            Console.Write(word + " ");
        Console.WriteLine();
    }
}
```

Bubble Sort, her ne kadar performans açısından diğer algoritmaların gerisinde kalsa da, basitliği ve kolay anlaşılabilirliği sayesinde öğretici bir algoritma olarak değerini korur. Daha verimli algoritmalar (Quick Sort, Merge Sort, vb.) genellikle gerçek uygulamalarda tercih edilse de, Bubble Sort algoritma prensiplerini anlamak için iyi bir başlangıç noktasıdır.
