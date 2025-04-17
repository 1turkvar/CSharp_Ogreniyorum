# C# Optional ve Named Parametreler Kılavuzu

## İçindekiler
- [Optional (İsteğe Bağlı) Parametreler](#optional-isteğe-bağlı-parametreler)
  - [Tanım ve Kullanım](#tanım-ve-kullanım)
  - [Sınırlamalar](#sınırlamalar)
  - [İyi Kullanım Örnekleri](#iyi-kullanım-örnekleri)
- [Named (İsimlendirilmiş) Parametreler](#named-isimlendirilmiş-parametreler)
  - [Tanım ve Kullanım](#tanım-ve-kullanım-1)
  - [Avantajları](#avantajları)
  - [Kullanım Örnekleri](#kullanım-örnekleri)
- [Optional ve Named Parametrelerin Birlikte Kullanımı](#optional-ve-named-parametrelerin-birlikte-kullanımı)
- [Dikkat Edilmesi Gerekenler](#dikkat-edilmesi-gerekenler)
- [Gerçek Dünya Örnekleri](#gerçek-dünya-örnekleri)
- [Özet](#özet)

## Optional (İsteğe Bağlı) Parametreler

### Tanım ve Kullanım

Optional parametreler, C# 4.0 ile birlikte tanıtılan ve bir metot çağrısında belirtilmesi zorunlu olmayan parametrelerdir. Bir parametreyi optional yapmak için, parametre tanımında bir varsayılan değer atanması gerekir.

```csharp
// Basit bir optional parametre örneği
public void Selamla(string isim = "Dünya")
{
    Console.WriteLine($"Merhaba, {isim}!");
}

// Kullanım
Selamla();         // "Merhaba, Dünya!" yazdırır
Selamla("Ahmet");  // "Merhaba, Ahmet!" yazdırır
```

Optional parametreler her zaman metot imzasının sonunda yer almalıdır. Bir optional parametreden sonra zorunlu parametre tanımlanamaz.

```csharp
// Doğru kullanım
public void Metot(int zorunluParametre, string optionalParametre = "Varsayılan")
{
    // Metot gövdesi
}

// Hatalı kullanım - Derleme hatası verir
// public void Metot(string optionalParametre = "Varsayılan", int zorunluParametre)
// {
//     // Metot gövdesi
// }
```

### Sınırlamalar

Optional parametreler için bazı sınırlamalar vardır:

1. Optional parametrelerin varsayılan değerleri derleme zamanında (compile-time) belli olmalıdır. Değişken değerler veya çalışma zamanında (runtime) hesaplanan değerler kullanılamaz.

```csharp
// Doğru
public void Metot(int sayi = 10)
{
    // Metot gövdesi
}

// Hatalı - Derleme hatası verir
// public void Metot(int sayi = DateTime.Now.Day)
// {
//     // Metot gövdesi
// }
```

2. Referans türleri için `null` varsayılan değeri olarak kullanılabilir.

```csharp
public void Listele(List<string> liste = null)
{
    if (liste == null)
    {
        Console.WriteLine("Liste boş!");
        return;
    }
    
    foreach (var item in liste)
    {
        Console.WriteLine(item);
    }
}
```

3. Optional parametreler her zaman metot imzasının sonunda yer almalıdır.

### İyi Kullanım Örnekleri

Optional parametreler genellikle şu durumlarda kullanılır:

1. **Yapılandırma seçenekleri**: Bir metoda çağrı yapılırken çoğu durumda varsayılan değerlerin yeterli olduğu durumlarda.

```csharp
public void BağlantıKur(string sunucu, string kullanıcıAdı, string şifre, int zamanAşımı = 30, bool güvenliMod = true)
{
    // Sunucuya bağlanma kodu
}

// Kullanım
BağlantıKur("db.example.com", "admin", "pass123"); // Varsayılan değerlerle
BağlantıKur("db.example.com", "admin", "pass123", 60, false); // Özel değerlerle
```

2. **Method Overloading (Metot Aşırı Yükleme) yerine**: Aynı işlevi farklı parametre sayılarıyla yapmak için birden çok metot yerine optional parametreler kullanılabilir.

```csharp
// Method overloading yerine
public void DosyaOluştur(string dosyaAdı, string içerik = "", bool üzerindeYaz = false, FileAttributes özellikler = FileAttributes.Normal)
{
    // Dosya oluşturma kodu
}
```

## Named (İsimlendirilmiş) Parametreler

### Tanım ve Kullanım

Named parametreler, metot çağrısı sırasında parametreleri isimleriyle belirtmenize olanak tanır. Bu özellik, parametre sırasını hatırlamak zorunda kalmadan metot çağrısı yapmanızı sağlar.

```csharp
public void KitapEkle(string başlık, string yazar, int yayınYılı, string yayıncı, string ISBN)
{
    // Kitap ekleme kodu
}

// Normal çağrı
KitapEkle("Tutunamayanlar", "Oğuz Atay", 1972, "İletişim Yayınları", "9789754705058");

// Named parametrelerle çağrı
KitapEkle(
    başlık: "Tutunamayanlar",
    yazar: "Oğuz Atay",
    yayınYılı: 1972,
    yayıncı: "İletişim Yayınları",
    ISBN: "9789754705058"
);

// Sıra değiştirerek çağrı (named parametreler sayesinde mümkün)
KitapEkle(
    ISBN: "9789754705058",
    yazar: "Oğuz Atay",
    başlık: "Tutunamayanlar",
    yayıncı: "İletişim Yayınları",
    yayınYılı: 1972
);
```

### Avantajları

Named parametrelerin birçok avantajı vardır:

1. **Okunabilirlik**: Özellikle çok parametreli metotlarda, hangi parametrenin ne olduğunu açıkça gösterir.
2. **Esneklik**: Parametrelerin sırasını değiştirmenize olanak tanır.
3. **Hata azaltma**: Yanlış parametre sırası nedeniyle oluşabilecek hataları önler.
4. **Belgeleme**: Kod, kendi kendini daha iyi belgelendirir.

### Kullanım Örnekleri

Named parametreler özellikle şu durumlarda faydalıdır:

1. **Çok parametreli metotlar**: Birden fazla parametreye sahip metotlarda kullanıldığında, kod okunabilirliğini artırır.

```csharp
public void KullanıcıOluştur(string ad, string soyad, string eposta, string telefon, DateTime doğumTarihi, string adres, string şehir, string ülke)
{
    // Kullanıcı oluşturma kodu
}

// Named parametrelerle çağrı
KullanıcıOluştur(
    ad: "Ali",
    soyad: "Yılmaz",
    eposta: "ali.yilmaz@example.com",
    telefon: "555-123-4567",
    doğumTarihi: new DateTime(1990, 5, 15),
    adres: "Atatürk Cad. No:123",
    şehir: "İstanbul",
    ülke: "Türkiye"
);
```

2. **Benzer tipteki parametreler**: Aynı türden birden fazla parametre içeren metotlarda karışıklığı önler.

```csharp
public void KoordinatBelirle(double enlem, double boylam)
{
    // Koordinat belirleme kodu
}

// Named parametrelerle çağrı
KoordinatBelirle(enlem: 41.0082, boylam: 28.9784); // İstanbul'un koordinatları
```

## Optional ve Named Parametrelerin Birlikte Kullanımı

Optional ve named parametreler sıklıkla birlikte kullanılır. Bu kombinasyon, metot çağrılarında maksimum esneklik sağlar:

```csharp
public void RaporOluştur(
    string başlık,
    DateTime tarih = default,
    string yazar = "Sistem",
    bool ayrıntılı = false,
    string format = "PDF",
    bool ePostaGönder = false,
    string[] alıcılar = null)
{
    // Rapor oluşturma kodu
    tarih = tarih == default ? DateTime.Now : tarih;
}

// Sadece zorunlu parametreyle çağrı
RaporOluştur("Aylık Satış Raporu");

// Bazı optional parametrelerle çağrı
RaporOluştur(
    başlık: "Aylık Satış Raporu",
    ayrıntılı: true,
    ePostaGönder: true,
    alıcılar: new[] { "satisekibi@example.com", "yonetim@example.com" }
);

// Sıra dışı ve named parametrelerle çağrı
RaporOluştur(
    format: "Excel",
    başlık: "Aylık Satış Raporu",
    ePostaGönder: true
);
```

## Dikkat Edilmesi Gerekenler

Optional ve named parametrelerle çalışırken dikkat edilmesi gereken bazı noktalar:

1. **Metot İmzası Değişiklikleri**: Varolan bir metodun imzasını değiştirirken, optional parametrelerin varsayılan değerlerini değiştirmek geriye dönük uyumsuzluk yaratabilir.

2. **Performans Etkileri**: Named parametreler derleme zamanında çözülür ve çalışma zamanında ek yük getirmez.

3. **COM Birlikte Çalışabilirlik**: Optional ve named parametreler, COM nesneleriyle çalışırken özellikle faydalıdır.

4. **API Tasarımı**: Yeni API tasarlarken, gelecekteki değişikliklere uyum sağlamak için optional parametreler kullanmak iyi bir uygulamadır.

## Gerçek Dünya Örnekleri

### Örnek 1: Dosya İşlemleri

```csharp
public class DosyaYöneticisi
{
    public void DosyaKaydet(
        string içerik, 
        string dosyaYolu, 
        bool üzerindeYaz = false, 
        Encoding kodlama = null, 
        bool yedekle = false)
    {
        kodlama = kodlama ?? Encoding.UTF8;
        
        if (yedekle && File.Exists(dosyaYolu))
        {
            string yedekDosya = $"{dosyaYolu}.bak";
            File.Copy(dosyaYolu, yedekDosya, true);
        }
        
        if (!üzerindeYaz && File.Exists(dosyaYolu))
        {
            throw new IOException($"Dosya zaten mevcut: {dosyaYolu}");
        }
        
        File.WriteAllText(dosyaYolu, içerik, kodlama);
    }
}

// Kullanım örnekleri
var yönetici = new DosyaYöneticisi();

// Temel kullanım
yönetici.DosyaKaydet("Merhaba Dünya", "test.txt");

// İsimlendirilmiş parametrelerle
yönetici.DosyaKaydet(
    içerik: "Merhaba Dünya",
    dosyaYolu: "test.txt",
    üzerindeYaz: true,
    yedekle: true
);

// Sadece belirli optional parametrelerle
yönetici.DosyaKaydet(
    içerik: "Merhaba Dünya",
    dosyaYolu: "test.txt",
    kodlama: Encoding.GetEncoding("windows-1254")
);
```

### Örnek 2: UI Öğesi Oluşturma

```csharp
public class UIYöneticisi
{
    public Button DüğmeOluştur(
        string metin,
        Action tıklamaOlayı = null,
        int genişlik = 100,
        int yükseklik = 30,
        Color arkaplanRengi = default,
        Color yazıRengi = default,
        string araçİpucu = null,
        bool etkin = true)
    {
        var düğme = new Button
        {
            Text = metin,
            Width = genişlik,
            Height = yükseklik,
            Enabled = etkin
        };
        
        if (tıklamaOlayı != null)
            düğme.Click += (sender, e) => tıklamaOlayı();
            
        if (arkaplanRengi != default)
            düğme.BackColor = arkaplanRengi;
            
        if (yazıRengi != default)
            düğme.ForeColor = yazıRengi;
            
        if (!string.IsNullOrEmpty(araçİpucu))
            düğme.ToolTip = araçİpucu;
            
        return düğme;
    }
}

// Kullanım örnekleri
var uiYönetici = new UIYöneticisi();

// Temel düğme
var basitDüğme = uiYönetici.DüğmeOluştur("Tamam");

// Özelleştirilmiş düğme
var özelDüğme = uiYönetici.DüğmeOluştur(
    metin: "Kaydet",
    tıklamaOlayı: () => SistemKaydet(),
    genişlik: 150,
    arkaplanRengi: Color.Blue,
    yazıRengi: Color.White,
    araçİpucu: "Değişiklikleri kaydet"
);

// Sadece belirli özellikleri değiştirilen düğme
var diğerDüğme = uiYönetici.DüğmeOluştur(
    metin: "İptal",
    etkin: false,
    araçİpucu: "Şu anda kullanılamıyor"
);
```

## Özet

C# dilindeki optional ve named parametreler, metot çağrılarını daha esnek, okunabilir ve hata yapmaya daha az eğilimli hale getirir. Bu özellikler:

1. **Optional Parametreler**: Varsayılan değerlere sahip olabilen ve çağrı sırasında belirtilmesi zorunlu olmayan parametrelerdir.
2. **Named Parametreler**: Metot çağrısında parametrelerin adlarını belirterek, sıradan bağımsız olarak değer atamaya olanak tanır.

Bu iki özellik birlikte kullanıldığında, özellikle çok parametreli metotlarda, kod okunabilirliğini ve esnekliğini büyük ölçüde artırır. Ayrıca, API tasarımında gelecekteki genişletmelere uyum sağlamak için iyi bir yaklaşımdır.

Bu özellikleri etkin bir şekilde kullanarak, daha temiz, daha az hata içeren ve daha bakımı kolay C# kodları yazabilirsiniz.
