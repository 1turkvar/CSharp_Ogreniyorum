# C# ile Selection Sort Algoritması

## İçindekiler
1. [Selection Sort Nedir?](#selection-sort-nedir)
2. [Selection Sort Algoritmasının Çalışma Prensibi](#selection-sort-algoritmasının-çalışma-prensibi)
3. [C# ile Selection Sort Uygulaması](#c-ile-selection-sort-uygulaması)
4. [Algoritmanın Adım Adım Takibi](#algoritmanın-adım-adım-takibi)
5. [Zaman ve Uzay Karmaşıklığı](#zaman-ve-uzay-karmaşıklığı)
6. [Avantajları ve Dezavantajları](#avantajları-ve-dezavantajları)
7. [Gerçek Dünya Uygulamaları](#gerçek-dünya-uygulamaları)
8. [Selection Sort vs Diğer Sıralama Algoritmaları](#selection-sort-vs-diğer-sıralama-algoritmaları)
9. [Optimizasyon İpuçları](#optimizasyon-ipuçları)
10. [Alıştırmalar](#alıştırmalar)

## Selection Sort Nedir?

Selection Sort, veri yapıları ve algoritmalar dünyasındaki en temel sıralama algoritmalarından biridir. Bu algoritma, adından da anlaşılacağı gibi, her adımda bir elemanı seçip doğru konuma yerleştirme prensibine dayanır. Özellikle küçük veri kümeleri için basit ve anlaşılır bir çözüm sunar.

## Selection Sort Algoritmasının Çalışma Prensibi

Selection Sort algoritması şu adımlarla çalışır:

1. Dizinin sıralanmamış kısmında en küçük (veya en büyük) elemanı bul
2. Bu elemanı, sıralanmamış kısmın en başındaki elemanla yer değiştir
3. Sıralanmış kısmı bir eleman artır (sıralanmamış kısmı bir eleman azalt)
4. Tüm dizi sıralanana kadar adımları tekrarla

Bir örnekle gösterelim. Dizimiz `[64, 25, 12, 22, 11]` olsun:

- İlk geçiş: En küçük eleman 11, ilk pozisyondaki 64 ile yer değiştirir. Dizi `[11, 25, 12, 22, 64]` olur.
- İkinci geçiş: Kalan kısımda (25, 12, 22, 64) en küçük eleman 12, ikinci pozisyondaki 25 ile yer değiştirir. Dizi `[11, 12, 25, 22, 64]` olur.
- Üçüncü geçiş: Kalan kısımda (25, 22, 64) en küçük eleman 22, üçüncü pozisyondaki 25 ile yer değiştirir. Dizi `[11, 12, 22, 25, 64]` olur.
- Dördüncü geçiş: Kalan kısımda (25, 64) en küçük eleman 25, dördüncü pozisyonda zaten doğru yerde. Değişiklik yok.
- Beşinci geçiş: Geriye tek eleman kaldığı için sıralama tamamlanmıştır.

Sonuç: `[11, 12, 22, 25, 64]`

## C# ile Selection Sort Uygulaması

Şimdi bu algoritmayı C# dilinde nasıl uygulayabileceğimizi görelim:

```csharp
using System;

class SelectionSort
{
    static void Sort(int[] arr)
    {
        int n = arr.Length;
        
        // Dizinin her bir elemanını dolaş
        for (int i = 0; i < n - 1; i++)
        {
            // En küçük elemanın indeksini bul
            int minIndex = i;
            for (int j = i + 1; j < n; j++)
            {
                if (arr[j] < arr[minIndex])
                {
                    minIndex = j;
                }
            }
            
            // En küçük eleman şu anki pozisyonda değilse, yer değiştir
            if (minIndex != i)
            {
                int temp = arr[i];
                arr[i] = arr[minIndex];
                arr[minIndex] = temp;
            }
        }
    }
    
    // Diziyi yazdırmak için yardımcı metod
    static void PrintArray(int[] arr)
    {
        foreach (int item in arr)
        {
            Console.Write(item + " ");
        }
        Console.WriteLine();
    }
    
    // Test etmek için Main metodu
    static void Main(string[] args)
    {
        int[] arr = { 64, 25, 12, 22, 11 };
        Console.WriteLine("Sıralanmamış dizi:");
        PrintArray(arr);
        
        Sort(arr);
        
        Console.WriteLine("Sıralanmış dizi:");
        PrintArray(arr);
    }
}
```

## Algoritmanın Adım Adım Takibi

Yukarıdaki kodu bir örnekle takip edelim. Başlangıç dizimiz `[64, 25, 12, 22, 11]` olsun:

**1. Geçiş (i = 0):**
- minIndex = 0 (başlangıçta)
- j = 1: arr[1] = 25, arr[minIndex] = 64. 25 < 64, minIndex = 1
- j = 2: arr[2] = 12, arr[minIndex] = 25. 12 < 25, minIndex = 2
- j = 3: arr[3] = 22, arr[minIndex] = 12. 22 > 12, minIndex değişmez
- j = 4: arr[4] = 11, arr[minIndex] = 12. 11 < 12, minIndex = 4
- minIndex (4) != i (0), bu yüzden arr[0] ile arr[4] yer değiştirir.
- Dizi: `[11, 25, 12, 22, 64]`

**2. Geçiş (i = 1):**
- minIndex = 1 (başlangıçta)
- j = 2: arr[2] = 12, arr[minIndex] = 25. 12 < 25, minIndex = 2
- j = 3: arr[3] = 22, arr[minIndex] = 12. 22 > 12, minIndex değişmez
- j = 4: arr[4] = 64, arr[minIndex] = 12. 64 > 12, minIndex değişmez
- minIndex (2) != i (1), bu yüzden arr[1] ile arr[2] yer değiştirir.
- Dizi: `[11, 12, 25, 22, 64]`

**3. Geçiş (i = 2):**
- minIndex = 2 (başlangıçta)
- j = 3: arr[3] = 22, arr[minIndex] = 25. 22 < 25, minIndex = 3
- j = 4: arr[4] = 64, arr[minIndex] = 22. 64 > 22, minIndex değişmez
- minIndex (3) != i (2), bu yüzden arr[2] ile arr[3] yer değiştirir.
- Dizi: `[11, 12, 22, 25, 64]`

**4. Geçiş (i = 3):**
- minIndex = 3 (başlangıçta)
- j = 4: arr[4] = 64, arr[minIndex] = 25. 64 > 25, minIndex değişmez
- minIndex (3) == i (3), bu yüzden yer değiştirme yok.
- Dizi: `[11, 12, 22, 25, 64]`

Sonuç olarak, sıralanmış dizi: `[11, 12, 22, 25, 64]`

## Zaman ve Uzay Karmaşıklığı

**Zaman Karmaşıklığı:**
- En iyi durum: O(n²)
- Ortalama durum: O(n²)
- En kötü durum: O(n²)

Selection Sort, giriş dizisinin nasıl sıralandığına bakılmaksızın her zaman O(n²) zaman karmaşıklığına sahiptir. Bunun nedeni, algoritmanın her zaman her elemanı bir kez karşılaştırmak zorunda olmasıdır.

**Uzay Karmaşıklığı:**
- O(1) - Yalnızca birkaç değişken kullanır, ekstra dizi oluşturmaz.

## Avantajları ve Dezavantajları

**Avantajları:**
1. Basit ve anlaşılır bir algoritma
2. Yazması ve uygulaması kolay
3. Takas (swap) işlemi sayısı en fazla O(n) olur (Bubble Sort'a göre daha az)
4. Küçük veri kümeleri için makul performans
5. Yerinde (in-place) sıralama yapar, ekstra bellek gerektirmez

**Dezavantajları:**
1. Büyük veri kümeleri için verimsiz (O(n²) zaman karmaşıklığı)
2. Kararlı (stable) bir sıralama algoritması değildir (aynı değere sahip öğelerin sırası değişebilir)
3. Quicksort, Mergesort veya Heapsort gibi daha gelişmiş algoritmalardan daha yavaştır

## Gerçek Dünya Uygulamaları

Selection Sort, genellikle şu durumlarda kullanılır:
- Küçük veri kümeleri için
- Bellek kısıtlamaları olan sistemlerde
- Takas (swap) işlemi maliyetinin yüksek olduğu durumlarda
- Eğitim amaçlı, algoritmaları anlamak için
- Bazı optimizasyon problemlerinde bir alt rutin olarak

## Selection Sort vs Diğer Sıralama Algoritmaları

| Algoritma | En İyi Durum | Ortalama Durum | En Kötü Durum | Kararlılık | Yerinde |
|-----------|--------------|----------------|---------------|------------|---------|
| Selection Sort | O(n²) | O(n²) | O(n²) | Hayır | Evet |
| Bubble Sort | O(n) | O(n²) | O(n²) | Evet | Evet |
| Insertion Sort | O(n) | O(n²) | O(n²) | Evet | Evet |
| Merge Sort | O(n log n) | O(n log n) | O(n log n) | Evet | Hayır |
| Quick Sort | O(n log n) | O(n log n) | O(n²) | Hayır | Evet |
| Heap Sort | O(n log n) | O(n log n) | O(n log n) | Hayır | Evet |

## Optimizasyon İpuçları

Selection Sort, temel yapısı gereği O(n²) zaman karmaşıklığından kaçamaz, ancak bazı küçük optimizasyonlar yapılabilir:

1. **Çift Yönlü Selection Sort**: Her adımda hem minimum hem de maksimum elemanı bul, bu sayede geçiş sayısını yarıya indir.

```csharp
static void OptimizedSelectionSort(int[] arr)
{
    int n = arr.Length;
    for (int i = 0, j = n - 1; i < j; i++, j--)
    {
        int min = i, max = i;
        
        // Minimum ve maksimum elemanları bul
        for (int k = i; k <= j; k++)
        {
            if (arr[k] < arr[min])
                min = k;
            if (arr[k] > arr[max])
                max = k;
        }
        
        // Minimum elemanı sol tarafa taşı
        if (min != i)
        {
            int temp = arr[i];
            arr[i] = arr[min];
            arr[min] = temp;
        }
        
        // Eğer maksimum eleman i'de ise ve i ile min yer değiştirdiyse
        // şimdi maksimum eleman min'de
        if (max == i)
            max = min;
        
        // Maksimum elemanı sağ tarafa taşı
        if (max != j)
        {
            int temp = arr[j];
            arr[j] = arr[max];
            arr[max] = temp;
        }
    }
}
```

2. **Generic Selection Sort**: Farklı veri tipleri için çalışabilecek Generic bir Selection Sort uygulaması:

```csharp
static void SelectionSort<T>(T[] arr) where T : IComparable<T>
{
    int n = arr.Length;
    for (int i = 0; i < n - 1; i++)
    {
        int minIndex = i;
        for (int j = i + 1; j < n; j++)
        {
            if (arr[j].CompareTo(arr[minIndex]) < 0)
            {
                minIndex = j;
            }
        }
        
        if (minIndex != i)
        {
            T temp = arr[i];
            arr[i] = arr[minIndex];
            arr[minIndex] = temp;
        }
    }
}
```

## Alıştırmalar

1. **Temel Uygulama**: Selection Sort algoritmasını kullanarak bir tam sayı dizisini sıralayan bir program yazın.

2. **Azalan Sıralama**: Selection Sort algoritmasını dizileri azalan sırada sıralayacak şekilde değiştirin.

3. **Karşılaştırma Sayısı**: Selection Sort'un yaptığı karşılaştırma sayısını sayan bir program yazın.

4. **Nesne Sıralama**: Öğrenci nesnelerini (isim, not, yaş gibi özelliklere sahip) farklı özelliklere göre sıralayan bir Selection Sort uygulaması yazın.

5. **Performans Karşılaştırması**: Selection Sort, Bubble Sort ve Insertion Sort algoritmalarını uygulayın ve aynı veri seti üzerinde performanslarını karşılaştırın.

```csharp
// Öğrenci sınıfı için örnek:
class Student : IComparable<Student>
{
    public string Name { get; set; }
    public int Age { get; set; }
    public double Grade { get; set; }
    
    public Student(string name, int age, double grade)
    {
        Name = name;
        Age = age;
        Grade = grade;
    }
    
    // Varsayılan olarak nota göre sıralama
    public int CompareTo(Student other)
    {
        return Grade.CompareTo(other.Grade);
    }
    
    public override string ToString()
    {
        return $"{Name}, {Age}, {Grade}";
    }
}

// Öğrencileri farklı özelliklere göre sıralamak için:
static void SortStudentsByName(Student[] students)
{
    SelectionSort(students, (s1, s2) => s1.Name.CompareTo(s2.Name));
}

static void SortStudentsByAge(Student[] students)
{
    SelectionSort(students, (s1, s2) => s1.Age.CompareTo(s2.Age));
}

static void SelectionSort<T>(T[] arr, Func<T, T, int> comparer)
{
    int n = arr.Length;
    for (int i = 0; i < n - 1; i++)
    {
        int minIndex = i;
        for (int j = i + 1; j < n; j++)
        {
            if (comparer(arr[j], arr[minIndex]) < 0)
            {
                minIndex = j;
            }
        }
        
        if (minIndex != i)
        {
            T temp = arr[i];
            arr[i] = arr[minIndex];
            arr[minIndex] = temp;
        }
    }
}
```

Selection Sort, bilgisayar bilimlerindeki temel algoritmalardan biridir. Öğrenmesi kolay olsa da, büyük veri kümeleri için çok verimli değildir. Ancak, bu algoritmanın çalışma prensiplerini anlamak, daha karmaşık sıralama algoritmalarını kavramak için önemli bir adımdır.
