# Classes in TypeScript

TypeScript mang lại hệ thống hướng đối tượng (OOP) đầy đủ cho JavaScript Classes, bổ sung phạm vi truy cập (Access Modifiers), lớp trừu tượng (Abstract Classes) và nạp chồng (Overloading).

---

## 1. Parameter Properties (Constructor Params)

Thay vì khai báo thuộc tính rồi gán thủ công `this.x = x`, bạn có thể thêm modifier (`public`, `private`, `protected`, `readonly`) trực tiếp vào tham số của `constructor`:

```typescript
class Product {
  // TypeScript tự động tạo thuộc tính và gán giá trị tương ứng
  constructor(
    public readonly id: number,
    public name: string,
    private price: number
  ) {}

  getPrice() {
    return this.price;
  }
}

const item = new Product(1, "Bàn phím cơ", 200);
console.log(item.name); // "Bàn phím cơ"
// item.id = 2; // Lỗi: readonly
// console.log(item.price); // Lỗi: private
```

---

## 2. Access Modifiers (Phạm vi truy cập)

* `public` (mặc định): Truy cập được ở bất kỳ đâu.
* `protected`: Chỉ truy cập được bên trong class đó và các class con kế thừa nó.
* `private`: Chỉ truy cập được bên trong class khai báo nó (kiểm tra ở cấp độ TypeScript).
* `#private` (ECMAScript Private Fields): Thuộc tính riêng tư thực sự ở cấp độ JavaScript runtime.

```typescript
class Person {
  public name: string;
  protected age: number;
  private ssn: string; // TS compile-time private
  #bankAccount: string; // Runtime private thực thụ của JS

  constructor(name: string, age: number, ssn: string, account: string) {
    this.name = name;
    this.age = age;
    this.ssn = ssn;
    this.#bankAccount = account;
  }
}

class Employee extends Person {
  getAge() {
    return this.age; // Hợp lệ vì age là protected
  }
  // getSSN() { return this.ssn; } // Lỗi: ssn là private của Person
}
```

---

## 3. Abstract Classes (Lớp trừu tượng)

Lớp cơ sở đóng vai trò làm khuôn mẫu, **không thể khởi tạo trực tiếp bằng `new`**, dùng để cho các lớp con kế thừa và bắt buộc triển khai các phương thức trừu tượng (`abstract`).

```typescript
abstract class Shape {
  constructor(public color: string) {}

  // Phương thức trừu tượng: không có thân hàm, lớp con bắt buộc phải viết code
  abstract getArea(): number;

  // Phương thức thông thường có sẵn code
  display() {
    console.log(`Hình có màu ${this.color} và diện tích: ${this.getArea()}`);
  }
}

class Circle extends Shape {
  constructor(color: string, private radius: number) {
    super(color);
  }

  getArea(): number {
    return Math.PI * this.radius ** 2;
  }
}

// const s = new Shape("red"); // Lỗi: Cannot create an instance of an abstract class.
const c = new Circle("xanh", 5);
c.display();
```

---

## 4. Kế thừa & Tính đa hình (Inheritance vs Polymorphism)

* **Inheritance (Kế thừa)**: Tái sử dụng logic từ class cha bằng `extends` và gọi `super()`.
* **Method Overriding (Ghi đè phương thức)**: Class con có thể viết lại phương thức của cha. Dùng từ khóa `override` (tùy chọn với cờ `noImplicitOverride`) để đảm bảo không ghi đè nhầm tên hàm.
* **Polymorphism (Đa hình)**: Các đối tượng thuộc các class con khác nhau có thể được gọi đồng nhất qua interface/class cha:

```typescript
class Animal {
  makeSound(): void {
    console.log("Some sound...");
  }
}

class Dog extends Animal {
  override makeSound(): void {
    console.log("Gâu gâu!");
  }
}

class Cat extends Animal {
  override makeSound(): void {
    console.log("Meo meo!");
  }
}

// Đa hình: mảng chứa các Animal nhưng mỗi con kêu một cách
const zoo: Animal[] = [new Dog(), new Cat()];
zoo.forEach(animal => animal.makeSound());
```

---

## 5. Constructor Overloading (Quá tải Constructor)

Tương tự như Function Overloading, constructor cũng có thể có nhiều signatures:

```typescript
class Point {
  x: number;
  y: number;

  // Overload signatures
  constructor(x: number, y: number);
  constructor(xy: number);

  // Implementation
  constructor(xOrXy: number, y?: number) {
    if (y === undefined) {
      this.x = xOrXy;
      this.y = xOrXy;
    } else {
      this.x = xOrXy;
      this.y = y;
    }
  }
}

const p1 = new Point(10, 20); // x: 10, y: 20
const p2 = new Point(5);      // x: 5, y: 5
```
