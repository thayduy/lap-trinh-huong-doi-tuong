# CHƯƠNG 3: KẾ THỪA & ĐA HÌNH – TRÁI TIM CỦA OOP

## 📚 MỤC TIÊU HỌC TẬP

Sau khi hoàn thành chương này, bạn sẽ:
- Hiểu sâu về Inheritance (Kế thừa) và khi nào nên dùng
- Phân biệt được Overriding và Overloading
- Sử dụng Abstract Class và Interface đúng cách
- Nắm vững Polymorphism và cơ chế Dynamic Binding
- Áp dụng Composition over Inheritance trong thiết kế
- Loại bỏ if/else bằng Polymorphism

---

## 📋 KIẾN THỨC CẦN CÓ (PREREQUISITES)

Trước khi học chương này, bạn cần nắm vững:

- [ ] ✅ **Class và Object** (Chương 2, phần 2.1)
  - Hiểu class là gì, object là gì
  - Biết cách tạo object với `new`
  - Hiểu fields và methods

- [ ] ✅ **Access Modifiers** (Chương 2, phần 2.2)
  - `private`, `protected`, `public`
  - Hiểu khi nào dùng modifier nào

- [ ] ✅ **Constructors** (Chương 2, phần 2.3)
  - Cách định nghĩa constructor
  - Constructor overloading
  - Từ khóa `this`

- [ ] ✅ **Method cơ bản** (Chương 2, phần 2.3)
  - Cách định nghĩa và gọi method
  - Tham số và return type

> **💡 Lưu ý:** Chương này là **trái tim của OOP**. Hãy đảm bảo bạn đã nắm vững Chương 2 trước khi học!

---

## 🎓 HƯỚNG DẪN HỌC SÂU CHƯƠNG 3

### Cách học hiệu quả (3 bước)

1. **Đọc lý thuyết** từng mục (3.1 → 3.6), không nhảy cóc.
2. **Chạy code mẫu** trong thư mục `CODE/VI_DU/CHUONG_3/` (đã test trên Java 17).
3. **Tự trả lời** câu hỏi cuối mỗi mục trước khi sang mục tiếp theo.

### Bảng map: Lý thuyết ↔ Code chạy thử

| Mục lý thuyết | Thư mục code | Lệnh chạy |
|---------------|--------------|-----------|
| 3.1 Kế thừa | `01_Inheritance/` | `javac *.java && java Main` |
| 3.2 Override/Overload | `02_OverrideOverload/` | `javac Main.java && java Main` |
| 3.3 Abstract | `03_Abstract/` | `javac Main.java && java Main` |
| 3.4 Interface | `04_Interface/` | `javac Main.java && java Main` |
| 3.5 Polymorphism | `05_Polymorphism/` | `javac Main.java && java Main` |
| 3.6 Composition | `06_Composition/` | `javac Main.java && java Main` |

> Chi tiết output mong đợi: xem `CODE/VI_DU/README.md`.

---

## 3.1. KẾ THỪA (INHERITANCE) – QUAN HỆ "IS-A"

### 3.1.1. Inheritance là gì?

#### Định nghĩa

**Inheritance (Kế thừa)** là cơ chế cho phép một class (subclass/child class) kế thừa các đặc điểm và hành vi từ class khác (superclass/parent class).

**Quan hệ "IS-A":**
- Subclass **LÀ MỘT** superclass
- Ví dụ: `Dog` **LÀ MỘT** `Animal`

#### Ví dụ thực tế

**Tưởng tượng:**
- **Class `Animal`**: Có chung đặc điểm (tên, tuổi) và hành vi (ăn, ngủ)
- **Class `Dog` extends Animal**: Kế thừa từ Animal, thêm hành vi riêng (sủa)
- **Class `Cat` extends Animal**: Kế thừa từ Animal, thêm hành vi riêng (kêu meo)

**Lợi ích:**
- ✅ Tái sử dụng code (không phải viết lại)
- ✅ Dễ bảo trì (sửa Animal → tất cả subclass tự động có)
- ✅ Mô hình hóa quan hệ thực tế

> **🧬 Tưởng tượng: Di truyền DNA**
> - **Superclass (Cha)**: Truyền lại bộ gen (fields/methods) cho con.
> - **Subclass (Con)**: Nhận bộ gen đó. Con có thể giữ nguyên, hoặc biến đổi (override) để phù hợp với môi trường sống.
> - Con cũng có thể phát triển thêm những đặc điểm mới (method mới) mà cha không có.

#### 🎓 Giảng sâu: Inheritance hoạt động thế nào trong JVM?

Khi viết `class Dog extends Animal`:

1. **Dog nhận** mọi `public`/`protected` field và method **không private** của Animal (trừ constructor).
2. **Dog có thêm** field/method riêng (`breed`, `bark()`).
3. Mỗi object `Dog` trên Heap **vừa chứa phần Animal vừa chứa phần Dog** (layout object nối tiếp).

**Câu hỏi thường gặp:**

| Câu hỏi | Trả lời ngắn |
|---------|--------------|
| Con có dùng được `private` của cha không? | **Không** trực tiếp; chỉ qua `protected`/`public` hoặc getter |
| Java cho kế thừa nhiều class không? | **Không** – chỉ `extends` **một** class; nhiều hành vi → `implements` interface |
| `extends` và `implements` cùng lúc? | **Có:** `class Dog extends Animal implements Runnable` |

#### 🎓 Giảng sâu: `super()` – thứ tự khởi tạo object

Khi `new Dog("Buddy", 3, "Golden")`:

```
Bước 1: JVM cấp phát vùng nhớ cho object Dog trên Heap
Bước 2: Gọi Dog(...) → dòng đầu super(name, age)
Bước 3: Animal(...) chạy → gán name, age
Bước 4: Quay lại Dog → gán breed
Bước 5: Object sẵn sàng sử dụng
```

Nếu **không** gọi `super(...)` mà cha **không có** constructor không tham số → **lỗi biên dịch**. Đây là lý do mọi constructor con phải “hoàn tất” phần cha trước.

---

### 3.1.2. Cú pháp extends

#### Cú pháp cơ bản

```java
class Superclass {
    // Fields và methods
}

class Subclass extends Superclass {
    // Fields và methods riêng
    // + Kế thừa từ Superclass
}
```

#### Ví dụ cụ thể

```java
// Superclass (Parent class)
public class Animal {
    protected String name;
    protected int age;
    
    public Animal(String name, int age) {
        this.name = name;
        this.age = age;
    }
    
    public void eat() {
        System.out.println(name + " is eating");
    }
    
    public void sleep() {
        System.out.println(name + " is sleeping");
    }
}

// Subclass (Child class)
public class Dog extends Animal {
    private String breed;
    
    // Constructor phải gọi super()
    public Dog(String name, int age, String breed) {
        super(name, age);  // Gọi constructor của Animal
        this.breed = breed;
    }
    
    // Method riêng của Dog
    public void bark() {
        System.out.println(name + " is barking: Woof! Woof!");
    }
    
    // Có thể sử dụng methods kế thừa
    public void displayInfo() {
        System.out.println("Dog: " + name + ", Age: " + age + ", Breed: " + breed);
    }
}

// Sử dụng
Dog dog = new Dog("Buddy", 3, "Golden Retriever");
dog.eat();      // Kế thừa từ Animal
dog.sleep();    // Kế thừa từ Animal
dog.bark();     // Method riêng của Dog
```

