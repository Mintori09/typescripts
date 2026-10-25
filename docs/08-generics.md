# Generics in TypeScript

**Generics** cho phép viết các hàm, interface hoặc class có khả năng tái sử dụng cao với nhiều kiểu dữ liệu khác nhau trong khi vẫn **bảo toàn hoàn toàn tính an toàn kiểu dữ liệu (Type Safety)** thay vì phải dùng `any`.

---

## 1. Generic Types

Dùng biến kiểu (thường đặt tên là `T`, `U`, `V`,...) như một tham số truyền vào:

### A. Generic Functions
```typescript
// T sẽ tự động khớp với kiểu của dữ liệu truyền vào
function identity<T>(arg: T): T {
  return arg;
}

const str = identity("Xin chào"); // TS tự suy luận T là string, str có kiểu string
const num = identity<number>(123); // Khai báo tường minh T là number
```

### B. Generic Interfaces
```typescript
interface ApiResponse<TData> {
  status: "success" | "error";
  data: TData;
  timestamp: number;
}

interface UserProfile {
  id: number;
  username: string;
}

const res: ApiResponse<UserProfile> = {
  status: "success",
  data: { id: 1, username: "mintori" },
  timestamp: Date.now(),
};
```

### C. Generic Classes
```typescript
class Box<T> {
  private content: T;

  constructor(value: T) {
    this.content = value;
  }

  getContent(): T {
    return this.content;
  }
}

const numberBox = new Box<number>(100);
console.log(numberBox.getContent() + 50); // Hợp lệ, biết chắc là number
```

---

## 2. Generic Constraints (`extends`)

Đôi khi bạn muốn `T` không phải là bất kỳ thứ gì, mà bắt buộc phải có một số thuộc tính nhất định. Dùng từ khóa `extends` để đặt điều kiện ràng buộc.

### Ràng buộc với Interface:
```typescript
interface HasLength {
  length: number;
}

// Bắt buộc T phải có thuộc tính .length
function logLength<T extends HasLength>(item: T): number {
  console.log(`Độ dài: ${item.length}`);
  return item.length;
}

logLength("Chuỗi văn bản"); // Hợp lệ (string có .length)
logLength([1, 2, 3]);       // Hợp lệ (mảng có .length)
logLength({ length: 10, value: "abc" }); // Hợp lệ
// logLength(123); // Lỗi: number không có thuộc tính length
```

### Ràng buộc khóa với `keyof`:
Đảm bảo khóa `K` phải thuộc các thuộc tính của đối tượng `T`:

```typescript
function getProp<T, K extends keyof T>(obj: T, key: K): T[K] {
  return obj[key];
}

const user = { name: "An", age: 20 };
getProp(user, "name"); // Hợp lệ, trả về string
// getProp(user, "address"); // Báo lỗi ngay lập tức vì 'address' không nằm trong keyof user
```

---

## 3. Generic Type Defaults (Giá trị mặc định cho Generic)

Tương tự tham số mặc định của hàm, bạn có thể gán kiểu mặc định cho `T`:

```typescript
interface Container<T = string> {
  value: T;
}

const c1: Container = { value: "mặc định là string" };
const c2: Container<number> = { value: 999 };
```
