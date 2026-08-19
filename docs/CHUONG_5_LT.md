# CHƯƠNG 5: XỬ LÝ NGOẠI LỆ (EXCEPTIONS) & KIỂM THỬ (TESTING)

## 📚 MỤC TIÊU HỌC TẬP

Sau khi hoàn thành chương này, bạn sẽ:
- Phân biệt được Checked và Unchecked Exceptions
- Xử lý Exception đúng cách với try-catch-finally
- Sử dụng Try-with-resources để quản lý tài nguyên
- Tạo Custom Exception
- Viết Unit Test với JUnit 5
- Áp dụng Testing best practices

---

## 📋 KIẾN THỨC CẦN CÓ (PREREQUISITES)

Trước khi học chương này, bạn cần nắm vững:

- [ ] ✅ **Class và Object** (Chương 2)
  - Tạo class, định nghĩa methods
  - Access modifiers

- [ ] ✅ **Inheritance** (Chương 3, phần 3.1)
  - `extends`, `super`
  - Tạo custom class

> **💡 Lưu ý:** Exception handling và Testing là **kỹ năng bắt buộc** trong lập trình chuyên nghiệp. Hãy học kỹ phần này!

---

## 🎓 HƯỚNG DẪN HỌC SÂU & CODE CHẠY THỬ

| Chủ đề | Code | Output mong đợi |
|--------|------|-----------------|
| Custom Exception | `CODE/VI_DU/CHUONG_5/01_Exception/` | Rút 30 OK; rút 100 → in lỗi số dư |

```bash
cd CODE/VI_DU/CHUONG_5/01_Exception
javac Main.java && java Main
```

**Output thực tế:**

```
Rút thành công. Số dư còn: 70.0
Lỗi: Số dư không đủ. Cần: 100.0, có: 70.0
```

**Giảng sâu:** `withdraw` **ném** exception thay vì return false – caller **bắt buộc** xử lý (try-catch) hoặc khai báo `throws`. Message rõ ràng giúp debug và hiển thị cho user.

---

## 5.1. PHÂN LOẠI NGOẠI LỆ

### 5.1.1. Exception là gì?

#### Định nghĩa

**Exception (Ngoại lệ)** là sự kiện xảy ra trong quá trình thực thi chương trình làm gián đoạn luồng xử lý bình thường.

**Ví dụ:**
- Chia cho 0
- Truy cập file không tồn tại
- Kết nối database thất bại
- Mảng vượt quá chỉ số

> **🚨 Tưởng tượng: Máy báo cháy (Smoke Detector)**
> - Exception giống như việc máy báo cháy hú còi.
> - Nó **KHÔNG** dập lửa (không tự sửa lỗi).
> - Nó chỉ **BÁO ĐỘNG** để bạn biết có vấn đề và xử lý (chạy thoát thân hoặc lấy bình cứu hỏa).
> - Nếu bạn lờ đi (không catch), ngôi nhà (chương trình) sẽ cháy rụi (crash).

---

#### Exception Hierarchy trong Java

```
Throwable
├── Error (Không nên catch)
│   ├── OutOfMemoryError
│   └── StackOverflowError
│
└── Exception
    ├── RuntimeException (Unchecked)
    │   ├── NullPointerException
    │   ├── ArrayIndexOutOfBoundsException
    │   ├── IllegalArgumentException
    │   └── ArithmeticException
    │
    └── Checked Exceptions
        ├── IOException
        ├── FileNotFoundException
        ├── SQLException
        └── ClassNotFoundException
```

---

### 5.1.2. Checked vs Unchecked Exceptions

#### Checked Exceptions

**Đặc điểm:**
- Phải được xử lý (catch) hoặc khai báo (throws)
- Compiler bắt buộc kiểm tra
- Thường là lỗi ngoài tầm kiểm soát của programmer

**Ví dụ:**
```java
// ❌ LỖI: Phải xử lý hoặc throws
public void readFile() {
    FileReader file = new FileReader("file.txt");  // FileNotFoundException (Checked)
}

// ✅ Cách 1: Catch
public void readFile() {
    try {
        FileReader file = new FileReader("file.txt");
    } catch (FileNotFoundException e) {
        e.printStackTrace();
    }
}

// ✅ Cách 2: Throws
public void readFile() throws FileNotFoundException {
    FileReader file = new FileReader("file.txt");
}
```

**Các Checked Exceptions phổ biến:**
- `IOException`
- `FileNotFoundException`
- `SQLException`
- `ClassNotFoundException`

---

#### Unchecked Exceptions (RuntimeException)

**Đặc điểm:**
- Không bắt buộc phải catch hoặc throws
- Compiler không kiểm tra
- Thường là lỗi logic của programmer