#### ▶️ Chạy thử & Giải thích từng bước

**Code đầy đủ (đã test):** `CODE/VI_DU/CHUONG_3/01_Inheritance/`

```bash
cd CODE/VI_DU/CHUONG_3/01_Inheritance
javac *.java && java Main
```

**Output thực tế khi chạy:**

```
=== 1. Tạo Dog (subclass) ===
Buddy đang ăn
Buddy đang ngủ
Buddy sủa: Gâu gâu! (giống: Golden)
Buddy sủa: Gâu gâu! (giống: Golden)

=== 2. Tạo Cat ===
Mimi đang ăn
Mimi kêu: Meo meo!

=== 3. IS-A: Dog là một Animal ===
Buddy đang ăn
Buddy sủa: Gâu gâu! (giống: Golden)
```

**Giải thích từng dòng quan trọng:**

| Dòng code | Ý nghĩa |
|-----------|---------|
| `super(name, age)` | Gọi constructor `Animal` trước – **bắt buộc** khởi tạo phần “cha” trên object |
| `this.breed = breed` | Sau khi cha xong, gán field riêng của Dog |
| `buddy.eat()` | Gọi method **kế thừa** – không cần viết lại trong Dog |
| `Animal a = buddy` | **Upcasting**: biến kiểu Animal trỏ object Dog (IS-A) |
| `a.makeSound()` | **Dynamic binding**: runtime gọi `Dog.makeSound()`, không phải Animal |

**❓ Tự kiểm tra:** Vì sao `a.bark()` sẽ **lỗi biên dịch** dù `a` thực chất đang trỏ Dog? → Vì kiểu biên dịch là `Animal`, Animal không có method `bark()`.

---

### 3.1.3. Từ khóa super

#### super là gì?

**`super`** là reference trỏ đến superclass (class cha).

#### Các cách sử dụng super

**1. Gọi constructor của superclass:**
```java
public class Dog extends Animal {
    private String breed;
    
    public Dog(String name, int age, String breed) {
        super(name, age);  // Phải là dòng đầu tiên
        this.breed = breed;
    }
}
```

**Lưu ý:**
- `super()` phải là dòng đầu tiên trong constructor
- Nếu không gọi `super()`, Java tự động gọi `super()` (constructor không tham số)
- Nếu superclass không có constructor không tham số → Phải gọi `super(...)` rõ ràng

---

**2. Truy cập fields/methods của superclass:**
```java
public class Animal {
    protected String name;
    
    public void display() {
        System.out.println("Animal: " + name);
    }
}

public class Dog extends Animal {
    public void display() {
        super.display();  // Gọi method của superclass
        System.out.println("Breed: " + breed);
    }
    
    public void showName() {
        System.out.println(super.name);  // Truy cập field của superclass
    }
}
```

---

**3. Phân biệt field/method của subclass và superclass:**
```java
public class Animal {
    protected String name = "Animal";
}

public class Dog extends Animal {
    private String name = "Dog";
    
    public void showNames() {
        System.out.println(name);        // "Dog" - của subclass
        System.out.println(super.name);  // "Animal" - của superclass
    }
}
```

---

#### So sánh `this` vs `super` (Chương 2 ↔ Chương 3)

| | `this` (Chương 2) | `super` (Chương 3) |
|--|-------------------|---------------------|
| **Trỏ tới** | Object hiện tại (class con) | Phần “cha” của object |
| **Constructor** | `this(...)` gọi constructor khác **cùng class** | `super(...)` gọi constructor **class cha** (dòng đầu) |
| **Field/method** | Field/method của class hiện tại | Field/method của class cha |
| **Khi nào dùng** | Phân biệt tham số vs field; chaining constructor | Gọi logic cha trước/sau khi override |

**Ví dụ ngắn:**

```java
public class Dog extends Animal {
    public Dog(String name) {
        super(name);           // gọi constructor Animal
        // this.breed = ...    // this = chính Dog đang tạo
    }
    @Override
    public void display() {
        super.display();       // in phần Animal trước
        System.out.println("Woof!");
    }
}
```

---

### 3.1.4. Khi nào nên dùng Inheritance?

#### Quan hệ "IS-A" thực sự

**✅ NÊN DÙNG:**
```java
// Dog IS-A Animal
class Dog extends Animal { }

// Car IS-A Vehicle
class Car extends Vehicle { }

// Student IS-A Person
class Student extends Person { }
```

**❌ KHÔNG NÊN DÙNG:**
```java
// Circle HAS-A Point (không phải IS-A)
// ❌ class Circle extends Point { }
// ✅ class Circle {
//       private Point center;  // Composition
//    }

// Employee HAS-A Address (không phải IS-A)
// ❌ class Employee extends Address { }
// ✅ class Employee {
//       private Address address;  // Composition
//    }
```

---

#### Ví dụ: Phân tích Đúng/Sai

**Ví dụ 1: Stack extends Vector?**
```java
// ❌ SAI: Stack không phải là Vector
// Stack có thể dùng Vector bên trong, nhưng không phải IS-A Vector
class Stack extends Vector { }  // Thiết kế sai trong Java

// ✅ ĐÚNG: Composition
class Stack {
    private Vector elements;  // HAS-A Vector
}
```

**Ví dụ 2: Square extends Rectangle?**
```java
// ⚠️ CẨN THẬN: Square IS-A Rectangle về mặt toán học
// Nhưng trong code có thể gây vấn đề:

class Rectangle {
    protected int width;
    protected int height;
    
    public void setWidth(int width) {
        this.width = width;
    }
    
    public void setHeight(int height) {
        this.height = height;
    }
}

class Square extends Rectangle {
    @Override
    public void setWidth(int width) {
        this.width = width;
        this.height = width;  // Phải giữ hình vuông
    }
    
    @Override
    public void setHeight(int height) {
        this.width = height;
        this.height = height;  // Phải giữ hình vuông
    }
}

// Vấn đề:
Rectangle rect = new Square();
rect.setWidth(5);
rect.setHeight(10);  // Square bây giờ là 10x10, không phải 5x5!
// Vi phạm Liskov Substitution Principle (sẽ học ở Chương 4)
```

**Kết luận:**
- Inheritance chỉ dùng khi quan hệ "IS-A" thực sự
- Cẩn thận với các trường hợp đặc biệt (như Square-Rectangle)

---

### 3.1.5. Multi-level Inheritance

#### Định nghĩa

**Multi-level Inheritance** là khi có nhiều cấp kế thừa:
```
Animal (superclass)
  └── Mammal (extends Animal)
        └── Dog (extends Mammal)
```

#### Ví dụ

```java
// Level 1: Animal
public class Animal {
    protected String name;
    
    public void eat() {
        System.out.println(name + " is eating");
    }
}

// Level 2: Mammal extends Animal
public class Mammal extends Animal {
    protected int numberOfLegs;
    
    public void breathe() {
        System.out.println(name + " is breathing");
    }
}

// Level 3: Dog extends Mammal
public class Dog extends Mammal {
    private String breed;
    
    public void bark() {
        System.out.println(name + " is barking");
    }
    
    // Kế thừa từ cả Animal và Mammal
    public void displayInfo() {
        System.out.println("Dog: " + name);
        System.out.println("Legs: " + numberOfLegs);
        eat();      // Từ Animal
        breathe();  // Từ Mammal
        bark();     // Riêng của Dog
    }
}
```

