# WellDev Interview Prep - OOP (Object-Oriented Programming) - উত্তরসহ

> Source: CPS Academy - "WellDev Interview Prep - Bangla" course, Module 14 (OOP ও Design), Lessons 61-64.
> **Note:** কোড উদাহরণগুলো Java-তে দেওয়া হয়েছে।

## সূচি
- OOP-এর চার স্তম্ভ - Encapsulation, Abstraction, Inheritance, Polymorphism
- OOP Base Concepts - Abstract Class, Access Modifier, Constructor
- SOLID Principles
- Design Patterns - Singleton, Dependency Injection

---
 
## Lesson 61 - OOP-এর চার স্তম্ভ (Encapsulation, Abstraction, Inheritance, Polymorphism)

**OOP কী:** Data এবং সেই Data-র উপর কাজ করা Function-গুলোকে একসাথে বাস্তব জগতের "Object" হিসেবে চিন্তা করা। এর মূল একক হলো **Class** (নকশা বা টেমপ্লেট) এবং **Object** (নকশা থেকে তৈরি আসল জিনিস)।

### ১. Encapsulation (আবদ্ধকরণ)
**সংজ্ঞা:** Data এবং Data-র উপর কাজ করা Method-গুলোকে একসাথে একটি Class-এর ভেতরে বেঁধে রাখা। ভিতরের Data যেন বাইরে থেকে সরাসরি পরিবর্তন করা না যায় (অর্থাৎ `private` রাখা), তার ব্যবস্থা করা। বাইরের কেউ শুধু নির্দিষ্ট `public` মেথড দিয়েই Data অ্যাক্সেস করতে পারে।
**উদাহরণ:** ব্যাংক অ্যাকাউন্ট - ব্যালেন্স সরাসরি বদলানো যায় না, শুধু `deposit()` বা `withdraw()` মেথড দিয়ে বদলানো যায় (যেখানে নির্দিষ্ট নিয়ম চেক করা হয়)।

```java
public class BankAccount {
    private double balance; // Data লুকানো আছে (private)

    public BankAccount(double initialBalance) {
        this.balance = initialBalance;
    }

    public void deposit(double amount) {
        if (amount > 0) {
            balance += amount;
        }
    }

    public boolean withdraw(double amount) {
        if (amount > 0 && amount <= balance) {
            balance -= amount;
            return true;
        }
        return false;
    }

    public double getBalance() {
        return balance;
    }
}
```
**Interview-উত্তর:** "Encapsulation হলো Data এবং Method-কে একসাথে বেঁধে ফেলা এবং ভেতরের স্টেটকে Private করে বাইরে থেকে রক্ষা করা। এর প্রধান সুবিধা হলো Data-র সুরক্ষা ও নিয়ন্ত্রণ।"

### ২. Abstraction (বিমূর্তকরণ)
**সংজ্ঞা:** ভেতরের জটিলতা লুকিয়ে শুধু প্রয়োজনীয় অংশ (Interface) বাইরের ব্যবহারকারীর কাছে তুলে ধরা। অর্থাৎ "কী করে" সেটা দেখাও, কিন্তু "কীভাবে করে" সেটা লুকাও।
**Encapsulation-এর সাথে পার্থক্য:** 
- Encapsulation = "কীভাবে কাজ করে" তা লুকানো (Data/State প্রাইভেট রাখা)। 
- Abstraction = "কী কাজ করে" তা দেখানো (ডিজাইন-স্তর)। Encapsulation দিয়েই Abstraction অর্জন করা হয়।

**উদাহরণ (গাড়ি):** আমরা শুধু স্টিয়ারিং, এক্সেলেরেটর বা ব্রেক দেখি (Abstraction); ভেতরের ইঞ্জিনের কাজ আমাদের কাছে লুকানো থাকে (Encapsulation)।

```java
public class Car {
    private void injectFuel() {} 
    private void ignite() {}
    private void runPistons() {}

    // শুধু এই মেথডটি বাইরের মানুষ ব্যবহার করবে
    public void start() {
        injectFuel();
        ignite();
        runPistons();
    }
}
```

### ৩. Inheritance (উত্তরাধিকার)
**সংজ্ঞা:** একটি Child Class যখন তার Parent Class-এর বৈশিষ্ট্য এবং আচরণ লাভ করে। এটি "is-a" সম্পর্ক তৈরি করে (যেমন: Dog is-a Animal)। এর প্রধান সুবিধা হলো **কোডের পুনঃব্যবহার (Code Reusability)**।

