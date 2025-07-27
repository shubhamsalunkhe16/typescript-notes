## What is TypeScript?

- `strongly typed`, `object-oriented`, `compiled language` that builds on JavaScript by adding static type definitions.

### Why Use TypeScript?

- `Static Typing`: Reduces runtime errors by catching them during development.
- `Improved Tooling`: Better support for IDEs and tools like IntelliSense.
- `Scalability`: Helps manage large-scale applications with ease.
- `Interoperability`: Works seamlessly with JavaScript.

### Installing TypeScript

```bash
npm install -g typescript
```

### Compiling TypeScript

```bash
tsc filename.ts
```

### TypeScript Config File

Use `tsc --init` to create a `tsconfig.json` file for project-level configuration.

```json
{
  "compilerOptions": {
    "target": "ES6",
    "module": "CommonJS",
    "strict": true,
    "outDir": "./dist",
    "rootDir": "./src"
  }
}
```

---

## Basic Concepts

### 2.1 Types

| **Type Name**      | **Example**                                               | **Description**                                                        | **Notes**                                                         |
| ------------------ | --------------------------------------------------------- | ---------------------------------------------------------------------- | ----------------------------------------------------------------- |
| **`number`**       | `let age: number = 25;`                                   | Represents numeric values (integers, floats).                          | Includes `NaN`, `Infinity`.                                       |
| **`string`**       | `let name: string = "Shubham";`                           | Represents text.                                                       | Use **template literals** for dynamic strings.                    |
| **`boolean`**      | `let isActive: boolean = true;`                           | Represents true/false.                                                 | Useful for flags & conditions.                                    |
| **`null`**         | `let value: null = null;`                                 | Explicitly empty value.                                                | Often used with `union` (`string \| null`).                       |
| **`undefined`**    | `let value: undefined;`                                   | Variable declared but not assigned.                                    | Default for uninitialized variables.                              |
| **`any`**          | `let data: any = "anything";`                             | Disables type checking.                                                | **Avoid** (breaks TypeScript safety).                             |
| **`unknown`**      | `let input: unknown = 10;`                                | Similar to `any` but **must be type-checked** before use.              | Safer than `any`.                                                 |
| **`void`**         | `function log(): void { console.log("Hello"); }`          | Used for functions that don’t return a value.                          | Cannot assign to variables.                                       |
| **`never`**        | `function throwErr(): never { throw new Error("Fail"); }` | Represents values that **never occur** (e.g., errors, infinite loops). | Rarely used, useful for exhaustive checks.                        |
| **`object`**       | `let user: object = { id: 1 };`                           | Represents non-primitive values.                                       | For **structured types**, use **interfaces** or **type aliases**. |
| **`array`**        | `let nums: number[] = [1, 2, 3];`                         | Represents lists of elements.                                          | Also `Array<number>`.                                             |
| **`tuple`**        | `let user: [string, number] = ["Tom", 25];`               | Fixed-length arrays with defined types per index.                      | Great for pairs & structured data.                                |
| **`enum`**         | `enum Role { Admin, User }`                               | Defines a set of named constants.                                      | Use **const enums** for performance.                              |
| **`union`**        | `let id: string \| number;`                               | Variable can hold **multiple types**.                                  | Use with **type narrowing**.                                      |
| **`intersection`** | `type AdminUser = User & Admin;`                          | Combines multiple types into one.                                      | Useful for extending interfaces.                                  |
| **`literal`**      | `let status: "success" \| "error";`                       | Variable restricted to **specific values**.                            | Great for **state management**.                                   |
| **`function`**     | `let add: (a: number, b: number) => number;`              | Describes a function’s parameter & return types.                       | Helps with **callback typing**.                                   |
| **`type alias`**   | `type ID = string \| number;`                             | Creates a **custom type name**.                                        | Useful for complex types.                                         |
| **`interface`**    | `interface User { id: number; name: string; }`            | Defines the **structure of an object**.                                | Preferred for **objects & contracts**.                            |
| **`class`**        | `class Person { name: string; }`                          | Defines **blueprints for objects**.                                    | Can implement **interfaces**.                                     |

```typescript
// -------- Primitive Types --------
let age: number = 25; // Number
let name: string = "Shubham"; // String
let isActive: boolean = true; // Boolean
let nothing: null = null; // Null
let notAssigned: undefined = undefined; // Undefined

// -------- Any & Unknown --------
let data: any = "anything"; // Any type (avoid if possible)
let input: unknown = 42; // Unknown (needs type check)

// -------- Void & Never --------
function log(): void {
  console.log("Hello");
} // No return value
function throwError(): never {
  throw new Error("Fail");
} // Never returns

// -------- Object --------
let user: object = { id: 1, name: "Shubham" }; // Generic object

// -------- Array & Tuple --------
let nums: number[] = [1, 2, 3]; // Array of numbers
let pair: [string, number] = ["Tom", 25]; // Tuple (fixed order & types)

// -------- Enum --------
enum Role {
  Admin,
  User,
}
let role: Role = Role.Admin; // Enum usage

// -------- Union & Intersection --------
type ID = string | number; // Union (either string or number)
type Name = { name: string };
type Age = { age: number };
type Person = Name & Age; // Intersection (must have both)

// -------- Literal Types --------
let status: "success" | "error" = "success"; // Only these values allowed

// -------- Functions --------
let add: (a: number, b: number) => number = (a, b) => a + b; // Function type

// -------- Type Alias --------
type UserID = string | number; // Custom type alias

// -------- Interface --------
interface User {
  id: number;
  name: string;
} // Object structure

// -------- Class --------
class PersonClass {
  constructor(public name: string) {}
}

// -------- Generics --------
function identity<T>(value: T): T {
  return value;
}
let result = identity<number>(10); // Generic function

// -------- Utility Types --------
interface FullUser {
  id: number;
  name?: string;
}
type ReadonlyUser = Readonly<FullUser>; // All properties readonly
type PartialUser = Partial<FullUser>; // All properties optional
type UserPreview = Pick<FullUser, "id">; // Only selected keys

// -------- Type Assertions --------
let value: unknown = "Hello";
let strLength = (value as string).length; // Type assertion
```

---

