# CHƯƠNG 6: COLLECTIONS & STREAM API – HIỆN ĐẠI HÓA JAVA

## 📚 MỤC TIÊU HỌC TẬP

Sau khi hoàn thành chương này, bạn sẽ:
- Sử dụng thành thạo Java Collections Framework
- Hiểu và áp dụng Generics
- Viết Lambda Expressions
- Sử dụng Functional Interfaces
- Xử lý dữ liệu với Stream API
- Chuyển đổi từ Imperative sang Declarative programming

---

## 📋 KIẾN THỨC CẦN CÓ (PREREQUISITES)

Trước khi học chương này, bạn cần nắm vững:

- [ ] ✅ **Mảng (Arrays)** (Chương 1, phần 0.7)
  - Khai báo, truy cập, duyệt mảng
  - Hiểu hạn chế của mảng (kích thước cố định)

- [ ] ✅ **Wrapper Classes** (Chương 1, phần 0.13) ⭐ **QUAN TRỌNG**
  - Integer, Double, String, Boolean, ...
  - Autoboxing và Unboxing
  - **Collections chỉ chấp nhận Wrapper Classes, không chấp nhận primitives!**

- [ ] ✅ **Interface** (Chương 3, phần 3.4)
  - Định nghĩa và implement interface
  - Functional Interfaces

- [ ] ✅ **Method** (Chương 1, 2)
  - Cách định nghĩa và gọi method
  - Tham số, return type

> **💡 Lưu ý:** 
> - Nếu bạn chưa học **Wrapper Classes** ở Chương 1, hãy quay lại học ngay! Collections không thể dùng primitives.
> - Chương này sẽ **hiện đại hóa** cách bạn xử lý dữ liệu trong Java.

---

## 🎓 HƯỚNG DẪN HỌC SÂU & CODE CHẠY THỬ

| Chủ đề | Code | So sánh |
|--------|------|---------|
| for vs Stream | `CODE/VI_DU/CHUONG_6/01_Collections_Stream/` | Cùng kết quả `[4, 8]` |

```bash
cd CODE/VI_DU/CHUONG_6/01_Collections_Stream
javac Main.java && java Main
```

**Giải thích từng bước Stream:**

1. `numbers.stream()` – tạo luồng dữ liệu từ List.
2. `.filter(n -> n % 2 == 0)` – giữ 2, 4 (intermediate).
3. `.map(n -> n * 2)` – thành 4, 8 (intermediate).
4. `.collect(Collectors.toList())` – gom kết quả (terminal – lúc này mới thực thi pipeline).

**❓ Tự kiểm tra:** Vì sao `List<int>` không compile? → Generic chỉ nhận **object**; dùng `Integer` + autoboxing.

---

## 6.1. JAVA COLLECTIONS FRAMEWORK

### 6.1.1. Collections là gì?

#### Định nghĩa

**Collections Framework** là tập hợp các interfaces và classes để lưu trữ và xử lý nhóm các objects.

**Lợi ích:**
- ✅ Không cần tự implement cấu trúc dữ liệu
- ✅ Tối ưu hiệu suất
- ✅ Code chuẩn, dễ đọc

> **💡 Nhắc lại từ Chương 1:**
> - Bạn đã học **Mảng (Arrays)** ở Chương 1, phần 0.7
> - Mảng có hạn chế: kích thước cố định, không thể thêm/xóa dễ dàng
> - **Collections** giải quyết các vấn đề này!
> 
> - Bạn đã học **Wrapper Classes** ở Chương 1, phần 0.13
> - Collections chỉ chấp nhận objects (Wrapper Classes), không chấp nhận primitives
> - Ví dụ: `List<Integer>` (✅), `List<int>` (❌)

> **🧰 Tưởng tượng: Hộp dụng cụ (The Toolbox)**
> - **Collection**: Là cái hộp dụng cụ đa năng.
> - **List**: Giống **Danh sách đi chợ**. Thứ tự quan trọng (mua trứng trước hay sữa sau), và có thể ghi 2 lần "sữa" (duplicate) nếu cần mua nhiều.
> - **Set**: Giống **Danh sách khách mời đám cưới**. Không được mời ai 2 lần (no duplicate), thứ tự không quan trọng.
> - **Map**: Giống **Cuốn từ điển** hoặc **Danh bạ điện thoại**. Tra cứu bằng Từ khóa (Tên) để ra Ý nghĩa (Số điện thoại). Tên không được trùng, nhưng số điện thoại có thể trùng.

---

#### Collections Hierarchy

```
Collection
├── List (Có thứ tự, cho phép duplicate)
│   ├── ArrayList
│   ├── LinkedList
│   └── Vector
│
├── Set (Không có thứ tự, không duplicate)
│   ├── HashSet
│   ├── TreeSet
│   └── LinkedHashSet
│
└── Queue (Hàng đợi)
    ├── PriorityQueue
    └── Deque

Map (Không extends Collection)
├── HashMap
├── TreeMap
└── LinkedHashMap
```

---

### 6.1.2. List Interface

#### Đặc điểm

**List:**
- Có thứ tự (ordered)
- Cho phép duplicate
- Có index (truy cập theo vị trí)

---

#### ArrayList

**Đặc điểm:**
- Implement bằng mảng động
- Truy cập nhanh (O(1))
- Thêm/xóa chậm ở giữa (O(n))

