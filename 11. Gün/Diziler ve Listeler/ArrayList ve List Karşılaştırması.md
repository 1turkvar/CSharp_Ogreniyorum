# C# ArrayList ve List Karşılaştırması

## Genel Bakış

C# programlama dilinde koleksiyon yapıları, verileri organize etmek ve yönetmek için kullanılan temel yapı taşlarıdır. `ArrayList` ve `List<T>`, C# içinde bulunan iki önemli koleksiyon türüdür. Bu belge, her iki yapının detaylarını, birbirlerine göre avantaj ve dezavantajlarını inceleyecektir.

## ArrayList

### Tanım ve Özellikleri

`ArrayList`, .NET Framework'ün ilk versiyonlarından beri var olan, dinamik boyutlu bir dizidir ve `System.Collections` namespace'i altında bulunur.

```csharp
using System.Collections;

ArrayList liste = new ArrayList();
```

### Temel Özellikleri

1. **Tip Güvenliği Yok**: `ArrayList` her türlü veriyi (`object` olarak) depolayabilir.
2. **Dinamik Boyut**: Eleman ekledikçe otomatik olarak genişler.
3. **Boxing/Unboxing**: Değer tipleri (int, double vb.) `object` tipine dönüştürülür (boxing) ve geri alınırken yeniden kendi tipine dönüştürülür (unboxing).
4. **Legacy Sınıf**: Modern C# uygulamalarında genellikle tercih edilmez.

### Avantajları

- Her türlü veriyi aynı koleksiyonda saklayabilir
- .NET Framework 1.0'dan beri mevcuttur, eski uygulamalarla uyumludur

### Dezavantajları

- Tip güvenliği yoktur, çalışma zamanı hataları oluşabilir
- Boxing/Unboxing işlemleri performans kaybına neden olur
- Compile-time tip kontrolü yoktur

## List\<T>

### Tanım ve Özellikleri

`List<T>`, .NET 2.0 ile birlikte tanıtılan, jenerik bir koleksiyon sınıfıdır ve `System.Collections.Generic` namespace'i altında bulunur.

```csharp
using System.Collections.Generic;

List<int> liste = new List<int>();
```

### Temel Özellikleri

1. **Tip Güvenliği**: Oluşturulduğu sırada belirtilen tek bir veri tipini saklar.
2. **Dinamik Boyut**: `ArrayList` gibi otomatik olarak genişleyebilir.
3. **Boxing/Unboxing Yok**: Değer tipleri direkt olarak saklanır, dönüşüm gerekmez.
4. **Daha İyi Performans**: Tip dönüşümü gerektirmediği için daha hızlıdır.
5. **LINQ Desteği**: Modern .NET özelliklerinden LINQ sorguları ile birlikte kullanılabilir.

### Avantajları

- Derleme zamanında tip kontrolü sayesinde hataları azaltır
- Boxing/Unboxing olmadığı için daha hızlıdır
- IntelliSense desteği daha iyidir
- LINQ ve diğer modern .NET özellikleriyle tam uyumludur

### Dezavantajları

- Yalnızca tanımlandığı türdeki verileri saklayabilir
- Farklı türlerde veri saklamak için ayrı List'ler oluşturulmalıdır

## Performans Karşılaştırması

### Bellek Kullanımı

- `ArrayList`: Her eleman `object` olarak saklanır, değer tipleri için ekstra bellek gerektirir
- `List<T>`: Elemanlar doğrudan kendi tipinde saklanır, daha verimli bellek kullanımı sağlar

### Hız

Boxing/Unboxing işlemleri nedeniyle `ArrayList` genellikle `List<T>`'den daha yavaştır. Özellikle büyük koleksiyonlarda bu fark önemli olabilir.

### Örnek Performans Testi

```csharp
using System;
using System.Collections;
using System.Collections.Generic;
using System.Diagnostics;

class Program
{
    static void Main()
    {
        const int elemanSayisi = 10000000;
        
        // ArrayList ile test
        var sw1 = Stopwatch.StartNew();
        ArrayList arrayList = new ArrayList();
        for (int i = 0; i < elemanSayisi; i++)
        {
            arrayList.Add(i);
        }
        int toplam1 = 0;
        foreach (int sayi in arrayList)
        {
            toplam1 += sayi;
        }
        sw1.Stop();
        
        // List<T> ile test
        var sw2 = Stopwatch.StartNew();
        List<int> list = new List<int>();
        for (int i = 0; i < elemanSayisi; i++)
        {
            list.Add(i);
        }
        int toplam2 = 0;
        foreach (int sayi in list)
        {
            toplam2 += sayi;
        }
        sw2.Stop();
        
        Console.WriteLine($"ArrayList süresi: {sw1.ElapsedMilliseconds} ms");
        Console.WriteLine($"List<int> süresi: {sw2.ElapsedMilliseconds} ms");
    }
}
```

Bu performans testi, özellikle büyük koleksiyonlarda `List<T>`'nin `ArrayList`'ten çok daha hızlı olduğunu gösterecektir.

