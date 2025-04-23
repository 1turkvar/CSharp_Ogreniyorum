# C# String Interpolation Rehberi

## İçindekiler
- [Giriş](#giriş)
- [Temel Kullanım](#temel-kullanım)
- [Formatlama Seçenekleri](#formatlama-seçenekleri)
- [İleri Düzey Kullanım](#i̇leri-düzey-kullanım)
- [String.Format ile Karşılaştırma](#stringformat-ile-karşılaştırma)
- [Performans Konuları](#performans-konuları)
- [En İyi Uygulamalar](#en-i̇yi-uygulamalar)
- [C# Sürümlerine Göre Yenilikler](#c-sürümlerine-göre-yenilikler)
- [Sık Yapılan Hatalar](#sık-yapılan-hatalar)

## Giriş

String interpolation (dize enterpolasyonu), C# 6.0 ile tanıtılan ve dize oluşturma işlemlerini daha okunabilir ve sürdürülebilir hale getiren bir özelliktir. Bu özellik, bir dize içinde ifadeler ve değişkenler kullanmanıza olanak tanır ve bu değerleri otomatik olarak dizgeye dönüştürür.

String interpolation, `$` sembolü ile başlayan ve `{` ve `}` karakterleri arasında değişkenler veya ifadeler içeren dizeler kullanır. Bu, kodunuzu daha temiz, anlaşılır ve bakımı daha kolay hale getirir.

## Temel Kullanım

### Sözdizimi

String interpolation için temel sözdizimi şu şekildedir:

```csharp
$"Metin {ifade} metin devamı"
```

Burada `{ifade}` kısmı herhangi bir geçerli C# ifadesi olabilir.

### Basit Örnekler

```csharp
string isim = "Ahmet";
int yas = 30;

// Temel string interpolation
string mesaj = $"Merhaba, benim adım {isim} ve {yas} yaşındayım.";
// Sonuç: "Merhaba, benim adım Ahmet ve 30 yaşındayım."

// İfadeler de kullanılabilir
string bilgi = $"Bir yıl sonra {isim} {yas + 1} yaşında olacak.";
// Sonuç: "Bir yıl sonra Ahmet 31 yaşında olacak."
```

### Kaçış Karakterleri

Curly brackets `{` ve `}` karakterlerini metin olarak kullanmak isterseniz, bunları iki kez yazmanız gerekir:

```csharp
string mesaj = $"Bir string interpolation örneği: {{değişken}}";
// Sonuç: "Bir string interpolation örneği: {değişken}"
```

## Formatlama Seçenekleri

String interpolation, değerlerin nasıl biçimlendirileceğini belirtmenize olanak tanır.

### Temel Formatlama

```csharp
decimal fiyat = 123.45m;

// Para birimi formatı
string fiyatMetni = $"Ürünün fiyatı: {fiyat:C}";
// Türkiye locale'inde: "Ürünün fiyatı: ₺123,45"

// Ondalık basamak kontrolü
string hassasFiyat = $"Hassas fiyat: {fiyat:F4}";
// Sonuç: "Hassas fiyat: 123.4500"
```

### Formatlama Sözdizimi

Formatlama sözdizimi şu şekildedir:

```csharp
{ifade[,hizalama][:formatDizesi]}
```

- `ifade`: Herhangi bir geçerli C# ifadesi
- `hizalama`: İsteğe bağlı, çıktının genişliğini ve hizalanmasını belirler
- `formatDizesi`: İsteğe bağlı, değerin nasıl biçimlendirileceğini belirtir

### Yaygın Format Belirteçleri

| Format | Açıklama | Örnek |
|--------|----------|-------|
| C veya c | Para birimi | `$"{123.45m:C}"` → `₺123,45` |
| D veya d | Onluk tam sayı | `$"{42:D5}"` → `00042` |
| E veya e | Bilimsel gösterim | `$"{12345:E2}"` → `1.23E+004` |
| F veya f | Sabit nokta | `$"{123.456:F2}"` → `123.46` |
| G veya g | Genel | `$"{12345:G3}"` → `1.23E4` |
| N veya n | Sayı | `$"{1234567:N}"` → `1.234.567,00` |
| P veya p | Yüzde | `$"{0.1234:P}"` → `%12,34` |
| X veya x | Onaltılık | `$"{255:X}"` → `FF` |

### Hizalama

Hizalama parametresi, çıktının genişliğini ve hizalamasını belirler:

```csharp
// Sağa hizalama (pozitif değer)
string sagHizalama = $"{42,10}";  // "        42"

// Sola hizalama (negatif değer)
string solaHizalama = $"{42,-10}"; // "42        "
```

### Tarih ve Zaman Formatlama

```csharp
DateTime simdi = DateTime.Now;

// Standart tarih formatları
string tarih1 = $"Kısa tarih: {simdi:d}";         // "Kısa tarih: 23.04.2025"
string tarih2 = $"Uzun tarih: {simdi:D}";         // "Uzun tarih: 23 Nisan 2025 Çarşamba"
string tarih3 = $"Kısa zaman: {simdi:t}";         // "Kısa zaman: 14:30"
string tarih4 = $"Uzun zaman: {simdi:T}";         // "Uzun zaman: 14:30:45"
string tarih5 = $"Tam tarih/zaman: {simdi:F}";    // "Tam tarih/zaman: 23 Nisan 2025 Çarşamba 14:30:45"

// Özel tarih formatları
string ozelTarih = $"Özel format: {simdi:yyyy-MM-dd HH:mm:ss}";  // "Özel format: 2025-04-23 14:30:45"
```

## İleri Düzey Kullanım

### Koşullu İfadeler

String interpolation içinde ternary operatörler veya diğer koşullu ifadeler kullanabilirsiniz:

```csharp
bool adminMi = true;
string mesaj = $"Hoş geldiniz {(adminMi ? "Yönetici" : "Kullanıcı")}!";
// Sonuç: "Hoş geldiniz Yönetici!"

int puan = 85;
string sonuc = $"Sınav sonucunuz: {puan} - {(puan >= 50 ? "Geçtiniz" : "Kaldınız")}";
// Sonuç: "Sınav sonucunuz: 85 - Geçtiniz"
```

### Sınıf Özellikleri ve Metotları

String interpolation, nesnelerin özelliklerine ve metotlarına erişim sağlar:

```csharp
class Kullanici
{
    public string Ad { get; set; }
    public string Soyad { get; set; }
    
    public string TamAd() => $"{Ad} {Soyad}";
}

Kullanici kullanici = new Kullanici { Ad = "Ali", Soyad = "Yılmaz" };
string bilgi = $"Kullanıcı bilgisi: {kullanici.TamAd()}";
// Sonuç: "Kullanıcı bilgisi: Ali Yılmaz"
```

### Verbatim String Interpolation

`@` ve `$` sembollerini birlikte kullanarak verbatim string interpolation yapabilirsiniz. Bu, çok satırlı dizeler veya özel karakterler içeren dizeler için kullanışlıdır:

```csharp
string yol = "C:\\Dosyalar\\Belgeler";
string kullanici = "Ahmet";

// Verbatim string interpolation
string dosyaYolu = $@"{yol}\{kullanici}\profil.txt";
// Sonuç: "C:\Dosyalar\Belgeler\Ahmet\profil.txt"

// Çok satırlı string interpolation
string cokSatirli = $@"
Kullanıcı: {kullanici}
Dosya Yolu: {yol}
Tarih: {DateTime.Now:d}
";
```

### Dize Enterpolasyonu Sırası

`$` sembolü, sözdizimi açısından `@` sembolünden önce veya sonra gelebilir:

```csharp
// Her iki kullanım da geçerlidir
string mesaj1 = $@"Çok satırlı {degisken}";
string mesaj2 = @$"Çok satırlı {degisken}";
```

## String.Format ile Karşılaştırma

String interpolation, aslında `String.Format` metodu için bir syntax sugar'dır. Derleyici, string interpolation ifadelerini `String.Format` çağrılarına dönüştürür.

### Farklılıklar ve Benzerlikler

```csharp
string isim = "Mehmet";
int yas = 28;

// String.Format kullanımı
string eskiYontem = String.Format("Merhaba, {0}! Sen {1} yaşındasın.", isim, yas);

// String interpolation kullanımı
string yeniYontem = $"Merhaba, {isim}! Sen {yas} yaşındasın.";

// İki yöntem de aynı sonucu verir
```

String interpolation'ın avantajları:
- Daha okunabilir kod
- Değişken adlarının doğrudan görünür olması
- Daha az hata olasılığı (indeks numaralarını hatırlamaya gerek yoktur)

## Performans Konuları

### İç Çalışma Prensibi

String interpolation'ın nasıl çalıştığını anlamak, performans açısından önemlidir:

```csharp
// Bu string interpolation
string mesaj = $"Merhaba {isim}, {yas} yaşındasın.";

// Derleyici tarafından aşağıdaki gibi dönüştürülür:
string mesaj = String.Format("Merhaba {0}, {1} yaşındasın.", isim, yas);
```

### Performans İpuçları

1. **Çok Sayıda Birleştirme İçin StringBuilder Kullanımı**

```csharp
// Çok sayıda string birleştirme için daha verimli
StringBuilder sb = new StringBuilder();
for (int i = 0; i < 1000; i++)
{
    sb.AppendFormat($"Sayı: {i}, Karesi: {i * i}");
    sb.AppendLine();
}
string sonuc = sb.ToString();
```

2. **FormattableString Kullanımı**

C# 6.0'dan itibaren, string interpolation ifadeleri `FormattableString` türüne dönüştürülebilir, bu da kültür bağımlı formatlama için faydalıdır:

```csharp
FormattableString fs = $"Fiyat: {12.34m:C}";
string tr = fs.ToString(new CultureInfo("tr-TR")); // Türkçe format: "Fiyat: ₺12,34"
string us = fs.ToString(new CultureInfo("en-US")); // İngilizce format: "Fiyat: $12.34"
```

## En İyi Uygulamalar

### Okunabilirliği Arttırma

1. **Karmaşık İfadelerden Kaçının**

```csharp
// Önerilmeyen - karmaşık bir ifade
string mesaj = $"Toplam: {liste.Where(x => x.Aktif).Sum(x => x.Deger * (1 + x.VergiOrani))}";

// Önerilen - daha okunabilir
var toplam = liste.Where(x => x.Aktif).Sum(x => x.Deger * (1 + x.VergiOrani));
string mesaj = $"Toplam: {toplam}";
```

2. **Tutarlı Formatlama Kullanın**

```csharp
// Değerlerin aynı formatla gösterilmesi tutarlılık sağlar
decimal fiyat1 = 123.45m;
decimal fiyat2 = 67.89m;
string fiyatlar = $"Fiyatlar: {fiyat1:C2} ve {fiyat2:C2}";
```

3. **İnterpolasyon İçinde İnterpolasyondan Kaçının**

```csharp
// Önerilmeyen - iç içe interpolation
string karmasik = $"Mesaj: {$"Merhaba {isim}!"}";

// Önerilen - daha basit ve okunabilir
string selamlama = $"Merhaba {isim}!";
string mesaj = $"Mesaj: {selamlama}";
```

### Güvenlik Konuları

String interpolation, SQL enjeksiyonu veya XSS saldırılarını önlemek için otomatik olarak escapeleme yapmaz. Güvenlik açısından hassas alanlarda, uygun escapeleme teknikleri kullanmalısınız:

```csharp
// SQL sorguları için parametreleri ayrı tutun
string kullaniciAdi = "admin'; DROP TABLE Users; --";
// YANLIŞ - SQL enjeksiyonuna açık
string sorgu = $"SELECT * FROM Users WHERE Username = '{kullaniciAdi}'";

// DOĞRU - Parametreli sorgu kullanımı
using (SqlCommand cmd = new SqlCommand("SELECT * FROM Users WHERE Username = @Username", connection))
{
    cmd.Parameters.AddWithValue("@Username", kullaniciAdi);
    // ...
}

// HTML çıktısı için encodeleme kullanın
string kullaniciGirdisi = "<script>alert('XSS');</script>";
// YANLIŞ - XSS'e açık
string html = $"<div>{kullaniciGirdisi}</div>";

// DOĞRU - HTML encodeleme kullanımı
string guvenliHtml = $"<div>{HttpUtility.HtmlEncode(kullaniciGirdisi)}</div>";
```

## C# Sürümlerine Göre Yenilikler

### C# 6.0
- Temel string interpolation özelliği tanıtıldı.

### C# 7.0
- Ek performans iyileştirmeleri.

### C# 8.0
- Nullable referans tipleri ile uyumluluk.

### C# 9.0
- İç derleyici iyileştirmeleri.

### C# 10.0
- Constant string interpolation için destek (sadece sabit değerlerle).
- Raw string literals ile birlikte kullanım (`"""` sözdizimi).

```csharp
// C# 10 - sabit string interpolation
const string isim = "Ali";
const string mesaj = $"Merhaba {isim}"; // Artık geçerli

// C# 11 - ham string literalleri ile
var cokSatirli = $$"""
    {
        "ad": "{{isim}}",
        "tarih": "{{DateTime.Now:yyyy-MM-dd}}"
    }
    """;
```

## Sık Yapılan Hatalar

### 1. Format Belirteçlerinin Yanlış Kullanımı

```csharp
decimal fiyat = 123.45m;

// HATALI - iki nokta eksik
string hatali = $"Fiyat {fiyat C}";

// DOĞRU
string dogru = $"Fiyat {fiyat:C}";
```

### 2. Kaçış Karakterlerinin Unutulması

```csharp
// HATALI - süslü parantez hatası
string hatali = $"Bu bir {değişken} ve bir {süslü parantez}";

// DOĞRU
string dogru = $"Bu bir {degisken} ve bir {{süslü parantez}}";
```

### 3. Kültür Bağımlı Formatlama Sorunları

```csharp
decimal fiyat = 1234.56m;

// Bu, çalıştığı kültüre göre farklı sonuçlar verebilir
string mesaj = $"Fiyat: {fiyat:C}";

// Belirli bir kültür kullanmak daha tutarlıdır
CultureInfo trCulture = new CultureInfo("tr-TR");
string mesajTR = string.Format(trCulture, "{0:C}", fiyat); // "₺1.234,56"
```

### 4. StringBuilder ile Karıştırma

```csharp
// HATALI - StringBuilder'da doğrudan interpolation kullanımı
StringBuilder sb = new StringBuilder();
for (int i = 0; i < 10; i++)
{
    sb.Append($"Sayı: {i}"); // Her iterasyonda yeni bir string oluşturur
}

// DOĞRU - StringBuilder'ın kendi formatlama metodunu kullanmak
for (int i = 0; i < 10; i++)
{
    sb.AppendFormat("Sayı: {0}", i);
    // veya
    sb.Append("Sayı: ").Append(i);
}
```

## Sonuç

String interpolation, C#'ta dize oluşturma ve biçimlendirme için modern, temiz ve güçlü bir yaklaşım sunar. Bu rehberde, temel kullanımdan ileri düzey tekniklere kadar string interpolation'ın birçok yönünü ele aldık.

Doğru kullanıldığında, string interpolation şunları sağlar:
- Daha okunabilir ve bakımı kolay kod
- Daha az hata olasılığı
- Güçlü formatlama seçenekleri
- Modern C# sözdizimi ile uyumluluk

String interpolation'ı etkin bir şekilde kullanmak, C# kodunuzun kalitesini ve netliğini artıracaktır.
