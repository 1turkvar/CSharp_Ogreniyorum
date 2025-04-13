# C# İlkel Veri Tipleri

## Genel Bakış

C# programlama dilinde kullanılan ilkel veri tipleri, verilerinizi saklamak ve işlemek için kullanılan temel yapı taşlarıdır. Bu veri tipleri .NET Framework'ün bir parçası olan Common Type System (CTS) içinde tanımlanmıştır.

## Tam Sayı Veri Tipleri

| Veri Tipi | Boyut    | Aralık                                                    | .NET Karşılığı |
|-----------|----------|-----------------------------------------------------------|----------------|
| `sbyte`   | 8 bit    | -128 ile 127                                              | System.SByte   |
| `byte`    | 8 bit    | 0 ile 255                                                 | System.Byte    |
| `short`   | 16 bit   | -32,768 ile 32,767                                        | System.Int16   |
| `ushort`  | 16 bit   | 0 ile 65,535                                              | System.UInt16  |
| `int`     | 32 bit   | -2,147,483,648 ile 2,147,483,647                          | System.Int32   |
| `uint`    | 32 bit   | 0 ile 4,294,967,295                                       | System.UInt32  |
| `long`    | 64 bit   | -9,223,372,036,854,775,808 ile 9,223,372,036,854,775,807  | System.Int64   |
| `ulong`   | 64 bit   | 0 ile 18,446,744,073,709,551,615                          | System.UInt64  |

### Literal Tanımlamalar
```csharp
int hexValue = 0x1A3F;          // Hexadecimal
int binaryValue = 0b1010_1100;  // Binary (C# 7.0+)
uint unsignedValue = 123U;
long bigNumber = 123L;
ulong biggerNumber = 123UL;
float pi = 3.14F;
double precise = 3.14159D;
decimal money = 123.45M;
```

### Örnek Kullanım:

```csharp
byte b = 255;
short s = 32767;
int i = 2147483647;
long l = 9223372036854775807L; // L soneki long tipini belirtir

// İşaretli ve işaretsiz tip farkı
sbyte signedByte = -128;  // negatif değer alabilir
byte unsignedByte = 255;  // sadece pozitif değer alabilir
```

## Kayan Nokta Veri Tipleri

| Veri Tipi | Boyut    | Hassasiyet                      | .NET Karşılığı  |
|-----------|----------|----------------------------------|-----------------|
| `float`   | 32 bit   | ~1.5 × 10^−45 ile ~3.4 × 10^38  | System.Single   |
| `double`  | 64 bit   | ~5.0 × 10^−324 ile ~1.7 × 10^308| System.Double   |
| `decimal` | 128 bit  | 28-29 anlamlı basamak           | System.Decimal  |

### Örnek Kullanım:

```csharp
float f = 3.14f;      // f soneki float tipini belirtir
double d = 3.14159;   // varsayılan olarak double kabul edilir
decimal m = 3.14159m; // m soneki decimal tipini belirtir

// Hassasiyet farkı
float floatValue = 0.1f + 0.2f;            // 0.3000001 gibi bir değer
decimal decimalValue = 0.1m + 0.2m;        // tam olarak 0.3
Console.WriteLine(floatValue == 0.3f);     // False olabilir
Console.WriteLine(decimalValue == 0.3m);   // True
```

## Karakter ve Metin Veri Tipleri

| Veri Tipi | Boyut    | Açıklama                            | .NET Karşılığı  |
|-----------|----------|-------------------------------------|-----------------|
| `char`    | 16 bit   | Unicode tek karakter                | System.Char     |
| `string`  | Değişken | Unicode karakter dizisi (metin)     | System.String   |

### Örnek Kullanım:

```csharp
char c = 'A';
string s = "Merhaba Dünya";

// Unicode karakterler
char turkishChar = 'ğ';
string turkishText = "Merhaba Türkiye";
```

## Mantıksal Veri Tipi

| Veri Tipi | Boyut    | Değerler                | .NET Karşılığı  |
|-----------|----------|-------------------------|-----------------|
| `bool`    | 8 bit    | `true` veya `false`     | System.Boolean  |

### Örnek Kullanım:

```csharp
bool isActive = true;
bool hasPermission = false;

if (isActive && hasPermission)
{
    Console.WriteLine("Kullanıcı aktif ve yetkili.");
}
```

## Özel Veri Tipleri

| Veri Tipi  | Açıklama                                                | .NET Karşılığı  |
|------------|--------------------------------------------------------|-----------------|
| `object`   | Tüm tiplerden değer alabilir (referans tipi)           | System.Object   |
| `dynamic`  | Çalışma zamanında tip belirlenir                       | System.Dynamic  |

### Örnek Kullanım:

```csharp
object obj1 = 42;         // int değer
object obj2 = "Merhaba";  // string değer

dynamic dyn1 = 100;
dynamic dyn2 = "Dünya";
```

## Varsayılan Değerler

Her ilkel veri tipinin atama yapılmadığı durumlarda alacağı varsayılan değerler vardır:

| Veri Tipi    | Varsayılan Değer |
|--------------|------------------|
| Sayısal tipler (int, float vb.) | 0               |
| bool         | false            |
| char         | '\0'             |
| string       | null             |
| object       | null             |

### Örnek Kullanım:

```csharp
int defaultInt;              // 0
bool defaultBool;            // false
char defaultChar;            // '\0'
string defaultString;        // null

// Bir değişkenin varsayılan değerini almak için:
int i = default(int);        // 0
bool b = default(bool);      // false
```

## Tür Dönüşümleri

