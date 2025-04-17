# C# Kalıtım Hiyerarşisi Rehberi

## İçindekiler
1. [Kalıtım Nedir?](#kalıtım-nedir)
2. [C# Kalıtım Temelleri](#c-kalıtım-temelleri)
3. [Kalıtım Türleri](#kalıtım-türleri)
4. [Temel Sınıf ve Türetilmiş Sınıf](#temel-sınıf-ve-türetilmiş-sınıf)
5. [Object Sınıfı](#object-sınıfı)
6. [Sızdırma Seviyeleri ve Erişim Belirleyiciler](#sızdırma-seviyeleri-ve-erişim-belirleyiciler)
7. [Virtual ve Override Anahtar Kelimeleri](#virtual-ve-override-anahtar-kelimeleri)
8. [Base Anahtar Kelimesi](#base-anahtar-kelimesi)
9. [Çoklu Kalıtım ve Arayüzler](#çoklu-kalıtım-ve-arayüzler)
10. [Sealed Sınıflar](#sealed-sınıflar)
11. [Abstract Sınıflar ve Metotlar](#abstract-sınıflar-ve-metotlar)
12. [Polimorfizm ve Kalıtım](#polimorfizm-ve-kalıtım)
13. [Örnek Senaryolar](#örnek-senaryolar)
14. [İyi Uygulama Örnekleri](#iyi-uygulama-örnekleri)
15. [Sık Yapılan Hatalar](#sık-yapılan-hatalar)

## Kalıtım Nedir?

Kalıtım, bir sınıfın başka bir sınıfın özelliklerini (alanlar, metotlar vb.) ve davranışlarını miras alabilmesini sağlayan nesne yönelimli programlama kavramıdır. Bu mekanizma, kod tekrarını azaltır ve hiyerarşik bir sınıf yapısı oluşturarak kodun yeniden kullanılabilirliğini artırır.

Kalıtım sayesinde:
- Kod tekrarı azalır
- Kodun bakımı kolaylaşır
- Hiyerarşik bir sınıf yapısı oluşturulabilir
- Ortak davranışlar tek bir yerde tanımlanabilir

## C# Kalıtım Temelleri

C#'da kalıtım, iki nokta (`:`) operatörü kullanılarak gerçekleştirilir. Türetilmiş sınıf (derived class), temel sınıfın (base class) tüm üyelerini miras alır.

```csharp
// Temel sınıf (base class)
public class Hayvan
{
    public string Ad { get; set; }
    public int Yas { get; set; }
    
    public void SesCikar()
    {
        Console.WriteLine("Ses çıkarıyor...");
    }
}

// Türetilmiş sınıf (derived class)
public class Kopek : Hayvan
{
    public string Cins { get; set; }
    
    public void Havla()
    {
        Console.WriteLine("Hav hav!");
    }
}
```

Bu örnekte, `Kopek` sınıfı `Hayvan` sınıfından türetilmiştir ve `Ad`, `Yas` özellikleri ile `SesCikar()` metodunu miras almıştır.

## Kalıtım Türleri

C#'da üç tür kalıtım vardır:

1. **Tek Seviyeli Kalıtım**: Bir sınıf sadece bir temel sınıftan türetilir.
   ```csharp
   class A { }
   class B : A { }
   ```

2. **Çok Seviyeli Kalıtım**: Bir sınıf başka bir sınıftan türetilirken, o sınıf da başka bir sınıftan türetilmiştir.
   ```csharp
   class A { }
   class B : A { }
   class C : B { }
   ```

3. **Hiyerarşik Kalıtım**: Bir temel sınıftan birden fazla sınıf türetilir.
   ```csharp
   class A { }
   class B : A { }
   class C : A { }
   ```

C# **çoklu kalıtımı** (bir sınıfın birden fazla sınıftan türetilmesi) doğrudan desteklemez, ancak arayüzler (interfaces) aracılığıyla benzer bir işlevsellik sağlanabilir.

## Temel Sınıf ve Türetilmiş Sınıf

- **Temel Sınıf (Base Class)**: Özellikleri ve davranışları miras alınan sınıftır.
- **Türetilmiş Sınıf (Derived Class)**: Temel sınıftan özellikleri ve davranışları miras alan sınıftır.

```csharp
// Temel sınıf
public class GeometrikSekil
{
    public double Alan { get; protected set; }
    public double Cevre { get; protected set; }
    
    public virtual void AlanHesapla()
    {
        Console.WriteLine("Alan hesaplanıyor...");
    }
    
    public virtual void CevreHesapla()
    {
        Console.WriteLine("Çevre hesaplanıyor...");
    }
}

// Türetilmiş sınıf
public class Dikdortgen : GeometrikSekil
{
    public double Uzunluk { get; set; }
    public double Genislik { get; set; }
    
    public Dikdortgen(double uzunluk, double genislik)
    {
        Uzunluk = uzunluk;
        Genislik = genislik;
    }
    
    public override void AlanHesapla()
    {
        Alan = Uzunluk * Genislik;
        Console.WriteLine($"Dikdörtgenin alanı: {Alan}");
    }
    
    public override void CevreHesapla()
    {
        Cevre = 2 * (Uzunluk + Genislik);
        Console.WriteLine($"Dikdörtgenin çevresi: {Cevre}");
    }
}
```

## Object Sınıfı

C#'da tüm sınıflar, doğrudan veya dolaylı olarak `System.Object` sınıfından türetilir. Bu, tüm nesnelerin aşağıdaki metodları miras aldığı anlamına gelir:

- `ToString()`: Nesnenin dize temsilini döndürür
- `Equals()`: İki nesnenin eşit olup olmadığını kontrol eder
- `GetHashCode()`: Nesne için bir karma değer döndürür
- `GetType()`: Nesnenin çalışma zamanı türünü döndürür

```csharp
public class Ogrenci
{
    public string Ad { get; set; }
    public string Soyad { get; set; }
    
    // ToString metodunu ezme (override)
    public override string ToString()
    {
        return $"{Ad} {Soyad}";
    }
}
```

## Sızdırma Seviyeleri ve Erişim Belirleyiciler

C#'da temel sınıf üyeleri, erişim belirleyicilerine göre türetilmiş sınıflara sızdırılır:

- **public**: Herkese açık; her yerden erişilebilir
- **protected**: Sadece aynı sınıf içinden ve türetilmiş sınıflardan erişilebilir
- **internal**: Aynı derleme (assembly) içinden erişilebilir
- **protected internal**: Aynı derleme içinden veya türetilmiş sınıflardan erişilebilir
- **private**: Sadece tanımlandığı sınıf içinden erişilebilir
- **private protected**: Aynı derleme içindeki türetilmiş sınıflardan erişilebilir

```csharp
public class Calisan
{
    public string Ad { get; set; }          // Her yerden erişilebilir
    protected string TCKN { get; set; }     // Sadece bu sınıf ve türetilmiş sınıflardan
    private double Maas { get; set; }       // Sadece bu sınıftan
    internal string Departman { get; set; } // Aynı derleme içinden
}

public class YoneticiCalisan : Calisan
{
    public void BilgileriGoster()
    {
        Console.WriteLine($"Ad: {Ad}");           // public, erişilebilir
        Console.WriteLine($"TCKN: {TCKN}");       // protected, erişilebilir
        Console.WriteLine($"Departman: {Departman}"); // internal, erişilebilir
        // Console.WriteLine($"Maaş: {Maas}");    // private, erişilemez!
    }
}
```

## Virtual ve Override Anahtar Kelimeleri

- **virtual**: Temel sınıfta bir metodun türetilmiş sınıflarda yeniden tanımlanabileceğini belirtir.
- **override**: Türetilmiş sınıfta, temel sınıftaki bir virtual metodun yeniden tanımlandığını belirtir.

```csharp
public class Hayvan
{
    public virtual void SesCikar()
    {
        Console.WriteLine("Ses çıkarıyor...");
    }
}

public class Kopek : Hayvan
{
    public override void SesCikar()
    {
        Console.WriteLine("Hav hav!");
    }
}

public class Kedi : Hayvan
{
    public override void SesCikar()
    {
        Console.WriteLine("Miyav!");
    }
}
```

## Base Anahtar Kelimesi

`base` anahtar kelimesi, türetilmiş sınıf içinden temel sınıfın üyelerine erişmek için kullanılır:

```csharp
public class Hayvan
{
    public string Ad { get; set; }
    
    public Hayvan(string ad)
    {
        Ad = ad;
    }
    
    public virtual void BilgiGoster()
    {
        Console.WriteLine($"Ben bir hayvanım. Adım: {Ad}");
    }
}

public class Kopek : Hayvan
{
    public string Cins { get; set; }
    
    public Kopek(string ad, string cins) : base(ad)
    {
        Cins = cins;
    }
    
    public override void BilgiGoster()
    {
        base.BilgiGoster(); // Temel sınıfın metodunu çağırma
        Console.WriteLine($"Ben bir köpeğim. Cinsim: {Cins}");
    }
}
```

## Çoklu Kalıtım ve Arayüzler

C# doğrudan çoklu kalıtımı desteklemez (bir sınıf birden fazla sınıftan türetilemez). Ancak, bir sınıf birden fazla arayüzü (interface) uygulayabilir:

```csharp
public interface IYuzebilir
{
    void Yuz();
}

public interface IUcabilir
{
    void Uc();
}

public class Kus : Hayvan, IUcabilir
{
    public Kus(string ad) : base(ad)
    {
    }
    
    public void Uc()
    {
        Console.WriteLine($"{Ad} uçuyor!");
    }
}

public class OrdeK : Hayvan, IYuzebilir, IUcabilir
{
    public OrdeK(string ad) : base(ad)
    {
    }
    
    public void Yuz()
    {
        Console.WriteLine($"{Ad} yüzüyor!");
    }
    
    public void Uc()
    {
        Console.WriteLine($"{Ad} uçuyor!");
    }
}
```

## Sealed Sınıflar

`sealed` anahtar kelimesi, bir sınıfın başka sınıflar tarafından türetilmesini engeller:

```csharp
public sealed class FinalSinif
{
    // Bu sınıftan başka sınıflar türetilemez
}

// Hata! Sealed sınıftan türetme yapılamaz
// public class TuretilmisSinif : FinalSinif
// {
// }
```

Ayrıca, `sealed` anahtar kelimesi ile bir override metodu da mühürlenebilir:

```csharp
public class Hayvan
{
    public virtual void SesCikar()
    {
        Console.WriteLine("Ses çıkarıyor...");
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
    //    Console.WriteLine("Özel havlama!");
    // }
}
```

## Abstract Sınıflar ve Metotlar

- **abstract sınıf**: Doğrudan örneği oluşturulamayan, sadece türetilmiş sınıflar için temel olarak kullanılan sınıftır.
- **abstract metot**: Temel sınıfta sadece tanımı yapılan, gövdesi türetilmiş sınıflarda override edilerek tamamlanması gereken metottur.

```csharp
public abstract class GeometrikSekil
{
    // Soyut özellikler
    public double Alan { get; protected set; }
    public double Cevre { get; protected set; }
    
    // Soyut metotlar - gövdesiz
    public abstract void AlanHesapla();
    public abstract void CevreHesapla();
    
    // Normal metot
    public void BilgiGoster()
    {
        Console.WriteLine($"Alan: {Alan}, Çevre: {Cevre}");
    }
}

public class Daire : GeometrikSekil
{
    public double YariCap { get; set; }
    
    public Daire(double yariCap)
    {
        YariCap = yariCap;
    }
    
    // Abstract metotların uygulanması zorunludur
    public override void AlanHesapla()
    {
        Alan = Math.PI * YariCap * YariCap;
    }
    
    public override void CevreHesapla()
    {
        Cevre = 2 * Math.PI * YariCap;
    }
}
```

## Polimorfizm ve Kalıtım

Polimorfizm (çok biçimlilik), kalıtım hiyerarşisindeki nesnelerin aynı arayüzü kullanarak farklı davranışlar sergilemesine olanak tanır:

```csharp
public class HayvanOrnegi
{
    public static void Main()
    {
        // Polimorfizm örneği
        Hayvan[] hayvanlar = new Hayvan[3];
        hayvanlar[0] = new Hayvan("Genel Hayvan");
        hayvanlar[1] = new Kopek("Karabaş", "Golden");
        hayvanlar[2] = new Kedi("Pamuk");
        
        // Aynı metot çağrısı, farklı davranışlar
        foreach (var hayvan in hayvanlar)
        {
            hayvan.SesCikar();
        }
    }
}
```

Çıktı:
```
Ses çıkarıyor...
Hav hav!
Miyav!
```

## Örnek Senaryolar

### Bankacılık Sistemi Örneği

```csharp
public abstract class Hesap
{
    public string HesapNo { get; set; }
    public string MusteriAd { get; set; }
    public decimal Bakiye { get; protected set; }
    
    public Hesap(string hesapNo, string musteriAd, decimal baslangicBakiye)
    {
        HesapNo = hesapNo;
        MusteriAd = musteriAd;
        Bakiye = baslangicBakiye;
    }
    
    public virtual void ParaYatir(decimal miktar)
    {
        if (miktar > 0)
        {
            Bakiye += miktar;
            Console.WriteLine($"{miktar:C} yatırıldı. Yeni bakiye: {Bakiye:C}");
        }
    }
    
    public abstract bool ParaCek(decimal miktar);
    
    public void BakiyeGoster()
    {
        Console.WriteLine($"Hesap No: {HesapNo}, Müşteri: {MusteriAd}, Bakiye: {Bakiye:C}");
    }
}

public class VadesizHesap : Hesap
{
    public decimal IslemUcreti { get; set; }
    
    public VadesizHesap(string hesapNo, string musteriAd, decimal baslangicBakiye, decimal islemUcreti)
        : base(hesapNo, musteriAd, baslangicBakiye)
    {
        IslemUcreti = islemUcreti;
    }
    
    public override bool ParaCek(decimal miktar)
    {
        if (miktar <= 0)
        {
            Console.WriteLine("Geçersiz miktar!");
            return false;
        }
        
        decimal toplamMaliyet = miktar + IslemUcreti;
        
        if (Bakiye >= toplamMaliyet)
        {
            Bakiye -= toplamMaliyet;
            Console.WriteLine($"{miktar:C} çekildi (İşlem ücreti: {IslemUcreti:C}). Yeni bakiye: {Bakiye:C}");
            return true;
        }
        else
        {
            Console.WriteLine("Yetersiz bakiye!");
            return false;
        }
    }
}

public class TasarrufHesabi : Hesap
{
    public decimal FaizOrani { get; set; }
    
    public TasarrufHesabi(string hesapNo, string musteriAd, decimal baslangicBakiye, decimal faizOrani)
        : base(hesapNo, musteriAd, baslangicBakiye)
    {
        FaizOrani = faizOrani;
    }
    
    public override bool ParaCek(decimal miktar)
    {
        if (miktar <= 0)
        {
            Console.WriteLine("Geçersiz miktar!");
            return false;
        }
        
        // Minimum bakiye kontrolü
        if (Bakiye - miktar >= 100)
        {
            Bakiye -= miktar;
            Console.WriteLine($"{miktar:C} çekildi. Yeni bakiye: {Bakiye:C}");
            return true;
        }
        else
        {
            Console.WriteLine("İşlem gerçekleştirilemedi. Minimum 100 TL bakiye kalmalıdır.");
            return false;
        }
    }
    
    public void FaizUygula()
    {
        decimal faizMiktari = Bakiye * FaizOrani / 100;
        Bakiye += faizMiktari;
        Console.WriteLine($"Faiz uygulandı. Yeni bakiye: {Bakiye:C}");
    }
}
```

### Kullanım Örneği

```csharp
class Program
{
    static void Main(string[] args)
    {
        // Hesap oluşturma
        VadesizHesap vadesiz = new VadesizHesap("123456", "Ahmet Yılmaz", 1000, 2);
        TasarrufHesabi tasarruf = new TasarrufHesabi("654321", "Ayşe Demir", 5000, 5);
        
        // İşlemler
        vadesiz.BakiyeGoster();
        vadesiz.ParaYatir(500);
        vadesiz.ParaCek(300);
        vadesiz.BakiyeGoster();
        
        Console.WriteLine();
        
        tasarruf.BakiyeGoster();
        tasarruf.ParaYatir(1000);
        tasarruf.ParaCek(500);
        tasarruf.FaizUygula();
        tasarruf.BakiyeGoster();
        
        // Polimorfizm örneği
        Console.WriteLine("\nPolimorfizm örneği:");
        Hesap[] hesaplar = { vadesiz, tasarruf };
        
        foreach (var hesap in hesaplar)
        {
            hesap.BakiyeGoster();
            hesap.ParaCek(100);
        }
    }
}
```

## İyi Uygulama Örnekleri

1. **ISP (Interface Segregation Principle) - Arayüz Ayrımı Prensibi**: Büyük arayüzler yerine daha küçük ve özel arayüzler tasarlayın.

```csharp
// Kötü tasarım
public interface ICalisan
{
    void Calis();
    void MolaVer();
    void RaporHazirla();
    void ToplantiyaKatil();
}

// İyi tasarım
public interface ICalisabilir
{
    void Calis();
}

public interface IMolaVerebilir
{
    void MolaVer();
}

public interface IRaporHazırlayabilir
{
    void RaporHazirla();
}
```

2. **LSP (Liskov Substitution Principle) - Liskov Yerine Geçme Prensibi**: Türetilmiş sınıflar, temel sınıfların yerine geçebilmelidir.

```csharp
// LSP'ye uygun
public class Dikdortgen
{
    public virtual int Genislik { get; set; }
    public virtual int Yukseklik { get; set; }
    
    public int AlanHesapla()
    {
        return Genislik * Yukseklik;
    }
}

public class Kare : Dikdortgen
{
    private int _kenar;
    
    public override int Genislik
    {
        get { return _kenar; }
        set { _kenar = value; }
    }
    
    public override int Yukseklik
    {
        get { return _kenar; }
        set { _kenar = value; }
    }
}
```

3. **Kompozisyon Kullanımı**: Bazen kalıtım yerine kompozisyon (composition) kullanmak daha iyidir.

```csharp
// Kalıtım yerine kompozisyon
public class Motor
{
    public void Calistir()
    {
        Console.WriteLine("Motor çalıştı");
    }
    
    public void Durdur()
    {
        Console.WriteLine("Motor durdu");
    }
}

public class Araba
{
    private Motor _motor = new Motor();
    
    public void Calistir()
    {
        _motor.Calistir();
        Console.WriteLine("Araba çalışıyor");
    }
    
    public void Durdur()
    {
        _motor.Durdur();
        Console.WriteLine("Araba durdu");
    }
}
```

## Sık Yapılan Hatalar

1. **Aşırı Kalıtım Kullanımı**: Hiyerarşinin çok derinleşmesi kodun anlaşılmasını zorlaştırır.

```csharp
// Aşırı derinleşmiş bir hiyerarşi
class A { }
class B : A { }
class C : B { }
class D : C { }
class E : D { }
// Bu durum kodun anlaşılmasını ve bakımını zorlaştırır
```

2. **"is-a" İlişkisi Olmaması**: Kalıtım, sadece "is-a" (... bir ... dir) ilişkisi olduğunda kullanılmalıdır.

```csharp
// Yanlış kullanım (Araba bir Motor değildir)
public class Motor { }
public class Araba : Motor { } // Hatalı kalıtım ilişkisi

// Doğru kullanım (Kompozisyon)
public class Motor { }
public class Araba
{
    private Motor _motor = new Motor();
}
```

3. **Fragile Base Class Problem**: Temel sınıftaki değişiklikler, türetilmiş sınıfları beklenmedik şekilde etkileyebilir.

```csharp
public class TemelSinif
{
    public virtual void MetotA()
    {
        Console.WriteLine("TemelSinif.MetotA");
    }
    
    public void MetotB()
    {
        MetotA(); // MetotA'ya bağımlılık
    }
}

public class TuretilmisSinif : TemelSinif
{
    public override void MetotA()
    {
        // Temel sınıfın beklediğinden farklı davranış
        // Bu, MetotB'nin davranışını da değiştirebilir
        Console.WriteLine("TuretilmisSinif.MetotA");
    }
}
```

4. **Soyut Sınıf vs. Arayüz Seçimi**: Ne zaman abstract sınıf, ne zaman interface kullanılacağını bilmemek.

- **Abstract sınıf** durumları:
  - Ortak bir uygulama paylaşılacaksa
  - İlgili sınıflar arasında ortak alanlar/özellikler varsa
  - Non-public üyeler gerekiyorsa

- **Interface** durumları:
  - Farklı sınıf hiyerarşilerindeki sınıflar benzer davranışlar sergileyecekse
  - Çoklu miras gerekiyorsa
  - Sadece sözleşme tanımlamak istiyorsanız

Bu rehber, C# programlama dilindeki kalıtım hiyerarşisi konusunda kapsamlı bir bakış sunmaktadır. Kalıtım, nesne yönelimli programlamanın güçlü bir özelliği olmasına rağmen, doğru kullanılması gerekir. İyi bir tasarım için, kalıtım ile kompozisyon arasında doğru seçimi yapmak ve SOLID prensiplerini göz önünde bulundurmak önemlidir.