**Ví dụ:**
```java
// ✅ OK: Không cần catch
public void divide(int a, int b) {
    int result = a / b;  // ArithmeticException nếu b = 0 (Unchecked)
}

// Có thể catch nếu muốn
public void divide(int a, int b) {
    try {
        int result = a / b;
    } catch (ArithmeticException e) {
        System.out.println("Cannot divide by zero!");
    }
}
```

**Các Unchecked Exceptions phổ biến:**
- `NullPointerException`
- `ArrayIndexOutOfBoundsException`
- `IllegalArgumentException`
- `ArithmeticException`

> **🚦 Tưởng tượng: Cảnh sát giao thông vs Biển báo công trường**
> - **Unchecked (Runtime)**: Giống như **vượt đèn đỏ**. Đây là lỗi của bạn (lỗi logic). Luật không bắt bạn phải đội mũ sắt để đề phòng, nhưng nếu vi phạm thì bị phạt (Exception).
> - **Checked**: Giống như gặp **biển "Đường đang thi công"**. Đây không phải lỗi của bạn, nhưng luật bắt buộc bạn phải chạy đường vòng (catch/throws). Bạn phải chuẩn bị tinh thần xử lý tình huống này trước khi ra đường.

---

#### So sánh Checked vs Unchecked

| Đặc điểm | Checked | Unchecked |
|----------|---------|-----------|
| **Phải catch/throws?** | ✅ Có | ❌ Không |
| **Compiler kiểm tra?** | ✅ Có | ❌ Không |
| **Kế thừa từ** | Exception | RuntimeException |
| **Khi nào xảy ra** | Runtime (nhưng có thể dự đoán) | Runtime (lỗi logic) |
| **Ví dụ** | FileNotFoundException | NullPointerException |

---

### 5.1.3. Khi nào nên bắt lỗi, ném lỗi, tự định nghĩa Exception?

#### Khi nào nên BẮT lỗi (catch)?

**✅ BẮT khi:**
- Có thể xử lý và tiếp tục chương trình
- Cần log lỗi để debug
- Cần thông báo cho user

**Ví dụ:**
```java
public void readConfigFile() {
    try {
        FileReader file = new FileReader("config.txt");
        // Đọc file
    } catch (FileNotFoundException e) {
        // Có thể xử lý: Dùng config mặc định
        System.out.println("Config file not found, using defaults");
        useDefaultConfig();
    }
}
```

---

#### Khi nào nên NÉM lỗi (throw/throws)?

**✅ NÉM khi:**
- Không thể xử lý tại chỗ
- Muốn để caller xử lý
- Lỗi quan trọng, không thể bỏ qua

**Ví dụ:**
```java
// Method không thể xử lý lỗi
public void connectToDatabase() throws SQLException {
    // Kết nối database
    // Nếu lỗi → Ném lên cho caller xử lý
    Connection conn = DriverManager.getConnection(url);
}

// Caller xử lý
public void initialize() {
    try {
        connectToDatabase();
    } catch (SQLException e) {
        // Xử lý ở đây
        System.err.println("Database connection failed!");
    }
}
```

---

#### Khi nào nên TỰ ĐỊNH NGHĨA Exception?

**✅ TỰ ĐỊNH NGHĨA khi:**
- Cần exception riêng cho business logic
- Muốn thông điệp lỗi rõ ràng hơn
- Cần thêm thông tin vào exception

**Ví dụ:**
```java
// Custom Exception
public class InsufficientBalanceException extends Exception {
    private double balance;
    private double amount;
    
    public InsufficientBalanceException(double balance, double amount) {
        super("Insufficient balance. Current: " + balance + ", Required: " + amount);
        this.balance = balance;
        this.amount = amount;
    }
    
    public double getBalance() { return balance; }
    public double getAmount() { return amount; }
}

// Sử dụng
public void withdraw(double amount) throws InsufficientBalanceException {
    if (amount > balance) {
        throw new InsufficientBalanceException(balance, amount);
    }
    balance -= amount;
}
```

---

## 5.2. CƠ CHẾ XỬ LÝ EXCEPTION

### 5.2.1. Try-Catch-Finally

#### Cú pháp cơ bản

```java
try {
    // Code có thể ném exception
} catch (ExceptionType e) {
    // Xử lý exception
} finally {
    // Code luôn chạy (dù có exception hay không)
}
```

---

#### Ví dụ cơ bản

```java
public void divide(int a, int b) {
    try {
        int result = a / b;
        System.out.println("Result: " + result);
    } catch (ArithmeticException e) {
        System.out.println("Error: Cannot divide by zero!");
        e.printStackTrace();
    } finally {
        System.out.println("Division operation completed");
    }
}

// Test
divide(10, 2);   // Result: 5, Division operation completed
divide(10, 0);    // Error: Cannot divide by zero!, Division operation completed
```

