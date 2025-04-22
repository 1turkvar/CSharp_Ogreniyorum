# C# Dynamic Tipleri: Kapsamlı Rehber

## İçindekiler
1. [Giriş](#giriş)
2. [Dynamic Anahtar Kelimesi](#dynamic-anahtar-kelimesi)
3. [Çalışma Prensibi](#çalışma-prensibi)
4. [Kullanım Senaryoları](#kullanım-senaryoları)
5. [Avantajları ve Dezavantajları](#avantajları-ve-dezavantajları)
6. [Örnekler](#örnekler)
7. [ExpandoObject](#expandoobject)
8. [DynamicObject](#dynamicobject)
9. [Dynamic vs var vs object](#dynamic-vs-var-vs-object)
10. [Performans Konuları](#performans-konuları)
11. [En İyi Pratikler](#en-iyi-pratikler)
12. [Sık Yapılan Hatalar](#sık-yapılan-hatalar)

## Giriş

C# temelde statik tip kontrolü yapılan bir programlama dilidir. Ancak C# 4.0 ile birlikte dile eklenen `dynamic` anahtar kelimesi, derleme zamanı tip kontrolünü erteleyerek çalışma zamanında gerçekleştirilmesini sağlar. Bu özellik, C# dilini daha esnek hale getirir ve özellikle belirli senaryolarda çok faydalı olabilir.

## Dynamic Anahtar Kelimesi

`dynamic` anahtar kelimesi, bir değişkenin tipinin derleme zamanında değil, çalışma zamanında belirlenmesini sağlar. Bu, değişkenin üzerinde yapılan işlemlerin geçerliliğinin derleme sırasında kontrol edilmeyeceği anlamına gelir.

```csharp
dynamic dynamicValue = 10;
dynamicValue = "Artık bir string";
dynamicValue = new List<int> { 1, 2, 3 };
```

Bu örnekte, `dynamicValue` değişkeni önce bir tamsayı, sonra bir string ve en sonunda bir liste olarak kullanılmıştır. Normalde statik tipli bir dilde bu tür tip değişimleri mümkün değildir, ancak `dynamic` sayesinde bu mümkün hale gelir.

## Çalışma Prensibi

`dynamic` tipi arkada `.NET` tarafından `System.Dynamic` namespace'i altındaki sınıflar ve Dynamic Language Runtime (DLR) kullanılarak uygulanır. DLR, .NET Framework'e dinamik davranışlar eklemek için tasarlanmış bir çalışma zamanı ortamıdır.

Dynamic değişkenler üzerinde yapılan işlemler şu adımları takip eder:

1. Derleme zamanında, derleyici tip kontrolünü atlar ve dynamic olarak işaretlenen ifadelerle ilgili çağrılar için dinamik metot çağrıları oluşturur.
2. Çalışma zamanında, DLR gerçek nesnenin tipine göre uygun metodu bulur ve çağırır.
3. Eğer çağrılan metot veya özellik mevcut değilse, bir çalışma zamanı istisnası (RuntimeBinderException) fırlatılır.

```csharp
dynamic obj = "Hello";
int length = obj.Length; // Çalışır, string'in Length özelliği vardır
int value = obj.Value;   // Çalışma zamanında hata verir, string'in Value özelliği yoktur
```

## Kullanım Senaryoları

`dynamic` tipinin özellikle faydalı olduğu senaryolar şunlardır:

### 1. COM Nesneleriyle Çalışma

```csharp
dynamic excel = Activator.CreateInstance(Type.GetTypeFromProgID("Excel.Application"));
excel.Visible = true;
excel.Workbooks.Add();
excel.Cells[1, 1].Value = "Merhaba, Excel!";
```

Bu örnekte, Excel COM nesnelerine erişim `dynamic` sayesinde çok daha temiz ve kolay hale gelmiştir.

### 2. JSON/XML Verilerini İşleme

```csharp
using Newtonsoft.Json;

string json = @"{
  'Name': 'Ali',
  'Age': 30,
  'Address': {
    'City': 'İstanbul',
    'Country': 'Türkiye'
  }
}";

dynamic data = JsonConvert.DeserializeObject(json);
Console.WriteLine($"İsim: {data.Name}, Yaş: {data.Age}");
Console.WriteLine($"Şehir: {data.Address.City}");
```

### 3. Yansıma (Reflection) İşlemlerini Basitleştirme

```csharp
// Reflection kullanımı
object obj = Activator.CreateInstance(Type.GetType("MyNamespace.MyClass"));
Type type = obj.GetType();
MethodInfo method = type.GetMethod("DoSomething");
method.Invoke(obj, new object[] { "parametre" });

// Dynamic kullanımı
dynamic dynObj = Activator.CreateInstance(Type.GetType("MyNamespace.MyClass"));
dynObj.DoSomething("parametre");
```

### 4. Farklı Diller Arası Birlikte Çalışabilirlik

```csharp
// Python.NET kütüphanesi ile Python kodunu çağırma
using Python.Runtime;

dynamic np = Py.Import("numpy");
dynamic array = np.array(new[] { 1, 2, 3, 4, 5 });
dynamic mean = np.mean(array);
Console.WriteLine($"Ortalama: {mean}");
```

### 5. Eklenti Sistemleri

```csharp
foreach (var plugin in loadedPlugins)
{
    dynamic pluginInstance = Activator.CreateInstance(plugin.GetType());
    pluginInstance.Initialize();
    pluginInstance.Run();
}
```

## Avantajları ve Dezavantajları

### Avantajları

1. **Esneklik**: Statik tip kontrolünden kurtularak daha esnek kod yazabilirsiniz.
2. **Daha Az Kod**: Özellikle COM nesneleri veya yansıma ile çalışırken çok daha az kod yazmanızı sağlar.
3. **Dinamik Dil Özellikleri**: C# gibi statik bir dile dinamik dillerin bazı faydalı özelliklerini ekler.
4. **Birlikte Çalışabilirlik**: Farklı diller ve sistemlerle daha kolay etkileşim sağlar.

### Dezavantajları

1. **Derleme Zamanı Güvenliği Kaybı**: Hataların derleme zamanında değil, çalışma zamanında tespit edilmesi.
2. **Performans Maliyeti**: Dynamic çözümler genellikle statik tipli alternatiflere göre daha yavaştır.
3. **IDE Desteğinin Azalması**: IntelliSense ve otomatik tamamlama gibi özellikler dynamic değişkenler için çalışmaz.
4. **Okunabilirlik ve Bakım Sorunları**: Kodun ne yaptığının anlaşılması zorlaşabilir.

## Örnekler

### Temel Kullanım

```csharp
dynamic d = 7;
Console.WriteLine(d.GetType()); // System.Int32

d = "Merhaba";
Console.WriteLine(d.GetType()); // System.String

d = true;
Console.WriteLine(d.GetType()); // System.Boolean

d = new List<int> { 1, 2, 3 };
Console.WriteLine(d.GetType()); // System.Collections.Generic.List`1[System.Int32]
```

### Dynamic Metot Parametreleri ve Dönüş Değerleri

```csharp
public dynamic ProcessValue(dynamic input)
{
    if (input is int)
        return input * 2;
    else if (input is string)
        return input + input;
    else if (input is bool)
        return !input;
    else
        return null;
}

// Kullanım
dynamic result1 = ProcessValue(10);      // 20
dynamic result2 = ProcessValue("Test");  // "TestTest"
dynamic result3 = ProcessValue(true);    // false
```

### Duck Typing Örneği

Duck typing, "Eğer bir kuş ördek gibi görünüyor, ördek gibi yüzüyor ve ördek gibi ötüyorsa, muhtemelen bir ördektir" prensibine dayanır. Dynamic ile bu prensibi C#'ta uygulayabilirsiniz:

```csharp
class Duck
{
    public void Quack() => Console.WriteLine("Vak!");
    public void Swim() => Console.WriteLine("Ördek yüzüyor!");
}

class Person
{
    public void Quack() => Console.WriteLine("İnsan ördek gibi sesleniyor!");
    public void Swim() => Console.WriteLine("İnsan yüzüyor!");
}

public void MakeDuckSounds(dynamic duck)
{
    duck.Quack();
    duck.Swim();
}

// Kullanım
Duck realDuck = new Duck();
Person person = new Person();

MakeDuckSounds(realDuck);  // "Vak!" ve "Ördek yüzüyor!"
MakeDuckSounds(person);    // "İnsan ördek gibi sesleniyor!" ve "İnsan yüzüyor!"
```

## ExpandoObject

`ExpandoObject`, dinamik olarak özelliklerin eklenip çıkarılabileceği bir nesne türüdür.

```csharp
using System.Dynamic;

dynamic person = new ExpandoObject();
person.Name = "Ahmet";
person.Age = 35;
person.Greet = (Action)(() => Console.WriteLine($"Merhaba, ben {person.Name}"));

// Özellik ekleme
person.Job = "Yazılım Geliştirici";

// Özellik silme
((IDictionary<string, object>)person).Remove("Age");

// Özellik kontrol etme
var personDict = (IDictionary<string, object>)person;
if (personDict.ContainsKey("Name"))
{
    Console.WriteLine($"İsim: {person.Name}");
}

// Özellikleri listeleme
foreach (var kvp in (IDictionary<string, object>)person)
{
    Console.WriteLine($"{kvp.Key}: {kvp.Value}");
}

// Metodu çağırma
person.Greet();
```

## DynamicObject

`DynamicObject` sınıfı, daha gelişmiş dinamik davranışlar uygulamak için kullanılır. DynamicObject'i genişleterek özel dinamik nesneler oluşturabilirsiniz.

```csharp
using System.Dynamic;

public class DynamicDictionary : DynamicObject
{
    private Dictionary<string, object> _dictionary = new Dictionary<string, object>();

    // Dinamik özellik erişimi
    public override bool TryGetMember(GetMemberBinder binder, out object result)
    {
        if (_dictionary.TryGetValue(binder.Name, out result))
            return true;
        
        result = null;
        return false;
    }

    // Dinamik özellik atama
    public override bool TrySetMember(SetMemberBinder binder, object value)
    {
        _dictionary[binder.Name] = value;
        return true;
    }

    // Dinamik metot çağrısı
    public override bool TryInvokeMember(InvokeMemberBinder binder, object[] args, out object result)
    {
        if (binder.Name == "Add" && args.Length == 2 && args[0] is string)
        {
            _dictionary[(string)args[0]] = args[1];
            result = null;
            return true;
        }
        
        return base.TryInvokeMember(binder, args, out result);
    }
}

// Kullanım
dynamic dynamicDict = new DynamicDictionary();
dynamicDict.Name = "Ali";
dynamicDict.Age = 30;
dynamicDict.Add("City", "İstanbul");

Console.WriteLine(dynamicDict.Name);  // "Ali"
Console.WriteLine(dynamicDict.City);  // "İstanbul"
```

## Dynamic vs var vs object

| Özellik | dynamic | var | object |
|---------|---------|-----|--------|
| Tip Kontrolü | Çalışma zamanında | Derleme zamanında | Derleme zamanında |
| Tip Çıkarımı | Hayır | Evet | Hayır |
| Metot Çağrısı | Çalışma zamanında çözümlenir | Derleme zamanında çözümlenir | Cast gerektirir |
| Boxing/Unboxing | Hayır (bazen) | Hayır | Evet |
| IDE Desteği | Sınırlı | Tam | Tam |

### Örnek Karşılaştırma

```csharp
// dynamic
dynamic dynamicValue = 10;
dynamicValue = "string değeri";  // Geçerli
dynamicValue.ToUpper();          // Çalışma zamanında çözümlenir

// var
var varValue = 10;  
// varValue = "string değeri";   // Derleme hatası
// varValue.ToUpper();           // Derleme hatası

// object
object objValue = 10;
objValue = "string değeri";      // Geçerli
// objValue.ToUpper();           // Derleme hatası
((string)objValue).ToUpper();    // Cast gereklidir
```

## Performans Konuları

Dynamic kullanımı, statik tipli alternatiflere göre performans açısından maliyetlidir. Bunun nedenleri:

1. **Çağrı Site'larının Oluşturulması**: Dynamic ifadeler için DLR çağrı site'ları oluşturur.
2. **Tip Çözümleme**: Çalışma zamanında tiplerin ve metotların çözümlenmesi gerekir.
3. **Önbelleğe Alma**: İlk çağrı yavaştır, ancak DLR sonraki çağrıları önbelleğe alır.

### Performans Ölçümü Örneği

```csharp
using System;
using System.Diagnostics;

class Program
{
    static void Main()
    {
        const int iterations = 1000000;
        
        // Statik tip ile test
        string staticString = "Test";
        Stopwatch sw1 = Stopwatch.StartNew();
        
        for (int i = 0; i < iterations; i++)
        {
            int length = staticString.Length;
        }
        
        sw1.Stop();
        
        // Dynamic tip ile test
        dynamic dynamicString = "Test";
        Stopwatch sw2 = Stopwatch.StartNew();
        
        for (int i = 0; i < iterations; i++)
        {
            int length = dynamicString.Length;
        }
        
        sw2.Stop();
        
        Console.WriteLine($"Statik tip: {sw1.ElapsedMilliseconds} ms");
        Console.WriteLine($"Dynamic tip: {sw2.ElapsedMilliseconds} ms");
    }
}
```

Bu örnek genellikle dynamic versiyonun statik tip versiyonundan 10-100 kat daha yavaş olduğunu gösterecektir.

## En İyi Pratikler

1. **Gerektiğinde Kullanın**: Dynamic tipi sadece gerçekten ihtiyaç duyduğunuz durumlarda kullanın.
2. **Kapsamı Sınırlayın**: Dynamic değişkenlerin kapsamını mümkün olduğunca dar tutun.
3. **Try-Catch ile Koruyun**: Dynamic çağrıları try-catch blokları içine alarak çalışma zamanı hatalarına karşı koruyun.
4. **Cast İşlemi Yapın**: Mümkün olduğunca erken bir aşamada dynamic değişkenleri bilinen tiplere dönüştürün.
5. **Test Edin**: Dynamic kod içeren bölümleri kapsamlı şekilde test edin.

```csharp
// İyi pratik örneği
public string GetUpperCaseName(dynamic person)
{
    try
    {
        // Dynamic'ten bilinen tipe mümkün olduğunca erken dönüştürme
        string name = person.Name;
        return name.ToUpper();
    }
    catch (Microsoft.CSharp.RuntimeBinder.RuntimeBinderException ex)
    {
        // Özel hata yönetimi
        Console.WriteLine($"Hata: {ex.Message}");
        return string.Empty;
    }
}
```

## Sık Yapılan Hatalar

1. **Gereksiz Kullanım**: Her durumda dynamic kullanmak yerine, static tiplerle çözülebilecek durumlarda static tipleri tercih edin.
2. **Hata Yakalama Eksikliği**: Dynamic çağrılar çalışma zamanında başarısız olabilir, bu hatalar yakalanmazsa uygulama çökebilir.
3. **Performans Göz Ardı Etme**: Dynamic kullanımının performans etkilerini düşünmeden yoğun hesaplama gerektiren kod bloklarında kullanmak.
4. **Zor Bakım**: Kodun her yerinde dynamic kullanımı, kodun anlaşılmasını ve bakımını zorlaştırır.

```csharp
// Kötü örnek
public void ProcessData(dynamic data)
{
    // Hiçbir hata kontrolü yok
    var result = data.Calculate();
    Console.WriteLine(result.ToString());
}

// Daha iyi örnek
public void ProcessData(dynamic data)
{
    try
    {
        if (data == null)
            throw new ArgumentNullException(nameof(data));
            
        var result = data.Calculate();
        Console.WriteLine(result?.ToString() ?? "Null sonuç");
    }
    catch (Microsoft.CSharp.RuntimeBinder.RuntimeBinderException ex)
    {
        Console.WriteLine($"Çalışma zamanı bağlama hatası: {ex.Message}");
    }
    catch (Exception ex)
    {
        Console.WriteLine($"Genel hata: {ex.Message}");
    }
}
```

---

Bu rehber, C# dilindeki `dynamic` tipi ve ilgili özellikleri hakkında kapsamlı bir genel bakış sağlamaktadır. `dynamic` kullanırken, bunun bir araç olduğunu ve her soruna uygun bir çözüm olmadığını unutmayın. En iyi sonuçlar için, ihtiyaçlarınızı değerlendirin ve dynamic ile statik tip kullanımı arasında uygun dengeyi bulun.
