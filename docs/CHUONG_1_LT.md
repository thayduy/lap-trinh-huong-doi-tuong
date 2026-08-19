# CHƯƠNG 1: TƯ DUY HƯỚNG ĐỐI TƯỢNG & THIẾT LẬP MÔI TRƯỜNG CHUYÊN NGHIỆP

## 📚 MỤC TIÊU HỌC TẬP

Sau khi hoàn thành chương này, bạn sẽ:
- Nắm vững cú pháp và kiến thức cơ bản của ngôn ngữ Java
- Viết được chương trình Java đơn giản với cấu trúc điều khiển và mảng
- Hiểu được sự khác biệt giữa lập trình thủ tục và lập trình hướng đối tượng
- Nắm được các khái niệm cơ bản của OOP và tại sao nó quan trọng
- Thiết lập được môi trường phát triển Java chuyên nghiệp
- Sử dụng được Git và GitHub để quản lý mã nguồn
- Viết được code sạch, dễ đọc, tuân thủ các quy ước đặt tên

---

## 0. NGÔN NGỮ LẬP TRÌNH JAVA CƠ BẢN

> **📌 LƯU Ý QUAN TRỌNG:** 
> Phần này dành cho những bạn **chưa biết gì về Java**. Nếu bạn đã có kiến thức Java cơ bản, có thể bỏ qua hoặc đọc nhanh để ôn tập.
> 
> **Sau khi hoàn thành phần này, bạn sẽ:**
> - ✅ Hiểu cú pháp Java cơ bản
> - ✅ Viết được chương trình Java đơn giản
> - ✅ Sẵn sàng học OOP (Chương 2+)
> 
> **Liên kết với các chương sau:**
> - Phần này → **Chương 2**: Class & Object (sử dụng `new`, `this`, reference)
> - Wrapper Classes → **Chương 6**: Collections (List, Set, Map cần Wrapper)
> - Method → **Chương 2**: Method trong Class
> - Package → **Chương 2**: Tổ chức Class trong Package

---

### 🎓 Hướng dẫn học phần Java cơ bản (0.1–0.20)

1. **Học tuần tự** từ 0.1 → 0.20; mỗi mục có ví dụ – hãy gõ lại, không chỉ đọc.
2. **Chạy thử tổng hợp:** `CODE/VI_DU/CHUONG_1/01_JavaCoBan/` – mảng, method, if-else.
3. **Sau mỗi mục**, tự trả lời: “Output in ra gì nếu đổi input?”

#### ▶️ Ví dụ chạy thử – Java cơ bản

```bash
cd CODE/VI_DU/CHUONG_1/01_JavaCoBan
javac Main.java && java Main
```

**Output:** `Điểm trung bình: 6.6` / `Xếp loại: Khá`

**Giải thích:** Method `average` duyệt mảng `{8,5,9,4,7}` → tổng 33, chia 5 = 6.6. Method `grade` dùng if-else lồng nhau – **compile-time** đã biết gọi nhánh nào.

---

### 0.1. Giới thiệu Java

#### Java là gì?

**Java** là ngôn ngữ lập trình:
- **Hướng đối tượng** (Object-Oriented)
- **Đa nền tảng** (Write Once, Run Anywhere - WORA)
- **Mạnh mẽ và an toàn** (Type-safe, Memory management)
- **Phổ biến** (Dùng trong nhiều lĩnh vực: Web, Mobile, Enterprise)

#### Đặc điểm của Java

1. **Compiled và Interpreted**:
   - Code Java được compile thành bytecode (.class)
   - Bytecode chạy trên JVM (Java Virtual Machine)

2. **Platform Independent**:
   - Code viết một lần, chạy mọi nơi (Windows, Linux, macOS)
   - Nhờ JVM

   > **🌟 Tưởng tượng:** Java giống như bạn viết một cuốn sách bằng tiếng Anh (Code). JVM giống như một người phiên dịch tài ba. Dù bạn đến nước nào (Windows, Mac, Linux), người phiên dịch này cũng sẽ dịch cuốn sách của bạn sang ngôn ngữ địa phương (Machine Code) để người dân ở đó hiểu được. Bạn chỉ cần viết sách 1 lần!

3. **Object-Oriented**:
   - Mọi thứ đều là object (trừ primitive types)
   - Hỗ trợ đầy đủ OOP: Class, Inheritance, Polymorphism, Encapsulation

4. **Simple và Familiar**:
   - Cú pháp giống C/C++ nhưng đơn giản hơn
   - Loại bỏ con trỏ, multiple inheritance

---

### 0.2. Cấu trúc Chương trình Java cơ bản

#### Chương trình Hello World

```java
// File: HelloWorld.java
public class HelloWorld {
    // Điểm bắt đầu của mọi chương trình Java
    public static void main(String[] args) {
        System.out.println("Hello, World!"); // In ra màn hình
    }
}
```

### 🔬 Phân tích Code (Code Anatomy)

Hãy mổ xẻ từng từ khóa một để hiểu trọn vẹn dòng lệnh quyền lực `public static void main`:

| Từ khóa | Ý nghĩa | Tại sao lại cần? |
|---------|---------|------------------|
| **public** | Công khai | Để JVM (một phần mềm bên ngoài) có thể nhìn thấy và gọi method này. Nếu để `private`, JVM sẽ không tìm thấy cửa vào. |
| **static** | Tĩnh | Để JVM có thể chạy method này **ngay lập tức** mà không cần tạo object (xây nhà). Nếu không có `static`, JVM phải `new HelloWorld()` trước, rất phiền phức cho điểm khởi đầu. |
| **void** | Không trả về | Main method chạy xong là xong, không cần trả lại giá trị gì cho hệ điều hành. |
| **main** | Chính | Đây là tên quy ước cứng. JVM được lập trình để tìm đúng cái tên `main`. Bạn đặt là `chinh()` thì JVM chịu thua. |
| **String[] args** | Tham số | Nhận dữ liệu từ dòng lệnh (Command Line). Ví dụ chạy `java HelloWorld 1 2` thì `args` sẽ chứa `["1", "2"]`. |

> **👨‍💻 Lời khuyên chuyên gia:** Đừng cố ghi nhớ máy móc. Hãy hiểu rằng `main` method cần **công khai** (`public`) để ai cũng gọi được, và cần **sẵn sàng** (`static`) để chạy ngay lập tức.


**Phân tích từng phần:**

1. **`public class HelloWorld`**:
   - `public`: Access modifier (công khai)
   - `class`: Từ khóa định nghĩa class
   - `HelloWorld`: Tên class (phải trùng với tên file)

2. **`public static void main(String[] args)`**:
   - `public`: Cửa mở, ai cũng thấy.
   - `static`: Cố định, không cần xây nhà (tạo object) mới vào được.
   - `void`: Làm xong không cần báo cáo lại (không trả về giá trị).
   - `main`: **"Cửa chính"**. Mọi chương trình đều bắt đầu chạy từ đây.
   - `String[] args`: Những món quà (tham số) khách mang theo khi vào nhà (thường ít dùng khi mới học).

   > **🏠 Tưởng tượng:** `class HelloWorld` là một ngôi nhà. `main` chính là cánh cửa ra vào. Khi chạy chương trình, máy tính sẽ tìm đúng cánh cửa này để bước vào và bắt đầu thực hiện các công việc bên trong.

   > **💡 PHÂN BIỆT QUAN TRỌNG:** 
   > Ở Chương 1, chúng ta dùng `class` chỉ để **chứa code** (như cái vỏ). 
   > Sang **Chương 2**, chúng ta mới học dùng `class` để **tạo đối tượng** (OOP) có thuộc tính và hành vi riêng. Đừng lo lắng nếu bạn thấy khái niệm "Class" ở đây hơi sơ sài, chúng ta chỉ đang dùng nó như một cái hộp đựng hàm `main` thôi!

3. **`System.out.println()`**:
   - In ra màn hình và xuống dòng

**Chạy chương trình:**
```bash
# Compile
javac HelloWorld.java

# Run
java HelloWorld
# Output: Hello, World!
```

---

### 0.3. Kiểu dữ liệu trong Java

#### Primitive Types (Kiểu nguyên thủy)

**8 kiểu primitive:**

| Kiểu | Kích thước | Phạm vi | Giá trị mặc định |
|------|------------|---------|------------------|
| `byte` | 1 byte | -128 đến 127 | 0 |
| `short` | 2 bytes | -32,768 đến 32,767 | 0 |
| `int` | 4 bytes | -2³¹ đến 2³¹-1 | 0 |
| `long` | 8 bytes | -2⁶³ đến 2⁶³-1 | 0L |
| `float` | 4 bytes | ~±3.4×10³⁸ | 0.0f |
| `double` | 8 bytes | ~±1.7×10³⁰⁸ | 0.0d |
| `char` | 2 bytes | '\u0000' đến '\uffff' | '\u0000' |
| `boolean` | 1 bit | true/false | false |

> **🥤 Tưởng tượng:** Hãy coi các kiểu nguyên thủy như các **ly đựng nước** với kích thước khác nhau:
> - `byte`: Như cái ly uống rượu (rất nhỏ, chỉ đựng số nhỏ -128 đến 127).
> - `int`: Như cái cốc uống nước bình thường (đựng được hầu hết các số ta dùng).
> - `long`: Như cái thùng phuy (đựng số siêu to khổng lồ).
> 
> **Tại sao không dùng `long` cho tất cả mọi thứ?** Vì lãng phí bộ nhớ! Giống như bạn không dùng cái thùng phuy để uống một ngụm nước vậy.

**Ví dụ:**
```java
int age = 25;
double price = 99.99;
char grade = 'A';
boolean isActive = true;
long population = 1000000000L;  // L cho long
float pi = 3.14f;               // f cho float
```

### 🧠 Phân tích Bộ nhớ (Memory Analysis)

Khi bạn viết `int age = 25;`, điều gì thực sự xảy ra trong RAM?

1.  **Cấp phát**: Java xin hệ điều hành cấp một ô nhớ nhỏ (4 bytes) trong vùng **Stack**.
2.  **Đặt tên**: Ô nhớ đó được gắn nhãn là `age`.
3.  **Gán giá trị**: Số `25` (nhị phân: `00...011001`) được ghi trực tiếp vào ô nhớ đó.

