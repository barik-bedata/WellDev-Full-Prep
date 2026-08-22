# WellDev Interview Prep — OOP (Object-Oriented Programming) — উত্তরসহ

> Source: CPS Academy — "WellDev Interview Prep — Bangla" course, Module 14 (OOP ও Design), Lessons 61–64.

## সূচি
- OOP-এর চার স্তম্ভ — Encapsulation, Abstraction, Inheritance, Polymorphism
- OOP Base Concepts — Abstract Class, Access Modifier, Constructor
- SOLID Principles
- Design Patterns — Singleton, Dependency Injection

---

## Lesson 61 — OOP-এর চার স্তম্ভ (Encapsulation, Abstraction, Inheritance, Polymorphism)

OOP কী: data ও সেই data-র উপর কাজ (function) একসাথে বাঁধা, বাস্তব জগতের "object" হিসেবে। মূল একক — class (নকশা) ও object (নকশা থেকে বানানো জিনিস)।

**১. Encapsulation (আবদ্ধকরণ)**
সংজ্ঞা: data ও data-র উপর কাজ করা method একসাথে class-এ বেঁধে রাখা, ভিতরের data বাইরে থেকে সরাসরি অগম্য (private) রাখা। বাইরের কেউ শুধু নির্দিষ্ট public method দিয়ে data স্পর্শ করতে পারে।
উদাহরণ: ব্যাংক অ্যাকাউন্ট — balance সরাসরি বদলানো যায় না, শুধু deposit()/withdraw() দিয়ে (যেখানে নিয়ম চেক হয়)।
```cpp
class BankAccount {
private:
    double balance;
public:
    BankAccount(double initial) : balance(initial) {}
    void deposit(double amount) { if (amount > 0) balance += amount; }
    bool withdraw(double amount) {
        if (amount > 0 && amount <= balance) { balance -= amount; return true; }
        return false;
    }
    double getBalance() { return balance; }
};
```
Interview-উত্তর: "Encapsulation হলো data ও method একসাথে বেঁধে, ভিতরের state private করে বাইরে থেকে রক্ষা করা। লাভ — data protection ও নিয়ন্ত্রণ।"

**২. Abstraction (বিমূর্তকরণ)**
সংজ্ঞা: জটিলতা লুকিয়ে শুধু দরকারি অংশ (interface) দেখানো — কী করে দেখাও, কীভাবে করে লুকাও।
Encapsulation থেকে পার্থক্য: Encapsulation = "কীভাবে" লুকানো (data/state private, implementation-স্তর); Abstraction = "কী" দেখানো (জটিলতা লুকানো, design-স্তর)। Encapsulation দিয়েই abstraction অর্জন হয়।
উদাহরণ (গাড়ি): steering/accelerator/brake দেখো (abstraction); engine casing-এ আবদ্ধ (encapsulation)।
```cpp
class Car {
private:
    void injectFuel() {} 
    void ignite() {}
    void runPistons() {}
public:
    void start() { injectFuel(); ignite(); runPistons(); }
};
```

**৩. Inheritance (উত্তরাধিকার)**
সংজ্ঞা: child class parent class-এর বৈশিষ্ট্য/আচরণ পায় — "is-a" সম্পর্ক (Dog is-a Animal), কোড পুনঃব্যবহার।
```cpp
class Animal {
protected:
    string name;
public:
    Animal(string n) : name(n) {}
    void eat() { cout << name << " খাচ্ছে\n"; }
};
class Dog : public Animal {
public:
    Dog(string n) : Animal(n) {}
    void fetch() { cout << name << " বল আনছে\n"; }
};
```
সমস্যা: fragile base class (parent বদলালে child ভাঙে), এবং শুধু code-reuse-এর জন্য inheritance ব্যবহার করলে ভুল যদি "is-a" সম্পর্ক না থাকে। সমাধান নীতি: **"composition over inheritance"** — is-a না হলে has-a (Car has-a Engine, is-a Engine না)।

