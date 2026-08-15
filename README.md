<p align="center">
    <img src="logo.svg" width="400" height="200">
</p>

CSS generator written in Plume: rules, typed units and custom properties, serialized to CSS text.

## Installation

```
quill add css
```

## Usage

```plume
use css

let rule = @Rule(.card)
    padding: $unit(2, rem)
    color: $var(main)
end

set rule.padding *= 2

$ String(rule)
```

```css
.card {padding: 4rem;color: var(--main);}
```

### `Rule(selector, ...props)`

Builds a `selector { ... }` block from a props table:

- string keys → CSS properties, names converted from `camelCase` to `kebab-case`
- numeric keys → nested rules, inserted as-is

### `unit(value, unitName)`

A number bound to its unit, with arithmetic (`+ - * /`):

```plume
$ unit(2, "rem") + unit(1, "rem")   // 3rem
$ unit(1, "px") + unit(0.5, "rem")  // calc(1px + 0.5rem)
```

Same unit → computed value; mixed units → wrapped in `calc()`.

### `var(name)`

A CSS custom property, emitted as `--name`. Used as a property value, it references the declared variable.

### Table → CSS

The `css` macro (used by `Rule`) serializes the props table to text: each value is rendered through its `tostring` (`unit`, `var`, ...). The whole rule is a CSS string.
