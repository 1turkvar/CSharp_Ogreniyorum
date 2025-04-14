# C# Aritmetik Operatörler

## İçindekiler
1. [Temel Aritmetik Operatörler](#temel-aritmetik-operatörler)
2. [Artırma ve Azaltma Operatörleri](#artırma-ve-azaltma-operatörleri)
3. [Atama Operatörleri](#atama-operatörleri)
4. [Matematiksel İşlem Önceliği](#matematiksel-işlem-önceliği)
5. [Özel Durumlar ve Taşmalar](#özel-durumlar-ve-taşmalar)
6. [Örnek Uygulamalar](#örnek-uygulamalar)

## Temel Aritmetik Operatörler

C# dilinde beş temel aritmetik operatör bulunmaktadır:

| Operatör | İşlev | Örnek | Sonuç |
|----------|-------|-------|-------|
| + | Toplama | 5 + 3 | 8 |
| - | Çıkarma | 5 - 3 | 2 |
| * | Çarpma | 5 * 3 | 15 |
| / | Bölme | 5 / 3 | 1 (int) veya 1.6666... (float/double) |
| % | Mod (Kalan) | 5 % 3 | 2 |

### Örnek Kod:

```csharp
int a = 10;
int b = 3;

int toplama = a + b;    // 13
int cikarma = a - b;    // 7
int carpma = a * b;     // 30
int bolme = a / b;      // 3 (int bölmesi)
int mod = a % b;        // 1 (10'un 3'e bölümünden kalan)

// Ondalıklı sayılarla bölme
double c = 10.0;
double d = 3.0;
double bolmeDouble = c / d;  // 3.3333...
```

### Bölme İşleminin Özellikleri

Bölme operatörü `/`, operand türlerine bağlı olarak farklı sonuçlar üretir:
- İki tamsayı arasındaki bölme işlemi tamsayı bölmesi yapar (kesirli kısım atılır)
- En az bir operand ondalıklı sayı ise, sonuç da ondalıklı olur

```csharp
int x = 7 / 2;           // Sonuç: 3 (kesirli kısım atılır)
double y = 7.0 / 2;      // Sonuç: 3.5
double z = 7 / 2.0;      // Sonuç: 3.5
```

## Artırma ve Azaltma Operatörleri

C#'da değişkenlerin değerlerini 1 artırmak veya azaltmak için özel operatörler vardır:

| Operatör | İşlev | Örnek | Açıklama |
|----------|-------|-------|----------|
| ++ | Artırma | x++ veya ++x | Değişkenin değerini 1 artırır |
| -- | Azaltma | x-- veya --x | Değişkenin değerini 1 azaltır |

Bu operatörler iki farklı şekilde kullanılabilir:
- **Önek (Prefix)**: `++x` veya `--x` - Önce işlem yapılır, sonra değer kullanılır
- **Sonek (Postfix)**: `x++` veya `x--` - Önce değer kullanılır, sonra işlem yapılır

### Örnek Kod:

```csharp
int a = 5;
int b = ++a;  // Önce a bir artırılır (a=6), sonra b'ye atanır (b=6)
Console.WriteLine($"a: {a}, b: {b}");  // a: 6, b: 6

int c = 5;
int d = c++;  // Önce c'nin değeri d'ye atanır (d=5), sonra c bir artırılır (c=6)
Console.WriteLine($"c: {c}, d: {d}");  // c: 6, d: 5

int e = 5;
int f = --e;  // Önce e bir azaltılır (e=4), sonra f'ye atanır (f=4)
Console.WriteLine($"e: {e}, f: {f}");  // e: 4, f: 4

int g = 5;
int h = g--;  // Önce g'nin değeri h'ye atanır (h=5), sonra g bir azaltılır (g=4)
Console.WriteLine($"g: {g}, h: {h}");  // g: 4, h: 5
```

## Atama Operatörleri

C#'da aritmetik işlemleri daha kısa şekilde yazmak için bileşik atama operatörleri kullanılabilir:

| Operatör | Eşdeğeri | Örnek |
|----------|----------|-------|
| += | x = x + y | x += 5; |
| -= | x = x - y | x -= 5; |
| *= | x = x * y | x *= 5; |
| /= | x = x / y | x /= 5; |
| %= | x = x % y | x %= 5; |

### Örnek Kod:

```csharp
int x = 10;

x += 5;  // x = x + 5; ile aynı, x şimdi 15
Console.WriteLine(x);  // 15

x -= 3;  // x = x - 3; ile aynı, x şimdi 12
Console.WriteLine(x);  // 12

x *= 2;  // x = x * 2; ile aynı, x şimdi 24
Console.WriteLine(x);  // 24

x /= 4;  // x = x / 4; ile aynı, x şimdi 6
Console.WriteLine(x);  // 6

x %= 4;  // x = x % 4; ile aynı, x şimdi 2
Console.WriteLine(x);  // 2
```

## Matematiksel İşlem Önceliği

C#'da aritmetik operatörlerin öncelik sırası matematikteki standart sırayla aynıdır:

1. Parantezler `( )`
2. Artırma, azaltma operatörleri (önek): `++x`, `--x`
3. Çarpma, bölme ve mod: `*`, `/`, `%`
4. Toplama ve çıkarma: `+`, `-`
5. Artırma, azaltma operatörleri (sonek): `x++`, `x--`
6. Atama operatörleri: `=`, `+=`, `-=`, `*=`, `/=`, `%=`

### Örnek Kod:

```csharp
int sonuc1 = 5 + 3 * 2;         // 3*2 önce yapılır: 5+6 = 11
int sonuc2 = (5 + 3) * 2;       // Parantez öncelikli: 8*2 = 16
int sonuc3 = 10 - 4 / 2 + 3;    // 4/2 önce: 10-2+3 = 11
int sonuc4 = 10 / (2 + 3);      // Parantez öncelikli: 10/5 = 2

int x = 5;
int y = 3;
int sonuc5 = ++x + y--;         // Önce ++x (x=6), sonra y-- işlemi (y kullanılır=3, sonra y=2): 6+3 = 9

Console.WriteLine($"sonuc1: {sonuc1}");  // 11
Console.WriteLine($"sonuc2: {sonuc2}");  // 16
Console.WriteLine($"sonuc3: {sonuc3}");  // 11
Console.WriteLine($"sonuc4: {sonuc4}");  // 2
Console.WriteLine($"sonuc5: {sonuc5}");  // 9
Console.WriteLine($"x: {x}, y: {y}");    // x: 6, y: 2
```

## Özel Durumlar ve Taşmalar

### Sıfıra Bölme

C#'da tamsayı veya ondalıklı sayılar sıfıra bölündüğünde farklı davranışlar sergilenir:
- Tamsayı bölmesinde (`int`, `long` vb.) sıfıra bölme hatası (`DivideByZeroException`) oluşur
- Ondalıklı sayılarda (`float`, `double`, `decimal`) sıfıra bölme `Infinity` veya `NaN` (Not a Number) sonucu verir

```csharp
// Tamsayı sıfıra bölme hatası
// int hata = 5 / 0;  // DivideByZeroException fırlatır

// Ondalıklı sayılarda sıfıra bölme
double sonuc1 = 5.0 / 0.0;  // Infinity
double sonuc2 = 0.0 / 0.0;  // NaN (Not a Number)
```

### Tamsayı Taşmaları

C#'da tamsayı değişkenleri belirli aralıklarda değer alabilir. Bu aralıkların dışına çıkıldığında taşma (overflow) meydana gelir:

```csharp
// int için maksimum değer: 2,147,483,647
int maxInt = int.MaxValue;
int overflow = maxInt + 1;  // -2,147,483,648 olur (taşma/overflow)

// int için minimum değer: -2,147,483,648
int minInt = int.MinValue;
int underflow = minInt - 1;  // 2,147,483,647 olur (taşma/underflow)
```

Taşmaların kontrol edilmesi için `checked` bloğu kullanılabilir:

```csharp
int maxValue = int.MaxValue;

// Bu blok OverflowException fırlatacaktır
try
{
    checked
    {
        int willOverflow = maxValue + 1;
    }
}
catch (OverflowException ex)
{
    Console.WriteLine("Taşma yakalandı: " + ex.Message);
}

// unchecked blok (varsayılan davranış)
unchecked
{
    int silentOverflow = maxValue + 1;  // Sessizce taşar, -2,147,483,648 olur
    Console.WriteLine(silentOverflow);
}
```

## Örnek Uygulamalar

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

        Console.WriteLine($"Toplama: {sayi1} + {sayi2} = {sayi1 + sayi2}");
        Console.WriteLine($"Çıkarma: {sayi1} - {sayi2} = {sayi1 - sayi2}");
        Console.WriteLine($"Çarpma: {sayi1} * {sayi2} = {sayi1 * sayi2}");
        
        // Sıfıra bölme kontrolü
        if (sayi2 != 0)
            Console.WriteLine($"Bölme: {sayi1} / {sayi2} = {sayi1 / sayi2}");
        else
            Console.WriteLine("Sıfıra bölme yapılamaz.");
            
        // Mod işlemi sadece tamsayılar için anlamlıdır
        if (sayi2 != 0)
            Console.WriteLine($"Mod: {sayi1} % {sayi2} = {sayi1 % sayi2}");
        else
            Console.WriteLine("Sıfıra mod işlemi yapılamaz.");
    }
}
```

### 2. Faktöriyel Hesaplama

```csharp
using System;

class Faktoriyel
{
    static void Main()
    {
        Console.WriteLine("Faktöriyeli hesaplanacak sayıyı girin (0-20 arası):");
        int sayi = Convert.ToInt32(Console.ReadLine());
        
        if (sayi < 0)
        {
            Console.WriteLine("Negatif sayıların faktöriyeli hesaplanamaz.");
            return;
        }
        
        if (sayi > 20)
        {
            Console.WriteLine("20'den büyük sayılar için taşma olabilir.");
            return;
        }
        
        long faktoriyel = 1;
        for (int i = 2; i <= sayi; i++)
        {
            faktoriyel *= i;  // faktoriyel = faktoriyel * i;
        }
        
        Console.WriteLine($"{sayi}! = {faktoriyel}");
    }
}
```

### 3. Ortalama Hesaplama

```csharp
using System;

class Ortalama
{
    static void Main()
    {
        Console.WriteLine("Kaç adet sayının ortalaması hesaplanacak?");
        int adet = Convert.ToInt32(Console.ReadLine());
        
        if (adet <= 0)
        {
            Console.WriteLine("Geçerli bir sayı girmelisiniz.");
            return;
        }
        
        double toplam = 0;
        
        for (int i = 1; i <= adet; i++)
        {
            Console.WriteLine($"{i}. sayıyı girin:");
            double sayi = Convert.ToDouble(Console.ReadLine());
            toplam += sayi;  // toplam = toplam + sayi;
        }
        
        double ortalama = toplam / adet;
        Console.WriteLine($"Girilen {adet} sayının ortalaması: {ortalama}");
    }
}
```

### 4. Daire Hesaplamaları

```csharp
using System;

class DaireHesaplamalari
{
    static void Main()
    {
        Console.WriteLine("Dairenin yarıçapını girin:");
        double yaricap = Convert.ToDouble(Console.ReadLine());
        
        if (yaricap <= 0)
        {
            Console.WriteLine("Yarıçap pozitif bir değer olmalıdır.");
            return;
        }
        
        // Alan hesaplama: π * r²
        double alan = Math.PI * yaricap * yaricap;
        // veya: double alan = Math.PI * Math.Pow(yaricap, 2);
        
        // Çevre hesaplama: 2 * π * r
        double cevre = 2 * Math.PI * yaricap;
        
        Console.WriteLine($"Dairenin alanı: {alan}");
        Console.WriteLine($"Dairenin çevresi: {cevre}");
    }
}
```

### 5. Aritmetik, Geometrik ve Harmonik Ortalama

```csharp
using System;

class CesitliOrtalamalar
{
    static void Main()
    {
        Console.WriteLine("Birinci sayıyı girin:");
        double sayi1 = Convert.ToDouble(Console.ReadLine());
        
        Console.WriteLine("İkinci sayıyı girin:");
        double sayi2 = Convert.ToDouble(Console.ReadLine());
        
        if (sayi1 <= 0 || sayi2 <= 0)
        {
            Console.WriteLine("Harmonik ortalama için pozitif sayılar gereklidir.");
            return;
        }
        
        // Aritmetik ortalama: (a + b) / 2
        double aritmetikOrt = (sayi1 + sayi2) / 2;
        
        // Geometrik ortalama: √(a * b)
        double geometrikOrt = Math.Sqrt(sayi1 * sayi2);
        
        // Harmonik ortalama: 2 / (1/a + 1/b)
        double harmonikOrt = 2 / ((1 / sayi1) + (1 / sayi2));
        
        Console.WriteLine($"Aritmetik ortalama: {aritmetikOrt}");
        Console.WriteLine($"Geometrik ortalama: {geometrikOrt}");
        Console.WriteLine($"Harmonik ortalama: {harmonikOrt}");
    }
}
```
