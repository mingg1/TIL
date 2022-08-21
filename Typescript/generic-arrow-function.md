# Generic arrow function

reference : https://www.carlrippon.com/generic-arrow-functions/

When I tried to make generic arrow functions, I encountered this kind of error.

```sh
const withCountry = <T>(country: string) => {
                     ^
return (WrappedComponent: ComponentType<T>) => {
return (props: Omit<T, 'country' | 'error'>) => {}}}

Cannot find name 'T'
```

If we take a look at the transpiled code, we could figure out what the problem is.

```sh
const withCountry = React.createElement(T, null, "(country: string) => {
return (WrappedComponent: ComponentType<T>) => {
return (props: Omit<T, 'country' | 'error'>) => {}}}")
```

Well, the angle brackets are considered as a JSX element.

## Solutions

1. Extends trick
2. Comma trick
3. Just using function keyword

### Extends trick

```sh
const withCountry = <T extends unknown>(country: string) =>  { ... }
```

every type is assignable to `unknown` type, which is type-safe compared to the `any` type.

### Comma trick

```sh
const withCountry = <T,>(country: string) => { ... }
```

It is a syntax for defining multiple parameters. This will not cause any errors. In the transpilation process, these angle brackets are treated as ones for generic parameters.

### Using function keyword

```sh
function withCountry<T>(country: string){ ... }
```

The simplest solution obviously :))