```java
public class Animal {
    protected String name;

    public Animal(String name) {
        this.name = name;
    }

    public void eat() {
        System.out.println(name + " খাচ্ছে");
    }
}

public class Dog extends Animal {
    public Dog(String name) {
        super(name); // Parent-এর কনস্ট্রাক্টর কল করা
    }

    public void fetch() {
        System.out.println(name + " বল আনছে");
    }
}
```
**সতর্কতা:** শুধু কোড কমানোর জন্য Inheritance ব্যবহার করা ভুল, যদি তাদের মধ্যে সত্যি সত্যি "is-a" সম্পর্ক না থাকে। এর বিকল্প হলো **Composition** ("has-a" সম্পর্ক)। যেমন: গাড়ি ইঞ্জিনের উত্তরাধিকারী নয়, বরং গাড়ির ভেতরে ইঞ্জিন থাকে (Car has-a Engine)।

### ৪. Polymorphism (বহুরূপতা)
**সংজ্ঞা:** একই Interface, কিন্তু ভিন্ন ভিন্ন রূপ। এর দুটি ধরন আছে:
- **Compile-time (Overloading):** একই নামের মেথড, কিন্তু প্যারামিটার ভিন্ন। কোড কম্পাইল করার সময়ই ঠিক হয়ে যায় কোনটি কল হবে।
- **Runtime (Overriding):** Parent-এর মেথডকে Child Class নিজের মতো করে পুনরায় লেখে। প্রোগ্রাম চলার সময় (Runtime) অবজেক্টের আসল টাইপ দেখে মেথড কল হয়।

```java
class Calculator {
    // Overloading (Compile-time)
    public int add(int a, int b) { return a + b; }
    public double add(double a, double b) { return a + b; } 
}

abstract class Shape {
    public abstract double area();
}

class Circle extends Shape {
    private double r;
    public Circle(double r) { this.r = r; }

    @Override
    public double area() { return 3.14159 * r * r; } // Overriding
}

class Square extends Shape {
    private double s;
    public Square(double s) { this.s = s; }

    @Override
    public double area() { return s * s; } // Overriding
}

// Runtime polymorphism উদাহরণ:
// Shape[] shapes = {new Circle(5), new Square(4)};
// for (Shape s : shapes) System.out.println(s.area());
```

**এক নজরে (Interview Cheat-table):**
| স্তম্ভ | এক লাইনে | কী দিয়ে অর্জিত হয় |
|---|---|---|
| Encapsulation | Data লুকানো, Method দিয়ে অ্যাক্সেস | `private` / `public` |
| Abstraction | জটিলতা লুকিয়ে দরকারি অংশ দেখানো | `abstract class` / `interface` |
| Inheritance | Parent থেকে বৈশিষ্ট্য পাওয়া (is-a) | `extends` / `implements` |
| Polymorphism | একই নামের মেথডের ভিন্ন রূপ | `override` / `overload` |

---

## Lesson 62 - OOP Base Concepts: Abstract Class, Access Modifier, Constructor

### প্রশ্ন ১ - Abstract Class কী?
এমন Class যার সরাসরি অবজেক্ট বানানো যায় না, একে শুধু Inherit (উত্তরাধিকার সূত্রে পাওয়া) করা যায়। এতে সাধারণত এমন Abstract মেথড থাকে যার কোনো বডি থাকে না-Child Class-কে অবশ্যই সেই মেথড লিখতে বাধ্য করে। 

```java
abstract class Shape {
    public abstract double area();      // Abstract মেথড (বডি নেই)
    
    public void describe() {
        System.out.println("Area: " + area()); // সাধারণ মেথড
    }
}

class Circle extends Shape {
    private double r;
    public Circle(double r) { this.r = r; }
    
    @Override
    public double area() { return 3.14159 * r * r; } 
}
// Shape s = new Shape(); // ভুল! Abstract class-এর অবজেক্ট তৈরি করা যায় না।
```

### প্রশ্ন ২ - Abstract Class বনাম Interface
| বৈশিষ্ট্য | Abstract Class | Interface |
|---|---|---|
| ইমপ্লিমেন্টেশন | সাধারণ মেথড থাকতে পারে | Java 8+ থেকে `default` মেথড থাকতে পারে, তবে সাধারণত শুধু `abstract` মেথড থাকে |
| স্টেট/ভ্যারিয়েবল | `instance variable` থাকতে পারে | শুধু `static final` (কনস্ট্যান্ট) থাকতে পারে |
| কয়টা Inherit/Implement | মাত্র একটি `extends` করা যায় | একাধিক `implements` করা যায় |
| মূল উদ্দেশ্য | কোড শেয়ার করা ও বাধ্য করা | ক্লাসের মাঝে শুধু চুক্তি (Contract) তৈরি করা |

