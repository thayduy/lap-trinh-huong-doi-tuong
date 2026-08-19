# 📘 GIÁO TRÌNH LÝ THUYẾT – LẬP TRÌNH HƯỚNG ĐỐI TƯỢNG (JAVA)

Giáo trình lý thuyết môn **Lập trình Hướng đối tượng sử dụng Java**, trình bày từ cơ bản đến nâng cao, kèm ví dụ, so sánh và bài tập.

---

## 🗺️ LỘ TRÌNH HỌC (6 CHƯƠNG)

```
Chương 1: Java cơ bản + Tư duy OOP + Môi trường
    ↓
Chương 2: Class & Object, Encapsulation, Static
    ↓
Chương 3: Kế thừa & Đa hình (trái tim OOP)
    ↓
Chương 4: SOLID, UML, Design Patterns
    ↓
Chương 5: Exception, Testing, File I/O
    ↓
Chương 6: Collections, Generics, Lambda, Stream API
```

**Nguyên tắc học:** Mỗi chương xây trên chương trước. Nếu thiếu kiến thức nền, quay lại chương trước (xem mục **Kiến thức cần có** ở đầu mỗi chương).

---

## 📚 DANH MỤC CHƯƠNG

| Chương | File | Nội dung chính | Code chạy thử |
|--------|------|----------------|---------------|
| **1** | [CHUONG_1_LT.md](CHUONG_1_LT.md) | Java cơ bản, OOP, Git, Maven | `CODE/VI_DU/CHUONG_1/` |
| **2** | [CHUONG_2_LT.md](CHUONG_2_LT.md) | Class/Object, Encapsulation, Static | `CODE/VI_DU/CHUONG_2/` |
| **3** | [CHUONG_3_LT.md](CHUONG_3_LT.md) | Inheritance, Polymorphism, Composition | `CODE/VI_DU/CHUONG_3/` (6 ví dụ) |
| **4** | [CHUONG_4_LT.md](CHUONG_4_LT.md) | SOLID, UML, Singleton, Factory | `CODE/VI_DU/CHUONG_4/` |
| **5** | [CHUONG_5_LT.md](CHUONG_5_LT.md) | Exception, JUnit, File I/O | `CODE/VI_DU/CHUONG_5/` |
| **6** | [CHUONG_6_LT.md](CHUONG_6_LT.md) | Collections, Lambda, Stream | `CODE/VI_DU/CHUONG_6/` |

> **Chi tiết lệnh chạy & output:** [CODE/VI_DU/README.md](../CODE/VI_DU/README.md)

---

## 🔗 LIÊN KẾT KIẾN THỨC GIỮA CÁC CHƯƠNG

| Từ chương | Kiến thức | Dùng ở chương | Ghi chú |
|-----------|-----------|---------------|---------|
| 1 | `new`, reference, null | 2 | Tạo object, Stack vs Heap |
| 1 | `static` method | 2 | Static members (2.6) |
| 1 | Wrapper Classes | 6 | `List<Integer>` không dùng `int` |
| 1 | Mảng | 6 | So sánh với List |
| 2 | Class, access modifiers | 3 | Inheritance, protected |
| 2 | `this` | 3 | So sánh với `super` |
| 3 | Interface, Polymorphism | 4 | OCP, DIP, Factory |
| 3 | Abstract class | 4 | Shape, PaymentMethod |
| 4 | DIP + DI | 5 | Test với mock dependency |
| 5 | Exception, File I/O | 6 | Stream đọc file, xử lý lỗi |

---

## 📋 ĐỀ THI & ÔN TẬP

| Loại | File |
|------|------|
| Giữa kỳ | DE-THI_GIUA_KY_1.MD, DE-THI_GIUA_KY_2.MD, DE-THI_GIUA_KY_3.MD |
| Cuối kỳ | DE-THI_CUOI_KY_1.MD, DE-THI_CUOI_KY_2.MD, DE-THI_CUOI_KY_3.MD |
| Đáp án cuối kỳ | DA_DE-THI_CUOI_KY_1.MD, DA_DE-THI_CUOI_KY_2.MD, DA_DE-THI_CUOI_KY_3.MD |

**Gợi ý ôn tập theo chủ đề thi:**

- **Cú pháp & OOP cơ bản:** Chương 1 (phần 0), 2, 3
- **Thiết kế & mẫu:** Chương 4 (SOLID, Singleton, Factory)
- **Xử lý lỗi & kiểm thử:** Chương 5
- **Collections & Stream:** Chương 6

---

## ✅ CHECKLIST TRƯỚC KHI VÀO TỪNG CHƯƠNG

### Vào Chương 2
- [ ] Viết được Hello World, if/for, method
- [ ] Hiểu primitive vs reference, `null`
- [ ] Đã dùng `new` trong ví dụ (sẽ học sâu ở Ch.2)

### Vào Chương 3
- [ ] Tạo class với private fields, getters/setters
- [ ] Hiểu constructor và `this`
- [ ] Biết `public` / `protected` / `private`

### Vào Chương 4
- [ ] `extends`, `implements`, override
- [ ] Polymorphism (biến kiểu cha, object kiểu con)
- [ ] Interface vs abstract class

### Vào Chương 5
- [ ] Class, inheritance (custom exception)
- [ ] Try-catch cơ bản (Ch.1 nếu có)

### Vào Chương 6
- [ ] **Wrapper Classes** (Ch.1 – bắt buộc)
- [ ] Interface, method reference cơ bản
- [ ] Mảng và vòng lặp

---

## 🎯 MỤC TIÊU MÔN HỌC (TÓM TẮT)

Sau khóa học, sinh viên có thể:

1. Viết chương trình Java hướng đối tượng đúng chuẩn
2. Thiết kế class với encapsulation, kế thừa, đa hình
3. Áp dụng SOLID và pattern cơ bản (Singleton, Factory)
4. Xử lý exception và viết unit test (JUnit 5)
5. Dùng Collections và Stream API xử lý dữ liệu
6. Làm việc với Git, Maven và clean code

---

## 📖 TÀI LIỆU THAM KHẢO CHUNG

- [Oracle Java Tutorials](https://docs.oracle.com/javase/tutorial/)
- *Effective Java* – Joshua Bloch
- *Head First Java* / *Head First Design Patterns* – O'Reilly
- [SOLID (Wikipedia)](https://en.wikipedia.org/wiki/SOLID)

---

*Cập nhật giáo trình: bổ sung liên kết chương, SOLID đầy đủ, bảng so sánh và ví dụ dễ hiểu.*