### 📉 Phân tích Luồng nhảy (Jump Flow)

Khi `divide(10, 0)` chạy, JVM thực hiện cú nhảy ngoạn mục:

1.  **Dòng 276**: `int result = 10 / 0;` → 🔥 **BÙM!** Lỗi `ArithmeticException`.
2.  **Java dừng ngay**: Dòng 277 (`println Result`) **KHÔNG BAO GIỜ** được chạy.
3.  **Nhảy cầu (Jump)**: Java tìm ngay block `catch` phù hợp. Nó thấy dòng 278 bắt `ArithmeticException`.
4.  **Đáp đất**: Chạy code trong block `catch` (dòng 279-280). In ra "Error...".
5.  **Chốt hạ**: Dù có lỗi hay không, `finally` (dòng 281) **LUÔN LUÔN** chạy để dọn dẹp hiện trường.

> **💡 Bài học:** Hãy coi `try-catch` như một gỡ bom. `try` là lúc cắt dây bom. Nếu nổ (`Exception`), bạn bị văng ra xa và rơi vào lưới đỡ (`catch`), chứ không chết (`Crash App`).


---

#### Multiple Catch Blocks

**Có thể catch nhiều loại exception:**

```java
try {
    // Code có thể ném nhiều loại exception
    int[] arr = new int[5];
    int value = arr[10];  // ArrayIndexOutOfBoundsException
    String str = null;
    int len = str.length();  // NullPointerException
} catch (ArrayIndexOutOfBoundsException e) {
    System.out.println("Array index out of bounds!");
} catch (NullPointerException e) {
    System.out.println("Null pointer exception!");
} catch (Exception e) {
    System.out.println("Other exception: " + e.getMessage());
}
```

**Lưu ý:**
- Catch từ cụ thể → chung chung
- `Exception` phải ở cuối cùng

---

#### Multi-Catch (Java 7+)

**Có thể catch nhiều exception trong một block:**

```java
try {
    // Code
} catch (FileNotFoundException | IOException e) {
    // Xử lý cả 2 loại exception
    System.out.println("File error: " + e.getMessage());
}
```

---

### 5.2.2. Try-with-Resources

#### Vấn đề với Finally

**Code cũ (phức tạp):**
```java
FileReader file = null;
try {
    file = new FileReader("file.txt");
    // Đọc file
} catch (IOException e) {
    e.printStackTrace();
} finally {
    if (file != null) {
        try {
            file.close();  // Phải close trong finally
        } catch (IOException e) {
            e.printStackTrace();
        }
    }
}
```

**Vấn đề:**
- ❌ Code dài, phức tạp
- ❌ Dễ quên close
- ❌ Nested try-catch

---

#### Giải pháp: Try-with-Resources (Java 7+)

**Cú pháp:**
```java
try (ResourceType resource = new ResourceType()) {
    // Sử dụng resource
} catch (Exception e) {
    // Xử lý exception
}
// Resource tự động được close
```

**Ví dụ:**
```java
// ✅ TỐT: Tự động close
try (FileReader file = new FileReader("file.txt")) {
    // Đọc file
    // File tự động close khi ra khỏi try block
} catch (IOException e) {
    e.printStackTrace();
}
// Không cần finally để close!
```