> **⚠️ Tại sao điều này quan trọng?** 
> Vì primitive types lưu giá trị **trực tiếp** trong Stack, nên việc truy cập cực kỳ nhanh. Nó không cần đi vòng vo qua Heap như Object. Đó là lý do tại sao dùng `int` nhanh hơn `Integer`.

```

---

#### Reference Types (Kiểu tham chiếu)

**Bao gồm:**
- Classes (String, Object, ...)
- Arrays (mảng)
- Interfaces

**Ví dụ:**
```java
String name = "John";  // Reference type
int[] numbers = {1, 2, 3};  // Array
Object obj = new Object();  // Object
```

**Đặc điểm:**
- Giá trị mặc định: `null`
- Lưu địa chỉ (reference) đến object trong Heap

> **💡 Liên kết với Chương 2:**
> - Reference types sẽ được học sâu hơn ở **Chương 2.1.3** (Stack vs Heap, Reference)
> - `null` và NullPointerException quan trọng khi làm việc với Objects

---

### 0.4. Biến và Hằng số

#### Biến (Variables)

**Cú pháp:**
```java
dataType variableName = value;
```

> **📦 Tưởng tượng:** Biến giống như một cái **hộp** để đựng đồ.
> - **dataType (Kiểu dữ liệu)**: Quy định hình dáng cái hộp (Hộp tròn chỉ đựng banh tròn, hộp vuông đựng bánh vuông).
> - **variableName (Tên biến)**: Nhãn dán bên ngoài cái hộp để biết hộp đó đựng gì.
> - **value (Giá trị)**: Món đồ bạn bỏ vào trong hộp.

**Ví dụ:**
```java
int age = 20;         // Hộp 'int' tên 'age' đựng số 20
String name = "Alice"; // Hộp 'String' tên 'name' đựng chữ "Alice"
double gpa = 3.5;
```

**Quy tắc đặt tên:**
- Bắt đầu bằng chữ cái, `_`, hoặc `$`
- Không thể là từ khóa Java
- camelCase cho biến
- Tên có ý nghĩa

```java
// ✅ ĐÚNG
int studentAge = 20;
String firstName = "John";

// ❌ SAI
int 2age = 20;        // Bắt đầu bằng số
String class = "A";   // class là từ khóa
int a = 10;           // Tên không có ý nghĩa
```

---

#### Hằng số (Constants)

**Dùng `final` và `static final`:**
```java
// Hằng số instance
final int MAX_STUDENTS = 100;

// Hằng số class (dùng chung)
public static final double PI = 3.14159;
public static final String DEFAULT_NAME = "Unknown";
```

**Quy ước:**
- Tên viết hoa, dùng underscore
- `final`: Không thể thay đổi sau khi gán

---

### 0.5. Toán tử (Operators)

#### Toán tử Số học

```java
int a = 10;
int b = 3;

int sum = a + b;      // 13
int diff = a - b;     // 7
int product = a * b;  // 30
int quotient = a / b; // 3 (chia nguyên)
int remainder = a % b;// 1 (chia lấy dư)

// Tăng/Giảm
int x = 5;
x++;        // x = 6 (post-increment)
++x;        // x = 7 (pre-increment)
x--;        // x = 6 (post-decrement)
--x;        // x = 5 (pre-decrement)
```

---

#### Toán tử So sánh

```java
int a = 10;
int b = 20;

boolean isEqual = (a == b);        // false
boolean notEqual = (a != b);       // true
boolean greater = (a > b);         // false
boolean less = (a < b);            // true
boolean greaterEqual = (a >= b);   // false
boolean lessEqual = (a <= b);      // true
```

---

#### Toán tử Logic

```java
boolean x = true;
boolean y = false;

boolean and = x && y;   // false (AND)
boolean or = x || y;    // true (OR)
boolean not = !x;       // false (NOT)
```

---

#### Toán tử Gán

```java
int a = 10;

a += 5;  // a = a + 5 → 15
a -= 3;  // a = a - 3 → 12
a *= 2;  // a = a * 2 → 24
a /= 4;  // a = a / 4 → 6
a %= 4;  // a = a % 4 → 2
```

---

### 0.6. Cấu trúc Điều khiển

#### If-Else

```java
int score = 85;

if (score >= 90) {
    System.out.println("Grade: A");
} else if (score >= 80) {
    System.out.println("Grade: B");
} else if (score >= 70) {
    System.out.println("Grade: C");
} else {
    System.out.println("Grade: F");
}
```

**Ternary Operator (Toán tử 3 ngôi):**
```java
int a = 10;
int b = 20;
int max = (a > b) ? a : b;  // max = 20
```

---

#### Switch-Case

```java
int day = 3;
String dayName;

switch (day) {
    case 1:
        dayName = "Monday";
        break;
    case 2:
        dayName = "Tuesday";
        break;
    case 3:
        dayName = "Wednesday";
        break;
    default:
        dayName = "Unknown";
}

// Switch expression (Java 14+)
dayName = switch (day) {
    case 1 -> "Monday";
    case 2 -> "Tuesday";
    case 3 -> "Wednesday";
    default -> "Unknown";
};
```

---

#### Vòng lặp For

```java
// For cơ bản
    System.out.println(i);  // 0, 1, 2, 3, 4
}

### 🕵️ Bảng vết chạy (Logic Trace)

Để hiểu vòng lặp, hãy "chạy bằng tay" (dry run) từng bước một:

| Bước | Giá trị `i` | Điều kiện `i < 5`? | Hành động | Thay đổi `i` |
|------|-------------|--------------------|-----------|--------------|
| 1    | 0           | ✅ True            | In ra `0` | `i++` → 1    |
| 2    | 1           | ✅ True            | In ra `1` | `i++` → 2    |
| 3    | 2           | ✅ True            | In ra `2` | `i++` → 3    |
| 4    | 3           | ✅ True            | In ra `3` | `i++` → 4    |
| 5    | 4           | ✅ True            | In ra `4` | `i++` → 5    |
| 6    | 5           | ❌ False           | **DỪNG**  | -            |

> **💡 Tại sao lại bắt đầu từ 0?**
> Trong lập trình, hầu hết các chỉ số (index) đều bắt đầu từ 0. Việc quen với `i = 0` sẽ giúp bạn làm việc với Mảng (Arrays) và List dễ dàng hơn sau này.


// For-each (Enhanced for)
int[] numbers = {1, 2, 3, 4, 5};
for (int num : numbers) {
    System.out.println(num);
}
```

---

#### Vòng lặp While

```java
// While
int i = 0;
while (i < 5) {
    System.out.println(i);
    i++;
}

// Do-While (chạy ít nhất 1 lần)
int j = 0;
do {
    System.out.println(j);
    j++;
} while (j < 5);
```

---

#### Break và Continue

```java
// Break: Thoát vòng lặp
for (int i = 0; i < 10; i++) {
    if (i == 5) {
        break;  // Thoát khi i = 5
    }
    System.out.println(i);  // 0, 1, 2, 3, 4
}

// Continue: Bỏ qua lần lặp hiện tại
for (int i = 0; i < 10; i++) {
    if (i % 2 == 0) {
        continue;  // Bỏ qua số chẵn
    }
    System.out.println(i);  // 1, 3, 5, 7, 9
}
```

---

### 0.7. Mảng (Arrays)

#### Khai báo và Khởi tạo Mảng

```java
// Cách 1: Khai báo rồi gán
int[] numbers = new int[5];
numbers[0] = 10;
numbers[1] = 20;
numbers[2] = 30;

// Cách 2: Khởi tạo ngay
int[] numbers2 = {10, 20, 30, 40, 50};

// Cách 3: Khai báo rồi khởi tạo sau
int[] numbers3;
numbers3 = new int[]{10, 20, 30};
```

---

#### Truy cập Mảng

```java
int[] numbers = {10, 20, 30, 40, 50};

// Truy cập phần tử
int first = numbers[0];   // 10
int second = numbers[1];  // 20

// Độ dài mảng
int length = numbers.length;  // 5

// Duyệt mảng
for (int i = 0; i < numbers.length; i++) {
    System.out.println(numbers[i]);
}

// For-each
for (int num : numbers) {
    System.out.println(num);
}
```

---

#### Mảng đa chiều

```java
// Mảng 2 chiều
int[][] matrix = {
    {1, 2, 3},
    {4, 5, 6},
    {7, 8, 9}
};

// Truy cập
int value = matrix[0][1];  // 2

// Duyệt
for (int i = 0; i < matrix.length; i++) {
    for (int j = 0; j < matrix[i].length; j++) {
        System.out.print(matrix[i][j] + " ");
    }
    System.out.println();
}
```

---

### 0.8. Method (Phương thức) cơ bản

#### Định nghĩa Method

**Cú pháp:**
```java
[access modifier] [static] returnType methodName(parameters) {
    // Body
    return value;  // Nếu có return type
}
```

**Ví dụ:**
```java
public class Calculator {
    // Method không trả về (void)
    public void printHello() {
        System.out.println("Hello!");
    }
    
    // Method trả về int
    public int add(int a, int b) {
        return a + b;
    }
    
    // Method static
    public static double multiply(double a, double b) {
        return a * b;
    }
    
    // Method với nhiều tham số
    public int sum(int... numbers) {
        int total = 0;
        for (int num : numbers) {
            total += num;
        }
        return total;
    }
}
```

---

#### Gọi Method

```java
// Tạo object với từ khóa 'new'
// 'new Calculator()' tạo một object mới trong bộ nhớ (Heap)
// 'calc' là reference (tham chiếu) trỏ đến object đó
Calculator calc = new Calculator();

// Gọi method instance (cần object)
// Method này thuộc về object, nên phải có object mới gọi được
calc.printHello();
int result = calc.add(5, 3);  // 8

// Gọi method static (không cần object)
// 'static' nghĩa là method thuộc về class, không thuộc về object
// Truy cập qua tên class: ClassName.methodName()
double product = Calculator.multiply(2.5, 3.0);  // 7.5