**Ví dụ:**
```java
List<String> list = new ArrayList<>();

// Thêm phần tử
list.add("Apple");
list.add("Banana");
list.add("Orange");

// Truy cập
String first = list.get(0);  // "Apple"

// Duyệt
for (String fruit : list) {
    System.out.println(fruit);
}

// Hoặc
list.forEach(fruit -> System.out.println(fruit));
```

---

#### LinkedList

**Đặc điểm:**
- Implement bằng linked list
- Thêm/xóa nhanh (O(1))
- Truy cập chậm (O(n))

**Ví dụ:**
```java
List<String> list = new LinkedList<>();
list.add("Apple");
list.add("Banana");
list.addFirst("First");  // Thêm vào đầu
list.addLast("Last");    // Thêm vào cuối
```

---

#### So sánh ArrayList vs LinkedList

| Đặc điểm | ArrayList | LinkedList |
|----------|-----------|------------|
| **Cấu trúc** | Mảng động | Linked list |
| **Truy cập** | O(1) - Nhanh | O(n) - Chậm |
| **Thêm cuối** | O(1) - Nhanh | O(1) - Nhanh |
| **Thêm/xóa giữa** | O(n) - Chậm | O(1) - Nhanh |
| **Bộ nhớ** | Ít hơn | Nhiều hơn (pointer) |
| **Khi nào dùng** | Truy cập nhiều | Thêm/xóa nhiều |

**Khuyến nghị:**
- ✅ Dùng **ArrayList** trong hầu hết trường hợp
- ✅ Dùng **LinkedList** khi thêm/xóa ở giữa nhiều

---

### 6.1.3. Set Interface

#### Đặc điểm

**Set:**
- Không có thứ tự (unordered) - trừ LinkedHashSet
- Không cho phép duplicate
- Không có index

---

#### HashSet

**Đặc điểm:**
- Không có thứ tự
- Nhanh nhất (O(1) cho add/contains)
- Dùng hashCode() và equals()

**Ví dụ:**
```java
Set<String> set = new HashSet<>();
set.add("Apple");
set.add("Banana");
set.add("Apple");  // Bị bỏ qua (duplicate)

System.out.println(set.size());  // 2
System.out.println(set.contains("Apple"));  // true
```

---

#### TreeSet

**Đặc điểm:**
- Có thứ tự (sorted)
- Chậm hơn HashSet (O(log n))
- Dùng Comparable hoặc Comparator

**Ví dụ:**
```java
Set<String> set = new TreeSet<>();
set.add("Banana");
set.add("Apple");
set.add("Orange");

// Tự động sắp xếp
for (String fruit : set) {
    System.out.println(fruit);  // Apple, Banana, Orange
}
```

---

#### LinkedHashSet

**Đặc điểm:**
- Giữ thứ tự chèn (insertion order)
- Nhanh như HashSet
- Kết hợp HashSet + LinkedList

**Ví dụ:**
```java
Set<String> set = new LinkedHashSet<>();
set.add("Banana");
set.add("Apple");
set.add("Orange");

// Giữ thứ tự chèn
for (String fruit : set) {
    System.out.println(fruit);  // Banana, Apple, Orange
}
```

---

### 6.1.4. Map Interface

#### Đặc điểm

**Map:**
- Lưu trữ key-value pairs
- Key không duplicate
- Value có thể duplicate

---

#### HashMap

**Đặc điểm:**
- Không có thứ tự
- Nhanh nhất (O(1))
- Cho phép null key và null value

**Ví dụ:**
```java
Map<String, Integer> map = new HashMap<>();
map.put("Apple", 10);
map.put("Banana", 20);
map.put("Orange", 15);

// Truy cập
int apples = map.get("Apple");  // 10

// Kiểm tra
if (map.containsKey("Apple")) {
    System.out.println("Apple exists");
}

// Duyệt
for (Map.Entry<String, Integer> entry : map.entrySet()) {
    System.out.println(entry.getKey() + ": " + entry.getValue());
}
```

---

#### TreeMap

**Đặc điểm:**
- Có thứ tự (sorted theo key)
- Chậm hơn HashMap (O(log n))
- Key phải implement Comparable

**Ví dụ:**
```java
Map<String, Integer> map = new TreeMap<>();
map.put("Banana", 20);
map.put("Apple", 10);
map.put("Orange", 15);

// Tự động sắp xếp theo key
for (String key : map.keySet()) {
    System.out.println(key + ": " + map.get(key));
    // Apple: 10, Banana: 20, Orange: 15
}
```

---

### 6.1.5. Tiêu chí Lựa chọn Cấu trúc Dữ liệu

#### Decision Tree

```
Cần duplicate?
├── Có → List
│   ├── Truy cập nhiều? → ArrayList
│   └── Thêm/xóa giữa nhiều? → LinkedList
│
└── Không → Set
    ├── Cần thứ tự?
    │   ├── Có → TreeSet (sorted) hoặc LinkedHashSet (insertion order)
    │   └── Không → HashSet
    │
    └── Cần key-value? → Map
        ├── Cần thứ tự? → TreeMap
        └── Không → HashMap
```

### 🏎️ Phân tích Hiệu Năng (Performance Analysis)

Chọn đúng "Hộp dụng cụ" quyết định tốc độ chương trình:

