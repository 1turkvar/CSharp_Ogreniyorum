# C# Metot Tanımlama ve Çağırma Rehberi

## İçindekiler
1. [Metot Nedir?](#metot-nedir)
2. [Metot Tanımlama](#metot-tanımlama)
   - [Erişim Belirleyiciler](#erişim-belirleyiciler)
   - [Dönüş Tipleri](#dönüş-tipleri)
   - [Parametreler](#parametreler)
3. [Metot Çağırma](#metot-çağırma)
4. [Parametre Türleri](#parametre-türleri)
   - [Değer Parametreleri](#değer-parametreleri)
   - [Referans Parametreleri (ref)](#referans-parametreleri-ref)
   - [Çıkış Parametreleri (out)](#çıkış-parametreleri-out)
   - [Aşırı Yüklenmiş Parametreler (params)](#aşırı-yüklenmiş-parametreler-params)
   - [İsteğe Bağlı Parametreler](#isteğe-bağlı-parametreler)
   - [İsimli Parametreler](#isimli-parametreler)
5. [Metot Aşırı Yükleme (Method Overloading)](#metot-aşırı-yükleme-method-overloading)
6. [Recursive (Özyinelemeli) Metotlar](#recursive-özyinelemeli-metotlar)
7. [Extension Metotlar](#extension-metotlar)
8. [Lambda Expression ve Anonim Metotlar](#lambda-expression-ve-anonim-metotlar)
9. [Örnek Uygulamalar](#örnek-uygulamalar)
10. [En İyi Metot Yazma Pratikleri](#en-iyi-metot-yazma-pratikleri)

---

## Metot Nedir?

Metot, belirli bir görevi yerine getirmek için tasarlanmış, yeniden kullanılabilir kod bloklarıdır. Bir metot:

- Kod tekrarını önler
- Programın okunabilirliğini artırır
- Programı modüler hale getirir
- Bakım ve güncellemeyi kolaylaştırır

C# dilinde bir metot tanımlamak için temel sözdizimi şu şekildedir:

```csharp
<erişim belirleyici> <dönüş tipi> <metot adı>(<parametre listesi>)
{
    // Metot gövdesi
    // Kodlar burada yer alır
    return <dönüş değeri>; // Eğer metot bir değer döndürüyorsa
}
```

---

## Metot Tanımlama

### Erişim Belirleyiciler

Erişim belirleyiciler, metoda nereden erişilebileceğini belirler:

- `public`: Herhangi bir sınıftan erişilebilir
- `private`: Sadece tanımlandığı sınıf içerisinden erişilebilir
- `protected`: Tanımlandığı sınıf ve bu sınıftan türetilen sınıflardan erişilebilir
- `internal`: Aynı assembly (proje) içerisindeki tüm sınıflardan erişilebilir
- `protected internal`: Aynı assembly'deki veya türetilmiş sınıflardan erişilebilir

```csharp
public int Topla(int a, int b)
{
    return a + b;
}

private void GizliMetot()
{
    Console.WriteLine("Bu metoda sadece kendi sınıfından erişilebilir.");
}
```

### Dönüş Tipleri

Metotlar çeşitli veri tiplerinde değerler döndürebilirler:

- Temel tipler: `int`, `float`, `double`, `bool`, `char` vs.
- Referans tipleri: `string`, `array`, sınıflar, arayüzler
- `void`: Değer döndürmeyen metotlar
- `Task`, `Task<T>`: Asenkron metotlar için

```csharp
// int döndüren metot
public int KareAl(int sayi)
{
    return sayi * sayi;
}

// string döndüren metot
public string AdSoyadBirlestir(string ad, string soyad)
{
    return ad + " " + soyad;
}

// void metot (değer döndürmez)
public void EkranaYaz(string mesaj)
{
    Console.WriteLine(mesaj);
}

// Task döndüren asenkron metot
public async Task<string> DosyaOkuAsync(string dosyaYolu)
{
    return await File.ReadAllTextAsync(dosyaYolu);
}
```

### Parametreler

Parametreler, metoda dışarıdan değer aktarmamızı sağlar:

```csharp
// İki parametre alan metot
public double DaireAlanHesapla(double yaricap, bool hassasHesaplama = false)
{
    double pi = hassasHesaplama ? 3.14159265359 : 3.14;
    return pi * yaricap * yaricap;
}
```

---

## Metot Çağırma

Metotlar aşağıdaki şekillerde çağrılabilir:

### 1. Aynı sınıf içerisinden çağırma:

```csharp
public class HesapMakinesi
{
    public int Topla(int a, int b)
    {
        return a + b;
    }
    
    public void IslemYap()
    {
        // Metot doğrudan çağrılabilir
        int sonuc = Topla(5, 3);
        Console.WriteLine($"Toplam: {sonuc}");
    }
}
```

### 2. Başka bir sınıftan çağırma:

```csharp
public class Program
{
    static void Main(string[] args)
    {
        // Önce sınıfın bir örneğini oluşturuyoruz
        HesapMakinesi hesaplayici = new HesapMakinesi();
        
        // Sonra metodu çağırıyoruz
        int toplam = hesaplayici.Topla(10, 20);
        Console.WriteLine($"Sonuç: {toplam}");
    }
}
```

### 3. Statik metotları çağırma:

```csharp
public class MatematikIslemleri
{
    public static double KareKok(double sayi)
    {
        return Math.Sqrt(sayi);
    }
}

// Kullanımı:
double sonuc = MatematikIslemleri.KareKok(16); // 4.0
```

---

## Parametre Türleri

### Değer Parametreleri

Varsayılan parametre türüdür. Değerin bir kopyası metoda aktarılır:

```csharp
public void DegerDegistir(int x)
{
    x = 100; // Orijinal değer değişmez, sadece kopya değişir
}

int sayi = 5;
DegerDegistir(sayi);
Console.WriteLine(sayi); // 5 yazdırır, değer değişmedi
```

### Referans Parametreleri (ref)

Parametrenin referansını metoda aktarır, böylece orijinal değer değiştirilebilir:

```csharp
public void RefDegistir(ref int x)
{
    x = 100; // Orijinal değer değişir
}

int sayi = 5;
RefDegistir(ref sayi);
Console.WriteLine(sayi); // 100 yazdırır, değer değişti
```

### Çıkış Parametreleri (out)

Metot içerisinde değer atanması zorunlu olan parametrelerdir:

```csharp
public bool TryParse(string metin, out int sonuc)
{
    if (int.TryParse(metin, out sonuc))
    {
        return true;
    }
    sonuc = 0; // out parametresine değer atamak zorunludur
    return false;
}

string deger = "123";
if (TryParse(deger, out int sonuc))
{
    Console.WriteLine($"Dönüştürülen değer: {sonuc}");
}
```

C# 7.0 ve sonrasında, out parametrelerini doğrudan metot çağrısında tanımlayabilirsiniz:

```csharp
string deger = "123";
if (int.TryParse(deger, out int sonuc))
{
    Console.WriteLine($"Dönüştürülen değer: {sonuc}");
}
```

### Aşırı Yüklenmiş Parametreler (params)

Değişken sayıda parametreyi bir dizi olarak alan metotlar tanımlamanızı sağlar:

```csharp
public int Topla(params int[] sayilar)
{
    int toplam = 0;
    foreach (int sayi in sayilar)
    {
        toplam += sayi;
    }
    return toplam;
}

// Kullanımı:
int sonuc1 = Topla(1, 2, 3);         // 6
int sonuc2 = Topla(10, 20, 30, 40);  // 100
int sonuc3 = Topla();                // 0
```

### İsteğe Bağlı Parametreler

Varsayılan değeri olan parametrelerdir. Metot çağrılırken bu parametrelere değer verilmesi isteğe bağlıdır:

```csharp
public string FormatliMetin(string metin, bool buyukHarf = false, string sonEk = ".")
{
    if (buyukHarf)
    {
        metin = metin.ToUpper();
    }
    return metin + sonEk;
}

// Kullanımı:
string s1 = FormatliMetin("Merhaba");               // "Merhaba."
string s2 = FormatliMetin("Merhaba", true);         // "MERHABA."
string s3 = FormatliMetin("Merhaba", false, "!");   // "Merhaba!"
```

### İsimli Parametreler

Parametreleri isimleriyle belirterek metotları çağırabilirsiniz. Bu, özellikle çok parametreli metotlarda kodun okunabilirliğini artırır:

```csharp
public void KullaniciOlustur(string ad, string soyad, int yas = 0, string eposta = null)
{
    // Kullanıcı oluşturma kodları...
}

// Kullanımı:
KullaniciOlustur(
    ad: "Ahmet", 
    soyad: "Yılmaz",
    eposta: "ahmet@ornek.com",  // Parametre sırasını değiştirdik
    yas: 30
);
```

---

## Metot Aşırı Yükleme (Method Overloading)

Aynı isimde fakat farklı parametrelere sahip birden fazla metot tanımlayabilirsiniz:

```csharp
public class Hesaplayici
{
    // Tam sayılarla toplama
    public int Topla(int a, int b)
    {
        return a + b;
    }
    
    // Üç sayı ile toplama
    public int Topla(int a, int b, int c)
    {
        return a + b + c;
    }
    
    // Ondalıklı sayılarla toplama
    public double Topla(double a, double b)
    {
        return a + b;
    }
}

// Kullanımı:
Hesaplayici hesap = new Hesaplayici();
int sonuc1 = hesap.Topla(5, 10);          // int Topla(int, int) çağrılır
int sonuc2 = hesap.Topla(5, 10, 15);      // int Topla(int, int, int) çağrılır
double sonuc3 = hesap.Topla(5.5, 10.5);   // double Topla(double, double) çağrılır
```

Metot aşırı yükleme için şartlar:
- Aynı isimde metotlar olmalı
- Parametrelerin sayısı veya tipleri farklı olmalı
- Sadece dönüş tipi farklı olan metotlar aşırı yükleme sayılmaz

---

## Recursive (Özyinelemeli) Metotlar

Bir metodun kendisini çağırmasına recursive (özyinelemeli) metot denir:

```csharp
public int Faktoriyel(int n)
{
    // Taban durum (base case)
    if (n <= 1)
        return 1;
    
    // Recursive çağrı
    return n * Faktoriyel(n - 1);
}

// Kullanımı:
int sonuc = Faktoriyel(5); // 5! = 5 * 4 * 3 * 2 * 1 = 120
```

Recursive metotlarda dikkat edilmesi gerekenler:
- Her zaman bir taban durum (base case) olmalıdır
- Her recursive çağrı, problemi daha küçük bir alt probleme indirgemelidir
- Sonsuz döngüye girmemek için sonlandırma koşulu olmalıdır
- Çok derin recursive çağrılar stack overflow hatasına neden olabilir

---

## Extension Metotlar

Mevcut tiplere, kaynak kodlarını değiştirmeden yeni fonksiyonlar ekleyebilirsiniz:

```csharp
// Extension metotlar static bir sınıf içinde tanımlanmalıdır
public static class StringExtensions
{
    // İlk parametre this anahtar kelimesi ile genişletilecek tipi belirtir
    public static string IlkHarfiBuyut(this string metin)
    {
        if (string.IsNullOrEmpty(metin))
            return metin;
            
        return char.ToUpper(metin[0]) + metin.Substring(1);
    }
}

// Kullanımı:
string isim = "ahmet";
string buyukHarfli = isim.IlkHarfiBuyut(); // "Ahmet"
```

---

## Lambda Expression ve Anonim Metotlar

C# dilinde lambda ifadeleri ve anonim metotlar kullanarak daha kısa ve esnek kod yazabilirsiniz:

### Anonim Metot:

```csharp
// Anonim metot tanımlama
Func<int, int, int> topla = delegate(int x, int y) 
{
    return x + y;
};

// Kullanımı
int sonuc = topla(5, 10); // 15
```

### Lambda İfadesi:

```csharp
// Lambda ifadesi ile aynı fonksiyonu tanımlama
Func<int, int, int> topla = (x, y) => x + y;

// Kullanımı
int sonuc = topla(5, 10); // 15

// Liste filtreleme örneği
List<int> sayilar = new List<int> { 1, 2, 3, 4, 5, 6, 7, 8, 9 };
List<int> ciftSayilar = sayilar.Where(x => x % 2 == 0).ToList(); // [2, 4, 6, 8]
```

---

## Örnek Uygulamalar

### 1. Basit Hesap Makinesi:

```csharp
public class HesapMakinesi
{
    public double Topla(double a, double b) => a + b;
    public double Cikar(double a, double b) => a - b;
    public double Carp(double a, double b) => a * b;
    
    public double Bol(double a, double b)
    {
        if (b == 0)
            throw new DivideByZeroException("Sıfıra bölme hatası!");
        return a / b;
    }
    
    public double Hesapla(double a, double b, string islem)
    {
        return islem switch
        {
            "+" => Topla(a, b),
            "-" => Cikar(a, b),
            "*" => Carp(a, b),
            "/" => Bol(a, b),
            _ => throw new ArgumentException("Geçersiz işlem!")
        };
    }
}

// Kullanım:
HesapMakinesi hesap = new HesapMakinesi();
Console.WriteLine(hesap.Hesapla(10, 5, "+")); // 15
Console.WriteLine(hesap.Hesapla(10, 5, "-")); // 5
Console.WriteLine(hesap.Hesapla(10, 5, "*")); // 50
Console.WriteLine(hesap.Hesapla(10, 5, "/")); // 2
```

### 2. Öğrenci Yönetim Sistemi:

```csharp
public class Ogrenci
{
    public int Id { get; set; }
    public string Ad { get; set; }
    public string Soyad { get; set; }
    public List<int> Notlar { get; set; } = new List<int>();
    
    // Tam ad döndüren metot
    public string TamAd() => $"{Ad} {Soyad}";
    
    // Not ekleyen metot
    public void NotEkle(int not)
    {
        if (not < 0 || not > 100)
            throw new ArgumentException("Not 0-100 arasında olmalıdır!");
        
        Notlar.Add(not);
    }
    
    // Ortalama hesaplayan metot
    public double Ortalama()
    {
        if (Notlar.Count == 0)
            return 0;
            
        return Notlar.Average();
    }
    
    // Durum kontrolü yapan metot
    public bool Basarili() => Ortalama() >= 60;
}

// Kullanım:
Ogrenci ogrenci = new Ogrenci 
{
    Id = 1,
    Ad = "Ayşe",
    Soyad = "Demir"
};

ogrenci.NotEkle(70);
ogrenci.NotEkle(85);
ogrenci.NotEkle(90);

Console.WriteLine($"Öğrenci: {ogrenci.TamAd()}");
Console.WriteLine($"Ortalama: {ogrenci.Ortalama()}");
Console.WriteLine($"Durum: {(ogrenci.Basarili() ? "Geçti" : "Kaldı")}");
```

---

## En İyi Metot Yazma Pratikleri

### 1. Anlamlı İsimlendirme Yapın
Metot isimleri, ne yaptığını açıkça belirtmelidir. Fiil içeren isimler kullanın.

```csharp
// Kötü
public void Islem() { ... }

// İyi
public void KullaniciKaydet() { ... }
```

### 2. Tek Sorumluluk İlkesini Takip Edin
Her metot sadece bir işi yapmalıdır.

```csharp
// Kötü
public void KullaniciKaydetVeEmailGonder() { ... }

// İyi
public void KullaniciKaydet() { ... }
public void EmailGonder(Kullanici kullanici) { ... }
```

### 3. Metotları Kısa Tutun
Uzun metotlar anlaşılması zor olabilir. 20-30 satırı geçen metotları bölmeyi düşünün.

### 4. Parametreleri Sınırlayın
Çok fazla parametre alan metotlar karmaşıklığa yol açar. 3-4'ten fazla parametre varsa, bir sınıf ya da yapı kullanmayı düşünün.

```csharp
// Kötü
public void KullaniciOlustur(string ad, string soyad, int yas, string email, string telefon, string adres) { ... }

// İyi
public void KullaniciOlustur(KullaniciBilgileri bilgiler) { ... }

public class KullaniciBilgileri
{
    public string Ad { get; set; }
    public string Soyad { get; set; }
    public int Yas { get; set; }
    public string Email { get; set; }
    public string Telefon { get; set; }
    public string Adres { get; set; }
}
```

### 5. Hata Durumlarını Düzgün Yönetin
Metotlar hata durumlarını düzgün bir şekilde yönetmelidir. Gerektiğinde istisna fırlatmalı veya hata kodu döndürmelidir.

```csharp
public bool KullaniciDogrula(string kullaniciAdi, string sifre)
{
    // Parametre kontrolü
    if (string.IsNullOrEmpty(kullaniciAdi) || string.IsNullOrEmpty(sifre))
        throw new ArgumentException("Kullanıcı adı veya şifre boş olamaz!");
        
    // Doğrulama işlemleri...
    return true; // veya false
}
```

### 6. XML Dokümantasyonu Kullanın
Metotlarınızı XML dokümantasyonu ile açıklayın. Bu, IntelliSense desteği sağlar.

```csharp
/// <summary>
/// İki sayıyı toplar.
/// </summary>
/// <param name="a">İlk sayı</param>
/// <param name="b">İkinci sayı</param>
/// <returns>İki sayının toplamı</returns>
public int Topla(int a, int b)
{
    return a + b;
}
```

### 7. Test Edilebilir Metotlar Yazın
Metotlarınızı birim testler yazılabilecek şekilde tasarlayın.

```csharp
// Kötü
public void KullaniciKaydet()
{
    // Doğrudan veritabanına erişim...
}

// İyi
public bool KullaniciKaydet(Kullanici kullanici, IVeritabaniServisi veritabani)
{
    // Bağımlılık enjeksiyonu ile daha test edilebilir
    return veritabani.Kaydet(kullanici);
}
```

---

Bu rehber, C# programlama dilinde metot tanımlama ve çağırma konularını kapsamlı bir şekilde ele almaktadır. Metotlar, kod organizasyonu ve yeniden kullanılabilirlik için vazgeçilmez yapı taşlarıdır. İyi tasarlanmış metotlar, kodunuzu daha okunaklı, bakımı kolay ve hata ayıklaması daha basit hale getirir.
