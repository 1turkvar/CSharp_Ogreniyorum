# C# Abstract (Soyut) Sınıflar Rehberi

## İçindekiler
1. [Abstract Sınıf Nedir?](#abstract-sınıf-nedir)
2. [Abstract Sınıfların Özellikleri](#abstract-sınıfların-özellikleri)
3. [Abstract Sınıf vs Interface](#abstract-sınıf-vs-interface)
4. [Abstract Metotlar](#abstract-metotlar)
5. [Abstract Sınıf Kullanım Örnekleri](#abstract-sınıf-kullanım-örnekleri)
6. [En İyi Uygulama Önerileri](#en-iyi-uygulama-önerileri)
7. [Sık Yapılan Hatalar](#sık-yapılan-hatalar)

## Abstract Sınıf Nedir?

Abstract sınıf, C# programlama dilinde **türetilen sınıflar için temel işlevsellik sağlayan** ancak **doğrudan örneklenemeyen** bir sınıf türüdür. Bir sınıfı abstract olarak işaretlemek için `abstract` anahtar kelimesi kullanılır.

```csharp
public abstract class Sekil
{
    // Abstract sınıf içeriği
}
```

Abstract sınıflar, nesne yönelimli programlamanın temel prensiplerinden biri olan **kalıtım** (inheritance) mekanizmasının önemli bir parçasıdır ve **kod tekrarını azaltmak**, **hiyerarşi oluşturmak** ve **uygulama mantığını standartlaştırmak** için kullanılır.

## Abstract Sınıfların Özellikleri

1. **Örneklenemezler**: Abstract sınıflardan doğrudan nesne oluşturulamaz.

   ```csharp
   // Hatalı kullanım
   Sekil sekil = new Sekil(); // Derleme hatası verir
   ```

2. **Tamamlanmamış ve Tamamlanmış Üyeler**: Abstract sınıflar hem abstract (tamamlanmamış) hem de concrete (tamamlanmış) üyeler içerebilir.

3. **Kalıtım**: Diğer sınıflar için temel sınıf olarak kullanılırlar.

4. **Erişim Belirleyiciler**: Abstract sınıflar ve üyeleri public, protected, internal, protected internal, private veya private protected olabilir.

5. **Yapıcı Metotlar (Constructor)**: Abstract sınıflar constructor içerebilir. Bu constructor, türetilen sınıfların nesneleri oluşturulurken çağrılır.

6. **Statik Üyeler**: Abstract sınıflar statik üyeler içerebilir.

## Abstract Sınıf vs Interface

Abstract sınıflar ve interface'ler arasındaki temel farklar şunlardır:

| Özellik | Abstract Sınıf | Interface |
|---------|---------------|-----------|
| Uygulama | Metotların uygulamasını içerebilir | C# 8.0 öncesinde uygulama içeremez, sonrasında default uygulama içerebilir |
| Erişim Belirleyiciler | Çeşitli erişim belirleyicileri kullanabilir | Tüm üyeler varsayılan olarak public'tir |
| Kalıtım | Bir sınıf sadece bir abstract sınıftan türetilebilir | Bir sınıf birden fazla interface uygulayabilir |
| Alan Tanımlama | Alan (field) tanımlayabilir | Alan tanımlayamaz (C# 8.0 öncesi) |
| Constructor | Constructor içerebilir | Constructor içeremez |
| Statik Üyeler | Statik üyeler içerebilir | Statik üyeler içeremez (C# 8.0 öncesi) |

**Ne zaman Abstract Sınıf kullanmalı?**
- "IS-A" ilişkisi varsa (bir şey başka bir şeyin türüyse)
- Türetilmiş sınıflar arasında paylaşılan işlevsellik varsa
- Temel uygulama kodu paylaşılacaksa
- Üyelerin erişim belirleyicilerinin kontrolü gerekiyorsa

**Ne zaman Interface kullanmalı?**
- "CAN-DO" ilişkisi varsa (bir şey belirli bir davranış sergileyebiliyorsa)
- Birden fazla kalıtım gerekiyorsa
- İlgisiz sınıflar benzer davranışlar sergileyecekse

## Abstract Metotlar

Abstract metotlar, abstract sınıflar içinde tanımlanan ve **gövdesi olmayan** metotlardır. Bu metotların uygulaması, abstract sınıfı miras alan sınıflar tarafından sağlanmalıdır.

```csharp
public abstract class Sekil
{
    // Abstract metot - gövdesi yok
    public abstract double AlanHesapla();
    
    // Normal metot - gövdesi var
    public void Bilgileri()
    {
        Console.WriteLine($"Alan: {AlanHesapla()}");
    }
}
```

Abstract metotlar:
- `abstract` anahtar kelimesi ile işaretlenmelidir
- Sadece abstract sınıflarda tanımlanabilir
- Gövdeleri yoktur ve noktalı virgül (;) ile biterler
- Private olamazlar
- Static, virtual, sealed veya override anahtar kelimeleri ile birlikte kullanılamazlar
- Türetilen sınıfta `override` anahtar kelimesi ile ezilmelidirler

## Abstract Sınıf Kullanım Örnekleri

### Örnek 1: Temel Geometrik Şekil Hiyerarşisi

```csharp
public abstract class Sekil
{
    protected string ad;
    
    // Constructor
    public Sekil(string ad)
    {
        this.ad = ad;
    }
    
    // Abstract metot
    public abstract double AlanHesapla();
    
    // Abstract metot
    public abstract double CevreHesapla();
    
    // Normal metot
    public void BilgileriGoster()
    {
        Console.WriteLine($"{ad} Bilgileri:");
        Console.WriteLine($"Alan: {AlanHesapla()}");
        Console.WriteLine($"Çevre: {CevreHesapla()}");
    }
}

// Türetilmiş sınıf: Dikdörtgen
public class Dikdortgen : Sekil
{
    private double uzunluk;
    private double genislik;
    
    public Dikdortgen(double uzunluk, double genislik) : base("Dikdörtgen")
    {
        this.uzunluk = uzunluk;
        this.genislik = genislik;
    }
    
    // Abstract metotları override etme
    public override double AlanHesapla()
    {
        return uzunluk * genislik;
    }
    
    public override double CevreHesapla()
    {
        return 2 * (uzunluk + genislik);
    }
}

// Türetilmiş sınıf: Daire
public class Daire : Sekil
{
    private double yaricap;
    
    public Daire(double yaricap) : base("Daire")
    {
        this.yaricap = yaricap;
    }
    
    // Abstract metotları override etme
    public override double AlanHesapla()
    {
        return Math.PI * yaricap * yaricap;
    }
    
    public override double CevreHesapla()
    {
        return 2 * Math.PI * yaricap;
    }
}
```

### Örnek 2: Banka Hesap Sistemi

```csharp
public abstract class BankaHesabi
{
    public string HesapNo { get; private set; }
    public string MusteriAdi { get; set; }
    protected decimal bakiye;
    
    public BankaHesabi(string hesapNo, string musteriAdi, decimal baslangicBakiyesi)
    {
        HesapNo = hesapNo;
        MusteriAdi = musteriAdi;
        bakiye = baslangicBakiyesi;
    }
    
    // Normal metot
    public void BakiyeGoster()
    {
        Console.WriteLine($"Hesap No: {HesapNo}, Müşteri: {MusteriAdi}, Bakiye: {bakiye:C}");
    }
    
    // Normal metot
    public virtual void ParaYatir(decimal miktar)
    {
        if (miktar > 0)
        {
            bakiye += miktar;
            Console.WriteLine($"{miktar:C} yatırıldı. Yeni bakiye: {bakiye:C}");
        }
    }
    
    // Abstract metot - Her hesap türü kendi çekim kurallarını belirlemeli
    public abstract void ParaCek(decimal miktar);
    
    // Abstract metot - Her hesap türü kendi faiz hesaplama yöntemini uygulamalı
    public abstract decimal FaizHesapla();
}

public class VadesizHesap : BankaHesabi
{
    public decimal IslemUcreti { get; set; }
    
    public VadesizHesap(string hesapNo, string musteriAdi, decimal baslangicBakiyesi, decimal islemUcreti) 
        : base(hesapNo, musteriAdi, baslangicBakiyesi)
    {
        IslemUcreti = islemUcreti;
    }
    
    public override void ParaCek(decimal miktar)
    {
        if (miktar <= 0)
        {
            Console.WriteLine("Geçersiz miktar.");
            return;
        }
        
        decimal toplamCekim = miktar + IslemUcreti;
        
        if (bakiye >= toplamCekim)
        {
            bakiye -= toplamCekim;
            Console.WriteLine($"{miktar:C} çekildi. {IslemUcreti:C} işlem ücreti alındı. Yeni bakiye: {bakiye:C}");
        }
        else
        {
            Console.WriteLine("Yetersiz bakiye.");
        }
    }
    
    public override decimal FaizHesapla()
    {
        // Vadesiz hesaplarda faiz yok
        return 0;
    }
}

public class VadeliHesap : BankaHesabi
{
    public decimal FaizOrani { get; set; }
    public DateTime VadeTarihi { get; private set; }
    
    public VadeliHesap(string hesapNo, string musteriAdi, decimal baslangicBakiyesi, decimal faizOrani, int vadeGunu) 
        : base(hesapNo, musteriAdi, baslangicBakiyesi)
    {
        FaizOrani = faizOrani;
        VadeTarihi = DateTime.Now.AddDays(vadeGunu);
    }
    
    public override void ParaCek(decimal miktar)
    {
        if (miktar <= 0)
        {
            Console.WriteLine("Geçersiz miktar.");
            return;
        }
        
        if (DateTime.Now < VadeTarihi)
        {
            Console.WriteLine("Vade süresi dolmadan para çekemezsiniz.");
            return;
        }
        
        if (bakiye >= miktar)
        {
            bakiye -= miktar;
            Console.WriteLine($"{miktar:C} çekildi. Yeni bakiye: {bakiye:C}");
        }
        else
        {
            Console.WriteLine("Yetersiz bakiye.");
        }
    }
    
    public override decimal FaizHesapla()
    {
        return bakiye * FaizOrani / 100;
    }
}
```

## En İyi Uygulama Önerileri

1. **Doğru Soyutlama Seviyesi**: Abstract sınıfları, gerçekten ortak işlevselliği ve davranışı olan sınıflar için kullanın.

2. **Interface ile Kombinasyon**: Abstract sınıflar ve interface'leri birlikte kullanarak hem "is-a" hem de "can-do" ilişkilerini modelleyin.

3. **Basit Tutun**: Bir abstract sınıf, sadece türetilen sınıfların paylaşması gereken özellikleri içermelidir.

4. **Belgelendirme**: Abstract sınıfları ve metotları düzgün şekilde belgelendirin, böylece türetilen sınıfların nasıl uygulanması gerektiği açık olur.

5. **İyi İsimlendirme**: Abstract sınıfların isimlerini, sınıfın amacını veya kategorisini yansıtacak şekilde belirleyin.

6. **Çok Seviyeli Hiyerarşiden Kaçının**: Çok derin kalıtım hiyerarşileri karmaşıklığa yol açabilir ve bakımı zorlaştırabilir.

## Sık Yapılan Hatalar

1. **Aşırı Soyutlaştırma**: Her şeyi soyutlaştırmak, gereksiz karmaşıklığa yol açabilir.

2. **Yetersiz Soyutlaştırma**: Kod tekrarına ve uyumluluk sorunlarına neden olabilir.

3. **Interface Yerine Abstract Sınıf Kullanma**: Eğer sadece davranış tanımlanıyorsa ve ortak kod paylaşımı yoksa, interface kullanmak daha uygundur.

4. **Abstract Metotları Private Yapma**: Abstract metotlar private olamaz, çünkü türetilen sınıflar tarafından override edilmeleri gerekir.

5. **Abstract Sınıflardan Nesne Oluşturmaya Çalışma**: Abstract sınıflardan doğrudan nesne oluşturulamaz.

## Sonuç

Abstract sınıflar, C# programlama dilinde nesne yönelimli tasarımın güçlü bir aracıdır. Doğru kullanıldığında, kodun yeniden kullanılabilirliğini artırır, bakımını kolaylaştırır ve daha tutarlı bir nesne modeli oluşturur. Abstract sınıflar ve interface'lerin doğru kombinasyonu, esnek ve genişletilebilir yazılım sistemleri geliştirmenin anahtarıdır.
