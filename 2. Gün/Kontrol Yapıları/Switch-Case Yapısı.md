# C# Switch-Case Yapısı Detaylı Rehber

## İçindekiler
1. [Giriş](#giriş)
2. [Temel Sözdizimi](#temel-sözdizimi)
3. [Basit Örnek](#basit-örnek)
4. [Case Düşüşü (Case Fall-Through)](#case-düşüşü-case-fall-through)
5. [Çoklu Case Değerleri](#çoklu-case-değerleri)
6. [Pattern Matching (C# 7.0+)](#pattern-matching-c-70)
7. [Switch Expression (C# 8.0+)](#switch-expression-c-80)
8. [When Koşulları](#when-koşulları)
9. [Tuple Pattern Matching](#tuple-pattern-matching)
10. [Property Pattern](#property-pattern)
11. [İç İçe Switch İfadeleri](#i̇ç-i̇çe-switch-i̇fadeleri)
12. [Switch vs If-Else](#switch-vs-if-else)
13. [İyi Uygulama Önerileri](#i̇yi-uygulama-önerileri)
14. [Özet](#özet)

## Giriş

Switch-case yapısı, bir değişkenin değerini birden çok sabit değerle karşılaştırmak ve eşleşen değere göre farklı kod bloklarını çalıştırmak için kullanılan bir kontrol akış ifadesidir. Bu yapı, özellikle bir değişkenin birçok olası değeri olduğunda if-else yapısına göre daha okunaklı ve yönetilebilir kod yazmanıza olanak tanır.

## Temel Sözdizimi

```csharp
switch (kontrol_değişkeni)
{
    case değer1:
        // Kontrol değişkeni değer1'e eşitse çalıştırılacak kodlar
        break;
    case değer2:
        // Kontrol değişkeni değer2'ye eşitse çalıştırılacak kodlar
        break;
    // Diğer case ifadeleri...
    default:
        // Hiçbir case ifadesi eşleşmezse çalıştırılacak kodlar
        break;
}
```

- **kontrol_değişkeni**: Değeri kontrol edilecek değişken veya ifade
- **case değer**: Kontrol değişkeninin karşılaştırılacağı sabit değer
- **break**: Case bloğunun sonunu belirtir ve switch ifadesinden çıkılmasını sağlar
- **default**: Hiçbir case değeri eşleşmezse çalıştırılacak blok (opsiyonel)

## Basit Örnek

Haftanın günlerini sayısal değerlerle eşleştiren basit bir switch-case örneği:

```csharp
int gunNumarasi = 3;
string gun;

switch (gunNumarasi)
{
    case 1:
        gun = "Pazartesi";
        break;
    case 2:
        gun = "Salı";
        break;
    case 3:
        gun = "Çarşamba";
        break;
    case 4:
        gun = "Perşembe";
        break;
    case 5:
        gun = "Cuma";
        break;
    case 6:
        gun = "Cumartesi";
        break;
    case 7:
        gun = "Pazar";
        break;
    default:
        gun = "Geçersiz gün numarası";
        break;
}

Console.WriteLine($"Bugün günlerden {gun}");
```

## Case Düşüşü (Case Fall-Through)

C#'ta her case bloğunun sonunda `break`, `return`, `throw` gibi bir ifade bulunmalıdır. Aksi takdirde derleme hatası alınır. Bu, C ve C++ gibi dillerde bulunan ve hatalara yol açabilen "case fall-through" (bir case'den diğerine düşme) davranışını önler.

**NOT:** C#'ta istenmeyen "case fall-through" durumlarını engellemek için her case bloğunun sonunda `break` kullanımı zorunludur.

## Çoklu Case Değerleri

C# 7.0 ve sonraki sürümlerde, birden fazla durumu aynı kod bloğuyla ilişkilendirmek için virgülle ayrılmış değerler kullanabilirsiniz:

```csharp
int ay = 2;
string mevsim;

switch (ay)
{
    case 12:
    case 1:
    case 2:
        mevsim = "Kış";
        break;
    case 3:
    case 4:
    case 5:
        mevsim = "İlkbahar";
        break;
    case 6:
    case 7:
    case 8:
        mevsim = "Yaz";
        break;
    case 9:
    case 10:
    case 11:
        mevsim = "Sonbahar";
        break;
    default:
        mevsim = "Geçersiz ay";
        break;
}

Console.WriteLine($"Bu ay {mevsim} mevsimidir.");
```

## Pattern Matching (C# 7.0+)

C# 7.0 ile birlikte, switch ifadelerinde desen eşleştirme (pattern matching) kullanılabilir. Bu, farklı veri tipleri ve koşulları işlemenize olanak tanır:

```csharp
object nesne = "Merhaba";

switch (nesne)
{
    case int i:
        Console.WriteLine($"Bu bir tamsayıdır: {i}");
        break;
    case string s:
        Console.WriteLine($"Bu bir metindir: {s}");
        break;
    case bool b when b == true:
        Console.WriteLine("Bu doğru bir boolean değeridir");
        break;
    case null:
        Console.WriteLine("Bu bir null değeridir");
        break;
    default:
        Console.WriteLine("Tanımlanamayan bir tip");
        break;
}
```

## Switch Expression (C# 8.0+)

C# 8.0 ile switch ifadeleri, daha kısa ve öz bir sözdizimi ile kullanılabilir:

```csharp
int gunNumarasi = 3;

string gun = gunNumarasi switch
{
    1 => "Pazartesi",
    2 => "Salı",
    3 => "Çarşamba",
    4 => "Perşembe",
    5 => "Cuma",
    6 => "Cumartesi",
    7 => "Pazar",
    _ => "Geçersiz gün numarası"  // _ işareti default case'i temsil eder
};

Console.WriteLine($"Bugün günlerden {gun}");
```

## When Koşulları

C# 7.0 ve sonraki sürümlerde, `when` anahtar kelimesi ile case ifadelerinde ek koşullar tanımlayabilirsiniz:

```csharp
int sayi = 10;

switch (sayi)
{
    case int n when n < 0:
        Console.WriteLine("Negatif sayı");
        break;
    case int n when n == 0:
        Console.WriteLine("Sıfır");
        break;
    case int n when n > 0 && n <= 10:
        Console.WriteLine("1 ile 10 arasında pozitif sayı");
        break;
    case int n when n > 10:
        Console.WriteLine("10'dan büyük pozitif sayı");
        break;
}
```

## Tuple Pattern Matching

C# 8.0 ve sonraki sürümlerde, switch ifadeleri ile tuple desenleri kullanabilirsiniz:

```csharp
var nokta = (5, 0);

string pozisyon = nokta switch
{
    (0, 0) => "Orijin",
    (var x, 0) => $"X ekseninde {x} noktasında",
    (0, var y) => $"Y ekseninde {y} noktasında",
    (var x, var y) when x == y => $"X=Y çizgisinde ({x},{y}) noktasında",
    (var x, var y) => $"({x},{y}) noktasında"
};

Console.WriteLine(pozisyon);
```

## Property Pattern

C# 8.0 ve sonraki sürümlerde, nesne özelliklerini doğrudan switch ifadelerinde kontrol edebilirsiniz:

```csharp
class Kullanici
{
    public string Isim { get; set; }
    public string Rol { get; set; }
    public int Yas { get; set; }
}

// Kullanım
Kullanici kullanici = new Kullanici { Isim = "Ahmet", Rol = "Admin", Yas = 30 };

string mesaj = kullanici switch
{
    { Rol: "Admin" } => "Yönetici paneline hoşgeldiniz",
    { Rol: "Kullanici", Yas: >= 18 } => "Kullanıcı paneline hoşgeldiniz",
    { Rol: "Kullanici", Yas: < 18 } => "Yaşınız yeterli değil",
    _ => "Bilinmeyen rol"
};

Console.WriteLine(mesaj);
```

## İç İçe Switch İfadeleri

Switch ifadeleri iç içe kullanılabilir, ancak bu genellikle kodun karmaşıklığını artırır ve okunabilirliği azaltır:

```csharp
char islem = '+';
int sayi1 = 10, sayi2 = 5;

switch (islem)
{
    case '+':
        Console.WriteLine($"Toplam: {sayi1 + sayi2}");
        break;
    case '-':
        Console.WriteLine($"Fark: {sayi1 - sayi2}");
        break;
    case '*':
    case 'x':
        Console.WriteLine($"Çarpım: {sayi1 * sayi2}");
        break;
    case '/':
        switch (sayi2)
        {
            case 0:
                Console.WriteLine("Sıfıra bölme hatası!");
                break;
            default:
                Console.WriteLine($"Bölüm: {sayi1 / sayi2}");
                break;
        }
        break;
    default:
        Console.WriteLine("Bilinmeyen işlem");
        break;
}
```

## Switch vs If-Else

**Switch kullanımının avantajları:**
- Bir değişkenin birçok olası değerini kontrol ederken daha okunaklı kod
- Derleyici optimizasyonları (özellikle sabit değerlerle karşılaştırma yapıldığında)
- Pattern matching özellikleri ile zengin kontrol imkanları

**If-else kullanımının avantajları:**
- Karmaşık veya aralık tabanlı koşullar için daha uygun
- Farklı değişkenlerin karşılaştırılması gerektiğinde daha pratik
- Eski C# sürümlerinde daha fazla esneklik

## İyi Uygulama Önerileri

1. **Default case kullanın:** Beklenmeyen değerlerle karşılaşma durumunda nasıl davranılacağını belirtin.

2. **Case'leri mantıklı sıralayın:** En sık karşılaşılan durumları üste koyarak performansı artırabilirsiniz.

3. **Pattern matching'i uygun yerde kullanın:** C# 7.0 ve sonraki sürümlerde, switch ifadelerinin gücünden yararlanın.

4. **Aşırı iç içe switch kullanmaktan kaçının:** Kodun okunabilirliğini azaltır ve bakımını zorlaştırır.

5. **Magic number'lardan kaçının:** Sabit değerler yerine açıklayıcı isimli sabitler veya enum'lar kullanın.

**Enum ile kullanım örneği:**

```csharp
enum HaftaninGunleri
{
    Pazartesi = 1,
    Sali,
    Carsamba,
    Persembe,
    Cuma,
    Cumartesi,
    Pazar
}

HaftaninGunleri gun = HaftaninGunleri.Carsamba;

switch (gun)
{
    case HaftaninGunleri.Pazartesi:
        Console.WriteLine("Haftanın ilk iş günü");
        break;
    case HaftaninGunleri.Carsamba:
        Console.WriteLine("Haftanın ortası");
        break;
    case HaftaninGunleri.Cuma:
        Console.WriteLine("Hafta sonu yaklaşıyor");
        break;
    case HaftaninGunleri.Cumartesi:
    case HaftaninGunleri.Pazar:
        Console.WriteLine("Hafta sonu");
        break;
    default:
        Console.WriteLine("Normal bir iş günü");
        break;
}
```

## Özet

C# switch-case yapısı, bir değişkenin birden çok olası değerini kontrol etmek ve her durum için farklı kod bloklarını çalıştırmak için güçlü bir kontrol akış mekanizmasıdır. C# dilinin gelişen sürümleriyle birlikte pattern matching, switch expression ve tuple pattern gibi modern özellikler eklenmiş, bu da switch-case yapısının kullanım alanlarını ve ifade gücünü önemli ölçüde artırmıştır.

C# 7.0 ve sonraki sürümlerdeki özelliklerle birlikte switch-case yapısı sadece eşitlik karşılaştırması yapmakla kalmayıp, tip kontrolü, özellik eşleştirme ve koşullu ifadeler gibi karmaşık durumları da eleganca ifade edebilecek bir yapıya dönüşmüştür.

İyi kodlama alışkanlıkları ile birlikte kullanıldığında, switch-case yapısı kodunuzu daha okunaklı, bakımı kolay ve hata olasılığı düşük hale getirebilir.
