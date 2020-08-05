# eslint-plugin-void-only-side-effects

The void operator can be used to ensure side effects don't leak return values.

```
const mutationCallback = value => void (object.field = value);
```

The void operator can also be abused to write expressions that are nothing more than `undefined` with extra steps.

```
const isUndefined = value => value === void 0;
```

This plugin enables the rule `"void-only-side-effects"` which disallows the latter use.

Code can be automatically fixed with `--fix`. The above example would be fixed to

```
const isUndefined = value => value === undefined;
```

If your code editor supports eslint suggestions, the rule will allow you to apply the fix through a suggestion.

## Usage

In `.eslintrc`

```
plugins: [
  "plugin:void-only-side-effects/recommended"
]
```

If that doesn't work on its own, add

```
rules: [
  "void-only-side-effects/void-only-side-effects": {severity}
]
```

Where severity is "error" or "warn".

## Default

By default the void operator is only allowed to be used with arguments that could potentially have side effects.

### Examples of correct code for this rule

```
void functionCall();

void (object.field = value);
```

### Examples of incorrect code for this rule

```
void 0;

void () => functionCall();
```

## allowTraps

The rule can be configured to allow arguments that may potentially trigger traps.

To allow all traps configure `"allowTraps": true`.

```
rules: [
  "void-only-side-effects/void-only-side-effects": [{severity}, { "allowTraps": true }]
]
```

To allow select traps configure `"allowTraps": {object}`.

```
rules: [
  "void-only-side-effects/void-only-side-effects": [
    {severity},
    {
      "allowTraps": {
        "get": {boolean},
        "has": {boolean},
        "ownKeys": {boolean}
      }
    }
  ]
]
```

You can omit false values from the configuration object.

### Examples of correct code for this option

```
void object.field; // get

void 'field' in object; // has

void { ...object }; // ownKeys
```
