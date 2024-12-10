
# Comprehensive Guide to TypeScript Types

## 1. Primitive Types

There are `4` ways we can assign or let Typscript `infer` the types in case of `Primitive types`.
We can take an example of `number` type for this like below,
```typescript
let a = 1234 // number
var b = Infinity * 0.10 // number
const c = 5678 // 5678
let d = a < b // boolean
let e: number = 100 // number
let f: 26.218 = 26.218 // 26.218
let g: 26.218 = 10 // Error TS2322: Type '10' is not assignable
 // to type '26.218'.
```
1. You can let TypeScript infer that your value is a number (a and b).
2. You can use const so TypeScript infers that your value is a specific number (c).
3. You can tell TypeScript explicitly that your value is a number (e).
4. You can tell TypeScript explicitly that your value is a specific number (f and g).

this is held true for `primitive types` like  `string`,`boolean`,`bifgint`.


### a. `string`
Represents textual data enclosed in single or double quotes.
```typescript
let greeting: string = "Hello, World!";
console.log(greeting.toUpperCase()); // Outputs: "HELLO, WORLD!"
```

### b. `number`
Supports integers, floats, and hexadecimal, binary, or octal numbers.
```typescript
let decimal: number = 10;
let hex: number = 0xff;
let binary: number = 0b1010;
let octal: number = 0o744;
console.log(decimal, hex, binary, octal); // 10, 255, 10, 484
```

### c. `boolean`
Only `true` or `false`.
```typescript
let isLoggedIn: boolean = true;
if (isLoggedIn) {
  console.log("Welcome back!");
}
```

### d. `null` and `undefined`
Both `null` and `undefined` are subtypes of all other types.
```typescript
let u: undefined = undefined;
let n: null = null;
let value: string | null = null;
value = "Now assigned";
```

### e. `bigint`
Used for large integers beyond `Number.MAX_SAFE_INTEGER`.
```typescript
let large: bigint = 123456789012345678901234567890n;
console.log(large * 2n);
```

### f. `symbol`
Creates unique values, often used for object keys.
```typescript
const key1 = Symbol("key");
const key2 = Symbol("key");
console.log(key1 === key2); // false
```

---

## 2. Any Type
Allows any value, disabling TypeScript's type-checking.
```typescript
let data: any = 10;
data = "string";
data = true; // Valid but unsafe
```

---

## 3. Unknown Type
A safer alternative to `any`; you must check the type explicitly.
```typescript
let input: unknown = "Hello";
if (typeof input === "string") {
  console.log(input.toUpperCase()); // Valid
}
```

---

## 4. Void Type
Used for functions that don't return a value.
```typescript
function logMessage(msg: string): void {
  console.log(msg);
}
logMessage("Logging...");
```

---

## 5. Never Type
Represents values that never occur (e.g., infinite loops or errors).
```typescript
function throwError(msg: string): never {
  throw new Error(msg);
}

function processValue(value: string | number): never | string {
  if (typeof value === "string") return value.toUpperCase();
  throwError("Unsupported type");
}
```

---

## 6. Array Type
Represents collections of similar types.
```typescript
let numbers: number[] = [1, 2, 3];
let strings: Array<string> = ["a", "b", "c"];
numbers.push(4); // Valid
```

---

## 7. Tuple Type
Fixed-length arrays with specific types.
```typescript
let user: [string, number, boolean] = ["Alice", 30, true];
console.log(user[0].toUpperCase()); // Outputs: ALICE
```

---

## 8. Object Type
Represents objects with defined property types.
```typescript
let person: { name: string; age: number; isAdmin: boolean } = {
  name: "John",
  age: 25,
  isAdmin: false,
};
console.log(person.name);
```

`Note` : Do not use `Object` as an type of your object. It's not good practice as it will not allow you to add, remove, access or orverride any value in the
object.
Example :
 ```typescript
let person:Object = {
  name: "John",
  age: 25,
  isAdmin: false,
};
console.log(person.name); //Error: Property 'name' does not exist on type 'Object'.
person.name = "op"; //Error
delete person.age;  //Error
person.address = "something"; //Error
```

Now if we try to add new property to the person object typescript will complain about it.
```typescript
person.add="JD town" //Error : Property 'add' does not exist on type '{ name: string; age: number; isAdmin: boolean; }
```

