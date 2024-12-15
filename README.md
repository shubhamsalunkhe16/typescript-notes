# Comprehensive Guide to TypeScript

## `1. Introduction to TypeScript`

### `What is TypeScript?`

- `strongly typed`, `object-oriented`, `compiled language` that builds on JavaScript by adding static type definitions.

### `Why Use TypeScript?`

- `Static Typing`: Reduces runtime errors by catching them during development.
- `Improved Tooling`: Better support for IDEs and tools like IntelliSense.
- `Scalability`: Helps manage large-scale applications with ease.
- `Interoperability`: Works seamlessly with JavaScript.

### `Installing TypeScript`

```bash
npm install -g typescript
```

### `Compiling TypeScript`

```bash
tsc filename.ts
```

### `TypeScript Config File`

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

## `2. Basic Concepts`

### `2.1 Types`

### `Primitive Types`

```typescript
let isActive: boolean = true;
let age: number = 25;
let name: string = "John";
let notDefined: undefined = undefined;
let notPresent: null = null;
let bigValue: bigint = BigInt(1000000000000000);
let id: symbol = Symbol("uniqueId");
```

### `Array`

```typescript
let numbers: number[] = [1, 2, 3];
let numbers2: Array<number> = [1, 2, 3];
let mixed: (number | string)[] = [1, "two", 3];
const names: readonly string[] = ["Dylan"];
```

### `Tuple`

- `typed array` with a `pre-defined length` and `types for each index`

```typescript
let user: [number, string] = [1, "John"];
---------------------------------------------------
// define our tuple
let ourTuple: [number, boolean, string];
// initialize correctly
ourTuple = [5, false, "Coding God was here"];
// We have no type safety in our tuple for indexes 3+
ourTuple.push("Something new and wrong");
console.log(ourTuple); // [ 5, false, 'Coding God was here', 'Something new and wrong' ]
---------------------------------------------------
// define our readonly tuple
const ourReadonlyTuple: readonly [number, boolean, string] = [
  5,
  true,
  "The Real Coding God",
];
// throws error as it is readonly.
ourReadonlyTuple.push("Coding God took a day off");
```

### `Objects`

```typescript
const aObject: Object = { id: 1, name: "foo" };
const bObject: { id: number } = { id: 1 };
```

### `Enum`

- special `class` that represents a group of constants (unchangeable variables).

```typescript
enum Color {
  Red,
  Green,
  Blue,
}
let color: Color = Color.Green;
---------------------------------------------------
enum StatusCodes {
  NotFound = 404,
  Success = 200,
  Accepted = 202,
  BadRequest = 400
}
// logs 404
console.log(StatusCodes.NotFound);
// logs 200
console.log(StatusCodes.Success);
```

### `Any`

- type that `disables type checking` and `effectively allows all types` to be used

```typescript
let randomValue: any = 10;
randomValue = "Hello";
```

### `unknown`

- The `unknown` type is a safer alternative to `any`.
- It represents a `type that could be anything`, but unlike `any`, you must perform type checks before using it.

### Example:

```typescript
let value: unknown;
value = "Hello, World!";
value = 42;

if (typeof value === "string") {
  console.log(value.toUpperCase()); // Safe to use as string
}
```

### `any` vs `unknown`

### Differences:

| Feature                      | `any`                                   | `unknown`                        |
| ---------------------------- | --------------------------------------- | -------------------------------- |
| Type Safety                  | No safety                               | Type-safe (requires type checks) |
| Access to Properties/Methods | Direct access                           | Requires type checking           |
| Usage                        | Used for quick prototyping or migration | Used for safer dynamic values    |

### Example:

### `any`:

```typescript
let anyValue: any = "Hello";
console.log(anyValue.toUpperCase()); // No error

anyValue = 42;
console.log(anyValue.toUpperCase()); // Runtime error
```

### `unknown`:

```typescript
let unknownValue: unknown = "Hello";

if (typeof unknownValue === "string") {
  console.log(unknownValue.toUpperCase()); // Safe to use
}

unknownValue = 42;
// console.log(unknownValue.toUpperCase()); // Compile-time error
```

### Key Points:

- Prefer `unknown` for better type safety.
- Avoid `any` unless absolutely necessary.

### `never`

- The `never` type represents values that never occur.
- It is often used in cases like functions that always throw errors or infinite loops.

### Example:

```typescript
function throwError(message: string): never {
  throw new Error(message);
}

function infiniteLoop(): never {
  while (true) {}
}
```

### Key Points:

- Functions returning `never` cannot have a reachable end point.
- Useful in exhaustive type checking.

---

### `Void`

- used to indicate a `function doesn't return any value.`

```typescript
function logMessage(message: string): void {
  console.log(message);
}
```

