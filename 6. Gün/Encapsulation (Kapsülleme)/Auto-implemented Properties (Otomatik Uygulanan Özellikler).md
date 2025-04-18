# C# Otomatik Uygulanan Özellikler (Auto-implemented Properties)

## İçindekiler
1. [Giriş](#giriş)
2. [Geleneksel Özellikler vs Otomatik Uygulanan Özellikler](#geleneksel-özellikler-vs-otomatik-uygulanan-özellikler)
3. [Sözdizimi ve Kullanım](#sözdizimi-ve-kullanım)
4. [İlk Değer Atama](#ilk-değer-atama)
5. [Salt Okunur Otomatik Özellikler](#salt-okunur-otomatik-özellikler)
6. [Expression-bodied Özellikler](#expression-bodied-özellikler)
7. [Init-only Özellikler (C# 9.0)](#init-only-özellikler)
8. [Record Tipler ve Özellikler](#record-tipler-ve-özellikler)
9. [En İyi Uygulamalar](#en-i̇yi-uygulamalar)
10. [Sık Karşılaşılan Hatalar](#sık-karşılaşılan-hatalar)
11. [Özet](#özet)

## Giriş

C# programlama dilinde otomatik uygulanan özellikler (auto-implemented properties), sınıf özelliklerini daha kısa ve öz bir şekilde tanımlamanıza olanak tanıyan bir özelliktir. Bu özellik C# 3.0 ile tanıtılmış olup, kod yazımını basitleştirerek geliştirici verimliliğini artırır.

Otomatik uygulanan özellikler, bir özelliğin arka planında saklama alanını (backing field) otomatik olarak oluşturup yönetir. Bu sayede kod tekrarını azaltır ve daha temiz bir kod yazmanıza yardımcı olur.

## Geleneksel Özellikler vs Otomatik Uygulanan Özellikler

### Geleneksel Özellikler

Geleneksel özellikler, bir özel alan (private field) ve bu alana erişimi düzenleyen get ve set metodlarından oluşur:

```csharp
public class Person
{
    private string _name; // Özel alan (backing field)
    
    public string Name
    {
        get { return _name; }
        set { _name = value; }
    }
}
```

### Otomatik Uygulanan Özellikler

Aynı işlevselliği otomatik uygulanan özellikler ile çok daha kısa bir şekilde yazabiliriz:

```csharp
public class Person
{
    public string Name { get; set; }
}
```

Bu durumda, C# derleyicisi arka planda otomatik olarak özel bir alan oluşturur ve get/set metodlarını bu alana erişecek şekilde yapılandırır.

## Sözdizimi ve Kullanım

Otomatik uygulanan özelliklerin temel sözdizimi şu şekildedir:

```csharp
[erişim_belirteci] [veri_tipi] [özellik_adı] { get; set; }
```

Örnek:

```csharp
public class Product
{
    // Temel otomatik uygulanan özellik
    public int Id { get; set; }
    
    // Farklı erişim belirteçleri ile
    public string Name { get; set; }
    public decimal Price { get; private set; }
    
    // Get ve set için farklı erişim belirteçleri
    public string Description { get; protected set; }
}
```

## İlk Değer Atama

C# 6.0 ve sonraki sürümlerde, otomatik uygulanan özelliklere doğrudan ilk değer atama yapabilirsiniz:

```csharp
public class Customer
{
    public int Id { get; set; } = 0;
    public string Name { get; set; } = "Misafir";
    public DateTime RegisterDate { get; set; } = DateTime.Now;
    public List<string> Orders { get; set; } = new List<string>();
}
```

## Salt Okunur Otomatik Özellikler

C# 6.0 ile birlikte, salt okunur otomatik özellikler tanımlayabilirsiniz. Bu özellikler yalnızca yapıcı metotta veya özelliğin tanımlandığı yerde değer alabilir:

```csharp
public class Employee
{
    // Salt okunur otomatik özellik
    public string EmployeeId { get; } = Guid.NewGuid().ToString();
    public string Department { get; }
    
    public Employee(string department)
    {
        // Yapıcı metotta değer atama
        Department = department;
    }
}
```

Salt okunur özellikler, değiştirilemez (immutable) sınıflar oluşturmak için özellikle faydalıdır.

## Expression-bodied Özellikler

C# 6.0 ve sonraki sürümlerde, expression-bodied üyeler kullanarak özellikleri daha kısa bir şekilde tanımlayabilirsiniz:

```csharp
public class Circle
{
    public double Radius { get; set; }
    
    // Expression-bodied salt okunur özellik
    public double Diameter => Radius * 2;
    public double Circumference => 2 * Math.PI * Radius;
    public double Area => Math.PI * Radius * Radius;
}
```

## Init-only Özellikler (C# 9.0)

C# 9.0 ile birlikte, "init" erişimcisi tanıtıldı. Bu, nesne başlatma sırasında değer atanabilen ancak daha sonra değiştirilemeyen özellikleri tanımlamanıza olanak tanır:

```csharp
public class Person
{
    public string FirstName { get; init; }
    public string LastName { get; init; }
    public DateTime DateOfBirth { get; init; }
    
    // Hesaplanan özellik
    public int Age => DateTime.Now.Year - DateOfBirth.Year;
}
```

Bu özelliklere nesne başlatma sözdizimi kullanarak değer atayabilirsiniz:

```csharp
var person = new Person 
{
    FirstName = "Ahmet",
    LastName = "Yılmaz",
    DateOfBirth = new DateTime(1990, 5, 15)
};

// Aşağıdaki kod derleme hatası verir
// person.FirstName = "Mehmet";
```

## Record Tipler ve Özellikler

C# 9.0 ile tanıtılan record tipleri, varsayılan olarak init-only özelliklerle çalışacak şekilde tasarlanmıştır:

```csharp
public record Employee
{
    public string Id { get; init; }
    public string Name { get; init; }
    public string Department { get; init; }
}
```

Record'lar, değişmez veri modelleri oluşturmak için idealdir ve otomatik olarak eşitlik karşılaştırması, değer kopyalama ve diğer yararlı özellikleri sağlar.

## En İyi Uygulamalar

Otomatik uygulanan özellikler kullanırken aşağıdaki en iyi uygulamaları göz önünde bulundurun:

1. **Kapsülleme İlkesine Uyun**: Özelliklerinizdeki get ve set erişimcilerinin doğru erişim belirteçlerine sahip olduğundan emin olun.

```csharp
// İyi uygulama
public class BankAccount
{
    public string AccountNumber { get; }  // Salt okunur
    public decimal Balance { get; private set; }  // Dışarıdan yalnızca okunabilir
    
    public BankAccount(string accountNumber)
    {
        AccountNumber = accountNumber;
        Balance = 0;
    }
    
    public void Deposit(decimal amount)
    {
        if (amount <= 0)
            throw new ArgumentException("Miktar pozitif olmalı");
            
        Balance += amount;
    }
}
```

2. **Özelliklerle Mantıksal İşlemler**: Eğer bir özelliğin get veya set kısmında karmaşık mantık gerekiyorsa, tam uygulanan özellik kullanın:

```csharp
private string _email;
public string Email
{
    get { return _email; }
    set 
    { 
        if (string.IsNullOrEmpty(value) || !value.Contains("@"))
            throw new ArgumentException("Geçersiz e-posta adresi");
            
        _email = value; 
    }
}
```

3. **İlk Değer Atama**: İlk değer ataması için yapıcı metot veya özellik başlatma sözdizimini tutarlı bir şekilde kullanın.

## Sık Karşılaşılan Hatalar

Otomatik uygulanan özellikler kullanırken dikkat edilmesi gereken yaygın hatalar:

1. **Doğrulama Eksikliği**: Otomatik özellikler doğrulama mantığı içermez. Veri doğrulama gerektiğinde tam uygulanan özellikler kullanılmalıdır.

2. **Yanlış Kapsülleme**: Tüm özelliklerin public get; public set; olarak tanımlanması, nesnenin durumunu kontrolsüz bir şekilde değiştirilebilir hale getirir.

3. **İçsel Değişikliklerin Bildirilmemesi**: Özellik değişikliklerinde bildirim (notification) gerektiğinde, INotifyPropertyChanged arabirimini uygulamanız gerekir.

```csharp
public class ViewModel : INotifyPropertyChanged
{
    private string _name;
    
    public event PropertyChangedEventHandler PropertyChanged;
    
    public string Name
    {
        get => _name;
        set
        {
            if (_name != value)
            {
                _name = value;
                PropertyChanged?.Invoke(this, new PropertyChangedEventArgs(nameof(Name)));
            }
        }
    }
}
```

## Özet

C# otomatik uygulanan özellikler:

- Kod tekrarını azaltır ve daha temiz bir kod yazmanıza yardımcı olur
- Arka planda saklama alanlarını otomatik olarak yönetir
- C# 6.0 ve sonraki sürümlerde ilk değer atama desteği sunar
- C# 9.0 ile "init" erişimcisi ve değişmez özellikler sağlar
- Record tipleriyle birlikte değişmez veri modelleri oluşturmayı kolaylaştırır

Otomatik uygulanan özellikler, C#'ın en kullanışlı özelliklerinden biridir ve modern C# kod stilinin önemli bir parçasıdır. Bu özelliği doğru bir şekilde kullanarak, daha kısa, öz ve bakımı kolay kod yazabilirsiniz.