**৪. Polymorphism (বহুরূপতা)**
সংজ্ঞা: এক interface, বহু রূপ। দুই ধরন:
- **Compile-time (Overloading):** একই নামের method, ভিন্ন parameter — compile-এ ঠিক হয়।
- **Runtime (Overriding):** child parent-এর method নিজের মতো লেখে (virtual দিয়ে) — runtime-এ আসল object-type দেখে ঠিক হয়।
```cpp
class Calculator {
public:
    int add(int a, int b) { return a+b; }
    double add(double a, double b) { return a+b; } // overloading
};
class Shape { public: virtual double area(){return 0;} virtual ~Shape(){} };
class Circle : public Shape { double r; public: Circle(double r):r(r){} double area() override {return 3.14159*r*r;} };
class Square : public Shape { double s; public: Square(double s):s(s){} double area() override {return s*s;} };
// Shape* shapes[] = {new Circle(5), new Square(4)}; for(auto s: shapes) s->area(); // runtime polymorphism
```

**এক নজরে (interview cheat-table):**
| স্তম্ভ | এক লাইনে | কী দিয়ে |
|---|---|---|
| Encapsulation | data লুকানো, method দিয়ে access | private/public |
| Abstraction | জটিলতা লুকিয়ে দরকারি দেখানো | abstract class/interface |
| Inheritance | parent থেকে বৈশিষ্ট্য (is-a) | `: public Parent` |
| Polymorphism | এক interface, বহু রূপ | virtual, overload |

মুখস্থ রাখার দুইটা পার্থক্য: Encapsulation vs Abstraction (data hiding vs complexity hiding); Overloading vs Overriding (compile-time/parameter vs runtime/virtual)।

Practice (design exercises, LeetCode-এ সরাসরি নাই): Shape hierarchy design (inheritance+abstraction+polymorphism), 146. LRU Cache (class design/encapsulation), 155. Min Stack (encapsulation), 706. Design HashMap।

---

## Lesson 62 — OOP Base Concepts: Abstract Class, Access Modifier, Constructor

**প্রশ্ন ১ — Abstract class কী?**
এমন class যার object সরাসরি বানানো যায় না, শুধু inherit করা যায়। অন্তত একটা abstract (pure virtual, `=0`) method থাকে যার body নাই — child-কে লিখতে বাধ্য করে। সাথে সাধারণ method/data-ও থাকতে পারে।
```cpp
class Shape {
public:
    virtual double area() = 0;      // pure virtual
    void describe(){ cout<<area(); }// shared method
    virtual ~Shape(){}
};
class Circle: public Shape { double r; public: Circle(double r):r(r){} double area() override{return 3.14159*r*r;} };
// Shape s; // ভুল! abstract class-এর object বানানো যায় না
```

**প্রশ্ন ২ — Abstract class vs Interface**
| | Abstract Class | Interface |
|---|---|---|
| Implementation | কিছু থাকতে পারে | নাই (pure) |
| Data | থাকতে পারে | নাই |
| কয়টা inherit/implement | এক | অনেক |
| ব্যবহার | code share + বাধ্য | শুধু চুক্তি |
C++-এ আলাদা `interface` keyword নাই — সব method pure virtual হলেই সেটা interface-এর কাজ করে। একটা class এক abstract class inherit করতে পারে কিন্তু একাধিক interface implement করতে পারে (multiple inheritance দিয়ে)।

**প্রশ্ন ৩ — Polymorphism vs Constructor**
আলাদা জিনিস: Constructor object তৈরির সময় স্বয়ংক্রিয় কল হওয়া special method (data initialize করে, class-এর নামে, return type নাই)। Polymorphism = এক interface বহু রূপ। **যোগসূত্র:** constructor overload করা যায় (ভিন্ন parameter-এর একাধিক constructor) — এটা compile-time polymorphism। কিন্তু **constructor override হয় না** — virtual constructor বলে কিছু নাই, তাই runtime polymorphism-এ অংশ নেয় না।

**প্রশ্ন ৪ — protected vs public (vs private)**
Access modifier — member কোথা থেকে ছোঁয়া যাবে ঠিক করে:
| Modifier | Class ভিতরে | Child | বাইরে |
|---|---|---|---|
| public | ✓ | ✓ | ✓ |
| protected | ✓ | ✓ | ✗ |
| private | ✓ | ✗ | ✗ |
মূল পার্থক্য: public সবার জন্য খোলা; protected শুধু class নিজে ও তার child-দের জন্য, বাইরের কারো জন্য না। Encapsulation-এর জন্য data সাধারণত private/protected, method public রাখা হয়।

