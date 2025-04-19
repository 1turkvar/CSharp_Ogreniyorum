# C# Delegeler: Tanımlama ve Kullanım Rehberi

## İçindekiler
1. [Delegeler Nedir?](#delegeler-nedir)
2. [Delegate Tanımlama](#delegate-tanımlama)
3. [Delegate Kullanımı](#delegate-kullanımı)
4. [Çoklu Delegate Zincirleme (Multicast Delegates)](#çoklu-delegate-zincirleme)
5. [Yerleşik Delegate Tipleri](#yerleşik-delegate-tipleri)
   - [Action Delegeleri](#action-delegeleri)
   - [Func Delegeleri](#func-delegeleri)
   - [Predicate Delegeleri](#predicate-delegeleri)
6. [Anonim Metodlar ve Lambda İfadeleri](#anonim-metodlar-ve-lambda-ifadeleri)
7. [Olaylar (Events) ve Delegeler](#olaylar-events-ve-delegeler)
8. [Asenkron Programlama ve Delegeler](#asenkron-programlama-ve-delegeler)
9. [Pratik Örnekler ve Senaryolar](#pratik-örnekler-ve-senaryolar)
10. [En İyi Uygulamalar ve İpuçları](#en-iyi-uygulamalar-ve-i̇puçları)

## Delegeler Nedir?

Delegeler, C# programlama dilinin güçlü özelliklerinden biridir ve metotları işaret eden tiplerdir. Yani, delegeler metotları referans eden veri tipleridir. Bir delege, bir veya birden fazla metodu işaret edebilir ve bu metotları çağırabilir.

Özet olarak delegeler:

- Tip güvenlidir (Type-safe)
- Object-oriented fonksiyon işaretçileridir
- Metot referanslarını saklar
- Callback mekanizması olarak kullanılabilir
- Olay (event) programlamada temel yapı taşıdır

## Delegate Tanımlama

Bir delege tanımlamak için `delegate` anahtar kelimesi kullanılır. Delegenin imzası, işaret edeceği metodun imzasıyla eşleşmelidir.

```csharp
// Temel delegate tanımı
delegate void MesajGosterDelegate(string mesaj);

// Parametre alan ve değer döndüren delegate
delegate int HesaplaDelegate(int sayi1, int sayi2);

// Generic delegate tanımı
delegate T GenericDelegate<T>(T parametre);
```

## Delegate Kullanımı

Delegeleri kullanmak için önce delege tipinde bir değişken tanımlanır, ardından bu değişkene uygun imzaya sahip bir metot atanır.

```csharp
using System;

class Program
{
    // Delegate tanımı
    delegate void MesajGosterDelegate(string mesaj);
    
    // Delege ile uyumlu metotlar
    static void MesajGoster(string mesaj)
    {
        Console.WriteLine($"Mesaj: {mesaj}");
    }
    
    static void UyariGoster(string mesaj)
    {
        Console.WriteLine($"UYARI: {mesaj}");
    }
    
    static void Main(string[] args)
    {
        // Delege örneği oluşturma
        MesajGosterDelegate mesajDelegate = MesajGoster;
        
        // Delegeyi çağırma
        mesajDelegate("Merhaba Dünya!");
        
        // Delegeyi başka bir metoda yönlendirme
        mesajDelegate = UyariGoster;
        mesajDelegate("Dikkatli olun!");
        
        Console.ReadKey();
    }
}
```

## Çoklu Delegate Zincirleme

Delegelerin önemli özelliklerinden biri, birden fazla metodu işaret edebilme yetenekleridir. Bu özelliğe "multicast delegate" denir.

```csharp
using System;

class Program
{
    delegate void IslemDelegate();
    
    static void Metod1()
    {
        Console.WriteLine("Metod 1 çağrıldı");
    }
    
    static void Metod2()
    {
        Console.WriteLine("Metod 2 çağrıldı");
    }
    
    static void Metod3()
    {
        Console.WriteLine("Metod 3 çağrıldı");
    }
    
    static void Main(string[] args)
    {
        // Delege örnekleri
        IslemDelegate islem1 = Metod1;
        IslemDelegate islem2 = Metod2;
        IslemDelegate islem3 = Metod3;
        
        // Delegeleri zincirleme (+ operatörüyle)
        IslemDelegate tumIslemler = islem1 + islem2 + islem3;
        
        // Tüm delegeleri çağırma
        tumIslemler();
        
        Console.WriteLine("\n----- Delege zincirinden çıkarma -----");
        
        // Delege zincirinden çıkarma (- operatörüyle)
        IslemDelegate azaltilmisIslemler = tumIslemler - islem2;
        azaltilmisIslemler();
        
        Console.ReadKey();
    }
}
```

## Yerleşik Delegate Tipleri

.NET Framework, sık kullanılan delegasyon senaryoları için yerleşik generic delegeler sağlar.

### Action Delegeleri

`Action` delegesi, değer döndürmeyen (void) metotlar için kullanılır.

```csharp
using System;

class Program
{
    static void MesajYazdir(string mesaj)
    {
        Console.WriteLine(mesaj);
    }
    
    static void IkiParametreliMetod(string mesaj, int tekrar)
    {
        for (int i = 0; i < tekrar; i++)
        {
            Console.WriteLine(mesaj);
        }
    }
    
    static void Main(string[] args)
    {
        // Parametre almayan Action
        Action selamla = () => Console.WriteLine("Merhaba!");
        selamla();
        
        // Tek parametreli Action
        Action<string> mesajGoster = MesajYazdir;
        mesajGoster("Action delegate örneği");
        
        // İki parametreli Action
        Action<string, int> tekrarla = IkiParametreliMetod;
        tekrarla("Tekrarlanan mesaj", 3);
        
        Console.ReadKey();
    }
}
```

### Func Delegeleri

`Func` delegesi, değer döndüren metotlar için kullanılır.

```csharp
using System;

class Program
{
    static int Topla(int a, int b)
    {
        return a + b;
    }
    
    static string AdSoyadBirlestir(string ad, string soyad)
    {
        return $"{ad} {soyad}";
    }
    
    static void Main(string[] args)
    {
        // Parametre almayan, int döndüren Func
        Func<int> rastgeleSayi = () => new Random().Next(1, 100);
        Console.WriteLine($"Rastgele sayı: {rastgeleSayi()}");
        
        // İki int parametre alan, int döndüren Func
        Func<int, int, int> toplaFunc = Topla;
        Console.WriteLine($"Toplam: {toplaFunc(5, 7)}");
        
        // İki string parametre alan, string döndüren Func
        Func<string, string, string> tamIsim = AdSoyadBirlestir;
        Console.WriteLine($"Tam isim: {tamIsim("Ahmet", "Yılmaz")}");
        
        Console.ReadKey();
    }
}
```

### Predicate Delegeleri

`Predicate` delegesi, bir koşulu test eden ve bool döndüren metotlar için kullanılır.

```csharp
using System;
using System.Collections.Generic;

class Program
{
    static bool CiftSayiMi(int sayi)
    {
        return sayi % 2 == 0;
    }
    
    static void Main(string[] args)
    {
        // Predicate kullanımı
        Predicate<int> ciftMi = CiftSayiMi;
        Console.WriteLine($"10 çift mi? {ciftMi(10)}");
        Console.WriteLine($"7 çift mi? {ciftMi(7)}");
        
        // Liste filtreleme örneği
        List<int> sayilar = new List<int> { 1, 2, 3, 4, 5, 6, 7, 8, 9, 10 };
        List<int> ciftSayilar = sayilar.FindAll(ciftMi);
        
        Console.WriteLine("\nÇift sayılar:");
        foreach (var sayi in ciftSayilar)
        {
            Console.WriteLine(sayi);
        }
        
        Console.ReadKey();
    }
}
```

## Anonim Metodlar ve Lambda İfadeleri

C#, delege örnekleri oluşturmanın daha kısa yollarını sunar: anonim metodlar ve lambda ifadeleri.

```csharp
using System;

class Program
{
    delegate int MatematikIslemi(int x, int y);
    
    static void Main(string[] args)
    {
        // Normal metot referansı
        MatematikIslemi toplama = Topla;
        Console.WriteLine($"Toplam: {toplama(5, 3)}");
        
        // Anonim metot kullanımı
        MatematikIslemi cikarma = delegate(int x, int y) {
            return x - y;
        };
        Console.WriteLine($"Fark: {cikarma(10, 4)}");
        
        // Lambda ifadesi kullanımı
        MatematikIslemi carpma = (x, y) => x * y;
        Console.WriteLine($"Çarpım: {carpma(6, 7)}");
        
        // Lambda ile karmaşık işlem
        MatematikIslemi karmaşık = (x, y) => {
            int sonuc = x * y;
            if (sonuc > 50) return sonuc / 2;
            return sonuc;
        };
        Console.WriteLine($"Karmaşık işlem sonucu: {karmaşık(9, 8)}");
        
        Console.ReadKey();
    }
    
    static int Topla(int x, int y)
    {
        return x + y;
    }
}
```

## Olaylar (Events) ve Delegeler

Delegeler, .NET'teki olay (event) mekanizmasının temelidir. Events, belirli bir olay gerçekleştiğinde bildirim almak isteyenlerin abone olabileceği bir mekanizma sağlar.

```csharp
using System;

// Olay argümanları için sınıf
public class SicaklikDegisimEventArgs : EventArgs
{
    public float YeniSicaklik { get; set; }
    
    public SicaklikDegisimEventArgs(float sicaklik)
    {
        YeniSicaklik = sicaklik;
    }
}

// Termometre sınıfı
public class Termometre
{
    private float _anlikSicaklik;
    
    // Olay tanımı
    public event EventHandler<SicaklikDegisimEventArgs> SicaklikDegisti;
    
    public float AnlikSicaklik
    {
        get { return _anlikSicaklik; }
        set
        {
            if (_anlikSicaklik != value)
            {
                _anlikSicaklik = value;
                // Olayı tetikle
                OnSicaklikDegisti(new SicaklikDegisimEventArgs(_anlikSicaklik));
            }
        }
    }
    
    // Olay tetikleyici metot
    protected virtual void OnSicaklikDegisti(SicaklikDegisimEventArgs e)
    {
        // Thread-safe olay tetikleme
        EventHandler<SicaklikDegisimEventArgs> handler = SicaklikDegisti;
        if (handler != null)
        {
            handler(this, e);
        }
    }
}

class Program
{
    static void Main(string[] args)
    {
        Termometre termometre = new Termometre();
        
        // Olaya abone olma
        termometre.SicaklikDegisti += Termometre_SicaklikDegisti;
        
        // Sıcaklık değerlerini değiştir ve olayı tetikle
        Console.WriteLine("Sıcaklık değerleri değiştiriliyor...");
        termometre.AnlikSicaklik = 25.5f;
        termometre.AnlikSicaklik = 26.0f;
        termometre.AnlikSicaklik = 30.5f;
        
        // Lambda ile olaya abone olma
        termometre.SicaklikDegisti += (sender, e) => {
            Console.WriteLine($"[Lambda] Kritik sıcaklık uyarısı: {e.YeniSicaklik} derece");
        };
        
        // Bir değişiklik daha
        termometre.AnlikSicaklik = 32.0f;
        
        Console.ReadKey();
    }
    
    // Olay işleyici metot
    private static void Termometre_SicaklikDegisti(object sender, SicaklikDegisimEventArgs e)
    {
        Console.WriteLine($"Sıcaklık değişti! Yeni değer: {e.YeniSicaklik} derece");
    }
}
```

## Asenkron Programlama ve Delegeler

Delegeler, asenkron programlamada da önemli bir rol oynar. Aşağıda asenkron operasyonlarda delegelerin kullanımına bir örnek verilmiştir.

```csharp
using System;
using System.Threading;
using System.Threading.Tasks;

class Program
{
    // Asenkron işlem için callback delegesi
    delegate void AsenkronIslemTamamlandi(string sonuc);
    
    static void Main(string[] args)
    {
        Console.WriteLine("Asenkron işlem başlatılıyor...");
        
        // Asenkron işlemi başlat
        UzunSurenIslemBaslat((sonuc) => {
            Console.WriteLine($"İşlem tamamlandı, sonuç: {sonuc}");
        });
        
        Console.WriteLine("Ana thread çalışmaya devam ediyor...");

        // Modern yaklaşım (Task ve async/await)
        ModernAsenkronOrnegi().Wait();
        
        Console.WriteLine("Program sonlandı. Çıkmak için bir tuşa basın.");
        Console.ReadKey();
    }
    
    // Callback delegesi kullanan eski tarz asenkron metot
    static void UzunSurenIslemBaslat(AsenkronIslemTamamlandi callback)
    {
        // Yeni bir thread başlat
        Thread t = new Thread(() => {
            Console.WriteLine("Uzun süren işlem çalışıyor...");
            // İşlemi simüle et
            Thread.Sleep(3000);
            // İşlem tamamlandığında callback'i çağır
            callback("İşlem başarılı!");
        });
        
        t.Start();
    }
    
    // Modern asenkron programlama yaklaşımı
    static async Task ModernAsenkronOrnegi()
    {
        Console.WriteLine("\nModern asenkron örnek başlatılıyor...");
        
        // Task tanımla
        Task<string> islem = Task.Run(() => {
            Console.WriteLine("Task içinde uzun süren işlem çalışıyor...");
            Thread.Sleep(2000);
            return "Task tamamlandı!";
        });
        
        // Sonucu bekle
        string sonuc = await islem;
        Console.WriteLine($"Task sonucu: {sonuc}");
    }
}
```

## Pratik Örnekler ve Senaryolar

### Örnek 1: Strateji Deseni Uygulama

```csharp
using System;

// Ödeme stratejisi delegesi
delegate decimal OdemeStratejisi(decimal tutar);

class Program
{
    static void Main(string[] args)
    {
        OdemeStratejisi normalOdeme = tutar => tutar;
        OdemeStratejisi ogrenciIndirimi = tutar => tutar * 0.8m;  // %20 indirim
        OdemeStratejisi vipIndirimi = tutar => tutar * 0.9m;      // %10 indirim
        
        decimal urunFiyati = 100;
        
        // Normal müşteri için hesaplama
        SiparisOlustur("Ahmet Yılmaz", urunFiyati, normalOdeme);
        
        // Öğrenci için hesaplama
        SiparisOlustur("Ayşe Demir (Öğrenci)", urunFiyati, ogrenciIndirimi);
        
        // VIP müşteri için hesaplama
        SiparisOlustur("Mehmet Öz (VIP)", urunFiyati, vipIndirimi);
        
        Console.ReadKey();
    }
    
    static void SiparisOlustur(string musteri, decimal tutar, OdemeStratejisi strateji)
    {
        decimal odenecekTutar = strateji(tutar);
        Console.WriteLine($"{musteri} için sipariş tutarı: {tutar:C}, ödeyeceği tutar: {odenecekTutar:C}");
    }
}
```

### Örnek 2: Filtre ve Sıralama İşlemleri

```csharp
using System;
using System.Collections.Generic;
using System.Linq;

class Urun
{
    public int Id { get; set; }
    public string Ad { get; set; }
    public decimal Fiyat { get; set; }
    public string Kategori { get; set; }
    
    public override string ToString()
    {
        return $"[{Id}] {Ad} - {Fiyat:C} ({Kategori})";
    }
}

class Program
{
    static void Main(string[] args)
    {
        List<Urun> urunler = new List<Urun>
        {
            new Urun { Id = 1, Ad = "Laptop", Fiyat = 15000, Kategori = "Elektronik" },
            new Urun { Id = 2, Ad = "Telefon", Fiyat = 8000, Kategori = "Elektronik" },
            new Urun { Id = 3, Ad = "Masa", Fiyat = 2000, Kategori = "Mobilya" },
            new Urun { Id = 4, Ad = "Sandalye", Fiyat = 800, Kategori = "Mobilya" },
            new Urun { Id = 5, Ad = "Tablet", Fiyat = 5000, Kategori = "Elektronik" },
            new Urun { Id = 6, Ad = "Kitaplık", Fiyat = 1500, Kategori = "Mobilya" }
        };
        
        // Filtreleme delegeleri
        Func<Urun, bool> elektronikUrunler = u => u.Kategori == "Elektronik";
        Func<Urun, bool> ucuzUrunler = u => u.Fiyat < 2000;
        
        // Sıralama delegeleri
        Func<Urun, decimal> fiyataGoreSirala = u => u.Fiyat;
        Func<Urun, string> adaGoreSirala = u => u.Ad;
        
        Console.WriteLine("Tüm ürünler:");
        UrunleriListele(urunler);
        
        Console.WriteLine("\nElektronik ürünler:");
        UrunleriListele(urunler.Where(elektronikUrunler).ToList());
        
        Console.WriteLine("\n2000 TL'den ucuz ürünler:");
        UrunleriListele(urunler.Where(ucuzUrunler).ToList());
        
        Console.WriteLine("\nFiyata göre sıralı ürünler:");
        UrunleriListele(urunler.OrderBy(fiyataGoreSirala).ToList());
        
        Console.WriteLine("\nAda göre sıralı ürünler:");
        UrunleriListele(urunler.OrderBy(adaGoreSirala).ToList());
        
        Console.WriteLine("\nDelegeler birleştirilerek: Fiyatı düşük elektronik ürünler:");
        UrunleriListele(urunler.Where(u => elektronikUrunler(u) && ucuzUrunler(u)).ToList());
        
        Console.ReadKey();
    }
    
    static void UrunleriListele(List<Urun> urunler)
    {
        foreach (var urun in urunler)
        {
            Console.WriteLine(urun);
        }
    }
}
```

### Örnek 3: Komut Deseni Uygulaması

```csharp
using System;
using System.Collections.Generic;

// Komut delegesi
delegate void Komut();

class HesapMakinesi
{
    private double sonuc = 0;
    private Dictionary<string, Komut> komutlar;
    
    public HesapMakinesi()
    {
        // Komutları tanımla
        komutlar = new Dictionary<string, Komut>
        {
            { "temizle", Temizle },
            { "goster", Goster }
        };
    }
    
    public void KomutEkle(string isim, Komut komut)
    {
        komutlar[isim] = komut;
    }
    
    public void KomutCalistir(string isim)
    {
        if (komutlar.ContainsKey(isim))
        {
            komutlar[isim]();
        }
        else
        {
            Console.WriteLine($"Komut bulunamadı: {isim}");
        }
    }
    
    public double Sonuc
    {
        get { return sonuc; }
        set { sonuc = value; }
    }
    
    private void Temizle()
    {
        sonuc = 0;
        Console.WriteLine("Sonuç temizlendi.");
    }
    
    private void Goster()
    {
        Console.WriteLine($"Mevcut sonuç: {sonuc}");
    }
}

class Program
{
    static void Main(string[] args)
    {
        HesapMakinesi hesapMakinesi = new HesapMakinesi();
        
        // Kendi komutlarımızı ekleyelim
        hesapMakinesi.KomutEkle("topla5", () => {
            hesapMakinesi.Sonuc += 5;
            Console.WriteLine("5 eklendi.");
        });
        
        hesapMakinesi.KomutEkle("carp2", () => {
            hesapMakinesi.Sonuc *= 2;
            Console.WriteLine("2 ile çarpıldı.");
        });
        
        // Komutları çalıştıralım
        hesapMakinesi.KomutCalistir("goster");  // 0
        hesapMakinesi.KomutCalistir("topla5");  // 5 eklendi
        hesapMakinesi.KomutCalistir("goster");  // 5
        hesapMakinesi.KomutCalistir("topla5");  // 5 daha eklendi
        hesapMakinesi.KomutCalistir("carp2");   // 2 ile çarpıldı
        hesapMakinesi.KomutCalistir("goster");  // 20
        hesapMakinesi.KomutCalistir("temizle"); // Temizlendi
        hesapMakinesi.KomutCalistir("goster");  // 0
        
        Console.ReadKey();
    }
}
```

## En İyi Uygulamalar ve İpuçları

1. **İsim Kuralları**
   - Delege tiplerini tanımlarken isimlendirme kuralına uyun: genellikle "EventHandler" veya "Callback" son ekleriyle bitin.
   - Örnek: `OdemeTamamlandiEventHandler`, `VeriYuklendiCallback`.

2. **Null Kontrolü**
   - Delegeleri çağırmadan önce null kontrolü yapın.
   ```csharp
   minCalisacakDelege?.Invoke(parametre);
   ```

3. **Çok Kademeli Delegelerle Dikkatli Olun**
   - Çok kademeli (multicast) delegelerde, delegenin birden fazla metodu işaret ettiğini ve her bir metodun çağrılacağını unutmayın.
   - Dönüş değeri olan çok kademeli delegelerde, sadece en son çağrılan metodun dönüş değeri alınır.

4. **Yerleşik Delegeleri Kullanın**
   - Mümkün olduğunca kendi delege tipinizi tanımlamak yerine `Action<>`, `Func<>` ve `Predicate<>` gibi yerleşik delegeleri kullanın.

5. **Tür Güvenliği**
   - Delegeler tür güvenlidir, bu nedenle beklenen imzaya sahip metodları kabul ederler.
   - Bu özellik, delegelerin C/C++'taki fonksiyon işaretçilerinden daha güvenli olmasını sağlar.

6. **Olaylarda Delegate Kullanımı**
   - Olay tanımlarken `EventHandler` veya `EventHandler<T>` kullanın.
   - Özel delegeler yerine genel delege tiplerini tercih edin.

7. **Lambda İfadeleri ve Kapsam**
   - Lambda ifadeleri yerel değişkenleri kapatabilir (capture), bu da kapsamlı bir şekilde anlaşılması gereken bir kavramdır.
   - Bu davranış, istenmeden bellek sızıntılarına veya beklenmeyen davranışlara neden olabilir.

8. **Asenkron Programlamada Kapsam Sorunları**
   - Asenkron metotlarda lambdaları kullanırken, lambda'nın kapsamı içindeki değişkenlerin değişebileceğini unutmayın.

9. **Performans Hususları**
   - Delege çağrıları, düzenli metot çağrılarından biraz daha yavaş olabilir, ancak modern .NET sürümlerinde bu fark minimumdur.
   - Kritik performans yollarında çok fazla delege kullanımından kaçının.

10. **Thread-Safe Olay Tetikleme**
    - Olayları tetiklerken thread güvenliği için delege referansını bir yerel değişkene kopyalayın.
    ```csharp
    var handler = OnVeriGuncellendi;
    handler?.Invoke(this, e);
    ```
