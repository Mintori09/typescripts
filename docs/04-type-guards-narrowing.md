# Type Guards & Narrowing

**Narrowing** (thu hẹp kiểu) là quá trình TypeScript dựa vào các điều kiện logic lúc chạy (runtime checks) để thu hẹp một kiểu dữ liệu rộng (như `string | number` hoặc `unknown`) thành một kiểu cụ thể hơn.

---

## 1. `typeof` Guard

Dùng để phân biệt các kiểu nguyên thủy (primitive types): `"string"`, `"number"`, `"boolean"`, `"symbol"`, `"undefined"`, `"object"`, `"function"`.

```typescript
function padLeft(padding: number | string, input: string): string {
  if (typeof padding === "number") {
    // Trong khối này, TS biết chắc chắn padding là number
    return " ".repeat(padding) + input;
  }
  // Ở đây, TS biết chắc chắn padding chỉ còn lại string
  return padding + input;
}
```

---

## 2. `instanceof` Guard

Dùng để kiểm tra một đối tượng có phải là thể hiện (instance) của một Class hoặc Constructor function hay không.

```typescript
function logValue(x: Date | string) {
  if (x instanceof Date) {
    // Trong khối này, x là Date
    console.log(x.toUTCString());
  } else {
    // Ở đây, x là string
    console.log(x.toUpperCase());
  }
}
```

---

## 3. Equality & Truthiness Narrowing

### Equality Narrowing (`===`, `!==`)
Khi so sánh bằng giữa các biến có kiểu khác nhau, TypeScript loại trừ các nhánh không thể xảy ra:

```typescript
function check(x: string | number, y: string | boolean) {
  if (x === y) {
    // x và y bằng nhau, mà giao của hai tập chỉ có 'string' -> cả hai đều là string
    console.log(x.toUpperCase(), y.toLowerCase());
  }
}
```

### Truthiness Narrowing (`if (value)`)
Loại bỏ các giá trị *falsy* (`false`, `0`, `""`, `null`, `undefined`, `NaN`):

```typescript
function printUsers(users?: string[] | null) {
  if (users) {
    // Trong này users chắc chắn là string[], không còn undefined hay null
    console.log(`Có ${users.length} người dùng.`);
  }
}
```

---

## 4. `in` Operator Guard

Kiểm tra sự tồn tại của một tên thuộc tính trong một đối tượng để phân biệt các kiểu Object hoặc Discriminated Unions:

```typescript
type Fish = { swim: () => void };
type Bird = { fly: () => void };

function move(animal: Fish | Bird) {
  if ("swim" in animal) {
    // TS nhận biết animal là Fish
    animal.swim();
  } else {
    // TS nhận biết animal là Bird
    animal.fly();
  }
}
```

---

## 5. Type Predicates (`parameterName is Type`)

Tự viết hàm kiểm tra kiểu (custom type guard). Giá trị trả về có dạng `param is Type` giúp TypeScript tin tưởng kết quả trả về của hàm:

```typescript
interface Cat {
  name: string;
  meow: () => void;
}

interface Dog {
  name: string;
  bark: () => void;
}

// Hàm kiểm tra: nếu trả về true thì pet chính là Cat
function isCat(pet: Cat | Dog): pet is Cat {
  return (pet as Cat).meow !== undefined;
}

function handlePet(pet: Cat | Dog) {
  if (isCat(pet)) {
    pet.meow(); // TS hiểu pet là Cat
  } else {
    pet.bark(); // TS hiểu pet là Dog
  }
}
```
