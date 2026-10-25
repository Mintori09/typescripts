# TypeScript Ecosystem

Hệ sinh thái các công cụ, thư viện và bộ khung hỗ trợ phát triển dự án TypeScript hiện đại.

---

## 1. Formatting (Định dạng mã nguồn)

* **Prettier**: Công cụ định dạng code phổ biến nhất hệ sinh thái JS/TS. Tự động căn lề, chấm phẩy, ngắt dòng nhất quán cho cả đội ngũ.
* **Biome**: Công cụ định dạng và linter thế hệ mới viết bằng Rust, tốc độ siêu nhanh (nhanh gấp 20-30 lần Prettier/ESLint), không cần cấu hình phức tạp.

```bash
# Định dạng bằng Biome
npx @biomejs/biome format --write ./src
```

---

## 2. Linting (Kiểm tra chất lượng mã)

* **ESLint** + **typescript-eslint**: Bộ công cụ chuẩn giúp phát hiện lỗi logic, biến không dùng, và thực thi các best-practices viết mã.
  - `@typescript-eslint/parser`: Phân tích cú pháp TypeScript để ESLint đọc hiểu.
  - `@typescript-eslint/eslint-plugin`: Cung cấp các rule chuyên dụng cho TypeScript.

```json
// eslint.config.mjs (Flat Config)
import tseslint from 'typescript-eslint';

export default tseslint.config(
  tseslint.configs.recommended,
);
```

---

## 3. Useful Packages (Thư viện tiện ích nên biết)

### A. `zod` — Schema Validation & Type Inference lúc chạy (Runtime)
TypeScript chỉ kiểm tra kiểu lúc biên dịch. `zod` giúp kiểm tra dữ liệu từ API lúc chạy thực tế và tự sinh ra Type tương ứng:

```typescript
import { z } from "zod";

const UserSchema = z.object({
  id: z.number(),
  name: z.string().min(2),
  email: z.string().email(),
});

// Tự động sinh ra kiểu TypeScript từ schema:
type User = z.infer<typeof UserSchema>;

// Validate dữ liệu API:
const result = UserSchema.safeParse(apiData);
if (result.success) {
  console.log(result.data.name);
}
```

### B. `ts-pattern` — Pattern Matching hoàn hảo
Hỗ trợ cú pháp kiểm tra khớp mẫu theo phong cách Rust / Scala:
```typescript
import { match } from "ts-pattern";

const response = match(state)
  .with({ status: "loading" }, () => "Đang tải...")
  .with({ status: "success", data: match.select() }, (d) => `Thành công: ${d}`)
  .with({ status: "error" }, () => "Gặp lỗi!")
  .exhaustive();
```

### C. `type-fest` — Bộ sưu tập các Type nâng cao
Thư viện cung cấp sẵn hàng trăm utility types phức tạp mà TS chưa có sẵn (ví dụ: `JsonValue`, `SetOptional`, `AsyncReturnType`,...).

---

## 4. Build Tools & Runtimes (Công cụ biên dịch và chạy)

* **`tsc` (TypeScript Compiler)**: Công cụ chính thức của Microsoft, dùng để kiểm tra kiểu (`tsc --noEmit`) và biên dịch ra JavaScript.
* **`tsx`**: Công cụ chạy trực tiếp file `.ts` nhẹ và nhanh nhất cho Node.js (dựa trên `esbuild`), thay thế hoàn toàn cho `ts-node`.
* **`esbuild`**: Trình biên dịch viết bằng Go, tốc độ biên dịch nhanh hơn hàng chục lần so với `tsc`.
* **`swc`**: Trình biên dịch viết bằng Rust (được Next.js và Node.js sử dụng bên dưới).
* **`tsup`**: Công cụ build các thư viện TypeScript ra cả ESM & CommonJS không cần cấu hình, cực nhanh (sử dụng esbuild bên trong).
* **`Vite`**: Bundler hiện đại cho Frontend (React, Vue, Svelte) hỗ trợ TypeScript ngay lập tức không cần cấu hình.
* **`Bun` & `Deno`**: Runtimes thế hệ mới chạy thẳng TypeScript native mà không cần cài đặt thêm bất kỳ công cụ biên dịch nào.
