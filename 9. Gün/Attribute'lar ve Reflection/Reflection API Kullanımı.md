# C# Reflection API Kullanımı

## İçindekiler
1. [Giriş](#giriş)
2. [Reflection Nedir?](#reflection-nedir)
3. [Reflection Namespace'leri](#reflection-namespaceleri)
4. [Type Sınıfı](#type-sınıfı)
5. [Assembly Sınıfı](#assembly-sınıfı)
6. [Üye Bilgilerine Erişim](#üye-bilgilerine-erişim)
   - [Metotlar](#metotlar)
   - [Özellikler](#özellikler)
   - [Alanlar](#alanlar)
   - [Eventler](#eventler)
7. [Dinamik Nesne Oluşturma](#dinamik-nesne-oluşturma)
8. [Metot Çağırma](#metot-çağırma)
9. [Generic Tipler ile Çalışma](#generic-tipler-ile-çalışma)
10. [Attribute'lar ile Çalışma](#attributelar-ile-çalışma)
11. [Performans Değerlendirmeleri](#performans-değerlendirmeleri)
12. [Güvenlik Önlemleri](#güvenlik-önlemleri)
13. [Örnek Uygulamalar](#örnek-uygulamalar)
14. [En İyi Uygulamalar](#en-iyi-uygulamalar)
15. [Kaynaklar](#kaynaklar)

## Giriş

Reflection, C# programlama dilinde çalışma zamanında tiplerin, metotların, özelliklerin ve diğer üyelerin incelenmesine ve manipüle edilmesine olanak tanıyan güçlü bir mekanizmadır. Bu rehber, C# Reflection API'sini detaylı bir şekilde anlatmayı ve örneklerle açıklamayı amaçlamaktadır.

## Reflection Nedir?

Reflection, bir programın kendi yapısını çalışma zamanında incelemesine ve değiştirmesine olanak tanıyan bir özelliktir. Bu, type metadata'ya erişim, metotları dinamik olarak çağırma, nesneleri dinamik olarak oluşturma gibi işlemleri mümkün kılar.

C# Reflection API'si ile:
- Tip bilgilerini çalışma zamanında alabilirsiniz
- Tiplerin metotlarını, özelliklerini, alanlarını ve olaylarını keşfedebilirsiniz
- Nesneleri dinamik olarak oluşturabilirsiniz
- Metotları dinamik olarak çağırabilirsiniz
- Attribute'ları inceleyebilirsiniz

## Reflection Namespace'leri

C# Reflection API'sinin temel namespace'leri şunlardır:

```csharp
using System;
using System.Reflection;
using System.Linq;
```

## Type Sınıfı

`Type` sınıfı, Reflection API'sinin temel yapı taşıdır. Bir tipin tüm metadata bilgilerine erişmenizi sağlar.

### Type Bilgisi Alma

```csharp
// Type bilgisini almak için çeşitli yöntemler
Type type1 = typeof(string);
Type type2 = "Hello".GetType();
Type type3 = Type.GetType("System.String");

Console.WriteLine($"Tip Adı: {type1.Name}");
Console.WriteLine($"Tam Adı: {type1.FullName}");
Console.WriteLine($"Namespace: {type1.Namespace}");
Console.WriteLine($"Assembly: {type1.Assembly.GetName().Name}");
```

### Type Özellikleri

```csharp
Type stringType = typeof(string);

Console.WriteLine($"IsClass: {stringType.IsClass}");
Console.WriteLine($"IsValueType: {stringType.IsValueType}");
Console.WriteLine($"IsInterface: {stringType.IsInterface}");
Console.WriteLine($"IsAbstract: {stringType.IsAbstract}");
Console.WriteLine($"IsEnum: {stringType.IsEnum}");
Console.WriteLine($"IsGenericType: {stringType.IsGenericType}");
Console.WriteLine($"IsPublic: {stringType.IsPublic}");
```

## Assembly Sınıfı

`Assembly` sınıfı, .NET assembly'lerine erişim sağlar ve içlerindeki tipleri incelemenize olanak tanır.

### Assembly Yükleme

```csharp
// Geçerli assembly
Assembly currentAssembly = Assembly.GetExecutingAssembly();

// Bir tipi içeren assembly
Assembly stringAssembly = typeof(string).Assembly;

// Dosya yolundan assembly yükleme
Assembly loadedAssembly = Assembly.LoadFrom("path/to/assembly.dll");

// Assembly adından yükleme
Assembly systemAssembly = Assembly.Load("System");
```

### Assembly İçindeki Tipleri İnceleme

```csharp
Assembly assembly = typeof(string).Assembly;

// Assembly'deki tüm tipleri alma
Type[] types = assembly.GetTypes();

Console.WriteLine($"Assembly: {assembly.GetName().Name}");
Console.WriteLine($"Tip sayısı: {types.Length}");

// Public tipleri listeleme
foreach (Type type in types.Where(t => t.IsPublic).Take(10))
{
    Console.WriteLine($"Public tip: {type.FullName}");
}
```

## Üye Bilgilerine Erişim

### Metotlar

```csharp
Type stringType = typeof(string);

// Tüm public metotları alma
MethodInfo[] methods = stringType.GetMethods(BindingFlags.Public | BindingFlags.Instance);

Console.WriteLine($"Public metot sayısı: {methods.Length}");

// İlk 5 metodu gösterme
foreach (MethodInfo method in methods.Take(5))
{
    Console.WriteLine($"Metot: {method.Name}");
    Console.WriteLine($" - Dönüş tipi: {method.ReturnType.Name}");
    
    // Parametreleri gösterme
    ParameterInfo[] parameters = method.GetParameters();
    if (parameters.Length > 0)
    {
        Console.WriteLine($" - Parametreler:");
        foreach (ParameterInfo param in parameters)
        {
            Console.WriteLine($"   - {param.ParameterType.Name} {param.Name}");
        }
    }
    else
    {
        Console.WriteLine($" - Parametre yok");
    }
    
    Console.WriteLine();
}

// Belirli bir metodu arama
MethodInfo substringMethod = stringType.GetMethod("Substring", new Type[] { typeof(int), typeof(int) });
if (substringMethod != null)
{
    Console.WriteLine($"Substring metodu bulundu: {substringMethod}");
}
```

### Özellikler

```csharp
Type dateTimeType = typeof(DateTime);

// Tüm public özellikleri alma
PropertyInfo[] properties = dateTimeType.GetProperties(BindingFlags.Public | BindingFlags.Instance);

Console.WriteLine($"Public özellik sayısı: {properties.Length}");

// Özellikleri gösterme
foreach (PropertyInfo property in properties)
{
    Console.WriteLine($"Özellik: {property.Name}");
    Console.WriteLine($" - Tipi: {property.PropertyType.Name}");
    Console.WriteLine($" - Okunabilir: {property.CanRead}");
    Console.WriteLine($" - Yazılabilir: {property.CanWrite}");
    Console.WriteLine();
}
```

### Alanlar

```csharp
Type listType = typeof(List<int>);

// Tüm private ve public alanları alma
FieldInfo[] fields = listType.GetFields(BindingFlags.NonPublic | BindingFlags.Public | BindingFlags.Instance);

Console.WriteLine($"Alan sayısı: {fields.Length}");

// Alanları gösterme
foreach (FieldInfo field in fields)
{
    Console.WriteLine($"Alan: {field.Name}");
    Console.WriteLine($" - Tipi: {field.FieldType.Name}");
    Console.WriteLine($" - Is Public: {field.IsPublic}");
    Console.WriteLine($" - Is Private: {field.IsPrivate}");
    Console.WriteLine();
}
```

### Eventler

```csharp
Type buttonType = typeof(System.Windows.Forms.Button);

// Tüm public eventleri alma
EventInfo[] events = buttonType.GetEvents(BindingFlags.Public | BindingFlags.Instance);

Console.WriteLine($"Event sayısı: {events.Length}");

// Eventleri gösterme
foreach (EventInfo eventInfo in events)
{
    Console.WriteLine($"Event: {eventInfo.Name}");
    Console.WriteLine($" - Event hanlder tipi: {eventInfo.EventHandlerType.Name}");
    Console.WriteLine();
}
```

## Dinamik Nesne Oluşturma

Reflection ile çalışma zamanında dinamik olarak nesneler oluşturabilirsiniz:

```csharp
// Default constructor ile nesne oluşturma
Type listType = typeof(List<string>);
object listObj = Activator.CreateInstance(listType);

// Constructor parametresi gerektiren tiplerde:
Type dicType = typeof(Dictionary<string, int>);
object dicObj = Activator.CreateInstance(dicType, new object[] { StringComparer.OrdinalIgnoreCase });

// Generic tip oluşturma
Type genericList = typeof(List<>);
Type stringListType = genericList.MakeGenericType(typeof(string));
object stringList = Activator.CreateInstance(stringListType);
```

## Metot Çağırma

Reflection ile çalışma zamanında dinamik olarak metotları çağırabilirsiniz:

```csharp
// Örnek bir liste oluştur
List<string> names = new List<string>();

// Add metodunu reflection ile çağır
Type listType = names.GetType();
MethodInfo addMethod = listType.GetMethod("Add");
addMethod.Invoke(names, new object[] { "John" });
addMethod.Invoke(names, new object[] { "Alice" });

// Listedeki elemanları kontrol et
Console.WriteLine($"Eleman sayısı: {names.Count}"); // 2
Console.WriteLine($"İlk eleman: {names[0]}"); // John

// Static metotları çağırma
Type mathType = typeof(Math);
MethodInfo maxMethod = mathType.GetMethod("Max", new Type[] { typeof(int), typeof(int) });
object result = maxMethod.Invoke(null, new object[] { 10, 20 });
Console.WriteLine($"Max(10, 20) = {result}"); // 20
```

## Generic Tipler ile Çalışma

```csharp
// Generic bir tip tanımlama
Type openGenericType = typeof(List<>);

// Generic tipi kapatma (specific type ile)
Type closedGenericType = openGenericType.MakeGenericType(typeof(int));

// Generic tipten nesne oluşturma
object listInstance = Activator.CreateInstance(closedGenericType);

// Generic metotları çağırma
MethodInfo addMethod = closedGenericType.GetMethod("Add");
addMethod.Invoke(listInstance, new object[] { 42 });
addMethod.Invoke(listInstance, new object[] { 100 });

// Generic tipin özelliklerine erişme
PropertyInfo countProperty = closedGenericType.GetProperty("Count");
int count = (int)countProperty.GetValue(listInstance);
Console.WriteLine($"Listede {count} eleman var.");
```

## Attribute'lar ile Çalışma

```csharp
// Örnek sınıf
[Serializable]
[Obsolete("Bu sınıf artık kullanılmamalı")]
public class SampleClass
{
    [Required]
    public string Name { get; set; }
    
    [Range(1, 100)]
    public int Age { get; set; }
}

// Attribute'ları kontrol etme
Type sampleType = typeof(SampleClass);

// Sınıf üzerindeki attribute'ları alma
object[] classAttributes = sampleType.GetCustomAttributes(false);
foreach (var attr in classAttributes)
{
    Console.WriteLine($"Sınıf attribute: {attr.GetType().Name}");
    
    if (attr is ObsoleteAttribute obsolete)
    {
        Console.WriteLine($" - Mesaj: {obsolete.Message}");
    }
}

// Özellik üzerindeki attribute'ları alma
PropertyInfo nameProperty = sampleType.GetProperty("Name");
object[] propAttributes = nameProperty.GetCustomAttributes(false);
foreach (var attr in propAttributes)
{
    Console.WriteLine($"Name özelliği attribute: {attr.GetType().Name}");
}

// Belirli bir attribute'un varlığını kontrol etme
bool isSerializable = sampleType.IsDefined(typeof(SerializableAttribute), false);
Console.WriteLine($"Serializable mi: {isSerializable}");
```

## Performans Değerlendirmeleri

Reflection, normal statik çağrılara göre daha yavaştır. Dikkatli kullanılmalıdır:

```csharp
using System.Diagnostics;

// Test sınıfı
public class Test
{
    public void RegularMethod() { }
    
    public static void ComparePerformance()
    {
        Test test = new Test();
        Type type = typeof(Test);
        MethodInfo method = type.GetMethod("RegularMethod");
        
        const int iterations = 1000000;
        
        // Normal çağrı
        Stopwatch sw1 = Stopwatch.StartNew();
        for (int i = 0; i < iterations; i++)
        {
            test.RegularMethod();
        }
        sw1.Stop();
        
        // Reflection ile çağrı
        Stopwatch sw2 = Stopwatch.StartNew();
        for (int i = 0; i < iterations; i++)
        {
            method.Invoke(test, null);
        }
        sw2.Stop();
        
        Console.WriteLine($"Normal çağrı: {sw1.ElapsedMilliseconds} ms");
        Console.WriteLine($"Reflection çağrısı: {sw2.ElapsedMilliseconds} ms");
        Console.WriteLine($"Reflection, normal çağrıdan {sw2.ElapsedMilliseconds / (double)sw1.ElapsedMilliseconds:F2} kat daha yavaş");
    }
}
```

## Güvenlik Önlemleri

Reflection ile çalışırken güvenlik önlemlerini göz önünde bulundurmak gerekir:

- `Assembly.Load` veya `Type.GetType` gibi API'lerde kullanıcı girdilerini doğrudan geçmeyin
- Private üyelere erişim için dikkatli olun, güvenlik açıkları oluşabilir
- ReflectionPermission ile güvenlik sınırlamalarını kontrol edin
- Sandbox veya kısıtlı ortamlarda, bu API'nin bazı bölümleri kısıtlanabilir

```csharp
// Assembly yüklerken güvenlik kontrolleri
public static Assembly SafeLoadAssembly(string assemblyName)
{
    // Kullanıcı girdisini doğrulama
    if (string.IsNullOrEmpty(assemblyName) || assemblyName.Contains(".."))
    {
        throw new SecurityException("Geçersiz assembly adı.");
    }
    
    try
    {
        return Assembly.Load(assemblyName);
    }
    catch (Exception ex)
    {
        Console.WriteLine($"Assembly yüklenemedi: {ex.Message}");
        return null;
    }
}
```

## Örnek Uygulamalar

### Basit Dependency Injection Konteyner Örneği

```csharp
public class SimpleContainer
{
    private Dictionary<Type, Type> _registrations = new Dictionary<Type, Type>();
    
    public void Register<TInterface, TImplementation>() where TImplementation : TInterface
    {
        _registrations[typeof(TInterface)] = typeof(TImplementation);
    }
    
    public T Resolve<T>()
    {
        return (T)Resolve(typeof(T));
    }
    
    public object Resolve(Type type)
    {
        if (_registrations.TryGetValue(type, out Type implementationType))
        {
            // Constructor'ları al
            ConstructorInfo[] constructors = implementationType.GetConstructors();
            
            // İlk constructor'ı kullan (basitlik için)
            ConstructorInfo constructor = constructors.First();
            
            // Constructor parametrelerini al
            ParameterInfo[] parameters = constructor.GetParameters();
            
            // Her parametre için bağımlılıkları çözümle
            object[] arguments = new object[parameters.Length];
            for (int i = 0; i < parameters.Length; i++)
            {
                arguments[i] = Resolve(parameters[i].ParameterType);
            }
            
            // Nesneyi oluştur
            return constructor.Invoke(arguments);
        }
        
        // Doğrudan çözümle
        if (!type.IsInterface && !type.IsAbstract)
        {
            return Activator.CreateInstance(type);
        }
        
        throw new InvalidOperationException($"Tip çözümlenemedi: {type.FullName}");
    }
}

// Kullanımı:
public interface ILogger
{
    void Log(string message);
}

public class ConsoleLogger : ILogger
{
    public void Log(string message)
    {
        Console.WriteLine($"LOG: {message}");
    }
}

public class UserService
{
    private readonly ILogger _logger;
    
    public UserService(ILogger logger)
    {
        _logger = logger;
    }
    
    public void DoSomething()
    {
        _logger.Log("DoSomething çağrıldı");
    }
}

// DI Konteyner kullanımı
public static void TestDI()
{
    var container = new SimpleContainer();
    container.Register<ILogger, ConsoleLogger>();
    
    UserService userService = container.Resolve<UserService>();
    userService.DoSomething(); // "LOG: DoSomething çağrıldı"
}
```

### Basit Mapping Kütüphanesi Örneği

```csharp
public class SimpleMapper
{
    public TDestination Map<TSource, TDestination>(TSource source)
        where TDestination : new()
    {
        // Hedef nesneyi oluştur
        TDestination destination = new TDestination();
        
        // Kaynak tipin özelliklerini al
        PropertyInfo[] sourceProperties = typeof(TSource).GetProperties();
        
        // Hedef tipin özelliklerini al
        PropertyInfo[] destinationProperties = typeof(TDestination).GetProperties();
        
        // Her bir hedef özelliği eşleştir
        foreach (PropertyInfo destProp in destinationProperties)
        {
            // Kaynak özelliği bul
            PropertyInfo sourceProp = Array.Find(sourceProperties, p => 
                p.Name == destProp.Name && 
                p.PropertyType == destProp.PropertyType);
            
            if (sourceProp != null && destProp.CanWrite)
            {
                // Değeri oku ve yaz
                object value = sourceProp.GetValue(source);
                destProp.SetValue(destination, value);
            }
        }
        
        return destination;
    }
}

// Kullanımı:
public class UserDto
{
    public int Id { get; set; }
    public string Name { get; set; }
    public int Age { get; set; }
}

public class UserViewModel
{
    public int Id { get; set; }
    public string Name { get; set; }
    public int Age { get; set; }
    public string Address { get; set; } // Eşleşmeyen özellik
}

public static void TestMapper()
{
    var mapper = new SimpleMapper();
    
    var user = new UserDto { Id = 1, Name = "John", Age = 30 };
    var viewModel = mapper.Map<UserDto, UserViewModel>(user);
    
    Console.WriteLine($"Id: {viewModel.Id}, Name: {viewModel.Name}, Age: {viewModel.Age}");
    // Output: Id: 1, Name: John, Age: 30
}
```

## En İyi Uygulamalar

1. **Performansı Optimize Edin**:
   - Reflection sonuçlarını önbelleğe alın
   - Kritik performans gereken bölümlerde reflection kullanmaktan kaçının
   - Expression trees veya IL emit kullanarak dinamik metodlar oluşturun

2. **Hata Yönetimini İyileştirin**:
   - Reflection işlemlerini her zaman try-catch blokları içinde kullanın
   - Belirli ReflectionTypeLoadException durumlarını ele alın

3. **Güvenlik Önlemlerini Uygulayın**:
   - Kullanıcı girdilerini doğrudan reflection API'lerine geçirmeyin
   - Yüklenen assembly'lerin güvenilir olduğundan emin olun

4. **Bellek Yönetimi**:
   - Büyük assembly'lerle çalışırken bellek kullanımını izleyin
   - Gereksiz metadata taşımaktan kaçının

5. **Kapsüllemeye Saygı Duyun**:
   - Private üyelere erişim gerektiğinde bunu belgelendirin
   - Private üyelere erişim için iyi bir gerekçeye sahip olun

```csharp
// Reflection sonuçlarını önbelleğe alma örneği
public class TypeCache
{
    private static readonly ConcurrentDictionary<Type, PropertyInfo[]> PropertyCache = 
        new ConcurrentDictionary<Type, PropertyInfo[]>();
    
    public static PropertyInfo[] GetProperties(Type type)
    {
        return PropertyCache.GetOrAdd(type, t => t.GetProperties());
    }
}
```

## Kaynaklar

- [Microsoft Resmi Belgeler: Reflection](https://docs.microsoft.com/en-us/dotnet/csharp/programming-guide/concepts/reflection)
- [.NET API: System.Reflection Namespace](https://docs.microsoft.com/en-us/dotnet/api/system.reflection)
- [Type Sınıfı](https://docs.microsoft.com/en-us/dotnet/api/system.type)
- [Assembly Sınıfı](https://docs.microsoft.com/en-us/dotnet/api/system.reflection.assembly)
- [MethodInfo Sınıfı](https://docs.microsoft.com/en-us/dotnet/api/system.reflection.methodinfo)