### প্রশ্ন ৩ - Access Modifiers (public, protected, private)
মেম্বার ভ্যারিয়েবল বা মেথড কোথা থেকে অ্যাক্সেস করা যাবে, তা নির্ধারণ করে:
| Modifier | একই Class-এর ভেতরে | Child Class (Subclass) | বাইরের জগৎ |
|---|---|---|---|
| `public` | ✓ | ✓ | ✓ |
| `protected` | ✓ | ✓ | ✗ (একই প্যাকেজের বাইরে) |
| `private` | ✓ | ✗ | ✗ |

**টিপস:** Encapsulation-এর জন্য Data সবসময় `private` (বা `protected`) রাখা হয়, এবং মেথড `public` রাখা হয়।

### প্রশ্ন ৪ - Constructor ও Garbage Collection (Destructor-এর বিকল্প)
- **Constructor:** অবজেক্ট তৈরির সময় স্বয়ংক্রিয়ভাবে কল হয় এবং ডেটা ইনিশিয়ালাইজ করে। এর নাম Class-এর নামের সমান হয়, কোনো রিটার্ন টাইপ থাকে না এবং এটি Overload করা যায়।
- **Destructor (Java-তে):** C++ এর মতো Java-তে Destructor নেই। এর বদলে Java-র **Garbage Collector (GC)** অব্যবহৃত অবজেক্টগুলোকে মেমরি থেকে স্বয়ংক্রিয়ভাবে মুছে ফেলে। তবে ফাইল বা ডাটাবেস কানেকশনের মতো Resource রিলিজ করার জন্য Java-তে `AutoCloseable` ইন্টারফেস এবং `try-with-resources` ব্যবহার করা হয়।

```java
class ResourceHandler implements AutoCloseable {
    public ResourceHandler() {
        System.out.println("Resource নিলাম"); // Constructor
    }
    
    @Override
    public void close() {
        System.out.println("Resource রিলিজ করলাম"); // C++ এর Destructor-এর বিকল্প
    }
}
// ব্যবহার:
// try (ResourceHandler res = new ResourceHandler()) { ... } 
// ব্লক শেষ হলে নিজে নিজেই close() মেথড কল হবে।
```

---

## Lesson 63 - SOLID Principles (ভালো Design-এর পাঁচ নিয়ম)

**S - Single Responsibility Principle (SRP)**: একটি Class-এর একটাই দায়িত্ব থাকা উচিত এবং তার পরিবর্তন হওয়ার কারণও যেন একটাই থাকে।
*উদাহরণ:* `Report` ক্লাস যদি নিজে রিপোর্ট তৈরি করে, সেভ করে এবং ইমেইল করে-তবে তা ভুল। এর বদলে আলাদা তিনটি ক্লাস হওয়া উচিত: `Report`, `ReportSaver`, `ReportEmailer`।

**O - Open/Closed Principle (OCP)**: ক্লাস Extension (নতুন ফিচার যোগ করার জন্য) খোলা থাকবে, কিন্তু Modification (পুরনো কোড পরিবর্তনের জন্য) বন্ধ থাকবে।
*উদাহরণ:* If-else দিয়ে Shape চেক করে এরিয়া বের না করে, Polymorphism ব্যবহার করা। নতুন শেপ এলে শুধু নতুন ক্লাস বানাতে হবে, পুরনো `totalArea` ফাংশন বদলাতে হবে না।
```java
abstract class Shape { public abstract double area(); }

class AreaCalculator {
    public double totalArea(Shape[] shapes) {
        double sum = 0;
        for (Shape s : shapes) {
            sum += s.area(); // নতুন শেপ যোগ হলেও এই কোড কখনো বদলাবে না
        }
        return sum; 
    }
}
```

**L - Liskov Substitution Principle (LSP)**: Parent ক্লাসের জায়গায় Child ক্লাসকে বসালে প্রোগ্রাম যেন না ভাঙে। Child-কে অবশ্যই Parent-এর চুক্তি মানতে হবে।
*উদাহরণ:* সব পাখি ওড়ে না (যেমন পেঙ্গুইন)। তাই `Penguin`-কে `FlyingBird`-এর Child বানালে সমস্যা হবে। এর বদলে তাকে শুধু `Bird`-এর Child বানাতে হবে।

**I - Interface Segregation Principle (ISP)**: কোনো ক্লাসকে এমন কোনো Interface ইমপ্লিমেন্ট করতে বাধ্য করা উচিত নয়, যার মেথড তার দরকার নেই। বড় একটি Interface-এর বদলে ছোট ছোট নির্দিষ্ট Interface বানানো ভালো।
*উদাহরণ:* `Robot`-কে যেন `eat()` মেথড ইমপ্লিমেন্ট করতে না হয়, তাই `Workable` এবং `Eatable` নামে আলাদা ইন্টারফেস বানানো উচিত।

