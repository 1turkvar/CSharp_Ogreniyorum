# C# Protected Erişim Belirleyicisi

## İçindekiler
1. [Giriş](#giriş)
2. [Protected Erişim Belirleyicisi Nedir?](#protected-erişim-belirleyicisi-nedir)
3. [Kullanım Alanları](#kullanım-alanları)
4. [Protected vs. Private](#protected-vs-private)
5. [Protected vs. Public](#protected-vs-public)
6. [Protected Internal Erişim Belirleyicisi](#protected-internal-erişim-belirleyicisi)
7. [En İyi Uygulama Önerileri](#en-iyi-uygulama-önerileri)
8. [Kod Örnekleri](#kod-örnekleri)
9. [Sık Karşılaşılan Hatalar](#sık-karşılaşılan-hatalar)
10. [Özet](#özet)

## Giriş

C# nesne yönelimli bir programlama dili olup, veri gizleme (encapsulation) prensibini uygulamak için çeşitli erişim belirleyicileri sunar. Bunlar arasında `public`, `private`, `protected`, `internal` ve `protected internal` bulunmaktadır. Bu belge, `protected` erişim belirleyicisini detaylı bir şekilde incelemektedir.

## Protected Erişim Belirleyicisi Nedir?

`protected` erişim belirleyicisi, bir sınıf üyesinin (değişkenler, metotlar, özellikler vb.) tanımlandığı sınıf içerisinden ve bu sınıftan türetilen alt sınıflardan (derived classes) erişilebilir olmasını sağlar. Sınıf dışından doğrudan erişim mümkün değildir.

### Temel Özellikleri:

- Kalıtım hiyerarşisinde erişim sağlar
- Tanımlandığı sınıf içinden erişilebilir
- Türetilen sınıflar içinden erişilebilir
- Sınıf dışındaki kodlardan erişilemez

## Kullanım Alanları

`protected` erişim belirleyicisi genellikle aşağıdaki durumlarda kullanılır:

1. **Temel Sınıf İşlevselliği**: Alt sınıfların kullanması gereken ancak dış dünyadan gizlenmesi gereken metotlar ve özellikler.

2. **Kalıtım Sınırlaması**: Sadece türetilen sınıfların kullanabileceği yapılar oluşturmak.

3. **Kademeli Soyutlama**: Karmaşık sınıf hiyerarşilerinde işlevselliği katmanlamak ve kontrol etmek.

4. **Yardımcı İşlevler**: Ana işlevselliği destekleyen ancak doğrudan erişilmemesi gereken yardımcı metotlar.

## Protected vs. Private

| Özellik | Protected | Private |
|---------|-----------|---------|
| Tanımlandığı sınıf içinden erişim | ✓ | ✓ |
| Türetilen sınıflardan erişim | ✓ | ✗ |
| Dış kodlardan erişim | ✗ | ✗ |

`private` üyeler sadece tanımlandıkları sınıf içinden erişilebilirken, `protected` üyeler hem tanımlandıkları sınıf içinden hem de bu sınıftan türetilen alt sınıflar içinden erişilebilir.

## Protected vs. Public

| Özellik | Protected | Public |
|---------|-----------|--------|
| Tanımlandığı sınıf içinden erişim | ✓ | ✓ |
| Türetilen sınıflardan erişim | ✓ | ✓ |
| Dış kodlardan erişim | ✗ | ✓ |

`public` üyeler her yerden erişilebilirken, `protected` üyeler sadece tanımlandıkları sınıf ve türetilen sınıflar içinden erişilebilir.

## Protected Internal Erişim Belirleyicisi

C#'ta `protected internal` kombinasyonu da mevcuttur. Bu erişim belirleyicisi:

- Tanımlandığı sınıf içinden
- Aynı assembly içindeki kodlardan
- Türetilen sınıflardan (farklı assembly'de olsa bile)

erişime izin verir.

## En İyi Uygulama Önerileri

1. **Davranış mirasını destekleyin**: `protected` üyeler, alt sınıfların temel sınıf davranışını genişletmesi gerektiğinde kullanılmalıdır.

2. **Dokümantasyon sağlayın**: `protected` üyeler, türetilen sınıflar tarafından kullanılmak üzere tasarlandığından iyi belgelenmelidir.

3. **Gerektiğinde sınırlayın**: Sadece alt sınıfların gerçekten ihtiyaç duyduğu üyeleri `protected` yapın.

4. **Sabit tutun**: `protected` üyelerin davranışını sık sık değiştirmeyin, bu türetilen sınıfları bozabilir.

5. **İmplementasyon detaylarını private tutun**: `protected` sadece alt sınıfların uygulama için bilmesi gereken ayrıntılar için kullanılmalıdır.

## Kod Örnekleri

### Temel Kullanım

```csharp
public class Hayvan
{
    protected string Ad { get; set; }
    
    public Hayvan(string ad)
    {
        Ad = ad;
    }
    
    protected void YemekYe()
    {
        Console.WriteLine($"{Ad} yemek yiyor.");
    }
    
    public void SesCikar()
    {
        Console.WriteLine("Ses çıkarıyor.");
    }
}

public class Kedi : Hayvan
{
    public Kedi(string ad) : base(ad)
    {
    }
    
    public void Davran()
    {
        // Protected üyelere erişebiliyoruz
        Console.WriteLine($"Kedinin adı: {Ad}");
        YemekYe();
        
        // Public üyelere de erişebiliyoruz
        SesCikar();
    }
}

// Kullanım
public class Program
{
    public static void Main()
    {
        Kedi kedi = new Kedi("Tekir");
        kedi.Davran();
        
        // Aşağıdaki kodlar derleme hatası verir:
        // kedi.Ad = "Pamuk"; // Ad protected olduğu için erişilemez
        // kedi.YemekYe();    // YemekYe protected olduğu için erişilemez
    }
}
```

### Protected Metot Kullanımı

```csharp
public abstract class Form
{
    protected virtual void OnLoad()
    {
        Console.WriteLine("Form loading...");
    }
    
    public void Show()
    {
        // Hazırlık kodları
        OnLoad();
        // Gösterme işlemleri
    }
}

public class LoginForm : Form
{
    protected override void OnLoad()
    {
        base.OnLoad();
        Console.WriteLine("Login form özel hazırlık işlemleri tamamlandı.");
    }
}
```

### Protected Properties

```csharp
public class BankaHesabi
{
    protected decimal Bakiye { get; set; }
    
    public BankaHesabi(decimal baslangicBakiye)
    {
        if (baslangicBakiye < 0)
            throw new ArgumentException("Başlangıç bakiyesi negatif olamaz");
            
        Bakiye = baslangicBakiye;
    }
    
    public virtual void ParaYatir(decimal miktar)
    {
        if (miktar <= 0)
            throw new ArgumentException("Yatırılan miktar pozitif olmalıdır");
            
        Bakiye += miktar;
    }
    
    public virtual bool ParaCek(decimal miktar)
    {
        if (miktar <= 0)
            throw new ArgumentException("Çekilen miktar pozitif olmalıdır");
            
        if (Bakiye >= miktar)
        {
            Bakiye -= miktar;
            return true;
        }
        
        return false;
    }
    
    public decimal BakiyeGoster()
    {
        return Bakiye;
    }
}

public class VadesizHesap : BankaHesabi
{
    private decimal _islemUcreti;
    
    public VadesizHesap(decimal baslangicBakiye, decimal islemUcreti)
        : base(baslangicBakiye)
    {
        _islemUcreti = islemUcreti;
    }
    
    public override bool ParaCek(decimal miktar)
    {
        decimal toplamTutar = miktar + _islemUcreti;
        
        // Bakiye protected olduğu için erişebiliyoruz
        if (Bakiye >= toplamTutar)
        {
            Bakiye -= toplamTutar;
            return true;
        }
        
        return false;
    }
}
```

## Sık Karşılaşılan Hatalar

1. **Aşırı Kullanım**: Çok fazla üyeyi `protected` yapmak, sınıf içi yapıya fazla bağımlılık yaratır.

2. **Yetersiz Kullanım**: Türetilen sınıfların ihtiyaç duyduğu üyeleri `private` yapmak, kod tekrarına neden olabilir.

3. **Güvenlik Yanılgısı**: `protected` üyelerin sınıfın dışından erişilememesi, tüm güvenlik ihtiyaçlarını karşılamaz.

4. **Belgeleme Eksikliği**: `protected` üyelerin nasıl ve ne zaman kullanılacağını açıkça belgelememek, yanlış kullanıma yol açabilir.

5. **Protected Üyelerde Değişiklik**: Temel sınıfın `protected` üyelerinde yapılan değişiklikler, türetilen tüm sınıfları etkileyebilir.

## Özet

`protected` erişim belirleyicisi, C#'ta nesne yönelimli programlamanın önemli bir bileşenidir. Bu belirleyici, sınıf hiyerarşilerinde veri gizleme ve işlevselliği alt sınıflara genişletme arasında denge kurmayı sağlar.

Temel özellikleri:

- Sınıf içinden erişim sağlar
- Türetilen sınıflardan erişim sağlar
- Dış dünyadan erişimi engeller
- Nesne yönelimli tasarımda kalıtım için önemli bir araçtır

Protected erişim belirleyicisini etkin bir şekilde kullanmak, temiz, bakımı kolay ve güvenli kod oluşturmanıza yardımcı olur ve sınıf hiyerarşilerinizde uygun soyutlama seviyelerini korumanıza olanak tanır.
