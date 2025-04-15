# C# Dizileri: Tanımlama ve Kullanma Rehberi

## İçindekiler
1. [Dizi Nedir?](#dizi-nedir)
2. [Dizi Tanımlama](#dizi-tanımlama)
3. [Dizilere Erişim ve Değer Atama](#dizilere-erişim-ve-değer-atama)
4. [Çok Boyutlu Diziler](#çok-boyutlu-diziler)
5. [Düzensiz Diziler (Jagged Arrays)](#düzensiz-diziler-jagged-arrays)
6. [Dizi Metodları](#dizi-metodları)
7. [Dizileri Döngülerle Kullanma](#dizileri-döngülerle-kullanma)
8. [Dizi İşlemleri ve Performans](#dizi-işlemleri-ve-performans)
9. [Örnek Uygulamalar](#örnek-uygulamalar)
10. [En İyi Uygulamalar ve İpuçları](#en-iyi-uygulamalar-ve-ipuçları)

## Dizi Nedir?

Dizi (Array), aynı veri tipindeki elemanların bellekte ardışık olarak saklandığı bir veri yapısıdır. C#'ta diziler, System.Array sınıfından türetilmiş referans tiplerdir. Boyutları tanımlandıktan sonra değiştirilemez ve her elemanı indeks numarasıyla erişilebilir (0'dan başlayarak).

## Dizi Tanımlama

C#'ta dizi tanımlamanın birkaç farklı yolu vardır:

### 1. Boş Dizi Tanımlama

```csharp
// Tanımlama ve sonradan bellekte yer ayırma
int[] sayilar;
sayilar = new int[5]; // 5 elemanlı dizi oluşturuldu, tüm elemanlar varsayılan değerde (0)

// Tek satırda tanımlama ve bellekte yer ayırma
int[] sayilar2 = new int[10]; // 10 elemanlı int dizisi
string[] isimler = new string[3]; // 3 elemanlı string dizisi
```

### 2. İlk Değer Atayarak Tanımlama

```csharp
// İlk değer atayarak tanımlama (initializer list)
int[] sayilar = new int[] { 10, 20, 30, 40, 50 };

// Kısa yazım şekli
int[] sayilar2 = { 10, 20, 30, 40, 50 };

string[] gunler = { "Pazartesi", "Salı", "Çarşamba", "Perşembe", "Cuma" };
```

### 3. var Anahtar Kelimesi ile Tanımlama

```csharp
var sayilar = new int[5];
var isimler = new[] { "Ali", "Veli", "Ayşe" }; // Tip çıkarımı yapılır (string[])
```

## Dizilere Erişim ve Değer Atama

Dizilerde elemanlara indeks numarasıyla erişilir. İndeks 0'dan başlar:

```csharp
int[] sayilar = new int[5];

// Değer atama
sayilar[0] = 10;
sayilar[1] = 20;
sayilar[2] = 30;
sayilar[3] = 40;
sayilar[4] = 50;

// Değer okuma
Console.WriteLine(sayilar[0]); // 10
Console.WriteLine(sayilar[2]); // 30

// Hata (dizinin boyutunu aşan indeks)
// sayilar[5] = 60; // IndexOutOfRangeException hatası verir
```

### Dizi Uzunluğuna Erişim

```csharp
int[] sayilar = { 10, 20, 30, 40, 50 };
Console.WriteLine($"Dizi uzunluğu: {sayilar.Length}"); // 5
Console.WriteLine($"Son eleman: {sayilar[sayilar.Length - 1]}"); // 50
```

## Çok Boyutlu Diziler

C#'ta iki tip çok boyutlu dizi vardır: Dikdörtgen (rectangular) ve düzensiz (jagged) diziler.

### Dikdörtgen Diziler (Rectangular Arrays)

```csharp
// 2 boyutlu dizi tanımlama
int[,] matris = new int[3, 4]; // 3 satır, 4 sütun

// İlk değer atayarak tanımlama
int[,] matris2 = {
    { 1, 2, 3, 4 },
    { 5, 6, 7, 8 },
    { 9, 10, 11, 12 }
};

// Değer atama
matris[0, 0] = 1;
matris[0, 1] = 2;
matris[2, 3] = 15;

// Değer okuma
Console.WriteLine(matris2[1, 2]); // 7

// Dizi boyutlarına erişim
Console.WriteLine($"Boyut sayısı: {matris2.Rank}"); // 2
Console.WriteLine($"Satır sayısı: {matris2.GetLength(0)}"); // 3
Console.WriteLine($"Sütun sayısı: {matris2.GetLength(1)}"); // 4
```

### 3 Boyutlu Diziler

```csharp
// 3 boyutlu dizi
int[,,] cube = new int[2, 3, 4]; // 2 derinlik, 3 satır, 4 sütun

// Değer atama
cube[0, 0, 0] = 1;
cube[1, 2, 3] = 42;

// İlk değer atayarak tanımlama
int[,,] cube2 = {
    {
        { 1, 2, 3, 4 },
        { 5, 6, 7, 8 },
        { 9, 10, 11, 12 }
    },
    {
        { 13, 14, 15, 16 },
        { 17, 18, 19, 20 },
        { 21, 22, 23, 24 }
    }
};
```

## Düzensiz Diziler (Jagged Arrays)

Düzensiz diziler, dizilerin dizisi olarak düşünülebilir ve her alt dizinin farklı uzunlukta olmasına izin verir.

```csharp
// Düzensiz dizi tanımlama
int[][] jaggedArray = new int[3][];

// Alt dizileri başlatma
jaggedArray[0] = new int[4]; // İlk satır 4 elemanlı
jaggedArray[1] = new int[2]; // İkinci satır 2 elemanlı
jaggedArray[2] = new int[5]; // Üçüncü satır 5 elemanlı

// Değer atama
jaggedArray[0][0] = 1;
jaggedArray[0][3] = 4;
jaggedArray[1][0] = 5;
jaggedArray[2][4] = 15;

// İlk değer atayarak tanımlama
int[][] jaggedArray2 = {
    new int[] { 1, 2, 3, 4 },
    new int[] { 5, 6 },
    new int[] { 7, 8, 9, 10, 11 }
};

// Alt dizi uzunluklarına erişim
Console.WriteLine($"İlk alt dizinin uzunluğu: {jaggedArray2[0].Length}"); // 4
Console.WriteLine($"İkinci alt dizinin uzunluğu: {jaggedArray2[1].Length}"); // 2
```

## Dizi Metodları

C# dizileri System.Array sınıfından türetildiği için birçok yararlı metod sunar:

### Temel Dizi Metodları

```csharp
int[] sayilar = { 30, 10, 50, 20, 40 };

// Sıralama
Array.Sort(sayilar);
// sayilar: { 10, 20, 30, 40, 50 }

// Ters çevirme
Array.Reverse(sayilar);
// sayilar: { 50, 40, 30, 20, 10 }

// Dizi temizleme (tüm elemanları varsayılan değere ayarlama)
Array.Clear(sayilar, 0, sayilar.Length);
// sayilar: { 0, 0, 0, 0, 0 }

// Diziyi belirli bir değerle doldurma
Array.Fill(sayilar, 7);
// sayilar: { 7, 7, 7, 7, 7 }

// Bir değerin dizide aranması
int[] numbers = { 10, 20, 30, 40, 50 };
int index = Array.IndexOf(numbers, 30); // 2
int lastIndex = Array.LastIndexOf(numbers, 30); // 2

// Bir diziyi başka bir diziye kopyalama
int[] kaynak = { 1, 2, 3, 4, 5 };
int[] hedef = new int[5];
Array.Copy(kaynak, hedef, 5);
// hedef: { 1, 2, 3, 4, 5 }

// Alternatif kopyalama
int[] hedef2 = new int[7];
kaynak.CopyTo(hedef2, 2);
// hedef2: { 0, 0, 1, 2, 3, 4, 5 }

// İki dizinin eşitliğini kontrol etme
bool esitMi = Enumerable.SequenceEqual(kaynak, hedef); // true
```

### LINQ ile Dizi İşlemleri (System.Linq)

```csharp
using System.Linq;

int[] sayilar = { 30, 10, 50, 20, 40 };

// Minimum/Maximum bulma
int min = sayilar.Min(); // 10
int max = sayilar.Max(); // 50

// Toplam
int toplam = sayilar.Sum(); // 150

// Ortalama
double ortalama = sayilar.Average(); // 30.0

// Filtreleme
int[] buyukSayilar = sayilar.Where(x => x > 25).ToArray();
// buyukSayilar: { 30, 50, 40 }

// Sıralama
int[] siraliSayilar = sayilar.OrderBy(x => x).ToArray();
// siraliSayilar: { 10, 20, 30, 40, 50 }

// Ters sıralama
int[] tersSiraliSayilar = sayilar.OrderByDescending(x => x).ToArray();
// tersSiraliSayilar: { 50, 40, 30, 20, 10 }

// İlk/son eleman alma
int ilk = sayilar.First(); // 30 (hata verebilir)
int son = sayilar.Last(); // 40 (hata verebilir)

// Güvenli ilk/son eleman alma
int? ilkEleman = sayilar.FirstOrDefault(); // 30 (boş dizide null)
int? sonEleman = sayilar.LastOrDefault(); // 40 (boş dizide null)

// Belirli bir koşulu sağlayan eleman sayısı
int buyukSayiAdedi = sayilar.Count(x => x > 25); // 3

// Belirli bir koşulu tüm elemanların sağlayıp sağlamadığı
bool hepsi30danBuyukMu = sayilar.All(x => x > 30); // false
bool herhangi30danBuyukMu = sayilar.Any(x => x > 30); // true
```

## Dizileri Döngülerle Kullanma

### for Döngüsü

```csharp
int[] sayilar = { 10, 20, 30, 40, 50 };

// Klasik for döngüsü
for (int i = 0; i < sayilar.Length; i++)
{
    Console.WriteLine($"sayilar[{i}] = {sayilar[i]}");
}
```

### foreach Döngüsü

```csharp
// foreach döngüsü - sadece okuma için
foreach (int sayi in sayilar)
{
    Console.WriteLine(sayi);
    // sayi = 100; // Hata! foreach döngüsünde değer değiştirilemez
}
```

### Çok Boyutlu Dizilerde Döngüler

```csharp
int[,] matris = {
    { 1, 2, 3 },
    { 4, 5, 6 },
    { 7, 8, 9 }
};

// İç içe for döngüleri ile
for (int i = 0; i < matris.GetLength(0); i++)
{
    for (int j = 0; j < matris.GetLength(1); j++)
    {
        Console.Write($"{matris[i, j]} ");
    }
    Console.WriteLine();
}

// foreach ile
foreach (int eleman in matris)
{
    Console.Write($"{eleman} ");
}
// Çıktı: 1 2 3 4 5 6 7 8 9
```

### Düzensiz Dizilerde Döngüler

```csharp
int[][] jaggedArray = {
    new int[] { 1, 2, 3 },
    new int[] { 4, 5 },
    new int[] { 6, 7, 8, 9 }
};

// İç içe for döngüleri ile
for (int i = 0; i < jaggedArray.Length; i++)
{
    for (int j = 0; j < jaggedArray[i].Length; j++)
    {
        Console.Write($"{jaggedArray[i][j]} ");
    }
    Console.WriteLine();
}

// foreach ile
foreach (int[] satir in jaggedArray)
{
    foreach (int eleman in satir)
    {
        Console.Write($"{eleman} ");
    }
    Console.WriteLine();
}
```

## Dizi İşlemleri ve Performans

### Dizi Boyutlandırması

Diziler sabit boyutludur. Büyüklüğü değiştirmek için:

```csharp
int[] sayilar = { 1, 2, 3 };

// Diziyi yeniden boyutlandırma
Array.Resize(ref sayilar, 5);
// sayilar: { 1, 2, 3, 0, 0 }

// Küçültme
Array.Resize(ref sayilar, 2);
// sayilar: { 1, 2 }
```

> Not: `Resize` metodu aslında yeni bir dizi oluşturur ve verileri kopyalar, performans açısından maliyetlidir.

### Alternatif: Değişken Boyutlu Koleksiyonlar

Sık sık boyut değiştirme ihtiyacı varsa, dizi yerine List<T> gibi koleksiyonlar kullanmak daha uygundur:

```csharp
List<int> sayiListesi = new List<int> { 1, 2, 3 };
sayiListesi.Add(4);
sayiListesi.Add(5);

// Liste -> Dizi dönüşümü
int[] sayiDizisi = sayiListesi.ToArray();
```

## Örnek Uygulamalar

### 1. Dizi Elemanlarının Ortalamasını Bulma

```csharp
int[] notlar = { 85, 90, 78, 92, 88 };
double ortalama = 0;

foreach (int not in notlar)
{
    ortalama += not;
}

ortalama /= notlar.Length;
Console.WriteLine($"Notların ortalaması: {ortalama}");

// LINQ ile daha kısa:
double ortalamaLinq = notlar.Average();
Console.WriteLine($"Notların ortalaması (LINQ): {ortalamaLinq}");
```

### 2. Dizi Elemanlarını Sıralama ve İkili Arama (Binary Search)

```csharp
int[] sayilar = { 23, 12, 86, 72, 41, 59 };

// Diziyi sıralama
Array.Sort(sayilar);
// sayilar: { 12, 23, 41, 59, 72, 86 }

// İkili arama (önceden sıralanmış dizide)
int arananDeger = 41;
int bulunanIndeks = Array.BinarySearch(sayilar, arananDeger);

if (bulunanIndeks >= 0)
    Console.WriteLine($"{arananDeger} değeri {bulunanIndeks} indeksinde bulundu.");
else
    Console.WriteLine($"{arananDeger} değeri dizide bulunamadı.");
```

### 3. Matris Toplama

```csharp
int[,] matrisA = {
    { 1, 2, 3 },
    { 4, 5, 6 }
};

int[,] matrisB = {
    { 7, 8, 9 },
    { 10, 11, 12 }
};

int satirSayisi = matrisA.GetLength(0);
int sutunSayisi = matrisA.GetLength(1);

int[,] toplamMatris = new int[satirSayisi, sutunSayisi];

for (int i = 0; i < satirSayisi; i++)
{
    for (int j = 0; j < sutunSayisi; j++)
    {
        toplamMatris[i, j] = matrisA[i, j] + matrisB[i, j];
    }
}

// Sonucu yazdırma
Console.WriteLine("Toplam Matris:");
for (int i = 0; i < satirSayisi; i++)
{
    for (int j = 0; j < sutunSayisi; j++)
    {
        Console.Write($"{toplamMatris[i, j]} ");
    }
    Console.WriteLine();
}
```

### 4. Dizi İçinde En Büyük ve En Küçük Değeri Bulma

```csharp
int[] sayilar = { 83, 12, 44, 67, 29, 95, 71 };

int enKucuk = sayilar[0];
int enBuyuk = sayilar[0];

foreach (int sayi in sayilar)
{
    if (sayi < enKucuk)
        enKucuk = sayi;
        
    if (sayi > enBuyuk)
        enBuyuk = sayi;
}

Console.WriteLine($"En küçük değer: {enKucuk}");
Console.WriteLine($"En büyük değer: {enBuyuk}");

// LINQ ile daha kısa:
int enKucukLinq = sayilar.Min();
int enBuyukLinq = sayilar.Max();
```

## En İyi Uygulamalar ve İpuçları

1. **Dizi Boyutu**: Dizi boyutunu önceden biliyorsanız, boyutu doğru şekilde belirleyin. Çok büyük boyutlandırma bellek israfına neden olabilir.

2. **Performans**: Çok sık eleman ekleme/çıkarma işlemleri yapacaksanız dizi yerine `List<T>` kullanmayı düşünün.

3. **Null Kontrolleri**: Dizilerle çalışırken null kontrolü yapmayı unutmayın.

```csharp
if (dizi != null && dizi.Length > 0)
{
    // Dizi ile işlem yap
}
```

4. **İndeks Sınırları**: Her zaman indeks sınırlarını kontrol edin veya try-catch bloklarıyla IndexOutOfRangeException'ı yakalayın.

5. **Döngü Seçimi**: Sadece okuma yapacaksanız `foreach` kullanın, indeks bilgisine veya değiştirme işlemine ihtiyacınız varsa `for` döngüsü kullanın.

6. **Paralel İşlemler**: Büyük dizilerde performans kazanmak için `Parallel.For` veya `Parallel.ForEach` kullanmayı düşünün.

```csharp
using System.Threading.Tasks;

int[] buyukDizi = new int[10000];

Parallel.For(0, buyukDizi.Length, i =>
{
    buyukDizi[i] = i * 2;
});
```

7. **LINQ Kullanımı**: LINQ, dizi işlemlerini kolaylaştırır ve kodu daha okunaklı hale getirir, ancak performans kritik uygulamalarda manuel döngüler daha hızlı olabilir.

8. **Yüksek Performanslı Operasyonlar**: Çok büyük dizilerde yüksek performans gerektiren işlemler için `Span<T>` veya `Memory<T>` kullanmayı değerlendirin (C# 7.2 ve sonrası).

```csharp
Span<int> numbers = stackalloc int[100]; // Stack'te yer ayırma (küçük diziler için)
for (int i = 0; i < numbers.Length; i++)
{
    numbers[i] = i;
}
```

Bu rehber, C# dilinde dizilerin temel ve ileri düzey kullanımını kapsayan bir kaynaktır. İhtiyaçlarınıza göre bu bilgileri projelerinizde uygulayabilir ve geliştirebilirsiniz.
