# CHƯƠNG 4: NGUYÊN LÝ THIẾT KẾ & DESIGN PATTERNS CƠ BẢN

## 📚 MỤC TIÊU HỌC TẬP

Sau khi hoàn thành chương này, bạn sẽ:
- Hiểu và áp dụng các nguyên lý **SOLID** (S, O, L, I, D)
- Đọc và vẽ **UML Class Diagram** cơ bản
- Nắm vững **Dependency Inversion** và **Dependency Injection**
- Hiểu và implement **Singleton Pattern**
- Hiểu và implement **Factory Pattern**
- Áp dụng Design Patterns vào thực tế

---

## 📋 KIẾN THỨC CẦN CÓ (PREREQUISITES)

Trước khi học chương này, bạn cần nắm vững:

- [ ] ✅ **Class, Object, Encapsulation** (Chương 2)
- [ ] ✅ **Inheritance, Interface, Polymorphism** (Chương 3)
- [ ] ✅ **Override, abstract class** (Chương 3) – dùng trong OCP và Factory

> **💡 Lưu ý:** Chương này chuyển từ “viết code chạy được” sang “thiết kế code dễ bảo trì”. Nếu Chương 3 chưa vững, nên ôn lại trước.

---

## 0. TẠI SAO CẦN NGUYÊN LÝ THIẾT KẾ?

### Vấn đề thường gặp

- Code **chạy được** nhưng sửa một chỗ làm vỡ chỗ khác
- Thêm tính năng mới phải sửa nhiều file, nhiều `if/else`
- Khó test vì class “ôm” quá nhiều việc
- Người khác đọc code không hiểu ý đồ thiết kế

### Nguyên lý thiết kế giúp gì?

| Khía cạnh | Không có nguyên tắc | Có SOLID + Pattern |
|-----------|---------------------|---------------------|
| Sửa lỗi | Dễ ảnh hưởng lan | Sửa đúng class, ít side effect |
| Mở rộng | Thêm `if` vào class cũ | Thêm class mới (OCP) |
| Test | Khó tách phần | Inject dependency, mock dễ |
| Làm nhóm | Không có “ngôn ngữ chung” | UML + SOLID là chuẩn chung |

**Ví dụ dễ nhớ:** Giống nhà **không tách phòng** (bếp, ngủ, khách một chỗ) vs **tách phòng rõ ràng** – sửa bếp không ảnh hưởng phòng ngủ.

**Thứ tự học trong chương:** UML (mô tả) → SOLID (nguyên tắc) → DIP/DI (phụ thuộc) → Singleton & Factory (mẫu hay dùng).

---

## 🎓 HƯỚNG DẪN HỌC SÂU & CODE CHẠY THỬ

| Chủ đề | Code | Lệnh |
|--------|------|------|
| DIP + Factory + DI | `CODE/VI_DU/CHUONG_4/01_SOLID_DIP_Factory/` | `javac Main.java && java Main` |

**Output đã kiểm tra:**

```
[MySQL] Lưu: Order#001:Sách Java
[PostgreSQL] Lưu: Order#002:Bút
```

**Trace từng bước:**

1. `DatabaseFactory.create("mysql")` → trả object `MySQLDatabase` (Factory).
2. `new OrderService(db)` → inject qua constructor (DI), `OrderService` chỉ biết interface `Database` (DIP).
3. `placeOrder` gọi `database.save(...)` → runtime chạy code MySQL hoặc PostgreSQL tùy object được inject.

**Thử tự tay:** Đổi `"mysql"` thành `"postgres"` trong Main – chỉ output đổi prefix, **không sửa** `OrderService`.

---

## 4.1. UML CLASS DIAGRAM (NHẬP MÔN)

### 4.1.1. UML là gì?

#### Định nghĩa

**UML (Unified Modeling Language)** là ngôn ngữ mô hình hóa chuẩn để:
- Mô tả thiết kế hệ thống
- Giao tiếp giữa các developer
- Tài liệu hóa kiến trúc

**UML Class Diagram** mô tả:
- Classes và quan hệ giữa chúng
- Attributes (fields) và Methods
- Access modifiers

---

### 4.1.2. Các thành phần cơ bản

#### Class trong UML

```
┌─────────────────────┐
│   ClassName         │
├─────────────────────┤
│ - attribute1: type  │  (- = private)
│ + attribute2: type  │  (+ = public)
│ # attribute3: type  │  (# = protected)
├─────────────────────┤
│ + method1(): type   │
│ - method2(): void   │
│ # method3(): type   │
└─────────────────────┘
```

**Ví dụ:**
```
┌─────────────────────┐
│   Student           │
├─────────────────────┤
│ - name: String      │
│ - age: int          │
│ - gpa: double       │
├─────────────────────┤
│ + getName(): String │
│ + setAge(int): void │
│ + displayInfo(): void│
└─────────────────────┘
```

---

### 4.1.3. Các loại quan hệ

#### 1. Association (Liên kết)

**Quan hệ "có liên quan đến"**

```
┌─────────┐         ┌─────────┐
│ Student │─────────│ Course  │
└─────────┘         └─────────┘
```

**Code:**
```java
public class Student {
    private Course course;  // Association
}
```

---

#### 2. Aggregation (Tập hợp)

**Quan hệ "HAS-A" - Object có thể tồn tại độc lập**

```
┌─────────┐         ┌─────────┐
│  Team   │◇───────│ Player  │
└─────────┘         └─────────┘
```

**Code:**
```java
public class Team {
    private List<Player> players;  // Aggregation
    
    // Player có thể tồn tại không cần Team
}
```

---

#### 3. Composition (Kết hợp)

**Quan hệ "HAS-A" - Object không thể tồn tại độc lập**