## Kod Örnekleri

### ArrayList Örneği

```csharp
using System;
using System.Collections;

class Program
{
    static void Main()
    {
        // ArrayList oluşturma
        ArrayList liste = new ArrayList();
        
        // Farklı tiplerde eleman ekleme
        liste.Add(42);         // int
        liste.Add("Merhaba");  // string
        liste.Add(3.14);       // double
        liste.Add(true);       // bool
        
        // Elemanlara erişim
        Console.WriteLine(liste[0]);  // 42
        
        // Elemanlara erişirken tip dönüşümü gerekir
        int sayi = (int)liste[0];
        string metin = (string)liste[1];
        
        // Tip dönüşümü hatası oluşabilir
        try
        {
            string hata = (string)liste[0];  // Hata: int'i string'e dönüştüremez
        }
        catch (InvalidCastException e)
        {
            Console.WriteLine("Tip dönüşümü hatası: " + e.Message);
        }
        
        // Temel ArrayList işlemleri
        Console.WriteLine("Eleman sayısı: " + liste.Count);
        
        liste.Insert(2, "Yeni eleman");  // 2. indekse eleman ekleme
        liste.RemoveAt(0);              // İlk elemanı silme
        liste.Remove("Merhaba");        // Belirtilen elemanı silme
        
        // Koleksiyonu temizleme
        liste.Clear();
    }
}
```

### List\<T> Örneği

```csharp
using System;
using System.Collections.Generic;

class Program
{
    static void Main()
    {
        // List<T> oluşturma
        List<int> sayilar = new List<int>();
        
        // Eleman ekleme
        sayilar.Add(42);
        sayilar.Add(100);
        sayilar.Add(7);
        
        // Derleme zamanı tip kontrolü
        // sayilar.Add("Metin"); // Derleme hatası: string tipinde değer eklenemez
        
        // Elemanlara erişim
        Console.WriteLine(sayilar[0]);  // 42
        
        // Tip dönüşümü gerekmez
        int sayi = sayilar[0];  // Doğrudan int olarak alınır
        
        // LINQ kullanımı
        var buyukSayilar = sayilar.FindAll(s => s > 40);
        Console.WriteLine("40'tan büyük sayılar:");
        foreach (var s in buyukSayilar)
        {
            Console.WriteLine(s);
        }
        
        // Temel List<T> işlemleri
        Console.WriteLine("Eleman sayısı: " + sayilar.Count);
        
        sayilar.Insert(1, 50);   // 1. indekse eleman ekleme
        sayilar.RemoveAt(0);     // İlk elemanı silme
        sayilar.Remove(7);       // Değeri 7 olan elemanı silme
        
        // Koleksiyonu temizleme
        sayilar.Clear();
        
        // Farklı tipte bir List oluşturma
        List<string> metinler = new List<string>();
        metinler.Add("Merhaba");
        metinler.Add("Dünya");
    }
}
```

## Ne Zaman Hangisini Kullanmalı?

### ArrayList'i Tercih Etmeniz Gereken Durumlar

- Eski .NET Framework sürümlerinde çalışıyorsanız (.NET 1.x gibi)
- Aynı koleksiyonda farklı tiplerde nesneler saklamanız gerekiyorsa
- Eski kodları korumak veya eski sistemlerle uyumluluk gerekiyorsa

### List\<T>'yi Tercih Etmeniz Gereken Durumlar

- Modern .NET uygulamaları geliştiriyorsanız
- Tip güvenliği istiyorsanız
- Yüksek performans gerektiren uygulamalar geliştiriyorsanız
- LINQ sorgularını kullanacaksanız
- Derleme zamanında hataların tespit edilmesini istiyorsanız

## Heterojenik Veri İhtiyacı İçin Çözümler

Eğer farklı tiplerde verileri saklamanız gerekiyorsa şu alternatifleri düşünebilirsiniz:

1. **Özel bir sınıf tanımlayın**: Farklı veri tiplerini tek bir sınıfta birleştirin ve `List<YourClass>` kullanın.
2. **Tuple kullanın**: `List<Tuple<string, int, bool>>` gibi.
3. **Object List kullanın**: `List<object>` ile tip güvenliğinden vazgeçebilirsiniz ama yine boxing/unboxing olacaktır.
4. **Dictionary kullanın**: Anahtar-değer çiftleri olarak farklı verileri saklayabilirsiniz.

## Sonuç

Modern C# uygulamalarında `List<T>` genellikle `ArrayList`'e tercih edilmelidir. `List<T>` daha iyi performans, tip güvenliği ve daha modern bir programlama deneyimi sunar. `ArrayList` yalnızca geriye dönük uyumluluk veya farklı tiplerde verileri aynı koleksiyonda saklama gibi özel durumlarda kullanılmalıdır.

.NET'in gelişim sürecinde, jenerik koleksiyonlar (.NET 2.0 ile birlikte gelmiştir) eskilerine göre önemli avantajlar sunan bir adım olmuştur ve modern C# kodunda standart olarak jenerik koleksiyonlar kullanılır.
