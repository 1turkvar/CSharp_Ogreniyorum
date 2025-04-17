# C# Method Overloading (Aşırı Yükleme)

## İçindekiler
1. [Method Overloading Nedir?](#method-overloading-nedir)
2. [Method Overloading'in Avantajları](#method-overloadingin-avantajları)
3. [Method Overloading Kuralları](#method-overloading-kuralları)
4. [Örnek Uygulamalar](#örnek-uygulamalar)
   - [Temel Örnekler](#temel-örnekler)
   - [Kompleks Örnekler](#kompleks-örnekler)
5. [Method Overloading vs Method Overriding](#method-overloading-vs-method-overriding)
6. [En İyi Uygulamalar](#en-i̇yi-uygulamalar)
7. [Sık Yapılan Hatalar](#sık-yapılan-hatalar)
8. [Özet](#özet)

## Method Overloading Nedir?

Method overloading (aşırı yükleme), aynı isme sahip ancak farklı parametre listelerine sahip birden fazla metot tanımlama yeteneğidir. C# derleyicisi, çağrı sırasında kullanılan argümanların sayısına ve tipine bakarak hangi metodu çağıracağına karar verir.

Method overloading, farklı durumlar için birden fazla metot oluşturmak yerine, aynı işlevi gerçekleştiren farklı parametre versiyonlarıyla tek bir metot adı kullanmanıza olanak tanır.

## Method Overloading'in Avantajları

1. **Kod Okunabilirliği:** Benzer işlevlere sahip metotlar için aynı isim kullanılması, kodun anlaşılmasını kolaylaştırır.
2. **Kodun Bakımı:** Benzer işlevleri için tek bir isim kullanılması, kodun bakımını kolaylaştırır.
3. **Esneklik:** Programcılar, aynı işlevi farklı veri tipleri veya parametre sayılarıyla kullanabilirler.
4. **Kullanım Kolaylığı:** Kullanıcılar, metodu çağırırken parametre tiplerini ve sayılarını düşünmeden doğru metot versiyonunu otomatik olarak kullanırlar.

## Method Overloading Kuralları

Method overloading yaparken aşağıdaki kurallara dikkat etmek gerekir:

1. **Parametrelerin Farklı Olması:** Overload edilen metotların parametre tipleri veya sayıları farklı olmalıdır.
2. **Dönüş Tipi Yeterli Değil:** Sadece dönüş tipinin farklı olması method overloading için yeterli değildir. Parametre listesinin de farklı olması gerekir.
3. **Parametre İsimlerinin Önemi Yok:** Parametre isimlerinin farklı olması overloading için yeterli değildir; parametre tipleri veya sayıları farklı olmalıdır.
4. **Parametre Modifikatörleri:** `ref`, `out` veya `params` gibi parametre modifikatörleri, metot imzasının bir parçasıdır ve overloading için kullanılabilir.

## Örnek Uygulamalar

### Temel Örnekler

Aşağıda, method overloading'in temel örnekleri verilmiştir:

```csharp
using System;

public class Calculator
{
    // Farklı sayıda parametre ile overloading
    public int Add(int a, int b)
    {
        return a + b;
    }

    public int Add(int a, int b, int c)
    {
        return a + b + c;
    }

    // Farklı parametre tipleri ile overloading
    public double Add(double a, double b)
    {
        return a + b;
    }

    // Karışık parametre tipleri ile overloading
    public double Add(int a, double b)
    {
        return a + b;
    }

    public double Add(double a, int b)
    {
        return a + b;
    }
}

class Program
{
    static void Main()
    {
        Calculator calc = new Calculator();
        
        // İki int parametresi ile çağrı
        Console.WriteLine("2 + 3 = " + calc.Add(2, 3));
        
        // Üç int parametresi ile çağrı
        Console.WriteLine("2 + 3 + 4 = " + calc.Add(2, 3, 4));
        
        // İki double parametresi ile çağrı
        Console.WriteLine("2,5 + 3,5 = " + calc.Add(2.5, 3.5));
        
        // Karışık parametreler ile çağrılar
        Console.WriteLine("2 + 3,5 = " + calc.Add(2, 3.5));
        Console.WriteLine("2,5 + 3 = " + calc.Add(2.5, 3));
    }
}
```

### Kompleks Örnekler

Aşağıda, daha kompleks method overloading örnekleri verilmiştir:

```csharp
using System;

public class TextFormatter
{
    // Temel format
    public string Format(string text)
    {
        return text.Trim();
    }
    
    // Özel prefix ile format
    public string Format(string text, string prefix)
    {
        return prefix + ": " + text.Trim();
    }
    
    // Prefix ve suffix ile format
    public string Format(string text, string prefix, string suffix)
    {
        return prefix + ": " + text.Trim() + " " + suffix;
    }
    
    // Bool parametresi ile uppercase veya lowercase dönüşümü
    public string Format(string text, bool upperCase)
    {
        return upperCase ? text.Trim().ToUpper() : text.Trim().ToLower();
    }

    // Optional parameters ile method (Bu aslında overloading değil, ama benzer bir etki yaratır)
    public string FormatWithOptions(string text, string prefix = "", string suffix = "", bool upperCase = false)
    {
        string result = text.Trim();
        
        if (!string.IsNullOrEmpty(prefix))
            result = prefix + ": " + result;
            
        if (!string.IsNullOrEmpty(suffix))
            result = result + " " + suffix;
            
        return upperCase ? result.ToUpper() : result;
    }
}

// Out ve ref parametreleri ile overloading
public class Parser
{
    // Normal versiyon
    public bool TryParse(string text, out int result)
    {
        return int.TryParse(text, out result);
    }
    
    // ref parametresi ile overload
    public bool TryParse(string text, ref int defaultValue)
    {
        int result;
        bool success = int.TryParse(text, out result);
        
        if (success)
            defaultValue = result;
            
        return success;
    }
}

class ComplexExample
{
    static void Main()
    {
        TextFormatter formatter = new TextFormatter();
        
        Console.WriteLine(formatter.Format("  Hello World  "));  // "Hello World"
        Console.WriteLine(formatter.Format("  Hello World  ", "Message"));  // "Message: Hello World"
        Console.WriteLine(formatter.Format("  Hello World  ", "Message", "(!)"));  // "Message: Hello World (!)"
        Console.WriteLine(formatter.Format("  Hello World  ", true));  // "HELLO WORLD"
        
        // Optional parametrelerle kullanım
        Console.WriteLine(formatter.FormatWithOptions("  Hello World  "));  // "Hello World"
        Console.WriteLine(formatter.FormatWithOptions("  Hello World  ", "Message"));  // "Message: Hello World"
        Console.WriteLine(formatter.FormatWithOptions("  Hello World  ", "Message", "(!)"));  // "Message: Hello World (!)"
        Console.WriteLine(formatter.FormatWithOptions("  Hello World  ", upperCase: true));  // "HELLO WORLD"
        
        // Out ve ref parametreleri ile kullanım
        Parser parser = new Parser();
        int result;
        
        if (parser.TryParse("123", out result))
            Console.WriteLine("Parsed value: " + result);
            
        int defaultValue = 0;
        if (parser.TryParse("123", ref defaultValue))
            Console.WriteLine("Updated default value: " + defaultValue);
    }
}
```

## Method Overloading vs Method Overriding

Method overloading ve method overriding (geçersiz kılma) sıkça karıştırılır, ancak tamamen farklı kavramlardır:

| Özellik | Method Overloading | Method Overriding |
|---------|-------------------|------------------|
| Tanım | Aynı isimli fakat farklı parametre listelerine sahip metotlar tanımlanır | Alt sınıfta, üst sınıfın bir metodunun aynı imza ile yeniden tanımlanmasıdır |
| Amaç | Farklı parametrelerle aynı işlevi sağlamak | Üst sınıfın davranışını değiştirmek veya genişletmek |
| Kapsam | Aynı sınıf içinde veya alt sınıflarda olabilir | Sadece kalıtım ilişkisi olduğunda mümkündür |
| Anahtar Kelimeler | Özel bir anahtar kelime gerekmez | `virtual`, `override` anahtar kelimeleri kullanılır |
| Çözüm Zamanı | Derleme zamanında çözülür (static binding) | Çalışma zamanında çözülür (dynamic binding) |

Örnek:

```csharp
public class Base
{
    // Method overloading örneği
    public void Display(int num)
    {
        Console.WriteLine("Displaying int: " + num);
    }
    
    public void Display(string text)
    {
        Console.WriteLine("Displaying string: " + text);
    }
    
    // Override edilebilir metot
    public virtual void ShowInfo()
    {
        Console.WriteLine("Base class info");
    }
}

public class Derived : Base
{
    // Base sınıfındaki method overloading örneklerine ek overload
    public void Display(bool value)
    {
        Console.WriteLine("Displaying boolean: " + value);
    }
    
    // Method overriding örneği
    public override void ShowInfo()
    {
        Console.WriteLine("Derived class info");
    }
}
```

## En İyi Uygulamalar

Method overloading kullanırken aşağıdaki en iyi uygulamaları göz önünde bulundurun:

1. **Tutarlılık:** Overload edilen tüm metotlar tutarlı davranışlar sergilemelidir.
2. **Anlamlı Parametre Farklılıkları:** Overload edilen metotlar arasındaki parametre farklılıkları anlamlı olmalıdır.
3. **Aşırı Kullanımdan Kaçınma:** Çok fazla overload oluşturmak kodun karmaşıklığını artırabilir.
4. **Dokümantasyon:** Her bir overload için açıklayıcı dokümantasyon ekleyin.
5. **Optional Parametreleri Düşünün:** Bazı durumlarda method overloading yerine optional parametreler kullanmak daha uygun olabilir.

## Sık Yapılan Hatalar

Method overloading ile ilgili sık yapılan hatalar:

1. **Sadece Dönüş Tipini Değiştirmek:** Sadece dönüş tipini değiştirerek overloading yapmaya çalışmak derleyici hatası verir.

```csharp
// HATALI - Derleme hatası verir
public int Calculate(int a, int b) { return a + b; }
public double Calculate(int a, int b) { return a + b; } // Error: Cannot define overloaded method 'Calculate' because it differs only by return type
```

2. **Parametre İsimlerini Değiştirmek:** Sadece parametre isimlerini değiştirerek overloading yapmaya çalışmak derleyici hatası verir.

```csharp
// HATALI - Derleme hatası verir
public void Process(int width, int height) { }
public void Process(int length, int breadth) { } // Error: Cannot define overloaded method 'Process' because it differs only by parameter names
```

3. **Belirsiz Çağrılar:** Overload edilen metotlar arasında belirsizliğe yol açabilecek parametreler tanımlanması.

```csharp
public void Display(int number) { }
public void Display(long number) { }

// Çağrı sırasında:
Display(10); // Belirsizlik yaratabilir, çünkü 10 hem int hem de long olarak yorumlanabilir
```

4. **Çok Fazla Overloading:** Bir metodu çok fazla overload etmek, kodun anlaşılmasını zorlaştırabilir.

## Özet

Method overloading (aşırı yükleme), C# dilinin sunduğu güçlü bir özelliktir ve aşağıdaki durumlar için idealdir:

- Benzer işlevleri tek bir metot adı altında toplamak
- Farklı veri tipleri veya parametre sayılarıyla aynı işlevi gerçekleştirmek
- Daha temiz ve okunabilir kod yazımı sağlamak
- Polimorfizm prensibini uygulamak

Doğru kullanıldığında, method overloading kodunuzu daha esnek, bakımı kolay ve kullanıcı dostu hale getirebilir. Ancak, aşırıya kaçmaktan ve karmaşıklığa yol açmaktan kaçınmak önemlidir.

Method overloading, C# programlamadaki temel kavramlardan biridir ve nesne yönelimli programlamanın gücünü sergileyen önemli bir örnektir.
