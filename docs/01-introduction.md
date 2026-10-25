## Typescripts và Javascript

- TypeScript và JavaScript đều là ngôn ngữ lập trình web. Trong khi JavaScript dùng kiểu động và bắt lỗi lúc runtime, TypeScript là tập mẹ của JavaScript bổ sung kiểu tĩnh tùy chọn, giúp phát hiện lỗi ngay khi viết code để tăng độ tin cậy và dễ bảo trì. Trước khi chạy trên trình duyệt, TypeScript phải được biên dịch sang JavaScript.

### Thiết lập Typescript.

- config trong tsocnfig.json quản lý các compiler optiosn và project settings.
- Example tsconfig.json:

```json
{
  "compilerOptions": {
    "module": "commonjs",
    "noImplicitAny": true,
    "removeComments": true,
    "preserveConstEnums": true,
    "sourceMap": true
  },
  "files": [
    "core.ts",
    "sys.ts",
    "types.ts",
    "scanner.ts",
    "parser.ts",
    "utilities.ts",
    "binder.ts",
    "checker.ts",
    "emitter.ts",
    "program.ts",
    "commandLineParser.ts",
    "tsc.ts",
    "diagnosticInformationMap.generated.ts"
  ],
  "compilerOptions": {
    "module": "system",
    "noImplicitAny": true,
    "removeComments": true,
    "preserveConstEnums": true,
    "outFile": "../../built/local/tsc.js",
    "sourceMap": true
  },
  "include": ["src/**/*"],
  "exclude": ["**/*.spec.ts"]
}
```

### Compiler options

- `tsc` tldr:

```bash
# Run a compile based on a backwards look through the fs for a tsconfig.json
tsc
# Emit JS for just the index.ts with the compiler defaults
tsc index.ts
# Emit JS for any .ts files in the folder src, with the default settings
tsc src/*.ts
# Emit files referenced in with the compiler settings from tsconfig.production.json
tsc --project tsconfig.production.json
# Emit d.ts files for a js file with showing compiler options which are booleans
tsc index.js --declaration --emitDeclarationOnly
# Emit a single .js file from two files via compiler options which take string arguments
tsc app.ts util.ts --target esnext --outfile index.js
```

- `ts-node` tldr:

```bash
# Execute a TypeScript file without compiling (Node + tsc):
ts-node path/to/file.ts
# Execute a TypeScript file without loading tsconfig.json:
ts-node --skipProject path/to/file.ts
# Evaluate TypeScript code passed as a literal:
ts-node --eval 'console.log("Hello World")'
# Execute a TypeScript file in script mode:
ts-node --script-mode path/to/file.ts
# Transpile a TypeScript file to JavaScript without executing it:
ts-node --transpileOnly path/to/file.ts
```
