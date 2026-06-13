# Selector

[![Version](https://flat.badgen.net/npm/v/@unsass/selector)](https://www.npmjs.com/package/@unsass/selector)
[![Downloads](https://flat.badgen.net/npm/dt/@unsass/selector)](https://www.npmjs.com/package/@unsass/selector)
[![License](https://flat.badgen.net/npm/license/@unsass/selector)](https://www.npmjs.com/package/@unsass/selector)

## Introduction

A small Sass toolkit for building CSS selectors programmatically. Compose scoped, prefixed or suffixed class names with
pseudo-classes, pseudo-elements and `@at-root` support through concise, composable mixins and helpers, so selector logic
stays readable and consistent.

<div align="center">

![Selector](.github/banner.svg)

</div>

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

```css
.md\:foo {
    color: darkcyan;
}
```

## Mixins

### `create($selector, $scope, $separator, $suffix, $pseudo-class, $pseudo-element, $root)`

Builds a class selector from a name, with optional scope, separator, pseudo and `@at-root` options.

| Parameter         | Description                                                | Default |
|-------------------|------------------------------------------------------------|---------|
| `$selector`       | Selector name (a leading `.` is stripped).                 | —       |
| `$scope`          | Scope affix value, as a single token or a list of tokens.  | `null`  |
| `$separator`      | Scope affix separator.                                     | `":"`   |
| `$suffix`         | Append the scope as a suffix instead of a prefix.          | `false` |
| `$pseudo-class`   | Pseudo-class appended to the selector.                     | `null`  |
| `$pseudo-element` | Pseudo-element appended to the selector.                   | `null`  |
| `$root`           | Wrap the output in an `@at-root` rule.                     | `false` |

```scss
@use "@unsass/selector";

@include selector.create("foo", "md") {
    color: darkcyan;
}
```

```css
.md\:foo {
    color: darkcyan;
}
```

#### `$scope`

Pass a list to chain several scope tokens.

```scss
@use "@unsass/selector";

@include selector.create("foo", ("md", "lg")) {
    color: darkcyan;
}
```

```css
.md\:lg\:foo {
    color: darkcyan;
}
```

#### `$separator`

Define your own scope separator character.

```scss
@use "@unsass/selector";

@include selector.create("foo", "md", "@") {
    color: darkcyan;
}
```

```css
.md\@foo {
    color: darkcyan;
}
```

#### `$suffix`

Append the scope value as a suffix on the selector.

```scss
@use "@unsass/selector";

@include selector.create("foo", "md", $suffix: true) {
    color: darkcyan;
}
```

```css
.foo\:md {
    color: darkcyan;
}
```

#### `$pseudo-class`

Define the pseudo-class suffix.

```scss
@use "@unsass/selector";

@include selector.create("foo", "hover", $pseudo-class: "hover") {
    color: darkcyan;
}
```

```css
.hover\:foo:hover {
    color: darkcyan;
}
```

#### `$pseudo-element`

Define the pseudo-element suffix.

```scss
@use "@unsass/selector";

@include selector.create("foo", "before", $pseudo-element: "before") {
    color: darkcyan;
}
```

```css
.before\:foo::before {
    color: darkcyan;
}
```

#### `$root`

Wrap the selector with an `@at-root` rule before code output.

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

```css
.md\:foo .bar {
    color: darkcyan;
}
```

### `media($query)`

Wraps the content in a `@media` rule.

```scss
@use "@unsass/selector";

.foo {
    @include selector.media("screen") {
        color: darkcyan;
    }
}
```

```css
@media screen {
    .foo {
        color: darkcyan;
    }
}
```

## Functions

### `to-class($name)`

Returns a class selector from a name.

```scss
@use "@unsass/selector";

$selector: selector.to-class("foo"); // ".foo"
```

### `to-id($name)`

Returns an id selector from a name.

```scss
@use "@unsass/selector";

$selector: selector.to-id("foo"); // "#foo"
```

### `pseudo-class($selector, $pseudo-class)`

Appends a pseudo-class to a selector.

```scss
@use "@unsass/selector";

$selector: selector.pseudo-class(".foo", "hover"); // ".foo:hover"
```

### `pseudo-element($selector, $pseudo-element)`

Appends a pseudo-element to a selector.

```scss
@use "@unsass/selector";

$selector: selector.pseudo-element(".foo", "before"); // ".foo::before"
```

### `is($selector, $args…)`

Restricts the selector with an `:is()` pseudo-class matching any of the given selectors.

```scss
@use "@unsass/selector";

$selector: selector.is("article", "h1", "h2"); // "article:is(h1, h2)"
```

### `where($selector, $args…)`

Like `is()`, but with zero specificity through the `:where()` pseudo-class.

```scss
@use "@unsass/selector";

$selector: selector.where(".prose", "a", "button"); // ".prose:where(a, button)"
```

### `has($selector, $args…)`

Restricts the selector to elements matching the relational `:has()` condition.

```scss
@use "@unsass/selector";

$selector: selector.has(".card", "> img"); // ".card:has(> img)"
```

### `exclude($selector, $args…)`

Excludes the given selectors through a `:not()` pseudo-class. Named `exclude` because `not` is a reserved Sass keyword.

```scss
@use "@unsass/selector";

$selector: selector.exclude(".btn", ".btn--disabled"); // ".btn:not(.btn--disabled)"
$selector: selector.exclude(".item", ".a", ".b"); // ".item:not(.a, .b)"
```

These helpers return selectors, so they compose:

```scss
@use "@unsass/selector";

#{selector.exclude(selector.is(".x", "a", "b"), ".y")} {
    color: darkcyan;
}
```

```css
.x:is(a, b):not(.y) {
    color: darkcyan;
}
```
