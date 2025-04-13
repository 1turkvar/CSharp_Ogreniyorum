# C# `var` Anahtar Kelimesi Rehberi

## İçindekiler
1. [Giriş](#giriş)
2. [Var Anahtar Kelimesinin Tanımı](#var-anahtar-kelimesinin-tanımı)
3. [Implicit Type (Örtük Tipleme)](#implicit-type-örtük-tipleme)
4. [Kullanım Örnekleri](#kullanım-örnekleri)
5. [Var ile Değişken Tanımlama Kuralları](#var-ile-değişken-tanımlama-kuralları)
6. [Var Kullanmanın Avantajları](#var-kullanmanın-avantajları)
7. [Var Kullanmanın Dezavantajları](#var-kullanmanın-dezavantajları)
8. [Var vs Explicit Type Declaration](#var-vs-explicit-type-declaration)
9. [LINQ ile Var Kullanımı](#linq-ile-var-kullanımı)
10. [Sık Yapılan Hatalar](#sık-yapılan-hatalar)
11. [En İyi Kullanım Pratikleri](#en-iyi-kullanım-pratikleri)
12. [Sonuç](#sonuç)

## Giriş

C# programlama dilinde `var` anahtar kelimesi, Visual Studio 2008 ile birlikte C# 3.0 sürümünde tanıtılmıştır. Bu anahtar kelime, değişken tanımlamada tür çıkarımı (type inference) yapmanıza olanak tanır. Bu rehberde, `var` anahtar kelimesinin detaylı incelemesini ve doğru kullanım şekillerini bulacaksınız.

## Var Anahtar Kelimesinin Tanımı

`var` anahtar kelimesi, bir değişken bildirirken açıkça bir veri türü belirtmek yerine, derleyicinin atanan değere göre otomatik olarak değişkenin türünü belirlemesini sağlar. Önemli nokta, `var` kullanımının dinamik tipleme anlamına gelmemesidir - hala statik tipleme kuralları geçerlidir ve değişkenin türü derleme zamanında belirlenir.

## Implicit Type (Örtük Tipleme)

`var` kullanımı genellikle "implicit typing" (örtük tipleme) olarak adlandırılır. Bu, değişkenin tipinin derleyici tarafından belirlenmesi anlamına gelir:

```csharp
// Explicit typing (Açık tipleme)
int sayi = 5;

// Implicit typing (Örtük tipleme)
var sayi = 5; // derleyici bunu int olarak algılar
```

Örtük tipleme ile değişken yine de güçlü bir şekilde tiplendirilmiştir. `var sayi = 5;` ifadesindeki `sayi` değişkeninin tipi, derleme zamanında `int` olarak belirlenir ve daha sonra farklı bir türde değer atanamaz.

## Kullanım Örnekleri

### 1. Temel Değişken Tanımlama

```csharp
var isim = "Ahmet";           // string türünde
var yas = 25;                 // int türünde
var maas = 5000.50;           // double türünde
var aktif = true;             // bool türünde
var karakter = 'A';           // char türünde
```

### 2. Karmaşık Türler

```csharp
var liste = new List<string>();              // List<string> türünde
var sozluk = new Dictionary<int, string>();  // Dictionary<int, string> türünde
var tarih = DateTime.Now;                    // DateTime türünde
```

### 3. Anonim Türler

```csharp
var kisi = new { Ad = "Mehmet", Yas = 30 };  // Anonim tür
Console.WriteLine($"{kisi.Ad}, {kisi.Yas}"); // Mehmet, 30
```

## Var ile Değişken Tanımlama Kuralları

`var` anahtar kelimesini kullanırken dikkat edilmesi gereken bazı kurallar:

1. Değişken tanımlandığında mutlaka bir başlangıç değeri verilmelidir:

```csharp
var sayi = 10;     // Doğru
var isim = "Ali";  // Doğru
var deger;         // Hata! Başlangıç değeri yok
```

2. `null` ile başlatılan değişkenler için tür belirtilmelidir:

```csharp
var nesne = null;  // Hata! Tür belirlenemiyor
string isim = null;  // Doğru
```

3. Aynı ifadede birden fazla `var` değişkeni tanımlanamaz:

```csharp
var x = 5, y = 10;  // Hata! Aynı ifadede birden fazla var kullanılamaz
int x = 5, y = 10;  // Doğru
```

4. `var` metodun dönüş tipi olarak kullanılamaz:

```csharp
var MetodAdi()  // Hata! Metod dönüş tipi olarak var kullanılamaz
{
    return 5;
}
```

5. `var` parametrelerde kullanılamaz:

```csharp
void Metod(var param)  // Hata! Parametre tipi olarak var kullanılamaz
{
    // kod
}
```

## Var Kullanmanın Avantajları

1. **Kod Okunabilirliği**: Özellikle uzun tip adlarında kod daha temiz görünür.

```csharp
// Var kullanmadan
Dictionary<string, List<CustomerOrder>> siparisler = new Dictionary<string, List<CustomerOrder>>();

// Var ile
var siparisler = new Dictionary<string, List<CustomerOrder>>();
```

2. **Anonim Türler İçin Vazgeçilmez**: Anonim türlerde tür adı olmadığı için var kullanmak gereklidir.

```csharp
var ogrenci = new { Ad = "Ali", No = 123 };
```

3. **Kod Değişikliklerinde Kolaylık**: Değişkenin türü değiştiğinde, kodun sadece bir yerinde değişiklik yapmanız yeterlidir.

```csharp
// Tür değiştiğinde iki yerde değişiklik gerekir
float oran = 0.5f;
// Daha sonra double'a çevirmek istersek:
// double oran = 0.5;

// var ile sadece bir yerde değişiklik yapılır
var oran = 0.5f;
// var oran = 0.5; // sadece değer değişir
```

4. **LINQ Sorgularında Kullanışlı**: LINQ sorgularının sonuçları için var kullanımı kodu daha temiz ve anlaşılır kılar.

## Var Kullanmanın Dezavantajları

1. **Kod Anlaşılabilirliği**: Bazı durumlarda değişkenin tipini hemen görememe sorunu olabilir.

```csharp
var sonuc = MetodCagir(); // MetodCagir()'in ne döndürdüğü ilk bakışta anlaşılmaz
```

2. **Hata Ayıklama Zorluğu**: Bazen değişkenin tipini anlamak için ekstra çaba gerekebilir.

3. **Yanlış Tip Çıkarımı Riski**: Beklemediğiniz bir tipte değişken oluşabilir.

```csharp
var sayi = 5.0; // double olarak çıkarım yapar, int değil
```

## Var vs Explicit Type Declaration

Aşağıdaki örnekler, `var` ve açık tip bildiriminin karşılaştırmasını gösterir:

```csharp
// Açık tip bildirimi
string ad = "Ayşe";
int yas = 30;
List<int> sayilar = new List<int> { 1, 2, 3 };

// var kullanımı
var ad = "Ayşe";
var yas = 30;
var sayilar = new List<int> { 1, 2, 3 };
```

Her iki durumda da değişkenler aynı tipe sahiptir, fakat `var` kullanımı özellikle karmaşık tiplerle çalışırken kodu daha okunaklı hale getirebilir.

## LINQ ile Var Kullanımı

LINQ (Language Integrated Query) sorguları ile `var` kullanımı özellikle yaygındır ve önerilir:

```csharp
// var kullanmadan
IEnumerable<string> isimler = kisiler.Where(k => k.Yas > 18).Select(k => k.Ad);

// var ile
var isimler = kisiler.Where(k => k.Yas > 18).Select(k => k.Ad);

// Anonim tiplerle LINQ kullanımında var gereklidir
var yetiskinler = kisiler.Where(k => k.Yas > 18)
                         .Select(k => new { k.Ad, k.Yas, Durum = "Yetişkin" });
```

## Sık Yapılan Hatalar

1. **Var'ı Dinamik Tip Sanmak**: `var` anahtar kelimesi ile `dynamic` aynı değildir:

```csharp
var x = 5;     // int tipi, derleme zamanında belirlenir
dynamic y = 5; // dynamic tipi, çalışma zamanında belirlenir

x = "metin";   // Hata! int tipine string atanamaz
y = "metin";   // Çalışır, dynamic tip çalışma zamanında değişebilir
```

2. **Tip Belirlenemeyen Durumlarda Kullanma**:

```csharp
var deger;           // Hata! Başlangıç değeri yok
var degisken = null; // Hata! Tür belirlenemiyor
```

3. **Tipin Önemi Olduğu Yerlerde Var Kullanma**:

```csharp
var sonuc = 5 / 2;    // sonuc int olur, değeri 2
double sonuc = 5 / 2; // sonuc yine 2.0 olur
double sonuc = 5.0 / 2; // sonuc 2.5 olur

// Doğru kullanım:
var sonuc = 5.0 / 2;  // sonuc double olur, değeri 2.5
```

## En İyi Kullanım Pratikleri

1. **Anlaşılır Değişken İsimleri Kullanın**: `var` kullanıyorsanız, değişken isimlerinden tipin anlaşılması faydalı olabilir.

```csharp
var kullaniciListesi = new List<Kullanici>(); // İsimden listenin ne içerdiği anlaşılabilir
var s = new List<Kullanici>();                // Zayıf isimlendirme
```

2. **Uygun Durumlarda Kullanın**: Özellikle aşağıdaki durumlarda `var` kullanımı önerilir:
   - Anonim türlerle çalışırken
   - LINQ sorgularında
   - Uzun tip adlarıyla çalışırken
   - Sağ tarafta tip açıkça belirtildiğinde

3. **Açık Olmayan Durumlarda Kullanmaktan Kaçının**:

```csharp
var sonuc = KarmasikMetod(); // Metodun dönüş tipi belirsizse açık tip kullanmak daha iyi olabilir
```

4. **Temel Tipler İçin Kullanım Tercihi**: Basit tipler (int, string, bool vb.) için `var` kullanımı tercihe bağlıdır, ancak tutarlı bir yaklaşım benimsemek önemlidir.

## Sonuç

C#'daki `var` anahtar kelimesi, doğru kullanıldığında kod okunabilirliğini ve bakımını kolaylaştıran güçlü bir araçtır. Örtük tiplemenin avantajlarını sunarken, statik tip güvenliğini korur. En iyi sonuçları elde etmek için, ekip içinde tutarlı bir kullanım politikası belirlemek ve yukarıda belirtilen en iyi pratikleri takip etmek önemlidir.

`var` anahtar kelimesi, C# dilinin evrimiyle birlikte gelişen ve modern C# programlamanın ayrılmaz bir parçası haline gelen önemli bir dil özelliğidir. Doğru kullanıldığında, kodunuzu daha temiz ve bakımı daha kolay hale getirebilir.
