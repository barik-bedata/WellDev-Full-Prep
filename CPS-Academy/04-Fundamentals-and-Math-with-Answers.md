# WellDev Interview Prep — Fundamentals ও Math — Categorized, উত্তরসহ

> Source: CPS Academy — "WellDev Interview Prep — Bangla" course, Module 1 (Programming Fundamentals, Lessons 1–9), Module 16 (CS Fundamentals, Lesson 68), Module 17 (Aptitude, Lesson 69), ও Question Bank-এর "Short Questions (Theory)" section।

## ক্যাটাগরি
1. Programming Fundamentals (Variable, Pointer, Increment, Naming)
2. Data Structures — Theory Questions
3. Algorithms — Theory Questions
4. CS Fundamentals ও Systems (OS, Network, Web)
5. Math / Aptitude

---

# ১. Programming Fundamentals

## Lesson 1 - Variable Swap (C ও C++)

দুইটি Variable-এর মান অদলবদল (Swap) করা ইন্টারভিউয়ের একটি সাধারণ প্রশ্ন। 

### ১. Variable SWAP: C

**Function-এর বাইরে (সাধারণ Swap):**
```c
int a = 5, b = 10;
int temp = a;
a = b;
b = temp;
```

**Function-এর ভেতরে (Pointer ব্যবহার করে):**
C-তে Pass-by-Reference নেই, তাই Pointer ব্যবহার করতে হয়।
```c
void swapValues(int *a, int *b) {
    int temp = *a;
    *a = *b;
    *b = temp;
}
// কল করার নিয়ম: swapValues(&x, &y);
```

**যোগ-বিয়োগ দিয়ে (Temp ছাড়া):**
```c
void swapWithoutTemp(int *a, int *b) {
    *a = *a + *b;
    *b = *a - *b;
    *a = *a - *b;
}
```
*(সতর্কতা: যোগফল Integer-এর সীমা ছাড়িয়ে গেলে Overflow হতে পারে।)*

**গুণ-ভাগ দিয়ে (Temp ছাড়া):**
```c
void swapWithMultiplication(int *a, int *b) {
    *a = *a * *b;
    *b = *a / *b;
    *a = *a / *b;
}
```
*(সতর্কতা: b এর মান 0 হলে Division by Zero এরোর হবে, এবং Overflow দ্রুত হবে।)*

**XOR দিয়ে (Temp ও Overflow ছাড়া):**
```c
void swapWithXor(int *a, int *b) {
    if (a == b) return; // একই অ্যাড্রেস হলে মান 0 হয়ে যাবে, তাই এই চেকটি জরুরি
    *a ^= *b;
    *b ^= *a;
    *a ^= *b;
}
```
*(সতর্কতা: XOR শুধু Integer-এর ওপর কাজ করে, float/double এ কাজ করে না।)*

---

### ২. Variable SWAP: C++

C++ এর ক্ষেত্রে C-এর পদ্ধতিগুলোর পাশাপাশি আরও কিছু আধুনিক পদ্ধতি রয়েছে।

**Function-এর বাইরে (সাধারণ Swap):**
```cpp
int a = 5, b = 10;
int temp = a;
a = b;
b = temp;
```

**Function-এর ভেতরে (Reference ব্যবহার করে):**
C++ এ Pointer-এর বদলে Reference (`&`) ব্যবহার করা ভালো, কারণ এতে কোড পরিষ্কার থাকে।
```cpp
void swapValues(int &a, int &b) {
    int temp = a;
    a = b;
    b = temp;
}
// কল করার নিয়ম: swapValues(x, y);
```

**যোগ-বিয়োগ দিয়ে (Temp ছাড়া):**
```cpp
void swapWithoutTemp(int &a, int &b) {
    a = a + b;
    b = a - b;
    a = a - b;
}
```

**গুণ-ভাগ দিয়ে (Temp ছাড়া):**
```cpp
void swapWithMultiplication(int &a, int &b) {
    a = a * b;
    b = a / b;
    a = a / b;
}
```

**XOR দিয়ে (Temp ও Overflow ছাড়া):**
```cpp
void swapWithXor(int &a, int &b) {
    if (&a == &b) return;
    a ^= b;
    b ^= a;
    a ^= b;
}
```

**Template দিয়ে (যেকোনো Type এর জন্য):**
C++ এ Template ব্যবহার করে একটি Function দিয়েই integer, string, double সবই Swap করা যায়।
```cpp
template <typename T>
void swapValues(T &a, T &b) {
    T temp = a;
    a = b;
    b = temp;
}
// string, int, double সবকিছুর জন্যই কাজ করবে।
```
*(নোট: C প্রোগ্রামিংয়ে Template বলে কিছু নেই, এটি শুধুমাত্র C++ এর ফিচার।)*

**Production/Real Code এ Swap (`std::swap`):**
ইন্টারভিউতে উপরের পদ্ধতিগুলো জিজ্ঞেস করা হলেও, আসল প্রজেক্টে সবসময় C++ এর নিজস্ব `std::swap` ব্যবহার করা উচিত।
```cpp
#include <utility>
int a = 5, b = 10;
std::swap(a, b); 
```
*(এটি অনেক দ্রুত এবং বড় অবজেক্টের ক্ষেত্রে Copy না বানিয়ে Move Semantics ব্যবহার করে।)*

