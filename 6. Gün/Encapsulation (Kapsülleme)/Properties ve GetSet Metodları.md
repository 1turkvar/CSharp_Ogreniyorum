# C# Properties ve Get/Set Metodları Kılavuzu

## İçindekiler
1. [Giriş](#giriş)
2. [Properties (Özellikler) Nedir?](#properties-özellikler-nedir)
3. [Property Sözdizimi](#property-sözdizimi)
4. [Auto-Implemented Properties](#auto-implemented-properties)
5. [Read-Only ve Write-Only Properties](#read-only-ve-write-only-properties)
6. [Expression-Bodied Properties](#expression-bodied-properties)
7. [Computed Properties](#computed-properties)
8. [Static Properties](#static-properties)
9. [Property Erişim Belirleyicileri](#property-erişim-belirleyicileri)
10. [Değer Doğrulama ve Properties](#değer-doğrulama-ve-properties)
11. [İleri Seviye Property Kullanımları](#i̇leri-seviye-property-kullanımları)
12. [Properties vs Fields](#properties-vs-fields)
13. [En İyi Uygulamalar](#en-i̇yi-uygulamalar)
14. [Sonuç](#sonuç)

## Giriş

C# programlama dilinde "property" (özellik) kavramı, sınıf verilerine kontrollü erişim sağlamak için kullanılan özel üyelerdir. Properties, dışarıdan bir alan (field) gibi görünürken, aslında içeride bir metodun davranışını sergilerler. Bu özellik, nesne yönelimli programlamanın kapsülleme prensibini başarılı bir şekilde uygulamayı sağlar.

## Properties (Özellikler) Nedir?

Property, bir sınıfın özniteliklerine (field) erişmek ve değiştirmek için kullanılan özel bir üyedir. Properties, aşağıdaki avantajları sağlar:

- Veri erişimini kontrol etme
- Değer atamalarını doğrulama
- Hesaplanmış değerler üretme
- Veri değişikliklerinde olay tetikleme
- Sınıfın iç yapısını dış dünyadan gizleme

Properties olmadan önce, geleneksel olarak getter ve setter metodları kullanılırdı:

```csharp
public class Kisi
{
    private string _ad;
    
    public string GetAd()
    {
        return _ad;
    }
    
    public void SetAd(string value)
    {
        _ad = value;
    }
}
```

Properties ile bu işlem daha zarif hale gelir:

```csharp
public class Kisi
{
    private string _ad;
    
    public string Ad
    {
        get { return _ad; }
        set { _ad = value; }
    }
}
```

## Property Sözdizimi

Bir property tanımlamak için temel sözdizimi şu şekildedir:

```csharp
[erişim_belirleyici] [veri_tipi] [property_adı]
{
    get { return [field]; }
    set { [field] = value; }
}
```

Burada:
- **erişim_belirleyici**: `public`, `private`, `protected`, `internal` veya bunların kombinasyonları.
- **veri_tipi**: Property'nin dönüş tipi.
- **property_adı**: Property'nin adı (genellikle Pascal Case ile yazılır).
- **get**: Property değerini okumak için kullanılan kod bloğu.
- **set**: Property değerini değiştirmek için kullanılan kod bloğu. `value` anahtar kelimesi, atanmak istenen değeri temsil eder.

Örnek:

```csharp
public class Calisan
{
    private string _ad;
    private decimal _maas;
    
    public string Ad
    {
        get { return _ad; }
        set { _ad = value; }
    }
    
    public decimal Maas
    {
        get { return _maas; }
        set 
        { 
            if (value >= 0)
                _maas = value;
            else
                throw new ArgumentException("Maaş negatif olamaz.");
        }
    }
}
```

## Auto-Implemented Properties

C# 3.0 ile birlikte, arka planda otomatik olarak özel bir alan oluşturan ve temel get/set işlemlerini gerçekleştiren "auto-implemented properties" (otomatik uygulanmış özellikler) tanıtıldı:

```csharp
public class Urun
{
    public string Ad { get; set; }
    public decimal Fiyat { get; set; }
}
```

Bu durumda, C# derleyicisi arka planda gizli alanlar oluşturur ve temel get/set fonksiyonlarını otomatik olarak uygular. Bu, kod yazımını büyük ölçüde basitleştirir.

C# 6.0 ile birlikte, auto-implemented property'lere başlangıç değerleri atama özelliği eklendi:

```csharp
public class Urun
{
    public string Ad { get; set; } = "İsimsiz Ürün";
    public decimal Fiyat { get; set; } = 0;
}
```

## Read-Only ve Write-Only Properties

Bir property'yi salt okunur (read-only) yapmak için `set` bloğunu kaldırabilirsiniz:

```csharp
public class Daire
{
    private double _yaricap;
    
    public double Yaricap
    {
        get { return _yaricap; }
        set { _yaricap = value; }
    }
    
    public double Alan
    {
        get { return Math.PI * _yaricap * _yaricap; }
    }
}
```

Yukarıdaki örnekte, `Alan` property'si salt okunurdur ve sadece hesaplanmış bir değer döndürür.

C# 6.0 ile birlikte, readonly auto-implemented property'ler tanıtıldı:

```csharp
public class Urun
{
    public string Kod { get; } = Guid.NewGuid().ToString();
    public string Ad { get; set; }
}
```

Write-only property'ler de mümkündür, ancak genellikle iyi bir tasarım pratiği değildir:

```csharp
public string Sifre
{
    set { _sifreliVeri = SifreleVeSakla(value); }
}
```

## Expression-Bodied Properties

C# 6.0 ve sonraki sürümlerde, kısa hesaplama gerektiren property'ler için "expression-bodied member" sözdizimi kullanılabilir:

```csharp
public class Daire
{
    public double Yaricap { get; set; }
    
    // Expression-bodied read-only property
    public double Alan => Math.PI * Yaricap * Yaricap;
    
    public double Cevre => 2 * Math.PI * Yaricap;
}
```

C# 7.0'da, hem get hem de set için expression-bodied sözdizimi eklendi:

```csharp
private string _ad;

public string Ad
{
    get => _ad;
    set => _ad = value ?? throw new ArgumentNullException(nameof(value));
}
```

## Computed Properties

Hesaplanmış property'ler, değerini diğer alanlardan veya hesaplamalardan alan property'lerdir:

```csharp
public class Dikdortgen
{
    public double Uzunluk { get; set; }
    public double Genislik { get; set; }
    
    public double Alan => Uzunluk * Genislik;
    public double Cevre => 2 * (Uzunluk + Genislik);
    public bool KareDir => Uzunluk == Genislik;
}
```

## Static Properties

Properties, static olarak da tanımlanabilir:

```csharp
public class Matematik
{
    public static double Pi => 3.14159265359;
    
    private static double _e = 2.71828;
    public static double E
    {
        get { return _e; }
    }
}
```

Static property'lere erişim için sınıf adı kullanılır:

```csharp
double pi = Matematik.Pi;
```

## Property Erişim Belirleyicileri

C# 7.0 ve sonraki sürümlerde, property accessors (get ve set) için farklı erişim belirleyicileri kullanılabilir:

```csharp
public class Kisi
{
    public string Ad { get; private set; }
    public string Soyad { get; private set; }
    
    public Kisi(string ad, string soyad)
    {
        Ad = ad;
        Soyad = soyad;
    }
    
    // Tam ad public olarak okunabilir, ama değiştirilemez
    public string TamAd
    {
        get { return $"{Ad} {Soyad}"; }
    }
}
```

Bu örnekte:
- `Ad` ve `Soyad` property'leri public olarak okunabilir, ancak yalnızca sınıf içinden değiştirilebilir.
- `TamAd` property'si salt okunurdur.

## Değer Doğrulama ve Properties

Property'lerin en önemli kullanım alanlarından biri, değer atamaları sırasında doğrulama yapmaktır:

```csharp
public class Urun
{
    private decimal _fiyat;
    
    public decimal Fiyat
    {
        get { return _fiyat; }
        set
        {
            if (value < 0)
                throw new ArgumentException("Fiyat negatif olamaz.");
            _fiyat = value;
        }
    }
    
    private int _stokMiktari;
    
    public int StokMiktari
    {
        get { return _stokMiktari; }
        set
        {
            if (value < 0)
                throw new ArgumentException("Stok miktarı negatif olamaz.");
            
            if (_stokMiktari != value)
            {
                _stokMiktari = value;
                StokDegisti?.Invoke(this, EventArgs.Empty);
            }
        }
    }
    
    public event EventHandler StokDegisti;
}
```

## İleri Seviye Property Kullanımları

### Indexer Properties

C#, bir sınıfa dizi benzeri erişim sağlayan özel bir property türü olan "indexer" özelliğini destekler:

```csharp
public class UrunKatalogu
{
    private Dictionary<string, decimal> _urunler = new Dictionary<string, decimal>();
    
    // Indexer property
    public decimal this[string urunKodu]
    {
        get
        {
            if (_urunler.ContainsKey(urunKodu))
                return _urunler[urunKodu];
            return 0;
        }
        set
        {
            _urunler[urunKodu] = value;
        }
    }
}
```

Kullanımı:

```csharp
UrunKatalogu katalog = new UrunKatalogu();
katalog["A001"] = 29.99m;
decimal fiyat = katalog["A001"]; // 29.99 döner
```

### Interface Implementasyonunda Properties

Interface'ler de property tanımları içerebilir:

```csharp
public interface IKisi
{
    string Ad { get; set; }
    string Soyad { get; set; }
    string TamAd { get; }
}

public class Calisan : IKisi
{
    public string Ad { get; set; }
    public string Soyad { get; set; }
    public string TamAd => $"{Ad} {Soyad}";
    
    public string Pozisyon { get; set; }
}
```

### Virtual ve Override Properties

Properties, sanal (virtual) olarak tanımlanabilir ve alt sınıflarda override edilebilir:

```csharp
public class Sekil
{
    public virtual double Alan { get; protected set; }
}

public class Daire : Sekil
{
    private double _yaricap;
    
    public double Yaricap
    {
        get { return _yaricap; }
        set
        {
            _yaricap = value;
            Alan = Math.PI * _yaricap * _yaricap;
        }
    }
}
```

### Abstract Properties

Soyut (abstract) sınıflarda, abstract properties tanımlanabilir:

```csharp
public abstract class Sekil
{
    public abstract double Alan { get; }
    public abstract double Cevre { get; }
}

public class Daire : Sekil
{
    public double Yaricap { get; set; }
    
    public override double Alan => Math.PI * Yaricap * Yaricap;
    public override double Cevre => 2 * Math.PI * Yaricap;
}
```

## Properties vs Fields

Properties ve fields (alanlar) arasındaki farkları anlamak önemlidir:

| Özellik | Field | Property |
|---------|-------|----------|
| Erişim kontrolü | Tek erişim belirleyici | Get/set için ayrı belirleyiciler |
| Doğrulama | Doğrudan değişiklik, doğrulama yok | Set bloğunda doğrulama yapılabilir |
| Hesaplama | Saklanan değer | Hesaplanabilir değer olabilir |
| Arayüzler | Interface'de tanımlanamaz | Interface'de tanımlanabilir |
| Serileştirme | Özelleştirilebilir değil | Özelleştirilebilir |
| Debugging | Doğrudan görülebilir | Get/set çağrıları izlenebilir |
| Miras | Override edilemez | Virtual/override edilebilir |

### Ne Zaman Field, Ne Zaman Property Kullanmalı?

- **Field kullanımı**:
  - Sınıf içi özel değişkenler için
  - Performans kritik durumlarda
  - Sadece sınıf içinden erişilecek veriler için

- **Property kullanımı**:
  - Public API'lerde dış dünyaya açılan değerler için
  - Değer doğrulama gereken durumlar için
  - Hesaplanmış değerler için
  - Interface'lerde ve abstarct sınıflarda
  - İleride değişebilecek implementasyonlar için

## En İyi Uygulamalar

### Naming Conventions (İsimlendirme Kuralları)

- Property isimleri Pascal Case kullanılmalıdır (`OgrenciAdi`, `TotalFiyat` gibi)
- Özel alanlar (backing fields) alt çizgi ile başlamalıdır (`_ogrenciAdi`)
- Boolean property'ler genellikle `Is`, `Has`, `Can` gibi ön eklerle başlamalıdır (`IsActive`, `HasPermission`)

### Tasarım Prensipleri

1. **Kapsülleme**: Property'leri, iç implementasyonu gizlemek için kullanın.

```csharp
// Kötü: Doğrudan public field
public class Kullanici
{
    public string Eposta; // Doğrulama yok!
}

// İyi: Property kullanımı
public class Kullanici
{
    private string _eposta;
    
    public string Eposta
    {
        get { return _eposta; }
        set
        {
            if (string.IsNullOrEmpty(value) || !value.Contains("@"))
                throw new ArgumentException("Geçersiz e-posta adresi.");
            _eposta = value;
        }
    }
}
```

2. **Değişmezlik (Immutability)**: Gerektiğinde read-only property'ler kullanın.

```csharp
public class Siparis
{
    public DateTime SiparisTarihi { get; }
    public string SiparisNumarasi { get; }
    
    public Siparis(string siparisNumarasi)
    {
        SiparisTarihi = DateTime.Now;
        SiparisNumarasi = siparisNumarasi;
    }
}
```

3. **İyi Dokümantasyon**: Property'leri XML dokümentasyonu ile belgelendirin.

```csharp
/// <summary>
/// Öğrencinin tam adını temsil eder.
/// </summary>
/// <remarks>Bu özellik salt okunurdur ve Ad ile Soyad özelliklerinden hesaplanır.</remarks>
public string TamAd => $"{Ad} {Soyad}";
```

4. **Side-Effect'lerden Kaçınma**: Get bloklarında yan etkiler oluşturmaktan kaçının.

```csharp
// Kötü: Get bloğunda yan etki
public string Ad
{
    get
    {
        LogAktivite(); // Get işlemi sırasında yan etki!
        return _ad;
    }
    set { _ad = value; }
}

// İyi: Get bloğu sadece değer döndürür
public string Ad
{
    get { return _ad; }
    set
    {
        if (_ad != value)
        {
            _ad = value;
            OnPropertyChanged(nameof(Ad));
        }
    }
}
```

5. **Performans Düşünceleri**: Pahalı hesaplamalar için değerleri önbelleğe alın.

```csharp
private string _tamAd;
private bool _tamAdGuncel = false;

public string Ad
{
    get { return _ad; }
    set
    {
        _ad = value;
        _tamAdGuncel = false;
    }
}

public string Soyad
{
    get { return _soyad; }
    set
    {
        _soyad = value;
        _tamAdGuncel = false;
    }
}

public string TamAd
{
    get
    {
        if (!_tamAdGuncel)
        {
            _tamAd = $"{_ad} {_soyad}";
            _tamAdGuncel = true;
        }
        return _tamAd;
    }
}
```

## Sonuç

C# Property ve get/set metodları, nesne yönelimli programlamanın temel ilkelerinden biri olan kapsüllemeyi kolayca uygulamayı sağlayan güçlü özelliklerdir. Properties, dış dünyaya bir değişken gibi görünürken, arka planda metodlar gibi çalışarak değer doğrulama, hesaplama ve diğer özellikler sağlar.

Bu kılavuzda gördüğümüz gibi, C# dilinde properties kullanarak:

- Daha temiz ve okunabilir bir kod yazabilirsiniz
- Sınıf verilerine kontrollü erişim sağlayabilirsiniz
- Değer doğrulama işlemleri gerçekleştirebilirsiniz
- Hesaplanmış değerler sunabilirsiniz
- Değer değişikliklerinde olaylar tetikleyebilirsiniz

Bu özellikler, C# dilini güçlü ve esnek bir nesne yönelimli programlama dili yapar.

Properties kullanırken, performans, tasarım ve bakım açısından en iyi uygulamaları takip etmek, daha sürdürülebilir ve etkili bir kod tabanı oluşturmanıza yardımcı olacaktır.
