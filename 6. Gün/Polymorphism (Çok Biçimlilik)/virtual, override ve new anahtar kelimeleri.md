# C# Virtual, Override ve New Anahtar Kelimeleri Rehberi

## İçindekiler
1. [Giriş](#giriş)
2. [Virtual Anahtar Kelimesi](#virtual-anahtar-kelimesi)
3. [Override Anahtar Kelimesi](#override-anahtar-kelimesi)
4. [New Anahtar Kelimesi](#new-anahtar-kelimesi)
5. [Virtual, Override ve New Karşılaştırması](#virtual-override-ve-new-karşılaştırması)
6. [Pratik Örnekler](#pratik-örnekler)
7. [En İyi Kullanım Pratikleri](#en-iyi-kullanım-pratikleri)
8. [Yaygın Hatalar ve Çözümleri](#yaygın-hatalar-ve-çözümleri)
9. [Özet](#özet)

## Giriş

C# nesne yönelimli bir programlama dilidir ve kalıtım (inheritance) bu programlama paradigmasının temel yapıtaşlarından biridir. Kalıtım ile ilgili olarak C#'ta kullanılan üç önemli anahtar kelime vardır: `virtual`, `override` ve `new`. Bu anahtar kelimeler, miras alınan sınıflar arasındaki metot davranışlarını kontrol etmekte ve çok biçimlilik (polymorphism) konseptini uygulamakta kritik bir rol oynar.

Bu dokümanda, bu üç anahtar kelimenin detaylı açıklamasını, kullanım senaryolarını ve birbirleri arasındaki ilişkileri inceleyeceğiz.

## Virtual Anahtar Kelimesi

### Tanım
`virtual` anahtar kelimesi, bir temel sınıftaki metot, özellik, indeksleyici veya event'in, türetilmiş sınıflar tarafından override edilebileceğini (geçersiz kılınabileceğini) belirtir.

### Özellikleri
- Bir metot virtual olarak işaretlendiğinde, türetilmiş sınıflar bu metodu override edebilir.
- Virtual metotlar, temel sınıfta bir uygulama (implementation) içermelidir.
- Sadece instance metotları, özellikleri, indeksleyicileri ve olayları virtual olarak işaretlenebilir. Statik üyeler veya yapıcı metotlar (constructor) virtual olamaz.
- Virtual metotlar abstract olamaz ve tersine abstract metotlar da her zaman sanal (virtual) kabul edilir.

### Sözdizimi (Syntax)
```csharp
public class TemelSinif
{
    public virtual void MetotAdi()
    {
        // Metot içeriği
    }
    
    public virtual int OzellikAdi { get; set; }
}
```

### Örnek
```csharp
public class Hayvan
{
    public virtual void SesCikar()
    {
        Console.WriteLine("Bilinmeyen hayvan sesi");
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

## Override Anahtar Kelimesi

### Tanım
`override` anahtar kelimesi, türetilmiş sınıfta, temel sınıftan miras alınan virtual (veya abstract) bir üyeyi yeniden tanımlamak için kullanılır.

### Özellikleri
- Override edilecek üye, temel sınıfta `virtual`, `abstract` veya başka bir sınıfta `override` olarak işaretlenmiş olmalıdır.
- Override eden ve edilen üyelerin aynı erişim belirleyicisine (access modifier) sahip olması gerekir.
- Override eden ve edilen üyelerin aynı döndürme tipine (return type) ve parametre listesine sahip olması gerekir.
- Override edilen üye `sealed` anahtar kelimesi ile mühürlenebilir, bu da daha fazla override edilmesini engeller.

### Sözdizimi (Syntax)
```csharp
public class TuretilmisSinif : TemelSinif
{
    public override void MetotAdi()
    {
        // Yeni metot içeriği
    }
    
    public override int OzellikAdi { get; set; }
}
```

### Örnek
```csharp
public class Hayvan
{
    public virtual void SesCikar()
    {
        Console.WriteLine("Bilinmeyen hayvan sesi");
    }
}

public class Kopek : Hayvan
{
    public override void SesCikar()
    {
        Console.WriteLine("Hav hav!");
    }
}

public class BulldogKopek : Kopek
{
    public override void SesCikar()
    {
        Console.WriteLine("Woof woof!");
    }
}
```

## New Anahtar Kelimesi

### Tanım
`new` anahtar kelimesi, türetilmiş sınıfta temel sınıftaki bir üyeyi gizlemek (hiding) için kullanılır. Bu, override etmekten farklıdır çünkü burada çok biçimlilik (polymorphism) söz konusu değildir.

### Özellikleri
- Temel sınıftaki üyeyi gizler, ancak temel sınıfın işlevselliğini değiştirmez.
- Temel sınıftaki üyenin virtual olması gerekmez.
- new ile işaretlenen üye, türetilmiş sınıfın tipi kullanıldığında çağrılır.
- Temel sınıfın referansı üzerinden çağrı yapıldığında, temel sınıftaki üye çağrılır.

### Sözdizimi (Syntax)
```csharp
public class TuretilmisSinif : TemelSinif
{
    public new void MetotAdi()
    {
        // Yeni metot içeriği
    }
    
    public new int OzellikAdi { get; set; }
}
```

### Örnek
```csharp
public class Hayvan
{
    public void BilgiVer()
    {
        Console.WriteLine("Bu bir hayvandır.");
    }
}

public class Kopek : Hayvan
{
    public new void BilgiVer()
    {
        Console.WriteLine("Bu bir köpektir.");
    }
}

// Kullanım
Kopek kopek = new Kopek();
kopek.BilgiVer(); // "Bu bir köpektir." yazdırır

Hayvan hayvan = kopek; // Upcasting
hayvan.BilgiVer(); // "Bu bir hayvandır." yazdırır (new kullandığımız için)
```

## Virtual, Override ve New Karşılaştırması

### Kullanım Amaçları
- **Virtual**: Temel sınıfta metotların türetilmiş sınıflar tarafından override edilebilmesini sağlar.
- **Override**: Türetilmiş sınıfta, temel sınıftan gelen virtual metotların yeniden tanımlanmasını sağlar.
- **New**: Türetilmiş sınıfta, temel sınıftan gelen bir metodu gizleyerek yeni bir uygulama sunmak için kullanılır.

### Davranış Farkları

Aşağıdaki örnek üzerinden farkları görelim:

```csharp
// Temel sınıf
public class TemelSinif
{
    public virtual void VirtualMetot()
    {
        Console.WriteLine("TemelSinif.VirtualMetot");
    }
    
    public void NormalMetot()
    {
        Console.WriteLine("TemelSinif.NormalMetot");
    }
}

// Türetilmiş sınıf 1
public class TuretilmisSinif1 : TemelSinif
{
    public override void VirtualMetot()
    {
        Console.WriteLine("TuretilmisSinif1.VirtualMetot");
    }
    
    public new void NormalMetot()
    {
        Console.WriteLine("TuretilmisSinif1.NormalMetot");
    }
}

// Türetilmiş sınıf 2
public class TuretilmisSinif2 : TuretilmisSinif1
{
    public override void VirtualMetot()
    {
        Console.WriteLine("TuretilmisSinif2.VirtualMetot");
    }
    
    public new void NormalMetot()
    {
        Console.WriteLine("TuretilmisSinif2.NormalMetot");
    }
}

// Test kodu
TemelSinif t1 = new TuretilmisSinif1();
TemelSinif t2 = new TuretilmisSinif2();

t1.VirtualMetot(); // Çıktı: TuretilmisSinif1.VirtualMetot
t2.VirtualMetot(); // Çıktı: TuretilmisSinif2.VirtualMetot

t1.NormalMetot(); // Çıktı: TemelSinif.NormalMetot
t2.NormalMetot(); // Çıktı: TemelSinif.NormalMetot
```

Bu örnekte:
- `virtual` ve `override` kombinasyonu çok biçimlilik sağlar. Nesne hangi türetilmiş sınıfın örneği olursa olsun, doğru uygulama çağrılır.
- `new` kullanımında ise, kullanılan referansın tipi hangi metot uygulamasının çağrılacağını belirler.

## Pratik Örnekler

### Örnek 1: Virtual-Override Zinciri

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

public class Kare : Dikdortgen
{
    public double Kenar
    {
        get { return Genislik; }
        set { Genislik = value; Yukseklik = value; }
    }
    
    // Override etmemize gerek yok, Dikdortgen'in AlanHesapla() 
    // metodu zaten doğru çalışacak
}
```

### Örnek 2: New Kullanımı

```csharp
public class DataRepository
{
    public void Save()
    {
        Console.WriteLine("Genel kaydetme işlemi");
    }
}

public class SqlDataRepository : DataRepository
{
    public new void Save()
    {
        // Temel sınıfın işlemini de çağırmak istersek:
        // base.Save();
        
        Console.WriteLine("SQL Server'a kaydediliyor");
    }
}

// Kullanım
SqlDataRepository sqlRepo = new SqlDataRepository();
sqlRepo.Save(); // "SQL Server'a kaydediliyor" yazdırır

DataRepository repo = sqlRepo;
repo.Save(); // "Genel kaydetme işlemi" yazdırır
```

### Örnek 3: Sealed Override

```csharp
public class Hayvan
{
    public virtual void SesCikar()
    {
        Console.WriteLine("Bilinmeyen ses");
    }
}

public class Kopek : Hayvan
{
    public sealed override void SesCikar()
    {
        Console.WriteLine("Hav hav!");
    }
}

public class BulldogKopek : Kopek
{
    // Hata! Sealed metot override edilemez
    // public override void SesCikar() { ... }
    
    // Ancak new ile gizlenebilir
    public new void SesCikar()
    {
        Console.WriteLine("Woof woof!");
    }
}
```

## En İyi Kullanım Pratikleri

### Virtual ve Override İçin
1. **Dikkatli planlayın**: Bir metodu virtual olarak işaretlemek, gelecekte değişebileceği anlamına gelir. Bunu bilinçli yapın.
2. **Base çağrısı düşünün**: Override ettiğinizde, gerekirse `base.MetotAdi()` kullanarak temel uygulamayı çağırın.
3. **Sealed kullanın**: Daha fazla override edilmesini istemediğiniz metotları `sealed override` olarak işaretleyin.
4. **Tutarlı davranış sağlayın**: Override edilen metotların, temel sınıf metotlarıyla tutarlı davranması gerekir (Liskov Substitution Principle).

### New İçin
1. **Mümkünse kaçının**: `new` kullanımı çoğunlukla kafa karıştırıcı olabilir; ideal bir tasarım aracı değildir.
2. **Belgelenmelidir**: Eğer `new` kullanmanız gerekiyorsa, nedenini belgeleyin.
3. **Temel işlevselliği bozmayın**: `new` kullanırken, tüm sınıf hiyerarşisindeki diğer metotların hala doğru çalıştığından emin olun.
4. **İsim değiştirmeyi düşünün**: Bazen `new` kullanmak yerine, türetilmiş sınıfta farklı bir metot adı kullanmak daha iyi olabilir.

## Yaygın Hatalar ve Çözümleri

### 1. "new" Kullanırken Polimorfik Davranışı Kaybetmek
**Hata:**
```csharp
TemelSinif t = new TuretilmisSinif();
t.MetotAdi(); // TemelSinif'in uygulaması çağrılır, TuretilmisSinif'in değil
```

**Çözüm:**
```csharp
// new yerine virtual/override kullanın
public class TemelSinif
{
    public virtual void MetotAdi() { ... }
}

public class TuretilmisSinif : TemelSinif
{
    public override void MetotAdi() { ... }
}
```

### 2. Base Çağrısını Unutmak
**Hata:**
```csharp
public override void Initialize()
{
    // Özelleştirilmiş başlatma kodu
    // base.Initialize() çağrısı yapılmadı!
}
```

**Çözüm:**
```csharp
public override void Initialize()
{
    base.Initialize(); // Önce temel uygulama çağrılır
    // Sonra özelleştirilmiş başlatma kodu
}
```

### 3. Virtual Üyeleri Statik Olarak Tanımlamaya Çalışmak
**Hata:**
```csharp
public static virtual void BirMetot() { ... } // Derleme hatası!
```

**Çözüm:**
```csharp
// Statik metotlar virtual olamaz
// Ya statik özelliğinden ya da virtual özelliğinden vazgeçin
public virtual void BirMetot() { ... } // virtual bir instance metodu
```

### 4. Erişim Belirleyicileri Uyumsuzluğu
**Hata:**
```csharp
public class TemelSinif
{
    protected virtual void BirMetot() { ... }
}

public class TuretilmisSinif : TemelSinif
{
    public override void BirMetot() { ... } // Derleme hatası!
}
```

**Çözüm:**
```csharp
public class TuretilmisSinif : TemelSinif
{
    protected override void BirMetot() { ... } // Aynı erişim belirleyici
}
```

## Özet

C#'ta nesne yönelimli programlamada:

1. **Virtual** anahtar kelimesi, temel sınıfta bir üyenin türetilmiş sınıflar tarafından override edilebileceğini belirtir.

2. **Override** anahtar kelimesi, türetilmiş sınıfta, temel sınıftan gelen virtual bir üyeyi yeniden tanımlar. Bu, çok biçimlilik (polymorphism) sağlar - temel sınıf referansı ile türetilmiş sınıf nesnesi kullanıldığında, doğru uygulama çağrılır.

3. **New** anahtar kelimesi, türetilmiş sınıfta, temel sınıftaki bir üyeyi gizler. Çok biçimlilik sağlamaz - hangi uygulamanın çağrılacağı kullanılan referansın tipine bağlıdır.

Bu anahtar kelimeler, C#'ta güçlü nesne yönelimli tasarımlar oluşturmanıza yardımcı olur. Virtual ve override, kalıtım hiyerarşisinde esnek ve genişletilebilir davranış sağlarken, new, genellikle başka tasarım seçeneklerinin düşünülmesi gereken durumlarda kullanılır.

Doğru anahtar kelimeyi seçerken, uygulamanızın gereksinimlerini ve sınıf hiyerarşinizin gelecekteki gelişimini göz önünde bulundurun.
