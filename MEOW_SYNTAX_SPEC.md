# Meow-Lang Language Specification

Comprehensive reference for the syntax, primitive data types, built-in functions, and module architecture of `meow-lang`. Used as a specification for compiler development, writing meow-lang code, or developing VS Code Extensions (Syntax Highlighting, Snippets).

---

## 1. Files & Basic Structure

- **File Extension**: `.meow`
- **Comments**:
  - Single-line: `// comment content`
  - Multi-line: `/* comment content */`
- **Statement Terminator**: Semicolon `;` (required after `var`, `const`, `return`, assignments, `import`, `break`, `continue`).

---

## 2. Keywords & Character Identification

### Keywords

```text
function    type        interface   import      as
var         const       return
if          else        switch      case        default
for         in          break       continue
true        false
```

### Built-in Primitive Types

```text
int         decimal     string      boolean     any
```

### Operators & Punctuation

```text
::          (Module namespace separator: console::println, string::len)
=           (Value assignment)
...         (Variadic parameter: int... a)
.           (Struct/interface field access: cat.name)
,           (List, element, and field separator)
;           (Statement terminator)
( )         (Expression / function call parentheses)
{ }         (Block / struct braces)
[ ]         (Array brackets)
```

---

## 3. Data Types & Literals

| Type          | Size / Internal Layout                      | Literal / Initialization Syntax   | Examples                                                  |
| ------------- | ------------------------------------------- | --------------------------------- | --------------------------------------------------------- |
| **`int`**     | 64-bit integer (`i64`)                      | Primitive integer                 | `0`, `42`, `-10`, `2026`                                  |
| **`decimal`** | 64-bit floating point (`double`)            | Floating-point number             | `3.14`, `0.5`, `12.56636`                                 |
| **`string`**  | C-string byte pointer                       | Single quotes (escape `\'`, `\\`) | `'Hello'`, `'Result: '`                                   |
| **`boolean`** | 1-bit boolean (`i1`)                        | Boolean literal                   | `true`, `false`                                           |
| **`any`**     | 16-byte Tagged Union `{ i64 tag, i64 val }` | Polymorphic (holds any type)      | Assigned from any value                                   |
| **`array`**   | 32-byte dynamic header pointer              | `type[e1, e2, ...]`               | `int[1, 2, 3]`, `string['a', 'b']`, `any[1, 'cat', true]` |
| **`struct`**  | User-defined data structure                 | `TypeName { field: val, ... }`    | `Cat { name: 'Miu', age: 2 }`                             |

---

## 4. Declarations & Definitions

### Module Import (`import`)

```meow
import 'math/advanced' as my_math;
import 'core/lexer' as lex;
```

### Struct Definition (`type`)

```meow
type Cat {
    string name,
    int age,
    boolean is_cute
}
```

### Structural Interface Definition (`interface`)

_(Compile-time Duck Typing: any struct possessing all matching fields by name and type automatically satisfies the interface without explicit `implements` keywords)._

```meow
interface HasContent {
    string content
}
```

### Variable Declarations (`var` & `const`)

```meow
var a = 10;
const PI = 3.14;
var arr = int[1, 2, 3];
var dynamic_val = any;
```

### Function Declarations (`function`)

```meow
// 1. Basic Function
function int add_nums(int a, int b) {
    return add(a, b);
}

// 2. Function Overloading
function decimal calculate(decimal a, decimal b) {
    return multiply(a, b);
}

function int calculate(int a, int b) {
    return add(a, b);
}

// 3. Interface Parameter (Structural Polymorphism)
function void display(HasContent item) {
    println(item.content);
}

// 4. Variadic Parameters (type... name)
// Within the function body, this acts as a dynamic array
function int sum_all(int... numbers) {
    var total = 0;
    for n in numbers {
        total = add(total, n);
    }
    return total;
}

// Combining fixed parameters with variadic parameters
function string join_words(string prefix, string... words) {
    var res = prefix;
    for w in words {
        res = string::concat(res, w);
    }
    return res;
}
```

---

## 5. Control Structures & Loops

### Branching `if - else`

```meow
if (greater_than(score, 90)) {
    println('Excellent!');
} else {
    println('Keep trying!');
}
```

### `switch - case` (Go-style: multi-value matching, no break needed, no fall-through)

```meow
switch (day) {
    case 2, 3, 4, 5, 6 {
        println('Weekday');
    }
    case 7, 8 {
        println('Weekend');
    }
    default {
        println('Invalid');
    }
}
```

### Loops: `for - in`

```meow
// 1. Iterating over an array
for item in fruits {
    println(item);
}

// 2. Iterating over built-in range(start, end)
for i in range(0, 9) {
    if (equal(i, 2)) {
        continue;
    }
    if (equal(i, 5)) {
        break;
    }
    println(i);
}
```

---

## 6. Built-in Functions & Modules

### A. Arithmetic & Logical Operators (Prefix Builtins)

