# C# For Döngüsü Detaylı Anlatım

## İçindekiler
- [For Döngüsü Nedir?](#for-döngüsü-nedir)
- [For Döngüsünün Yapısı](#for-döngüsünün-yapısı)
- [For Döngüsü Örnekleri](#for-döngüsü-örnekleri)
- [For Döngüsünün Özel Kullanımları](#for-döngüsünün-özel-kullanımları)
- [Foreach ile Karşılaştırma](#foreach-ile-karşılaştırma)
- [İyi Programlama Uygulamaları](#iyi-programlama-uygulamaları)
- [Sık Yapılan Hatalar](#sık-yapılan-hatalar)
- [Alıştırmalar](#alıştırmalar)

## For Döngüsü Nedir?

For döngüsü, C# programlama dilinde belirli bir kod bloğunu belirli sayıda tekrarlamak için kullanılan temel bir kontrol yapısıdır. Bu döngü tipi, özellikle tekrar sayısının önceden bilindiği durumlarda tercih edilir.

For döngüsü genellikle şu durumlar için kullanılır:
- Dizileri ve koleksiyonları işlemek
- Belirli bir sayıda işlem yapmak
- Matematiksel hesaplamalar ve algoritmalar

## For Döngüsünün Yapısı

C# for döngüsünün temel sözdizimi şu şekildedir:

```csharp
for (başlangıç_ifadesi; koşul_ifadesi; artırma_ifadesi)
{
    // Tekrarlanacak kod bloğu
}
```

Bu yapıdaki bileşenler:

1. **Başlangıç İfadesi**: Döngüye başlamadan önce bir kere çalıştırılır. Genellikle sayaç değişkenini başlatmak için kullanılır.
2. **Koşul İfadesi**: Her döngü iterasyonundan önce değerlendirilir. Sonuç `true` ise döngü devam eder, `false` ise döngü sonlanır.
3. **Artırma İfadesi**: Her döngü iterasyonundan sonra çalıştırılır. Genellikle sayaç değişkenini güncellemek için kullanılır.

## For Döngüsü Örnekleri

### Temel For Döngüsü Örneği

```csharp
// 1'den 10'a kadar olan sayıları yazdırma
for (int i = 1; i <= 10; i++)
{
    Console.WriteLine(i);
}
```

### Dizi Elemanlarını İşleme

```csharp
int[] sayilar = { 2, 4, 6, 8, 10 };

// Diziyi dolaşarak elemanları yazdırma
for (int i = 0; i < sayilar.Length; i++)
{
    Console.WriteLine($"Dizinin {i}. elemanı: {sayilar[i]}");
}
```

### Geriye Doğru Sayma

```csharp
// 10'dan 1'e kadar geriye sayma
for (int i = 10; i >= 1; i--)
{
    Console.WriteLine(i);
}
Console.WriteLine("Bitti!");
```

### İç İçe For Döngüleri

```csharp
// Çarpım tablosu oluşturma
for (int i = 1; i <= 10; i++)
{
    for (int j = 1; j <= 10; j++)
    {
        Console.Write($"{i}x{j}={i*j}\t");
    }
    Console.WriteLine(); // Yeni satır
}
```

## For Döngüsünün Özel Kullanımları

### Boş Bileşenler

For döngüsündeki başlangıç, koşul veya artırma ifadeleri isteğe bağlıdır:

```csharp
// Başlangıç ifadesi dışarda tanımlanmış
int i = 0;
for (; i < 5; i++)
{
    Console.WriteLine(i);
}

// Artırma ifadesi döngü içinde yapılıyor
for (int j = 0; j < 5;)
{
    Console.WriteLine(j);
    j++;
}

// Sonsuz döngü (break ile sonlandırılmalı)
for (;;)
{
    Console.WriteLine("Sonsuz döngü!");
    // Bir noktada break kullanılmalı
    break;
}
```

### Birden Fazla Değişken Kullanma

```csharp
// İki değişkenli for döngüsü
for (int i = 0, j = 10; i < j; i++, j--)
{
    Console.WriteLine($"i = {i}, j = {j}");
}
```

### Kontrol İfadeleri ile Kullanım

```csharp
for (int i = 1; i <= 20; i++)
{
    if (i % 2 == 0)
    {
        Console.WriteLine($"{i} çift sayıdır.");
    }
    else if (i % 5 == 0)
    {
        Console.WriteLine($"{i} 5'in katıdır.");
        continue; // Döngünün geri kalan kısmını atla
    }
    
    if (i == 15)
    {
        Console.WriteLine("15'e ulaştık, döngüden çıkıyoruz.");
        break; // Döngüyü tamamen sonlandır
    }
}
```

## Foreach ile Karşılaştırma

For ve foreach döngüleri farklı amaçlar için kullanılır:

```csharp
string[] isimler = { "Ali", "Ayşe", "Mehmet", "Zeynep" };

// For döngüsü ile dizi işleme
for (int i = 0; i < isimler.Length; i++)
{
    Console.WriteLine($"{i+1}. kişi: {isimler[i]}");
}

// Aynı işlem foreach ile
foreach (string isim in isimler)
{
    Console.WriteLine(isim);
    // foreach'de indeks bilgisi doğrudan erişilebilir değildir
    // foreach içinde dizi elemanları değiştirilemez
}
```

**For vs Foreach**:
- For: İndeks gerektiğinde, elemanları değiştirmek gerektiğinde veya özel artırma/azaltma mantığı kullanılacaksa tercih edilir.
- Foreach: Sadece koleksiyondaki her elemana sırayla erişmek istendiğinde daha temiz ve okunaklıdır.

## İyi Programlama Uygulamaları

### Performans İpuçları

```csharp
// Dizi uzunluğunu her seferinde hesaplamak yerine saklamak
int[] buyukDizi = new int[10000];

// Daha Az Verimli
for (int i = 0; i < buyukDizi.Length; i++)
{
    // İşlemler...
}

// Daha Verimli
int uzunluk = buyukDizi.Length;
for (int i = 0; i < uzunluk; i++)
{
    // İşlemler...
}
```

### Okunabilirlik İpuçları

```csharp
// Anlamlı sayaç isimleri kullanma
for (int satir = 0; satir < 5; satir++)
{
    for (int sutun = 0; sutun < 3; sutun++)
    {
        Console.Write($"({satir},{sutun}) ");
    }
    Console.WriteLine();
}

// Karmaşık koşullarda açıklayıcı yorum eklemek
for (int i = 0; i < 100; i += 3) // 3'er 3'er artırıyoruz
{
    // İşlemler...
}
```

## Sık Yapılan Hatalar

### Sonsuz Döngüler

```csharp
// Hatalı - Sonsuz döngü
for (int i = 0; i >= 0; i++)
{
    Console.WriteLine(i);
    // i her zaman 0'dan büyük olacağı için döngü sonlanmaz
    // Bu tür durumlarda break kullanılmalı veya üst sınır belirlenmelidir
}
```

### Off-by-one Hataları

```csharp
int[] dizi = { 10, 20, 30, 40, 50 };

// Hatalı - Dizinin dışına çıkar
for (int i = 0; i <= dizi.Length; i++) // <= kullanılmamalı
{
    // i = dizi.Length olduğunda hata verir
    Console.WriteLine(dizi[i]);
}

// Doğru kullanım
for (int i = 0; i < dizi.Length; i++)
{
    Console.WriteLine(dizi[i]);
}
```

### Döngü İçinde Sayaç Değiştirme

```csharp
// Tehlikeli - Döngü içinde sayacı değiştirmek
for (int i = 0; i < 10; i++)
{
    Console.WriteLine(i);
    if (i == 5)
    {
        i = 8; // Döngü mantığını bozabilir, kaçınılmalıdır
    }
}
```

## Alıştırmalar

1. **Temel Alıştırma**: 1 ile 100 arasındaki çift sayıları yazdıran bir for döngüsü yazın.

```csharp
for (int i = 2; i <= 100; i += 2)
{
    Console.Write($"{i} ");
}
```

2. **Orta Seviye**: Asal sayıları bulan bir program yazın. 2 ile 100 arasındaki tüm asal sayıları yazdırın.

```csharp
for (int sayi = 2; sayi <= 100; sayi++)
{
    bool asalMi = true;
    
    for (int bolen = 2; bolen <= Math.Sqrt(sayi); bolen++)
    {
        if (sayi % bolen == 0)
        {
            asalMi = false;
            break;
        }
    }
    
    if (asalMi)
    {
        Console.Write($"{sayi} ");
    }
}
```

3. **İleri Seviye**: 10x10 boyutunda bir çarpım tablosu oluşturun ve ekrana formatlı şekilde yazdırın.

```csharp
// Başlık satırı
Console.Write("    |");
for (int i = 1; i <= 10; i++)
{
    Console.Write($"{i,4}");
}
Console.WriteLine("\n----+----------------------------------------");

// Çarpım tablosu
for (int i = 1; i <= 10; i++)
{
    Console.Write($"{i,3} |");
    for (int j = 1; j <= 10; j++)
    {
        Console.Write($"{i*j,4}");
    }
    Console.WriteLine();
}
```

---

Bu doküman, C# programlama dilindeki for döngüsünün temel ve ileri seviye kullanımları hakkında kapsamlı bir kılavuz sunmaktadır. Döngü yapıları, her programlama dilinin temel yapı taşlarından biridir ve C#'ta for döngüsünü etkili bir şekilde kullanmak, verimli ve okunaklı kod yazmanın önemli bir parçasıdır.
