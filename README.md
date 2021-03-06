# svelte-validator

## Install

This is WIP project. Use at your own responsibility!

```
npm i -S svelte-validator
```

## Usage

```html
<script>
  import createValidator, { required, minLength, hasError } from 'svelte-validator'

  const [valueStore, errorStore, command] = createValidator({
    initial: '',
    rules: [
      required({ message: 'Cannot be blank!' }), // Can put arbitrary object
      minLength(3, { message: 'Should be longer than 3.' }),
    ]
  })
</script>

<form>
  <input bind:value="{$valueStore}">
  {#if 'required' in $errorStore}
    <span>{$errorStore.required.message}</span>
  {/if}
  {#if 'minLength' in $errorStore}
    <span>{$errorStore.minLength.message}</span>
  {/if}

  <button type="submit" disabled="{hasError($errorStore)}">Submit</button>
</form>
```

### `createValidator` Options

#### `rules`

An array of validators.

#### `initial`

Initial value of `valueStore`.

#### `immediate`

If `false`, validation does not run until calling `command.activate()`. Default `true`.
For example this can be used to prevent display errors until first blur event occurs.

```html
<input type="text" on:blur="{command.activate}">
```

### Builtin Validators

- `required(options)`
- `minValue(min, options)`
- `maxValue(max, options)`
- `betweenValue([min, max], options)`
- `minLength(length, options)`
- `maxLength(length, options)`
- `betweenLength([min, max], options)`
- `format(regex, options)`

You can put any object on `options`. It can be accessed via `errorStore` like `$errorStore.minValue`. See implementation for more details.

### Custom Validator

You can implement your own validator. It should be an object which has `name` and `isValid` properties, and optionally `options`.

```javascript
const myRule = {
  name: 'myRule',
  isValid: (value) => {
    // true or false
  },
  options: {},
}

const [valueStore, errorStore] = createValidator({ rules: [myRule] })
// $errorStore.myRule appears when value violates the rule.
```