### 2.2 Functions

| **Feature**              | **Example**                                                                                                                                               | **Description**                                             | **Notes**                                         |
| ------------------------ | --------------------------------------------------------------------------------------------------------------------------------------------------------- | ----------------------------------------------------------- | ------------------------------------------------- |
| **Function Declaration** | `function add(a: number, b: number): number { return a + b; }`                                                                                            | Standard function with **parameter & return types**.        | Explicit typing improves readability.             |
| **Function Expression**  | `const multiply: (x: number, y: number) => number = (x, y) => x * y;`                                                                                     | Assigns a function to a variable with a **type**.           | Useful for callbacks & higher-order functions.    |
| **Optional Parameters**  | `function greet(name: string, age?: number) {}`                                                                                                           | Parameters marked with `?` are **optional**.                | Must come **after required parameters**.          |
| **Default Parameters**   | `function greet(name: string, age: number = 25) {}`                                                                                                       | Provides a **default value** if none is passed.             | Reduces need for extra checks.                    |
| **Rest Parameters**      | `function sum(...nums: number[]): number { return nums.reduce((a,b)=>a+b); }`                                                                             | Collects **multiple arguments** into an array.              | Always comes **last in the parameter list**.      |
| **Function Overloading** | `function combine(a: number, b: number): number; function combine(a: string, b: string): string; function combine(a: any, b: any): any { return a + b; }` | Multiple **signatures for different input types**.          | Helps handle **different argument combinations**. |
| **Void Return Type**     | `function logMessage(msg: string): void { console.log(msg); }`                                                                                            | Indicates the function **doesn’t return a value**.          | Useful for logging & side-effect functions.       |
| **Never Return Type**    | `function throwError(msg: string): never { throw new Error(msg); }`                                                                                       | Function that **never completes** (errors, infinite loops). | Helps with **exhaustive type checks**.            |
| **Function Type Alias**  | `type MathOp = (a: number, b: number) => number; const subtract: MathOp = (a, b) => a - b;`                                                               | Defines a **custom type** for function shape.               | Makes function signatures **reusable**.           |
| **Function Interface**   | `interface Printer { (msg: string): void; } const print: Printer = (msg) => console.log(msg);`                                                            | Declares a **function type contract** using interfaces.     | Useful for complex function contracts.            |
| **Arrow Functions**      | `const double = (n: number): number => n * 2;`                                                                                                            | Short syntax for anonymous functions.                       | Preserves `this` from surrounding context.        |
| **`this` Parameter**     | `interface User { name: string; greet(this: User): void; }`                                                                                               | Explicitly defines the type of `this` in a function.        | Helps prevent wrong context usage.                |
| **Generic Functions**    | `function identity<T>(value: T): T { return value; }`                                                                                                     | Makes functions **reusable for any type**.                  | Ideal for **type-safe utilities**.                |
| **Async Functions**      | `async function fetchData(): Promise<string> { return "Data loaded"; }`                                                                                   | Declares **asynchronous functions** returning a Promise.    | Always return `Promise<T>` automatically.         |

```ts
// -------- Basic Function --------
function greet(name: string): string {
  return `Hello, ${name}`; // Returns a string
}

// -------- Anonymous Function --------
const sum = function (a: number, b: number): number {
  return a + b;
};

// -------- Arrow Function --------
const multiply = (a: number, b: number): number => a * b;

// -------- Optional Parameters --------
function printMessage(message: string, prefix?: string): string {
  return `${prefix ?? "Note"}: ${message}`;
}

// -------- Default Parameters --------
function power(base: number, exponent: number = 2): number {
  return base ** exponent; // Default exponent = 2
}

// -------- Rest Parameters --------
function addAll(...numbers: number[]): number {
  return numbers.reduce((acc, n) => acc + n, 0);
}

// -------- Function Overloading --------
function format(value: string): string;
function format(value: number): string;
function format(value: string | number): string {
  return `Formatted: ${value}`;
}

// -------- Function as Type --------
type MathOperation = (a: number, b: number) => number;
const subtract: MathOperation = (a, b) => a - b;

// -------- Function with Callback --------
function processArray(
  arr: number[],
  callback: (num: number) => number
): number[] {
  return arr.map(callback);
}

// -------- Generic Function --------
function identity<T>(value: T): T {
  return value; // Works for any type
}
let numVal = identity<number>(10);
let strVal = identity("TypeScript"); // Type inferred

// -------- Function Returning Promise --------
async function fetchData(): Promise<string> {
  return "Data fetched";
}

// -------- Void Function --------
function logMessage(message: string): void {
  console.log(message); // No return
}

// -------- Never Function --------
function throwError(msg: string): never {
  throw new Error(msg); // Never returns
}
```

---

## 3. Advanced Concepts

### 3.1 Interfaces

- TypeScript interfaces define the `structure of an object`, specifying `property types and method signatures`.

| **Feature**                       | **Example**                                                                               | **Description**                                             | **Notes**                                           |
| --------------------------------- | ----------------------------------------------------------------------------------------- | ----------------------------------------------------------- | --------------------------------------------------- |
| **Basic Interface**               | `interface User { id: number; name: string; }`                                            | Defines the **shape of an object** (properties & types).    | Only **structure**, no implementation.              |
| **Optional Properties**           | `interface User { id: number; name?: string; }`                                           | Adds `?` to mark properties as **optional**.                | Useful for partial objects (e.g., update forms).    |
| **Readonly Properties**           | `interface User { readonly id: number; name: string; }`                                   | Marks properties as **immutable** after initialization.     | Prevents accidental modifications.                  |
| **Function Types**                | `interface Greet { (name: string): string; }`                                             | Declares **function signatures** inside an interface.       | Alternative to `type` for function shapes.          |
| **Index Signatures**              | `interface Dictionary { [key: string]: string; }`                                         | Allows **dynamic property names** with defined value types. | Useful for objects with unknown keys.               |
| **Extending Interfaces**          | `interface Admin extends User { role: string; }`                                          | Inherits properties from another interface.                 | Can extend **multiple interfaces** using `extends`. |
| **Intersection via Extends**      | `interface SuperAdmin extends User, Admin { level: number; }`                             | Combine multiple interfaces into one.                       | Avoids duplication, good for composability.         |
| **Hybrid Types**                  | `interface Counter { (start: number): string; interval: number; reset(): void; }`         | Combines **function + object properties** in one interface. | Great for **complex API contracts**.                |
| **Class Implementation**          | `class Person implements User { constructor(public id: number, public name: string) {} }` | Enforces **classes to follow a contract**.                  | Ensures **type safety in OOP**.                     |
| **Extending Classes**             | `interface ExtendedPerson extends Person { address: string; }`                            | Allows **interfaces to extend classes** (inherit members).  | Treats class as a type, not implementation.         |
| **Merging (Declaration Merging)** | `interface User { id: number; } interface User { name: string; }`                         | TypeScript **merges multiple interface declarations**.      | Avoids conflicts & adds flexibility.                |
| **Generic Interfaces**            | `interface ApiResponse<T> { data: T; success: boolean; }`                                 | Makes interfaces **reusable with different types**.         | Great for APIs, reusable contracts.                 |

