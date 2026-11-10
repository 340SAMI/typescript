# Type Narrowing in TypeScript

## Introduction

Variables can hold many types of values. For example, an API might provide a date as either a string or a `Date` object. TypeScript represents this possibility with a union type, but a union alone does not tell us which value we have at a particular moment. Type narrowing is the process of checking a value so TypeScript can safely treat it as a more specific type.

## Detailed Explanation

The `typeof` operator is useful for primitive values such as strings and numbers. After the check, TypeScript allows operations that belong to that type:

```ts
function describe(value: string | number): string {
  if (typeof value === "string") {
    return value.trim();
  }
  return value.toFixed(1);
}
```

For objects created from classes, `instanceof` checks which class constructed a value. This example handles either a date object or its string representation:

```ts
function yearOf(value: Date | string): number {
  const date = value instanceof Date ? value : new Date(value);
  return date.getFullYear();
}
```

The `in` operator checks whether an object has a particular property. It is useful when union members have different property names:

```ts
type Cat = { meow: () => void };
type Dog = { bark: () => void };

function makeSound(pet: Cat | Dog): void {
  if ("meow" in pet) {
    pet.meow();
  } else {
    pet.bark();
  }
}
```

Each check provides evidence about the current value, so TypeScript narrows the union inside that branch. Choosing a check that matches the value's shape makes the code easier to understand as well as safer.

## Conclusion

Union types let a function accept flexible inputs, while narrowing lets its implementation handle each possibility correctly. `typeof`, `instanceof`, and `in` cover common checks for primitives, class instances, and object properties. Together, they help catch invalid operations before the program runs without giving up the convenience of union types.