```
┌─────────┐         ┌─────────┐
│  House  │◆───────│  Room   │
└─────────┘         └─────────┘
```

**Code:**
```java
public class House {
    private List<Room> rooms;  // Composition
    
    // Room không thể tồn tại không cần House
}
```

---

#### 4. Inheritance (Kế thừa)

**Quan hệ "IS-A"**

```
┌─────────┐
│ Animal  │
└────┬────┘
     │
     │
┌────┴────┐
│   Dog   │
└─────────┘
```

**Code:**
```java
public class Dog extends Animal {
    // ...
}
```

---

#### 5. Implementation (Triển khai Interface)

**Class implement Interface**

```
┌──────────────┐
│  <<interface>>│
│   Flyable    │
└──────┬───────┘
       │
       │ implements
       │
┌──────┴───────┐
│     Bird     │
└──────────────┘
```

**Code:**
```java
public class Bird implements Flyable {
    // ...
}
```

#### So sánh nhanh: Association – Aggregation – Composition

| Quan hệ | Ý nghĩa | Ví dụ | Trong code |
|---------|---------|-------|------------|
| **Association** | A dùng B | Student – Course | `private Course course` |
| **Aggregation** | A chứa B, B sống độc lập | Team – Player | `List<Player> players` |
| **Composition** | A sở hữu B, B gắn vòng đời A | House – Room | Tạo Room trong constructor House |

**Cách nhớ:** IS-A → Inheritance/Implementation; HAS-A → Association/Aggregation/Composition.

---

### 4.1.4. Ví dụ UML hoàn chỉnh

**Hệ thống Quản lý Thư viện:**

```
┌──────────────┐         ┌──────────────┐
│    Book      │────────│    Author    │
└──────────────┘         └──────────────┘
       │
       │
       │
┌──────┴───────┐
│    Loan      │
└──────┬───────┘
       │
       │
┌──────┴───────┐         ┌──────────────┐
│   Member     │────────│   Library    │
└──────────────┘         └──────────────┘
```

**Code tương ứng:**
```java
public class Book {
    private Author author;  // Association
}

public class Loan {
    private Book book;     // Association
    private Member member; // Association
}

public class Library {
    private List<Member> members;  // Aggregation
}
```

---

## 4.2. NGUYÊN LÝ SOLID (TRỌNG TÂM)

### 4.2.1. SOLID là gì?

#### Định nghĩa

**SOLID** là 5 nguyên lý thiết kế phần mềm:
- **S** - Single Responsibility Principle
- **O** - Open/Closed Principle
- **L** - Liskov Substitution Principle
- **I** - Interface Segregation Principle
- **D** - Dependency Inversion Principle

**Mục tiêu:**
- Code dễ bảo trì
- Code dễ mở rộng
- Code dễ test
- Code linh hoạt

**Trong chương này, chúng ta học đủ 5 chữ SOLID; trọng tâm thực hành là S, O, D và hai pattern Singleton, Factory.**

---

### 4.2.2. S – Single Responsibility Principle (SRP)

#### Định nghĩa

**"Một class chỉ nên có một lý do để thay đổi"**

**Nghĩa là:**
- Một class chỉ nên có một trách nhiệm
- Nếu class có nhiều trách nhiệm → Tách thành nhiều class

---

#### Ví dụ: Vi phạm SRP

```java
// ❌ TỒI: Class có nhiều trách nhiệm
public class Student {
    private String name;
    private int age;
    
    // Trách nhiệm 1: Quản lý thông tin Student
    public void setName(String name) { this.name = name; }
    public void setAge(int age) { this.age = age; }
    
    // Trách nhiệm 2: Lưu vào Database
    public void saveToDatabase() {
        // Code lưu database
    }
    
    // Trách nhiệm 3: Gửi Email
    public void sendEmail(String message) {
        // Code gửi email
    }
    
    // Trách nhiệm 4: Tạo Report
    public void generateReport() {
        // Code tạo report
    }
}
```

**Vấn đề:**
- ❌ Nếu thay đổi cách lưu database → Phải sửa Student
- ❌ Nếu thay đổi cách gửi email → Phải sửa Student
- ❌ Nếu thay đổi cách tạo report → Phải sửa Student
- ❌ Class quá phức tạp, khó test

---

#### Giải pháp: Tách thành nhiều class

```java
// ✅ TỐT: Mỗi class một trách nhiệm

// Class 1: Quản lý thông tin Student
public class Student {
    private String name;
    private int age;
    
    public void setName(String name) { this.name = name; }
    public void setAge(int age) { this.age = age; }
    public String getName() { return name; }
    public int getAge() { return age; }
}

// Class 2: Lưu vào Database
public class StudentRepository {
    public void save(Student student) {
        // Code lưu database
    }
    
    public Student findById(String id) {
        // Code tìm trong database
    }
}

// Class 3: Gửi Email
public class EmailService {
    public void sendEmail(String to, String message) {
        // Code gửi email
    }
}

// Class 4: Tạo Report
public class ReportGenerator {
    public void generateReport(Student student) {
        // Code tạo report
    }
}
```

**Lợi ích:**
- ✅ Mỗi class chỉ có một lý do để thay đổi
- ✅ Dễ test từng phần riêng biệt
- ✅ Dễ bảo trì và mở rộng

#### So sánh trước / sau SRP

| Tiêu chí | Một class làm hết | Tách theo SRP |
|----------|-------------------|---------------|
| Sửa database | Sửa class Student | Chỉ sửa StudentRepository |
| Test logic sinh viên | Phải mock DB + email | Test Student độc lập |
| Tái sử dụng | Khó dùng riêng phần email | EmailService dùng ở module khác |

---

#### Cách nhận biết vi phạm SRP