// Method với varargs (nhiều tham số)
int total = calc.sum(1, 2, 3, 4, 5);  // 15
```

**Giải thích thêm:**
- **`new Calculator()`**: 
  - `new` là từ khóa đặc biệt để tạo object mới
  - Tạo object trong Heap (bộ nhớ)
  - Sẽ học sâu hơn về Stack vs Heap ở Chương 2.1.3
  
- **`static` method**: 
  - Method thuộc về class, không thuộc về object
  - Không cần tạo object để gọi
  - Truy cập qua tên class: `ClassName.methodName()`
  - Sẽ học sâu hơn ở Chương 2.6 (Static Members)

> **🏗️ Tưởng tượng:** `class Calculator` là một bản vẽ (blueprint) để xây nhà.
> - **`new Calculator()`**: Là hành động xây một ngôi nhà thật từ bản vẽ đó.
> - **Instance Method (`printHello`, `add`)**: Giống như việc "Mở cửa sổ" hay "Bật đèn" trong ngôi nhà đó. Bạn **phải xây nhà xong** (có object) thì mới mở cửa sổ được.
> - **Static Method (`multiply`)**: Giống như thông tin ghi trên bản vẽ (ví dụ: "Diện tích: 100m2"). Bạn **không cần xây nhà** cũng đọc được thông tin này trực tiếp trên bản vẽ (`Calculator.multiply`).

### 🧐 So sánh dòng-chảy (Flow Comparison)

**Trường hợp 1: Instance Method (`add`)**
1. `new Calculator()`: Tốn tiền mua đất, xây nhà (Cấp bộ nhớ Heap).
2. `calc.add()`: Vào nhà, dùng cái máy tính của nhà đó để cộng.
3. **Kết luận**: Tốn kém, cần object. Dùng khi hành động liên quan đến trạng thái riêng của object (ví dụ: `account.withdraw()` trừ tiền của đúng ông A chứ không phải ông B).

**Trường hợp 2: Static Method (`multiply`)**
1. `Calculator.multiply()`: Nhìn vào tờ giấy hướng dẫn sử dụng (Class) và tính nhẩm luôn.
2. **Kết luận**: Nhanh, gọn, không tốn bộ nhớ tạo object. Dùng cho các hàm tiện ích (Utils) tính toán thuần túy (như `Math.sqrt`, `LocalDateTime.now`).


> **💡 Liên kết với Chương 2:**
> - Method trong class sẽ được học chi tiết ở **Chương 2.3** (Constructors, Getters/Setters)
> - `static` method sẽ được giải thích rõ hơn ở **Chương 2.6** (Static Members)
> - `new` để tạo object sẽ được học sâu ở **Chương 2.1.2** (Tạo Object)
> - Stack vs Heap sẽ được học ở **Chương 2.1.3** (Cơ chế cấp phát bộ nhớ)

---

### 0.9. Input/Output cơ bản

#### Output (Xuất dữ liệu)

```java
// System.out.println() - In và xuống dòng
System.out.println("Hello");
System.out.println("World");
// Output:
// Hello
// World

// System.out.print() - In không xuống dòng
System.out.print("Hello ");
System.out.print("World");
// Output: Hello World

// System.out.printf() - Format string
String name = "John";
int age = 25;
System.out.printf("Name: %s, Age: %d\n", name, age);
// Output: Name: John, Age: 25
```

---

#### Input (Nhập dữ liệu)

**Dùng Scanner:**
```java
import java.util.Scanner;

public class InputExample {
    public static void main(String[] args) {
        Scanner scanner = new Scanner(System.in);
        
        // Đọc String
        System.out.print("Enter your name: ");
        String name = scanner.nextLine();
        
        // Đọc int
        System.out.print("Enter your age: ");
        int age = scanner.nextInt();
        
        // Đọc double
        System.out.print("Enter your GPA: ");
        double gpa = scanner.nextDouble();
        
        System.out.println("Name: " + name + ", Age: " + age + ", GPA: " + gpa);
        
        scanner.close();  // Đóng scanner
    }
}
```

---

### 0.10. String trong Java

#### String là gì?

**String** là một class đặc biệt trong Java để xử lý chuỗi.

```java
// Cách 1: String literal
String str1 = "Hello";

// Cách 2: new String()
String str2 = new String("Hello");
```

---

#### Các Method phổ biến

```java
String str = "Hello World";

// Độ dài
int length = str.length();  // 11

// Nối chuỗi
String combined = str + " Java";  // "Hello World Java"
String combined2 = str.concat(" Java");  // "Hello World Java"

// So sánh
boolean equals = str.equals("Hello World");  // true
boolean equalsIgnoreCase = str.equalsIgnoreCase("HELLO WORLD");  // true
int compare = str.compareTo("Hello");  // > 0

// Tìm kiếm
boolean contains = str.contains("World");  // true
int index = str.indexOf("World");  // 6

// Cắt chuỗi
String substring = str.substring(0, 5);  // "Hello"
String[] parts = str.split(" ");  // ["Hello", "World"]

// Chuyển đổi
String upper = str.toUpperCase();  // "HELLO WORLD"
String lower = str.toLowerCase();  // "hello world"
String trimmed = "  Hello  ".trim();  // "Hello"
```

---

### 0.11. Package và Import

#### Package là gì?

**Package** là cách tổ chức code thành các nhóm logic.

```java
// File: com/example/Student.java
package com.example;

public class Student {
    // ...
}
```

**Cấu trúc thư mục:**
```
project/
└── src/
    └── com/
        └── example/
            └── Student.java
```

---

#### Import

**Import class từ package khác:**
```java
// Import một class
import java.util.Scanner;
import java.util.ArrayList;

// Import tất cả
import java.util.*;

// Import static (Java 5+)
import static java.lang.Math.PI;
import static java.lang.Math.sqrt;

// Sử dụng
double area = PI * radius * radius;
double result = sqrt(16);  // 4.0
```

---

### 0.12. Type Casting (Ép kiểu)

#### Implicit Casting (Ép kiểu tự động)

**Ép kiểu từ nhỏ sang lớn (tự động):**
```java
int a = 10;
double b = a;  // Tự động ép int → double
// b = 10.0

float f = 3.14f;
double d = f;  // Tự động ép float → double
```

**Thứ tự:**
```
byte → short → int → long → float → double
char → int
```

---

#### Explicit Casting (Ép kiểu thủ công)

**Ép kiểu từ lớn sang nhỏ (phải cast):**
```java
double d = 10.5;
int i = (int) d;  // Ép double → int
// i = 10 (mất phần thập phân)

long l = 100L;
int i2 = (int) l;  // Ép long → int
```

**⚠️ Cảnh báo:**
```java
double d = 300.7;
byte b = (byte) d;  // ❌ Mất dữ liệu! (byte chỉ -128 đến 127)
// b = 44 (không phải 300)
```

---

#### String và Number Conversion

**String → Number:**
```java
String str = "123";
int num = Integer.parseInt(str);      // String → int
double d = Double.parseDouble("3.14"); // String → double
```

**Number → String:**
```java
int num = 123;
String str1 = String.valueOf(num);     // int → String
String str2 = Integer.toString(num);   // int → String
String str3 = num + "";                // Cách đơn giản
```

---

### 0.13. Wrapper Classes

#### Wrapper Classes là gì?

**Wrapper Classes** là class tương ứng với mỗi primitive type:

| Primitive | Wrapper |
|-----------|---------|
| `byte` | `Byte` |
| `short` | `Short` |
| `int` | `Integer` |
| `long` | `Long` |
| `float` | `Float` |
| `double` | `Double` |
| `char` | `Character` |
| `boolean` | `Boolean` |

**Tại sao cần?**
- Collections chỉ chấp nhận objects (không chấp nhận primitives)
- Có các utility methods hữu ích

---

#### Autoboxing và Unboxing

**Autoboxing (Tự động đóng gói):**
```java
// Primitive → Wrapper (tự động)
int num = 10;
Integer wrapper = num;  // Tự động: Integer.valueOf(num)
```

**Unboxing (Tự động mở gói):**
```java
// Wrapper → Primitive (tự động)
Integer wrapper = 10;
int num = wrapper;  // Tự động: wrapper.intValue()
```

**Ví dụ:**
```java
List<Integer> numbers = new ArrayList<>();
numbers.add(10);        // Autoboxing: int → Integer
int value = numbers.get(0);  // Unboxing: Integer → int
```

> **💡 Liên kết với Chương 6:**
> - Wrapper Classes **BẮT BUỘC** khi dùng Collections (Chương 6.1)
> - `List<Integer>`, `Set<String>`, `Map<String, Integer>` đều cần Wrapper Classes

---

#### Utility Methods

```java
// Integer
int max = Integer.MAX_VALUE;      // 2147483647
int min = Integer.MIN_VALUE;      // -2147483648
int parsed = Integer.parseInt("123");  // String → int
String str = Integer.toString(123);     // int → String

// Double
double maxD = Double.MAX_VALUE;
boolean isNaN = Double.isNaN(0.0/0.0);  // true

// Character
boolean isDigit = Character.isDigit('5');  // true
boolean isLetter = Character.isLetter('A'); // true
char upper = Character.toUpperCase('a');   // 'A'
```

---

### 0.14. Scope của Biến (Phạm vi)

#### Local Variables (Biến cục bộ)

**Biến khai báo trong method:**
```java
public void method() {
    int localVar = 10;  // Chỉ dùng được trong method này
    System.out.println(localVar);
}
// localVar không thể dùng ở đây
```

**Đặc điểm:**
- Chỉ tồn tại trong block `{ }`
- Phải khởi tạo trước khi dùng
- Không có giá trị mặc định

---

#### Instance Variables (Biến instance)

**Biến khai báo trong class (không có static):**
```java
public class Student {
    private String name;  // Instance variable
    
    public void setName(String name) {
        this.name = name;  // Dùng this để phân biệt
    }
}
```

**Đặc điểm:**
- Mỗi object có bản copy riêng
- Có giá trị mặc định
- Tồn tại suốt đời object

---

#### Class Variables (Biến class - Static)

**Biến khai báo với `static`:**
```java
public class Student {
    private static int count = 0;  // Class variable (dùng chung)
    
    public Student() {
        count++;  // Tăng count mỗi khi tạo object mới
    }
    
    public static int getCount() {
        return count;  // Dùng chung cho tất cả objects
    }
}
```

**Đặc điểm:**
- Dùng chung cho tất cả objects
- Truy cập qua class name: `Student.getCount()`
- Không cần tạo object

---

### 0.15. null và NullPointerException

#### null là gì?

**`null`** là giá trị đặc biệt cho reference types:
- Không trỏ đến object nào
- Chỉ dùng cho reference types (không dùng cho primitives)

```java
String name = null;  // ✅ OK
int age = null;      // ❌ LỖI - primitives không thể null
```

---

#### NullPointerException

**Lỗi khi truy cập method/field của null:**
```java
String name = null;
int length = name.length();  // ❌ NullPointerException!
```

**Cách tránh:**
```java
String name = null;

