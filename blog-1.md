# `any` vs `unknown` in TypeScript: Why One is a Trap

`any` is Typescript's escape hatch — and most developers reaches for it the moment a type error feels hard to fix. What they do not realize is that `any` does not solve the problem. it just silences the compiler and pushes the errors to runtime.

```ts
let data: any = "hello";
data.toFixed();
```

TypeScript stops tracking the value entirely. Every variable that touches it loses types information too, queitly spreading the damages.

`unknown` is the best alternative of `any`. It also represents a value whose type is not known, but TypeScript will not let you to use it until you prove what it actually is:

```ts
let data: unknown = "hello";

if (typeof data === "string") {
  console.log(data.toUpperCase());
}
``

This is called **type narrowing** — you check it first, then act. It will work well for API responses, parsed JSON, or any value which is coming from outside your control.
```
