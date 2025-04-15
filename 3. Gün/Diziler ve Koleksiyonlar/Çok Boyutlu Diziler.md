# C# Çok Boyutlu Diziler

## İçindekiler
1. [Giriş](#giriş)
2. [Çok Boyutlu Dizilerin Türleri](#çok-boyutlu-dizilerin-türleri)
   - [Düzenli Çok Boyutlu Diziler](#düzenli-çok-boyutlu-diziler)
   - [Düzensiz Diziler (Jagged Arrays)](#düzensiz-diziler-jagged-arrays)
3. [Çok Boyutlu Dizilerin Tanımlanması](#çok-boyutlu-dizilerin-tanımlanması)
4. [Çok Boyutlu Dizilerde Elemanlara Erişim](#çok-boyutlu-dizilerde-elemanlara-erişim)
5. [Çok Boyutlu Dizileri Döngülerle İşleme](#çok-boyutlu-dizileri-döngülerle-işleme)
6. [Çok Boyutlu Dizilerde Yaygın Metodlar](#çok-boyutlu-dizilerde-yaygın-metodlar)
7. [Örnek Uygulamalar](#örnek-uygulamalar)
8. [Performans Değerlendirmeleri](#performans-değerlendirmeleri)
9. [En İyi Kullanım Pratikleri](#en-iyi-kullanım-pratikleri)
10. [Sonuç](#sonuç)

## Giriş

Çok boyutlu diziler, verileri birden fazla boyutta organize etmek için kullanılan veri yapılarıdır. C#'ta çok boyutlu diziler, matematikteki matrisler gibi davranır ve satır ve sütunlar şeklinde düzenlenmiş veri koleksiyonlarını temsil eder. Bu diziler, özellikle tablo verileri, grafik işleme, oyun geliştirme ve bilimsel hesaplamalar gibi alanlarda yaygın olarak kullanılır.

C#'ta iki tür çok boyutlu dizi vardır:
1. **Düzenli çok boyutlu diziler** (Rectangular arrays)
2. **Düzensiz diziler** (Jagged arrays)

Bu belge, her iki tür dizinin de yapısını, kullanımını ve uygulamalarını detaylı bir şekilde incelemektedir.

## Çok Boyutlu Dizilerin Türleri

### Düzenli Çok Boyutlu Diziler

Düzenli çok boyutlu diziler, her bir boyutun aynı uzunlukta olduğu dizilerdir. Bu dizilerin her bir elemanı, dizinin boyutuna göre aynı sayıda indeks ile erişilebilir.

#### İki Boyutlu Diziler

İki boyutlu diziler, en yaygın kullanılan çok boyutlu dizi türüdür ve matematikteki matrislere benzer. Satır ve sütunlardan oluşur.

```csharp
// İki boyutlu dizi tanımlama
int[,] matris = new int[3, 4]; // 3 satır, 4 sütunluk bir dizi

// Başlangıç değerleriyle tanımlama
int[,] matris = new int[,] {
    {1, 2, 3, 4},
    {5, 6, 7, 8},
    {9, 10, 11, 12}
};

// Veya daha kısa şekilde
int[,] matris = {
    {1, 2, 3, 4},
    {5, 6, 7, 8},
    {9, 10, 11, 12}
};
```

#### Üç veya Daha Fazla Boyutlu Diziler

C#'ta 3 veya daha fazla boyutlu diziler de oluşturabilirsiniz. Ancak boyut arttıkça, bu dizileri görselleştirmek ve yönetmek zorlaşabilir.

```csharp
// Üç boyutlu dizi tanımlama
int[,,] kup = new int[3, 3, 3]; // 3x3x3 küp şeklinde bir dizi

// Başlangıç değerleriyle tanımlama
int[,,] kup = new int[,,] {
    { {1, 2, 3}, {4, 5, 6}, {7, 8, 9} },
    { {10, 11, 12}, {13, 14, 15}, {16, 17, 18} },
    { {19, 20, 21}, {22, 23, 24}, {25, 26, 27} }
};
```

### Düzensiz Diziler (Jagged Arrays)

Düzensiz diziler, "dizilerin dizisi" olarak da düşünülebilir. Bu tür dizilerde, her bir iç dizi farklı uzunluğa sahip olabilir. Düzensiz diziler, düzenli çok boyutlu dizilerden farklı olarak köşeli parantezlerin her boyut için ayrı ayrı kullanılmasıyla tanımlanır.

```csharp
// Düzensiz dizi tanımlama
int[][] duzensizDizi = new int[3][]; // 3 satırlı bir düzensiz dizi

// Her bir satırı ayrı ayrı tanımlama
duzensizDizi[0] = new int[2]; // İlk satır 2 elemanlı
duzensizDizi[1] = new int[4]; // İkinci satır 4 elemanlı
duzensizDizi[2] = new int[3]; // Üçüncü satır 3 elemanlı

// Başlangıç değerleriyle tanımlama
int[][] duzensizDizi = new int[][] {
    new int[] {1, 2},
    new int[] {3, 4, 5, 6},
    new int[] {7, 8, 9}
};
```

## Çok Boyutlu Dizilerin Tanımlanması

### Düzenli Çok Boyutlu Dizilerin Tanımlanması

```csharp
// Boyut belirtilerek tanımlama
tip[,] diziAdi = new tip[boyut1, boyut2];
tip[,,] diziAdi = new tip[boyut1, boyut2, boyut3];

// Başlangıç değerleriyle tanımlama
tip[,] diziAdi = new tip[,] { {deger1, deger2, ...}, {deger3, deger4, ...}, ... };
```

Örnek:

```csharp
// 2x3 boyutlu bir string dizisi oluşturma
string[,] isimler = new string[2, 3];

// 2x3 boyutlu bir dizi oluşturma ve değerler atama
int[,] sayilar = new int[,] {
    {1, 2, 3},
    {4, 5, 6}
};
```

### Düzensiz Dizilerin Tanımlanması

```csharp
// Ana diziyi tanımlama
tip[][] diziAdi = new tip[anaDiziBoyutu][];

// İç dizileri tanımlama
diziAdi[0] = new tip[icDiziBoyutu1];
diziAdi[1] = new tip[icDiziBoyutu2];
// ...

// Başlangıç değerleriyle tanımlama
tip[][] diziAdi = new tip[][] {
    new tip[] {deger1, deger2, ...},
    new tip[] {deger3, deger4, ...},
    // ...
};
```

Örnek:

```csharp
// 3 satırlı bir düzensiz dizi oluşturma
int[][] duzensizDizi = new int[3][];

// Her bir satırı farklı boyutlarda tanımlama
duzensizDizi[0] = new int[2] {1, 2};            // 2 elemanlı
duzensizDizi[1] = new int[4] {3, 4, 5, 6};      // 4 elemanlı
duzensizDizi[2] = new int[3] {7, 8, 9};         // 3 elemanlı
```

## Çok Boyutlu Dizilerde Elemanlara Erişim

### Düzenli Çok Boyutlu Dizilerde Elemanlara Erişim

Düzenli çok boyutlu dizilerde elemanlara erişmek için, her bir boyut için bir indeks belirtilir.

```csharp
// İki boyutlu dizide elemana erişim
deger = diziAdi[satırIndeks, sutunIndeks];

// Üç boyutlu dizide elemana erişim
deger = diziAdi[indeks1, indeks2, indeks3];
```

Örnek:

```csharp
int[,] matris = {
    {1, 2, 3},
    {4, 5, 6},
    {7, 8, 9}
};

// (1,2) konumundaki elemana erişim (ikinci satır, üçüncü sütun)
int eleman = matris[1, 2]; // eleman = 6

// Eleman değerini değiştirme
matris[0, 0] = 10; // Sol üst köşedeki eleman artık 10
```

### Düzensiz Dizilerde Elemanlara Erişim

Düzensiz dizilerde, her bir boyut için ayrı köşeli parantez kullanılır.

```csharp
// Düzensiz dizide elemana erişim
deger = diziAdi[anaDiziIndeks][icDiziIndeks];
```

Örnek:

```csharp
int[][] duzensizDizi = new int[][] {
    new int[] {1, 2},
    new int[] {3, 4, 5},
    new int[] {6, 7, 8, 9}
};

// İkinci satır, üçüncü elemana erişim
int eleman = duzensizDizi[1][2]; // eleman = 5

// Eleman değerini değiştirme
duzensizDizi[0][0] = 10; // İlk satır, ilk sütundaki eleman artık 10
```

## Çok Boyutlu Dizileri Döngülerle İşleme

### Düzenli Çok Boyutlu Dizileri Döngülerle İşleme

Çok boyutlu dizileri işlemek için genellikle iç içe döngüler kullanılır. İki boyutlu bir dizi için, dış döngü satırları, iç döngü ise sütunları dolaşır.

```csharp
int[,] matris = new int[3, 4];

// Matrisi doldurma
for (int i = 0; i < 3; i++)
{
    for (int j = 0; j < 4; j++)
    {
        matris[i, j] = i * 4 + j;
    }
}

// Matrisi yazdırma
for (int i = 0; i < 3; i++)
{
    for (int j = 0; j < 4; j++)
    {
        Console.Write(matris[i, j] + "\t");
    }
    Console.WriteLine();
}
```

#### GetLength Metodunun Kullanımı

Çok boyutlu dizilerde `GetLength(boyutNumarası)` metodu, belirtilen boyutun uzunluğunu döndürür. Bu, dizi boyutlarını kodda sabit olarak belirtmek yerine dinamik olarak kullanılmasını sağlar.

```csharp
int[,] matris = new int[3, 4];

// GetLength ile dinamik olarak dizi boyutlarını kullanma
for (int i = 0; i < matris.GetLength(0); i++) // matris.GetLength(0) = 3 (satır sayısı)
{
    for (int j = 0; j < matris.GetLength(1); j++) // matris.GetLength(1) = 4 (sütun sayısı)
    {
        matris[i, j] = i * matris.GetLength(1) + j;
        Console.Write(matris[i, j] + "\t");
    }
    Console.WriteLine();
}
```

### Düzensiz Dizileri Döngülerle İşleme

Düzensiz dizileri işlerken, her bir iç dizinin farklı boyuta sahip olabileceğini unutmamak gerekir.

```csharp
int[][] duzensizDizi = new int[3][];
duzensizDizi[0] = new int[2] {1, 2};
duzensizDizi[1] = new int[4] {3, 4, 5, 6};
duzensizDizi[2] = new int[3] {7, 8, 9};

// Düzensiz diziyi dolaşma
for (int i = 0; i < duzensizDizi.Length; i++)
{
    for (int j = 0; j < duzensizDizi[i].Length; j++)
    {
        Console.Write(duzensizDizi[i][j] + "\t");
    }
    Console.WriteLine();
}
```

#### foreach Döngüsüyle İşleme

Çok boyutlu dizileri `foreach` döngüsüyle de işleyebilirsiniz.

```csharp
// Düzenli çok boyutlu dizi için foreach
int[,] matris = {
    {1, 2, 3},
    {4, 5, 6},
    {7, 8, 9}
};

foreach (int eleman in matris)
{
    Console.Write(eleman + " ");
}
// Çıktı: 1 2 3 4 5 6 7 8 9

// Düzensiz dizi için foreach (iç içe)
int[][] duzensizDizi = new int[][] {
    new int[] {1, 2},
    new int[] {3, 4, 5},
    new int[] {6, 7, 8, 9}
};

foreach (int[] satir in duzensizDizi)
{
    foreach (int eleman in satir)
    {
        Console.Write(eleman + " ");
    }
    Console.WriteLine();
}
```

## Çok Boyutlu Dizilerde Yaygın Metodlar

C#'ta çok boyutlu diziler, `System.Array` sınıfından miras aldıkları bir dizi kullanışlı metoda sahiptir.

### 1. Array.GetLength()

Belirtilen boyutun uzunluğunu döndürür.

```csharp
int[,] matris = new int[3, 4];
int satirSayisi = matris.GetLength(0); // 3
int sutunSayisi = matris.GetLength(1); // 4
```

### 2. Array.Rank

Dizinin boyut sayısını döndürür.

```csharp
int[,] ikiBoytlu = new int[3, 4];
int[,,] ucBoyutlu = new int[3, 4, 5];

Console.WriteLine(ikiBoytlu.Rank); // 2
Console.WriteLine(ucBoyutlu.Rank); // 3
```

### 3. Array.Clear()

Bir dizinin tüm elemanlarını varsayılan değerlerine sıfırlar.

```csharp
int[,] matris = {
    {1, 2, 3},
    {4, 5, 6}
};

// Tüm diziyi temizle
Array.Clear(matris, 0, matris.Length);

// Belirli bir aralığı temizle
// Array.Clear(matris, başlangıçİndeksi, elemanSayısı);
```

### 4. Array.Copy()

Bir dizinin bir kısmını başka bir diziye kopyalar.

```csharp
int[,] kaynak = {
    {1, 2, 3},
    {4, 5, 6}
};

int[,] hedef = new int[2, 3];

// Kopyalama
Array.Copy(kaynak, hedef, kaynak.Length);
```

### 5. Clone()

Bir dizinin tam bir kopyasını oluşturur.

```csharp
int[,] orijinal = {
    {1, 2, 3},
    {4, 5, 6}
};

int[,] klon = (int[,])orijinal.Clone();
```

## Örnek Uygulamalar

### Örnek 1: Matris Çarpımı

İki matrisin çarpımını hesaplayan bir örnek:

```csharp
using System;

class Program
{
    static void Main()
    {
        int[,] matris1 = {
            {1, 2, 3},
            {4, 5, 6}
        }; // 2x3 matris

        int[,] matris2 = {
            {7, 8},
            {9, 10},
            {11, 12}
        }; // 3x2 matris

        // Sonuç matrisi boyutu: matris1'in satır sayısı x matris2'nin sütun sayısı
        int[,] sonuc = new int[matris1.GetLength(0), matris2.GetLength(1)]; // 2x2 matris

        // Matris çarpımı hesaplama
        for (int i = 0; i < matris1.GetLength(0); i++)
        {
            for (int j = 0; j < matris2.GetLength(1); j++)
            {
                sonuc[i, j] = 0;
                for (int k = 0; k < matris1.GetLength(1); k++)
                {
                    sonuc[i, j] += matris1[i, k] * matris2[k, j];
                }
            }
        }

        // Sonucu yazdırma
        Console.WriteLine("Matris Çarpımı Sonucu:");
        for (int i = 0; i < sonuc.GetLength(0); i++)
        {
            for (int j = 0; j < sonuc.GetLength(1); j++)
            {
                Console.Write(sonuc[i, j] + "\t");
            }
            Console.WriteLine();
        }
    }
}
```

### Örnek 2: Görüntü İşleme Simülasyonu

Bir pikselin komşularının ortalamasını alarak basit bir bulanıklaştırma işlemi:

```csharp
using System;

class Program
{
    static void Main()
    {
        // 5x5 bir görüntü oluşturma (gri tonlama değerleri 0-255 arası)
        int[,] goruntu = {
            {50, 60, 70, 80, 90},
            {55, 65, 75, 85, 95},
            {60, 70, 80, 90, 100},
            {65, 75, 85, 95, 105},
            {70, 80, 90, 100, 110}
        };

        Console.WriteLine("Orijinal Görüntü:");
        GoruntuyuYazdir(goruntu);

        // Bulanıklaştırılmış görüntü için yeni bir dizi oluştur
        int[,] bulanik = new int[goruntu.GetLength(0), goruntu.GetLength(1)];

        // Bulanıklaştırma işlemi (3x3 kernel kullanarak)
        for (int i = 1; i < goruntu.GetLength(0) - 1; i++)
        {
            for (int j = 1; j < goruntu.GetLength(1) - 1; j++)
            {
                int toplam = 0;
                
                // 3x3 komşuluk
                for (int ki = -1; ki <= 1; ki++)
                {
                    for (int kj = -1; kj <= 1; kj++)
                    {
                        toplam += goruntu[i + ki, j + kj];
                    }
                }
                
                // Ortalama değeri hesapla (9 komşu piksel)
                bulanik[i, j] = toplam / 9;
            }
        }

        // Kenar pikselleri kopyala (bu basit örnekte kenarları işlemiyoruz)
        for (int i = 0; i < goruntu.GetLength(0); i++)
        {
            bulanik[i, 0] = goruntu[i, 0];
            bulanik[i, goruntu.GetLength(1) - 1] = goruntu[i, goruntu.GetLength(1) - 1];
        }
        
        for (int j = 0; j < goruntu.GetLength(1); j++)
        {
            bulanik[0, j] = goruntu[0, j];
            bulanik[goruntu.GetLength(0) - 1, j] = goruntu[goruntu.GetLength(0) - 1, j];
        }

        Console.WriteLine("\nBulanıklaştırılmış Görüntü:");
        GoruntuyuYazdir(bulanik);
    }

    static void GoruntuyuYazdir(int[,] goruntu)
    {
        for (int i = 0; i < goruntu.GetLength(0); i++)
        {
            for (int j = 0; j < goruntu.GetLength(1); j++)
            {
                Console.Write(goruntu[i, j] + "\t");
            }
            Console.WriteLine();
        }
    }
}
```

### Örnek 3: Düzensiz Dizi ile Pascal Üçgeni

Pascal üçgenini düzensiz dizi kullanarak oluşturan bir örnek:

```csharp
using System;

class Program
{
    static void Main()
    {
        int satirSayisi = 10;
        int[][] pascalUcgeni = new int[satirSayisi][];

        // Pascal üçgenini oluştur
        for (int i = 0; i < satirSayisi; i++)
        {
            pascalUcgeni[i] = new int[i + 1];
            pascalUcgeni[i][0] = 1; // Her satırın ilk elemanı 1
            pascalUcgeni[i][i] = 1; // Her satırın son elemanı 1

            // Orta elemanları hesapla
            for (int j = 1; j < i; j++)
            {
                pascalUcgeni[i][j] = pascalUcgeni[i - 1][j - 1] + pascalUcgeni[i - 1][j];
            }
        }

        // Pascal üçgenini yazdır
        Console.WriteLine("Pascal Üçgeni:");
        for (int i = 0; i < pascalUcgeni.Length; i++)
        {
            // Hizalama için boşluk ekle
            for (int k = 0; k < satirSayisi - i; k++)
            {
                Console.Write("  ");
            }

            for (int j = 0; j < pascalUcgeni[i].Length; j++)
            {
                Console.Write("{0,4}", pascalUcgeni[i][j]);
            }
            Console.WriteLine();
        }
    }
}
```

## Performans Değerlendirmeleri

### Düzenli vs Düzensiz Diziler

Her iki dizi türünün de avantajları ve dezavantajları vardır:

#### Düzenli Çok Boyutlu Diziler
- **Avantajlar**:
  - Bellekte sürekli (contiguous) alan kullanır
  - Daha az bellek adresleme gerektirir
  - Düzenli veri yapıları için daha uygundur (matrisler, tablolar)
  
- **Dezavantajlar**:
  - Tüm boyutlar aynı uzunlukta olmalıdır
  - Satır veya sütun sayısı değiştirilemez

#### Düzensiz Diziler
- **Avantajlar**:
  - Her bir iç dizi farklı boyutta olabilir
  - Belleği daha verimli kullanabilir (gereksiz alanlar ayrılmaz)
  - Dinamik olarak boyutlandırılabilir
  
- **Dezavantajlar**:
  - Bellekte dağınık alanlar kullanır
  - Her bir iç diziye erişim için ek bir işlem gerekir
  - Daha fazla bellek adresleme gerektirir

### Bellek Kullanımı

```csharp
// Düzenli 10x10 dizi: 100 eleman için bellek ayrılır
int[,] duzenliDizi = new int[10, 10];

// Düzensiz dizi: İhtiyaç duyulan kadar eleman için bellek ayrılır
int[][] duzensizDizi = new int[5][];
duzensizDizi[0] = new int[3];  // 3 eleman
duzensizDizi[1] = new int[7];  // 7 eleman
duzensizDizi[2] = new int[2];  // 2 eleman
duzensizDizi[3] = new int[8];  // 8 eleman
duzensizDizi[4] = new int[4];  // 4 eleman
// Toplam: 24 eleman
```

## En İyi Kullanım Pratikleri

### Ne Zaman Düzenli Dizi Kullanmalı?

- Verilerin tüm boyutlarda aynı uzunlukta olduğu durumlar
- Matematiksel işlemler ve matris hesaplamaları
- Tablo veya grid benzeri veri yapıları
- Boyutların çalışma zamanında değişmediği durumlar

### Ne Zaman Düzensiz Dizi Kullanmalı?

- Her satırın farklı uzunlukta olabileceği durumlar
- Bellek kullanımının optimize edilmesi gereken durumlar
- Dinamik olarak genişleyen veri yapıları
- Özel veri yapıları (üçgen matrisler, seyrek matrisler)

### Genel Öneriler

1. **Doğru Dizi Türünü Seçin**: Uygulamanızın ihtiyaçlarına göre düzenli veya düzensiz dizi seçin.

2. **Sabit Boyutlardan Kaçının**: Dizi boyutlarını kodda sabit olarak belirtmek yerine, değişkenler veya yapılandırma dosyaları kullanın.

3. **GetLength() Kullanın**: Düzenli dizilerde boyutlara erişmek için her zaman `GetLength()` metodunu kullanın.

4. **Döngü Optimizasyonu**: İç içe döngülerde, en iç döngünün en yüksek performansla çalışmasını sağlayın.

5. **Bellek Yönetimi**: Büyük diziler oluştururken bellek kullanımını ve yaşam süresini göz önünde bulundurun.

6. **Dizi Sınırlarını Kontrol Edin**: Dizinin sınırları dışına erişim, çalışma zamanı hatalarına neden olur. Her zaman sınırları kontrol edin.

```csharp
// Güvenli erişim
if (i >= 0 && i < dizi.GetLength(0) && j >= 0 && j < dizi.GetLength(1))
{
    // Diziye güvenli erişim
    int deger = dizi[i, j];
}
```

## Sonuç

C#'ta çok boyutlu diziler, verileri organize etmek ve işlemek için güçlü araçlardır. Düzenli çok boyutlu diziler ve düzensiz diziler (jagged arrays) farklı senaryolarda avantajlar sunar.

Düzenli çok boyutlu diziler, düzenli veri yapıları ve matematiksel işlemler için idealdir. Düzensiz diziler ise, veri boyutları değişken olduğunda veya bellek optimizasyonu gerektiğinde daha iyi bir seçimdir.

Bu belge, C#'ta çok boyutlu dizilerin temel yapısını, kullanımını ve en iyi pratiklerini kapsamlı bir şekilde ele almıştır. Bu bilgiler, uygulamalarınızda çok boyutlu dizileri etkili bir şekilde kullanmanıza yardımcı olacaktır.