---

## Lesson 4 — "Single Number" | XOR-এর আসল শক্তি (LeetCode 136)

প্রশ্ন: array-তে প্রতিটা সংখ্যা দুইবার আছে, একটা ছাড়া। সেটা বের করো, O(n) time O(1) space।

Brute force: hash map-এ count রাখা — O(n) time কিন্তু O(n) space (শর্ত ভাঙে)।

Optimal (XOR):
```cpp
int singleNumber(vector<int>& nums) {
    int result = 0;
    for (int num : nums) result ^= num;
    return result;
}
```
কারণ: জোড়ায় থাকা সংখ্যা A^A=0 হয়ে কেটে যায়, বাকি থাকে একাকী সংখ্যা (0^X=X)। O(n) time, O(1) space।

গুরুত্বপূর্ণ পয়েন্ট:
- **ক্রম গুরুত্বপূর্ণ না** — XOR commutative ও associative, তাই array shuffle করলেও উত্তর একই।
- **Edge cases সব ঠিকভাবে কাজ করে** — একটা মাত্র element, negative সংখ্যা (two's complement bit pattern-এও A^A=0 সত্য), এমনকি একাকী সংখ্যা নিজেই 0 হলেও।
- **যদি প্রতিটা সংখ্যা তিনবার থাকে (LeetCode 137, Single Number II)** — XOR ভাঙে কারণ A^A^A=A (কাটে না)। সমাধান: প্রতিটা bit position-এ কতগুলো ১ আছে গুনে, %3 নিলে যা থাকে সেটাই উত্তরের bit।
```cpp
int singleNumberThrice(vector<int>& nums){
    int result=0;
    for(int i=0;i<32;i++){
        int cnt=0;
        for(int num:nums) if(num & (1<<i)) cnt++;
        if(cnt%3!=0) result |= (1<<i);
    }
    return result;
}
```
- **যদি দুইটা সংখ্যা একাকী থাকে (LeetCode 260, Single Number III)** — পুরো array XOR করলে দুই উত্তরের XOR পাওয়া যায় (a^b)। এর যেকোনো একটা সেট bit দিয়ে array-কে দুই ভাগে ভাগ করে প্রতি ভাগ আলাদাভাবে XOR করলে দুইটা উত্তর আলাদা হয়ে যায়:
```cpp
vector<int> singleNumberTwo(vector<int>& nums){
    long xorAll=0; for(int n:nums) xorAll^=n;
    int diffBit = xorAll & (-xorAll); // rightmost set bit
    int first=0, second=0;
    for(int n:nums){ if(n & diffBit) first^=n; else second^=n; }
    return {first, second};
}
```

Practice: 136 (মূল), 137 (তিনবার), 260 (দুইটা একাকী), 268 Missing Number, 389 Find the Difference

---

## Lesson 5 — "Write the output of the following code" | i++ আর ++i

প্রশ্ন:
```cpp
int i = 5;
cout << i++ << endl;
cout << ++i << endl;
```
উত্তর: **5** তারপর **7**।
- `i++` (post): আগে বর্তমান মান ফেরত দেয়, পরে বাড়ায় → cout পায় 5, তারপর i=6
- `++i` (pre): আগে বাড়ায়, পরে মান ফেরত দেয় → i=6→7, cout পায় 7

গুরুত্বপূর্ণ ফলো-আপ:
1) **++i = 10 চলে, i++ = 10 চলে না** — কারণ `++i` আসল variable-এর reference ফেরত দেয় (lvalue), `i++` পুরনো মানের একটা copy ফেরত দেয় (rvalue, তার উপর assign করা যায় না — "lvalue required" error)।
2) **কোনটা দ্রুত?** int-এর জন্য কোনো পার্থক্য নাই (compiler optimize করে ফেলে একই instruction-এ)। কিন্তু iterator/object-এ `it++` একটা পুরো copy বানায় (constructor চলে), `++it` বানায় না — তাই object/iterator-এ `++it` অভ্যাস করা ভালো।
3) **i = i++ + ++i;** → **Undefined Behavior**। একই statement-এ একই variable দুইবার modify, `+`-এর দুই পাশের ক্রম unsequenced। সঠিক উত্তর: "এর কোনো নির্দিষ্ট উত্তর নাই, আমি এমন code লিখব না।"
4) **cout << i++ << ++i;** — C++17-এর আগে UB। **C++17 থেকে** `<<`-এর operand-গুলোর ক্রম নির্দিষ্ট (বাম থেকে ডান), তাই ফলাফল নির্দিষ্ট: i=5 হলে output **"57"**। (`+` এখনো unsequenced, `<<` C++17 থেকে sequenced — এটাই পার্থক্য)
5) **নিজের class-এ operator++ overload:**
```cpp
class Counter {
    int value;
public:
    Counter(int v=0):value(v){}
    Counter& operator++() { value++; return *this; }       // pre — কোনো copy নাই
    Counter operator++(int) {                                 // post — dummy int parameter দিয়ে আলাদা করা হয়
        Counter old = *this;  // এখানেই copy তৈরি হয় — এই জন্যই it++ ধীর
        value++;
        return old;
    }
    int get() const { return value; }
};
```