**Lưu ý:**
- Java không hỗ trợ **Multiple Inheritance** (một class extends nhiều class)
- Chỉ có thể extends 1 class
- Nhưng có thể implement nhiều interface (sẽ học sau)

---

## 3.2. OVERRIDING VS OVERLOADING

### 3.2.1. Overriding (Ghi đè)

#### Định nghĩa

**Overriding (Ghi đè)** là khi subclass định nghĩa lại method đã có trong superclass với:
- **Cùng tên**
- **Cùng tham số** (số lượng, kiểu, thứ tự)
- **Cùng return type** (hoặc subtype - sẽ học sau)

> **🎮 Tưởng tượng: Chiếc điều khiển đa năng (Polymorphism)**
> - Bạn có một chiếc điều khiển có nút **"Power"**.
> - Khi chĩa vào **TV**, nút Power làm TV bật lên.
> - Khi chĩa vào **Máy lạnh**, nút Power làm máy lạnh chạy.
> - **Nút bấm (Method name)** là giống nhau, nhưng **Hành động (Implementation)** là khác nhau tùy theo đối tượng nhận lệnh. Đây chính là bản chất của Đa hình (Polymorphism) và Overriding.

#### Ví dụ

```java
public class Animal {
    public void makeSound() {
        System.out.println("Animal makes a sound");
    }
}

public class Dog extends Animal {
    @Override  // Annotation để đánh dấu (khuyến nghị dùng)
    public void makeSound() {
        System.out.println("Dog barks: Woof! Woof!");
    }
}

public class Cat extends Animal {
    @Override
    public void makeSound() {
        System.out.println("Cat meows: Meow! Meow!");
    }
}

// Sử dụng
Animal animal = new Animal();
animal.makeSound();  // "Animal makes a sound"

Dog dog = new Dog();
dog.makeSound();  // "Dog barks: Woof! Woof!"

Cat cat = new Cat();
cat.makeSound();  // "Cat meows: Meow! Meow!"
```

### 🎬 Phân tích Hậu trường (Runtime Look-up)

Tại sao khi gọi `animal.makeSound()` (ở ví dụ trên là `Animal animal = new Animal()`), nó lại kêu chung chung, còn `dog.makeSound()` lại kêu "Woof"?

Cơ chế này gọi là **Dynamic Binding** (Ràng buộc động). Tại thời điểm chạy (Runtime), JVM sẽ làm như sau:

1.  **Bước 1**: Nhìn vào **Object thực tế** đang nằm trong Heap (không phải kiểu của biến reference).
2.  **Bước 2**: Tìm xem trong Object đó có method `makeSound()` được override không?
    - Nếu có (ví dụ Object là `Dog`): Chạy bản `Override`.
    - Nếu không (ví dụ Object là `Animal` thường): Chạy bản gốc của `Animal`.

**Ví dụ lừa tình:**
```java
Animal myPet = new Dog(); // Biến kiểu Animal, nhưng chứa Dog
myPet.makeSound(); 
```
- **Hỏi**: Chạy của ai?
- **Đáp**: Chạy của **Dog**! Vì JVM nhìn vào cái ruột (`new Dog()`), không nhìn cái mác (`Animal myPet`).


---

#### Quy tắc Overriding

**1. Access modifier không thể hẹp hơn:**
```java
public class Animal {
    protected void method() { }  // protected
}

public class Dog extends Animal {
    // ✅ OK: public > protected
    @Override
    public void method() { }
    
    // ❌ LỖI: private < protected
    // @Override
    // private void method() { }
}
```

**2. Return type phải giống hoặc là subtype:**
```java
public class Animal {
    public Animal getParent() {
        return new Animal();
    }
}

public class Dog extends Animal {
    // ✅ OK: Dog là subtype của Animal
    @Override
    public Dog getParent() {
        return new Dog();
    }
}
```

**3. Không thể override static methods:**
```java
public class Animal {
    public static void staticMethod() { }
}

public class Dog extends Animal {
    // ❌ Không phải override, chỉ là method mới (hiding)
    public static void staticMethod() { }
}
```

**4. Không thể override final methods:**
```java
public class Animal {
    public final void cannotOverride() { }
}

public class Dog extends Animal {
    // ❌ LỖI: Không thể override final method
    // @Override
    // public void cannotOverride() { }
}
```

---

#### Annotation @Override

**Tại sao dùng `@Override`?**
- ✅ Compiler kiểm tra: Đảm bảo đúng là override
- ✅ Tránh lỗi: Nếu superclass thay đổi tên method → Compiler báo lỗi
- ✅ Code rõ ràng: Người đọc biết đây là override

**Ví dụ lỗi khi không dùng @Override:**
```java
public class Animal {
    public void makeSound() { }
}

public class Dog extends Animal {
    // Lỗi đánh máy: makeSoud thay vì makeSound
    public void makeSoud() {  // Không phải override!
        // Compiler không báo lỗi nếu không có @Override
    }
}

// Với @Override:
public class Dog extends Animal {
    @Override
    public void makeSoud() {  // ❌ Compiler báo lỗi ngay!
        // Method 'makeSoud()' does not override method from 'Animal'
    }
}
```

---

#### Annotations trong Java (Tổng quan)

**Annotation** là “nhãn” gắn lên class, method, field để compiler hoặc framework hiểu thêm ý nghĩa.

| Annotation | Dùng ở đâu | Ý nghĩa |
|------------|------------|---------|
| `@Override` | Method | Đánh dấu ghi đè method cha – compiler kiểm tra |
| `@Deprecated` | Class, method | Đánh dấu “sắp bỏ”, cảnh báo khi dùng |
| `@FunctionalInterface` | Interface | Interface chỉ có 1 abstract method (Ch.6 – Lambda) |
| `@SuppressWarnings` | Method, class | Tắt cảnh báo compiler (dùng cẩn thận) |

**Ví dụ:**

```java
@FunctionalInterface
public interface Calculator {
    int calculate(int a, int b);  // chỉ 1 method abstract
}

public class LegacyApi {
    @Deprecated
    public void oldMethod() { }
    
    public void newMethod() { }
}
```

> **💡 Ghi nhớ:** Trong môn học, **`@Override` là bắt buộc thói quen** khi override; **`@FunctionalInterface`** sẽ gặp lại ở Chương 6 với Lambda.

---

### 3.2.2. Overloading (Nạp chồng)

#### Định nghĩa

**Overloading (Nạp chồng)** là khi có nhiều methods với:
- **Cùng tên**
- **Khác tham số** (số lượng, kiểu, thứ tự)
- Có thể khác return type (nhưng không đủ để phân biệt)

#### Ví dụ

```java
public class Calculator {
    // Overload 1: 2 tham số int
    public int add(int a, int b) {
        return a + b;
    }
    
    // Overload 2: 3 tham số int
    public int add(int a, int b, int c) {
        return a + b + c;
    }
    
    // Overload 3: 2 tham số double
    public double add(double a, double b) {
        return a + b;
    }
    
    // Overload 4: 2 tham số String
    public String add(String a, String b) {
        return a + b;
    }
}

// Sử dụng
Calculator calc = new Calculator();
calc.add(5, 3);           // Gọi overload 1
calc.add(5, 3, 2);        // Gọi overload 2
calc.add(5.5, 3.2);       // Gọi overload 3
calc.add("Hello", "World"); // Gọi overload 4
```

---

#### Quy tắc Overloading

