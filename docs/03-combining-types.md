# Combining Types

Các cách kết hợp, gom nhóm và trích xuất kiểu dữ liệu trong TypeScript.

---

## 1. Union Types (`|`)

Biểu thị một giá trị có thể thuộc **một trong nhiều kiểu** khác nhau.

```typescript
// Biến có thể là string hoặc number
let id: string | number;
id = "ABC-123"; // Hợp lệ
id = 456;       // Hợp lệ
// id = false;  // Lỗi: Type 'boolean' is not assignable to type 'string | number'

// Trong hàm:
function printId(id: string | number) {
  if (typeof id === "string") {
    console.log(id.toUpperCase());
  } else {
    console.log(id.toFixed(2));
  }
}
```

---

## 2. Intersection Types (`&`)

Kết hợp **nhiều kiểu lại với nhau**, đối tượng mới phải thỏa mãn **tất cả** các thuộc tính của các kiểu thành phần.

```typescript
type Timestamps = {
  createdAt: Date;
  updatedAt: Date;
};

type User = {
  id: number;
  name: string;
};

// UserWithMeta có đủ thuộc tính của cả User và Timestamps
type UserWithMeta = User & Timestamps;

const member: UserWithMeta = {
  id: 1,
  name: "mintori",
  createdAt: new Date(),
  updatedAt: new Date(),
};
```

---

## 3. Type Aliases (`type`)

Dùng để đặt tên (bí danh) cho bất kỳ kiểu dữ liệu nào (Primitive, Union, Tuple, Object,...).

```typescript
// Bí danh cho Union type
type Status = "pending" | "fulfilled" | "rejected";

// Bí danh cho Object
type Point = {
  x: number;
  y: number;
};

// Bí danh cho Function
type Callback = (data: string) => void;

function fetchUser(onSuccess: Callback) {
  onSuccess("data loaded");
}
```

---

## 4. `keyof` Operator

Toán tử `keyof` lấy ra tất cả các tên thuộc tính (keys) của một object type và tạo thành một **Union của các string literal**.

```typescript
interface Product {
  id: number;
  title: string;
  price: number;
}

// type ProductKey = "id" | "title" | "price"
type ProductKey = keyof Product;

// Hàm đọc thuộc tính an toàn (Type-safe property getter)
function getProperty<T, K extends keyof T>(obj: T, key: K): T[K] {
  return obj[key];
}

const laptop: Product = { id: 101, title: "ThinkPad", price: 1500 };

const title = getProperty(laptop, "title"); // Kiểu trả về tự động là string
const price = getProperty(laptop, "price"); // Kiểu trả về tự động là number
// getProperty(laptop, "discount"); // Lỗi: Argument of type '"discount"' is not assignable
```