// Cách 1: Kiểm tra null
if (name != null) {
    int length = name.length();
}

// Cách 2: Dùng Optional (sẽ học sau)
// Cách 3: Dùng try-catch
try {
    int length = name.length();
} catch (NullPointerException e) {
    System.out.println("name is null");
}
```

---

### 0.16. StringBuilder (Hiệu quả hơn String)

#### Vấn đề với String

**String là immutable (bất biến):**
```java
String str = "Hello";
str = str + " World";  // Tạo String mới, không thay đổi String cũ
// Nhiều lần nối → Tạo nhiều String objects → Chậm
```

---

#### Giải pháp: StringBuilder

**StringBuilder có thể thay đổi:**
```java
StringBuilder sb = new StringBuilder();
sb.append("Hello");
sb.append(" ");
sb.append("World");
String result = sb.toString();  // "Hello World"

// Hoặc chain
String result2 = new StringBuilder()
    .append("Hello")
    .append(" ")
    .append("World")
    .toString();
```

**Lợi ích:**
- ✅ Nhanh hơn khi nối nhiều chuỗi
- ✅ Tiết kiệm bộ nhớ
- ✅ Có thể thay đổi

**Khi nào dùng:**
- Nối nhiều chuỗi (> 3 lần)
- Vòng lặp nối chuỗi

> **💡 Liên kết với Chương 6:**
> - StringBuilder thường dùng khi xử lý dữ liệu với **Stream API** (Chương 6.4)

---

### 0.17. Math Class

#### Các Method phổ biến

```java
// Giá trị tuyệt đối
int abs = Math.abs(-5);  // 5

// Làm tròn
double ceil = Math.ceil(3.7);   // 4.0 (làm tròn lên)
double floor = Math.floor(3.7); // 3.0 (làm tròn xuống)
long round = Math.round(3.7);   // 4 (làm tròn gần nhất)

// Min/Max
int min = Math.min(5, 3);  // 3
int max = Math.max(5, 3);  // 5

// Lũy thừa
double power = Math.pow(2, 3);  // 8.0 (2³)

// Căn bậc hai
double sqrt = Math.sqrt(16);  // 4.0

// Random (0.0 đến 1.0)
double random = Math.random();  // 0.0 ≤ random < 1.0
int dice = (int)(Math.random() * 6) + 1;  // 1-6
```

---

### 0.18. Comment trong Java

#### Các loại Comment

**1. Single-line Comment:**
```java
// Đây là comment một dòng
int age = 20;  // Comment ở cuối dòng
```

**2. Multi-line Comment:**
```java
/*
 * Đây là comment nhiều dòng
 * Có thể viết nhiều dòng
 */
```

**3. JavaDoc Comment:**
```java
/**
 * Mô tả method/class
 * 
 * @param name Tên người dùng
 * @return Tuổi
 */
public int getAge(String name) {
    // ...
}
```

**Lưu ý:**
- Comment giải thích "TẠI SAO", không phải "LÀM GÌ"
- Code nên tự mô tả (self-documenting)

> **💡 Liên kết với Chương 1.3:**
> - Comment best practices sẽ được học chi tiết ở **Chương 1.3.3** (Clean Code)
> - JavaDoc sẽ được học ở **Chương 1.3.3** (JavaDoc)

---

### 0.19. Tổng kết Phần Java Cơ bản

#### Checklist Kiến thức

Trước khi chuyển sang học OOP, hãy đảm bảo bạn đã nắm vững:

- [ ] ✅ Cú pháp Java cơ bản (class, method, main)
- [ ] ✅ Kiểu dữ liệu (primitive, reference)
- [ ] ✅ Biến và hằng số
- [ ] ✅ Toán tử (số học, so sánh, logic)
- [ ] ✅ Cấu trúc điều khiển (if-else, switch, for, while)
- [ ] ✅ Mảng (khai báo, truy cập, duyệt)
- [ ] ✅ Method (định nghĩa, gọi, tham số)
- [ ] ✅ Input/Output (Scanner, System.out)
- [ ] ✅ String và các method
- [ ] ✅ Type casting (ép kiểu)
- [ ] ✅ Wrapper Classes (Integer, Double, ...)
- [ ] ✅ Scope của biến (local, instance, static)
- [ ] ✅ null và NullPointerException
- [ ] ✅ StringBuilder
- [ ] ✅ Math class
- [ ] ✅ Comment

#### Sẵn sàng cho OOP?

Nếu bạn đã nắm vững các kiến thức trên, bạn đã sẵn sàng học:
- **Chương 2**: Class & Object (sử dụng `new`, `this`, reference)
- **Chương 3**: Inheritance & Polymorphism
- **Chương 6**: Collections (cần Wrapper Classes)

---

### 0.20. Ví dụ Tổng hợp

#### Chương trình Quản lý Sinh viên đơn giản

```java
import java.util.Scanner;

public class StudentManager {
    public static void main(String[] args) {
        Scanner scanner = new Scanner(System.in);
        
        // Nhập thông tin
        System.out.print("Enter student name: ");
        String name = scanner.nextLine();
        
        System.out.print("Enter student age: ");
        int age = scanner.nextInt();
        
        System.out.print("Enter student GPA: ");
        double gpa = scanner.nextDouble();
        
        // Xử lý
        String grade = calculateGrade(gpa);
        boolean isPassed = (gpa >= 2.0);
        
        // Hiển thị
        System.out.println("\n=== Student Information ===");
        System.out.println("Name: " + name);
        System.out.println("Age: " + age);
        System.out.println("GPA: " + gpa);
        System.out.println("Grade: " + grade);
        System.out.println("Status: " + (isPassed ? "Passed" : "Failed"));
        
        scanner.close();
    }
    
    public static String calculateGrade(double gpa) {
        if (gpa >= 3.5) {
            return "A";
        } else if (gpa >= 3.0) {
            return "B";
        } else if (gpa >= 2.5) {
            return "C";
        } else if (gpa >= 2.0) {
            return "D";
        } else {
            return "F";
        }
    }
}
```

---

## 1.1. TỪ TƯ DUY THỦ TỤC (PROCEDURAL) SANG HƯỚNG ĐỐI TƯỢNG (OOP)

### 1.1.1. Lập trình Thủ tục là gì?

**Lập trình thủ tục (Procedural Programming)** là cách tiếp cận lập trình truyền thống, tập trung vào:
- **Hàm (Functions/Procedures)**: Chia nhỏ chương trình thành các hàm
- **Dữ liệu và Logic tách biệt**: Dữ liệu được truyền vào hàm như tham số
- **Thứ tự thực thi**: Code chạy từ trên xuống dưới

#### Ví dụ: Quản lý Sinh viên (Cách thủ tục)

```c
// Dữ liệu
struct Student {
    char name[50];
    int age;
    float gpa;
};

// Các hàm xử lý
void printStudent(struct Student s) {
    printf("Name: %s, Age: %d, GPA: %.2f\n", s.name, s.age, s.gpa);
}

void updateGPA(struct Student *s, float newGPA) {
    s->gpa = newGPA;
}

int main() {
    struct Student student1 = {"Nguyen Van A", 20, 3.5};
    printStudent(student1);
    updateGPA(&student1, 3.8);
    return 0;
}
```

**Đặc điểm:**
- Dữ liệu (`struct Student`) và hàm (`printStudent`, `updateGPA`) tách biệt
- Phải truyền dữ liệu vào hàm mỗi lần gọi
- Khó quản lý khi hệ thống lớn

---

### 1.1.2. Đặc điểm và Giới hạn của Lập trình Thủ tục

#### ✅ Ưu điểm:
1. **Đơn giản**: Dễ hiểu cho người mới bắt đầu
2. **Hiệu suất**: Thường nhanh hơn (ít overhead)
3. **Phù hợp**: Cho các bài toán nhỏ, đơn giản

#### ❌ Nhược điểm:

**1. Khó quản lý độ phức tạp:**
```
Khi hệ thống có 1000 hàm và 500 cấu trúc dữ liệu:
- Khó biết hàm nào dùng cấu trúc nào
- Dễ tạo ra dependencies phức tạp
- Khó tìm và sửa lỗi
```

**2. Dữ liệu và Logic tách biệt:**
```c
// Vấn đề: Có thể vô tình truyền sai dữ liệu
void calculateSalary(struct Employee *emp, float hours) {
    // Nhưng nếu truyền nhầm struct Student vào?
    emp->salary = hours * emp->rate;
}
```

**3. Khó tái sử dụng:**
- Mỗi hàm phải xử lý nhiều trường hợp khác nhau
- Code dễ bị lặp lại (DRY - Don't Repeat Yourself bị vi phạm)

**4. Khó bảo trì:**
- Thay đổi cấu trúc dữ liệu → phải sửa tất cả hàm liên quan
- Khó mở rộng tính năng mới

---

### 1.1.3. Bài toán mà OOP Giải quyết

#### Vấn đề thực tế:

**Tình huống**: Xây dựng hệ thống quản lý ngân hàng

**Yêu cầu:**
- Quản lý tài khoản (Account)
- Quản lý khách hàng (Customer)
- Xử lý giao dịch (Transaction)
- Tính lãi suất
- Báo cáo

**Với cách thủ tục:**
```c
// 100+ hàm rải rác
void createAccount(...);
void deposit(...);
void withdraw(...);
void calculateInterest(...);
void generateReport(...);
// ... và 50+ hàm khác

// 50+ cấu trúc dữ liệu
struct Account { ... };
struct Customer { ... };
struct Transaction { ... };
// ... và 20+ struct khác

// Vấn đề: Khó biết hàm nào liên quan đến Account?
// Khó thêm tính năng mới (ví dụ: Account có thể là Savings hoặc Checking)
```

**Với OOP:**
```java
// Mỗi class gom dữ liệu + hành vi lại với nhau
class Account {
    private double balance;
    public void deposit(double amount) { ... }
    public void withdraw(double amount) { ... }
}

class SavingsAccount extends Account {
    public void calculateInterest() { ... }
}

