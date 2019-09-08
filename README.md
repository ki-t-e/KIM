# KIM (KiBoard Input Methods)

The files in this directory provide keyboard layouts for use with [KiBoard](https://github.com/ki-t-e/KiBoard/).
Only writing system keys are provided; control keys and other functional keys are left to KiBoard to handle.

## Usage

```js
// Simply do a JS default import to acquire an input method.
import QWERTY from "./QWERTY.js";
```

## Format

Source KIM files are available in the `Sources/InputMethods/` directory in this repository.
Sources are given in [CSON (CoffeeScript Object Notation)](https://github.com/bevry/cson) format.

### Overall Structure

```coffee
actions:
	actionsForStates... # Values may be any key value
keyMaps: [ # Array of key maps
]
terminators:
	terminatorsForStates... # Values must be strings
```

### Key Maps

```coffee
mode: # String
modifiers: # Array of modifiers, or single modifiers object
keys... # Key
```

If `mode` is given and nonempty, then the key map only applies when the keyboard is in the given mode.
Otherwise, the key map only applies when the keyboard is in the default mode.

`modifiers` gives the modifiers (Shift, etc.) for which the key map applies.
The first key map which has a matching modifiers object and a value for the pressed key will be used.
If empty, the key map applies regardless of modifiers.

The names of keys match those given by [<cite>UI Events KeyboardEvent code Values</cite>](https://www.w3.org/TR/uievents-code/).
The values are one of the following:

+ A string or number, if the key always outputs the same value.

+ An object with an `action` property, if the value of the key depends on the current state.
The value of this property should match a key in `actions`.

+ An object with a `state` property, which activates a state.
The value of this property should match a key in `terminators`.

+ An object with a `mode` property, which activates a mode.
There should be a key map with a `mode` property which matches the value of this one.
If `shift` is true, then the mode only applies while the key is being held.
The empty string refers to the default mode.

### Modifiers

The following keyboard modifiers are available for detection:

```coffee
AltGraph: # Boolean
CapsLock: # Boolean
Shift: # Boolean
```

If a modifier is not present, then the key map applies regardless of whether the given key is pressed.
