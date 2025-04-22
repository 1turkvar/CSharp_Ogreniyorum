# C# Extension Metotları

## İçindekiler
1. [Extension Metot Nedir?](#extension-metot-nedir)
2. [Extension Metot Tanımlama Kuralları](#extension-metot-tanımlama-kuralları)
3. [Basit Örnek](#basit-örnek)
4. [Extension Metot Kullanım Senaryoları](#extension-metot-kullanım-senaryoları)
5. [Extension Metot vs. Inheritance](#extension-metot-vs-inheritance)
6. [Extension Metot Kısıtlamaları](#extension-metot-kısıtlamaları)
7. [Best Practices](#best-practices)
8. [LINQ ve Extension Metotlar](#linq-ve-extension-metotlar)
9. [Generic Extension Metotlar](#generic-extension-metotlar)
10. [Interface Genişletme](#interface-genişletme)
11. [Özet](#özet)

## Extension Metot Nedir?

Extension metotlar (genişletme metotları), C# 3.0 ile tanıtılan ve mevcut tiplere kodunu değiştirmeden yeni fonksiyonellikler eklemenizi sağlayan statik metotlardır. Bu özellik, özellikle aşağıdaki durumlarda çok faydalıdır:

- Kaynak koduna erişiminiz olmayan sınıfları genişletmek istediğinizde
- Sealed (mühürlenmiş) sınıfları genişletmek istediğinizde
- Primitive tiplere (string, int vb.) yeni davranışlar eklemek istediğinizde

## Extension Metot Tanımlama Kuralları

Extension metot tanımlamak için aşağıdaki kurallara uymanız gerekir:

1. Metot **static** olarak tanımlanmalıdır
2. Metot **static bir sınıf** içinde yer almalıdır
3. İlk parametresi, genişletmek istediğiniz tipin önünde **this** anahtar kelimesi ile belirtilmelidir

```csharp
namespace ExtensionMethods
{
    public static class MyExtensions
    {
        public static int WordCount(this string str)
        {
            return str.Split(new char[] { ' ', '.', '?' }, 
                   StringSplitOptions.RemoveEmptyEntries).Length;
        }
    }
}
```

## Basit Örnek

Aşağıdaki örnekte string sınıfını genişleterek bir metnin tersini döndüren bir extension metot tanımlıyoruz:

```csharp
using System;

namespace ExtensionMethods
{
    public static class StringExtensions
    {
        public static string Reverse(this string input)
        {
            char[] charArray = input.ToCharArray();
            Array.Reverse(charArray);
            return new string(charArray);
        }
    }

    class Program
    {
        static void Main(string[] args)
        {
            string sample = "Merhaba Dünya";
            string reversed = sample.Reverse();  // Extension metodu kullanma
            
            Console.WriteLine($"Orijinal: {sample}");
            Console.WriteLine($"Tersine çevrilmiş: {reversed}");
            
            // Çıktı:
            // Orijinal: Merhaba Dünya
            // Tersine çevrilmiş: aynüD abahreM
        }
    }
}
```

## Extension Metot Kullanım Senaryoları

1. **Yardımcı Metotlar**: Sık kullanılan dönüşüm işlemleri için extension metotlar kullanılabilir.

```csharp
public static class DateTimeExtensions
{
    public static string ToTurkishDateString(this DateTime date)
    {
        return date.ToString("dd.MM.yyyy");
    }
    
    public static bool IsWeekend(this DateTime date)
    {
        return date.DayOfWeek == DayOfWeek.Saturday || date.DayOfWeek == DayOfWeek.Sunday;
    }
}

// Kullanım
DateTime bugun = DateTime.Now;
Console.WriteLine(bugun.ToTurkishDateString());  // 22.04.2025
Console.WriteLine(bugun.IsWeekend() ? "Hafta sonu" : "Hafta içi");
```

2. **Koleksiyon İşlemleri**: Liste, dizi vb. koleksiyonlar için yardımcı metotlar.

```csharp
public static class CollectionExtensions
{
    public static bool IsNullOrEmpty<T>(this IEnumerable<T> collection)
    {
        return collection == null || !collection.Any();
    }
    
    public static string ToDelimitedString<T>(this IEnumerable<T> collection, string delimiter = ", ")
    {
        return collection == null ? string.Empty : string.Join(delimiter, collection);
    }
}

// Kullanım
List<string> names = new List<string> { "Ali", "Veli", "Ayşe" };
Console.WriteLine(names.ToDelimitedString(" - "));  // Ali - Veli - Ayşe

List<string> emptyList = null;
if (emptyList.IsNullOrEmpty())  // Exception fırlatmaz, true döner
{
    Console.WriteLine("Liste boş veya null");
}
```

## Extension Metot vs. Inheritance

Extension metotların kalıtım (inheritance) ile önemli farkları vardır:

| Extension Metotlar | Kalıtım (Inheritance) |
|--------------------|------------------------|
| Orijinal sınıfın kodunu değiştirmez | Orijinal sınıfı genişletir |
| Sadece public memberları kullanabilir | Private ve protected üyelere erişebilir |
| Runtime'da değil, derleme zamanında çözümlenir | Runtime polimorfizm sağlar |
| Sınıf sealed olsa bile kullanılabilir | Sealed sınıfları genişletemezsiniz |
| Genellikle yardımcı metotlar için kullanılır | Nesne davranışını tamamen değiştirmek için kullanılır |

## Extension Metot Kısıtlamaları

Extension metotların bazı önemli kısıtlamaları bulunmaktadır:

1. Extension metotlar orijinal sınıfın **private** veya **protected** üyelerine erişemezler
2. Extension metotlar Runtime'da değil, derleme zamanında çözümlenirler
3. Extension metotlar virtual, abstract, override veya sealed olamazlar
4. Extension metotlar genişlettikleri sınıfın üyelerini geçersiz kılamazlar (override edemezler)
5. Extension metotlar instance değişkenleri içeremezler

## Best Practices

Extension metotları kullanırken dikkat edilmesi gereken noktalar:

1. **Namespace Kullanımı**: Extension metotlar uygun bir namespace içinde olmalıdır
   ```csharp
   using ExtensionMethods; // Extension metotların olduğu namespace
   ```

2. **Adlandırma Standardı**: Extension metotlar açıklayıcı isimlerle adlandırılmalıdır
   ```csharp
   public static bool IsValidEmail(this string email)
   ```

3. **Yan Etkilerden Kaçınma**: Extension metotlar, genişlettikleri nesnenin durumunu değiştirmemelidir
   ```csharp
   // İyi: Yeni bir nesne döner
   public static string Capitalize(this string s) => char.ToUpper(s[0]) + s.Substring(1);
   
   // Kötü: Parametre olarak aldığı listeyi değiştirir
   public static void AddIfNotExists<T>(this List<T> list, T item) { /*...*/ }
   ```

4. **Null Kontrolü**: Extension metotlar her zaman null kontrolü yapmalıdır
   ```csharp
   public static string Reverse(this string input)
   {
       if (input == null)
           throw new ArgumentNullException(nameof(input));
           
       // Metot gövdesi...
   }
   ```

5. **Kapsam Sınırlaması**: Extension metotlar genellikle belirli tiplere özel olmalıdır
   ```csharp
   // StringExtensions.cs dosyasında
   public static class StringExtensions { /*...*/ }
   
   // DateTimeExtensions.cs dosyasında
   public static class DateTimeExtensions { /*...*/ }
   ```

## LINQ ve Extension Metotlar

LINQ (Language Integrated Query), extension metotların en yaygın kullanıldığı alanlardan biridir. LINQ'nun birçok metodu (`Where`, `Select`, `OrderBy`, vb.), extension metot olarak tanımlanmıştır.

```csharp
using System;
using System.Collections.Generic;
using System.Linq;

class Program
{
    static void Main()
    {
        List<int> numbers = new List<int> { 5, 3, 9, 2, 8, 1, 7 };
        
        // Extension metot sözdizimi kullanarak LINQ
        var evenNumbers = numbers.Where(n => n % 2 == 0).OrderBy(n => n);
        
        // Metot çağırma sözdizimi kullanarak aynı LINQ sorgusu
        var evenNumbers2 = Enumerable.OrderBy(Enumerable.Where(numbers, n => n % 2 == 0), n => n);
        
        foreach (var num in evenNumbers)
        {
            Console.WriteLine(num);
        }
        // Çıktı: 2, 8
    }
}
```

## Generic Extension Metotlar

Generic tipler için de extension metotlar tanımlayabilirsiniz:

```csharp
public static class GenericExtensions
{
    public static bool HasItems<T>(this IEnumerable<T> source)
    {
        return source != null && source.Any();
    }
    
    public static T GetOrDefault<T>(this Dictionary<string, T> dict, string key, T defaultValue = default)
    {
        return dict.TryGetValue(key, out T value) ? value : defaultValue;
    }
}

// Kullanım
List<int> numbers = new List<int> { 1, 2, 3 };
if (numbers.HasItems())
{
    // İşlem yap...
}

Dictionary<string, int> ages = new Dictionary<string, int>
{
    ["Ali"] = 30,
    ["Veli"] = 25
};

int ayseYas = ages.GetOrDefault("Ayşe", 18);  // Ayşe anahtarı yok, 18 döner
Console.WriteLine(ayseYas);  // 18
```

## Interface Genişletme

Interface'ler de extension metotlarla genişletilebilir. Bu, interface'e yeni bir metot eklemek istediğinizde tüm implementasyonları değiştirmek zorunda kalmadan yapabilmenizi sağlar:

```csharp
public interface IRepository<T>
{
    IEnumerable<T> GetAll();
    T GetById(int id);
    void Add(T entity);
    void Delete(int id);
}

public static class RepositoryExtensions
{
    public static IEnumerable<T> GetByIds<T>(this IRepository<T> repository, IEnumerable<int> ids)
    {
        return ids.Select(id => repository.GetById(id)).Where(item => item != null);
    }
    
    public static void AddRange<T>(this IRepository<T> repository, IEnumerable<T> entities)
    {
        foreach (var entity in entities)
        {
            repository.Add(entity);
        }
    }
}

// Kullanım
public class ProductRepository : IRepository<Product>
{
    // Interface implementasyonu...
}

var repository = new ProductRepository();
var products = repository.GetByIds(new[] { 1, 2, 3 });  // Extension metot kullanımı
repository.AddRange(newProducts);  // Extension metot kullanımı
```

## Özet

Extension metotlar, C#'ta mevcut tiplerin işlevselliğini kodlarını değiştirmeden genişletmenizi sağlar. Özellikle kaynak koduna erişemediğiniz tiplerde ve sealed sınıflarda kullanışlıdır. LINQ gibi güçlü özelliklerin temelini oluşturur.

Extension metotlar kullanırken:

1. Metotlar static bir sınıf içinde statik olarak tanımlanmalıdır
2. İlk parametre, genişletilecek tipin önünde `this` anahtar kelimesi ile işaretlenmelidir
3. Genişletilen nesneyi değiştirmek yerine yeni bir değer döndürün
4. Her zaman null kontrolü yapın
5. Açıklayıcı metot isimleri kullanın
6. Extension metotları uygun namespace'ler içinde düzenleyin

Doğru şekilde kullanıldığında, extension metotlar kodunuzu daha okunabilir, daha modüler ve daha sürdürülebilir hale getirebilir.