মূল কথা: **++i আসল জিনিস দেয়, i++ একটা copy দেয়** — Lesson 1-এর "assignment মানে copy" ধারণারই ধারাবাহিকতা।

Practice: এখানে LeetCode equivalent নাই (ভাষার নিয়ম, algorithm না) — নিজে হাতে code চালিয়ে verify করাই অনুশীলন। (Godbolt.org-এ assembly compare করা যায়।)

---

## Lesson 6 — একই statement-এ একাধিক Increment | Sequence Point-এর ফাঁদ

প্রশ্ন: `int i=5; i = i++ + ++i; cout << i;` — output কী?
**উত্তর: নির্দিষ্ট কোনো উত্তর নাই — Undefined Behavior (UB)।** কারণ `+`-এর দুই পাশের কাজ (i++ ও ++i) unsequenced, আর একই statement-এ একই variable দুইবার modify হচ্ছে — এটাই UB-র সংজ্ঞা।

গুরুত্বপূর্ণ পয়েন্ট:
1) **Sequence point** (পুরনো concept, C++03): এমন বিন্দু যেখানে আগের সব side-effect ঘটে গেছে নিশ্চিত (যেমন `;`)। নিয়ম ছিল: দুই sequence point-এর মাঝে একটা variable একবারের বেশি বদলানো যাবে না।
   C++11 থেকে তিনটা সম্পর্ক এসেছে: **sequenced before** (নির্দিষ্ট ক্রম), **unsequenced** (ক্রম অনির্দিষ্ট, মিশেও যেতে পারে — বিপজ্জনক), **indeterminately sequenced** (ক্রম অনির্দিষ্ট, কিন্তু মিশবে না)।

2) **cout << i++ << ++i; কেন নিরাপদ (C++17 থেকে)?** — C++17 থেকে `<<`-এর operand বাম থেকে ডানে sequenced। কিন্তু `+` এখনো unsequenced। তাই operator ভেদে আলাদা: `<<` নিরাপদ, `+` এখনো UB।

3) **a[i] = i++; (C++17)** — C++17-এ assignment-এর নিয়ম: ডান পাশ (i++) আগে চলে, তারপর বাম পাশ (a[i]) হিসাব হয়। তাই i=0 হলে ফলাফল হয় **a[1]=0** (a[0] না) — কারণ ডানপাশ আগে execute হয়ে i ইতিমধ্যে ১ হয়ে গেছে।

4) **f(i++, i++) — UB না কিন্তু unspecified**: function argument-গুলো indeterminately sequenced — ক্রম অনির্দিষ্ট কিন্তু একটা শেষ না হয়ে আরেকটা শুরু হবে না (মিশবে না)। তাই `f(5,6)` বা `f(6,5)` দুইটাই সম্ভব, কিন্তু crash/আজেবাজে মান হবে না। **UB বনাম Unspecified পার্থক্য:**

| | Undefined Behavior | Unspecified |
|---|---|---|
| মানে | কোনো নিয়ম নাই | কয়েকটা সম্ভাবনা, একটা ঘটবে |
| Crash সম্ভব? | হ্যাঁ | না |
| উদাহরণ | i=i+++++i; | f(i++,i++) |

5) **প্র্যাকটিক্যাল নিয়ম (মুখস্থ না করে এইটা মনে রাখো):** এক statement-এ এক variable-কে একবারের বেশি বদলিও না, আর যেটা বদলাচ্ছ সেটা একই statement-এ আবার পড়োও না। জটিল হলে ভেঙে আলাদা লাইনে লিখো।

Tools: `g++ -Wall` দিলে UB সম্পর্কে warning দেয়; `g++ -fsanitize=undefined` (UBSan) দিয়ে runtime-এ UB ধরা যায়।

Practice: LeetCode equivalent নাই (ভাষার নিয়ম, algorithm না)। Godbolt.org-এ C++14 vs C++17 তুলনা করে দেখা যায়।

---

## Lesson 7 — Array-র নাম print করলে কী print হয়? (Array-to-Pointer Decay)

প্রশ্ন: `int ara[5]={10,20,30,40,50}; printf("%p", ara);` কী print হয়?
**উত্তর: Base address** (প্রথম element-এর ঠিকানা) — একে বলে **array-to-pointer decay**। পুরো array (২০ byte) ক্ষয়ে গিয়ে শুধু একটা ঠিকানায় (int*) পরিণত হয়।

গুরুত্বপূর্ণ পয়েন্ট:
1) **ara == &ara[0]** — মান ও type দুইটাই এক (দুইটাই int*)। তাই `ara[i]` আসলে `*(ara+i)`-এর সংক্ষিপ্ত রূপ, এবং যোগ commutative বলে `i[ara]`-ও বৈধ (লেখা উচিত না)।

2) **ara vs &ara — মান এক, type আলাদা!**
   - `ara` → type `int*` (একটা int-এর ঠিকানা)
   - `&ara` → type `int (*)[5]` (পুরো ৫-element array-র ঠিকানা)
   প্রমাণ: `ara+1` → ৪ byte এগোয়; `&ara+1` → ২০ byte এগোয় (পুরো array পার হয়)।

