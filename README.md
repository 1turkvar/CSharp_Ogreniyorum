
# 📝 C# Çalışma Günlüğü

Merhaba!  
Bu repo, C# öğrenme sürecimi belgelediğim bir çalışma günlüğüdür. Hedefim, hem öğrendiklerimi kalıcı hale getirmek hem de bu süreci benim gibi yeni başlayanlarla paylaşmak. 🚀

## 🎯 Amaçlarım
- C# dilini temelden ileri seviyeye öğrenmek
- Her gün düzenli olarak küçük notlar ve örnek projeler eklemek
- Konsol uygulamalarıyla başlayıp ileride .NET ve MAUI projelerine geçmek
- Öğrendiklerimi paylaşarak kendime ait bir kaynak oluşturmak

## 📚 İçerik
| Tarih | Konu | Açıklama |
|------|------|----------|
| ... | 1- Değişkenler ve Veri Tipleri | .... |
| ... | ... | ... |

## 📂 Klasör Yapısı
--


⭐ Faydalı bulduysan repo'yu yıldızlamayı unutma!


# **Derinlemesine ve Sistematik C# Öğrenim Programı**  
*Sıfırdan ileri seviyeye adım adım C# öğrenme rehberi*  

Bu program, **temelden ileri düzeye** C# öğrenmek isteyenler için **detaylı konu anlatımları, örnek uygulamalar, dikkat edilmesi gereken noktalar ve öğrenme hedefleri** içermektedir.  

## **Programın Yapısı**  
1. **Temel Seviye (Hafta 1-4)**  
2. **Orta Seviye (Hafta 5-8)**  
3. **İleri Seviye (Hafta 9-12)**  
4. **Uzmanlık ve Proje Geliştirme (Hafta 13-16)**  

---

# **📌 1. Hafta: Temel Kavramlar ve Programlaya Giriş**  
### **Öğrenme Hedefleri:**  
✔️ Programlama mantığını anlamak  
✔️ Visual Studio ve .NET ekosistemini tanımak  
✔️ Temel veri tiplerini ve operatörleri kullanabilmek  

### **Konu Başlıkları:**  
- **Programlamaya Giriş**  
  - Algoritma nedir?  
  - Pseudocode (Sözde Kod) yazma  
- **C# ve .NET Tanıtımı**  
  - .NET Framework vs .NET Core vs .NET 5/6/7+  
  - Visual Studio Kurulumu  
- **İlk C# Programı (Hello World)**  
  - `Console.WriteLine()` ve `Console.ReadLine()`  
  - `Main()` metodu ve yapısı  
- **Değişkenler ve Veri Tipleri**  
  - `int`, `double`, `string`, `bool`  
  - `var` anahtar kelimesi  
- **Operatörler**  
  - Aritmetik (`+`, `-`, `*`, `/`, `%`)  
  - Karşılaştırma (`==`, `!=`, `>`, `<`)  
  - Mantıksal (`&&`, `||`, `!`)  

### **Dikkat Edilmesi Gerekenler:**  
⚠️ Değişken isimlendirme kuralları (`camelCase`, `PascalCase`)  
⚠️ Veri tipi dönüşümleri (`Convert.ToInt32()`, `Parse()`)  

### **Uygulama Örneği:**  
```csharp
// Kullanıcıdan iki sayı alıp toplama
Console.Write("1. Sayıyı Girin: ");
int sayi1 = Convert.ToInt32(Console.ReadLine());
Console.Write("2. Sayıyı Girin: ");
int sayi2 = Convert.ToInt32(Console.ReadLine());
Console.WriteLine($"Toplam: {sayi1 + sayi2}");
```

---

# **📌 2. Hafta: Kontrol Yapıları ve Döngüler**  
### **Öğrenme Hedefleri:**  
✔️ `if-else` ve `switch-case` yapılarını kullanabilmek  
✔️ `for`, `while`, `do-while` döngülerini öğrenmek  

