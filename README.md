# TypeScript Learning Roadmap & Notes

---

## Mục lục tài liệu (`docs/`)

| # | Tài liệu | Nội dung chính |
| :-: | :--- | :--- |
| **01** | [Giới thiệu & Thiết lập](docs/01-introduction.md) | TypeScript vs JavaScript, cấu hình `tsconfig.json`, các tùy chọn `tsc`. |
| **02** | [Hệ thống kiểu dữ liệu](docs/02-typescript-types.md) | Primitive, Object types, Top/Bottom types (`unknown`, `any`, `never`), Type Assertions (`as const`, `as [Type]`, `as any`, `!`, `satisfies`), Type Inference & Structural Typing. |
| **03** | [Kết hợp kiểu dữ liệu](docs/03-combining-types.md) | Union Types (`\|`), Intersection Types (`&`), Type Aliases (`type`), và toán tử `keyof`. |
| **04** | [Thu hẹp kiểu dữ liệu](docs/04-type-guards-narrowing.md) | `typeof`, `instanceof`, Equality/Truthiness narrowing, toán tử `in`, và Custom Type Predicates (`is`). |
| **05** | [TypeScript Interfaces](docs/05-interfaces.md) | Khai báo Interface, kế thừa (`extends`), so sánh `Type vs Interface`, Declaration Merging, và Hybrid Types. |
| **06** | [TypeScript Functions](docs/06-functions.md) | Gán kiểu cho hàm, tham số tùy chọn/mặc định/rest, Function Overloading, và kiểu của `this`. |
| **07** | [Lập trình hướng đối tượng (Classes)](docs/07-classes.md) | Parameter Properties, Access Modifiers (`public`, `protected`, `private`, `#private`), Abstract Classes, kế thừa & đa hình, Method Overriding, Constructor Overloading. |
| **08** | [Generics](docs/08-generics.md) | Generic Functions, Interfaces, Classes; Generic Constraints (`extends`, `keyof`), và Generic Defaults. |
| **09** | [Utility Types](docs/09-utility-types.md) | 12 Utility Types phổ biến: `Partial`, `Required`, `Readonly`, `Pick`, `Omit`, `Record`, `Exclude`, `Extract`, `NonNullable`, `Parameters`, `ReturnType`, `Awaited`, `InstanceType`. |
| **10** | [Kiểu nâng cao & Decorators](docs/10-advanced-types.md) | Literal Types, Template Literal Types, Mapped Types, Conditional Types (`infer`), Recursive Types (JSON/Tree), và Decorators. |
| **11** | [TypeScript Modules](docs/11-typescript-modules.md) | ES Modules (`import type`), Namespaces (legacy), Ambient Modules (`declare module`), Module/Namespace Augmentation, Global Augmentation. |
| **12** | [Hệ sinh thái TypeScript](docs/12-ecosystem.md) | Formatting (Prettier, Biome), Linting (ESLint), Thư viện hữu ích (`zod`, `ts-pattern`, `type-fest`), và Build Tools/Runtimes (`tsc`, `tsx`, `esbuild`, `swc`, `Vite`, `Bun`, `Deno`). |

---

## Cách chạy thử mã nguồn TypeScript

Hiện nay, bạn có thể chạy thử các đoạn code trong tài liệu bằng một số công cụ hiện đại và nhanh chóng mà không cần cài đặt phức tạp:

### 1. Dùng `tsx` (Khuyên dùng cho Node.js)

```bash
# Chạy trực tiếp một file .ts bất kỳ
npx tsx path/to/file.ts

# Chế độ theo dõi thay đổi (watch mode)
npx tsx watch path/to/file.ts
```

### 2. Dùng Node.js native (Node.js v22.6+)

```bash
node --experimental-strip-types path/to/file.ts
```

### 3. Dùng Bun hoặc Deno

```bash
bun run path/to/file.ts
# hoặc
deno run path/to/file.ts
```

---

## Nguyên tắc ghi nhớ nhanh

1. **TypeScript chỉ tồn tại lúc phát triển (Compile-time):** Sau khi build ra JavaScript, toàn bộ các định nghĩa kiểu (`type`, `interface`, type annotations) đều bị xóa bỏ hoàn toàn.
2. **Ưu tiên an toàn:**
   - Dùng `unknown` thay vì `any`.
   - Dùng `satisfies` hoặc `as const` thay vì ép kiểu mù quáng bằng `as any`.
   - Dùng `zod` để kiểm tra dữ liệu runtime từ API.