> **🚪 Tưởng tượng: Cửa tự đóng (Spring-loaded Door)**
> - Try-with-resources giống như cái cửa có lò xo tự đóng.
> - Bạn đi vào (mở file), làm gì thì làm.
> - Khi bạn bước ra (kết thúc block), cửa tự động đóng "RẦM" lại sau lưng. Bạn không cần quay lại đóng cửa (gọi `close()`). An toàn tuyệt đối!
```

---

#### AutoCloseable Interface

**Resource phải implement `AutoCloseable`:**

```java
public interface AutoCloseable {
    void close() throws Exception;
}
```

**Các class đã implement:**
- `FileReader`, `FileWriter`
- `BufferedReader`, `BufferedWriter`
- `Scanner`
- `Connection` (JDBC)
- `InputStream`, `OutputStream`

---

#### Multiple Resources

**Có thể khai báo nhiều resources:**

```java
try (
    FileReader reader = new FileReader("input.txt");
    FileWriter writer = new FileWriter("output.txt")
) {
    // Sử dụng reader và writer
    // Cả 2 tự động close (theo thứ tự ngược lại)
} catch (IOException e) {
    e.printStackTrace();
}
```

---

### 5.2.3. Best Practices: Không bao giờ "nuốt" lỗi

#### Vấn đề: Swallowing Exceptions

**❌ TỒI: Nuốt exception (không làm gì)**
```java
try {
    // Code có thể lỗi
    processData();
} catch (Exception e) {
    // Không làm gì - NGUY HIỂM!
}
```

**Vấn đề:**
- ❌ Lỗi bị che giấu
- ❌ Khó debug
- ❌ Chương trình chạy sai nhưng không biết

---

#### ✅ TỐT: Xử lý exception đúng cách

**1. Log exception:**
```java
try {
    processData();
} catch (Exception e) {
    logger.error("Error processing data", e);  // Log để debug
    // Hoặc
    e.printStackTrace();  // Ít nhất in ra console
}
```

**2. Thông báo cho user:**
```java
try {
    processData();
} catch (Exception e) {
    System.out.println("An error occurred. Please try again.");
    logger.error("Error", e);  // Log chi tiết cho developer
}
```

**3. Ném lại nếu không thể xử lý:**
```java
try {
    processData();
} catch (Exception e) {
    logger.error("Error", e);
    throw new RuntimeException("Failed to process data", e);  // Ném lại
}
```

**4. Wrap exception:**
```java
try {
    processData();
} catch (IOException e) {
    throw new DataProcessingException("Failed to process data", e);  // Wrap
}
```

---

## 5.3. UNIT TESTING – TƯ DUY KIỂM THỬ HIỆN ĐẠI

### 5.3.1. Unit Testing là gì?

#### Định nghĩa

**Unit Test** là kiểm thử từng đơn vị code (method, class) một cách độc lập.

**Đặc điểm:**
- Test một đơn vị nhỏ nhất
- Chạy nhanh
- Độc lập (không phụ thuộc nhau)
- Có thể chạy tự động

---

#### Tại sao cần Unit Test?

**Lợi ích:**
- ✅ Phát hiện lỗi sớm
- ✅ Tự tin refactor code
- ✅ Tài liệu sống (code làm gì)
- ✅ Tăng chất lượng code

> **🎪 Tưởng tượng: Lưới an toàn (Safety Net)**
> - Viết code mà không có Unit Test giống như diễn viên xiếc đi trên dây mà **không có lưới bảo vệ**. Sơ sẩy là "chết" (bug production).
> - Khi có Unit Test, bạn tự tin nhảy múa, nhào lộn (Refactor code). Nếu lỡ trượt chân, tấm lưới (Test fail) sẽ đỡ bạn ngay lập tức. Bạn biết mình sai ở đâu và leo lên đi lại.

**Ví dụ:**
```java
// Code cần test
public class Calculator {
    public int add(int a, int b) {
        return a + b;
    }
}

// Unit Test
@Test
public void testAdd() {
    Calculator calc = new Calculator();
    int result = calc.add(2, 3);
    assertEquals(5, result);  // Kiểm tra kết quả
}
```

---

### 5.3.2. JUnit 5 - Framework Testing

#### Giới thiệu JUnit 5

**JUnit 5** là framework testing phổ biến nhất cho Java.

**Cài đặt (Maven):**
```xml
<dependencies>
    <dependency>
        <groupId>org.junit.jupiter</groupId>
        <artifactId>junit-jupiter</artifactId>
        <version>5.10.0</version>
        <scope>test</scope>
    </dependency>
</dependencies>
```

---

#### Cấu trúc Test cơ bản

```java
import org.junit.jupiter.api.Test;
import static org.junit.jupiter.api.Assertions.*;

public class CalculatorTest {
    
    @Test
    public void testAdd() {
        // Arrange (Chuẩn bị)
        Calculator calc = new Calculator();
        
        // Act (Thực hiện)
        int result = calc.add(2, 3);
        
        // Assert (Kiểm tra)
        assertEquals(5, result);
    }
}
```

### 🛡️ Phân tích Chiến thuật AAA

Tại sao lại phải chia làm 3 bước `Arrange`, `Act`, `Assert`?

1.  **Arrange (Chuẩn bị)**: Thiết lập bối cảnh. Giống như việc bạn bày biện nguyên liệu trước khi nấu ăn.
    *   *Tại sao?* Để đảm bảo môi trường test là sạch sẽ và cô lập (Isolated).
2.  **Act (Hành động)**: Chỉ gọi **ĐÚNG 1** hàm cần test.
    *   *Tại sao?* Nếu lỗi xảy ra, bạn biết ngay do hàm này gây ra, không phải do các bước rườm rà khác.
3.  **Assert (Khẳng định)**: So sánh `Thực tế` (Result) với `Kỳ vọng` (Expected).
    *   *Tại sao?* Đây là chốt chặn cuối cùng. Nếu `Assert` sai, code bạn có vấn đề logic. Nó hoạt động như một "Cam kết bảo hành".

> **⚠️ Sai lầm thường gặp:** Một số bạn viết code `sout` (System.out.println) trong test để nhìn bằng mắt. Đừng làm thế! Hãy để `Assert` tự động phán xét Đúng/Sai.


**Cấu trúc AAA:**
- **Arrange**: Chuẩn bị dữ liệu
- **Act**: Thực hiện hành động
- **Assert**: Kiểm tra kết quả

---

#### Các Assertions phổ biến

```java
import static org.junit.jupiter.api.Assertions.*;