3) **Array কি pointer? — না।** Array হলো একটানা কিছু memory ঘর; pointer একটা variable যাতে ঠিকানা জমা থাকে। প্রমাণ: `sizeof(ara)`=20 কিন্তু `sizeof(int*)`=8; আর `ara++` compile error দেয় (ara-র নিজস্ব কোনো memory/variable নাই, শুধু নাম)।

4) **printf("%p", ara) টেকনিক্যালি ভুল** — `%p` চায় `void*`, কিন্তু ara decay হয়ে `int*`। বেশিরভাগ platform-এ representation এক বলে কাজ করে, কিন্তু standard-এ UB। সঠিক: `printf("%p", (void*)ara);`

5) **sizeof(ara) কোথায় ২০, কোথায় ৮?** — main()-এর ভিতরে (যেখানে array declare করা) sizeof(ara)=20, decay হয় না। Decay না হওয়ার ৩ জায়গা: `sizeof(ara)`, `&ara`, আর C++ reference-এ bind করলে (`int (&ref)[5] = ara;`)। বাকি সব জায়গায় (function argument-এ পাঠালে) decay হয়ে যায় — সেটাই Lesson 8-এর বিষয় (function-এর ভিতরে sizeof(ara) দেয় 8, কারণ ওটা এখন শুধু একটা pointer)।

Element সংখ্যা বের করার idiom: `int count = sizeof(ara)/sizeof(ara[0]);` — কিন্তু এটা শুধু original array-তে কাজ করে, function-এ পাঠানো array-তে না (decay হয়ে যাওয়ায়)।

Practice: এই প্রশ্নের সরাসরি LeetCode নাই (memory-model concept, algorithm না)। Godbolt.org-এ assembly compare করে verify করা যায়।

---

## Lesson 8 — sizeof(ara) Function-এর ভিতরে আর বাইরে আলাদা কেন?

প্রশ্ন: main()-এর ভিতরে `sizeof(ara)` (ara হলো int[5]) দেয় ২০, কিন্তু একই array একটা function-এ পাঠিয়ে ভিতরে sizeof করলে দেয় ৮ — কেন?

উত্তর: Function-এর সীমানা পার হওয়ার সময় array **decay** করে pointer-এ পরিণত হয় (Lesson 7)। বাইরে ara সত্যিকারের array (sizeof=20=5×4), ভিতরে ara একটা pointer (sizeof=8, ৬৪-bit machine-এ)।

গুরুত্বপূর্ণ পয়েন্ট:
1) **void f(int ara[5]) লিখলেও [5] সম্পূর্ণ উপেক্ষিত হয়** — Compiler নিয়ম: parameter-এ array লিখলে চুপচাপ pointer বানিয়ে দেয়। `int ara[5]`, `int ara[100]`, `int ara[]`, `int *ara` — চারটাই হুবহু একই function। ফলে ৩-element array পাঠালেও কোনো error/warning ছাড়াই compile ও run হয় (এবং out-of-bounds access করলে garbage value)।
2) **ভিতরে দৈর্ঘ্য জানার উপায় নাই** — তথ্যটা সীমানা পার হওয়ার সময় হারিয়ে যায়। সমাধান: দৈর্ঘ্য আলাদা parameter হিসেবে পাঠাতে হবে — এটাই C-র চিরাচরিত রীতি (`memcpy(dest,src,n)`, `qsort(base,count,size,cmp)` ইত্যাদি)। `sizeof(ara)/sizeof(ara[0])` কৌশল ভিতরে ব্যবহার করলে ভুল উত্তর দেয় (8/4=2)।
3) **C++-এ সমাধান (৩ উপায়):**
   - Reference: `void f(int (&ara)[5])` — decay হয় না, sizeof ঠিক দেয়, ভুল সাইজ পাঠালে compile error।
   - Template: `template<size_t N> void f(int (&ara)[N])` — যেকোনো সাইজে কাজ করে, N compiler নিজেই বের করে।
   - Container: `std::vector<int>&` বা `std::array` — নিজের আকার নিজেই বহন করে (এই জন্যই LeetCode সবসময় `vector<int>& nums` ব্যবহার করে, `int* nums, int n` না)।
4) **2D array পাঠালে** — শুধু প্রথম dimension decay হয়, বাকিগুলো টিকে থাকে। `int ara[3][4]` হয়ে যায় `int (*)[4]` (৩ হারায়, ৪ থাকে, কারণ row-এর length ছাড়া `ara[i][j]`-এর ঠিকানা হিসাব করা যায় না)। তাই parameter-এ প্রথমটা বাদে বাকি সব dimension লিখতেই হয়: `void f(int ara[][4], int rows)`।
5) **void f(int ara[5]) লেখা ভালো অভ্যাস কি না?** — বিতর্কিত। বিপক্ষে যুক্তি প্রবল: এটা "মিথ্যা প্রতিশ্রুতি" (পাঠক ভাবে compiler ৫ যাচাই করছে, করছে না)। ভালো practice: C-তে `int *ara, int n` (যা সত্যি তাই লেখা), C++-এ vector/reference ব্যবহার করা।

Practice: এই প্রশ্নের সরাসরি LeetCode নাই (memory/language-model concept)। যাচাই: godbolt.org-এ assembly দেখে parameter int* হয়ে যাওয়া কনফার্ম করা যায়।