### Bilinçli (Explicit) Dönüşüm:

Veri kaybına neden olabilecek dönüşümlerde kullanılır:

```csharp
double d = 3.14;
int i = (int)d;              // 3 (ondalık kısım kaybedildi)

long l = 1000;
byte b = (byte)l;            // Veri kaybolabilir
```

### Bilinçsiz (Implicit) Dönüşüm:

Veri kaybı olmadan yapılabilen dönüşümlerdir:

```csharp
int i = 100;
long l = i;                  // int her zaman long'a sığar
float f = i;                 // int her zaman float'a sığar

char c = 'A';
int asciiValue = c;          // karakterin ASCII değeri alınır
```

### Convert Sınıfı:

```csharp
string numberText = "123";
int number = Convert.ToInt32(numberText);  // string'den int'e dönüşüm

double pi = 3.14159;
string piText = Convert.ToString(pi);      // double'dan string'e dönüşüm
```

### Parse Metodu:

```csharp
string ageText = "25";
int age = int.Parse(ageText);

string priceText = "29.99";
decimal price = decimal.Parse(priceText, CultureInfo.InvariantCulture);
```

### TryParse Metodu (Güvenli dönüşüm):

```csharp
string input = "abc";
int result;
bool success = int.TryParse(input, out result);
// success: false, result: 0
```

## Değer ve Referans Tipleri

İlkel veri tipleri çoğunlukla değer tipleridir:

- **Değer tipleri**: int, double, float, bool, char, struct
- **Referans tipleri**: string, object, class, interface, delegate

### Davranış Farkı:

```csharp
// Değer tipi örneği
int x = 10;
int y = x;  // y'ye x'in değeri kopyalanır
x = 20;     // x değişir, y etkilenmez (hala 10)

// Referans tipi örneği
string s1 = "Test";
string s2 = s1;  // s2, s1 ile aynı nesneyi işaret eder
s1 = "Yeni";     // s1 yeni bir nesneye işaret eder, s2 etkilenmez

// Referans tip olarak sınıf örneği
Person p1 = new Person { Name = "Ali" };
Person p2 = p1;  // p2, p1 ile aynı nesneyi işaret eder
p1.Name = "Veli"; // p1 üzerinden özellik değiştirilirse, p2 de etkilenir
// p2.Name şimdi "Veli" oldu
```

## Nullable Tipler

Value tipleri null değer alamazlar, ancak nullable yapılabilirler:

```csharp
int normalInt = 5;
// normalInt = null; // Hata!

int? nullableInt = 10;
nullableInt = null;  // Geçerli

bool? completed = null;
if (completed.HasValue)
{
    Console.WriteLine("İşlem durumu: " + completed.Value);
}
else
{
    Console.WriteLine("İşlem durumu belirlenmedi.");
}
```

## C# 7.0 ve Sonrası Özellikleri

### Sayı Literal Gösterimleri:

```csharp
// İkili sayı gösterimi (C# 7.0)
int binary = 0b1010_1011;  // 171

// Basamak ayırıcı (digit separator)
int million = 1_000_000;  // okuması daha kolay
decimal price = 1_299.99m;
```

### Değişken Çıkarımı (C# 10.0 ve sonrası):

```csharp
var number = 10;           // int olarak çıkarılır
var text = "Hello";        // string olarak çıkarılır
var items = new List<int>(); // List<int> olarak çıkarılır

// C# 10.0 ile gelen const olarak var kullanımı
const var constPi = 3.14;  // double olarak çıkarılır
```

## Performans Konuları

- Değer tipleri stack'te, referans tipleri heap'te saklanır
- Değer tipleri genellikle daha hızlı erişim sağlar
- Büyük struct'lar performans sorunlarına neden olabilir

```csharp
// Performans kritik bir uygulamada
int[] numbers = new int[1000000]; // Değer tipi dizi
// vs
List<int> numberList = new List<int>(1000000); // Boxing/unboxing olabilir
```

## En İyi Uygulamalar

1. Doğru veri tipini seçin:
   - Para birimi için `decimal`
   - Bilimsel hesaplamalar için `double`
   - Tam sayılar için genellikle `int` yeterlidir

2. Veri tipi sınırlarını bilin:
   ```csharp
   int maxInt = int.MaxValue;  // 2,147,483,647
   if (maxInt + 1 > 0)  // false! Overflow olur ve negatif olur
   {
       // Bu kod çalışmaz
   }
   ```

3. Doğru dönüşüm metodunu kullanın:
   - `TryParse` hata durumlarında exception fırlatmaz
   - Kritik uygulamalarda explicit dönüşümleri dikkatli yapın

4. `var` kullanımını anlaşılabilir durumlarda tercih edin:
   ```csharp
   // İyi kullanım:
   var response = GetServerResponse();
   
   // Karışıklığa neden olabilecek kullanım:
   var x = GetSomeValue();  // x'in tipi belirsiz
   ```

5. Nullable tipleri bilinçli kullanın:
   ```csharp
   int? possibleValue = GetPossibleInteger();
   int definiteValue = possibleValue ?? 0;  // null coalescing operatörü
   ```

## Özet

C# ilkel veri tipleri, programlamanın temel yapı taşlarıdır. Doğru veri tipini seçmek, hem programınızın doğru çalışması hem de performansı için kritik öneme sahiptir. Her veri tipinin bellek kullanımı, aralığı ve kullanım alanları farklıdır. Bu bilgiler ışığında uygulamanızın gereksinimlerine en uygun veri tiplerini seçmeniz önemlidir.
