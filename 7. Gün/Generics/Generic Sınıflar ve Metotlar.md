# C# Generic Sınıflar ve Metotlar

## İçindekiler
1. [Giriş](#giriş)
2. [Generic Kavramı](#generic-kavramı)
3. [Generic Sınıflar](#generic-sınıflar)
   - [Tanımlama ve Kullanım](#tanımlama-ve-kullanım)
   - [Birden Fazla Tip Parametresi](#birden-fazla-tip-parametresi)
   - [Varsayılan Değerler](#varsayılan-değerler)
   - [Kısıtlamalar (Constraints)](#kısıtlamalar-constraints)
4. [Generic Metotlar](#generic-metotlar)
   - [Tanımlama ve Kullanım](#tanımlama-ve-kullanım-1)
   - [Generic Sınıflarda Generic Metotlar](#generic-sınıflarda-generic-metotlar)
   - [Generic Metodlarda Kısıtlamalar](#generic-metodlarda-kısıtlamalar)
5. [Generic Arayüzler (Interfaces)](#generic-arayüzler-interfaces)
6. [Generic Delegeler](#generic-delegeler)
7. [Covariance ve Contravariance](#covariance-ve-contravariance)
   - [Covariance (out)](#covariance-out)
   - [Contravariance (in)](#contravariance-in)
8. [Generic Collection'lar](#generic-collectionlar)
9. [En İyi Uygulama Örnekleri](#en-i̇yi-uygulama-örnekleri)
10. [Performans Avantajları](#performans-avantajları)
11. [Özet](#özet)

## Giriş

C# programlama dilinde generic'ler, tip güvenliği sağlayan ve kod tekrarını önleyen güçlü bir özelliktir. Generic'ler sayesinde, farklı veri tipleriyle çalışabilen tek bir sınıf veya metot tanımlayabilirsiniz. Bu dokümanda C# generic sınıflar ve metotlar detaylı olarak incelenecektir.

## Generic Kavramı

Generic'ler, C# 2.0 ile birlikte dile eklenmiştir ve "tip parametreleri" kullanarak farklı veri tipleriyle çalışabilen kod yazmamızı sağlar. Generic'lerin temel amacı şunlardır:

- **Tip Güvenliği**: Derleme zamanında tip kontrolü yapılarak çalışma zamanı hatalarını azaltır.
- **Kod Tekrarını Önleme**: Farklı tipler için aynı işlevselliğe sahip kod yazmak yerine, tek bir generic yapı kullanılabilir.
- **Performans**: Boxing/unboxing işlemlerini önleyerek performans artışı sağlar.
- **Yeniden Kullanılabilirlik**: Aynı algoritmayı farklı veri tipleriyle kullanabilmeyi sağlar.

## Generic Sınıflar

### Tanımlama ve Kullanım

Generic bir sınıf, tip parametreleri alan ve bu parametrelere göre çalışan bir sınıftır. Tanımlanırken sınıf adından sonra `<T>` şeklinde belirtilir:

```csharp
public class Box<T>
{
    private T item;

    public Box(T item)
    {
        this.item = item;
    }

    public T GetItem()
    {
        return item;
    }

    public void SetItem(T item)
    {
        this.item = item;
    }
}
```

Bu sınıfı kullanmak için:

```csharp
Box<int> intBox = new Box<int>(123);
int number = intBox.GetItem(); // 123

Box<string> stringBox = new Box<string>("Merhaba");
string message = stringBox.GetItem(); // "Merhaba"
```

### Birden Fazla Tip Parametresi

Generic sınıflar birden fazla tip parametresi alabilirler:

```csharp
public class Pair<TFirst, TSecond>
{
    public TFirst First { get; set; }
    public TSecond Second { get; set; }

    public Pair(TFirst first, TSecond second)
    {
        First = first;
        Second = second;
    }
}

// Kullanımı
Pair<int, string> pair = new Pair<int, string>(1, "Bir");
Console.WriteLine($"{pair.First} - {pair.Second}"); // "1 - Bir"
```

### Varsayılan Değerler

Generic tip için varsayılan değer `default` anahtar kelimesi ile alınabilir:

```csharp
public class Container<T>
{
    private T value;
    
    public Container()
    {
        // T tipi için varsayılan değer
        value = default(T); 
        // C# 7.1 ve sonrasında sadece "default" olarak da yazılabilir
    }
}
```

### Kısıtlamalar (Constraints)

Generic tiplere belirli kısıtlamalar getirilebilir. Bu kısıtlamalar, tip parametrelerinin belirli özelliklere sahip olmasını garanti eder:

```csharp
// T tipi, IComparable<T> arayüzünü uygulamalı
public class SortableList<T> where T : IComparable<T>
{
    private List<T> items = new List<T>();
    
    public void Add(T item)
    {
        items.Add(item);
    }
    
    public void Sort()
    {
        items.Sort(); // IComparable<T> sayesinde sıralama yapılabilir
    }
}
```

Kullanılabilecek kısıtlama türleri:

- `where T : struct` - T bir değer tipi olmalı
- `where T : class` - T bir referans tipi olmalı
- `where T : new()` - T parametresiz bir yapıcı metoda sahip olmalı
- `where T : <base class>` - T belirtilen sınıftan türetilmiş olmalı
- `where T : <interface>` - T belirtilen arayüzü uygulamalı
- `where T : U` - T, U tipinden türetilmiş olmalı

## Generic Metotlar

### Tanımlama ve Kullanım

Generic metotlar, tip parametrelerini alan metotlardır ve generic olmayan sınıflarda da tanımlanabilirler:

```csharp
public class Utilities
{
    public static T Max<T>(T a, T b) where T : IComparable<T>
    {
        return a.CompareTo(b) > 0 ? a : b;
    }
}

// Kullanımı
int maxInt = Utilities.Max<int>(5, 10); // 10
string maxString = Utilities.Max<string>("abc", "xyz"); // "xyz"

// Tip parametrelerinin çıkarımı sayesinde tip belirtmeden de kullanılabilir
int maxInt2 = Utilities.Max(5, 10); // 10
```

### Generic Sınıflarda Generic Metotlar

Generic bir sınıf içinde, sınıfın tip parametrelerinden farklı olarak kendi tip parametrelerine sahip generic metotlar tanımlanabilir:

```csharp
public class DataProcessor<T>
{
    private T data;
    
    public DataProcessor(T data)
    {
        this.data = data;
    }
    
    // U, sınıfın T tipinden farklı bir tip parametresi
    public U Process<U>(Func<T, U> processor)
    {
        return processor(data);
    }
}

// Kullanımı
DataProcessor<string> processor = new DataProcessor<string>("Hello");
int length = processor.Process<int>(s => s.Length); // 5
```

### Generic Metodlarda Kısıtlamalar

Generic metodlarda da sınıflardaki gibi kısıtlamalar kullanılabilir:

```csharp
public class MathOperations
{
    public static T Add<T>(T a, T b) where T : INumber<T>
    {
        return a + b;
    }
    
    public static bool AreEqual<T>(T a, T b) where T : IEquatable<T>
    {
        return a.Equals(b);
    }
}
```

## Generic Arayüzler (Interfaces)

Generic arayüzler, tip parametreleri alan arayüzlerdir:

```csharp
public interface IRepository<T>
{
    void Add(T item);
    void Remove(T item);
    T GetById(int id);
    IEnumerable<T> GetAll();
}

// Uygulanması
public class CustomerRepository : IRepository<Customer>
{
    private List<Customer> customers = new List<Customer>();
    
    public void Add(Customer item)
    {
        customers.Add(item);
    }
    
    public void Remove(Customer item)
    {
        customers.Remove(item);
    }
    
    public Customer GetById(int id)
    {
        return customers.FirstOrDefault(c => c.Id == id);
    }
    
    public IEnumerable<Customer> GetAll()
    {
        return customers;
    }
}
```

## Generic Delegeler

C#'ta delegeler de generic olabilir:

```csharp
// Generic delegeler
public delegate TResult Transformer<T, TResult>(T input);

// Kullanımı
Transformer<string, int> stringToInt = s => int.Parse(s);
int result = stringToInt("123"); // 123

// Action ve Func generic delegeleri
Action<string> printString = s => Console.WriteLine(s);
Func<int, int, int> add = (a, b) => a + b;

printString("Merhaba"); // "Merhaba" yazdırır
int sum = add(5, 3); // 8
```

## Covariance ve Contravariance

C# 4.0 ile generic arayüzlere ve delegelere covariance ve contravariance desteği eklenmiştir.

### Covariance (out)

Covariance, daha spesifik bir tipin, daha genel bir tip yerine kullanılabilmesini sağlar. `out` anahtar kelimesi ile belirtilir:

```csharp
// Covariant generic arayüz
public interface IProducer<out T>
{
    T Produce();
}

// Kullanımı
public class AnimalProducer : IProducer<Animal>
{
    public Animal Produce()
    {
        return new Animal();
    }
}

public class DogProducer : IProducer<Dog>
{
    public Dog Produce()
    {
        return new Dog();
    }
}

// Dog, Animal'dan türediği için:
IProducer<Animal> producer = new DogProducer(); // Geçerli (covariance)
```

### Contravariance (in)

Contravariance, daha genel bir tipin, daha spesifik bir tip yerine kullanılabilmesini sağlar. `in` anahtar kelimesi ile belirtilir:

```csharp
// Contravariant generic arayüz
public interface IConsumer<in T>
{
    void Consume(T item);
}

// Kullanımı
public class AnimalConsumer : IConsumer<Animal>
{
    public void Consume(Animal animal)
    {
        // İşlemler...
    }
}

// Dog, Animal'dan türediği için:
IConsumer<Dog> dogConsumer = new AnimalConsumer(); // Geçerli (contravariance)
```

## Generic Collection'lar

.NET Framework, tip güvenli koleksiyonlar sağlamak için generic collection'lar sunar:

- `List<T>`: Dinamik dizi
- `Dictionary<TKey, TValue>`: Anahtar-değer çiftleri
- `HashSet<T>`: Benzersiz elemanlar içeren küme
- `Queue<T>`: İlk giren ilk çıkar (FIFO) kuyruk
- `Stack<T>`: Son giren ilk çıkar (LIFO) yığın
- `LinkedList<T>`: Çift yönlü bağlı liste

Örnek kullanım:

```csharp
// List<T>
List<string> names = new List<string> { "Ali", "Ayşe", "Mehmet" };
names.Add("Zeynep");
foreach (string name in names)
{
    Console.WriteLine(name);
}

// Dictionary<TKey, TValue>
Dictionary<string, int> ages = new Dictionary<string, int>
{
    { "Ali", 25 },
    { "Ayşe", 30 }
};
ages["Mehmet"] = 22;
Console.WriteLine(ages["Ali"]); // 25
```

## En İyi Uygulama Örnekleri

1. **İsimlendirme Kuralları**:
   - Tek tip parametresi için geleneksel olarak `T` kullanılır.
   - Birden fazla tip parametresi durumunda anlamlı isimler kullanın (örn. `TKey`, `TValue`).

2. **Yeterli Kısıtlama Ekleyin**:
   - Generic tiplerin gerektirdiği özellikleri kısıtlamalarla belirtin.
   - Böylece derleme zamanında daha iyi hata kontrolü sağlanır.

3. **Varsayılan Değerler**:
   - Generic tipler için varsayılan değer gerektiğinde `default(T)` kullanın.

4. **Belirli Metotlar için Generic'ler**:
   - Tüm sınıfı generic yapmak yerine, sadece gerekli metotları generic yapın.

## Performans Avantajları

Generic'ler, değer tipleri ile çalışırken özellikle boxing/unboxing işlemlerini önleyerek performans avantajı sağlar:

```csharp
// Generic olmayan koleksiyon
ArrayList list = new ArrayList();
list.Add(5); // int boxing'e uğrar (int → object)
int num = (int)list[0]; // unboxing gerekir (object → int)

// Generic koleksiyon
List<int> genericList = new List<int>();
genericList.Add(5); // boxing yok
int num2 = genericList[0]; // unboxing yok
```

## Özet

C# generic sınıflar ve metotlar, tip güvenli, yeniden kullanılabilir ve performanslı kod yazmanıza olanak sağlar. Bu özellik:

- Tek bir kod tabanıyla birden fazla veri tipini desteklemenizi sağlar
- Derleme zamanında tip güvenliği sunar
- Boxing/unboxing işlemlerini azaltarak performans artışı sağlar
- Kod tekrarını önler ve bakımı kolaylaştırır

Generic'ler, modern C# geliştirmesinin temel yapı taşlarından biridir ve .NET Framework'ün koleksiyon sınıfları, LINQ ve Entity Framework gibi birçok önemli teknolojinin altyapısında kullanılmaktadır.
