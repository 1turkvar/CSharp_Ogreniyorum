# C# While ve Do-While Döngüleri - Kapsamlı Rehber

## İçindekiler
1. [Giriş](#giriş)
2. [While Döngüsü](#while-döngüsü)
   - [Sözdizimi ve Yapısı](#while-sözdizimi-ve-yapısı)
   - [Çalışma Mantığı](#while-çalışma-mantığı)
   - [Temel Örnekler](#while-temel-örnekler)
   - [İleri Düzey Örnekler](#while-ileri-düzey-örnekler)
3. [Do-While Döngüsü](#do-while-döngüsü)
   - [Sözdizimi ve Yapısı](#do-while-sözdizimi-ve-yapısı)
   - [Çalışma Mantığı](#do-while-çalışma-mantığı)
   - [Temel Örnekler](#do-while-temel-örnekler)
   - [İleri Düzey Örnekler](#do-while-ileri-düzey-örnekler)
4. [While vs. Do-While Karşılaştırması](#while-vs-do-while-karşılaştırması)
5. [Sonsuz Döngüler ve Dikkat Edilmesi Gerekenler](#sonsuz-döngüler-ve-dikkat-edilmesi-gerekenler)
6. [Break ve Continue İfadeleri](#break-ve-continue-ifadeleri)
7. [Yaygın Kullanım Senaryoları](#yaygın-kullanım-senaryoları)
8. [İyi Uygulama Örnekleri](#iyi-uygulama-örnekleri)
9. [Özet](#özet)

## Giriş

Döngüler, programlamada belirli bir kod bloğunun bir koşul sağlandığı sürece tekrarlanmasını sağlayan yapılardır. C# dilinde döngüler, tekrarlayan görevleri otomatize etmek için kullanılır. Bu dokümanda, C#'ın iki önemli döngü yapısı olan `while` ve `do-while` döngülerini detaylı bir şekilde inceleyeceğiz.

## While Döngüsü

### While Sözdizimi ve Yapısı

`while` döngüsü, belirtilen koşul `true` olduğu sürece kod bloğunu çalıştırır. Sözdizimi şu şekildedir:

```csharp
while (koşul)
{
    // Koşul true olduğu sürece çalıştırılacak kod bloğu
}
```

Burada:
- `koşul`: Her döngü iterasyonundan önce değerlendirilen bir boolean ifadesidir.
- Kod bloğu: Koşul `true` olduğu sürece çalıştırılacak komutları içerir.

### While Çalışma Mantığı

1. `while` döngüsü önce belirtilen koşulu değerlendirir.
2. Eğer koşul `true` ise, döngü gövdesindeki kodlar çalıştırılır.
3. Kod bloğu çalıştırıldıktan sonra, kontrol tekrar koşula döner.
4. Koşul hala `true` ise, bu süreç tekrarlanır.
5. Koşul `false` olduğunda, döngü sona erer ve program akışı döngüden sonraki ifadelere geçer.

### While Temel Örnekler

**Örnek 1: Sayaç ile 1'den 5'e kadar sayıları yazdırma**

```csharp
int i = 1;
while (i <= 5)
{
    Console.WriteLine(i);
    i++;
}
```

Çıktı:
```
1
2
3
4
5
```

**Örnek 2: Kullanıcıdan pozitif bir sayı alma**

```csharp
int sayi = 0;
while (sayi <= 0)
{
    Console.Write("Lütfen pozitif bir sayı girin: ");
    int.TryParse(Console.ReadLine(), out sayi);
    
    if (sayi <= 0)
    {
        Console.WriteLine("Geçersiz giriş! Pozitif bir sayı giriniz.");
    }
}
Console.WriteLine($"Girdiğiniz pozitif sayı: {sayi}");
```

### While İleri Düzey Örnekler

**Örnek 3: Bir sayının faktöriyelini hesaplama**

```csharp
int sayi = 5;
int faktoriyel = 1;
int i = 1;

while (i <= sayi)
{
    faktoriyel *= i;
    i++;
}

Console.WriteLine($"{sayi}! = {faktoriyel}");
```

Çıktı:
```
5! = 120
```

**Örnek 4: Fibonacci serisinin ilk n terimini hesaplama**

```csharp
int n = 10;
int a = 0, b = 1;
int count = 0;

Console.WriteLine("Fibonacci Serisi:");

while (count < n)
{
    Console.Write($"{a} ");
    int temp = a;
    a = b;
    b = temp + b;
    count++;
}
```

Çıktı:
```
Fibonacci Serisi:
0 1 1 2 3 5 8 13 21 34
```

## Do-While Döngüsü

### Do-While Sözdizimi ve Yapısı

`do-while` döngüsü, kod bloğunu en az bir kez çalıştırdıktan sonra koşulu değerlendirir ve koşul `true` olduğu sürece devam eder. Sözdizimi şu şekildedir:

```csharp
do
{
    // En az bir kez çalıştırılacak kod bloğu
} while (koşul);
```

Burada:
- Kod bloğu: Her durumda en az bir kez çalıştırılacak komutları içerir.
- `koşul`: Her döngü iterasyonundan sonra değerlendirilen bir boolean ifadesidir.

### Do-While Çalışma Mantığı

1. `do-while` döngüsünde önce kod bloğu çalıştırılır.
2. Ardından koşul değerlendirilir.
3. Eğer koşul `true` ise, döngü tekrarlanır ve adım 1'e geri dönülür.
4. Koşul `false` olduğunda, döngü sona erer ve program akışı bir sonraki ifadeye geçer.

### Do-While Temel Örnekler

**Örnek 1: Sayaç ile 1'den 5'e kadar sayıları yazdırma**

```csharp
int i = 1;
do
{
    Console.WriteLine(i);
    i++;
} while (i <= 5);
```

Çıktı:
```
1
2
3
4
5
```

**Örnek 2: Kullanıcıdan doğru bir şifre alana kadar devam etme**

```csharp
string dogruSifre = "abc123";
string girilenSifre;

do
{
    Console.Write("Lütfen şifrenizi girin: ");
    girilenSifre = Console.ReadLine();
    
    if (girilenSifre != dogruSifre)
    {
        Console.WriteLine("Yanlış şifre! Tekrar deneyin.");
    }
} while (girilenSifre != dogruSifre);

Console.WriteLine("Giriş başarılı!");
```

### Do-While İleri Düzey Örnekler

**Örnek 3: Menü tabanlı bir uygulamada kullanıcı çıkış yapana kadar işlemlere devam etme**

```csharp
int secim;

do
{
    Console.WriteLine("\nMenü:");
    Console.WriteLine("1. Toplama");
    Console.WriteLine("2. Çıkarma"); 
    Console.WriteLine("3. Çarpma");
    Console.WriteLine("4. Bölme");
    Console.WriteLine("0. Çıkış");
    Console.Write("Seçiminiz: ");
    
    if (!int.TryParse(Console.ReadLine(), out secim))
    {
        Console.WriteLine("Geçersiz giriş! Tekrar deneyin.");
        secim = -1;
        continue;
    }
    
    switch (secim)
    {
        case 1:
            Console.WriteLine("Toplama işlemi seçildi");
            break;
        case 2:
            Console.WriteLine("Çıkarma işlemi seçildi");
            break;
        case 3:
            Console.WriteLine("Çarpma işlemi seçildi");
            break;
        case 4:
            Console.WriteLine("Bölme işlemi seçildi");
            break;
        case 0:
            Console.WriteLine("Programdan çıkılıyor...");
            break;
        default:
            Console.WriteLine("Geçersiz seçim! Tekrar deneyin.");
            break;
    }
} while (secim != 0);
```

**Örnek 4: Rastgele bir sayıyı tahmin etme oyunu**

```csharp
Random rnd = new Random();
int hedefSayi = rnd.Next(1, 101);  // 1-100 arası rastgele sayı
int tahmin;
int denemeSayisi = 0;

Console.WriteLine("1 ile 100 arasında bir sayıyı tahmin edin!");

do
{
    Console.Write("Tahmininiz: ");
    if (!int.TryParse(Console.ReadLine(), out tahmin))
    {
        Console.WriteLine("Geçersiz giriş! Lütfen bir sayı girin.");
        continue;
    }
    
    denemeSayisi++;
    
    if (tahmin < hedefSayi)
    {
        Console.WriteLine("Daha büyük bir sayı girin.");
    }
    else if (tahmin > hedefSayi)
    {
        Console.WriteLine("Daha küçük bir sayı girin.");
    }
} while (tahmin != hedefSayi);

Console.WriteLine($"Tebrikler! {hedefSayi} sayısını {denemeSayisi} denemede buldunuz.");
```

## While vs. Do-While Karşılaştırması

| Özellik | While | Do-While |
|---------|-------|----------|
| Koşul değerlendirme zamanı | Döngüye girmeden önce | Bir iterasyon tamamlandıktan sonra |
| Minimum çalışma sayısı | 0 (koşul başlangıçta false ise hiç çalışmaz) | 1 (koşul sonradan kontrol edildiği için en az bir kez çalışır) |
| Kullanım alanı | Koşula bağlı olarak hiç çalışmaması gereken durumlarda | En az bir kez çalıştırılması gereken durumlarda |

**While döngüsü için uygun senaryolar:**
- Bir dosyadan veri okurken dosya sonuna gelinip gelinmediğini kontrol etme
- Belirli bir koşul sağlanana kadar işlem yapma
- Koşul sağlanmıyorsa hiç işlem yapmadan geçme

**Do-While döngüsü için uygun senaryolar:**
- Menü sistemleri oluşturma
- Kullanıcıdan en az bir kez veri alma
- Doğrulama işlemleri yapma

## Sonsuz Döngüler ve Dikkat Edilmesi Gerekenler

Sonsuz döngüler, koşulun hiçbir zaman `false` değerine ulaşmadığı döngülerdir. Bazen bu tür döngüler kasıtlı olarak oluşturulabilir, ancak genellikle bir programlama hatasının sonucudur.

**While ile sonsuz döngü:**
```csharp
while (true)
{
    // Sonsuz döngü - çıkış için break ifadesi kullanılmalı
    Console.WriteLine("Sonsuz döngü");
    
    // Örnek çıkış koşulu
    if (Console.KeyAvailable && Console.ReadKey(true).Key == ConsoleKey.Escape)
    {
        break;
    }
}
```

**Do-While ile sonsuz döngü:**
```csharp
do
{
    // Sonsuz döngü - çıkış için break ifadesi kullanılmalı
    Console.WriteLine("Sonsuz döngü");
    
    // Örnek çıkış koşulu
    if (Console.KeyAvailable && Console.ReadKey(true).Key == ConsoleKey.Escape)
    {
        break;
    }
} while (true);
```

**Dikkat Edilmesi Gereken Noktalar:**
1. Döngü koşulunun bir noktada `false` olacağından emin olun
2. Sonsuz döngülerde mutlaka bir çıkış mekanizması (örn. `break`) bulundurun
3. Sayaç kullanıyorsanız, sayacın doğru şekilde artırıldığından/azaltıldığından emin olun
4. Döngü içinde değiştirilmeyen bir koşulun sonsuz döngüye neden olabileceğini unutmayın

## Break ve Continue İfadeleri

### Break
`break` ifadesi, bir döngüyü anında sonlandırmak için kullanılır. Döngüde belirli bir koşul oluştuğunda döngüden çıkmak istediğinizde kullanışlıdır.

```csharp
int i = 1;
while (i <= 10)
{
    Console.WriteLine(i);
    if (i == 5)
    {
        Console.WriteLine("5'e ulaştık, döngüden çıkıyoruz.");
        break;  // 5'e ulaştığımızda döngüyü sonlandır
    }
    i++;
}
```

Çıktı:
```
1
2
3
4
5
5'e ulaştık, döngüden çıkıyoruz.
```

### Continue
`continue` ifadesi, döngünün mevcut iterasyonunu atlar ve bir sonraki iterasyona geçer. Belirli koşullarda döngünün bir kısmını atlamak istediğinizde kullanışlıdır.

```csharp
int i = 0;
while (i < 10)
{
    i++;
    if (i % 2 == 0)
    {
        continue;  // Çift sayılar için döngünün geri kalanını atla
    }
    Console.WriteLine(i);  // Sadece tek sayılar yazdırılır
}
```

Çıktı:
```
1
3
5
7
9
```

## Yaygın Kullanım Senaryoları

### While Döngüsü için Yaygın Senaryolar:

1. **Liste veya dizi üzerinde işlem yapma:**
   ```csharp
   List<int> sayilar = new List<int> { 1, 3, 5, 7, 9 };
   int index = 0;
   
   while (index < sayilar.Count)
   {
       Console.WriteLine($"Sayı: {sayilar[index]}");
       index++;
   }
   ```

2. **Dosya okuma işlemleri:**
   ```csharp
   using System.IO;
   
   StreamReader sr = new StreamReader("dosya.txt");
   string satir;
   
   while ((satir = sr.ReadLine()) != null)
   {
       Console.WriteLine(satir);
   }
   
   sr.Close();
   ```

3. **Belirli bir koşul sağlanana kadar işlem yapma:**
   ```csharp
   Random rnd = new Random();
   int zar;
   
   while ((zar = rnd.Next(1, 7)) != 6)
   {
       Console.WriteLine($"Zar: {zar}, 6 gelmedi, tekrar atıyoruz.");
   }
   
   Console.WriteLine("Zar: 6 geldi, döngüden çıkıyoruz.");
   ```

### Do-While Döngüsü için Yaygın Senaryolar:

1. **Kullanıcı doğrulama işlemleri:**
   ```csharp
   string parola;
   bool gecerli;
   
   do
   {
       Console.Write("Parolanızı girin (en az 6 karakter): ");
       parola = Console.ReadLine();
       
       gecerli = parola.Length >= 6;
       
       if (!gecerli)
       {
           Console.WriteLine("Parola çok kısa!");
       }
   } while (!gecerli);
   
   Console.WriteLine("Geçerli parola girdiniz.");
   ```

2. **Menü sistemleri:**
   ```csharp
   int secim;
   
   do
   {
       Console.WriteLine("\n=== Uygulama Menüsü ===");
       Console.WriteLine("1. Yeni Kayıt Ekle");
       Console.WriteLine("2. Kayıtları Listele");
       Console.WriteLine("3. Kayıt Ara");
       Console.WriteLine("0. Çıkış");
       Console.Write("Seçiminiz: ");
       
       int.TryParse(Console.ReadLine(), out secim);
       
       // Seçime göre işlem yap
       switch (secim)
       {
           case 1:
               Console.WriteLine("Yeni kayıt ekleniyor...");
               break;
           case 2:
               Console.WriteLine("Kayıtlar listeleniyor...");
               break;
           case 3:
               Console.WriteLine("Kayıt arama işlemi yapılıyor...");
               break;
           case 0:
               Console.WriteLine("Programdan çıkılıyor...");
               break;
           default:
               Console.WriteLine("Geçersiz seçim!");
               break;
       }
   } while (secim != 0);
   ```

3. **Oyun döngüleri:**
   ```csharp
   bool oyunDevamEdiyor = true;
   int puan = 0;
   
   do
   {
       Console.WriteLine($"Mevcut puan: {puan}");
       Console.WriteLine("1. Oyna");
       Console.WriteLine("2. Çıkış");
       
       int secim;
       int.TryParse(Console.ReadLine(), out secim);
       
       switch (secim)
       {
           case 1:
               int kazanilanPuan = new Random().Next(1, 11);
               puan += kazanilanPuan;
               Console.WriteLine($"{kazanilanPuan} puan kazandınız!");
               break;
           case 2:
               oyunDevamEdiyor = false;
               break;
       }
   } while (oyunDevamEdiyor);
   
   Console.WriteLine($"Oyun bitti. Toplam puanınız: {puan}");
   ```

## İyi Uygulama Örnekleri

1. **Döngü koşullarını basit ve anlaşılır tutun**
   ```csharp
   // Kötü örnek
   while (i < 10 && j > 0 && !isDone && (x == y || z != 0))
   
   // İyi örnek
   bool kosuluKarsilaniyor = i < 10 && j > 0 && !isDone && (x == y || z != 0);
   while (kosuluKarsilaniyor)
   ```

2. **Sonsuz döngülerde mutlaka çıkış koşulu belirleyin**
   ```csharp
   while (true)
   {
       // İşlemler
       
       if (cikisKosulu)
       {
           break;  // Çıkış koşulu sağlandığında döngüden çık
       }
   }
   ```

3. **Sayaç değişkenlerini doğru şekilde güncelleyin**
   ```csharp
   int i = 0;
   while (i < 10)
   {
       // İşlemler
       
       i++;  // Sayacı güncellemeyi unutmayın
   }
   ```

4. **Karmaşık döngülerden kaçının**
   ```csharp
   // Karmaşık iç içe döngüler yerine
   // metotlara bölerek kodu daha okunaklı hale getirin
   void KarmasikIslem()
   {
       while (kosul1)
       {
           while (kosul2)
           {
               // Karmaşık iç içe işlemler
           }
       }
   }
   
   // Bunun yerine:
   void DisIslem()
   {
       while (kosul1)
       {
           IcIslem();
       }
   }
   
   void IcIslem()
   {
       while (kosul2)
       {
           // İşlemler
       }
   }
   ```

5. **Performans için döngü koşullarını optimize edin**
   ```csharp
   // Kötü örnek - Her iterasyonda Collection.Count hesaplanır
   int i = 0;
   while (i < myCollection.Count)
   {
       // İşlemler
       i++;
   }
   
   // İyi örnek - Count değeri bir kez hesaplanır
   int count = myCollection.Count;
   int i = 0;
   while (i < count)
   {
       // İşlemler
       i++;
   }
   ```

## Özet

- **While Döngüsü**: Koşul true olduğu sürece kod bloğunu çalıştırır. Koşul başlangıçta false ise, döngü hiç çalışmaz.
  ```csharp
  while (koşul)
  {
      // Kod bloğu
  }
  ```

- **Do-While Döngüsü**: Kod bloğunu en az bir kez çalıştırır ve ardından koşul true olduğu sürece döngüye devam eder.
  ```csharp
  do
  {
      // Kod bloğu
  } while (koşul);
  ```

- **Temel Farklılıklar**:
  - While, koşulu başta kontrol eder
  - Do-While, koşulu sonda kontrol eder
  - While, hiç çalışmayabilir
  - Do-While, en az bir kez çalışır

- **Önemli Kullanım Alanları**:
  - Veri doğrulama
  - Kullanıcı girişi alma
  - Menü sistemleri
  - Dosya işlemleri
  - Belirli bir koşul sağlanana kadar tekrar etme

- **İyi Uygulamalar**:
  - Sonsuz döngülerde çıkış koşulu belirleme
  - Sayaçları doğru şekilde güncelleme
  - Karmaşık koşulları anlaşılır hale getirme
  - Döngü içeriğini optimize etme

Döngüleri doğru şekilde kullanarak, kodunuzu daha verimli ve okunaklı hale getirebilirsiniz. Her iki döngü tipinin avantajları ve dezavantajları vardır, bu nedenle doğru senaryoda doğru döngü tipini seçmek önemlidir.
