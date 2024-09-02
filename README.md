# SCSS Fluid Calculator

A slightly modified SCSS utility for calculating fluid typography and spacing based on the Utopia Core SCSS calculator [Utopia.fyi](https://utopia.fyi). The documentation **only** highlights the customizations of the original package! For detailed information, check out the original repository on [GitHub](https://github.com/trys/utopia-core-scss/) or [NPM](https://www.npmjs.com/package/utopia-core-scss).

## Installation

```bash
npm @webshr/scss-fluid-calculator
```

```scss
@use 'node_modules/@webshr/fluid-calculator' as calculator;
```

## Documentation

The fluid typography calculator outputs in two formats: mixins & functions. Mixins generate actual (slightly) opinionated CSS custom properties. Functions sit beneath mixins doing the real work, returning calculated values for you to write your own custom CSS. The both use identical APIs and only differ by name (`generateX` for mixins, `calculateX` for functions).

All mixins/functions default `relativeTo` to `viewport` (`vi`), but can be overriden to `container` (`cqi`), or `viewport-width` (`vw`).

## Mixins

### `generateFluidScale()`

Generate a fluid scale between two widths, sizes and scales. Set the number of positive and negative steps. Custom property `prefix` can be overriden from `step-`.

#### Example

```scss
:root {
  // Calling this:
  @include calculator.generateFluidScale((
    "minWidth": 320,
    "maxWidth": 1240,
    "minSize": 18,
    "maxSize": 20,
    "minScale": 1.2,
    "maxScale": 1.25,
    "positiveSteps": 2,
    "negativeSteps": 2,
    /* Optional params */
    "relativeTo": "viewport",
    "prefix": "step-",
  ));

  // Generates this:
  --step-xs: clamp(...);
  --step-s: clamp(...);
  --step-m: clamp(...);
  --step-l: clamp(...);
  --step-xl: clamp(...);
}
```

### `generateClamp()`

Generate a single clamp custom property from a min/max width & size. Default to using `rem` and `vi` but this can be overriden to use `px` and `cqi`. Pass `usePx: true` to separate space from browser text zoom preferences. Custom property `prefix` can be overriden from `space`.

#### Example

```scss
:root {
  // Calling this:
  @include calculator.generateClamp((
    "minWidth": 320,
    "maxWidth": 1240,
    "minSize": 16,
    "maxSize": 20,
    /* Optional params */
    "usePx": true,
    "relativeTo": "container",
    "prefix": "box-spacing",
  ))

  // Generates this:
  --box-spacing: clamp(...);
}
```

## Functions

### `calculateScale()`

Calculate a fluid scale between two widths, sizes and scales. Set the number of positive and negative steps, and whether you want the scale to be relative to the `viewport` or the `container`.

#### Example

```scss
$typeScale: calculator.calculateScale((
  "minWidth": 320,
  "maxWidth": 1240,
  "minSize": 18,
  "maxSize": 20,
  "minScale": 1.2,
  "maxScale": 1.25,
  "positiveSteps": 5,
  "negativeSteps": 2,
  /* Optional params */
  "relativeTo": "container",
))

// $typeScale == (
//    (
//      "step": 0,
//      "minFontSize": 18,
//      "maxFontSize": 20,
//      "clamp": clamp(...),
//    )
//  )
```

### `calculateClamps()`

Calculate multiple clamps from a single set of min/max widths. Supply an array of number pairs to interpolate between.

#### Example

```scss
$clamps: calculator.calculateClamps((
  "minWidth": 320,
  "maxWidth": 1240,
  "pairs": (
    (16, 48),
    (40, 18)
  ),
  /* Optional params */
  "usePx": true,
  "relativeTo": "container"
));

// $clamps == (
//    (
//      "label": "16-48",
//      "clamp": clamp(...),
//    ),
//    (
//      "label": "40-16",
//      "clamp": clamp(...),
//    )
//  )
```

### `calculateClamp()`

Calculate a single clamp calculation from a min/max width & size. Default to using `rem` and `vi` but this can be overriden to use `px` and `cqi`.

#### Example

```scss
$clamp: calculator.calculateClamp((
  "minWidth": 320,
  "maxWidth": 1240,
  "minSize": 16,
  "maxSize": 48,
  /* Optional params */
  "usePx": true,
  "relativeTo": "container"
));

// clamp(1rem, 0.3043rem + 3.4783vi, 3rem);
```