### `2.2 Functions`

### `Function with Types`

```typescript
function add(a: number, b: number): number {
  return a + b;
}
```

### `Optional and Default Parameters`

```typescript
function greet(name: string, age?: number): string {
  return `Hello, ${name}. ${age ? `You are ${age}.` : ""}`;
}

function multiply(a: number, b: number = 1): number {
  return a * b;
}
```

### `Rest Parameters`

```typescript
function sum(...numbers: number[]): number {
  return numbers.reduce((acc, curr) => acc + curr, 0);
}
```

---

## `3. Advanced Concepts`

### `3.1 Interfaces`

- TypeScript interfaces define the `structure of an object`, specifying `property types and method signatures`.

### `Defining an Interface`

```typescript
interface User {
  id: number;
  name: string;
  isActive?: boolean;
}

let user: User = { id: 1, name: "Alice" };
```

### `Function Type Interface`

```typescript
interface Greet {
  (name: string): string;
}

const greet: Greet = (name) => `Hello, ${name}`;
```

### `Extending Interfaces`

- Interfaces can extend other interfaces, allowing for the reuse and expansion of existing interface definitions.

### Example:

```typescript
interface Person {
  name: string;
  age: number;
}

interface Employee extends Person {
  employeeId: number;
}

const emp: Employee = {
  name: "John Doe",
  age: 30,
  employeeId: 101,
};
```

### `Implementing Interfaces`

- Classes can implement interfaces, ensuring they adhere to a specific structure.

### Example:

```typescript
interface Vehicle {
  speed: number;
  drive(): void;
}

class Car implements Vehicle {
  speed: number;

  constructor(speed: number) {
    this.speed = speed;
  }

  drive() {
    console.log(`Driving at ${this.speed} km/h`);
  }
}

const car = new Car(120);
car.drive();
```

### `type` vs `interface`

### Differences:

| Feature                 | `type`                               | `interface`                     |
| ----------------------- | ------------------------------------ | ------------------------------- |
| Syntax                  | `type MyType = { ... }`              | `interface MyInterface { ... }` |
| Extensibility           | Cannot be reopened to add properties | Can be extended or reopened     |
| Merging                 | Not possible                         | Possible                        |
| Union Types             | Supported                            | Not Supported                   |
| Declaration Performance | Slightly faster                      | Slightly slower                 |

### Example:

#### Using `type`:

```typescript
type Point = {
  x: number;
  y: number;
};

const p: Point = { x: 10, y: 20 };
```

#### Using `interface`:

```typescript
interface Point {
  x: number;
  y: number;
}

const p: Point = { x: 10, y: 20 };
```

### When to Use:

- Use `interface` when defining object shapes and extending structures.
- Use `type` for unions, intersections, or other advanced types.

### `3.2 Classes`

### `Basic Class`

```typescript
class Person {
  private name: string;

  constructor(name: string) {
    this.name = name;
  }

  greet(): string {
    return `Hi, I'm ${this.name}`;
  }
}

const person = new Person("John");
console.log(person.greet());
```

### `Inheritance`

```typescript
class Employee extends Person {
  private role: string;

  constructor(name: string, role: string) {
    super(name);
    this.role = role;
  }

  describe(): string {
    return `${this.greet()} and I work as a ${this.role}.`;
  }
}

const employee = new Employee("Alice", "Developer");
console.log(employee.describe());
```

### `Abstract Classes`

```typescript
abstract class Shape {
  abstract area(): number;
}

class Circle extends Shape {
  constructor(private radius: number) {
    super();
  }