```ts
// -------- Basic Interface --------
interface User {
  id: number;
  name: string;
}

// -------- Optional Properties --------
interface Product {
  id: number;
  name: string;
  description?: string; // Optional
}

// -------- Readonly Properties --------
interface Config {
  readonly apiKey: string; // Cannot be reassigned
}

// -------- Function Interface --------
interface MathOperation {
  (a: number, b: number): number;
}
const add: MathOperation = (a, b) => a + b;

// -------- Index Signature --------
interface Dictionary {
  [key: string]: string; // Key-value pairs
}
let dict: Dictionary = { hello: "world" };

// -------- Extending Interfaces --------
interface Person {
  name: string;
}
interface Employee extends Person {
  employeeId: number;
}
const emp: Employee = { name: "Shubham", employeeId: 101 };

// -------- Multiple Inheritance --------
interface Address {
  city: string;
}
interface Contact {
  phone: string;
}
interface Customer extends Address, Contact {
  name: string;
}
const customer: Customer = {
  name: "Amit",
  city: "Mumbai",
  phone: "1234567890",
};

// -------- Interface for Class Implementation --------
interface Logger {
  log(message: string): void;
}
class ConsoleLogger implements Logger {
  log(message: string): void {
    console.log(message);
  }
}

// -------- Hybrid Interface (Function + Properties) --------
interface Counter {
  (start: number): string;
  count: number;
}
function getCounter(): Counter {
  const counter = ((start: number) => `Count: ${start}`) as Counter;
  counter.count = 0;
  return counter;
}
const c = getCounter();
console.log(c(5)); // "Count: 5"
console.log(c.count); // 0
```

### 3.2 Types

| **Feature**                  | **Example**                                                  | **Description**                                                | **Notes**                                       |
| ---------------------------- | ------------------------------------------------------------ | -------------------------------------------------------------- | ----------------------------------------------- |
| **Basic Type Alias**         | `type ID = string \| number;`                                | Creates a **custom name** for a type (primitive, union, etc.). | Useful for **reusability & readability**.       |
| **Union Types**              | `type Status = "success" \| "error" \| "pending";`           | Allows a value to be **one of multiple types**.                | Great for **state values**.                     |
| **Intersection Types**       | `type AdminUser = User & Admin;`                             | Combines multiple types into one.                              | Similar to interface `extends`.                 |
| **Function Types**           | `type MathOp = (a: number, b: number) => number;`            | Defines a **function signature**.                              | Alternative to `interface` for function shapes. |
| **Tuple Types**              | `type Coordinates = [number, number];`                       | Defines a **fixed-length array** with typed elements.          | Useful for **pairs or structured lists**.       |
| **Object Types**             | `type User = { id: number; name: string; };`                 | Creates **object structures** similar to interfaces.           | Preferred for **inline object types**.          |
| **Literal Types**            | `type Direction = "up" \| "down" \| "left" \| "right";`      | Restricts a value to **specific string/number literals**.      | Helps prevent invalid assignments.              |
| **Type with Optional Props** | `type User = { id: number; name?: string; };`                | Allows **optional properties** using `?`.                      | Similar to optional in interfaces.              |
| **Readonly Types**           | `type Config = { readonly apiKey: string; };`                | Makes properties **immutable** after assignment.               | Helps maintain data integrity.                  |
| **Generic Types**            | `type ApiResponse<T> = { data: T; success: boolean; };`      | Makes a type **reusable with any data type**.                  | Great for **API responses, utilities**.         |
| **Utility Types**            | `type PartialUser = Partial<User>;`                          | Uses built-in helpers like `Partial`, `Pick`, `Omit`.          | Speeds up type creation & refactoring.          |
| **Mapped Types**             | `type ReadonlyUser<T> = { readonly [K in keyof T]: T[K]; };` | Creates **new types dynamically** from existing ones.          | Enables **advanced type transformations**.      |
| **Conditional Types**        | `type IsString<T> = T extends string ? true : false;`        | Creates types **based on conditions**.                         | Powerful for **dynamic type logic**.            |

```ts
// -------- Basic Type Alias --------
type ID = string | number; // Can hold string or number
let userId: ID = 123;

// -------- Object Type --------
type User = {
  id: number;
  name: string;
  email?: string; // Optional
};

// -------- Function Type --------
type MathOperation = (a: number, b: number) => number;
const multiply: MathOperation = (a, b) => a * b;

// -------- Union Type --------
type Status = "success" | "error" | "pending";
let state: Status = "success";

// -------- Intersection Type --------
type Name = { name: string };
type Age = { age: number };
type Person = Name & Age; // Must have both
const person: Person = { name: "Shubham", age: 27 };

// -------- Tuple Type --------
type Coordinates = [number, number];
let location: Coordinates = [19.07, 72.87];

// -------- Nested Type --------
type ApiResponse<T> = {
  data: T;
  success: boolean;
  error?: string;
};
const response: ApiResponse<string> = { data: "OK", success: true };

// -------- Recursive Type --------
type Tree<T> = {
  value: T;
  children?: Tree<T>[];
};
const tree: Tree<number> = { value: 1, children: [{ value: 2 }] };

// -------- Discriminated Union --------
type Circle = { kind: "circle"; radius: number };
type Square = { kind: "square"; side: number };
type Shape = Circle | Square;

function area(shape: Shape): number {
  switch (shape.kind) {
    case "circle":
      return Math.PI * shape.radius ** 2;
    case "square":
      return shape.side ** 2;
  }
}
```

