# Java Generics - Complete Study Notes (Hinglish Style) 📚

## Generics Kya Hai Bhai? 🤔

Generics ko samjho jaise ek "template" ya "blueprint" jo different data types ke saath kaam kar sakta hai! Instead of har data type ke liye alag code likhne ke (jaise ek Integer ke liye, ek String ke liye), Generics tumhe ek baar code likhne deta hai aur phir kisi bhi type ke saath use kar sakte ho.

**Simple Definition**: Generics parameterized types hain jo classes, methods, aur interfaces ko different data types ke saath kaam karne dete hain while type safety maintain karte hain.

## Generics Ki Zarurat Kyu Hai Yaar? 🎯

### 1. **Type Safety** (Errors Ko Jaldi Pakdo!)
Generics se pehle, tum galti se different types ko collections mein mix kar dete the, jo runtime errors deta tha. Generics ke saath, compiler turant mistake catch kar leta hai!

**Before Generics (Galat Tarika!):**
```java
ArrayList list = new ArrayList();
list.add(123);        // Integer
list.add("Hello");    // String - Ye baad mein runtime error dega!
```

**With Generics (Sahi Tarika!):**
```java
ArrayList<Integer> list = new ArrayList<Integer>();
list.add(123);        // ✅ Bilkul sahi
list.add("Hello");    // ❌ Compiler error - turant mistake pakad lega!
```

### 2. **Type Casting Ki Tension Nahi** (Kam Pareshani!)
Generics ke bina, tumhe collections se objects nikaalte time cast karna padta tha. Generics ke saath, compiler automatically type janta hai!

**Pehle:**
```java
String item = (String) list.get(0);  // Manual casting zaroori thi
```

**Generics Ke Saath:**
```java
String item = list.get(0);  // Koi casting nahi chahiye! 🎉
```

### 3. **Code Reusability** (Ek Baar Likho, Har Jagah Use Karo!)
Ek generic class/method multiple data types ke saath kaam kar sakta hai:

```java
ArrayList<String> stringList = new ArrayList<String>();
ArrayList<Integer> intList = new ArrayList<Integer>();
ArrayList<Double> doubleList = new ArrayList<Double>();
// Same ArrayList class, bas different types!
```

## Generics Ke Types 📚

### 1. **Generic Classes**
Ek class jo kisi bhi data type ke saath kaam kar sakti hai:

```java
class Box<T> {  // T ek type parameter hai
    private T content;
    
    public Box(T content) {
        this.content = content;
    }
    
    public T getContent() {
        return content;
    }
}

// Kaise Use Kare:
Box<String> stringBox = new Box<String>("Hello World!");
Box<Integer> intBox = new Box<Integer>(42);
Box<Double> doubleBox = new Box<Double>(3.14);
```

### 2. **Generic Methods**
Methods jo different data types ko parameters ke roop mein accept kar sakte hain:

```java
public class Utility {
    // Generic method
    public static <T> void printArray(T[] array) {
        for (T element : array) {
            System.out.println(element);
        }
    }
}

// Kaise Use Kare:
String[] names = {"Alice", "Bob", "Charlie"};
Integer[] numbers = {1, 2, 3, 4, 5};

Utility.printArray(names);    // String array ke saath kaam karta hai
Utility.printArray(numbers);  // Integer array ke saath kaam karta hai
```

## Common Type Parameters 🔤

Ye "standard names" hain jo generic programming mein use karte hain:

- **T** - Type (sabse common, kisi bhi type ke liye)
- **E** - Element (collections mein use hota hai)
- **K** - Key (maps mein use hota hai)
- **V** - Value (maps mein use hota hai)
- **N** - Number (numeric types ke liye)

**HashMap Ka Example:**
```java
HashMap<Integer, String> studentMap = new HashMap<>();
//        ^K      ^V
//       Key     Value
studentMap.put(101, "John");
studentMap.put(102, "Alice");
```

## Wildcards - Flexible Dost! 🃏

Kabhi kabhi tum exact type ke baare mein flexible rehna chahte ho. Isliye wildcards ka use karte hain!

### 1. **Upper Bound Wildcards** `<? extends ClassName>`
**Matlab**: "Koi bhi type jo ClassName ka child hai (extends karta hai)"