**Dấu hiệu:**
- Class có quá nhiều methods (> 10-15)
- Class có nhiều lý do để thay đổi
- Khó đặt tên class (phải dùng "và", "hoặc")
- Khó test class

**Ví dụ:**
```java
// ❌ Tên class có "và" → Có thể vi phạm SRP
class StudentAndCourseManager { }

// ✅ Tách thành 2 class
class StudentManager { }
class CourseManager { }
```

---

### 4.2.3. O – Open/Closed Principle (OCP)

#### Định nghĩa

**"Software entities should be open for extension, but closed for modification"**

**Nghĩa là:**
- **Open for extension**: Có thể thêm tính năng mới
- **Closed for modification**: Không cần sửa code cũ

---

#### Ví dụ: Vi phạm OCP

```java
// ❌ TỒI: Phải sửa code khi thêm loại mới
public class AreaCalculator {
    public double calculateArea(Object shape) {
        if (shape instanceof Circle) {
            Circle circle = (Circle) shape;
            return Math.PI * circle.getRadius() * circle.getRadius();
        } else if (shape instanceof Rectangle) {
            Rectangle rect = (Rectangle) shape;
            return rect.getWidth() * rect.getHeight();
        } else if (shape instanceof Triangle) {
            Triangle triangle = (Triangle) shape;
            return 0.5 * triangle.getBase() * triangle.getHeight();
        }
        // Thêm loại mới → Phải sửa code này!
        return 0;
    }
}
```

**Vấn đề:**
- ❌ Thêm `Square` → Phải sửa `AreaCalculator`
- ❌ Vi phạm OCP
- ❌ Dễ lỗi (quên case nào đó)

---

#### Giải pháp: Dùng Polymorphism

```java
// ✅ TỐT: Open for extension, closed for modification

// Abstract class hoặc Interface
public abstract class Shape {
    public abstract double calculateArea();
}

// Implementations
public class Circle extends Shape {
    private double radius;
    
    public Circle(double radius) {
        this.radius = radius;
    }
    
    @Override
    public double calculateArea() {
        return Math.PI * radius * radius;
    }
}

public class Rectangle extends Shape {
    private double width;
    private double height;
    
    public Rectangle(double width, double height) {
        this.width = width;
        this.height = height;
    }
    
    @Override
    public double calculateArea() {
        return width * height;
    }
}

// Calculator không cần sửa khi thêm loại mới
public class AreaCalculator {
    public double calculateArea(Shape shape) {
        return shape.calculateArea();  // Polymorphism!
    }
}

// Thêm loại mới - KHÔNG CẦN SỬA AreaCalculator!
public class Triangle extends Shape {
    private double base;
    private double height;
    
    public Triangle(double base, double height) {
        this.base = base;
        this.height = height;
    }
    
    @Override
    public double calculateArea() {
        return 0.5 * base * height;
    }
}
```

**Lợi ích:**
- ✅ Thêm loại mới → Chỉ cần tạo class mới
- ✅ Không cần sửa code cũ
- ✅ Tuân thủ OCP

---

#### Thực hành: Refactor code vi phạm OCP

**Code cũ:**
```java
public class PaymentProcessor {
    public void processPayment(String type, double amount) {
        if (type.equals("credit")) {
            // Process credit card
        } else if (type.equals("paypal")) {
            // Process PayPal
        }
        // Thêm loại mới → Phải sửa code này
    }
}
```

**Code mới (tuân thủ OCP):**
```java
// Interface
public interface PaymentMethod {
    void pay(double amount);
}

// Implementations
public class CreditCard implements PaymentMethod {
    @Override
    public void pay(double amount) {
        // Process credit card
    }
}

public class PayPal implements PaymentMethod {
    @Override
    public void pay(double amount) {
        // Process PayPal
    }
}

// Processor không cần sửa khi thêm loại mới
public class PaymentProcessor {
    public void processPayment(PaymentMethod method, double amount) {
        method.pay(amount);  // Polymorphism!
    }
}

// Thêm loại mới - KHÔNG CẦN SỬA PaymentProcessor!
public class Bitcoin implements PaymentMethod {
    @Override
    public void pay(double amount) {
        // Process Bitcoin
    }
}
```

#### So sánh: if/instanceof vs Polymorphism (OCP)

| | Nhiều if/instanceof | Interface + đa hình |
|--|---------------------|---------------------|
| Thêm loại mới | Sửa class trung tâm | Thêm class mới implement interface |
| Rủi ro regression | Cao (đụng code cũ) | Thấp |
| Liên hệ Chương 3 | — | Dùng Polymorphism đã học |

**Ví dụ dễ nhớ:** Cài **app mới** trên điện thoại (mở rộng) thay vì **tháo máy hàn chip** (sửa phần cứng).

---

### 4.2.4. L – Liskov Substitution Principle (LSP)

#### Định nghĩa (một câu)

**“Class con phải thay thế được class cha mà không làm hỏng chương trình.”**

Nếu đoạn code dùng biến kiểu cha (`Shape`, `Rectangle`), thay bằng bất kỳ class con nào cũng phải **đúng ý nghĩa**, không gây lỗi hoặc hành vi bất ngờ.

#### Ví dụ đời thường

Trong toán, **hình vuông là hình chữ nhật đặc biệt**. Trong code, nếu `Square extends Rectangle` và override `setWidth`/`setHeight` để luôn giữ hai cạnh bằng nhau, người code kiểu `Rectangle` sẽ **bị bất ngờ** khi set width=4, height=5 mà diện tích không phải 20.

#### Ví dụ code: Vi phạm LSP

