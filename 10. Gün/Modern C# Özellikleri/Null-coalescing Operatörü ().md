# C# Null-coalescing Operatörü (??) Rehberi

## İçindekiler
1. [Giriş](#giriş)
2. [Temel Kullanım](#temel-kullanım)
3. [Çalışma Prensibi](#çalışma-prensibi)
4. [Farklı Veri Tipleriyle Kullanım](#farklı-veri-tipleriyle-kullanım)
5. [Null-coalescing Operatörünü Zincirleme](#null-coalescing-operatörünü-zincirleme)
6. [Null-coalescing Atama Operatörü (??=)](#null-coalescing-atama-operatörü)
7. [Null-conditional Operatörü (?.) ile Birlikte Kullanım](#null-conditional-operatörü-ile-birlikte-kullanım)
8. [En İyi Kullanım Örnekleri](#en-iyi-kullanım-örnekleri)
9. [Yaygın Hatalar ve Dikkat Edilmesi Gerekenler](#yaygın-hatalar-ve-dikkat-edilmesi-gerekenler)
10. [Örnek Senaryolar](#örnek-senaryolar)

## Giriş

Null-coalescing operatörü (`??`), C# dilinde null değer kontrolü yapmak ve alternatif bir değer sağlamak için kullanılan güçlü bir özelliktir. C# 2.0 ile birlikte dile eklenmiştir ve o zamandan beri null referans durumlarını yönetmenin en temiz ve etkili yollarından biri olmuştur.

Bu operatör, null olabilecek bir değeri kontrol eder ve eğer değer null ise, belirtilen alternatif değeri döndürür. Bu, kodun daha kısa, daha okunabilir ve daha güvenli olmasını sağlar.

## Temel Kullanım

Null-coalescing operatörünün temel sözdizimi şu şekildedir:

```csharp
değişken ?? alternatifDeğer
```

Bu ifade şu şekilde değerlendirilir:
- Eğer `değişken` null değilse, `değişken`'in değeri döndürülür.
- Eğer `değişken` null ise, `alternatifDeğer` döndürülür.

### Basit Örnek

```csharp
string isim = null;
string gösterilecekİsim = isim ?? "İsimsiz Kullanıcı";

Console.WriteLine(gösterilecekİsim);  // "İsimsiz Kullanıcı" yazdırır

isim = "Ahmet";
gösterilecekİsim = isim ?? "İsimsiz Kullanıcı";

Console.WriteLine(gösterilecekİsim);  // "Ahmet" yazdırır
```

## Çalışma Prensibi

Null-coalescing operatörünün çalışma prensibi aslında oldukça basittir, ancak güçlü bir soyutlama sağlar. Operatör, şu if-else yapısının daha kısa bir versiyonudur:

```csharp
// Bu kod:
result = value ?? defaultValue;

// Aşağıdaki if-else yapısına eşdeğerdir:
if (value != null)
    result = value;
else
    result = defaultValue;
```

Bu operatör, null kontrolü için tekrarlanan kod yazımını azaltır ve kodun okunabilirliğini artırır.

## Farklı Veri Tipleriyle Kullanım

Null-coalescing operatörü, null olabilen (nullable) tüm veri tipleriyle çalışabilir:

### Referans Tipleri

```csharp
string mesaj = null;
string gösterilecekMesaj = mesaj ?? "Varsayılan mesaj";
```

### Nullable Değer Tipleri

```csharp
int? sayı = null;
int değer = sayı ?? 0;  // sayı null olduğu için değer 0 olur

sayı = 42;
değer = sayı ?? 0;  // sayı null olmadığı için değer 42 olur
```

### Karmaşık Nesneler

```csharp
Kullanıcı aktifKullanıcı = null;
Kullanıcı gösterilecekKullanıcı = aktifKullanıcı ?? new Kullanıcı { İsim = "Misafir" };
```

## Null-coalescing Operatörünü Zincirleme

Null-coalescing operatörü zincirleme kullanılabilir, bu da birden fazla alternatif değer belirlemenize olanak tanır:

```csharp
string isim = null;
string kullanıcıAdı = null;
string gösterilecekİsim = isim ?? kullanıcıAdı ?? "İsimsiz Kullanıcı";

// İlk null olmayan değeri kullanır, hepsi null ise son alternatifi alır
Console.WriteLine(gösterilecekİsim);  // "İsimsiz Kullanıcı" yazdırır

kullanıcıAdı = "ahmet123";
gösterilecekİsim = isim ?? kullanıcıAdı ?? "İsimsiz Kullanıcı";
Console.WriteLine(gösterilecekİsim);  // "ahmet123" yazdırır

isim = "Ahmet Yılmaz";
gösterilecekİsim = isim ?? kullanıcıAdı ?? "İsimsiz Kullanıcı";
Console.WriteLine(gösterilecekİsim);  // "Ahmet Yılmaz" yazdırır
```

## Null-coalescing Atama Operatörü (??=)

C# 8.0 ile birlikte `??=` operatörü tanıtıldı. Bu operatör, bir değişken null ise, sağ taraftaki değeri o değişkene atar:

```csharp
// Bu kod:
değişken ??= değer;

// Aşağıdakine eşdeğerdir:
if (değişken == null)
    değişken = değer;
```

### Örnek

```csharp
string mesaj = null;
mesaj ??= "Varsayılan mesaj";
Console.WriteLine(mesaj);  // "Varsayılan mesaj" yazdırır

// Artık mesaj null olmadığından, aşağıdaki atama işlemi gerçekleşmez
mesaj ??= "Yeni mesaj";
Console.WriteLine(mesaj);  // Hala "Varsayılan mesaj" yazdırır
```

## Null-conditional Operatörü (?.) ile Birlikte Kullanım

Null-coalescing operatörü (`??`), null-conditional operatörü (`?.`) ile birlikte kullanıldığında çok güçlü bir kombinasyon oluşturur:

```csharp
// Kullanıcı null ise veya Kullanıcı.İsim null ise "İsimsiz" yazdırır
string gösterilecekİsim = kullanıcı?.İsim ?? "İsimsiz";

// Aşağıdaki if-else yapısına eşdeğerdir:
string gösterilecekİsim;
if (kullanıcı == null || kullanıcı.İsim == null)
    gösterilecekİsim = "İsimsiz";
else
    gösterilecekİsim = kullanıcı.İsim;
```

### Koleksiyonlarla Kullanım

```csharp
// Liste null ise veya boş ise "Liste boş" yazdırır
string mesaj = liste?.Count > 0 ? "Liste dolu" : "Liste boş";

// Daha güvenli bir yaklaşım (liste null kontrolü yapar)
string mesaj = (liste?.Count ?? 0) > 0 ? "Liste dolu" : "Liste boş";
```

## En İyi Kullanım Örnekleri

Null-coalescing operatörünün en yaygın ve etkili kullanım alanları şunlardır:

### 1. Varsayılan Değerler Belirlemek

```csharp
public void İşlem(string parametre = null)
{
    string değer = parametre ?? "varsayılan";
    // değer ile çalış...
}
```

### 2. Yapılandırma Değerlerini Yönetmek

```csharp
int zamanaşımı = config?.Timeout?.Seconds ?? 30;
```

### 3. Boş Koleksiyonlardan Kaçınmak

```csharp
public IEnumerable<Ürün> ÜrünleriGetir()
{
    var ürünler = veritabanı.Ürünler;
    return ürünler ?? Enumerable.Empty<Ürün>();
}
```

### 4. Hata Mesajlarını Standartlaştırmak

```csharp
string hataMesajı = exception?.Message ?? "Bilinmeyen bir hata oluştu";
```

## Yaygın Hatalar ve Dikkat Edilmesi Gerekenler

### 1. Performans Etkileri

Null-coalescing operatörünün sağ tarafındaki ifade, sol taraf null değilse değerlendirilmez (lazy evaluation). Bu, performans açısından olumlu bir özelliktir, ancak bu davranışın farkında olmak önemlidir.

```csharp
// TümÜrünleriGetir() çağrısı yalnızca önbellek null ise yapılır
var ürünler = önbellek ?? TümÜrünleriGetir();
```

### 2. Boş String vs Null

Boş string (`""`) null değildir, bu nedenle null-coalescing operatörü boş stringler için alternatif değer ataması yapmaz:

```csharp
string isim = "";
string gösterilecekİsim = isim ?? "İsimsiz";
Console.WriteLine(gösterilecekİsim);  // Boş string yazdırır, "İsimsiz" değil!

// Eğer boş string'i de kontrol etmek istiyorsanız:
gösterilecekİsim = string.IsNullOrEmpty(isim) ? "İsimsiz" : isim;
```

### 3. Tip Uyumsuzlukları

Sol ve sağ operandların tip uyumluluğuna dikkat etmek gerekir:

```csharp
// Bu çalışır çünkü int?, int tipine dönüştürülebilir
int? nullableSayı = null;
int sayı = nullableSayı ?? 0;

// Bu çalışmaz çünkü string ve int tiplerinin doğrudan ilişkisi yoktur
// string değer = nullableSayı ?? "sıfır";  // Derleme hatası!

// Bu şekilde çalışır
string değer = nullableSayı?.ToString() ?? "sıfır";
```

## Örnek Senaryolar

### Senaryo 1: Kullanıcı Ayarları

```csharp
public class KullanıcıAyarları
{
    public string TemaRengi { get; set; }
    public int FontBoyutu { get; set; }
    public bool BildirimleriAktifleştir { get; set; }
}

public void AyarlarıUygula(KullanıcıAyarları kullanıcıAyarları)
{
    var varsayılanAyarlar = new KullanıcıAyarları 
    { 
        TemaRengi = "Mavi", 
        FontBoyutu = 12, 
        BildirimleriAktifleştir = true 
    };
    
    var uygulanacakAyarlar = kullanıcıAyarları ?? varsayılanAyarlar;
    
    // Şimdi özellikler için ayrı ayrı null kontrolü yapabiliriz
    string temaRengi = uygulanacakAyarlar.TemaRengi ?? "Mavi";
    int fontBoyutu = uygulanacakAyarlar.FontBoyutu > 0 ? uygulanacakAyarlar.FontBoyutu : 12;
    
    // Ayarları uygula...
}
```

### Senaryo 2: API Yanıtları

```csharp
public async Task<ApiYanıt> VeriGetir()
{
    try
    {
        var yanıt = await _apiService.VeriGetirAsync();
        return yanıt ?? new ApiYanıt { Durum = "Veri bulunamadı" };
    }
    catch (Exception ex)
    {
        return new ApiYanıt { Hata = ex.Message ?? "Bilinmeyen bir hata oluştu" };
    }
}
```

### Senaryo 3: Komut Satırı Argümanları

```csharp
public static void Main(string[] args)
{
    string dosyaYolu = args.Length > 0 ? args[0] : null;
    
    // Dosya yolu belirtilmemişse varsayılan dosyayı kullan
    string kullanılacakDosyaYolu = dosyaYolu ?? "varsayılan.txt";
    
    // Eğer dosya yoksa da varsayılan dosyayı kullan
    if (!File.Exists(kullanılacakDosyaYolu))
        kullanılacakDosyaYolu = "varsayılan.txt";
    
    // veya daha kısa bir şekilde:
    kullanılacakDosyaYolu = File.Exists(kullanılacakDosyaYolu) ? kullanılacakDosyaYolu : "varsayılan.txt";
    
    Console.WriteLine($"Dosya: {kullanılacakDosyaYolu}");
}
```

## Sonuç

Null-coalescing operatörü (`??`) ve null-coalescing atama operatörü (`??=`), C# programlarında null değerlerin yönetimini basitleştiren güçlü araçlardır. Bu operatörler, kodunuzu daha kısa, daha okunabilir ve daha sağlam hale getirir. Özellikle null-conditional operatörü (`?.`) ile birlikte kullanıldığında, kapsamlı null güvenliği sağlayabilirler.

Bu operatörleri etkin bir şekilde kullanmak, savunmacı programlama tekniklerini uygulamak ve NullReferenceException hatalarından kaçınmak için önemlidir. C# 8.0 ve üzeri sürümlerde, nullable referans tipleri özelliği ile birlikte kullanıldığında, daha da güçlü bir null güvenliği sağlarlar.
