# **Pseudocode (Sözde Kod) Nedir?**  
**Pseudocode**, bir algoritmayı doğal bir dil (İngilizce, Türkçe vb.) ve basit programlama yapıları kullanarak ifade etme yöntemidir. Gerçek kod yazmadan önce mantığı planlamak için kullanılır.  

### **Pseudocode Avantajları**  
✔ **Dilden bağımsızdır** (C#, Java, Python fark etmez).  
✔ **Anlaşılması kolaydır** (Karmaşık sözdizimi kuralları yoktur).  
✔ **Algoritma tasarımını kolaylaştırır**.  

---

## **Pseudocode Kuralları ve Örnekler**  
### **1. Temel Yapılar**  
| Yapı          | Pseudocode Örneği       | Açıklama                     |
|--------------|------------------------|-----------------------------|
| **Değişken Tanımlama** | `sayi = 10`            | Değişken oluşturma.         |
| **Giriş/Çıkış**       | `Oku(ad)` veya `Yaz("Merhaba")` | Kullanıcıdan veri alma/ekrana yazdırma. |
| **Aritmetik İşlemler** | `toplam = sayi1 + sayi2` | Matematiksel işlemler.      |
| **Karar Yapıları**    | ```EĞER (koşul) İSE ... DEĞİLSE ...``` | `if-else` mantığı.          |
| **Döngüler**         | ```DÖNGÜ (koşul) ... DÖNGÜ SON``` | `while` veya `for` döngüsü. |

---

### **2. Kontrol Yapıları Örnekleri**  
#### **a) If-Else (Eğer-Değilse)**  
```plaintext
EĞER (not >= 50) İSE
    YAZ("Geçti")
DEĞİLSE
    YAZ("Kaldı")
```

#### **b) For Döngüsü**  
```plaintext
FOR i = 1'den 10'a KADAR
    YAZ(i)
```

#### **c) While Döngüsü**  
```plaintext
sayi = 1
DÖNGÜ (sayi <= 10) İKEN
    YAZ(sayi)
    sayi = sayi + 1
```

---

## **3. Gerçek Hayat Örneği: Ortalama Hesaplama**  
### **Pseudocode:**  
```plaintext
toplam = 0
FOR i = 1'den 5'e KADAR
    YAZ("Sayı girin: ")
    OKU(sayi)
    toplam = toplam + sayi
ortalama = toplam / 5
YAZ("Ortalama: ", ortalama)
```

### **C# Karşılığı:**  
```csharp
int toplam = 0;
for (int i = 1; i <= 5; i++)
{
    Console.Write("Sayı girin: ");
    int sayi = Convert.ToInt32(Console.ReadLine());
    toplam += sayi;
}
double ortalama = toplam / 5.0;
Console.WriteLine("Ortalama: " + ortalama);
```

---

## **4. Pseudocode → C# Dönüşüm Tablosu**  
| Pseudocode          | C# Karşılığı           |
|--------------------|-----------------------|
| `OKU(deger)`       | `Console.ReadLine()`  |
| `YAZ(metin)`       | `Console.WriteLine()` |
| `EĞER ... İSE`     | `if (...) { ... }`    |
| `DÖNGÜ (koşul)`    | `while (koşul)`       |

---

## **5. Alıştırmalar**  
1️⃣ **Faktöriyel Hesaplama**  
```plaintext
sonuc = 1
FOR i = 1'den n'e KADAR
    sonuc = sonuc * i
YAZ(sonuc)
```

2️⃣ **Çift Sayıları Yazdırma**  
```plaintext
FOR i = 1'den 100'e KADAR
    EĞER (i % 2 == 0) İSE
        YAZ(i)
``` 

---

**Sonuç:** Pseudocode, algoritma geliştirme sürecinde **hata yapma riskini azaltır** ve **kodun mantığını netleştirir**. C# öğrenirken bu yöntemi kullanarak daha verimli çalışabilirsiniz! 🚀  

> **Not:** Bu içerik **Markdown** formatındadır. PDF’e dönüştürmek için [**Markdown to PDF**](https://markdowntopdf.com/) gibi araçları kullanabilirsiniz.