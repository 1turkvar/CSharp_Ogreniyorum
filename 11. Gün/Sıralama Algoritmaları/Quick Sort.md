# C# ile Quick Sort Algoritması

## İçindekiler
1. [Quick Sort Nedir?](#quick-sort-nedir)
2. [Algoritmanın Çalışma Mantığı](#algoritmanın-çalışma-mantığı)
3. [Zaman ve Alan Karmaşıklığı](#zaman-ve-alan-karmaşıklığı)
4. [C# ile Quick Sort Implementasyonu](#c-ile-quick-sort-implementasyonu)
5. [Algoritmanın Adım Adım İzlenmesi](#algoritmanın-adım-adım-izlenmesi)
6. [Pivot Seçim Stratejileri](#pivot-seçim-stratejileri)
7. [Avantajları ve Dezavantajları](#avantajları-ve-dezavantajları)
8. [Quick Sort'u Optimize Etme Yöntemleri](#quick-sortu-optimize-etme-yöntemleri)
9. [Farklı Veri Türlerinde Quick Sort](#farklı-veri-türlerinde-quick-sort)
10. [Kaynaklar](#kaynaklar)

## Quick Sort Nedir?

Quick Sort (Hızlı Sıralama), verimli bir sıralama algoritmasıdır ve "böl ve yönet" (divide and conquer) stratejisine dayanır. Bu algoritma, Tony Hoare tarafından 1960 yılında geliştirilmiş olup, ortalama durumda O(n log n) zaman karmaşıklığına sahiptir.

## Algoritmanın Çalışma Mantığı

Quick Sort şu adımlarla çalışır:

1. Diziden bir "pivot" eleman seçilir.
2. Diğer elemanlar, pivot elemandan küçük olanlar solda, büyük olanlar sağda olacak şekilde yeniden düzenlenir (partitioning işlemi).
3. Bu işlem sonucunda pivot elemanın nihai konumu belirlenir.
4. Aynı işlem, pivotun solundaki ve sağındaki alt diziler için tekrarlanır (rekürsif olarak).
5. Alt diziler tek elemana düşene kadar rekürsif işlem devam eder.

## Zaman ve Alan Karmaşıklığı

**Zaman Karmaşıklığı:**
- En iyi durum: O(n log n)
- Ortalama durum: O(n log n)
- En kötü durum: O(n²) - Dizinin zaten sıralı olduğu veya tüm elemanların aynı olduğu durumda

**Alan Karmaşıklığı:**
- O(log n) - rekürsif çağrıların yığın belleğinde tutulmasından dolayı

## C# ile Quick Sort Implementasyonu

### Temel Quick Sort Algoritması

```csharp
using System;

class QuickSort
{
    // Diziyi sıralayan ana fonksiyon
    public static void Sort(int[] arr)
    {
        if (arr == null || arr.Length == 0)
            return;
            
        QuickSortRecursive(arr, 0, arr.Length - 1);
    }
    
    // Rekürsif Quick Sort fonksiyonu
    private static void QuickSortRecursive(int[] arr, int low, int high)
    {
        if (low < high)
        {
            // Partition işlemi ile pivot elemanın indeksini bul
            int pivotIndex = Partition(arr, low, high);
            
            // Pivot elemanının sağındaki ve solundaki alt dizileri sırala
            QuickSortRecursive(arr, low, pivotIndex - 1);
            QuickSortRecursive(arr, pivotIndex + 1, high);
        }
    }
    
    // Partition işlemi: Pivottan küçükleri sola, büyükleri sağa yerleştirir
    private static int Partition(int[] arr, int low, int high)
    {
        // Pivot olarak en sağdaki elemanı seçiyoruz
        int pivot = arr[high];
        
        // Küçük elemanların son indeksi
        int i = low - 1;
        
        // Pivot ile karşılaştırma ve yer değiştirme
        for (int j = low; j < high; j++)
        {
            if (arr[j] <= pivot)
            {
                i++;
                // Elemanları yer değiştir
                Swap(arr, i, j);
            }
        }
        
        // Pivot elemanı doğru konumuna yerleştir
        Swap(arr, i + 1, high);
        
        // Pivot elemanın yeni indeksini döndür
        return i + 1;
    }
    
    // İki elemanın yerini değiştiren yardımcı fonksiyon
    private static void Swap(int[] arr, int i, int j)
    {
        int temp = arr[i];
        arr[i] = arr[j];
        arr[j] = temp;
    }
    
    // Test için örnek kullanım
    public static void Main(string[] args)
    {
        int[] arr = { 10, 7, 8, 9, 1, 5 };
        Console.WriteLine("Sıralanmamış dizi:");
        PrintArray(arr);
        
        Sort(arr);
        
        Console.WriteLine("\nSıralanmış dizi:");
        PrintArray(arr);
    }
    
    // Diziyi yazdıran yardımcı fonksiyon
    private static void PrintArray(int[] arr)
    {
        foreach (var item in arr)
        {
            Console.Write(item + " ");
        }
        Console.WriteLine();
    }
}
```

### Generic Quick Sort Implementasyonu

```csharp
using System;
using System.Collections.Generic;

class GenericQuickSort
{
    // Generic Quick Sort fonksiyonu
    public static void Sort<T>(T[] arr) where T : IComparable<T>
    {
        if (arr == null || arr.Length == 0)
            return;
            
        QuickSortRecursive(arr, 0, arr.Length - 1);
    }
    
    // Rekürsif Quick Sort fonksiyonu
    private static void QuickSortRecursive<T>(T[] arr, int low, int high) where T : IComparable<T>
    {
        if (low < high)
        {
            int pivotIndex = Partition(arr, low, high);
            
            QuickSortRecursive(arr, low, pivotIndex - 1);
            QuickSortRecursive(arr, pivotIndex + 1, high);
        }
    }
    
    // Partition işlemi
    private static int Partition<T>(T[] arr, int low, int high) where T : IComparable<T>
    {
        T pivot = arr[high];
        int i = low - 1;
        
        for (int j = low; j < high; j++)
        {
            if (arr[j].CompareTo(pivot) <= 0)
            {
                i++;
                Swap(arr, i, j);
            }
        }
        
        Swap(arr, i + 1, high);
        return i + 1;
    }
    
    // İki elemanın yerini değiştiren yardımcı fonksiyon
    private static void Swap<T>(T[] arr, int i, int j)
    {
        T temp = arr[i];
        arr[i] = arr[j];
        arr[j] = temp;
    }
    
    // Test için örnek kullanım
    public static void Main(string[] args)
    {
        // İnteger dizisi ile test
        int[] intArr = { 10, 7, 8, 9, 1, 5 };
        Console.WriteLine("Integer dizi - Sıralanmamış:");
        PrintArray(intArr);
        
        Sort(intArr);
        
        Console.WriteLine("\nInteger dizi - Sıralanmış:");
        PrintArray(intArr);
        
        // String dizisi ile test
        string[] strArr = { "banana", "apple", "cherry", "date", "fig" };
        Console.WriteLine("\nString dizi - Sıralanmamış:");
        PrintArray(strArr);
        
        Sort(strArr);
        
        Console.WriteLine("\nString dizi - Sıralanmış:");
        PrintArray(strArr);
    }
    
    // Diziyi yazdıran yardımcı fonksiyon
    private static void PrintArray<T>(T[] arr)
    {
        foreach (var item in arr)
        {
            Console.Write(item + " ");
        }
        Console.WriteLine();
    }
}
```

## Algoritmanın Adım Adım İzlenmesi

Aşağıda, Quick Sort algoritmasının adım adım çalışmasını gösteren bir örnek bulunmaktadır. Bu örnek için `[10, 80, 30, 90, 40, 50, 70]` dizisini kullanalım:

1. İlk olarak, son eleman olan 70'i pivot olarak seçiyoruz.
2. Partition işlemi sonrası:
   - 70'den küçük elemanlar: [10, 30, 40, 50]
   - 70'den büyük elemanlar: [80, 90]
   - Dizinin yeni hali: [10, 30, 40, 50, 70, 80, 90]
   - Pivot (70) şimdi doğru konumda
3. Alt dizileri sıralama:
   - Sol alt dizi [10, 30, 40, 50] için Quick Sort uygulama
   - Sağ alt dizi [80, 90] için Quick Sort uygulama
4. Rekürsif işlemler devam ederek tüm dizi sıralanır

## Pivot Seçim Stratejileri

Quick Sort performansı, pivot seçimine büyük ölçüde bağlıdır. Bazı yaygın pivot seçim stratejileri:

1. **Son Eleman**: Dizinin son elemanını pivot olarak seçme (Yukarıdaki implementasyonda kullanıldı)
2. **İlk Eleman**: Dizinin ilk elemanını pivot olarak seçme
3. **Ortadaki Eleman**: Dizinin ortasındaki elemanı pivot olarak seçme
4. **Rastgele Eleman**: Rastgele bir elemanı pivot olarak seçme
5. **Medyan-of-Three**: İlk, orta ve son elemanın medyanını pivot olarak seçme

Aşağıda, Medyan-of-Three yöntemini kullanan bir implementasyon örneği verilmiştir:

```csharp
private static int MedianOfThreePartition(int[] arr, int low, int high)
{
    // İlk, orta ve son elemanın indeksleri
    int mid = low + (high - low) / 2;
    
    // İlk, orta ve son elemanları sırala
    if (arr[mid] < arr[low])
        Swap(arr, mid, low);
    if (arr[high] < arr[low])
        Swap(arr, high, low);
    if (arr[high] < arr[mid])
        Swap(arr, high, mid);
    
    // Ortanca değeri pivot olarak seç (şu an arr[mid]'de)
    Swap(arr, mid, high - 1);  // Pivotu high-1 konumuna taşı
    
    return Partition(arr, low, high);
}
```

## Avantajları ve Dezavantajları

### Avantajları
1. **Verimlilik**: Çoğu durumda O(n log n) zaman karmaşıklığı ile oldukça hızlıdır.
2. **Yerinde Sıralama**: Ekstra bellek gerektirmez, yerinde (in-place) çalışır.
3. **Cache Dostu**: Bellek erişimi lokaldir ve cache performansını artırır.
4. **Ortalama Performans**: Pratikte çoğu veri türü için çok iyi performans gösterir.

### Dezavantajları
1. **Kararsızlık**: Quick Sort, kararsız bir sıralama algoritmasıdır (eşit elemanların sırası değişebilir).
2. **En Kötü Durum**: En kötü durumda O(n²) karmaşıklığa sahiptir.
3. **Rekürsif Yapı**: Stack overflow hatası riski vardır (büyük dizilerde).

## Quick Sort'u Optimize Etme Yöntemleri

1. **İyi Pivot Seçimi**: Medyan-of-Three veya rastgele pivot seçimi kullanarak en kötü durum senaryosundan kaçınma.
2. **Küçük Alt Diziler İçin Insertion Sort**: Küçük alt diziler (genellikle 10-20 eleman) için Insertion Sort kullanma.
3. **Tail Rekürsiyon Eliminasyonu**: Rekürsif çağrıları azaltmak için tail rekürsiyon optimizasyonu.
4. **Iterative Implementasyon**: Rekürsif yerine iterative (yinelemeli) çözüm kullanma.

```csharp
// Küçük alt diziler için Insertion Sort kullanımı
private static void QuickSortOptimized(int[] arr, int low, int high)
{
    const int INSERTION_SORT_THRESHOLD = 10;
    
    while (low < high)
    {
        // Küçük diziler için Insertion Sort kullan
        if (high - low < INSERTION_SORT_THRESHOLD)
        {
            InsertionSort(arr, low, high);
            break;
        }
        else
        {
            int pivotIndex = Partition(arr, low, high);
            
            // Daha küçük alt diziyi rekürsif olarak sırala
            if (pivotIndex - low < high - pivotIndex)
            {
                QuickSortOptimized(arr, low, pivotIndex - 1);
                low = pivotIndex + 1;  // Tail rekürsiyon eliminasyonu
            }
            else
            {
                QuickSortOptimized(arr, pivotIndex + 1, high);
                high = pivotIndex - 1;  // Tail rekürsiyon eliminasyonu
            }
        }
    }
}

private static void InsertionSort(int[] arr, int low, int high)
{
    for (int i = low + 1; i <= high; i++)
    {
        int key = arr[i];
        int j = i - 1;
        
        while (j >= low && arr[j] > key)
        {
            arr[j + 1] = arr[j];
            j--;
        }
        
        arr[j + 1] = key;
    }
}
```

## Farklı Veri Türlerinde Quick Sort

C#'ta LINQ kullanarak Quick Sort'u farklı şekillerde uygulayabiliriz. Aşağıda, LINQ tabanlı bir Quick Sort implementasyonu verilmiştir:

```csharp
using System;
using System.Collections.Generic;
using System.Linq;

class LinqQuickSort
{
    public static IEnumerable<T> QuickSort<T>(IEnumerable<T> list) where T : IComparable<T>
    {
        if (!list.Any())
            return list;
            
        var pivot = list.First();
        var rest = list.Skip(1);
        
        var lessOrEqual = rest.Where(x => x.CompareTo(pivot) <= 0);
        var greater = rest.Where(x => x.CompareTo(pivot) > 0);
        
        return QuickSort(lessOrEqual).Append(pivot).Concat(QuickSort(greater));
    }
    
    public static void Main(string[] args)
    {
        // Integer listesi testi
        List<int> intList = new List<int> { 10, 7, 8, 9, 1, 5 };
        Console.WriteLine("Sıralanmamış liste:");
        PrintList(intList);
        
        var sortedIntList = QuickSort(intList).ToList();
        
        Console.WriteLine("\nSıralanmış liste:");
        PrintList(sortedIntList);
        
        // String listesi testi
        List<string> strList = new List<string> { "banana", "apple", "cherry", "date", "fig" };
        Console.WriteLine("\nSıralanmamış string listesi:");
        PrintList(strList);
        
        var sortedStrList = QuickSort(strList).ToList();
        
        Console.WriteLine("\nSıralanmış string listesi:");
        PrintList(sortedStrList);
    }
    
    private static void PrintList<T>(IEnumerable<T> list)
    {
        foreach (var item in list)
        {
            Console.Write(item + " ");
        }
        Console.WriteLine();
    }
}
```

## Özel Sınıfları Sıralama

C#'ta özel sınıfları sıralamak için Quick Sort'u nasıl kullanabileceğimize dair bir örnek:

```csharp
using System;
using System.Collections.Generic;

class Person : IComparable<Person>
{
    public string Name { get; set; }
    public int Age { get; set; }
    
    public Person(string name, int age)
    {
        Name = name;
        Age = age;
    }
    
    // IComparable implementasyonu
    public int CompareTo(Person other)
    {
        // Önce yaşa göre sırala
        int ageComparison = Age.CompareTo(other.Age);
        if (ageComparison != 0)
            return ageComparison;
            
        // Yaşlar eşitse, isme göre sırala
        return Name.CompareTo(other.Name);
    }
    
    public override string ToString()
    {
        return $"{Name} ({Age})";
    }
}

class CustomObjectQuickSort
{
    // Quick Sort algoritması implementasyonu
    public static void Sort<T>(T[] arr) where T : IComparable<T>
    {
        if (arr == null || arr.Length == 0)
            return;
            
        QuickSortRecursive(arr, 0, arr.Length - 1);
    }
    
    private static void QuickSortRecursive<T>(T[] arr, int low, int high) where T : IComparable<T>
    {
        if (low < high)
        {
            int pivotIndex = Partition(arr, low, high);
            QuickSortRecursive(arr, low, pivotIndex - 1);
            QuickSortRecursive(arr, pivotIndex + 1, high);
        }
    }
    
    private static int Partition<T>(T[] arr, int low, int high) where T : IComparable<T>
    {
        T pivot = arr[high];
        int i = low - 1;
        
        for (int j = low; j < high; j++)
        {
            if (arr[j].CompareTo(pivot) <= 0)
            {
                i++;
                Swap(arr, i, j);
            }
        }
        
        Swap(arr, i + 1, high);
        return i + 1;
    }
    
    private static void Swap<T>(T[] arr, int i, int j)
    {
        T temp = arr[i];
        arr[i] = arr[j];
        arr[j] = temp;
    }
    
    public static void Main(string[] args)
    {
        // Kişi nesnelerinden oluşan bir dizi oluştur
        Person[] people = {
            new Person("Ali", 30),
            new Person("Ayşe", 25),
            new Person("Mehmet", 35),
            new Person("Fatma", 25),
            new Person("Can", 30)
        };
        
        Console.WriteLine("Sıralanmamış kişiler:");
        PrintArray(people);
        
        Sort(people);
        
        Console.WriteLine("\nSıralanmış kişiler (önce yaşa, sonra isme göre):");
        PrintArray(people);
    }
    
    private static void PrintArray<T>(T[] arr)
    {
        foreach (var item in arr)
        {
            Console.WriteLine(item);
        }
    }
}
```

## Kaynaklar

1. Cormen, T. H., Leiserson, C. E., Rivest, R. L., & Stein, C. (2009). Introduction to Algorithms (3rd ed.). MIT Press.
2. Sedgewick, R., & Wayne, K. (2011). Algorithms (4th ed.). Addison-Wesley Professional.
3. Microsoft Docs: [Sorting Data](https://docs.microsoft.com/en-us/dotnet/csharp/programming-guide/concepts/linq/sorting-data)
4. GeeksforGeeks: [QuickSort Algorithm](https://www.geeksforgeeks.org/quick-sort/)
