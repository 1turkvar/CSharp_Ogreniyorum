# C# Records: Kapsamlı Rehber

## İçindekiler
1. [Records Nedir?](#records-nedir)
2. [Records Tanımlama](#records-tanımlama)
3. [Değer Eşitliği](#değer-eşitliği)
4. [Değişmezlik (Immutability)](#değişmezlik-immutability)
5. [Özellikler ve Alanlar](#özellikler-ve-alanlar)
6. [With Expressions](#with-expressions)
7. [Veri Sınıfları (class) vs Records](#veri-sınıfları-class-vs-records)
8. [Records Kalıtımı](#records-kalıtımı)
9. [Deconstruction (Yapı Çözme)](#deconstruction-yapı-çözme)
10. [Özelleştirilmiş Eşitlik Karşılaştırması](#özelleştirilmiş-eşitlik-karşılaştırması)
11. [Pozisyonel ve Nominal Records](#pozisyonel-ve-nominal-records)
12. [Yaygın Kullanım Senaryoları](#yaygın-kullanım-senaryoları)
13. [Performans Değerlendirmeleri](#performans-değerlendirmeleri)
14. [En İyi Uygulamalar](#en-iyi-uygulamalar)

## Records Nedir?

Records, C# 9.0 ile tanıtılan, özellikle veri taşıma operasyonları için tasarlanmış referans türleridir. Geleneksel sınıflardan farklı olarak, Records değer semantiğine sahiptir ve varsayılan olarak değişmez (immutable) olarak tasarlanmışlardır.

Records, POCO (Plain Old CLR Objects) sınıflarını veya veri transfer nesnelerini (DTO) oluştururken yazmanız gereken tekrarlayan kodu önemli ölçüde azaltır. Bu yapılar aşağıdaki özellikleri otomatik olarak sağlar:

- Değer tabanlı eşitlik karşılaştırması
- Otomatik oluşturulmuş ToString() yöntemi
- Değişmezlik desteği
- Yapı çözme (deconstruction) özellikleri
- "With" ifadeleri ile yeni kopyalar oluşturma yeteneği

## Records Tanımlama

Records tanımlamanın birkaç yolu vardır:

### 1. Pozisyonel Sözdizimi (Concise Syntax)

```csharp
// Pozisyonel record tanımı
public record Person(string FirstName, string LastName, int Age);
```

Bu sözdizimi, belirtilen özelliklerle bir init-only (yalnızca başlatılabilir) özelliklerine sahip bir record oluşturur.

### 2. Standart Sözdizimi

```csharp
// Standart record tanımı
public record Person
{
    public string FirstName { get; init; }
    public string LastName { get; init; }
    public int Age { get; init; }
}
```

### 3. Struct Record (C# 10.0+)

```csharp
// Struct record tanımı - değer türü olarak record
public record struct Point(double X, double Y);
```

### 4. Class Record (Açık Belirtme)

```csharp
// Record sınıfını açıkça belirtme
public record class Employee(string Name, string Department);
```

## Değer Eşitliği

Records'un en önemli özelliklerinden biri değer eşitliği sağlamasıdır. İki record aynı türdeyse ve tüm özellikleri eşitse, bu iki record eşit kabul edilir.

```csharp
var person1 = new Person("John", "Doe", 30);
var person2 = new Person("John", "Doe", 30);
var person3 = new Person("Jane", "Doe", 25);

Console.WriteLine(person1 == person2); // True
Console.WriteLine(person1 == person3); // False
Console.WriteLine(person1.Equals(person2)); // True
```

Bu davranış, geleneksel sınıfların referans eşitliğinden farklıdır. Normal sınıflarda, iki nesne ancak aynı referansa sahipse eşit kabul edilir.

## Değişmezlik (Immutability)

Records varsayılan olarak özellikleri "init-only" olarak tanımlar, bu da değerlerin yalnızca nesne oluşturulduğunda ayarlanabileceği ve daha sonra değiştirilemeyeceği anlamına gelir.

```csharp
var person = new Person("John", "Doe", 30);

// Aşağıdaki kod derleme hatası verir
// person.FirstName = "Jane";
```

Ancak, gerekirse değiştirilebilir özellikler de ekleyebilirsiniz:

```csharp
public record Person(string FirstName, string LastName)
{
    public int Age { get; set; } // Bu özellik değiştirilebilir
    public DateTime LastModified { get; set; }
}
```

## Özellikler ve Alanlar

Pozisyonel sözdizimi kullanıldığında, belirtilen tüm parametreler için record otomatik olarak public init-only özellikler oluşturur. Ancak ihtiyacınıza göre ek özellikler, alanlar ve yöntemler ekleyebilirsiniz:

```csharp
public record Person(string FirstName, string LastName)
{
    // Otomatik hesaplanan özellik
    public string FullName => $"{FirstName} {LastName}";
    
    // Özel alan
    private readonly DateTime _created = DateTime.Now;
    
    // Public erişilebilir özellik
    public DateTime Created => _created;
    
    // Yöntem
    public string Greet() => $"Hello, {FirstName}!";
}
```

## With Expressions

Records ile "with expressions" kullanarak, mevcut bir record'un yeni bir kopyasını oluşturabilir ve bazı özelliklerini değiştirebilirsiniz:

```csharp
var person1 = new Person("John", "Doe", 30);
var person2 = person1 with { Age = 31 };
var person3 = person2 with { FirstName = "Jane", LastName = "Smith" };

Console.WriteLine(person1); // Person { FirstName = John, LastName = Doe, Age = 30 }
Console.WriteLine(person2); // Person { FirstName = John, LastName = Doe, Age = 31 }
Console.WriteLine(person3); // Person { FirstName = Jane, LastName = Smith, Age = 31 }
```

Bu yaklaşım, değişmezlik prensibini korurken nesneleri kolayca klonlamayı ve güncelleştirmeyi sağlar.

## Veri Sınıfları (class) vs Records

Records ile normal sınıflar arasındaki temel farklar:

| Özellik | Record | Class |
|---------|--------|-------|
| Eşitlik Karşılaştırması | Değer eşitliği | Referans eşitliği |
| Değişmezlik | Varsayılan olarak değişmez | Varsayılan olarak değiştirilebilir |
| ToString() Davranışı | Otomatik olarak tüm özellikler | Object.ToString() |
| Yapı Çözme | Pozisyonel records için otomatik | Manuel implementasyon gerekli |
| With Expressions | Desteklenir | Desteklenmez |
| Kalıtım | Record kalıtımı destekler | Class kalıtımı destekler |

## Records Kalıtımı

Records diğer record türlerinden türetilebilir:

```csharp
public record Person(string FirstName, string LastName);
public record Employee(string FirstName, string LastName, string Department) : Person(FirstName, LastName);
```

Türetilmiş record'lar için değer eşitliği, hem temel record'un özellikleri hem de türetilmiş record'a özgü özellikleri içerir.

Önemli kısıtlamalar:
- Bir record bir class'tan türetilemez (ancak interface'leri implement edebilir)
- Bir class bir record'dan türetilemez

## Deconstruction (Yapı Çözme)

Pozisyonel sözdiziminde tanımlanan record'lar, otomatik olarak bir Deconstruct yöntemi sağlar:

```csharp
public record Person(string FirstName, string LastName, int Age);

// Kullanımı
var person = new Person("John", "Doe", 30);
var (firstName, lastName, age) = person;

Console.WriteLine($"{firstName} {lastName} is {age} years old.");
```

Standart sözdizimi kullanıyorsanız, yapı çözme işlemini manuel olarak uygulamanız gerekir:

```csharp
public record Person
{
    public string FirstName { get; init; }
    public string LastName { get; init; }
    public int Age { get; init; }
    
    public void Deconstruct(out string firstName, out string lastName, out int age)
    {
        firstName = FirstName;
        lastName = LastName;
        age = Age;
    }
}
```

## Özelleştirilmiş Eşitlik Karşılaştırması

Eşitlik karşılaştırması sırasında bazı özellikleri göz ardı etmek isterseniz, `EqualityContract` ve `Equals` yöntemlerini geçersiz kılabilirsiniz:

```csharp
public record Person(string FirstName, string LastName)
{
    public DateTime LastUpdated { get; set; }
    
    // LastUpdated özelliğini eşitlik karşılaştırmasından hariç tutuyoruz
    public virtual bool Equals(Person other)
    {
        if (other == null) return false;
        return FirstName == other.FirstName && LastName == other.LastName;
        // LastUpdated karşılaştırılmıyor
    }
    
    public override int GetHashCode()
    {
        return HashCode.Combine(FirstName, LastName);
    }
}
```

## Pozisyonel ve Nominal Records

C#'ta iki tür record tanımı vardır:

1. **Pozisyonel Records**: Kısa sözdizimi kullanılarak tanımlanır ve otomatik olarak özellikler, constructor ve deconstruct metodu oluşturulur.

```csharp
public record Person(string FirstName, string LastName, int Age);
```

2. **Nominal Records**: Geleneksel sınıf benzeri sözdizimi kullanılarak tanımlanır.

```csharp
public record Person
{
    public string FirstName { get; init; }
    public string LastName { get; init; }
    public int Age { get; init; }
}
```

## Yaygın Kullanım Senaryoları

Records özellikle aşağıdaki senaryolarda faydalıdır:

1. **Veri Transfer Nesneleri (DTO)**: API'ler arasında veri taşımak için kullanılan değişmez nesneler.

```csharp
public record UserDto(int Id, string Username, string Email);
```

2. **Domain Modeli**: Değer nesneleri ve değişmez varlıklar için.

```csharp
public record Money(decimal Amount, string Currency);
public record Product(int Id, string Name, Money Price);
```

3. **Durum Yönetimi**: Özellikle işlevsel programlama yaklaşımında veya Redux benzeri durum yönetimi sistemlerinde.

```csharp
public record AppState(UserData User, ImmutableList<Notification> Notifications);
```

4. **Pattern Matching**: C#'ın desen eşleştirme özellikleriyle iyi çalışır.

```csharp
public record Shape(string Type);
public record Circle(double Radius) : Shape("Circle");
public record Rectangle(double Width, double Height) : Shape("Rectangle");

void ProcessShape(Shape shape)
{
    var area = shape switch
    {
        Circle c => Math.PI * c.Radius * c.Radius,
        Rectangle r => r.Width * r.Height,
        _ => throw new ArgumentException("Unknown shape")
    };
    Console.WriteLine($"Area: {area}");
}
```

## Performans Değerlendirmeleri

Records'lar normal sınıflara göre bazı ek bellek ve işlem maliyetlerine sahip olabilir:

- Değer eşitliği karşılaştırması, özellikle çok sayıda özelliğe sahip karmaşık record'lar için daha fazla CPU kullanabilir
- With expressions, yeni bir nesne oluşturma ve tüm verileri kopyalama gerektirdiğinden ek bellek kullanımına neden olabilir
- Record struct türünü kullanarak performans artışı sağlayabilirsiniz (referans tipten ziyade değer tipi olarak)

```csharp
// Değer tipi record - daha verimli bellek kullanımı sağlayabilir
public record struct Point(double X, double Y, double Z);
```

## En İyi Uygulamalar

1. **Değişmezliği Koruyun**: Records değişmez veri yapıları için tasarlanmıştır. Değiştirilebilir özellikler eklemek gerekiyorsa, bunun gerçekten gerekli olup olmadığını düşünün.

2. **Karmaşık Mantık İçin Sınıfları Tercih Edin**: Records veri taşıma için idealdir, ancak karmaşık iş mantığı içeren türler için geleneksel sınıflar daha uygun olabilir.

3. **Record Struct Kullanımı**: Küçük, basit veri yapıları için ve yüksek performans gerektiren durumlarda `record struct` kullanmayı düşünün.

4. **Hash Tabanlı Koleksiyonlardaki Davranışlara Dikkat Edin**: Records değiştirilebilir özellikler içeriyorsa, bunlar HashSet veya Dictionary gibi koleksiyonlarda anahtar olarak kullanıldığında beklenmeyen davranışlara neden olabilir.

5. **Serialize/Deserialize İşlemlerine Dikkat Edin**: Records'un JSON veya diğer serileştirme formatlarıyla çalışma şekli, constructor parametreleri ve özellik adlandırma kurallarına bağlı olarak değişebilir.

```csharp
// JSON serileştirme için uygun record
public record Product(int Id, string Name, decimal Price)
{
    // Ek özellikler için parametre adları ve özellik adları eşleşmelidir
    [JsonIgnore] // Serileştirme dışında tutmak için
    public DateTime LastUpdated { get; init; } = DateTime.Now;
}
```

---

Records, C#'ta veri odaklı programlama için güçlü bir araçtır. Değer semantiği ve değişmezlik desteği ile daha güvenli ve öngörülebilir kodlar yazmanıza yardımcı olur. Geleneksel sınıfların yerini tamamen almak için tasarlanmamışlardır; bunun yerine, belirli kullanım durumları için optimize edilmiş ek bir araçtır.
