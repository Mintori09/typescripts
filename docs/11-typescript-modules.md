# TypeScript Modules

TypeScript hỗ trợ hai mô hình phân chia mã: **Namespaces** (mô hình cũ thời TS sơ khai) và **ES Modules** (mô hình chuẩn hiện đại của JavaScript).

---

## 1. External Modules (ES Modules - Chuẩn Hiện Đại)

Bất kỳ file nào có chứa `import` hoặc `export` ở cấp cao nhất (top-level) đều được coi là một Module riêng biệt có phạm vi độc lập (không làm ô nhiễm global scope).

```typescript
// math.ts
export const PI = 3.14159;
export function add(a: number, b: number): number {
  return a + b;
}

// app.ts
import { PI, add } from "./math";
console.log(add(10, PI));
```

### Type-Only Imports & Exports (`import type`)
Giúp trình đóng gói (bundler) biết chắc chắn đây chỉ là định nghĩa kiểu và loại bỏ sạch sẽ khi build ra JavaScript:
```typescript
import type { UserProfile } from "./types";
export type { UserProfile };
```

---

## 2. Namespaces (Cũ / Legacy)

Namespaces (trước đây gọi là "Internal Modules") gom nhóm mã vào một biến toàn cục để tránh trùng tên:

```typescript
namespace Validation {
  export interface StringValidator {
    isValid(s: string): boolean;
  }

  export class ZipCodeValidator implements StringValidator {
    isValid(s: string) {
      return s.length === 5;
    }
  }
}

// Sử dụng:
const validator = new Validation.ZipCodeValidator();
```
> **Khuyên dùng:** Trong các dự án hiện đại, hãy luôn dùng **ES Modules (`import`/`export`)**. Chỉ nên dùng `namespace` khi viết file khai báo `.d.ts` cho các thư viện cũ chạy bằng thẻ `<script>`.

---

## 3. Ambient Modules (`declare module`)

Dùng trong các file định nghĩa `.d.ts` để khai báo cấu trúc cho một module không có sẵn kiểu (ví dụ thư viện JS thuần hoặc file CSS/hình ảnh):

```typescript
// globals.d.ts

// 1. Khai báo kiểu cho file ảnh png
declare module "*.png" {
  const src: string;
  export default src;
}

// 2. Khai báo kiểu cho một thư viện bên ngoài chưa có @types/
declare module "untyped-legacy-library" {
  export function doSomething(options: any): void;
}
```

---

## 4. Module Augmentation & Namespace Augmentation

Mở rộng thuộc tính hoặc hàm của một module/namespace đã có mà không cần sửa trực tiếp mã nguồn gốc:

```typescript
// Mở rộng module 'express' để gắn thêm thông tin user vào Request
import "express";

declare module "express" {
  export interface Request {
    currentUser?: {
      id: string;
      role: string;
    };
  }
}

// Sau đó trong controller bạn có thể truy cập req.currentUser mà không bị báo lỗi!
```

---

## 5. Global Augmentation

Mở rộng các đối tượng toàn cục có sẵn của môi trường (như `window`, `process.env`, `document`):

```typescript
// global.d.ts
declare global {
  interface Window {
    analyticsTracker: {
      trackEvent: (eventName: string) => void;
    };
  }

  namespace NodeJS {
    interface ProcessEnv {
      DATABASE_URL: string;
      PORT: string;
    }
  }
}

// Sử dụng mà không cần ép kiểu `as any`:
window.analyticsTracker.trackEvent("PAGE_VIEW");
console.log(process.env.DATABASE_URL);

export {}; // Đảm bảo file được nhận diện là một module
```