- **Arithmetic**:
  - `add(a, b)`: Addition `+`
  - `subtract(a, b)`: Subtraction `-`
  - `multiply(a, b)`: Multiplication `*`
  - `divide(a, b)`: Division `/`
  - `modulo(a, b)`: Remainder `%`
- **Comparison**:
  - `equal(a, b)`: Equality `==`
  - `not_equal(a, b)`: Inequality `!=`
  - `greater_than(a, b)`: Greater than `>`
  - `greater_equal(a, b)` / `greater_than_or_equal(a, b)`: `>=`
  - `less_than(a, b)`: Less than `<`
  - `less_equal(a, b)` / `less_than_or_equal(a, b)`: `<=`
- **Short-circuit Logic**:
  - `and(a, b)`: Short-circuit logical AND
  - `or(a, b)`: Short-circuit logical OR
  - `not(a)`: Logical NOT `!`
- **Runtime Type & Null Check**:
  - `type(any a) -> string`: Returns type name (`'int'`, `'decimal'`, `'string'`, `'boolean'`, `'array'`).
  - `is_null(any a) -> boolean`: Checks if a pointer is null.

---

### B. Module `console::` & Terminal I/O

- `console::print(val)` / `print(val)`: Prints to terminal without trailing newline.
- `console::println(val)` / `println(val)`: Prints to terminal with trailing newline.
  - _Polymorphic_: Supports `int`, `decimal`, `string`, `boolean`, `array`, `any`.

---

### C. Module `array::` (Dynamic Array Operations)

- `array::length(arr) -> int`: Returns number of elements.
- `array::get(arr, index) -> element`: Retrieves element at `index`.
- `array::set(arr, index, value)`: Updates element at `index`.
- `array::append(arr, value)`: Appends an element to the end of array (auto-resizing buffer).

---

### D. Module `string::` (String Processing)

- `string::len(s) -> int`: Returns string length.
- `string::get_char(s, index) -> int`: Returns ASCII character code (0-255) at `index`.
- `string::from_char(charCode) -> string`: Creates a 1-character string from ASCII code.
- `string::concat(s1, s2) -> string`: Concatenates two strings.
- `string::substr(s, start, length) -> string`: Extracts substring safely.
- `string::find(s, target) -> int`: Returns index of first occurrence or `-1`.
- `string::from_int(value) -> string`: Converts integer to string.
- `string::to_int(s) -> int`: Parses integer from string.
- `string::trim(s) -> string`: Strips whitespace (`' '`, `'\t'`, `'\n'`, `'\r'`) from both ends.

---

### E. Module `file::` (File I/O)

- `file::read(string path) -> string`: Reads entire file content into string.
- `file::write(string path, string content) -> boolean`: Overwrites content to file.
- `file::append(string path, string content) -> boolean`: Appends content to file.
- `file::exists(string path) -> boolean`: Checks if file exists (`true` / `false`).

---

### F. Module `sys::` (System Commands)

- `sys::exec(string command) -> int`: Executes shell command (invoking compiler, linker, directory creation, etc.). Returns exit code (`0` on success).

---

## 7. Recommended TextMate Grammar Scope Mapping

| Syntax Group                 | Example Pattern                                                           | Suggested TextMate Scope                   |
| ---------------------------- | ------------------------------------------------------------------------- | ------------------------------------------ | --------------------------- |
| **Control Keywords**         | `\b(if\|else\|switch\|case\|default\|for\|in\|break\|continue\|return)\b` | `keyword.control.meow`                     |
| **Declaration Keywords**     | `\b(function\|type\|interface\|import\|as\|var\|const)\b`                 | `keyword.control.meow`                     |
| **Primitive Types**          | `\b(int\|decimal\|string\|boolean\|any\|void)\b`                          | `support.type.primitive.meow`              |
| **User Defined Types**       | `\b[A-Z][a-zA-Z0-9_]*\b`                                                  | `entity.name.type.meow`                    |
| **Module Namespace**         | `\b(console\|array\|string\|file\|sys)(?=::)`                             | `support.class.meow`                       |
| **Function Calls**           | `[a-zA-Z_][a-zA-Z0-9_]*(?=\s*\()`                                         | `entity.name.function.meow`                |
| **Boolean Literals**         | `\b(true\|false)\b`                                                       | `constant.language.boolean.meow`           |
| **Numeric Literals**         | `\b[0-9]+(\.[0-9]+)?\b`                                                   | `constant.numeric.meow`                    |
| **String Literals**          | `'(\\.                                                                    | [^'])\*'`                                  | `string.quoted.single.meow` |
| **Punctuation & Separators** | `::\|...\|\.\|=                                                           | ,\|;`                                      | `keyword.control.meow`      |
| **Comments**                 | `//.*$` or `/\*[\s\S]*?\*/`                                               | `comment.line.meow` / `comment.block.meow` |
