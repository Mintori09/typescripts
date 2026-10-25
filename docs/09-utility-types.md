# Utility Types

TypeScript cung cấp sẵn nhiều **Utility Types** để biến đổi và thao tác kiểu dữ liệu thuận tiện mà không cần phải viết lại code thủ công.

---

## 1. Biến đổi thuộc tính của Object

### `Partial<Type>`
Biến tất cả thuộc tính của `Type` thành **tùy chọn (optional `?:`)**:
```typescript
interface Todo {
  title: string;
  description: string;
}

// Thích hợp cho hàm cập nhật (update/patch)
function updateTodo(todo: Todo, fieldsToUpdate: Partial<Todo>): Todo {
  return { ...todo, ...fieldsToUpdate };
}

updateTodo({ title: "Học TS", description: "docs" }, { title: "Học Utility Types" });
```

### `Required<Type>`
Ngược lại với `Partial`, biến tất cả thuộc tính tùy chọn thành **bắt buộc**:
```typescript
interface Props {
  a?: number;
  b?: string;
}

const obj: Required<Props> = { a: 5, b: "yes" }; // Thiếu 'a' hoặc 'b' sẽ báo lỗi
```

### `Readonly<Type>`
Biến tất cả thuộc tính của `Type` thành **chỉ đọc (`readonly`)**:
```typescript
interface Config {
  apiUrl: string;
}

const config: Readonly<Config> = { apiUrl: "https://api.example.com" };
// config.apiUrl = "new url"; // Lỗi: Cannot assign to 'apiUrl' because it is a read-only property
```

### `Pick<Type, Keys>`
Trích xuất một tập hợp các thuộc tính cụ thể từ `Type`:
```typescript
interface User {
  id: number;
  name: string;
  email: string;
  address: string;
}

// Chỉ lấy id và name:
type UserSummary = Pick<User, "id" | "name">;
// { id: number; name: string }
```

### `Omit<Type, Keys>`
Ngược lại với `Pick`, loại bỏ một hoặc nhiều thuộc tính khỏi `Type`:
```typescript
// Lấy toàn bộ User ngoại trừ address:
type UserWithoutAddress = Omit<User, "address">;
// { id: number; name: string; email: string }
```

---

## 2. Thao tác với Map & Union

### `Record<Keys, Type>`
Tạo kiểu object với tập hợp các khóa `Keys` và kiểu dữ liệu giá trị là `Type`:
```typescript
type Page = "home" | "about" | "contact";

interface PageInfo {
  title: string;
}

const nav: Record<Page, PageInfo> = {
  home: { title: "Trang chủ" },
  about: { title: "Giới thiệu" },
  contact: { title: "Liên hệ" },
};
```

### `Exclude<UnionType, ExcludedMembers>`
Loại bỏ các thành viên khỏi một Union type:
```typescript
type T0 = Exclude<"a" | "b" | "c", "a">; // "b" | "c"
type T1 = Exclude<string | number | (() => void), Function>; // string | number
```

### `Extract<Type, Union>`
Chỉ trích xuất những kiểu nào thuộc cả 2 tập hợp (phép giao):
```typescript
type T2 = Extract<"a" | "b" | "c", "a" | "f">; // "a"
```

### `NonNullable<Type>`
Loại bỏ `null` và `undefined` khỏi `Type`:
```typescript
type MaybeString = string | null | undefined;
type DefiniteString = NonNullable<MaybeString>; // string
```

---

## 3. Thao tác với Function, Class & Promise

### `ReturnType<Type>`
Lấy kiểu dữ liệu trả về của một hàm:
```typescript
function getUser() {
  return { id: 1, name: "mintori" };
}

type UserResult = ReturnType<typeof getUser>; // { id: number; name: string }
```

### `Parameters<Type>`
Lấy một Tuple chứa danh sách kiểu các tham số truyền vào hàm:
```typescript
function greet(name: string, age: number): void {}

type GreetArgs = Parameters<typeof greet>; // [name: string, age: number]
```

### `Awaited<Type>`
Giải phóng (unwrap) kiểu dữ liệu bên trong một `Promise` (kể cả Promise lồng nhau):
```typescript
async function fetchData(): Promise<{ status: number }> {
  return { status: 200 };
}

// Trích xuất trực tiếp kiểu giá trị sau khi await:
type Data = Awaited<ReturnType<typeof fetchData>>; // { status: number }
```

### `InstanceType<Type>`
Lấy kiểu thể hiện (instance) của một Constructor function hoặc Class:
```typescript
class DatabaseConnection {
  connect() {}
}

type DBInstance = InstanceType<typeof DatabaseConnection>; // DatabaseConnection
```
