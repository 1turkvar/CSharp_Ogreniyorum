# C# Değişken Tanımlama Kuralları

C# programlama dilinde değişkenler, verileri geçici olarak depolamak için kullanılan isimlendirilmiş bellek alanlarıdır. Doğru ve etkili değişken tanımlamaları yapmak, kodunuzun okunabilirliğini ve bakımını kolaylaştırır. Bu dokümanda C# değişken tanımlama kurallarını detaylı bir şekilde ele alacağız.

## İçindekiler
1. [Temel Değişken Tanımlama Sözdizimi](#temel-değişken-tanımlama-sözdizimi)
2. [Değişken İsimlendirme Kuralları](#değişken-isimlendirme-kuralları)
3. [C# Veri Tipleri](#c-veri-tipleri)
4. [Değişken Tanımlama ve İlk Değer Atama](#değişken-tanımlama-ve-ilk-değer-atama)
5. [Sabitler](#sabitler)
6. [İmplicit ve Explicit Tip Dönüşümleri](#implicit-ve-explicit-tip-dönüşümleri)
7. [var Anahtar Kelimesi](#var-anahtar-kelimesi)
8. [Değişkenlerin Kapsamı (Scope)](#değişkenlerin-kapsamı-scope)
9. [Değişken Değiştiricileri (Modifiers)](#değişken-değiştiricileri-modifiers)
10. [İyi Uygulama Örnekleri](#i̇yi-uygulama-örnekleri)

## Temel Değişken Tanımlama Sözdizimi

C#'ta değişken tanımlamak için kullanılan temel sözdizimi:

```csharp
<veri_tipi> <değişken_adı> [= <başlangıç_değeri>];
```

Örnek:
```csharp
int sayi = 10;
string isim = "Ahmet";
double fiyat;
```

## Değişken İsimlendirme Kuralları

C#'ta değişken isimlendirmede uyulması gereken kurallar:

### Zorunlu Kurallar:
1. Değişken isimleri bir harf, alt çizgi (_) veya @ işareti ile başlamalıdır
2. Değişken isimlerinde sayılar kullanılabilir, ancak ilk karakter olamaz
3. Değişken isimlerinde boşluk ve özel karakterler (!, +, -, *, /, vb.) kullanılamaz
4. Değişken isimleri C# anahtar kelimeleri (keywords) olamaz (örn: `int`, `if`, `class`)
5. Değişken isimleri büyük/küçük harf duyarlıdır (`sayi` ve `Sayi` farklı değişkenlerdir)

### @ Öneki:
C# anahtar kelimelerini değişken ismi olarak kullanmak için @ öneki kullanılabilir:

```csharp
int @int = 5; // "int" anahtar kelimesi @ öneki ile değişken adı olarak kullanılmıştır
string @class = "Matematik"; // "class" anahtar kelimesi değişken adı olarak kullanılmıştır
```

### İsimlendirme Kuralları (Conventions):
1. **Yerel değişkenler:** camelCase kullanılır (ilk kelime küçük, sonrakiler büyük harfle başlar)
   ```csharp
   int ogrenciSayisi = 25;
   string kullaniciAdi = "admin";
   ```

2. **Sınıf üyeleri:** PascalCase kullanılır (tüm kelimeler büyük harfle başlar)
   ```csharp
   public class Ogrenci
   {
       private string _ad; // private alanlarda alt çizgi (_) öneki kullanılır
       public string Ad { get; set; }
       public int Yas { get; set; }
   }
   ```

3. Kısaltmalar için 2 karakterli kısaltmalarda her iki harf büyük, daha uzun kısaltmalarda sadece ilk harf büyük yazılır:
   ```csharp
   string xmlDokumanı;
   HttpResponse cevap;
   ```

## C# Veri Tipleri

C#'ta temel veri tipleri şunlardır:

### Sayısal Veri Tipleri:

| Veri Tipi | Boyut (Byte) | Aralık | Kullanım Örneği |
|-----------|--------------|--------|-----------------|
| `byte`    | 1            | 0 ile 255 | `byte yas = 25;` |
| `sbyte`   | 1            | -128 ile 127 | `sbyte sicaklik = -10;` |
| `short`   | 2            | -32,768 ile 32,767 | `short urunKodu = 1523;` |
| `ushort`  | 2            | 0 ile 65,535 | `ushort mesafe = 5000;` |
| `int`     | 4            | -2,147,483,648 ile 2,147,483,647 | `int nufus = 85000000;` |
| `uint`    | 4            | 0 ile 4,294,967,295 | `uint pozitifSayi = 100000;` |
| `long`    | 8            | -9,223,372,036,854,775,808 ile 9,223,372,036,854,775,807 | `long buyukSayi = 9223372036854775800;` |
| `ulong`   | 8            | 0 ile 18,446,744,073,709,551,615 | `ulong cokBuyukSayi = 18446744073709551615;` |

### Kayan Noktalı Sayı Tipleri:

| Veri Tipi | Boyut (Byte) | Hassasiyet | Kullanım Örneği |
|-----------|--------------|------------|-----------------|
| `float`   | 4            | ~7 basamak | `float oran = 0.5f;` |
| `double`  | 8            | ~15-16 basamak | `double pi = 3.14159265359;` |
| `decimal` | 16           | 28-29 anlamlı basamak | `decimal para = 1250.75m;` |

### Diğer Veri Tipleri:

| Veri Tipi | Açıklama | Kullanım Örneği |
|-----------|----------|-----------------|
| `char`    | 16-bit Unicode karakter | `char harf = 'A';` |
| `string`  | Unicode karakterlerden oluşan metin | `string mesaj = "Merhaba Dünya!";` |
| `bool`    | Mantıksal değer (true/false) | `bool aktifMi = true;` |
| `object`  | Tüm tiplerin temel sınıfı | `object nesne = 42;` |
| `DateTime`| Tarih ve saat | `DateTime simdi = DateTime.Now;` |

## Değişken Tanımlama ve İlk Değer Atama

### İlk Değer Atama (Initialization):

```csharp
int sayi = 5; // Tanımlama ve ilk değer atama bir arada
string ad = "Ali";
bool kontrol = false;
```

### Çoklu Değişken Tanımlama:

```csharp
// Aynı tipte birden fazla değişken tanımlama
int x = 1, y = 2, z = 3;

// Aynı tipte değişkenleri ayrı satırlarda tanımlama
int a;
int b;
int c;
```

### Default Değerler:

Yerel değişkenler kullanılmadan önce mutlaka değer ataması yapılmalıdır. Sınıf üye değişkenleri (fields) için ise varsayılan değerler otomatik atanır:

| Veri Tipi | Varsayılan Değer |
|-----------|------------------|
| Sayısal tipler (`int`, `float` vb.) | `0` |
| `bool` | `false` |
| `char` | `'\0'` (null karakter) |
| Referans tipleri (`string`, sınıflar) | `null` |

## Sabitler

Sabit değerler `const` anahtar kelimesi ile tanımlanır ve değerleri değiştirilemez:

```csharp
const double PI = 3.14159265359;
const string FIRMA_ADI = "ABC Yazılım";
const int MAKSIMUM_KULLANICI = 100;
```

Sabit tanımlamada dikkat edilmesi gerekenler:
- Tanımlama anında değer ataması zorunludur
- Sadece derleme zamanında bilinen değerler atanabilir
- Değişkenler ve metot dönüş değerleri sabit olarak tanımlanamaz

## İmplicit ve Explicit Tip Dönüşümleri

### İmplicit (Otomatik) Dönüşüm:

Veri kaybı riski olmayan dönüşümler C# tarafından otomatik yapılır:

```csharp
int tamsayi = 100;
long buyukSayi = tamsayi; // int -> long otomatik dönüşüm

float ondalik = 10.5f;
double buyukOndalik = ondalik; // float -> double otomatik dönüşüm
```

### Explicit (Elle) Dönüşüm:

Veri kaybı riski olan dönüşümlerde cast operatörü kullanılmalıdır:

```csharp
double ondalik = 10.75;
int tamsayi = (int)ondalik; // double -> int dönüşümü, ondalık kısım kaybedilir (tamsayi = 10)

long buyukSayi = 1000;
int kucukSayi = (int)buyukSayi; // long -> int dönüşümü, değer aralık içindeyse sorun olmaz
```

### Convert Sınıfı:

```csharp
string sayiMetni = "123";
int sayi = Convert.ToInt32(sayiMetni);

double ondalik = 45.67;
string metin = Convert.ToString(ondalik);
```

### Parse ve TryParse Metotları:

```csharp
string deger = "123";
int sayi = int.Parse(deger);

string ondalikDeger = "12.45";
double ondalik = double.Parse(ondalikDeger, CultureInfo.InvariantCulture);

// Hata kontrolü ile dönüşüm
if (int.TryParse(deger, out int sonuc))
{
    // Dönüşüm başarılı, sonuc değişkeni kullanılabilir
}
else
{
    // Dönüşüm başarısız
}
```

## var Anahtar Kelimesi

`var` anahtar kelimesi, değişken tipinin derleyici tarafından otomatik belirlenmesini sağlar:

```csharp
var sayi = 10; // int tipinde
var isim = "Ayşe"; // string tipinde
var liste = new List<int>(); // List<int> tipinde
var tarih = DateTime.Now; // DateTime tipinde
```

`var` kullanımında dikkat edilmesi gerekenler:
- Değişkene mutlaka ilk değer ataması yapılmalıdır
- Atanan değerin tipi derleme zamanında kesin olarak belirlenmelidir
- `var` ile tanımlanan değişkenin tipi sonradan değiştirilemez
- `var` anahtar kelimesi sınıf üyesi olarak kullanılamaz

## Değişkenlerin Kapsamı (Scope)

C#'ta değişkenlerin kapsamı tanımlandıkları blok ile sınırlıdır:

### Yerel Değişkenler (Local Variables):
```csharp
void Metot()
{
    int x = 10; // Metot içinde tanımlanan yerel değişken
    
    if (x > 5)
    {
        int y = 20; // if bloğu içinde tanımlanan yerel değişken
        // Burada hem x hem de y kullanılabilir
    }
    
    // Burada sadece x kullanılabilir, y kullanılamaz
}
```

### Sınıf Üye Değişkenleri (Fields):
```csharp
class Ogrenci
{
    private int ogrenciNo; // Sınıf üye değişkeni (field)
    private string ad; // Sınıf üye değişkeni (field)
    
    public void SetBilgiler(int no, string isim)
    {
        ogrenciNo = no;
        ad = isim;
    }
}
```

### Statik Değişkenler:
```csharp
class Uygulama
{
    public static int KullaniciSayisi = 0; // Tüm sınıf örnekleri için ortak değişken
    
    public Uygulama()
    {
        KullaniciSayisi++; // Her örnek oluşturulduğunda sayı artar
    }
}
```

## Değişken Değiştiricileri (Modifiers)

### Erişim Belirleyicileri:
- `public`: Her yerden erişilebilir
- `private`: Sadece tanımlandığı sınıf içinden erişilebilir
- `protected`: Tanımlandığı sınıf ve ondan türetilen sınıflardan erişilebilir
- `internal`: Aynı assembly (proje) içinden erişilebilir
- `protected internal`: Aynı assembly içinden veya türetilen sınıflardan erişilebilir

### Diğer Değiştiriciler:
- `static`: Sınıfın tüm örnekleri için ortak, sınıf adıyla erişilir
- `readonly`: Sadece tanımlanırken veya constructor içinde değer atanabilir
- `const`: Derleme zamanında sabit değer
- `volatile`: Çoklu iş parçacığı ortamlarında kullanılır
- `event`: Olay tanımlamak için kullanılır

Örnek:
```csharp
public class Personel
{
    public string Ad { get; set; } // Her yerden erişilebilir
    private int yas; // Sadece sınıf içinden erişilebilir
    protected string departman; // Bu sınıftan türetilen sınıflar erişebilir
    internal double maas; // Aynı proje içinden erişilebilir
    
    public static int PersonelSayisi; // Statik değişken
    public readonly int PersonelId; // Salt okunur değişken
    public const string FIRMA = "ABC Şirketi"; // Sabit değer
    
    public Personel(int id)
    {
        PersonelId = id; // readonly değişkene constructor içinde değer atanabilir
        PersonelSayisi++; // Statik değişken her nesne oluşturulduğunda artar
    }
}
```

## İyi Uygulama Örnekleri

### 1. Anlamlı İsimlendirme:
```csharp
// Kötü örnek
int a = 5;
double b = 10.5;

// İyi örnek
int ogrenciSayisi = 5;
double kilometre = 10.5;
```

### 2. Değişken Sayısını Sınırlama:
Bir metot veya blok içinde çok fazla değişken kullanmaktan kaçının. Gerekirse kodunuzu daha küçük parçalara bölün.

### 3. İlk Değer Ataması:
Değişkenleri kullanmadan önce mutlaka ilk değerlerini atayın:

```csharp
// Kötü örnek
int sayi;
// ... birçok kod satırı
sayi = sayi + 5; // sayi değişkenine ilk değer atanmadığı için hata

// İyi örnek
int sayi = 0; // veya kullanım amacına uygun başka bir başlangıç değeri
// ... birçok kod satırı
sayi = sayi + 5; // Sorun yok
```

### 4. Değişken Kapsamını Sınırlama:
Değişkenleri mümkün olan en küçük kapsamda tanımlayın:

```csharp
// Kötü örnek
void Metot()
{
    int deger = 10;
    // ... birçok kod satırı
    if (kosul)
    {
        // ... deger burada kullanılıyor
    }
}

// İyi örnek
void Metot()
{
    if (kosul)
    {
        int deger = 10;
        // ... deger sadece burada kullanılıyor
    }
}
```

### 5. var Kullanımı:
`var` anahtar kelimesini tip açıkça anlaşılabilir olduğunda kullanın:

```csharp
// Kabul edilebilir var kullanımı
var isim = "Ahmet"; // string olduğu açık
var sayilar = new List<int>(); // List<int> olduğu açık

// Kafa karıştırıcı var kullanımı
var sonuc = GetResult(); // GetResult() metodunun ne döndürdüğü belirsiz
```

### 6. const ve readonly Doğru Kullanımı:
Değişmeyecek değerler için `const` veya `readonly` kullanın:

```csharp
// Derleme zamanında bilinen sabitler için const
public const double PI = 3.14159265359;

// Çalışma zamanında belirlenen değişmez değerler için readonly
public readonly Guid Id = Guid.NewGuid();
```

---

Bu doküman C# programlama dilinde değişken tanımlama ve kullanma konusunda temel ve ileri seviye bilgileri içermektedir. Daha fazla örnek ve detaylı bilgi için Microsoft'un resmi C# dokümantasyonuna başvurabilirsiniz.