| Hành động | ArrayList | LinkedList | HashSet | HashMap |
|-----------|-----------|------------|---------|---------|
| **Truy cập (Get)** | ⚡️ Cực nhanh (O(1)) | 🐌 Rùa bò (O(n)) | - | ⚡️ Cực nhanh (O(1)) |
| **Thêm/Xóa cuối** | ⚡️ Cực nhanh | ⚡️ Cực nhanh | ⚡️ Cực nhanh | ⚡️ Cực nhanh |
| **Thêm/Xóa giữa** | 🐌 Rùa bò (Dời mảng) | ⚡️ Cực nhanh (Nối dây) | - | - |
| **Tìm kiếm (Find)**| 🐢 Chậm (Duyệt cả list) | 🐢 Chậm | ⚡️ Cực nhanh | ⚡️ Cực nhanh |

> **Kinh nghiệm xương máu:**
> - Nếu bạn cần **tìm kiếm** nhanh (ví dụ: tìm User theo ID) → Đừng dùng `List`, hãy dùng `Map`.
> - Nếu bạn cần **lưu trữ đơn giản** để duyệt → Dùng `ArrayList`.
> - Hạn chế dùng `LinkedList` trừ khi bạn hiểu rõ mình đang làm gì (thường ít khi cần).


---

#### Ví dụ thực tế

**1. Danh sách sinh viên (có thể trùng tên):**
```java
List<Student> students = new ArrayList<>();  // ✅ List
```

**2. Danh sách ID duy nhất:**
```java
Set<String> uniqueIds = new HashSet<>();  // ✅ Set
```

**3. Dictionary (từ điển):**
```java
Map<String, String> dictionary = new HashMap<>();  // ✅ Map
```

**4. Leaderboard (bảng xếp hạng - cần sorted):**
```java
Map<String, Integer> scores = new TreeMap<>();  // ✅ TreeMap
```

---

## 6.2. GENERICS

### 6.2.1. Generics là gì?

#### Vấn đề không dùng Generics

**Code cũ (không type-safe):**
```java
List list = new ArrayList();
list.add("Hello");
list.add(123);
list.add(new Date());

// Phải cast khi lấy ra
String str = (String) list.get(0);  // OK
String str2 = (String) list.get(1);  // ❌ ClassCastException!
```

**Vấn đề:**
- ❌ Không type-safe
- ❌ Dễ lỗi ClassCastException
- ❌ Phải cast mỗi lần

> **u001f🍾 Tưởng tượng: Hũ đựng bánh (Labelled Jar)**
> - **Không dùng Generics**: Giống như cái hũ thủy tinh trơn, không dán nhãn. Bạn có thể bỏ bánh quy, kẹo, hay thậm chí... ốc vít vào đó. Khi lấy ra ăn, bạn phải nhìn kỹ (cast) mới dám ăn, nếu không sẽ gãy răng (Runtime Error).
> - **Dùng Generics (`Box<Cookie>`)**: Giống như cái hũ có dán nhãn **"CHỈ ĐỰNG BÁNH QUY"**. Nếu bạn cố bỏ ốc vít vào, cái nắp sẽ không đóng lại được (Compile Error). Khi lấy ra, bạn nhắm mắt cũng biết đó là bánh quy (Type-safe).

---

#### Giải pháp: Generics

**Code mới (type-safe):**
```java
List<String> list = new ArrayList<>();
list.add("Hello");
// list.add(123);  // ❌ Compiler error ngay!

String str = list.get(0);  // ✅ Không cần cast
```

**Lợi ích:**
- ✅ Type-safe (compiler kiểm tra)
- ✅ Không cần cast
- ✅ Code rõ ràng hơn

---

### 6.2.2. Cú pháp Generics

#### Generic Class

```java
// Định nghĩa
public class Box<T> {
    private T item;
    
    public void setItem(T item) {
        this.item = item;
    }
    
    public T getItem() {
        return item;
    }
}

// Sử dụng
Box<String> stringBox = new Box<>();
stringBox.setItem("Hello");
String value = stringBox.getItem();  // ✅ Type-safe

Box<Integer> intBox = new Box<>();
intBox.setItem(123);
Integer number = intBox.getItem();  // ✅ Type-safe
```

---

#### Generic Method

```java
public class Utils {
    // Generic method
    public static <T> void printArray(T[] array) {
        for (T item : array) {
            System.out.println(item);
        }
    }
    
    // Generic method với return type
    public static <T> T getFirst(List<T> list) {
        return list.get(0);
    }
}

// Sử dụng
String[] strings = {"Hello", "World"};
Utils.printArray(strings);

Integer[] numbers = {1, 2, 3};
Utils.printArray(numbers);
```

---

#### Bounded Type Parameters

**Giới hạn kiểu generic:**

```java
// Chỉ chấp nhận Number và subclass
public class NumberBox<T extends Number> {
    private T number;
    
    public double getDoubleValue() {
        return number.doubleValue();  // Có thể gọi methods của Number
    }
}

// Sử dụng
NumberBox<Integer> intBox = new NumberBox<>();  // ✅ OK
NumberBox<Double> doubleBox = new NumberBox<>();  // ✅ OK
// NumberBox<String> stringBox = new NumberBox<>();  // ❌ LỖI
```

---

### 6.2.3. Wildcards

