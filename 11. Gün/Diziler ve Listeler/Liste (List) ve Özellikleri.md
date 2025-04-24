# C# Liste (List) ve Özellikleri

## İçindekiler
1. [List Nedir?](#list-nedir)
2. [List Oluşturma](#list-oluşturma)
3. [List'in Temel Özellikleri](#listin-temel-özellikleri)
4. [Temel List Metotları](#temel-list-metotları)
5. [List İterasyonu (Döngüler)](#list-iterasyonu-döngüler)
6. [List Sıralama ve Filtreleme](#list-sıralama-ve-filtreleme)
7. [LINQ ile List İşlemleri](#linq-ile-list-işlemleri)
8. [List Performans Değerlendirmesi](#list-performans-değerlendirmesi)
9. [List vs Array vs ArrayList](#list-vs-array-vs-arraylist)
10. [Pratik Örnekler](#pratik-örnekler)

## List Nedir?

List<T>, C# programlama dilinde System.Collections.Generic namespace'i altında bulunan, aynı türdeki nesnelerin dinamik bir koleksiyonunu temsil eden güçlü bir generic sınıftır. Array'lerden farklı olarak, boyutu dinamik olarak değişebilir ve ekleme, silme gibi işlemler için zengin bir metot kümesine sahiptir.

**Temel Özellikleri:**
- Generic yapıdadır (<T> ile belirtilen herhangi bir türü tutabilir)
- Dinamik boyutludur (otomatik olarak büyüyüp küçülebilir)
- Index tabanlı erişim sağlar (0'dan başlayan indeksler)
- LINQ sorgularıyla uyumludur
- Çeşitli işlemler için zengin metot kütüphanesi sunar

## List Oluşturma

List oluşturmanın birkaç yolu vardır:

```csharp
// Boş bir liste oluşturma
List<int> sayilar = new List<int>();

// Kapasite belirterek liste oluşturma
List<string> isimler = new List<string>(10);

// Başlangıç değerleriyle liste oluşturma
List<double> ondalikSayilar = new List<double> { 1.5, 2.7, 3.8, 4.9 };

// Var olan bir koleksiyondan liste oluşturma
int[] sayiDizisi = { 10, 20, 30, 40, 50 };
List<int> sayiListesi = new List<int>(sayiDizisi);

// C# 9.0 ve sonrası için new syntax
List<int> yeniSayilar = new() { 1, 2, 3, 4, 5 };
```

## List'in Temel Özellikleri

### Özellikler (Properties)

| Özellik | Açıklama |
|---------|----------|
| `Count` | Listedeki eleman sayısını döndürür |
| `Capacity` | Listenin içsel dizisinin boyutunu döndürür |
| `Item[index]` | İndeks kullanarak listedeki elemanlara erişmeyi sağlar (liste[i] şeklinde kullanılır) |

### Kapasite vs Sayı

`List<T>` dahili olarak bir dizi kullanır ve koleksiyonun boyutu artırılması gerektiğinde, daha büyük bir dizi oluşturulur ve mevcut öğeler yeni diziye kopyalanır:

```csharp
List<int> sayilar = new List<int>();
Console.WriteLine($"Başlangıç kapasitesi: {sayilar.Capacity}"); // Genellikle 4

// 10 eleman ekleme
for (int i = 0; i < 10; i++)
{
    sayilar.Add(i);
    Console.WriteLine($"Eleman sayısı: {sayilar.Count}, Kapasite: {sayilar.Capacity}");
}

// Kapasiteyi küçültme
sayilar.TrimExcess();
Console.WriteLine($"TrimExcess sonrası kapasite: {sayilar.Capacity}");
```

## Temel List Metotları

### Eleman Ekleme ve Silme

```csharp
List<string> meyveler = new List<string>();

// Eleman ekleme
meyveler.Add("Elma");          // Tek eleman ekleme
meyveler.AddRange(new[] { "Armut", "Muz", "Çilek" });  // Birden çok eleman ekleme
meyveler.Insert(1, "Portakal"); // Belirli bir indekse eleman ekleme

// Eleman silme
meyveler.Remove("Elma");        // Belirli bir elemanı silme
meyveler.RemoveAt(0);           // Belirli indeksteki elemanı silme
meyveler.RemoveRange(0, 2);     // Belirli aralıktaki elemanları silme
meyveler.RemoveAll(m => m.StartsWith("A")); // Koşula uyan tüm elemanları silme
meyveler.Clear();               // Tüm elemanları silme
```

### Arama ve Kontrol İşlemleri

```csharp
List<string> meyveler = new List<string> { "Elma", "Armut", "Muz", "Çilek", "Kiraz" };

// Arama işlemleri
bool varMi = meyveler.Contains("Elma");          // Elemanın varlığını kontrol etme
int indeks = meyveler.IndexOf("Muz");            // Elemanın indeksini bulma
int sonIndeks = meyveler.LastIndexOf("Elma");    // Elemanın son bulunduğu indeks
bool herhangiBiri = meyveler.Exists(m => m.Length > 5); // Koşulu sağlayan eleman var mı

// İlk ve son eleman bulma
string ilkMeyve = meyveler.First();              // İlk elemanı döndürür (boşsa hata verir)
string sonMeyve = meyveler.Last();               // Son elemanı döndürür (boşsa hata verir)
string ilkUzunMeyve = meyveler.Find(m => m.Length > 4); // Koşulu sağlayan ilk eleman
List<string> uzunMeyveler = meyveler.FindAll(m => m.Length > 4); // Koşulu sağlayan tüm elemanlar
```

### Diğer Faydalı Metotlar

```csharp
// Listenin kopyasını alma
List<string> kopya = new List<string>(meyveler);
List<string> digerKopya = meyveler.ToList();

// Listeyi diziye dönüştürme
string[] meyveArray = meyveler.ToArray();

// Tüm elemanları dönüştürme
List<int> uzunluklar = meyveler.ConvertAll(m => m.Length);

// Belirli bir eleman sayısına kadar kesme
List<string> ilkIkiMeyve = meyveler.GetRange(0, 2);

// Tüm elemanlar üzerinde işlem yapma
meyveler.ForEach(m => Console.WriteLine(m.ToUpper()));

// Listeyi sıralama
meyveler.Sort(); // Varsayılan sıralama
meyveler.Reverse(); // Listeyi tersine çevirme
```

## List İterasyonu (Döngüler)

Bir List üzerinde döngü kullanmanın birkaç yolu vardır:

```csharp
List<string> meyveler = new List<string> { "Elma", "Armut", "Muz", "Çilek", "Kiraz" };

// For döngüsü
for (int i = 0; i < meyveler.Count; i++)
{
    Console.WriteLine($"{i}. eleman: {meyveler[i]}");
}

// Foreach döngüsü
foreach (string meyve in meyveler)
{
    Console.WriteLine(meyve);
}

// ForEach metodu
meyveler.ForEach(meyve => Console.WriteLine(meyve));

// LINQ ile
meyveler.Select((meyve, index) => $"{index}. eleman: {meyve}")
        .ToList()
        .ForEach(Console.WriteLine);
```

## List Sıralama ve Filtreleme

### Temel Sıralama

```csharp
List<string> meyveler = new List<string> { "Elma", "armut", "Muz", "çilek", "Kiraz" };

// Temel sıralama
meyveler.Sort(); // Varsayılan sıralama (alfanümerik)

// Özel sıralama (case-insensitive)
meyveler.Sort(StringComparer.OrdinalIgnoreCase);

// Lambda ile özel sıralama
meyveler.Sort((x, y) => x.Length.CompareTo(y.Length)); // Uzunluğa göre sıralama
```

### IComparer Kullanarak Sıralama

```csharp
class Ogrenci
{
    public string Ad { get; set; }
    public string Soyad { get; set; }
    public int Yas { get; set; }
}

class OgrenciYasComparer : IComparer<Ogrenci>
{
    public int Compare(Ogrenci x, Ogrenci y)
    {
        return x.Yas.CompareTo(y.Yas);
    }
}

// Kullanımı
List<Ogrenci> ogrenciler = new List<Ogrenci>
{
    new Ogrenci { Ad = "Ali", Soyad = "Yılmaz", Yas = 20 },
    new Ogrenci { Ad = "Ayşe", Soyad = "Demir", Yas = 19 },
    new Ogrenci { Ad = "Mehmet", Soyad = "Kaya", Yas = 22 }
};

ogrenciler.Sort(new OgrenciYasComparer());
```

### Filtreleme İşlemleri

```csharp
List<string> meyveler = new List<string> { "Elma", "Armut", "Muz", "Çilek", "Kiraz", "Ananas" };

// FindAll ile filtreleme
List<string> aIleBaslayanlar = meyveler.FindAll(m => m.StartsWith("A"));

// RemoveAll ile filtreleme (silme)
meyveler.RemoveAll(m => m.Length < 5);

// LINQ ile filtreleme
var uzunMeyveler = meyveler.Where(m => m.Length > 5).ToList();
```

## LINQ ile List İşlemleri

LINQ (Language Integrated Query), koleksiyonlar üzerinde sorgulama yapmak için güçlü bir C# özelliğidir.

```csharp
List<string> meyveler = new List<string> { "Elma", "Armut", "Muz", "Çilek", "Kiraz", "Ananas" };

// Filtreleme
var aIleBaslayanlar = from m in meyveler
                      where m.StartsWith("A")
                      select m;

// Sıralama
var siraliMeyveler = from m in meyveler
                     orderby m.Length, m
                     select m;

// Gruplama
var uzunluklaraGore = from m in meyveler
                      group m by m.Length into g
                      select new { Uzunluk = g.Key, Meyveler = g.ToList() };

// Dönüştürme
var uzunluklar = meyveler.Select(m => new { Meyve = m, Uzunluk = m.Length });

// Agregasyon
int toplamKarakter = meyveler.Sum(m => m.Length);
double ortalamaUzunluk = meyveler.Average(m => m.Length);
int enUzun = meyveler.Max(m => m.Length);
```

## List Performans Değerlendirmesi

List<T> koleksiyonunun temel operasyonlarının karmaşıklığı:

| İşlem | Karmaşıklık | Açıklama |
|-------|-------------|----------|
| Ekleme (Add) | O(1) veya O(n) | Kapasite yeterli ise O(1), değilse yeniden boyutlandırma gerekir O(n) |
| Erişim (index) | O(1) | Dizinin indeksine doğrudan erişim |
| Arama (Contains, IndexOf) | O(n) | Tüm liste taranması gerekebilir |
| Ekleme (Insert) | O(n) | Diğer tüm öğelerin kaydırılması gerekir |
| Silme (Remove, RemoveAt) | O(n) | Öğelerin kaydırılması gerekebilir |
| Sıralama (Sort) | O(n log n) | QuickSort algoritması kullanılır |

### Performans İpuçları

1. **Başlangıç kapasitesi belirtme**: Eğer yaklaşık eleman sayısını biliyorsanız, başlangıçta kapasiteyi belirtin:
   ```csharp
   List<int> sayilar = new List<int>(10000); // 10000 kapasiteli liste
   ```

2. **TrimExcess kullanımı**: Liste üzerindeki işlemler bittikten sonra fazla kapasiteyi serbest bırakmak için:
   ```csharp
   sayilar.TrimExcess();
   ```

3. **Binary Search**: Sıralı listelerde, doğrusal arama yerine binary search kullanın:
   ```csharp
   sayilar.Sort();
   int indeks = sayilar.BinarySearch(50);
   ```

4. **RemoveAll kullanımı**: Birden fazla elemanı silmek için tekrarlı Remove çağrıları yerine:
   ```csharp
   sayilar.RemoveAll(s => s < 0); // Tüm negatif sayıları sil
   ```

## List vs Array vs ArrayList

### List vs Array

| Özellik | List<T> | Array |
|---------|---------|-------|
| Boyut | Dinamik | Sabit |
| Tür güvenliği | Generic (tür güvenli) | Tür güvenli |
| Ekstra metotlar | Add, Remove, Find vb. | Yok (Array sınıfıyla genişletilebilir) |
| Performans | Dinamik özelliklerinden dolayı küçük overhead | Hafif ve hızlı |
| Kullanım | Dinamik koleksiyonlar için | Sabit boyutlu veriler için |

```csharp
// Array
int[] sayiDizisi = new int[5];
sayiDizisi[0] = 10;

// List
List<int> sayiListesi = new List<int>();
sayiListesi.Add(10);
```

### List vs ArrayList

| Özellik | List<T> | ArrayList |
|---------|---------|-----------|
| Tür güvenliği | Generic (tür güvenli) | Non-generic (object türünde) |
| Boxing/Unboxing | Yok | Değer türleri için var |
| Performans | Daha iyi (boxing/unboxing yok) | Değer türleri için daha yavaş |
| .NET sürümü | 2.0 ve sonrası | 1.0 ve sonrası |

```csharp
// ArrayList (eski stil)
ArrayList eskiListe = new ArrayList();
eskiListe.Add(10);
eskiListe.Add("metin"); // Farklı türler eklenebilir
int sayi = (int)eskiListe[0]; // Cast gerekli

// List<T> (modern)
List<int> yeniListe = new List<int>();
yeniListe.Add(10);
// yeniListe.Add("metin"); // Derleme hatası! Sadece int eklenebilir
int sayi2 = yeniListe[0]; // Cast gerekmez
```

## Pratik Örnekler

### Örnek 1: Öğrenci Yönetim Sistemi

```csharp
class Ogrenci
{
    public int Id { get; set; }
    public string Ad { get; set; }
    public string Soyad { get; set; }
    public double NotOrtalamasi { get; set; }

    public override string ToString()
    {
        return $"{Id} - {Ad} {Soyad} (Ortalama: {NotOrtalamasi:F2})";
    }
}

class OgrenciYonetimi
{
    public static void Main()
    {
        List<Ogrenci> ogrenciler = new List<Ogrenci>
        {
            new Ogrenci { Id = 1, Ad = "Ali", Soyad = "Yılmaz", NotOrtalamasi = 85.6 },
            new Ogrenci { Id = 2, Ad = "Ayşe", Soyad = "Kara", NotOrtalamasi = 92.3 },
            new Ogrenci { Id = 3, Ad = "Mehmet", Soyad = "Demir", NotOrtalamasi = 78.9 },
            new Ogrenci { Id = 4, Ad = "Zeynep", Soyad = "Çelik", NotOrtalamasi = 90.1 }
        };

        // 1. Tüm öğrencileri listele
        Console.WriteLine("Tüm Öğrenciler:");
        ogrenciler.ForEach(Console.WriteLine);

        // 2. Ortalamaya göre sırala
        Console.WriteLine("\nNot Ortalamasına Göre Sıralı Liste:");
        ogrenciler.Sort((o1, o2) => o2.NotOrtalamasi.CompareTo(o1.NotOrtalamasi));
        ogrenciler.ForEach(Console.WriteLine);

        // 3. Belirli ortalamanın üzerindeki öğrencileri filtrele
        Console.WriteLine("\n85 Üzeri Ortalamaya Sahip Öğrenciler:");
        List<Ogrenci> basarililar = ogrenciler.FindAll(o => o.NotOrtalamasi > 85);
        basarililar.ForEach(Console.WriteLine);

        // 4. Yeni öğrenci ekle
        ogrenciler.Add(new Ogrenci { Id = 5, Ad = "Fatma", Soyad = "Şahin", NotOrtalamasi = 88.5 });
        
        // 5. Öğrenci silme
        ogrenciler.RemoveAll(o => o.NotOrtalamasi < 80);
        
        // 6. LINQ ile istatistikler
        double enYuksekNot = ogrenciler.Max(o => o.NotOrtalamasi);
        double ortalamaNot = ogrenciler.Average(o => o.NotOrtalamasi);
        
        Console.WriteLine($"\nEn yüksek not: {enYuksekNot:F2}");
        Console.WriteLine($"Ortalama not: {ortalamaNot:F2}");
    }
}
```

### Örnek 2: To-Do Uygulaması

```csharp
class ToDoItem
{
    public int Id { get; set; }
    public string Baslik { get; set; }
    public string Aciklama { get; set; }
    public bool Tamamlandi { get; set; }
    public DateTime OlusturulmaTarihi { get; set; }
    public DateTime? TamamlanmaTarihi { get; set; }
    
    public override string ToString()
    {
        string durum = Tamamlandi ? "✓" : "□";
        return $"{durum} [{Id}] {Baslik}" + 
               (Tamamlandi ? $" (Tamamlandı: {TamamlanmaTarihi:dd.MM.yyyy})" : "");
    }
}

class ToDoUygulamasi
{
    private static List<ToDoItem> gorevler = new List<ToDoItem>();
    private static int sonId = 0;
    
    public static void Main()
    {
        // Örnek görevler ekle
        GorevEkle("Alışveriş", "Süt, ekmek, peynir al");
        GorevEkle("Fatura öde", "Elektrik ve su faturalarını öde");
        GorevEkle("Randevu", "Doktor randevusunu hatırla");
        
        // Bir görevi tamamla
        GorevTamamla(1);
        
        // Görevleri listele
        GörevleriListele();
        
        // Tamamlanmamış görevleri filtrele
        Console.WriteLine("\nTamamlanmamış görevler:");
        List<ToDoItem> bekleyenler = gorevler.FindAll(g => !g.Tamamlandi);
        bekleyenler.ForEach(Console.WriteLine);
        
        // Tarih aralığındaki görevleri bul
        DateTime baslangic = DateTime.Now.AddDays(-1);
        DateTime bitis = DateTime.Now.AddDays(1);
        
        Console.WriteLine($"\n{baslangic:dd.MM.yyyy} - {bitis:dd.MM.yyyy} arasındaki görevler:");
        var tarihAraligindakiler = gorevler.FindAll(g => 
            g.OlusturulmaTarihi >= baslangic && g.OlusturulmaTarihi <= bitis);
        tarihAraligindakiler.ForEach(Console.WriteLine);
    }
    
    static void GorevEkle(string baslik, string aciklama)
    {
        sonId++;
        gorevler.Add(new ToDoItem
        {
            Id = sonId,
            Baslik = baslik, 
            Aciklama = aciklama,
            Tamamlandi = false,
            OlusturulmaTarihi = DateTime.Now
        });
    }
    
    static void GorevTamamla(int id)
    {
        ToDoItem gorev = gorevler.Find(g => g.Id == id);
        if (gorev != null)
        {
            gorev.Tamamlandi = true;
            gorev.TamamlanmaTarihi = DateTime.Now;
        }
    }
    
    static void GörevleriListele()
    {
        Console.WriteLine("TÜM GÖREVLER:");
        gorevler.ForEach(Console.WriteLine);
    }
}
```

Bu döküman, C# List sınıfının tüm temel özelliklerini ve pratik kullanım örneklerini kapsamaktadır. Bu bilgiler, List ile ilgili günlük programlama görevlerinizde size rehberlik edecektir.