class CheckingAccount extends Account {
    public void processCheck() { ... }
}
```

**Lợi ích:**
- ✅ Dữ liệu và hành vi gắn liền với nhau
- ✅ Dễ mở rộng (thêm loại Account mới)
- ✅ Dễ bảo trì (sửa Account chỉ ảnh hưởng Account)
- ✅ Dễ hiểu (Account có gì, làm được gì rõ ràng)

---

### 1.1.4. So sánh Tư duy Procedural và OOP

#### Bảng so sánh:

| Khía cạnh | Procedural | OOP |
|-----------|------------|-----|
| **Đơn vị cơ bản** | Hàm (Function) | Đối tượng (Object) |
| **Dữ liệu** | Tách biệt với logic | Gắn liền với hành vi |
| **Tập trung** | "Làm thế nào" (How) | "Là gì" và "Làm gì" (What) |
| **Mô hình** | Top-down | Bottom-up |
| **Tái sử dụng** | Khó, phải copy code | Dễ, dùng kế thừa |
| **Bảo mật** | Khó kiểm soát truy cập | Dễ (Encapsulation) |
| **Phù hợp** | Hệ thống nhỏ, đơn giản | Hệ thống lớn, phức tạp |

#### Ví dụ minh họa: Tính diện tích hình chữ nhật

**Cách Procedural:**
```c
// Dữ liệu
struct Rectangle {
    double width;
    double height;
};

// Hàm tính diện tích
double calculateArea(struct Rectangle rect) {
    return rect.width * rect.height;
}

// Sử dụng
struct Rectangle r = {5.0, 3.0};
double area = calculateArea(r);
```

**Cách OOP:**
```java
// Class: Gom dữ liệu + hành vi
class Rectangle {
    private double width;
    private double height;
    
    public Rectangle(double width, double height) {
        this.width = width;
        this.height = height;
    }
    
    public double calculateArea() {
        return width * height;
    }
}

// Sử dụng
Rectangle rect = new Rectangle(5.0, 3.0);
double area = rect.calculateArea();
```

**Phân tích:**
- **Procedural**: Phải truyền `rect` vào hàm mỗi lần
- **OOP**: `rect` tự biết cách tính diện tích của mình
- **OOP**: Dữ liệu được bảo vệ (`private`), không thể truy cập trực tiếp

---

### 1.1.5. Các Khái niệm Cốt lõi của OOP

#### 1. **Class (Lớp)**
- **Định nghĩa**: Khuôn mẫu, bản thiết kế của đối tượng
- **Ví dụ**: `Car` là class, mô tả các đặc điểm (màu sắc, tốc độ) và hành vi (chạy, dừng)

#### 2. **Object (Đối tượng)**
- **Định nghĩa**: Thực thể cụ thể được tạo từ class
- **Ví dụ**: `myCar` là một object của class `Car`

#### 3. **Encapsulation (Đóng gói)**
- **Định nghĩa**: Che giấu chi tiết bên trong, chỉ expose những gì cần thiết
- **Ví dụ**: Bạn không cần biết động cơ hoạt động thế nào, chỉ cần biết nhấn ga để chạy

#### 4. **Inheritance (Kế thừa)**
- **Định nghĩa**: Class con kế thừa đặc điểm từ class cha
- **Ví dụ**: `ElectricCar` kế thừa từ `Car`, có thêm pin

#### 5. **Polymorphism (Đa hình)**
- **Định nghĩa**: Cùng một hành vi nhưng thể hiện khác nhau
- **Ví dụ**: `Car.start()` và `ElectricCar.start()` hoạt động khác nhau

#### 6. **Abstraction (Trừu tượng hóa)**
- **Định nghĩa**: Ẩn đi chi tiết phức tạp, chỉ hiển thị những gì cần thiết
- **Ví dụ**: Interface `Drivable` chỉ định nghĩa `drive()`, không quan tâm cách implement

---

## 1.2. MÔI TRƯỜNG PHÁT TRIỂN & CÔNG CỤ CHUYÊN NGHIỆP

### 1.2.1. Giới thiệu IDE: Cursor IDE (AI-first Code Editor)

#### IDE là gì?

**IDE (Integrated Development Environment)** = Môi trường phát triển tích hợp

**Bao gồm:**
- Code editor (soạn thảo code)
- Compiler (biên dịch)
- Debugger (gỡ lỗi)
- Build tools (công cụ build)
- Version control (quản lý phiên bản)

#### Tại sao dùng IDE?

**Không dùng IDE (chỉ dùng Notepad + Command line):**
```bash
# Phải tự compile
javac HelloWorld.java

# Phải tự chạy
java HelloWorld

# Không có gợi ý code
# Không có debugger
# Không có quản lý dự án
```

**Dùng IDE:**
- ✅ Auto-complete (gợi ý code)
- ✅ Syntax highlighting (tô màu code)
- ✅ Error detection (phát hiện lỗi ngay)
- ✅ Debugger tích hợp
- ✅ Quản lý dependencies
- ✅ Git tích hợp

#### Cursor IDE - AI-first Code Editor

**Đặc điểm:**
- **AI-powered**: Hỗ trợ viết code bằng AI
- **Modern**: Dựa trên VS Code, nhưng có AI tích hợp
- **Productive**: Tăng năng suất lập trình

**Các tính năng:**
1. **AI Code Completion**: Gợi ý code thông minh
2. **AI Chat**: Hỏi đáp về code
3. **AI Refactoring**: Refactor code tự động
4. **Git Integration**: Quản lý Git trực tiếp

**Cài đặt Cursor:**
1. Truy cập: https://cursor.sh
2. Download và cài đặt
3. Mở Cursor và bắt đầu code!

---

### 1.2.2. Cấu hình Môi trường: Java Development Kit (JDK)

#### JDK là gì?

**JDK (Java Development Kit)** = Bộ công cụ phát triển Java

**Bao gồm:**
- **JRE (Java Runtime Environment)**: Môi trường chạy Java
- **Compiler (javac)**: Biên dịch .java → .class
- **JVM (Java Virtual Machine)**: Máy ảo Java
- **Tools**: javadoc, jar, jdb, ...

#### Các phiên bản JDK

**LTS (Long Term Support) - Khuyến nghị dùng:**
- **JDK 17** (2021): Ổn định, phổ biến
- **JDK 21** (2023): Mới nhất, nhiều tính năng

**Non-LTS:**
- JDK 18, 19, 20, 22, ... (cập nhật nhanh, ít hỗ trợ)

**Khuyến nghị:** Dùng **JDK 17** hoặc **JDK 21** cho dự án mới

#### Cài đặt JDK trên macOS

**Cách 1: Dùng Homebrew (Khuyến nghị)**
```bash
# Cài Homebrew (nếu chưa có)
/bin/bash -c "$(curl -fsSL https://raw.githubusercontent.com/Homebrew/install/HEAD/install.sh)"

# Cài JDK 17
brew install openjdk@17

# Cài JDK 21
brew install openjdk@21

# Thiết lập JAVA_HOME
echo 'export PATH="/opt/homebrew/opt/openjdk@17/bin:$PATH"' >> ~/.zshrc
source ~/.zshrc
```

**Cách 2: Download từ Oracle/Adoptium**
1. Truy cập: https://adoptium.net/
2. Chọn JDK 17 hoặc 21
3. Download và cài đặt

#### Kiểm tra cài đặt

```bash
# Kiểm tra phiên bản Java
java -version

# Kết quả mong đợi:
# openjdk version "17.0.x" 2023-xx-xx
# OpenJDK Runtime Environment (build 17.0.x+xx)
# OpenJDK 64-Bit Server VM (build 17.0.x+xx, mixed mode, sharing)

# Kiểm tra compiler
javac -version
# javac 17.0.x
```

#### Cấu hình JAVA_HOME

**JAVA_HOME** là biến môi trường trỏ đến thư mục cài đặt JDK

**Trên macOS (zsh):**
```bash
# Tìm đường dẫn JDK
/usr/libexec/java_home -V

# Thêm vào ~/.zshrc
export JAVA_HOME=$(/usr/libexec/java_home -v 17)
export PATH=$JAVA_HOME/bin:$PATH

# Áp dụng
source ~/.zshrc

# Kiểm tra
echo $JAVA_HOME
```

---

### 1.2.3. Quản lý Phiên bản (Git & GitHub)

#### Tại sao cần Git?

**Vấn đề không dùng Git:**
```
project_v1.java
project_v2.java
project_v2_final.java
project_v2_final_really.java
project_v3.java
project_v3_backup.java
...
```

**Với Git:**
- ✅ Lưu lịch sử thay đổi
- ✅ Quay lại phiên bản cũ
- ✅ Làm việc nhóm dễ dàng
- ✅ Branch để thử nghiệm

#### Git là gì?

**Git** = Hệ thống quản lý phiên bản phân tán (Distributed Version Control System)

**Khái niệm cơ bản:**
- **Repository (Repo)**: Kho chứa mã nguồn
- **Commit**: Lưu snapshot của code tại một thời điểm
- **Branch**: Nhánh phát triển riêng
- **Merge**: Gộp các nhánh lại

#### Cài đặt Git

**macOS:**
```bash
# Cài bằng Homebrew
brew install git

# Hoặc đã có sẵn (kiểm tra)
git --version
```

**Cấu hình Git lần đầu:**
```bash
# Thiết lập tên
git config --global user.name "Your Name"

# Thiết lập email
git config --global user.email "your.email@example.com"

# Kiểm tra
git config --list
```

#### Quy trình làm việc cơ bản (Workflow)

**1. Khởi tạo Repository (init)**
```bash
# Tạo thư mục dự án
mkdir my-java-project
cd my-java-project

# Khởi tạo Git repository
git init

# Kết quả: Tạo thư mục .git (ẩn)
```

**2. Thêm file vào staging (add)**
```bash
# Tạo file Java
echo 'public class Hello { }' > Hello.java

# Thêm file vào staging area
git add Hello.java

# Hoặc thêm tất cả file
git add .

# Kiểm tra trạng thái
git status
```

**3. Commit (Lưu snapshot)**
```bash
# Commit với message
git commit -m "Initial commit: Add Hello.java"

# Kết quả:
# [main (root-commit) abc1234] Initial commit: Add Hello.java
#  1 file changed, 1 insertion(+)
```

**4. Xem lịch sử**
```bash
# Xem các commit
git log

# Xem ngắn gọn
git log --oneline
```

**5. Push lên GitHub**

**Tạo repository trên GitHub:**
1. Truy cập: https://github.com
2. Đăng nhập/Đăng ký
3. Click "New repository"
4. Đặt tên, chọn Public/Private
5. Click "Create repository"

**Kết nối và push:**
```bash
# Thêm remote repository
git remote add origin https://github.com/username/repo-name.git

# Push code lên
git push -u origin main