We have to strickly follow the `blueprint` or `type` of the object. Also if we want to assign a new object value to the person object then
that object should follow the `blueprint` or `type` of the object as well. `Note` that `additional` properties in a new object are `allowed`
but `ommitting` properties that are in the `blueprint` or the `type` of original object is `not allowed`.

```typescript
let person: { name: string; age: number; isAdmin: boolean } = {
  name: "John",
  age: 25,
  isAdmin: false,
};

let newObj = {
    name:"sam",
    age:45,
    //age:45, // ommiting age property Wrong ! Error :Property 'age' is missing in type
    isAdmin:true, 
    add:"JD town" //Addition properties are allowed.
}

person = newObj;
console.log(person) //output ==> {"name": "sam","age": 45,"isAdmin": true,"add": "JD town"} 
```

Also If you ever want to use `delet` i.e., to remove a property from your object you have to make it `optional` in the object type itself it is true for
objects delclared with the `const` keyword.
like below,
 ```typescript
let person:{   
    name:string,
    age?:number,  // <== making this property optional so that we can delete it in future.
    //age:number <== this is invalid if want to use delete on this property.
    isAdmin:boolean
} = {
  name: "John",
  age: 25,
  isAdmin: false,
};

delete person.age;
console.log(person); // output ==> {"name": "John","isAdmin": false} 

```



---

## 9. Union Type
Defines a variable that can hold multiple types.
```typescript
let id: number | string;
id = 101; // Valid
id = "ID102"; // Also valid
```

---

## 10. Intersection Type
Combines properties of multiple types into one.
```typescript
type User = { name: string };
type Admin = { isAdmin: boolean };
type AdminUser = User & Admin;

let admin: AdminUser = { name: "Alice", isAdmin: true };
```

---

## 11. Literal Type
Restricts a variable to specific values.
```typescript
let direction: "up" | "down" | "left" | "right";
direction = "up"; // Valid
// direction = "forward"; // Error
```

---

## 12. Enum Type
Defines a set of named constants.
```typescript
enum Color {
  Red,
  Green,
  Blue,
}
let favoriteColor: Color = Color.Red;
console.log(favoriteColor); // Outputs: 0
```

---

## 13. Function Type
Defines function structure (parameters and return type).
```typescript
let multiply: (a: number, b: number) => number;
multiply = (x, y) => x * y;
console.log(multiply(2, 3));
```

---

## 14. Type Aliases
Used for creating reusable type definitions.
```typescript
type Point = { x: number; y: number };
let p: Point = { x: 1, y: 2 };
```

---

## 15. Interfaces
Define contracts for object structures or classes.
```typescript
interface Vehicle {
  make: string;
  model: string;
}
let car: Vehicle = { make: "Toyota", model: "Corolla" };
```

---

## 16. Optional and Readonly Properties

### a. Optional
```typescript
interface User {
  name: string;
  age?: number;
}
let user: User = { name: "Alice" };
```

### b. Readonly
```typescript
interface Point {
  readonly x: number;
}
let p: Point = { x: 10 };
// p.x = 20; // Error
```

---

## 17. Type Assertions
Explicitly specify the type of a variable.
```typescript
let someValue: any = "Hello";
let strLength: number = (someValue as string).length;
```

---

## 18. Indexed Access Types
Access types of object properties dynamically.
```typescript
type Person = { name: string; age: number };
type AgeType = Person["age"]; // number
```

---

## 19. Mapped Types
Transform all properties of a type.
```typescript
type ReadOnly<T> = {
  readonly [K in keyof T]: T[K];
};
type User = { name: string; age: number };
type ReadOnlyUser = ReadOnly<User>;
```

---

## 20. Conditional Types
Perform type logic based on conditions.
```typescript
type IsString<T> = T extends string ? true : false;
type Test1 = IsString<string>; // true
type Test2 = IsString<number>; // false
```

---

## 21. Utility Types
Predefined types for common patterns.
```typescript
type User = { name: string; age: number };
type PartialUser = Partial<User>;
let user: PartialUser = { name: "John" }; // `age` is optional
```

---

## 22. Keyof Operator
Extract keys of a type as a union.
```typescript
type User = { name: string; age: number };
type UserKeys = keyof User; // "name" | "age"
```

---

## 23. Infer Keyword
Infers types in conditional types.
```typescript
type ReturnType<T> = T extends (...args: any[]) => infer R ? R : never;
function greet(): string {
  return "Hello!";
}
type GreetReturnType = ReturnType<typeof greet>; // string
```
