# C# Null-conditional Operatör (?.) Rehberi

## İçindekiler
1. [Giriş](#giriş)
2. [Temel Kullanım](#temel-kullanım)
3. [Geleneksel Null Kontrolü ile Karşılaştırma](#geleneksel-null-kontrolü-ile-karşılaştırma)
4. [Zincirleme Kullanım](#zincirleme-kullanım)
5. [Diziler ve Koleksiyonlarla Kullanım](#diziler-ve-koleksiyonlarla-kullanım)
6. [Metot Çağrılarıyla Kullanım](#metot-çağrılarıyla-kullanım)
7. [Null-coalescing Operatörü (??) ile Birlikte Kullanım](#null-coalescing-operatörü--ile-birlikte-kullanım)
8. [Lambda İfadeleri ve Delegelerle Kullanım](#lambda-ifadeleri-ve-delegelerle-kullanım)
9. [İyi Uygulama Örnekleri](#iyi-uygulama-örnekleri)
10. [Yaygın Hatalar ve Dikkat Edilmesi Gerekenler](#yaygın-hatalar-ve-dikkat-edilmesi-gerekenler)
11. [Performans Değerlendirmesi](#performans-değerlendirmesi)
12. [Özet](#özet)

## Giriş

C# null-conditional operatörü (?.), C# 6.0 ile tanıtılan ve kodunuzu daha kısa, daha okunabilir ve daha güvenli hale getirmeye yardımcı olan bir özelliktir. Bu operatör, nesne referanslarının null olup olmadığını kontrol etmek için kullanılır ve null referans istisnalarını (NullReferenceException) önlemeye yardımcı olur.

Genel olarak, null-conditional operatörü, bir nesnenin null olması durumunda güvenli bir şekilde özelliklerine veya metotlarına erişmenizi sağlar. Eğer nesne null ise, operatör tüm ifadeyi değerlendirir ve null döndürür, böylece kodunuzun beklenmedik bir şekilde çökmesini önler.

## Temel Kullanım

Null-conditional operatörü (?.) kullanmanın temel sözdizimi şu şekildedir:

```csharp
nesne?.özellik
nesne?.metot()
```

Bu operatör, nesnenin null olup olmadığını kontrol eder:
- Eğer nesne null değilse, özellik erişimi veya metot çağrısı normal şekilde gerçekleştirilir.
- Eğer nesne null ise, ifade null olarak değerlendirilir ve hiçbir özellik erişimi veya metot çağrısı gerçekleştirilmez.

## Geleneksel Null Kontrolü ile Karşılaştırma

Geleneksel null kontrol yöntemi:

```csharp
// Geleneksel yöntem
if (kisi != null)
{
    string ad = kisi.Ad;
    Console.WriteLine(ad);
}
```

Null-conditional operatörü kullanarak:

```csharp
// Null-conditional operatörü ile
string ad = kisi?.Ad;
Console.WriteLine(ad);
```

Bu örnekte, `kisi` null ise, `kisi?.Ad` ifadesi null döndürür ve herhangi bir istisna oluşmaz. Kod daha kısa ve daha okunabilir hale gelir.

## Zincirleme Kullanım

Null-conditional operatörü, zincirleme özelliklere erişimde özellikle faydalıdır:

```csharp
// Geleneksel yöntem
if (kisi != null && kisi.Adres != null && kisi.Adres.Sehir != null)
{
    string sehir = kisi.Adres.Sehir.Adi;
    Console.WriteLine(sehir);
}

// Null-conditional operatörü ile
string sehir = kisi?.Adres?.Sehir?.Adi;
Console.WriteLine(sehir); // kisi, Adres veya Sehir null ise, sehir değişkeni de null olur
```

## Diziler ve Koleksiyonlarla Kullanım

Null-conditional operatörü, diziler ve koleksiyonlarla çalışırken de kullanışlıdır:

```csharp
// Dizi null kontrolü
string ilkEleman = dizi?[0];

// Koleksiyon null kontrolü
int? elemanSayisi = liste?.Count;
```

Dizilerde özellikle indeks erişimi için `?.` operatörünün farklı bir varyasyonu olan `?[]` kullanılır. Bu, `dizi?[0]` ifadesinin `dizi` null ise null döndürmesini sağlar.

## Metot Çağrılarıyla Kullanım

Null-conditional operatörü, metot çağrılarında da kullanılabilir:

```csharp
// Metot çağrısında null kontrolü
kisi?.TamAd();
```

Bu durumda, `kisi` null ise metot çağrılmaz ve ifade null olarak değerlendirilir. Metot bir değer döndürüyorsa, değer tipi nullable olmayan bir tip ise (örneğin int), sonuç tip nullable olarak değerlendirilir (int?).

## Null-coalescing Operatörü (??) ile Birlikte Kullanım

Null-conditional operatörü (?.), null-coalescing operatörü (??) ile birlikte kullanılarak daha güçlü ifadeler oluşturulabilir:

```csharp
// Null-conditional ve null-coalescing operatörleri birlikte
string ad = kisi?.Ad ?? "İsimsiz";
Console.WriteLine(ad);
```

Bu örnekte, `kisi` null ise veya `kisi.Ad` null ise, "İsimsiz" değeri kullanılır.

## Lambda İfadeleri ve Delegelerle Kullanım

Null-conditional operatörü, lambda ifadeleri ve delegelerle çalışırken de kullanışlıdır:

```csharp
// Delege null kontrolü
delegate?.Invoke(parametre);

// Event null kontrolü
event?.Invoke(sender, e);
```

Burada, delege veya event null ise, Invoke metodu çağrılmaz ve hiçbir istisna oluşmaz.

## İyi Uygulama Örnekleri

### Olay İşleyicilerde Kullanım

```csharp
public class Button
{
    public event EventHandler Clicked;

    public void OnClick()
    {
        // Eski yöntem
        if (Clicked != null)
        {
            Clicked(this, EventArgs.Empty);
        }

        // Null-conditional operatörü ile
        Clicked?.Invoke(this, EventArgs.Empty);
    }
}
```

### API Çağrılarında Kullanım

```csharp
public class ApiClient
{
    private HttpClient _httpClient;

    public async Task<string> GetDataAsync(string url)
    {
        try
        {
            var response = await _httpClient?.GetAsync(url);
            return await response?.Content?.ReadAsStringAsync();
        }
        catch (Exception ex)
        {
            Console.WriteLine(ex.Message);
            return null;
        }
    }
}
```

### Veritabanı İşlemlerinde Kullanım

```csharp
public class Repository
{
    private DbContext _context;

    public User GetUserById(int id)
    {
        return _context?.Users?.FirstOrDefault(u => u.Id == id);
    }
}
```

## Yaygın Hatalar ve Dikkat Edilmesi Gerekenler

### Değer Tiplerinde Kullanım

Null-conditional operatörü, değer tipleriyle çalışırken dikkatli olunmalıdır:

```csharp
// Değer tipi sonucu için nullable değer tipi döner
int? uzunluk = metin?.Length;
```

Burada, `metin` null ise, `uzunluk` null olur. Ancak `int` değer tipi olduğu için, `int?` (nullable int) tipine dönüştürülmesi gerekir.

### Void Metotlarda Kullanım

Void metotlarda null-conditional operatörü kullanılırken dikkatli olunmalıdır:

```csharp
// Void metotta null kontrolü
nesne?.VoidMetot(); // Bu geçerlidir
```

Bu durumda, eğer `nesne` null ise, `VoidMetot` çağrılmaz ve ifade bir değer döndürmez.

### Zincirleme İşlemlerde Son Adım

Zincirleme işlemlerde, son adım önemlidir:

```csharp
// Zincirleme işlem
kisi?.Adres?.Sehir?.Adi.ToUpper(); // Dikkat! Eğer Adi null ise, NullReferenceException oluşur.

// Doğru kullanım
kisi?.Adres?.Sehir?.Adi?.ToUpper(); // Adi null ise, sonuç null olur.
```

## Performans Değerlendirmesi

Null-conditional operatörü, kodunuzu daha kısa ve daha okunabilir hale getirirken, performans açısından geleneksel if kontrollerine göre genellikle benzer veya daha iyi sonuçlar verir. Derleyici optimizasyonları sayesinde, null kontrolü için ek işlem yükü genellikle minimal düzeydedir.

Ancak, çok derinlemesine zincirleme işlemler veya performans kritik uygulamalarda, geleneksel kontroller tercih edilebilir. Her durumda, kendi uygulamanızın gereksinimlerine göre en uygun yaklaşımı seçmelisiniz.

## Özet

C# null-conditional operatörü (?.), kodunuzu daha kısa, daha okunabilir ve null referans istisnalarına karşı daha güvenli hale getiren güçlü bir özelliktir. Özellikle zincirleme özellik erişimleri, dizi erişimleri, metot çağrıları ve delegelerde kullanımı son derece faydalıdır.

Bu operatörü doğru ve etkili bir şekilde kullanarak, kodunuzu daha sağlam ve bakımı daha kolay hale getirebilirsiniz. Özellikle null-coalescing operatörü (??) ile birlikte kullanıldığında, daha az kod yazarak daha fazla işlevsellik sağlayabilirsiniz.

C# 6.0 ve üzeri sürümleri kullanıyorsanız, geleneksel null kontrollerini null-conditional operatörü ile değiştirmeyi düşünmelisiniz. Bu, kodunuzu daha modern ve daha temiz hale getirecektir.