# Lần sau chỉ cần:
git push
```

#### Workflow hàng ngày

```bash
# 1. Kiểm tra thay đổi
git status

# 2. Xem code đã thay đổi gì
git diff

# 3. Thêm file vào staging
git add .

# 4. Commit
git commit -m "Mô tả thay đổi"

# 5. Push lên GitHub
git push
```

#### .gitignore

**Tạo file `.gitignore` để bỏ qua các file không cần commit:**
```
# Compiled class files
*.class

# Log files
*.log

# IDE files
.idea/
.vscode/
*.iml

# OS files
.DS_Store
Thumbs.db

# Build output
target/
out/
```

---

### 1.2.4. Build Tools: Maven (Giới thiệu)

#### Tại sao cần Maven?

**Không dùng Maven:**
- Phải tự download thư viện (JAR files)
- Phải tự quản lý dependencies
- Phải tự compile, test, package
- Khó chia sẻ dự án

**Dùng Maven:**
- ✅ Tự động quản lý dependencies
- ✅ Cấu trúc dự án chuẩn
- ✅ Build tự động
- ✅ Dễ chia sẻ

#### Cài đặt Maven

**macOS:**
```bash
brew install maven

# Kiểm tra
mvn -version
```

#### Cấu trúc dự án Maven

```
my-project/
├── pom.xml              # File cấu hình Maven
├── src/
│   ├── main/
│   │   └── java/        # Code chính
│   │       └── com/
│   │           └── example/
│   │               └── App.java
│   └── test/
│       └── java/        # Code test
│           └── com/
│               └── example/
│                   └── AppTest.java
└── target/              # Output (tự động tạo)
```

#### pom.xml cơ bản

```xml
<?xml version="1.0" encoding="UTF-8"?>
<project xmlns="http://maven.apache.org/POM/4.0.0"
         xmlns:xsi="http://www.w3.org/2001/XMLSchema-instance"
         xsi:schemaLocation="http://maven.apache.org/POM/4.0.0 
         http://maven.apache.org/xsd/maven-4.0.0.xsd">
    <modelVersion>4.0.0</modelVersion>

    <!-- Thông tin dự án -->
    <groupId>com.example</groupId>
    <artifactId>my-project</artifactId>
    <version>1.0.0</version>
    <packaging>jar</packaging>

    <name>My Java Project</name>

    <!-- Phiên bản Java -->
    <properties>
        <maven.compiler.source>17</maven.compiler.source>
        <maven.compiler.target>17</maven.compiler.target>
        <project.build.sourceEncoding>UTF-8</project.build.sourceEncoding>
    </properties>

    <!-- Dependencies (sẽ học sau) -->
    <dependencies>
        <!-- JUnit 5 cho testing -->
        <dependency>
            <groupId>org.junit.jupiter</groupId>
            <artifactId>junit-jupiter</artifactId>
            <version>5.10.0</version>
            <scope>test</scope>
        </dependency>
    </dependencies>
</project>
```

#### Maven Lifecycle

```bash
# Compile code
mvn compile

# Chạy tests
mvn test

# Package thành JAR
mvn package

# Clean (xóa target/)
mvn clean

# Tất cả: clean → compile → test → package
mvn clean package
```

---

## 1.3. CLEAN CODE – MÃ SẠCH & TƯ DUY KỸ SƯ

### 1.3.1. Quy ước Đặt tên Chuẩn (Naming Convention)

#### Tại sao cần Naming Convention?

**Code không có quy ước:**
```java
int x = 10;
String n = "John";
void f() { ... }
class c { ... }
```

**Vấn đề:**
- ❌ Khó đọc, khó hiểu
- ❌ Khó bảo trì
- ❌ Không chuyên nghiệp

**Code có quy ước:**
```java
int studentCount = 10;
String studentName = "John";
void calculateAverage() { ... }
class Student { ... }
```

**Lợi ích:**
- ✅ Dễ đọc, dễ hiểu
- ✅ Tự mô tả (self-documenting)
- ✅ Chuyên nghiệp

---

#### Quy ước đặt tên trong Java

**1. Class (Lớp) - PascalCase**
```java
// ✅ ĐÚNG
class Student { }
class BankAccount { }
class UserService { }

// ❌ SAI
class student { }        // Phải viết hoa chữ cái đầu
class bank_account { }  // Không dùng underscore
class userService { }   // Phải PascalCase
```

**2. Method (Phương thức) - camelCase**
```java
// ✅ ĐÚNG
public void calculateTotal() { }
public void getUserName() { }
public boolean isValid() { }

// ❌ SAI
public void CalculateTotal() { }  // Phải camelCase
public void get_user_name() { }   // Không dùng underscore
```

**3. Variable (Biến) - camelCase**
```java
// ✅ ĐÚNG
int studentCount;
String firstName;
boolean isActive;

// ❌ SAI
int StudentCount;      // Phải camelCase
String first_name;     // Không dùng underscore
int x;                 // Tên quá ngắn, không mô tả
```

**4. Constant (Hằng số) - UPPER_SNAKE_CASE**
```java
// ✅ ĐÚNG
public static final int MAX_STUDENTS = 100;
public static final String DEFAULT_NAME = "Unknown";
public static final double PI = 3.14159;

// ❌ SAI
public static final int maxStudents = 100;  // Phải UPPER_CASE
public static final String defaultName = "Unknown";
```

**5. Package (Gói) - lowercase**
```java
// ✅ ĐÚNG
package com.example.library;
package com.company.project.util;

// ❌ SAI
package com.example.Library;  // Phải lowercase
package com.example.library_util;  // Tránh underscore
```

---

#### Nguyên tắc Đặt tên Tốt

**1. Tên phải mô tả rõ mục đích**
```java
// ❌ TỒI
int d;  // d là gì?
void f() { }  // f làm gì?

// ✅ TỐT
int daysSinceCreation;
void calculateTotalPrice() { }
```

**2. Tránh viết tắt không rõ ràng**
```java
// ❌ TỒI
int usrCnt;  // User count? Unclear
String addr;  // Address? Unclear

// ✅ TỐT
int userCount;
String address;
```

**3. Tên boolean nên là câu hỏi Yes/No**
```java
// ✅ TỐT
boolean isActive;
boolean hasPermission;
boolean canEdit;
boolean isEmpty;

// ❌ TỒI
boolean active;  // Không rõ là gì
boolean flag;    // Quá chung chung
```

**4. Tên method nên là động từ**
```java
// ✅ TỐT
void calculateTotal() { }
String getUserName() { }
boolean isValid() { }
void saveToDatabase() { }

// ❌ TỒI
void total() { }        // Nên là calculateTotal()
String userName() { }  // Nên là getUserName()
```

**5. Tên class nên là danh từ**
```java
// ✅ TỐT
class Student { }
class BankAccount { }
class UserService { }

// ❌ TỒI
class Calculate { }    // Nên là Calculator
class Manage { }       // Nên là Manager
```

---

### 1.3.2. Tư duy Viết Code cho Con người Đọc (Readability)

#### Code được đọc nhiều hơn được viết

**Thống kê:**
- 80% thời gian: **Đọc code** (để hiểu, debug, maintain)
- 20% thời gian: **Viết code mới**

**Kết luận:** Code phải dễ đọc!

---

#### Ví dụ: Code khó đọc vs Dễ đọc

**Code khó đọc:**
```java
public double calc(int t, double p, double r) {
    return p * Math.pow(1 + r/100, t);
}
```

**Vấn đề:**
- ❌ Không biết `t`, `p`, `r` là gì
- ❌ Không biết công thức tính gì
- ❌ Khó hiểu logic

**Code dễ đọc:**
```java
/**
 * Tính số tiền sau khi gửi tiết kiệm với lãi suất kép
 * 
 * @param years Số năm gửi
 * @param principal Số tiền gốc
 * @param interestRate Lãi suất (%)
 * @return Số tiền sau khi đáo hạn
 */
public double calculateCompoundInterest(
        int years, 
        double principal, 
        double interestRate) {
    double rateDecimal = interestRate / 100.0;
    double multiplier = Math.pow(1 + rateDecimal, years);
    return principal * multiplier;
}
```

**Cải thiện:**
- ✅ Tên biến rõ ràng
- ✅ Có JavaDoc giải thích
- ✅ Tách biến trung gian (`rateDecimal`, `multiplier`)
- ✅ Dễ hiểu logic

---

#### Nguyên tắc Viết Code Dễ đọc

**1. Sử dụng biến trung gian**
```java
// ❌ TỒI
if (user.getAge() >= 18 && user.getAge() <= 65 && user.isActive()) {
    // ...
}

// ✅ TỐT
int userAge = user.getAge();
boolean isEligibleAge = userAge >= 18 && userAge <= 65;
boolean isActiveUser = user.isActive();

if (isEligibleAge && isActiveUser) {
    // ...
}
```

**2. Tránh magic numbers**
```java
// ❌ TỒI
if (age >= 18) { ... }
double tax = price * 0.1;

// ✅ TỐT
private static final int LEGAL_AGE = 18;
private static final double TAX_RATE = 0.1;

if (age >= LEGAL_AGE) { ... }
double tax = price * TAX_RATE;
```

**3. Tách method dài thành nhiều method nhỏ**
```java
// ❌ TỒI
public void processOrder(Order order) {
    // 50 dòng code...
    if (order.isValid()) {
        // 20 dòng code...
        if (order.hasItems()) {
            // 30 dòng code...
        }
    }
}

// ✅ TỐT
public void processOrder(Order order) {
    if (!isValidOrder(order)) {
        return;
    }
    
    calculateTotal(order);
    applyDiscount(order);
    processPayment(order);
    sendConfirmation(order);
}

private boolean isValidOrder(Order order) {
    return order != null && order.hasItems();
}

private void calculateTotal(Order order) {
    // ...
}
```

**4. Sử dụng tên biến mô tả**
```java
// ❌ TỒI
List<String> list = getUsers();
for (String s : list) {
    if (s.length() > 5) {
        // ...
    }
}

// ✅ TỐT
List<String> userNames = getUsers();
for (String userName : userNames) {
    if (userName.length() > MIN_USERNAME_LENGTH) {
        // ...
    }
}
```

---

### 1.3.3. Comment Code: Khi nào Cần thiết, Khi nào là "Rác"

#### Tại sao Comment có thể là "Rác"?

**Comment rác:**
```java
// Khai báo biến i
int i = 0;

// Tăng i lên 1
i++;

