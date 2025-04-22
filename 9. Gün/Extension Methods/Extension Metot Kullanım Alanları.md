# C# Extension Metotları Rehberi

## İçindekiler
1. [Extension Metot Nedir?](#extension-metot-nedir)
2. [Temel Sözdizimi](#temel-sözdizimi)
3. [Extension Metotların Kullanım Alanları](#extension-metotların-kullanım-alanları)
4. [LINQ ve Extension Metotlar](#linq-ve-extension-metotlar)
5. [En İyi Uygulamalar ve Dikkat Edilmesi Gerekenler](#en-iyi-uygulamalar-ve-dikkat-edilmesi-gerekenler)
6. [Gerçek Dünya Örnekleri](#gerçek-dünya-örnekleri)
7. [Kısıtlamalar](#kısıtlamalar)
8. [Sonuç](#sonuç)

## Extension Metot Nedir?

Extension metotlar, var olan tiplere (sınıf, yapı, arayüz, vs.) kaynak kodlarını değiştirmeden yeni metotlar ekleyebilmemizi sağlayan özel statik metotlardır. C# 3.0 ile dile eklenen bu özellik, özellikle var olan .NET Framework sınıflarını ve üçüncü parti kütüphaneleri genişletmek için yaygın olarak kullanılır.

## Temel Sözdizimi

Bir extension metot tanımlamak için:

```csharp
public static class StringExtensions
{
    public static string Capitalize(this string input)
    {
        if (string.IsNullOrEmpty(input))
            return input;
            
        return char.ToUpper(input[0]) + input.Substring(1);
    }
}
```

Burada dikkat edilmesi gereken noktalar:
- Extension metotlar **statik bir sınıf** içinde tanımlanmalıdır
- Extension metot **statik** olmalıdır
- İlk parametre, genişletilecek tipin önüne **this** anahtar kelimesi konularak belirtilir

Kullanımı:

```csharp
string text = "merhaba dünya";
string capitalized = text.Capitalize(); // "Merhaba dünya"
```

## Extension Metotların Kullanım Alanları

### 1. Yerleşik Tipleri Genişletme

.NET Framework'te bulunan `string`, `int`, `DateTime` gibi temel tiplere yeni işlevsellikler ekleyebilirsiniz.

```csharp
public static class DateTimeExtensions
{
    public static bool IsWeekend(this DateTime date)
    {
        return date.DayOfWeek == DayOfWeek.Saturday || 
               date.DayOfWeek == DayOfWeek.Sunday;
    }
    
    public static DateTime StartOfMonth(this DateTime date)
    {
        return new DateTime(date.Year, date.Month, 1);
    }
}

// Kullanımı
DateTime today = DateTime.Now;
if (today.IsWeekend())
{
    Console.WriteLine("Bugün hafta sonu!");
}
```

### 2. Arayüzlerin Genişletilmesi

Arayüzleri genişleterek, o arayüzü uygulayan tüm sınıflara bir kerede işlevsellik ekleyebilirsiniz.

```csharp
public static class EnumerableExtensions
{
    public static void ForEach<T>(this IEnumerable<T> source, Action<T> action)
    {
        foreach (var item in source)
        {
            action(item);
        }
    }
}

// Kullanımı
List<string> names = new List<string> { "Ali", "Veli", "Ayşe" };
names.ForEach(name => Console.WriteLine(name));
```

### 3. Zincir Metodlar Oluşturma (Method Chaining)

Extension metotlar, fonksiyonel programlamada sıkça kullanılan zincir metotlar oluşturmayı kolaylaştırır:

```csharp
public static class QueryExtensions
{
    public static IEnumerable<T> Filter<T>(this IEnumerable<T> source, Func<T, bool> predicate)
    {
        return source.Where(predicate);
    }
    
    public static IEnumerable<R> Transform<T, R>(this IEnumerable<T> source, Func<T, R> transformer)
    {
        return source.Select(transformer);
    }
}

// Kullanımı
var result = students
    .Filter(s => s.Age > 18)
    .Transform(s => new { Name = s.Name, IsAdult = true });
```

### 4. Yardımcı Metotlar Kütüphanesi Oluşturma

Projelerinizde sürekli kullandığınız işlevler için yardımcı metotlar kütüphanesi oluşturabilirsiniz:

```csharp
public static class StringHelpers
{
    public static bool IsValidEmail(this string input)
    {
        // E-posta doğrulama mantığı
        return Regex.IsMatch(input, @"^[^@\s]+@[^@\s]+\.[^@\s]+$");
    }
    
    public static bool IsValidTCKN(this string input)
    {
        // TC Kimlik numarası doğrulama mantığı
        if (input.Length != 11) return false;
        // Diğer doğrulama kuralları...
        return true;
    }
}
```

### 5. Null Kontrolü İçin Güvenlik Katmanı

Null referans hatalarını önlemek için güvenlik katmanı ekleyebilirsiniz:

```csharp
public static class ObjectExtensions
{
    public static T ThrowIfNull<T>(this T obj, string paramName) where T : class
    {
        if (obj == null)
            throw new ArgumentNullException(paramName);
        
        return obj;
    }
    
    public static string SafeToString(this object obj)
    {
        return obj?.ToString() ?? string.Empty;
    }
}
```

### 6. İş Kurallarının Uygulanması

Domain-Driven Design yaklaşımında, domain modellerine iş kuralları ekleyebilirsiniz:

```csharp
public static class OrderExtensions
{
    public static bool IsEligibleForDiscount(this Order order)
    {
        return order.TotalAmount > 1000 || order.Items.Count > 10;
    }
    
    public static decimal CalculateShippingCost(this Order order)
    {
        // Kargo ücreti hesaplama mantığı
        if (order.TotalAmount > 500)
            return 0; // Ücretsiz kargo
            
        return 35.99m; // Standart kargo ücreti
    }
}
```

## LINQ ve Extension Metotlar

LINQ (Language Integrated Query), C#'ta extension metotların en yaygın kullanım alanıdır. LINQ'nun Where, Select, OrderBy gibi tüm metotları aslında `IEnumerable<T>` arayüzü için yazılmış extension metotlardır.

```csharp
// System.Linq içinde bu şekilde tanımlanmıştır
public static IEnumerable<TSource> Where<TSource>(
    this IEnumerable<TSource> source,
    Func<TSource, bool> predicate)
{
    // Filtreleme mantığı
}

// Kullanımı
var adults = people.Where(p => p.Age >= 18);
```

Kendi LINQ benzeri extension metotlarınızı da yazabilirsiniz:

```csharp
public static class MyLinqExtensions
{
    public static T FirstOrDefault<T>(this IEnumerable<T> source, T defaultValue)
    {
        foreach (var item in source)
        {
            return item;
        }
        return defaultValue;
    }
    
    public static IEnumerable<T> Take<T>(this IEnumerable<T> source, int count, int skip = 0)
    {
        int current = 0;
        foreach (var item in source)
        {
            if (current < skip)
            {
                current++;
                continue;
            }
            
            if (current < skip + count)
            {
                yield return item;
                current++;
            }
            else
            {
                break;
            }
        }
    }
}
```

## En İyi Uygulamalar ve Dikkat Edilmesi Gerekenler

### 1. Statik Sınıflar İçinde Tanımlama

Extension metotlar her zaman statik bir sınıf içinde tanımlanmalıdır. İsimlendirme standartlarına uygun olarak, genellikle sınıf isimleri çoğul olarak ve "Extensions" son eki ile oluşturulur:

```csharp
public static class StringExtensions { /* ... */ }
public static class DateTimeExtensions { /* ... */ }
```

### 2. Namespace Kullanımı

Extension metotları mantıklı namespace'ler altında gruplayın. Bir extension metodun kullanılabilmesi için, tanımlandığı namespace'in `using` direktifi ile projeye dahil edilmesi gerekir.

```csharp
namespace MyCompany.Utilities.Extensions
{
    public static class StringExtensions { /* ... */ }
}

// Başka bir dosyada
using MyCompany.Utilities.Extensions;
```

### 3. İşlevsel Uyumluluk

Extension metotlar, genişlettikleri tipin doğal davranışlarına ve tasarım felsefesine uygun olmalıdır. Örneğin, string tipi için bir extension metot yazdığınızda, .NET'in diğer string metotlarının davranışlarına ve isimlendirme stiline uygun olmalıdır.

### 4. Yan Etkilerden Kaçınma

Extension metotlar ideal olarak yan etkisiz (pure) olmalıdır. Yani, genişlettikleri nesnenin durumunu değiştirmemeli ve aynı girdi için her zaman aynı çıktıyı üretmelidir.

```csharp
// İyi bir örnek (yan etkisiz)
public static string ToTitleCase(this string input)
{
    // Yeni bir string döndürür, orijinali değiştirmez
    return CultureInfo.CurrentCulture.TextInfo.ToTitleCase(input.ToLower());
}

// Kaçınılması gereken (yan etkili)
public static void AddItem<T>(this List<T> list, T item)
{
    list.Add(item); // Listeyi değiştirir
}
```

### 5. Exception Yönetimi

Extension metotlar istisna fırlatmadan önce uygun kontroller yapmalıdır. Özellikle null referans kontrolü önemlidir:

```csharp
public static string Truncate(this string text, int maxLength)
{
    if (text == null)
        throw new ArgumentNullException(nameof(text));
        
    if (maxLength < 0)
        throw new ArgumentOutOfRangeException(nameof(maxLength));
        
    return text.Length <= maxLength ? text : text.Substring(0, maxLength);
}
```

### 6. Çok Fazla Extension Metot Kullanımından Kaçınma

Bir tipe çok fazla extension metot eklemek, IntelliSense listesinin kirlenmesine ve geliştirici deneyiminin olumsuz etkilenmesine yol açabilir. Sadece gerçekten gerekli olan extension metotları oluşturun.

## Gerçek Dünya Örnekleri

### 1. Veri Dönüşümleri

```csharp
public static class DataConversionExtensions
{
    public static int ToInt32(this string value, int defaultValue = 0)
    {
        if (int.TryParse(value, out int result))
            return result;
            
        return defaultValue;
    }
    
    public static decimal ToDecimal(this string value, decimal defaultValue = 0)
    {
        if (decimal.TryParse(value, NumberStyles.Any, CultureInfo.InvariantCulture, out decimal result))
            return result;
            
        return defaultValue;
    }
    
    public static bool ToBoolean(this string value)
    {
        if (string.IsNullOrEmpty(value))
            return false;
            
        return value.ToLower() == "true" || 
               value == "1" || 
               value.ToLower() == "yes" || 
               value.ToLower() == "evet";
    }
}
```

### 2. Web Uygulamalarında Kullanım

```csharp
public static class ControllerExtensions
{
    public static IActionResult JsonSuccess(this Controller controller, object data = null)
    {
        return controller.Json(new { success = true, data });
    }
    
    public static IActionResult JsonError(this Controller controller, string message)
    {
        return controller.Json(new { success = false, message });
    }
}

// ASP.NET Core uygulamasında kullanımı
public IActionResult GetUsers()
{
    try
    {
        var users = _userService.GetAllUsers();
        return this.JsonSuccess(users);
    }
    catch (Exception ex)
    {
        return this.JsonError(ex.Message);
    }
}
```

### 3. Entity Framework ile Sorgu Geliştirme

```csharp
public static class QueryableExtensions
{
    public static IQueryable<T> WhereIf<T>(
        this IQueryable<T> source, 
        bool condition, 
        Expression<Func<T, bool>> predicate)
    {
        if (condition)
            return source.Where(predicate);
            
        return source;
    }
    
    public static IQueryable<T> Paginate<T>(
        this IQueryable<T> source, 
        int pageNumber, 
        int pageSize)
    {
        return source
            .Skip((pageNumber - 1) * pageSize)
            .Take(pageSize);
    }
}

// Kullanımı
var query = dbContext.Users
    .WhereIf(!string.IsNullOrEmpty(searchTerm), u => u.Name.Contains(searchTerm))
    .WhereIf(minAge.HasValue, u => u.Age >= minAge.Value)
    .OrderBy(u => u.Name)
    .Paginate(pageNumber, pageSize);
```

### 4. Test Otomasyon Frameworkleri

```csharp
public static class FluentAssertionExtensions
{
    public static T ShouldNotBeNull<T>(this T obj) where T : class
    {
        if (obj == null)
            throw new AssertionException("Object should not be null");
            
        return obj;
    }
    
    public static string ShouldContain(this string actual, string expected)
    {
        if (!actual.Contains(expected))
            throw new AssertionException($"String '{actual}' should contain '{expected}'");
            
        return actual;
    }
}

// Test kodu örneği
[Test]
public void UserService_GetUserById_ShouldReturnUser()
{
    var user = _userService.GetUserById(1)
        .ShouldNotBeNull()
        .Name.ShouldContain("John");
}
```

## Kısıtlamalar

Extension metotların bazı kısıtlamaları vardır:

1. **Erişim sınırlamaları**: Extension metotlar genişlettikleri tipin private üyelerine erişemezler. Sadece public ve internal (aynı assembly içinde) üyelere erişebilirler.

2. **Override edilemezler**: Extension metotlar, genişlettikleri tipin gerçek metotları değildir ve polimorfizm kurallarına tabi değildir. Bir türün extension metodu, türetilmiş sınıflar için override edilemez.

3. **İnstance metotlara göre daha düşük öncelik**: Bir sınıfta instance metot varsa, aynı isim ve parametrelerle tanımlanmış extension metot çağrılmaz, bunun yerine instance metot çağrılır.

```csharp
class MyClass
{
    public void DoSomething() { /* ... */ }
}

static class MyClassExtensions
{
    public static void DoSomething(this MyClass obj) { /* ... */ }
}

var obj = new MyClass();
obj.DoSomething(); // Instance metot çağrılır, extension metot değil
```

4. **Generic kısıtlamaları**: Extension metotlar, generic type parameter'ları için kısıtlamalar alabilir, ancak bu kısıtlamalar normal metotlardaki kadar esnek değildir.

## Sonuç

Extension metotlar, C# dilinin en güçlü özelliklerinden biridir ve kodunuzu daha okunabilir, bakımı kolay ve modüler hale getirmenize yardımcı olur. Doğru kullanıldığında, kodunuzun kalitesini ve geliştirme verimliliğinizi önemli ölçüde artırabilir. 

Ancak, her güçlü özellik gibi, extension metotları da dikkatli kullanmak gerekir. Mevcut sınıfların tasarım felsefesine saygı duymak, yan etkilerden kaçınmak ve aşırı kullanımdan kaçınmak önemlidir.

Extension metotların avantajları:
- Mevcut tipleri değiştirmeden genişletebilme
- Kodun okunabilirliğini artırma
- Yaygın işlemleri merkezileştirme
- Fonksiyonel programlama tarzını destekleme
- Zincir metot (method chaining) desenini kolaylaştırma

Extension metotlar, modern C# programlamanın vazgeçilmez bir parçasıdır ve hem LINQ gibi framework özelliklerinin temelini oluşturur hem de kendi projelerinizde kodunuzu daha temiz ve modüler hale getirmenize yardımcı olur.