**1. Phải khác về tham số:**
```java
// ✅ OK: Khác số lượng tham số
public void method(int a) { }
public void method(int a, int b) { }

// ✅ OK: Khác kiểu tham số
public void method(int a) { }
public void method(double a) { }

// ❌ LỖI: Chỉ khác return type (không đủ)
public int method(int a) { return a; }
public void method(int a) { }  // Compiler error!
```

**2. Có thể khác access modifier:**
```java
public void method(int a) { }
private void method(double a) { }  // ✅ OK
```

**3. Có thể khác static:**
```java
public void method(int a) { }
public static void method(double a) { }  // ✅ OK
```

---

### 3.2.3. So sánh Overriding vs Overloading

| Đặc điểm | Overriding | Overloading |
|----------|------------|-------------|
| **Quan hệ** | Giữa superclass và subclass | Trong cùng class (hoặc kế thừa) |
| **Tên method** | Phải giống | Phải giống |
| **Tham số** | Phải giống | Phải khác |
| **Return type** | Phải giống (hoặc subtype) | Có thể khác |
| **Access modifier** | Không thể hẹp hơn | Có thể khác |
| **Mục đích** | Thay đổi hành vi | Mở rộng chức năng |
| **Binding** | Runtime (Dynamic) | Compile-time (Static) |
| **Annotation** | @Override (khuyến nghị) | Không cần |

---

#### Ví dụ tổng hợp

```java
public class Animal {
    // Method gốc
    public void makeSound() {
        System.out.println("Animal makes a sound");
    }
    
    // Overload: Khác tham số
    public void makeSound(int volume) {
        System.out.println("Animal makes a sound at volume " + volume);
    }
}

public class Dog extends Animal {
    // Override: Ghi đè method không tham số
    @Override
    public void makeSound() {
        System.out.println("Dog barks: Woof!");
    }
    
    // Override: Ghi đè method có tham số
    @Override
    public void makeSound(int volume) {
        System.out.println("Dog barks at volume " + volume);
    }
    
    // Overload: Method mới (không override)
    public void makeSound(String message) {
        System.out.println("Dog says: " + message);
    }
}

// Sử dụng
Dog dog = new Dog();
dog.makeSound();           // "Dog barks: Woof!" (override)
dog.makeSound(10);         // "Dog barks at volume 10" (override)
dog.makeSound("Hello");    // "Dog says: Hello" (overload)
```

#### ▶️ Chạy thử Override vs Overload

**Code:** `CODE/VI_DU/CHUONG_3/02_OverrideOverload/Main.java`

```bash
cd CODE/VI_DU/CHUONG_3/02_OverrideOverload
javac Main.java && java Main
```

**Output:**

```
=== OVERLOADING ===
add(2,3) = 5
add(2.5,3.5) = 6.0
add(1,2,3) = 6

=== OVERRIDING ===
Xin chào từ Child (đã ghi đè)
```

**Giảng sâu – phân biệt compile-time vs runtime:**

| | Overloading | Overriding |
|--|-------------|------------|
| **Ai quyết định gọi method nào?** | Compiler (lúc biên dịch) | JVM (lúc chạy) |
| **Ví dụ** | `add(2,3)` vs `add(2.5,3.5)` – compiler chọn theo kiểu tham số | `Parent p = new Child(); p.greet()` – runtime gọi `Child.greet()` |
| **Cách nhớ** | “Cùng tên, **khác tham số**” | “Con **ghi đè** cha, **cùng chữ ký**” |

---

## 3.3. ABSTRACT CLASS & PHƯƠNG THỨC TRỪU TƯỢNG

### 3.3.1. Abstract Class là gì?

#### Định nghĩa

**Abstract Class** là class không thể tạo object trực tiếp, chỉ dùng để kế thừa.

- Có thể có fields và constructors

> **🏗️ Tưởng tượng: Ngôi nhà chưa hoàn thiện**
> - **Abstract Class**: Giống như bản vẽ một ngôi nhà đã xây xong móng và tường, nhưng **chưa lợp mái**.
> - Vì chưa lợp mái (chứa abstract method), bạn **không thể vào ở** (không thể tạo object `new`).
> - Bạn phải thuê thợ (Subclass) để lợp mái theo ý thích (implement abstract method) thì mới thành ngôi nhà hoàn chỉnh (Concrete Class) để ở được.

#### Tại sao cần Abstract Class?

**Vấn đề:**
```java
// Animal có method makeSound() nhưng không biết implement thế nào
public class Animal {
    public void makeSound() {
        // Làm gì đây? Mỗi loài động vật kêu khác nhau!
        System.out.println("???");
    }
}
```

**Giải pháp: Abstract Class**
```java
// Animal là abstract - không thể tạo object
public abstract class Animal {
    protected String name;
    
    // Abstract method - chưa implement, subclass phải implement
    public abstract void makeSound();
    
    // Concrete method - đã implement, subclass có thể dùng
    public void eat() {
        System.out.println(name + " is eating");
    }
}

// Subclass phải implement abstract method
public class Dog extends Animal {
    @Override
    public void makeSound() {
        System.out.println("Woof! Woof!");
    }
}

// Không thể tạo object từ abstract class
// Animal animal = new Animal();  // ❌ LỖI!

// Chỉ có thể tạo object từ concrete subclass
Dog dog = new Dog();  // ✅ OK
```

---

### 3.3.2. Cú pháp Abstract Class

#### Abstract Method

```java
[access modifier] abstract returnType methodName(parameters);
```

**Lưu ý:**
- Không có body `{ }`
- Phải kết thúc bằng `;`
- Chỉ có trong abstract class hoặc interface

#### Ví dụ đầy đủ

```java
public abstract class Shape {
    protected String color;
    
    // Constructor
    public Shape(String color) {
        this.color = color;
    }
    
    // Abstract method - subclass phải implement
    public abstract double calculateArea();
    public abstract double calculatePerimeter();
    
    // Concrete method - đã implement
    public String getColor() {
        return color;
    }
    
    public void displayInfo() {
        System.out.println("Shape color: " + color);
        System.out.println("Area: " + calculateArea());
        System.out.println("Perimeter: " + calculatePerimeter());
    }
}

// Subclass phải implement tất cả abstract methods
public class Circle extends Shape {
    private double radius;
    
    public Circle(String color, double radius) {
        super(color);
        this.radius = radius;
    }
    
    @Override
    public double calculateArea() {
        return Math.PI * radius * radius;
    }
    
    @Override
    public double calculatePerimeter() {
        return 2 * Math.PI * radius;
    }
}

public class Rectangle extends Shape {
    private double width;
    private double height;
    
    public Rectangle(String color, double width, double height) {
        super(color);
        this.width = width;
        this.height = height;
    }
    
    @Override
    public double calculateArea() {
        return width * height;
    }
    
}
```

### 📐 Phân tích Thiết kế (Structure Analysis)

Tại sao lại phải chia ra `Shape` (abstract) và `Circle/Rectangle` (concrete) rắc rối thế này?

Đây là áp dụng **Template Method Pattern** (Mẫu thiết kế Khuôn mẫu):

1.  **Người Kiến trúc sư (Class Shape)**: Quy định luật chơi chung. "Đã là Hình thì bắt buộc phải tính được Diện tích và Chu vi. Tôi không biết tính cụ thể thế nào, nhưng các anh (Subclass) bắt buộc phải có".
2.  **Người Thợ xây (Circle, Rectangle)**: Tuân thủ luật chơi. "Tôi là Tròn, công thức diện tích của tôi là PI*r^2. Tôi đã điền vào chỗ trống mà Kiến trúc sư để lại".