@Test
public void testAssertions() {
    // assertEquals - So sánh bằng
    assertEquals(5, 2 + 3);
    assertEquals("Hello", "Hello");
    
    // assertNotEquals - So sánh khác
    assertNotEquals(5, 2 + 2);
    
    // assertTrue - Kiểm tra true
    assertTrue(5 > 3);
    
    // assertFalse - Kiểm tra false
    assertFalse(5 < 3);
    
    // assertNull - Kiểm tra null
    String str = null;
    assertNull(str);
    
    // assertNotNull - Kiểm tra không null
    String str2 = "Hello";
    assertNotNull(str2);
    
    // assertSame - Cùng object
    Object obj1 = new Object();
    Object obj2 = obj1;
    assertSame(obj1, obj2);
    
    // assertNotSame - Khác object
    Object obj3 = new Object();
    assertNotSame(obj1, obj3);
    
    // assertThrows - Kiểm tra exception
    assertThrows(ArithmeticException.class, () -> {
        int result = 10 / 0;
    });
}
```

---

### 5.3.3. Kỹ thuật viết Test Case

#### Test Case tốt

**Đặc điểm:**
- ✅ Tên test mô tả rõ ràng
- ✅ Test một điều duy nhất
- ✅ Độc lập (không phụ thuộc test khác)
- ✅ Có thể chạy nhiều lần (repeatable)

**Ví dụ:**
```java
@Test
public void testAdd_PositiveNumbers_ReturnsSum() {
    // Arrange
    Calculator calc = new Calculator();
    
    // Act
    int result = calc.add(5, 3);
    
    // Assert
    assertEquals(8, result);
}

@Test
public void testAdd_NegativeNumbers_ReturnsSum() {
    Calculator calc = new Calculator();
    int result = calc.add(-5, -3);
    assertEquals(-8, result);
}

@Test
public void testAdd_Zero_ReturnsOtherNumber() {
    Calculator calc = new Calculator();
    int result = calc.add(5, 0);
    assertEquals(5, result);
}
```

---

#### Kiểm thử Biên (Boundary Testing)

**Test các giá trị biên:**
```java
@Test
public void testCalculateDiscount_BoundaryValues() {
    DiscountCalculator calc = new DiscountCalculator();
    
    // Biên dưới
    assertEquals(0, calc.calculateDiscount(0));
    
    // Biên trên
    assertEquals(50, calc.calculateDiscount(1000));
    
    // Ngay dưới biên
    assertEquals(0, calc.calculateDiscount(99));
    
    // Ngay trên biên
    assertEquals(10, calc.calculateDiscount(100));
    
    // Giữa biên
    assertEquals(20, calc.calculateDiscount(500));
}
```

---

#### Kiểm thử Lớp tương đương (Equivalence Partitioning)

**Chia input thành các lớp tương đương:**

```java
// Method: calculateGrade(double score)
// Lớp 1: 0-49 → "F"
// Lớp 2: 50-59 → "D"
// Lớp 3: 60-69 → "C"
// Lớp 4: 70-79 → "B"
// Lớp 5: 80-100 → "A"

@Test
public void testCalculateGrade_ClassF() {
    GradeCalculator calc = new GradeCalculator();
    assertEquals("F", calc.calculateGrade(0));
    assertEquals("F", calc.calculateGrade(25));
    assertEquals("F", calc.calculateGrade(49));
}

@Test
public void testCalculateGrade_ClassA() {
    GradeCalculator calc = new GradeCalculator();
    assertEquals("A", calc.calculateGrade(80));
    assertEquals("A", calc.calculateGrade(90));
    assertEquals("A", calc.calculateGrade(100));
}
```

---

#### Test Exception

**Kiểm tra method ném exception:**

```java
@Test
public void testDivide_ByZero_ThrowsException() {
    Calculator calc = new Calculator();
    
    // Kiểm tra ném ArithmeticException
    assertThrows(ArithmeticException.class, () -> {
        calc.divide(10, 0);
    });
}

