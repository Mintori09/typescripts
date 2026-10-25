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

- Non-null Assertion Operator (`!`) : cam đoan với trình biên dịch rằng giá trị chắc chắn không phải `null` hoặc `undefined`

```typescript
function processName(name: string | null) {
  const cleanName: string = name!; // Bỏ qua cảnh báo null/undefined
  console.log(cleanName.toUpperCase());
}
```

- `satisfies` Operator : kiểm tra biến khớp với kiểu/interface mong muốn nhưng KHÔNG làm mất đi (widen) kiểu dữ liệu literal cụ thể

```typescript
type Colors = "red" | "green" | "blue";
type RGB = [red: number, green: number, blue: number];

const palette = {
  red: [255, 0, 0],
  green: "#00ff00",
  blue: [0, 0, 255],
} satisfies Record<Colors, string | RGB>;

// Giữ nguyên kiểu cụ thể: palette.green biết chính xác là string, gọi được hàm string:
console.log(palette.green.toUpperCase()); 
// palette.red biết chính xác là mảng RGB, gọi được hàm mảng:
palette.red.map(x => x * 2);
```

---

### Type Inference & Type Compatibility

- Type Inference : TypeScript tự động suy luận kiểu dữ liệu dựa trên giá trị khởi tạo hoặc giá trị trả về của hàm

```typescript
let age = 25; // Inferred là number
let message = "hello"; // Inferred là string
function add(a: number, b: number) {
  return a + b; // Inferred kiểu trả về là number
}
```

- Type Compatibility (Structural Typing) : TypeScript kiểm tra sự tương thích dựa trên cấu trúc (hình dạng - shape) của đối tượng chứ không dựa trên tên kiểu dữ liệu (nếu nó đi như vịt, kêu như vịt thì nó là vịt)

```typescript
interface Point2D {
  x: number;
  y: number;
}

interface Point3D {
  x: number;
  y: number;
  z: number;
}

let p2: Point2D;
let p3: Point3D = { x: 10, y: 20, z: 30 };

// Hợp lệ: p3 có đủ các thuộc tính mà Point2D yêu cầu (x và y)
p2 = p3; 

// Không hợp lệ: p2 thiếu thuộc tính 'z' mà Point3D yêu cầu
// p3 = p2; // Error: Property 'z' is missing in type 'Point2D'
```
