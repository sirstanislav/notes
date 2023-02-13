# TS

JavaScript has 8 types:

- undefined
- null
- boolean
- string
- number
- BigInt
- symbol
- object

All types are dynamic and we can use they and modify when code is running.

TypeScript adds new statics types and type can’t be changed.

```tsx
let str: string;
str = "abc"; //Correct
str = 123; //Error
```

TS doesn’t read the variable before we don’t put the value. TS can automatic understand the type of variable without annotation.

```tsx
let str = "abc";
str = 123; //Error
```

The type can have several annotation:

```tsx
let score: number | string = 10; //union of types
```

Using aliases:

```tsx
type Score = number | string;

const myScore: Score = "10";
```

## Array

```tsx
const arr: string[] = ["a", "b", "c"];
arr.push(10); //error
```

or

```tsx
const arr: string[] = [];
arr.push("example");
```

or

```tsx
const arr: Array<string> = [];
```

Array inside array

```tsx
const arr: string[][] = [];
arr.push(["a", "b", "c"]);
arr.push([1, 2, 3]); //error
```

With aliases

```tsx
type StrNum = string | number;
const arr: StrNum[] = [];
```

### Tuple || Ennupla

Fixed numbers of elements

```tsx
const tuple: [string, boolean, number] = ["abc", true, 0];
```

It's needs for example for cvs files

```cvs
"Nome","Cognome","Età"
"Mario","Rossi",20
"Luigi","Bianchi",51
"Clara","Esposito",18
"Gennaro","Fumagalli",35
"Francesco, Maria","Cossu",37
```

```tsx
type CVS = [string, string, number];

const cvs: CVS[] = [
  ["Mario", "Rossi", 20],
  ["Francesco, Maria", "Cossu", 37],
  ["Luigi", "Bianchi", 51],
  ["Clara", "Esposito", 18],
  ["Gennaro", "Fumagalli", 35],
  ["Francesco, Maria", "Cossu", 37],
];
```

## Object

```tsx
type Obj = {
  a: number;
  b: number;
  c: string;
};

//or

interface Obj {
  readonly a: number;
  b: number;
  c?: string; // simbolo interagativo significa may be, may not be
  print?: () => number;
  [key: string]: string | number; //allows to have more no fixed keys
}

const obj: Obj = {
  a: 1,
  b: 3,
  c: "abc",
  d: "string",
  f: 10,
};
```

Merge interfaces

```tsx
interface Person {
  name: string;
}

interface Person {
  age: number;
}

const obj: Person = {
  name: "Stas",
  age: 40,
};

interface Account {
  email: string;
  login: string;
  active: boolean;
}

interface Developer extends Person, Account {
  skills: string[];
  level?: string;
}

const user: Developer = {
  name: "Stas",
  age: 40,
  email: "",
  login: "",
  active: false,
  skills: ["JavaScript", "TypeScript"],
};
```

Merge types

```tsx
type Person = {
  age: number;
};

type Account = {
  email: string;
  login: string;
  active: boolean;
};

type Developer = {
  skills: string[];
  level?: string;
};

type FrontDev = Person & Account & Developer;

const developer: FrontDev[] = [];
```

### Function

```tsx
const fun = (num: number): string => {
  return String(num); //from number to string
};
```

```tsx
function createPoint(x = 0, y = 0): [number, number] {
  return [x, y];
}
```

```tsx
function fun(...num: number[]): string {
  /*from unknown amount of numbers
  to string*/
  return num.join("-");
}
console.log(fun(2, 3, 4));
```

```tsx
interface Printable {
  label: string;
}

function printReport(obj: Printable): void {
  console.log(obj.label);
}

const drink = {
  label: "pepsi",
  price: 2,
};

const phone = {
  label: "iPhone",
  price: 1000,
};

printReport(drink); //ok
printReport(phone); //ok
printReport({ label: "", price: 100 }); //error
```

### Function Overloads

```tsx
// Overload signatures
function greet(person: string): string;
function greet(persons: string[]): string[];

// Implementation signature
function greet(person: unknown): unknown {
  if (typeof person === "string") {
    return `Hello, ${person}!`;
  } else if (Array.isArray(person)) {
    return person.map((name) => `Hello, ${name}!`);
  }
  throw new Error("Unable to greet");
}
```

## Genericss

```tsx
const valueFactory = (x: number) => x;
const myValue = valueFactory(11);

type TypeFactory<X> = X;
type MyType = TypeFactory<boolean>;
```

```tsx
interface ValueContainer<Value> {
  value: Value;
}

type StringContainer = ValueContainer<string>;

const x: StringContainer = {
  value: "abc", // ok
};
```

```tsx
// this two class equal
class ArrayOfNumbers {
  constructor(public colection: number[]) {}

  get(index: number): number {
    return this.colection[index];
  }
}

class ArrayOfString {
  constructor(public collection: string[]) {}

  get(index: number): string {
    return this.collection[index];
  }
}

// to the last one

class ArrayOfAnything<Type> {
  constructor(public collection: Type[]) {}

  get(index: number): Type {
    return this.collection[index];
  }
}

console.log(new ArrayOfAnything<string>(["1", "2", "3", "4"]));
console.log(new ArrayOfAnything<number>([1, 2, 3, 4]));
```

```tsx
// this two class equal
function printString(array: string[]): void {
  for (let index = 0; index < array.length; index++) {
    console.log(array[index]);
  }
}

function printNumber(array: number[]): void {
  for (let index = 0; index < array.length; index++) {
    console.log(array[index]);
  }
}
// to the last one

function printAnythin<Type>(array: Type[]): void {
  for (let index = 0; index < array.length; index++) {
    console.log(array[index]);
  }
}

printAnythin<number>([1, 2, 3]);
```

```tsx
function fillArray<Type>(len: number, elem: Type): Type[] {
  return new Array<Type>(len).fill(elem);
}

const newArr = fillArray<string>(10, "*");
```

```tsx
interface LengthWise {
  length: number;
}

function printLength<Type extends LengthWise>(arg: Type): number {
  return arg.length;
}

printLength({ a: 1, length: 1 });
```

```tsx
function getProperty<Type, Key extends keyof Type>(obj: Type, key: Key) {
  return obj[key];
}

const obj = {
  a: 1,
  b: 2,
  c: 3,
};

// K === 'a' | 'b' | 'c'

console.log(getProperty(obj, "c"));
```

## Classes
