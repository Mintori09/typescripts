## List of types

### Primitvie Types

- boolean : true | false
- number : integer numbers | floating-point numberrs (use double precision 64-bit floating point format IEEE 754)
- string : sequence of characters | single quote '' | double quote "" | backticks ``
- void : absence of a value | A function declared with a void return type is not expected to return any data back to the caller.
- undefined : declared but has not yet been assigned a value | NaN
- null : intentional absence of a value | 0

### Object Types

- interface : set of properties and their types

```typescript
interface PaintOptions {
  shape: Shape;
  xPos?: number;
  yPos?: number;
}
function paintShape({ shape, xPos = 0, yPos = 0 }: PaintOptions) {}
interface children extends parent {}
```

- class

```typescript
class Point {
  x: number;
  y: number;
}
const pt = new Point();

class OKGreeter {
  // Not initialized, but no error
  name!: string;
}
readonly : prevents asignments to the field outside of the constructor

class Base {
  greet() {
    console.log("Hello, world!");
  }
}

class Derived extends Base {
  greet(name?: string) {
    if (name === undefined) {
      super.greet();
    } else {
      console.log(`Hello, ${name.toUpperCase()}`);
    }
  }
}

const d = new Derived();
d.greet();
d.greet("reader");
```

- enum

```typescript
enum HttpStatus {
    OK = 200,
    BadRequest = 400,
    Unauthorized = 401, 
    NotFound = 404,
}
const HTTP_OK = HttpStatus.OK;

/// Sau khi biên dịch thành js : HTTP_OK = 200;
```

- Array : mảng chứa các phần tử cùng kiểu

```typescript
const numbers: number[] = [1, 2, 3];
const names: Array<string> = ["Alice", "Bob"]; // Generic syntax
const readonlyList: readonly number[] = [1, 2]; // Không thể push/pop/sửa
```

- Tuple : mảng có độ dài cố định và biết trước kiểu của từng vị trí

```typescript
let user: [number, string] = [1, "mintori"]; // Đúng thứ tự: [id, name]
let response: [status: number, message?: string] = [200]; // Named & optional element
```

- Object : định nghĩa cấu trúc đối tượng (object literal type) hoặc kiểu tham chiếu không nguyên thủy

```typescript
// Object literal type (phổ biến)
const car: { brand: string; year?: number } = { brand: "Toyota" };

// object (non-primitive): bất kỳ thứ gì không phải primitive (number, string, boolean,...)
function create(o: object | null) {}
```

### Top Types

- any : kiểu tự do, tắt toàn bộ kiểm tra kiểu của TypeScript (không an toàn)

```typescript
let value: any = "hello";
value.foo.bar(); // Cho phép gọi bất kỳ thứ gì mà không báo lỗi lúc biên dịch (dễ crash lúc chạy)
```

- unknown : kiểu an toàn hơn `any`, đại diện cho giá trị chưa rõ kiểu, bắt buộc phải kiểm tra kiểu (type narrowing) trước khi sử dụng

```typescript
let value: unknown = "hello";
// value.toUpperCase(); // Lỗi: 'value' is of type 'unknown'

if (typeof value === "string") {
  console.log(value.toUpperCase()); // Hợp lệ sau khi đã kiểm tra typeof
}
```

### Bottom Types

- never : đại diện cho giá trị không bao giờ xảy ra (hàm ném lỗi, vòng lặp vô tận, hoặc nhánh logic không thể chạm tới)

```typescript
// 1. Hàm luôn ném lỗi hoặc không bao giờ kết thúc
function throwError(msg: string): never {
  throw new Error(msg);
}

// 2. Dùng để kiểm tra đầy đủ các trường hợp (Exhaustive checking trong switch-case)
type Action = "start" | "stop";
function handleAction(action: Action) {
  switch (action) {
    case "start": break;
    case "stop": break;
    default:
      const _exhaustiveCheck: never = action; // Báo lỗi nếu sót trường hợp mới
  }
}
```

### Type Assertions (`as`)

- as const : khóa giá trị thành literal hằng số và gán `readonly` cho toàn bộ thuộc tính

```typescript
const config = { role: "admin", maxRetries: 3 } as const;
// config.role = "user"; // Lỗi: Cannot assign to read-only property
```

- as [Type] : khẳng định/ép kiểu khi lập trình viên biết rõ kiểu cụ thể hơn TypeScript

```typescript
const input = document.getElementById("username") as HTMLInputElement;
console.log(input.value); // Truy cập an toàn thuộc tính của HTMLInputElement
```

- as any : tắt hoàn toàn kiểm tra kiểu của TypeScript (chỉ dùng khi debug hoặc lib JS cũ)

```typescript
const data: string = "hello";
(data as any).customMethod(); // Bỏ qua type check (dễ crash lúc chạy nếu hàm không tồn tại)
```