### Type vs Interface in TypeScript

| **Aspect**                | **`type`**                                                                                   | **`interface`**                                                                  |
| ------------------------- | -------------------------------------------------------------------------------------------- | -------------------------------------------------------------------------------- |
| **Purpose**               | Used to define **aliases** for any type: primitive, object, union, intersection, tuple, etc. | Used to define the **shape (contract)** of objects, classes, or functions.       |
| **Primitives**            | ✅ Can define primitives: `type ID = string \| number;`                                      | ❌ Cannot directly define primitives.                                            |
| **Objects**               | ✅ Can define objects: `type User = { id: number; }`                                         | ✅ Can define objects: `interface User { id: number; }`                          |
| **Functions**             | ✅ Can define function types: `type Fn = (x: number) => number;`                             | ✅ Can define function signatures: `interface Fn { (x: number): number }`        |
| **Tuples**                | ✅ Can define tuples: `type Pair = [string, number];`                                        | ❌ Cannot define tuples directly.                                                |
| **Unions**                | ✅ Can create unions: `type Status = "on" \| "off";`                                         | ❌ Cannot create unions directly.                                                |
| **Intersections**         | ✅ Supports intersections: `type AdminUser = User & Admin;`                                  | ✅ Supports via `extends`: `interface AdminUser extends User, Admin {}`          |
| **Extending**             | ❌ Cannot extend after creation (must create a new type).                                    | ✅ Can extend multiple interfaces (`extends`).                                   |
| **Declaration Merging**   | ❌ Not allowed — redeclaring a type will throw an error.                                     | ✅ **Allowed** — multiple interface declarations get merged.                     |
| **Mapped Types**          | ✅ Can use mapped types: `type ReadonlyUser<T> = { readonly [K in keyof T]: T[K] };`         | ❌ Cannot use mapped types.                                                      |
| **Conditional Types**     | ✅ Can use conditional types: `type IsString<T> = T extends string ? true : false;`          | ❌ Cannot use conditional types.                                                 |
| **Generic Support**       | ✅ Fully supports generics.                                                                  | ✅ Fully supports generics.                                                      |
| **Implements (Classes)**  | ❌ Cannot be directly implemented by a class.                                                | ✅ Can be implemented by classes (`class A implements Interface {}`).            |
| **Declaration Reopening** | ❌ Cannot reopen (must redefine).                                                            | ✅ Can reopen and add new properties later.                                      |
| **Readability**           | Good for complex types but can get **harder to read** with nested unions.                    | More **readable** for object structures and APIs.                                |
| **Use Cases**             | Best for: **Unions, tuples, utility types, conditional/mapped types, primitives.**           | Best for: **Defining object/class contracts, APIs, and when merging is needed.** |

### 3.3 Classes

| **Feature**              | **Example**                                                                                                          | **Description**                                                                                             | **Notes**                                                    |
| ------------------------ | -------------------------------------------------------------------------------------------------------------------- | ----------------------------------------------------------------------------------------------------------- | ------------------------------------------------------------ |
| **Basic Class**          | `class Person { name: string; constructor(name: string) { this.name = name; } }`                                     | Defines a blueprint for creating objects.                                                                   | Members default to **public**.                               |
| **Fields (Properties)**  | `class Car { brand: string = "BMW"; }`                                                                               | Declares variables that belong to class instances.                                                          | Can have **default values**.                                 |
| **Constructor**          | `constructor(name: string) { this.name = name; }`                                                                    | Special method for initializing class properties.                                                           | Can be overloaded (with optional params).                    |
| **Access Modifiers**     | `public`, `private`, `protected`                                                                                     | Controls visibility: `public` (default), `private` (within class), `protected` (within class & subclasses). | Improves **encapsulation**.                                  |
| **Readonly Properties**  | `readonly id: number;`                                                                                               | Property can only be set **once (on init)**.                                                                | Enforces immutability.                                       |
| **Optional Properties**  | `class User { age?: number; }`                                                                                       | Marks properties as **optional**.                                                                           | Similar to optional in interfaces.                           |
| **Methods**              | `class Person { greet() { return "Hello"; } }`                                                                       | Functions defined within a class.                                                                           | Can access `this` (class members).                           |
| **Getters & Setters**    | `get fullName() { return this.fName + " " + this.lName; } set fullName(n: string) { this.fName = n.split(" ")[0]; }` | Provides **controlled access** to properties.                                                               | Allows **computed properties**.                              |
| **Static Members**       | `static count: number = 0; static increment() { this.count++; }`                                                     | Belongs to the class itself, **not instances**.                                                             | Access via `ClassName.member`.                               |
| **Inheritance**          | `class Dog extends Animal { bark() { return "Woof"; } }`                                                             | Enables **reusing behavior** from a parent class.                                                           | Use `super()` to call the parent constructor.                |
| **Abstract Classes**     | `abstract class Shape { abstract area(): number; }`                                                                  | Cannot be instantiated. Used as a **blueprint**.                                                            | Must be extended & abstract methods **must** be implemented. |
| **Implements Interface** | `class Employee implements Person { id: number; name: string; }`                                                     | Forces class to follow a **specific contract**.                                                             | Ensures consistent API shape.                                |
| **Private Constructors** | `private constructor() {}`                                                                                           | Restricts creating instances (e.g., **Singleton pattern**).                                                 | Often used with `static` factory methods.                    |
| **Parameter Properties** | `constructor(public name: string, private age: number) {}`                                                           | Short syntax for declaring & initializing properties in one step.                                           | Saves boilerplate code.                                      |
| **Generic Classes**      | `class Box<T> { content: T; constructor(content: T) { this.content = content; } }`                                   | Classes that work with **any data type**.                                                                   | Makes classes **reusable & type-safe**.                      |

