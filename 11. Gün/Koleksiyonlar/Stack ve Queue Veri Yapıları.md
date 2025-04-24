avantajları vardır. Doğru senaryo için doğru veri yapısını seçmek, yazılım geliştirme sürecinde kritik bir karardır.

## Gelişmiş Konular

### Eşzamanlı Koleksiyonlar

.NET Framework'ün `System.Collections.Concurrent` namespace'inde, çok iş parçacıklı uygulamalar için thread-safe versiyonları bulunur:

```csharp
using System;
using System.Collections.Concurrent;
using System.Threading;
using System.Threading.Tasks;

class ConcurrentCollectionsDemo
{
    static void Main()
    {
        // ConcurrentStack örneği
        ConcurrentStack<int> concurrentStack = new ConcurrentStack<int>();
        
        // ConcurrentQueue örneği
        ConcurrentQueue<int> concurrentQueue = new ConcurrentQueue<int>();
        
        // Paralel olarak stack'e öğe ekleme
        Parallel.For(0, 10, (i) => {
            concurrentStack.Push(i);
            Console.WriteLine($"Thread {Thread.CurrentThread.ManagedThreadId} stack'e {i} ekledi.");
        });
        
        // Paralel olarak queue'ya öğe ekleme
        Parallel.For(0, 10, (i) => {
            concurrentQueue.Enqueue(i);
            Console.WriteLine($"Thread {Thread.CurrentThread.ManagedThreadId} queue'ya {i} ekledi.");
        });
        
        // Stack'ten öğeleri çıkarma
        Console.WriteLine("\nStack'ten öğeleri çıkarma:");
        int item;
        while (concurrentStack.TryPop(out item))
        {
            Console.WriteLine($"Stack'ten çıkarılan: {item}");
        }
        
        // Queue'dan öğeleri çıkarma
        Console.WriteLine("\nQueue'dan öğeleri çıkarma:");
        while (concurrentQueue.TryDequeue(out item))
        {
            Console.WriteLine($"Queue'dan çıkarılan: {item}");
        }
    }
}
```

### Linq ile Stack ve Queue Kullanımı

```csharp
using System;
using System.Collections.Generic;
using System.Linq;

class LinqWithStacksAndQueues
{
    static void Main()
    {
        // Stack ile LINQ kullanımı
        Stack<int> stack = new Stack<int>();
        for (int i = 1; i <= 10; i++)
            stack.Push(i);
            
        // Stack üzerinde filtreleme
        var evenNumbers = stack.Where(n => n % 2 == 0);
        Console.WriteLine("Stack'teki çift sayılar:");
        foreach (var num in evenNumbers)
            Console.Write($"{num} ");
        Console.WriteLine();
        
        // Stack elemanları üzerinde işlem yapma
        var squared = stack.Select(n => n * n);
        Console.WriteLine("\nStack elemanlarının kareleri:");
        foreach (var num in squared)
            Console.Write($"{num} ");
        Console.WriteLine();
        
        // Queue ile LINQ kullanımı
        Queue<string> queue = new Queue<string>();
        queue.Enqueue("Elma");
        queue.Enqueue("Armut");
        queue.Enqueue("Kiraz");
        queue.Enqueue("Ananas");
        queue.Enqueue("Muz");
        
        // Queue üzerinde filtreleme
        var aWithFruits = queue.Where(f => f.StartsWith("A"));
        Console.WriteLine("\nA ile başlayan meyveler:");
        foreach (var fruit in aWithFruits)
            Console.Write($"{fruit} ");
        Console.WriteLine();
        
        // Queue elemanlarını sıralama (queue'nun kendi sıralamasını etkilemez)
        var sortedFruits = queue.OrderBy(f => f);
        Console.WriteLine("\nMeyveler alfabetik sırayla:");
        foreach (var fruit in sortedFruits)
            Console.Write($"{fruit} ");
        Console.WriteLine();
        
        // Hem filtreleme hem dönüştürme
        var processedFruits = queue
            .Where(f => f.Length > 4)
            .Select(f => f.ToUpper());
        Console.WriteLine("\n4 karakterden uzun meyveler (büyük harfle):");
        foreach (var fruit in processedFruits)
            Console.Write($"{fruit} ");
        Console.WriteLine();
    }
}
```

### Immutable (Değiştirilemez) Stack ve Queue

.NET 'in `System.Collections.Immutable` paketindeki immutable koleksiyonlar:

```csharp
using System;
using System.Collections.Immutable;

class ImmutableCollectionsDemo
{
    static void Main()
    {
        // Immutable Stack oluşturma
        ImmutableStack<int> immutableStack = ImmutableStack<int>.Empty;
        
        // Yeni elemanlar ekleyerek yeni stack'ler oluşturma
        immutableStack = immutableStack.Push(1);
        immutableStack = immutableStack.Push(2);
        immutableStack = immutableStack.Push(3);
        
        Console.WriteLine("Immutable Stack içeriği:");
        foreach (var item in immutableStack)
            Console.Write($"{item} ");
        Console.WriteLine();
        
        // Pop işlemi yeni bir stack döndürür, orijinal stack değişmez
        int value;
        ImmutableStack<int> newStack;
        immutableStack = immutableStack.Pop(out value, out newStack);
        
        Console.WriteLine($"\nPop edilen değer: {value}");
        Console.WriteLine("Pop sonrası immutable stack içeriği:");
        foreach (var item in immutableStack)
            Console.Write($"{item} ");
        Console.WriteLine();
        
        // Immutable Queue oluşturma
        ImmutableQueue<string> immutableQueue = ImmutableQueue<string>.Empty;
        
        // Yeni elemanlar ekleyerek yeni queue'lar oluşturma
        immutableQueue = immutableQueue.Enqueue("Bir");
        immutableQueue = immutableQueue.Enqueue("İki");
        immutableQueue = immutableQueue.Enqueue("Üç");
        
        Console.WriteLine("\nImmutable Queue içeriği:");
        foreach (var item in immutableQueue)
            Console.Write($"{item} ");
        Console.WriteLine();
        
        // Dequeue işlemi yeni bir queue döndürür, orijinal queue değişmez
        string dequeueValue;
        ImmutableQueue<string> newQueue;
        immutableQueue = immutableQueue.Dequeue(out dequeueValue, out newQueue);
        
        Console.WriteLine($"\nDequeue edilen değer: {dequeueValue}");
        Console.WriteLine("Dequeue sonrası immutable queue içeriği:");
        foreach (var item in immutableQueue)
            Console.Write($"{item} ");
        Console.WriteLine();
    }
}
```

### Özelleştirilmiş Sıralama

```csharp
using System;
using System.Collections.Generic;

class PriorityQueue<T> where T : IComparable<T>
{
    private List<T> heap = new List<T>();
    
    public int Count => heap.Count;
    
    public void Enqueue(T item)
    {
        heap.Add(item);
        int i = heap.Count - 1;
        
        while (i > 0)
        {
            int parent = (i - 1) / 2;
            
            if (heap[parent].CompareTo(heap[i]) <= 0)
                break;
                
            T temp = heap[parent];
            heap[parent] = heap[i];
            heap[i] = temp;
            
            i = parent;
        }
    }
    
    public T Dequeue()
    {
        if (heap.Count == 0)
            throw new InvalidOperationException("Priority queue is empty");
            
        T result = heap[0];
        int lastIndex = heap.Count - 1;
        heap[0] = heap[lastIndex];
        heap.RemoveAt(lastIndex);
        
        int i = 0;
        while (true)
        {
            int smallest = i;
            int left = 2 * i + 1;
            int right = 2 * i + 2;
            
            if (left < heap.Count && heap[left].CompareTo(heap[smallest]) < 0)
                smallest = left;
                
            if (right < heap.Count && heap[right].CompareTo(heap[smallest]) < 0)
                smallest = right;
                
            if (smallest == i)
                break;
                
            T temp = heap[i];
            heap[i] = heap[smallest];
            heap[smallest] = temp;
            
            i = smallest;
        }
        
        return result;
    }
    
    public T Peek()
    {
        if (heap.Count == 0)
            throw new InvalidOperationException("Priority queue is empty");
            
        return heap[0];
    }
}

class Task : IComparable<Task>
{
    public string Name { get; set; }
    public int Priority { get; set; }
    
    public Task(string name, int priority)
    {
        Name = name;
        Priority = priority;
    }
    
    public int CompareTo(Task other)
    {
        // Düşük sayısal değer = yüksek öncelik
        return Priority.CompareTo(other.Priority);
    }
    
    public override string ToString()
    {
        return $"{Name} (Öncelik: {Priority})";
    }
}

class PriorityQueueDemo
{
    static void Main()
    {
        PriorityQueue<Task> taskQueue = new PriorityQueue<Task>();
        
        // Görevleri öncelik sırasıyla ekle
        taskQueue.Enqueue(new Task("E-postaları kontrol et", 3));
        taskQueue.Enqueue(new Task("Raporu tamamla", 1));  // Yüksek öncelik
        taskQueue.Enqueue(new Task("Toplantıya hazırlan", 2));
        taskQueue.Enqueue(new Task("Öğle yemeği", 4));
        taskQueue.Enqueue(new Task("Acil müşteri sorunu", 1));  // Yüksek öncelik
        
        Console.WriteLine("Görevler öncelik sırasına göre işleniyor:");
        while (taskQueue.Count > 0)
        {
            Task task = taskQueue.Dequeue();
            Console.WriteLine($"İşleniyor: {task}");
        }
    }
}
```

### Hesap Makinesi Uygulaması

İnfix notasyondaki matematiksel ifadeleri değerlendiren bir hesap makinesi örneği:

```csharp
using System;
using System.Collections.Generic;

class Calculator
{
    // İnfix ifadeyi postfix'e çevirme (Shunting-yard algoritması)
    static Queue<string> InfixToPostfix(string[] tokens)
    {
        Queue<string> output = new Queue<string>();
        Stack<string> operators = new Stack<string>();
        
        foreach (string token in tokens)
        {
            // Sayı ise çıktıya ekle
            if (double.TryParse(token, out _))
            {
                output.Enqueue(token);
            }
            // Operatör ise
            else if (token == "+" || token == "-" || token == "*" || token == "/")
            {
                while (operators.Count > 0 &&
                       operators.Peek() != "(" &&
                       HasHigherPrecedence(operators.Peek(), token))
                {
                    output.Enqueue(operators.Pop());
                }
                operators.Push(token);
            }
            // Sol parantez ise stack'e ekle
            else if (token == "(")
            {
                operators.Push(token);
            }
            // Sağ parantez ise sol parantez bulunana kadar stack'ten çıkar
            else if (token == ")")
            {
                while (operators.Count > 0 && operators.Peek() != "(")
                {
                    output.Enqueue(operators.Pop());
                }
                
                if (operators.Count > 0 && operators.Peek() == "(")
                {
                    operators.Pop(); // Sol parantezi çıkar
                }
            }
        }
        
        // Kalan operatörleri çıktıya ekle
        while (operators.Count > 0)
        {
            if (operators.Peek() == "(")
            {
                throw new ArgumentException("Parantezler eşleşmiyor!");
            }
            output.Enqueue(operators.Pop());
        }
        
        return output;
    }
    
    // Operatör önceliği kontrolü
    static bool HasHigherPrecedence(string op1, string op2)
    {
        if ((op1 == "*" || op1 == "/") && (op2 == "+" || op2 == "-"))
            return true;
        return false;
    }
    
    // Postfix ifadeyi değerlendirme
    static double EvaluatePostfix(Queue<string> postfix)
    {
        Stack<double> values = new Stack<double>();
        
        while (postfix.Count > 0)
        {
            string token = postfix.Dequeue();
            
            // Sayı ise stack'e ekle
            if (double.TryParse(token, out double value))
            {
                values.Push(value);
            }
            // Operatör ise stack'ten iki sayı al, işlemi yap ve sonucu stack'e geri koy
            else
            {
                if (values.Count < 2)
                    throw new InvalidOperationException("Geçersiz ifade!");
                    
                double b = values.Pop();
                double a = values.Pop();
                
                switch (token)
                {
                    case "+": values.Push(a + b); break;
                    case "-": values.Push(a - b); break;
                    case "*": values.Push(a * b); break;
                    case "/": 
                        if (b == 0)
                            throw new DivideByZeroException("Sıfıra bölme hatası!");
                        values.Push(a / b); 
                        break;
                    default:
                        throw new ArgumentException($"Desteklenmeyen operatör: {token}");
                }
            }
        }
        
        if (values.Count != 1)
            throw new InvalidOperationException("Geçersiz ifade!");
            
        return values.Pop();
    }
    
    static void Main()
    {
        while (true)
        {
            Console.Write("\nMatematiksel ifade girin (çıkış için 'q'): ");
            string input = Console.ReadLine();
            
            if (input.ToLower() == "q")
                break;
                
            try
            {
                // Boşluklara göre böl, ama parantezleri ayrı tokenlar olarak ele al
                string processed = input.Replace("(", " ( ").Replace(")", " ) ");
                string[] tokens = processed.Split(new[] { ' ' }, StringSplitOptions.RemoveEmptyEntries);
                
                Queue<string> postfix = InfixToPostfix(tokens);
                double result = EvaluatePostfix(postfix);
                
                Console.WriteLine($"Sonuç: {result}");
            }
            catch (Exception ex)
            {
                Console.WriteLine($"Hata: {ex.Message}");
            }
        }
    }
}
```

## Sonuç

Stack ve Queue, modern programlamada temel veri yapılarıdır. Her biri benzersiz özelliklere ve kullanım alanlarına sahiptir. Bu rehberde, C# dilinde Stack ve Queue veri yapılarının işlevlerini, özelliklerini ve uygulamalarını kapsamlı bir şekilde inceledik.

Bir yazılım geliştirici olarak, bu veri yapılarının doğru kullanımı algoritmaların okunabilirliğini ve verimliliğini büyük ölçüde artırabilir. Verilerin nasıl işleneceğine karar verirken, aşağıdaki soruları kendinize sorarak hangi veri yapısının daha uygun olduğuna karar verebilirsiniz:

1. Verilerinize hangi sırayla erişmek istiyorsunuz?
2. Eleman ekleme ve çıkarma işlemleri ne sıklıkta gerçekleşecek?
3. Arabellek, geçmiş takibi veya ters çevirme gerekiyor mu?
4. Çok iş parçacıklı bir ortamda mı çalışıyorsunuz?
5. Veri değişmezliği (immutability) önemli mi?

Bu sorulara verdiğiniz cevaplar, Stack, Queue veya diğer veri yapılarından hangisini seçeceğiniz konusunda size rehberlik edecektir.

Hem Stack hem de Queue, pek çok problem için verimli çözümler sunarlar ve modern yazılım geliştirmenin vazgeçilmez parçalarıdır. Bu rehber sayesinde, bu temel veri yapılarını C# projelerinizde güvenle kullanabilir, performans ve kod kalitesini artırabilirsiniz.