```java
public class Rectangle {
    protected int width, height;
    public void setWidth(int w)  { width = w; }
    public void setHeight(int h) { height = h; }
    public int getArea() { return width * height; }
}

public class Square extends Rectangle {
    @Override
    public void setWidth(int w)  { width = height = w; }
    @Override
    public void setHeight(int h) { width = height = h; }
}

// Kỳ vọng: Rectangle r; r.setWidth(4); r.setHeight(5); → area = 20
// Thực tế với Square: area = 25 → VI PHẠM LSP
```

#### Cách làm đúng

- Không ép `Square extends Rectangle` nếu hành vi không tương thích.
- Dùng interface chung: `Shape` với `getArea()`; `Rectangle` và `Square` đều implement `Shape`.

#### So sánh & Đánh giá LSP

| Tuân thủ LSP | Vi phạm LSP |
|--------------|-------------|
| Thay con vào chỗ cha → chương trình vẫn đúng | Thay con → kết quả sai hoặc exception |
| Đa hình đáng tin cậy | Debug khó vì “đúng kiểu nhưng sai hành vi” |

**Câu hỏi tự kiểm tra:** “Nếu tôi thay subclass vào mọi chỗ đang dùng superclass, code còn đúng không?”

---

### 4.2.5. I – Interface Segregation Principle (ISP)

#### Định nghĩa (một câu)

**“Client không nên bị ép phụ thuộc vào method của interface mà nó không dùng.”**

Tách interface lớn thành nhiều interface nhỏ, gắn với **một vai trò**.

#### Ví dụ đời thường

Remote **50 nút** (TV, điều hòa, quạt…) trong khi bạn chỉ cần bật quạt → rối. Cách tốt: remote quạt chỉ có nút quạt.

#### Ví dụ code

**Vi phạm ISP:**

```java
public interface IWorker {
    void work();
    void eat();
    void sleep();
    double getSalary();
}

public class Robot implements IWorker {
    public void work() { /* ... */ }
    public void eat() { }      // Robot không ăn – method rỗng
    public void sleep() { }
    public double getSalary() { return 0; }
}
```

**Tuân thủ ISP:**

```java
public interface IWorkable { void work(); }
public interface IEatable { void eat(); }
public interface IPayable { double getSalary(); }

public class Human implements IWorkable, IEatable, IPayable { /* ... */ }
public class Robot implements IWorkable { /* chỉ work */ }
```

#### So sánh & Đánh giá ISP

| Interface “bự” | Interface nhỏ (ISP) |
|----------------|---------------------|
| Class implement method không cần | Class chỉ implement đúng vai trò |
| Sửa interface → ảnh hưởng nhiều class | Sửa một vai trò → ít ảnh hưởng |

---

### 4.2.6. Bảng tóm tắt SOLID – Tra nhanh

| Chữ | Tên | Một câu nhớ | Ví dụ dễ hiểu |
|-----|-----|-------------|----------------|
| **S** | Single Responsibility | Một class một việc | Bồi bàn không nấu ăn |
| **O** | Open/Closed | Thêm mới bằng class mới, không sửa code cũ | Cài app mới thay vì tháo máy |
| **L** | Liskov Substitution | Con thay cha không làm hỏng logic | Vuông không nên “giả” chữ nhật nếu hành vi khác |
| **I** | Interface Segregation | Interface nhỏ, đúng vai trò | Remote quạt chỉ có nút quạt |
| **D** | Dependency Inversion | Phụ thuộc interface, không phụ thuộc class cụ thể | Đèn cắm ổ chuẩn, không hàn chết vào một ổ |

**Thứ tự áp dụng khi viết code:** S (tách class) → O (interface + đa hình) → D (inject dependency) → L, I (khi thiết kế kế thừa và interface).

---

## 4.3. DEPENDENCY INVERSION PRINCIPLE (D – SOLID)

### 4.3.1. Dependency Inversion là gì?

#### Định nghĩa

**"High-level modules should not depend on low-level modules. Both should depend on abstractions."**

**Nghĩa là:**
- Module cấp cao không nên phụ thuộc vào module cấp thấp
- Cả hai nên phụ thuộc vào abstraction (interface)

---

### 4.3.2. High-level vs Low-level Modules

#### Định nghĩa

**High-level Module (Module cấp cao):**
- Chứa business logic
- Quyết định "Làm gì" (What)
- Ví dụ: `PaymentService`, `OrderService`

**Low-level Module (Module cấp thấp):**
- Chứa implementation chi tiết
- Quyết định "Làm thế nào" (How)
- Ví dụ: `DatabaseRepository`, `FileRepository`, `EmailService`

---

#### Ví dụ: Vi phạm DIP

```java
// ❌ TỒI: High-level phụ thuộc vào Low-level

// Low-level module
public class MySQLDatabase {
    public void save(String data) {
        // Code lưu vào MySQL
    }
}

// High-level module - PHỤ THUỘC vào MySQLDatabase
public class PaymentService {
    private MySQLDatabase database;  // ❌ Phụ thuộc cụ thể
    
    public PaymentService() {
        this.database = new MySQLDatabase();  // ❌ Tạo dependency cứng
    }
    
    public void processPayment(double amount) {
        // Business logic
        database.save("Payment: " + amount);  // ❌ Phụ thuộc MySQL
    }
}
```

**Vấn đề:**
- ❌ Muốn đổi sang PostgreSQL → Phải sửa `PaymentService`
- ❌ Khó test (không thể mock database)
- ❌ Vi phạm DIP

---

#### Giải pháp: Depend on Abstraction

