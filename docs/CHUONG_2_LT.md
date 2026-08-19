# CHƯƠNG 2: LỚP (CLASS) & ĐỐI TƯỢNG (OBJECT) – MODERN JAVA APPROACH

## 📚 MỤC TIÊU HỌC TẬP

Sau khi hoàn thành chương này, bạn sẽ:
- Hiểu sâu về Class và Object trong Java
- Nắm vững cơ chế cấp phát bộ nhớ (Stack vs Heap)
- Thực hành Encapsulation và Data Hiding
- Sử dụng thành thạo Constructors, Getters/Setters
- Áp dụng Java Records cho Data Classes
- Thiết kế Immutable Objects

---

## 📋 KIẾN THỨC CẦN CÓ (PREREQUISITES)

Trước khi học chương này, bạn cần nắm vững:

- [ ] ✅ **Cú pháp Java cơ bản** (Chương 1, phần 0.2)
  - Hiểu cấu trúc class, method, main
  - Biết cách compile và chạy chương trình Java

- [ ] ✅ **Kiểu dữ liệu** (Chương 1, phần 0.3)
  - Primitive types (int, double, boolean, ...)
  - Reference types (String, arrays, ...)

- [ ] ✅ **Method cơ bản** (Chương 1, phần 0.8)
  - Cách định nghĩa method
  - Cách gọi method
  - Tham số và return type
  - `static` method (đã học cơ bản)

- [ ] ✅ **Từ khóa `new`** (Chương 1, phần 0.8)
  - Đã dùng `new` để tạo object trong ví dụ
  - Bây giờ sẽ học sâu hơn về cách `new` hoạt động

- [ ] ✅ **Reference và null** (Chương 1, phần 0.3, 0.15)
  - Hiểu reference types
  - Biết về `null` và NullPointerException

> **💡 Lưu ý:** Nếu bạn chưa nắm vững các kiến thức trên, hãy quay lại **Chương 1, phần 0** để ôn tập trước!

---

## 🎓 HƯỚNG DẪN HỌC SÂU CHƯƠNG 2

| Mục lý thuyết | Code chạy thử |
|---------------|---------------|
| Encapsulation (2.2) | `CODE/VI_DU/CHUONG_2/01_Encapsulation/` |

```bash
cd CODE/VI_DU/CHUONG_2/01_Encapsulation
javac Main.java && java Main
```

**Output:** Chủ TK An, số dư 1500 → 1200 sau rút 300.

**Giảng sâu – vì sao `private`?** Field `balance` không thể `acc.balance = 999999` từ bên ngoài → mọi thay đổi qua `deposit`/`withdraw` có **kiểm soát** (amount > 0, không rút quá số dư).

---

## 2.1. CLASS, OBJECT & VÒNG ĐỜI ĐỐI TƯỢNG

### 2.1.1. Class (Lớp) - Khuôn mẫu

#### Định nghĩa

**Class** là một khuôn mẫu (template) để tạo ra các đối tượng. Class định nghĩa:
- **Attributes (Thuộc tính)**: Dữ liệu mà đối tượng có
- **Methods (Phương thức)**: Hành vi mà đối tượng có thể thực hiện

#### Ví dụ thực tế

**Tưởng tượng:**
- **Class `Car`** = Bản vẽ thiết kế xe hơi
- **Object `myCar`** = Chiếc xe cụ thể được sản xuất từ bản vẽ

**Bản vẽ (Class) mô tả:**
- Màu sắc, số bánh, động cơ (attributes)
- Chạy, dừng, rẽ (methods)

**Chiếc xe (Object) cụ thể:**
- Màu đỏ, 4 bánh, động cơ V8
- Có thể chạy, dừng, rẽ

---

> **🍪 Tưởng tượng khác: Khuôn làm bánh**
> - **Class**: Cái khuôn cắt bánh hình ngôi sao.
> - **Object**: Những chiếc bánh quy ngôi sao được tạo ra từ khuôn đó.
> - Bạn có thể dùng 1 cái khuôn để tạo ra hàng trăm chiếc bánh. Mỗi chiếc bánh có thể trang trí khác nhau (màu sắc, topping) nhưng đều có hình ngôi sao.

---

#### Cú pháp Class cơ bản

```java
// Cú pháp
[access modifier] class ClassName {
    // Fields (Thuộc tính)
    [access modifier] [static] [final] dataType fieldName;
    
    // Constructors (Hàm khởi tạo)
    [access modifier] ClassName(parameters) {
        // Khởi tạo đối tượng
    }
    
    // Methods (Phương thức)
    [access modifier] [static] returnType methodName(parameters) {
        // Thực hiện hành vi
    }
}
```

**Ví dụ cụ thể:**
```java
public class Student {
    // Fields (Thuộc tính)
    private String name;
    private int age;
    private double gpa;
    
    // Constructor (Hàm khởi tạo)
    public Student(String name, int age, double gpa) {
        this.name = name;
        this.age = age;
        this.gpa = gpa;
    }
    
    // Methods (Phương thức)
    public void displayInfo() {
        System.out.println("Name: " + name + ", Age: " + age + ", GPA: " + gpa);
    }
    
    public double getGpa() {
        return gpa;
    }
}
```

---

### 2.1.2. Object (Đối tượng) - Thực thể

#### Định nghĩa

**Object** là một thực thể cụ thể được tạo từ class. Mỗi object có:
- **Trạng thái riêng**: Giá trị của các fields
- **Hành vi**: Các methods được định nghĩa trong class

#### Tạo Object

**Cú pháp:**
```java
ClassName objectName = new ClassName(arguments);
```

**Ví dụ:**
```java
// Tạo object từ class Student
Student student1 = new Student("Nguyen Van A", 20, 3.5);
Student student2 = new Student("Tran Thi B", 21, 3.8);

// Mỗi object có trạng thái riêng
student1.displayInfo();  // Name: Nguyen Van A, Age: 20, GPA: 3.5
student2.displayInfo();  // Name: Tran Thi B, Age: 21, GPA: 3.8
```

**Phân tích:**
- `student1` và `student2` là 2 object khác nhau
- Cùng class `Student` nhưng có dữ liệu khác nhau
- Gọi cùng method `displayInfo()` nhưng kết quả khác nhau

---

### 2.1.3. Cơ chế Cấp phát Bộ nhớ trong Java

#### Stack vs Heap

Java quản lý bộ nhớ trong 2 vùng chính:

> **🧠 Tưởng tượng:**
> - **Stack (Ngăn xếp)**: Giống như một **Danh sách việc cần làm (To-Do List)** trên bàn làm việc. Bạn ghi việc cần làm ra giấy note, làm xong việc nào thì vò nát tờ giấy ném đi ngay. Nó rất nhanh, gọn, nhưng chỉ chứa thông tin tạm thời.
> - **Heap (Đống)**: Giống như một **Nhà kho khổng lồ**. Khi bạn mua một món đồ lớn (TV, Tủ lạnh - tức là Object), bạn không để lên bàn làm việc (Stack) mà cất vào nhà kho (Heap). Trên bàn làm việc, bạn chỉ giữ một tờ giấy ghi địa chỉ của món đồ trong kho (Reference).

163: **2. Heap (Đống) - "Nhà kho"**
164: - Lưu trữ: Objects, arrays (Những thứ to lớn).
165: - Đặc điểm: Chậm hơn Stack, nhưng rộng rãi. Được dọn dẹp bởi "người lao công" (Garbage Collector).
166: - Kích thước: Lớn, động.

