# C# If-Else İfadeleri: Kapsamlı Rehber

## İçindekiler
1. [Giriş](#giriş)
2. [If İfadesi](#if-ifadesi)
3. [If-Else İfadesi](#if-else-ifadesi)
4. [If-Else If-Else İfadesi](#if-else-if-else-ifadesi)
5. [İç İçe If-Else İfadeleri](#iç-içe-if-else-ifadeleri)
6. [Mantıksal Operatörler](#mantıksal-operatörler)
7. [Karşılaştırma Operatörleri](#karşılaştırma-operatörleri)
8. [Ternary Operatör](#ternary-operatör) 
9. [Switch Deyimi ile Karşılaştırma](#switch-deyimi-ile-karşılaştırma)
10. [İyi Uygulama Örnekleri](#iyi-uygulama-örnekleri)
11. [Yaygın Hatalar](#yaygın-hatalar)
12. [Örnek Projeler](#örnek-projeler)

## Giriş

If-else ifadeleri, C# programlama dilinde karar verme yapıları olarak kullanılır. Bu yapılar, belirli koşulların doğru veya yanlış olmasına bağlı olarak farklı kod bloklarının çalıştırılmasını sağlar. Programların farklı koşullara göre farklı davranışlar sergilemesini sağlayan temel yapı taşlarından biridir.

## If İfadesi

En basit karar verme yapısı, tek bir `if` ifadesidir. Belirtilen koşul doğru (`true`) ise, if bloğundaki kod çalıştırılır; aksi halde bu kod bloğu atlanır.

```csharp
if (koşul)
{
    // Koşul doğru ise bu kod bloğu çalıştırılır
}
```

**Örnek:**

```csharp
int yas = 18;

if (yas >= 18)
{
    Console.WriteLine("Reşitsiniz.");
}
```

## If-Else İfadesi

`if-else` yapısı, bir koşulun doğru olduğu durumda bir kod bloğunun, yanlış olduğu durumda ise başka bir kod bloğunun çalıştırılmasını sağlar.

```csharp
if (koşul)
{
    // Koşul doğru ise bu kod bloğu çalıştırılır
}
else
{
    // Koşul yanlış ise bu kod bloğu çalıştırılır
}
```

**Örnek:**

```csharp
int yas = 16;

if (yas >= 18)
{
    Console.WriteLine("Reşitsiniz.");
}
else
{
    Console.WriteLine("Reşit değilsiniz.");
}
```

## If-Else If-Else İfadesi

Birden fazla koşulun kontrol edilmesi gerektiğinde, `if-else if-else` yapısı kullanılır. İlk doğru koşula ait kod bloğu çalıştırılır ve diğer koşullar değerlendirilmez.

```csharp
if (koşul1)
{
    // Koşul1 doğru ise bu kod bloğu çalıştırılır
}
else if (koşul2)
{
    // Koşul1 yanlış ve koşul2 doğru ise bu kod bloğu çalıştırılır
}
else if (koşul3)
{
    // Koşul1 ve koşul2 yanlış, koşul3 doğru ise bu kod bloğu çalıştırılır
}
else
{
    // Hiçbir koşul doğru değilse bu kod bloğu çalıştırılır
}
```

**Örnek:**

```csharp
int puan = 75;

if (puan >= 90)
{
    Console.WriteLine("A");
}
else if (puan >= 80)
{
    Console.WriteLine("B");
}
else if (puan >= 70)
{
    Console.WriteLine("C");
}
else if (puan >= 60)
{
    Console.WriteLine("D");
}
else
{
    Console.WriteLine("F");
}
```

## İç İçe If-Else İfadeleri

If-else ifadeleri iç içe kullanılabilir. Bu, daha karmaşık karar verme mantığını uygulamaya olanak tanır.

```csharp
if (dışKoşul)
{
    // Dış koşul doğru ise bu kod çalıştırılır
    
    if (içKoşul)
    {
        // Dış koşul ve iç koşul doğru ise bu kod çalıştırılır
    }
    else
    {
        // Dış koşul doğru, iç koşul yanlış ise bu kod çalıştırılır
    }
}
else
{
    // Dış koşul yanlış ise bu kod çalıştırılır
}
```

**Örnek:**

```csharp
bool ogrenci = true;
int yas = 25;

if (ogrenci)
{
    if (yas < 18)
    {
        Console.WriteLine("Öğrenci indirimi: %20");
    }
    else
    {
        Console.WriteLine("Öğrenci indirimi: %10");
    }
}
else
{
    Console.WriteLine("İndirim yok");
}
```

## Mantıksal Operatörler

C# if-else ifadelerinde koşulların değerlendirilmesi için kullanılan mantıksal operatörler:

| Operatör | Açıklama | Örnek |
|----------|----------|-------|
| && (ve)  | Her iki koşul da doğru ise sonuç doğrudur | `if (a > 5 && b < 10)` |
| \|\| (veya) | En az bir koşul doğru ise sonuç doğrudur | `if (a > 5 \|\| b < 10)` |
| ! (değil) | Koşulun tersini alır | `if (!yagmurYagiyor)` |

**Örnek:**

```csharp
int yas = 25;
bool ogrenci = true;

if (yas >= 18 && ogrenci)
{
    Console.WriteLine("Reşit öğrencisiniz.");
}

if (yas < 18 || ogrenci)
{
    Console.WriteLine("İndirimden faydalanabilirsiniz.");
}

if (!ogrenci)
{
    Console.WriteLine("Öğrenci değilsiniz.");
}
```

## Karşılaştırma Operatörleri

C# if-else ifadelerinde sıkça kullanılan karşılaştırma operatörleri:

| Operatör | Açıklama | Örnek |
|----------|----------|-------|
| == | Eşitlik | `if (a == b)` |
| != | Eşit değil | `if (a != b)` |
| > | Büyüktür | `if (a > b)` |
| < | Küçüktür | `if (a < b)` |
| >= | Büyük veya eşit | `if (a >= b)` |
| <= | Küçük veya eşit | `if (a <= b)` |

**Örnek:**

```csharp
int a = 10;
int b = 20;

if (a == b)
{
    Console.WriteLine("a, b'ye eşittir.");
}

if (a != b)
{
    Console.WriteLine("a, b'ye eşit değildir.");
}

if (a > b)
{
    Console.WriteLine("a, b'den büyüktür.");
}

if (a < b)
{
    Console.WriteLine("a, b'den küçüktür.");
}
```

## Ternary Operatör

Ternary operatör (`? :`), basit if-else ifadelerinin daha kısa bir şekilde yazılmasını sağlar.

```csharp
sonuç = koşul ? doğruDurumundaDeger : yanlışDurumundaDeger;
```

**Örnek:**

```csharp
int yas = 20;
string durum = (yas >= 18) ? "Reşit" : "Reşit değil";
Console.WriteLine(durum); // "Reşit" yazdırır
```

İç içe ternary operatörler de kullanılabilir, ancak okunabilirlik açısından dikkatli olmak gerekir:

```csharp
int puan = 75;
string not = (puan >= 90) ? "A" : 
             (puan >= 80) ? "B" : 
             (puan >= 70) ? "C" : 
             (puan >= 60) ? "D" : "F";
Console.WriteLine(not); // "C" yazdırır
```

## Switch Deyimi ile Karşılaştırma

Çoklu koşulların değerlendirildiği durumlarda `if-else if-else` yerine `switch` deyimi daha okunaklı olabilir:

```csharp
switch (değişken)
{
    case değer1:
        // değişken değer1'e eşitse bu kod çalıştırılır
        break;
    case değer2:
        // değişken değer2'ye eşitse bu kod çalıştırılır
        break;
    default:
        // değişken hiçbir değere eşit değilse bu kod çalıştırılır
        break;
}
```

**Örnek:**

```csharp
int gun = 3;
string gunAdi;

switch (gun)
{
    case 1:
        gunAdi = "Pazartesi";
        break;
    case 2:
        gunAdi = "Salı";
        break;
    case 3:
        gunAdi = "Çarşamba";
        break;
    case 4:
        gunAdi = "Perşembe";
        break;
    case 5:
        gunAdi = "Cuma";
        break;
    case 6:
        gunAdi = "Cumartesi";
        break;
    case 7:
        gunAdi = "Pazar";
        break;
    default:
        gunAdi = "Geçersiz gün";
        break;
}

Console.WriteLine(gunAdi); // "Çarşamba" yazdırır
```

## İyi Uygulama Örnekleri

1. **Süslü Parantezleri Her Zaman Kullanın:**
```csharp
// İyi uygulama
if (kosul)
{
    // Kod
}

// Tavsiye edilmez (tek satır olsa bile)
if (kosul)
    // Kod
```

2. **Karmaşık İf-Else Yapılarını Sadeleştirin:**
```csharp
// Karmaşık ve zor okunur
if (a > b)
{
    if (c > d)
    {
        if (e > f)
        {
            // Kod
        }
    }
}

// Daha okunaklı
if (a > b && c > d && e > f)
{
    // Kod
}
```

3. **Null Kontrolü için Null Birleştirme Operatörü Kullanın:**
```csharp
// İf-else ile
string mesaj;
if (isim != null)
{
    mesaj = isim;
}
else
{
    mesaj = "Misafir";
}

// Null birleştirme operatörü ile daha kısa
string mesaj = isim ?? "Misafir";
```

4. **Fazla İç İçe Koşullardan Kaçının:**
```csharp
// Erken dönüş yapısı kullanarak okunabilirliği artırmak
public void ProcessRequest(Request request)
{
    if (request == null)
    {
        Console.WriteLine("Geçersiz istek");
        return;
    }
    
    if (!request.IsValid())
    {
        Console.WriteLine("Geçersiz istek formatı");
        return;
    }
    
    // Ana işlevsellik burada
    ProcessValidRequest(request);
}
```

## Yaygın Hatalar

1. **Süslü Parantez Unutmak:**
```csharp
// Hatalı kullanım - süslü parantezler olmadığında sadece ilk satır if'e bağlıdır
if (kosul)
    Console.WriteLine("Bu satır koşula bağlı");
    Console.WriteLine("Bu satır her zaman çalışır"); // Hata! İf bloğunun dışında
```

2. **Eşitlik Operatörleri ile Atama Operatörlerini Karıştırmak:**
```csharp
// Hatalı - atama yapıyor, karşılaştırma değil
if (x = 10) // x değeri 10 olarak atanır ve if her zaman true olur
{
    // Kod
}

// Doğru - karşılaştırma
if (x == 10)
{
    // Kod
}
```

3. **Yanlış Mantıksal Operatör Kullanmak:**
```csharp
// Hatalı mantık - VEYA yerine VE kullanıldı
if (age < 18 && age > 65) // Hiçbir zaman true olmaz
{
    Console.WriteLine("İndirimli bilet alabilirsiniz");
}

// Doğru mantık
if (age < 18 || age > 65)
{
    Console.WriteLine("İndirimli bilet alabilirsiniz");
}
```

4. **Float/Double için Eşitlik Kontrolü:**
```csharp
// Hatalı - ondalık sayılarda hassasiyet sorunları
if (x == 1.1) // Kesin eşitlik kontrolü
{
    // Kod
}

// Doğru - tolerans ile kontrol
double tolerans = 0.0001;
if (Math.Abs(x - 1.1) < tolerans)
{
    // Kod
}
```

## Örnek Projeler

### 1. Basit Hesap Makinesi

```csharp
using System;

class HesapMakinesi
{
    static void Main()
    {
        Console.WriteLine("Birinci sayıyı girin:");
        double sayi1 = Convert.ToDouble(Console.ReadLine());
        
        Console.WriteLine("İkinci sayıyı girin:");
        double sayi2 = Convert.ToDouble(Console.ReadLine());
        
        Console.WriteLine("İşlemi seçin (+, -, *, /):");
        char islem = Convert.ToChar(Console.ReadLine());
        
        double sonuc = 0;
        bool gecerliIslem = true;
        
        if (islem == '+')
        {
            sonuc = sayi1 + sayi2;
        }
        else if (islem == '-')
        {
            sonuc = sayi1 - sayi2;
        }
        else if (islem == '*')
        {
            sonuc = sayi1 * sayi2;
        }
        else if (islem == '/')
        {
            if (sayi2 != 0)
            {
                sonuc = sayi1 / sayi2;
            }
            else
            {
                Console.WriteLine("Sıfıra bölme hatası!");
                gecerliIslem = false;
            }
        }
        else
        {
            Console.WriteLine("Geçersiz işlem!");
            gecerliIslem = false;
        }
        
        if (gecerliIslem)
        {
            Console.WriteLine($"Sonuç: {sonuc}");
        }
    }
}
```

### 2. Basit Giriş Doğrulama

```csharp
using System;

class KullaniciGirisi
{
    static void Main()
    {
        string dogruKullaniciAdi = "admin";
        string dogruSifre = "1234";
        
        Console.WriteLine("Kullanıcı adını girin:");
        string kullaniciAdi = Console.ReadLine();
        
        Console.WriteLine("Şifreyi girin:");
        string sifre = Console.ReadLine();
        
        if (kullaniciAdi == dogruKullaniciAdi && sifre == dogruSifre)
        {
            Console.WriteLine("Giriş başarılı. Hoş geldiniz!");
        }
        else if (kullaniciAdi != dogruKullaniciAdi)
        {
            Console.WriteLine("Kullanıcı adı hatalı!");
        }
        else
        {
            Console.WriteLine("Şifre hatalı!");
        }
    }
}
```

### 3. Öğrenci Not Hesaplama

```csharp
using System;

class NotHesaplama
{
    static void Main()
    {
        Console.WriteLine("Vize notunu girin (0-100):");
        double vize = Convert.ToDouble(Console.ReadLine());
        
        Console.WriteLine("Final notunu girin (0-100):");
        double final = Convert.ToDouble(Console.ReadLine());
        
        // Girilen değerlerin doğruluğunu kontrol et
        if (vize < 0 || vize > 100 || final < 0 || final > 100)
        {
            Console.WriteLine("Hatalı not girişi! Notlar 0-100 arasında olmalıdır.");
            return;
        }
        
        // Ortalama hesapla
        double ortalama = vize * 0.4 + final * 0.6;
        
        Console.WriteLine($"Ortalamanız: {ortalama}");
        
        // Harf notu hesapla
        string harfNotu;
        
        if (ortalama >= 90)
        {
            harfNotu = "AA";
        }
        else if (ortalama >= 85)
        {
            harfNotu = "BA";
        }
        else if (ortalama >= 80)
        {
            harfNotu = "BB";
        }
        else if (ortalama >= 75)
        {
            harfNotu = "CB";
        }
        else if (ortalama >= 70)
        {
            harfNotu = "CC";
        }
        else if (ortalama >= 65)
        {
            harfNotu = "DC";
        }
        else if (ortalama >= 60)
        {
            harfNotu = "DD";
        }
        else if (ortalama >= 50)
        {
            harfNotu = "FD";
        }
        else
        {
            harfNotu = "FF";
        }
        
        Console.WriteLine($"Harf notunuz: {harfNotu}");
        
        // Geçme-kalma durumu
        if (ortalama >= 60 || harfNotu == "DD")
        {
            Console.WriteLine("Tebrikler, dersi geçtiniz!");
        }
        else if (ortalama >= 50)
        {
            Console.WriteLine("Koşullu geçtiniz.");
        }
        else
        {
            Console.WriteLine("Dersten kaldınız.");
        }
    }
}
```

Bu rehber, C# programlama dilindeki if-else ifadelerini kapsamlı bir şekilde ele almaktadır. Farklı kullanım senaryolarını ve örnekleri inceleyerek, kendi programlarınızda karar verme yapılarını etkili bir şekilde kullanabilirsiniz.