#### ? (Unbounded Wildcard)

```java
// Chấp nhận bất kỳ type nào
public void printList(List<?> list) {
    for (Object item : list) {
        System.out.println(item);
    }
}
```

---

#### ? extends Type (Upper Bounded)

```java
// Chấp nhận Number và subclass
public double sum(List<? extends Number> numbers) {
    double sum = 0;
    for (Number num : numbers) {
        sum += num.doubleValue();
    }
    return sum;
}

// Sử dụng
List<Integer> integers = Arrays.asList(1, 2, 3);
List<Double> doubles = Arrays.asList(1.5, 2.5, 3.5);
sum(integers);  // ✅ OK
sum(doubles);   // ✅ OK
```

---

#### ? super Type (Lower Bounded)

```java
// Chấp nhận Integer và superclass
public void addNumbers(List<? super Integer> list) {
    list.add(1);
    list.add(2);
}

// Sử dụng
List<Number> numbers = new ArrayList<>();
addNumbers(numbers);  // ✅ OK

List<Object> objects = new ArrayList<>();
addNumbers(objects);  // ✅ OK
```

---

## 6.3. LAMBDA EXPRESSIONS & FUNCTIONAL INTERFACES

### 6.3.1. Lambda Expressions là gì?

#### Vấn đề: Code dài dòng

**Code cũ (Anonymous Inner Class):**
```java
List<String> names = Arrays.asList("Alice", "Bob", "Charlie");

// Sắp xếp với Comparator
Collections.sort(names, new Comparator<String>() {
    @Override
    public int compare(String a, String b) {
        return a.length() - b.length();
    }
});
```

**Vấn đề:**
- ❌ Code dài dòng
- ❌ Khó đọc
- ❌ Nhiều boilerplate

---

#### Giải pháp: Lambda Expression

**Code mới (ngắn gọn):**
```java
List<String> names = Arrays.asList("Alice", "Bob", "Charlie");

// Lambda expression
Collections.sort(names, (a, b) -> a.length() - b.length());
```

**Lợi ích:**
- ✅ Ngắn gọn
- ✅ Dễ đọc
- ✅ Functional programming style

---

### 6.3.2. Cú pháp Lambda

#### Cú pháp cơ bản

```java
(parameters) -> expression

// Hoặc
(parameters) -> {
    statements;
    return value;
}
```

**Ví dụ:**
```java
// 1. Không tham số
Runnable r = () -> System.out.println("Hello");

// 2. Một tham số (có thể bỏ dấu ngoặc)
Function<String, Integer> f = s -> s.length();

// 3. Nhiều tham số
BiFunction<Integer, Integer, Integer> add = (a, b) -> a + b;

// 4. Nhiều dòng
Function<String, String> upper = s -> {
    String result = s.toUpperCase();
    return result;
};
```

---

#### Ví dụ thực tế

**1. Với List:**
```java
List<String> names = Arrays.asList("Alice", "Bob", "Charlie");

// Duyệt với Lambda
names.forEach(name -> System.out.println(name));

// Hoặc method reference
names.forEach(System.out::println);
```

**2. Với Comparator:**
```java
List<String> names = Arrays.asList("Alice", "Bob", "Charlie");

// Sắp xếp theo độ dài
names.sort((a, b) -> a.length() - b.length());

// Sắp xếp theo thứ tự alphabet
names.sort((a, b) -> a.compareTo(b));
```

**3. Với Predicate:**
```java
List<Integer> numbers = Arrays.asList(1, 2, 3, 4, 5);

// Lọc số chẵn
numbers.removeIf(n -> n % 2 == 0);
```

---

### 6.3.3. Functional Interfaces

#### Định nghĩa

**Functional Interface** là interface chỉ có **một abstract method**.

**Annotation:**
```java
@FunctionalInterface
public interface MyFunction {
    void doSomething();
}
```

---

#### Các Functional Interfaces phổ biến

**1. Predicate<T>**
```java
// Kiểm tra điều kiện (trả về boolean)
Predicate<String> isLong = s -> s.length() > 5;
boolean result = isLong.test("Hello World");  // true
```

**2. Consumer<T>**
```java
// Nhận input, không trả về gì
Consumer<String> print = s -> System.out.println(s);
print.accept("Hello");  // In "Hello"
```

**3. Function<T, R>**
```java
// Nhận T, trả về R
Function<String, Integer> length = s -> s.length();
int len = length.apply("Hello");  // 5
```

**4. Supplier<T>**
```java
// Không nhận gì, trả về T
Supplier<String> greeting = () -> "Hello";
String msg = greeting.get();  // "Hello"
```

**5. BiFunction<T, U, R>**
```java
// Nhận T và U, trả về R
BiFunction<Integer, Integer, Integer> add = (a, b) -> a + b;
int sum = add.apply(5, 3);  // 8
```

---

#### Ví dụ sử dụng

```java
List<String> names = Arrays.asList("Alice", "Bob", "Charlie", "David");

// Predicate: Lọc tên dài hơn 4 ký tự
Predicate<String> longName = name -> name.length() > 4;
List<String> longNames = names.stream()
    .filter(longName)
    .collect(Collectors.toList());
// ["Alice", "Charlie", "David"]

// Function: Chuyển thành chữ hoa
Function<String, String> upper = String::toUpperCase;
List<String> upperNames = names.stream()
    .map(upper)
    .collect(Collectors.toList());
// ["ALICE", "BOB", "CHARLIE", "DAVID"]

// Consumer: In từng phần tử
Consumer<String> print = System.out::println;
names.forEach(print);
```