**D - Dependency Inversion Principle (DIP)**: উচ্চ-স্তরের মডিউল কখনো নিচু-স্তরের (Concrete) ক্লাসের ওপর নির্ভর করবে না-উভয়েই Abstraction (Interface/Abstract Class)-এর ওপর নির্ভর করবে।

```java
interface MessageSender { void send(String msg); }

class NotificationService {
    private MessageSender sender;

    public NotificationService(MessageSender s) {
        this.sender = s; // Concrete ক্লাসের বদলে Interface-এর ওপর নির্ভরতা
    }

    public void notifyUser(String msg) {
        sender.send(msg);
    }
}
```

---

## Lesson 64 - Design Patterns: Singleton ও Dependency Injection

### প্রশ্ন ১ - Singleton Pattern কী?
পুরো প্রোগ্রামে একটি Class-এর মাত্র **একটাই অবজেক্ট (Instance)** তৈরি হবে এবং সবাই সেটিই অ্যাক্সেস করবে-এটি নিশ্চিত করাই Singleton-এর কাজ।
**কেন দরকার:** ডাটাবেস কানেকশন পুল, কনফিগারেশন সেটিংস বা লগার ইত্যাদিতে, যেগুলোর একটাই অবজেক্ট থাকা উচিত।
**কীভাবে:** Constructor `private` করতে হবে (যাতে বাইরে থেকে `new` করা না যায়) + একটি `static getInstance()` মেথড বানাতে হবে।

```java
public class Database {
    private static Database instance;
    private int connectionCount;

    private Database() { // Private constructor
        this.connectionCount = 0;
    }

    public static Database getInstance() {
        if (instance == null) {
            instance = new Database(); // প্রথমবার অবজেক্ট তৈরি
        }
        return instance; // পরের বার আগের অবজেক্টটিই ফেরত দেবে
    }

    public void query(String q) {
        connectionCount++;
    }
}
```

### প্রশ্ন ২ - Dependency Injection (DI) কী?
একটি অবজেক্ট যখন নিজের ডিপেনডেন্সি (প্রয়োজনীয় অন্য অবজেক্ট) নিজে ভেতরে না বানিয়ে বাইরে থেকে গ্রহণ করে, তাকে Dependency Injection বলে। সাধারণত এটি Constructor দিয়ে করা হয় ("Constructor Injection")।
**কেন দরকার:** ভেতরে সরাসরি `new EmailSender()` বানালে তা শক্ত কাপলিং (Tight Coupling) তৈরি করে। পরে SMS পাঠাতে চাইলে ক্লাসের কোড বদলাতে হয়। DI ব্যবহার করলে কোড বদলাতে হয় না এবং টেস্টিং (Mocking) অনেক সহজ হয়।

```java
interface MessageSender { void send(String msg); }

class EmailSender implements MessageSender {
    @Override public void send(String m) { /* ইমেইল পাঠানোর কোড */ }
}

class SMSSender implements MessageSender {
    @Override public void send(String m) { /* SMS পাঠানোর কোড */ }
}

class OrderService {
    private MessageSender sender; // Abstraction

    // Constructor Injection
    public OrderService(MessageSender s) {
        this.sender = s; 
    }

    public void confirm(String item) {
        sender.send("নিশ্চিত: " + item);
    }
}

// ব্যবহার:
// MessageSender email = new EmailSender();
// OrderService service = new OrderService(email); // বাইরে থেকে Inject করা হলো
// service-এর কোড না বদলেই ভবিষ্যতে খুব সহজে SMSSender Inject করা যাবে।
```

**Singleton vs DI:** Singleton লুকিয়ে গ্লোবাল ডিপেনডেন্সি তৈরি করে; অন্যদিকে DI ডিপেনডেন্সিকে স্পষ্টভাবে বাইরে থেকে নিয়ে আসে। তাই টেস্টিং সহজ করার জন্য প্রায়ই Singleton-এর বদলে DI-কে বেশি প্রাধান্য দেওয়া হয়।

### প্রশ্ন ৩ - Design Pattern-এর ধরন (Gang of Four)
- **Creational** (অবজেক্ট কীভাবে তৈরি হয়): Singleton, Factory, Builder
- **Structural** (ক্লাস/অবজেক্ট কীভাবে জোড়া লাগে): Adapter, Decorator, Facade
- **Behavioral** (অবজেক্ট কীভাবে যোগাযোগ করে): Observer, Strategy, Iterator
