# C# Generic Kısıtlamalar (Constraints)

## İçindekiler
1. [Giriş](#giriş)
2. [Generic Kısıtlama Türleri](#generic-kısıtlama-türleri)
   - [where T : struct](#where-t--struct)
   - [where T : class](#where-t--class)
   - [where T : class?](#where-t--class-1)
   - [where T : notnull](#where-t--notnull)
   - [where T : unmanaged](#where-t--unmanaged)
   - [where T : new()](#where-t--new)
   - [where T : <temel sınıf>](#where-t--temel-sınıf)
   - [where T : <arayüz>](#where-t--arayüz)
   - [where T : U](#where-t--u)
3. [Birden Fazla Kısıtlama Kullanmak](#birden-fazla-kısıtlama-kullanmak)
4. [Generic Kısıtlamaların Avantajları](#generic-kısıtlamaların-avantajları)
5. [Gerçek Hayat Örnekleri](#gerçek-hayat-örnekleri)
6. [En İyi Uygulama Önerileri](#en-iyi-uygulama-önerileri)
7. [Sonuç](#sonuç)

## Giriş

Generic türler, C# dilinde tip güvenliğini korurken kod tekrarını azaltmanın güçlü bir yoludur. Bazen, generic parametrelerin belirli özelliklere sahip olmasını isteyebiliriz. Örneğin, bir karşılaştırma sınıfı yazarken, karşılaştırılacak nesnelerin `IComparable` arayüzünü uygulamasını isteyebiliriz, ya da bir nesne fabrikası tasarlarken, nesnelerin parametresiz bir yapıcıya sahip olmasını talep edebiliriz.

İşte burada generic kısıtlamalar (constraints) devreye girer. Kısıtlamalar, generic bir türün parametrelerine uygulayabileceğimiz sınırlamalardır. Bu sayede derleyici, kodu daha iyi kontrol edebilir ve kod çalıştırıldığında oluşabilecek hataları önceden tespit edebilir.

## Generic Kısıtlama Türleri

C# dilinde kullanabileceğimiz kısıtlama türleri şunlardır:

### where T : struct

Bu kısıtlama, T'nin bir değer türü olmasını zorunlu kılar. Örneğin int, double, bool veya yapılar (struct) bu kısıtlamayı karşılar.

```csharp
public class ValueTypeExample<T> where T : struct
{
    public T Value { get; set; }
    
    public void DisplayType()
    {
        Console.WriteLine($"T türü bir değer tipidir: {typeof(T).Name}");
    }
}

// Kullanım örneği
var intExample = new ValueTypeExample<int>();
var doubleExample = new ValueTypeExample<double>();
// var stringExample = new ValueTypeExample<string>(); // Derleme hatası - string bir değer tipi değildir
```

### where T : class

Bu kısıtlama, T'nin bir referans türü olmasını zorunlu kılar. Örneğin string, diziler veya sınıflar bu kısıtlamayı karşılar.

```csharp
public class ReferenceTypeExample<T> where T : class
{
    public T Value { get; set; }
    
    public bool IsNull()
    {
        return Value == null;
    }
}

// Kullanım örneği
var stringExample = new ReferenceTypeExample<string>();
var arrayExample = new ReferenceTypeExample<int[]>();
// var intExample = new ReferenceTypeExample<int>(); // Derleme hatası - int bir referans tipi değildir
```

### where T : class?

C# 8.0 ve sonrasında kullanılabilen bu kısıtlama, T'nin nullable referans türü olmasını sağlar.

```csharp
public class NullableReferenceTypeExample<T> where T : class?
{
    public T? Value { get; set; }
    
    public void ProcessValue(Action<T> processor)
    {
        if (Value != null)
        {
            processor(Value);
        }
    }
}
```

### where T : notnull

C# 8.0 ve sonrasında kullanılabilen bu kısıtlama, T'nin null olamayan bir tür olmasını zorunlu kılar. Hem değer tipleri hem de null olmayan referans tipleri bu kısıtlamayı karşılar.

```csharp
public class NotNullExample<T> where T : notnull
{
    public Dictionary<T, string> Mapping { get; } = new Dictionary<T, string>();
    
    public void AddMapping(T key, string value)
    {
        Mapping[key] = value;
    }
}
```

### where T : unmanaged

C# 7.3 ve sonrasında kullanılabilen bu kısıtlama, T'nin yönetilmeyen bir tür (unmanaged type) olmasını zorunlu kılar. Yönetilmeyen türler, referans tipleri içermeyen değer tipleridir. İlkel türler (int, bool, char vb.), yapılar ve enum türleri (sadece yönetilmeyen alan içeriyorlarsa) bu kısıtlamayı karşılar.

```csharp
public unsafe class UnmanagedExample<T> where T : unmanaged
{
    public void PrintSize()
    {
        Console.WriteLine($"T türünün boyutu: {sizeof(T)} byte");
    }
    
    public T* AllocateMemory(int count)
    {
        return (T*)Marshal.AllocHGlobal(sizeof(T) * count).ToPointer();
    }
    
    public void FreeMemory(T* pointer)
    {
        Marshal.FreeHGlobal(new IntPtr(pointer));
    }
}
```

### where T : new()

Bu kısıtlama, T'nin parametresiz bir yapıcıya (constructor) sahip olmasını zorunlu kılar. Bu kısıtlama, diğer kısıtlamalar kullanılıyorsa en sonda belirtilmelidir.

```csharp
public class FactoryExample<T> where T : new()
{
    public T CreateInstance()
    {
        return new T();
    }
    
    public List<T> CreateMultiple(int count)
    {
        var list = new List<T>(count);
        for (int i = 0; i < count; i++)
        {
            list.Add(new T());
        }
        return list;
    }
}

// Kullanım örneği
var stringFactory = new FactoryExample<string>();
var listFactory = new FactoryExample<List<int>>();
// var intArrayFactory = new FactoryExample<int[]>(); // Derleme hatası - int[] parametresiz yapıcıya sahip değildir
```

### where T : \<temel sınıf\>

Bu kısıtlama, T'nin belirtilen temel sınıftan türemiş olmasını veya temel sınıfın kendisi olmasını zorunlu kılar.

```csharp
public class Animal
{
    public virtual void MakeSound()
    {
        Console.WriteLine("Ses çıkarıyor...");
    }
}

public class Dog : Animal
{
    public override void MakeSound()
    {
        Console.WriteLine("Hav hav!");
    }
}

public class AnimalHandler<T> where T : Animal
{
    public void MakeAnimalSound(T animal)
    {
        animal.MakeSound();
    }
    
    public T CreateDefaultAnimal()
    {
        // T tipinin parametresiz yapıcıya sahip olduğunu bilmiyoruz,
        // bu nedenle direkt new T() kullanamayız
        return default; // Bu null dönebilir
    }
}

// Kullanım örneği
var dogHandler = new AnimalHandler<Dog>();
var animalHandler = new AnimalHandler<Animal>();
// var stringHandler = new AnimalHandler<string>(); // Derleme hatası - string, Animal'dan türememiştir
```

### where T : \<arayüz\>

Bu kısıtlama, T'nin belirtilen arayüzü uygulamasını zorunlu kılar.

```csharp
public class ComparerExample<T> where T : IComparable<T>
{
    public T FindMax(T[] items)
    {
        if (items == null || items.Length == 0)
        {
            throw new ArgumentException("Dizi boş olamaz.");
        }
        
        T max = items[0];
        for (int i = 1; i < items.Length; i++)
        {
            if (items[i].CompareTo(max) > 0)
            {
                max = items[i];
            }
        }
        
        return max;
    }
}

// Kullanım örneği
var intComparer = new ComparerExample<int>();
var maxInt = intComparer.FindMax(new[] { 1, 5, 3, 9, 2 }); // 9

var stringComparer = new ComparerExample<string>();
var maxString = stringComparer.FindMax(new[] { "a", "z", "c", "f" }); // "z"

// var objectComparer = new ComparerExample<object>(); // Derleme hatası - object, IComparable<object> uygulamaz
```

### where T : U

Bu kısıtlama, bir generic parametrenin (T) başka bir generic parametreden (U) türemiş olmasını veya o parametreyi uygulamasını zorunlu kılar.

```csharp
public class BaseConstraintExample<T, U> where T : U
{
    public void DemonstrateInheritance(T derivedValue, U baseValue)
    {
        Console.WriteLine($"T, U'dan türetilmiştir: {typeof(T).Name} -> {typeof(U).Name}");
        
        // T, U'dan türediği için, T tipindeki değer U tipine atanabilir
        U anotherBaseValue = derivedValue;
        Console.WriteLine("Türetilmiş değer, temel tipe dönüştürülebilir.");
    }
}

// Kullanım örneği
public class BaseClass { }
public class DerivedClass : BaseClass { }

var example = new BaseConstraintExample<DerivedClass, BaseClass>();
example.DemonstrateInheritance(new DerivedClass(), new BaseClass());

var anotherExample = new BaseConstraintExample<string, object>();
anotherExample.DemonstrateInheritance("Hello", new object());

// var invalidExample = new BaseConstraintExample<object, string>(); // Derleme hatası - object, string'den türememiştir
```

## Birden Fazla Kısıtlama Kullanmak

Bir generic parametre için birden fazla kısıtlama uygulayabiliriz. Bu durumda, generic parametre tüm kısıtlamaları karşılamalıdır.

```csharp
public interface IProcessor
{
    void Process();
}

public interface ILogger
{
    void Log(string message);
}

public class ProcessorLogger<T> where T : class, IProcessor, ILogger, new()
{
    public void ExecuteAndLog()
    {
        T instance = new T();
        instance.Log("İşlem başlıyor...");
        instance.Process();
        instance.Log("İşlem tamamlandı.");
    }
}

// Kullanım için bir sınıf tanımlayalım
public class TaskProcessor : IProcessor, ILogger
{
    public void Process()
    {
        Console.WriteLine("İşlem yürütülüyor...");
    }
    
    public void Log(string message)
    {
        Console.WriteLine($"LOG: {message}");
    }
}

// Kullanım örneği
var processor = new ProcessorLogger<TaskProcessor>();
processor.ExecuteAndLog();
```

## Generic Kısıtlamaların Avantajları

Generic kısıtlamaları kullanmanın birçok avantajı vardır:

1. **Tip Güvenliği**: Derleyici, kısıtlamalara uymayan tipleri reddeder, böylece çalışma zamanında oluşabilecek hataları önler.

2. **Kod Okunabilirliği**: Kısıtlamalar, generic tipin hangi özelliklere sahip olması gerektiğini belirterek kodun daha anlaşılır olmasını sağlar.

3. **Performans İyileştirmeleri**: JIT derleyici, kısıtlamaları kullanarak daha optimize edilmiş kod üretebilir. Örneğin, struct kısıtlaması, değer tiplerinin kutulama (boxing) işlemini önleyebilir.

4. **API Tasarımını İyileştirme**: Kısıtlamalar, API'nizin kullanıcılarına hangi tiplerin kullanılması gerektiği konusunda net yönergeler sağlar.

5. **IntelliSense Desteği**: Kısıtlamalar sayesinde, IDE'ler generic parametre için uygun metotları ve özellikleri önerebilir.

## Gerçek Hayat Örnekleri

Generic kısıtlamaların gerçek hayatta nasıl kullanıldığını görmek için bazı örnekler:

### Örnek 1: Repository Deseni

```csharp
public interface IEntity
{
    int Id { get; set; }
}

public class Repository<T> where T : class, IEntity, new()
{
    private readonly List<T> _entities = new List<T>();
    
    public void Add(T entity)
    {
        if (entity.Id == 0)
        {
            entity.Id = _entities.Count + 1;
        }
        _entities.Add(entity);
    }
    
    public T GetById(int id)
    {
        return _entities.FirstOrDefault(e => e.Id == id);
    }
    
    public List<T> GetAll()
    {
        return _entities.ToList();
    }
    
    public void Update(T entity)
    {
        var existing = GetById(entity.Id);
        if (existing != null)
        {
            _entities.Remove(existing);
            _entities.Add(entity);
        }
    }
    
    public void Delete(int id)
    {
        var entity = GetById(id);
        if (entity != null)
        {
            _entities.Remove(entity);
        }
    }
    
    public T CreateNew()
    {
        return new T();
    }
}

// Kullanım örneği
public class Customer : IEntity
{
    public int Id { get; set; }
    public string Name { get; set; }
    public string Email { get; set; }
}

public class Product : IEntity
{
    public int Id { get; set; }
    public string Name { get; set; }
    public decimal Price { get; set; }
}

// Repository örnekleri
var customerRepo = new Repository<Customer>();
var productRepo = new Repository<Product>();
```

### Örnek 2: Generic Serileştirici

```csharp
public class JsonSerializer<T> where T : class, new()
{
    public string Serialize(T obj)
    {
        // Basit bir serileştirme örneği
        var properties = typeof(T).GetProperties();
        var jsonPairs = new List<string>();
        
        foreach (var property in properties)
        {
            var value = property.GetValue(obj);
            var jsonValue = value switch
            {
                null => "null",
                string str => $"\"{str}\"",
                bool b => b.ToString().ToLower(),
                _ => value.ToString()
            };
            
            jsonPairs.Add($"\"{property.Name}\": {jsonValue}");
        }
        
        return "{" + string.Join(", ", jsonPairs) + "}";
    }
    
    public T Deserialize(string json)
    {
        // Gerçek bir uygulamada daha karmaşık olacaktır
        // Burada sadece örnek amaçlı basit bir uygulama
        
        var obj = new T();
        
        // JSON'ı ayrıştır ve özellikleri ayarla
        // ...
        
        return obj;
    }
}

// Kullanım örneği
var person = new Person
{
    Name = "Ahmet",
    Age = 30,
    IsActive = true
};

var serializer = new JsonSerializer<Person>();
var json = serializer.Serialize(person);
Console.WriteLine(json);
// Çıktı: {"Name": "Ahmet", "Age": 30, "IsActive": true}

var deserializedPerson = serializer.Deserialize(json);
```

## En İyi Uygulama Önerileri

Generic kısıtlamalarını kullanırken şu önerileri göz önünde bulundurun:

1. **Minimal Kısıtlamalar Kullanın**: Kodunuzun çalışması için gereken en az sayıda kısıtlamayı uygulayın. Bu, kodunuzun daha esnek olmasını sağlar.

2. **Kısıtlamaları Belgeyin**: XML belgelendirme yorumlarıyla generic parametrelerin neden belirli kısıtlamalara sahip olduğunu açıklayın.

3. **`new()` Kısıtlamasını Son Sıraya Koyun**: Birden fazla kısıtlama kullanıyorsanız, `new()` kısıtlamasını her zaman en sona koyun.

4. **Kısıtlamalar için Arayüzleri Tercih Edin**: Mümkünse, somut sınıflar yerine arayüzleri kısıtlama olarak kullanın. Bu, daha esnek ve test edilebilir kod yazmanıza yardımcı olur.

5. **Değer ve Referans Tipi Kısıtlamalarını Değerlendirin**: Değer tipleri için kutulama/kutudan çıkarma (boxing/unboxing) işlemlerini önlemek için `struct` kısıtlamasını veya referans tipleri için `null` kontrolünü kolaylaştırmak için `class` kısıtlamasını düşünün.

## Sonuç

C# dilindeki generic kısıtlamalar, generic türlerin gücünü ve esnekliğini artırırken kod güvenliğini sağlamak için önemli bir araçtır. Kısıtlamalar sayesinde, derleyici generic parametrelerin beklenen özelliklere sahip olduğundan emin olabilir ve bu da daha güvenli ve optimize edilmiş kod üretmenize yardımcı olur.

Generic kısıtlamaları etkili bir şekilde kullanarak, hem tip güvenliğini koruyan hem de yeniden kullanılabilir ve esnek kod oluşturabilirsiniz. Bu, özellikle büyük ölçekli projelerde ve kütüphanelerde çok değerlidir.