---

## 6.4. STREAM API (TƯ DUY DECLARATIVE)

### 6.4.1. Imperative vs Declarative

#### Imperative (Mệnh lệnh) - "HOW"

**Tập trung vào "Làm thế nào":**
```java
List<String> names = Arrays.asList("Alice", "Bob", "Charlie", "David");
List<String> result = new ArrayList<>();

// Imperative: Mô tả từng bước
for (String name : names) {
    if (name.length() > 4) {  // Lọc
        String upper = name.toUpperCase();  // Chuyển đổi
        result.add(upper);  // Thêm vào list
    }
}
```

**Đặc điểm:**
- ❌ Code dài
- ❌ Phải quản lý state (biến result)
- ❌ Khó đọc

> **🏭 Tưởng tượng: Dây chuyền sản xuất (Assembly Line)**
> - **Stream API** biến việc xử lý dữ liệu thành một dây chuyền nhà máy.
> - **Source (List)**: Nguyên liệu thô đầu vào.
> - **Filter**: Máy sàng lọc (chỉ lấy nguyên liệu tốt).
> - **Map**: Máy chế biến (gọt vỏ, sơn màu).
> - **Collect**: Đóng gói thành phẩm.
> - Bạn chỉ cần đứng chỉ tay "Tôi muốn lọc cái này, chế biến thế kia" (Declarative), máy móc sẽ tự chạy. Bạn không cần tự tay bưng bê từng món (Imperative Loop).

---

#### Declarative (Khai báo) - "WHAT"

**Tập trung vào "Làm gì":**
```java
List<String> names = Arrays.asList("Alice", "Bob", "Charlie", "David");

// Declarative: Mô tả kết quả mong muốn
List<String> result = names.stream()
    .filter(name -> name.length() > 4)  // Lọc
    .map(String::toUpperCase)            // Chuyển đổi
    .collect(Collectors.toList());       // Thu thập
```

### 🏭 Phân tích Dây chuyền (Pipeline Trace)

Hãy nhìn dữ liệu chạy qua băng chuyền như thế nào:

1.  **Source**: `["Alice", "Bob", "Charlie", "David"]`
2.  **Filter (`> 4`)**:
    *   "Alice" (5) → ✅ Giữ
    *   "Bob" (3) → ❌ Loại
    *   "Charlie" (7) → ✅ Giữ
    *   "David" (5) → ✅ Giữ
    *   *Output*: `["Alice", "Charlie", "David"]`
3.  **Map (`toUpperCase`)**:
    *   "Alice" → "ALICE"
    *   "Charlie" → "CHARLIE"
    *   "David" → "DAVID"
    *   *Output*: `["ALICE", "CHARLIE", "DAVID"]`
4.  **Collect**: Đóng gói vào List mới.

> **💡 Tại sao hay hơn Loop?**
> Bạn không cần tạo biến tạm (`result`), không cần viết `if/else` lồng nhau. Logic cực kỳ rõ ràng "từ trái sang phải".


**Đặc điểm:**
- ✅ Code ngắn gọn
- ✅ Dễ đọc (đọc như câu văn)
- ✅ Không cần quản lý state

---

### 6.4.2. Stream API là gì?

#### Định nghĩa

**Stream API** là API để xử lý collections theo kiểu functional programming.

**Đặc điểm:**
- Không lưu trữ dữ liệu (không phải cấu trúc dữ liệu)
- Functional (không thay đổi source)
- Lazy evaluation (chỉ xử lý khi cần)
- Có thể parallel

---

#### Tạo Stream

```java
// Từ Collection
List<String> list = Arrays.asList("a", "b", "c");
Stream<String> stream = list.stream();

// Từ Array
String[] array = {"a", "b", "c"};
Stream<String> stream2 = Arrays.stream(array);

// Từ values
Stream<String> stream3 = Stream.of("a", "b", "c");

// Từ range
IntStream range = IntStream.range(1, 10);  // 1-9
```

---

### 6.4.3. Các Toán tử Stream

#### 1. filter() - Lọc

**Lọc phần tử thỏa điều kiện:**
```java
List<Integer> numbers = Arrays.asList(1, 2, 3, 4, 5, 6);

List<Integer> evens = numbers.stream()
    .filter(n -> n % 2 == 0)  // Lọc số chẵn
    .collect(Collectors.toList());
// [2, 4, 6]
```

---

#### 2. map() - Chuyển đổi

**Chuyển đổi mỗi phần tử:**
```java
List<String> names = Arrays.asList("alice", "bob", "charlie");

List<String> upper = names.stream()
    .map(String::toUpperCase)  // Chuyển thành chữ hoa
    .collect(Collectors.toList());
// ["ALICE", "BOB", "CHARLIE"]

// map() có thể đổi kiểu
List<Integer> lengths = names.stream()
    .map(String::length)  // String → Integer
    .collect(Collectors.toList());
// [5, 3, 7]
```

---

#### 3. sorted() - Sắp xếp

