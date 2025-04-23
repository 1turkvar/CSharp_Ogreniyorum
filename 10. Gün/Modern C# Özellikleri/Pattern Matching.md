# C# Pattern Matching Detaylı Rehber

## İçindekiler

1. [Giriş](#giriş)
2. [Pattern Matching Nedir?](#pattern-matching-nedir)
3. [C# Pattern Matching Türleri](#c-pattern-matching-türleri)
   - [Tür Pattern Matching (Type Pattern)](#tür-pattern-matching-type-pattern)
   - [Sabit Pattern Matching (Constant Pattern)](#sabit-pattern-matching-constant-pattern)
   - [Var Pattern](#var-pattern)
   - [Relational Pattern (İlişkisel Pattern)](#relational-pattern-ilişkisel-pattern)
   - [Logical Pattern (Mantıksal Pattern)](#logical-pattern-mantıksal-pattern)
   - [Property Pattern (Özellik Pattern)](#property-pattern-özellik-pattern)
   - [Tuple Pattern](#tuple-pattern)
   - [Positional Pattern (Konumsal Pattern)](#positional-pattern-konumsal-pattern)
   - [List Pattern](#list-pattern)
   - [Switch Expression](#switch-expression)
4. [Pattern Matching Kullanım Alanları](#pattern-matching-kullanım-alanları)
5. [Pattern Matching ile İlgili İyi Pratikler](#pattern-matching-ile-ilgili-iyi-pratikler)
6. [Örnek Uygulamalar](#örnek-uygulamalar)
7. [Performans Değerlendirmeleri](#performans-değerlendirmeleri)
8. [Sonuç](#sonuç)

## Giriş

C# dilinde pattern matching, C# 7.0 ile tanıtılan ve sonraki sürümlerde sürekli geliştirilen bir özelliktir. Pattern matching, verileri belirli kalıplara göre test etmenize, eşleşen kalıplarda spesifik işlemler yapmanıza ve kodunuzu daha okunabilir, bakımı kolay hale getirmenize olanak tanır.

Bu rehber, C# pattern matching özelliklerini detaylı olarak inceleyecek ve gerçek dünya örnekleriyle bu tekniklerin nasıl kullanılacağını gösterecektir.

## Pattern Matching Nedir?

Pattern matching, bir değeri belirli bir "kalıp" veya "örüntü" ile eşleştirme ve bu eşleşmeye göre işlem yapma yöntemidir. Bu kalıplar basit bir tür kontrolünden karmaşık nesne yapısı eşleştirmelerine kadar uzanabilir.

C#'ta pattern matching, `is` operatörü ve `switch` ifadesi ile kullanılır. C# 8.0 ve sonraki sürümlerde, switch expressions gibi daha gelişmiş pattern matching yetenekleri eklenmiştir.

## C# Pattern Matching Türleri

### Tür Pattern Matching (Type Pattern)

Bir nesnenin belirli bir türe ait olup olmadığını kontrol etmek ve eşleşme durumunda nesneyi o türe dönüştürmek için kullanılır.

```csharp
// C# 7.0 öncesi:
if (obj is string)
{
    string str = (string)obj;
    // str ile işlem yap
}

// C# 7.0 ve sonrası:
if (obj is string str)
{
    // str doğrudan kullanılabilir
    Console.WriteLine(str.Length);
}
```

Switch ifadesi ile:

```csharp
switch (obj)
{
    case string str:
        Console.WriteLine($"String: {str}");
        break;
    case int num:
        Console.WriteLine($"Integer: {num}");
        break;
    case Person p:
        Console.WriteLine($"Person: {p.Name}");
        break;
    default:
        Console.WriteLine("Unknown type");
        break;
}
```

### Sabit Pattern Matching (Constant Pattern)

Bir değeri sabit bir değerle karşılaştırmak için kullanılır.

```csharp
if (obj is null)
{
    // obj null ise çalışır
}

switch (dayOfWeek)
{
    case "Monday":
        return 1;
    case "Tuesday":
        return 2;
    // ...
}
```

### Var Pattern

`var` pattern, değeri bir değişkene atamak için kullanılır ve tüm değerlerle eşleşir.

```csharp
if (obj is var x)
{
    // x, obj'nin değerini içerir, her zaman çalışır
    Console.WriteLine($"Value: {x}");
}

switch (expression)
{
    case var result when result.Length > 10:
        Console.WriteLine("Long result");
        break;
    case var result:
        Console.WriteLine($"Result: {result}");
        break;
}
```

### Relational Pattern (İlişkisel Pattern)

C# 9.0 ile gelmiştir. Sayısal değerleri karşılaştırmak için <, >, <=, >= operatörlerini kullanabilirsiniz.

```csharp
static string GetTemperatureDescription(double temperature) => temperature switch
{
    < 0 => "Dondurucu",
    < 15 => "Soğuk",
    < 25 => "Ilık",
    < 35 => "Sıcak",
    _ => "Çok sıcak"
};
```

### Logical Pattern (Mantıksal Pattern)

C# 9.0 ile gelmiştir. `and`, `or` ve `not` mantıksal operatörleri kullanarak kalıpları birleştirebilirsiniz.

```csharp
static string GetTemperatureAlert(double temperature) => temperature switch
{
    < 0 or > 40 => "Tehlikeli sıcaklık!",
    >= 0 and < 15 => "Soğuk, dikkatli olun",
    >= 15 and <= 30 => "Normal sıcaklık",
    > 30 and <= 40 => "Sıcak, bol su için",
    _ => "Bu durum hiç gerçekleşmez"
};

if (person is not null and { Age: >= 18 })
{
    // Person null değil ve yaşı 18 veya üstü
}
```

### Property Pattern (Özellik Pattern)

Bir nesnenin özelliklerini kontrol etmek için kullanılır.

```csharp
if (person is { Name: "John", Age: 30 })
{
    // person.Name "John" ve person.Age 30 ise çalışır
}

static string GetDiscountType(Customer customer) => customer switch
{
    { IsVIP: true } => "VIP İndirimi",
    { OrderCount: > 10 } => "Sadık Müşteri İndirimi",
    { LastOrderDate: var date } when (DateTime.Now - date).TotalDays < 30 => "Yeni Sipariş İndirimi",
    _ => "Standart fiyat"
};
```

### Tuple Pattern

Çoklu değerleri aynı anda kontrol etmek için kullanılır.

```csharp
static string GetQuadrant(Point p) => (p.X, p.Y) switch
{
    (0, 0) => "Orijin",
    (_, 0) => "X ekseni",
    (0, _) => "Y ekseni",
    (> 0, > 0) => "1. Çeyrek",
    (< 0, > 0) => "2. Çeyrek",
    (< 0, < 0) => "3. Çeyrek",
    (> 0, < 0) => "4. Çeyrek",
    _ => "Bu durum hiç oluşmaz"
};
```

### Positional Pattern (Konumsal Pattern)

Bir sınıfın pozisyonel özelliklerini kontrol etmek için kullanılır. Sınıf, bir deconstruct metodu sunmalıdır.

```csharp
public class Point
{
    public int X { get; }
    public int Y { get; }
    
    public Point(int x, int y) => (X, Y) = (x, y);
    
    public void Deconstruct(out int x, out int y) => (x, y) = (X, Y);
}

static string GetQuadrant(Point p) => p switch
{
    (0, 0) => "Orijin",
    (_, 0) => "X ekseni",
    (0, _) => "Y ekseni",
    (> 0, > 0) => "1. Çeyrek",
    (< 0, > 0) => "2. Çeyrek",
    (< 0, < 0) => "3. Çeyrek",
    (> 0, < 0) => "4. Çeyrek",
    _ => "Bu durum hiç oluşmaz"
};
```

### List Pattern

C# 11.0 ile gelmiştir. Liste ve dizilerdeki elemanları kontrol etmek için kullanılır.

```csharp
static string CheckSequence(int[] numbers) => numbers switch
{
    [] => "Boş dizi",
    [1, 2, 3] => "1,2,3 dizisi",
    [1, 2, ..] => "1, 2 ile başlayan dizi",
    [.., 98, 99] => "98, 99 ile biten dizi",
    [1, .. var middle, 100] => $"1 ile başlayıp 100 ile biten, ortası: {string.Join(", ", middle)}",
    [var first, _, _, var last] => $"4 elemanlı dizi, ilk: {first}, son: {last}",
    _ => "Hiçbir kalıba uymayan dizi"
};
```

### Switch Expression 

C# 8.0 ile gelmiştir. Daha özlü ve fonksiyonel bir switch ifadesi sunar.

```csharp
static decimal CalculateDiscount(Customer customer) => customer.Type switch
{
    CustomerType.Regular => 0.1m,
    CustomerType.Premium => 0.2m,
    CustomerType.VIP => 0.3m,
    _ => 0.0m
};

// Property pattern ile birlikte
static decimal CalculateDiscount(Customer customer) => customer switch
{
    { Type: CustomerType.Regular, OrderCount: > 10 } => 0.15m,
    { Type: CustomerType.Regular } => 0.1m,
    { Type: CustomerType.Premium, OrderCount: > 5 } => 0.25m,
    { Type: CustomerType.Premium } => 0.2m,
    { Type: CustomerType.VIP } => 0.3m,
    _ => 0.0m
};
```

## Pattern Matching Kullanım Alanları

Pattern matching aşağıdaki alanlarda özellikle faydalıdır:

1. **Polimorfik Davranış**: Farklı nesne türlerine göre farklı işlemler yapmak için.

```csharp
public static void ProcessShape(Shape shape)
{
    switch (shape)
    {
        case Circle c:
            Console.WriteLine($"Daire alanı: {Math.PI * c.Radius * c.Radius}");
            break;
        case Rectangle r:
            Console.WriteLine($"Dikdörtgen alanı: {r.Width * r.Height}");
            break;
        case Triangle t:
            Console.WriteLine($"Üçgen alanı: {t.Base * t.Height / 2}");
            break;
        default:
            Console.WriteLine("Bilinmeyen şekil türü");
            break;
    }
}
```

2. **Durum Tabanlı İşlemler**: Verilerin durumuna göre farklı işlemler yapmak için.

```csharp
public static string GetOrderStatus(Order order) => order switch
{
    { IsPaid: false } => "Ödeme bekliyor",
    { IsPaid: true, IsShipped: false } => "Hazırlanıyor",
    { IsShipped: true, IsDelivered: false } => "Kargoda",
    { IsDelivered: true } => "Teslim edildi",
    _ => "Bilinmeyen durum"
};
```

3. **Veri Dönüşümleri**: Farklı veri formatları arasında dönüşüm yapmak için.

```csharp
public static object ConvertToType(object value, Type targetType) => (value, targetType) switch
{
    (string s, Type t) when t == typeof(int) && int.TryParse(s, out _) => int.Parse(s),
    (string s, Type t) when t == typeof(DateTime) && DateTime.TryParse(s, out _) => DateTime.Parse(s),
    (int i, Type t) when t == typeof(string) => i.ToString(),
    (DateTime d, Type t) when t == typeof(string) => d.ToString("yyyy-MM-dd"),
    _ => throw new InvalidOperationException($"Cannot convert {value?.GetType().Name ?? "null"} to {targetType.Name}")
};
```

4. **Doğrulama İşlemleri**: Karmaşık koşullu doğrulamalar için.

```csharp
public static string ValidatePassword(string password) => password switch
{
    null => "Şifre boş olamaz",
    { Length: < 8 } => "Şifre en az 8 karakter olmalı",
    { Length: > 50 } => "Şifre 50 karakterden uzun olamaz",
    var p when !p.Any(char.IsDigit) => "Şifre en az bir rakam içermeli",
    var p when !p.Any(char.IsUpper) => "Şifre en az bir büyük harf içermeli",
    var p when !p.Any(char.IsLower) => "Şifre en az bir küçük harf içermeli",
    var p when !p.Any(c => !char.IsLetterOrDigit(c)) => "Şifre en az bir özel karakter içermeli",
    _ => "Geçerli şifre"
};
```

5. **Parser İşlemleri**: Metin, XML, JSON gibi formatları ayrıştırmak için.

```csharp
public static IEnumerable<Token> Tokenize(string input)
{
    var charArray = input.ToCharArray();
    for (int i = 0; i < charArray.Length; i++)
    {
        char current = charArray[i];
        yield return current switch
        {
            '(' => new Token(TokenType.OpenParen, "("),
            ')' => new Token(TokenType.CloseParen, ")"),
            '+' => new Token(TokenType.Plus, "+"),
            '-' => new Token(TokenType.Minus, "-"),
            '*' => new Token(TokenType.Multiply, "*"),
            '/' => new Token(TokenType.Divide, "/"),
            var c when char.IsDigit(c) => ProcessNumber(charArray, ref i),
            var c when char.IsWhiteSpace(c) => continue,
            _ => throw new InvalidOperationException($"Bilinmeyen token: {current}")
        };
    }
}
```

## Pattern Matching ile İlgili İyi Pratikler

1. **Karmaşık `if-else` yapıları yerine pattern matching kullanın**: Kodunuzu daha okunaklı hale getirir.

2. **En spesifik pattern'i önce yerleştirin**: Daha genel pattern'lar, daha spesifik olanların önüne geçebilir.

```csharp
// Yanlış
switch (customer)
{
    case { Type: CustomerType.Regular }:  // Daha genel
        // ...
        break;
    case { Type: CustomerType.Regular, OrderCount: > 10 }:  // Asla erişilmez!
        // ...
        break;
}

// Doğru
switch (customer)
{
    case { Type: CustomerType.Regular, OrderCount: > 10 }:  // Daha spesifik
        // ...
        break;
    case { Type: CustomerType.Regular }:  // Daha genel
        // ...
        break;
}
```

3. **Recursive pattern matching kullanın**: İç içe pattern matching, karmaşık veri yapılarını analiz etmek için güçlüdür.

```csharp
static string DescribeNode(TreeNode node) => node switch
{
    { Value: var v, Left: null, Right: null } => $"Yaprak düğüm, değer: {v}",
    { Value: var v, Left: var left, Right: null } => $"Değeri {v} olan ve sadece sol çocuğu {DescribeNode(left)} olan düğüm",
    { Value: var v, Left: null, Right: var right } => $"Değeri {v} olan ve sadece sağ çocuğu {DescribeNode(right)} olan düğüm",
    { Value: var v, Left: var left, Right: var right } => $"Değeri {v} olan, sol çocuğu {DescribeNode(left)} ve sağ çocuğu {DescribeNode(right)} olan tam düğüm",
    null => "Boş düğüm"
};
```

4. **Pattern matching'i Guard Clause'lar ile birleştirin**: Method başında hızlı kontrollerle erken dönüşler yapmak için kullanın.

```csharp
public void ProcessOrder(Order order)
{
    // Guard clause
    if (order is not { IsValid: true, Items.Count: > 0 })
    {
        throw new ArgumentException("Geçersiz sipariş");
    }
    
    // Ana işlem
    // ...
}
```

5. **Switch expression'ları discard (`_`) ile sonlandırın**: Beklenmeyen durumları yakalamak için.

```csharp
public static decimal CalculateTax(Product product, Country country) => (product, country) switch
{
    (_, { Code: "USA" }) => product.Price * 0.07m,
    ({ Category: "Electronics" }, { Code: "UK" }) => product.Price * 0.20m,
    ({ Category: "Books" }, { Code: "UK" }) => product.Price * 0.0m,
    ({ Category: var category }, { Code: var code }) => throw new NotImplementedException($"Vergi hesaplaması {category} kategorisi ve {code} ülkesi için tanımlanmamış"),
    _ => throw new ArgumentException("Geçersiz ürün veya ülke")
};
```

## Örnek Uygulamalar

### Örnek 1: Hesap Makinesi İşlemci

```csharp
public static double CalculateOperation(double left, double right, string operation) => operation switch
{
    "+" => left + right,
    "-" => left - right,
    "*" => left * right,
    "/" when right != 0 => left / right,
    "/" => throw new DivideByZeroException(),
    "%" when right != 0 => left % right,
    "%" => throw new DivideByZeroException(),
    "^" => Math.Pow(left, right),
    "log" => Math.Log(right, left),
    "max" => Math.Max(left, right),
    "min" => Math.Min(left, right),
    _ => throw new ArgumentException($"Bilinmeyen operatör: {operation}")
};
```

### Örnek 2: API Yanıt İşleyici

```csharp
public static string ProcessApiResponse(ApiResponse response) => response switch
{
    { StatusCode: 200, Data: var data } => $"Başarılı: {data}",
    { StatusCode: 400, ErrorMessage: var error } => $"Bad Request: {error}",
    { StatusCode: 401 } => "Yetkisiz erişim",
    { StatusCode: 404 } => "Kaynak bulunamadı",
    { StatusCode: >= 500 } => "Sunucu hatası",
    { StatusCode: var code } when code >= 200 && code < 300 => "Başarılı bir yanıt",
    { StatusCode: var code } when code >= 300 && code < 400 => "Yönlendirme",
    { StatusCode: var code } when code >= 400 && code < 500 => $"İstemci hatası: {code}",
    _ => "Bilinmeyen yanıt"
};
```

### Örnek 3: Hiyerarşik Veri İşleyici

```csharp
public static string ProcessNode(JsonNode node) => node switch
{
    JsonObject obj => ProcessObject(obj),
    JsonArray arr => ProcessArray(arr),
    JsonValue val when val.TryGetValue<string>(out var s) => $"String: {s}",
    JsonValue val when val.TryGetValue<int>(out var i) => $"Integer: {i}",
    JsonValue val when val.TryGetValue<bool>(out var b) => $"Boolean: {b}",
    JsonValue val when val.TryGetValue<double>(out var d) => $"Double: {d}",
    null => "Null",
    _ => "Bilinmeyen JSON düğümü"
};

private static string ProcessObject(JsonObject obj)
{
    var properties = obj.Select(kvp => $"{kvp.Key}: {ProcessNode(kvp.Value)}");
    return $"Object {{ {string.Join(", ", properties)} }}";
}

private static string ProcessArray(JsonArray arr)
{
    var items = arr.Select(item => ProcessNode(item));
    return $"Array [ {string.Join(", ", items)} ]";
}
```

### Örnek 4: Özel İşlem Zinciri

```csharp
public static IProcessor CreateProcessor(int priority, string operation) => (priority, operation) switch
{
    (>= 10, "log") => new LoggingProcessor(),
    (>= 5, "validate") => new ValidationProcessor(),
    (>= 5, "transform") => new TransformProcessor(),
    (>= 1, "cache") => new CachingProcessor(),
    (_, "audit") => new AuditProcessor(),
    (_, var op) => throw new ArgumentException($"Bilinmeyen işlem: {op}")
};
```

## Performans Değerlendirmeleri

Pattern matching, genellikle okunabilirlik ve bakım kolaylığı sağlar, ancak performans açısından bazı hususlara dikkat etmek gerekir:

1. **Karmaşık Pattern'lar**: Çok karmaşık pattern'lar, derleyici tarafından optimize edilmiş olsa da, performans açısından maliyetli olabilir. Çok yoğun işlemlerde kontrol etmeyi düşünün.

2. **Recursive Pattern Matching**: Çok derin iç içe pattern'lar, performans sorunlarına yol açabilir. Özellikle büyük veri yapılarında dikkatli kullanın.

3. **Pattern'ların Sırası**: Daha sık eşleşen pattern'ları ilk sıralara koymak, performansı artırabilir.

4. **switch expression vs switch statement**: switch expression genellikle daha özlü ve işlevseldir, ancak bazı durumlarda switch statement daha esnektir ve daha fazla kod yürütme kontrolü sağlar.

## Sonuç

C# pattern matching, modern C# programlamasının güçlü bir parçasıdır. Bu özellik, kodunuzu daha okunabilir, daha az karmaşık ve daha sürdürülebilir hale getirmenize olanak tanır. Uygun kullanıldığında, karmaşık nesne yapılarını ve koşulları elegantça işlemenize yardımcı olur.

Pattern matching, özellikle fonksiyonel programlama paradigmalarını C# kodunuza entegre etmek istediğinizde çok değerlidir. Nesneye dayalı programlama ile fonksiyonel programlama arasında bir köprü oluşturur ve C#'ın çok paradigmalı bir dil olarak gücünü artırır.

Pattern matching yeteneklerini kodunuzda kullanarak, daha temiz, daha güvenli ve daha ifade edici kod yazabilirsiniz. Her C# geliştiricisinin araç setinde bulunması gereken önemli bir yetenektir.