```ts
// -------- Basic Class --------
class Person {
  name: string; // Public by default
  age: number;

  constructor(name: string, age: number) {
    this.name = name;
    this.age = age;
  }

  greet(): string {
    return `Hello, my name is ${this.name}`;
  }
}

const p1 = new Person("Shubham", 27);

// -------- Access Modifiers --------
class Employee {
  public id: number; // Accessible everywhere
  private salary: number; // Accessible only within class
  protected department: string; // Accessible in class + subclasses

  constructor(id: number, salary: number, department: string) {
    this.id = id;
    this.salary = salary;
    this.department = department;
  }
}

// -------- Readonly Properties --------
class Config {
  readonly apiKey: string;
  constructor(apiKey: string) {
    this.apiKey = apiKey; // Can only be assigned once
  }
}

// -------- Getters & Setters --------
class User {
  private _name: string = "";
  get name(): string {
    return this._name;
  }
  set name(value: string) {
    if (value.length < 3) throw new Error("Name too short");
    this._name = value;
  }
}

// -------- Inheritance --------
class Animal {
  constructor(public species: string) {}
  makeSound(): string {
    return "Some sound";
  }
}
class Dog extends Animal {
  constructor() {
    super("Dog");
  }
  makeSound(): string {
    return "Bark!";
  }
}

// -------- Abstract Classes --------
abstract class Shape {
  abstract area(): number; // Must be implemented by subclasses
}
class Circle extends Shape {
  constructor(public radius: number) {
    super();
  }
  area(): number {
    return Math.PI * this.radius ** 2;
  }
}

// -------- Static Members --------
class MathUtils {
  static PI: number = 3.14159;
  static square(x: number): number {
    return x * x;
  }
}
const area = MathUtils.square(5);

// -------- Implementing Interfaces --------
interface Logger {
  log(msg: string): void;
}
class ConsoleLogger implements Logger {
  log(msg: string): void {
    console.log("Log:", msg);
  }
}
```

### 3.3 Generics

- allow creating 'type variables' which can be used to create classes, functions & type aliases that don't need to explicitly define the types that they use.
- Generics makes it easier to write reusable code.

| **Feature**                     | **Example**                                                                                 | **Description**                                                            | **Notes**                                                         |
| ------------------------------- | ------------------------------------------------------------------------------------------- | -------------------------------------------------------------------------- | ----------------------------------------------------------------- |
| **Basic Generic Function**      | `function identity<T>(value: T): T { return value; }`                                       | Allows a function to **work with any type** while maintaining type safety. | Type **inferred automatically** if not passed explicitly.         |
| **Generic with Multiple Types** | `function pair<T, U>(a: T, b: U): [T, U] { return [a, b]; }`                                | Uses **multiple type parameters** for flexibility.                         | Good for pairing different types.                                 |
| **Generic Interface**           | `interface ApiResponse<T> { data: T; success: boolean; }`                                   | Makes **interfaces reusable** with different data types.                   | Great for API responses & reusable contracts.                     |
| **Generic Class**               | `class Box<T> { content: T; constructor(content: T) { this.content = content; } }`          | Classes that **store or operate on different types**.                      | Often used in data structures (e.g., `Stack<T>`).                 |
| **Generic Constraints**         | `function printLength<T extends { length: number }>(item: T) { console.log(item.length); }` | Restricts **generic types to a specific shape**.                           | Prevents invalid operations on types without required properties. |
| **Default Generic Types**       | `function fetchData<T = string>(): T { return "data" as T; }`                               | Provides a **default type** when none is specified.                        | Helps keep code concise.                                          |
| **Keyof with Generics**         | `function getProp<T, K extends keyof T>(obj: T, key: K): T[K] { return obj[key]; }`         | Ensures key exists on the object type.                                     | Prevents accessing invalid keys.                                  |
| **Generic Type Alias**          | `type Wrapper<T> = { value: T };`                                                           | Creates **reusable generic types** for wrapping values.                    | Often used for containers or result types.                        |
| **Generic Utility Functions**   | `function merge<T, U>(obj1: T, obj2: U): T & U { return { ...obj1, ...obj2 }; }`            | Combines **two types into one**.                                           | Good for **mixins** & dynamic merging.                            |
| **Generic in Promises**         | `async function fetchData<T>(): Promise<T> { return {} as T; }`                             | Defines the **resolved type of a Promise**.                                | Great for **API responses & async workflows**.                    |

```ts
// -------- Basic Generic Function --------
function identity<T>(value: T): T {
  return value; // Works with any type
}
let num = identity<number>(10);
let str = identity("Hello"); // Type inferred

// -------- Generic with Multiple Types --------
function pair<T, U>(first: T, second: U): [T, U] {
  return [first, second];
}
const mixed = pair<string, number>("Age", 27);

// -------- Generic Interfaces --------
interface ApiResponse<T> {
  data: T;
  success: boolean;
}
const userResponse: ApiResponse<{ id: number; name: string }> = {
  data: { id: 1, name: "Shubham" },
  success: true,
};

// -------- Generic Classes --------
class Box<T> {
  contents: T;
  constructor(contents: T) {
    this.contents = contents;
  }
  getContents(): T {
    return this.contents;
  }
}
const stringBox = new Box<string>("Hello");
const numberBox = new Box<number>(123);

// -------- Generic Constraints --------
function getLength<T extends { length: number }>(item: T): number {
  return item.length; // Only works for items with a length property
}
let len = getLength("TypeScript");
let arrLen = getLength([1, 2, 3]);

// -------- Default Generic Types --------
function createArray<T = string>(value: T, count: number): T[] {
  return new Array(count).fill(value);
}
const defaultArray = createArray("Hello", 3); // Defaults to string

// -------- Generic with keyof --------
function getProperty<T, K extends keyof T>(obj: T, key: K): T[K] {
  return obj[key];
}
const person = { name: "Shubham", age: 27 };
let personName = getProperty(person, "name");

// -------- Generic Utility Type --------
type Nullable<T> = T | null;
const maybeNumber: Nullable<number> = null;
```

### 3.4 Type Assertions

- allows you to set the type of a value and tell the compiler not to infer it.

