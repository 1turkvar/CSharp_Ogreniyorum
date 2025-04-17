# C# 'base' Anahtar Kelimesi Rehberi

## İçindekiler
1. [Giriş](#giriş)
2. [Base Anahtar Kelimesinin Temel Kullanımı](#base-anahtar-kelimesinin-temel-kullanımı)
3. [Temel Sınıf Yapıcısına Erişim](#temel-sınıf-yapıcısına-erişim)
4. [Gizlenmiş Metotlara Erişim](#gizlenmiş-metotlara-erişim)
5. [Gizlenmiş Özelliklere Erişim](#gizlenmiş-özelliklere-erişim)
6. [Virtual Metotların Genişletilmesi](#virtual-metotların-genişletilmesi)
7. [Sınırlılıklar ve İyi Uygulamalar](#sınırlılıklar-ve-i̇yi-uygulamalar)
8. [Gerçek Dünya Örnekleri](#gerçek-dünya-örnekleri)
9. [Özet](#özet)

## Giriş

Nesne yönelimli programlamanın temel konseptlerinden biri olan kalıtım (inheritance), C# dilinde sıkça kullanılan güçlü bir özelliktir. Kalıtımda, bir sınıf başka bir sınıftan türetildiğinde, üst sınıfın (base class/parent class) özelliklerini ve davranışlarını miras alır. C# dilindeki `base` anahtar kelimesi, bu kalıtım ilişkisinde alt sınıftan (derived class/child class) üst sınıfa erişim sağlamak için kullanılır.

`base` anahtar kelimesi, türetilen sınıfın içinden temel sınıftaki üyelere (metotlar, özellikler, vb.) erişmeyi sağlar. Bu, özellikle alt sınıfta aynı isimli üyeler (metotlar, özellikler) bulunduğunda veya temel sınıf yapıcısına erişilmek istendiğinde kritik bir rol oynar.

## Base Anahtar Kelimesinin Temel Kullanımı

`base` anahtar kelimesi temelde iki ana amaç için kullanılır:

1. Temel sınıfın yapıcı metodunu çağırmak
2. Temel sınıftaki gizlenmiş (hidden) veya ezilmiş (overridden) üyelere erişmek

Kullanım sözdizimi şu şekildedir:

```csharp
// Temel sınıfın yapıcısını çağırmak için
base(parametre1, parametre2, ...);

// Temel sınıftaki metoda erişmek için
base.MetotAdı(parametre1, parametre2, ...);

// Temel sınıftaki özelliğe erişmek için
base.ÖzellikAdı;
```

## Temel Sınıf Yapıcısına Erişim

Alt sınıf oluşturulduğunda, üst sınıfın yapıcısı otomatik olarak çağrılır. Ancak bazen üst sınıfın belirli bir parametreli yapıcısını çağırmak gerekebilir. İşte bu durumda `base` anahtar kelimesi kullanılır:

```csharp
public class TemelSınıf
{
    public TemelSınıf()
    {
        Console.WriteLine("TemelSınıf varsayılan yapıcısı çağrıldı");
    }
    
    public TemelSınıf(string mesaj)
    {
        Console.WriteLine($"TemelSınıf parametre alan yapıcısı çağrıldı: {mesaj}");
    }
}

public class AltSınıf : TemelSınıf
{
    // Temel sınıfın parametresiz yapıcısını çağırır (varsayılan)
    public AltSınıf() : base()
    {
        Console.WriteLine("AltSınıf varsayılan yapıcısı çağrıldı");
    }
    
    // Temel sınıfın parametreli yapıcısını çağırır
    public AltSınıf(string mesaj) : base(mesaj)
    {
        Console.WriteLine("AltSınıf parametre alan yapıcısı çağrıldı");
    }
}
```

Yukarıdaki örnekte, `AltSınıf` yapıcıları `base` anahtar kelimesini kullanarak `TemelSınıf` yapıcılarını açıkça çağırır. Eğer `: base()` belirtilmezse, derleyici otomatik olarak temel sınıfın parametresiz yapıcısını çağırır.

## Gizlenmiş Metotlara Erişim

Alt sınıfta temel sınıftakiyle aynı isme sahip bir metot tanımlandığında, temel sınıftaki metot "gizlenir" (hiding). Bu durumda `base` anahtar kelimesi, temel sınıftaki gizlenmiş metoda erişim sağlar:

```csharp
public class TemelSınıf
{
    public void Mesaj()
    {
        Console.WriteLine("TemelSınıf'tan mesaj");
    }
}

public class AltSınıf : TemelSınıf
{
    // Bu metot, temel sınıftaki Mesaj() metodunu gizler
    public new void Mesaj()
    {
        Console.WriteLine("AltSınıf'tan mesaj");
        
        // Temel sınıftaki gizlenmiş metoda erişim
        base.Mesaj();
    }
}
```

Bu örnekte `AltSınıf.Mesaj()` metodu çağrıldığında, önce "AltSınıf'tan mesaj" yazdırılır, ardından `base.Mesaj()` ile temel sınıftaki metot çağrılarak "TemelSınıf'tan mesaj" yazdırılır.

## Gizlenmiş Özelliklere Erişim

Metotlara benzer şekilde, alt sınıfta temel sınıftakiyle aynı isme sahip bir özellik tanımlandığında da `base` anahtar kelimesi temel sınıftaki özelliğe erişim sağlar:

```csharp
public class TemelSınıf
{
    public string Ad { get; set; } = "Temel Ad";
    
    public virtual void BilgiGöster()
    {
        Console.WriteLine($"TemelSınıf Ad: {Ad}");
    }
}

public class AltSınıf : TemelSınıf
{
    // Bu özellik, temel sınıftaki Ad özelliğini gizler
    public new string Ad { get; set; } = "Alt Ad";
    
    public override void BilgiGöster()
    {
        Console.WriteLine($"AltSınıf Ad: {Ad}");
        Console.WriteLine($"TemelSınıf Ad: {base.Ad}");
    }
}
```

Bu örnekte, `AltSınıf` içinde `Ad` özelliği tekrar tanımlanmıştır. `BilgiGöster()` metodu içinde `base.Ad` kullanarak temel sınıftaki özelliğe erişilir.

## Virtual Metotların Genişletilmesi

C#'ta `virtual` olarak işaretlenmiş metotlar, alt sınıflarda `override` anahtar kelimesi ile ezilebilir (override). Bu durumda `base` anahtar kelimesi, temel sınıftaki uygulamayı çağırmak için kullanılabilir:

```csharp
public class TemelSınıf
{
    public virtual void İşlemYap()
    {
        Console.WriteLine("TemelSınıf temel işlemi yapıyor");
    }
}

public class AltSınıf : TemelSınıf
{
    public override void İşlemYap()
    {
        // Önce temel sınıftaki uygulamayı çağır
        base.İşlemYap();
        
        // Ek işlemler yap
        Console.WriteLine("AltSınıf ek işlemler yapıyor");
    }
}
```

Bu örnekte, `AltSınıf.İşlemYap()` çağrıldığında, önce `base.İşlemYap()` ile temel sınıftaki uygulama çalıştırılır, ardından alt sınıfın kendi ek işlemleri gerçekleştirilir. Bu, metotların davranışlarını genişletmek (extend) için yaygın bir kalıptır.

## Sınırlılıklar ve İyi Uygulamalar

`base` anahtar kelimesinin kullanımında dikkat edilmesi gereken bazı noktalar ve iyi uygulamalar:

1. **Sadece bir seviye yukarı erişim**: `base` anahtar kelimesi sadece doğrudan üst sınıfa erişim sağlar, "büyük ebeveyn" sınıflara doğrudan erişim sağlamaz.

2. **Static üyelerde kullanılamaz**: `base` anahtar kelimesi, statik metotlar içinde kullanılamaz çünkü kalıtım nesne seviyesinde gerçekleşir, sınıf seviyesinde değil.

3. **Yapıcı zincirleme**: Yapıcılarda `base` kullanımı, ilk ifade olmalıdır ve `:` operatörüyle birlikte kullanılır.

4. **Aşırı kullanımdan kaçının**: `base` anahtar kelimesinin aşırı kullanımı, sınıflar arasında yüksek bağımlılığa neden olabilir ve kodun bakımını zorlaştırabilir.

5. **Interface uygulamalarında kullanılamaz**: `base` anahtar kelimesi, interface uygulamalarına erişmek için kullanılamaz, sadece sınıf kalıtımında geçerlidir.

## Gerçek Dünya Örnekleri

### Örnek 1: Form Uygulaması

Windows Forms uygulamalarında, özel form sınıfları genellikle `Form` sınıfından türetilir ve bazı olaylar (events) ezilir:

```csharp
public class ÖzelForm : Form
{
    public ÖzelForm()
    {
        Text = "Özel Form";
        Width = 400;
        Height = 300;
    }
    
    protected override void OnLoad(EventArgs e)
    {
        // Form.OnLoad metodunu çağır
        base.OnLoad(e);
        
        // Ek işlemler
        Console.WriteLine("Form yüklendi");
    }
    
    protected override void OnClosing(FormClosingEventArgs e)
    {
        // Kullanıcıya forma kapatmak istediğinden emin olup olmadığını sor
        DialogResult result = MessageBox.Show("Formu kapatmak istediğinize emin misiniz?", 
                                           "Onay", 
                                           MessageBoxButtons.YesNo);
        
        if (result == DialogResult.No)
        {
            e.Cancel = true;
            return;
        }
        
        // Form.OnClosing metodunu çağır
        base.OnClosing(e);
    }
}
```

### Örnek 2: Exception Sınıfları

Özel istisna (exception) sınıfları oluştururken, genellikle `Exception` sınıfından veya onun alt sınıflarından türetilir:

```csharp
public class Özelİstisna : Exception
{
    public Özelİstisna() : base()
    {
    }
    
    public Özelİstisna(string mesaj) : base(mesaj)
    {
    }
    
    public Özelİstisna(string mesaj, Exception innerException) 
        : base(mesaj, innerException)
    {
    }
    
    // Serileştirme için özel yapıcı
    protected Özelİstisna(System.Runtime.Serialization.SerializationInfo info,
                        System.Runtime.Serialization.StreamingContext context) 
        : base(info, context)
    {
    }
}
```

### Örnek 3: Abstract Sınıf Uygulaması

Soyut (abstract) sınıflar, kalıtım için bir çerçeve sağlar ve alt sınıfların belirli metotları uygulamasını zorunlu kılar:

```csharp
public abstract class Şekil
{
    public abstract double AlanHesapla();
    
    public virtual void BilgiGöster()
    {
        Console.WriteLine($"Bu bir şekildir. Alanı: {AlanHesapla()}");
    }
}

public class Dikdörtgen : Şekil
{
    public double Genişlik { get; set; }
    public double Yükseklik { get; set; }
    
    public Dikdörtgen(double genişlik, double yükseklik)
    {
        Genişlik = genişlik;
        Yükseklik = yükseklik;
    }
    
    public override double AlanHesapla()
    {
        return Genişlik * Yükseklik;
    }
    
    public override void BilgiGöster()
    {
        // Temel sınıftaki metodu çağır
        base.BilgiGöster();
        
        // Ek bilgiler göster
        Console.WriteLine($"Bu bir dikdörtgendir. Genişlik: {Genişlik}, Yükseklik: {Yükseklik}");
    }
}
```

## Özet

`base` anahtar kelimesi, C# dilinde kalıtım mekanizmasının önemli bir parçasıdır ve alt sınıfların temel sınıf üyelerine erişmesini sağlar. Başlıca kullanım alanları şunlardır:

1. Temel sınıfın yapıcılarına erişim
2. Gizlenmiş (hidden) metot ve özelliklere erişim
3. Ezilmiş (overridden) sanal metotların temel uygulamasına erişim

`base` anahtar kelimesinin doğru kullanımı, kalıtımın gücünden tam olarak yararlanmaya ve kodun düzgün şekilde genişletilebilir olmasına yardımcı olur. Ancak, aşırı kullanımdan kaçınmak ve sınıflar arasında yüksek bağımlılık oluşturmamak önemlidir.

Nesne yönelimli programlamada, kodun yeniden kullanılabilirliğini ve esnekliğini artırmak için `base` anahtar kelimesi ve kalıtım mekanizması etkili bir şekilde kullanılmalıdır.
