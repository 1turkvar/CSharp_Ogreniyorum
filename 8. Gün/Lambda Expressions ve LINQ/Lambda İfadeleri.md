# C# Lambda İfadeleri Rehberi

## İçindekiler
1. [Lambda İfadeleri Nedir?](#lambda-ifadeleri-nedir)
2. [Lambda İfade Sözdizimi](#lambda-ifade-sözdizimi)
3. [Lambda İfadelerinin Kullanım Alanları](#lambda-ifadelerinin-kullanım-alanları)
4. [Lambda İfadeleri ile Koleksiyon İşlemleri](#lambda-ifadeleri-ile-koleksiyon-işlemleri)
5. [Lambda ve Delegeler](#lambda-ve-delegeler)
6. [Lambda ve Expression Trees](#lambda-ve-expression-trees)
7. [Lambda İfadelerinde Değişken Kapsamı](#lambda-ifadelerinde-değişken-kapsamı)
8. [Lambda İfadeleri ve Asenkron Programlama](#lambda-ifadeleri-ve-asenkron-programlama)
9. [Lambda İfadelerinin Avantajları ve Dezavantajları](#lambda-ifadelerinin-avantajları-ve-dezavantajları)
10. [En İyi Kullanım Örnekleri](#en-iyi-kullanım-örnekleri)
11. [Sık Yapılan Hatalar](#sık-yapılan-hatalar)

## Lambda İfadeleri Nedir?

Lambda ifadeleri, C# programlama dilinde anonim fonksiyonlar oluşturmak için kullanılan kısa ve öz bir sözdizimi sunan özelliktir. C# 3.0 ile birlikte dile eklenmiştir ve fonksiyonel programlama paradigmasını destekler.

Lambda ifadeleri:
- İsimsiz (anonim) fonksiyonlardır
- Kısa ve öz bir şekilde yazılabilir
- Bir değişkene atanabilir veya doğrudan parametre olarak geçilebilir
- Delege tiplerinin (Func<>, Action<>, Predicate<> vb.) somut bir uygulaması olarak kullanılabilir
- LINQ sorgularında, asenkron operasyonlarda ve olay işleyicilerinde sıkça kullanılır

## Lambda İfade Sözdizimi

Lambda ifadelerinin temel sözdizimi şu şekildedir:

```csharp
(parametre_listesi) => ifade_veya_kod_bloğu
```

Burada:
- `=>` lambda operatörüdür ("goes to" veya "lambda operatörü" olarak okunur)
- Sol tarafta parametreler
- Sağ tarafta gerçekleştirilecek işlem veya dönüş değeri

Lambda ifadelerinin farklı yazım şekilleri:

### 1. Parametresiz Lambda İfadeleri:

```csharp
() => Console.WriteLine("Merhaba, Lambda!")
```

### 2. Tek Parametreli Lambda İfadeleri:

```csharp
// Parametre tipleri belirtildiğinde
(int x) => x * x

// Parametre tipleri belirtilmediğinde (tip çıkarımı)
x => x * x  // Parantezler tek parametre olduğunda opsiyoneldir
```

### 3. Çok Parametreli Lambda İfadeleri:

```csharp
// Tip belirterek
(int x, int y) => x + y

// Tip çıkarımı ile
(x, y) => x + y  // Çoklu parametrelerde parantezler zorunludur
```

### 4. İfade Gövdeli Lambda:

```csharp
x => x * x  // Tek satırlık ifade, otomatik olarak dönüş değeri olur
```

### 5. Blok Gövdeli Lambda:

```csharp
x => {
    var sonuc = x * x;
    return sonuc;  // Blok gövdeli ifadelerde return kullanılmalıdır
}
```

## Lambda İfadelerinin Kullanım Alanları

### Delegeler ve Olaylar

```csharp
// Func<T, TResult> delegesi ile kullanım
Func<int, int> kareAl = x => x * x;
int sonuc = kareAl(5);  // sonuc = 25

// Action<T> delegesi ile kullanım
Action<string> mesajYaz = mesaj => Console.WriteLine(mesaj);
mesajYaz("Merhaba");  // Ekrana "Merhaba" yazdırır

// Event handler olarak kullanım
button.Click += (sender, e) => {
    Console.WriteLine("Butona tıklandı!");
};
```

### LINQ Sorguları

```csharp
var sayilar = new List<int> { 1, 2, 3, 4, 5, 6, 7, 8, 9, 10 };

// Çiftleri filtrele
var ciftSayilar = sayilar.Where(x => x % 2 == 0);

// Karesini al
var kareler = sayilar.Select(x => x * x);

// Birden fazla koşul
var filtreliSayilar = sayilar.Where(x => x > 3 && x < 8);

// OrderBy kullanımı
var siraliSayilar = sayilar.OrderByDescending(x => x);
```

### Asenkron İşlemler

```csharp
// Task döndüren lambda
Func<Task> asenkronIslem = async () => {
    await Task.Delay(1000);
    Console.WriteLine("İşlem tamamlandı");
};

// Asenkron işlemi çağırma
await asenkronIslem();
```

## Lambda İfadeleri ile Koleksiyon İşlemleri

LINQ ile lambda ifadelerinin birlikte kullanımına daha detaylı bakalım:

### Filtreleme (Where)

```csharp
var ogrenciler = new List<Ogrenci>();
// ...öğrenci listesi doldurulur...

// 18 yaşından büyük öğrencileri filtrele
var yetiskinOgrenciler = ogrenciler.Where(o => o.Yas > 18);

// Birden fazla koşulla filtreleme
var secilenOgrenciler = ogrenciler.Where(o => o.Yas > 20 && o.Ortalama >= 3.0);
```

### Dönüştürme (Select)

```csharp
// İsimleri bir listeye çıkarma
var isimler = ogrenciler.Select(o => o.Ad);

// Yeni anonim nesneler oluşturma
var basitBilgiler = ogrenciler.Select(o => new { 
    AdSoyad = $"{o.Ad} {o.Soyad}", 
    Yas = o.Yas 
});
```

### Sıralama (OrderBy, ThenBy)

```csharp
// Yaşa göre artan sıralama
var yasaGoreSirali = ogrenciler.OrderBy(o => o.Yas);

// Önce bölüme göre sonra ortalamaya göre azalan sıralama
var siraliOgrenciler = ogrenciler
    .OrderBy(o => o.Bolum)
    .ThenByDescending(o => o.Ortalama);
```

### Gruplama (GroupBy)

```csharp
// Bölümlere göre gruplama
var bolumGruplari = ogrenciler.GroupBy(o => o.Bolum);

// Grup içinde işlemler yapma
foreach (var grup in bolumGruplari)
{
    Console.WriteLine($"Bölüm: {grup.Key}");
    Console.WriteLine($"Öğrenci Sayısı: {grup.Count()}");
    Console.WriteLine($"Ortalama Yaş: {grup.Average(o => o.Yas)}");
}
```

### Agregasyon Fonksiyonları

```csharp
// Maksimum, minimum, ortalama, toplam
double maxOrtalama = ogrenciler.Max(o => o.Ortalama);
int minYas = ogrenciler.Min(o => o.Yas);
double ortalamaYas = ogrenciler.Average(o => o.Yas);
int toplamKredi = ogrenciler.Sum(o => o.AldigiKrediler);
```

### Birleştirme (Join)

```csharp
var ogrenciler = new List<Ogrenci>();
var dersler = new List<Ders>();

// Öğrencileri aldıkları derslerle birleştirme
var ogrenciDersleri = ogrenciler
    .Join(dersler,
        ogrenci => ogrenci.Id,
        ders => ders.OgrenciId,
        (ogrenci, ders) => new { 
            OgrenciAdi = ogrenci.Ad, 
            DersAdi = ders.Ad 
        });
```

## Lambda ve Delegeler

Lambda ifadeleri, C#'taki farklı delege tipleriyle çalışır:

### Func Delegeleri

Değer döndüren metodları temsil eder:

```csharp
// Func<TResult> - parametre almaz, sonuç döndürür
Func<string> selamla = () => "Merhaba Dünya";

// Func<T, TResult> - bir parametre alır, sonuç döndürür
Func<int, int> kareAl = x => x * x;

// Func<T1, T2, TResult> - iki parametre alır, sonuç döndürür
Func<int, int, int> topla = (x, y) => x + y;

// 16'ya kadar parametre alabilir
Func<int, int, int, int, int> dortParametre = (a, b, c, d) => a + b + c + d;
```

### Action Delegeleri

Değer döndürmeyen (void) metodları temsil eder:

```csharp
// Action - parametre almaz, değer döndürmez
Action selam = () => Console.WriteLine("Merhaba");

// Action<T> - bir parametre alır, değer döndürmez
Action<string> mesajYaz = mesaj => Console.WriteLine(mesaj);

// Action<T1, T2> - iki parametre alır, değer döndürmez
Action<string, int> tekrarYaz = (mesaj, tekrar) => {
    for (int i = 0; i < tekrar; i++)
        Console.WriteLine(mesaj);
};
```

### Predicate Delegeleri

Boolean değer döndüren (koşul ifadesi) metodları temsil eder:

```csharp
// Predicate<T> - bir T parametresi alır, bool döndürür
Predicate<int> ciftMi = sayi => sayi % 2 == 0;

// List<T> Find metodu bir Predicate<T> alır
var sayilar = new List<int> { 1, 2, 3, 4, 5 };
int ilkCift = sayilar.Find(ciftMi);  // 2
```

### Custom Delegeler

Kendi delege tipinizi tanımlayabilir ve lambda ifadeleri ile kullanabilirsiniz:

```csharp
// Özel delege tanımlama
delegate double MatematikIslem(double x, double y);

// Lambda ile kullanım
MatematikIslem carpma = (x, y) => x * y;
double sonuc = carpma(5.2, 3.7);
```

## Lambda ve Expression Trees

Lambda ifadeleri, çalıştırılabilir kod olarak kullanılmalarının yanı sıra aynı zamanda "Expression Tree" (İfade Ağacı) olarak da kullanılabilir:

```csharp
// Çalıştırılabilir lambda ifadesi (delege)
Func<int, bool> ciftMiFunc = x => x % 2 == 0;

// İfade ağacı olarak lambda
Expression<Func<int, bool>> ciftMiExpr = x => x % 2 == 0;
```

İfade ağaçları, lambdanın çalıştırılabilir kod yerine ayrıştırılabilir bir yapıya dönüştürülmüş halidir. Bu özellikle:

- Entity Framework gibi ORM'lerde SQL sorgularına dönüştürülmek için
- Dinamik sorgu oluşturmak için
- Kodu çalışma zamanında analiz etmek için kullanılır

Örnek:

```csharp
// Expression kullanımı
Expression<Func<Urun, bool>> filtre = u => u.Fiyat > 100 && u.Kategori == "Elektronik";

// Entity Framework bu ifadeyi SQL sorgusuna dönüştürür
var pahalıElektronikler = dbContext.Urunler.Where(filtre);
```

İfade ağaçlarını analiz etmek:

```csharp
Expression<Func<int, bool>> expr = x => x > 10;

// İfade ağacı bilgilerini yazdırma
var binaryExpr = (BinaryExpression)expr.Body;
Console.WriteLine($"Operatör: {binaryExpr.NodeType}");  // GreaterThan
Console.WriteLine($"Sol operand: {binaryExpr.Left}");   // x
Console.WriteLine($"Sağ operand: {binaryExpr.Right}");  // 10
```

## Lambda İfadelerinde Değişken Kapsamı

Lambda ifadeleri "değişken yakalama" (variable capture) yapabilir, yani dışarıdaki kapsamdaki değişkenlere erişebilir:

```csharp
int carpan = 10;
Func<int, int> carpmaIslemi = x => x * carpan;

int sonuc = carpmaIslemi(5);  // 50 (5 * 10)

// Değiştirilen dış değişken, lambda içinde de değişir
carpan = 20;
sonuc = carpmaIslemi(5);  // 100 (5 * 20)
```

### Kapanış (Closure)

Lambda ifadesi dışarıdaki değişkeni yakaladığında bir "kapanış" (closure) oluşturulur. Bu kapanış, değişkenin değerini değil referansını yakalar:

```csharp
// Listeye lambda ekleyen fonksiyon
List<Func<int>> FonksiyonListesiOlustur()
{
    var fonksiyonlar = new List<Func<int>>();
    
    for (int i = 0; i < 5; i++)
    {
        fonksiyonlar.Add(() => i);  // i değişkenini yakalar
    }
    
    return fonksiyonlar;
}

// Fonksiyonları çağırma
var fonksiyonlar = FonksiyonListesiOlustur();
foreach (var fonk in fonksiyonlar)
{
    Console.WriteLine(fonk());  // Tüm fonksiyonlar 5 döndürür!
}
```

Buradaki sorun, tüm lambda ifadelerinin aynı `i` değişkenini yakalamasıdır. Döngü tamamlandığında `i` değeri 5 olur ve tüm lambdalar bu değeri kullanır.

Doğru yaklaşım:

```csharp
List<Func<int>> FonksiyonListesiOlustur()
{
    var fonksiyonlar = new List<Func<int>>();
    
    for (int i = 0; i < 5; i++)
    {
        int kapalıDeğişken = i;  // Her iterasyonda yeni bir değişken
        fonksiyonlar.Add(() => kapalıDeğişken);
    }
    
    return fonksiyonlar;
}

// Şimdi fonksiyonlar 0, 1, 2, 3, 4 döndürür
```

C# 5.0'dan itibaren `foreach` döngülerinde bu sorun otomatik olarak düzeltilmiştir, ancak `for` döngülerinde hala dikkatli olunmalıdır.

## Lambda İfadeleri ve Asenkron Programlama

Lambda ifadeleri, asenkron programlama ile birlikte kullanılabilir:

```csharp
// Asenkron lambda ifadesi
Func<Task> asenkronIslem = async () => {
    Console.WriteLine("İşlem başlıyor...");
    await Task.Delay(1000);  // 1 saniye bekle
    Console.WriteLine("İşlem tamamlandı!");
};

// Değer döndüren asenkron lambda
Func<string, Task<int>> kelimeSay = async (url) => {
    using (HttpClient client = new HttpClient())
    {
        string icerik = await client.GetStringAsync(url);
        return icerik.Split(' ').Length;
    }
};

// Kullanımı
await asenkronIslem();
int kelimeSayisi = await kelimeSay("https://example.com");
```

### Asenkron Lambda ile LINQ

```csharp
var urls = new List<string> {
    "https://example.com/1",
    "https://example.com/2",
    "https://example.com/3"
};

// Select ile asenkron fonksiyonu çağırma (C# 7.0 ve sonrası)
var indirmeTasks = urls.Select(url => DownloadAsync(url));
var sonuclar = await Task.WhenAll(indirmeTasks);

// Helper method
async Task<string> DownloadAsync(string url)
{
    using (HttpClient client = new HttpClient())
    {
        return await client.GetStringAsync(url);
    }
}
```

## Lambda İfadelerinin Avantajları ve Dezavantajları

### Avantajlar

1. **Kısa ve Öz Kod**: Az kod ile çok iş yapabilirsiniz.
2. **Daha Okunabilir**: Basit işlemlerde kodun amacını daha net gösterir.
3. **İşlevsel Programlama**: Fonksiyonel programlama tekniklerini C# ile kullanmanızı sağlar.
4. **Yerel Bağlam**: Dışarıdaki değişkenlere erişebilir, kapanışlar (closures) oluşturabilir.
5. **LINQ Entegrasyonu**: LINQ ile mükemmel bir şekilde çalışır.
6. **Esneklik**: Farklı delegeleri ve callback mekanizmalarını kolayca oluşturabilirsiniz.

### Dezavantajlar

1. **Debug Zorlukları**: Lambda ifadelerini debug etmek bazen zor olabilir.
2. **Okunabilirlik Kaybı**: Karmaşık lambda ifadeleri kod okunabilirliğini azaltabilir.
3. **Kapanış Tuzakları**: Değişken yakalama (variable capture) beklenmedik sorunlara yol açabilir.
4. **Performans Düşünceleri**: Bazı durumlarda normalde metotlardan biraz daha yavaş olabilir.
5. **Kapsam İzleme Zorluğu**: Büyük lambdalarda değişken kapsamlarını takip etmek zorlaşabilir.

## En İyi Kullanım Örnekleri

### 1. Event Handler

```csharp
button.Click += (sender, e) => statusLabel.Text = "Butona tıklandı";
```

### 2. LINQ Sorguları

```csharp
var aktifKullanicilar = kullanicilar
    .Where(k => k.AktifMi)
    .OrderBy(k => k.KayitTarihi)
    .Select(k => new { k.Id, k.KullaniciAdi });
```

### 3. Async İşlemler

```csharp
btnIndir.Click += async (s, e) => {
    btnIndir.Enabled = false;
    var data = await webClient.DownloadDataAsync(url);
    ProcessData(data);
    btnIndir.Enabled = true;
};
```

### 4. Callback Fonksiyonlar

```csharp
dataProcessor.OnComplete(sonuc => {
    statusBar.Text = $"İşlem tamamlandı: {sonuc.ToplamSatir} satır işlendi";
    logService.Log($"İşlem sonucu: {sonuc.BasariOrani}%");
});
```

### 5. Sıralama Kriterlerini Tanımlama

```csharp
var siparisler = new List<Siparis>();
// Sipariş durumuna göre özel sıralama
siparisler.Sort((s1, s2) => {
    if (s1.Durum == SiparisDurum.Tamamlandi && s2.Durum != SiparisDurum.Tamamlandi)
        return 1;
    if (s1.Durum != SiparisDurum.Tamamlandi && s2.Durum == SiparisDurum.Tamamlandi)
        return -1;
    return s1.Tarih.CompareTo(s2.Tarih);
});
```

## Sık Yapılan Hatalar

### 1. Değişken Yakalama Hataları

```csharp
// Hatalı kullanım
List<Action> actions = new List<Action>();
for (int i = 0; i < 5; i++)
{
    actions.Add(() => Console.WriteLine(i));
}
// Tüm eylemler 5 değerini yazdırır

// Doğru kullanım
List<Action> actionsCorrect = new List<Action>();
for (int i = 0; i < 5; i++)
{
    int kapatılan = i;
    actionsCorrect.Add(() => Console.WriteLine(kapatılan));
}
```

### 2. Aşırı Karmaşık Lambda

```csharp
// Aşırı karmaşık ve okunması zor lambda
var sonuc = veri.Where(x => {
    if (x.Tip == VeriTipi.A) {
        if (x.Deger > 100 && !x.İptalEdildi) {
            return x.Alt.Any(a => a.Durum == Durum.Aktif && a.OnayliMi);
        }
        return false;
    }
    return x.Tip == VeriTipi.B && x.Tarih > DateTime.Now.AddDays(-7);
});

// Daha okunabilir alternatif: Yardımcı metotları kullanma
bool KriterUygunMu(Veri x) {
    if (x.Tip == VeriTipi.A) {
        return x.Deger > 100 && !x.İptalEdildi && 
               x.Alt.Any(a => a.Durum == Durum.Aktif && a.OnayliMi);
    }
    return x.Tip == VeriTipi.B && x.Tarih > DateTime.Now.AddDays(-7);
}

var sonucOkunabilir = veri.Where(KriterUygunMu);
```

### 3. İfadeye Karşı İfade Ağacı Karışıklığı

```csharp
// İfade ağaçları ile çalışan bir metod (örn. EF Core Where)
IQueryable<Musteri> MusteriBul(Expression<Func<Musteri, bool>> filtre)
{
    return dbContext.Musteriler.Where(filtre);
}

// Hatalı kullanım (derleme hatası verir)
Func<Musteri, bool> musteriFiltresi = m => m.Bakiye > 1000;
var sonuc = MusteriBul(musteriFiltresi);  // Hata!

// Doğru kullanım
Expression<Func<Musteri, bool>> dogruFiltre = m => m.Bakiye > 1000;
var sonuc = MusteriBul(dogruFiltre);
```

### 4. Mutable Değişkenlerin Referansını Yakalama

```csharp
// Tehlikeli kullanım - mutable değişkenin referansını yakalama
List<int> sayilar = new List<int> { 1, 2, 3 };
Action<int> ekleVeYazdır = x => {
    sayilar.Add(x);
    Console.WriteLine(string.Join(", ", sayilar));
};

ekleVeYazdır(4);  // 1, 2, 3, 4
sayilar.Clear();  // Listeyi temizle
ekleVeYazdır(5);  // Sadece 5 (diğer sayılar gitti!)
```

### 5. Exception Handling

```csharp
// Hatalı kullanım - tüm lambda try-catch içine alındı
try {
    var sonuc = veriler.Select(v => IslemiYap(v)).ToList();
} catch (Exception ex) {
    LogError(ex);  // Sadece ilk hata yakalanır, diğerleri kaybolur
}

// Daha iyi kullanım - her öğe ayrı ayrı ele alınır
var sonuclar = veriler.Select(v => {
    try {
        return IslemiYap(v);
    } catch (Exception ex) {
        LogError(ex);
        return defaultDeger;
    }
}).ToList();
```

Bu rehber C# lambda ifadelerinin temel ve ileri düzey kullanımları hakkında kapsamlı bir bakış sağlar. Lambda ifadeleri, modern C# programlamada kod okunabilirliğini ve sürdürülebilirliğini artırmak için vazgeçilmez bir araçtır.
