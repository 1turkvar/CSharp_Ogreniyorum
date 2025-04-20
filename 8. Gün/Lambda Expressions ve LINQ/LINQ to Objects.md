# C# LINQ to Objects Kapsamlı Rehberi

## İçindekiler

1. [LINQ'a Giriş](#linq-giriş)
2. [LINQ Temel Operatörleri](#linq-temel-operatörleri)
3. [Sorgu Sözdizimi](#sorgu-sözdizimi)
4. [Filtreleme Operatörleri](#filtreleme-operatörleri)
5. [Sıralama Operatörleri](#sıralama-operatörleri)
6. [Projeksiyonlar](#projeksiyonlar)
7. [Gruplama ve Kümeleme](#gruplama-ve-kümeleme)
8. [Birleştirme İşlemleri](#birleştirme-işlemleri)
9. [Toplama Fonksiyonları](#toplama-fonksiyonları)
10. [Ertelenmiş ve Anında Yürütme](#ertelenmiş-ve-anında-yürütme)
11. [LINQ ile İleri Seviye Teknikler](#ileri-seviye-teknikler)
12. [Performans İpuçları](#performans-ipuçları)
13. [Örnek Uygulamalar](#örnek-uygulamalar)
14. [Sonuç](#sonuç)

<a id="linq-giriş"></a>
## 1. LINQ'a Giriş

LINQ (Language Integrated Query - Dile Entegre Sorgu), Microsoft tarafından .NET Framework 3.5 ile tanıtılan ve C# diline sorgu yetenekleri ekleyen bir teknolojidir. LINQ, farklı veri kaynakları üzerinde sorgu işlemleri yapmanızı sağlayan birleşik bir programlama modeli sunar.

LINQ to Objects, bellekte bulunan koleksiyonlar (diziler, listeler, sözlükler vb.) üzerinde LINQ sorgularını çalıştırmanıza olanak tanır. Bu, verileri filtreleme, sıralama, gruplama ve dönüştürme gibi işlemleri standart ve okunaklı bir şekilde gerçekleştirmenizi sağlar.

### LINQ'un Avantajları

- **Kod Okunabilirliği**: Sorgu işlemleri daha okunaklı ve anlaşılır hale gelir.
- **Tip Güvenliği**: Derleme zamanında tip kontrolü sağlar.
- **IntelliSense Desteği**: IDE içinde akıllı kod tamamlama özelliği sunar.
- **Standartlaştırma**: Farklı veri kaynakları için aynı sorgu sözdizimini kullanabilirsiniz.
- **Daha Az Kod**: Döngüler ve koşullu ifadeler için daha az kod yazmanızı sağlar.

### LINQ Kullanımı İçin Gerekli using Direktifleri

```csharp
using System;
using System.Collections.Generic;
using System.Linq;
```

<a id="linq-temel-operatörleri"></a>
## 2. LINQ Temel Operatörleri

LINQ, çeşitli operatörler sağlayarak veri işleme işlemlerini kolaylaştırır. Bu operatörler, uzantı metotları (extension methods) olarak uygulanmıştır. İşte en yaygın kullanılan LINQ operatörleri:

### Filtreleme
- `Where`: Belirli bir koşulu karşılayan öğeleri seçer.
- `OfType`: Belirli bir türdeki öğeleri filtreler.

### Sıralama
- `OrderBy`: Bir koleksiyonu belirtilen anahtara göre artan sırada sıralar.
- `OrderByDescending`: Bir koleksiyonu belirtilen anahtara göre azalan sırada sıralar.
- `ThenBy`: İkincil sıralama kriteri ekler.
- `ThenByDescending`: İkincil sıralama kriterini azalan şekilde ekler.
- `Reverse`: Koleksiyonun sırasını tersine çevirir.

### Projeksiyonlar
- `Select`: Her öğeyi belirtilen biçime dönüştürür.
- `SelectMany`: İç içe koleksiyonları düzleştirir.

### Toplama
- `Count / LongCount`: Eleman sayısını döndürür.
- `Sum`: Sayısal değerleri toplar.
- `Min / Max`: En küçük/büyük değeri bulur.
- `Average`: Ortalama değeri hesaplar.
- `Aggregate`: Özel toplama işlemleri için kullanılır.

### Öğe İşlemleri
- `First / FirstOrDefault`: İlk elemanı döndürür.
- `Last / LastOrDefault`: Son elemanı döndürür.
- `Single / SingleOrDefault`: Tek bir eleman döndürür.
- `ElementAt / ElementAtOrDefault`: Belirli bir indeksteki elemanı döndürür.
- `DefaultIfEmpty`: Koleksiyon boşsa varsayılan değer döndürür.

### Küme İşlemleri
- `Distinct`: Tekrarlanan öğeleri kaldırır.
- `Union`: İki koleksiyonun birleşimini döndürür.
- `Intersect`: İki koleksiyonun kesişimini döndürür.
- `Except`: İki koleksiyon arasındaki farkı döndürür.

### Sayfalama
- `Skip`: Belirtilen sayıda elemanı atlar.
- `Take`: Belirtilen sayıda elemanı alır.
- `SkipWhile`: Belirli bir koşul doğru olduğu sürece öğeleri atlar.
- `TakeWhile`: Belirli bir koşul doğru olduğu sürece öğeleri alır.

### Gruplama
- `GroupBy`: Öğeleri belirtilen anahtara göre gruplar.
- `ToLookup`: Anahtar-değer çiftleri oluşturur.

### Birleştirme
- `Join`: İki koleksiyonu birleştirir (iç birleştirme).
- `GroupJoin`: İki koleksiyonu gruplandırarak birleştirir.

### Dönüştürme
- `ToArray`: Sonucu bir diziye dönüştürür.
- `ToList`: Sonucu bir listeye dönüştürür.
- `ToDictionary`: Sonucu bir sözlüğe dönüştürür.
- `Cast`: Öğeleri belirtilen türe dönüştürür.

<a id="sorgu-sözdizimi"></a>
## 3. Sorgu Sözdizimi

LINQ sorgularını yazmanın iki farklı yolu vardır:

### 1. Metot Sözdizimi (Method Syntax)

Bu sözdizimi, C# metot zincirlemesini (method chaining) kullanır:

```csharp
var sonuc = koleksiyon.Where(x => x.Fiyat > 100)
                      .OrderBy(x => x.Isim)
                      .Select(x => new { x.Isim, x.Fiyat });
```

### 2. Sorgu Sözdizimi (Query Syntax)

Bu sözdizimi, SQL'e benzer bir sözdizimi kullanır:

```csharp
var sonuc = from x in koleksiyon
            where x.Fiyat > 100
            orderby x.Isim
            select new { x.Isim, x.Fiyat };
```

Her iki sözdizimi de aynı sonucu üretir ve hangi sözdizimini kullanacağınız genellikle kişisel tercihinize bağlıdır. Ancak bazı karmaşık sorgular için bir sözdizimi diğerinden daha uygun olabilir.

### Sorgu ve Metot Sözdizimlerinin Karşılaştırılması

| Sorgu Sözdizimi | Metot Sözdizimi |
|-----------------|-----------------|
| `from x in coll` | `coll` |
| `where x.Prop == value` | `.Where(x => x.Prop == value)` |
| `orderby x.Prop` | `.OrderBy(x => x.Prop)` |
| `orderby x.Prop descending` | `.OrderByDescending(x => x.Prop)` |
| `select x` | `.Select(x => x)` |
| `group x by x.Prop` | `.GroupBy(x => x.Prop)` |
| `join y in coll2 on x.Prop equals y.Prop` | `.Join(coll2, x => x.Prop, y => y.Prop, (x, y) => new { x, y })` |

<a id="filtreleme-operatörleri"></a>
## 4. Filtreleme Operatörleri

Filtreleme operatörleri, koleksiyonlardaki belirli öğeleri seçmek için kullanılır. En yaygın kullanılan filtreleme operatörü `Where`'dir.

### Where

```csharp
// Çift sayıları filtrele
int[] numbers = { 1, 2, 3, 4, 5, 6, 7, 8, 9, 10 };
var evenNumbers = numbers.Where(n => n % 2 == 0);
// Sonuç: 2, 4, 6, 8, 10

// Metot sözdizimi
var evenNumbers = numbers.Where(n => n % 2 == 0);

// Sorgu sözdizimi
var evenNumbers = from n in numbers
                 where n % 2 == 0
                 select n;
```

### OfType

Belirli bir türdeki öğeleri filtrelemek için kullanılır:

```csharp
object[] items = { "string", 1, 2.5, 3, "another string", 4.5 };
var integers = items.OfType<int>();
// Sonuç: 1, 3
```

### Çoklu Koşullu Filtreler

Birden fazla koşulu birleştirmek için mantıksal operatörler kullanabilirsiniz:

```csharp
var sonuc = numbers.Where(n => n % 2 == 0 && n > 5);
// Sonuç: 6, 8, 10

// VEYA koşulu
var sonuc = numbers.Where(n => n < 3 || n > 8);
// Sonuç: 1, 2, 9, 10
```

### First ve FirstOrDefault

Bir koleksiyondaki ilk elemanı seçmek için:

```csharp
var ilkCift = numbers.First(n => n % 2 == 0);
// Sonuç: 2

// Koşulu sağlayan eleman bulunamazsa FirstOrDefault varsayılan değeri döndürür
var ilkYuzdenuBuyuk = numbers.FirstOrDefault(n => n > 100);
// Sonuç: 0 (int türünün varsayılan değeri)
```

### Last ve LastOrDefault

Bir koleksiyondaki son elemanı seçmek için:

```csharp
var sonCift = numbers.Last(n => n % 2 == 0);
// Sonuç: 10

var sonIkiyuzdenBuyuk = numbers.LastOrDefault(n => n > 200);
// Sonuç: 0 (varsayılan değer)
```

### Single ve SingleOrDefault

Bir koleksiyonda tam olarak bir eleman olduğunu bildiğinizde:

```csharp
var tekBes = numbers.Single(n => n == 5);
// Sonuç: 5

// Koşula uyan birden fazla eleman varsa hata fırlatır
// var hata = numbers.Single(n => n % 2 == 0); // Hata

// Koşula uyan eleman yoksa varsayılan değeri döndürür
var olmayan = numbers.SingleOrDefault(n => n > 100);
// Sonuç: 0
```

### Any ve All

Koleksiyonda belirli koşulları kontrol etmek için:

```csharp
// Herhangi bir çift sayı var mı?
bool hasEven = numbers.Any(n => n % 2 == 0);
// Sonuç: true

// Tüm sayılar 0'dan büyük mü?
bool allPositive = numbers.All(n => n > 0);
// Sonuç: true
```

<a id="sıralama-operatörleri"></a>
## 5. Sıralama Operatörleri

Sıralama operatörleri, koleksiyonlardaki öğeleri belirli bir düzene göre sıralamak için kullanılır.

### OrderBy

Öğeleri artan sırada sıralamak için:

```csharp
string[] names = { "Tom", "Dick", "Harry", "Mary", "Jay" };
var orderedNames = names.OrderBy(n => n);
// Sonuç: Dick, Harry, Jay, Mary, Tom

// Sorgu sözdizimi
var orderedNames = from n in names
                  orderby n
                  select n;
```

### OrderByDescending

Öğeleri azalan sırada sıralamak için:

```csharp
var descendingNames = names.OrderByDescending(n => n);
// Sonuç: Tom, Mary, Jay, Harry, Dick

// Sorgu sözdizimi
var descendingNames = from n in names
                     orderby n descending
                     select n;
```

### ThenBy ve ThenByDescending

İkincil sıralama kriterleri eklemek için:

```csharp
var people = new[] {
    new { Name = "Alice", Age = 25 },
    new { Name = "Bob", Age = 30 },
    new { Name = "Charlie", Age = 25 },
    new { Name = "Dave", Age = 30 }
};

// Önce yaşa göre artan, sonra isme göre artan sıralama
var orderedPeople = people.OrderBy(p => p.Age).ThenBy(p => p.Name);
// Sonuç: Alice(25), Charlie(25), Bob(30), Dave(30)

// Sorgu sözdizimi
var orderedPeople = from p in people
                   orderby p.Age, p.Name
                   select p;

// Önce yaşa göre artan, sonra isme göre azalan sıralama
var mixedOrder = people.OrderBy(p => p.Age).ThenByDescending(p => p.Name);
// Sonuç: Charlie(25), Alice(25), Dave(30), Bob(30)

// Sorgu sözdizimi
var mixedOrder = from p in people
                orderby p.Age, p.Name descending
                select p;
```

### Reverse

Koleksiyonun sırasını tersine çevirmek için:

```csharp
var reversed = numbers.Reverse();
// Sonuç: 10, 9, 8, 7, 6, 5, 4, 3, 2, 1
```

### Özel Sıralama

Özel bir karşılaştırıcı (comparer) kullanarak sıralama yapabilirsiniz:

```csharp
// Metinleri uzunluklarına göre sıralama
var byLength = names.OrderBy(n => n.Length);
// Sonuç: Tom, Jay, Dick, Mary, Harry

// Büyük-küçük harf duyarsız sıralama
var caseInsensitive = names.OrderBy(n => n, StringComparer.OrdinalIgnoreCase);
```

<a id="projeksiyonlar"></a>
## 6. Projeksiyonlar

Projeksiyonlar, koleksiyondaki her öğeyi başka bir biçime dönüştürmek için kullanılır. Bu, genellikle `Select` ve `SelectMany` operatörleriyle gerçekleştirilir.

### Select

`Select` operatörü, bir koleksiyondaki her öğeyi başka bir biçime dönüştürür:

```csharp
int[] numbers = { 1, 2, 3, 4, 5 };

// Her sayının karesini al
var squares = numbers.Select(n => n * n);
// Sonuç: 1, 4, 9, 16, 25

// Sorgu sözdizimi
var squares = from n in numbers
              select n * n;

// Anonim tip oluşturma
var numInfo = numbers.Select(n => new { Number = n, Square = n * n, IsEven = n % 2 == 0 });
// Sonuç: { Number = 1, Square = 1, IsEven = false }, { Number = 2, Square = 4, IsEven = true }, ...

// Sorgu sözdizimi
var numInfo = from n in numbers
              select new { Number = n, Square = n * n, IsEven = n % 2 == 0 };
```

### SelectMany

`SelectMany` operatörü, iç içe koleksiyonları düzleştirmek için kullanılır:

```csharp
var teams = new[] {
    new { Name = "Team A", Members = new[] { "Alice", "Bob", "Charlie" } },
    new { Name = "Team B", Members = new[] { "Dave", "Eve", "Frank" } }
};

// Tüm takım üyelerini düz bir liste olarak al
var allMembers = teams.SelectMany(t => t.Members);
// Sonuç: "Alice", "Bob", "Charlie", "Dave", "Eve", "Frank"

// Sorgu sözdizimi
var allMembers = from team in teams
                 from member in team.Members
                 select member;

// Hem takım adını hem de üye adını içeren projeksiyonlar
var teamMembers = teams.SelectMany(
    t => t.Members, 
    (team, member) => new { TeamName = team.Name, MemberName = member }
);
// Sonuç: { TeamName = "Team A", MemberName = "Alice" }, { TeamName = "Team A", MemberName = "Bob" }, ...

// Sorgu sözdizimi
var teamMembers = from team in teams
                  from member in team.Members
                  select new { TeamName = team.Name, MemberName = member };
```

### Index Kullanımı

`Select` içinde indeks bilgisine erişmek için:

```csharp
var numbersWithIndex = numbers.Select((n, index) => new { Value = n, Index = index });
// Sonuç: { Value = 1, Index = 0 }, { Value = 2, Index = 1 }, ...
```

<a id="gruplama-ve-kümeleme"></a>
## 7. Gruplama ve Kümeleme

### GroupBy

`GroupBy` operatörü, bir koleksiyondaki öğeleri belirli bir anahtara göre gruplar:

```csharp
var people = new[] {
    new { Name = "Alice", Department = "HR" },
    new { Name = "Bob", Department = "IT" },
    new { Name = "Charlie", Department = "HR" },
    new { Name = "Dave", Department = "IT" },
    new { Name = "Eve", Department = "Finance" }
};

// Departmana göre gruplama
var groupsByDept = people.GroupBy(p => p.Department);

// Her grup için işlem yapma
foreach (var group in groupsByDept)
{
    Console.WriteLine($"Department: {group.Key}");
    foreach (var person in group)
    {
        Console.WriteLine($"  {person.Name}");
    }
}

// Sorgu sözdizimi
var groupsByDept = from p in people
                  group p by p.Department;
```

### Özel Grup Sonuçları

Gruplama sonucunda özel projeksiyonlar oluşturabilirsiniz:

```csharp
var deptSummary = people.GroupBy(
    p => p.Department,
    (key, group) => new { 
        Department = key, 
        Count = group.Count(), 
        Members = string.Join(", ", group.Select(p => p.Name)) 
    }
);
// Sonuç: { Department = "HR", Count = 2, Members = "Alice, Charlie" }, ...

// Sorgu sözdizimi
var deptSummary = from p in people
                 group p by p.Department into g
                 select new { 
                     Department = g.Key, 
                     Count = g.Count(), 
                     Members = string.Join(", ", g.Select(p => p.Name)) 
                 };
```

### ToLookup

`ToLookup` operatörü, `GroupBy`'a benzer ancak sonuçları hemen değerlendirir ve bir arama tablosu oluşturur:

```csharp
var deptLookup = people.ToLookup(p => p.Department);

// Belirli bir departmandaki kişileri bulmak için
var hrPeople = deptLookup["HR"];
// Sonuç: Alice, Charlie

// Olmayan bir anahtar sorgulandığında boş koleksiyon döndürür (hata fırlatmaz)
var marketingPeople = deptLookup["Marketing"];
// Sonuç: Boş koleksiyon
```

### Distinct

Benzersiz öğeleri almak için:

```csharp
int[] numbers = { 1, 2, 2, 3, 3, 3, 4, 4, 4, 4 };
var uniqueNumbers = numbers.Distinct();
// Sonuç: 1, 2, 3, 4
```

### Union, Intersect ve Except

Küme işlemleri için:

```csharp
int[] set1 = { 1, 2, 3, 4, 5 };
int[] set2 = { 3, 4, 5, 6, 7 };

// Birleşim: Her iki kümede bulunan benzersiz öğeler
var union = set1.Union(set2);
// Sonuç: 1, 2, 3, 4, 5, 6, 7

// Kesişim: Her iki kümede de bulunan öğeler
var intersection = set1.Intersect(set2);
// Sonuç: 3, 4, 5

// Fark: İlk kümede olup ikinci kümede olmayan öğeler
var difference = set1.Except(set2);
// Sonuç: 1, 2
```

<a id="birleştirme-işlemleri"></a>
## 8. Birleştirme İşlemleri

LINQ, farklı koleksiyonları birleştirmek için çeşitli operatörler sunar.

### Join (İç Birleştirme)

İki koleksiyonu ortak bir anahtar üzerinden birleştirmek için:

```csharp
var categories = new[] {
    new { ID = 1, Name = "Electronics" },
    new { ID = 2, Name = "Clothing" },
    new { ID = 3, Name = "Books" }
};

var products = new[] {
    new { ID = 101, Name = "Laptop", CategoryID = 1 },
    new { ID = 102, Name = "Mobile", CategoryID = 1 },
    new { ID = 103, Name = "T-Shirt", CategoryID = 2 },
    new { ID = 104, Name = "Jeans", CategoryID = 2 },
    new { ID = 105, Name = "Novel", CategoryID = 3 },
    new { ID = 106, Name = "Textbook", CategoryID = 3 },
    new { ID = 107, Name = "Accessory", CategoryID = 4 } // Eşleşmeyen kategori
};

// Kategorileri ve ürünleri birleştirme
var joinResult = categories.Join(
    products,
    category => category.ID,
    product => product.CategoryID,
    (category, product) => new { 
        ProductName = product.Name, 
        CategoryName = category.Name 
    }
);
// Sonuç: { ProductName = "Laptop", CategoryName = "Electronics" }, ...
// Not: CategoryID'si 4 olan ürün sonuçta gösterilmez (eşleşme yok)

// Sorgu sözdizimi
var joinResult = from c in categories
                join p in products
                on c.ID equals p.CategoryID
                select new { ProductName = p.Name, CategoryName = c.Name };
```

### GroupJoin

`GroupJoin`, bir koleksiyonu diğerine bağlar ve alt koleksiyonlar oluşturur:

```csharp
var categoryProducts = categories.GroupJoin(
    products,
    category => category.ID,
    product => product.CategoryID,
    (category, productGroup) => new {
        CategoryName = category.Name,
        Products = productGroup.Select(p => p.Name)
    }
);
// Sonuç: 
// { CategoryName = "Electronics", Products = ["Laptop", "Mobile"] }
// { CategoryName = "Clothing", Products = ["T-Shirt", "Jeans"] }
// { CategoryName = "Books", Products = ["Novel", "Textbook"] }

// Sorgu sözdizimi
var categoryProducts = from c in categories
                      join p in products
                      on c.ID equals p.CategoryID into productGroup
                      select new {
                          CategoryName = c.Name,
                          Products = productGroup.Select(p => p.Name)
                      };
```

### Sol Dış Birleştirme (Left Outer Join)

LINQ'da doğrudan bir sol dış birleştirme operatörü yoktur, ancak `GroupJoin` ve `SelectMany` kullanarak gerçekleştirilebilir:

```csharp
var leftJoin = categories.GroupJoin(
    products,
    category => category.ID,
    product => product.CategoryID,
    (category, productGroup) => new {
        category,
        productGroup
    })
    .SelectMany(
        x => x.productGroup.DefaultIfEmpty(),
        (x, product) => new {
            CategoryName = x.category.Name,
            ProductName = product == null ? "(No products)" : product.Name
        }
    );

// Sorgu sözdizimi
var leftJoin = from c in categories
              join p in products
              on c.ID equals p.CategoryID into productGroup
              from p in productGroup.DefaultIfEmpty()
              select new {
                  CategoryName = c.Name,
                  ProductName = p == null ? "(No products)" : p.Name
              };
```

### Çapraz Birleştirme (Cross Join)

İki koleksiyonun kartezyen çarpımını almak için:

```csharp
var crossJoin = categories.SelectMany(
    c => products,
    (c, p) => new { Category = c.Name, Product = p.Name }
);

// Sorgu sözdizimi
var crossJoin = from c in categories
               from p in products
               select new { Category = c.Name, Product = p.Name };
```

<a id="toplama-fonksiyonları"></a>
## 9. Toplama Fonksiyonları

LINQ, veri kümeleri üzerinde çeşitli toplama işlemleri yapmanıza olanak tanır.

### Count

Öğe sayısını hesaplamak için:

```csharp
int[] numbers = { 1, 2, 3, 4, 5, 6, 7, 8, 9, 10 };

// Toplam öğe sayısı
int totalCount = numbers.Count();
// Sonuç: 10

// Belirli bir koşulu karşılayan öğelerin sayısı
int evenCount = numbers.Count(n => n % 2 == 0);
// Sonuç: 5
```

### Sum

Sayısal değerleri toplamak için:

```csharp
// Tüm sayıların toplamı
int sum = numbers.Sum();
// Sonuç: 55

// Çift sayıların toplamı
int evenSum = numbers.Where(n => n % 2 == 0).Sum();
// Sonuç: 30

// Özel bir hesaplama sonucunu toplamak için
var people = new[] {
    new { Name = "Alice", Salary = 50000 },
    new { Name = "Bob", Salary = 60000 },
    new { Name = "Charlie", Salary = 55000 }
};
decimal totalSalary = people.Sum(p => p.Salary);
// Sonuç: 165000
```

### Min ve Max

En küçük ve en büyük değerleri bulmak için:

```csharp
// En küçük değer
int min = numbers.Min();
// Sonuç: 1

// En büyük değer
int max = numbers.Max();
// Sonuç: 10

// Özel bir özelliğe göre minimum ve maksimum
var shortestName = people.Min(p => p.Name.Length);
var longestName = people.Max(p => p.Name.Length);

// Özelliğe göre nesneyi bulmak için
var lowestPaidPerson = people.OrderBy(p => p.Salary).First();
var highestPaidPerson = people.OrderByDescending(p => p.Salary).First();
```

### Average

Ortalama değeri hesaplamak için:

```csharp
double average = numbers.Average();
// Sonuç: 5.5

// Özel bir özelliğin ortalaması
double avgSalary = people.Average(p => p.Salary);
// Sonuç: 55000
```

### Aggregate

Özel toplama işlemleri yapmak için:

```csharp
// Tüm sayıların çarpımı
int product = numbers.Aggregate((result, next) => result * next);
// Sonuç: 3628800 (10!)

// Başlangıç değeri ile aggregate
string commaSeparated = numbers.Aggregate(
    "Numbers: ", // seed (başlangıç değeri)
    (result, next) => result + next + ", "
);

# C# LINQ to Objects Kapsamlı Rehberi (Devam)

<a id="ertelenmiş-ve-anında-yürütme"></a>
## 10. Ertelenmiş ve Anında Yürütme

LINQ sorgularının değerlendirme biçimleri, sorguların performansını ve davranışını anlamak için önemli bir konudur.

### Ertelenmiş Yürütme (Deferred Execution)

LINQ sorguları genellikle **ertelenmiş yürütme** modelini kullanır. Bu, sorgunun tanımlandığı anda değil, sonuçlar gerçekten gerektiğinde yürütüldüğü anlamına gelir.

```csharp
// Sorgu tanımlanır ama henüz çalıştırılmaz
var query = numbers.Where(n => {
    Console.WriteLine($"Filtering {n}");
    return n % 2 == 0;
});

Console.WriteLine("Sorgu tanımlandı, ancak henüz çalıştırılmadı.");

// Sorgu değerleri itere edildiğinde çalıştırılır
foreach (var number in query)
{
    Console.WriteLine($"Sonuç: {number}");
}

// Sorgu tekrar çalıştırılır
Console.WriteLine("İkinci iterasyon:");
foreach (var number in query)
{
    Console.WriteLine($"Sonuç: {number}");
}
```

Bu örnekte, `Where` operatörü sorguyu tanımlar ancak çalıştırmaz. Sorgu, sonuçlar üzerinde döngü ile gezindiğimizde çalıştırılır. Ayrıca, her iterasyonda sorgu yeniden değerlendirilir.

### Anında Yürütme (Immediate Execution)

Bazı LINQ operatörleri sonuçları hemen değerlendirir:

```csharp
// Anında yürütülen operatörler
int count = numbers.Count();
int sum = numbers.Sum();
int first = numbers.First();
bool any = numbers.Any(n => n > 5);
List<int> list = numbers.Where(n => n % 2 == 0).ToList();
int[] array = numbers.Where(n => n % 2 == 0).ToArray();
```

Anında yürütülen başlıca operatörler:
- Toplama işlemleri: `Count`, `Sum`, `Min`, `Max`, `Average`, `Aggregate`
- Öğe işlemleri: `First`, `Last`, `Single`, `ElementAt` ve bunların `OrDefault` varyantları
- Dönüştürme işlemleri: `ToList`, `ToArray`, `ToDictionary`, `ToLookup`
- Boolean sonuç veren operatörler: `Any`, `All`, `Contains`

### Veri Kaynağı Değişikliklerinin Etkisi

Ertelenmiş yürütme nedeniyle, veri kaynağındaki değişiklikler sorgu sonuçlarını etkileyebilir:

```csharp
var numbers = new List<int> { 1, 2, 3, 4, 5 };
var query = numbers.Where(n => n % 2 == 0);

// İlk değerlendirme
foreach (var n in query)
{
    Console.WriteLine(n); // 2, 4
}

// Veri kaynağını değiştir
numbers.Add(6);
numbers.Add(7);
numbers.Add(8);

// İkinci değerlendirme - güncellenmiş veri kaynağını kullanır
foreach (var n in query)
{
    Console.WriteLine(n); // 2, 4, 6, 8
}
```

### En İyi Uygulama İpuçları

1. Ertelenmiş yürütmeyi ne zaman kullanmalı:
   - Büyük veri kümeleri için filtreleme ve projeksiyonlar yaparken
   - Sorguyu çeşitli koşullara göre dinamik olarak oluşturmanız gerektiğinde
   - Veri kaynağındaki güncellemeleri otomatik olarak yansıtmanız gerektiğinde

2. Anında yürütmeyi ne zaman kullanmalı:
   - Sonuçları hemen bir koleksiyona dönüştürmeniz gerektiğinde
   - Aynı sorguda birden çok kez yineleme yapacaksanız (performans için)
   - Veri kaynağındaki değişikliklerin sorguyu etkilememesi gerektiğinde
   - Sonuçları başka bir metoda geçirirken

<a id="ileri-seviye-teknikler"></a>
## 11. LINQ ile İleri Seviye Teknikler

### Özel Karşılaştırıcılar (Custom Comparers)

Özel karşılaştırma mantığı uygulamak için `IEqualityComparer<T>` veya `IComparer<T>` arayüzlerini kullanabilirsiniz:

```csharp
// Büyük-küçük harf duyarsız karşılaştırma örneği
public class CaseInsensitiveComparer : IEqualityComparer<string>
{
    public bool Equals(string x, string y)
    {
        return x?.ToLower() == y?.ToLower();
    }

    public int GetHashCode(string obj)
    {
        if (obj == null) return 0;
        return obj.ToLower().GetHashCode();
    }
}

// Kullanımı
string[] names = { "Alice", "bob", "Charlie", "BOB" };
var distinctNames = names.Distinct(new CaseInsensitiveComparer());
// Sonuç: "Alice", "bob", "Charlie" (ikinci "BOB" elendi)

// Yerleşik karşılaştırıcılar
var distinctNames = names.Distinct(StringComparer.OrdinalIgnoreCase);
```

### Sorgularda Yer Tutucular (Placeholders) ve Değişkenler

Sorgu sözdiziminde `let` ifadesi, ara değerleri depolamak için kullanılabilir:

```csharp
// Metot sözdizimi
var result = people
    .Select(p => new {
        p.Name,
        NameLength = p.Name.Length,
        UppercaseName = p.Name.ToUpper()
    })
    .Where(x => x.NameLength > 5)
    .OrderBy(x => x.UppercaseName);

// Sorgu sözdizimi - let ile
var result = from p in people
             let nameLength = p.Name.Length
             let uppercaseName = p.Name.ToUpper()
             where nameLength > 5
             orderby uppercaseName
             select new { p.Name, NameLength = nameLength, UppercaseName = uppercaseName };
```

### Zincirleme Sorgular

Karmaşık sorgular için alt sorguları zincirlemeye kullanabilirsiniz:

```csharp
// Departmanları ortalama maaşa göre sıralamak
var result = (from p in people
             group p by p.Department into deptGroup
             let avgSalary = deptGroup.Average(p => p.Salary)
             orderby avgSalary descending
             select new {
                 Department = deptGroup.Key,
                 AverageSalary = avgSalary,
                 HighestPaid = deptGroup.OrderByDescending(p => p.Salary).First().Name
             }).ToList();
```

### LINQ ile Fonksiyonel Programlama

LINQ, fonksiyonel programlama tekniklerini C# içinde kullanmanızı sağlar:

```csharp
// Sayı listesini 1'den 100'e kadar oluşturma
var numbers = Enumerable.Range(1, 100);

// Çift sayıları filtreleme, her birinin karesini alma, ilk 10'u seçme
var result = numbers
    .Where(n => n % 2 == 0)     // Filtre fonksiyonu
    .Select(n => n * n)         // Map fonksiyonu
    .Take(10);                  // İlk 10 elemanı seçme
```

### İç İçe Sorgular

Karmaşık veri yapıları için iç içe sorgulara örnek:

```csharp
var departments = new[] {
    new {
        Name = "IT",
        Teams = new[] {
            new {
                Name = "Development",
                Members = new[] { "Alice", "Bob", "Charlie" }
            },
            new {
                Name = "QA",
                Members = new[] { "Dave", "Eve" }
            }
        }
    },
    new {
        Name = "HR",
        Teams = new[] {
            new {
                Name = "Recruiting",
                Members = new[] { "Frank", "Grace" }
            }
        }
    }
};

// Tüm çalışanların düz listesini alma
var allEmployees = departments
    .SelectMany(d => d.Teams)
    .SelectMany(t => t.Members);

// Departman ve takım bilgilerini de içeren çalışan bilgisi
var employeeDetails = from dept in departments
                     from team in dept.Teams
                     from member in team.Members
                     select new {
                         Employee = member,
                         Team = team.Name,
                         Department = dept.Name
                     };
```

<a id="performans-ipuçları"></a>
## 12. Performans İpuçları

LINQ sorgularının performansını optimize etmek için bazı ipuçları:

### 1. Erken Filtreleme

Koleksiyonun boyutunu mümkün olduğunca erken küçültün:

```csharp
// İyi performans - önce filtrele, sonra dönüştür
var result = collection
    .Where(item => item.IsValid)     // Önce filtrele
    .Select(item => ExpensiveTransform(item));

// Daha kötü performans - tüm öğeleri dönüştür, sonra filtrele
var result = collection
    .Select(item => ExpensiveTransform(item))    // Önce tüm öğeleri dönüştür
    .Where(item => item.IsValid);                // Sonra filtrele
```

### 2. `ToList()` ve `ToArray()` Operatörlerini Dikkatli Kullanın

Gereksiz koleksiyon kopyaları oluşturmaktan kaçının:

```csharp
// Zincirlenmiş sorgu - her ToList() çağrısı yeni bir koleksiyon oluşturur
var result = collection
    .Where(x => x.IsValid).ToList()      // Gereksiz Liste
    .OrderBy(x => x.Name).ToList()       // Gereksiz Liste
    .Select(x => x.Value);

// Daha iyi yaklaşım - sonuçların son halini alın
var result = collection
    .Where(x => x.IsValid)
    .OrderBy(x => x.Name)
    .Select(x => x.Value)
    .ToList();  // Sadece bir kere dönüştürün
```

### 3. Uygun Veri Yapısını Seçin

Farklı senaryolar için optimize edilmiş veri yapıları kullanın:

```csharp
// Sıklıkla arama yapıyorsanız
var lookup = collection.ToLookup(item => item.Key);
var foundItems = lookup[searchKey];  // Hızlı arama

// Dictionary kullanımı
var dictionary = collection.ToDictionary(item => item.ID);
var item = dictionary[searchID];     // O(1) karmaşıklığıyla arama

// Küme işlemleri için HashSet
var set1 = new HashSet<int>(collection1);
var set2 = new HashSet<int>(collection2);
bool containsAll = set1.IsSubsetOf(set2);    // Etkili küme işlemleri
```

### 4. AsEnumerable() ve AsQueryable()

Sorgu sağlayıcılarını değiştirmek için:

```csharp
// Veritabanı sorgusu olarak yürütülmesini engellemek için
var localFiltering = dbContext.Employees
    .AsEnumerable()  // Sonraki işlemler bellek içinde yapılır
    .Where(e => ComplexLocalFunction(e));
```

### 5. Sayfalama İşlemleri

Büyük veri kümeleri için sayfalama kullanın:

```csharp
// Sayfa başına 10 öğe, 3. sayfa
int pageSize = 10;
int pageNumber = 3;
var pagedResult = collection
    .Skip((pageNumber - 1) * pageSize)
    .Take(pageSize);
```

### 6. Paralel LINQ (PLINQ) ile Performans Artırma

Paralel işleme ile sorguları hızlandırabilirsiniz:

```csharp
// Paralel LINQ sorgusu
var result = collection
    .AsParallel()   // Paralel işleme etkinleştir
    .Where(x => ExpensiveTest(x))
    .Select(x => ExpensiveTransform(x))
    .ToList();

// Paralel yürütme sırasını korumak için
var result = collection
    .AsParallel()
    .AsOrdered()    // Orijinal sırayı koru
    .Where(x => ExpensiveTest(x));

// Paralelliği özelleştirme
var result = collection
    .AsParallel()
    .WithDegreeOfParallelism(4)      // Paralel iş parçacığı sayısını sınırla
    .WithExecutionMode(ParallelExecutionMode.ForceParallelism)
    .Where(x => ExpensiveTest(x));
```

<a id="örnek-uygulamalar"></a>
## 13. Örnek Uygulamalar

Gerçek dünya senaryolarına dayalı LINQ uygulamaları:

### Örnek 1: Veri Analizi

```csharp
// Satış verilerinin analizi
public class Sale
{
    public string Product { get; set; }
    public string Category { get; set; }
    public double Amount { get; set; }
    public DateTime Date { get; set; }
    public string Region { get; set; }
}

// Örnek veri
var sales = new List<Sale>
{
    new Sale { Product = "Laptop", Category = "Electronics", Amount = 1200, Date = new DateTime(2023, 1, 15), Region = "North" },
    new Sale { Product = "Desktop", Category = "Electronics", Amount = 1500, Date = new DateTime(2023, 2, 10), Region = "South" },
    new Sale { Product = "Monitor", Category = "Electronics", Amount = 300, Date = new DateTime(2023, 1, 20), Region = "East" },
    new Sale { Product = "Keyboard", Category = "Accessories", Amount = 50, Date = new DateTime(2023, 3, 5), Region = "West" },
    new Sale { Product = "Mouse", Category = "Accessories", Amount = 25, Date = new DateTime(2023, 3, 10), Region = "North" },
    new Sale { Product = "Printer", Category = "Electronics", Amount = 200, Date = new DateTime(2023, 2, 20), Region = "South" },
    new Sale { Product = "Headphones", Category = "Accessories", Amount = 100, Date = new DateTime(2023, 1, 5), Region = "East" },
    // Daha fazla veri ekleyin...
};

// 1. Kategori bazında toplam satış tutarı
var salesByCategory = sales
    .GroupBy(s => s.Category)
    .Select(g => new { 
        Category = g.Key, 
        TotalAmount = g.Sum(s => s.Amount),
        Count = g.Count()
    })
    .OrderByDescending(x => x.TotalAmount);

// 2. Aylık satış analizi
var salesByMonth = sales
    .GroupBy(s => new { s.Date.Year, s.Date.Month })
    .Select(g => new {
        Year = g.Key.Year,
        Month = g.Key.Month,
        TotalAmount = g.Sum(s => s.Amount),
        AverageAmount = g.Average(s => s.Amount)
    })
    .OrderBy(x => x.Year).ThenBy(x => x.Month);

// 3. En çok satış yapılan bölgeler
var topRegions = sales
    .GroupBy(s => s.Region)
    .Select(g => new { 
        Region = g.Key, 
        TotalAmount = g.Sum(s => s.Amount) 
    })
    .OrderByDescending(x => x.TotalAmount)
    .Take(3);

// 4. Bölge ve kategori bazında çapraz analiz
var crossAnalysis = sales
    .GroupBy(s => new { s.Region, s.Category })
    .Select(g => new {
        Region = g.Key.Region,
        Category = g.Key.Category,
        TotalAmount = g.Sum(s => s.Amount)
    })
    .OrderBy(x => x.Region).ThenBy(x => x.Category);

// 5. 100 dolardan fazla olan satışlar
var highValueSales = sales
    .Where(s => s.Amount > 100)
    .OrderByDescending(s => s.Amount)
    .Select(s => new {
        Product = s.Product,
        Amount = s.Amount,
        Date = s.Date.ToShortDateString()
    });
```

### Örnek 2: Dosya Sistemi İşlemleri

```csharp
// Dosya sistemi analizi
using System.IO;

// Belirli bir klasördeki dosyaları analiz etme
string directoryPath = @"C:\Projects";
var files = new DirectoryInfo(directoryPath).GetFiles("*.*", SearchOption.AllDirectories);

// 1. Dosya uzantısına göre analiz
var analysisByExtension = files
    .GroupBy(f => f.Extension.ToLowerInvariant())
    .Select(g => new {
        Extension = string.IsNullOrEmpty(g.Key) ? "(no extension)" : g.Key,
        Count = g.Count(),
        TotalSize = g.Sum(f => f.Length),
        AverageSize = g.Average(f => f.Length)
    })
    .OrderByDescending(x => x.Count);

// 2. En büyük 10 dosya
var largestFiles = files
    .OrderByDescending(f => f.Length)
    .Take(10)
    .Select(f => new {
        FileName = f.Name,
        Directory = f.DirectoryName,
        SizeInMB = Math.Round((double)f.Length / (1024 * 1024), 2)
    });

// 3. Son bir hafta içinde değiştirilen dosyalar
var recentlyModified = files
    .Where(f => f.LastWriteTime > DateTime.Now.AddDays(-7))
    .OrderByDescending(f => f.LastWriteTime)
    .Select(f => new {
        FileName = f.Name,
        ModifiedDate = f.LastWriteTime.ToString("yyyy-MM-dd HH:mm:ss"),
        SizeInKB = Math.Round((double)f.Length / 1024, 2)
    });

// 4. Klasör bazında dosya sayısı ve boyutu
var analysisByFolder = files
    .GroupBy(f => f.DirectoryName)
    .Select(g => new {
        FolderPath = g.Key,
        FolderName = new DirectoryInfo(g.Key).Name,
        FileCount = g.Count(),
        TotalSizeInMB = Math.Round((double)g.Sum(f => f.Length) / (1024 * 1024), 2)
    })
    .OrderByDescending(x => x.FileCount);

// 5. Dosya adı desenine göre arama
string searchPattern = "report";
var matchingFiles = files
    .Where(f => f.Name.Contains(searchPattern, StringComparison.OrdinalIgnoreCase))
    .OrderBy(f => f.Name)
    .Select(f => new {
        FullPath = f.FullName,
        SizeInKB = Math.Round((double)f.Length / 1024, 2),
        CreationDate = f.CreationTime.ToString("yyyy-MM-dd")
    });
```

### Örnek 3: XML İşleme

```csharp
using System.Xml.Linq;

// XML belgesi
string xml = @"
<library>
  <books>
    <book id='1'>
      <title>C# Programming</title>
      <author>John Smith</author>
      <year>2021</year>
      <categories>
        <category>Programming</category>
        <category>Computer Science</category>
      </categories>
    </book>
    <book id='2'>
      <title>Database Design</title>
      <author>Jane Doe</author>
      <year>2020</year>
      <categories>
        <category>Database</category>
        <category>Computer Science</category>
      </categories>
    </book>
    <!-- More books... -->
  </books>
</library>";

XDocument doc = XDocument.Parse(xml);

// 1. Tüm kitap başlıklarını almak
var titles = doc.Descendants("book")
               .Select(b => (string)b.Element("title"));

// 2. 2020'den sonra yayınlanan kitaplar
var recentBooks = doc.Descendants("book")
                    .Where(b => (int)b.Element("year") > 2020)
                    .Select(b => new {
                        Title = (string)b.Element("title"),
                        Author = (string)b.Element("author"),
                        Year = (int)b.Element("year")
                    });

// 3. Belirli bir kategorideki kitaplar
var programmingBooks = doc.Descendants("book")
                         .Where(b => b.Descendants("category")
                                     .Any(c => (string)c == "Programming"))
                         .Select(b => (string)b.Element("title"));

// 4. Yazarlara göre kitap sayısı
var booksByAuthor = doc.Descendants("book")
                      .GroupBy(b => (string)b.Element("author"))
                      .Select(g => new {
                          Author = g.Key,
                          BookCount = g.Count(),
                          Books = g.Select(b => (string)b.Element("title"))
                      });

// 5. XML'den yeni bir XML oluşturma
var transformedXml = new XElement("authors",
    doc.Descendants("book")
       .GroupBy(b => (string)b.Element("author"))
       .Select(g => new XElement("author",
           new XAttribute("name", g.Key),
           new XElement("books",
               g.Select(b => new XElement("book",
                   new XAttribute("id", (string)b.Attribute("id")),
                   new XElement("title", (string)b.Element("title")),
                   new XElement("year", (string)b.Element("year"))
               ))
           )
       ))
);
```

<a id="sonuç"></a>
## 14. Sonuç

LINQ to Objects, C# ile veri işleme konusunda büyük kolaylıklar sağlayan güçlü bir teknolojidir. Bellek içi koleksiyonlar için sunduğu sorgu yetenekleri, kodunuzu daha kısa, daha okunaklı ve daha bakımı kolay hale getirmenize yardımcı olur.

Bu rehberde gördüğümüz gibi, LINQ çok çeşitli senaryolarda kullanılabilir:

1. **Veri Filtreleme ve Dönüştürme**: Koleksiyonlardan belirli öğeleri seçme, dönüştürme ve biçimlendirme.

2. **Veri Gruplama ve Özetleme**: Verileri kategorize etme, gruplama ve özet istatistikler çıkarma.

3. **Veri Birleştirme**: Farklı koleksiyonları ortak anahtarlara göre birleştirme.

4. **Toplama İşlemleri**: Sayısal verileri toplama, ortalama alma, minimum ve maksimum değerleri bulma.

5. **Sorgu Optimizasyonu**: Ertelenmiş yürütme ve diğer performans tekniklerini kullanarak sorguları optimize etme.

LINQ'un gerçek gücü, tüm bu işlemleri standart ve tutarlı bir API ile yapabilmesinden kaynaklanır. Bu sayede, farklı veri kaynakları için aynı sorgu yapısını kullanabilir ve veri işleme mantığınızı kodunuzun geri kalanından ayırabilirsiniz.

LINQ ile programlamanın en iyi yanlarından biri, fonksiyonel programlama paradigmasının sunduğu esneklik ve ifade gücünden yararlanırken, C#'ın tip güvenliği ve sağlam IDE desteğini de koruyabilmenizdir.

Veri işleme görevlerinizde LINQ kullanmaya başladıkça, kodunuzun daha kısa, daha anlaşılır ve daha bakımı kolay hale geldiğini göreceksiniz.

## Kaynaklar ve İleri Okuma

- [Microsoft Docs: LINQ (Language Integrated Query)](https://docs.microsoft.com/en-us/dotnet/csharp/programming-guide/concepts/linq/)
- [101 LINQ Örnekleri](https://docs.microsoft.com/en-us/samples/dotnet/try-samples/101-linq-samples/)
- "C# in Depth" - Jon Skeet
- "Pro LINQ: Language Integrated Query in C# 2010" - Joseph Rattz
- "LINQ Pocket Reference" - Joseph Albahari ve Ben Albahari