**প্রশ্ন ৫ — Constructor ও Destructor**
Constructor: object তৈরির সময় স্বয়ংক্রিয় কল, data initialize করে; class-এর নামে, return type নাই, overload করা যায়।
Destructor (`~ClassName`): object ধ্বংসের সময় স্বয়ংক্রিয় কল, resource (memory/file) মুক্ত করে; একটাই হয়, parameter নাই।
এটাই **RAII** (Resource Acquisition Is Initialization) — constructor-এ resource নেওয়া, destructor-এ ছাড়া — C++-এর মূল resource-management নীতি।
```cpp
class Resource {
    int* data;
public:
    Resource(int size){ data=new int[size]; }  // constructor — resource নিলাম
    ~Resource(){ delete[] data; }               // destructor — resource ছাড়লাম
};
```

Practice (design exercises): Abstract Shape→Circle/Square/Triangle, Employee hierarchy (protected data), RAII FileHandler class, 155. Min Stack।

---

## Lesson 63 — SOLID Principles (ভালো Design-এর পাঁচ নিয়ম)

**S — Single Responsibility Principle**: এক class-এর একটাই দায়িত্ব থাকা উচিত, একটাই কারণ যেন বদলাতে হয়। উদাহরণ: Report class যদি generate + save + email তিনটাই করে — খারাপ। আলাদা করো: Report, ReportSaver, ReportEmailer।

**O — Open/Closed Principle**: class extension-এ খোলা, modification-এ বন্ধ থাকা উচিত। নতুন আচরণে নতুন code যোগ করো, পুরনো (কাজ-করা) code বদলিও না। উদাহরণ: if-else দিয়ে shape চেক করা AreaCalculator-এর বদলে, প্রতিটা Shape নিজের `area()` জানুক (polymorphism) — নতুন shape মানে শুধু নতুন class, পুরনো function অটুট।
```cpp
class Shape { public: virtual double area()=0; };
class Circle: public Shape { ... };
double totalArea(Shape* shapes[], int n){ double s=0; for(...) s+=shapes[i]->area(); return s; } // কখনো বদলাতে হয় না
```

**L — Liskov Substitution Principle**: child class parent-এর জায়গায় বসতে পারবে প্রোগ্রাম না ভেঙে (child parent-এর চুক্তি মানবে)। Classic উদাহরণ: Square extends Rectangle সমস্যাজনক — Rectangle-এ width/height স্বাধীন, Square-এ সমান রাখতে হয়, তাই substitution-এ অদ্ভুত আচরণ। সমাধান: is-a সম্পর্ক আচরণেও সত্য কিনা যাচাই করা (যেমন সব পাখি ওড়ে না — penguin-কে FlyingBird-এর বদলে শুধু Bird বানানো)।

**I — Interface Segregation Principle**: class-কে এমন interface implement করতে বাধ্য করো না যার method তার লাগে না। একটা বড় "fat" interface-এর বদলে ছোট নির্দিষ্ট interface ভালো। উদাহরণ: `Worker{work(); eat();}` — Robot-কে eat() লিখতে হয় (অর্থহীন)। সমাধান: আলাদা `Workable` ও `Eatable` interface, দরকারমতো implement করা।

**D — Dependency Inversion Principle**: উঁচু-স্তরের module নিচু-স্তরের concrete class-এর উপর নির্ভর করবে না — উভয়েই abstraction-এর উপর নির্ভর করবে। উদাহরণ: NotificationService ভিতরে সরাসরি EmailSender বানালে SMS-এ পাল্টাতে service বদলাতে হয়। সমাধান: `MessageSender` interface-এর উপর নির্ভর করা, concrete object বাইরে থেকে দেওয়া (এটাই Dependency Injection)।
```cpp
class MessageSender { public: virtual void send(string)=0; };
class NotificationService {
    MessageSender* sender;
public:
    NotificationService(MessageSender* s): sender(s) {}
    void notify(string msg){ sender->send(msg); }
};
```

**এক নজরে:**
| অক্ষর | Principle | এক লাইনে | মেটায় কোন সমস্যা |
|---|---|---|---|
| S | Single Responsibility | এক class, এক দায়িত্ব | এক বদল অন্য জায়গা ভাঙা |
| O | Open/Closed | extension খোলা, modification বন্ধ | নতুন feature-এ পুরনো কোড ভাঙা |
| L | Liskov Substitution | child parent-জায়গায় বসবে | ভুল inheritance |
| I | Interface Segregation | ছোট নির্দিষ্ট interface | অপ্রয়োজনীয় method বাধ্য |
| D | Dependency Inversion | abstraction-এর উপর নির্ভর | concrete-এ শক্ত coupling |

