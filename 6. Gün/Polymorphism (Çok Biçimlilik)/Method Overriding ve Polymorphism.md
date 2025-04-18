# C# Method Overriding ve Polymorphism

## İçindekiler
1. [Giriş](#giriş)
2. [Polymorphism (Çok Biçimlilik) Nedir?](#polymorphism-çok-biçimlilik-nedir)
3. [Method Overriding (Metot Ezme)](#method-overriding-metot-ezme)
4. [Method Overriding ile Polymorphism İlişkisi](#method-overriding-ile-polymorphism-ilişkisi)
5. [virtual, override ve new Anahtar Kelimeleri](#virtual-override-ve-new-anahtar-kelimeleri)
6. [Abstract Sınıflar ve Method Overriding](#abstract-sınıflar-ve-method-overriding)
7. [Interface'ler ve Polymorphism](#interfaceler-ve-polymorphism)
8. [Sealed Metotlar ve Sınıflar](#sealed-metotlar-ve-sınıflar)
9. [Base Anahtar Kelimesi](#base-anahtar-kelimesi)
10. [Detaylı Örnekler](#detaylı-örnekler)
11. [İyi Uygulamalar ve Öneriler](#iyi-uygulamalar-ve-öneriler)
12. [Sonuç](#sonuç)

## Giriş

Nesne yönelimli programlamanın (OOP) temel prensiplerinden olan polymorphism ve method overriding, C# programlamanın güçlü özellikleridir. Bu kavramlar, daha modüler, yeniden kullanılabilir ve bakımı kolay kod yazmanıza olanak tanır.

## Polymorphism (Çok Biçimlilik) Nedir?

Polymorphism, kelime anlamı olarak "çok biçimlilik" demektir ve nesne yönelimli programlamada bir nesnenin farklı formlarda davranabilme yeteneğini ifade eder. C#'ta polymorphism iki ana kategoriye ayrılır:

1. **Compile-time (Derleme zamanı) Polymorphism**: Method overloading ile gerçekleştirilir.
2. **Run-time (Çalışma zamanı) Polymorphism**: Method overriding ile gerçekleştirilir.

Bu belge, run-time polymorphism ve method overriding üzerine odaklanacaktır.

## Method Overriding (Metot Ezme)

Method overriding, bir alt sınıfın (derived class) üst sınıftan (base class) miras aldığı bir metodu yeniden tanımlama işlemidir. Bu, alt sınıfın kendi özel uygulamasını sağlamasına olanak tanır.

### Method Overriding İçin Gereksinimler:

1. Miras ilişkisi olmalıdır (inheritance)
2. Üst sınıftaki metot `virtual`, `abstract` veya `override` olarak işaretlenmiş olmalıdır
3. Alt sınıftaki metot `override` anahtar kelimesi ile işaretlenmelidir
4. Her iki metodun da aynı isim, dönüş tipi ve parametrelere sahip olması gerekir

```csharp
// Basit bir method overriding örneği
public class Hayvan
{
    public virtual void SesCikar()
    {
        Console.WriteLine("Hayvan ses çıkarıyor.");
    }
}

public class Kopek : Hayvan
{
    public override void SesCikar()
    {
        Console.WriteLine("Hav hav!");
    }
}
```

## Method Overriding ile Polymorphism İlişkisi

Method overriding, runtime polymorphism'in temel aracıdır. Bir üst sınıf referansı alt sınıf nesnesini işaret ettiğinde, çağrılan metodun hangi versiyonunun çalıştırılacağı çalışma zamanında belirlenir.

```csharp
Hayvan hayvan1 = new Hayvan(); // Hayvan referansı, Hayvan nesnesi
Hayvan hayvan2 = new Kopek();  // Hayvan referansı, Kopek nesnesi

hayvan1.SesCikar(); // "Hayvan ses çıkarıyor." yazdırır
hayvan2.SesCikar(); // "Hav hav!" yazdırır
```

Bu örnekte, `hayvan2` bir `Hayvan` referansı olmasına rağmen, `Kopek` nesnesini işaret ettiği için `SesCikar()` metodu çağrıldığında `Kopek` sınıfındaki override edilmiş versiyon çalıştırılır. Bu, runtime polymorphism'in özüdür.

## virtual, override ve new Anahtar Kelimeleri

### virtual

`virtual` anahtar kelimesi, bir metodun alt sınıflar tarafından override edilebileceğini belirtir.

```csharp
public class Base
{
    public virtual void Display()
    {
        Console.WriteLine("Bu Base sınıfının Display metodu");
    }
}
```

### override

`override` anahtar kelimesi, üst sınıftan miras alınan bir metodun alt sınıfta yeniden tanımlandığını belirtir.

```csharp
public class Derived : Base
{
    public override void Display()
    {
        Console.WriteLine("Bu Derived sınıfının Display metodu");
    }
}
```

### new

`new` anahtar kelimesi, üst sınıftaki metodu override etmek yerine, alt sınıfta aynı isimle tamamen yeni bir metot tanımlandığını belirtir. Bu durumda polymorphism gerçekleşmez.

```csharp
public class AnotherDerived : Base
{
    public new void Display()
    {
        Console.WriteLine("Bu AnotherDerived sınıfının (new ile) Display metodu");
    }
}

// Kullanımı:
Base b1 = new AnotherDerived();
b1.Display(); // "Bu Base sınıfının Display metodu" yazdırır
```

`new` ve `override` arasındaki fark çok önemlidir:

- `override` kullanıldığında, üst sınıf referansı ile çağrıldığında bile alt sınıfın metodu çalıştırılır (polymorphism).
- `new` kullanıldığında, üst sınıf referansı ile çağrıldığında üst sınıfın metodu çalıştırılır (polymorphism gerçekleşmez).

## Abstract Sınıflar ve Method Overriding

Abstract sınıflar, doğrudan örneklenemeyen ve genellikle abstract metotlar içeren sınıflardır. Abstract metotlar ise sadece imzası tanımlanıp, uygulaması alt sınıflara bırakılan metotlardır.

```csharp
public abstract class Sekil
{
    public abstract double AlanHesapla(); // Abstract metot - uygulaması yok
    
    public virtual void Bilgi()
    {
        Console.WriteLine("Bu bir şekildir.");
    }
}

public class Dikdortgen : Sekil
{
    public double Genislik { get; set; }
    public double Yukseklik { get; set; }
    
    public override double AlanHesapla()
    {
        return Genislik * Yukseklik;
    }
    
    public override void Bilgi()
    {
        Console.WriteLine($"Bu bir dikdörtgendir. Alanı: {AlanHesapla()}");
    }
}
```

Abstract sınıflarla ilgili önemli noktalar:

- Abstract sınıflar örneklenemez (`new Sekil()` yapılamaz)
- Abstract metotların uygulaması yoktur, sadece imzası vardır
- Abstract metotlar türetilen sınıflarda `override` edilmelidir
- Abstract metotlar zaten varsayılan olarak virtual'dır, ayrıca `virtual` anahtar kelimesine gerek yoktur
- Abstract sınıflar abstract olmayan metotlar da içerebilir

## Interface'ler ve Polymorphism

Interface'ler abstract sınıflara benzese de farklıdır. Interface'ler sadece metot, özellik, event ve indeksleyici imzalarını içerir, hiçbir uygulama içermez. Bir sınıf birden fazla interface'i implement edebilir.

```csharp
public interface IHareket
{
    void Hareket();
}

public interface IYiyecek
{
    void Yemek();
}

public class Canli
{
    public virtual void Bilgi()
    {
        Console.WriteLine("Bu bir canlıdır.");
    }
}

public class Insan : Canli, IHareket, IYiyecek
{
    public void Hareket()
    {
        Console.WriteLine("İnsan yürüyerek hareket eder.");
    }
    
    public void Yemek()
    {
        Console.WriteLine("İnsan türlü yiyecekler yer.");
    }
    
    public override void Bilgi()
    {
        Console.WriteLine("Bu bir insandır.");
    }
}
```

Interface'ler polymorphism için güçlü bir araçtır:

```csharp
IHareket hareketliNesne = new Insan();
hareketliNesne.Hareket(); // "İnsan yürüyerek hareket eder." yazdırır
```

Interface'ler ile method overriding arasındaki fark:

- Interface metotları explicit olarak `virtual` veya `abstract` olarak işaretlenmez
- Interface'i implement eden sınıflarda `override` anahtar kelimesi kullanılmaz
- Bir sınıf birden fazla interface implement edebilir, ancak sadece bir sınıftan miras alabilir

## Sealed Metotlar ve Sınıflar

`sealed` anahtar kelimesi, bir metodun daha fazla override edilmesini veya bir sınıfın miras alınmasını engellemek için kullanılır.

```csharp
public class Hayvan
{
    public virtual void SesCikar()
    {
        Console.WriteLine("Hayvan ses çıkarıyor.");
    }
}

public class Kopek : Hayvan
{
    public sealed override void SesCikar()
    {
        Console.WriteLine("Hav hav!");
    }
}

public class OzelKopek : Kopek
{
    // Hata! Sealed metot override edilemez
    // public override void SesCikar() 
    // {
    //     Console.WriteLine("Özel hav hav!");
    // }
}
```

Sealed sınıf örneği:

```csharp
public sealed class BitkiHucresi
{
    // Bu sınıftan miras alınamaz
}

// Hata! Sealed sınıftan türetilemez
// public class OzelBitkiHucresi : BitkiHucresi
// {
// }
```

## Base Anahtar Kelimesi

`base` anahtar kelimesi, alt sınıftan üst sınıfın üyelerine erişmek için kullanılır. Özellikle override edilen metotlarda, üst sınıfın uygulamasını da çağırmak istediğinizde kullanışlıdır.

```csharp
public class Hayvan
{
    public virtual void SesCikar()
    {
        Console.WriteLine("Hayvan ses çıkarıyor.");
    }
}

public class Kopek : Hayvan
{
    public override void SesCikar()
    {
        // Önce üst sınıfın metodunu çağır
        base.SesCikar();
        // Sonra özel davranışı ekle
        Console.WriteLine("Hav hav!");
    }
}
```

Bu örnekte, `Kopek` sınıfının `SesCikar` metodu çağrıldığında, önce `Hayvan` sınıfının `SesCikar` metodu çalıştırılır, ardından kendi özel uygulaması çalıştırılır.

## Detaylı Örnekler

### Örnek 1: Banka Hesapları

```csharp
public class Hesap
{
    public string HesapNo { get; set; }
    protected double bakiye;
    
    public Hesap(string hesapNo, double baslangicBakiye)
    {
        HesapNo = hesapNo;
        bakiye = baslangicBakiye;
    }
    
    public virtual void ParaYatir(double miktar)
    {
        if (miktar > 0)
        {
            bakiye += miktar;
            Console.WriteLine($"{miktar} TL yatırıldı. Yeni bakiye: {bakiye} TL");
        }
    }
    
    public virtual bool ParaCek(double miktar)
    {
        if (miktar > 0 && miktar <= bakiye)
        {
            bakiye -= miktar;
            Console.WriteLine($"{miktar} TL çekildi. Yeni bakiye: {bakiye} TL");
            return true;
        }
        Console.WriteLine("Yetersiz bakiye veya geçersiz miktar.");
        return false;
    }
    
    public virtual void BakiyeGoster()
    {
        Console.WriteLine($"Hesap No: {HesapNo}, Bakiye: {bakiye} TL");
    }
}

public class TasarrufHesabi : Hesap
{
    private double faizOrani;
    
    public TasarrufHesabi(string hesapNo, double baslangicBakiye, double faizOrani) 
        : base(hesapNo, baslangicBakiye)
    {
        this.faizOrani = faizOrani;
    }
    
    public void FaizUygula()
    {
        double faiz = bakiye * faizOrani;
        bakiye += faiz;
        Console.WriteLine($"Faiz uygulandı. Kazanılan faiz: {faiz} TL. Yeni bakiye: {bakiye} TL");
    }
    
    public override void BakiyeGoster()
    {
        base.BakiyeGoster();
        Console.WriteLine($"Faiz Oranı: %{faizOrani * 100}");
    }
}

public class VadesizHesap : Hesap
{
    private double islemUcreti;
    
    public VadesizHesap(string hesapNo, double baslangicBakiye, double islemUcreti) 
        : base(hesapNo, baslangicBakiye)
    {
        this.islemUcreti = islemUcreti;
    }
    
    public override bool ParaCek(double miktar)
    {
        double toplamMaliyet = miktar + islemUcreti;
        
        if (toplamMaliyet <= bakiye)
        {
            bakiye -= toplamMaliyet;
            Console.WriteLine($"{miktar} TL çekildi, {islemUcreti} TL işlem ücreti alındı. Yeni bakiye: {bakiye} TL");
            return true;
        }
        Console.WriteLine("Yetersiz bakiye veya geçersiz miktar.");
        return false;
    }
    
    public override void BakiyeGoster()
    {
        base.BakiyeGoster();
        Console.WriteLine($"İşlem Ücreti: {islemUcreti} TL");
    }
}
```

Bu örnekte polymorphism'i görelim:

```csharp
Hesap[] hesaplar = new Hesap[3];
hesaplar[0] = new Hesap("12345", 1000);
hesaplar[1] = new TasarrufHesabi("67890", 2000, 0.05);
hesaplar[2] = new VadesizHesap("54321", 1500, 5);

foreach (var hesap in hesaplar)
{
    hesap.BakiyeGoster(); // Polymorphism - her sınıfın kendi BakiyeGoster metodu çağrılır
    Console.WriteLine("------------------------");
}

hesaplar[0].ParaCek(500); // Normal hesap para çekme
hesaplar[2].ParaCek(500); // VadesizHesap para çekme (işlem ücreti alınır)
```

### Örnek 2: Şekil Hiyerarşisi

```csharp
public abstract class Sekil
{
    public abstract double AlanHesapla();
    public abstract double CevreHesapla();
    
    public virtual void BilgiYazdir()
    {
        Console.WriteLine($"Alan: {AlanHesapla()}, Çevre: {CevreHesapla()}");
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
    
    public override void BilgiYazdir()
    {
        Console.WriteLine($"Daire - Yarıçap: {YariCap}");
        base.BilgiYazdir();
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
    
    public override void BilgiYazdir()
    {
        Console.WriteLine($"Dikdörtgen - Genişlik: {Genislik}, Yükseklik: {Yukseklik}");
        base.BilgiYazdir();
    }
}

public class Ucgen : Sekil
{
    public double Kenar1 { get; set; }
    public double Kenar2 { get; set; }
    public double Kenar3 { get; set; }
    
    public Ucgen(double kenar1, double kenar2, double kenar3)
    {
        Kenar1 = kenar1;
        Kenar2 = kenar2;
        Kenar3 = kenar3;
    }
    
    public override double AlanHesapla()
    {
        // Heron formülü
        double s = (Kenar1 + Kenar2 + Kenar3) / 2;
        return Math.Sqrt(s * (s - Kenar1) * (s - Kenar2) * (s - Kenar3));
    }
    
    public override double CevreHesapla()
    {
        return Kenar1 + Kenar2 + Kenar3;
    }
    
    public override void BilgiYazdir()
    {
        Console.WriteLine($"Üçgen - Kenarlar: {Kenar1}, {Kenar2}, {Kenar3}");
        base.BilgiYazdir();
    }
}
```

Kullanımı:

```csharp
// Şekil listesi oluştur
List<Sekil> sekiller = new List<Sekil>
{
    new Daire(5),
    new Dikdortgen(4, 6),
    new Ucgen(3, 4, 5)
};

// Polymorphism ile tüm şekillerin bilgilerini yazdır
foreach (var sekil in sekiller)
{
    sekil.BilgiYazdir();
    Console.WriteLine("------------------------");
}
```

## İyi Uygulamalar ve Öneriler

1. **Açık ve Anlaşılır Hiyerarşi Oluşturun**: Sınıf hiyerarşiniz mantıklı ve anlaşılır olmalıdır. "IS-A" ilişkisi sağlanmalıdır (bir köpek bir hayvandır).

2. **Liskov Substitution Prensibini Uygulayın**: Alt sınıflar, üst sınıfın yerine kullanılabilmelidir. Bir metodu override ederken, üst sınıfın metoduna göre beklenmedik davranışlar göstermemelidir.

3. **Interface Segregation Prensibini Uygulayın**: Büyük, genel interface'ler yerine daha küçük ve özel interface'ler tercih edin.

4. **Base Metot Çağrılarını Dikkatlice Kullanın**: `base` anahtar kelimesini kullanırken, üst sınıfın metodunun ne yaptığını iyi anlayın. Bazen üst sınıfın metodunu çağırmak mantıklı olmayabilir.

5. **Sınıf Hiyerarşisini Çok Derin Yapmayın**: Çok katmanlı miras hiyerarşisi, kodun anlaşılmasını ve bakımını zorlaştırır.

6. **Sealed Kullanımını Sınırlı Tutun**: `sealed` anahtar kelimesini sadece güvenlik kaygıları veya optimize edilmiş performans gereken durumlarda kullanın.

7. **Anlaşılır Metot İsimleri Kullanın**: Override edilen metodlar için, üst sınıftaki ile aynı isim zorunlu olsa da, metodun ismi ne yaptığını açıkça anlatmalıdır.

8. **Dokümantasyon Ekleyin**: Özellikle abstract sınıflar ve interface'ler için, her bir metodun ne yaptığını ve nasıl kullanılması gerektiğini belirten dokümantasyon ekleyin.

## Sonuç

Method overriding ve polymorphism, C#'ın nesne yönelimli programlama yapısının güçlü özellikleridir. Bu kavramlar sayesinde:

- Kod tekrarını azaltabilirsiniz
- Kodunuzu daha modüler ve genişletilebilir hale getirebilirsiniz
- Farklı türdeki nesneleri tutarlı bir şekilde işleyebilirsiniz
- Açık/Kapalı prensibini (Open/Closed Principle) uygulayabilirsiniz - mevcut kodu değiştirmeden yeni işlevsellik ekleyebilirsiniz

Bu kavramları iyi anlamak ve uygulamak, daha temiz, daha bakımı kolay ve daha esnek kod yazmanıza yardımcı olacaktır.