#### Ví dụ minh họa

```java
public class MemoryExample {
    public static void main(String[] args) {
        // Biến local lưu trong Stack
        int number = 10;  // Stack
        
        // Object được tạo trong Heap
        // Reference 'student' lưu trong Stack
        // Object thực tế lưu trong Heap
        Student student = new Student("John", 20, 3.5);
        
        // Khi method kết thúc:
        // - 'number' và 'student' (reference) bị xóa khỏi Stack
        // - Object trong Heap được Garbage Collector dọn dẹp (nếu không còn reference nào trỏ đến)
    }
}
```

**Sơ đồ bộ nhớ:**
```
Stack                    Heap
--------                 ------
number: 10
                          gpa: 3.5
                        }
```

### 🧠 Phân tích Dòng-chảy Bộ nhớ (Memory Trace)

Hãy xem chuyện gì xảy ra sau cánh gà khi đoạn code trên chạy:

| Dòng Code | Vùng Stack | Vùng Heap | Giải thích |
|-----------|------------|-----------|------------|
| `int number = 10;` | `number: 10` | (Trống) | `int` là primitive, lưu trực tiếp giá trị vào Stack. Nhanh gọn. |
| `new Student(...)` | (Chờ reference) | `Student object` <br> `{name="John", ...}` | Java tìm một chỗ trống trong "Nhà kho" Heap để xây object Student. |
| `Student student = ...` | `student: 0x1A2B` | (Như trên) | Java lấy địa chỉ nhà kho (`0x1A2B`) và ghi vào biến `student` trên Stack. |
| (Kết thúc method) | **Xóa sạch** | **Chờ dọn dẹp** | Stack tự hủy khi xong việc. Object trong Heap nằm chờ bác lao công (Garbage Collector) đến hốt. |


---

#### Reference (Tham chiếu)

**Reference** là địa chỉ trỏ đến object trong Heap.

**Ví dụ:**
```java
Student student1 = new Student("A", 20, 3.5);
Student student2 = student1;  // student2 trỏ đến cùng object với student1

student2.setName("B");
System.out.println(student1.getName());  // "B" - Vì cùng object!
```

**Phân tích:**
```
Stack                    Heap
--------                 ------
student1: [ref1] ───┐
                     └──> Student { name: "A" }
student2: [ref1] ────┘
```

**Kết luận:**
- `student1` và `student2` là 2 reference khác nhau
- Nhưng cả 2 đều trỏ đến cùng 1 object
- Thay đổi qua `student2` ảnh hưởng đến `student1`

**So sánh Object:**
```java
Student student1 = new Student("A", 20, 3.5);
Student student2 = new Student("A", 20, 3.5);

// So sánh reference (==)
System.out.println(student1 == student2);  // false - Khác object

// So sánh nội dung (cần override equals())
System.out.println(student1.equals(student2));  // true - Nếu đã override equals()
```

---

### 2.1.4. Vòng đời Đối tượng

#### Các giai đoạn

**1. Tạo (Creation)**
```java
Student student = new Student("John", 20, 3.5);
```
- Cấp phát bộ nhớ trong Heap
- Khởi tạo fields với giá trị mặc định
- Gọi constructor

**2. Sử dụng (Usage)**
```java
student.displayInfo();
student.setGpa(3.8);
double gpa = student.getGpa();
```
- Object được sử dụng thông qua reference

**3. Không còn tham chiếu (Unreferenced)**
```java
student = null;  // Reference không còn trỏ đến object
```
- Object vẫn tồn tại trong Heap
- Nhưng không thể truy cập được

**4. Garbage Collection (Thu gom rác)**
- Garbage Collector tự động dọn dẹp object không còn reference
- Không thể biết chính xác khi nào GC chạy

**Ví dụ đầy đủ:**
```java
public class ObjectLifecycle {
    public static void main(String[] args) {
        // 1. Tạo object
        Student student = new Student("John", 20, 3.5);
        
        // 2. Sử dụng
        student.displayInfo();
        
        // 3. Không còn reference
        student = null;
        
        // 4. Garbage Collector sẽ dọn dẹp (tự động, không cần code)
    }
}
```

---

### 2.1.5. Hợp đồng của Mọi Đối Tượng (The Object Contract)

Trong Java, mọi class bạn tạo ra đều ngầm định kế thừa từ class siêu tổ tiên: `java.lang.Object`.
Điều này có nghĩa là mỗi object đều sở hữu sẵn một số method cực kỳ quan trọng. Để code OOP đúng chuẩn, bạn **bắt buộc phải hiểu** và thường xuyên ghi đè (override) các method này.

#### 1. Method `toString()` - Định danh chuỗi
Mặc định, nếu bạn in một object `System.out.println(student)`, Java sẽ in ra một chuỗi khó hiểu như `Student@1a2b3c`.
Để hiển thị nội dung có ý nghĩa, bạn cần override `toString()`:

```java
public class Student {
    private String name;
    private int age;
    
    @Override
    public String toString() {
        return "Student{name='" + name + "', age=" + age + "}";
    }
}
// Kết quả: Student{name='John', age=20}
```

#### 2. Method `equals()` - Bằng nhau về mặt Logic
Toán tử `==` dùng để so sánh **địa chỉ vùng nhớ** (xem 2 biến có trỏ đến cùng 1 object trên Heap không).
Nhưng trong thực tế, 2 sinh viên được coi là "như nhau" nếu họ có cùng Mã Sinh Viên (ID), dù nằm ở 2 vùng nhớ khác nhau. Đó là lúc dùng `equals()`.

```java
Student s1 = new Student("123", "John");
Student s2 = new Student("123", "John");

System.out.println(s1 == s2);      // FALSE (2 object khác nhau trên Heap)
System.out.println(s1.equals(s2)); // FALSE nếu chưa override, TRUE nếu đã override
```

**Cách override `equals()` đúng chuẩn:**
```java
@Override
public boolean equals(Object obj) {
    if (this == obj) return true; // 1. Tối ưu: Nếu trỏ cùng vùng nhớ thì chắc chắn bằng
    if (obj == null || this.getClass() != obj.getClass()) return false; // 2. Kiểm tra null và khác kiểu
    Student student = (Student) obj; // 3. Ép kiểu
    return this.id.equals(student.id); // 4. So sánh logic (ID giống nhau là 1 người)
}
```

#### 3. Method `hashCode()` - Mã băm
**Quy tắc Vàng (The Contract):** Nếu 2 object bằng nhau theo `equals()`, thì chúng **bắt buộc** phải có `hashCode()` giống nhau.
`hashCode()` được sử dụng bởi các cấu trúc dữ liệu cực nhanh như `HashSet`, `HashMap` (Học ở Chương 6). Nếu bạn override `equals()` mà quên override `hashCode()`, cấu trúc dữ liệu sẽ chạy sai hoàn toàn!

```java
@Override
public int hashCode() {
    return Objects.hash(id); // Tạo mã băm dựa trên ID
}
```
> **💡 Lời khuyên của Giáo sư:** Luôn để IDE (IntelliJ, Eclipse, VSCode) tự động generate `equals()` và `hashCode()` cho bạn. Đừng tự viết tay vì rất dễ sai sót!

---

### 2.1.6. Sao chép Đối tượng: Shallow Copy vs Deep Copy

Khi bạn gán `student2 = student1`, bạn KHÔNG hề tạo ra object mới. Bạn chỉ tạo thêm một biến reference trỏ vào cùng một chỗ.
Vậy nếu muốn "nhân bản" (Clone) một object thì sao?