**Sắp xếp phần tử:**
```java
List<String> names = Arrays.asList("Charlie", "Alice", "Bob");

List<String> sorted = names.stream()
    .sorted()  // Sắp xếp tự nhiên
    .collect(Collectors.toList());
// ["Alice", "Bob", "Charlie"]

// Sắp xếp tùy chỉnh
List<String> sortedByLength = names.stream()
    .sorted((a, b) -> a.length() - b.length())  // Theo độ dài
    .collect(Collectors.toList());
// ["Bob", "Alice", "Charlie"]
```

---

#### 4. distinct() - Loại bỏ duplicate

**Loại bỏ phần tử trùng:**
```java
List<Integer> numbers = Arrays.asList(1, 2, 2, 3, 3, 3, 4);

List<Integer> unique = numbers.stream()
    .distinct()
    .collect(Collectors.toList());
// [1, 2, 3, 4]
```

---

#### 5. limit() và skip()

**Giới hạn số lượng:**
```java
List<Integer> numbers = Arrays.asList(1, 2, 3, 4, 5, 6, 7, 8);

List<Integer> first3 = numbers.stream()
    .limit(3)  // Lấy 3 phần tử đầu
    .collect(Collectors.toList());
// [1, 2, 3]

List<Integer> skip3 = numbers.stream()
    .skip(3)  // Bỏ qua 3 phần tử đầu
    .collect(Collectors.toList());
// [4, 5, 6, 7, 8]
```

---

#### 6. collect() - Thu thập

**Thu thập kết quả:**
```java
List<String> names = Arrays.asList("Alice", "Bob", "Charlie");

// To List
List<String> list = names.stream()
    .collect(Collectors.toList());

// To Set
Set<String> set = names.stream()
    .collect(Collectors.toSet());

// To Map
Map<String, Integer> map = names.stream()
    .collect(Collectors.toMap(
        name -> name,           // Key
        name -> name.length()  // Value
    ));
// {"Alice": 5, "Bob": 3, "Charlie": 7}

// Joining
String joined = names.stream()
    .collect(Collectors.joining(", "));
// "Alice, Bob, Charlie"
```

---

#### 7. reduce() - Tổng hợp

**Tổng hợp tất cả phần tử:**
```java
List<Integer> numbers = Arrays.asList(1, 2, 3, 4, 5);

// Tổng
int sum = numbers.stream()
    .reduce(0, (a, b) -> a + b);
// 15

// Hoặc
int sum2 = numbers.stream()
    .reduce(0, Integer::sum);

// Tích
int product = numbers.stream()
    .reduce(1, (a, b) -> a * b);
// 120

// Max
Optional<Integer> max = numbers.stream()
    .reduce(Integer::max);
// Optional[5]
```

---

#### 8. forEach() - Duyệt

**Duyệt và thực hiện hành động:**
```java
List<String> names = Arrays.asList("Alice", "Bob", "Charlie");

names.stream()
    .forEach(name -> System.out.println(name));
// In từng tên
```

---

### 6.4.4. Chain of Operations

#### Kết hợp nhiều toán tử

**Ví dụ: Tìm tên dài hơn 4 ký tự, chuyển thành chữ hoa, sắp xếp:**
```java
List<String> names = Arrays.asList("Alice", "Bob", "Charlie", "David", "Eve");

List<String> result = names.stream()
    .filter(name -> name.length() > 4)      // Lọc: ["Alice", "Charlie", "David"]
    .map(String::toUpperCase)              // Chuyển đổi: ["ALICE", "CHARLIE", "DAVID"]
    .sorted()                               // Sắp xếp: ["ALICE", "CHARLIE", "DAVID"]
    .collect(Collectors.toList());          // Thu thập

System.out.println(result);
// [ALICE, CHARLIE, DAVID]
```

---

#### Ví dụ thực tế: Xử lý dữ liệu phức tạp

**Bài toán: Từ danh sách sinh viên, tìm sinh viên có GPA >= 3.5, sắp xếp theo GPA giảm dần, lấy top 3:**
```java
List<Student> students = Arrays.asList(
    new Student("Alice", 3.8),
    new Student("Bob", 3.2),
    new Student("Charlie", 3.9),
    new Student("David", 3.6),
    new Student("Eve", 3.4)
);

List<String> topStudents = students.stream()
    .filter(s -> s.getGpa() >= 3.5)                    // Lọc GPA >= 3.5
    .sorted((a, b) -> Double.compare(b.getGpa(), a.getGpa()))  // Sắp xếp giảm dần
    .limit(3)                                           // Lấy top 3
    .map(Student::getName)                              // Lấy tên
    .collect(Collectors.toList());                      // Thu thập

System.out.println(topStudents);
// [Charlie, Alice, David]
```

---

### 6.4.5. Terminal vs Intermediate Operations

#### Phân loại

**Intermediate Operations (Toán tử trung gian):**
- Trả về Stream
- Lazy (chỉ thực thi khi có terminal operation)
- Có thể chain nhiều operations
- Ví dụ: `filter()`, `map()`, `sorted()`, `distinct()`

**Terminal Operations (Toán tử kết thúc):**
- Trả về kết quả cụ thể (không phải Stream)
- Thực thi toàn bộ pipeline
- Ví dụ: `collect()`, `forEach()`, `reduce()`, `count()`

---

#### Ví dụ

