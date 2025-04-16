# C# Alanlar (Fields) ve Özellikler (Properties)

## İçindekiler
1. [Giriş](#giriş)
2. [Alanlar (Fields)](#alanlar-fields)
   - [Alan Tanımlama](#alan-tanımlama)
   - [Erişim Belirleyiciler](#erişim-belirleyiciler)
   - [Readonly Alanlar](#readonly-alanlar)
   - [Static Alanlar](#static-alanlar)
   - [Const Alanlar](#const-alanlar)
3. [Özellikler (Properties)](#özellikler-properties)
   - [Temel Özellik Yapısı](#temel-özellik-yapısı)
   - [Auto-Implemented Properties](#auto-implemented-properties)
   - [ReadOnly ve WriteOnly Özellikler](#readonly-ve-writeonly-özellikler)
   - [Expression-Bodied Properties](#expression-bodied-properties)
   - [Init-Only Properties](#init-only-properties)
4. [Alanlar ve Özellikler Arasındaki Farklar](#alanlar-ve-özellikler-arasındaki-farklar)
5. [En İyi Uygulama Örnekleri](#en-iyi-uygulama-örnekleri)
6. [İleri Düzey Konular](#i̇leri-düzey-konular)
   - [Backing Fields](#backing-fields)
   - [Property Patterns](#property-patterns)
   - [Indexers](#indexers)
7. [Sonuç](#sonuç)

## Giriş

C# programlama dilinde, veri yönetimi ve erişimi için iki temel yapı kullanılır: alanlar (fields) ve özellikler (properties). Bu iki kavram, nesne yönelimli programlamanın temel taşlarından olan kapsülleme (encapsulation) prensibini uygulamak için kritik öneme sahiptir.

## Alanlar (Fields)

Alanlar, bir sınıf veya yapı içinde veri depolamak için kullanılan değişkenlerdir. Doğrudan sınıf veya yapı içinde tanımlanırlar ve sınıfın durumunu (state) temsil ederler.

### Alan Tanımlama

Bir alan, veri tipi ve isim belirtilerek tanımlanır:

```csharp
public class Person
{
    // Field tanımlaması
    private string name;
    private int age;
    public double height;
}
```

### Erişim Belirleyiciler

Alanlar farklı erişim belirleyicileri (access modifiers) ile tanımlanabilirler:

- **private**: Sadece tanımlandığı sınıf içinden erişilebilir.
- **public**: Herhangi bir yerden erişilebilir.
- **protected**: Sadece tanımlandığı sınıf ve bu sınıftan türetilen sınıflardan erişilebilir.
- **internal**: Aynı assembly içindeki kodlardan erişilebilir.
- **protected internal**: Aynı assembly içindeki kodlardan veya türetilen sınıflardan erişilebilir.
- **private protected**: Aynı assembly içindeki türetilen sınıflardan erişilebilir.

```csharp
public class Person
{
    private string name;           // Sadece Person sınıfı içinden erişilebilir
    protected int age;             // Person ve türetilmiş sınıflardan erişilebilir
    public double height;          // Herhangi bir yerden erişilebilir
    internal string address;       // Aynı assembly içinden erişilebilir
    protected internal bool isActive;    // Türetilen sınıflar veya aynı assembly içinden
    private protected DateTime birthDate; // Aynı assembly içindeki türetilen sınıflardan
}
```

### Readonly Alanlar

`readonly` anahtar kelimesi ile tanımlanan alanlar, sadece tanımlandıkları anda veya constructor içinde değer alabilirler, daha sonra değiştirilemezler:

```csharp
public class Person
{
    private readonly string id;
    
    public Person(string personId)
    {
        id = personId; // Constructor içinde değer atanabilir
    }
    
    public void ChangeId(string newId)
    {
        // id = newId; // Hata! readonly alan sadece constructor içinde değiştirilebilir
    }
}
```

### Static Alanlar

`static` anahtar kelimesi ile tanımlanan alanlar, sınıf düzeyinde olup, tüm sınıf örnekleri (instance) için ortak bir değer taşırlar:

```csharp
public class Counter
{
    private static int count = 0;
    
    public Counter()
    {
        count++;
    }
    
    public static int GetCount()
    {
        return count;
    }
}
```

```csharp
// Kullanımı:
Counter c1 = new Counter(); // count = 1
Counter c2 = new Counter(); // count = 2
int totalCount = Counter.GetCount(); // 2
```

### Const Alanlar

`const` anahtar kelimesi ile tanımlanan alanlar, derleme zamanında (compile-time) değeri bilinen ve hiçbir zaman değişmeyen sabitleri tanımlar:

```csharp
public class MathConstants
{
    public const double PI = 3.14159265359;
    public const int DAYS_IN_WEEK = 7;
}
```

Const alanlar otomatik olarak statiktir ve tanımlandıkları anda değer ataması yapılmalıdır.

## Özellikler (Properties)

Özellikler, alanların değerlerine kontrollü erişim sağlayan C# dil özelliğidir. Bir özellik, bir alana erişimi düzenleyen ve veri doğrulaması, işleme veya bildirim yapılmasına imkan tanıyan bir yöntemdir.

### Temel Özellik Yapısı

Bir özellik, `get` ve `set` blokları ile tanımlanır:

```csharp
public class Person
{
    private string name; // Backing field
    
    public string Name
    {
        get { return name; }
        set { name = value; } // value anahtar kelimesi atanan değeri temsil eder
    }
}
```

Özellikler, kontrollü erişim sağlayarak veri doğrulaması yapmak için idealdir:

```csharp
public class Person
{
    private int age;
    
    public int Age
    {
        get { return age; }
        set 
        { 
            if (value >= 0 && value <= 120)
                age = value;
            else
                throw new ArgumentOutOfRangeException("Yaş 0-120 arasında olmalıdır.");
        }
    }
}
```

### Auto-Implemented Properties

C# 3.0 ve sonrasında, arka planda otomatik olarak bir alan oluşturan daha kısa bir özellik sözdizimi sunulmuştur:

```csharp
public class Person
{
    // Auto-implemented property
    public string Name { get; set; }
    
    // Başlangıç değeri ile
    public int Age { get; set; } = 18;
}
```

Bu durumda, compiler otomatik olarak `name` ve `age` adında gizli (private) alanlar oluşturur ve get/set erişimcileri bu alanlara yönlendirir.

### ReadOnly ve WriteOnly Özellikler

Sadece `get` veya sadece `set` blokları olan özellikler tanımlanabilir:

```csharp
public class Person
{
    private string name;
    
    // Read-only property
    public string Name
    {
        get { return name; }
    }
    
    // Write-only property
    public string SecretCode
    {
        set { /* Kodla bir şeyler yap */ }
    }
    
    public Person(string initialName)
    {
        name = initialName;
    }
}
```

Auto-implemented read-only property de tanımlanabilir:

```csharp
public class Person
{
    // Auto-implemented read-only property
    public string ID { get; } = Guid.NewGuid().ToString();
    
    public Person() { }
    
    public Person(string id)
    {
        ID = id; // Constructor içinde değer atanabilir
    }
}
```

### Expression-Bodied Properties

C# 6.0 ve sonrasında, lambda ifadeleriyle kısa özellik tanımlama imkanı vardır:

```csharp
public class Person
{
    public string FirstName { get; set; }
    public string LastName { get; set; }
    
    // Expression-bodied read-only property
    public string FullName => $"{FirstName} {LastName}";
}
```

### Init-Only Properties

C# 9.0 ile gelen `init` anahtar kelimesi, nesne oluşturma (object initialization) sırasında değer atamaya izin veren ancak daha sonra değiştirilemeyen özellikler tanımlamaya olanak tanır:

```csharp
public class Person
{
    // Init-only property
    public string Name { get; init; }
    public DateTime BirthDate { get; init; }
    
    // Hesaplanmış property
    public int Age => (DateTime.Now - BirthDate).Days / 365;
}
```

```csharp
// Kullanımı:
var person = new Person { 
    Name = "Ahmet", 
    BirthDate = new DateTime(1990, 1, 1) 
};

// person.Name = "Mehmet"; // Hata! init-only property sonradan değiştirilemez
```

## Alanlar ve Özellikler Arasındaki Farklar

| Özellik | Alanlar (Fields) | Özellikler (Properties) |
|---------|-----------------|------------------------|
| Amaç | Veri depolamak | Verilere kontrollü erişim sağlamak |
| Erişim kontrolü | Basit erişim belirleyiciler | get/set bloklarıyla detaylı kontrol |
| Veri doğrulama | Yapılamaz | get/set bloklarında yapılabilir |
| Sanal olma | Sanal (virtual) olamaz | Sanal olabilir ve geçersiz kılınabilir (override) |
| İşlevsellik | Sadece değer depolar | İşlem yapabilir, hesaplama içerebilir |
| Arayüzlerde kullanım | Arayüzlerde tanımlanamaz | Arayüzlerde tanımlanabilir |
| Serileştirme | Erişilebilir olduğunda serileştirilebilir | Public olduğunda serileştirilebilir |

## En İyi Uygulama Örnekleri

### 1. Alanları Gizli (Private) Tutma

Kapsülleme prensibine uygun olarak, genellikle alanlar `private` olarak tanımlanmalı ve erişim özellikler üzerinden sağlanmalıdır:

```csharp
public class Customer
{
    private string email; // Alan gizli tutulur
    
    public string Email // Erişim özellik üzerinden
    {
        get { return email; }
        set 
        {
            if (IsValidEmail(value))
                email = value;
            else
                throw new ArgumentException("Geçersiz e-posta adresi.");
        }
    }
    
    private bool IsValidEmail(string email)
    {
        // E-posta doğrulama mantığı...
        return email.Contains("@") && email.Contains(".");
    }
}
```

### 2. Auto-Implemented Properties Kullanımı

Eğer özel bir işlem veya doğrulama gerekmiyorsa, kod basitliği için auto-implemented properties tercih edilmelidir:

```csharp
public class Product
{
    public string Name { get; set; }
    public decimal Price { get; set; }
    public int Stock { get; set; }
}
```

### 3. Hesaplanmış Özellikler

Diğer verilerden hesaplanabilen değerler için read-only hesaplanmış özellikler kullanın:

```csharp
public class Rectangle
{
    public double Width { get; set; }
    public double Height { get; set; }
    
    // Hesaplanmış özellik
    public double Area => Width * Height;
    public double Perimeter => 2 * (Width + Height);
}
```

### 4. İmmutable Nesneler Oluşturma

İmmutable (değiştirilemez) nesneler için, init-only properties ve readonly fields kullanın:

```csharp
public class ImmutablePerson
{
    // Readonly alan
    private readonly DateTime birthDate;
    
    // Init-only property
    public string Name { get; init; }
    
    // Readonly property (backing field ile)
    public DateTime BirthDate 
    { 
        get { return birthDate; } 
    }
    
    // Hesaplanmış property
    public int Age => (DateTime.Now - BirthDate).Days / 365;
    
    public ImmutablePerson(string name, DateTime birthDate)
    {
        Name = name;
        this.birthDate = birthDate;
    }
}
```

## İleri Düzey Konular

### Backing Fields

Auto-implemented properties, backing field'ı otomatik oluşturur. Ancak bazı durumlarda özel bir backing field kullanmak gerekebilir:

```csharp
public class Temperature
{
    private double celsius; // Backing field
    
    public double Celsius
    {
        get { return celsius; }
        set { celsius = value; }
    }
    
    public double Fahrenheit
    {
        get { return celsius * 9 / 5 + 32; }
        set { celsius = (value - 32) * 5 / 9; }
    }
}
```

C# 7.0 ve sonrasında, backing field'a kolayca erişmek için `value` pattern kullanılabilir:

```csharp
public string Name
{
    get => name;
    set => name = value ?? throw new ArgumentNullException(nameof(Name));
}
```

### Property Patterns

C# 8.0 ve sonrasında, özellikler üzerinde pattern matching kullanılabilir:

```csharp
public class Person
{
    public string Name { get; set; }
    public int Age { get; set; }
}

// Kullanımı:
void CheckPerson(Person person)
{
    // Property pattern
    if (person is { Age: >= 18, Name: "Ahmet" })
    {
        Console.WriteLine("Kişi Ahmet ve yetişkin");
    }
}
```

### Indexers

Indexers, bir sınıfın dizi gibi indekslenebilmesini sağlayan özel özellik türleridir:

```csharp
public class WordCollection
{
    private Dictionary<int, string> words = new Dictionary<int, string>();
    
    // Indexer tanımlama
    public string this[int index]
    {
        get 
        { 
            return words.ContainsKey(index) ? words[index] : string.Empty; 
        }
        set { words[index] = value; }
    }
}

// Kullanımı:
var collection = new WordCollection();
collection[0] = "Merhaba";
collection[1] = "Dünya";
Console.WriteLine(collection[0]); // "Merhaba"
```

## Sonuç

C# dilinde alanlar (fields) ve özellikler (properties), nesne yönelimli programlamanın kapsülleme prensibini etkin bir şekilde uygulamaya yardımcı olan önemli yapı taşlarıdır. Alanlar, veriyi doğrudan depolamak için kullanılırken, özellikler bu verilere kontrollü erişim sağlar ve işlem mantığı ekleyebilir.

Modern C# uygulamalarında, genellikle doğrudan erişilebilir public alanlar yerine auto-implemented properties kullanılır. Bu, kapsülleme prensiplerine uyarken kodun okunabilirliğini ve bakımını kolaylaştırır. Ayrıca, gelecekte gerektiğinde doğrulama veya diğer işlemleri eklemek için propertyleri genişletmek daha kolaydır.

İyi bir C# programcısı, her bir duruma uygun şekilde alanları ve özellikleri etkin olarak kullanmayı bilmelidir. Veri gizliliğini korumak, veri bütünlüğünü sağlamak ve nesne davranışlarını doğru şekilde modellemek için bu yapıları doğru şekilde kullanmak önemlidir.