@Test
public void testWithdraw_InsufficientBalance_ThrowsException() {
    BankAccount account = new BankAccount(100.0);
    
    InsufficientBalanceException exception = assertThrows(
        InsufficientBalanceException.class,
        () -> account.withdraw(200.0)
    );
    
    // Kiểm tra message
    assertEquals("Insufficient balance", exception.getMessage());
}
```

---

### 5.3.4. Sử dụng Assert thay vì System.out.println()

#### ❌ TỒI: Dùng System.out.println()

```java
public void testAdd() {
    Calculator calc = new Calculator();
    int result = calc.add(2, 3);
    System.out.println("Result: " + result);  // ❌ Phải tự kiểm tra bằng mắt
    // Không tự động phát hiện lỗi
}
```

**Vấn đề:**
- ❌ Phải tự kiểm tra bằng mắt
- ❌ Không tự động phát hiện lỗi
- ❌ Không thể chạy tự động trong CI/CD

---

#### ✅ TỐT: Dùng Assert

```java
@Test
public void testAdd() {
    Calculator calc = new Calculator();
    int result = calc.add(2, 3);
    assertEquals(5, result);  // ✅ Tự động kiểm tra
    // Nếu sai → Test fail ngay
}
```

**Lợi ích:**
- ✅ Tự động kiểm tra
- ✅ Fail ngay nếu sai
- ✅ Có thể chạy tự động

---

### 5.3.5. @BeforeEach và @AfterEach

#### Setup và Teardown

**@BeforeEach: Chạy trước mỗi test**
```java
public class BankAccountTest {
    private BankAccount account;
    
    @BeforeEach
    public void setUp() {
        // Chạy trước mỗi test
        account = new BankAccount(1000.0);
    }
    
    @Test
    public void testDeposit() {
        account.deposit(500.0);
        assertEquals(1500.0, account.getBalance());
    }
    
    @Test
    public void testWithdraw() {
        account.withdraw(300.0);
        assertEquals(700.0, account.getBalance());
    }
}
```

**@AfterEach: Chạy sau mỗi test**
```java
@AfterEach
public void tearDown() {
    // Cleanup sau mỗi test
    account = null;
}
```

---

## 5.4. FILE I/O CƠ BẢN

### 5.4.1. File, Path, Files (NIO.2)

#### Java NIO.2 (Java 7+)

**NIO.2** cung cấp API hiện đại để làm việc với file.

**Import:**
```java
import java.nio.file.Path;
import java.nio.file.Paths;
import java.nio.file.Files;
import java.nio.file.StandardOpenOption;
```

---

#### Path và Paths

**Tạo Path:**
```java
// Cách 1: Dùng Paths.get()
Path path1 = Paths.get("file.txt");
Path path2 = Paths.get("/home/user/file.txt");
Path path3 = Paths.get("C:", "Users", "user", "file.txt");

// Cách 2: Dùng Path.of() (Java 11+)
Path path4 = Path.of("file.txt");
```

---

#### Files - Utility Class

**Đọc file:**
```java
Path path = Paths.get("file.txt");

try {
    // Đọc toàn bộ file thành String
    String content = Files.readString(path);
    
    // Đọc thành List<String> (mỗi dòng một phần tử)
    List<String> lines = Files.readAllLines(path);
    
    // Đọc thành byte[]
    byte[] bytes = Files.readAllBytes(path);
} catch (IOException e) {
    e.printStackTrace();
}
```

**Ghi file:**
```java
Path path = Paths.get("output.txt");

try {
    // Ghi String
    String content = "Hello, World!";
    Files.writeString(path, content);
    
    // Ghi List<String>
    List<String> lines = Arrays.asList("Line 1", "Line 2");
    Files.write(path, lines);
    
    // Ghi byte[]
    byte[] data = "Hello".getBytes();
    Files.write(path, data);
    
    // Append (thêm vào cuối)
    Files.writeString(path, "\nNew line", StandardOpenOption.APPEND);
} catch (IOException e) {
    e.printStackTrace();
}
```

---

#### Try-with-Resources với I/O

**Đọc file với BufferedReader:**
```java
Path path = Paths.get("file.txt");

try (BufferedReader reader = Files.newBufferedReader(path)) {
    String line;
    while ((line = reader.readLine()) != null) {
        System.out.println(line);
    }
} catch (IOException e) {
    e.printStackTrace();
}
```

**Ghi file với BufferedWriter:**
```java
Path path = Paths.get("output.txt");