---

## Lesson 9 — Function-এর উপযুক্ত নাম দেওয়া (Code Quality / Naming)

প্রশ্ন: `int f(int n){ if(n<=1) return n; return n+f(n-1); }` — এই recursive function (প্রথম N সংখ্যার যোগফল বের করে) এর একটা উপযুক্ত নাম দাও।

উত্তর: **sumOfFirstN** — মূলনীতি: **নাম বলবে function কী দেয়, কীভাবে দেয় সেটা না।** যাচাই: call site পড়ো — `sumOfFirstN(100)` ইংরেজি বাক্যের মতো শোনায়, `f(100)` শোনায় না।

গুরুত্বপূর্ণ পয়েন্ট:
1) **calculateSum ভালো নয়** — "calculate" শব্দ কোনো তথ্য দেয় না (প্রতিটা function-ই হিসাব করে); "Sum" অসম্পূর্ণ (কীসের sum?)। মাপকাঠি: প্রতিটা শব্দকে জিজ্ঞেস করো "তুলে দিলে কিছু হারায়?"
2) **recursiveSum ভুল** — Implementation (recursion/loop/formula) নামে লেখা উচিত না, কারণ এটা internal detail এবং ভবিষ্যতে বদলাতে পারে (আজ recursion, কাল loop) — তখন নাম মিথ্যা হয়ে যাবে বা সব call site বদলাতে হবে।
3) **অতিরিক্ত বিস্তারিত নামও (sumOfFirstNNaturalNumbers) খারাপ** — "NaturalNumbers" কোনো নতুন তথ্য দেয় না, শুধু লাইন লম্বা করে। নিয়ম: নাম যত ছোট scope-এ থাকবে, তত ছোট/সংক্ষিপ্ত হতে পারে।
4) **Parameter নাম `n` ঠিক আছে** — কারণ এর scope মাত্র কয়েক লাইন, এবং "কতগুলো" বোঝাতে `n` প্রতিষ্ঠিত C-রীতি। কিন্তু বড় scope/অনেক parameter থাকলে (যেমন n,m,k,x — ৩০ লাইন জুড়ে) পূর্ণ নাম দরকার। নিয়ম: **scope যত বড়, নাম তত স্পষ্ট হওয়া উচিত।**
5) **আসলে সূত্র দিয়েই তো হয় (n*(n+1)/2)!** — হ্যাঁ, এবং এটাই সবচেয়ে ভালো সমাধান:
```cpp
long long sumOfFirstN(int n) {
    return (long long)n * (n + 1) / 2;
}
```
Recursion: O(n) time, O(n) space (stack — বড় n-এ stack overflow)। Formula: O(1) time, O(1) space। **cast (long long) গুণ করার আগে দিতে হবে, নাহলে int overflow হবে** (n=100000 হলে উত্তর ৫০০ কোটি, int-এ ধরে না)। n ও n+1-এর একটা সবসময় জোড় হওয়ায় /2 সবসময় নিরাপদ (fraction থাকে না)।

**মূল শিক্ষা:** implementation recursion থেকে formula-তে সম্পূর্ণ বদলে গেলেও নাম `sumOfFirstN` একই থাকল — এটাই প্রমাণ করে কেন নামে "কী" থাকা উচিত, "কীভাবে" না।

Practice: এই প্রশ্নের সরাসরি LeetCode নাই। অনুশীলন: নিজের পুরনো LeetCode solution-গুলোর `helper`/`solve`/`dfs` জাতীয় নাম বদলে অর্থপূর্ণ নাম দেওয়া (যেমন `dfs` → `countIslandsFrom`)।

---

# ২. Data Structures — Theory Questions

**Q: Array vs Linked List — কোনটা কখন ভালো? পার্থক্য কী?**

| | Array | Linked List |
|---|---|---|
| Memory | একটানা (contiguous) | ছড়ানো, প্রতিটা node আলাদা |
| Random access | O(1) — index দিয়ে সরাসরি | O(n) — শুরু থেকে হেঁটে যেতে হয় |
| Insert/Delete (মাঝখানে) | O(n) — বাকি সব সরাতে হয় | O(1) — যদি node-এর reference থাকে |
| আকার | Fixed (static array) বা resize-ব্যয়বহুল | Dynamic — সহজে বাড়ে-কমে |
| Memory overhead | কম (শুধু data) | বেশি (data + next pointer) |
| Cache performance | ভালো (contiguous memory, cache-friendly) | খারাপ (scattered memory) |

সিদ্ধান্ত: বেশি random access/search লাগলে **array/vector**; বেশি insert/delete (বিশেষত মাঝখানে/শুরুতে) লাগলে **linked list**। বাস্তবে বেশিরভাগ ক্ষেত্রে array (vector) ভালো — cache-friendly ও simple, তাই linked list শুধু নির্দিষ্ট প্রয়োজনে (যেমন LRU cache-এর ভিতরে) ব্যবহার হয়।

**Q: Linked List-এর সুবিধা ও অসুবিধা কী?**
- সুবিধা: Dynamic size (রানটাইমে বাড়ে-কমে), মাঝখানে/শুরুতে O(1) insert-delete (node reference থাকলে), memory fragmentation সহনশীল (contiguous block দরকার নেই)।
- অসুবিধা: Random access নেই (O(n) traversal লাগে), extra memory (pointer/reference-এর জন্য), cache-unfriendly (scattered memory), reverse traversal কঠিন (singly linked list-এ)।

