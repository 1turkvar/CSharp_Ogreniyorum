# C# Constructor (Yapıcı Metot) Rehberi

## İçindekiler
1. [Yapıcı Metot Nedir?](#yapici-metot-nedir)
2. [Yapıcı Metot Özellikleri](#yapici-metot-ozellikleri)
3. [Varsayılan Constructor](#varsayilan-constructor)
4. [Parametreli Constructor](#parametreli-constructor)
5. [Özel Constructor](#ozel-constructor)
6. [Static Constructor](#static-constructor)
7. [Constructor Overloading](#constructor-overloading)
8. [Constructor Chaining](#constructor-chaining)
9. [Constructor ve Kalıtım İlişkisi](#constructor-ve-kalitim-iliskisi)
10. [Best Practices](#best-practices)
11. [Örnek Uygulamalar](#ornek-uygulamalar)

## Yapıcı Metot Nedir? <a name="yapici-metot-nedir"></a>

Constructor (yapıcı metot), bir sınıftan nesne oluşturulduğunda otomatik olarak çağrılan ve nesnenin başlangıç durumunu ayarlayan özel bir metottur. Constructorlar, nesnelerin bellekte oluşturulduğu anda başlangıç değerlerini atamak ve gerekli kaynakları hazırlamak için kullanılır.

```csharp
public class Ogrenci
{
    // Constructor (Yapıcı Metot)
    public Ogrenci()
    {
        Console.WriteLine("Yeni bir öğrenci nesnesi oluşturuldu.");
    }
}

// Kullanımı
Ogrenci ogrenci1 = new Ogrenci(); // Constructor otomatik çağrılır
```

## Yapıcı Metot Özellikleri <a name="yapici-metot-ozellikleri"></a>

Constructor'ların temel özellikleri şunlardır:

1. Sınıf ile aynı ismi taşırlar
2. Geri dönüş tipi yoktur (void dahil)
3. Nesne oluşturulduğunda otomatik olarak çağrılırlar
4. Erişim belirleyicileri olabilir (genellikle public)
5. Overload edilebilirler (farklı parametre listesi ile birden fazla constructor tanımlanabilir)
6. Base ve this anahtar kelimeleri ile diğer constructorlar çağrılabilir
7. Private olarak tanımlanabilirler (Singleton pattern gibi durumlarda)

## Varsayılan Constructor <a name="varsayilan-constructor"></a>

Eğer bir sınıf için hiçbir constructor tanımlanmazsa, C# derleyicisi parametre almayan varsayılan bir constructor (default constructor) oluşturur. Bu constructor, sınıfın tüm üye değişkenlerini default değerlerine atar.

```csharp
public class Araba
{
    public string Marka; // default olarak null
    public int Yil;      // default olarak 0
    
    // Varsayılan constructor derleyici tarafından otomatik oluşturulur
    // public Araba() { }
}

// Kullanımı
Araba araba1 = new Araba(); // Varsayılan constructor çağrılır
```

Ancak, eğer herhangi bir constructor tanımlarsanız, varsayılan constructor otomatik olarak oluşturulmaz. Eğer parametre almayan constructor'a ihtiyacınız varsa, kendiniz tanımlamalısınız.

## Parametreli Constructor <a name="parametreli-constructor"></a>

Nesneleri oluştururken başlangıç değerlerini daha kolay bir şekilde atamak için parametreli constructor'lar kullanılır.

```csharp
public class Ogrenci
{
    public string Ad;
    public string Soyad;
    public int OgrenciNo;
    
    // Parametreli constructor
    public Ogrenci(string ad, string soyad, int ogrenciNo)
    {
        Ad = ad;
        Soyad = soyad;
        OgrenciNo = ogrenciNo;
    }
}

// Kullanımı
Ogrenci ogrenci1 = new Ogrenci("Ali", "Yılmaz", 12345);
```

## Özel Constructor <a name="ozel-constructor"></a>

Private constructor'lar, sınıfın dışından doğrudan nesne oluşturulmasını engeller. Genellikle Singleton tasarım deseni gibi durumlarda kullanılır.

```csharp
public class DatabaseConnection
{
    private static DatabaseConnection _instance;
    
    // Private constructor
    private DatabaseConnection()
    {
        Console.WriteLine("Veritabanı bağlantısı oluşturuldu.");
    }
    
    // Singleton pattern
    public static DatabaseConnection Instance
    {
        get
        {
            if (_instance == null)
            {
                _instance = new DatabaseConnection();
            }
            return _instance;
        }
    }
}

// Kullanımı
// DatabaseConnection db = new DatabaseConnection(); // Hata! Constructor private
DatabaseConnection db = DatabaseConnection.Instance; // Doğru kullanım
```

## Static Constructor <a name="static-constructor"></a>

Static constructor'lar, bir sınıfın static üyelerini başlatmak için kullanılır. Parametresizdir ve bir sınıfın static üyelerine ilk erişimde yalnızca bir kez çağrılır.

```csharp
public class Uygulama
{
    public static string UygulamaVersiyonu;
    public static DateTime BaslangicZamani;
    
    // Static constructor
    static Uygulama()
    {
        Console.WriteLine("Static constructor çalıştı.");
        UygulamaVersiyonu = "1.0.0";
        BaslangicZamani = DateTime.Now;
    }
    
    // Normal constructor
    public Uygulama()
    {
        Console.WriteLine("Normal constructor çalıştı.");
    }
}

// Kullanımı
Console.WriteLine(Uygulama.UygulamaVersiyonu); // Static constructor çağrılır
Uygulama app1 = new Uygulama(); // Normal constructor çağrılır
Uygulama app2 = new Uygulama(); // Sadece normal constructor çağrılır, static constructor tekrar çağrılmaz
```

Static constructor'ların özellikleri:
- Erişim belirleyici (public, private vb.) kullanamaz
- Parametre alamaz
- Açıkça çağrılamaz
- Sınıfa ilk erişimde otomatik çalışır
- Uygulama süresince yalnızca bir kez çalıştırılır

## Constructor Overloading <a name="constructor-overloading"></a>

Bir sınıfta, farklı parametre listelerine sahip birden fazla constructor tanımlayabilirsiniz. Bu, nesnelerin farklı şekillerde başlatılmasına olanak tanır.

```csharp
public class Dikdortgen
{
    public double Genislik;
    public double Yukseklik;
    
    // Parametresiz constructor
    public Dikdortgen()
    {
        Genislik = 1.0;
        Yukseklik = 1.0;
    }
    
    // Tek parametreli constructor (kare)
    public Dikdortgen(double kenar)
    {
        Genislik = kenar;
        Yukseklik = kenar;
    }
    
    // İki parametreli constructor
    public Dikdortgen(double genislik, double yukseklik)
    {
        Genislik = genislik;
        Yukseklik = yukseklik;
    }
    
    public double AlanHesapla()
    {
        return Genislik * Yukseklik;
    }
}

// Kullanımı
Dikdortgen d1 = new Dikdortgen();         // Genislik=1, Yukseklik=1
Dikdortgen d2 = new Dikdortgen(5);        // Genislik=5, Yukseklik=5
Dikdortgen d3 = new Dikdortgen(3, 4);     // Genislik=3, Yukseklik=4
```

## Constructor Chaining <a name="constructor-chaining"></a>

Constructor chaining, bir constructor'ın başka bir constructor'ı çağırmasıdır. Bu, kod tekrarını önlemek için kullanılır. C# dilinde `this` anahtar kelimesi ile aynı sınıftaki diğer constructor'ları çağırabilirsiniz.

```csharp
public class Calisan
{
    public string Ad;
    public string Soyad;
    public string Departman;
    public decimal Maas;
    
    // Ana constructor
    public Calisan(string ad, string soyad, string departman, decimal maas)
    {
        Ad = ad;
        Soyad = soyad;
        Departman = departman;
        Maas = maas;
    }
    
    // Departman parametresi olmayan constructor
    public Calisan(string ad, string soyad, decimal maas)
        : this(ad, soyad, "Belirsiz", maas)
    {
        // İş mantığı buraya yazılabilir
    }
    
    // Sadece ad ve soyad alan constructor
    public Calisan(string ad, string soyad)
        : this(ad, soyad, "Belirsiz", 0)
    {
        // İş mantığı buraya yazılabilir
    }
}

// Kullanımı
Calisan c1 = new Calisan("Ahmet", "Yılmaz", "Bilişim", 15000);
Calisan c2 = new Calisan("Mehmet", "Kaya", 12000); // Departman="Belirsiz"
Calisan c3 = new Calisan("Ayşe", "Demir"); // Departman="Belirsiz", Maas=0
```

## Constructor ve Kalıtım İlişkisi <a name="constructor-ve-kalitim-iliskisi"></a>

Kalıtım kullanıldığında, alt sınıf (derived class) nesnesi oluşturulduğunda, önce üst sınıfın (base class) constructor'ı, ardından alt sınıfın constructor'ı çalıştırılır. Alt sınıf constructor'ında `base` anahtar kelimesi ile üst sınıfın belirli bir constructor'ını çağırabilirsiniz.

```csharp
public class Kisi
{
    public string Ad;
    public string Soyad;
    
    public Kisi(string ad, string soyad)
    {
        Ad = ad;
        Soyad = soyad;
        Console.WriteLine("Kisi constructor'ı çalıştı.");
    }
}

public class Ogrenci : Kisi
{
    public int OgrenciNo;
    
    // base anahtar kelimesi ile üst sınıf constructor'ını çağırma
    public Ogrenci(string ad, string soyad, int ogrenciNo)
        : base(ad, soyad)
    {
        OgrenciNo = ogrenciNo;
        Console.WriteLine("Ogrenci constructor'ı çalıştı.");
    }
}

// Kullanımı
Ogrenci ogr = new Ogrenci("Ali", "Veli", 12345);
// Çıktı:
// Kisi constructor'ı çalıştı.
// Ogrenci constructor'ı çalıştı.
```

Eğer alt sınıf constructor'ında açıkça `base` ile üst sınıf constructor'ı çağrılmazsa, üst sınıfın parametresiz constructor'ı otomatik olarak çağrılır. Eğer üst sınıfta parametresiz constructor yoksa, derleyici hata verir.

## Best Practices <a name="best-practices"></a>

Constructor'ları kullanırken dikkat edilmesi gereken bazı best practice'ler:

1. **Constructor'ları Basit Tutun**: Constructor'lar karmaşık iş mantığı içermemeli, sadece nesnenin temel durumunu ayarlamalıdır.

2. **Exception Fırlatmaktan Kaçının**: Constructor'larda mümkünse exception fırlatmaktan kaçının, nesnenin güvenli bir şekilde oluşturulmasını sağlayın.

3. **Constructor Injection**: Bağımlılıkları constructor üzerinden enjekte edin (Dependency Injection pattern).

4. **Immutable Nesneler**: Immutable (değişmez) nesneler için tüm değerleri constructor'da alıp atayın.

5. **Factory Metot Deseni**: Karmaşık nesne oluşturma işlemleri için Constructor yerine Factory metotları kullanmayı düşünün.

6. **Validasyon**: Constructor'da parametre validasyonları yaparak geçersiz nesne oluşumunu engelleyin.

```csharp
public class Musteri
{
    public string Ad { get; }
    public string Soyad { get; }
    public string Email { get; }
    
    public Musteri(string ad, string soyad, string email)
    {
        // Parametre validasyonu
        if (string.IsNullOrEmpty(ad))
            throw new ArgumentException("Ad boş olamaz");
        
        if (string.IsNullOrEmpty(soyad))
            throw new ArgumentException("Soyad boş olamaz");
        
        if (string.IsNullOrEmpty(email) || !email.Contains("@"))
            throw new ArgumentException("Geçersiz email formatı");
        
        Ad = ad;
        Soyad = soyad;
        Email = email;
    }
}
```

## Örnek Uygulamalar <a name="ornek-uygulamalar"></a>

### Örnek 1: Banka Hesabı

```csharp
public class BankaHesabi
{
    public string HesapNo { get; }
    public string MusteriAdi { get; }
    private decimal _bakiye;
    
    // Default constructor
    public BankaHesabi()
    {
        HesapNo = Guid.NewGuid().ToString().Substring(0, 8);
        MusteriAdi = "Misafir";
        _bakiye = 0;
    }
    
    // Parametreli constructor
    public BankaHesabi(string musteriAdi, decimal baslangicBakiye)
    {
        if (baslangicBakiye < 0)
            throw new ArgumentException("Başlangıç bakiyesi negatif olamaz");
        
        HesapNo = Guid.NewGuid().ToString().Substring(0, 8);
        MusteriAdi = musteriAdi;
        _bakiye = baslangicBakiye;
    }
    
    // Bakiye property'si
    public decimal Bakiye
    {
        get { return _bakiye; }
    }
    
    // Para yatırma metodu
    public void ParaYatir(decimal miktar)
    {
        if (miktar <= 0)
            throw new ArgumentException("Yatırılacak miktar pozitif olmalıdır");
        
        _bakiye += miktar;
    }
    
    // Para çekme metodu
    public bool ParaCek(decimal miktar)
    {
        if (miktar <= 0)
            throw new ArgumentException("Çekilecek miktar pozitif olmalıdır");
        
        if (_bakiye >= miktar)
        {
            _bakiye -= miktar;
            return true;
        }
        
        return false;
    }
}

// Kullanımı
BankaHesabi hesap1 = new BankaHesabi(); // Boş hesap
BankaHesabi hesap2 = new BankaHesabi("Ahmet Yılmaz", 1000); // 1000 TL bakiyeli hesap
```

### Örnek 2: Singleton Pattern

```csharp
public class LogManager
{
    private static LogManager _instance;
    private static readonly object _lock = new object();
    
    // Private constructor
    private LogManager()
    {
        Console.WriteLine("LogManager initialized");
    }
    
    // Thread-safe singleton instance
    public static LogManager Instance
    {
        get
        {
            if (_instance == null)
            {
                lock (_lock)
                {
                    if (_instance == null)
                    {
                        _instance = new LogManager();
                    }
                }
            }
            return _instance;
        }
    }
    
    public void Log(string message)
    {
        Console.WriteLine($"[{DateTime.Now}] {message}");
    }
}

// Kullanımı
LogManager.Instance.Log("Uygulama başlatıldı.");
LogManager.Instance.Log("Kullanıcı girişi yapıldı.");
```

### Örnek 3: Immutable Nesne

```csharp
public class Nokta
{
    public double X { get; }
    public double Y { get; }
    
    public Nokta(double x, double y)
    {
        X = x;
        Y = y;
    }
    
    // Nokta değiştirilemez (immutable), yeni nokta oluşturulur
    public Nokta TasiX(double mesafe)
    {
        return new Nokta(X + mesafe, Y);
    }
    
    public Nokta TasiY(double mesafe)
    {
        return new Nokta(X, Y + mesafe);
    }
    
    public double MesafeHesapla(Nokta digerNokta)
    {
        double dx = X - digerNokta.X;
        double dy = Y - digerNokta.Y;
        return Math.Sqrt(dx * dx + dy * dy);
    }
    
    public override string ToString()
    {
        return $"({X}, {Y})";
    }
}

// Kullanımı
Nokta n1 = new Nokta(0, 0);
Nokta n2 = n1.TasiX(3).TasiY(4); // Yeni nokta (3, 4)
Console.WriteLine($"İki nokta arası mesafe: {n1.MesafeHesapla(n2)}"); // 5
```

### Örnek 4: Factory Metotları

```csharp
public class Sekil
{
    // Protected constructor - doğrudan erişimi engelleme
    protected Sekil()
    {
    }
    
    // Factory metotları
    public static Sekil OlusturDaire(double yaricap)
    {
        return new Daire(yaricap);
    }
    
    public static Sekil OlusturKare(double kenar)
    {
        return new Kare(kenar);
    }
    
    public static Sekil OlusturDikdortgen(double genislik, double yukseklik)
    {
        return new Dikdortgen(genislik, yukseklik);
    }
    
    // Virtual methods
    public virtual double AlanHesapla()
    {
        return 0;
    }
    
    public virtual double CevreHesapla()
    {
        return 0;
    }
}

public class Daire : Sekil
{
    public double Yaricap { get; }
    
    // Internal constructor
    internal Daire(double yaricap)
    {
        if (yaricap <= 0)
            throw new ArgumentException("Yarıçap pozitif olmalıdır");
            
        Yaricap = yaricap;
    }
    
    public override double AlanHesapla()
    {
        return Math.PI * Yaricap * Yaricap;
    }
    
    public override double CevreHesapla()
    {
        return 2 * Math.PI * Yaricap;
    }
}

public class Kare : Sekil
{
    public double Kenar { get; }
    
    internal Kare(double kenar)
    {
        if (kenar <= 0)
            throw new ArgumentException("Kenar uzunluğu pozitif olmalıdır");
            
        Kenar = kenar;
    }
    
    public override double AlanHesapla()
    {
        return Kenar * Kenar;
    }
    
    public override double CevreHesapla()
    {
        return 4 * Kenar;
    }
}

public class Dikdortgen : Sekil
{
    public double Genislik { get; }
    public double Yukseklik { get; }
    
    internal Dikdortgen(double genislik, double yukseklik)
    {
        if (genislik <= 0 || yukseklik <= 0)
            throw new ArgumentException("Boyutlar pozitif olmalıdır");
            
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

// Kullanımı
Sekil daire = Sekil.OlusturDaire(5);
Sekil kare = Sekil.OlusturKare(4);
Sekil dikdortgen = Sekil.OlusturDikdortgen(3, 6);

Console.WriteLine($"Daire Alanı: {daire.AlanHesapla()}");
Console.WriteLine($"Kare Alanı: {kare.AlanHesapla()}");
Console.WriteLine($"Dikdörtgen Alanı: {dikdortgen.AlanHesapla()}");
```

Bu örnekler, constructor'ların farklı senaryolarda nasıl kullanılabileceğini göstermektedir. C# dilindeki constructor'lar, nesne yönelimli programlamanın temel taşlarından biridir ve doğru kullanıldığında kod kalitesini ve güvenliğini artırabilir.
