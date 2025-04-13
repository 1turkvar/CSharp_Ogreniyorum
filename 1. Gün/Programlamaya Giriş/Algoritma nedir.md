# **Algoritma Nedir? - C# Programlamaya Giriş**  

## **1. Algoritma Tanımı**  
**Algoritma**, bir problemi çözmek veya belirli bir görevi yerine getirmek için izlenen **adım adım mantıksal yöntemdir**. Bilgisayar bilimlerinde, programlama sürecinin temelini oluşturur.  

### **Özellikleri:**  
- **Kesinlik (Belirginlik):** Her adım açık ve net olmalıdır.  
- **Sonluluk:** Algoritma sınırlı sayıda adımdan oluşmalıdır.  
- **Girdi/Çıktı:** Veri almalı ve sonuç üretmelidir.  
- **Etkinlik:** Mümkün olan en az kaynakla çözüm sunmalıdır.  

---

## **2. Algoritma Örnekleri**  
### **Örnek 1: İki Sayının Toplamı**  
1. **Başla**  
2. Sayı1'i gir (Örn: 5)  
3. Sayı2'yi gir (Örn: 3)  
4. Toplam = Sayı1 + Sayı2  
5. Toplam'ı ekrana yaz  
6. **Bitir**  

### **Örnek 2: Bir Sayının Tek/Çift Olduğunu Bulma**  
1. **Başla**  
2. Sayı'yı gir (Örn: 8)  
3. Eğer Sayı % 2 == 0 ise  
   - Ekrana "Çift" yaz  
4. Değilse  
   - Ekrana "Tek" yaz  
5. **Bitir**  

---

## **3. Algoritma Gösterim Yöntemleri**  
### **a) Sözde Kod (Pseudocode)**  
Programlama diline benzeyen ancak daha serbest bir yazım şeklidir.  

```plaintext
FUNCTION Topla(sayi1, sayi2)
    sonuc = sayi1 + sayi2
    RETURN sonuc
END FUNCTION
```

### **b) Akış Şeması (Flowchart)**  
- **Başla/Bitir:** Oval  
- **Girdi/Çıktı:** Paralelkenar  
- **Karar Verme:** Elmas  
- **İşlemler:** Dikdörtgen  

Örnek:  
```
  [Başla]  
     |  
  [Sayı Gir]  
     |  
  [Sayı % 2 == 0?] -- Evet --> [Ekrana "Çift" Yaz]  
     |  
   Hayır  
     |  
  [Ekrana "Tek" Yaz]  
     |  
  [Bitir]  
```

---

## **4. C#'ta Algoritma Uygulaması**  
### **Örnek: İki Sayının Ortalamasını Hesaplama**  
```csharp
using System;

class Program
{
    static void Main()
    {
        Console.WriteLine("Birinci sayıyı girin:");
        int sayi1 = Convert.ToInt32(Console.ReadLine());

        Console.WriteLine("İkinci sayıyı girin:");
        int sayi2 = Convert.ToInt32(Console.ReadLine());

        double ortalama = (sayi1 + sayi2) / 2.0;
        Console.WriteLine("Ortalama: " + ortalama);
    }
}
```

---

## **5. Algoritma Analizi (Big-O Notasyonu)**  
Algoritmanın performansını ölçmek için kullanılır:  

| Algoritma       | Zaman Karmaşıklığı | Açıklama                     |
|-----------------|--------------------|------------------------------|
| Sabit Zaman     | O(1)               | Tek işlem (Örn: Dizi erişimi)|
| Doğrusal Arama  | O(n)               | Tüm elemanları kontrol etme  |
| İkili Arama     | O(log n)           | Sıralı dizide arama          |
| Bubble Sort     | O(n²)              | İç içe döngü kullanan sıralama |

---


### **Sonuç**  
Algoritmalar, programlamanın temelidir. C# gibi dillerde uygulanmadan önce mantıksal adımların doğru kurulması gerekir. Basit problemlerle başlayarak algoritmik düşünme becerinizi geliştirebilirsiniz.  

**✅ Ödev:** Klavyeden girilen 3 sayının en büyüğünü bulan algoritmayı yazın (Sözde Kod + C# Uygulaması).  

> **Not:** Daha fazla örnek ve detay için [Microsoft C# Dokümantasyonu](https://docs.microsoft.com/tr-tr/dotnet/csharp/)'na göz atabilirsiniz.  

**Keyifli kodlamalar!** 🚀