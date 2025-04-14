# C# foreach Döngüsü Detaylı Anlatım

## İçindekiler
1. [Giriş](#giriş)
2. [foreach Döngüsünün Sözdizimi](#foreach-döngüsünün-sözdizimi)
3. [foreach Döngüsünün Çalışma Mantığı](#foreach-döngüsünün-çalışma-mantığı)
4. [foreach ile Kullanılabilen Koleksiyonlar](#foreach-ile-kullanılabilen-koleksiyonlar)
5. [foreach Döngüsü Örnekleri](#foreach-döngüsü-örnekleri)
6. [foreach vs for Karşılaştırması](#foreach-vs-for-karşılaştırması)
7. [foreach Kullanırken Dikkat Edilmesi Gerekenler](#foreach-kullanırken-dikkat-edilmesi-gerekenler)
8. [IEnumerable ve foreach İlişkisi](#ienumerable-ve-foreach-i̇lişkisi)
9. [LINQ ile foreach Kullanımı](#linq-ile-foreach-kullanımı)
10. [Özet](#özet)

## Giriş

`foreach` döngüsü, C# programlama dilindeki en kullanışlı döngü yapılarından biridir. Bu döngü, diziler, koleksiyonlar ve diğer yinelenebilir nesneler üzerinde dolaşmak için özel olarak tasarlanmıştır. `foreach` döngüsü, koleksiyondaki her bir öğeyi tek tek işlemenizi sağlar ve geleneksel `for` döngüsüne göre daha okunabilir ve basit bir alternatif sunar.

## foreach Döngüsünün Sözdizimi

`foreach` döngüsünün genel sözdizimi şu şekildedir:

```csharp
foreach (ElementType element in collection)
{
    // Koleksiyondaki her eleman için çalıştırılacak kod
}
```

Burada:
- `ElementType`: Koleksiyonda bulunan elemanların veri tipi
- `element`: Her yinelemede koleksiyondan alınan elemanı temsil eden değişken
- `collection`: Üzerinde dolaşılacak dizi, liste veya başka bir koleksiyon

## foreach Döngüsünün Çalışma Mantığı

`foreach` döngüsü aşağıdaki adımları izleyerek çalışır:

1. Koleksiyonun bir yineleyici (iterator) nesnesi oluşturulur
2. Koleksiyonun başına konumlanılır
3. Koleksiyonda daha fazla eleman olup olmadığı kontrol edilir
4. Eğer eleman varsa, ilgili eleman alınır ve belirtilen değişkene atanır
5. Döngü gövdesindeki kod bloğu çalıştırılır
6. Döngü, koleksiyonun sonuna gelinene kadar 3.-5. adımları tekrarlar

Döngünün arkasında, koleksiyonun bir `IEnumerator` nesnesi kullanılır. Bu, C# derleyicisinin `foreach` döngüsünü standart bir yapıya dönüştürmesiyle sağlanır.

## foreach ile Kullanılabilen Koleksiyonlar

`foreach` döngüsü aşağıdaki koleksiyon türleriyle kullanılabilir:

- Diziler (`Array`)
- Listeler (`List<T>`)
- Sözlükler (`Dictionary<TKey, TValue>`)
- HashSet'ler (`HashSet<T>`)
- Kuyruklar (`Queue<T>`)
- Yığınlar (`Stack<T>`)
- `IEnumerable` veya `IEnumerable<T>` arayüzünü uygulayan herhangi bir koleksiyon

## foreach Döngüsü Örnekleri

### Örnek 1: Dizide dolaşma

```csharp
string[] meyveler = { "Elma", "Armut", "Muz", "Portakal", "Çilek" };

foreach (string meyve in meyveler)
{
    Console.WriteLine(meyve);
}
```

### Örnek 2: Liste üzerinde dolaşma

```csharp
List<int> sayilar = new List<int> { 1, 2, 3, 4, 5 };

foreach (int sayi in sayilar)
{
    Console.WriteLine($"Sayının karesi: {sayi * sayi}");
}
```

### Örnek 3: Sözlük üzerinde dolaşma

```csharp
Dictionary<string, int> yaslar = new Dictionary<string, int>
{
    { "Ali", 25 },
    { "Ayşe", 32 },
    { "Mehmet", 41 }
};

foreach (KeyValuePair<string, int> kisi in yaslar)
{
    Console.WriteLine($"{kisi.Key} {kisi.Value} yaşındadır.");
}
```

### Örnek 4: Karakter dizisi üzerinde dolaşma

```csharp
string metin = "Merhaba Dünya";

foreach (char karakter in metin)
{
    Console.WriteLine(karakter);
}
```

## foreach vs for Karşılaştırması

| foreach                                      | for                                           |
|----------------------------------------------|-----------------------------------------------|
| Koleksiyonun tamamında dolaşmak için idealdir | Belirli indekslere erişmek istediğinizde kullanışlıdır |
| Sözdizimi daha kısa ve okunabilirdir         | Daha fazla kod gerektirir                      |
| İndeks değişkeni yönetimi gerekmez           | İndeks değişkeni yönetimi gerekir              |
| Koleksiyonu değiştiremezsiniz                | Döngü içinde koleksiyonu değiştirebilirsiniz   |
| Sadece ileri yönde dolaşabilirsiniz          | İleri, geri veya belirli adımlarla dolaşabilirsiniz |

### for ile Aynı İşlemi Yapma Örneği:

```csharp
// foreach kullanımı
foreach (string meyve in meyveler)
{
    Console.WriteLine(meyve);
}

// Aynı işlemin for ile yapılması
for (int i = 0; i < meyveler.Length; i++)
{
    Console.WriteLine(meyveler[i]);
}
```

## foreach Kullanırken Dikkat Edilmesi Gerekenler

### 1. Koleksiyonu Değiştirme Kısıtlaması

`foreach` döngüsü içinde üzerinde dolaştığınız koleksiyonu değiştiremezsiniz. Bunu yapmaya çalışırsanız, `InvalidOperationException` hatası alırsınız.

```csharp
// Bu kod hata verecektir
List<int> sayilar = new List<int> { 1, 2, 3, 4, 5 };
foreach (int sayi in sayilar)
{
    if (sayi == 3)
    {
        sayilar.Remove(sayi); // Hata: "Collection was modified"
    }
}
```

### 2. Doğru Çözüm: Koleksiyonun Bir Kopyasını Kullanma

```csharp
// Doğru yaklaşım
List<int> sayilar = new List<int> { 1, 2, 3, 4, 5 };
List<int> silinecekler = new List<int>();

foreach (int sayi in sayilar)
{
    if (sayi == 3)
    {
        silinecekler.Add(sayi);
    }
}

foreach (int silinecek in silinecekler)
{
    sayilar.Remove(silinecek);
}
```

### 3. İterasyon Değişkenini Değiştirme

`foreach` döngüsünde iterasyon değişkeni salt okunurdur, değiştirilemez:

```csharp
// Bu kod derleme hatası verir
foreach (int sayi in sayilar)
{
    sayi = sayi * 2; // Hata: İterasyon değişkeni değiştirilemez
}
```

### 4. Referans Tipi Nesnelerin İçeriğini Değiştirme

Referans tipi nesneler için, nesnenin kendisini değiştiremezsiniz, ancak nesnenin özelliklerini değiştirebilirsiniz:

```csharp
List<Ogrenci> ogrenciler = new List<Ogrenci>();
// ... öğrencileri ekle

foreach (Ogrenci ogrenci in ogrenciler)
{
    // ogrenci = new Ogrenci(); // Hata!
    ogrenci.Puan += 10; // Bu geçerli, nesnenin özelliğini değiştiriyoruz
}
```

## IEnumerable ve foreach İlişkisi

`foreach` döngüsü, `IEnumerable` veya `IEnumerable<T>` arayüzünü uygulayan herhangi bir koleksiyon ile çalışabilir. Bu arayüzler, bir koleksiyonun elemanlarını sırayla dolaşabilme yeteneği sağlar.

```csharp
public interface IEnumerable
{
    IEnumerator GetEnumerator();
}

public interface IEnumerable<out T> : IEnumerable
{
    new IEnumerator<T> GetEnumerator();
}
```

Kendi koleksiyonunuzun `foreach` ile kullanılabilmesi için bu arayüzleri uygulamalısınız:

```csharp
public class BenimKoleksiyonum<T> : IEnumerable<T>
{
    private List<T> _items = new List<T>();

    public IEnumerator<T> GetEnumerator()
    {
        return _items.GetEnumerator();
    }

    IEnumerator IEnumerable.GetEnumerator()
    {
        return GetEnumerator();
    }

    public void Add(T item)
    {
        _items.Add(item);
    }
}
```

Şimdi bu koleksiyon `foreach` ile kullanılabilir:

```csharp
BenimKoleksiyonum<string> koleksiyon = new BenimKoleksiyonum<string>();
koleksiyon.Add("Bir");
koleksiyon.Add("İki");
koleksiyon.Add("Üç");

foreach (string eleman in koleksiyon)
{
    Console.WriteLine(eleman);
}
```

## LINQ ile foreach Kullanımı

LINQ (Language Integrated Query) sorguları ile `foreach` kullanımı çok güçlü bir kombinasyondur:

```csharp
// LINQ sorgusunu foreach ile kullanma
List<int> sayilar = new List<int> { 1, 2, 3, 4, 5, 6, 7, 8, 9, 10 };

foreach (int sayi in sayilar.Where(x => x % 2 == 0))
{
    Console.WriteLine($"Çift sayı: {sayi}");
}
```

### Lambda İfadeleri ile foreach Döngüsünün Alternatifi

C# 10 ve sonrasında, koleksiyonların üzerinde dolaşmak için `ForEach` metodu da kullanabilirsiniz:

```csharp
// List<T> sınıfının ForEach metodu
List<string> isimler = new List<string> { "Ali", "Veli", "Ayşe" };
isimler.ForEach(isim => Console.WriteLine(isim));

// LINQ ile foreach yerine kullanım
sayilar.Where(x => x > 5).ToList().ForEach(sayi => Console.WriteLine(sayi));
```

## Özet

- `foreach` döngüsü, koleksiyonlarda dolaşmak için özel olarak tasarlanmış, okunabilir ve kolay kullanımlı bir C# yapısıdır.
- Diziler, listeler, sözlükler ve `IEnumerable` arayüzünü uygulayan tüm koleksiyonlarla çalışır.
- Döngü içinde koleksiyonu değiştiremezsiniz ve iterasyon değişkeni salt okunurdur.
- LINQ sorguları ile birlikte kullanıldığında veri işleme gücünü artırır.
- Kendi koleksiyonlarınızın `foreach` ile kullanılabilmesi için `IEnumerable` arayüzünü uygulamalısınız.
- Birçok durumda geleneksel `for` döngüsüne göre daha temiz ve anlaşılır kod sağlar.

`foreach` döngüsü, modern C# uygulamalarında koleksiyonları işlemek için tercih edilen yöntemdir ve kodunuzu daha okunabilir hale getirmenize yardımcı olur.
