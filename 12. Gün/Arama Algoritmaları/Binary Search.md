# C# Binary Search Algoritması Rehberi

## İçindekiler
1. [Binary Search Nedir?](#binary-search-nedir)
2. [Çalışma Prensibi](#çalışma-prensibi)
3. [Zaman ve Alan Karmaşıklığı](#zaman-ve-alan-karmaşıklığı)
4. [C#'ta Binary Search Implementasyonu](#cta-binary-search-implementasyonu)
   - [Özyinelemeli (Recursive) Yaklaşım](#özyinelemeli-recursive-yaklaşım)
   - [Yinelemeli (Iterative) Yaklaşım](#yinelemeli-iterative-yaklaşım)
5. [.NET Framework'teki Hazır Binary Search Methodları](#net-frameworkteki-hazır-binary-search-methodları)
   - [Array.BinarySearch](#arraybinarysearch)
   - [List<T>.BinarySearch](#listtbinarysearch)
6. [Dikkat Edilmesi Gereken Noktalar](#dikkat-edilmesi-gereken-noktalar)
7. [Örnek Uygulamalar](#örnek-uygulamalar)
8. [Performans Karşılaştırması](#performans-karşılaştırması)
9. [Sık Karşılaşılan Problemler](#sık-karşılaşılan-problemler)
10. [Sonuç](#sonuç)

## Binary Search Nedir?

Binary Search (İkili Arama), **sıralanmış** bir veri kümesinde eleman aramak için kullanılan, "böl ve fethet" (divide and conquer) paradigmasına dayanan etkili bir arama algoritmasıdır. Lineer aramada her eleman tek tek kontrol edilirken, binary search her adımda arama alanını yarıya indirir, bu da algoritmanın logaritmik bir zaman karmaşıklığına (O(log n)) sahip olmasını sağlar.

## Çalışma Prensibi

Binary Search algoritması şu adımları takip eder:

1. Dizinin ortasındaki elemanı seç
2. Aranan değeri ortadaki eleman ile karşılaştır
   - Eğer aranan değer ortadaki elemana eşitse, arama başarılı
   - Eğer aranan değer ortadaki elemandan küçükse, sol yarıda aramaya devam et
   - Eğer aranan değer ortadaki elemandan büyükse, sağ yarıda aramaya devam et
3. 2. adımı, aranan değer bulunana veya arama aralığı boş kalana kadar tekrarla

Dikkat edilmesi gereken en önemli nokta, binary search algoritmasının yalnızca **sıralı** dizilerde çalıştığıdır. Dizi sıralı değilse, önce sıralanmalıdır.

## Zaman ve Alan Karmaşıklığı

- **Zaman Karmaşıklığı**:
  - En iyi durum: O(1) - Aranan eleman ortada ise
  - Ortalama durum: O(log n)
  - En kötü durum: O(log n)

- **Alan Karmaşıklığı**:
  - Iterative implementasyonda: O(1)
  - Recursive implementasyonda: O(log n) (yığın çağrıları nedeniyle)

## C#'ta Binary Search Implementasyonu

### Özyinelemeli (Recursive) Yaklaşım

```csharp
public static int BinarySearchRecursive(int[] array, int target, int left, int right)
{
    if (left > right)
        return -1; // Eleman bulunamadı
    
    int mid = left + (right - left) / 2;
    
    // Orta eleman hedef mi?
    if (array[mid] == target)
        return mid;
    
    // Hedef ortadan küçük mü?
    if (array[mid] > target)
        return BinarySearchRecursive(array, target, left, mid - 1);
    
    // Hedef ortadan büyük
    return BinarySearchRecursive(array, target, mid + 1, right);
}

// Kullanımı:
int[] sortedArray = { 2, 5, 8, 12, 16, 23, 38, 56, 72, 91 };
int target = 23;
int index = BinarySearchRecursive(sortedArray, target, 0, sortedArray.Length - 1);
```

### Yinelemeli (Iterative) Yaklaşım

```csharp
public static int BinarySearchIterative(int[] array, int target)
{
    int left = 0;
    int right = array.Length - 1;
    
    while (left <= right)
    {
        int mid = left + (right - left) / 2;
        
        // Orta eleman hedef mi?
        if (array[mid] == target)
            return mid;
        
        // Hedef ortadan küçük mü?
        if (array[mid] > target)
            right = mid - 1;
        else
            left = mid + 1;
    }
    
    return -1; // Eleman bulunamadı
}

// Kullanımı:
int[] sortedArray = { 2, 5, 8, 12, 16, 23, 38, 56, 72, 91 };
int target = 23;
int index = BinarySearchIterative(sortedArray, target);
```

**Not**: `(left + right) / 2` yerine `left + (right - left) / 2` kullanılması, büyük sayılarla çalışırken taşma (overflow) sorununu önler.

## .NET Framework'teki Hazır Binary Search Methodları

C#, bir dizi veri yapısında binary search yapmak için built-in methodlar sunar.

### Array.BinarySearch

```csharp
public static int Main()
{
    int[] sortedArray = { 2, 5, 8, 12, 16, 23, 38, 56, 72, 91 };
    int target = 23;
    
    int index = Array.BinarySearch(sortedArray, target);
    
    if (index >= 0)
        Console.WriteLine($"Eleman bulundu, indeks: {index}");
    else
        Console.WriteLine($"Eleman bulunamadı. Olması gereken konum: {~index}");
        
    return 0;
}
```

`Array.BinarySearch` methodunun temel özellikleri:

- Eleman bulunursa, pozitif indeks döndürür
- Eleman bulunamazsa, negatif bir sayı döndürür:
  - Bu sayının bitsel tersi (~), aranan elemanın eklenmesi gereken konumu gösterir
- Aynı elemanın birden fazla kopyası varsa, bunlardan hangisinin döndürüleceği garanti edilmez

### List\<T>.BinarySearch

```csharp
public static int Main()
{
    List<int> sortedList = new List<int> { 2, 5, 8, 12, 16, 23, 38, 56, 72, 91 };
    int target = 23;
    
    int index = sortedList.BinarySearch(target);
    
    if (index >= 0)
        Console.WriteLine($"Eleman bulundu, indeks: {index}");
    else
        Console.WriteLine($"Eleman bulunamadı. Olması gereken konum: {~index}");
        
    return 0;
}
```

**Not**: Hem `Array.BinarySearch` hem de `List<T>.BinarySearch` içerisinde parametre olarak bir `IComparer` veya karşılaştırma delegesi geçebilirsiniz. Bu, özel karşılaştırma mantığı kullanarak aramayı özelleştirmenize olanak tanır.

## Genellenmiş (Generic) Binary Search İmplementasyonu

```csharp
public static int BinarySearch<T>(T[] array, T target) where T : IComparable<T>
{
    int left = 0;
    int right = array.Length - 1;
    
    while (left <= right)
    {
        int mid = left + (right - left) / 2;
        
        int comparisonResult = array[mid].CompareTo(target);
        
        if (comparisonResult == 0)
            return mid;
        else if (comparisonResult > 0)
            right = mid - 1;
        else
            left = mid + 1;
    }
    
    return -1;
}

// Kullanımı:
string[] names = { "Alice", "Bob", "Charlie", "David", "Eve", "Frank" };
Array.Sort(names); // Binary search öncesi sıralama zorunlu
int index = BinarySearch(names, "Eve");
```

## Özel Nesne Koleksiyonlarında Binary Search

```csharp
public class Person : IComparable<Person>
{
    public string Name { get; set; }
    public int Age { get; set; }
    
    public int CompareTo(Person other)
    {
        // Yaşa göre sıralama
        return this.Age.CompareTo(other.Age);
    }
    
    public override string ToString()
    {
        return $"{Name} ({Age})";
    }
}

// Kullanımı:
Person[] people = {
    new Person { Name = "Alice", Age = 25 },
    new Person { Name = "Bob", Age = 30 },
    new Person { Name = "Charlie", Age = 35 }, 
    new Person { Name = "David", Age = 40 },
    new Person { Name = "Eve", Age = 45 }
};

Person target = new Person { Age = 35 }; // İsim önemli değil, sadece yaşa göre arayacağız
int index = Array.BinarySearch(people, target);
```

## Özel Karşılaştırıcı (Comparer) ile Binary Search

```csharp
public class PersonNameComparer : IComparer<Person>
{
    public int Compare(Person x, Person y)
    {
        return string.Compare(x.Name, y.Name);
    }
}

// Kullanımı:
Person[] people = {
    new Person { Name = "Alice", Age = 25 },
    new Person { Name = "Bob", Age = 30 },
    new Person { Name = "Charlie", Age = 35 }, 
    new Person { Name = "David", Age = 40 },
    new Person { Name = "Eve", Age = 45 }
};

// İsme göre sıralama
Array.Sort(people, new PersonNameComparer());

Person target = new Person { Name = "Charlie" }; // Yaş önemli değil
int index = Array.BinarySearch(people, target, new PersonNameComparer());
```

## Dikkat Edilmesi Gereken Noktalar

1. **Dizinin Sıralı Olması**: Binary search algoritması çalışmadan önce veri yapısı **kesinlikle sıralanmış olmalıdır**. Aksi takdirde, doğru sonuç alınamaz.

2. **Taşma Sorunu**: Ortanca indeksi hesaplarken, büyük dizilerde taşma sorununu önlemek için `mid = (left + right) / 2` yerine `mid = left + (right - left) / 2` kullanın.

3. **Duplicate Elemanlar**: Dizide aynı değere sahip birden fazla eleman varsa, binary search bunlardan hangisini döndüreceğini garanti etmez.

4. **Veri Yapısı Seçimi**: Binary search için ArrayList yerine Array veya List\<T> kullanmak genellikle daha verimlidir.

5. **Bulunmayan Elemanlar**: .NET'in binary search methodları, aranan eleman bulunamazsa, negatif bir sayı döndürür. Bu sayının bitsel tersi (~), aranan elemanın eklenmesi gereken pozisyonu belirtir.

## Örnek Uygulamalar

### 1. Sıralı Bir Liste İçinde Binary Search

```csharp
using System;
using System.Collections.Generic;
using System.Diagnostics;

class Program
{
    static void Main()
    {
        // Büyük bir sıralı liste oluştur
        List<int> numbers = new List<int>();
        for (int i = 0; i < 10000000; i++)
        {
            numbers.Add(i * 2); // Çift sayılar
        }
        
        int target = 9876542; // Aramak istediğimiz sayı
        
        // Linear Search için zamanlama
        Stopwatch linearTimer = Stopwatch.StartNew();
        int linearIndex = numbers.IndexOf(target);
        linearTimer.Stop();
        
        // Binary Search için zamanlama
        Stopwatch binaryTimer = Stopwatch.StartNew();
        int binaryIndex = numbers.BinarySearch(target);
        binaryTimer.Stop();
        
        Console.WriteLine($"Hedef: {target}");
        Console.WriteLine($"Linear Search: İndeks {linearIndex}, Süre {linearTimer.ElapsedMilliseconds}ms");
        Console.WriteLine($"Binary Search: İndeks {binaryIndex}, Süre {binaryTimer.ElapsedMilliseconds}ms");
    }
}
```

### 2. Binary Search ile Örüntü Keşfetme

```csharp
using System;

class Program
{
    static void Main()
    {
        // Problem: Bir fonksiyonun verilen x değeri için ilk kez true döndürdüğü en küçük değeri bulmak
        // Örneğin, f(x) = x^2 > 1000 için en küçük x değeri
        
        // Binary search ile çözüm:
        int left = 0;
        int right = 1000;
        int result = -1;
        
        while (left <= right)
        {
            int mid = left + (right - left) / 2;
            
            if (IsConditionMet(mid))
            {
                result = mid;
                right = mid - 1; // Daha küçük bir değer var mı diye kontrol et
            }
            else
            {
                left = mid + 1;
            }
        }
        
        Console.WriteLine($"x^2 > 1000 koşulunu sağlayan en küçük x değeri: {result}");
    }
    
    static bool IsConditionMet(int x)
    {
        return x * x > 1000;
    }
}
```

### 3. Matris Üzerinde Binary Search

```csharp
using System;

class Program
{
    static void Main()
    {
        // Satır ve sütunları sıralı matris
        int[,] matrix = {
            { 1,  4,  7,  11, 15 },
            { 2,  5,  8,  12, 19 },
            { 3,  6,  9,  16, 22 },
            { 10, 13, 14, 17, 24 },
            { 18, 21, 23, 26, 30 }
        };
        
        int target = 16;
        bool found = SearchMatrix(matrix, target);
        
        Console.WriteLine($"{target} matriste {(found ? "bulundu" : "bulunamadı")}.");
    }
    
    static bool SearchMatrix(int[,] matrix, int target)
    {
        int rows = matrix.GetLength(0);
        int cols = matrix.GetLength(1);
        
        // Sağ üst köşeden başla
        int row = 0;
        int col = cols - 1;
        
        while (row < rows && col >= 0)
        {
            if (matrix[row, col] == target)
                return true;
            
            if (matrix[row, col] > target)
                col--; // Sol tarafa git
            else
                row++; // Aşağı git
        }
        
        return false;
    }
}
```

## Performans Karşılaştırması

Aşağıdaki tablo, farklı boyutlardaki sıralı dizilerde Linear Search ve Binary Search algoritmaları arasındaki performans farkını göstermektedir:

| Dizi Boyutu | Linear Search (ms) | Binary Search (ms) | Hız Farkı |
|-------------|-------------------|-------------------|-----------|
| 10          | <1                | <1                | -         |
| 100         | <1                | <1                | -         |
| 1,000       | ~1                | <1                | ~2x       |
| 10,000      | ~5                | <1                | ~10x      |
| 100,000     | ~50               | <1                | ~100x     |
| 1,000,000   | ~500              | <1                | ~1000x    |
| 10,000,000  | ~5000             | <1                | ~10000x   |

Görüldüğü gibi, veri setinin boyutu arttıkça binary search'ün performans avantajı logaritmik olarak artmaktadır.

## Sık Karşılaşılan Problemler

### 1. Sıralanmamış Dizide Binary Search Kullanımı

**Sorun**: Binary search algoritması, sıralı olmayan bir dizide kullanılırsa yanlış sonuçlar verir.

**Çözüm**: Binary search kullanmadan önce diziyi sıralayın.

```csharp
int[] unsortedArray = { 12, 4, 78, 23, 45, 1, 8 };
Array.Sort(unsortedArray); // Önce sırala
int index = Array.BinarySearch(unsortedArray, 23); // Sonra ara
```

### 2. Taşma (Overflow) Sorunu

**Sorun**: Büyük dizilerde `(left + right) / 2` işlemi taşmaya neden olabilir.

**Çözüm**: `left + (right - left) / 2` formülünü kullanın.

```csharp
// Yanlış:
int mid = (left + right) / 2;

// Doğru:
int mid = left + (right - left) / 2;
```

### 3. Duplicate Elemanlarda İlk veya Son Elemanı Bulma

**Sorun**: Standart binary search, duplicate elemanlar arasından hangisini döndüreceğini garanti etmez.

**Çözüm**: Özel bir binary search implementasyonu kullanın.

```csharp
public static int FindFirstOccurrence(int[] arr, int target)
{
    int left = 0;
    int right = arr.Length - 1;
    int result = -1;
    
    while (left <= right)
    {
        int mid = left + (right - left) / 2;
        
        if (arr[mid] == target)
        {
            result = mid;      // Geçici sonucu kaydet
            right = mid - 1;   // Sol tarafta daha başka bulunup bulunmadığını kontrol et
        }
        else if (arr[mid] > target)
        {
            right = mid - 1;
        }
        else
        {
            left = mid + 1;
        }
    }
    
    return result;
}

public static int FindLastOccurrence(int[] arr, int target)
{
    int left = 0;
    int right = arr.Length - 1;
    int result = -1;
    
    while (left <= right)
    {
        int mid = left + (right - left) / 2;
        
        if (arr[mid] == target)
        {
            result = mid;      // Geçici sonucu kaydet
            left = mid + 1;    // Sağ tarafta daha başka bulunup bulunmadığını kontrol et
        }
        else if (arr[mid] > target)
        {
            right = mid - 1;
        }
        else
        {
            left = mid + 1;
        }
    }
    
    return result;
}
```

## Sonuç

Binary Search algoritması, sıralı veri yapılarında eleman aramak için oldukça verimli bir yöntemdir. O(log n) zaman karmaşıklığı sayesinde, özellikle büyük veri setlerinde linear search'e göre çok daha hızlıdır.

C#'ta binary search'ü hem kendi yazdığınız implementasyonlarla hem de .NET Framework'ün sunduğu hazır methodlarla kullanabilirsiniz. Hangi yaklaşımı seçeceğiniz, projenizin gereksinimlerine ve özelleştirme ihtiyaçlarınıza bağlıdır.

Binary search algoritmasını etkili bir şekilde kullanmak için, veri yapınızın her zaman sıralı olduğundan emin olun ve duplicate elemanlar gibi özel durumları dikkate alın.

Unutmayın, algoritma seçimi, problem çözümünün önemli bir parçasıdır ve doğru algoritma seçimi performansı büyük ölçüde artırabilir.