```java
// ✅ TỐT: Depend on Abstraction

// Abstraction (Interface)
public interface Database {
    void save(String data);
}

// Low-level implementations
public class MySQLDatabase implements Database {
    @Override
    public void save(String data) {
        // Code lưu vào MySQL
    }
}

public class PostgreSQLDatabase implements Database {
    @Override
    public void save(String data) {
        // Code lưu vào PostgreSQL
    }
}

// High-level module - PHỤ THUỘC vào Interface
public class PaymentService {
    private Database database;  // ✅ Phụ thuộc abstraction
    
    // Dependency Injection
    public PaymentService(Database database) {
        this.database = database;  // ✅ Nhận dependency từ bên ngoài
    }
    
    public void processPayment(double amount) {
        // Business logic
        database.save("Payment: " + amount);  // ✅ Không phụ thuộc implementation cụ thể
    }
}

// Sử dụng
Database mysql = new MySQLDatabase();
PaymentService service1 = new PaymentService(mysql);

Database postgres = new PostgreSQLDatabase();
PaymentService service2 = new PaymentService(postgres);

// Dễ test - có thể mock
Database mockDb = new MockDatabase();
PaymentService testService = new PaymentService(mockDb);
```

**Lợi ích:**
- ✅ Dễ thay đổi implementation (MySQL → PostgreSQL)
- ✅ Dễ test (có thể mock)
- ✅ Tuân thủ DIP

---

### 4.3.3. Dependency Injection (DI)

#### Định nghĩa

**Dependency Injection** là kỹ thuật:
- Không tạo dependency bên trong class
- Nhận dependency từ bên ngoài (constructor, setter, field)

---

#### Các cách Inject Dependency

**1. Constructor Injection (Khuyến nghị):**
```java
public class PaymentService {
    private Database database;
    
    // Inject qua constructor
    public PaymentService(Database database) {
        this.database = database;
    }
}

// Sử dụng
Database db = new MySQLDatabase();
PaymentService service = new PaymentService(db);
```

**2. Setter Injection:**
```java
public class PaymentService {
    private Database database;
    
    // Inject qua setter
    public void setDatabase(Database database) {
        this.database = database;
    }
}

// Sử dụng
PaymentService service = new PaymentService();
service.setDatabase(new MySQLDatabase());
```

**3. Field Injection (Không khuyến nghị):**
```java
public class PaymentService {
    @Inject  // Framework annotation
    private Database database;
}
```

---

### 4.3.4. Tại sao từ khóa new là "kẻ thù"?

#### Vấn đề với new

```java
// ❌ TỒI: Tạo dependency bên trong class
public class PaymentService {
    private Database database;
    
    public PaymentService() {
        this.database = new MySQLDatabase();  // ❌ Hard-coded dependency
    }
}
```

**Vấn đề:**
- ❌ Khó test (không thể mock)
- ❌ Khó thay đổi (phải sửa code)
- ❌ Vi phạm DIP

---

#### Giải pháp: Inject từ bên ngoài

```java
// ✅ TỐT: Inject từ bên ngoài
public class PaymentService {
    private Database database;
    
    public PaymentService(Database database) {
        this.database = database;  // ✅ Nhận từ bên ngoài
    }
}

// Tạo dependency ở nơi khác (Factory, Main, Framework)
Database db = new MySQLDatabase();
PaymentService service = new PaymentService(db);
```

---

## 4.4. LIÊN HỆ DEPENDENCY INVERSION VỚI FACTORY & INTERFACE

### 4.4.1. Luồng thiết kế chuẩn

#### Client → Interface → Factory → Implementation

```
┌─────────┐      ┌──────────┐      ┌────────┐      ┌──────────────┐
│ Client  │─────▶│Interface │◀─────│ Factory│─────▶│Implementation│
└─────────┘      └──────────┘      └────────┘      └──────────────┘
```

**Ví dụ:**
```java
// 1. Interface (Abstraction)
public interface Database {
    void save(String data);
}

// 2. Implementations
public class MySQLDatabase implements Database {
    @Override
    public void save(String data) {
        // MySQL implementation
    }
}

public class PostgreSQLDatabase implements Database {
    @Override
    public void save(String data) {
        // PostgreSQL implementation
    }
}

// 3. Factory (Tạo implementation)
public class DatabaseFactory {
    public static Database createDatabase(String type) {
        if (type.equals("mysql")) {
            return new MySQLDatabase();
        } else if (type.equals("postgres")) {
            return new PostgreSQLDatabase();
        }
        throw new IllegalArgumentException("Unknown database type");
    }
}

// 4. Client (Sử dụng)
public class PaymentService {
    private Database database;
    
    public PaymentService(Database database) {
        this.database = database;
    }
    
    public void processPayment(double amount) {
        database.save("Payment: " + amount);
    }
}

// 5. Main (Kết nối tất cả)
public class Main {
    public static void main(String[] args) {
        // Factory tạo implementation
        Database db = DatabaseFactory.createDatabase("mysql");
        
        // Client nhận qua constructor
        PaymentService service = new PaymentService(db);
        
        // Sử dụng
        service.processPayment(100.0);
    }
}
```

**Lợi ích:**
- ✅ Client không biết implementation cụ thể
- ✅ Dễ thay đổi implementation
- ✅ Tuân thủ DIP

---

## 4.5. DESIGN PATTERNS CƠ BẢN

### 4.5.1. Design Pattern là gì?

#### Định nghĩa

**Design Pattern** là giải pháp tái sử dụng cho các vấn đề thiết kế phổ biến.

**Lợi ích:**
- ✅ Giải pháp đã được kiểm chứng
- ✅ Dễ hiểu và giao tiếp
- ✅ Tái sử dụng

**Trong chương này, chúng ta học 2 patterns cơ bản:**
1. Singleton Pattern
2. Factory Pattern

---

### 4.5.2. Singleton Pattern

#### Mục đích

**Singleton Pattern** đảm bảo một class chỉ có **một instance duy nhất** và cung cấp điểm truy cập toàn cục.

