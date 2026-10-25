# TypeScript Interfaces

`interface` là cách định nghĩa hợp đồng (contract) cho cấu trúc của một đối tượng hoặc phương thức trong TypeScript.

---

## 1. Interface Declaration

Cú pháp khai báo cơ bản với các loại thuộc tính:

```typescript
interface User {
  readonly id: number;      // Chỉ đọc, không thể gán lại sau khi tạo
  name: string;             // Bắt buộc
  age?: number;             // Tùy chọn (optional)
  email?: string | null;    // Cho phép string hoặc null

  // Phương thức
  sayHi(): string;
  login(token: string): boolean;
}
```

---

## 2. Extending Interfaces (`extends`)

Interface có thể kế thừa từ một hoặc nhiều interface khác để tái sử dụng:

```typescript
interface Animal {
  name: string;
}

interface Swimmable {
  swimSpeed: number;
}

// Kế thừa đa interface
interface Duck extends Animal, Swimmable {
  quack(): void;
}

const donald: Duck = {
  name: "Donald",
  swimSpeed: 10,
  quack() {
    console.log("Quack quack!");
  },
};
```

---

## 3. Types vs Interfaces

| Tiêu chí | `interface` | `type` (Type Alias) |
| :--- | :--- | :--- |
| **Mở rộng** | Dùng từ khóa `extends` | Dùng Intersection `&` |
| **Declaration Merging** | Có thể khai báo trùng tên nhiều lần để gộp thuộc tính lại | Báo lỗi duplicate identifier |
| **Khả năng biểu diễn** | Chủ yếu dành cho Object & Class | Biểu diễn được mọi thứ (Primitive, Union, Tuple, Function) |
| **Hiệu năng biên dịch** | Tối ưu hơn cho type checking của object phân tầng | Chậm hơn đôi chút nếu có nhiều `&` lồng nhau phức tạp |

> **Khuyên dùng:** Ưu tiên dùng `interface` khi định nghĩa cấu trúc Object hoặc thiết kế Class/OOP. Dùng `type` khi cần Union (`|`), Tuple hoặc các phép biến đổi type nâng cao.

---

## 4. Declaration Merging (Gộp khai báo)

Nếu khai báo nhiều `interface` cùng tên trong cùng scope, TypeScript sẽ tự động hợp nhất các trường:

```typescript
interface Box {
  height: number;
  width: number;
}

interface Box {
  scale: number; // Tự động gộp thêm thuộc tính này
}

const myBox: Box = {
  height: 10,
  width: 20,
  scale: 1.5, // Bắt buộc phải có cả 3 thuộc tính
};
```
*Tính năng này thường dùng khi mở rộng kiểu của các thư viện bên thứ 3 (Module/Global Augmentation).*

---

## 5. Hybrid Types

Một đối tượng vừa có thể gọi như một hàm, vừa mang theo các thuộc tính bổ sung:

```typescript
interface Counter {
  (start: number): string; // Cho phép gọi: counter(10)
  interval: number;        // Thuộc tính phụ
  reset(): void;           // Phương thức phụ
}

function getCounter(): Counter {
  const counter = function (start: number) {
    return `Started at ${start}`;
  } as Counter;

  counter.interval = 123;
  counter.reset = function () {
    console.log("Counter reset");
  };

  return counter;
}

const c = getCounter();
c(10); // Gọi như hàm
c.reset(); // Gọi phương thức
console.log(c.interval); // Đọc thuộc tính
```