> **✅ Lợi ích:**
> Bạn có thể tạo ra một List chứa đủ loại hình: `List<Shape> shapes`. Sau đó duyệt qua list và gọi `s.calculateArea()`.
> - Hình Tròn tự tính theo kiểu Tròn.
> - Hình Vuông tự tính theo kiểu Vuông.
> - Code xử lý (vòng lặp) không cần quan tâm cụ thể là hình gì. Đây là sức mạnh tối thượng của **Polymorphism**!


---

### 3.3.3. So sánh Abstract Class vs Concrete Class

| Đặc điểm | Abstract Class | Concrete Class |
|----------|---------------|----------------|
| **Tạo object** | ❌ Không thể | ✅ Có thể |
| **Abstract methods** | ✅ Có thể có | ❌ Không thể |
| **Concrete methods** | ✅ Có thể có | ✅ Có thể có |
| **Fields** | ✅ Có thể có | ✅ Có thể có |
| **Constructor** | ✅ Có thể có | ✅ Có thể có |
| **Kế thừa** | Phải extends | Có thể extends |
| **Từ khóa** | `abstract` | Không có |

#### ▶️ Chạy thử Abstract Class

**Code:** `CODE/VI_DU/CHUONG_3/03_Abstract/Main.java`

```bash
javac Main.java && java Main
```

**Output:**

```
Màu: đỏ
Diện tích hình tròn: 78.53981633974483
Màu: xanh
Diện tích hình chữ nhật: 24.0
```

**Giải thích:** Dòng `Shape s = new Shape(...)` sẽ **lỗi biên dịch** vì abstract class không cho `new`. Chỉ subclass cụ thể (`Circle`, `Rectangle`) mới tạo object được; biến có thể kiểu `Shape` (đa hình).

---

### 3.3.4. Khi nào dùng Abstract Class?

**✅ NÊN DÙNG khi:**
- Có code chung cho nhiều subclass
- Cần định nghĩa template method pattern
- Cần fields và constructors

**Ví dụ:**
```java
// Template method pattern
public abstract class DataProcessor {
    // Template method - định nghĩa flow
    public final void process() {
        readData();
        processData();  // Abstract - mỗi subclass implement khác
        saveData();
    }
    
    protected abstract void processData();
    
    protected void readData() {
        System.out.println("Reading data...");
    }
    
    protected void saveData() {
        System.out.println("Saving data...");
    }
}
```

---

## 3.4. INTERFACE – HỢP ĐỒNG HÀNH VI

### 3.4.1. Interface là gì?

#### Định nghĩa

**Interface** là một "hợp đồng" định nghĩa các methods mà class phải implement.

**Đặc điểm:**
- Chỉ định nghĩa method signatures (không có body)
- Không có fields (trước Java 8)
- Không có constructors
- Class implement interface phải implement tất cả methods

#### Ví dụ

```java
// Interface định nghĩa hành vi
public interface Flyable {
    void fly();  // Chỉ định nghĩa, không implement
}

public interface Swimmable {
    void swim();
}

// Class implement interface
public class Bird implements Flyable {
    @Override
    public void fly() {
        System.out.println("Bird is flying");
    }
}

public class Duck implements Flyable, Swimmable {
    @Override
    public void fly() {
        System.out.println("Duck is flying");
    }
    
    @Override
    public void swim() {
        System.out.println("Duck is swimming");
    }
}
```

---

### 3.4.2. Interface vs Abstract Class

| Đặc điểm | Interface | Abstract Class |
|----------|-----------|---------------|
| **Từ khóa** | `interface` | `abstract class` |
| **Fields** | Chỉ constants (public static final) | Có thể có fields |
| **Methods** | Chỉ abstract (trước Java 8) | Abstract + Concrete |
| **Constructor** | ❌ Không có | ✅ Có thể có |
| **Multiple inheritance** | ✅ Implement nhiều interface | ❌ Chỉ extends 1 class |
| **Mục đích** | Định nghĩa hành vi (behavior) | Định nghĩa cấu trúc (structure) |
| **Quan hệ** | "CAN-DO" (có thể làm) | "IS-A" (là một) |

---

#### Khi nào dùng Interface?

**✅ DÙNG INTERFACE khi:**
- Định nghĩa hành vi (behavior)
- Nhiều class không liên quan cần cùng hành vi
- Cần multiple inheritance

**Ví dụ:**
```java
// Nhiều class không liên quan nhưng đều có thể bay
public class Bird implements Flyable { }
public class Airplane implements Flyable { }
public class Superman implements Flyable { }
```

---

#### Khi nào dùng Abstract Class?

**✅ DÙNG ABSTRACT CLASS khi:**
- Có code chung cho subclass
- Quan hệ "IS-A" rõ ràng
- Cần fields và constructors

**Ví dụ:**
```java
// Tất cả đều là Animal
public abstract class Animal { }
public class Dog extends Animal { }
public class Cat extends Animal { }
```

---

### 3.4.3. Modern Java: Default Methods và Static Methods

#### Default Methods (Java 8+)

**Default methods** cho phép interface có implementation:

```java
public interface Flyable {
    // Abstract method (bắt buộc implement)
    void fly();
    
    // Default method (có implementation, không bắt buộc override)
    default void takeOff() {
        System.out.println("Taking off...");
    }
    
    default void land() {
        System.out.println("Landing...");
    }
}

// Class chỉ cần implement fly()
public class Bird implements Flyable {
    @Override
    public void fly() {
        System.out.println("Bird is flying");
    }
    
    // Có thể dùng default methods
    // Hoặc override nếu muốn
}

// Sử dụng
Bird bird = new Bird();
bird.fly();      // "Bird is flying"
bird.takeOff();  // "Taking off..." (default method)
bird.land();     // "Landing..." (default method)
```

**Lợi ích:**
- ✅ Thêm methods mới vào interface mà không phá vỡ code cũ
- ✅ Code chung cho tất cả implementers

---

#### Static Methods (Java 8+)

**Static methods** trong interface:

```java
public interface MathOperation {
    int calculate(int a, int b);
    
    // Static method - gọi trực tiếp từ interface
    static MathOperation add() {
        return (a, b) -> a + b;
    }
    
    static MathOperation subtract() {
        return (a, b) -> a - b;
    }
}

// Sử dụng
MathOperation addOp = MathOperation.add();
int result = addOp.calculate(5, 3);  // 8
```

---

### 3.4.4. Multiple Interfaces

**Java cho phép implement nhiều interface:**

```java
public interface Flyable {
    void fly();
}

public interface Swimmable {
    void swim();
}

public interface Walkable {
    void walk();
}

// Implement nhiều interface
public class Duck implements Flyable, Swimmable, Walkable {
    @Override
    public void fly() {
        System.out.println("Duck is flying");
    }
    
    @Override
    public void swim() {
        System.out.println("Duck is swimming");
    }
    
    @Override
    public void walk() {
        System.out.println("Duck is walking");
    }
}
```

#### ▶️ Chạy thử Interface (Multiple Interfaces)

**Code:** `CODE/VI_DU/CHUONG_3/04_Interface/Main.java`

```bash
javac Main.java && java Main
```

**Output:**

```
Donald bay lượn
Donald bơi trên hồ
Donald bay lượn
```

**Giảng sâu:** `Flyable f = duck` – biến kiểu interface, object kiểu class. Chỉ gọi được method trong `Flyable` (`fly`), không gọi `swim()` qua `f` trừ khi cast hoặc dùng biến `Swimmable`.