**Khi nào dùng:**
- Database connection
- Logger
- Configuration
- Cache

---

#### Cách cài đặt cơ bản (Không thread-safe)

```java
// ❌ Không thread-safe
public class Singleton {
    private static Singleton instance;
    
    // Private constructor - không cho tạo từ bên ngoài
    private Singleton() {
    }
    
    public static Singleton getInstance() {
        if (instance == null) {
            instance = new Singleton();
        }
        return instance;
    }
}
```

**Vấn đề:**
- ❌ Không thread-safe (nhiều thread có thể tạo nhiều instance)

---

#### Cách cài đặt Thread-safe (Eager Initialization)

```java
// ✅ Thread-safe: Eager Initialization
public class Singleton {
    // Tạo instance ngay khi class được load
    private static final Singleton instance = new Singleton();
    
    private Singleton() {
    }
    
    public static Singleton getInstance() {
        return instance;
    }
}
```

**Ưu điểm:**
- ✅ Thread-safe
- ✅ Đơn giản

**Nhược điểm:**
- ❌ Tạo instance ngay cả khi không dùng (lãng phí)

---

#### Cách cài đặt Thread-safe (Lazy Initialization với synchronized)

```java
// ✅ Thread-safe: Lazy Initialization
public class Singleton {
    private static Singleton instance;
    
    private Singleton() {
    }
    
    // Synchronized để thread-safe
    public static synchronized Singleton getInstance() {
        if (instance == null) {
            instance = new Singleton();
        }
        return instance;
    }
}
```

**Ưu điểm:**
- ✅ Thread-safe
- ✅ Lazy (chỉ tạo khi cần)

**Nhược điểm:**
- ❌ Chậm hơn (synchronized mỗi lần gọi)

---

#### Cách cài đặt Thread-safe (Double-Checked Locking)

```java
// ✅ Thread-safe: Double-Checked Locking
public class Singleton {
    private static volatile Singleton instance;  // volatile quan trọng!
    
    private Singleton() {
    }
    
    public static Singleton getInstance() {
        if (instance == null) {  // Check 1: Không synchronized
            synchronized (Singleton.class) {
                if (instance == null) {  // Check 2: Có synchronized
                    instance = new Singleton();
                }
            }
        }
        return instance;
    }
}
```

**Ưu điểm:**
- ✅ Thread-safe
- ✅ Lazy
- ✅ Nhanh hơn (chỉ synchronized lần đầu)

**Lưu ý:**
- Phải dùng `volatile` để đảm bảo visibility

#### Bảng so sánh các cách implement Singleton

| Cách | Thread-safe | Tạo khi nào | Ưu | Nhược |
|------|-------------|-------------|-----|-------|
| if null đơn giản | ❌ | Lần đầu gọi | Dễ hiểu | Đa luồng tạo nhiều instance |
| Eager | ✅ | Load class | Đơn giản, an toàn | Tạo dù chưa dùng |
| Lazy + synchronized | ✅ | Lần đầu gọi | Chỉ tạo khi cần | Mỗi lần gọi đều lock |
| Double-Checked | ✅ | Lần đầu gọi | Lazy + nhanh hơn | Code phức tạp, cần volatile |

**Khuyến nghị môn học:** Bài tập dùng **Eager** hoặc **Lazy + synchronized**; biết Double-Checked là đủ cho nâng cao.

---

#### Cảnh báo lạm dụng Singleton

**❌ KHÔNG NÊN dùng khi:**
- Có thể dùng dependency injection
- Cần test (khó mock singleton)
- Cần nhiều instance

**✅ NÊN dùng khi:**
- Thực sự cần một instance duy nhất
- Resource tốn kém (database connection)
- Global state (configuration)

---

### 4.5.3. Factory Pattern

#### Mục đích

**Factory Pattern** cung cấp interface để tạo object mà không cần chỉ định class cụ thể.

**Khi nào dùng:**
- Không biết trước loại object cần tạo
- Muốn tách logic tạo object khỏi code sử dụng
- Cần tạo object phức tạp

---

#### Simple Factory

```java
// Interface
public interface Animal {
    void makeSound();
}

// Implementations
public class Dog implements Animal {
    @Override
    public void makeSound() {
        System.out.println("Woof!");
    }
}

public class Cat implements Animal {
    @Override
    public void makeSound() {
        System.out.println("Meow!");
    }
}

// Factory
public class AnimalFactory {
    public static Animal createAnimal(String type) {
        if (type.equalsIgnoreCase("dog")) {
            return new Dog();
        } else if (type.equalsIgnoreCase("cat")) {
            return new Cat();
        }
        throw new IllegalArgumentException("Unknown animal type: " + type);
    }
}

// Sử dụng
Animal dog = AnimalFactory.createAnimal("dog");
dog.makeSound();  // "Woof!"

Animal cat = AnimalFactory.createAnimal("cat");
cat.makeSound();  // "Meow!"
```

**Lợi ích:**
- ✅ Tách logic tạo object
- ✅ Dễ thêm loại mới
- ✅ Client không biết implementation cụ thể

---

#### Factory Method Pattern

```java
// Abstract Factory
public abstract class AnimalFactory {
    public abstract Animal createAnimal();
    
    public void displayAnimal() {
        Animal animal = createAnimal();
        animal.makeSound();
    }
}

// Concrete Factories
public class DogFactory extends AnimalFactory {
    @Override
    public Animal createAnimal() {
        return new Dog();
    }
}

public class CatFactory extends AnimalFactory {
    @Override
    public Animal createAnimal() {
        return new Cat();
    }
}

// Sử dụng
AnimalFactory factory = new DogFactory();
factory.displayAnimal();  // "Woof!"

factory = new CatFactory();
factory.displayAnimal();  // "Meow!"
```

