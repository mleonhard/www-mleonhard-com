# TypeScript WTFs

## No Immutable Params
Function params are mutable by default and cannot be declared immutable.
<https://github.com/microsoft/TypeScript/issues/18497>
```typescript
function f(const x) {} // Compiler error.
function f(x) {
  const x = x; // Workaround.
}
```

## Confusing Optional Syntax
Optional syntax is on the variable, not the type.
```typescript
function f(x: bool? = false) {} // Compiler error.
function f(x?: bool = false) {} // Accepted.
```

## Implicit Cast to Any
Function calls implicitly cast parameters to `any`.
```typescript
export function hasFoo(value: any): value is { foo: string } {
    return "foo" in value;
}

const value: unknown = getValue();
if (hasFoo(value)) { // Silently changes `value` from `unknown` to `any`.
    console.log(t.foo);
}

if (hasFoo(value as any)) { // This is more clear.
    console.log(t.foo);
}
```

I suppose this is so codebases can forbid `as`, which is much more risky than implicit casts to `any`.