#### 1. Shallow Copy (Sao chép Nông)
Là việc tạo ra object mới, copy toàn bộ giá trị primitive sang, nhưng với **reference type**, nó chỉ copy địa chỉ vùng nhớ.

```java
public class Student implements Cloneable {
    String name;
    Address address; // Reference type
    
    @Override
    protected Object clone() throws CloneNotSupportedException {
        return super.clone(); // Shallow Copy mặc định của Java
    }
}
```
**Hậu quả:** Nếu bạn sửa `address` của bản clone, bản gốc cũng bị đổi theo! (Vì dùng chung object Address).

#### 2. Deep Copy (Sao chép Sâu)
Tạo ra object mới hoàn toàn, và đệ quy tạo mới LUÔN cả các object bên trong nó.

```java
@Override
protected Object clone() throws CloneNotSupportedException {
    Student cloned = (Student) super.clone();
    cloned.address = new Address(this.address.getCity()); // Deep copy: Tạo hẳn Address mới!
    return cloned;
}
```
> **💡 Lời khuyên của Giáo sư:** Kỹ thuật `Cloneable` của Java khá cũ và dễ lỗi. Ngày nay, để Deep Copy, lập trình viên thường dùng **Copy Constructor** (tạo hàm constructor nhận vào chính object đó rồi copy từng thuộc tính), hoặc dùng thư viện JSON (như Gson/Jackson) để chuyển Object thành chuỗi JSON rồi parse ngược lại thành Object mới.

---

## 2.2. ĐÓNG GÓI (ENCAPSULATION)

### 2.2.1. Encapsulation là gì?

#### Định nghĩa

**Encapsulation (Đóng gói)** là nguyên lý:
- **Gom dữ liệu và hành vi** lại với nhau trong một class
- **Che giấu chi tiết bên trong**, chỉ expose những gì cần thiết
- **Bảo vệ dữ liệu** khỏi truy cập trái phép

#### Ví dụ thực tế

**Tưởng tượng một chiếc xe:**
- Bạn không cần biết động cơ hoạt động thế nào bên trong
- Chỉ cần biết: Nhấn ga → Xe chạy
- Động cơ được "đóng gói" bên trong, không thể truy cập trực tiếp

**Trong lập trình:**
```java
public class BankAccount {
    private double balance;  // Che giấu - không thể truy cập trực tiếp
    
    public void deposit(double amount) {  // Expose - có thể sử dụng
        if (amount > 0) {
            balance += amount;
        }
    }
    
    public double getBalance() {  // Expose - có thể xem
        return balance;
    }
}
```

**Lợi ích:**
- ✅ Kiểm soát truy cập (validation trong `deposit()`)
- ✅ Dễ thay đổi implementation (đổi cách lưu `balance` không ảnh hưởng code bên ngoài)
- ✅ Bảo mật (không thể set `balance` trực tiếp)

---

### 2.2.2. Access Modifiers (Các mức Truy cập)

#### 4 mức truy cập trong Java

| Modifier | Trong class | Trong package | Trong subclass | Mọi nơi |
|----------|-------------|---------------|----------------|---------|
| `private` | ✅ | ❌ | ❌ | ❌ |
| `default` (không có) | ✅ | ✅ | ❌ | ❌ |
| `protected` | ✅ | ✅ | ✅ | ❌ |
| `public` | ✅ | ✅ | ✅ | ✅ |

> **🔒 Tưởng tượng: Mức độ riêng tư trong đời sống**
> - `private`: **Nhật ký cá nhân**. Chỉ mình bạn đọc được.
> - `default`: **Giấy note trên tủ lạnh**. Chỉ gia đình (cùng Package) đọc được.
> - `protected`: **Gia sản dòng họ**. Gia đình và con cháu (Subclass) được dùng.
> - `public`: **Biển quảng cáo ngoài trời**. Ai đi qua cũng thấy.

#### 1. private

**Chỉ truy cập được trong cùng class**

```java
public class Student {
    private String name;  // Chỉ truy cập trong class Student
    
    private void secretMethod() {  // Chỉ gọi trong class Student
        // ...
    }
    
    public void publicMethod() {
        name = "John";  // ✅ OK - Trong cùng class
        secretMethod();  // ✅ OK - Trong cùng class
    }
}

// Ở class khác
public class Main {
    public static void main(String[] args) {
        Student student = new Student();
        // student.name = "John";  // ❌ LỖI - Không thể truy cập private
        // student.secretMethod();  // ❌ LỖI - Không thể gọi private method
    }
}
```

**Khi nào dùng `private`:**
- Fields (thuộc tính) - Hầu hết trường hợp
- Methods chỉ dùng nội bộ trong class

---

#### 2. default (Package-private)

**Truy cập được trong cùng package**

```java
// File: com.example.Student.java
package com.example;

class Student {  // default - không có modifier
    String name;  // default
    
    void display() {  // default
        System.out.println(name);
    }
}

// File: com.example.Main.java (cùng package)
package com.example;

public class Main {
    public static void main(String[] args) {
        Student student = new Student();
        student.name = "John";  // ✅ OK - Cùng package
        student.display();  // ✅ OK - Cùng package
    }
}

// File: com.other.Main.java (package khác)
package com.other;

import com.example.Student;

public class Main {
    public static void main(String[] args) {
        Student student = new Student();
        // student.name = "John";  // ❌ LỖI - Khác package
        // student.display();  // ❌ LỖI - Khác package
    }
}
```

**Khi nào dùng `default`:**
- Classes/methods chỉ dùng trong package
- Ít dùng, thường dùng `private` hoặc `public`

---

#### 3. protected

**Truy cập được trong cùng package và subclass**

```java
package com.example;

public class Animal {
    protected String name;  // protected
    
    protected void eat() {  // protected
        System.out.println(name + " is eating");
    }
}

// File: com.example.Dog.java (cùng package)
package com.example;

public class Dog extends Animal {
    public void bark() {
        name = "Buddy";  // ✅ OK - Subclass
        eat();  // ✅ OK - Subclass
    }
}

// File: com.other.Cat.java (package khác, nhưng extends Animal)
package com.other;

import com.example.Animal;

public class Cat extends Animal {
    public void meow() {
        name = "Fluffy";  // ✅ OK - Subclass (dù khác package)
        eat();  // ✅ OK - Subclass
    }
}

// File: com.other.Main.java (không phải subclass)
package com.other;

import com.example.Animal;

public class Main {
    public static void main(String[] args) {
        Animal animal = new Animal();
        // animal.name = "Test";  // ❌ LỖI - Không phải subclass
        // animal.eat();  // ❌ LỖI - Không phải subclass
    }
}
```

**Khi nào dùng `protected`:**
- Fields/methods cần cho subclass nhưng không public
- Thường dùng trong kế thừa

---

#### 4. public

**Truy cập được mọi nơi**

```java
public class Student {
    public String name;  // public - ai cũng truy cập được
    
    public void display() {  // public - ai cũng gọi được
        System.out.println(name);
    }
}
```

**Khi nào dùng `public`:**
- API công khai (methods cần cho người dùng class)
- Ít dùng cho fields (thường dùng `private` + getter/setter)

---

### 2.2.3. Data Hiding (Che giấu Dữ liệu)

#### Tại sao cần Data Hiding?

**Vấn đề không che giấu:**
```java
public class BankAccount {
    public double balance;  // ❌ Nguy hiểm!
}

// Ở nơi khác
BankAccount account = new BankAccount();
account.balance = -1000;  // ❌ Có thể set số âm - Sai logic!
account.balance = 999999999;  // ❌ Có thể hack - Không kiểm soát!
```

