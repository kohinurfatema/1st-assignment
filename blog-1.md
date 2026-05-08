# Why `unknown` is Safer Than `any` in TypeScript

## Introduction

TypeScript's entire value proposition is type safety — catching bugs at compile time before they reach production. But two types, `any` and `unknown`, both represent "I don't know what this is." The difference between them determines whether TypeScript protects you or steps aside entirely. Understanding this difference, and the concept of **type narrowing**, is essential for writing robust TypeScript code.

---

## The Problem with `any`

When you annotate a value as `any`, you tell TypeScript to stop checking that value completely. You can call methods on it, pass it anywhere, index it — and TypeScript will never complain.

```ts
function processInput(input: any) {
  console.log(input.toUpperCase()); // No error at compile time
  console.log(input.nonExistentMethod()); // Still no error!
}

processInput(42); // Runtime crash: input.toUpperCase is not a function
```

This is the **type safety hole**. `any` disables the type checker for that value. You lose all the benefits TypeScript provides, and errors only surface at runtime — potentially in production.

---

## The Safer Alternative: `unknown`

`unknown` is the type-safe counterpart to `any`. Like `any`, it can hold any value. Unlike `any`, TypeScript **forces you to check the type before using it**.

```ts
function processInput(input: unknown) {
  console.log(input.toUpperCase()); // Compile error: Object is of type 'unknown'
}
```

TypeScript refuses to let you use an `unknown` value until you prove what it actually is. That proof is called **type narrowing**.

---

## Type Narrowing

Type narrowing is the process of refining a broad type (`unknown`, `string | number`, etc.) into a specific type using runtime checks. TypeScript reads these checks and narrows the type inside each branch.

### Using `typeof`

```ts
function processInput(input: unknown): string {
  if (typeof input === "string") {
    return input.toUpperCase(); // TypeScript knows: input is string here
  }
  if (typeof input === "number") {
    return input.toFixed(2); // TypeScript knows: input is number here
  }
  return "Unsupported type";
}
```

### Using `instanceof`

```ts
function handleError(error: unknown): string {
  if (error instanceof Error) {
    return error.message; // Safe: error is narrowed to Error
  }
  return "An unexpected error occurred";
}
```

### Using a Type Guard Function

For complex types, you can write a custom type guard:

```ts
interface User {
  id: number;
  name: string;
}

function isUser(value: unknown): value is User {
  return (
    typeof value === "object" &&
    value !== null &&
    "id" in value &&
    "name" in value
  );
}

function greetUser(value: unknown): string {
  if (isUser(value)) {
    return `Hello, ${value.name}`; // Safe: value is narrowed to User
  }
  return "Hello, stranger";
}
```

---

## `any` vs `unknown` — Side by Side

| Feature | `any` | `unknown` |
|---|---|---|
| Accepts any value | Yes | Yes |
| Requires type check before use | No | Yes |
| Disables type checking | Yes | No |
| Safe for unpredictable data | No | Yes |
| Recommended for production code | No | Yes |

---

## When to Use Each

- Use **`unknown`** whenever you receive data from an external source — API responses, user input, `JSON.parse()`, error objects in `catch` blocks.
- Use **`any`** only as a last resort when migrating a large JavaScript codebase to TypeScript and you need to move fast temporarily. Always plan to replace it.

---

## Conclusion

`any` is a trapdoor that silences TypeScript's compiler, turning your typed code into untyped JavaScript. `unknown` keeps the safety net intact by demanding that you narrow the type before using the value. Combined with type narrowing techniques like `typeof`, `instanceof`, and custom type guards, `unknown` lets you handle unpredictable data safely and confidently. Prefer `unknown` — your future self will thank you.
