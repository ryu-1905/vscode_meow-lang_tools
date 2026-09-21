<p align="center">
  <img src="images/icon.png" width="128" height="128" alt="Meow Lang Logo" />
</p>

<h1 align="center">Meow Lang Tools for Visual Studio Code</h1>

<p align="center">
  Extension hỗ trợ toàn diện cho ngôn ngữ lập trình <b>meow-lang</b> (.meow) trên Visual Studio Code.
</p>

---

## ✨ Tính năng nổi bật (Features)

- 🎨 **Cú pháp màu sắc trực quan (Rich Syntax Highlighting)**:
  - Phân loại rõ ràng từ khóa (`if`, `switch`, `for`, `function`, `type`...).
  - Nhận diện hàm built-in (`print`, `add`, `equal`, `range`...), namespace module (`console::`, `string::`, `file::`...).
  - Phân biệt biến, hằng số (`const PI`), kiểu dữ liệu nguyên thủy và struct tự định nghĩa.
  - Tối ưu bảng màu dịu mắt, tránh các ký tự màu trắng chói.
- ⚡ **Code Snippets tiện lợi**:
  - Gõ tắt nhanh các cấu trúc thông dụng như `print`, `fn`, `type`, `switch`, `for`, `if` chỉ với một phím `Tab`.
- 📐 **Trải nghiệm soạn thảo thông minh (Smart Editing)**:
  - Tự động đóng cặp ngoặc tròn `()`, vuông `[]`, nhọn `{}` và chuỗi ký tự nháy đơn `''`.
  - Tự động thụt dòng (Indentation) chuẩn xác khi xuống dòng.
  - Hỗ trợ comment một dòng `//` (phím tắt `Ctrl + /`) và nhiều dòng `/* ... */`.
  - Hỗ trợ comment tài liệu `/** ... */` tự động điền `* ` khi Enter.
  - Hỗ trợ Code Folding khối lệnh `{ ... }` và vùng `// #region` ... `// #endregion`.

---

## 🚀 Danh mục Snippets gõ tắt

| Phím tắt (Prefix) | Bấm phím | Mã sinh ra                                                                     |
| :---------------- | :------: | :----------------------------------------------------------------------------- |
| `print`           |  `Tab`   | `print('Hello Meow!');`                                                        |
| `fn`              |  `Tab`   | Khai báo hàm `function void name() { ... }` _(hỗ trợ menu chọn kiểu trả về)_   |
| `fnvar`           |  `Tab`   | Khai báo hàm tham số biến thiên `function int sum_all(int... numbers) { ... }` |
| `type`            |  `Tab`   | Định nghĩa Struct `type Name { ... }`                                          |
| `interface`       |  `Tab`   | Định nghĩa Structural Interface `interface HasContent { ... }`                 |
| `if`              |  `Tab`   | Câu lệnh điều kiện `if (condition) { ... }`                                    |
| `ifelse`          |  `Tab`   | Cấu trúc rẽ nhánh `if (...) { ... } else { ... }`                              |
| `switch`          |  `Tab`   | Cấu trúc `switch (value) { case 1 { ... } default { ... } }`                   |
| `for`             |  `Tab`   | Vòng lặp duyệt danh sách `for item in collection { ... }`                      |
| `forr`            |  `Tab`   | Vòng lặp khoảng số `for i in range(0, 10) { ... }`                             |
| `var`             |  `Tab`   | Khai báo biến `var name = value;`                                              |
| `const`           |  `Tab`   | Khai báo hằng số `const NAME = value;`                                         |
| `import`          |  `Tab`   | `import 'module/path' as alias;`                                               |

---

## 📂 Cấu trúc dự án

```text
vscode_meow-lang_tools/
├── images/
│   ├── icon.png                 # Logo PNG chuẩn cho VS Code
│   └── icon.svg                 # File logo vector gốc
├── snippets/
│   └── meow.json                # Định nghĩa toàn bộ code snippets
├── syntaxes/
│   └── meow.tmLanguage.json     # Bộ quy tắc TextMate Grammar bôi màu cú pháp
├── language-configuration.json  # Cấu hình thụt dòng, auto-closing ngoặc, comment
├── package.json                 # Manifest khai báo extension
├── MEOW_SYNTAX_SPEC.md          # Đặc tả chi tiết cú pháp ngôn ngữ meow-lang
└── README.md                    # Tài liệu hướng dẫn
```

---

## 📝 Giấy phép (License)

Phát triển dành riêng cho cộng đồng lập trình viên `meow-lang`.