---

### 3.5.1. Polymorphism là gì?

#### Định nghĩa

**Polymorphism (Đa hình)** là khả năng một object có thể có nhiều hình thái khác nhau.

**Trong Java:**
- Cùng một reference có thể trỏ đến nhiều loại object khác nhau
- Cùng một method call có thể thực thi code khác nhau tùy object

#### Ví dụ

```java
Animal animal1 = new Dog();   // Animal reference trỏ đến Dog object
Animal animal2 = new Cat();   // Animal reference trỏ đến Cat object

animal1.makeSound();  // "Woof!" - Gọi method của Dog
animal2.makeSound();  // "Meow!" - Gọi method của Cat
```

**Phân tích:**
- Cùng kiểu reference: `Animal`
- Cùng method call: `makeSound()`
- Nhưng kết quả khác nhau tùy object thực tế

---

### 3.5.2. Upcasting và Downcasting

#### Upcasting (Ép kiểu lên)

**Upcasting**: Ép kiểu từ subclass lên superclass (tự động)

```java
Dog dog = new Dog();
Animal animal = dog;  // Upcasting - tự động
// Hoặc
Animal animal = new Dog();  // Upcasting - tự động
```

**Đặc điểm:**
- ✅ Tự động, không cần cast
- ✅ An toàn (Dog luôn là Animal)
- ✅ Mất quyền truy cập methods riêng của Dog

```java
Animal animal = new Dog();
animal.makeSound();  // ✅ OK - makeSound() có trong Animal
// animal.bark();    // ❌ LỖI - bark() chỉ có trong Dog
```

---

#### Downcasting (Ép kiểu xuống)

**Downcasting**: Ép kiểu từ superclass xuống subclass (phải cast)

```java
Animal animal = new Dog();
Dog dog = (Dog) animal;  // Downcasting - phải cast
dog.bark();  // ✅ OK - Bây giờ có thể gọi bark()
```

**⚠️ Nguy hiểm:**
```java
Animal animal = new Cat();  // Thực tế là Cat
Dog dog = (Dog) animal;     // ❌ ClassCastException!
```

**Giải pháp: Kiểm tra với instanceof**
```java
Animal animal = new Cat();

if (animal instanceof Dog) {
    Dog dog = (Dog) animal;
    dog.bark();
} else {
    System.out.println("Not a Dog!");
}
```

---

### 3.5.3. Dynamic Binding (Runtime Polymorphism)

#### Định nghĩa

**Dynamic Binding** là cơ chế Java quyết định method nào được gọi **tại runtime** dựa trên **kiểu thực tế của object**, không phải kiểu reference.

#### Ví dụ

```java
public class Animal {
    public void makeSound() {
        System.out.println("Animal makes a sound");
    }
}

public class Dog extends Animal {
    @Override
    public void makeSound() {
        System.out.println("Dog barks: Woof!");
    }
}

public class Cat extends Animal {
    @Override
    public void makeSound() {
        System.out.println("Cat meows: Meow!");
    }
}

// Test
Animal animal1 = new Dog();   // Reference: Animal, Object: Dog
Animal animal2 = new Cat();   // Reference: Animal, Object: Cat

animal1.makeSound();  // "Dog barks: Woof!" - Gọi method của Dog
animal2.makeSound();  // "Cat meows: Meow!" - Gọi method của Cat
```

**Phân tích:**
- Compile-time: Compiler chỉ biết `animal1` là `Animal`
- Runtime: JVM biết `animal1` thực tế là `Dog` → Gọi `Dog.makeSound()`

---

### 3.5.4. Ứng dụng: Loại bỏ if/else bằng Polymorphism

#### Vấn đề: Code có nhiều if/else

```java
// ❌ TỒI: Nhiều if/else
public void processAnimal(Animal animal) {
    if (animal instanceof Dog) {
        Dog dog = (Dog) animal;
        dog.bark();
        dog.fetch();
    } else if (animal instanceof Cat) {
        Cat cat = (Cat) animal;
        cat.meow();
        cat.scratch();
    } else if (animal instanceof Bird) {
        Bird bird = (Bird) animal;
        bird.tweet();
        bird.fly();
    }
    // Thêm loại mới → Phải sửa code này
}
```

**Vấn đề:**
- ❌ Vi phạm Open/Closed Principle (sẽ học Chương 4)
- ❌ Khó mở rộng (thêm loại mới phải sửa code cũ)
- ❌ Dễ lỗi (quên case nào đó)

---

#### Giải pháp: Polymorphism

```java
// ✅ TỐT: Dùng polymorphism
public abstract class Animal {
    protected String name;
    
    public Animal(String name) {
        this.name = name;
    }
    
    // Abstract method - mỗi subclass implement khác
    public abstract void makeSound();
    public abstract void performAction();
}

public class Dog extends Animal {
    public Dog(String name) {
        super(name);
    }
    
    @Override
    public void makeSound() {
        System.out.println(name + " barks: Woof!");
    }
    
    @Override
    public void performAction() {
        System.out.println(name + " is fetching");
    }
}

public class Cat extends Animal {
    public Cat(String name) {
        super(name);
    }
    
    @Override
    public void makeSound() {
        System.out.println(name + " meows: Meow!");
    }
    
    @Override
    public void performAction() {
        System.out.println(name + " is scratching");
    }
}

// Code xử lý đơn giản, không cần if/else
public void processAnimal(Animal animal) {
    animal.makeSound();      // Polymorphism tự động chọn đúng method
    animal.performAction();  // Polymorphism tự động chọn đúng method
}

// Sử dụng
Animal dog = new Dog("Buddy");
Animal cat = new Cat("Fluffy");

processAnimal(dog);  // Tự động gọi Dog methods
processAnimal(cat);  // Tự động gọi Cat methods

// Thêm loại mới - KHÔNG CẦN SỬA CODE CŨ!
public class Bird extends Animal {
    // ...
}
```

**Lợi ích:**
- ✅ Không cần if/else
- ✅ Dễ mở rộng (thêm class mới, không sửa code cũ)
- ✅ Tuân thủ Open/Closed Principle

#### ▶️ Chạy thử Polymorphism (Payment – không if/else)

**Code:** `CODE/VI_DU/CHUONG_3/05_Polymorphism/Main.java`

```bash
javac Main.java && java Main
```

**Output:**

```
Thanh toán 100.0 bằng thẻ tín dụng
Thanh toán 250.5 qua PayPal
Thanh toán 50.0 bằng tiền mặt
```

**Trace tư duy:** `process(card, 100)` → compiler chỉ biết `PaymentMethod.pay()` → runtime object là `CreditCard` → chạy `CreditCard.pay`. Thêm `Bitcoin` = class mới, **không sửa** `PaymentProcessor`.

---

#### Ví dụ thực tế: Payment System

```java
// ❌ TỒI: Nhiều if/else
public void processPayment(String paymentType, double amount) {
    if (paymentType.equals("credit")) {
        // Process credit card
    } else if (paymentType.equals("paypal")) {
        // Process PayPal
    } else if (paymentType.equals("bitcoin")) {
        // Process Bitcoin
    }
}

// ✅ TỐT: Polymorphism
public interface PaymentMethod {
    void pay(double amount);
}

public class CreditCard implements PaymentMethod {
    @Override
    public void pay(double amount) {
        System.out.println("Paying " + amount + " with credit card");
    }
}

public class PayPal implements PaymentMethod {
    @Override
    public void pay(double amount) {
        System.out.println("Paying " + amount + " with PayPal");
    }
}

// Code xử lý đơn giản
public void processPayment(PaymentMethod method, double amount) {
    method.pay(amount);  // Polymorphism!
}
```