// In ra màn hình
System.out.println(i);
```

**Vấn đề:**
- ❌ Comment chỉ lặp lại code
- ❌ Không cung cấp thông tin mới
- ❌ Code thay đổi nhưng comment không update → gây hiểu nhầm

**Nguyên tắc:**
> **Code nên tự mô tả. Nếu cần comment để giải thích code, hãy refactor code!**

---

#### Khi nào Comment là Cần thiết?

**1. Giải thích "TẠI SAO" (Why), không phải "LÀM GÌ" (What)**
```java
// ✅ TỐT: Giải thích lý do
// Sử dụng quicksort thay vì mergesort vì dữ liệu gần như đã sắp xếp
// và quicksort nhanh hơn 30% trong trường hợp này
Arrays.sort(data, new QuickSortComparator());

// ❌ TỒI: Chỉ lặp lại code
// Sắp xếp mảng data
Arrays.sort(data);
```

**2. Giải thích Business Logic phức tạp**
```java
// ✅ TỐT
// Tính lãi suất theo quy tắc: 
// - Năm đầu: 5%
// - Năm 2-5: 7%
// - Sau năm 5: 10%
// Đây là quy định của ngân hàng ABC (Ref: Policy Doc #123)
double interestRate = calculateInterestRate(years);
```

**3. Cảnh báo về hậu quả**
```java
// ✅ TỐT
// KHÔNG được thay đổi thứ tự này!
// Phải validate trước khi save, nếu không sẽ gây lỗi database constraint
validateUser(user);
saveUser(user);
```

**4. TODO/FIXME (Tạm thời)**
```java
// TODO: Tối ưu query này khi có hơn 1000 records
List<User> users = database.getAllUsers();

// FIXME: Bug với timezone, cần xử lý lại
Date currentDate = new Date();
```

**5. JavaDoc cho Public API**
```java
/**
 * Tính tổng tiền của đơn hàng sau khi áp dụng giảm giá và thuế.
 * 
 * @param order Đơn hàng cần tính
 * @param discountCode Mã giảm giá (có thể null)
 * @return Tổng tiền cuối cùng
 * @throws InvalidOrderException Nếu đơn hàng không hợp lệ
 * @since 1.0
 */
public double calculateTotal(Order order, String discountCode) {
    // ...
}
```

---

#### JavaDoc - Tài liệu hóa Code

**JavaDoc** là công cụ tạo tài liệu tự động từ comment đặc biệt.

**Cú pháp:**
```java
/**
 * Mô tả class/method (bắt buộc)
 * 
 * @param parameterName Mô tả tham số
 * @return Mô tả giá trị trả về
 * @throws ExceptionType Mô tả exception
 * @since Phiên bản
 * @author Tác giả
 * @see Liên kết đến class/method khác
 */
```

**Ví dụ đầy đủ:**
```java
/**
 * Đại diện cho một tài khoản ngân hàng.
 * 
 * <p>Class này quản lý thông tin và các giao dịch của tài khoản,
 * bao gồm nạp tiền, rút tiền, và kiểm tra số dư.
 * 
 * <p>Ví dụ sử dụng:
 * <pre>{@code
 * BankAccount account = new BankAccount("123456", 1000.0);
 * account.deposit(500.0);
 * account.withdraw(200.0);
 * double balance = account.getBalance();
 * }</pre>
 * 
 * @author Nguyen Van A
 * @version 1.0
 * @since 1.0
 */
public class BankAccount {
    /**
     * Số tài khoản
     */
    private String accountNumber;
    
    /**
     * Số dư hiện tại
     */
    private double balance;
    
    /**
     * Tạo tài khoản mới với số dư ban đầu.
     * 
     * @param accountNumber Số tài khoản (không được null)
     * @param initialBalance Số dư ban đầu (phải >= 0)
     * @throws IllegalArgumentException Nếu accountNumber null hoặc initialBalance < 0
     */
    public BankAccount(String accountNumber, double initialBalance) {
        // ...
    }
    
    /**
     * Nạp tiền vào tài khoản.
     * 
     * @param amount Số tiền nạp (phải > 0)
     * @return Số dư mới sau khi nạp
     * @throws IllegalArgumentException Nếu amount <= 0
     */
    public double deposit(double amount) {
        // ...
    }
}
```

**Generate JavaDoc:**
```bash
# Với Maven
mvn javadoc:javadoc

# Với javadoc tool
javadoc -d docs -sourcepath src/main/java com.example.library
```

---

#### Code Smells - Dấu hiệu Code xấu

**1. Long Method (Method quá dài)**
```java
// ❌ TỒI: Method 100+ dòng
public void processEverything() {
    // 100 dòng code...
}

// ✅ TỐT: Tách thành nhiều method
public void processEverything() {
    validateInput();
    processData();
    saveResult();
    sendNotification();
}
```

**2. Long Parameter List (Quá nhiều tham số)**
```java
// ❌ TỒI
public void createUser(String name, String email, String phone, 
                       int age, String address, String city, 
                       String country, String zipCode) {
    // ...
}

// ✅ TỐT: Dùng object
public void createUser(UserInfo userInfo) {
    // ...
}
```

**3. Duplicate Code (Code lặp lại)**
```java
// ❌ TỒI
public void processOrder1(Order order) {
    if (order.isValid()) {
        calculateTotal(order);
        // ... 20 dòng code giống nhau
    }
}

public void processOrder2(Order order) {
    if (order.isValid()) {
        calculateTotal(order);
        // ... 20 dòng code giống nhau
    }
}

// ✅ TỐT: Extract method
private void processValidOrder(Order order) {
    calculateTotal(order);
    // ... 20 dòng code
}

public void processOrder1(Order order) {
    if (order.isValid()) {
        processValidOrder(order);
    }
}
```

**4. Magic Numbers (Số ma thuật)**
```java
// ❌ TỒI
if (age >= 18 && age <= 65) { ... }
double tax = price * 0.1;

// ✅ TỐT
private static final int MIN_AGE = 18;
private static final int MAX_AGE = 65;
private static final double TAX_RATE = 0.1;

if (age >= MIN_AGE && age <= MAX_AGE) { ... }
double tax = price * TAX_RATE;
```

---

## 1.4. JAVA MODULES (JPMS) – QUẢN LÝ DỰ ÁN LỚN

### 1.4.1. Java Platform Module System (Java 9+)

#### Vấn đề của Java cũ (Classpath Hell)
Trước Java 9, Java sử dụng **Classpath** để tìm kiếm các class. Điều này gây ra:
- ❌ **Không ẩn được package**: Bất kỳ class public nào cũng có thể bị truy cập.
- ❌ **Missing dependencies**: Không biết thiếu thư viện nào cho đến khi chạy (Runtime Error).
- ❌ **JDK quá nặng**: Phải tải toàn bộ JDK dù chỉ dùng 1 phần nhỏ.

#### Giải pháp: Modules
**Module** là một nhóm các packages và resources được đóng gói cùng nhau.
- Có file mô tả `module-info.java`.
- Kiểm soát rõ ràng: cái gì được vào, cái gì được ra.

---

### 1.4.2. File module-info.java

File này nằm ở thư mục gốc của source code (`src/main/java/module-info.java`).

```java
module com.example.myapp {
    // 1. requires: Khai báo module cần thiết
    requires java.sql;
    requires com.google.gson;
    
    // 2. exports: Cho phép module khác dùng package này
    exports com.example.myapp.api;
    // (Package com.example.myapp.internal sẽ bị ẩn)
    
