# C# Parametre Türleri: ref, out, params

## İçindekiler
1. [Giriş](#giriş)
2. [Değer ve Referans Tipleri](#değer-ve-referans-tipleri)
3. [ref Parametresi](#ref-parametresi)
4. [out Parametresi](#out-parametresi)
5. [params Parametresi](#params-parametresi)
6. [in Parametresi](#in-parametresi)
7. [Parametre Türlerinin Karşılaştırılması](#parametre-türlerinin-karşılaştırılması)
8. [En İyi Kullanım Pratikleri](#en-iyi-kullanım-pratikleri)
9. [Özet](#özet)

## Giriş

C# programlama dilinde, bir metoda parametre geçirmenin farklı yolları vardır. Varsayılan olarak, değer tipleri (int, float, struct vb.) değer olarak, referans tipleri (class, interface, delegate vb.) ise referans olarak geçirilir. Ancak C#, bu davranışı değiştirmenize ve parametrelerin nasıl işleneceğini kontrol etmenize olanak sağlayan özel anahtar kelimeler sunar: `ref`, `out` ve `params`.

Bu anahtar kelimeler, C# programcılarına kodlarının işlevselliğini ve performansını artırmak için güçlü araçlar sağlar.

## Değer ve Referans Tipleri

Parametre türlerini anlamadan önce, C#'ta değer ve referans tiplerinin nasıl çalıştığını bilmek önemlidir:

### Değer Tipleri
- `int`, `float`, `double`, `bool`, `char`, `struct`, `enum` gibi tipler
- Stack bellek bölgesinde saklanır
- Bir değişkenden diğerine atanırken değerin kendisi kopyalanır
- Bir metoda geçirildiğinde, değerin bir kopyası oluşturulur

### Referans Tipleri
- `class`, `interface`, `delegate`, `array`, `string` gibi tipler
- Heap bellek bölgesinde saklanır
- Bir değişkenden diğerine atanırken, nesnenin bellek adresinin referansı kopyalanır
- Bir metoda geçirildiğinde, referansın bir kopyası oluşturulur, ancak her iki referans da aynı nesneye işaret eder

## ref Parametresi

`ref` anahtar kelimesi, değer tiplerini referans olarak geçirmenizi sağlar. Bu, metodun orijinal değişken üzerinde doğrudan çalışabileceği ve değişiklik yapabileceği anlamına gelir.

### Sözdizimi

```csharp
void MetodAdı(ref TipAdı parametreAdı)
{
    // Metodun gövdesi
}

// Çağrı
TipAdı değişken = değer;
MetodAdı(ref değişken);
```

### Özellikleri

1. Parametrenin metoda geçirilmeden önce başlatılması (initialize) zorunludur
2. Metod içinde değiştirilen değer, çağrıldığı yerde de değişir
3. Hem değer hem de referans tipleri ile kullanılabilir
4. Metod çağrısında `ref` anahtar kelimesinin kullanılması zorunludur

### Örnek

```csharp
void Swap(ref int x, ref int y)
{
    int temp = x;
    x = y;
    y = temp;
}

int a = 5, b = 10;
Console.WriteLine($"Değiştirmeden önce: a = {a}, b = {b}");
Swap(ref a, ref b);
Console.WriteLine($"Değiştirdikten sonra: a = {a}, b = {b}");

// Çıktı:
// Değiştirmeden önce: a = 5, b = 10
// Değiştirdikten sonra: a = 10, b = 5
```

### Kullanım Senaryoları

- İki veya daha fazla değeri değiştirmek (swap)
- Bir metodun birden fazla değeri değiştirmesi gerektiğinde
- Büyük struct tiplerini kopyalamadan metoda geçirmek istediğinizde (performans iyileştirmesi)

## out Parametresi

`out` anahtar kelimesi, bir metodun çıktı değerlerini döndürmesini sağlar. `ref` parametresine benzer, ancak metod içinde değerin atanması zorunludur.

### Sözdizimi

```csharp
void MetodAdı(out TipAdı parametreAdı)
{
    // Metod içinde parametreye değer atanmalıdır
    parametreAdı = değer;
}

// Çağrı
TipAdı değişken; // Başlatılması zorunlu değil
MetodAdı(out değişken);
```

### Özellikleri

1. Parametrenin metoda geçirilmeden önce başlatılması zorunlu değildir
2. Metod içinde parametreye değer atanması zorunludur (tüm kod yollarında)
3. Metod çağrısında `out` anahtar kelimesinin kullanılması zorunludur
4. Hem değer hem de referans tipleri ile kullanılabilir

### Örnek

```csharp
bool TryParse(string input, out int result)
{
    try
    {
        result = int.Parse(input);
        return true;
    }
    catch
    {
        result = 0; // Değer atanması zorunludur
        return false;
    }
}

string input = "123";
if (TryParse(input, out int sonuc))
{
    Console.WriteLine($"Dönüştürme başarılı: {sonuc}");
}
else
{
    Console.WriteLine("Dönüştürme başarısız");
}

// Çıktı:
// Dönüştürme başarılı: 123
```

### C# 7.0 ve Sonrası İyileştirmeler

C# 7.0 ile birlikte, `out` parametrelerini doğrudan metod çağrısı sırasında tanımlayabilirsiniz:

```csharp
if (int.TryParse("123", out int result))
{
    Console.WriteLine($"Sonuç: {result}");
}
```

Ayrıca, değeri kullanmayacaksanız discard operatörü ile göz ardı edebilirsiniz:

```csharp
if (int.TryParse("123", out _))
{
    Console.WriteLine("Dönüştürme başarılı");
}
```

### Kullanım Senaryoları

- Bir metodun başarı/başarısızlık durumunu döndürürken aynı zamanda bir değer de döndürmesi gerektiğinde
- Birden fazla değer döndürmek gerektiğinde
- Factory metod desenleri
- `TryParse` gibi dönüşüm metodları

## params Parametresi

`params` anahtar kelimesi, bir metoda değişken sayıda argüman geçirmenizi sağlar. Bu parametreler, tek bir dizi parametresi olarak işlenir.

### Sözdizimi

```csharp
void MetodAdı(params TipAdı[] parametreAdı)
{
    // Metodun gövdesi
}

// Çağrı yöntemleri
MetodAdı(değer1, değer2, değer3); // Virgülle ayrılmış değerler
MetodAdı(yeniDizi); // Önceden oluşturulmuş bir dizi
MetodAdı(); // Parametre geçmeden
```

### Özellikleri

1. Her metodda yalnızca bir `params` parametresi olabilir
2. `params` parametresi, metodun son parametresi olmalıdır
3. `params` anahtar kelimesi yalnızca tek boyutlu dizilerle kullanılabilir

### Örnek

```csharp
int Topla(params int[] sayilar)
{
    int toplam = 0;
    foreach (int sayi in sayilar)
    {
        toplam += sayi;
    }
    return toplam;
}

// Farklı çağrı yöntemleri
Console.WriteLine(Topla(1, 2, 3, 4, 5)); // 15
Console.WriteLine(Topla(10, 20)); // 30

int[] dizi = { 1, 2, 3, 4, 5 };
Console.WriteLine(Topla(dizi)); // 15

Console.WriteLine(Topla()); // 0
```

### Kullanım Senaryoları

- Farklı sayıda argüman alabilen metodlar oluşturmak
- String.Format, Console.WriteLine gibi metod çağrılarını basitleştirmek
- İşlemleri birden çok öğe üzerinde gerçekleştirmek (toplama, birleştirme vb.)

## in Parametresi

`in` anahtar kelimesi, C# 7.2 ile eklenmiştir ve `ref` parametresinin salt okunur versiyonudur. Büyük struct tiplerini kopyalamadan, ancak değiştirilmesini önleyerek metoda geçirmenizi sağlar.

### Sözdizimi

```csharp
void MetodAdı(in TipAdı parametreAdı)
{
    // parametreAdı değiştirilemez
    // parametreAdı = yeniDeğer; // Derleme hatası
}

// Çağrı
TipAdı değişken = değer;
MetodAdı(in değişken); // veya sadece MetodAdı(değişken);
```

### Özellikleri

1. Parametrenin değeri metod içinde değiştirilemez
2. Büyük struct tipleri için performans avantajı sağlar
3. Çağrı yaparken `in` anahtar kelimesi isteğe bağlıdır

### Örnek

```csharp
void MesafeHesapla(in Vector3D nokta1, in Vector3D nokta2)
{
    // nokta1 ve nokta2 değiştirilemez
    double mesafe = Math.Sqrt(
        Math.Pow(nokta2.X - nokta1.X, 2) +
        Math.Pow(nokta2.Y - nokta1.Y, 2) +
        Math.Pow(nokta2.Z - nokta1.Z, 2)
    );
    
    Console.WriteLine($"Mesafe: {mesafe}");
}

struct Vector3D
{
    public double X { get; set; }
    public double Y { get; set; }
    public double Z { get; set; }
    
    public Vector3D(double x, double y, double z)
    {
        X = x;
        Y = y;
        Z = z;
    }
}

var nokta1 = new Vector3D(0, 0, 0);
var nokta2 = new Vector3D(3, 4, 0);

MesafeHesapla(in nokta1, in nokta2); // "in" kullanarak
MesafeHesapla(nokta1, nokta2); // "in" kullanmadan da çalışır
```

### Kullanım Senaryoları

- Büyük struct tiplerinin performansını iyileştirmek
- Salt okunur veri yapılarını geçirmek
- Değiştirilmemesi gereken parametreleri belirtmek

## Parametre Türlerinin Karşılaştırılması

| Özellik | Normal | ref | out | in | params |
|---------|--------|-----|-----|------|--------|
| Değer atanması zorunlu mu? | Hayır | Evet (çağrı öncesi) | Evet (metod içinde) | Evet (çağrı öncesi) | Hayır |
| Metod içinde değiştirilebilir mi? | Evet (yerel kopya) | Evet (orijinal değer) | Evet (orijinal değer) | Hayır | Evet (dizi elemanları) |
| Anahtar kelime çağrıda zorunlu mu? | Hayır | Evet | Evet | Hayır | Hayır |
| Birden fazla değer alabilir mi? | Hayır | Hayır | Hayır | Hayır | Evet |
| Parametrenin pozisyonu | Herhangi | Herhangi | Herhangi | Herhangi | Son parametre |
| Metod overloading'i etkiler mi? | Hayır | Evet | Evet | Evet | Hayır |

## En İyi Kullanım Pratikleri

### ref Kullanırken
- Sadece gerektiğinde kullanın; normal parametreler çoğu durumda daha okunabilir
- Metod adında değişiklik yapılacağını belirtin (örn. `SwapValues`)
- Performans iyileştirmesi için büyük struct'ları geçerken kullanın

### out Kullanırken
- Metod adı, çıktı parametresinin ne olduğunu belirtmeli (örn. `TryParse`, `TryGetValue`)
- Başarı/başarısızlık durumunu boolean dönüş değeri ile belirtin
- C# 7.0+ sürümlerinde inline değişken tanımlama özelliğini kullanın

### params Kullanırken
- Parametrelerin aynı tipte olduğundan emin olun
- Performans kritik durumlarda dikkatli kullanın (arka planda dizi yaratılır)
- Overload oluşturmak yerine tercih edin

### in Kullanırken
- Büyük struct tipleri (16 byte veya daha büyük) için kullanın
- Çağrı yapan kodda `in` anahtar kelimesini kullanmak isteğe bağlıdır, ancak okunabilirlik için kullanabilirsiniz
- Salt okunur veri yapıları için tercih edin

## Özet

C# parametre türleri, metodların veri ile nasıl etkileşime gireceğini kontrol etmenize olanak tanır:

- **Normal parametre**: Değer tipleri değer olarak, referans tipleri referans olarak geçirilir.
- **ref**: Değişkeni referans olarak geçirir, metod içinde değişiklik yapılabilir, değişiklikler orijinal değişkene yansır.
- **out**: Değişkeni referans olarak geçirir, metod içinde değer atanması zorunludur, çağrı öncesi değer atanması zorunlu değildir.
- **in**: Değişkeni referans olarak geçirir ancak salt okunurdur, metod içinde değiştirilemez.
- **params**: Değişken sayıda argüman geçirmeyi sağlar, tek bir dizi parametresi olarak işlenir.

Bu parametre türlerini doğru yerlerde kullanarak, C# kodunuzun okunabilirliğini, esnekliğini ve performansını artırabilirsiniz.
