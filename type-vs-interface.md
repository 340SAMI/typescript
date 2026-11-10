# Type Aliases vs. Interfaces

## Introduction

When describing data in TypeScript, both `type` aliases and `interface` declarations can give a name to an object shape. That overlap can make it unclear which one to choose. In everyday object-oriented code, either can describe the fields an object should have. The differences become useful when a model needs to be extended or when a type is more than an object.

## Detailed Explanation

An interface describes an object contract and can be extended by another interface. Interfaces with the same name can also merge their declarations, which can be useful when intentionally extending a library's types:

```ts
interface Account {
  email: string;
}

interface AdminAccount extends Account {
  permissions: string[];
}
```

A type alias can name an object too, but it can also express unions, tuples, and combinations of existing types. For instance, a value can be either a bank transfer or a card payment:

```ts
type Payment =
  | { kind: "transfer"; accountNumber: string }
  | { kind: "card"; lastFourDigits: string };

function describePayment(payment: Payment): string {
  return payment.kind === "transfer"
    ? `Transfer to ${payment.accountNumber}`
    : `Card ending in ${payment.lastFourDigits}`;
}
```

Both constructs use structural typing: an object is compatible when it has the required members, regardless of its declared name. Interfaces can extend other interfaces, and type aliases can combine shapes with intersections, but aliases cannot be reopened through declaration merging. Neither is universally better; the choice depends on what the type needs to express.

## Conclusion

Use an interface when defining an object-shaped contract that may be extended or deliberately augmented. Use a type alias when you need a union, tuple, or another type expression, or when that form makes the code clearer. For a simple object shape, consistency with the surrounding project is often the best guide.
