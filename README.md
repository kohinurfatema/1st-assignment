# Assignment B7A1 — Advanced Problem Solving with TypeScript & OOP

## Overview

This repository contains the solution for the **B7A1 assignment** from the Next Level Web Development course. It demonstrates core TypeScript concepts including data typing, interfaces, generics, classes with inheritance, and type guards.

---

## File Structure

```
├── solutions.ts   # All 7 coding problem solutions
├── blog-1.md      # Blog: any vs unknown and type narrowing
├── blog-2.md      # Blog: Four pillars of OOP in TypeScript
└── README.md      # Project documentation
```

---

## Problem Solutions (`solutions.ts`)

| # | Function / Class | Description |
|---|---|---|
| 1 | `filterEvenNumbers` | Filters even numbers from an array |
| 2 | `reverseString` | Reverses a string |
| 3 | `checkType` | Returns `"String"` or `"Number"` using type guards |
| 4 | `getProperty` | Generic function to get a property value by key |
| 5 | `toggleReadStatus` | Adds `isRead: true` to a `Book` object |
| 6 | `Person` / `Student` | Class inheritance with a `getDetails()` method |
| 7 | `getIntersection` | Returns common elements between two arrays |

---

## Blog Posts

| File | Topic |
|---|---|
| `blog-1.md` | Why `any` is a type safety hole, why `unknown` is safer, and how type narrowing works |
| `blog-2.md` | The four pillars of OOP — Encapsulation, Abstraction, Inheritance, and Polymorphism |

---

## How to Run

1. Install TypeScript globally if not already installed:
   ```bash
   npm install -g typescript
   ```

2. Compile the solution file:
   ```bash
   tsc solutions.ts
   ```

3. Run the compiled output:
   ```bash
   node solutions.js
   ```
