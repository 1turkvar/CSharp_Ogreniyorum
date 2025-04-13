# C# Tip Dönüşümleri (Casting) Rehberi

## İçindekiler
1. [Giriş](#giriş)
2. [Örtük (Implicit) Dönüşümler](#örtük-implicit-dönüşümler)
3. [Açık (Explicit) Dönüşümler](#açık-explicit-dönüşümler)
4. [Boxing ve Unboxing](#boxing-ve-unboxing)
5. [Convert Sınıfı ile Dönüşümler](#convert-sınıfı-ile-dönüşümler)
6. [Parse ve TryParse Metodları](#parse-ve-tryparse-metodları)
7. [ToString() Metodu](#tostring-metodu)
8. [Kullanıcı Tanımlı Dönüşümler](#kullanıcı-tanımlı-dönüşümler)
9. [is ve as Operatörleri](#is-ve-as-operatörleri)
10. [Genel Öneriler ve İyi Uygulamalar](#genel-öneriler-ve-iyi-uygulamalar)
11. [Sık Karşılaşılan Hatalar](#sık-karşılaşılan-hatalar)
12. [Örnek Senaryolar](#örnek-senaryolar)

## Giriş

Tip dönüşümleri (casting), C# programlama dilinde bir veri tipini başka bir veri tipine dönüştürme işlemidir. C#'ta tip dönüşümleri güvenlik açısından kontrollü bir şekilde gerçekleştirilir ve hataları önlemek için çeşitli mekanizmalar sunar.

Tip dönüşümleri temel olarak iki ana kategoriye ayrılır:
- **Örtük (Implicit) Dönüşümler**: Veri kaybı riski olmayan, otomatik yapılan dönüşümlerdir.
- **Açık (Explicit) Dönüşümler**: Veri kaybı riski olan, programcının açıkça belirtmesi gereken dönüşümlerdir.

## Örtük (Implicit) Dönüşümler

Örtük dönüşümler, veri kaybı olmadan güvenli bir şekilde yapılabilen dönüşümlerdir. Küçük boyutlu veri tiplerinden daha büyük boyutlu veri tiplerine dönüşümler genellikle örtük olarak yapılabilir.

### Örtük Dönüşüm Örnekleri:

```csharp
byte byteValue = 10;
int intValue = byteValue;  // Örtük dönüşüm: byte -> int

int intNumber = 100;
long longNumber = intNumber;  // Örtük dönüşüm: int -> long

int intVal = 500;
double doubleVal = intVal;  // Örtük dönüşüm: int -> double

float floatValue = 10.5f;
double doubleValue = floatValue;  // Örtük dönüşüm: float -> double
```

### Örtük Dönüşüm Yapılabilen Tipler:

- sbyte → short, int, long, float, double, decimal
- byte → short, ushort, int, uint, long, ulong, float, double, decimal
- short → int, long, float, double, decimal
- ushort → int, uint, long, ulong, float, double, decimal
- int → long, float, double, decimal
- uint → long, ulong, float, double, decimal
- long → float, double, decimal
- ulong → float, double, decimal
- float → double
- char → ushort, int, uint, long, ulong, float, double, decimal

## Açık (Explicit) Dönüşümler

Açık dönüşümler, veri kaybı riski olan durumlarda programcının açıkça belirtmesi gereken dönüşümlerdir. Büyük boyutlu bir veri tipinden küçük boyutlu bir veri tipine dönüşüm yaparken açık dönüşüm kullanılmalıdır.

### Açık Dönüşüm Sözdizimi:

```csharp
(hedefTip)dönüştürülecekDeğer
```

### Açık Dönüşüm Örnekleri:

```csharp
int intValue = 100;
byte byteValue = (byte)intValue;  // Açık dönüşüm: int -> byte

double doubleValue = 15.78;
int intVal = (int)doubleValue;  // Açık dönüşüm: double -> int (ondalık kısım kaybedilir)

long longValue = 987654321;
int intValue2 = (int)longValue;  // Açık dönüşüm: long -> int (veri kaybı olabilir)
```

### Taşma (Overflow) Kontrolleri:

C#'ta taşma kontrolü için `checked` ve `unchecked` blokları kullanılabilir:

```csharp
// checked bloğu ile taşma kontrolü
checked
{
    int maxInt = int.MaxValue;
    // Aşağıdaki kod çalışma zamanında OverflowException fırlatır
    int overflow = maxInt + 1;
}

// unchecked bloğu ile taşma kontrolü devre dışı bırakılır
unchecked
{
    int maxInt = int.MaxValue;
    // Taşma olur ama exception fırlatılmaz, değer int.MinValue olur
    int overflow = maxInt + 1;
}
```

## Boxing ve Unboxing

Boxing ve unboxing, değer tiplerinin (value types) referans tiplerine (reference types) ve tersi şekilde dönüştürülmesi işlemleridir.

### Boxing:

Değer tipinden referans tipine (object) dönüştürme işlemidir:

```csharp
int intValue = 123;
object boxedValue = intValue;  // Boxing: int -> object
```

### Unboxing:

Referans tipinden (object) değer tipine dönüştürme işlemidir:

```csharp
object boxedValue = 123;
int unboxedValue = (int)boxedValue;  // Unboxing: object -> int
```

### Performans Etkisi:

Boxing ve unboxing işlemleri performans açısından maliyetlidir. Özellikle yoğun döngülerde veya performans kritik uygulamalarda mümkün olduğunca kaçınılmalıdır.

## Convert Sınıfı ile Dönüşümler

.NET Framework'ün `System.Convert` sınıfı, bir veri tipini başka bir tipe dönüştürmek için çeşitli statik metodlar sağlar.

### Convert Metodları:

```csharp
int intValue = Convert.ToInt32("123");  // String -> int
double doubleValue = Convert.ToDouble("123.45");  // String -> double
bool boolValue = Convert.ToBoolean("True");  // String -> bool
DateTime dateValue = Convert.ToDateTime("2023-10-20");  // String -> DateTime

string stringValue = Convert.ToString(123);  // int -> String
```

### Convert vs Casting:

- `Convert` sınıfı, nullable tipleri ve null değerleri daha iyi yönetir.
- `Convert` metodları yuvarlama işlemlerinde daha tutarlıdır.
- `Convert.ToInt32(double)` yuvarlama yaparken, `(int)double` kesme işlemi yapar.

```csharp
double d = 10.7;
int i1 = (int)d;  // i1 = 10 (kesme işlemi)
int i2 = Convert.ToInt32(d);  // i2 = 11 (yuvarlama işlemi)
```

## Parse ve TryParse Metodları

Parse ve TryParse metodları, string değerleri diğer tiplere dönüştürmek için kullanılır.

### Parse Metodları:

```csharp
int intValue = int.Parse("123");
double doubleValue = double.Parse("123.45");
bool boolValue = bool.Parse("true");
DateTime dateValue = DateTime.Parse("2023-10-20");
```

### TryParse Metodları:

TryParse metodu dönüşümün başarılı olup olmadığını bir bool değer döndürür ve başarılıysa dönüştürülen değeri out parametresi ile verir:

```csharp
if (int.TryParse("123", out int result))
{
    Console.WriteLine($"Dönüşüm başarılı: {result}");
}
else
{
    Console.WriteLine("Dönüşüm başarısız");
}
```

### Parse vs Convert:

- `Parse` sadece stringden belirli tipe dönüşüm yapar.
- `Convert` sınıfı çok daha fazla tip arası dönüşümü destekler.
- `Parse` null değerlerinde exception fırlatır, `Convert` ise default değer döndürür.

## ToString() Metodu

C#'taki her sınıf, `Object` sınıfından miras aldığı `ToString()` metoduna sahiptir. Bu metod, nesneyi string temsiline dönüştürür.

### ToString() Kullanımı:

```csharp
int intValue = 123;
string stringValue = intValue.ToString();  // "123"

double doubleValue = 123.45;
string stringValue2 = doubleValue.ToString();  // "123.45"

// Formatlama ile ToString kullanımı
double price = 123.45;
string formattedPrice = price.ToString("C");  // "₺123,45" (para birimi formatı)
```

### Format Belirteçleri:

- `"C"` veya `"c"`: Para birimi formatı
- `"D"` veya `"d"`: Ondalık format (sadece tam sayılar için)
- `"E"` veya `"e"`: Bilimsel format
- `"F"` veya `"f"`: Sabit nokta formatı
- `"G"` veya `"g"`: Genel format
- `"N"` veya `"n"`: Sayı formatı (bin ayırıcılarıyla)
- `"P"` veya `"p"`: Yüzde formatı
- `"X"` veya `"x"`: Onaltılık format

## Kullanıcı Tanımlı Dönüşümler

C#'ta kendi sınıflarınız için özel dönüşüm operatörlerini tanımlayabilirsiniz.

### Örtük Dönüşüm Operatörü Tanımlama:

```csharp
public class Meter
{
    public double Value { get; }

    public Meter(double value)
    {
        Value = value;
    }

    // Örtük dönüşüm: double -> Meter
    public static implicit operator Meter(double value)
    {
        return new Meter(value);
    }

    // Örtük dönüşüm: Meter -> double
    public static implicit operator double(Meter meter)
    {
        return meter.Value;
    }
}

// Kullanımı:
Meter length = 5.0;  // double -> Meter örtük dönüşümü
double value = length;  // Meter -> double örtük dönüşümü
```

### Açık Dönüşüm Operatörü Tanımlama:

```csharp
public class Centimeter
{
    public double Value { get; }

    public Centimeter(double value)
    {
        Value = value;
    }
}

public class Meter
{
    public double Value { get; }

    public Meter(double value)
    {
        Value = value;
    }

    // Açık dönüşüm: Meter -> Centimeter
    public static explicit operator Centimeter(Meter meter)
    {
        return new Centimeter(meter.Value * 100);
    }

    // Açık dönüşüm: Centimeter -> Meter
    public static explicit operator Meter(Centimeter cm)
    {
        return new Meter(cm.Value / 100);
    }
}

// Kullanımı:
Meter m = new Meter(1.5);
Centimeter cm = (Centimeter)m;  // Açık dönüşüm
```

## is ve as Operatörleri

C#'ta `is` ve `as` operatörleri, referans tiplerinin ve boxing edilmiş değer tiplerinin tip kontrolü ve dönüşümü için kullanılır.

### is Operatörü:

`is` operatörü, bir nesnenin belirli bir tipe uyup uymadığını kontrol eder:

```csharp
object obj = "Hello";

if (obj is string)
{
    Console.WriteLine("obj bir string'dir");
}

// C# 7.0+ ile Pattern Matching kullanımı:
if (obj is string str)
{
    Console.WriteLine($"obj bir string'dir: {str}");
}
```

### as Operatörü:

`as` operatörü, bir referans tipini başka bir referans tipine dönüştürür. Dönüşüm başarısız olursa null döndürür:

```csharp
object obj = "Hello";

string str = obj as string;  // Başarılı: str = "Hello"
DateTime? date = obj as DateTime?;  // Başarısız: date = null
```

### is vs as vs Explicit Cast:

```csharp
object obj = "Hello";

// Yaklaşım 1: Explicit cast
try
{
    string str1 = (string)obj;  // Exception riski var
}
catch (InvalidCastException)
{
    // Dönüşüm başarısız
}

// Yaklaşım 2: is kontrolü + explicit cast
if (obj is string)
{
    string str2 = (string)obj;  // Güvenli, çünkü önceden tip kontrolü yapıldı
}

// Yaklaşım 3: as operatörü + null kontrolü
string str3 = obj as string;
if (str3 != null)
{
    // str3 ile işlem yap
}

// Yaklaşım 4: C# 7.0+ Pattern Matching
if (obj is string str4)
{
    // str4 ile işlem yap
}
```

## Genel Öneriler ve İyi Uygulamalar

### Güvenli Dönüşümler için Öneriler:

1. **TryParse Tercih Edin**: Parse yerine TryParse kullanmak, exception yönetimi açısından daha verimlidir.

```csharp
// Önerilmeyen:
try
{
    int value = int.Parse(inputStr);
}
catch (FormatException)
{
    // Hata yönetimi
}

// Önerilen:
if (int.TryParse(inputStr, out int value))
{
    // Başarılı
}
else
{
    // Başarısız
}
```

2. **Veri Kaybını Önleyin**: Büyük değer aralığından küçüğe dönüşümlerde dikkatli olun.

```csharp
long bigValue = long.MaxValue;

// Tehlikeli:
int smallValue = (int)bigValue;  // Veri kaybı!

// Daha güvenli:
if (bigValue <= int.MaxValue && bigValue >= int.MinValue)
{
    int safeValue = (int)bigValue;
}
else
{
    // Değer int aralığının dışında
}
```

3. **Kültür Farklılıklarına Dikkat Edin**: Özellikle ondalık sayılarda kültüre özgü formatları göz önünde bulundurun.

```csharp
// Kültürden bağımsız dönüşüm:
double value = double.Parse("123.45", CultureInfo.InvariantCulture);

// Belirli bir kültüre göre dönüşüm:
double valueDE = double.Parse("123,45", new CultureInfo("de-DE"));
```

## Sık Karşılaşılan Hatalar

### 1. InvalidCastException

Uygunsuz tip dönüşümlerinde ortaya çıkar:

```csharp
object obj = "123";
int i = (int)obj;  // InvalidCastException: string doğrudan int'e dönüştürülemez
```

Çözüm:
```csharp
object obj = "123";
int i = Convert.ToInt32(obj);  // veya int.Parse((string)obj);
```

### 2. FormatException

Uygun olmayan string formatlarında ortaya çıkar:

```csharp
string input = "abc";
int value = int.Parse(input);  // FormatException: Sayısal olmayan string
```

Çözüm:
```csharp
string input = "abc";
if (int.TryParse(input, out int value))
{
    // Başarılı dönüşüm
}
else
{
    // Başarısız dönüşüm
}
```

### 3. OverflowException

Hedef tipin kapasitesini aşan değer dönüşümlerinde ortaya çıkar:

```csharp
int largeValue = int.MaxValue;
byte smallValue = checked((byte)largeValue);  // OverflowException
```

Çözüm:
```csharp
int largeValue = int.MaxValue;
if (largeValue <= byte.MaxValue && largeValue >= byte.MinValue)
{
    byte smallValue = (byte)largeValue;
}
else
{
    // Değer byte aralığının dışında
}
```

## Örnek Senaryolar

### Senaryo 1: Kullanıcı Girdisini İşleme

```csharp
Console.Write("Lütfen yaşınızı girin: ");
string input = Console.ReadLine();

if (int.TryParse(input, out int age))
{
    if (age >= 0 && age <= 120)
    {
        Console.WriteLine($"Girilen yaş: {age}");
    }
    else
    {
        Console.WriteLine("Geçerli bir yaş aralığı giriniz (0-120).");
    }
}
else
{
    Console.WriteLine("Geçerli bir sayı girmediniz.");
}
```

### Senaryo 2: Farklı Sayı Sistemleri Arası Dönüşüm

```csharp
// Onluk tabandan ikili tabana dönüşüm
int decimalValue = 42;
string binaryString = Convert.ToString(decimalValue, 2);  // "101010"

// İkili tabandan onluk tabana dönüşüm
string binaryInput = "101010";
int decimalResult = Convert.ToInt32(binaryInput, 2);  // 42

// Onaltılık tabandan onluk tabana dönüşüm
string hexInput = "2A";
int decimalFromHex = Convert.ToInt32(hexInput, 16);  // 42
```

### Senaryo 3: JSON Veri Dönüşümü

```csharp
using System.Text.Json;

// Sınıf tanımı
public class Person
{
    public string Name { get; set; }
    public int Age { get; set; }
}

// Nesne oluşturma
Person person = new Person { Name = "Ahmet", Age = 30 };

// Nesne -> JSON string dönüşümü
string jsonString = JsonSerializer.Serialize(person);
// {"Name":"Ahmet","Age":30}

// JSON string -> Nesne dönüşümü
Person deserializedPerson = JsonSerializer.Deserialize<Person>(jsonString);
```

### Senaryo 4: Enum Dönüşümleri

```csharp
public enum DayOfWeek
{
    Monday,
    Tuesday,
    Wednesday,
    Thursday,
    Friday,
    Saturday,
    Sunday
}

// String -> Enum dönüşümü
string dayName = "Monday";
DayOfWeek day = (DayOfWeek)Enum.Parse(typeof(DayOfWeek), dayName);

// Int -> Enum dönüşümü
int dayValue = 1;
DayOfWeek dayFromInt = (DayOfWeek)dayValue;

// Enum -> String dönüşümü
string dayString = day.ToString();

// Enum -> Int dönüşümü
int dayNumber = (int)day;
```

C# programlama dilinde tip dönüşümleri, güvenli ve etkin kod yazmanın önemli bir parçasıdır. Doğru dönüşüm yöntemini seçmek, uygulamanızın performansını ve güvenliğini doğrudan etkiler. Bu rehberi kullanarak C#'ta tip dönüşümlerini daha etkin bir şekilde kullanabilirsiniz.