| **Feature**                     | **Example**                                                                                 | **Description**                                                                | **Notes**                                                         |                                                         |
| ------------------------------- | ------------------------------------------------------------------------------------------- | ------------------------------------------------------------------------------ | ----------------------------------------------------------------- | ------------------------------------------------------- |
| **Basic Type Assertion**        | `let value: unknown = "Hello"; let strLength = (value as string).length;`                   | Tells TypeScript to **treat a value as a specific type**.                      | Does **not** change runtime behavior — only affects the compiler. |                                                         |
| **`as` Syntax (Preferred)**     | `let num = someValue as number;`                                                            | Modern, **recommended** way to assert types.                                   | Cleaner and works in JSX.                                         |                                                         |
| **Angle-Bracket Syntax**        | `let num = <number>someValue;`                                                              | Legacy syntax for assertions.                                                  | ❌ **Not allowed in `.tsx`** files (conflicts with JSX).          |                                                         |
| **Assertion with DOM Elements** | `const input = document.querySelector("input") as HTMLInputElement; input.value = "Hello";` | Used when **querying DOM elements** to access their properties.                | Ensures type safety when dealing with the DOM.                    |                                                         |
| **Assertion for Union Types**   | \`let id: string                                                                            | number; let strID = (id as string).toUpperCase();\`                            | Allows treating a **union as a narrower type** temporarily.       | Only safe when you **know the actual type** at runtime. |
| **Non-Null Assertion**          | `let element = document.getElementById("app")!; element.innerHTML = "Hi";`                  | Asserts that a value is **not null or undefined**.                             | Use carefully — can cause runtime errors if wrong.                |                                                         |
| **Double Assertions**           | `let num = ("hello" as unknown) as number;`                                                 | Forces a value to a type through an **intermediate type**.                     | Avoid if possible — indicates poor type design.                   |                                                         |
| **Type Casting vs Assertion**   | **Casting** converts data at runtime; **Assertion** only affects TypeScript's type system.  | Helps clarify the difference between TypeScript types and JS runtime behavior. | Assertions are **compile-time only** (no actual conversion).      |                                                         |

```typescript
// -------- Basic Type Assertion --------
let value: unknown = "Hello TypeScript";
let length1 = (value as string).length; // "as" syntax
let length2 = (<string>value).length; // Angle-bracket syntax (not in JSX)

// -------- DOM Element Assertion --------
const input = document.querySelector("#username") as HTMLInputElement;
input.value = "Shubham"; // Safe because we asserted type

// -------- Non-Null Assertion --------
const el = document.getElementById("myDiv")!;
el.innerHTML = "Content"; // `!` tells TS it's not null

// -------- Asserting to a More Specific Type --------
interface Person {
  name: string;
}
const obj = {} as Person;
obj.name = "Amit"; // TypeScript allows since asserted as Person

// -------- Asserting to Unknown First (Double Assertion) --------
let num = "123" as unknown as number; // Avoid — unsafe casting

// -------- When to Use --------
// 1. When you know more about a value than TS does (e.g., DOM queries, API data).
// 2. For type narrowing when working with `unknown` or `any`.
// 3. Avoid unnecessary assertions — prefer proper type definitions.
```

### 3.5 Type Aliases and Union Types

| **Feature**               | **Example**                                                                            | **Description**                                                       | **Notes**                                             |
| ------------------------- | -------------------------------------------------------------------------------------- | --------------------------------------------------------------------- | ----------------------------------------------------- |
| **Basic Type Alias**      | `type ID = string \| number;`                                                          | Creates a **custom name** for a type (primitive, union, tuple, etc.). | Improves **readability & reusability**.               |
| **Object Type Alias**     | `type User = { id: number; name: string; };`                                           | Defines an **object shape** using a type alias.                       | Similar to interfaces, but more flexible.             |
| **Union Types**           | `type Status = "success" \| "error" \| "pending";`                                     | Allows a value to be **one of several types**.                        | Ideal for **state management** or predefined options. |
| **Union with Primitives** | `type Input = string \| number;`                                                       | Combines **different primitive types**.                               | Common for APIs that accept multiple input formats.   |
| **Union with Objects**    | `type Shape = Circle \| Square;`                                                       | Combines multiple **object types** into one.                          | Helps model **discriminated unions**.                 |
| **Literal Types**         | `type Direction = "up" \| "down" \| "left" \| "right";`                                | Restricts a value to **specific string/number literals**.             | Prevents invalid assignments.                         |
| **Nullable Types**        | `type Nullable<T> = T \| null \| undefined;`                                           | Makes a type **nullable**.                                            | Useful for optional API values or DB records.         |
| **Function Type Alias**   | `type MathOp = (a: number, b: number) => number;`                                      | Defines **function signatures** with a type alias.                    | Preferred for reusable function contracts.            |
| **Nested Type Aliases**   | `type ApiResponse<T> = { data: T; success: boolean; error?: string; };`                | Combines generics with type aliases.                                  | Great for **APIs & reusable structures**.             |
| **Discriminated Unions**  | `type Shape = { kind: "circle"; radius: number } \| { kind: "square"; side: number };` | Union types **with a distinguishing property**.                       | Enables **type-safe narrowing** using `switch`.       |

```typescript
// -------- Type Alias (Basic) --------
type ID = string | number; // Can hold string or number
let userId: ID = 123;
userId = "ABC123";

// -------- Object Type Alias --------
type User = {
  id: number;
  name: string;
  email?: string; // Optional property
};
const user: User = { id: 1, name: "Shubham" };

// -------- Function Type Alias --------
type MathOperation = (a: number, b: number) => number;
const add: MathOperation = (a, b) => a + b;

// -------- Union Type --------
type Status = "success" | "error" | "pending"; // Literal union
let requestStatus: Status = "pending";

// -------- Complex Union --------
type ApiResponse =
  | { success: true; data: string }
  | { success: false; error: string };
const res: ApiResponse = { success: true, data: "OK" };

// -------- Intersection with Union --------
type Name = { name: string };
type Age = { age: number };
type Person = Name & Age; // Intersection
const person: Person = { name: "Shubham", age: 27 };

// -------- Discriminated Union (Tagged Union) --------
type Circle = { kind: "circle"; radius: number };
type Square = { kind: "square"; side: number };
type Shape = Circle | Square;

