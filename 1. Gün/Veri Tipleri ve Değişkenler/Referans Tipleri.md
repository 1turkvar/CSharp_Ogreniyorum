# C# Referans Tipleri

## İçindekiler
1. [Giriş](#giriş)
2. [Değer Tipleri ve Referans Tipleri Arasındaki Farklar](#değer-tipleri-ve-referans-tipleri-arasındaki-farklar)
3. [Temel Referans Tipleri](#temel-referans-tipleri)
   - [Class (Sınıf)](#class-sınıf)
   - [Interface (Arayüz)](#interface-arayüz)
   - [Delegate (Delege)](#delegate-delege)
   - [Array (Dizi)](#array-dizi)
   - [String (Metin)](#string-metin)
   - [Object (Nesne)](#object-nesne)
   - [Dynamic](#dynamic)
4. [Referans Tiplerinin Çalışma Mantığı](#referans-tiplerinin-çalışma-mantığı)
   - [Heap ve Stack Bellek](#heap-ve-stack-bellek)
   - [Garbage Collection (Çöp Toplama)](#garbage-collection-çöp-toplama)
5. [Referans Değişkenler](#referans-değişkenler)
   - [ref Anahtar Kelimesi](#ref-anahtar-kelimesi)
   - [out Anahtar Kelimesi](#out-anahtar-kelimesi)
   - [in Anahtar Kelimesi](#in-anahtar-kelimesi)
6. [Null Referanslar ve Null İşlemleri](#null-referanslar-ve-null-işlemleri)
   - [Null Kontrolü](#null-kontrolü)
   - [Null-Conditional Operatörü (?.)](#null-conditional-operatörü-)
   - [Null-Coalescing Operatörü (??)](#null-coalescing-operatörü-)
   - [Nullable Referans Tipleri](#nullable-referans-tipleri)
7. [Boxing ve Unboxing](#boxing-ve-unboxing)
8. [İyi Uygulamalar ve Performans İpuçları](#iyi-uygulamalar-ve-performans-ipuçları)
9. [Örnek Senaryolar](#örnek-senaryolar)
10. [Sonuç](#sonuç)

## Giriş

C# programlama dilinde, veri tipleri temelde iki ana kategoriye ayrılır: değer tipleri (value types) ve referans tipleri (reference types). Bu döküman, C# dilindeki referans tiplerini detaylı bir şekilde incelemektedir.

Referans tipleri, verinin kendisini doğrudan saklamak yerine, verinin bellekteki adresini (referansını) tutan tiplerdir. Bu özellik, referans tiplerinin davranışını, bellek yönetimini ve kullanımını değer tiplerinden farklı kılar.

## Değer Tipleri ve Referans Tipleri Arasındaki Farklar

Referans tiplerini daha iyi anlamak için, öncelikle değer tipleri ile arasındaki temel farklara bakalım:

| Özellik | Değer Tipleri | Referans Tipleri |
|---------|---------------|------------------|
| Depolama | Stack'te saklanır | Heap'te saklanır, stack'te sadece referansı tutulur |
| Atama | Değerin kendisi kopyalanır | Referans (bellek adresi) kopyalanır |
| Varsayılan değer | Tipin varsayılan değeri (örn. int için 0) | null |
| Bellek yönetimi | Kullanım sona erdiğinde otomatik temizlenir | Garbage Collector tarafından yönetilir |
| Örnek tipler | struct, enum, int, double, bool, char | class, interface, delegate, string, array, object |
| Eşitlik kontrolü | İçerik karşılaştırılır | Referans karşılaştırılır (aynı nesneyi işaret edip etmedikleri) |
| Boyut | Sabit ve önceden bilinir | Dinamik ve çalışma zamanında değişebilir |

## Temel Referans Tipleri

C# dilinde bulunan temel referans tipleri şunlardır:

### Class (Sınıf)

Sınıflar, nesne yönelimli programlamanın temel yapı taşlarıdır ve veriler ile davranışları bir arada kapsüllerler.

```csharp
// Sınıf tanımlama
public class Personel
{
    // Özellikler (Properties)
    public string Ad { get; set; }
    public string Soyad { get; set; }
    public int Yas { get; set; }
    
    // Metotlar
    public string TamAd()
    {
        return $"{Ad} {Soyad}";
    }
}

// Kullanımı
Personel personel1 = new Personel();
personel1.Ad = "Ahmet";
personel1.Soyad = "Yılmaz";

Personel personel2 = personel1; // Referans kopyalanır
personel2.Ad = "Mehmet"; // personel1.Ad değeri de "Mehmet" olur
```

### Interface (Arayüz)

Arayüzler, sınıfların uygulaması gereken sözleşmeleri tanımlar. Doğrudan örneği oluşturulamaz, ancak referans değişkenleri olarak kullanılabilirler.

```csharp
// Arayüz tanımlama
public interface IHesaplanabilir
{
    double Hesapla();
}

// Arayüzü uygulama
public class Dikdortgen : IHesaplanabilir
{
    public double Genislik { get; set; }
    public double Yukseklik { get; set; }
    
    public double Hesapla()
    {
        return Genislik * Yukseklik;
    }
}

// Kullanımı
IHesaplanabilir hesap = new Dikdortgen { Genislik = 5, Yukseklik = 3 };
double alan = hesap.Hesapla(); // 15
```

### Delegate (Delege)

Delegeler, metotlara referans tutan tiplerdir. Olay tabanlı programlama ve callback fonksiyonlarında sıklıkla kullanılırlar.

```csharp
// Delege tanımlama
public delegate void MesajHandler(string mesaj);

// Kullanımı
public class MesajService
{
    public void MesajGonder(string mesaj, MesajHandler handler)
    {
        // Mesaj gönderme işlemleri...
        handler(mesaj); // Callback'i çağır
    }
}

public class Program
{
    static void Main()
    {
        MesajService service = new MesajService();
        
        // Delege örneği oluşturma
        MesajHandler handler = MesajYazdir;
        
        // veya lambda ifadesi ile
        MesajHandler handler2 = (m) => Console.WriteLine($"Lambda: {m}");
        
        service.MesajGonder("Merhaba Dünya", handler);
        service.MesajGonder("Selam", handler2);
    }
    
    static void MesajYazdir(string mesaj)
    {
        Console.WriteLine($"Mesaj alındı: {mesaj}");
    }
}
```

### Array (Dizi)

Diziler, aynı tipte birden fazla öğeyi sıralı bir şekilde tutabilen referans tipleridir.

```csharp
// Dizi tanımlama
int[] sayilar = new int[5]; // 5 elemanlı bir dizi
sayilar[0] = 10;
sayilar[1] = 20;

// Alternatif tanımlama
string[] isimler = { "Ali", "Veli", "Ayşe", "Fatma" };

// Çok boyutlu diziler
int[,] matris = new int[3, 4]; // 3x4 matris
matris[0, 0] = 1;
matris[2, 3] = 15;

// Düzensiz diziler (jagged arrays)
int[][] duzensizDizi = new int[3][];
duzensizDizi[0] = new int[2] { 1, 2 };
duzensizDizi[1] = new int[4] { 3, 4, 5, 6 };
duzensizDizi[2] = new int[3] { 7, 8, 9 };
```

### String (Metin)

String, metinsel verileri tutan özel bir referans tipidir. C#'ta string'ler değiştirilemez (immutable) yapıdadır.

```csharp
// String oluşturma
string ad = "Ahmet";
string soyad = "Yılmaz";
string tamAd = ad + " " + soyad; // "Ahmet Yılmaz"

// String işlemleri
string buyuk = tamAd.ToUpper(); // "AHMET YILMAZ"
string kisim = tamAd.Substring(0, 5); // "Ahmet"
bool iceriyorMu = tamAd.Contains("met"); // true

// String değiştirilemezdir (immutable)
string s1 = "Merhaba";
string s2 = s1;
s1 = "Selam"; // s1 değişir ama s2 hala "Merhaba" değerini korur
```

### Object (Nesne)

Object tipi, tüm tiplerin temel sınıfıdır ve herhangi bir veri tipini tutabilir.

```csharp
// Object kullanımı
object obj1 = 42; // int değer
object obj2 = "Merhaba"; // string değer
object obj3 = new DateTime(2023, 4, 13); // DateTime nesnesi

// Tür dönüşümü gereklidir
int sayi = (int)obj1;
string metin = obj2 as string;
```

### Dynamic

Dynamic tipi, derleme zamanında tip kontrolü yapmayan ve çalışma zamanında dinamik olarak çözümlenen bir referans tipidir.

```csharp
// Dynamic kullanımı
dynamic deger = 100;
deger = "Şimdi bir string"; // Tip değişikliği sorun olmaz
deger = new List<int>(); // Dinamik olarak farklı bir tipe dönüşebilir

// Metot çağrıları çalışma zamanında çözümlenir
deger.Add(42); // Listeye eleman ekleme
Console.WriteLine(deger.Count); // 1
```

## Referans Tiplerinin Çalışma Mantığı

### Heap ve Stack Bellek

C#'ta bellek yönetimi, temel olarak iki farklı bölgede gerçekleşir:

1. **Stack (Yığın) Bellek**: Değer tipleri ve referans değişkenleri burada saklanır. LIFO (Last In, First Out) mantığıyla çalışır. Genellikle daha hızlı erişim sağlar.

2. **Heap (Öbek) Bellek**: Referans tiplerinin asıl verileri burada saklanır. Dinamik olarak yönetilir ve Garbage Collector tarafından temizlenir.

```
+-----------+           +------------+
|   Stack   |           |    Heap    |
+-----------+           +------------+
| int x = 5 |           | Person p1  |
+-----------+           |  {Ad="Ali",|
| Person p  |---------->|  Yas=30}   |
+-----------+           +------------+
                        | string s   |
                        | "Merhaba"  |
                        +------------+
```

Bir referans tipi oluşturulduğunda:
1. Heap'te nesne için bellek ayrılır
2. Nesnenin verileri bu bellek alanına yazılır
3. Stack'te, bu bellek adresini işaret eden bir referans değişkeni oluşturulur

### Garbage Collection (Çöp Toplama)

C#'ta, artık kullanılmayan nesneleri otomatik olarak temizleyen bir Garbage Collection mekanizması bulunur:

1. Bir nesneye artık hiçbir referans göstermiyorsa, bu nesne "erişilemez" duruma gelir
2. GC, belirli aralıklarla çalışarak bu erişilemez nesneleri tespit eder
3. Erişilemez nesneler heap bellekten temizlenir ve bellek boşaltılır

```csharp
void OrnekMetot()
{
    Personel p = new Personel(); // Heap'te nesne oluşturulur
    p.Ad = "Ali";
    
    // Metot sona erdiğinde, p değişkeni stack'ten silinir
    // Artık heap'teki Personel nesnesine erişim olmadığı için
    // bu nesne GC tarafından temizlenmeye aday olur
}
```

## Referans Değişkenler

C# 7.0 ile birlikte, değer tiplerini referans olarak kullanabilmek için ref, out ve in anahtar kelimeleri geliştirilmiştir.

### ref Anahtar Kelimesi

Bir değişkeni referans olarak iletmek ve metodun bu değişkenin orijinal değerini değiştirmesine izin vermek için kullanılır.

```csharp
void DegerDegistir(ref int x)
{
    x = x * 2; // Orijinal değişken değişir
}

int sayi = 10;
DegerDegistir(ref sayi);
Console.WriteLine(sayi); // 20
```

### out Anahtar Kelimesi

Metot içinde değer atanması gereken parametreler için kullanılır. Metot çağrılmadan önce değişkene değer atanması gerekmez.

```csharp
bool TryParse(string metin, out int sonuc)
{
    if (int.TryParse(metin, out sonuc))
    {
        return true;
    }
    sonuc = 0;
    return false;
}

// Kullanımı
TryParse("123", out int deger);
Console.WriteLine(deger); // 123
```

### in Anahtar Kelimesi

Bir değişkenin referans olarak iletilmesini sağlar ancak metodun bu değişkeni değiştirmesini engeller (salt okunur).

```csharp
void VeriGoster(in int x)
{
    // x = 20; // Hata! in parametresi değiştirilemez
    Console.WriteLine(x);
}

int sayi = 10;
VeriGoster(in sayi);
```

## Null Referanslar ve Null İşlemleri

### Null Kontrolü

Referans tipleri null değer alabilir, bu da "hiçbir nesneyi işaret etmiyor" anlamına gelir. Null bir referansa erişmeye çalışmak, `NullReferenceException` hatasına neden olur.

```csharp
Personel p = null;
// p.Ad = "Ali"; // NullReferenceException hatası verir

// Null kontrolü yapılmalıdır
if (p != null)
{
    p.Ad = "Ali";
}
```

### Null-Conditional Operatörü (?.)

Bir referansın null olup olmadığını kontrol eder ve null ise işlemi atlar.

```csharp
Personel p = null;
string ad = p?.Ad; // p null olduğu için ad da null olur

List<string> liste = null;
int? uzunluk = liste?.Count; // uzunluk null olur
```

### Null-Coalescing Operatörü (??)

Bir değişken null ise alternatif bir değer döndürmek için kullanılır.

```csharp
Personel p = null;
string ad = p?.Ad ?? "İsimsiz"; // p?.Ad null olduğu için "İsimsiz" atanır

// Nesne atamaları için de kullanılabilir
Personel aktifPersonel = p ?? new Personel { Ad = "Varsayılan" };
```

### Nullable Referans Tipleri

C# 8.0 ile birlikte, referans tiplerinin null olabilirliğini belirtmek için nullable referans tipleri özelliği eklenmiştir.

```csharp
// Nullable referans tiplerini etkinleştirmek için:
// <Nullable>enable</Nullable> (csproj dosyasında)
// veya
// #nullable enable (dosya başında)

// Non-nullable referans tipi (null olamaz)
string isim = "Ali";
// isim = null; // Uyarı verir

// Nullable referans tipi (null olabilir)
string? soyisim = "Yılmaz";
soyisim = null; // Geçerli
```

## Boxing ve Unboxing

Boxing, bir değer tipinin object tipine (referans tipi) dönüştürülmesi işlemidir. Unboxing ise bunun tersidir.

```csharp
// Boxing
int sayi = 42;
object obj = sayi; // int değeri object'e (referans tipine) dönüştürülür

// Unboxing
int geriDonusum = (int)obj; // Explicit cast gereklidir
```

Boxing/unboxing işlemleri performans açısından maliyetlidir, çünkü:
1. Boxing sırasında heap'te yeni bir nesne oluşturulur
2. Unboxing sırasında tip dönüşümü kontrolü yapılır

## İyi Uygulamalar ve Performans İpuçları

1. **Gereksiz Boxing/Unboxing'den Kaçının**:
   ```csharp
   // Kötü kullanım
   ArrayList liste = new ArrayList();
   liste.Add(5); // Boxing gerçekleşir
   
   // İyi kullanım
   List<int> liste = new List<int>();
   liste.Add(5); // Boxing gerçekleşmez
   ```

2. **Büyük Referans Nesnelerini using ile Yönetin**:
   ```csharp
   using (FileStream fs = new FileStream("dosya.txt", FileMode.Open))
   {
       // İşlemler...
   } // fs otomatik olarak dispose edilir
   ```

3. **Null Kontrollerini Doğru Yapın**:
   ```csharp
   // C# 8.0 öncesi
   if (obj != null && obj.Property != null)
   {
       // Güvenli erişim
   }
   
   // C# 8.0 ve sonrası
   if (obj?.Property is not null)
   {
       // Güvenli erişim
   }
   ```

4. **Immutable String Özelliğine Dikkat Edin**:
   ```csharp
   // Kötü kullanım (her + işlemi yeni bir string nesnesi oluşturur)
   string mesaj = "";
   for (int i = 0; i < 1000; i++)
   {
       mesaj += i.ToString();
   }
   
   // İyi kullanım
   StringBuilder sb = new StringBuilder();
   for (int i = 0; i < 1000; i++)
   {
       sb.Append(i);
   }
   string mesaj = sb.ToString();
   ```

5. **Referans Tiplerini Karşılaştırırken Dikkatli Olun**:
   ```csharp
   // Referans karşılaştırma (aynı nesne mi?)
   bool ayniNesneMi = object.ReferenceEquals(obj1, obj2);
   
   // İçerik karşılaştırma
   bool ayniIcerikMi = obj1.Equals(obj2);
   ```

## Örnek Senaryolar

### Senaryo 1: Referans Tipi Atama

```csharp
public class Program
{
    static void Main()
    {
        // İki ayrı Personel nesnesi oluşturalım
        Personel p1 = new Personel { Ad = "Ali", Yas = 30 };
        Personel p2 = new Personel { Ad = "Veli", Yas = 25 };
        
        Console.WriteLine($"p1: {p1.Ad}, {p1.Yas}");
        Console.WriteLine($"p2: {p2.Ad}, {p2.Yas}");
        
        // p2 referansını p1'e atayalım
        p2 = p1;
        
        // p1 üzerinden değişiklik yapalım
        p1.Ad = "Ahmet";
        
        // Her iki referans da aynı nesneyi işaret ettiği için
        // p2.Ad değeri de "Ahmet" olacaktır
        Console.WriteLine($"p1: {p1.Ad}, {p1.Yas}");
        Console.WriteLine($"p2: {p2.Ad}, {p2.Yas}");
    }
}

public class Personel
{
    public string Ad { get; set; }
    public int Yas { get; set; }
}
```

### Senaryo 2: Referans Değişmeyen Metot Parametreleri

```csharp
public class Program
{
    static void Main()
    {
        Personel p = new Personel { Ad = "Ali", Yas = 30 };
        Console.WriteLine($"Metot çağrısı öncesi: {p.Ad}, {p.Yas}");
        
        YasArttir(p);
        Console.WriteLine($"Metot çağrısı sonrası: {p.Ad}, {p.Yas}");
        
        PersonelDegistir(p);
        Console.WriteLine($"PersonelDegistir sonrası: {p.Ad}, {p.Yas}");
    }
    
    static void YasArttir(Personel p)
    {
        // Referans edilen nesnenin özelliği değişir
        p.Yas += 5;
        Console.WriteLine($"Metot içinde: {p.Ad}, {p.Yas}");
    }
    
    static void PersonelDegistir(Personel p)
    {
        // Burada p referansı yeni bir nesneye atanır
        // ancak bu değişiklik metot dışına yansımaz
        p = new Personel { Ad = "Veli", Yas = 40 };
        Console.WriteLine($"PersonelDegistir içinde: {p.Ad}, {p.Yas}");
    }
}

public class Personel
{
    public string Ad { get; set; }
    public int Yas { get; set; }
}
```

### Senaryo 3: IDisposable Arayüzü ve using Deyimi

```csharp
public class Program
{
    static void Main()
    {
        // using deyimi ile otomatik dispose
        using (KaynakYoneticisi kaynak = new KaynakYoneticisi("Dosya1"))
        {
            kaynak.Islem();
        } // Blok sonunda Dispose() metodu otomatik çağrılır
        
        // Manuel kaynak yönetimi
        KaynakYoneticisi kaynak2 = null;
        try
        {
            kaynak2 = new KaynakYoneticisi("Dosya2");
            kaynak2.Islem();
        }
        finally
        {
            kaynak2?.Dispose();
        }
    }
}

public class KaynakYoneticisi : IDisposable
{
    private bool disposed = false;
    private string kaynakAdi;
    
    public KaynakYoneticisi(string ad)
    {
        kaynakAdi = ad;
        Console.WriteLine($"{kaynakAdi} kaynağı açıldı.");
    }
    
    public void Islem()
    {
        if (disposed)
            throw new ObjectDisposedException(kaynakAdi);
            
        Console.WriteLine($"{kaynakAdi} üzerinde işlem yapılıyor...");
    }
    
    public void Dispose()
    {
        Dispose(true);
        GC.SuppressFinalize(this);
    }
    
    protected virtual void Dispose(bool disposing)
    {
        if (!disposed)
        {
            if (disposing)
            {
                // Yönetilen kaynakları temizle
            }
            
            // Yönetilmeyen kaynakları temizle
            Console.WriteLine($"{kaynakAdi} kaynağı kapandı.");
            disposed = true;
        }
    }
    
    ~KaynakYoneticisi()
    {
        Dispose(false);
    }
}
```

## Sonuç

C#'ta referans tipleri, nesne yönelimli programlamanın temelini oluşturur ve karmaşık uygulamaların geliştirilmesinde kritik bir rol oynar. Referans tiplerinin doğru anlaşılması ve etkin kullanılması, performanslı ve hatasız uygulamalar geliştirmek için büyük önem taşır.

Bu belgede, C#'taki temel referans tiplerini, bellek yönetimini, null işlemlerini ve iyi uygulamaları inceledik. Bu bilgiler, C# programlama dilinde daha etkin ve güvenli kodlar yazmanıza yardımcı olacaktır.

Referans tiplerinin davranışlarını ve özelliklerini anlamak, özellikle büyük ve karmaşık projelerde hata ayıklama ve performans optimizasyonu yapabilmek için kritik öneme sahiptir.

Daha derinlemesine bilgi için, Microsoft'un resmi C# dokümantasyonunu incelemenizi öneririm.
