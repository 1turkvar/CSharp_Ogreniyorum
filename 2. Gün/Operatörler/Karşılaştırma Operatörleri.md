# C# Karşılaştırma Operatörleri

## Temel Karşılaştırma Operatörleri

C# programlama dilinde, değerleri karşılaştırmak için kullanabileceğiniz çeşitli operatörler bulunmaktadır. Bu operatörler her zaman bir boolean değeri (`true` veya `false`) döndürür.

| Operatör | Adı | Açıklama | Örnek |
|----------|-----|----------|-------|
| `==` | Eşitlik | İki değerin eşit olup olmadığını kontrol eder | `x == y` |
| `!=` | Eşit Değil | İki değerin farklı olup olmadığını kontrol eder | `x != y` |
| `>` | Büyüktür | Sol operandın sağ operanddan büyük olup olmadığını kontrol eder | `x > y` |
| `<` | Küçüktür | Sol operandın sağ operanddan küçük olup olmadığını kontrol eder | `x < y` |
| `>=` | Büyük Eşit | Sol operandın sağ operanddan büyük veya eşit olup olmadığını kontrol eder | `x >= y` |
| `<=` | Küçük Eşit | Sol operandın sağ operanddan küçük veya eşit olup olmadığını kontrol eder | `x <= y` |

## Kullanım Örnekleri

### Temel Kullanım

```csharp
int a = 5, b = 10;

bool sonuc1 = a == b;    // false
bool sonuc2 = a != b;    // true
bool sonuc3 = a > b;     // false
bool sonuc4 = a < b;     // true
bool sonuc5 = a >= b;    // false
bool sonuc6 = a <= b;    // true

Console.WriteLine($"a == b: {sonuc1}");
Console.WriteLine($"a != b: {sonuc2}");
Console.WriteLine($"a > b: {sonuc3}");
Console.WriteLine($"a < b: {sonuc4}");
Console.WriteLine($"a >= b: {sonuc5}");
Console.WriteLine($"a <= b: {sonuc6}");
```

### if-else İfadelerinde Kullanım

```csharp
int yas = 18;

if (yas >= 18)
{
    Console.WriteLine("Reşitsiniz");
}
else
{
    Console.WriteLine("Reşit değilsiniz");
}
```

### switch İfadesinde Karşılaştırma (C# 8.0 ve sonrası)

```csharp
int puan = 85;

string basariDurumu = puan switch
{
    >= 90 => "A",
    >= 80 and < 90 => "B",
    >= 70 and < 80 => "C",
    >= 60 and < 70 => "D",
    _ => "F"
};

Console.WriteLine($"Başarı durumu: {basariDurumu}");
```

## Referans Tipler İçin Dikkat Edilmesi Gerekenler

Referans türleri (örn. sınıflar) karşılaştırılırken `==` ve `!=` operatörleri, varsayılan olarak referans eşitliğini kontrol eder, içerik eşitliğini değil. Yani iki nesnenin aynı bellek adresini gösterip göstermediğini kontrol eder.

```csharp
public class Kisi
{
    public string Ad { get; set; }
}

Kisi kisi1 = new Kisi { Ad = "Ali" };
Kisi kisi2 = new Kisi { Ad = "Ali" };
Kisi kisi3 = kisi1;

bool esitMi1 = kisi1 == kisi2;  // false (farklı nesneler)
bool esitMi2 = kisi1 == kisi3;  // true (aynı nesneye referans)
```

İçerik karşılaştırması yapmak için `Equals` metodunu override etmeniz veya özel karşılaştırma mantığı yazmanız gerekir:

```csharp
public class Kisi
{
    public string Ad { get; set; }
    
    public override bool Equals(object obj)
    {
        if (obj is Kisi digerKisi)
            return Ad == digerKisi.Ad;
        return false;
    }
    
    public override int GetHashCode()
    {
        return Ad?.GetHashCode() ?? 0;
    }
}

Kisi kisi1 = new Kisi { Ad = "Ali" };
Kisi kisi2 = new Kisi { Ad = "Ali" };

bool esitMi = kisi1.Equals(kisi2);  // true (içerik karşılaştırması)
```

## Null Karşılaştırmaları

C# 7.0 ve sonraki sürümlerde null karşılaştırmaları için bazı özel operatörler eklenmiştir:

### Null-conditional operatör (`?.`)

Null kontrolü yaparken kod tekrarını azaltır:

```csharp
// Geleneksel yöntem
string ad = null;
int? uzunluk = null;

if (ad != null)
{
    uzunluk = ad.Length;
}

// Null-conditional operatör ile
uzunluk = ad?.Length; // ad null ise, uzunluk da null olur
```

### Null-coalescing operatör (`??`)

Bir değer null ise yerine başka bir değer kullanmayı sağlar:

```csharp
string ad = null;
string gorunecekAd = ad ?? "İsimsiz"; // ad null olduğu için gorunecekAd "İsimsiz" olur
```

### Null-coalescing atama operatörü (`??=`) (C# 8.0 ve sonrası)

Bir değişken null ise ona bir değer atar:

```csharp
string ad = null;
ad ??= "İsimsiz"; // ad null olduğu için "İsimsiz" olarak atanır
Console.WriteLine(ad); // "İsimsiz" yazdırır

string soyad = "Demir";
soyad ??= "Bilinmiyor"; // soyad null olmadığı için değişmez
Console.WriteLine(soyad); // "Demir" yazdırır
```

## Pattern Matching ile Karşılaştırma (C# 7.0 ve sonrası)

C# 7.0 ve sonrası, pattern matching özellikleriyle karşılaştırmaları daha esnek hale getirmiştir:

```csharp
object deger = 5;

if (deger is int sayi)
{
    Console.WriteLine($"Değer bir tam sayıdır: {sayi}");
}

string sonuc = deger switch
{
    int n when n > 0 => "Pozitif tam sayı",
    int n when n < 0 => "Negatif tam sayı",
    int n => "Sıfır",
    string s => $"String: {s}",
    _ => "Diğer tip"
};
```

## Özet

C# karşılaştırma operatörleri, program akışını kontrol etmenin temel araçlarıdır. Farklı veri tipleriyle çalışırken dikkatli olmak gerekir, özellikle referans türleriyle çalışırken eşitlik karşılaştırması yaparken. Modern C# versiyonları (C# 7.0 ve üzeri), daha güçlü ve okunabilir karşılaştırma ifadeleri yazmanıza olanak tanıyan pattern matching, null-conditional ve null-coalescing gibi gelişmiş özelliklere sahiptir.