---

## 3.6. COMPOSITION OVER INHERITANCE

### 3.6.1. Mặt trái của Inheritance

#### Fragile Base Class Problem

**Vấn đề:** Thay đổi superclass có thể phá vỡ subclass:

```java
public class Stack {
    private List<String> elements = new ArrayList<>();
    
    public void push(String item) {
        elements.add(item);
    }
    
    public String pop() {
        return elements.remove(elements.size() - 1);
    }
    
    public int size() {
        return elements.size();
    }
}

public class CountingStack extends Stack {
    private int count = 0;
    
    @Override
    public void push(String item) {
        super.push(item);
        count++;
    }
    
    @Override
    public int size() {
        return count;  // Override để trả về count
    }
}

// Vấn đề: Nếu Stack thay đổi implementation của size()
// → CountingStack có thể bị ảnh hưởng
```

---

#### Vấn đề với Inheritance không phù hợp

**Ví dụ: Circle extends Point?**
```java
// ❌ SAI: Circle không phải là Point
class Circle extends Point {
    private double radius;
    // ...
}

// Vấn đề:
// - Circle có thể dùng methods của Point không phù hợp
// - Quan hệ không đúng (Circle HAS-A Point, không phải IS-A Point)
```

---

### 3.6.2. Composition (Kết hợp)

#### Định nghĩa

**Composition** là quan hệ "HAS-A" (có một):
- Class chứa object của class khác
- Object được tạo và quản lý bởi class chứa nó

#### Ví dụ

```java
// ✅ ĐÚNG: Circle HAS-A Point
public class Point {
    private int x;
    private int y;
    
    public Point(int x, int y) {
        this.x = x;
        this.y = y;
    }
    
    // Methods của Point
    public double distanceToOrigin() {
        return Math.sqrt(x * x + y * y);
    }
}

public class Circle {
    private Point center;  // HAS-A Point
    private double radius;
    
    public Circle(Point center, double radius) {
        this.center = center;
        this.radius = radius;
    }
    
    public double getArea() {
        return Math.PI * radius * radius;
    }
    
    // Có thể expose methods của Point nếu cần
    public double getCenterDistanceToOrigin() {
        return center.distanceToOrigin();
    }
}
```

---

### 3.6.3. Khi nào dùng Composition?

**✅ DÙNG COMPOSITION khi:**
- Quan hệ "HAS-A" (có một)
- Cần linh hoạt hơn inheritance
- Muốn tránh fragile base class problem

**Ví dụ:**
```java
// Employee HAS-A Address
public class Employee {
    private Address address;  // Composition
    private String name;
    
    public Employee(String name, Address address) {
        this.name = name;
        this.address = address;
    }
}

// Car HAS-A Engine
public class Car {
    private Engine engine;  // Composition
    
    public Car(Engine engine) {
        this.engine = engine;
    }
}
```

#### ▶️ Chạy thử Composition

**Code:** `CODE/VI_DU/CHUONG_3/06_Composition/Main.java`

```bash
javac Main.java && java Main
```

**Output:**

```
Xe chuẩn bị chạy
Động cơ nổ...
Xe đã sẵn sàng
```

**So sánh trực quan:**

| Inheritance (IS-A) | Composition (HAS-A) |
|--------------------|---------------------|
| `class Car extends Engine` ❌ sai ngữ nghĩa | `class Car { Engine engine; }` ✅ |
| Gắn chặt vòng đời | Có thể đổi Engine |
| Dùng khi quan hệ “là một” thật sự | Dùng khi “có một”, linh hoạt |

---

#### Kết hợp Interface và Composition

**Ví dụ: Payment System linh hoạt:**

```java
// Interface định nghĩa hành vi
public interface PaymentProcessor {
    void processPayment(double amount);
}

// Implementations
public class CreditCardProcessor implements PaymentProcessor {
    @Override
    public void processPayment(double amount) {
        System.out.println("Processing credit card: " + amount);
    }
}

public class PayPalProcessor implements PaymentProcessor {
    @Override
    public void processPayment(double amount) {
        System.out.println("Processing PayPal: " + amount);
    }
}

// Class sử dụng Composition + Interface
public class PaymentService {
    private PaymentProcessor processor;  // Composition
    
    // Dependency Injection - có thể thay đổi processor
    public PaymentService(PaymentProcessor processor) {
        this.processor = processor;
    }
    
    public void pay(double amount) {
        processor.processPayment(amount);
    }
    
    // Có thể thay đổi processor
    public void setProcessor(PaymentProcessor processor) {
        this.processor = processor;
    }
}

// Sử dụng
PaymentProcessor creditCard = new CreditCardProcessor();
PaymentService service = new PaymentService(creditCard);
service.pay(100.0);

// Dễ dàng thay đổi
PaymentProcessor paypal = new PayPalProcessor();
service.setProcessor(paypal);
service.pay(200.0);
```

**Lợi ích:**
- ✅ Linh hoạt (có thể thay đổi implementation)
- ✅ Dễ test (có thể mock interface)
- ✅ Tuân thủ Dependency Inversion Principle

---

### 3.6.5. So sánh: Inheritance vs Composition

| Đặc điểm | Inheritance | Composition |
|----------|-------------|-------------|
| **Quan hệ** | IS-A | HAS-A |
| **Coupling** | Tight (chặt chẽ) | Loose (lỏng lẻo) |
| **Linh hoạt** | Kém (khó thay đổi) | Tốt (dễ thay đổi) |
| **Reusability** | Tốt (kế thừa code) | Tốt (tái sử dụng object) |
| **Khi nào dùng** | Quan hệ IS-A rõ ràng | Quan hệ HAS-A, cần linh hoạt |

**Nguyên tắc:**
> **"Favor Composition over Inheritance"** - Ưu tiên Composition hơn Inheritance

---

## 3.7. KẾ THỪA NÂNG CAO (ADVANCED INHERITANCE)

### 3.7.1. Inner Classes & Anonymous Classes
Trong Java, ta có thể khai báo class bên trong một class khác. Đặc biệt quan trọng là **Anonymous Class** (Lớp vô danh) – tiền đề để học Lambda Expressions ở Chương 6.

#### Anonymous Class (Lớp vô danh)
Thay vì tạo một file `.java` mới chỉ để `implements` một interface dùng 1 lần, ta có thể tạo class ngay tại chỗ:

```java
public interface Greeting {
    void sayHello();
}

public class Main {
    public static void main(String[] args) {
        // Tạo trực tiếp object từ interface bằng Anonymous Class
        Greeting vietnamese = new Greeting() {
            @Override
            public void sayHello() {
                System.out.println("Xin chào!");
            }
        };
        
        vietnamese.sayHello();
    }
}
```
*Lưu ý: Anonymous Class rất hay dùng trong xử lý sự kiện (Event Listener) của GUI (Swing/Android).*

### 3.7.2. Sealed Classes (Kiểm soát Kế thừa - Java 15+)
Trước đây, class hoặc là `final` (không ai được kế thừa), hoặc là `public` (bất cứ ai cũng có thể kế thừa). 
**Sealed Classes** cho phép bạn chỉ định **chính xác class nào** được phép kế thừa nó.