```java
List<Integer> numbers = Arrays.asList(1, 2, 3, 4, 5);

// Intermediate operations (chưa thực thi)
Stream<Integer> stream = numbers.stream()
    .filter(n -> n % 2 == 0)  // Intermediate
    .map(n -> n * 2);          // Intermediate

// Terminal operation (thực thi tất cả)
List<Integer> result = stream.collect(Collectors.toList());  // Terminal
// [4, 8]
```

---

## 6.5. JAVA RECORD (JAVA 14+) - DATA CARRIER CLASSES

### 6.5.1. Vấn đề của class thông thường (POJO/DTO)
Khi bạn muốn tạo một class chỉ để chứa dữ liệu (ví dụ: `User`, `Point`, `ProductDTO`), bạn phải viết rất nhiều code lặp đi lặp lại (boilerplate code):
- Khai báo fields (`private final`)
- Viết Constructor
- Viết Getters
- Override `equals()` và `hashCode()` (Cực kỳ quan trọng khi dùng với HashMap/HashSet)
- Override `toString()`

**Code truyền thống:**
```java
public class Point {
    private final int x;
    private final int y;

    public Point(int x, int y) {
        this.x = x;
        this.y = y;
    }

    public int getX() { return x; }
    public int getY() { return y; }

    @Override
    public boolean equals(Object o) { ... }

    @Override
    public int hashCode() { ... }

    @Override
    public String toString() { ... }
}
```
*Bạn mất 30 dòng code chỉ để lưu tọa độ x và y!*

---

### 6.5.2. Giải pháp: Java Record
Từ Java 14, bạn có thể thay thế toàn bộ class trên bằng **ĐÚNG MỘT DÒNG CODE**:

```java
public record Point(int x, int y) {}
```

**Record tự động làm gì cho bạn?**
1. **Immutability (Bất biến)**: Các biến `x`, `y` tự động là `private final`. Không thể sửa giá trị sau khi tạo.
2. **Tự động sinh Constructor**.
3. **Tự động sinh Getter**: Lưu ý, getter của Record là `point.x()` và `point.y()`, không phải `getX()`.
4. **Tự động sinh `equals()`, `hashCode()` và `toString()` chuẩn chỉ**.

---

### 6.5.3. Sử dụng Record với Collections (Sự kết hợp hoàn hảo)

Bởi vì Record tự động implement `equals()` và `hashCode()` rất chuẩn xác, nó là **ứng cử viên hoàn hảo số 1** để làm **Key trong HashMap** hoặc **Phần tử trong HashSet**.

**Ví dụ:**
```java
public record StudentKey(String studentId, String classCode) {}

public class Main {
    public static void main(String[] args) {
        Map<StudentKey, Double> grades = new HashMap<>();
        
        StudentKey key1 = new StudentKey("SV01", "CS101");
        grades.put(key1, 9.5);
        
        // Tạo một key MỚI nhưng có cùng dữ liệu
        StudentKey key2 = new StudentKey("SV01", "CS101");
        
        // HashMap hoạt động chính xác vì Record đã tự động overide equals và hashCode
        System.out.println(grades.get(key2)); // In ra: 9.5
        
        // Thử toString()
        System.out.println(key1); // In ra: StudentKey[studentId=SV01, classCode=CS101]
    }
}
```

**Khi nào dùng Record?**
- ✅ Làm Key cho `Map`.
- ✅ Làm Data Transfer Object (DTO) để trả data từ DB hoặc API.
- ✅ Làm Tuple để trả về nhiều kết quả từ 1 hàm.
- ❌ KHÔNG dùng Record nếu đối tượng cần thay đổi trạng thái (mutable) như `Entity` có hàm `setX()`.

---

## 📊 BẢNG LỰA CHỌN & ÔN TẬP (CHƯƠNG 6)

### Nhắc lại Wrapper Classes (Chương 1)

Collections **chỉ chứa object**, không chứa primitive:

```java
List<Integer> list = new ArrayList<>();  // ✅
// List<int> list = ...;               // ❌ không compile

list.add(10);        // autoboxing: int → Integer
int x = list.get(0); // unboxing: Integer → int
```

### Cây quyết định: Chọn List / Set / Map

```
Cần lưu nhóm phần tử?
├── Cần tra cứu theo KEY → Map (HashMap, TreeMap)
└── Chỉ cần tập phần tử
    ├── Cần thứ tự, cho phép trùng → List (ArrayList / LinkedList)
    └── Không trùng → Set (HashSet / TreeSet)
```

### ArrayList vs LinkedList (tóm tắt)

| Thao tác | ArrayList | LinkedList |
|----------|-----------|------------|
| Truy cập index | O(1) – nhanh | O(n) – chậm |
| Thêm/xóa đầu danh sách | O(n) – chậm | O(1) – nhanh |
| **Chọn khi** | Đọc nhiều, ít chèn đầu | Chèn/xóa đầu nhiều |

### Imperative vs Declarative (Stream)

| Imperative | Declarative (Stream) |
|------------|----------------------|
| “Làm từng bước” (for, if) | “Muốn gì” (filter, map, collect) |
| Dễ debug từng dòng | Ngắn gọn, dễ đọc với pipeline |
| Ví dụ: vòng for lọc số chẵn | `stream().filter(n -> n%2==0).collect(...)` |

---

## 🔗 TỔNG KẾT MÔN HỌC (SAU CHƯƠNG 6)