### **Konu Başlıkları:**  
- **Koşul İfadeleri**  
  - `if`, `else if`, `else`  
  - Ternary operatör (`? :`)  
  - `switch-case` yapısı  
- **Döngüler**  
  - `for` döngüsü  
  - `while` ve `do-while`  
  - Sonsuz döngü ve `break`, `continue`  

### **Uygulama Örneği:**  
```csharp
// 1'den 100'e kadar tek sayıları yazdırma
for (int i = 1; i <= 100; i++)
{
    if (i % 2 != 0)
        Console.WriteLine(i);
}
```

---

# **📌 3. Hafta: Metotlar ve Diziler**  
### **Öğrenme Hedefleri:**  
✔️ Metot oluşturma ve kullanma  
✔️ Dizi ve koleksiyonlarla çalışma  

### **Konu Başlıkları:**  
- **Metotlar (Fonksiyonlar)**  
  - `void` ve return tipindeki metotlar  
  - Parametreli ve parametresiz metotlar  
  - `ref` ve `out` anahtar kelimeleri  
- **Diziler (Arrays)**  
  - Tek boyutlu ve çok boyutlu diziler  
  - `Array.Sort()`, `Array.Reverse()`  

### **Uygulama Örneği:**  
```csharp
// Dizideki en büyük sayıyı bulan metot
static int EnBuyukSayi(int[] dizi)
{
    int enBuyuk = dizi[0];
    foreach (int sayi in dizi)
    {
        if (sayi > enBuyuk)
            enBuyuk = sayi;
    }
    return enBuyuk;
}
```

---

# **📌 4. Hafta: Nesne Yönelimli Programlama (OOP) - Giriş**  
### **Öğrenme Hedefleri:**  
✔️ Sınıf ve nesne kavramlarını anlamak  
✔️ `public`, `private`, `protected` erişim belirleyicileri  

### **Konu Başlıkları:**  
- **Sınıf (Class) ve Nesne (Object)**  
  - Constructor (Yapıcı Metot)  
  - Property (Özellikler)  
- **Encapsulation (Kapsülleme)**  
  - Getter-Setter metodları  
  - Auto-implemented properties  

### **Uygulama Örneği:**  
```csharp
class Ogrenci
{
    public string Ad { get; set; }
    public int Yas { get; set; }

    public void BilgileriYazdir()
    {
        Console.WriteLine($"Ad: {Ad}, Yaş: {Yas}");
    }
}

// Kullanım
Ogrenci ogr = new Ogrenci();
ogr.Ad = "Ahmet";
ogr.Yas = 20;
ogr.BilgileriYazdir();
```

---

# **📌 5-8. Hafta: Orta Seviye C# Konuları**  
✔️ **Hafta 5:** Kalıtım (Inheritance) ve Polimorfizm  
✔️ **Hafta 6:** Arayüzler (Interfaces) ve Soyut Sınıflar (Abstract Classes)  
✔️ **Hafta 7:** Hata Yönetimi (Exception Handling - `try-catch-finally`)  
✔️ **Hafta 8:** Delegeler (Delegates) ve Olaylar (Events)  

---

# **📌 9-12. Hafta: İleri Seviye C# Konuları**  
✔️ **Hafta 9:** LINQ (Language Integrated Query)  
✔️ **Hafta 10:** Asenkron Programlama (`async-await`)  
✔️ **Hafta 11:** Dosya İşlemleri (`File`, `StreamReader`, `StreamWriter`)  
✔️ **Hafta 12:** Entity Framework Core (Temel ORM)  

---

# **📌 13-16. Hafta: Proje Geliştirme ve Uzmanlık**  
✔️ **Hafta 13:** Console Uygulaması (Öğrenci Kayıt Sistemi)  
✔️ **Hafta 14:** Windows Forms veya WPF Uygulaması  
✔️ **Hafta 15:** Web API Geliştirme (ASP.NET Core)  
✔️ **Hafta 16:** Veritabanı Entegrasyonlu Proje (SQL Server + EF Core)  

---
