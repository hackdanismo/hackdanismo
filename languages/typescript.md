# TypeScript
`TypeScript` is a programming language created by Microsoft as a subset of `JavaScript`. `TypeScript` builds on `JavaScript` and includes `types` and `type checking`. 

`TypeScript` warns of errors before a program runs.

`TypeScript` files usually have the file extension of: `.ts`

It is useful because it helps to catch bugs earlier and makes large codebases easier to understand. It also improves autocomplete and editor tooling, but works with existing JavaScript libraries. Unlike `JavaScript`, browsers do not execute `TypeScript` directly; it's usually compiled/transpiled into `JavaScript` first.

For example, in `JavaScript`:

```javascript
function add(a, b) {
    return a + b;
}
```

In `TypeScript`, we can specify that `a` and `b` must be numbers:

```typescript
function add(a: number, b: number): number {
    return a + b;
}
```

+ `function`: tells `TypeScript` that this is a function.
+ `add`: is the name of the function.
+ `a: number`: means the first parameter is called `a` and it must be a number.
+ `b: number`: means the second parameter is called `b` and it must be a number.
+ `: number`: after the parenthesis means the function is expected to return a number.
+ `return a + b`: adds `a` and `b`, then sends the result back.

If you accidentally write:

```typescript
add("5", 10);
```

As `"5"` is a `string` not a `number` so a warning will be returned.

## Data Types
In `TypeScript`, common `data types` include:

```typescript
// string: text
let name: string = "Dan";
// number: integers and decimals
let age: number = 40;
// boolean: true or false
let isLoggedIn: boolean = true;
// array: is a list of values
let scores: number[] = [10, 20, 30];
let names: string[] = ["Alice", "Bob"];

// object: a structured value
let user: { name: string, age: number } = {
    name: "Alice",
    age: 25
};

// null and undefined: represent missing values
let result: null = null;
let value: undefined = undefined;
// any: allows any type, but removes much to TypeScript's safety
let data: any = "hello";
// unknown: can hold anything, but TypeScript makes you check the type before using it
let data: unknown = "hello";

// void: commonly used for functions that don't return a value
function sayHello(): void {
    console.log("Hello");
}

// Types can be combined using a union
let id: string | number;
id = 123;
id = "ABC123";
```

If the two inputs have different types, the return type depends on what operation we want to perform. For example, suppose `a` is a `number` and `b` is a `string`:

```typescript
function add (a: number, b: string): string {
    return a + b;
}

// Call this function
add(5, " applies");        // Returns: "5 apples"
```

Adding a number to a string produces a string. We can also allow either parameter to be a number or a string using a union type:

```typescript
function add (a: number | string, b: number | string): number | string {
    if (typeof a === "number" && typeof b === "number") {
        return a + b;
    }
    return String(a) + String(b);
}

// Call this function
add(5, 3);                // 8
add("Hi ", "Bob");        // "Hi Bob"
add(5, " apples");        // "5 apples"
```

This function may return either a number or a string.

## Type and Interface
In `TypeScript`, you can define the shape of an object with either a `type` or `interface`. 

```typescript
// type
type User = {
    name: string;
    age: number;
    isAdmin: boolean;
};

// OR, use interface:
interface User = {
    name: string;
    age: number;
    isAdmin: boolean;
};

const user: User = {
    name: "Alice",
    age: 25,
    isAdmin: false
};
```

We can also type an object directly without creating a reusable type:

```typescript
const user: { name: string; age: number, isAdmin: boolean } = {
    name: "Alice",
    age: 25,
    isAdmin: false
};
```

+ `interface` is mainly for object shapes and classes.
+ `type` is more flexible. It can describe objects, unions, primitives, tuples, and more.
+ `interface` can be extended very naturally.
+ `interface` can be declared more than once and `TypeScript` will merge the declarations.

When using `type` or `interface` with functions:

```typescript
// With type
type AddFunction = (a: number, b: number) => number;

const add: AddFunction = (a, b) => {
    return a + b;
};

// With interface
interface AddFunction {
    (a: number, b: number): number;
}

const add: AddFunction = (a, b) => {
    return a + b;
};
```