  area(): number {
    return Math.PI * this.radius ` 2;
  }
}
```

### `3.3 Generics`

- allow creating 'type variables' which can be used to create classes, functions & type aliases that don't need to explicitly define the types that they use.
- Generics makes it easier to write reusable code.

### `Generic Functions`

```typescript
function identity<T>(arg: T): T {
  return arg;
}

let output = identity<string>("Hello");
```

### `Generic Classes`

```typescript
// Classes
class Box<T> {
  private contents: T;

  constructor(contents: T) {
    this.contents = contents;
  }

  getContents(): T {
    return this.contents;
  }
}

const stringBox = new Box<string>("Gift");
console.log(stringBox.getContents());
```

### `Type Aliases`

```typescript
type Wrapped<T> = { value: T };
const wrappedValue: Wrapped<number> = { value: 10 };
----------------------------------------------------
```

### `3.4 Type Assertions`

- allows you to set the type of a value and tell the compiler not to infer it.

```typescript
let someValue: unknown = "Hello";
let strLength: number = (someValue as string).length;
```

### `3.5 Type Aliases and Union Types`

### `Type Aliases`

```typescript
type ID = string | number;
let userId: ID = 123;
```

### `Union Types`

```typescript
type Transport = "Bus" | "Car" | "Bike" | "Walk";
function formatID(id: string | number): string {
  return `ID: ${id}`;
}
```

### `3.6 Utility Types`

### `Partial`

- Partial changes all the properties in an object to be optional.

```typescript
interface Point {
  x: number;
  y: number;
}

let pointPart: Partial<Point> = {}; // `Partial` allows x and y to be optional
pointPart.x = 10;
```

### `Required`

- Required changes all the properties in an object to be required.

```typescript
interface Car {
  make: string;
  model: string;
  mileage?: number;
}

let myCar: Required<Car> = {
  make: "Ford",
  model: "Focus",
  mileage: 12000, // `Required` forces mileage to be defined
};
```

### `Readonly`

- Readonly is used to create a new type where all properties are readonly, meaning they cannot be modified once assigned a value.

```typescript
interface Person {
  name: string;
  age: number;
}
const person: Readonly<Person> = {
  name: "Dylan",
  age: 35,
};
person.name = "Israel"; // prog.ts(11,8): error TS2540: Cannot assign to 'name' because it is a read-only property.
```

### `Record`

- Record is a shortcut to defining an object type with a specific key type and value type.

```typescript
const nameAgeMap: Record<string, number> = {
  Alice: 21,
  Bob: 25,
};
```

### `Pick`

- Pick removes all but the specified keys from an object type.

```typescript
interface Person {
  name: string;
  age: number;
  location?: string;
}

const bob: Pick<Person, "name"> = {
  name: "Bob",
  // `Pick` has only kept name, so age and location were removed from the type and they can't be defined here
};
```

### `Omit`

- Omit removes keys from an object type.

```typescript
interface Person {
  name: string;
  age: number;
  location?: string;
}

const bob: Omit<Person, "age" | "location"> = {
  name: "Bob",
  // `Omit` has removed age and location from the type and they can't be defined here
};
```

### `Exclude`

- Exclude removes types from a union.

```typescript
type Primitive = string | number | boolean;
const value: Exclude<Primitive, string> = true; // a string cannot be used here since Exclude removed it from the type.
```

### `3.7 Modules`

### `Export/Import`

```typescript
// math.ts
export function add(a: number, b: number): number {
  return a + b;
}

// app.ts
import { add } from "./math";
console.log(add(2, 3));
```

### `3.8 Decorators`

### `Class Decorator`

```typescript
function Logger(constructor: Function) {
  console.log(`Logging: ${constructor.name}`);
}

@Logger
class ExampleClass {
  //...
}
```

### `3.9 Advanced Types`

### `Intersection Types`

```typescript
interface Person {
  name: string;
}
interface Employee {
  role: string;
}
type Worker = Person & Employee;

const worker: Worker = { name: "John", role: "Developer" };
```

### `Mapped Types`

- allows you to create new types by transforming the properties of an existing type

```typescript
type ReadonlyUser = Readonly<User>;
type OptionalUser = Partial<User>;
```

### `Conditional Types`

- enable types to be defined based on a condition

```typescript
type Num<T> = T extends number[] ? number : T extends string[] ? string : never;

// Return num
const num: Num<number[]> = 4;

// Return invalid
const stringnum: Num<number> = "7";

console.log(num, stringnum);
```

---

## `4. Using TypeScript Effectively with React`

### 1. Setting Up TypeScript with React

### Steps:

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

### Functional Components

Use `React.FC` or explicitly type props.

#### Example:

```typescript
interface GreetingProps {
  name: string;
}

const Greeting: React.FC<GreetingProps> = ({ name }) => {
  return <h1>Hello, {name}!</h1>;
};
```

Or:

```typescript
const Greeting = ({ name }: GreetingProps) => {
  return <h1>Hello, {name}!</h1>;
};
```

### Class Components

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

### Example:

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

### Creating Context:

```typescript
interface ThemeContextProps {
  theme: string;
  toggleTheme: () => void;
}

const ThemeContext = React.createContext<ThemeContextProps | undefined>(
  undefined
);
```

### Using Context:

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

### Consuming Context:

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

### Example:

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

## `5. Practical Example`

```typescript
interface Product {
  id: number;
  name: string;
  price: number;
}

class Cart {
  private items: Product[] = [];

  addProduct(product: Product): void {
    this.items.push(product);
  }

  calculateTotal(): number {
    return this.items.reduce((total, item) => total + item.price, 0);
  }
}

const cart = new Cart();
cart.addProduct({ id: 1, name: "Book", price: 20 });
cart.addProduct({ id: 2, name: "Pen", price: 5 });

console.log(cart.calculateTotal()); // Output: 25
```

---
