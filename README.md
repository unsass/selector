# Selector

[![Version](https://flat.badgen.net/npm/v/@unsass/selector)](https://www.npmjs.com/package/@unsass/selector)
[![Downloads](https://flat.badgen.net/npm/dt/@unsass/selector)](https://www.npmjs.com/package/@unsass/selector)
[![License](https://flat.badgen.net/npm/license/@unsass/selector)](https://www.npmjs.com/package/@unsass/selector)

## Introduction

Sass functions and mixins to manage CSS selectors.

## Installing

```shell
npm install @unsass/selector
```

## Usage

```scss
@use "@unsass/selector";

@include selector.create("foo", "md") {
    color: darkcyan;
}
```

### Result

```css
.md\:foo {
    color: darkcyan;
}
```

## API

| Mixin                                                                                   | Description                                                |
|-----------------------------------------------------------------------------------------|------------------------------------------------------------|
| `create($selector, $scope, $separator, $suffix, $pseudo-class, $pseudo-element, $root)` | Sets new CSS selector with class scope and pseudo options. |

### `$separator`

Define your own scope separator character.

```scss
@use "@unsass/selector";

@include selector.create("foo", "md", "@") {
    color: darkcyan;
}
```

### Result

```css
.md\@foo {
    color: darkcyan;
}
```

### `$suffix`

Define the scope value has a prefix on selector.

```scss
@use "@unsass/selector";

@include selector.create("foo", "md", $suffix: true) {
    color: darkcyan;
}
```

### Result

```css
.foo\:md {
    color: darkcyan;
}
```

### `$pseudo-class`

Define the pseudo class suffix.

```scss
@use "@unsass/selector";

@include selector.create("foo", "hover", $pseudo-class: "hover") {
    color: darkcyan;
}
```

### Result

```css
.hover\:foo:hover {
    color: darkcyan;
}
```

### `$pseudo-element`

Define the pseudo element suffix.

```scss
@use "@unsass/selector";

@include selector.create("foo", "before", $pseudo-element: "before") {
    color: darkcyan;
}
```

### Result

```css
.before\:foo::before {
    color: darkcyan;
}
```

### `$root`

Wrap the selector with `@at-root` rule before code output.

```scss
@use "@unsass/selector";

.foo {
    .bar {
        @include selector.create(&, "md", $root: true) {
            color: darkcyan;
        }
    }
}
```

### Result

```css
.md\:foo .bar {
    color: darkcyan;
}
```
