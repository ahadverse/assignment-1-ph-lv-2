# How Pick and Omit Reduce Code Duplication in TypeScript

Last week I had review a PR where someone had three nearly identical interfaces — `Company`, `CompanyResponse`, and `CompanyPublic` — all defined separately. The only real difference was that `CompanyPublic` was missing `apiKey` and `CompanyResponse` was missing `createdAt`. Any time the base `Company` changed, you had to remember to update all three. Of course nobody did, and the types had quietly drifted out of sync.

This is exactly the problems `Pick` and `Omit` were made for.

## The Setup

Say you've got a `Company` interfaces:

```ts
interface Company {
  id: number;
  name: string;
  email: string;
  apiKey: string;
  createdAt: Date;
}
```

You need a version for API responses that strips the API key. Without utility types, you copy the interface and delete the field. Works fine — until you add `industry: string` to `Company` six weeks later and forget you have three other interfaces doing the same job.

## Pick vs Omit

They do opposites things. `Pick` creates a type from a whitelist of fields:

```ts
type CompanyPreview = Pick<Company, "id" | "name">;
```

`Omit` creates a type by blacklisting:

```ts
type PublicCompany = Omit<Company, "apiKey">;
```

My rule of thumb: if you're keeping most of the fields, use `Omit`. If you're grabbing just a few specific ones, use `Pick`. Either way, both types are stay in sync with `Company` automatically — add a field to `Company` and `PublicCompany` gets it for free forever. Remove one and TypeScript tells you immedaitely.

## Why Its Matters

The real benefit isn't just less typing. Its that your types stop being lies. When you manually duplicate an interface, you're betting that every future developer — including yourself at 11pm — will remember to update all the copies.