try (BufferedWriter writer = Files.newBufferedWriter(path)) {
    writer.write("Line 1");
    writer.newLine();
    writer.write("Line 2");
} catch (IOException e) {
    e.printStackTrace();
}
```

---

### 5.4.2. Xử lý Exception trong I/O

#### Best Practices

**1. Luôn dùng try-with-resources:**
```java
// ✅ TỐT
try (BufferedReader reader = Files.newBufferedReader(path)) {
    // Đọc file
} catch (IOException e) {
    logger.error("Error reading file", e);
}
```

**2. Xử lý exception cụ thể:**
```java
try {
    Files.readString(path);
} catch (FileNotFoundException e) {
    // File không tồn tại
    System.out.println("File not found: " + path);
} catch (IOException e) {
    // Lỗi I/O khác
    logger.error("IO error", e);
}
```

**3. Kiểm tra file tồn tại:**
```java
Path path = Paths.get("file.txt");

if (Files.exists(path)) {
    try {
        String content = Files.readString(path);
    } catch (IOException e) {
        e.printStackTrace();
    }
} else {
    System.out.println("File does not exist");
}
```

---

## 5.5. ADVANCED I/O: OBJECT SERIALIZATION

### 5.5.1. Serialization là gì?
**Serialization** (Tuần tự hóa) là quá trình chuyển đổi trạng thái của một Object thành byte stream để:
- Lưu vào file (Persistence)
- Gửi qua mạng (Network)
- Lưu vào Database

**Deserialization** là quá trình ngược lại: Byte stream  → Object.

---

### 5.5.2. Interface Serializable

Để một class có thể serialize, nó phải implements interface `java.io.Serializable`.
Đây là một **Marker Interface** (không có method nào).

```java
import java.io.Serializable;

public class Student implements Serializable {
    private static final long serialVersionUID = 1L; // Version control
    
    private String name;
    private int age;
    private transient String password; // transient: KHÔNG serialize trường này
    
    // Contructors, getters, setters...
}
```

#### Từ khóa quan trọng:
- `serialVersionUID`: ID để đảm bảo phiên bản class khi đọc/ghi là khớp nhau.
- `transient`: Đánh dấu biến **không** muốn lưu (ví dụ: mật khẩu, dữ liệu tạm).

---

### 5.5.3. Ghi và Đọc Object

Dùng `ObjectOutputStream` và `ObjectInputStream`.

**Ghi Object (Serialization):**
```java
Student s1 = new Student("Nam", 20, "secret123");