**Giải pháp: Che giấu + Validation**
```java
public class BankAccount {
    private double balance;  // ✅ Che giấu
    
    public void deposit(double amount) {
        if (amount > 0) {  // ✅ Validation
            balance += amount;
        } else {
            throw new IllegalArgumentException("Amount must be positive");
        }
    }
    
    public void withdraw(double amount) {
        if (amount > 0 && amount <= balance) {  // ✅ Validation
            balance -= amount;
        } else {
            throw new IllegalArgumentException("Invalid amount");
        }
    }
    
    public double getBalance() {  // ✅ Chỉ xem, không sửa
        return balance;
    }
}
```

**Lợi ích:**
- ✅ Kiểm soát truy cập (validation)
- ✅ Bảo vệ tính toàn vẹn dữ liệu
- ✅ Dễ thay đổi implementation (đổi cách lưu `balance` không ảnh hưởng code bên ngoài)

---

#### Best Practice: Luôn dùng private cho Fields

```java
// ❌ TỒI
public class Student {
    public String name;
    public int age;
}

// ✅ TỐT
public class Student {
    private String name;
    private int age;
    
    // Expose qua getters/setters nếu cần
    public String getName() { return name; }
    public void setName(String name) { this.name = name; }
}
```

---

## 2.3. CONSTRUCTORS, GETTERS/SETTERS & TỪ KHÓA THIS

### 2.3.1. Constructor (Hàm khởi tạo)

#### Constructor là gì?

**Constructor** là method đặc biệt:
- Tên trùng với tên class
- Không có return type (không phải `void`)
- Tự động gọi khi tạo object bằng `new`
- Dùng để khởi tạo giá trị ban đầu cho object

#### Cú pháp

```java
[access modifier] ClassName(parameters) {
    // Khởi tạo
}
```

#### Ví dụ

```java
public class Student {
    private String name;
    private int age;
    private double gpa;
    
    // Constructor
    public Student(String name, int age, double gpa) {
        this.name = name;
        this.age = age;
        this.gpa = gpa;
    }
}

// Sử dụng
Student student = new Student("John", 20, 3.5);
```

---

#### Default Constructor

**Nếu không định nghĩa constructor, Java tự tạo default constructor:**

```java
public class Student {
    private String name;
    // Không có constructor nào
}

// Java tự tạo constructor mặc định (ẩn):
// public Student() { }

// Có thể dùng:
Student student = new Student();  // ✅ OK
```

**Lưu ý:**
- Default constructor chỉ khởi tạo fields với giá trị mặc định:
  - `int`, `double`, `float`, `long`, `byte`, `short`, `char` → `0` hoặc `'\u0000'`
  - `boolean` → `false`
  - Object references → `null`

```java
public class Student {
    private String name;  // null
    private int age;     // 0
    private double gpa;  // 0.0
    private boolean active;  // false
}

Student student = new Student();
// name = null, age = 0, gpa = 0.0, active = false
```

---

#### Constructor Overloading

**Có thể định nghĩa nhiều constructor với tham số khác nhau:**

```java
public class Student {
    private String name;
    private int age;
    private double gpa;
    
    // Constructor 1: Đầy đủ tham số
    public Student(String name, int age, double gpa) {
        this.name = name;
        this.age = age;
        this.gpa = gpa;
    }
    
    // Constructor 2: Chỉ name và age
    public Student(String name, int age) {
        this.name = name;
        this.age = age;
        this.gpa = 0.0;  // Mặc định
    }
    
    // Constructor 3: Chỉ name
    public Student(String name) {
        this.name = name;
        this.age = 0;  // Mặc định
        this.gpa = 0.0;  // Mặc định
    }
    
    // Constructor 4: Không tham số
    public Student() {
        this.name = "Unknown";
        this.age = 0;
        this.gpa = 0.0;
    }
}

// Sử dụng
Student s1 = new Student("John", 20, 3.5);  // Constructor 1
Student s2 = new Student("Jane", 21);       // Constructor 2
Student s3 = new Student("Bob");             // Constructor 3
Student s4 = new Student();                  // Constructor 4
```

**Lưu ý:**
- Các constructor phải khác nhau về số lượng hoặc kiểu tham số
- Không thể có 2 constructor giống hệt nhau

---

#### Constructor Chaining (Gọi constructor khác)

**Dùng `this()` để gọi constructor khác trong cùng class:**

```java
public class Student {
    private String name;
    private int age;
    private double gpa;
    
    // Constructor đầy đủ
    public Student(String name, int age, double gpa) {
        this.name = name;
        this.age = age;
        this.gpa = gpa;
    }
    
    // Constructor gọi constructor đầy đủ
    public Student(String name, int age) {
        this(name, age, 0.0);  // Gọi constructor trên
    }
    
    // Constructor gọi constructor 2 tham số
    public Student(String name) {
        this(name, 0);  // Gọi constructor 2 tham số
    }
    
    // Constructor không tham số
    public Student() {
        this("Unknown");  // Gọi constructor 1 tham số
    }
}
```

### 🏃 Phân tích Luồng chạy (Execution Flow)

Khi bạn gọi `new Student()`, nó giống như một cuộc **chạy tiếp sức**:

1.  **Bước 1**: Bạn gọi `new Student()` (Không tham số).
2.  **Bước 2**: Constructor này gọi `this("Unknown")`. Nó chưa làm gì cả, chuyển ngay gậy cho đồng đội.
3.  **Bước 3**: Constructor 1 tham số nhận `name="Unknown"`. Nó lại gọi `this("Unknown", 0)`. Lại chuyển gậy tiếp!
4.  **Bước 4**: Constructor 2 tham số nhận `age=0`. Nó gọi `this("Unknown", 0, 0.0)`. Chuyển gậy lần cuối.
5.  **Bước 5**: Constructor đầy đủ (3 tham số) nhận gậy. Giờ nó mới thực sự làm việc: gán `this.name`, `this.age`, `this.gpa`.
6.  **Bước 6**: Xong việc!

