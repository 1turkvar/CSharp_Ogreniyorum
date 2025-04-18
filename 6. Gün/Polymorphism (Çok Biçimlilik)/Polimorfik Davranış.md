# C# Polimorfik Davranış Rehberi

## İçindekiler
1. [Giriş](#giriş)
2. [Polimorfizm Nedir?](#polimorfizm-nedir)
3. [C#'da Polimorfizmin Türleri](#cda-polimorfizmin-türleri)
   - [Derleme Zamanı Polimorfizmi](#derleme-zamanı-polimorfizmi)
   - [Çalışma Zamanı Polimorfizmi](#çalışma-zamanı-polimorfizmi)
4. [Sanal Metotlar ve Override](#sanal-metotlar-ve-override)
5. [Soyut Sınıflar ve Metotlar](#soyut-sınıflar-ve-metotlar)
6. [Arayüzler ile Polimorfizm](#arayüzler-ile-polimorfizm)
7. [Örnek Senaryolar](#örnek-senaryolar)
8. [En İyi Uygulamalar](#en-iyi-uygulamalar)
9. [Sonuç](#sonuç)

## Giriş

Polimorfizm, nesne yönelimli programlamanın temel ilkelerinden biridir ve "çok biçimlilik" anlamına gelir. Bu kavram, C# gibi nesne yönelimli dillerde çok güçlü ve yaygın olarak kullanılır. Bu rehberde, C#'da polimorfik davranışı detaylı bir şekilde inceleyeceğiz.

## Polimorfizm Nedir?

Polimorfizm, aynı ad altında farklı davranışların veya işlevlerin gerçekleştirilebilmesidir. Daha teknik bir tanımla, bir referansın veya koleksiyonun farklı türdeki nesnelere işaret edebilmesi ve bu nesnelerin kendi uygulamalarına göre davranabilmesidir.

Polimorfizm sayesinde:
- Kodun yeniden kullanılabilirliği artar
- Genişletilebilirlik kolaylaşır
- Farklı nesne türleri tek bir arayüz üzerinden yönetilebilir
- Kod daha esnek ve modüler olur

## C#'da Polimorfizmin Türleri

C#'da polimorfizm temelde iki ana kategoriye ayrılır:

### Derleme Zamanı Polimorfizmi

Derleme zamanı polimorfizmi, derleyicinin hangi metodun çağrılacağına derleme zamanında karar vermesidir. Bu tür polimorfizm iki şekilde gerçekleşir:

1. **Metot Aşırı Yükleme (Method Overloading)**: Aynı isimde fakat farklı parametre listelerine sahip metotlar tanımlamaktır.

```csharp
public class Hesaplayici
{
    public int Topla(int a, int b)
    {
        return a + b;
    }

    public double Topla(double a, double b)
    {
        return a + b;
    }

    public int Topla(int a, int b, int c)
    {
        return a + b + c;
    }
}
```

2. **Operatör Aşırı Yükleme (Operator Overloading)**: C#'da operatörlerin davranışlarını özelleştirmek için kullanılır.

```csharp
public class Kompleks
{
    public int Gercek { get; set; }
    public int Sanal { get; set; }

    public static Kompleks operator +(Kompleks a, Kompleks b)
    {
        return new Kompleks 
        { 
            Gercek = a.Gercek + b.Gercek, 
            Sanal = a.Sanal + b.Sanal 
        };
    }
}
```

### Çalışma Zamanı Polimorfizmi

Çalışma zamanı polimorfizmi, bir metodun çağrılacağı nesnenin türünün çalışma zamanında belirlenmesidir. Bu tür polimorfizm, kalıtım (inheritance) ile yakından ilişkilidir ve şu şekillerde uygulanabilir:

1. **Metot Geçersiz Kılma (Method Overriding)**: Türetilmiş sınıfta, temel sınıfın sanal veya soyut metotlarını yeniden tanımlamaktır.

2. **Arayüz Uygulaması (Interface Implementation)**: Bir sınıfın belirli bir arayüzü uygulaması ve o arayüzün metotlarını kendi gereksinimlerine göre hayata geçirmesidir.

## Sanal Metotlar ve Override

C#'da `virtual` ve `override` anahtar kelimeleri, çalışma zamanı polimorfizminin temelini oluşturur:

```csharp
public class Sekil
{
    public virtual double AlanHesapla()
    {
        return 0;
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
}

public class Daire : Sekil
{
    public double YariCap { get; set; }

    public override double AlanHesapla()
    {
        return Math.PI * YariCap * YariCap;
    }
}
```

Bu örnekte:
- `Sekil` sınıfında `virtual` anahtar kelimesi ile bir `AlanHesapla` metodu tanımlanmıştır
- `Dikdortgen` ve `Daire` sınıfları, `Sekil` sınıfından türetilmiş ve `override` anahtar kelimesi ile `AlanHesapla` metodunu kendi ihtiyaçlarına göre yeniden tanımlamıştır

Bu yapı şöyle kullanılabilir:

```csharp
Sekil sekil1 = new Dikdortgen { Genislik = 5, Yukseklik = 10 };
Sekil sekil2 = new Daire { YariCap = 7 };

Console.WriteLine(sekil1.AlanHesapla()); // 50
Console.WriteLine(sekil2.AlanHesapla()); // 153.93...
```

Her iki durumda da referans türü `Sekil` olmasına rağmen, çalışma zamanında doğru nesnenin `AlanHesapla` metodu çağrılır. Bu, polimorfik davranışın özüdür.

## Soyut Sınıflar ve Metotlar

Soyut (abstract) sınıflar ve metotlar, polimorfizmin daha güçlü bir şekilde uygulanmasını sağlar:

```csharp
public abstract class Canli
{
    public abstract void Hareket();
    
    public void YemekYe()
    {
        Console.WriteLine("Yemek yiyor...");
    }
}

public class Kus : Canli
{
    public override void Hareket()
    {
        Console.WriteLine("Kuş uçuyor.");
    }
}

public class Balik : Canli
{
    public override void Hareket()
    {
        Console.WriteLine("Balık yüzüyor.");
    }
}
```

Bu örnekte:
- `Canli` sınıfı `abstract` olarak tanımlanmıştır, bu sınıftan doğrudan nesne oluşturulamaz
- `Hareket` metodu `abstract` olarak tanımlanmıştır, bu metodun alt sınıflarda uygulanması zorunludur
- `YemekYe` metodu normal bir metottur ve tüm alt sınıflar tarafından kullanılabilir

## Arayüzler ile Polimorfizm

Arayüzler (interfaces), C#'da polimorfizmin en güçlü kullanım şeklini sağlar:

```csharp
public interface IHareket
{
    void Ilerle();
}

public interface ISes
{
    void SesCikar();
}

public class Kopek : IHareket, ISes
{
    public void Ilerle()
    {
        Console.WriteLine("Köpek koşuyor.");
    }

    public void SesCikar()
    {
        Console.WriteLine("Hav hav!");
    }
}

public class Araba : IHareket
{
    public void Ilerle()
    {
        Console.WriteLine("Araba ilerliyor.");
    }
}
```

Arayüzler sayesinde bir nesne birden fazla davranış kalıbı sergileyebilir:

```csharp
IHareket[] hareketliler = new IHareket[]
{
    new Kopek(),
    new Araba()
};

foreach (var h in hareketliler)
{
    h.Ilerle(); // Her nesne kendi Ilerle metodunu uygular
}

ISes sesCikaran = new Kopek();
sesCikaran.SesCikar(); // "Hav hav!"
```

## Örnek Senaryolar

### Senaryo 1: Şekil Çizme Uygulaması

```csharp
public abstract class Sekil
{
    public string Renk { get; set; }
    public abstract void Ciz();
    public abstract double AlanHesapla();
}

public class Dikdortgen : Sekil
{
    public double Genislik { get; set; }
    public double Yukseklik { get; set; }

    public override void Ciz()
    {
        Console.WriteLine($"{Renk} renkli dikdörtgen çizildi.");
    }

    public override double AlanHesapla()
    {
        return Genislik * Yukseklik;
    }
}

public class Daire : Sekil
{
    public double YariCap { get; set; }

    public override void Ciz()
    {
        Console.WriteLine($"{Renk} renkli daire çizildi.");
    }

    public override double AlanHesapla()
    {
        return Math.PI * YariCap * YariCap;
    }
}
```

Kullanımı:

```csharp
List<Sekil> sekiller = new List<Sekil>
{
    new Dikdortgen { Renk = "Kırmızı", Genislik = 10, Yukseklik = 5 },
    new Daire { Renk = "Mavi", YariCap = 3 }
};

foreach (var sekil in sekiller)
{
    sekil.Ciz();
    Console.WriteLine($"Alan: {sekil.AlanHesapla()}");
}
```

### Senaryo 2: Ödeme Sistemi

```csharp
public interface IOdeme
{
    bool OdemeYap(decimal tutar);
    string OdemeBilgisi();
}

public class KrediKartiOdeme : IOdeme
{
    public string KartNumarasi { get; set; }
    public string SonKullanmaTarihi { get; set; }

    public bool OdemeYap(decimal tutar)
    {
        // Kredi kartı ödeme işlemleri
        Console.WriteLine($"{tutar:C} tutarında kredi kartı ile ödeme yapıldı.");
        return true;
    }

    public string OdemeBilgisi()
    {
        return $"Kredi Kartı: {KartNumarasi.Substring(0, 4)}-XXXX-XXXX-{KartNumarasi.Substring(12, 4)}";
    }
}

public class BankaHavalesiOdeme : IOdeme
{
    public string IBAN { get; set; }
    public string AdSoyad { get; set; }

    public bool OdemeYap(decimal tutar)
    {
        // Banka havalesi işlemleri
        Console.WriteLine($"{tutar:C} tutarında banka havalesi ile ödeme yapıldı.");
        return true;
    }

    public string OdemeBilgisi()
    {
        return $"Havale: {AdSoyad}, IBAN: {IBAN.Substring(0, 4)}-...-{IBAN.Substring(IBAN.Length - 4)}";
    }
}
```

Kullanımı:

```csharp
public class SatisIslem
{
    public decimal Tutar { get; set; }
    public IOdeme OdemeYontemi { get; set; }

    public bool SatisiTamamla()
    {
        Console.WriteLine($"Toplam tutar: {Tutar:C}");
        Console.WriteLine($"Ödeme yöntemi: {OdemeYontemi.OdemeBilgisi()}");
        return OdemeYontemi.OdemeYap(Tutar);
    }
}

// Kullanım
var satis = new SatisIslem
{
    Tutar = 149.99m,
    OdemeYontemi = new KrediKartiOdeme
    {
        KartNumarasi = "1234567890123456",
        SonKullanmaTarihi = "12/25"
    }
};

satis.SatisiTamamla();

// Ödeme yöntemini değiştirme
satis.OdemeYontemi = new BankaHavalesiOdeme
{
    IBAN = "TR123456789012345678901234",
    AdSoyad = "Ahmet Yılmaz"
};

satis.SatisiTamamla();
```

## En İyi Uygulamalar

1. **Liskov Substitution Prensibi (LSP)**: Bir programda, temel sınıf türündeki nesneler türetilmiş sınıf nesneleriyle değiştirildiğinde program davranışı değişmemelidir.

2. **Gerektiğinde Interface, Gerektiğinde Abstract Class**: Sadece metot imzalarını tanımlamak istiyorsanız arayüzleri, bazı ortak uygulamaları paylaşmak istiyorsanız soyut sınıfları tercih edin.

3. **'is' ve 'as' Operatörlerini Bilinçli Kullanın**: Bu operatörler polimorfik davranışı zayıflatabilir. Mümkün olduğunca tip dönüşümlerinden kaçının.

```csharp
// Önerilmeyen kullanım
if (sekil is Daire)
{
    var daire = (Daire)sekil;
    // Daire'ye özel işlemler
}

// Polimorfik yaklaşım
sekil.AlanHesapla(); // Her şekil kendi alanını hesaplar
```

4. **Aşırı Tasarımdan Kaçının**: Gereğinden fazla soyutlama yapmayın, karmaşıklığı artırabilir.

5. **Base Çağrılarına Dikkat Edin**: `override` metotlarda `base` çağrıları kullanırken dikkatli olun, yan etkiler oluşabilir.

```csharp
public override void Ciz()
{
    base.Ciz(); // Temel sınıftaki işlemleri yap
    // Özel işlemler
}
```

## Sonuç

Polimorfizm, C# dilinin en güçlü özelliklerinden biridir ve yazılım tasarımının esnekliğini artırır. Doğru kullanıldığında kodun bakımını, genişletilebilirliğini ve yeniden kullanılabilirliğini büyük ölçüde iyileştirir.

Bu rehberde C#'da polimorfik davranışın farklı yönlerini inceledik:
- Derleme zamanı ve çalışma zamanı polimorfizmi
- Sanal ve geçersiz kılınan metotlar
- Soyut sınıflar ve metotlar
- Arayüzler ile polimorfizm
- Gerçek dünya senaryoları

Polimorfizm, Nesne Yönelimli Programlamanın diğer ilkeleri olan Encapsulation (Kapsülleme) ve Inheritance (Kalıtım) ile birlikte kullanıldığında, C#'da güçlü ve esnek uygulamalar geliştirmenize olanak tanır.