function area(shape: Shape): number {
  switch (shape.kind) {
    case "circle":
      return Math.PI * shape.radius ** 2;
    case "square":
      return shape.side ** 2;
  }
}
```

### 3.6 Utility Types

| **Utility Type**               | **Example**                                                   | **Description**                                          | **Notes**                                            |
| ------------------------------ | ------------------------------------------------------------- | -------------------------------------------------------- | ---------------------------------------------------- |
| **`Partial<T>`**               | `type PartialUser = Partial<User>;`                           | Makes **all properties optional** in type `T`.           | Great for **update operations** (e.g., PATCH APIs).  |
| **`Required<T>`**              | `type FullUser = Required<User>;`                             | Makes **all properties required** in type `T`.           | Opposite of `Partial`.                               |
| **`Readonly<T>`**              | `type ReadonlyUser = Readonly<User>;`                         | Makes **all properties immutable**.                      | Prevents accidental mutations.                       |
| **`Pick<T, K>`**               | `type UserPreview = Pick<User, "id" \| "name">;`              | Creates a type by **selecting specific keys** from `T`.  | Good for **lightweight projections**.                |
| **`Omit<T, K>`**               | `type UserWithoutPassword = Omit<User, "password">;`          | Creates a type by **excluding specific keys** from `T`.  | Often used to hide sensitive data.                   |
| **`Record<K, T>`**             | `type Roles = Record<"admin" \| "user", string>;`             | Creates an **object type with keys `K` and values `T`**. | Useful for **maps or lookup tables**.                |
| **`Exclude<T, U>`**            | `type NonString = Exclude<string \| number, string>;`         | Removes types from `T` that are assignable to `U`.       | Helps refine union types.                            |
| **`Extract<T, U>`**            | `type OnlyString = Extract<string \| number, string>;`        | Extracts types from `T` that are assignable to `U`.      | Opposite of `Exclude`.                               |
| **`NonNullable<T>`**           | `type Safe = NonNullable<string \| null \| undefined>;`       | Removes `null` and `undefined` from `T`.                 | Great for strict APIs.                               |
| **`ReturnType<T>`**            | `type FnReturn = ReturnType<() => string>;`                   | Extracts the **return type** of a function.              | Helps infer function outputs.                        |
| **`Parameters<T>`**            | `type FnParams = Parameters<(a: number, b: string) => void>;` | Extracts a **tuple of parameters** from a function.      | Useful for **wrappers & decorators**.                |
| **`ConstructorParameters<T>`** | `type CtorParams = ConstructorParameters<typeof Person>;`     | Extracts **constructor parameter types**.                | Helps in **class factories & dependency injection**. |
| **`InstanceType<T>`**          | `type PersonInstance = InstanceType<typeof Person>;`          | Extracts the **instance type** of a class constructor.   | Good for **working with dynamic classes**.           |
| **`Awaited<T>`**               | `type Resolved = Awaited<Promise<string>>;`                   | Extracts the **resolved type** of a Promise.             | Helps with async/await workflows.                    |

```typescript
// -------- Partial<T> --------
// Makes all properties optional
interface User {
  id: number;
  name: string;
  email: string;
}
type PartialUser = Partial<User>;
const user1: PartialUser = { name: "Shubham" };

// -------- Required<T> --------
// Makes all properties required
type RequiredUser = Required<PartialUser>;
const user2: RequiredUser = { id: 1, name: "Shubham", email: "abc@gmail.com" };

// -------- Readonly<T> --------
// Makes all properties readonly
type ReadonlyUser = Readonly<User>;
const user3: ReadonlyUser = { id: 1, name: "Amit", email: "amit@gmail.com" };
// user3.name = "New Name"; // ❌ Error

// -------- Pick<T, K> --------
// Selects specific keys from a type
type UserPreview = Pick<User, "id" | "name">;
const user4: UserPreview = { id: 1, name: "Shubham" };

// -------- Omit<T, K> --------
// Removes specific keys from a type
type UserWithoutEmail = Omit<User, "email">;
const user5: UserWithoutEmail = { id: 2, name: "Amit" };

// -------- Record<K, T> --------
// Creates an object with keys of type K and values of type T
type UserRoles = Record<string, "admin" | "user">;
const roles: UserRoles = { Shubham: "admin", Amit: "user" };

// -------- Exclude<T, U> --------
// Removes types from a union
type Status = "success" | "error" | "pending";
type NonPending = Exclude<Status, "pending">; // "success" | "error"

// -------- Extract<T, U> --------
// Extracts only the matching types
type OnlyPending = Extract<Status, "pending">; // "pending"

// -------- NonNullable<T> --------
// Removes null and undefined from a type
type StrictUser = NonNullable<string | null | undefined>; // string

// -------- ReturnType<T> --------
// Extracts return type of a function
function greet(name: string) {
  return `Hello, ${name}`;
}
type GreetReturn = ReturnType<typeof greet>; // string

// -------- Parameters<T> --------
// Extracts parameter types as a tuple
type GreetParams = Parameters<typeof greet>; // [name: string]

// -------- ConstructorParameters<T> --------
// Extracts constructor parameter types
class Person {
  constructor(public name: string, public age: number) {}
}
type PersonParams = ConstructorParameters<typeof Person>; // [string, number]