---

#### Ứng dụng Factory Pattern với Dependency Inversion

```java
// Interface
public interface Database {
    void connect();
    void save(String data);
}

// Implementations
public class MySQLDatabase implements Database {
    @Override
    public void connect() {
        System.out.println("Connecting to MySQL...");
    }
    
    @Override
    public void save(String data) {
        System.out.println("Saving to MySQL: " + data);
    }
}

public class PostgreSQLDatabase implements Database {
    @Override
    public void connect() {
        System.out.println("Connecting to PostgreSQL...");
    }
    
    @Override
    public void save(String data) {
        System.out.println("Saving to PostgreSQL: " + data);
    }
}

// Factory
public class DatabaseFactory {
    public static Database createDatabase(String type) {
        if (type.equalsIgnoreCase("mysql")) {
            return new MySQLDatabase();
        } else if (type.equalsIgnoreCase("postgres")) {
            return new PostgreSQLDatabase();
        }
        throw new IllegalArgumentException("Unknown database type");
    }
}

// Service sử dụng Factory
public class PaymentService {
    private Database database;
    
    public PaymentService(Database database) {
        this.database = database;
    }
    
    public void processPayment(double amount) {
        database.connect();
        database.save("Payment: " + amount);
    }
}

// Main
public class Main {
    public static void main(String[] args) {
        // Factory tạo database
        Database db = DatabaseFactory.createDatabase("mysql");
        
        // Service nhận database
        PaymentService service = new PaymentService(db);
        
        // Sử dụng
        service.processPayment(100.0);
    }
}
```

**Lợi ích:**
- ✅ Tuân thủ DIP
- ✅ Dễ thay đổi implementation
- ✅ Dễ test

---

### 4.5.4. Strategy Pattern (Kẻ hủy diệt If/Else)

#### Mục đích
**Strategy Pattern** cho phép bạn định nghĩa một tập hợp các thuật toán (các chiến lược), đóng gói từng thuật toán lại, và làm cho chúng có thể thay thế lẫn nhau. Pattern này giúp thuật toán biến đổi độc lập với client sử dụng nó.

Đây là pattern hoàn hảo nhất để giải thích và áp dụng **Open/Closed Principle (OCP)**.

#### Ví dụ: Bài toán tính giá vé máy bay (Vi phạm OCP)
```java
public class TicketCalculator {
    public double calculateTicketPrice(String strategy, double basePrice) {
        if (strategy.equals("TET_HOLIDAY")) {
            return basePrice * 1.5;
        } else if (strategy.equals("SUMMER_PROMO")) {
            return basePrice * 0.8;
        } else {
            return basePrice;
        }
    }
}
```
**Vấn đề:** Mỗi khi phòng Marketing tung ra khuyến mãi mới (ví dụ: Black Friday), bạn phải mở class này ra, thêm một cái `if` nữa. -> **VI PHẠM OCP**.

#### Cách giải quyết bằng Strategy Pattern
**1. Tạo Interface (Chiến lược chung)**
```java
public interface PricingStrategy {
    double calculate(double basePrice);
}
```

**2. Tạo các Concrete Strategy (Cài đặt thuật toán)**
```java
// Chiến lược ngày Tết
public class TetHolidayStrategy implements PricingStrategy {
    @Override
    public double calculate(double basePrice) {
        return basePrice * 1.5;
    }
}

// Chiến lược mùa Hè
public class SummerPromoStrategy implements PricingStrategy {
    @Override
    public double calculate(double basePrice) {
        return basePrice * 0.8;
    }
}

// Thêm mới chiến lược Black Friday CỰC DỄ, không cần sửa code cũ
public class BlackFridayStrategy implements PricingStrategy {
    @Override
    public double calculate(double basePrice) {
        return basePrice * 0.5;
    }
}
```

**3. Context (Nơi sử dụng chiến lược)**
```java
public class TicketContext {
    private PricingStrategy strategy; // Dùng Composition (HAS-A)

    // Cho phép thay đổi chiến lược linh hoạt lúc Runtime
    public void setStrategy(PricingStrategy strategy) {
        this.strategy = strategy;
    }

    public double getFinalPrice(double basePrice) {
        return strategy.calculate(basePrice); // Đa hình (Polymorphism)
    }
}
```

**4. Client sử dụng**
```java
TicketContext ticket = new TicketContext();

// Lúc thường
ticket.setStrategy(new SummerPromoStrategy());
System.out.println(ticket.getFinalPrice(1000)); // 800

// Đến Black Friday, chỉ việc thay đổi Strategy
ticket.setStrategy(new BlackFridayStrategy());
System.out.println(ticket.getFinalPrice(1000)); // 500
```

**Lợi ích tuyệt đối:**
- ✅ **Tuân thủ OCP**: Thêm hàng ngàn chương trình khuyến mãi mới mà không cần chạm vào `TicketContext`.
- ✅ Loại bỏ hoàn toàn khối lệnh `if/else` chằng chịt.
- ✅ Có thể chuyển đổi chiến lược ngay lúc chương trình đang chạy (Runtime).

---

## 🔗 CẦU NỐI SANG CHƯƠNG 5

| Chương 4 | Chương 5 | Liên hệ |
|----------|----------|---------|
| DIP + inject dependency | Unit Test với mock | Test `PaymentService` với Database giả |
| Factory tạo object | Exception khi config sai | Factory ném `IllegalArgumentException` |
| SRP – class nhỏ | Dễ test từng class | Mỗi class một bộ test |

**Thực hành:** Viết test cho class đã thiết kế theo DIP (Ch.4) – đây là kỹ năng bắt buộc ở Ch.5.

---

## 📝 TÓM TẮT CHƯƠNG 4

