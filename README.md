# scss-codemods

This project uses postcss to refactor scss code to conform to lint rules that are intended to improve grepability/readability.

## Installation

### Globally via `npm`

```sh
npm i -g scss-codemods
```

### Running on-demand

```
npx scss-codemods [command] [options]
```

## `union-class-name`

"Promotes" CSS classes that have the `&-` nesting union selector. Attempts to fix issues flagged by [scss/no-union-class-name](https://github.com/kristerkari/stylelint-scss/blob/master/src/rules/selector-no-union-class-name/README.md) stylelint rule.

e.g.

```scss
.rule {
  &-suffix {
    color: blue;
  }
}

// becomes

.rule-suffix {
  color: blue;
}
```

Intended to improve "grepability" of the selectors that are produced in the browser.

### Usage

```bash
scss-codemods union-class-name --reorder never <files>
```

### Options

#### `--reorder`

Determines the freedom provided to the codemod to reorder rules to better match the desired format (default: `never`).


**Values:**
- `never`: won't promote rules if it would result in the reordering of selectors.
- `safe-only`: will promote rules that result in the reordering of selectors as long as the reordered selectors don't have the same specificity.
- `allow-unsafe`: will promote rules regardless of reordering, could result in differing styles if both classes are used on the same element and there's conflicting properties.

*Recommended that you test the resulting css thoroughly, especially with the `allow-unsafe` option.*

#### `--promote-dollar-vars`

Determines how dollar vars will be promoted (default: `global`). **not implemented**

**Values:**
- `no-global`: won't promote a dollar var to global context, prevents some rules from being promoted.
- `global`: allows promoting dollar vars to global

#### Not Implemented: `--namespace-dollar-vars`

Determines how dollar var promotion namespacing behavior (default: `never`).

- `never`: will promote a dollar var as necessary, will fail if there are duplicate vars at the same level as the promoted var.
- `when-necessary`: will namespace a dollar var with the classname it was promoted out of, will still fail if there is a duplicate promoted var.
- `always`: will always namespace dollar vars with the classname