**Q: Linked List-এর Head কী? এটি কি কোনো Node নাকি Pointer?**
Head মূলত কোনো Node নয়, এটি হলো একটি **Pointer** (বা Reference), যা Linked List-এর প্রথম Node-এর মেমরি অ্যাড্রেস (ঠিকানা) ধরে রাখে। Head-এর মাধ্যমে আমরা পুরো List-টি অ্যাক্সেস করতে পারি। যদি List ফাঁকা (empty) থাকে, তবে Head-এর মান `NULL` হয়।

**Q: Circular Linked List কী?**
এমন linked list যেখানে শেষ node আবার প্রথম node-কে নির্দেশ করে (লেজ মাথায় ফিরে আসে) — তাই কোনো `NULL` দিয়ে শেষ হয় না। ব্যবহার: round-robin scheduling, circular buffer। Insertion/deletion সাধারণ linked list-এর মতোই, শুধু শেষ node-এর `next` সবসময় head-কে নির্দেশ করে রাখতে হয় (বিশেষ care লাগে যেন loop ভুলভাবে না ভাঙে)।

**Q: Stack ও Queue বর্ণনা করো।**
- **Stack (LIFO — Last In First Out):** শেষে যা ঢোকে, প্রথমে তা বের হয়। Operations: `push` (উপরে যোগ), `pop` (উপর থেকে সরানো), `top/peek`। ব্যবহার: function call stack, undo operation, expression evaluation, DFS।
- **Queue (FIFO — First In First Out):** প্রথমে যা ঢোকে, প্রথমে তা বের হয়। Operations: `enqueue` (পিছনে যোগ), `dequeue` (সামনে থেকে সরানো)। ব্যবহার: task scheduling, BFS, printer queue।

**Q: Graph ও Tree-এর পার্থক্য কী?**
| | Tree | Graph |
|---|---|---|
| Cycle | নেই (acyclic) | থাকতে পারে |
| Root | একটা নির্দিষ্ট root থাকে | root বলে কিছু নেই (unless rooted graph) |
| Edge সংখ্যা | ঠিক n−1 (n নোডে) | যেকোনো সংখ্যা |
| Path | দুই নোডের মধ্যে ঠিক একটাই path | একাধিক path থাকতে পারে |
| উদাহরণ | file system, org hierarchy | social network, road map |
এক লাইনে: **Tree হলো একটা বিশেষ ধরনের Graph** — connected, acyclic, এবং n নোডে n−1 edge।

**Q: DFS ও BFS-এ কোন data structure লাগে?**
- **DFS (Depth-First Search):** Stack (explicit, অথবা recursion-এর call stack implicit ভাবে)।
- **BFS (Breadth-First Search):** Queue।