try (ObjectOutputStream oos = new ObjectOutputStream(
        new FileOutputStream("student.dat"))) {
    oos.writeObject(s1);
    System.out.println("Đã lưu student!");
} catch (IOException e) {
    e.printStackTrace();
}
```

**Đọc Object (Deserialization):**
```java
try (ObjectInputStream ois = new ObjectInputStream(
        new FileInputStream("student.dat"))) {
    Student s2 = (Student) ois.readObject(); // Cần ép kiểu
    System.out.println("Đã đọc: " + s2.getName());
} catch (IOException | ClassNotFoundException e) {
    e.printStackTrace();
}
```

> **⚠️ Cảnh báo:** Serialization mặc định của Java có nhiều vấn đề về bảo mật và hiệu năng. Trong các dự án hiện đại (nhất là Web/API), người ta thường dùng **JSON** (Gson, Jackson) thay vì Serialization này. Tuy nhiên, hiểu về nó là cần thiết.

---

## 📊 BẢNG SO SÁNH & TRA CỨU NHANH (CHƯƠNG 5)

### Checked vs Unchecked

| | Checked | Unchecked |
|--|---------|-----------|
| **Kế thừa** | `Exception` (không phải RuntimeException) | `RuntimeException` |
| **Bắt buộc xử lý?** | Có (try-catch hoặc throws) | Không |
| **Ví dụ** | IOException, SQLException | NullPointerException, IllegalArgumentException |
| **Khi nào** | Lỗi có thể phục hồi, bên ngoài kiểm soát | Lỗi lập trình, logic sai |

### Try-catch vs Try-with-resources

| | try-catch-finally | try-with-resources |
|--|-------------------|---------------------|
| **Đóng tài nguyên** | Phải finally hoặc tự close | Tự đóng (AutoCloseable) |
| **Dùng cho** | Logic xử lý lỗi chung | File, stream, connection |
| **Khuyến nghị** | Mọi exception | **Luôn ưu tiên** khi đọc/ghi file |

### Unit Test – AAA

| Bước | Ý nghĩa | Ví dụ |
|------|---------|-------|
| **Arrange** | Chuẩn bị dữ liệu | `Calculator calc = new Calculator()` |
| **Act** | Gọi method cần test | `int r = calc.add(2, 3)` |
| **Assert** | Kiểm tra kết quả | `assertEquals(5, r)` |

---

## 🔗 CẦU NỐI SANG CHƯƠNG 6

| Chương 5 | Chương 6 | Liên hệ |
|----------|----------|---------|
| Đọc file (List dòng) | `List<String>` + Stream | Xử lý từng dòng bằng stream |
| Exception khi parse số | `map`, `filter` an toàn | Validate trước khi đưa vào collection |
| Unit test | Test logic trên List | Assert kết quả sau `collect()` |

**Nhắc lại:** Trước khi vào Ch.6, ôn **Wrapper Classes** (Ch.1) – `List<Integer>` không dùng `int`.

---

## 📝 TÓM TẮT CHƯƠNG 5

### Kiến thức đã học:
1. ✅ Checked vs Unchecked Exceptions
2. ✅ Try-catch-finally & Try-with-resources
3. ✅ Custom Exception
4. ✅ Unit Testing với JUnit 5
5. ✅ Boundary Testing và Equivalence Partitioning
6. ✅ File I/O với NIO.2
7. ✅ **Object Serialization (Lưu trữ đối tượng)**

### Kỹ năng đã có:
- ✅ Xử lý Exception đúng cách
- ✅ Viết Unit Test chuyên nghiệp
- ✅ Đọc/ghi file với Java NIO.2
- ✅ Serialization Object
- ✅ Áp dụng Testing best practices

---

## 🎯 BÀI TẬP CHƯƠNG 5

### Bài 1: Exception Handling cơ bản

**Yêu cầu:**
Tạo class `Calculator` với method `divide(int a, int b)`:
1. Ném `ArithmeticException` nếu b = 0
2. Viết method test với try-catch
3. In thông báo lỗi thân thiện

**Test:**
```java
Calculator calc = new Calculator();
calc.divide(10, 2);  // 5
calc.divide(10, 0);  // Exception với thông báo
```

---

### Bài 2: Custom Exception

**Yêu cầu:**
1. Tạo custom exception `InsufficientBalanceException`
2. Class `BankAccount` với method `withdraw(double amount)`
3. Ném exception nếu số dư không đủ
4. Test exception với JUnit

**Test:**
```java
BankAccount account = new BankAccount(100.0);
account.withdraw(50.0);   // OK
account.withdraw(100.0);  // InsufficientBalanceException
```

---

### Bài 3: Try-with-Resources

**Yêu cầu:**
1. Đọc file "input.txt"
2. Xử lý dữ liệu
3. Ghi kết quả vào "output.txt"
4. Sử dụng try-with-resources

**Yêu cầu:**
- Xử lý tất cả exceptions
- Đảm bảo file được close đúng cách

---

### Bài 4: Unit Test với JUnit 5

**Yêu cầu:**
Tạo class `StringUtils` với methods:
1. `reverse(String str)`: Đảo ngược chuỗi
2. `isPalindrome(String str)`: Kiểm tra đối xứng
3. `countWords(String str)`: Đếm số từ

**Viết Unit Test:**
- Test các trường hợp bình thường
- Test boundary cases (null, empty string)
- Test exception (nếu có)

---

### Bài 5: Boundary Testing

**Yêu cầu:**
Tạo class `GradeCalculator` với method `calculateGrade(double score)`:
- 0-49: "F"
- 50-59: "D"
- 60-69: "C"
- 70-79: "B"
- 80-100: "A"

**Viết test cho:**
- Các giá trị biên (0, 49, 50, 59, 60, ...)
- Giá trị ngoài phạm vi (< 0, > 100)

---

### Bài 6: File I/O

**Yêu cầu:**
1. Tạo class `FileManager` với methods:
   - `readFile(String filename)`: Đọc file
   - `writeFile(String filename, String content)`: Ghi file
   - `appendToFile(String filename, String content)`: Thêm vào cuối

2. Xử lý exceptions đầy đủ
3. Viết Unit Test

---

### Bài 7: Tổng hợp - Hệ thống Quản lý File với Testing

**Yêu cầu:**
Tạo hệ thống quản lý file với:

1. **Class `FileService`**:
   - `readFile(Path path)`: Đọc file
   - `writeFile(Path path, String content)`: Ghi file
   - `copyFile(Path source, Path target)`: Copy file
   - Xử lý tất cả exceptions

2. **Custom Exception `FileServiceException`**

3. **Unit Tests đầy đủ**:
   - Test đọc file thành công
   - Test đọc file không tồn tại
   - Test ghi file
   - Test copy file

**Yêu cầu:**
- Sử dụng try-with-resources
- Custom exception với thông tin chi tiết
- Test coverage tốt

---

## 📚 TÀI LIỆU THAM KHẢO

1. **JUnit 5 User Guide**: https://junit.org/junit5/docs/current/user-guide/
2. **Java NIO.2 Tutorial**: https://docs.oracle.com/javase/tutorial/essential/io/fileio.html
3. **Effective Java** - Joshua Bloch (Item 69-77: Exceptions)

---

**Chúc bạn học tốt! 🚀**