Bạn đã đi trọn lộ trình:

```
Ch.1 Java + OOP tư duy → Ch.2 Class/Object → Ch.3 Kế thừa/Đa hình
→ Ch.4 SOLID & Pattern → Ch.5 Exception & Test → Ch.6 Collections & Stream
```

**Dự án tích hợp gợi ý:** Hệ thống quản lý thư viện / sản phẩm – dùng class (Ch.2–3), thiết kế SOLID (Ch.4), test + file (Ch.5), lưu danh sách bằng Collections + Stream (Ch.6).

**Ôn thi:** Xem [README.md](README.md) – mục đề thi và checklist từng chương.

---

## 📝 TÓM TẮT CHƯƠNG 6

### Kiến thức đã học:
1. ✅ Java Collections Framework (List, Set, Map)
2. ✅ So sánh ArrayList vs LinkedList, HashSet vs TreeSet
3. ✅ Generics và Type Safety
4. ✅ Lambda Expressions
5. ✅ Functional Interfaces (Predicate, Consumer, Function)
6. ✅ Stream API và các toán tử
7. ✅ Imperative vs Declarative programming

### Kỹ năng đã có:
- ✅ Chọn cấu trúc dữ liệu phù hợp
- ✅ Sử dụng Generics đúng cách
- ✅ Viết Lambda Expressions
- ✅ Xử lý dữ liệu với Stream API
- ✅ Chuyển đổi code từ Imperative sang Declarative

---

## 🎯 BÀI TẬP CHƯƠNG 6

### Bài 1: Collections cơ bản

**Yêu cầu:**
1. Tạo `List<String>` chứa tên 5 sinh viên
2. Tạo `Set<Integer>` chứa ID duy nhất
3. Tạo `Map<String, Double>` chứa điểm số (tên → điểm)
4. Duyệt và in tất cả

---

### Bài 2: So sánh ArrayList vs LinkedList

**Yêu cầu:**
1. Tạo ArrayList và LinkedList với 10000 phần tử
2. Đo thời gian:
   - Thêm 1000 phần tử vào đầu
   - Truy cập phần tử ở giữa 1000 lần
3. So sánh kết quả và giải thích

---

### Bài 3: Generics

**Yêu cầu:**
1. Tạo generic class `Pair<T, U>` lưu 2 giá trị
2. Tạo generic method `swap()` đổi vị trí 2 phần tử trong mảng
3. Tạo bounded generic class `NumberBox<T extends Number>`

**Test:**
```java
Pair<String, Integer> pair = new Pair<>("Age", 25);
System.out.println(pair.getFirst());  // "Age"
System.out.println(pair.getSecond()); // 25
```

---

### Bài 4: Lambda Expressions

**Yêu cầu:**
1. Sắp xếp List<String> theo độ dài (dùng Lambda)
2. Lọc List<Integer> chỉ lấy số chẵn (dùng Lambda)
3. Chuyển List<String> thành chữ hoa (dùng Lambda)

---

### Bài 5: Functional Interfaces

**Yêu cầu:**
Tạo các Functional Interface và sử dụng:
1. `Predicate<Integer>`: Kiểm tra số nguyên tố
2. `Function<String, Integer>`: Đếm số từ trong chuỗi
3. `Consumer<String>`: In chuỗi với format đặc biệt

---

### Bài 6: Stream API - Xử lý dữ liệu

**Yêu cầu:**
Cho danh sách sinh viên:
```java
List<Student> students = Arrays.asList(
    new Student("Alice", 20, 3.8),
    new Student("Bob", 21, 3.2),
    new Student("Charlie", 19, 3.9),
    new Student("David", 22, 3.6),
    new Student("Eve", 20, 3.4)
);
```

**Dùng Stream API để:**
1. Tìm sinh viên có GPA >= 3.5
2. Sắp xếp theo GPA giảm dần
3. Lấy top 3
4. Tính GPA trung bình
5. Nhóm theo tuổi

---

### Bài 7: Tổng hợp - Hệ thống Quản lý Sản phẩm

**Yêu cầu:**
Tạo hệ thống quản lý sản phẩm với:

1. **Class `Product`**:
   - Fields: id, name, price, category

2. **Class `ProductManager`**:
   - Sử dụng Collections để lưu trữ
   - Methods sử dụng Stream API:
     - `findByCategory(String category)`: Tìm theo danh mục
     - `findExpensiveProducts(double minPrice)`: Tìm sản phẩm đắt
     - `getAveragePrice()`: Tính giá trung bình
     - `getProductsByPriceRange(double min, double max)`: Tìm trong khoảng giá
     - `getTopExpensiveProducts(int n)`: Lấy n sản phẩm đắt nhất
     - `groupByCategory()`: Nhóm theo danh mục

**Yêu cầu:**
- Sử dụng Generics
- Tất cả methods dùng Stream API
- Code Declarative (không dùng vòng lặp for)

---

## 📚 TÀI LIỆU THAM KHẢO

1. **Java Collections Tutorial**: https://docs.oracle.com/javase/tutorial/collections/
2. **Java Stream API**: https://docs.oracle.com/javase/8/docs/api/java/util/stream/package-summary.html
3. **Effective Java** - Joshua Bloch (Item 26-37: Generics)

---

**Chúc bạn học tốt! 🚀**