মূল কথা: SOLID-এর সব principle-এর লক্ষ্য এক — কোডকে **changeable** রাখা।

Practice (design exercises): Payment system (Card/bKash/Nagad — DIP+OCP+SRP একসাথে), 146. LRU Cache (SRP), Logger design (DIP), Notification system email/SMS/push (OCP+DIP)।

---

## Lesson 64 — Design Patterns: Singleton ও Dependency Injection

**প্রশ্ন ১ — Singleton Pattern কী?**
একটা class-এর শুধু একটাই object (instance) পুরো প্রোগ্রামে তৈরি হবে, সবাই সেটাই access করবে — এটা নিশ্চিত করা।
কেন দরকার: database connection pool, configuration/settings, logger, cache — যেগুলোর একটাই থাকা উচিত।
কীভাবে: constructor **private** (বাইরে থেকে `new` করা যায় না) + একটা **static getInstance()** method যা প্রথমবার object বানায়, পরে সেই একটাই ফেরত দেয়; copy constructor/assignment `= delete` করে duplicate ঠেকানো হয়।
```cpp
class Database {
    static Database* instance;
    int connectionCount;
    Database() : connectionCount(0) {}   // private constructor
public:
    static Database* getInstance() {
        if (instance == nullptr) instance = new Database();
        return instance;
    }
    void query(string q) { connectionCount++; }
    Database(const Database&) = delete;
    void operator=(const Database&) = delete;
};
Database* Database::instance = nullptr;
```
সতর্কতা: অতিরিক্ত ব্যবহারে global-state সমস্যা (testing কঠিন, hidden dependency)।

**প্রশ্ন ২ — Dependency Injection (DI) কী?**
একটা object তার dependency নিজে ভিতরে না বানিয়ে বাইরে থেকে গ্রহণ করা (সাধারণত constructor দিয়ে — "constructor injection"; এছাড়া setter injection, method injection)।
কেন দরকার: ভিতরে সরাসরি dependency বানালে (`EmailSender sender;`) — শক্ত coupling (অন্য sender-এ পাল্টাতে class বদলাতে হয়), testing কঠিন (mock দেওয়া যায় না)। এটা SOLID-এর **Dependency Inversion**-এর বাস্তব প্রয়োগ।
```cpp
class MessageSender { public: virtual void send(string)=0; };
class EmailSender: public MessageSender { void send(string m) override {...} };
class SMSSender: public MessageSender { void send(string m) override {...} };

class OrderService {
    MessageSender* sender;         // abstraction, concrete না
public:
    OrderService(MessageSender* s): sender(s) {}   // constructor injection
    void confirm(string item){ sender->send("নিশ্চিত: "+item); }
};
// EmailSender email; OrderService s1(&email);
// SMSSender sms;    OrderService s2(&sms);  // service-এর কোড না বদলেই sender পাল্টানো গেল
```
**Singleton vs DI**: Singleton hidden global dependency তৈরি করে; DI dependency স্পষ্ট করে (বাইরে থেকে দেওয়া) — তাই DI প্রায়ই Singleton-এর ভালো বিকল্প (testing সহজ)।

**প্রশ্ন ৩ — Design Pattern-এর ধরন (Gang of Four)**
- **Creational** (object কীভাবে তৈরি হয়): Singleton, Factory, Builder
- **Structural** (class/object কীভাবে জোড়া লাগে): Adapter, Decorator, Facade
- **Behavioral** (object কীভাবে যোগাযোগ করে): Observer, Strategy, Iterator

| Pattern | শ্রেণী | কী করে |
|---|---|---|
| Singleton | Creational | একটাই object |
| Factory | Creational | object তৈরির দায়িত্ব আলাদা |
| Observer | Behavioral | এক বদলালে সবাই জানে (event/subscriber) |
| Strategy | Behavioral | algorithm runtime-এ পাল্টানো |
| Decorator | Structural | object-এ আচরণ যোগ (মোড়ানো) |
| Adapter | Structural | দুই বেমানান interface জোড়া লাগানো |

Practice (design exercises): Singleton Config manager, DI দিয়ে PaymentService (Card/bKash inject), Observer pattern (event→subscribers), Factory (shape তৈরি নাম দিয়ে)।