> **Tại sao lại làm thế (The Why)?**
> Thay vì copy-paste code gán giá trị ở cả 4 constructor (DRY - Don't Repeat Yourself), chúng ta dồn hết logic vào **một constructor chính** (Master Constructor). Các constructor khác chỉ là "cò mồi" (convenience constructors) để gọi về constructor chính với giá trị mặc định. Code gọn hơn, sửa lỗi dễ hơn (chỉ cần sửa 1 chỗ).


**Lưu ý:**
- `this()` phải là dòng đầu tiên trong constructor
- Không thể gọi `this()` và `super()` cùng lúc

---

### 2.3.2. Từ khóa this

#### this là gì?

**`this`** là reference trỏ đến object hiện tại (object đang thực thi method/constructor).

#### Các cách sử dụng this

**1. Phân biệt field và parameter cùng tên:**
```java
public class Student {
    private String name;
    
    public void setName(String name) {
        this.name = name;  // this.name = field, name = parameter
    }
}
```

**2. Gọi constructor khác:**
```java
public Student(String name) {
    this(name, 0);  // Gọi constructor khác
}
```

**3. Truyền object hiện tại làm tham số:**
```java
public class Student {
    public void register(Course course) {
        course.addStudent(this);  // Truyền chính object này
    }
}
```

**4. Return object hiện tại (Method chaining):**
```java
public class Student {
    private String name;
    private int age;
    
    public Student setName(String name) {
        this.name = name;
        return this;  // Return chính object này
    }
    
    public Student setAge(int age) {
        this.age = age;
        return this;
    }
}

// Sử dụng method chaining
Student student = new Student()
    .setName("John")
    .setAge(20);
```

---

### 2.3.3. Getters và Setters

#### Tại sao cần Getters/Setters?

**Vấn đề:**
- Fields `private` → Không thể truy cập từ bên ngoài
- Cần cách để đọc/ghi giá trị

**Giải pháp:**
- **Getter**: Method để đọc giá trị field
- **Setter**: Method để ghi giá trị field

---

#### Getter (Accessor)

**Cú pháp:**
```java
public returnType getFieldName() {
    return fieldName;
}
```

**Ví dụ:**
```java
public class Student {
    private String name;
    private int age;
    private double gpa;
    
    // Getter cho name
    public String getName() {
        return name;
    }
    
    // Getter cho age
    public int getAge() {
        return age;
    }
    
    // Getter cho gpa
    public double getGpa() {
        return gpa;
    }
}

// Sử dụng
Student student = new Student("John", 20, 3.5);
String name = student.getName();  // "John"
int age = student.getAge();       // 20
double gpa = student.getGpa();     // 3.5
```

**Quy ước:**
- Tên method: `get` + Tên field (viết hoa chữ cái đầu)
- Boolean: Có thể dùng `is` thay vì `get`
  ```java
  private boolean active;
  
  public boolean isActive() {  // Thay vì getActive()
      return active;
  }
  ```

---

#### Setter (Mutator)

**Cú pháp:**
```java
public void setFieldName(dataType fieldName) {
    this.fieldName = fieldName;
}
```

**Ví dụ:**
```java
public class Student {
    private String name;
    private int age;
    private double gpa;
    
    // Setter cho name
    public void setName(String name) {
        this.name = name;
    }
    
    // Setter cho age (có validation)
    public void setAge(int age) {
        if (age >= 0 && age <= 150) {
            this.age = age;
        } else {
            throw new IllegalArgumentException("Age must be between 0 and 150");
        }
    }
    
    // Setter cho gpa (có validation)
    public void setGpa(double gpa) {
        if (gpa >= 0.0 && gpa <= 4.0) {
            this.gpa = gpa;
        } else {
            throw new IllegalArgumentException("GPA must be between 0.0 and 4.0");
        }
    }
}

// Sử dụng
Student student = new Student();
student.setName("John");
student.setAge(20);
student.setGpa(3.5);
```

**Quy ước:**
- Tên method: `set` + Tên field (viết hoa chữ cái đầu)
- Nên có validation trong setter

---

#### Quy tắc thiết kế Getter/Setter thông minh

**1. Không phải field nào cũng cần setter:**
```java
public class Student {
    private String id;  // ID không thể thay đổi
    private String name;
    private Date createdAt;  // Ngày tạo không thể thay đổi
    
    // ✅ Có getter, không có setter
    public String getId() {
        return id;
    }
    
    // ✅ Có getter, không có setter
    public Date getCreatedAt() {
        return createdAt;
    }
    
    // ✅ Có cả getter và setter
    public String getName() { return name; }
    public void setName(String name) { this.name = name; }
}
```

**2. Setter có validation:**
```java
public void setAge(int age) {
    if (age < 0) {
        throw new IllegalArgumentException("Age cannot be negative");
    }
    this.age = age;
}
```

**3. Tránh lộ tính đóng gói:**
```java
// ❌ TỒI: Trả về reference trực tiếp
public List<String> getCourses() {
    return courses;  // Nguy hiểm! Có thể modify từ bên ngoài
}

// ✅ TỐT: Trả về copy hoặc unmodifiable
public List<String> getCourses() {
    return new ArrayList<>(courses);  // Trả về copy
    // Hoặc
    // return Collections.unmodifiableList(courses);
}
```

**4. Immutable objects: Không có setter**
```java
public class Point {
    private final int x;
    private final int y;
    
    public Point(int x, int y) {
        this.x = x;
        this.y = y;
    }
    
    // Chỉ có getter, không có setter
    public int getX() { return x; }
    public int getY() { return y; }
}
```

---

## 2.4. JAVA RECORDS (JAVA 14+) – DATA CLASS HIỆN ĐẠI

### 2.4.1. Record là gì?

#### Định nghĩa

**Record** là một loại class đặc biệt trong Java (từ Java 14):
- Dùng để tạo **immutable data classes**
- Tự động generate: constructor, getters, `equals()`, `hashCode()`, `toString()`
- Giảm thiểu boilerplate code

#### Vấn đề với POJO truyền thống

**POJO (Plain Old Java Object) truyền thống:**
```java
public class Point {
    private final int x;
    private final int y;
    
    public Point(int x, int y) {
        this.x = x;
        this.y = y;
    }
    
    public int getX() {
        return x;
    }
    
    public int getY() {
        return y;
    }
    
    @Override
    public boolean equals(Object o) {
        if (this == o) return true;
        if (o == null || getClass() != o.getClass()) return false;
        Point point = (Point) o;
        return x == point.x && y == point.y;
    }
    
    @Override
    public int hashCode() {
        return Objects.hash(x, y);
    }
    
    @Override
    public String toString() {
        return "Point{" +
                "x=" + x +
                ", y=" + y +
                '}';
    }
}
```

**Vấn đề:**
- ❌ Quá nhiều code lặp lại (boilerplate)
- ❌ Dễ quên override `equals()`, `hashCode()`
- ❌ Khó maintain

---

#### Giải pháp: Java Record

**Với Record:**
```java
public record Point(int x, int y) {
    // Chỉ cần 1 dòng!
}
```

**Tự động có:**
- ✅ Constructor: `Point(int x, int y)`
- ✅ Getters: `x()`, `y()` (không phải `getX()`, `getY()`)
- ✅ `equals()` và `hashCode()`
- ✅ `toString()`

**Sử dụng:**
```java
Point p1 = new Point(10, 20);
Point p2 = new Point(10, 20);

System.out.println(p1.x());      // 10
System.out.println(p1.y());     // 20
System.out.println(p1.equals(p2));  // true
System.out.println(p1);          // Point[x=10, y=20]
```

---

### 2.4.2. Cú pháp Record

#### Cú pháp cơ bản

```java
public record RecordName(type1 field1, type2 field2, ...) {
    // Body (tùy chọn)
}
```

**Ví dụ:**
```java
// Record đơn giản
public record Student(String name, int age, double gpa) {
}

// Sử dụng
Student student = new Student("John", 20, 3.5);
System.out.println(student.name());  // "John"
System.out.println(student.age());   // 20
System.out.println(student.gpa());   // 3.5
```

---

#### Record với Validation

**Có thể thêm constructor tùy chỉnh để validation:**
```java
public record Student(String name, int age, double gpa) {
    // Compact constructor (Java 14+)
    public Student {
        if (age < 0 || age > 150) {
            throw new IllegalArgumentException("Age must be between 0 and 150");
        }
        if (gpa < 0.0 || gpa > 4.0) {
            throw new IllegalArgumentException("GPA must be between 0.0 and 4.0");
        }
        // Không cần gán: Java tự động gán sau validation
    }
}

// Sử dụng
Student student = new Student("John", 20, 3.5);  // ✅ OK
Student invalid = new Student("John", -5, 3.5);  // ❌ Exception
```

---

#### Record với Methods

**Có thể thêm methods:**
```java
public record Point(int x, int y) {
    // Instance method
    public double distanceToOrigin() {
        return Math.sqrt(x * x + y * y);
    }
    
    // Static method
    public static Point origin() {
        return new Point(0, 0);
    }
}

// Sử dụng
Point p = new Point(3, 4);
double distance = p.distanceToOrigin();  // 5.0
Point origin = Point.origin();  // Point[x=0, y=0]
```

---

### 2.4.3. So sánh Record vs POJO

| Đặc điểm | POJO | Record |
|----------|------|--------|
| **Code** | Nhiều dòng | Ít dòng |
| **Immutable** | Phải tự implement | Mặc định |
| **Getters** | `getX()`, `getY()` | `x()`, `y()` |
| **equals/hashCode** | Phải tự override | Tự động |
| **toString** | Phải tự override | Tự động |
| **Kế thừa** | Có thể extends | Không thể extends (final) |
| **Thêm fields** | Dễ | Không thể (immutable) |

**Khi nào dùng Record:**
- ✅ Data classes đơn giản
- ✅ Immutable objects
- ✅ DTOs (Data Transfer Objects)
- ✅ Value objects

**Khi nào dùng POJO:**
- ✅ Cần kế thừa
- ✅ Cần mutable (có thể thay đổi)
- ✅ Cần logic phức tạp

---

## 2.5. IMMUTABLE OBJECTS – ĐỐI TƯỢNG BẤT BIẾN

### 2.5.1. Immutable là gì?

#### Định nghĩa

**Immutable Object** là object không thể thay đổi sau khi được tạo:
- Không thể thay đổi giá trị fields
- Nếu cần "thay đổi", phải tạo object mới

#### Ví dụ thực tế

**String trong Java là immutable:**
```java
String str = "Hello";
str.toUpperCase();  // Trả về "HELLO" mới, không thay đổi str
System.out.println(str);  // Vẫn là "Hello"

// Để "thay đổi", phải gán lại
str = str.toUpperCase();  // Tạo object mới
System.out.println(str);  // "HELLO"
```

---

### 2.5.2. Lợi ích của Immutable Objects

#### 1. Thread Safety (An toàn đa luồng)

**Vấn đề với Mutable:**
```java
// Mutable - Nguy hiểm trong đa luồng
public class Counter {
    private int count;
    
    public void increment() {
        count++;  // Race condition!
    }
}

// Nhiều thread cùng increment → Kết quả sai
```

**Giải pháp với Immutable:**
```java
// Immutable - An toàn
public record Counter(int count) {
    public Counter increment() {
        return new Counter(count + 1);  // Tạo object mới
    }
}

// Mỗi thread có object riêng → An toàn
```

---

#### 2. Dễ hiểu và Debug

**Mutable - Khó debug:**
```java
Point p = new Point(10, 20);
someMethod(p);  // p có thể bị thay đổi bên trong
System.out.println(p);  // Không biết giá trị là gì
```

**Immutable - Dễ debug:**
```java
Point p = new Point(10, 20);
someMethod(p);  // p không thể thay đổi
System.out.println(p);  // Vẫn là (10, 20)
```

---

#### 3. Có thể dùng làm Key trong Map

**Mutable - Nguy hiểm:**
```java
Map<Point, String> map = new HashMap<>();
Point p = new Point(10, 20);
map.put(p, "Value");

p.setX(30);  // Thay đổi key
map.get(p);  // ❌ Không tìm thấy! (hashCode đã thay đổi)
```

**Immutable - An toàn:**
```java
Map<Point, String> map = new HashMap<>();
Point p = new Point(10, 20);
map.put(p, "Value");

// p không thể thay đổi → Key luôn đúng
map.get(p);  // ✅ Tìm thấy
```

---

### 2.5.3. Kỹ thuật thiết kế Immutable Class

#### Quy tắc thiết kế

**1. Tất cả fields là `final` và `private`:**
```java
public class ImmutablePoint {
    private final int x;
    private final int y;
    
    public ImmutablePoint(int x, int y) {
        this.x = x;
        this.y = y;
    }
    
    // Chỉ có getters, không có setters
    public int getX() { return x; }
    public int getY() { return y; }
}
```

**2. Không có setter:**
```java
// ❌ Không có setter
// public void setX(int x) { this.x = x; }
```

**3. Không cho phép subclass override:**
```java
// Class phải là final
public final class ImmutablePoint {
    // ...
}
```

**4. Không expose mutable objects:**
```java
public class ImmutableStudent {
    private final String name;
    private final List<String> courses;  // Mutable!
    
    public ImmutableStudent(String name, List<String> courses) {
        this.name = name;
        // ❌ TỒI: Trực tiếp gán
        // this.courses = courses;  // Có thể modify từ bên ngoài
        
        // ✅ TỐT: Tạo copy
        this.courses = new ArrayList<>(courses);
    }
    
    public List<String> getCourses() {
        // ❌ TỒI: Trả về reference trực tiếp
        // return courses;
        
        // ✅ TỐT: Trả về unmodifiable hoặc copy
        return Collections.unmodifiableList(courses);
        // Hoặc: return new ArrayList<>(courses);
    }
}
```

---

#### Ví dụ đầy đủ: Immutable Class

```java
import java.util.ArrayList;
import java.util.Collections;
import java.util.List;

public final class ImmutableStudent {
    private final String name;
    private final int age;
    private final List<String> courses;
    
    public ImmutableStudent(String name, int age, List<String> courses) {
        this.name = name;
        this.age = age;
        // Tạo copy để bảo vệ
        this.courses = new ArrayList<>(courses);
    }
    
    // Chỉ có getters
    public String getName() {
        return name;
    }
    
    public int getAge() {
        return age;
    }
    
    // Trả về unmodifiable list
    public List<String> getCourses() {
        return Collections.unmodifiableList(courses);
    }
    
    // Method để "thay đổi" - tạo object mới
    public ImmutableStudent withName(String newName) {
        return new ImmutableStudent(newName, age, courses);
    }
    
    public ImmutableStudent withAge(int newAge) {
        return new ImmutableStudent(name, newAge, courses);
    }
    
    public ImmutableStudent addCourse(String course) {
        List<String> newCourses = new ArrayList<>(courses);
        newCourses.add(course);
        return new ImmutableStudent(name, age, newCourses);
    }
    
    @Override
    public String toString() {
        return "ImmutableStudent{" +
                "name='" + name + '\'' +
                ", age=" + age +
                ", courses=" + courses +
                '}';
    }
}
```

**Sử dụng:**
```java
List<String> courses = new ArrayList<>();
courses.add("Math");
courses.add("Physics");

ImmutableStudent student = new ImmutableStudent("John", 20, courses);

// Thêm course mới - tạo object mới
ImmutableStudent updated = student.addCourse("Chemistry");

System.out.println(student);   // Vẫn giữ nguyên
System.out.println(updated);   // Có thêm Chemistry
```

---

#### So sánh: Record vs Immutable Class

**Record (Tự động immutable):**
```java
public record Point(int x, int y) {
    public Point add(Point other) {
        return new Point(x + other.x, y + other.y);
    }
}
```

**Immutable Class (Tự implement):**
```java
public final class Point {
    private final int x;
    private final int y;
    
    public Point(int x, int y) {
        this.x = x;
        this.y = y;
    }
    
    public int getX() { return x; }
    public int getY() { return y; }
    
    public Point add(Point other) {
        return new Point(x + other.x, y + other.y);
    }
}
```

**Kết luận:**
- **Record**: Đơn giản hơn, tự động immutable
- **Immutable Class**: Linh hoạt hơn, có thể thêm logic phức tạp

---

## 2.6. STATIC MEMBERS – THÀNH VIÊN TĨNH

### 2.6.1. Static là gì?

#### Định nghĩa

**`static`** là từ khóa đánh dấu thành viên (field hoặc method) thuộc về **class**, không thuộc về **object**.

**Đặc điểm:**
- Dùng chung cho tất cả objects của class
- Truy cập qua tên class (không cần tạo object)
- Chỉ có 1 bản copy duy nhất trong bộ nhớ

---

### 2.6.2. Static Variables (Biến tĩnh)

#### Ví dụ

```java
public class Student {
    // Instance variable - mỗi object có bản riêng
    private String name;
    private int age;
    
    // Static variable - dùng chung cho tất cả objects
    private static int totalStudents = 0;
    
    public Student(String name, int age) {
        this.name = name;
        this.age = age;
        totalStudents++;  // Tăng mỗi khi tạo object mới
    }
    
    // Static method để truy cập static variable
    public static int getTotalStudents() {
        return totalStudents;
    }
}

// Sử dụng
Student s1 = new Student("Alice", 20);
Student s2 = new Student("Bob", 21);
Student s3 = new Student("Charlie", 22);

System.out.println(Student.getTotalStudents());  // 3
// Tất cả objects dùng chung totalStudents
```

**Phân tích:**
- `totalStudents` là static → Chỉ có 1 bản copy
- Tất cả objects cùng class dùng chung
- Truy cập qua `Student.getTotalStudents()` (không cần object)

---

#### So sánh Static vs Instance Variable

| Đặc điểm | Instance Variable | Static Variable |
|----------|------------------|----------------|
| **Số lượng** | Mỗi object 1 bản copy | Chỉ 1 bản copy cho cả class |
| **Truy cập** | Qua object: `obj.field` | Qua class: `Class.field` |
| **Tồn tại** | Suốt đời object | Suốt đời chương trình |
| **Khi nào dùng** | Dữ liệu riêng của object | Dữ liệu dùng chung |

---

### 2.6.3. Static Methods (Phương thức tĩnh)

#### Ví dụ

```java
public class MathUtils {
    // Static method - không cần object
    public static int add(int a, int b) {
        return a + b;
    }
    
    public static double calculateCircleArea(double radius) {
        return Math.PI * radius * radius;
    }
    
    // Instance method - cần object
    private int value;
    
    public void setValue(int value) {
        this.value = value;
    }
}

// Sử dụng static method
int sum = MathUtils.add(5, 3);  // Không cần new MathUtils()
double area = MathUtils.calculateCircleArea(5.0);

// Sử dụng instance method
MathUtils util = new MathUtils();
util.setValue(10);  // Cần object
```

---

#### Quy tắc Static Methods

**1. Không thể truy cập instance variables:**
```java
public class Student {
    private String name;  // Instance variable
    
    public static void printName() {
        // System.out.println(name);  // ❌ LỖI! Không thể truy cập instance variable
    }
}
```

**2. Không thể dùng `this`:**
```java
public class Student {
    public static void method() {
        // this.name = "John";  // ❌ LỖI! this không tồn tại trong static method
    }
}
```

**3. Chỉ có thể truy cập static members:**
```java
public class Student {
    private static int count = 0;  // Static variable
    private String name;             // Instance variable
    
    public static void incrementCount() {
        count++;  // ✅ OK - Truy cập static variable
        // name = "John";  // ❌ LỖI - Không thể truy cập instance variable
    }
}
```

---

### 2.6.4. Static Block

#### Định nghĩa

**Static block** là block code chạy **một lần duy nhất** khi class được load vào memory.

**Cú pháp:**
```java
static {
    // Code chạy khi class được load
}
```

**Ví dụ:**
```java
public class DatabaseConnection {
    private static String connectionString;
    
    // Static block - chạy trước constructor
    static {
        System.out.println("Loading database configuration...");
        connectionString = "jdbc:mysql://localhost:3306/mydb";
        // Có thể đọc từ file, environment variables, ...
    }
    
    public DatabaseConnection() {
        System.out.println("Creating connection with: " + connectionString);
    }
}

// Sử dụng
DatabaseConnection conn1 = new DatabaseConnection();
// Output:
// Loading database configuration...
// Creating connection with: jdbc:mysql://localhost:3306/mydb

DatabaseConnection conn2 = new DatabaseConnection();
// Output:
// Creating connection with: jdbc:mysql://localhost:3306/mydb
// (Static block chỉ chạy 1 lần)
```

---

### 2.6.5. Khi nào dùng Static?

#### ✅ NÊN DÙNG Static khi:

**1. Utility Methods (Phương thức tiện ích):**
```java
public class StringUtils {
    public static boolean isEmpty(String str) {
        return str == null || str.trim().isEmpty();
    }
    
    public static String capitalize(String str) {
        if (isEmpty(str)) return str;
        return str.substring(0, 1).toUpperCase() + str.substring(1);
    }
}

// Sử dụng
if (StringUtils.isEmpty(name)) {
    // ...
}
```

**2. Constants (Hằng số):**
```java
public class Constants {
    public static final double PI = 3.14159;
    public static final int MAX_STUDENTS = 100;
    public static final String DEFAULT_NAME = "Unknown";
}

// Sử dụng
double area = Constants.PI * radius * radius;
```

**3. Counter/Shared State (Đếm/Dữ liệu dùng chung):**
```java
public class Student {
    private static int nextId = 1;  // ID tự động tăng
    
    private int id;
    private String name;
    
    public Student(String name) {
        this.id = nextId++;
        this.name = name;
    }
}
```

**4. Factory Methods:**
```java
public class Student {
    private String name;
    
    private Student(String name) {
        this.name = name;
    }
    
    // Static factory method
    public static Student createWithDefaultName() {
        return new Student("Unknown");
    }
    
    public static Student createWithName(String name) {
        return new Student(name);
    }
}
```

---

#### ❌ KHÔNG NÊN DÙNG Static khi:

**1. Cần state riêng cho mỗi object:**
```java
// ❌ TỒI
public class Student {
    private static String name;  // Tất cả students cùng tên!
}

// ✅ TỐT
public class Student {
    private String name;  // Mỗi student có tên riêng
}
```

**2. Cần override trong subclass:**
```java
// ❌ Static methods không thể override
public class Animal {
    public static void makeSound() { }
}

public class Dog extends Animal {
    // Không phải override, chỉ là method mới
    public static void makeSound() { }
}
```

---

### 2.6.6. Ví dụ Tổng hợp

```java
public class BankAccount {
    // Static variable - đếm số tài khoản
    private static int accountCount = 0;
    
    // Static constant
    private static final double INTEREST_RATE = 0.05;
    
    // Instance variables
    private int accountNumber;
    private double balance;
    
    // Static block - khởi tạo
    static {
        System.out.println("BankAccount class loaded");
        // Có thể load config, connect database, ...
    }
    
    // Constructor
    public BankAccount(double initialBalance) {
        accountCount++;
        this.accountNumber = accountCount;
        this.balance = initialBalance;
    }
    
    // Static method - tính lãi suất
    public static double calculateInterest(double amount) {
        return amount * INTEREST_RATE;
    }
    
    // Static method - lấy số lượng tài khoản
    public static int getAccountCount() {
        return accountCount;
    }
    
    // Instance method
    public void deposit(double amount) {
        balance += amount;
    }
    
    public double getBalance() {
        return balance;
    }
}

// Sử dụng
System.out.println("Interest for 1000: " + BankAccount.calculateInterest(1000));  // 50.0

BankAccount acc1 = new BankAccount(1000);
BankAccount acc2 = new BankAccount(2000);

System.out.println("Total accounts: " + BankAccount.getAccountCount());  // 2
```

---

> **💡 Liên kết với Chương 1:**
> - Bạn đã học về `static` method cơ bản ở **Chương 1, phần 0.8**
> - Bây giờ học sâu hơn về static variables, static blocks, và khi nào nên dùng

---

## 🔗 CẦU NỐI SANG CHƯƠNG 3

Chương 2 dạy **một class độc lập**. Chương 3 dạy **nhiều class liên quan** (cha–con, interface):

| Kiến thức Chương 2 | Dùng ở Chương 3 | Ví dụ |
|--------------------|-----------------|-------|
| `protected` | Subclass truy cập field/method của cha | `protected String name` trong Animal |
| Constructor + `this` | Constructor + `super()` | `super(name, age)` trong Dog |
| Polymorphism (preview) | Override, dynamic binding | `Animal a = new Dog()` |
| Encapsulation | Kế thừa không phá vỡ đóng gói | Getter/setter vẫn dùng khi extends |

**Câu hỏi ôn trước Chương 3:** “Dog **là một** Animal” có đúng quan hệ IS-A không? → Nếu có, có thể dùng `extends`.

---

## 📝 TÓM TẮT CHƯƠNG 2

### Kiến thức đã học:
1. ✅ Class và Object - Khuôn mẫu và thực thể
2. ✅ Cơ chế cấp phát bộ nhớ (Stack vs Heap)
3. ✅ Reference và vòng đời object
4. ✅ Encapsulation và Access Modifiers
5. ✅ Constructors (default, overloaded, chaining)
6. ✅ Từ khóa `this`
7. ✅ Getters và Setters
8. ✅ Java Records (Modern Java)
9. ✅ Immutable Objects
10. ✅ Static Members (variables, methods, blocks)

### Kỹ năng đã có:
- ✅ Thiết kế class với encapsulation đúng cách
- ✅ Sử dụng constructors và this
- ✅ Viết getters/setters với validation
- ✅ Tạo immutable objects
- ✅ Sử dụng Java Records

---

## 🎯 BÀI TẬP CHƯƠNG 2

### Bài 1: Class và Object cơ bản

**Yêu cầu:**
Tạo class `Rectangle` với:
- Fields: `width`, `height` (private, double)
- Constructor: Nhận width và height
- Methods:
  - `calculateArea()`: Tính diện tích
  - `calculatePerimeter()`: Tính chu vi
  - `displayInfo()`: In thông tin (width, height, area, perimeter)

**Test:**
```java
Rectangle rect = new Rectangle(5.0, 3.0);
rect.displayInfo();
// Output: Width: 5.0, Height: 3.0, Area: 15.0, Perimeter: 16.0
```

---

### Bài 2: Encapsulation và Access Modifiers

**Yêu cầu:**
Tạo class `BankAccount` với:
- Fields (private):
  - `accountNumber` (String)
  - `balance` (double)
- Constructor: Nhận accountNumber, balance mặc định = 0
- Methods:
  - `deposit(double amount)`: Nạp tiền (validation: amount > 0)
  - `withdraw(double amount)`: Rút tiền (validation: amount > 0 và <= balance)
  - `getBalance()`: Xem số dư
  - `getAccountNumber()`: Xem số tài khoản

**Test:**
```java
BankAccount account = new BankAccount("123456");
account.deposit(1000);
account.withdraw(300);
System.out.println("Balance: " + account.getBalance());  // 700.0
```

---

### Bài 3: Constructor Overloading

**Yêu cầu:**
Tạo class `Student` với nhiều constructor:
1. `Student(String name, int age, double gpa)`
2. `Student(String name, int age)` - gpa mặc định = 0.0
3. `Student(String name)` - age = 0, gpa = 0.0
4. `Student()` - name = "Unknown", age = 0, gpa = 0.0

Sử dụng constructor chaining (`this()`).

**Test:**
Tạo 4 object với 4 constructor khác nhau và in thông tin.

---

### Bài 4: Getters/Setters với Validation

**Yêu cầu:**
Tạo class `Product` với:
- Fields (private):
  - `name` (String, không null, không rỗng)
  - `price` (double, >= 0)
  - `quantity` (int, >= 0)
- Getters và Setters với validation:
  - Setter ném `IllegalArgumentException` nếu không hợp lệ

**Test:**
```java
Product product = new Product();
product.setName("Laptop");
product.setPrice(1000.0);
product.setQuantity(10);

// Test validation
try {
    product.setPrice(-100);  // ❌ Exception
} catch (IllegalArgumentException e) {
    System.out.println("Error: " + e.getMessage());
}
```

---

### Bài 5: Java Records

**Yêu cầu:**
1. Tạo record `Person` với: name, age, email
2. Thêm validation trong compact constructor (age >= 0, email không null)
3. Thêm method `isAdult()`: Trả về true nếu age >= 18
4. So sánh với class `Person` truyền thống (POJO)

**Test:**
```java
Person person = new Person("John", 25, "john@example.com");
System.out.println(person.isAdult());  // true
System.out.println(person);  // Person[name=John, age=25, email=john@example.com]
```

---

### Bài 6: Immutable Objects

**Yêu cầu:**
Tạo class `ImmutableBook` (immutable) với:
- Fields (final, private): title, author, isbn, publishedYear
- Constructor: Khởi tạo tất cả fields
- Chỉ có getters, không có setters
- Method `withTitle(String newTitle)`: Trả về Book mới với title mới
- Method `withAuthor(String newAuthor)`: Trả về Book mới với author mới

**Test:**
```java
ImmutableBook book = new ImmutableBook("Java Guide", "Author A", "123", 2023);
ImmutableBook updated = book.withTitle("Advanced Java Guide");

System.out.println(book.getTitle());     // "Java Guide" (không đổi)
System.out.println(updated.getTitle());  // "Advanced Java Guide"
```

---

### Bài 7: Tổng hợp - Quản lý Sinh viên

**Yêu cầu:**
Tạo hệ thống quản lý sinh viên với:

1. **Class `Student`**:
   - Fields: id, name, age, gpa (tất cả private)
   - Constructors: Đầy đủ và không tham số
   - Getters/Setters với validation
   - Method `displayInfo()`

2. **Class `StudentManager`**:
   - Field: `List<Student> students` (private)
   - Methods:
     - `addStudent(Student student)`
     - `removeStudent(String id)`
     - `findStudentById(String id)`: Trả về Student hoặc null
     - `displayAllStudents()`
     - `getAverageGpa()`: Tính GPA trung bình

3. **Class `Main`**:
   - Tạo StudentManager
   - Thêm ít nhất 3 sinh viên
   - Test tất cả methods

**Yêu cầu bổ sung:**
- Tuân thủ Clean Code
- JavaDoc đầy đủ
- Validation trong setters

---

## 📚 TÀI LIỆU THAM KHẢO

1. **Java Documentation**: https://docs.oracle.com/javase/tutorial/java/concepts/
2. **Java Records**: https://openjdk.org/jeps/395
3. **Effective Java** - Joshua Bloch (Item 17: Minimize mutability)

---

**Chúc bạn học tốt! 🚀**

