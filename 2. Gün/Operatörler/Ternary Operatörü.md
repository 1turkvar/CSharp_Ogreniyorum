# C# Ternary Operatörü - Kapsamlı Rehber

## İçindekiler
1. [Giriş](#giriş)
2. [Ternary Operatörünün Sözdizimi](#ternary-operatörünün-sözdizimi)
3. [Nasıl Çalışır?](#nasıl-çalışır)
4. [Örnekler](#örnekler)
   - [Temel Kullanım](#temel-kullanım)
   - [Değişkene Atama](#değişkene-atama)
   - [Metot Parametreleri](#metot-parametreleri)
   - [String Formatlama](#string-formatlama)
   - [İç İçe Kullanım](#iç-içe-kullanım)
5. [If-Else ile Karşılaştırma](#if-else-ile-karşılaştırma)
6. [En İyi Kullanım Pratikleri](#en-iyi-kullanım-pratikleri)
7. [Yaygın Hatalar ve Çözümleri](#yaygın-hatalar-ve-çözümleri)
8. [Ternary Operatörüyle İlgili İpuçları](#ternary-operatörüyle-ilgili-ipuçları)
9. [Özet](#özet)

## Giriş

Ternary operatörü, C# dilinde koşullu ifadeleri tek bir satırda değerlendirmek için kullanılan güçlü bir araçtır. Bu operatör, basit `if-else` ifadelerini daha kısa ve öz bir şekilde yazmanızı sağlar. Adı, üç bölümden oluşmasından gelir: koşul, doğru durumda döndürülecek değer ve yanlış durumda döndürülecek değer.

## Ternary Operatörünün Sözdizimi

```csharp
koşul ? doğruysa_değer : yanlışsa_değer
```

Bu sözdiziminde:
- `koşul`: Değerlendirilecek Boolean ifadesi
- `doğruysa_değer`: Koşul true olarak değerlendirildiğinde döndürülecek değer
- `yanlışsa_değer`: Koşul false olarak değerlendirildiğinde döndürülecek değer

## Nasıl Çalışır?

Ternary operatörü şu şekilde çalışır:

1. İlk olarak, `koşul` değerlendirilir.
2. Eğer `koşul` true ise, `doğruysa_değer` döndürülür.
3. Eğer `koşul` false ise, `yanlışsa_değer` döndürülür.

Ternary operatörü, bir ifadedir ve bir değer döndürür. Bu, onu atama ifadelerinde ve metot çağrılarında kullanımını özellikle etkili kılar.

## Örnekler

### Temel Kullanım

```csharp
// Bir sayının pozitif, negatif veya sıfır olduğunu kontrol etme
int sayi = -5;
string sonuc = sayi > 0 ? "Pozitif" : (sayi < 0 ? "Negatif" : "Sıfır");
Console.WriteLine(sonuc); // "Negatif" yazdırır
```

### Değişkene Atama

```csharp
// Yaşa göre fiyat belirleme
int yas = 15;
double fiyat = yas < 18 ? 5.0 : 10.0;
Console.WriteLine($"Bilet fiyatı: {fiyat} TL"); // "Bilet fiyatı: 5 TL" yazdırır
```

### Metot Parametreleri

```csharp
// Ternary operatörü ile metot parametresi belirleme
public void SelamVer(string ad)
{
    Console.WriteLine($"Merhaba {string.IsNullOrEmpty(ad) ? "Misafir" : ad}!");
}

SelamVer(""); // "Merhaba Misafir!" yazdırır
SelamVer("Ahmet"); // "Merhaba Ahmet!" yazdırır
```

### String Formatlama

```csharp
// Çoğul/tekil durumu ele alma
int ürünSayısı = 1;
Console.WriteLine($"Sepetinizde {ürünSayısı} ürün{(ürünSayısı > 1 ? "ler" : "")} bulunmaktadır.");
// "Sepetinizde 1 ürün bulunmaktadır." yazdırır

ürünSayısı = 3;
Console.WriteLine($"Sepetinizde {ürünSayısı} ürün{(ürünSayısı > 1 ? "ler" : "")} bulunmaktadır.");
// "Sepetinizde 3 ürünler bulunmaktadır." yazdırır
```

### İç İçe Kullanım

```csharp
// İç içe ternary operatörleri
int puan = 75;
string not = puan >= 90 ? "A" : (puan >= 80 ? "B" : (puan >= 70 ? "C" : (puan >= 60 ? "D" : "F")));
Console.WriteLine($"Notunuz: {not}"); // "Notunuz: C" yazdırır
```

## If-Else ile Karşılaştırma

Ternary operatörü, birçok durumda `if-else` yapısına alternatif olarak kullanılabilir:

```csharp
// if-else kullanımı
int sayi = 10;
string mesaj;
if (sayi > 0)
{
    mesaj = "Pozitif";
}
else
{
    mesaj = "Pozitif değil";
}

// Ternary operatörü ile aynı işlev
string mesaj2 = sayi > 0 ? "Pozitif" : "Pozitif değil";
```

Ternary operatörü kodu daha kısa hale getirir ve tek bir değer atama durumlarında özellikle kullanışlıdır. Ancak, çok karmaşık koşullar için okunabilirliği azalabilir.

## En İyi Kullanım Pratikleri

1. **Basit koşullar için kullanın**: Ternary operatörü, basit koşullu atamalar için idealdir. Karmaşık koşullar için geleneksel `if-else` yapısını tercih edin.

2. **Okunabilirliği koruyun**: İç içe ternary operatörleri kullanırken kodu okunabilir tutmak için parantezler kullanın.

3. **Tek bir değer döndürmek için kullanın**: Ternary operatörü bir değer döndürmek için tasarlanmıştır, her iki durumda da aynı türde değerler döndürün.

4. **Yan etkileri karıştırmayın**: Ternary operatörünün içinde karmaşık işlemler veya metot çağrıları kullanmaktan kaçının.

## Yaygın Hatalar ve Çözümleri

### 1. Farklı türlerde değerler döndürme

```csharp
// Hatalı kullanım
var sonuc = koşul ? "metin" : 123; // Derleme hatası verir

// Doğru kullanım
var sonuc = koşul ? "metin" : "123"; // Her iki durumda da string döndürür
```

### 2. İfade yerine ifade olmayan kod yazma

```csharp
// Hatalı kullanım
koşul ? Console.WriteLine("Doğru") : Console.WriteLine("Yanlış"); // Derleme hatası

// Doğru kullanım
Console.WriteLine(koşul ? "Doğru" : "Yanlış");
```

### 3. Aşırı karmaşık ternary zincirleri

```csharp
// Okunması zor
string sonuc = a > b ? "Büyük" : a < b ? "Küçük" : a == 0 ? "Sıfır" : "Eşit";

// Daha okunabilir
string sonuc = a > b ? "Büyük" : 
               (a < b ? "Küçük" : 
               (a == 0 ? "Sıfır" : "Eşit"));

// Veya if-else kullanmak daha iyi olabilir
```

## Ternary Operatörüyle İlgili İpuçları

1. **Null Koşullu Operatörle Birlikte Kullanım**: C# 6.0 ve sonrasında, null koşullu operatör (`?.`) ile birlikte kullanılabilir:

```csharp
string isim = kişi?.Ad ?? "İsimsiz";
```

2. **Kısa Devre Değerlendirmesi**: Ternary operatörü, kısa devre değerlendirmesi yapar. Yani, koşul true ise, ikinci ifade değerlendirilir ve üçüncü değerlendirilmez. Koşul false ise, üçüncü ifade değerlendirilir ve ikinci değerlendirilmez.

```csharp
// Sadece bir metot çağrılır
string sonuc = koşul ? MetotBir() : Metotİki();
```

3. **Lambda İfadeleri İçinde Kullanım**: Ternary operatörü, lambda ifadeleri içinde sıklıkla kullanılır:

```csharp
var liste = yeni.Select(x => x.Değer > 10 ? "Büyük" : "Küçük").ToList();
```

## Özet

C# ternary operatörü (`koşul ? doğruysa_değer : yanlışsa_değer`), basit koşullu atamaları tek bir satırda ifade etmenizi sağlayan güçlü bir dil özelliğidir. Bu operatör:

- Kodu daha kısa ve öz hale getirir
- Değişken atamaları, metot çağrıları ve diğer ifade bağlamlarında kullanılabilir
- Okunabilirlik ve basitlik dengesini koruyarak kullanıldığında kod kalitesini artırır

Ternary operatörü doğru kullanıldığında, kodunuzu hem daha kısa hem de daha anlaşılır hale getirebilir. Ancak, karmaşık koşullar veya eylemler için geleneksel kontrol yapıları daha uygun olabilir.
