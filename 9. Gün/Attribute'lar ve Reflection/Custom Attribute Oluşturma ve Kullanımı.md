# C# Custom Attribute Oluşturma ve Kullanımı

## İçindekiler
1. [Attribute Kavramı](#attribute-kavramı)
2. [Özel Attribute Oluşturma](#özel-attribute-oluşturma)
3. [AttributeUsage Attribute](#attributeusage-attribute)
4. [Attribute Parametreleri](#attribute-parametreleri)
5. [Attribute'ları Okuma (Reflection)](#attributeları-okuma-reflection)
6. [Yaygın Kullanım Senaryoları](#yaygın-kullanım-senaryoları)
7. [Örnek Projeler](#örnek-projeler)
8. [En İyi Uygulamalar](#en-iyi-uygulamalar)

## Attribute Kavramı

Attribute'lar C# programlarına bildirimsel (declarative) bilgiler ekleyen özel yapılardır. Bunlar, kod içerisindeki sınıflara, metotlara, özelliklere ve diğer elemanlara iliştirilmiş meta verilerdir. .NET Framework, birçok yerleşik attribute sunar, ancak kendi özel attribute'larınızı da oluşturabilirsiniz.

### Yerleşik Attribute Örnekleri

```csharp
[Serializable]
public class OrnekSinif { }

[Obsolete("Bu metot artık kullanılmıyor, yerine YeniMetot() kullanın.")]
public void EskiMetot() { }

[DllImport("user32.dll")]
public static extern int MessageBox(IntPtr hWnd, string text, string caption, int type);
```

## Özel Attribute Oluşturma

Özel bir attribute oluşturmak için, `System.Attribute` sınıfından türeyen bir sınıf oluşturmanız gerekir. İsim kuralı olarak, attribute sınıflarının sonuna "Attribute" kelimesini eklemek bir standarttır.

### Temel Attribute Yapısı

```csharp
using System;

[AttributeUsage(AttributeTargets.Class | AttributeTargets.Method, AllowMultiple = true)]
public class YazarAttribute : Attribute
{
    private string isim;
    private string tarih;

    public YazarAttribute(string isim)
    {
        this.isim = isim;
        this.tarih = DateTime.Now.ToShortDateString();
    }

    public string Isim
    {
        get { return isim; }
    }

    public string Tarih
    {
        get { return tarih; }
        set { tarih = value; }
    }
}
```

### Attribute'u Kullanma

```csharp
[Yazar("Ahmet Yılmaz", Tarih = "01.01.2023")]
public class UygulamaSinifi
{
    [Yazar("Mehmet Demir")]
    public void MetotOrnek()
    {
        // Metot içeriği
    }
}
```

## AttributeUsage Attribute

`AttributeUsage`, bir attribute'un nasıl kullanılacağını tanımlamak için kullanılan bir meta-attribute'dur. Aşağıdaki parametreleri alır:

- **ValidOn**: Attribute'un hangi program elemanlarına uygulanabileceğini belirtir.
- **AllowMultiple**: Aynı attribute'un bir elemana birden fazla kez uygulanıp uygulanamayacağını belirtir.
- **Inherited**: Alt sınıfların, üst sınıfa uygulanan attribute'u miras alıp alamayacağını belirtir.

```csharp
[AttributeUsage(
    AttributeTargets.Class | AttributeTargets.Method | AttributeTargets.Property,
    AllowMultiple = true,
    Inherited = false
)]
public class OzelAttribute : Attribute
{
    // Attribute içeriği
}
```

### AttributeTargets Değerleri

| Değer | Açıklama |
|-------|----------|
| All | Tüm bildirim türleri |
| Assembly | Derleme |
| Class | Sınıf |
| Constructor | Yapıcı metot |
| Delegate | Delege |
| Enum | Numaralandırma |
| Event | Olay |
| Field | Alan |
| GenericParameter | Jenerik parametre |
| Interface | Arayüz |
| Method | Metot |
| Module | Modül |
| Parameter | Parametre |
| Property | Özellik |
| ReturnValue | Dönüş değeri |
| Struct | Yapı |

## Attribute Parametreleri

Attribute'lar iki tür parametre alabilir:

1. **Konumsal parametreler (Positional parameters)**: Constructor aracılığıyla geçilir.
2. **İsimli parametreler (Named parameters)**: Public özelliklere atanır.

```csharp
public class KonfigurasyonAttribute : Attribute
{
    // Konumsal parametre
    public KonfigurasyonAttribute(string ad)
    {
        Ad = ad;
    }

    // Konumsal parametre
    public string Ad { get; private set; }

    // İsimli parametre
    public int Oncelik { get; set; }

    // İsimli parametre
    public bool Devre { get; set; } = true;
}

// Kullanım
[Konfigurasyon("SistemAyarları", Oncelik = 1, Devre = false)]
public class Ayarlar
{
    // Sınıf içeriği
}
```

## Attribute'ları Okuma (Reflection)

Attribute'lar, çalışma zamanında (runtime) Reflection kullanılarak okunabilir. Bu, kodunuzu daha dinamik ve yapılandırılabilir hale getirir.

### GetCustomAttributes Metodu ile Attribute Okuma

```csharp
using System;
using System.Reflection;

public static void AttributeBilgileriniGoster(Type tip)
{
    // Sınıf üzerindeki tüm YazarAttribute'ları al
    YazarAttribute[] yazarlar = (YazarAttribute[])tip.GetCustomAttributes(typeof(YazarAttribute), false);
    
    foreach (YazarAttribute yazar in yazarlar)
    {
        Console.WriteLine($"Yazar: {yazar.Isim}, Tarih: {yazar.Tarih}");
    }

    // Metotlar üzerindeki attribute'ları kontrol et
    foreach (MethodInfo metot in tip.GetMethods())
    {
        YazarAttribute[] metotYazarlari = (YazarAttribute[])metot.GetCustomAttributes(typeof(YazarAttribute), false);
        
        foreach (YazarAttribute yazar in metotYazarlari)
        {
            Console.WriteLine($"Metot: {metot.Name}, Yazar: {yazar.Isim}, Tarih: {yazar.Tarih}");
        }
    }
}

// Kullanım
AttributeBilgileriniGoster(typeof(UygulamaSinifi));
```

### IsDefined ile Attribute Varlığını Kontrol Etme

```csharp
bool serializeEdilebilir = Attribute.IsDefined(typeof(UygulamaSinifi), typeof(SerializableAttribute));

if (serializeEdilebilir)
{
    Console.WriteLine("Bu sınıf serileştirilebilir.");
}
```

### GetCustomAttribute ile Tek Attribute Okuma

```csharp
ObsoleteAttribute obsoleteAttribute = (ObsoleteAttribute)Attribute.GetCustomAttribute(
    typeof(UygulamaSinifi).GetMethod("EskiMetot"),
    typeof(ObsoleteAttribute)
);

if (obsoleteAttribute != null)
{
    Console.WriteLine($"Uyarı: {obsoleteAttribute.Message}");
}
```

## Yaygın Kullanım Senaryoları

### 1. Doğrulama (Validation)

```csharp
[AttributeUsage(AttributeTargets.Property, AllowMultiple = false)]
public class ZorunluAttribute : Attribute
{
    public string HataMesaji { get; set; } = "Bu alan zorunludur.";
}

[AttributeUsage(AttributeTargets.Property, AllowMultiple = false)]
public class MaxUzunlukAttribute : Attribute
{
    public int MaxUzunluk { get; }
    public string HataMesaji { get; set; }

    public MaxUzunlukAttribute(int maxUzunluk)
    {
        MaxUzunluk = maxUzunluk;
        HataMesaji = $"Bu alan en fazla {maxUzunluk} karakter olabilir.";
    }
}

public class Kullanici
{
    [Zorunlu]
    public string KullaniciAdi { get; set; }

    [Zorunlu]
    [MaxUzunluk(50, HataMesaji = "E-posta adresi çok uzun!")]
    public string Email { get; set; }
}

// Doğrulama işlemi
public static class Dogrulayici
{
    public static List<string> Dogrula(object nesne)
    {
        List<string> hatalar = new List<string>();
        Type tip = nesne.GetType();

        foreach (PropertyInfo ozellik in tip.GetProperties())
        {
            // Zorunlu kontrolü
            if (Attribute.IsDefined(ozellik, typeof(ZorunluAttribute)))
            {
                object deger = ozellik.GetValue(nesne);
                if (deger == null || (deger is string && string.IsNullOrEmpty((string)deger)))
                {
                    ZorunluAttribute attr = (ZorunluAttribute)Attribute.GetCustomAttribute(ozellik, typeof(ZorunluAttribute));
                    hatalar.Add($"{ozellik.Name}: {attr.HataMesaji}");
                }
            }

            // Maksimum uzunluk kontrolü
            if (Attribute.IsDefined(ozellik, typeof(MaxUzunlukAttribute)))
            {
                object deger = ozellik.GetValue(nesne);
                if (deger is string strDeger)
                {
                    MaxUzunlukAttribute attr = (MaxUzunlukAttribute)Attribute.GetCustomAttribute(ozellik, typeof(MaxUzunlukAttribute));
                    if (strDeger.Length > attr.MaxUzunluk)
                    {
                        hatalar.Add($"{ozellik.Name}: {attr.HataMesaji}");
                    }
                }
            }
        }

        return hatalar;
    }
}
```

### 2. ORM (Object-Relational Mapping)

```csharp
[AttributeUsage(AttributeTargets.Class)]
public class TabloAttribute : Attribute
{
    public string TabloAdi { get; }

    public TabloAttribute(string tabloAdi)
    {
        TabloAdi = tabloAdi;
    }
}

[AttributeUsage(AttributeTargets.Property)]
public class KolonAttribute : Attribute
{
    public string KolonAdi { get; }
    public bool PrimaryKey { get; set; } = false;
    public bool AutoIncrement { get; set; } = false;

    public KolonAttribute(string kolonAdi = null)
    {
        KolonAdi = kolonAdi;
    }
}

[Tablo("Musteriler")]
public class Musteri
{
    [Kolon(PrimaryKey = true, AutoIncrement = true)]
    public int Id { get; set; }
    
    [Kolon("MusteriAdi")]
    public string Ad { get; set; }
    
    [Kolon("MusteriSoyadi")]
    public string Soyad { get; set; }
}

// SQL sorgusu oluşturma örneği
public static string InsertSorgusuOlustur<T>(T nesne)
{
    Type tip = typeof(T);
    TabloAttribute tabloAttr = (TabloAttribute)Attribute.GetCustomAttribute(tip, typeof(TabloAttribute));
    string tabloAdi = tabloAttr?.TabloAdi ?? tip.Name;
    
    List<string> kolonlar = new List<string>();
    List<string> degerler = new List<string>();
    
    foreach (PropertyInfo ozellik in tip.GetProperties())
    {
        if (Attribute.IsDefined(ozellik, typeof(KolonAttribute)))
        {
            KolonAttribute kolonAttr = (KolonAttribute)Attribute.GetCustomAttribute(ozellik, typeof(KolonAttribute));
            
            // AutoIncrement alanları atla
            if (kolonAttr.AutoIncrement)
                continue;
                
            string kolonAdi = kolonAttr.KolonAdi ?? ozellik.Name;
            kolonlar.Add(kolonAdi);
            
            object deger = ozellik.GetValue(nesne);
            if (deger == null)
            {
                degerler.Add("NULL");
            }
            else if (deger is string || deger is DateTime)
            {
                degerler.Add($"'{deger}'");
            }
            else
            {
                degerler.Add(deger.ToString());
            }
        }
    }
    
    return $"INSERT INTO {tabloAdi} ({string.Join(", ", kolonlar)}) VALUES ({string.Join(", ", degerler)})";
}
```

### 3. Serileştirme Kontrolü

```csharp
[AttributeUsage(AttributeTargets.Property, AllowMultiple = false)]
public class SerileştirmeAttribute : Attribute
{
    public bool Dahil { get; set; } = true;
    public string SerileştirmeAdi { get; set; }

    public SerileştirmeAttribute(bool dahil = true)
    {
        Dahil = dahil;
    }
}

public class Urun
{
    public int Id { get; set; }
    
    public string Ad { get; set; }
    
    [Serileştirme(false)]
    public decimal MaliyetFiyati { get; set; }
    
    [Serileştirme(SerileştirmeAdi = "price")]
    public decimal SatisFiyati { get; set; }
}

// JSON serileştirme örneği
public static string JsonSerialize(object obj)
{
    Type tip = obj.GetType();
    var properties = tip.GetProperties()
        .Where(p => {
            if (!Attribute.IsDefined(p, typeof(SerileştirmeAttribute)))
                return true;
                
            SerileştirmeAttribute attr = (SerileştirmeAttribute)Attribute.GetCustomAttribute(p, typeof(SerileştirmeAttribute));
            return attr.Dahil;
        });
    
    Dictionary<string, object> jsonObj = new Dictionary<string, object>();
    foreach (var prop in properties)
    {
        string propName = prop.Name;
        
        // Özel serileştirme adı varsa kullan
        if (Attribute.IsDefined(prop, typeof(SerileştirmeAttribute)))
        {
            SerileştirmeAttribute attr = (SerileştirmeAttribute)Attribute.GetCustomAttribute(prop, typeof(SerileştirmeAttribute));
            if (!string.IsNullOrEmpty(attr.SerileştirmeAdi))
            {
                propName = attr.SerileştirmeAdi;
            }
        }
        
        jsonObj[propName] = prop.GetValue(obj);
    }
    
    // Gerçek bir uygulamada JSON serileştirme kütüphanesi kullanılır
    // Bu örnek basitleştirilmiştir
    return ConvertToJsonString(jsonObj);
}

private static string ConvertToJsonString(Dictionary<string, object> dict)
{
    List<string> jsonPairs = new List<string>();
    foreach (var pair in dict)
    {
        string value;
        if (pair.Value == null)
        {
            value = "null";
        }
        else if (pair.Value is string || pair.Value is DateTime)
        {
            value = $"\"{pair.Value}\"";
        }
        else
        {
            value = pair.Value.ToString();
        }
        
        jsonPairs.Add($"\"{pair.Key}\": {value}");
    }
    
    return $"{{{string.Join(", ", jsonPairs)}}}";
}
```

## Örnek Projeler

### 1. Komut Satırı Arayüzü (CLI) Uygulaması

```csharp
using System;
using System.Collections.Generic;
using System.Reflection;

[AttributeUsage(AttributeTargets.Method)]
public class KomutAttribute : Attribute
{
    public string Isim { get; }
    public string Aciklama { get; set; }

    public KomutAttribute(string isim)
    {
        Isim = isim;
    }
}

public class CliUygulama
{
    [Komut("yardim", Aciklama = "Kullanılabilir komutları listeler")]
    public void KomutlariListele()
    {
        Console.WriteLine("Kullanılabilir komutlar:");
        
        Type tip = this.GetType();
        foreach (MethodInfo metot in tip.GetMethods())
        {
            if (Attribute.IsDefined(metot, typeof(KomutAttribute)))
            {
                KomutAttribute komut = (KomutAttribute)Attribute.GetCustomAttribute(metot, typeof(KomutAttribute));
                Console.WriteLine($"  {komut.Isim}: {komut.Aciklama}");
            }
        }
    }
    
    [Komut("ekle", Aciklama = "Yeni bir öğe ekler")]
    public void OgeEkle(string isim)
    {
        Console.WriteLine($"\"{isim}\" ögesi eklendi.");
    }
    
    [Komut("sil", Aciklama = "Bir öğeyi siler")]
    public void OgeSil(string id)
    {
        Console.WriteLine($"ID: {id} olan öge silindi.");
    }
    
    [Komut("listele", Aciklama = "Tüm öğeleri listeler")]
    public void OgeleriListele()
    {
        Console.WriteLine("Tüm öğeler listeleniyor...");
        // Listeleme mantığı
    }
    
    public void KomutuCalistir(string komutAdi, string[] parametreler)
    {
        Type tip = this.GetType();
        foreach (MethodInfo metot in tip.GetMethods())
        {
            if (Attribute.IsDefined(metot, typeof(KomutAttribute)))
            {
                KomutAttribute komut = (KomutAttribute)Attribute.GetCustomAttribute(metot, typeof(KomutAttribute));
                if (komut.Isim.Equals(komutAdi, StringComparison.OrdinalIgnoreCase))
                {
                    // Parametre sayısını kontrol et
                    ParameterInfo[] metotParametreleri = metot.GetParameters();
                    if (metotParametreleri.Length != parametreler.Length)
                    {
                        Console.WriteLine($"Hata: '{komutAdi}' komutu {metotParametreleri.Length} parametre bekliyor, {parametreler.Length} verildi.");
                        return;
                    }
                    
                    try
                    {
                        // Parametreleri dönüştür ve metodu çağır
                        object[] donusturulmusParametreler = new object[parametreler.Length];
                        for (int i = 0; i < parametreler.Length; i++)
                        {
                            donusturulmusParametreler[i] = Convert.ChangeType(parametreler[i], metotParametreleri[i].ParameterType);
                        }
                        
                        metot.Invoke(this, donusturulmusParametreler);
                        return;
                    }
                    catch (Exception ex)
                    {
                        Console.WriteLine($"Hata: {ex.Message}");
                        return;
                    }
                }
            }
        }
        
        Console.WriteLine($"Hata: '{komutAdi}' komutu bulunamadı. Yardım için 'yardim' komutunu kullanın.");
    }
}

class Program
{
    static void Main(string[] args)
    {
        CliUygulama uygulama = new CliUygulama();
        
        while (true)
        {
            Console.Write("> ");
            string girdi = Console.ReadLine();
            
            if (string.IsNullOrWhiteSpace(girdi))
                continue;
                
            if (girdi.Equals("cikis", StringComparison.OrdinalIgnoreCase))
                break;
                
            string[] parcalar = girdi.Split(' ');
            string komut = parcalar[0];
            string[] parametreler = new string[parcalar.Length - 1];
            Array.Copy(parcalar, 1, parametreler, 0, parcalar.Length - 1);
            
            uygulama.KomutuCalistir(komut, parametreler);
        }
    }
}
```

### 2. Dependency Injection Container

```csharp
using System;
using System.Collections.Generic;
using System.Reflection;

[AttributeUsage(AttributeTargets.Class)]
public class ServiceAttribute : Attribute
{
    public Type Arabirim { get; }
    public ServiceLifetime Lifetime { get; set; } = ServiceLifetime.Transient;

    public ServiceAttribute(Type arabirim = null)
    {
        Arabirim = arabirim;
    }

    public enum ServiceLifetime
    {
        Singleton,
        Transient,
        Scoped
    }
}

public class Container
{
    private Dictionary<Type, ServiceInfo> services = new Dictionary<Type, ServiceInfo>();
    private Dictionary<Type, object> singletons = new Dictionary<Type, object>();

    private class ServiceInfo
    {
        public Type ImplementationType { get; set; }
        public ServiceAttribute.ServiceLifetime Lifetime { get; set; }
    }

    public void TumServisleriTara(Assembly assembly)
    {
        foreach (Type tip in assembly.GetTypes())
        {
            if (Attribute.IsDefined(tip, typeof(ServiceAttribute)))
            {
                ServiceAttribute attr = (ServiceAttribute)Attribute.GetCustomAttribute(tip, typeof(ServiceAttribute));
                Type serviceType = attr.Arabirim ?? tip;
                
                services[serviceType] = new ServiceInfo
                {
                    ImplementationType = tip,
                    Lifetime = attr.Lifetime
                };
                
                Console.WriteLine($"Servis kayıt edildi: {serviceType.Name} -> {tip.Name} ({attr.Lifetime})");
            }
        }
    }

    public T GetService<T>()
    {
        return (T)GetService(typeof(T));
    }

    public object GetService(Type serviceType)
    {
        if (!services.TryGetValue(serviceType, out ServiceInfo serviceInfo))
        {
            throw new InvalidOperationException($"Servis bulunamadı: {serviceType.Name}");
        }

        if (serviceInfo.Lifetime == ServiceAttribute.ServiceLifetime.Singleton)
        {
            if (!singletons.TryGetValue(serviceType, out object instance))
            {
                instance = CreateInstance(serviceInfo.ImplementationType);
                singletons[serviceType] = instance;
            }
            return instance;
        }
        else
        {
            return CreateInstance(serviceInfo.ImplementationType);
        }
    }

    private object CreateInstance(Type type)
    {
        // En basit oluşturucu ile örneği oluştur
        ConstructorInfo constructor = type.GetConstructors()[0];
        ParameterInfo[] parameters = constructor.GetParameters();
        
        object[] constructorParameters = new object[parameters.Length];
        for (int i = 0; i < parameters.Length; i++)
        {
            constructorParameters[i] = GetService(parameters[i].ParameterType);
        }
        
        return constructor.Invoke(constructorParameters);
    }
}

// Örnek kullanım:

public interface ILogger
{
    void Log(string message);
}

[Service(typeof(ILogger), Lifetime = ServiceAttribute.ServiceLifetime.Singleton)]
public class ConsoleLogger : ILogger
{
    public void Log(string message)
    {
        Console.WriteLine($"LOG: {message}");
    }
}

public interface IRepository
{
    void Save(string data);
}

[Service(typeof(IRepository))]
public class Repository : IRepository
{
    private readonly ILogger logger;
    
    public Repository(ILogger logger)
    {
        this.logger = logger;
    }
    
    public void Save(string data)
    {
        logger.Log($"Veri kaydedildi: {data}");
    }
}

[Service]
public class AppService
{
    private readonly IRepository repository;
    
    public AppService(IRepository repository)
    {
        this.repository = repository;
    }
    
    public void DoWork()
    {
        repository.Save("Örnek veri");
    }
}

class Program
{
    static void Main(string[] args)
    {
        Container container = new Container();
        container.TumServisleriTara(Assembly.GetExecutingAssembly());
        
        AppService app = container.GetService<AppService>();
        app.DoWork();
    }
}
```

## En İyi Uygulamalar

1. **Anlamlı İsimlendirme**: Attribute sınıflarının isimlerini, amacını açıkça belirtecek şekilde seçin ve sonuna "Attribute" ekleyin.

2. **Doğru AttributeUsage Tanımlama**: Attribute'un nerede ve nasıl kullanılabileceğini `AttributeUsage` ile açıkça belirtin.

3. **Belgeleme**: Attribute'un amacını ve kullanımını açık bir şekilde belgelendirin.

4. **Hata Kontrolü**: Attribute içindeki parametrelerin doğruluğunu kontrol edin ve gerektiğinde istisna fırlatın.

5. **Performans**: Reflection yavaş olabilir, bu nedenle attribute okuma sonuçlarını önbelleğe alın.

6. **İmmutability**: Mümkünse, attribute'ları değiştirilemez (immutable) tasarlayın.

7. **Arayüz Yerine Kullanmayın**: Attribute'ları arayüz (interface) yerine kullanmayın; davranış tanımlamak yerine meta veri eklemek için kullanın.

8. **Aşırı Kullanımdan Kaçının**: Her yerde attribute kullanmayın; sadece gerektiğinde kullanın.

### Örnek: Attribute Önbelleğe Alma

```csharp
public static class AttributeCache<T> where T : Attribute
{
    private static Dictionary<MemberInfo, T> cache = new Dictionary<MemberInfo, T>();

    public static T GetAttribute(MemberInfo memberInfo, bool inherit = false)
    {
        if (cache.TryGetValue(memberInfo, out T attribute))
            return attribute;
            
        attribute = (T)Attribute.GetCustomAttribute(memberInfo, typeof(T), inherit);
        cache[memberInfo] = attribute;
        return attribute;
    }
    
    public static void ClearCache()
    {
        cache.Clear();
    }
}
```

---

Bu rehber, C# özel attribute'larını oluşturma ve kullanma konusunda temel ve ileri düzey bilgileri kapsar. Attribute'lar, kodunuzu daha bildirimsel, daha okunabilir ve daha esnek hale getirmek için güçlü bir araçtır. Özellikle framework ve kütüphane geliştirme süreçlerinde çok değerli olabilirler.
