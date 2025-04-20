# C# Anonim Metotlar (Anonymous Methods) Detaylı Anlatım

## İçindekiler
1. [Giriş ve Temel Kavramlar](#giriş-ve-temel-kavramlar)
2. [Anonim Metot Sözdizimi](#anonim-metot-sözdizimi)
3. [Delegeler ve Anonim Metotlar](#delegeler-ve-anonim-metotlar)
4. [Lambda İfadeleri ile Karşılaştırma](#lambda-ifadeleri-ile-karşılaştırma)
5. [Değişken Yakalama (Variable Capturing)](#değişken-yakalama-variable-capturing)
6. [Kullanım Alanları](#kullanım-alanları)
7. [Sınırlamalar ve Dikkat Edilmesi Gerekenler](#sınırlamalar-ve-dikkat-edilmesi-gerekenler)
8. [Özetleme ve İyi Pratikler](#özetleme-ve-iyi-pratikler)
9. [Örnek Uygulamalar](#örnek-uygulamalar)

## Giriş ve Temel Kavramlar

C# programlama dilinde, anonim metotlar (anonymous methods) isim verilmeden tanımlanan ve genellikle bir delege örneğine atanan metot yapılarıdır. Bu özellik C# 2.0 sürümüyle birlikte dile eklenmiştir ve daha sonra C# 3.0'da lambda ifadeleri ile genişletilmiştir.

Anonim metotlar sayesinde:
- Kod daha kısa ve okunabilir hale gelir
- Tek kullanımlık metotlar için ayrı bir metot tanımlamanıza gerek kalmaz
- İşlevselliği doğrudan kullanıldığı yerde tanımlayabilirsiniz
- Delegelerin kullanımı daha pratik hale gelir

Temel olarak, anonim metotlar, bir metot imzası ve metot gövdesinden oluşur, ancak metoda bir isim verilmez.

## Anonim Metot Sözdizimi

Anonim bir metot, `delegate` anahtar kelimesi kullanılarak tanımlanır:

```csharp
delegate (parametre_listesi) 
{
    // Metot gövdesi
}
```

Basit bir örnek:

```csharp
// Anonim metodu bir delege değişkenine atama
Func<int, int, int> toplama = delegate(int x, int y) 
{
    return x + y;
};

// Anonim metodu çağırma
int sonuc = toplama(5, 3); // sonuc = 8
```

Eğer delege türü bağlamdan anlaşılabiliyorsa, parametre tiplerini belirtmenize gerek yoktur:

```csharp
Func<int, int, int> carpma = delegate(int a, int b) { return a * b; };
// veya daha kısa şekilde:
Func<int, int, int> carpma = delegate(a, b) { return a * b; };
```

Eğer hiç parametre almıyorsa, boş parantezler kullanabilirsiniz:

```csharp
Action mesajGoster = delegate() { Console.WriteLine("Merhaba Dünya!"); };
// veya parametre parantezlerini tamamen atlayabilirsiniz:
Action mesajGoster = delegate { Console.WriteLine("Merhaba Dünya!"); };
```

## Delegeler ve Anonim Metotlar

Anonim metotlar çoğunlukla delegelerle birlikte kullanılır. Bir delege, belirli bir imzaya sahip metotları işaret edebilen bir tür referanstır. Anonim metotlar, bu delegelerin değerleri olarak kullanılabilir.

C#'ta yaygın olarak kullanılan hazır delege türleri:

1. **Action<T1, T2, ..., T16>**: Parametreler alan ancak değer döndürmeyen metotlar için
2. **Func<T1, T2, ..., T16, TResult>**: Parametreler alan ve değer döndüren metotlar için
3. **Predicate<T>**: Bir T tipinde parametre alan ve bool döndüren metotlar için

Örnek:

```csharp
// Action kullanımı (değer döndürmeyen)
Action<string> selamla = delegate(string isim) 
{
    Console.WriteLine($"Merhaba, {isim}!");
};
selamla("Ali"); // Çıktı: Merhaba, Ali!

// Func kullanımı (değer döndüren)
Func<double, double> kareAl = delegate(double sayi) 
{
    return sayi * sayi;
};
double kareSonuc = kareAl(4.5); // kareSonuc = 20.25

// Predicate kullanımı (bool döndüren)
Predicate<int> ciftSayiMi = delegate(int sayi) 
{
    return sayi % 2 == 0;
};
bool sonuc = ciftSayiMi(6); // sonuc = true
```

Kendi delege türlerinizi de tanımlayabilirsiniz:

```csharp
// Özel delege tanımı
delegate int Hesaplayici(int a, int b);

// Anonim metot kullanımı
Hesaplayici cikar = delegate(int x, int y) { return x - y; };
int fark = cikar(10, 3); // fark = 7
```

## Lambda İfadeleri ile Karşılaştırma

C# 3.0 ile birlikte gelen lambda ifadeleri, anonim metotların daha kısa ve daha özlü bir alternatifidir. Modern C# kodunda lambda ifadeleri, anonim metotlara göre daha yaygın olarak kullanılmaktadır.

Aynı işlevselliği farklı şekillerde yazmanın karşılaştırması:

```csharp
// 1. Normal metot tanımı
int Topla(int x, int y) 
{
    return x + y;
}

// 2. Anonim metot kullanımı
Func<int, int, int> topla1 = delegate(int x, int y) 
{
    return x + y;
};

// 3. Lambda ifadesi (blok gövdeli)
Func<int, int, int> topla2 = (int x, int y) => 
{
    return x + y;
};

// 4. Lambda ifadesi (ifade gövdeli)
Func<int, int, int> topla3 = (x, y) => x + y;
```

Anonim metotlar ve lambda ifadeleri arasındaki temel farklar:

1. **Sözdizimi**: Lambda ifadeleri daha kısa ve özlüdür
2. **Dönüş tipi çıkarımı**: Lambda ifadeleri, dönüş tipini otomatik olarak çıkarabilir
3. **Parametre tiplerini belirtme**: Her iki yapıda da parametre tipleri opsiyoneldir
4. **Boş parametre listesi**: Lambda ifadelerinde `()`, anonim metotlarda `delegate()`
5. **Tarihsel bağlam**: Anonim metotlar C# 2.0, lambda ifadeleri C# 3.0 ile gelmiştir

## Değişken Yakalama (Variable Capturing)

Anonim metotların en güçlü özelliklerinden biri, tanımlandıkları kapsamdaki (scope) yerel değişkenlere erişebilme ve onları "yakalama" (capture) yeteneğidir. Bu, metot tamamlandıktan sonra bile bu değişkenlere erişim sağlanabilmesini mümkün kılar.

```csharp
public void DegiskenYakalamaOrnegi()
{
    int sayac = 0;
    
    // Sayaç değişkenini yakalayan bir anonim metot
    Action arttir = delegate 
    {
        sayac++; // Metot dışındaki değişkene erişim
        Console.WriteLine($"Sayaç değeri: {sayac}");
    };
    
    arttir(); // Çıktı: Sayaç değeri: 1
    arttir(); // Çıktı: Sayaç değeri: 2
    
    Console.WriteLine($"Son değer: {sayac}"); // Çıktı: Son değer: 2
}
```

Değişken yakalama ile ilgili dikkat edilmesi gereken noktalar:

1. **Yaşam Süresi**: Yakalanan değişkenlerin yaşam süresi, anonim metot kullanıldığı sürece devam eder
2. **Kapanış (Closures)**: Derleyici arka planda, yakalanan değişkenleri tutan bir sınıf oluşturur
3. **Değer veya Referans**: Yakalanan değişkenler, gerçek değişkenlerin referanslarıdır, kopyaları değil
4. **Döngülerde Kullanım**: Döngüler içinde değişken yakalama yapılırken dikkatli olunmalıdır

Döngülerle ilgili yaygın bir tuzak:

```csharp
List<Action> islemler = new List<Action>();

// YANLIŞ: Tüm işlemler aynı i değerini kullanacak
for (int i = 0; i < 5; i++)
{
    islemler.Add(delegate { Console.WriteLine(i); });
}

// Tüm işlemler döngü bittiğindeki i değerini (5) yazdırır
foreach (var islem in islemler)
{
    islem(); // Her biri 5 yazdırır
}

// DOĞRU: Her iterasyon için ayrı bir değişken kullanma
for (int i = 0; i < 5; i++)
{
    int gecici = i; // Her iterasyon için yerel bir değişken
    islemler.Add(delegate { Console.WriteLine(gecici); });
}

// Bu durumda 0, 1, 2, 3, 4 yazdırılır
```

C# 5.0 ve sonraki sürümlerde, döngü değişkenlerinin yakalanması durumu düzeltilmiştir, ancak daha eski sürümlerde yukarıdaki çözüm kullanılmalıdır.

## Kullanım Alanları

Anonim metotlar aşağıdaki durumlarda özellikle faydalıdır:

### 1. Olay İşleyicileri (Event Handlers)

```csharp
Button buton = new Button();
buton.Text = "Tıkla";

// Anonim olay işleyicisi
buton.Click += delegate(object sender, EventArgs e) 
{
    MessageBox.Show("Butona tıklandı!");
};
```

### 2. LINQ Sorguları

```csharp
List<int> sayilar = new List<int> { 1, 2, 3, 4, 5, 6, 7, 8, 9, 10 };

// Anonim metot ile filtreleme
var ciftSayilar = sayilar.FindAll(delegate(int sayi) 
{
    return sayi % 2 == 0;
});

// Anonim metot ile dönüştürme
var kareler = sayilar.ConvertAll(delegate(int sayi) 
{
    return sayi * sayi;
});
```

### 3. Parametre Olarak Geçirilen Fonksiyonlar

```csharp
public void ListeIsle<T>(List<T> liste, Action<T> islem)
{
    foreach (T item in liste)
    {
        islem(item);
    }
}

// Kullanım
List<string> isimler = new List<string> { "Ali", "Veli", "Ayşe" };
ListeIsle(isimler, delegate(string isim) 
{
    Console.WriteLine($"Merhaba {isim}!");
});
```

### 4. Asenkron Programlama

```csharp
void AsenkronOrnegi()
{
    Task.Run(delegate 
    {
        // Arka planda çalışacak uzun bir işlem
        for (int i = 0; i < 10; i++)
        {
            Console.WriteLine($"Arka plan işlemi: {i}");
            Thread.Sleep(500);
        }
    });
    
    Console.WriteLine("Ana thread çalışmaya devam ediyor...");
}
```

### 5. Thread ve Timer İşlemleri

```csharp
// Timer'da anonim metot kullanımı
System.Windows.Forms.Timer timer = new System.Windows.Forms.Timer();
timer.Interval = 1000;
timer.Tick += delegate(object sender, EventArgs e) 
{
    Console.WriteLine($"Şimdiki zaman: {DateTime.Now}");
};
timer.Start();

// Thread'de anonim metot kullanımı
Thread thread = new Thread(delegate() 
{
    for (int i = 0; i < 5; i++)
    {
        Console.WriteLine($"Thread içinde: {i}");
        Thread.Sleep(1000);
    }
});
thread.Start();
```

## Sınırlamalar ve Dikkat Edilmesi Gerekenler

Anonim metotların bazı sınırlamaları ve dikkat edilmesi gereken durumlar vardır:

1. **Metot Aşırı Yükleme Yok**: Anonim metotların aşırı yüklemesi (overloading) yapılamaz
2. **Hazır Operatörler Yok**: `ref` ve `out` parametreler için uygun değildir (C# 7.0 öncesi)
3. **Sınıf Üyelerine Sınırlı Erişim**: `this` anahtar kelimesi anonim metot içinde farklı anlamlara gelebilir
4. **Değişken Yakalama Davranışı**: Döngü değişkenleri yakalanırken dikkatli olunmalıdır
5. **Okunabilirlik**: Karmaşık anonim metotlar, kodun okunabilirliğini azaltabilir
6. **Hata Ayıklama**: Anonim metotlarda hata ayıklama normal metotlara göre daha zor olabilir

Örnek sınırlama:

```csharp
// HATALI: Ref parametreli anonim metot (C# 7.0 öncesi)
delegate(ref int x) { x = x * 2; }; // Derleme hatası verir

// DOĞRU: Normal metot ile ref kullanımı
void IkiyeKatla(ref int x) { x = x * 2; }
```

C# 7.0 ve sonraki sürümlerde, lambda ifadeleri ile ref ve out parametreleri kullanmak mümkündür, ancak klasik anonim metot sözdizimi ile hala kısıtlamalar vardır.

## Özetleme ve İyi Pratikler

Anonim metotları etkili kullanmak için bazı iyi pratikler:

1. **Kısa ve Öz Tutun**: Anonim metotlar, kısa ve basit işlemler için en iyidir
2. **Karmaşık İşlemler İçin İsimli Metotlar Kullanın**: Uzun ve karmaşık mantık için normal metotları tercih edin
3. **Lambda İfadelerini Tercih Edin**: Modern C# kodunda lambda ifadeleri daha yaygındır ve okunabilirliği artırır
4. **Değişken Yakalama Konusunda Dikkatli Olun**: Özellikle döngülerde
5. **İsimlendirme Kuralları**: Delege değişkenlerini, ne yaptığını açıklayan isimlerle adlandırın

Anonim metotlar vs. lambda ifadeleri ne zaman kullanılmalı:

- **Anonim Metotlar**: 
  - Eski kodlarda geriye dönük uyumluluk için
  - Blok kapsamında birden fazla deyim gerektiğinde
  - Değişken yakalama davranışı önemli olduğunda

- **Lambda İfadeleri**:
  - Modern C# kodunda genel olarak tercih edilir
  - Tek bir ifade içeren kısa metotlar için
  - LINQ sorguları ve fonksiyonel tarzda programlama için

## Örnek Uygulamalar

### Örnek 1: Liste İşleme Uygulaması

```csharp
using System;
using System.Collections.Generic;

class Program
{
    static void Main()
    {
        List<int> sayilar = new List<int> { 1, 5, 10, 15, 20, 25, 30 };
        
        // Anonim metotlarla liste işlemleri
        Console.WriteLine("Tüm sayılar:");
        ListeUzerindeCalis(sayilar, delegate(int sayi) 
        {
            Console.WriteLine(sayi);
        });
        
        Console.WriteLine("\n10'dan büyük sayılar:");
        List<int> filtrelenmis = ListeFiltrele(sayilar, delegate(int sayi) 
        {
            return sayi > 10;
        });
        
        ListeUzerindeCalis(filtrelenmis, delegate(int sayi) 
        {
            Console.WriteLine(sayi);
        });
        
        Console.WriteLine("\nSayıların kareleri:");
        List<int> donusturulmus = ListeDonustur(sayilar, delegate(int sayi) 
        {
            return sayi * sayi;
        });
        
        ListeUzerindeCalis(donusturulmus, delegate(int sayi) 
        {
            Console.WriteLine(sayi);
        });
    }
    
    // Liste elemanlarını işleyen metot
    static void ListeUzerindeCalis<T>(List<T> liste, Action<T> islem)
    {
        foreach (T eleman in liste)
        {
            islem(eleman);
        }
    }
    
    // Liste elemanlarını filtreleyen metot
    static List<T> ListeFiltrele<T>(List<T> liste, Predicate<T> filtre)
    {
        List<T> sonuc = new List<T>();
        foreach (T eleman in liste)
        {
            if (filtre(eleman))
            {
                sonuc.Add(eleman);
            }
        }
        return sonuc;
    }
    
    // Liste elemanlarını dönüştüren metot
    static List<TOutput> ListeDonustur<TInput, TOutput>(
        List<TInput> liste, 
        Func<TInput, TOutput> donusturucu)
    {
        List<TOutput> sonuc = new List<TOutput>();
        foreach (TInput eleman in liste)
        {
            sonuc.Add(donusturucu(eleman));
        }
        return sonuc;
    }
}
```

### Örnek 2: Hesap Makinesi Uygulaması

```csharp
using System;
using System.Collections.Generic;

class HesapMakinesi
{
    // İşlem delegesi
    delegate double MatematikIslemi(double x, double y);
    
    static void Main()
    {
        // İşlemleri anonim metotlarla tanımlama
        Dictionary<string, MatematikIslemi> islemler = new Dictionary<string, MatematikIslemi>();
        
        islemler.Add("toplama", delegate(double a, double b) { return a + b; });
        islemler.Add("çıkarma", delegate(double a, double b) { return a - b; });
        islemler.Add("çarpma", delegate(double a, double b) { return a * b; });
        islemler.Add("bölme", delegate(double a, double b) 
        { 
            if (b == 0)
                throw new DivideByZeroException("Sıfıra bölme hatası!");
            return a / b; 
        });
        
        // Kullanıcı girdisi
        Console.Write("İlk sayı: ");
        double sayi1 = Convert.ToDouble(Console.ReadLine());
        
        Console.Write("İkinci sayı: ");
        double sayi2 = Convert.ToDouble(Console.ReadLine());
        
        Console.Write("İşlem (toplama, çıkarma, çarpma, bölme): ");
        string islem = Console.ReadLine().ToLower();
        
        try
        {
            if (islemler.ContainsKey(islem))
            {
                double sonuc = islemler[islem](sayi1, sayi2);
                Console.WriteLine($"Sonuç: {sonuc}");
            }
            else
            {
                Console.WriteLine("Geçersiz işlem!");
            }
        }
        catch (Exception ex)
        {
            Console.WriteLine($"Hata: {ex.Message}");
        }
    }
}
```

### Örnek 3: Olay (Event) Kullanımı

```csharp
using System;
using System.Collections.Generic;

// Özel olay argümanları
class SicaklikDegisimEventArgs : EventArgs
{
    public double YeniSicaklik { get; private set; }
    
    public SicaklikDegisimEventArgs(double sicaklik)
    {
        YeniSicaklik = sicaklik;
    }
}

// Sıcaklık sensörü sınıfı
class SicaklikSensoru
{
    private double _anlikSicaklik;
    private Random _random = new Random();
    
    // Olay tanımı
    public event EventHandler<SicaklikDegisimEventArgs> SicaklikDegisti;
    
    public double AnlikSicaklik
    {
        get { return _anlikSicaklik; }
        private set
        {
            if (_anlikSicaklik != value)
            {
                _anlikSicaklik = value;
                
                // Olayı tetikle
                OnSicaklikDegisti(new SicaklikDegisimEventArgs(_anlikSicaklik));
            }
        }
    }
    
    protected virtual void OnSicaklikDegisti(SicaklikDegisimEventArgs e)
    {
        SicaklikDegisti?.Invoke(this, e);
    }
    
    public void SimuleSicaklikDegisimi()
    {
        // -5 ile +5 arasında rastgele bir değişim
        double degisim = (_random.NextDouble() * 10) - 5;
        AnlikSicaklik += degisim;
    }
    
    public SicaklikSensoru(double baslangicSicakligi)
    {
        _anlikSicaklik = baslangicSicakligi;
    }
}

class Program
{
    static void Main()
    {
        SicaklikSensoru sensor = new SicaklikSensoru(25.0);
        
        // Anonim metot ile olay işleyici ekleme
        sensor.SicaklikDegisti += delegate(object sender, SicaklikDegisimEventArgs e)
        {
            Console.WriteLine($"Sıcaklık değişti: {e.YeniSicaklik:F1}°C");
            
            if (e.YeniSicaklik > 30)
                Console.WriteLine("UYARI: Sıcaklık kritik seviyenin üstünde!");
        };
        
        // İkinci bir olay işleyici daha ekleme
        sensor.SicaklikDegisti += delegate(object sender, SicaklikDegisimEventArgs e)
        {
            // Basit bir log kaydı
            string logMesaji = $"[{DateTime.Now:HH:mm:ss}] Sıcaklık: {e.YeniSicaklik:F1}°C";
            Console.WriteLine($"LOG: {logMesaji}");
        };
        
        // Simülasyon
        Console.WriteLine("Sıcaklık sensörü simülasyonu başlatılıyor...");
        for (int i = 0; i < 10; i++)
        {
            Console.WriteLine("\nSimülasyon adım " + (i + 1));
            sensor.SimuleSicaklikDegisimi();
            System.Threading.Thread.Sleep(1000);
        }
    }
}
```

Bu örnekler, anonim metotların pratik kullanımını göstermektedir. Anonim metotlar, C#'ta delegelerin ve olay işleyicilerinin kullanımını kolaylaştıran, kodun daha kısa ve okunabilir olmasını sağlayan önemli bir özelliktir.