    // 3. opens: Cho phép Reflection (dùng cho Hibernate, Spring...)
    opens com.example.myapp.io;
}
```

#### Các từ khóa chính:
- `requires`: Module này cần module nào để chạy.
- `exports`: Public package nào cho bên ngoài dùng.
- `opens`: Cho phép truy cập qua Reflection (quan trọng với Frameworks).

---

### 1.4.3. Lợi ích của Modules
- ✅ **Strong Encapsulation**: Ẩn các package nội bộ (internal implementation).
- ✅ **Reliable Configuration**: Báo lỗi thiếu thư viện ngay khi biên dịch.
- ✅ **Custom Runtime**: Chỉ đóng gói những module cần thiết (giảm dung lượng App từ 100MB -> 15MB với `jlink`).

> **💡 Lưu ý:** Trong các dự án nhỏ hoặc khi học, bạn có thể chưa cần Modules. Nhưng với các hệ thống lớn hoặc microservices, Modules là công cụ quản lý kiến trúc cực mạnh.

---

## 🔗 CẦU NỐI SANG CHƯƠNG 2

Bạn đã học **Java cơ bản** và **tư duy OOP**. Trước khi vào Chương 2, hãy nắm các “cầu nối” sau:

| Kiến thức Chương 1 | Sẽ học sâu ở Chương 2 | Ghi nhớ nhanh |
|--------------------|------------------------|---------------|
| `new Student()` trong ví dụ | Object trên Heap, reference trên Stack | `new` = tạo object, biến = tham chiếu |
| `static void main` | Static members (2.6) | `static` = thuộc class, không cần object |
| Method với tham số | Method trong class, `this` | Method trong class = hành vi của object |
| `private` trong ví dụ nhỏ | Encapsulation đầy đủ | Che dữ liệu, mở API qua getter/setter |
| Wrapper Classes | Collections (Ch.6) | `Integer` không phải `int` trong `List<>` |

**Bài tập chuyển tiếp:** Viết class `Book` với `title`, `author` (private), constructor và `displayInfo()` – đây là bản “tiền thân” của mọi class OOP ở Chương 2.

---

## 📝 TÓM TẮT CHƯƠNG 1

### Kiến thức đã học:

**Phần Java Cơ bản:**
1. ✅ Cấu trúc chương trình Java (class, method, main)
2. ✅ Kiểu dữ liệu (primitive types, reference types)
3. ✅ Biến và hằng số
4. ✅ Toán tử (số học, so sánh, logic)
5. ✅ Cấu trúc điều khiển (if-else, switch, for, while)
6. ✅ Mảng (array) một chiều và đa chiều
7. ✅ Method (định nghĩa, gọi, tham số, return, static)
8. ✅ Input/Output cơ bản (Scanner, System.out)
9. ✅ String và các method xử lý chuỗi
10. ✅ Package và Import
11. ✅ Type Casting (ép kiểu)
12. ✅ Wrapper Classes (Integer, Double, Character, Boolean)
13. ✅ Scope của biến (local, instance, static)
14. ✅ null và NullPointerException
15. ✅ StringBuilder (hiệu quả hơn String)
16. ✅ Math class (các hàm toán học)
17. ✅ Comment trong Java (single-line, multi-line, JavaDoc)

**Phần OOP & Môi trường:**
1. ✅ Sự khác biệt giữa Procedural và OOP
2. ✅ Các khái niệm cốt lõi của OOP
3. ✅ Thiết lập môi trường Java (JDK, IDE, Maven)
4. ✅ Quản lý phiên bản với Git/GitHub
5. ✅ Clean Code và Naming Convention
6. ✅ JavaDoc và Comment best practices

### Kỹ năng đã có:
- ✅ Viết chương trình Java cơ bản
- ✅ Sử dụng cấu trúc điều khiển và vòng lặp
- ✅ Làm việc với mảng và String
- ✅ Tạo và gọi methods
- ✅ Nhập/xuất dữ liệu
- ✅ Cài đặt và cấu hình JDK
- ✅ Sử dụng Git cơ bản
- ✅ Viết code sạch, dễ đọc
- ✅ Tài liệu hóa code với JavaDoc

---

## 🎯 BÀI TẬP CHƯƠNG 1

### 📌 PHẦN A: BÀI TẬP JAVA CƠ BẢN

#### Bài A1: Hello World và Cấu trúc cơ bản

**Yêu cầu:**
1. Tạo class `HelloWorld` in ra "Hello, World!"
2. Thêm method `greet(String name)` in ra "Hello, [name]!"
3. Gọi method `greet()` từ `main()`

**Test:**
```java
HelloWorld.main(null);
// Output:
// Hello, World!
// Hello, Alice!
```

---

#### Bài A2: Kiểu dữ liệu và Biến

**Yêu cầu:**
1. Khai báo các biến với các kiểu dữ liệu khác nhau:
   - `int age = 20`
   - `double gpa = 3.75`
   - `char grade = 'A'`
   - `boolean isActive = true`
   - `String name = "John"`
2. In ra giá trị của tất cả biến
3. Thực hiện các phép toán: cộng, trừ, nhân, chia

---

#### Bài A3: Cấu trúc điều khiển

**Yêu cầu:**
Viết chương trình `GradeCalculator`:
1. Nhập điểm số (0-100)
2. Tính và in ra grade:
   - 90-100: A
   - 80-89: B
   - 70-79: C
   - 60-69: D
   - <60: F
3. Dùng if-else và switch-case

---

#### Bài A4: Vòng lặp

**Yêu cầu:**
1. In ra các số từ 1 đến 10 (dùng for)
2. In ra các số chẵn từ 2 đến 20 (dùng while)
3. Tính tổng các số từ 1 đến 100
4. In ra bảng cửu chương 9 (dùng vòng lặp lồng nhau)

---

#### Bài A5: Mảng

**Yêu cầu:**
1. Tạo mảng số nguyên chứa 10 phần tử
2. Nhập giá trị cho mảng từ bàn phím
3. Tìm số lớn nhất, nhỏ nhất
4. Tính tổng và trung bình
5. Sắp xếp mảng (dùng thuật toán đơn giản)

---

#### Bài A6: Method

**Yêu cầu:**
Tạo class `MathUtils` với các methods:
1. `add(int a, int b)`: Cộng 2 số
2. `multiply(int a, int b)`: Nhân 2 số
3. `isEven(int n)`: Kiểm tra số chẵn
4. `max(int a, int b)`: Tìm số lớn hơn
5. `factorial(int n)`: Tính giai thừa

**Test:**
```java
MathUtils utils = new MathUtils();
System.out.println(utils.add(5, 3));        // 8
System.out.println(utils.isEven(4));         // true
System.out.println(utils.factorial(5));      // 120
```

---

#### Bài A7: String

**Yêu cầu:**
1. Nhập một chuỗi từ bàn phím
2. In ra độ dài chuỗi
3. Chuyển thành chữ hoa và chữ thường
4. Đảo ngược chuỗi
5. Kiểm tra chuỗi có phải palindrome không (đối xứng)

**Ví dụ:**
```
Input: "Hello"
Output:
- Length: 5
- Uppercase: HELLO
- Lowercase: hello
- Reversed: olleH
- Is palindrome: false

Input: "madam"
- Is palindrome: true
```

---

#### Bài A8: Input/Output

**Yêu cầu:**
Tạo chương trình `StudentInfo`:
1. Nhập thông tin sinh viên:
   - Tên
   - Tuổi
   - Điểm Toán
   - Điểm Lý
   - Điểm Hóa
2. Tính điểm trung bình
3. In ra thông tin đầy đủ với format đẹp

**Output mẫu:**
```
=== STUDENT INFORMATION ===
Name: Nguyen Van A
Age: 20
Math: 8.5
Physics: 9.0
Chemistry: 8.0
Average: 8.5
```

---

#### Bài A9: Tổng hợp - Máy tính đơn giản

**Yêu cầu:**
Tạo chương trình `Calculator` với menu:
```
=== CALCULATOR ===
1. Addition
2. Subtraction
3. Multiplication
4. Division
5. Exit
Choose an option:
```

**Chức năng:**
- Nhập 2 số và phép toán
- Thực hiện phép tính
- Hiển thị kết quả
- Lặp lại cho đến khi chọn Exit

---

#### Bài A10: Tổng hợp - Quản lý Danh sách

**Yêu cầu:**
Tạo chương trình `ListManager` quản lý danh sách số nguyên:
1. Thêm số vào danh sách
2. Xóa số khỏi danh sách
3. Tìm số trong danh sách
4. Hiển thị tất cả số
5. Tính tổng, trung bình
6. Tìm số lớn nhất, nhỏ nhất

**Gợi ý:** Dùng mảng và các vòng lặp

---

### 📌 PHẦN B: BÀI TẬP OOP & MÔI TRƯỜNG

### Bài 1: So sánh Procedural vs OOP

**Yêu cầu:**
Viết 2 chương trình tính diện tích và chu vi hình chữ nhật:
1. **Cách Procedural**: Dùng struct và hàm
2. **Cách OOP**: Dùng class

**Gợi ý:**
- Procedural: struct Rectangle + hàm calculateArea(), calculatePerimeter()
- OOP: class Rectangle với methods calculateArea(), calculatePerimeter()

**Đánh giá:**
- So sánh 2 cách, chỉ ra ưu/nhược điểm
- Giải thích tại sao OOP tốt hơn trong trường hợp này

---

### Bài 2: Thiết lập Môi trường

**Yêu cầu:**
1. Cài đặt JDK 17 hoặc 21
2. Kiểm tra cài đặt: `java -version`, `javac -version`
3. Cài đặt Maven: `mvn -version`
4. Tạo project Maven với cấu trúc chuẩn
5. Tạo file `HelloWorld.java` và chạy thành công

**Deliverable:**
- Screenshot kết quả các lệnh kiểm tra
- Cấu trúc thư mục project
- File `pom.xml`
- File `HelloWorld.java` chạy được

---

### Bài 3: Git Workflow

**Yêu cầu:**
1. Tạo repository Git local
2. Tạo file `README.md` với nội dung giới thiệu project
3. Commit với message: "Initial commit: Add README"
4. Tạo file `HelloWorld.java`
5. Commit với message: "Add HelloWorld class"
6. Xem lịch sử commit: `git log --oneline`
7. (Tùy chọn) Tạo repository trên GitHub và push code lên

**Deliverable:**
- Screenshot `git log --oneline`
- File `.gitignore` (nếu có)
- Link GitHub repository (nếu có)

---

### Bài 4: Clean Code - Refactoring

**Cho đoạn code sau (Code xấu):**
```java
public class Calc {
    public double c(double p, double r, int t) {
        return p * Math.pow(1 + r/100, t);
    }
    
    public void p(String n, int a) {
        System.out.println("Name: " + n + ", Age: " + a);
    }
}
```

**Yêu cầu:**
1. Refactor code để tuân thủ Clean Code
2. Đặt tên rõ ràng, mô tả
3. Thêm JavaDoc đầy đủ
4. Thêm constants cho magic numbers (nếu có)
5. Viết comment giải thích "TẠI SAO" (nếu cần)

**Deliverable:**
- Code sau khi refactor
- Giải thích các thay đổi

---

### Bài 5: JavaDoc

**Yêu cầu:**
Tạo class `Calculator` với các methods sau và viết JavaDoc đầy đủ:

```java
public class Calculator {
    // Thêm JavaDoc cho class
    
    public double add(double a, double b) {
        // Thêm JavaDoc
    }
    
    public double subtract(double a, double b) {
        // Thêm JavaDoc
    }
    
    public double multiply(double a, double b) {
        // Thêm JavaDoc
    }
    
    public double divide(double a, double b) {
        // Thêm JavaDoc (lưu ý: chia cho 0)
    }
}
```

**Yêu cầu JavaDoc:**
- Mô tả class
- Mô tả từng method
- @param cho mỗi tham số
- @return cho giá trị trả về
- @throws cho exception (nếu có)
- Ví dụ sử dụng (nếu cần)

**Deliverable:**
- File `Calculator.java` với JavaDoc đầy đủ
- Generate JavaDoc và xem kết quả: `mvn javadoc:javadoc`

---

### Bài 6: Tổng hợp - Tạo Project Đầu tiên

**Yêu cầu:**
Tạo một project Java hoàn chỉnh với:

1. **Cấu trúc Maven chuẩn**
2. **Git repository** với ít nhất 3 commits
3. **Class `Student`** với:
   - Fields: name, age, gpa
   - Constructors, getters, setters
   - Method `displayInfo()` in thông tin
   - JavaDoc đầy đủ
4. **Class `Main`** để test
5. **README.md** mô tả project
6. **Code sạch**, tuân thủ naming convention

**Deliverable:**
- Toàn bộ source code
- `pom.xml`
- `README.md`
- Screenshot kết quả chạy chương trình
- Link GitHub (nếu có)

---

## 📚 TÀI LIỆU THAM KHẢO

1. **Clean Code** - Robert C. Martin
2. **Effective Java** - Joshua Bloch
3. **Java Naming Conventions**: https://www.oracle.com/java/technologies/javase/codeconventions-namingconventions.html
4. **Git Documentation**: https://git-scm.com/doc
5. **Maven Guide**: https://maven.apache.org/guides/

---

**Chúc bạn học tốt! 🚀**

