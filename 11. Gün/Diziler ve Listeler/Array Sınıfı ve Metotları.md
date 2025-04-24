# C# Array Sınıfı ve Metotları

C# dilinde Array sınıfı, dizilerle çalışmanıza olanak sağlayan temel bir koleksiyon tipidir. Bu dokümanda C# dizilerinin temel özellikleri ve Array sınıfının sunduğu yararlı metotları detaylı olarak inceleyeceğiz.

## İçindekiler

1. [Dizilere Genel Bakış](#dizilere-genel-bakış)
2. [Dizi Tanımlama ve Başlatma](#dizi-tanımlama-ve-başlatma)
3. [Dizilere Erişim](#dizilere-erişim)
4. [Çok Boyutlu Diziler](#çok-boyutlu-diziler)
5. [Array Sınıfı Metotları](#array-sınıfı-metotları)
6. [Sık Kullanılan Array Metotları](#sık-kullanılan-array-metotları)
7. [LINQ ile Dizilerde İşlemler](#linq-ile-dizilerde-i̇şlemler)
8. [En İyi Uygulamalar](#en-i̇yi-uygulamalar)
9. [Genel Örnekler](#genel-örnekler)

## Dizilere Genel Bakış

Diziler, aynı türdeki değerleri sıralı bir koleksiyon olarak saklayan veri yapılarıdır. C#'ta tüm diziler `System.Array` sınıfından türetilir ve aşağıdaki özelliklere sahiptir:

- **Sabit Boyut**: Bir dizi oluşturulduktan sonra boyutu değiştirilemez.
- **Sıfır Tabanlı İndeksleme**: İlk elemana indeks 0 ile erişilir.
- **Referans Tipi**: Diziler, değer tipi değil referans tipidir.
- **Türe Özgü**: Bir dizi belirli bir türün elemanlarını içerebilir.

## Dizi Tanımlama ve Başlatma

C#'ta diziler çeşitli şekillerde tanımlanabilir ve başlatılabilir:

```csharp
// Temel dizi tanımlama
int[] sayilar;

// Boyut belirterek tanımlama
int[] sayilar = new int[5];

// Tanımlama ve başlatma
int[] sayilar = new int[] { 1, 2, 3, 4, 5 };
// veya kısaca
int[] sayilar = { 1, 2, 3, 4, 5 };

// Önce tanımlayıp sonra başlatma
string[] isimler;
isimler = new string[] { "Ali", "Veli", "Ayşe" };
```

## Dizilere Erişim

Dizi elemanlarına indeks kullanarak erişilebilir:

```csharp
int[] sayilar = { 10, 20, 30, 40, 50 };

// Elemana erişim
int ilkEleman = sayilar[0];  // 10
int ucuncuEleman = sayilar[2];  // 30

// Eleman değiştirme
sayilar[1] = 25;  // Artık dizi: { 10, 25, 30, 40, 50 }
```

## Çok Boyutlu Diziler

C#'ta iki tip çok boyutlu dizi bulunur:

### 1. Dikdörtgensel Diziler (Rectangular Arrays)

```csharp
// 2x3 boyutunda dizi tanımlama
int[,] matris = new int[2, 3];

// Başlatma
int[,] matris = new int[,] { { 1, 2, 3 }, { 4, 5, 6 } };

// Elemana erişim
int deger = matris[1, 2];  // 6 (ikinci satır, üçüncü sütun)
```

### 2. Düzensiz Diziler (Jagged Arrays)

```csharp
// Düzensiz dizi tanımlama (dizi dizisi)
int[][] duzensizDizi = new int[3][];

// Her satır için farklı boyutlu diziler oluşturma
duzensizDizi[0] = new int[] { 1, 2 };
duzensizDizi[1] = new int[] { 3, 4, 5 };
duzensizDizi[2] = new int[] { 6 };

// Elemana erişim
int deger = duzensizDizi[1][2];  // 5
```

## Array Sınıfı Metotları

`System.Array` sınıfı, dizilerle çalışmak için bir dizi statik metot ve özellik sunar:

### Temel Özellikler

- **Length**: Dizideki toplam eleman sayısı
- **Rank**: Dizinin boyut sayısı
- **GetLength(int dimension)**: Belirtilen boyuttaki eleman sayısı

```csharp
int[] sayilar = { 1, 2, 3, 4, 5 };
Console.WriteLine(sayilar.Length);  // 5

int[,] matris = new int[3, 4];
Console.WriteLine(matris.Rank);  // 2
Console.WriteLine(matris.GetLength(0));  // 3 (ilk boyut)
Console.WriteLine(matris.GetLength(1));  // 4 (ikinci boyut)
```

## Sık Kullanılan Array Metotları

### Sort

Bir diziyi artan sırada düzenler:

```csharp
int[] sayilar = { 4, 2, 7, 1, 5 };
Array.Sort(sayilar);
// sayilar artık { 1, 2, 4, 5, 7 }
```

### Reverse

Bir dizinin elemanlarını tersine çevirir:

```csharp
int[] sayilar = { 1, 2, 3, 4, 5 };
Array.Reverse(sayilar);
// sayilar artık { 5, 4, 3, 2, 1 }
```

### IndexOf

Bir elemanın dizideki ilk indeksini bulur:

```csharp
int[] sayilar = { 10, 20, 30, 20, 40 };
int indeks = Array.IndexOf(sayilar, 20);  // 1
```

### LastIndexOf

Bir elemanın dizideki son indeksini bulur:

```csharp
int[] sayilar = { 10, 20, 30, 20, 40 };
int indeks = Array.LastIndexOf(sayilar, 20);  // 3
```

### Find ve FindAll

Belirli bir koşulu sağlayan elemanları bulmak için:

```csharp
int[] sayilar = { 1, 2, 3, 4, 5, 6, 7, 8, 9 };

// İlk çift sayıyı bul
int ciftSayi = Array.Find(sayilar, eleman => eleman % 2 == 0);  // 2

// Tüm çift sayıları bul
int[] ciftSayilar = Array.FindAll(sayilar, eleman => eleman % 2 == 0);  // { 2, 4, 6, 8 }
```

### Exists

Belirli bir koşulu sağlayan eleman olup olmadığını kontrol eder:

```csharp
int[] sayilar = { 1, 2, 3, 4, 5 };
bool varMi = Array.Exists(sayilar, eleman => eleman > 3);  // true
```

### Copy

Bir dizinin elemanlarını başka bir diziye kopyalar:

```csharp
int[] kaynak = { 1, 2, 3, 4, 5 };
int[] hedef = new int[5];

Array.Copy(kaynak, hedef, 5);
// hedef artık { 1, 2, 3, 4, 5 }

// Belirli bir aralığı kopyalamak için:
int[] kismiHedef = new int[3];
Array.Copy(kaynak, 1, kismiHedef, 0, 3);
// kismiHedef artık { 2, 3, 4 }
```

### Clone

Bir dizinin tam bir kopyasını oluşturur:

```csharp
int[] orijinal = { 1, 2, 3, 4, 5 };
int[] kopya = (int[])orijinal.Clone();
```

### Clear

Bir dizinin belirli bir aralığındaki elemanları varsayılan değerlerine ayarlar:

```csharp
int[] sayilar = { 1, 2, 3, 4, 5 };
Array.Clear(sayilar, 1, 2);  // İndeks 1'den başlayarak 2 elemanı sıfırla
// sayilar artık { 1, 0, 0, 4, 5 }
```

### Resize

Bir dizi referansını yeniden boyutlandırır:

```csharp
int[] sayilar = { 1, 2, 3 };
Array.Resize(ref sayilar, 5);  // Diziyi 5 elemanlı yapar
// sayilar artık { 1, 2, 3, 0, 0 }

Array.Resize(ref sayilar, 2);  // Diziyi 2 elemanlı yapar
// sayilar artık { 1, 2 }
```

### BinarySearch

Sıralı bir dizide ikili arama yapar:

```csharp
int[] sayilar = { 2, 4, 6, 8, 10, 12 };
int indeks = Array.BinarySearch(sayilar, 8);  // 3
```

## LINQ ile Dizilerde İşlemler

C# 3.0'dan itibaren, LINQ (Language Integrated Query) ile dizilerde daha gelişmiş işlemler yapılabilir:

```csharp
using System.Linq;

int[] sayilar = { 1, 2, 3, 4, 5, 6, 7, 8, 9 };

// Çift sayıları seç
var ciftler = sayilar.Where(x => x % 2 == 0);  // { 2, 4, 6, 8 }

// Sayıların karesini al
var kareler = sayilar.Select(x => x * x);  // { 1, 4, 9, 16, 25, 36, 49, 64, 81 }

// İlk 3 sayıyı seç
var ilkUc = sayilar.Take(3);  // { 1, 2, 3 }

// Sayıları topla
int toplam = sayilar.Sum();  // 45

// En küçük ve en büyük değer
int min = sayilar.Min();  // 1
int max = sayilar.Max();  // 9

// Ortalama
double ortalama = sayilar.Average();  // 5

// Filtreleme ve sıralama
var filtreliVeSirali = sayilar
    .Where(x => x > 3)
    .OrderByDescending(x => x);  // { 9, 8, 7, 6, 5, 4 }
```

## En İyi Uygulamalar

1. **Boyut Belirlemede Dikkatli Olun**: Diziler sabit boyutludur; sık değişen boyutlar için `List<T>` kullanın.
2. **Dizileri Null Kontrolü Yapın**: Null dizilerle çalışırken NullReferenceException hatalarını önlemek için kontrol yapmayı unutmayın.
3. **Out-of-bounds Hatalarından Kaçının**: İndekslerin dizi sınırları içinde olduğundan emin olun.
4. **İşlem Performansı**: Büyük dizilerde doğru algoritma seçimi önemlidir (örn. sıralama için Sort, arama için BinarySearch).
5. **LINQ Kullanımı**: Komplex sorgulama işlemlerinde LINQ kullanarak kodunuzu daha okunabilir hale getirin.

## Genel Örnekler

### Örnek 1: Dizi Elemanlarını Toplama

```csharp
int[] sayilar = { 1, 2, 3, 4, 5 };
int toplam = 0;

foreach (int sayi in sayilar)
{
    toplam += sayi;
}

Console.WriteLine($"Toplam: {toplam}");  // Toplam: 15

// LINQ ile daha kısa:
int toplamLinq = sayilar.Sum();
Console.WriteLine($"LINQ ile toplam: {toplamLinq}");  // LINQ ile toplam: 15
```

### Örnek 2: En Büyük ve En Küçük Değer Bulma

```csharp
int[] sayilar = { 15, 7, 23, 4, 19 };

// Klasik yöntem
int enKucuk = sayilar[0];
int enBuyuk = sayilar[0];

for (int i = 1; i < sayilar.Length; i++)
{
    if (sayilar[i] < enKucuk)
        enKucuk = sayilar[i];
    
    if (sayilar[i] > enBuyuk)
        enBuyuk = sayilar[i];
}

Console.WriteLine($"En küçük: {enKucuk}, En büyük: {enBuyuk}");  // En küçük: 4, En büyük: 23

// LINQ ile daha kısa:
int minLinq = sayilar.Min();
int maxLinq = sayilar.Max();
Console.WriteLine($"LINQ ile en küçük: {minLinq}, en büyük: {maxLinq}");  // LINQ ile en küçük: 4, en büyük: 23
```

### Örnek 3: Filtreleme ve Dönüşüm

```csharp
string[] isimler = { "Ali", "Veli", "Ayşe", "Fatma", "Mehmet" };

// 4 karakterden uzun isimleri filtrele ve büyük harfe çevir
string[] filtrelenmis = Array.FindAll(isimler, isim => isim.Length > 4);
for (int i = 0; i < filtrelenmis.Length; i++)
{
    filtrelenmis[i] = filtrelenmis[i].ToUpper();
}

// Sonuçları göster
Console.WriteLine(string.Join(", ", filtrelenmis));  // FATMA, MEHMET

// LINQ ile daha kısa:
var filtrelenmisLinq = isimler
    .Where(isim => isim.Length > 4)
    .Select(isim => isim.ToUpper());
    
Console.WriteLine(string.Join(", ", filtrelenmisLinq));  // FATMA, MEHMET
```

---

Bu doküman, C#'ta Array sınıfı ve metotları hakkında kapsamlı bir genel bakış sunmaktadır. Dizilerle çalışırken karşılaşabileceğiniz yaygın senaryolar için referans olarak kullanabilirsiniz. Daha fazla bilgi için .NET dokümantasyonunu inceleyebilirsiniz.