# C# LINQ: Query ve Method Syntax Rehberi

## İçindekiler
1. [LINQ Nedir?](#linq-nedir)
2. [Query Syntax](#query-syntax)
   - [Temel Yapı](#query-syntax-temel-yapı)
   - [Filtreleme (Where)](#query-syntax-filtreleme)
   - [Sıralama (OrderBy)](#query-syntax-sıralama)
   - [Gruplama (GroupBy)](#query-syntax-gruplama)
   - [Join İşlemleri](#query-syntax-join)
   - [Projeksiyon (Select)](#query-syntax-projeksiyon)
3. [Method Syntax](#method-syntax)
   - [Temel Yapı](#method-syntax-temel-yapı)
   - [Filtreleme Metodları](#method-syntax-filtreleme)
   - [Sıralama Metodları](#method-syntax-sıralama)
   - [Gruplama Metodları](#method-syntax-gruplama)
   - [Join Metodları](#method-syntax-join)
   - [Projeksiyon Metodları](#method-syntax-projeksiyon)
   - [Agregatörleme Metodları](#method-syntax-agregatörleme)
4. [Query Syntax ve Method Syntax Karşılaştırması](#karşılaştırma)
5. [LINQ Operator Referansı](#operator-referansı)
6. [En İyi Kullanım Pratikleri](#en-iyi-kullanım)

## <a name="linq-nedir"></a>LINQ Nedir?

LINQ (Language Integrated Query), C# programlama dilinde veri sorgulama işlemleri için sunulan, dile entegre bir sorgulama teknolojisidir. LINQ, farklı veri kaynaklarına (koleksiyonlar, XML, veritabanları vb.) tek bir sorgulama modeli sağlar.

```csharp
// Gerekli namespace'ler
using System;
using System.Collections.Generic;
using System.Linq;
```

LINQ ile yapabileceğiniz bazı temel işlemler:
- Veri filtreleme
- Veri sıralaması
- Veri gruplama
- Veri dönüştürme
- Veri birleştirme
- Agregatörleme fonksiyonları (sayma, toplama, ortalama alma vb.)

LINQ iki farklı yazım biçimi sunar:
1. **Query Syntax**: SQL benzeri bir söz dizimi
2. **Method Syntax**: Zincirleme metod çağrıları kullanan bir söz dizimi

## <a name="query-syntax"></a>Query Syntax

### <a name="query-syntax-temel-yapı"></a>Temel Yapı

Query Syntax, SQL sorgu diline benzer bir yapıya sahiptir ve genellikle okunabilirliği arttırır.

```csharp
// Temel Query Syntax yapısı
var sonuc = from eleman in koleksiyon
            where eleman.Ozellik > 10
            orderby eleman.Ozellik
            select eleman;
```

### <a name="query-syntax-filtreleme"></a>Filtreleme (Where)

```csharp
// 18 yaşından büyük kişileri filtrele
var yetiskinler = from kisi in kisiler
                  where kisi.Yas > 18
                  select kisi;

// Çoklu koşul
var filtrelenmisKisiler = from kisi in kisiler
                         where kisi.Yas > 18 && kisi.Sehir == "Istanbul"
                         select kisi;
```

### <a name="query-syntax-sıralama"></a>Sıralama (OrderBy)

```csharp
// Yaşa göre artan sıralama
var siralanmisKisiler = from kisi in kisiler
                       orderby kisi.Yas
                       select kisi;

// Çoklu sıralama (önce şehre göre, sonra yaşa göre azalan)
var siralanmisKisiler = from kisi in kisiler
                       orderby kisi.Sehir, kisi.Yas descending
                       select kisi;
```

### <a name="query-syntax-gruplama"></a>Gruplama (GroupBy)

```csharp
// Şehirlere göre gruplama
var gruplar = from kisi in kisiler
              group kisi by kisi.Sehir into sehirGrubu
              select new { Sehir = sehirGrubu.Key, Kisiler = sehirGrubu.ToList() };

// Gruplama ve sayma
var sehirIstatistikleri = from kisi in kisiler
                         group kisi by kisi.Sehir into sehirGrubu
                         select new {
                             Sehir = sehirGrubu.Key,
                             KisiSayisi = sehirGrubu.Count(),
                             OrtalamaYas = sehirGrubu.Average(k => k.Yas)
                         };
```

### <a name="query-syntax-join"></a>Join İşlemleri

```csharp
// İki koleksiyonu birleştirme
var sonuc = from kisi in kisiler
            join siparis in siparisler
            on kisi.Id equals siparis.MusteriId
            select new { kisi.Ad, siparis.SiparisNo, siparis.Tutar };

// Left Join
var leftJoin = from kisi in kisiler
               join siparis in siparisler
               on kisi.Id equals siparis.MusteriId into siparisGrubu
               from s in siparisGrubu.DefaultIfEmpty()
               select new { kisi.Ad, SiparisNo = s?.SiparisNo ?? "Sipariş Yok", Tutar = s?.Tutar ?? 0 };
```

### <a name="query-syntax-projeksiyon"></a>Projeksiyon (Select)

```csharp
// Yeni anonim tip oluşturma
var kisiBilgisi = from kisi in kisiler
                 select new { kisi.Ad, kisi.Soyad, DogumYili = DateTime.Now.Year - kisi.Yas };

// Yeni sınıf örneği oluşturma
var kisiBilgisi = from kisi in kisiler
                 select new KisiBilgi {
                     TamAd = $"{kisi.Ad} {kisi.Soyad}",
                     Yas = kisi.Yas
                 };
```

## <a name="method-syntax"></a>Method Syntax

### <a name="method-syntax-temel-yapı"></a>Temel Yapı

Method Syntax, uzantı metodları kullanarak zincirleme şeklinde çağrılar yapma yaklaşımıdır.

```csharp
// Temel Method Syntax yapısı
var sonuc = koleksiyon
    .Where(eleman => eleman.Ozellik > 10)
    .OrderBy(eleman => eleman.Ozellik)
    .Select(eleman => eleman);
```

### <a name="method-syntax-filtreleme"></a>Filtreleme Metodları

```csharp
// Where ile filtreleme
var yetiskinler = kisiler.Where(kisi => kisi.Yas > 18);

// Çoklu koşul
var filtrelenmisKisiler = kisiler.Where(kisi => kisi.Yas > 18 && kisi.Sehir == "Istanbul");

// İlk 5 eleman
var ilkBesKisi = kisiler.Take(5);

// İlk 5 eleman atla, sonraki 10 eleman al
var sayfalama = kisiler.Skip(5).Take(10);

// Belirli koşulu sağlayan tek bir öğe
var kisi = kisiler.Single(k => k.Id == 5); // Eğer bulunamazsa veya birden fazla eleman varsa exception fırlatır
var kisi = kisiler.SingleOrDefault(k => k.Id == 5); // Bulunamazsa null döner, birden fazla varsa exception fırlatır

// İlk bulunan eleman
var kisi = kisiler.First(k => k.Sehir == "Ankara"); // Bulunamazsa exception fırlatır
var kisi = kisiler.FirstOrDefault(k => k.Sehir == "Ankara"); // Bulunamazsa null döner
```

### <a name="method-syntax-sıralama"></a>Sıralama Metodları

```csharp
// Yaşa göre artan sıralama
var siralanmisKisiler = kisiler.OrderBy(kisi => kisi.Yas);

// Yaşa göre azalan sıralama
var siralanmisKisiler = kisiler.OrderByDescending(kisi => kisi.Yas);

// Çoklu sıralama
var siralanmisKisiler = kisiler
    .OrderBy(kisi => kisi.Sehir)
    .ThenByDescending(kisi => kisi.Yas);

// Sıralama ve tersten çevirme
var terstenSirali = kisiler.OrderBy(kisi => kisi.Yas).Reverse();
```

### <a name="method-syntax-gruplama"></a>Gruplama Metodları

```csharp
// GroupBy ile gruplama
var gruplar = kisiler
    .GroupBy(kisi => kisi.Sehir)
    .Select(grup => new { Sehir = grup.Key, Kisiler = grup.ToList() });

// Gruplama ve sayma
var sehirIstatistikleri = kisiler
    .GroupBy(kisi => kisi.Sehir)
    .Select(grup => new {
        Sehir = grup.Key,
        KisiSayisi = grup.Count(),
        OrtalamaYas = grup.Average(k => k.Yas)
    });

// ToLookup - Hemen değerlendirilen gruplama
var sehirGruplari = kisiler.ToLookup(kisi => kisi.Sehir);
var istanbulKisileri = sehirGruplari["Istanbul"];
```

### <a name="method-syntax-join"></a>Join Metodları

```csharp
// Join
var sonuc = kisiler
    .Join(
        siparisler,
        kisi => kisi.Id,
        siparis => siparis.MusteriId,
        (kisi, siparis) => new { kisi.Ad, siparis.SiparisNo, siparis.Tutar }
    );

// GroupJoin (Left Join)
var leftJoin = kisiler
    .GroupJoin(
        siparisler,
        kisi => kisi.Id,
        siparis => siparis.MusteriId,
        (kisi, siparisler) => new { Kisi = kisi, Siparisler = siparisler }
    )
    .SelectMany(
        x => x.Siparisler.DefaultIfEmpty(),
        (x, siparis) => new { x.Kisi.Ad, SiparisNo = siparis?.SiparisNo ?? "Sipariş Yok", Tutar = siparis?.Tutar ?? 0 }
    );
```

### <a name="method-syntax-projeksiyon"></a>Projeksiyon Metodları

```csharp
// Select ile projeksiyon
var adlar = kisiler.Select(kisi => kisi.Ad);

// Anonim tip oluşturma
var kisiBilgisi = kisiler.Select(kisi => new { kisi.Ad, kisi.Soyad, DogumYili = DateTime.Now.Year - kisi.Yas });

// Diğer koleksiyon içindeki koleksiyonu düzleştirme
var tumUrunler = musteriler.SelectMany(m => m.Urunler);

// Select ile indeks kullanımı
var numaralandirilmisListe = kisiler.Select((kisi, index) => new { No = index + 1, kisi.Ad });
```

### <a name="method-syntax-agregatörleme"></a>Agregatörleme Metodları

```csharp
// Toplam
int toplamYas = kisiler.Sum(kisi => kisi.Yas);

// Ortalama
double ortalamaYas = kisiler.Average(kisi => kisi.Yas);

// Sayma
int istanbulluSayisi = kisiler.Count(kisi => kisi.Sehir == "Istanbul");
bool varmı = kisiler.Any(kisi => kisi.Yas > 65);
bool hepsiMi = kisiler.All(kisi => kisi.Yas > 18);

// Min/Max
int enKucukYas = kisiler.Min(kisi => kisi.Yas);
int enBuyukYas = kisiler.Max(kisi => kisi.Yas);
var enGenc = kisiler.OrderBy(k => k.Yas).First();
var enYasli = kisiler.OrderByDescending(k => k.Yas).First();

// Özel agregatörleme
int ciftYaslarinToplami = kisiler
    .Where(kisi => kisi.Yas % 2 == 0)
    .Aggregate(0, (toplam, kisi) => toplam + kisi.Yas);
```

## <a name="karşılaştırma"></a>Query Syntax ve Method Syntax Karşılaştırması

### Avantajlar ve Dezavantajlar

**Query Syntax**
- Avantajlar:
  - SQL benzeri yapısı nedeniyle database sorguları yazmaya alışkın geliştiriciler için tanıdık olabilir
  - Karmaşık sorgularda özellikle okunabilirliği daha iyi olabilir
  - Sorgularda join ve gruplamayı daha doğal bir şekilde ifade edebilir
  
- Dezavantajlar:
  - Tüm LINQ operatörleri query syntax'da kullanılamaz
  - Her zaman en sonunda method syntax'a dönüştürülür

**Method Syntax**
- Avantajlar:
  - Daha fazla esneklik ve tüm LINQ operatörlerine erişim
  - Lambda ifadelerini kullanarak daha kompakt kod
  - Zincirlenebilir metodlar sayesinde akıcı kod yazımı
  - IntelliSense ile daha iyi otomatik tamamlama
  
- Dezavantajlar:
  - Karmaşık sorgularda okunabilirlik azalabilir
  - İç içe sorgularda parantez yönetimi zorlaşabilir

### Örnek Karşılaştırma

**Query Syntax**
```csharp
var sonuc = from kisi in kisiler
            where kisi.Yas > 18
            orderby kisi.Soyad, kisi.Ad
            select new { kisi.Ad, kisi.Soyad, Yetiskin = true };
```

**Method Syntax**
```csharp
var sonuc = kisiler
    .Where(kisi => kisi.Yas > 18)
    .OrderBy(kisi => kisi.Soyad)
    .ThenBy(kisi => kisi.Ad)
    .Select(kisi => new { kisi.Ad, kisi.Soyad, Yetiskin = true });
```

## <a name="operator-referansı"></a>LINQ Operator Referansı

### Filtreleme Operatörleri
- **Where**: Belirtilen koşulu sağlayan elemanları seçer
- **OfType**: Belirtilen türdeki elemanları seçer
- **Distinct**: Tekrar eden elemanları çıkartır
- **Take**: Belirtilen sayıda elemanı baştan alır
- **Skip**: Belirtilen sayıda elemanı atlar
- **TakeWhile**: Belirtilen koşul sağlandığı sürece elemanları alır
- **SkipWhile**: Belirtilen koşul sağlandığı sürece elemanları atlar

### Sıralama Operatörleri
- **OrderBy**: Belirtilen özelliğe göre artan sıralama yapar
- **OrderByDescending**: Belirtilen özelliğe göre azalan sıralama yapar
- **ThenBy**: İkincil sıralama kriteri ekler (artan)
- **ThenByDescending**: İkincil sıralama kriteri ekler (azalan)
- **Reverse**: Koleksiyonu tersine çevirir

### Projeksiyon Operatörleri
- **Select**: Her bir elemanı dönüştürür
- **SelectMany**: İç içe koleksiyonları düzleştirir

### Birleştirme Operatörleri
- **Join**: İki koleksiyonu birleştirir (inner join)
- **GroupJoin**: İki koleksiyonu gruplandırarak birleştirir
- **Zip**: İki koleksiyonu eşleşen indekslere göre birleştirir
- **Concat**: İki koleksiyonu ardışık olarak birleştirir

### Gruplama Operatörleri
- **GroupBy**: Belirtilen kritere göre gruplar
- **ToLookup**: Belirtilen kritere göre anında değerlendirilen gruplar oluşturur

### Set Operatörleri
- **Distinct**: Tekrar eden elemanları çıkartır
- **Union**: İki koleksiyonun birleşimi (tekrarsız)
- **Intersect**: İki koleksiyonun kesişimi
- **Except**: İki koleksiyonun farkı (ilkinde olup ikincide olmayanlar)

### Eleman Operatörleri
- **First**: İlk elemanı getirir, yoksa hata fırlatır
- **FirstOrDefault**: İlk elemanı getirir, yoksa varsayılan değer döner
- **Last**: Son elemanı getirir, yoksa hata fırlatır
- **LastOrDefault**: Son elemanı getirir, yoksa varsayılan değer döner
- **Single**: Tek elemanı getirir, yoksa veya birden fazla varsa hata fırlatır
- **SingleOrDefault**: Tek elemanı getirir, yoksa varsayılan değer döner, birden fazla varsa hata fırlatır
- **ElementAt**: Belirtilen indeksteki elemanı getirir
- **ElementAtOrDefault**: Belirtilen indeksteki elemanı getirir, yoksa varsayılan değer döner
- **DefaultIfEmpty**: Koleksiyon boşsa varsayılan değer döner

### Agregatörleme Operatörleri
- **Count**: Eleman sayısını verir
- **Sum**: Sayısal değerlerin toplamını verir
- **Average**: Sayısal değerlerin ortalamasını verir
- **Min**: En küçük değeri verir
- **Max**: En büyük değeri verir
- **Aggregate**: Özel bir agregatörleme işlemi yapar

### Dönüştürme Operatörleri
- **ToArray**: Array'e dönüştürür
- **ToList**: List'e dönüştürür
- **ToDictionary**: Dictionary'e dönüştürür
- **ToLookup**: Lookup'a dönüştürür
- **AsEnumerable**: IEnumerable'a dönüştürür
- **AsQueryable**: IQueryable'a dönüştürür

## <a name="en-iyi-kullanım"></a>En İyi Kullanım Pratikleri

### Performans İpuçları
1. **Ertelenmiş Değerlendirme**
   ```csharp
   // Bu sorgu hemen çalıştırılmaz, sadece tanımlanır
   var sorgu = kisiler.Where(k => k.Yas > 18);
   
   // Sorgu ancak veriye erişildiğinde çalıştırılır
   foreach (var kisi in sorgu) { ... }
   ```

2. **Hemen Değerlendirme**
   ```csharp
   // ToList, ToArray gibi metodlar sorguyu hemen değerlendirir
   var liste = kisiler.Where(k => k.Yas > 18).ToList();
   ```

3. **IQueryable vs IEnumerable**
   ```csharp
   // IQueryable - veritabanı sorguları için optimization sağlar
   IQueryable<Kisi> sorgu = dbContext.Kisiler.Where(k => k.Yas > 18);
   
   // IEnumerable - bellek içi koleksiyonlar için
   IEnumerable<Kisi> liste = kisiler.Where(k => k.Yas > 18);
   ```

### Yaygın Hatalar ve Çözümleri

1. **Multiple Enumeration (Çoklu Sayma) Problemi**
   ```csharp
   // Hatalı kullanım - sorgu her kullanıldığında yeniden değerlendirilir
   var sorgu = kisiler.Where(k => k.Yas > 18);
   var sayı = sorgu.Count(); // İlk değerlendirme
   var ortalama = sorgu.Average(k => k.Yas); // İkinci değerlendirme
   
   // Doğru kullanım
   var liste = kisiler.Where(k => k.Yas > 18).ToList();
   var sayı = liste.Count;
   var ortalama = liste.Average(k => k.Yas);
   ```

2. **N+1 Sorgu Problemi (Entity Framework'te)**
   ```csharp
   // Hatalı kullanım
   var musteriler = dbContext.Musteriler.ToList();
   foreach (var musteri in musteriler) {
       // Her müşteri için ayrı sorgu çalıştırılır
       var siparisler = musteri.Siparisler;
   }
   
   // Doğru kullanım - Eager Loading
   var musteriler = dbContext.Musteriler.Include(m => m.Siparisler).ToList();
   ```

3. **Büyük Veri Setlerinde Paging Kullanmama**
   ```csharp
   // Hatalı kullanım - çok fazla veri çekilir
   var tumKisiler = dbContext.Kisiler.ToList();
   
   // Doğru kullanım - sayfalama
   var sayfa1 = dbContext.Kisiler.Skip(0).Take(100).ToList();
   ```

4. **Gereksiz Materialization**
   ```csharp
   // Hatalı kullanım
   var liste = dbContext.Kisiler.ToList();
   var yetiskinler = liste.Where(k => k.Yas > 18).ToList();
   
   // Doğru kullanım
   var yetiskinler = dbContext.Kisiler.Where(k => k.Yas > 18).ToList();
   ```

### Sorgu ve Method Syntax Seçimi İpuçları

1. **Query Syntax İçin İdeal Durumlar**
   - Karmaşık join işlemleri
   - Çok aşamalı gruplama işlemleri
   - Koşulları açık bir şekilde ifade etmek istediğiniz durumlar
   - SQL ile aşina olanların yazdığı kod

2. **Method Syntax İçin İdeal Durumlar**
   - Basit filtreleme, sıralama ve seçme işlemleri
   - Aggregate fonksiyonlar (Sum, Count, Average vb.)
   - Method zincirleme yoğun işlemler
   - Take, Skip, First, Last gibi operatörler
   - Extension metodlarını kullanmak istediğiniz durumlar

3. **Karma Kullanım**
   ```csharp
   // Query ve Method Syntax'ı birlikte kullanabilirsiniz
   var sonuc = from kisi in kisiler.Where(k => k.Aktif)
               orderby kisi.Yas
               select new { kisi.Ad, kisi.Soyad }
               into kisiDetay
               where kisiDetay.Ad.StartsWith("A")
               select kisiDetay;
   ```

### Örnek Uygulama Alanları

1. **Entity Framework ile Veritabanı Sorgulaması**
   ```csharp
   using (var db = new UygulamaDbContext()) {
       var sonuc = from k in db.Kisiler
                   join a in db.Adresler on k.Id equals a.KisiId
                   where k.Yas > 18 && a.Sehir == "Istanbul"
                   select new { k.Ad, k.Soyad, a.Sehir };
       
       // veya
       
       var sonuc = db.Kisiler
           .Join(db.Adresler, k => k.Id, a => a.KisiId, (k, a) => new { Kisi = k, Adres = a })
           .Where(x => x.Kisi.Yas > 18 && x.Adres.Sehir == "Istanbul")
           .Select(x => new { x.Kisi.Ad, x.Kisi.Soyad, x.Adres.Sehir });
   }
   ```

2. **XML Verisi Üzerinde Sorgulama**
   ```csharp
   XDocument doc = XDocument.Load("veriler.xml");
   
   var sonuc = from eleman in doc.Descendants("Kisi")
               where (int)eleman.Element("Yas") > 18
               select new {
                   Ad = (string)eleman.Element("Ad"),
                   Soyad = (string)eleman.Element("Soyad")
               };
   ```

3. **Bellek İçi Koleksiyonlar**
   ```csharp
   List<Urun> urunler = GetUrunler();
   
   var sonuc = urunler
       .Where(u => u.Fiyat > 100)
       .OrderBy(u => u.Kategori)
       .ThenByDescending(u => u.Fiyat)
       .Select(u => new { u.Ad, u.Fiyat, IndirimliTutar = u.Fiyat * 0.9m });
   ```

4. **Paralel Sorgular**
   ```csharp
   // Büyük veri setleri için paralel işleme
   var sonuc = kisiler.AsParallel()
       .Where(k => k.Yas > 18)
       .OrderBy(k => k.Soyad)
       .ToList();
   ```

### Kapanış

C# LINQ'nun Query ve Method Syntax'ları, veri sorgulama işlemlerini kolaylaştıran güçlü araçlardır. Bu rehberde, her iki yaklaşımın temel yapıları, avantajları ve dezavantajları, yaygın operatörler ve en iyi kullanım pratikleri incelenmiştir. 

Kendi programlama stilinize ve ihtiyaçlarınıza en uygun yaklaşımı seçebilirsiniz. Birçok durumda iki yaklaşımı da bir arada kullanabilirsiniz.

LINQ'yu en etkin şekilde kullanarak, daha az kod yazarak daha güçlü sorgular oluşturabilir ve kod kalitesini artırabilirsiniz.
