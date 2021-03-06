# Effective Typescript Summary

My notes while reading "Effective TypeScript: 62 Specific Ways to Improve Your TypeScript" by [Dan Vanderkam][author].

You know, it is difficult to absorb all the information when reading a whole book from the first page to the last page. This time I decided to take note and also, test and the code while reading. I found it is effective to help me to understand much better than only lying on the bed and crush through the book. I will do it again in the future.

It only contains some basic concepts from the end of each chapter. If you want to learn more, I <u>highly recommend you to buy the book</u>.

- [Effective Typescript website][website] - Dan posted some parts of the book as the blog post. Very interesting to read if you can't afford the full book.
- [Code Sample][github]

If you are the publisher and think this repository should not be public, please write me an email to trungk18 [at] gmail [dot] com and I will make it private.

Happy reading!

[![Tweet](https://img.shields.io/twitter/url/http/shields.io.svg?style=social)][tweet]

[tweet]: https://twitter.com/intent/tweet?text=Effective%20TypeScript%20Book%20Summary&url=https://github.com/trungk18/typescript-data-structures&hashtags=typescript

## Abbreviation

| Abbr | Meaning    |
| ---- | ---------- |
| TS   | TypeScript |
| JS   | JavaScript |

## Chapter 1: Getting to Know TypeScript

### Item 1: Understand the Relationship Between TypeScript and JavaScript

- TS is a superset of JS. In other words, all JS programs are already TS programs. TS has some syntax of its own, so TS is not, in general, valid JS programs.

- TS adds a type system that models JS's runtime behavior and tries to spot code which will throw exception at runtime. But you shouldn't it to flag every exception. It is possible for code to pass the type checker but still throw error at runtime.

```typescript
const names = ["Alice", "Bob"];
console.log(names[2].toUpperCase());
```

- While TypeScript's type system largely models JS behavior, there are some constructs that JS allows but TypeScript chooses to bar, such as calling functions with wrong number of arguments. This is largely a matter of taste.

```typescript
const a = null + 7; // Evaluates to 7 in JS
// ~~~~ Operator '+' cannot be applied to types ...

const b = [] + 12; // Evaluates to '12' in JS
// ~~~~ Operator '+' cannot be applied to types ...

alert("Hello", "TypeScript"); // alerts "Hello"
// ~~~~ Expected 0-1 arguments, but got 2
```

### Item 2: Know Which TypeScript Options You’re Using

- The TS compiler includes several settings which affect core aspects of the language.
- Configure TS using `tsconfig.json` rather than command-line options.
- Turn on `noImplicitAny` unless you are transitioning a JS project to TS.
- Use `strictNullChecks` to prevent `undefined is not an object` - style runtime errors.
- Aim to enable `strict` to get the most thorough checking that TS can offer.

### Item 3: Understand That Code Generation Is Independent of Types

- Code generation is independent of the type system. This means that TS types cannot affect the runtime behavior or performance of your code.
- It is possible for a program with type errors to produce code ("compile")
- TS types are not available at runtime. To query a type at runtime, you need some way to reconstruct it. Tagged unions and property checking are common ways to do this. Some constructs, such as `class`, introduce both a TS type and a value that is available at runtime.

<details>
<summary><b>:x: Not OK - Using Interface instanceof</b></summary>

```typescript
interface Square {
  width: number;
}
interface Rectangle extends Square {
  height: number;
}
type Shape = Square | Rectangle;

function calculateArea(shape: Shape) {
  if (shape instanceof Rectangle) {
    // 'Rectangle' only refers to a type, but is being used as a value here
    return shape.width * shape.height;
    // Property 'height' does not exist on type 'Shape'
  } else {
    return shape.width * shape.width;
  }
}
```

</details>

<details>
<summary><b>:heavy_check_mark: OK - Using Class and instanceof </b></summary>

```typescript
class Square {
  constructor(public width: number) {}
}
class Rectangle extends Square {
  constructor(public width: number, public height: number) {
    super(width);
  }
}
type Shape = Square | Rectangle;

function calculateArea(shape: Shape) {
  if (shape instanceof Rectangle) {
    shape; // Type is Rectangle
    return shape.width * shape.height;
  } else {
    shape; // Type is Square
    return shape.width * shape.width; // OK
  }
}
```

</details>

View more code [Chapter 3 Code Example][chapter3]

### Item 4: Get Comfortable with Structural Typing

- JS is a duck typed and TS uses structural typing to model this.

  - Duck typed: If you pass a function a value with all the right properties, it won't care how you made the value.
  - Structural typing: values assignable to your interface might have properties beyond those explicitly listed in your type declarations. Type is not "sealed". [View code][chapter4-1]

- Be aware that classes also follow structural typing rules. You might not have an instance of the class you expect! [View code][chapter4-2]
- Use structural typing to facilitate unit testing. [View code][chapter4-3]

[website]: https://effectivetypescript.com/
[github]: https://github.com/danvk/effective-typescript
[author]: https://github.com/danvk
[chapter3]: https://github.com/danvk/effective-typescript/tree/master/samples/ch01-intro/item-03-independent
[chapter4-1]: https://github.com/danvk/effective-typescript/blob/master/samples/ch01-intro/item-04-structural/structural-04.ts
[chapter4-2]: https://github.com/danvk/effective-typescript/blob/master/samples/ch01-intro/item-04-structural/structural-10.ts
[chapter4-3]: https://github.com/danvk/effective-typescript/blob/master/samples/ch01-intro/item-04-structural/structural-14.ts