```java
// List of Number ya Number ke kisi bhi subclass ko accept kar sakta hai
public static double sum(List<? extends Number> numbers) {
    double total = 0;
    for (Number num : numbers) {
        total += num.doubleValue();
    }
    return total;
}

// Ye sab ke saath kaam karta hai:
List<Integer> intList = Arrays.asList(1, 2, 3);
List<Double> doubleList = Arrays.asList(1.1, 2.2, 3.3);
sum(intList);    // ✅ Integer, Number ka child hai
sum(doubleList); // ✅ Double, Number ka child hai
```

### 2. **Lower Bound Wildcards** `<? super ClassName>`
**Matlab**: "Koi bhi type jo ClassName ka parent hai"

```java
public static void addNumbers(List<? super Integer> list) {
    list.add(10);  // Safely Integer add kar sakte hain
    list.add(20);
}

// Ye sab ke saath kaam karta hai:
List<Integer> intList = new ArrayList<>();
List<Number> numList = new ArrayList<>();
List<Object> objList = new ArrayList<>();

addNumbers(intList);  // ✅ Integer khud Integer hai
addNumbers(numList);  // ✅ Number, Integer ka parent hai
addNumbers(objList);  // ✅ Object, Integer ka parent hai
```

### 3. **Unbounded Wildcards** `<?>`
**Matlab**: "Koi bhi type chalega"

```java
public static void printList(List<?> list) {
    for (Object item : list) {  // Sab kuch Object hai!
        System.out.println(item);
    }
}

// KISI BHI list ke saath kaam karta hai:
printList(Arrays.asList("Hello", "World"));        // String list
printList(Arrays.asList(1, 2, 3));                 // Integer list
printList(Arrays.asList(true, false));             // Boolean list
```

## Yaad Rakhne Ke Tips! 💡

1. **PECS Rule**: "Producer Extends, Consumer Super"
   - Agar tum collection se **read kar rahe ho** → `<? extends>` use karo
   - Agar tum collection mein **write kar rahe ho** → `<? super>` use karo

2. **Type Erasure**: Runtime pe generic type information "erase" ho jaati hai - so `List<String>` sirf `List` ban jaata hai. Isliye generic code mein `new T()` nahi kar sakte.

3. **Primitives Use Nahi Kar Sakte**: Wrapper classes use karo (`Integer`, `int` nahi)

## Common Galtiyan - Inse Bacho! ⚠️

1. **Raw types aur generics ko mix mat karo:**
```java
// Galat:
List<String> list = new ArrayList();  // Right side pe generic missing hai

// Sahi:
List<String> list = new ArrayList<String>();
// Ya Java 7+ mein:
List<String> list = new ArrayList<>();  // Diamond operator
```

2. **Wildcards mein type bounds yaad rakho:**
```java
// Ye compile nahi hoga:
List<? extends Number> list = new ArrayList<>();
list.add(5);  // ❌ ? extends mein add nahi kar sakte

// Lekin ye kaam karega:
List<? super Integer> list = new ArrayList<>();
list.add(5);  // ✅ Integer ko ? super Integer mein add kar sakte hain
```

## Real-World Example 🌟

Dekho practical scenario mein generics kaise use karte hain:

```java
public class DataProcessor<T> {
    private List<T> data;
    
    public DataProcessor() {
        this.data = new ArrayList<>();
    }
    
    public void addItem(T item) {
        data.add(item);
    }
    
    public List<T> getAllItems() {
        return new ArrayList<>(data);  // Safety ke liye copy return karo
    }
    
    public T getLastItem() {
        if (data.isEmpty()) return null;
        return data.get(data.size() - 1);
    }
}

// Kaise Use Kare:
DataProcessor<String> textProcessor = new DataProcessor<>();
textProcessor.addItem("Hello");
textProcessor.addItem("World");

DataProcessor<Integer> numberProcessor = new DataProcessor<>();
numberProcessor.addItem(42);
numberProcessor.addItem(99);
```

## Summary 📝

Generics ek universal adapter ki tarah hain jo kisi bhi type ke saath plug-in ho jaata hai! Ye tumhare code ko banata hai:
- **Safer** (compile time pe errors pakadta hai)
- **Cleaner** (casting ki zarurat nahi)
- **Reusable** (ek class/method multiple types ke liye)
- **More expressive** (code clearly dikhata hai ki kya types ke saath kaam karta hai)

**Yaad Rakho**: Generics tumhara dost hai robust, type-safe Java code likhne ke liye jo flexible aur reliable dono hai! 🚀

**Bonus Tip**: Practice karte raho different examples ke saath, tabhi properly samajh aayega. All the best for your preparation! 💪