**Q: Linked List-এ binary search চলে না কেন?**
Binary search-এর মূল শর্ত — মাঝখানের element-এ **O(1) সময়ে সরাসরি (random access)** পৌঁছানো (`arr[mid]`)। Linked list-এ random access নেই — মাঝখানে পৌঁছাতে head থেকে হেঁটে যেতে হয়, যা O(n)। তাই প্রতি ধাপে "মাঝখানে যাওয়া"-ই O(n), পুরো binary search তখন O(n log n) হয়ে যায় — যা সাধারণ linear search-এর (O(n)) চেয়ে খারাপ। সমাধান: sorted linked list-কে BST-তে রূপান্তর করে সেখানে search করা ([109. Convert Sorted List to BST](https://leetcode.com/problems/convert-sorted-list-to-binary-search-tree/))।

---

# ৩. Algorithms — Theory Questions

**Q: Insertion Sort কীভাবে কাজ করে?**
Array-কে দুই ভাগে ভাবা হয় — sorted অংশ (শুরুতে) ও unsorted অংশ। প্রতি ধাপে unsorted অংশের প্রথম element নিয়ে sorted অংশের সঠিক জায়গায় বসানো হয় (কার্ডের হাত সাজানোর মতো)। Time: O(n²) worst/average, O(n) best (already sorted)। Space: O(1) — in-place। ছোট বা প্রায়-sorted array-তে দক্ষ।
```cpp
void insertionSort(vector<int>& arr) {
    for (int i = 1; i < arr.size(); i++) {
        int key = arr[i], j = i - 1;
        while (j >= 0 && arr[j] > key) { arr[j+1] = arr[j]; j--; }
        arr[j+1] = key;
    }
}
```

**Q: Selection Sort ও Insertion Sort-এর পার্থক্য কী?**
- **Selection Sort:** প্রতি ধাপে unsorted অংশ থেকে **সবচেয়ে ছোট element খুঁজে** sorted অংশের শেষে বসায় (swap করে)। সবসময় O(n²) — best/worst/average সব ক্ষেত্রেই (তুলনা কমে না)।
- **Insertion Sort:** প্রতি ধাপে একটা element নিয়ে sorted অংশে সঠিক জায়গায় **shift করে বসায়**। Best case O(n) (প্রায়-sorted data-তে দ্রুত)।
মূল পার্থক্য: Selection Sort সবসময় "min খুঁজে বসায়" (swap-ভিত্তিক), Insertion Sort "বসানোর জায়গা খুঁজে shift করে" (এবং প্রায়-sorted data-তে অনেক দ্রুত)।

**Q: BFS, DFS, Merge Sort, Quick Sort, ও Recursion ব্যাখ্যা করো।**
- **BFS:** Level অনুযায়ী (স্তরে স্তরে) graph/tree traverse করা, Queue ব্যবহার করে। Shortest path (unweighted graph-এ) বের করতে ব্যবহৃত।
- **DFS:** যতদূর সম্ভব এক পথে গভীরে গিয়ে, তারপর ফিরে এসে (backtrack) অন্য পথ — Stack/recursion ব্যবহার করে।
- **Merge Sort:** Divide-and-conquer — array-কে অর্ধেক অর্ধেক ভাগ করে (recursion), প্রতিটা অংশ sort করে, তারপর merge করে জোড়া লাগানো। O(n log n) সবসময় (stable, কিন্তু O(n) extra space লাগে)।
- **Quick Sort:** একটা pivot বেছে, তার চেয়ে ছোট-বড় দুই ভাগে partition করে, প্রতি ভাগে recursively sort করা। Average O(n log n), worst case O(n²) (খারাপ pivot হলে), কিন্তু in-place (O(log n) space)।
- **Recursion:** একটা function নিজেকে নিজে কল করা, একটা base case-এ থেমে যাওয়া পর্যন্ত। প্রতিটা call stack-এ জমা হয় (call stack), base case-এ পৌঁছে ফলাফল ফিরিয়ে আনে।

---

# ৪. CS Fundamentals ও Systems

## Lesson 68 — CS Fundamentals: Kernel, HTTP, Network, JS Event

**১. Kernel কী?**
OS-এর মূল অংশ ("হৃদয়") — hardware ও software-এর মধ্যে সেতু। কাজ: **process management** (CPU scheduling), **memory management**, **device management** (driver দিয়ে hardware নিয়ন্ত্রণ), **system calls** (application-এর hardware-অনুরোধ সামলানো)। Application সরাসরি hardware ছুঁতে পারে না (নিরাপত্তার জন্য) — application চলে **user mode**-এ, kernel চলে **kernel mode**-এ (পূর্ণ অধিকার)। এই বিভাজন একটা program-কে পুরো সিস্টেম crash করা থেকে রক্ষা করে। উপমা: kernel = kitchen manager, application = গ্রাহক (সরাসরি রান্নাঘরে ঢুকতে পারে না, order/system-call দেয়)।

**২. Common HTTP Status Codes**
প্রথম অঙ্ক শ্রেণী নির্দেশ করে: **2xx**=সফল, **3xx**=redirect, **4xx**=client-এর ভুল, **5xx**=server-এর ভুল।
| Code | নাম | মানে |
|---|---|---|
| 200 | OK | সফল |
| 201 | Created | নতুন resource তৈরি (POST) |
| 301 | Moved Permanently | URL স্থায়ীভাবে বদলেছে |
| 400 | Bad Request | client-এর পাঠানো data ভুল |
| 401 | Unauthorized | login/authentication লাগবে ("তুমি কে জানি না") |
| 403 | Forbidden | login আছে কিন্তু access নাই ("জানি, কিন্তু ঢুকতে দেব না") |
| 404 | Not Found | resource নাই |
| 500 | Internal Server Error | server-এ কোড ভাঙল |
| 503 | Service Unavailable | server ব্যস্ত/সাময়িক down |

**৩. Network Address কী, কীভাবে বের করবে?**
Device-কে network-এ চেনানোর ঠিকানা — প্রধানত **IP address**: IPv4 (192.168.1.10 জাতীয়, ৪টা 0-255 সংখ্যা) বা IPv6 (নতুন, লম্বা, IPv4 ফুরিয়ে যাওয়ায় দরকার)। সাথে **MAC address** (network card-এর স্থায়ী hardware ঠিকানা)।
বের করার কমান্ড: Windows-এ `ipconfig`, Linux/Mac-এ `ifconfig` বা `ip addr`। Public IP: browser-এ "what is my IP" বা `curl ifconfig.me`।
বিশেষ address: `127.0.0.1` (localhost/নিজের device), Private range (`192.168.x.x`, `10.x.x.x` — শুধু local network-এ)। **Subnet mask** (যেমন `255.255.255.0`) বলে IP-র কোন অংশ network, কোন অংশ device।

**৪. JavaScript Event ও Document-এ কাজ**
Event = user/browser-এর ঘটনা (click, keypress, load, mouseover ইত্যাদি) যাতে JS সাড়া দেয়।
```javascript
document.getElementById("myBtn").addEventListener("click", function() {
    alert("বাটন চাপা হলো!");
});
```
Common events: `click`/`dblclick` (মাউস), `keydown`/`keyup` (কীবোর্ড), `submit` (form), `load` (page), `mouseover`/`mouseout`, `change` (input)।
**Event bubbling**: child element-এ event ঘটলে তা parent-দের দিকেও "bubble up" করে (ভিতর থেকে বাইরে) — এই বৈশিষ্ট্য দিয়ে **event delegation** করা যায় (অনেক child-এর জন্য parent-এ একটা মাত্র listener)। Listener function একটা **event object** পায় যাতে বিস্তারিত তথ্য থাকে (কোন key, মাউস position, কোন element)।

**এক নজরে:**
| প্রশ্ন | এক লাইনে |
|---|---|
| Kernel | OS-এর মূল, hardware-software সেতু; process/memory/device+syscall |
| HTTP codes | 2xx সফল, 3xx redirect, 4xx client ভুল, 5xx server ভুল |
| Network address | IP (IPv4/IPv6) দিয়ে device চেনা; ipconfig/ifconfig; 127.0.0.1=localhost |
| JS event | user/browser ঘটনা; addEventListener; bubbling (ভিতর→বাইরে) |

**Q: UI Design উন্নত করতে কী কী step নেবে?** (Discussion-type প্রশ্ন)
সংক্ষিপ্ত কাঠামো: (১) **User research** — আসল ব্যবহারকারীর প্রয়োজন বোঝা (২) **Consistency** — রং/font/spacing একই নিয়মে সব জায়গায় (design system/style guide) (৩) **Simplicity/Clarity** — অপ্রয়োজনীয় জিনিস বাদ, গুরুত্বপূর্ণ action সহজে দৃশ্যমান (visual hierarchy) (৪) **Feedback** — ব্যবহারকারীর প্রতিটা action-এ স্পষ্ট সাড়া (loading state, error message) (৫) **Accessibility** — color contrast, keyboard navigation, screen-reader সহায়তা (৬) **Testing/Iteration** — real user দিয়ে test করে বারবার উন্নত করা (A/B testing)।

---

# ৫. Math / Aptitude

## Lesson 69 — Aptitude: Percentage, Profit-Loss, Speed-Distance-Time (দ্রুত গণিত)

**১. Percentage মূল সূত্র**
- x-এর p% = x × p/100
- a, b-এর কত শতাংশ = (a/b) × 100
- p% বৃদ্ধি → নতুন = x × (1 + p/100)
- p% হ্রাস → নতুন = x × (1 − p/100)
**ফাঁদ:** p% বৃদ্ধি তারপর p% হ্রাস করলে মূলে ফেরে না! (১০০ থেকে ২০% বাড়িয়ে ১২০, তারপর ২০% কমালে ৯৬, ১০০ না — কারণ দ্বিতীয়বার বড় সংখ্যার (১২০) উপর হিসাব)
**Successive % পরিবর্তন সূত্র:** a% তারপর b% = a + b + (ab/100)% । উদাহরণ: 20% + (−20%) = 20−20+(20×−20/100) = −4% (৪% কমে)।

**২. Profit-Loss**
- Profit = SP−CP (SP>CP হলে); Loss = CP−SP (CP>SP হলে)
- **Profit%/Loss% সবসময় CP-র উপর হিসাব হয়, SP-র উপর না** (সবচেয়ে common ভুল!): Profit% = (Profit/CP)×100
- SP = CP × (1 + Profit%/100)
উদাহরণ: ৮০-তে কিনে ১০০-তে বিক্রি → Profit=20, Profit%=(20/80)×100=**25%** (SP=100-এর উপর হিসাব করলে ভুল ২০% হতো)

**৩. Speed-Distance-Time**
- মূল সূত্র: **Distance = Speed × Time**
- একক রূপান্তর: km/h → m/s = ×5/18; m/s → km/h = ×18/5
- **গড় Speed ফাঁদ:** সমান দূরত্ব ভিন্ন speed-এ গেলে গড় speed সরল গড় (mean) না, বরং **harmonic mean**: গড় speed = 2×s1×s2/(s1+s2)। উদাহরণ: যাওয়া ৪০, ফেরা ৬০ km/h → গড় = 2×40×60/(40+60) = **48** km/h (৫০ না, কারণ ধীর গতিতে বেশি সময় লাগে)
- **Relative speed:** একই দিকে ভ্রমণ করলে speed **বিয়োগ** (s1−s2); বিপরীত দিকে হলে speed **যোগ** (s1+s2)

**৪. দ্রুত হিসাবের কৌশল**
- শতাংশ-ভগ্নাংশ মুখস্থ: 50%=1/2, 25%=1/4, 20%=1/5, 10%=1/10, 33.3%=1/3, 12.5%=1/8
- শতাংশ উল্টানো: a-এর b% = b-এর a% (যেটা সহজ হিসাব করো)
- MCQ-তে estimation দিয়ে অসম্ভব option বাদ দেওয়া
- একক সবসময় মিলিয়ে নেওয়া (m/s vs km/h, ঘণ্টা vs মিনিট) — ভুল একক সবচেয়ে common ভুল

**তিনটা মূল ফাঁদ মনে রাখো:**
1. বৃদ্ধি + সমান হ্রাস মূলে ফেরে না
2. Profit%/Loss% সবসময় CP-র উপর, SP-র উপর না
3. গড় speed সরল গড় না, harmonic mean

Practice: এটা programming না, তাই LeetCode নেই — R.S. Aggarwal জাতীয় aptitude বই, IndiaBix-এর মতো অনলাইন quiz, campus placement paper দিয়ে সময় ধরে দৈনিক অনুশীলন উপকারী।
