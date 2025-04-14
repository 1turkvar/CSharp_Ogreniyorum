# C# break, continue ve return İfadeleri

C# programlama dilinde akış kontrolü için kullanılan `break`, `continue` ve `return` ifadeleri, farklı durumlarda program akışını değiştirmek için kullanılır. Bu döküman, her birinin ne olduğunu, nasıl kullanıldığını ve farklı senaryolardaki uygulamalarını detaylı bir şekilde açıklamaktadır.

## İçindekiler
1. [break İfadesi](#break-ifadesi)
2. [continue İfadesi](#continue-ifadesi)
3. [return İfadesi](#return-ifadesi)
4. [Örnekler ve Karşılaştırmalar](#örnekler-ve-karşılaştırmalar)
5. [En İyi Kullanım Pratikleri](#en-iyi-kullanım-pratikleri)

## break İfadesi

`break` ifadesi, döngüleri (`for`, `while`, `do-while`, `foreach`) veya `switch` deyimlerini sonlandırmak için kullanılır.

### Kullanım Amaçları

1. **Döngüden Çıkma**: Bir döngünün belirli bir koşul gerçekleştiğinde anında sonlandırılması gerektiğinde kullanılır.
2. **switch Deyiminde**: Bir `case` bloğunun sonunda, diğer `case` bloklarının çalıştırılmasını önlemek için kullanılır.

### Sözdizimi

```csharp
// Döngülerde kullanımı
for (int i = 0; i < 10; i++)
{
    if (i == 5)
    {
        break; // Döngüden çık
    }
    Console.WriteLine(i);
}

// switch deyiminde kullanımı
switch (değer)
{
    case 1:
        // İşlemler
        break;
    case 2:
        // İşlemler
        break;
    default:
        // Varsayılan işlemler
        break;
}
```

### Örnekler

**Örnek 1: Döngüden Çıkma**

```csharp
int[] sayilar = { 1, 3, 5, 7, 9, 11, 13 };
bool bulundu = false;

for (int i = 0; i < sayilar.Length; i++)
{
    if (sayilar[i] == 7)
    {
        Console.WriteLine("7 sayısı bulundu!");
        bulundu = true;
        break; // Döngüden çık
    }
}

if (!bulundu)
{
    Console.WriteLine("7 sayısı bulunamadı!");
}
```

**Örnek 2: İç İçe Döngülerde Kullanım**

```csharp
for (int i = 0; i < 3; i++)
{
    for (int j = 0; j < 3; j++)
    {
        if (i == 1 && j == 1)
        {
            Console.WriteLine("İç döngüden çıkılıyor!");
            break; // Sadece iç döngüden çıkar
        }
        Console.WriteLine($"i = {i}, j = {j}");
    }
}
```

**Önemli Not**: `break` ifadesi sadece içinde bulunduğu en içteki döngü veya switch deyimini sonlandırır, dıştaki döngüleri etkilemez.

## continue İfadesi

`continue` ifadesi, döngülerde (`for`, `while`, `do-while`, `foreach`) mevcut iterasyonu atlayıp bir sonraki iterasyona geçmek için kullanılır.

### Kullanım Amaçları

1. **Belirli Koşulları Atlamak**: Döngü içinde belirli durumları işlemeden geçmek istediğinizde.
2. **Döngü İçindeki Koşullu İşlemleri Basitleştirmek**: Derin iç içe if-else yapıları yerine erken continue kullanmak kodun okunabilirliğini artırabilir.

### Sözdizimi

```csharp
for (int i = 0; i < 10; i++)
{
    if (i % 2 == 0)
    {
        continue; // Bu iterasyonu atla ve sonraki iterasyona geç
    }
    Console.WriteLine(i); // Sadece tek sayılar yazdırılır
}
```

### Örnekler

**Örnek 1: Belirli Değerleri Atlama**

```csharp
for (int i = 1; i <= 10; i++)
{
    if (i % 3 == 0)
    {
        continue; // 3'ün katı olan sayıları atla
    }
    Console.WriteLine($"Sayı: {i}");
}
```

**Örnek 2: while Döngüsü İçinde Kullanım**

```csharp
int j = 0;
while (j < 10)
{
    j++;
    if (j % 2 == 0)
    {
        continue; // Çift sayıları atla
    }
    Console.WriteLine($"Tek sayı: {j}");
}
```

**Örnek 3: İç İçe Döngülerde Kullanım**

```csharp
for (int i = 0; i < 3; i++)
{
    for (int j = 0; j < 3; j++)
    {
        if (i == j)
        {
            continue; // i = j olduğunda bu iterasyonu atla
        }
        Console.WriteLine($"i = {i}, j = {j}");
    }
}
```

## return İfadesi

`return` ifadesi, bir metodun yürütülmesini sonlandırır ve isteğe bağlı olarak çağıran metoda bir değer döndürür.

### Kullanım Amaçları

1. **Değer Döndürme**: Bir metodun sonucunu çağıran koda iletmek.
2. **Erken Çıkış**: Belirli koşullar altında metodun çalışmasını erken sonlandırmak.

### Sözdizimi

```csharp
// Değer döndüren metot
public int Topla(int a, int b)
{
    return a + b; // int türünde değer döndürür
}

// Değer döndürmeyen (void) metot
public void IslemYap(int deger)
{
    if (deger < 0)
    {
        Console.WriteLine("Negatif değer işlenemez!");
        return; // Metottan çık, değer döndürmez
    }
    
    // İşlemler...
    Console.WriteLine($"İşlem tamamlandı: {deger}");
}
```

### Örnekler

**Örnek 1: Basit Değer Döndürme**

```csharp
public int KareAl(int sayi)
{
    return sayi * sayi;
}

// Kullanımı
int sonuc = KareAl(5); // sonuc = 25
```

**Örnek 2: Koşullu Dönüş**

```csharp
public string NotDegerlendirme(int not)
{
    if (not < 0 || not > 100)
    {
        return "Geçersiz not!";
    }
    
    if (not >= 90)
    {
        return "A";
    }
    else if (not >= 80)
    {
        return "B";
    }
    else if (not >= 70)
    {
        return "C";
    }
    else if (not >= 60)
    {
        return "D";
    }
    else
    {
        return "F";
    }
}
```

**Örnek 3: Void Metotta Erken Çıkış**

```csharp
public void KullaniciDogrula(string kullaniciAdi, string sifre)
{
    if (string.IsNullOrEmpty(kullaniciAdi) || string.IsNullOrEmpty(sifre))
    {
        Console.WriteLine("Kullanıcı adı ve şifre boş olamaz!");
        return; // Metottan çık
    }
    
    // Doğrulama işlemleri...
    Console.WriteLine("Kullanıcı doğrulama başarılı!");
}
```

## Örnekler ve Karşılaştırmalar

### break vs. continue

Aşağıdaki örnekte, `break` ve `continue` ifadelerinin farkını görebilirsiniz:

```csharp
// break örneği
Console.WriteLine("break örneği:");
for (int i = 0; i < 10; i++)
{
    if (i == 5)
    {
        break; // Döngü burada tamamen sonlanır
    }
    Console.Write($"{i} "); // 0 1 2 3 4 yazdırılır
}

// continue örneği
Console.WriteLine("\n\ncontinue örneği:");
for (int i = 0; i < 10; i++)
{
    if (i == 5)
    {
        continue; // Sadece i=5 durumu atlanır
    }
    Console.Write($"{i} "); // 0 1 2 3 4 6 7 8 9 yazdırılır
}
```

### break vs. return

Aşağıdaki örnekte, `break` ve `return` ifadelerinin farkını görebilirsiniz:

```csharp
public void BreakOrnegi(int[] dizi, int aranan)
{
    bool bulundu = false;
    
    for (int i = 0; i < dizi.Length; i++)
    {
        if (dizi[i] == aranan)
        {
            Console.WriteLine($"Sayı {i}. indexte bulundu.");
            bulundu = true;
            break; // Döngüden çık ama metot devam eder
        }
    }
    
    if (bulundu)
    {
        Console.WriteLine("Arama başarılı!");
    }
    else
    {
        Console.WriteLine("Sayı bulunamadı!");
    }
}

public bool ReturnOrnegi(int[] dizi, int aranan)
{
    for (int i = 0; i < dizi.Length; i++)
    {
        if (dizi[i] == aranan)
        {
            Console.WriteLine($"Sayı {i}. indexte bulundu.");
            return true; // Hem döngüden hem de metottan çık
        }
    }
    
    Console.WriteLine("Sayı bulunamadı!");
    return false;
}
```

## En İyi Kullanım Pratikleri

### break İçin
- Döngü içinde aradığınız değeri bulduğunuzda gereksiz işlemleri önlemek için kullanın.
- switch-case yapılarında her case bloğunun sonunda kullanın (C# 8.0'dan önce zorunludur).
- İç içe döngülerde sadece içteki döngüden çıkmak istediğinizde kullanın.

### continue İçin
- Kod okunabilirliğini artırmak için koşullu işlemleri basitleştirmek amacıyla kullanın.
- Kompleks if-else yapıları yerine, erken continue kullanımı tercih edin.
- Döngünün bir kısmını belirli koşullar altında atlamak için kullanın.

### return İçin
- Metodunuzun ana amacını gerçekleştirdiğinde kullanın.
- Hata durumlarında erken çıkış için kullanın.
- Performans için gereksiz işlemleri önlemek amacıyla erken return kullanın.
- Metodun sonunda tek bir return kullanmaya çalışarak kod okunabilirliğini artırın (bu her zaman mümkün olmayabilir).

## Özet

| İfade | Kullanım Alanı | Etki |
|-------|---------------|------|
| `break` | Döngüler, switch | İçinde bulunduğu döngü veya switch yapısını sonlandırır |
| `continue` | Döngüler | Mevcut iterasyonu atlayıp bir sonraki iterasyona geçer |
| `return` | Metotlar | Metodun çalışmasını sonlandırır ve opsiyonel olarak değer döndürür |

Bu akış kontrol ifadeleri, C# programlama dilinde kod akışını yönetmek ve daha verimli, okunabilir kod yazmak için güçlü araçlardır. Doğru yerde kullanıldıklarında, kodunuzun performansını ve netliğini önemli ölçüde artırabilirler.