### Kiến thức đã học:
1. ✅ UML Class Diagram cơ bản
2. ✅ Các quan hệ: Association, Aggregation, Composition, Inheritance, Implementation
3. ✅ SOLID: SRP, OCP, **LSP**, **ISP**, DIP
4. ✅ Dependency Injection (Constructor, Setter)
5. ✅ Singleton Pattern (Eager, Lazy, Double-Checked)
6. ✅ Factory Pattern (Simple Factory, kết hợp DIP)

### Kỹ năng đã có:
- ✅ Đọc và vẽ UML Class Diagram
- ✅ Nhận biết và refactor code vi phạm SOLID
- ✅ Áp dụng DIP trong thiết kế
- ✅ Implement Singleton và Factory Pattern

---

## 🎯 BÀI TẬP CHƯƠNG 4

### Bài 1: UML Class Diagram

**Yêu cầu:**
Vẽ UML Class Diagram cho hệ thống sau:

1. **Class `Student`**:
   - Fields: id, name, age (private)
   - Methods: getters, setters, displayInfo()

2. **Class `Course`**:
   - Fields: code, name, credits (private)
   - Methods: getters, setters

3. **Class `Enrollment`**:
   - Fields: student (Student), course (Course), grade (double)
   - Methods: calculateGPA()

**Quan hệ:**
- Student và Course có quan hệ Association qua Enrollment
- Enrollment có Composition với Student và Course

---

### Bài 2: Single Responsibility Principle

**Yêu cầu:**
Refactor class sau để tuân thủ SRP:

```java
public class User {
    private String name;
    private String email;
    
    public void setName(String name) { this.name = name; }
    public void setEmail(String email) { this.email = email; }
    
    public void saveToDatabase() {
        // Code lưu database
    }
    
    public void sendEmail(String message) {
        // Code gửi email
    }
    
    public void generateReport() {
        // Code tạo report
    }
}
```

**Yêu cầu:**
- Tách thành nhiều class, mỗi class một trách nhiệm
- Giải thích tại sao refactor

---

### Bài 3: Open/Closed Principle

**Yêu cầu:**
Refactor code sau để tuân thủ OCP:

```java
public class AreaCalculator {
    public double calculateArea(Object shape) {
        if (shape instanceof Circle) {
            Circle c = (Circle) shape;
            return Math.PI * c.getRadius() * c.getRadius();
        } else if (shape instanceof Rectangle) {
            Rectangle r = (Rectangle) shape;
            return r.getWidth() * r.getHeight();
        }
        return 0;
    }
}
```

**Yêu cầu:**
- Dùng Abstract Class hoặc Interface
- Thêm class `Triangle` mà không sửa `AreaCalculator`

---

### Bài 4: Dependency Inversion Principle

**Yêu cầu:**
Refactor code sau để tuân thủ DIP:

```java
public class PaymentService {
    private MySQLDatabase database;
    
    public PaymentService() {
        this.database = new MySQLDatabase();
    }
    
    public void processPayment(double amount) {
        database.save("Payment: " + amount);
    }
}
```

**Yêu cầu:**
1. Tạo interface `Database`
2. Tạo class `PostgreSQLDatabase` implement `Database`
3. Refactor `PaymentService` để phụ thuộc vào interface
4. Sử dụng Dependency Injection

---

### Bài 5: Singleton Pattern

**Yêu cầu:**
Tạo class `Logger` sử dụng Singleton Pattern:
1. Thread-safe (dùng một trong các cách đã học)
2. Method `log(String message)` để ghi log
3. Method `getInstance()` để lấy instance

**Test:**
```java
Logger logger1 = Logger.getInstance();
Logger logger2 = Logger.getInstance();

System.out.println(logger1 == logger2);  // true

logger1.log("Message 1");
logger2.log("Message 2");
```

---

### Bài 6: Factory Pattern

**Yêu cầu:**
Tạo hệ thống Payment với Factory Pattern:

1. **Interface `PaymentMethod`**:
   - Method: `pay(double amount)`

2. **Implementations**:
   - `CreditCard`: Pay với credit card
   - `PayPal`: Pay với PayPal
   - `Bitcoin`: Pay với Bitcoin

3. **Factory `PaymentFactory`**:
   - Method: `createPaymentMethod(String type)`

4. **Class `PaymentService`**:
   - Method: `processPayment(PaymentMethod method, double amount)`

**Test:**
```java
PaymentMethod credit = PaymentFactory.createPaymentMethod("credit");
PaymentService service = new PaymentService();
service.processPayment(credit, 100.0);
```

---

### Bài 7: Tổng hợp - Hệ thống Quản lý File

**Yêu cầu:**
Tạo hệ thống quản lý file với:

1. **Interface `FileStorage`**:
   - Methods: `save(String data)`, `load(String filename)`

2. **Implementations**:
   - `LocalFileStorage`: Lưu vào file local
   - `CloudFileStorage`: Lưu vào cloud

3. **Factory `StorageFactory`**:
   - Tạo storage dựa trên config

4. **Class `FileManager`**:
   - Phụ thuộc vào `FileStorage` (DIP)
   - Methods: `saveFile()`, `loadFile()`

5. **Singleton `Config`**:
   - Lưu cấu hình hệ thống

**Yêu cầu:**
- Tuân thủ SOLID (S, O, D)
- Sử dụng Factory và Singleton Pattern
- JavaDoc đầy đủ

---

## 📚 TÀI LIỆU THAM KHẢO

1. **SOLID Principles**: https://en.wikipedia.org/wiki/SOLID
2. **Design Patterns**: "Head First Design Patterns" - Eric Freeman
3. **Refactoring**: "Refactoring" - Martin Fowler

---

**Chúc bạn học tốt! 🚀**

