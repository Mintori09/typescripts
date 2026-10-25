# Advanced Types & Decorators

Các kỹ thuật biến đổi kiểu nâng cao (Type Metaprogramming) và Decorators trong TypeScript.

---

## 1. Literal Types

Giới hạn giá trị của biến chỉ được phép nhận chính xác một giá trị cụ thể thay vì kiểu nguyên thủy rộng:

```typescript
// String Literal Types
type Direction = "north" | "south" | "east" | "west";

// Numeric Literal Types
type DiceRoll = 1 | 2 | 3 | 4 | 5 | 6;

// Boolean Literal Types
type YesOnly = true;
```

---

## 2. Template Literal Types

Dùng cú pháp backticks để ghép các chuỗi kiểu lại với nhau:

```typescript
type World = "world";
type Greeting = `hello ${World}`; // "hello world"

type EventType = "click" | "hover";
type EventHandler = `on${Capitalize<EventType>}`; // "onClick" | "onHover"

// Tạo CSS Padding helpers
type Size = "sm" | "md" | "lg";
type MarginClass = `m-${Size}` | `p-${Size}`; // "m-sm" | "m-md" | "m-lg" | "p-sm" ...
```

---

## 3. Mapped Types

Duyệt qua danh sách các key (thường là `keyof`) để tạo ra một object type mới:

```typescript
type FeatureFlags = {
  darkMode: () => void;
  newUserOnboarding: () => void;
};

// Biến tất cả phương thức thành boolean flag:
type OptionsFlags<Type> = {
  [Property in keyof Type]: boolean;
};

type FeatureOptions = OptionsFlags<FeatureFlags>;
// { darkMode: boolean; newUserOnboarding: boolean }

// Thêm hoặc xóa modifiers (+readonly, -readonly, +?, -?):
type Mutable<Type> = {
  -readonly [Property in keyof Type]: Type[Property]; // Xóa readonly
};

type Concrete<Type> = {
  [Property in keyof Type]-?: Type[Property]; // Xóa dấu ?, biến thành bắt buộc
};
```

---

## 4. Conditional Types (`T extends U ? X : Y`)

Kiểu dữ liệu rẽ nhánh theo điều kiện, tương tự toán tử ba ngôi `condition ? a : b`:

```typescript
type IsString<T> = T extends string ? "yes" : "no";

type A = IsString<string>; // "yes"
type B = IsString<number>; // "no"

// Sử dụng từ khóa 'infer' để suy luận kiểu ẩn bên trong:
type UnpackArray<T> = T extends (infer Element)[] ? Element : T;

type Item1 = UnpackArray<string[]>; // string
type Item2 = UnpackArray<number>;   // number
```

---

## 5. Recursive Types (Kiểu đệ quy)

Một kiểu dữ liệu có thể tự tham chiếu đến chính nó, cực kỳ hữu dụng để mô hình hóa cấu trúc cây (Tree) hoặc định dạng JSON:

```typescript
// Định nghĩa kiểu dữ liệu JSON hoàn chỉnh
type JSONValue =
  | string
  | number
  | boolean
  | null
  | JSONValue[]
  | { [key: string]: JSONValue };

const sampleJson: JSONValue = {
  name: "mintori",
  skills: ["TS", "JS"],
  profile: {
    age: 20,
    active: true,
  },
};

// Cấu trúc cây thư mục (File Tree)
type FileNode = {
  name: string;
  children?: FileNode[]; // Tự tham chiếu đệ quy
};
```

---

## 6. Decorators

Decorators là cú pháp `@decorator` gắn vào Class, Method, Accessor hoặc Field để bổ sung metadata hoặc thay đổi hành vi:

```typescript
// Method Decorator: Đo thời gian thực thi của hàm
function LogExecutionTime(
  target: any,
  propertyKey: string,
  descriptor: PropertyDescriptor
) {
  const originalMethod = descriptor.value;

  descriptor.value = function (...args: any[]) {
    console.time(propertyKey);
    const result = originalMethod.apply(this, args);
    console.timeEnd(propertyKey);
    return result;
  };

  return descriptor;
}

class ApiService {
  @LogExecutionTime
  fetchData() {
    // Giả lập xử lý nặng
    for (let i = 0; i < 1_000_000; i++) {}
    return "Xong";
  }
}
```
*Lưu ý: Decorators hiện nay đã là chuẩn Stage 3 của ECMAScript (hỗ trợ native từ TypeScript 5.0+).*