```java
// Chỉ có Circle và Square mới được phép extends Shape
public sealed class Shape permits Circle, Square {
    // ...
}

// Lớp con bắt buộc phải khai báo là final, sealed, hoặc non-sealed
public final class Circle extends Shape {
    private double radius;
}

public non-sealed class Square extends Shape {
    private double side;
}
```
**Ý nghĩa OOP**: Giúp thiết kế hệ thống chặt chẽ hơn, ngăn chặn việc class lạ từ bên ngoài extends class cốt lõi của framework/thư viện của bạn.

---

## 🔗 3.8 CẦU NỐI SANG CHƯƠNG 4

Chương 3 cho bạn **công cụ OOP** (kế thừa, interface, đa hình). Chương 4 dạy **cách thiết kế tốt**:

| Chương 3 | Chương 4 | Liên hệ |
|----------|----------|---------|
| Interface + Polymorphism | OCP, DIP | Mở rộng không sửa code cũ |
| Abstract class `Shape` | UML, Factory | Vẽ và tạo object qua factory |
| Composition | SRP | Tách class theo trách nhiệm |
| `@Override` | LSP | Override đúng “hợp đồng” cha |

**Câu hỏi chuyển tiếp:** “Thêm loại thanh toán mới có phải sửa class xử lý thanh toán không?” → Nếu có, cần OCP + Factory (Ch.4).

---

## 📝 TÓM TẮT CHƯƠNG 3

### Kiến thức đã học:
1. ✅ Inheritance và quan hệ IS-A
2. ✅ Từ khóa `extends` và `super`
3. ✅ Overriding vs Overloading
4. ✅ Abstract Class và Abstract Methods
5. ✅ Interface và Multiple Interfaces
6. ✅ Default Methods và Static Methods (Java 8+)
7. ✅ Polymorphism và Dynamic Binding
8. ✅ Upcasting và Downcasting
9. ✅ Composition over Inheritance
10. ✅ Loại bỏ if/else bằng Polymorphism

### Kỹ năng đã có:
- ✅ Thiết kế hệ thống với Inheritance đúng cách
- ✅ Sử dụng Abstract Class và Interface
- ✅ Áp dụng Polymorphism trong code
- ✅ Chọn giữa Inheritance và Composition

---

## 🎯 BÀI TẬP CHƯƠNG 3

### Bài 1: Inheritance cơ bản

**Yêu cầu:**
1. Tạo class `Vehicle` (superclass):
   - Fields: brand, year
   - Methods: start(), stop(), displayInfo()

2. Tạo class `Car` extends `Vehicle`:
   - Thêm field: numberOfDoors
   - Override `displayInfo()` để hiển thị thêm numberOfDoors

3. Tạo class `Motorcycle` extends `Vehicle`:
   - Thêm field: hasSidecar
   - Override `displayInfo()`

**Test:**
```java
Car car = new Car("Toyota", 2023, 4);
car.start();
car.displayInfo();

Motorcycle bike = new Motorcycle("Honda", 2022, false);
bike.start();
bike.displayInfo();
```

---

### Bài 2: Overriding và Overloading

**Yêu cầu:**
Tạo class `Calculator` với:
1. Method `add(int a, int b)` - Overload với các kiểu: int, double, String
2. Method `multiply(int a, int b)` - Override trong subclass `ScientificCalculator`

**Test:**
```java
Calculator calc = new Calculator();
System.out.println(calc.add(5, 3));        // 8
System.out.println(calc.add(5.5, 3.2));     // 8.7
System.out.println(calc.add("Hello", "World")); // "HelloWorld"

ScientificCalculator sciCalc = new ScientificCalculator();
System.out.println(sciCalc.multiply(5, 3));  // Override logic
```

---

### Bài 3: Abstract Class

**Yêu cầu:**
1. Tạo abstract class `Shape`:
   - Abstract methods: `calculateArea()`, `calculatePerimeter()`
   - Concrete method: `displayInfo()`

2. Tạo các subclass: `Circle`, `Rectangle`, `Triangle`
   - Implement abstract methods

**Test:**
```java
Shape circle = new Circle(5.0);
circle.displayInfo();

Shape rectangle = new Rectangle(4.0, 6.0);
rectangle.displayInfo();
```

---

### Bài 4: Interface

**Yêu cầu:**
1. Tạo interface `Drawable` với method `draw()`
2. Tạo interface `Resizable` với method `resize(double factor)`
3. Tạo class `Circle` implement cả 2 interface

**Test:**
```java
Circle circle = new Circle(5.0);
circle.draw();
circle.resize(1.5);
```

---

### Bài 5: Polymorphism

**Yêu cầu:**
Tạo hệ thống động vật với:
1. Abstract class `Animal` với abstract method `makeSound()`
2. Subclasses: `Dog`, `Cat`, `Bird`
3. Method `performShow(Animal animal)` sử dụng polymorphism

**Test:**
```java
Animal[] animals = {
    new Dog("Buddy"),
    new Cat("Fluffy"),
    new Bird("Tweety")
};

for (Animal animal : animals) {
    animal.makeSound();  // Polymorphism!
}
```

---

### Bài 6: Loại bỏ if/else

**Yêu cầu:**
Refactor code sau để loại bỏ if/else bằng polymorphism:

```java
// Code cũ (có if/else)
public void processPayment(String type, double amount) {
    if (type.equals("credit")) {
        System.out.println("Processing credit card: " + amount);
    } else if (type.equals("paypal")) {
        System.out.println("Processing PayPal: " + amount);
    } else if (type.equals("bitcoin")) {
        System.out.println("Processing Bitcoin: " + amount);
    }
}
```

**Yêu cầu:**
1. Tạo interface `PaymentMethod`
2. Tạo các class implement: `CreditCard`, `PayPal`, `Bitcoin`
3. Refactor method `processPayment()` để dùng polymorphism

---

### Bài 7: Composition

**Yêu cầu:**
1. Tạo class `Engine` với method `start()`
2. Tạo class `Car` sử dụng Composition (HAS-A Engine)
3. So sánh với cách dùng Inheritance (Car extends Engine - SAI)

**Test:**
```java
Engine engine = new Engine();
Car car = new Car(engine);
car.start();  // Gọi engine.start()
```

---

### Bài 8: Tổng hợp - Hệ thống Nhân viên

**Yêu cầu:**
Tạo hệ thống quản lý nhân viên với:

1. **Abstract class `Employee`**:
   - Fields: id, name, salary
   - Abstract method: `calculateBonus()`
   - Concrete method: `displayInfo()`

2. **Subclasses**:
   - `Manager`: Bonus = 20% salary
   - `Developer`: Bonus = 10% salary
   - `Designer`: Bonus = 15% salary

3. **Interface `Workable`**:
   - Method: `work()`

4. **Class `Department`**:
   - Field: `List<Employee> employees`
   - Method: `calculateTotalBonus()` - Dùng polymorphism

**Yêu cầu bổ sung:**
- Tuân thủ Clean Code
- JavaDoc đầy đủ
- Sử dụng Composition nếu cần

---

## 📚 TÀI LIỆU THAM KHẢO

1. **Java Documentation**: https://docs.oracle.com/javase/tutorial/java/IandI/
2. **Effective Java** - Joshua Bloch (Item 18: Favor composition over inheritance)
3. **Head First Design Patterns** - Strategy Pattern

---

**Chúc bạn học tốt! 🚀**

