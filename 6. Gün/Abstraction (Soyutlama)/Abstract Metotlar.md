# C# Abstract Metotlar Detaylı Rehber

## İçindekiler
1. [Abstract Metot Nedir?](#abstract-metot-nedir)
2. [Abstract Metotların Özellikleri](#abstract-metotların-özellikleri)
3. [Abstract Sınıflar ve Abstract Metotlar](#abstract-sınıflar-ve-abstract-metotlar)
4. [Abstract Metot Kullanım Örnekleri](#abstract-metot-kullanım-örnekleri)
5. [Abstract Metotlar vs Interface Metotları](#abstract-metotlar-vs-interface-metotları)
6. [İyi Uygulama Örnekleri](#iyi-uygulama-örnekleri)
7. [Yaygın Hatalar ve Çözümleri](#yaygın-hatalar-ve-çözümleri)
8. [İleri Düzey Konular](#ileri-düzey-konular)

## Abstract Metot Nedir?

Abstract metot, bir sınıf içerisinde tanımlanan fakat implementasyonu (gövdesi) olmayan, bu implementasyonun türetilmiş sınıflar tarafından sağlanması zorunlu olan metotlardır. Abstract metotlar, nesne yönelimli programlamada polimorfizm ilkesini gerçekleştirmenin en önemli araçlarından biridir.

```csharp
public abstract void MetotAdi();  // Abstract metot tanımı - dikkat edin, gövdesi yok
```

## Abstract Metotların Özellikleri

Abstract metotların önemli özellikleri şunlardır:

1. **Gövdesi Yoktur**: Abstract metotların implementasyonu (kod gövdesi) yoktur, sadece tanımları vardır.

2. **Abstract Sınıflarda Tanımlanır**: Abstract metotlar yalnızca abstract sınıflarda tanımlanabilir.

3. **Override Edilmelidir**: Abstract bir sınıftan türetilen sınıflar, tüm abstract metotları override etmek (ezmek) zorundadır.

4. **Erişim Belirleyicileri**: Abstract metotlar private olamaz (çünkü private metotlar türetilmiş sınıflar tarafından görülemez).

5. **Statik Olamaz**: Abstract metotlar, doğaları gereği statik olarak tanımlanamazlar.

6. **Sealed Olamaz**: Abstract metotlar sealed (mühürlü) olarak işaretlenemezler.

## Abstract Sınıflar ve Abstract Metotlar

Abstract metotlar, yalnızca abstract olarak işaretlenmiş sınıflarda tanımlanabilirler. Abstract sınıflardan doğrudan nesne oluşturulamaz, bunun yerine türetilmiş sınıflar kullanılır.

```csharp
public abstract class SoyutSinif
{
    // Abstract metot
    public abstract void SoyutMetot();
    
    // Normal metot (concrete)
    public void NormalMetot()
    {
        Console.WriteLine("Bu normal bir metottur");
    }
}

public class TuretilmisSinif : SoyutSinif
{
    // Abstract metodu override etme
    public override void SoyutMetot()
    {
        Console.WriteLine("SoyutMetot implement edildi");
    }
}
```

## Abstract Metot Kullanım Örnekleri

### Örnek 1: Şekil Hiyerarşisi

```csharp
public abstract class Sekil
{
    public abstract double AlanHesapla();
    public abstract double CevreHesapla();
    
    public void BilgileriGoster()
    {
        Console.WriteLine($"Alan: {AlanHesapla()}");
        Console.WriteLine($"Çevre: {CevreHesapla()}");
    }
}

public class Dikdortgen : Sekil
{
    public double Genislik { get; set; }
    public double Yukseklik { get; set; }
    
    public Dikdortgen(double genislik, double yukseklik)
    {
        Genislik = genislik;
        Yukseklik = yukseklik;
    }
    
    public override double AlanHesapla()
    {
        return Genislik * Yukseklik;
    }
    
    public override double CevreHesapla()
    {
        return 2 * (Genislik + Yukseklik);
    }
}

public class Daire : Sekil
{
    public double YariCap { get; set; }
    
    public Daire(double yariCap)
    {
        YariCap = yariCap;
    }
    
    public override double AlanHesapla()
    {
        return Math.PI * YariCap * YariCap;
    }
    
    public override double CevreHesapla()
    {
        return 2 * Math.PI * YariCap;
    }
}
```

### Örnek 2: Belge İşleme

```csharp
public abstract class Belge
{
    public string Ad { get; set; }
    public string Icerik { get; set; }
    
    public abstract void Kaydet();
    public abstract void Yukle(string yol);
    public abstract void Yazdir();
    
    // Ortak fonksiyonalite
    public void BelgeBilgisiGoster()
    {
        Console.WriteLine($"Belge Adı: {Ad}");
        Console.WriteLine($"İçerik Uzunluğu: {Icerik?.Length ?? 0} karakter");
    }
}

public class PDFBelge : Belge
{
    public override void Kaydet()
    {
        Console.WriteLine($"{Ad} PDF olarak kaydediliyor...");
        // PDF kaydetme işlemi
    }
    
    public override void Yukle(string yol)
    {
        Console.WriteLine($"{yol} konumundan PDF yükleniyor...");
        // PDF yükleme işlemi
    }
    
    public override void Yazdir()
    {
        Console.WriteLine("PDF belgesi yazdırılıyor...");
        // PDF yazdırma işlemi
    }
}

public class WordBelge : Belge
{
    public override void Kaydet()
    {
        Console.WriteLine($"{Ad} Word belgesi olarak kaydediliyor...");
        // Word belgesi kaydetme işlemi
    }
    
    public override void Yukle(string yol)
    {
        Console.WriteLine($"{yol} konumundan Word belgesi yükleniyor...");
        // Word belgesi yükleme işlemi
    }
    
    public override void Yazdir()
    {
        Console.WriteLine("Word belgesi yazdırılıyor...");
        // Word yazdırma işlemi
    }
}
```

## Abstract Metotlar vs Interface Metotları

Abstract metotlar ve interface metotları arasındaki temel farklar:

| Özellik | Abstract Metotlar | Interface Metotları |
|---------|-------------------|---------------------|
| Tanımlandığı Yer | Abstract sınıflar | Interface'ler |
| Erişim Belirleyicileri | Public, protected, internal olabilir | Varsayılan olarak public (C# 8.0 öncesi) |
| İmplementasyon | Yok, türetilmiş sınıflarda zorunlu | Yok, türetilmiş sınıflarda zorunlu |
| Miras | Tek bir abstract sınıftan miras alınabilir | Birden fazla interface uygulanabilir |
| Değişkenler | Sınıf değişkenleri tanımlanabilir | Değişkenler tanımlanamaz (C# 8.0 öncesi) |
| Default İmplementasyon | Yok | C# 8.0 ve sonrasında mümkün |

### Örnek Karşılaştırma:

```csharp
// Abstract sınıf ve metot
public abstract class AbstractSinif
{
    protected int _deger;
    
    public AbstractSinif(int deger)
    {
        _deger = deger;
    }
    
    public abstract void Metot();
    
    public void OrtakMetot()
    {
        Console.WriteLine("Bu metot tüm alt sınıflar için ortaktır");
    }
}

// Interface
public interface IArayuz
{
    void Metot();
    
    // C# 8.0+ ile mümkün
    void DefaultMetot()
    {
        Console.WriteLine("Bu varsayılan bir implementasyondur");
    }
}
```

## İyi Uygulama Örnekleri

### 1. Template Method Deseni

Abstract metotların en yaygın kullanım şekillerinden biri Template Method tasarım desenidir:

```csharp
public abstract class RaporOlusturucu
{
    // Template metod - algoritmanın iskeletini tanımlar
    public void RaporOlustur()
    {
        VerileriTopla();
        VerileriIsle();
        RaporuBicimlendir();
        RaporuKaydet();
    }
    
    // Abstract metotlar - alt sınıflarda uygulanacak
    protected abstract void VerileriTopla();
    protected abstract void VerileriIsle();
    protected abstract void RaporuBicimlendir();
    
    // Hook metodu - isteğe bağlı override edilebilir
    protected virtual void RaporuKaydet()
    {
        Console.WriteLine("Rapor kaydedildi");
    }
}

public class ExcelRaporOlusturucu : RaporOlusturucu
{
    protected override void VerileriTopla()
    {
        Console.WriteLine("Excel için veriler toplanıyor");
    }
    
    protected override void VerileriIsle()
    {
        Console.WriteLine("Excel için veriler işleniyor");
    }
    
    protected override void RaporuBicimlendir()
    {
        Console.WriteLine("Excel formatında rapor biçimlendiriliyor");
    }
    
    // Hook metodunu değiştirme
    protected override void RaporuKaydet()
    {
        Console.WriteLine("Rapor XLSX formatında kaydedildi");
    }
}
```

### 2. Strategy Deseni Uygulaması

```csharp
public abstract class OdemeStratejisi
{
    public abstract bool OdemeYap(decimal tutar);
    public abstract string OdemeBilgisiGetir();
}

public class KrediKartiOdeme : OdemeStratejisi
{
    private string _kartNumarasi;
    private string _sonKullanmaTarihi;
    
    public KrediKartiOdeme(string kartNumarasi, string sonKullanmaTarihi)
    {
        _kartNumarasi = kartNumarasi;
        _sonKullanmaTarihi = sonKullanmaTarihi;
    }
    
    public override bool OdemeYap(decimal tutar)
    {
        Console.WriteLine($"Kredi kartı ile {tutar}₺ ödeme yapıldı");
        return true;
    }
    
    public override string OdemeBilgisiGetir()
    {
        return $"Kredi Kartı: {_kartNumarasi.Substring(0, 4)}XXXXXXXX{_kartNumarasi.Substring(_kartNumarasi.Length - 4)}";
    }
}

public class HavaleOdeme : OdemeStratejisi
{
    private string _ibanNo;
    
    public HavaleOdeme(string ibanNo)
    {
        _ibanNo = ibanNo;
    }
    
    public override bool OdemeYap(decimal tutar)
    {
        Console.WriteLine($"Havale ile {tutar}₺ ödeme yapıldı");
        return true;
    }
    
    public override string OdemeBilgisiGetir()
    {
        return $"Havale - IBAN: {_ibanNo.Substring(0, 2)}XXXXXXXXXX";
    }
}

// Kullanım
public class SiparisIslem
{
    private OdemeStratejisi _odemeStratejisi;
    
    public void OdemeStratejisiAyarla(OdemeStratejisi strateji)
    {
        _odemeStratejisi = strateji;
    }
    
    public bool SiparisiTamamla(decimal tutar)
    {
        if (_odemeStratejisi == null)
        {
            throw new InvalidOperationException("Ödeme stratejisi ayarlanmamış");
        }
        
        return _odemeStratejisi.OdemeYap(tutar);
    }
}
```

## Yaygın Hatalar ve Çözümleri

### 1. Abstract Sınıfta Abstract Metotların Uygulanmaya Çalışılması

**Hata:**
```csharp
public abstract class HataliSinif
{
    // Hata: Abstract metot implementasyonu içeremez
    public abstract void HataliMetot() { }
}
```

**Çözüm:**
```csharp
public abstract class DogruSinif
{
    // Doğru: Abstract metot sadece tanım içerir
    public abstract void DogruMetot();
}
```

### 2. Abstract Olmayan Sınıfta Abstract Metot Tanımlama

**Hata:**
```csharp
public class HataliSinif
{
    // Hata: Normal sınıf abstract metot içeremez
    public abstract void HataliMetot();
}
```

**Çözüm:**
```csharp
public abstract class DogruSinif
{
    public abstract void DogruMetot();
}

// Ya da
public class AlternatifSinif
{
    // Eğer abstract metot istemiyorsanız, normal metot kullanın
    public void NormalMetot()
    {
        // İmplementasyon
    }
}
```

### 3. Abstract Metotların Override Edilmemesi

**Hata:**
```csharp
public abstract class TemelSinif
{
    public abstract void ZorunluMetot();
}

// Hata: Abstract metot uygulanmamış
public class HataliAltSinif : TemelSinif
{
    // ZorunluMetot() implementasyonu eksik
}
```

**Çözüm:**
```csharp
public abstract class TemelSinif
{
    public abstract void ZorunluMetot();
}

// Doğru: Abstract metot override edilmiş
public class DogruAltSinif : TemelSinif
{
    public override void ZorunluMetot()
    {
        // İmplementasyon
    }
}

// Alternatif: Alt sınıf da abstract olabilir
public abstract class AltAbstractSinif : TemelSinif
{
    // ZorunluMetot'u uygulamaya gerek yok
    // Bu sınıftan türetilen sınıflar uygulamak zorunda
}
```

## İleri Düzey Konular

### 1. Abstract Metotlar ve Sealed Anahtar Kelimesi

Türetilmiş sınıflarda override edilen abstract metotlar `sealed` olarak işaretlenebilir:

```csharp
public abstract class TemelSinif
{
    public abstract void Metot();
}

public class AltSinif : TemelSinif
{
    public override void Metot()
    {
        Console.WriteLine("AltSinif'ta uygulandı");
    }
}

public class DahaAltSinif : AltSinif
{
    public sealed override void Metot()
    {
        Console.WriteLine("DahaAltSinif'ta uygulandı ve mühürlendi");
        base.Metot(); // Üst sınıf implementasyonunu çağırabilir
    }
}

public class EnAltSinif : DahaAltSinif
{
    // Hata: 'Metot' sealed olarak işaretlendiği için burada override edilemez
    // public override void Metot() { }
}
```

### 2. Abstract Metotlar ve Erişim Belirleyicileri

Abstract metotlar `private` olamazlar, ancak `protected` veya `internal` olabilirler:

```csharp
public abstract class TemelSinif
{
    // Public abstract metot - tüm türetilmiş sınıflar ve dışarıdan erişilebilir
    public abstract void PublicMetot();
    
    // Protected abstract metot - sadece türetilmiş sınıflar erişebilir
    protected abstract void ProtectedMetot();
    
    // Internal abstract metot - aynı assembly içindeki türetilmiş sınıflar erişebilir
    internal abstract void InternalMetot();
    
    // Protected internal abstract metot
    protected internal abstract void ProtectedInternalMetot();
}
```

### 3. Generic Abstract Metotlar

Abstract metotlar generic tip parametreleri de alabilirler:

```csharp
public abstract class GenericIslemler<T>
{
    public abstract void Ekle(T item);
    public abstract T Getir(int index);
    public abstract bool Sil(T item);
    
    // Generic tip kısıtlamalı abstract metot
    public abstract TResult Donustur<TResult>(T kaynak) where TResult : class;
}

public class IntegerIslemler : GenericIslemler<int>
{
    private List<int> _liste = new List<int>();
    
    public override void Ekle(int item)
    {
        _liste.Add(item);
    }
    
    public override int Getir(int index)
    {
        return _liste[index];
    }
    
    public override bool Sil(int item)
    {
        return _liste.Remove(item);
    }
    
    public override TResult Donustur<TResult>(int kaynak)
    {
        // int'i TResult tipine dönüştürme işlemi
        // Örnek: int'i string'e dönüştürdüğümüzü varsayalım
        if (typeof(TResult) == typeof(string))
        {
            return kaynak.ToString() as TResult;
        }
        
        return default;
    }
}
```

### 4. Asenkron (Async) Abstract Metotlar

Abstract metotlar asenkron olarak da tanımlanabilir:

```csharp
public abstract class VeriErisim
{
    public abstract Task<bool> BaglantiKurAsync(string baglantiBilgisi);
    public abstract Task<List<T>> VeriGetirAsync<T>(string sorgu) where T : class, new();
    public abstract Task<int> VeriKaydetAsync<T>(T nesne) where T : class;
}

public class SqlVeriErisim : VeriErisim
{
    public override async Task<bool> BaglantiKurAsync(string baglantiBilgisi)
    {
        await Task.Delay(100); // Simüle edilmiş bağlantı kurma işlemi
        return true;
    }
    
    public override async Task<List<T>> VeriGetirAsync<T>(string sorgu)
    {
        await Task.Delay(200); // Simüle edilmiş veri getirme işlemi
        return new List<T>();
    }
    
    public override async Task<int> VeriKaydetAsync<T>(T nesne)
    {
        await Task.Delay(150); // Simüle edilmiş veri kaydetme işlemi
        return 1; // Etkilenen satır sayısı
    }
}
```

### 5. Abstract Sınıflar ve Abstract Metotların Kullanım Senaryoları

Abstract sınıflar ve metotlar genellikle şu durumlarda kullanılır:

1. **Temel Sınıf İşlevselliği Sağlama**: Alt sınıflar için ortak işlevsellik sunmak istediğinizde.
2. **Zorunlu Davranış Tanımlama**: Alt sınıfların uygulaması gereken metotları belirlemek istediğinizde.
3. **İskelet Algoritma Oluşturma**: Belirli bir algoritmanın adımlarını tanımlamak ancak bazı adımların implementasyonunu alt sınıflara bırakmak istediğinizde (Template Method pattern).
4. **Kısmi İmplementasyon Sağlama**: Bazı metotları uygulamak ve bazılarını alt sınıflara bırakmak istediğinizde.
5. **İş Mantığını Modelleme**: Uygulamanın temel iş mantığını modelleme ve uzmanlaşmış implementasyonları alt sınıflara bırakma.

## Sonuç

Abstract metotlar, C# dilinde kod tekrarını azaltmak, kodun kalitesini artırmak ve daha sürdürülebilir yazılımlar geliştirmek için güçlü bir araçtır. Object-oriented programlama prensiplerini uygulamak için abstract metotları doğru şekilde kullanmak, kodunuzun daha esnek, genişletilebilir ve bakımı kolay olmasını sağlar.

Farklı tasarım desenlerinde (özellikle Template Method, Strategy, Factory Method gibi) abstract metotlar kilit bir rol oynar ve karmaşık sistemlerin daha modüler ve düzenli tasarlanmasına yardımcı olur.

---

Bu rehber, C# abstract metotları hakkında temel ve ileri düzey bilgileri içermektedir. Daha fazla bilgi için Microsoft'un resmi dokümantasyonuna başvurabilirsiniz.