// -------- InstanceType<T> --------
// Extracts instance type of a class
type PersonInstance = InstanceType<typeof Person>; // Person
```

### 3.7 Advanced Types

#### Intersection Types

| **Feature**                 | **Example**                                                                                                     | **Description**                                             | **Notes**                                                  |
| --------------------------- | --------------------------------------------------------------------------------------------------------------- | ----------------------------------------------------------- | ---------------------------------------------------------- |
| **Basic Intersection**      | `type A = { name: string }; type B = { age: number }; type Person = A & B;`                                     | Combines **multiple types into one**.                       | The resulting type must have **all properties** from each. |
| **Object Intersection**     | `type Address = { city: string }; type Employee = { id: number } & Address;`                                    | Merges object shapes into one type.                         | Useful for **extending** types.                            |
| **With Interfaces**         | `interface A { name: string } interface B { age: number } type Person = A & B;`                                 | Works with **interfaces & types** together.                 | Makes types **composable**.                                |
| **Function Intersection**   | `ts\ntype Log = (msg: string) => void;\ntype ErrorLog = (err: Error) => void;\ntype Logger = Log & ErrorLog;\n` | Functions that must satisfy **multiple signatures**.        | Rare but used in **overloads & mixins**.                   |
| **Class Intersection**      | `ts\nclass A { a() {} } class B { b() {} } type Combined = A & B;`                                              | Combines instance types of classes.                         | Requires **type casting** to use combined instance.        |
| **With Generics**           | `type ApiResponse<T> = { data: T } & { success: boolean };`                                                     | Creates **generic reusable types** with extra fields.       | Used for **API wrappers** & **decorators**.                |
| **Narrowing Intersections** | `ts\ntype Admin = { role: "admin" }; type User = { name: string }; type AdminUser = Admin & User;\n`            | Ensures a type has **properties of all intersected types**. | Good for **role-based entities**.                          |
| **Intersection vs Union**   | `A & B` means **must satisfy both**; `A \| B` means **can be either**.                                          | Intersection is **strict**, union is **flexible**.          | Use **intersection** when you need **all constraints**.    |

```typescript
// Basic intersection
type Name = { name: string };
type Age = { age: number };
type Person = Name & Age;

const p1: Person = { name: "Shubham", age: 27 }; // Must have both

// Generic intersection
type ApiResponse<T> = { data: T } & { success: boolean };
const response: ApiResponse<string> = { data: "Hello", success: true };

// Intersection with interfaces
interface Admin {
  role: "admin";
}
interface User {
  name: string;
}
type AdminUser = Admin & User;
const admin: AdminUser = { role: "admin", name: "Amit" };
```

#### Mapped Types

- allows you to create new types by transforming the properties of an existing type

```typescript
type ReadonlyUser = Readonly<User>;
type OptionalUser = Partial<User>;
```

#### Conditional Types

- enable types to be defined based on a condition

```ts
type Num<T> = T extends number[] ? number : T extends string[] ? string : never;

// Return num
const num: Num<number[]> = 4;

// Return invalid
const stringnum: Num<number> = "7";

console.log(num, stringnum);
```

---

## Using TypeScript Effectively with React

### 1. Setting Up TypeScript with React

#### Steps:

1. Install TypeScript and necessary type definitions:
   ```bash
   npm install typescript @types/react @types/react-dom
   ```
2. Rename `.js` files to `.tsx`.
3. Configure `tsconfig.json`:
   ```json
   {
     "compilerOptions": {
       "target": "es5",
       "module": "esnext",
       "jsx": "react",
       "strict": true
     }
   }
   ```

---

### 2. Typing Props and State

#### Functional Components

Use `React.FC` or explicitly type props.

#### Example:

```typescript
const Greeting = ({ name }: GreetingProps) => {
  return <h1>Hello, {name}!</h1>;
};
```

#### Class Components

#### Example:

```typescript
interface CounterProps {
  initialCount: number;
}

interface CounterState {
  count: number;
}

class Counter extends React.Component<CounterProps, CounterState> {
  constructor(props: CounterProps) {
    super(props);
    this.state = { count: props.initialCount };
  }

  increment = () => {
    this.setState({ count: this.state.count + 1 });
  };

  render() {
    return (
      <div>
        <p>Count: {this.state.count}</p>
        <button onClick={this.increment}>Increment</button>
      </div>
    );
  }
}
```

---

### 3. Typing Events

Use React's `SyntheticEvent` for typing events.

#### Example:

```typescript
const handleChange = (event: React.ChangeEvent<HTMLInputElement>) => {
  console.log(event.target.value);
};

return <input type="text" onChange={handleChange} />;
```

---

### 4. Typing Refs

#### Example:

```typescript
const inputRef = React.useRef<HTMLInputElement>(null);

const focusInput = () => {
  inputRef.current?.focus();
};

return (
  <div>
    <input ref={inputRef} type="text" />
    <button onClick={focusInput}>Focus Input</button>
  </div>
);
```

---

### 5. Typing Context

#### Creating Context:

```typescript
interface ThemeContextProps {
  theme: string;
  toggleTheme: () => void;
}

const ThemeContext = React.createContext<ThemeContextProps | undefined>(
  undefined
);
```

#### Using Context:

```typescript
const ThemeProvider: React.FC = ({ children }) => {
  const [theme, setTheme] = React.useState("light");

  const toggleTheme = () => {
    setTheme((prev) => (prev === "light" ? "dark" : "light"));
  };

  return (
    <ThemeContext.Provider value={{ theme, toggleTheme }}>
      {children}
    </ThemeContext.Provider>
  );
};
```

#### Consuming Context:

```typescript
const ThemedComponent: React.FC = () => {
  const context = React.useContext(ThemeContext);

  if (!context) {
    throw new Error("ThemedComponent must be used within a ThemeProvider");
  }

  const { theme, toggleTheme } = context;

  return (
    <div>
      <p>Current Theme: {theme}</p>
      <button onClick={toggleTheme}>Toggle Theme</button>
    </div>
  );
};
```

---

### 6. Typing Custom Hooks

#### Example:

```typescript
function useCounter(initialValue: number): [number, () => void, () => void] {
  const [count, setCount] = React.useState(initialValue);

  const increment = () => setCount((prev) => prev + 1);
  const decrement = () => setCount((prev) => prev - 1);

  return [count, increment, decrement];
}

const CounterComponent: React.FC = () => {
  const [count, increment, decrement] = useCounter(0);

  return (
    <div>
      <p>Count: {count}</p>
      <button onClick={increment}>Increment</button>
      <button onClick={decrement}>Decrement</button>
    </div>
  );
};
```

---

### 7. Best Practices

1. **Strict Typing**: Always enable strict mode in `tsconfig.json`.
2. **Avoid `any`**: Use specific types or `unknown` if unsure.
3. **Component Props**: Always define and use interfaces or types for props.
4. **Default Props**: Use default parameter values or explicitly define defaults in props.
5. **Use `React.FC`**: For consistency in typing functional components.
6. **Type Assertions**: Avoid unless absolutely necessary.
7. **Refactor Reusable Types**: Extract common types into separate files for reuse.
