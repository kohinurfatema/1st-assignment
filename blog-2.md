# The Four Pillars of OOP in TypeScript

## Introduction

Object-Oriented Programming (OOP) is a paradigm built around four core principles: **Encapsulation**, **Abstraction**, **Inheritance**, and **Polymorphism**. In large-scale TypeScript projects, these pillars are not just theoretical concepts — they are practical tools that reduce complexity, prevent bugs, and make codebases easier to maintain and extend. This post breaks down each pillar with real TypeScript examples.

---

## 1. Encapsulation

Encapsulation means bundling data and the methods that operate on it inside a class, and **restricting direct access** to internal state using access modifiers (`private`, `protected`, `public`).

Without encapsulation, any part of the codebase can modify an object's internal state, making bugs hard to trace.

```ts
class BankAccount {
  private balance: number;

  constructor(initialBalance: number) {
    this.balance = initialBalance;
  }

  deposit(amount: number): void {
    if (amount > 0) this.balance += amount;
  }

  withdraw(amount: number): void {
    if (amount <= this.balance) this.balance -= amount;
  }

  getBalance(): number {
    return this.balance;
  }
}

const account = new BankAccount(1000);
account.deposit(500);
account.getBalance(); // 1500
// account.balance = -9999; // Compile error: 'balance' is private
```

By making `balance` private, we guarantee it can only change through validated methods. The logic stays in one place and bugs are contained.

---

## 2. Abstraction

Abstraction means **hiding implementation details** and exposing only what is necessary. In TypeScript, this is achieved using `abstract` classes and interfaces.

```ts
abstract class Shape {
  abstract getArea(): number;

  describe(): string {
    return `This shape has an area of ${this.getArea()}`;
  }
}

class Circle extends Shape {
  constructor(private radius: number) {
    super();
  }

  getArea(): number {
    return Math.PI * this.radius ** 2;
  }
}

class Rectangle extends Shape {
  constructor(private width: number, private height: number) {
    super();
  }

  getArea(): number {
    return this.width * this.height;
  }
}

const circle = new Circle(5);
circle.describe(); // "This shape has an area of 78.53..."
```

Consumers of `Shape` only need to know about `getArea()` and `describe()`. The internal calculation is irrelevant to them. This reduces cognitive load and decouples the "what" from the "how."

---

## 3. Inheritance

Inheritance allows a class to **reuse and extend** the behavior of another class, avoiding duplication and establishing a clear hierarchy.

```ts
class Person {
  constructor(public name: string, public age: number) {}

  greet(): string {
    return `Hi, I am ${this.name}`;
  }
}

class Student extends Person {
  constructor(name: string, age: number, public grade: string) {
    super(name, age);
  }

  getDetails(): string {
    return `Name: ${this.name}, Age: ${this.age}, Grade: ${this.grade}`;
  }
}

class Teacher extends Person {
  constructor(name: string, age: number, public subject: string) {
    super(name, age);
  }

  getDetails(): string {
    return `Name: ${this.name}, Age: ${this.age}, Subject: ${this.subject}`;
  }
}

const student = new Student("Alice", 20, "A");
student.greet();      // "Hi, I am Alice" — inherited from Person
student.getDetails(); // "Name: Alice, Age: 20, Grade: A"
```

`Student` and `Teacher` both inherit `name`, `age`, and `greet()` from `Person`. Shared logic lives in one place — change `greet()` once and both classes benefit.

---

## 4. Polymorphism

Polymorphism means **one interface, many forms**. A parent type reference can point to any subclass, and the correct method is called at runtime.

```ts
class Animal {
  makeSound(): string {
    return "Some generic sound";
  }
}

class Dog extends Animal {
  makeSound(): string {
    return "Woof!";
  }
}

class Cat extends Animal {
  makeSound(): string {
    return "Meow!";
  }
}

function makeAnimalsSpeak(animals: Animal[]): string[] {
  return animals.map((animal) => animal.makeSound());
}

const animals: Animal[] = [new Dog(), new Cat(), new Dog()];
makeAnimalsSpeak(animals); // ["Woof!", "Meow!", "Woof!"]
```

`makeAnimalsSpeak` does not need to know whether it is dealing with a `Dog` or a `Cat`. It only knows about `Animal`. Adding a new animal type (`Bird`, `Snake`) requires zero changes to `makeAnimalsSpeak` — it just works.

---

## How the Four Pillars Work Together

| Pillar | What it does | Benefit in large projects |
|---|---|---|
| Encapsulation | Hides internal state | Prevents unintended mutations |
| Abstraction | Hides implementation details | Reduces complexity for consumers |
| Inheritance | Reuses and extends behavior | Eliminates code duplication |
| Polymorphism | One interface, many forms | Makes code open for extension, closed for modification |

---

## Conclusion

The four pillars of OOP are not independent rules — they work together as a system. Encapsulation protects state, abstraction simplifies interfaces, inheritance shares logic, and polymorphism enables flexibility. In large-scale TypeScript projects where dozens of developers work on the same codebase, these principles are what keep logic manageable, modules decoupled, and features easy to add without breaking existing behavior.
