# TypeScript Functions

Cách định nghĩa kiểu cho hàm, tham số, giá trị trả về và kỹ thuật quá tải hàm (Function Overloading).

---

## 1. Typing Functions

### Khai báo kiểu cơ bản:
```typescript
// Named function: tham số có kiểu, giá trị trả về có kiểu
function add(x: number, y: number): number {
  return x + y;
}

// Arrow function:
const multiply = (a: number, b: number): number => a * b;

// Định nghĩa function type độc lập:
type MathOperation = (x: number, y: number) => number;
const divide: MathOperation = (a, b) => a / b;
```

### Tham số tùy chọn, mặc định & Rest:
```typescript
// y?: number -> tham số tùy chọn (undefined nếu không truyền)
// z: number = 0 -> tham số có giá trị mặc định
function calculate(x: number, y?: number, z: number = 0): number {
  return x + (y ?? 0) + z;
}

// Rest parameters (...args):
function sumAll(prefix: string, ...numbers: number[]): string {
  const total = numbers.reduce((acc, curr) => acc + curr, 0);
  return `${prefix}: ${total}`;
}
```

---

## 2. Function Overloading (Quá tải hàm)

Cho phép một hàm có thể được gọi với các bộ tham số và kiểu trả về khác nhau.

**Cấu trúc gồm 2 phần:**
1. **Overload Signatures**: Các dòng mô tả các cách gọi hợp lệ (không có thân hàm).
2. **Implementation Signature**: Một thân hàm duy nhất có thể xử lý tổng quát tất cả các trường hợp trên.

```typescript
// 1. Overload signatures:
function makeDate(timestamp: number): Date;
function makeDate(m: number, d: number, y: number): Date;

// 2. Implementation signature:
function makeDate(mOrTimestamp: number, d?: number, y?: number): Date {
  if (d !== undefined && y !== undefined) {
    return new Date(y, mOrTimestamp, d);
  } else {
    return new Date(mOrTimestamp);
  }
}

// Gọi hàm:
const d1 = makeDate(1672531199000);   // Khớp overload 1 tham số
const d2 = makeDate(11, 20, 2026);    // Khớp overload 3 tham số
// const d3 = makeDate(11, 20);       // Báo lỗi: Không có overload nào nhận 2 tham số!
```

---

## 3. Khai báo kiểu `this` trong hàm

TypeScript cho phép chỉ định rõ kiểu dữ liệu mà từ khóa `this` sẽ trỏ tới bên trong hàm (đặt `this` làm tham số giả đầu tiên):

```typescript
interface DBConnection {
  runQuery(sql: string): void;
}

function execute(this: DBConnection, query: string) {
  this.runQuery(query); // TypeScript đảm bảo `this` là DBConnection
}

const db: DBConnection = {
  runQuery: (sql) => console.log(`Executing: ${sql}`),
};

// Gọi bằng call/apply:
execute.call(db, "SELECT * FROM users");
```
