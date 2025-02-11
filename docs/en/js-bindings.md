# Bindings

The following variables and functions are only available in JavaScript code.


## Variables

### HOST

the host name (previously available as `GAME` variable). Possible values include `gta3`, `vc`, `re3`, `reVC`, `sa`, `gta3_unreal`, `vc_unreal`, `sa_unreal`, `unknown`. 

CLEO plugins can use SDK to customize the name for their needs.

```js
if (HOST === "gta3") {
  showTextBox("This is GTA III");
}
if (HOST === "sa") {
  showTextBox("This is San Andreas");
}
if (HOST === "unknown") {
  showTextBox("This host is not natively supported");
}
```

### ONMISSION 

the global flag controlling whether the player is on a mission now. Not available on an `unknown` host.

```js
if (!ONMISSION) {
  showTextBox("Not on a mission. Setting ONMISSION to true");
  ONMISSION = true;
}
```

### TIMERA, TIMERB

two auto-incrementing timers useful for measuring time intervals.

```js
while (true) {
  TIMERA = 0;
  // wait 1000 ms
  while (TIMERA < 1000) {
    wait(0);
  }
  showTextBox("1 second passed");
}
```

### __dirname 

an absolute path to the directory with the current file

### __filename 

(since 0.9.4) an absolute path to the current file

## Functions

### log

`log(...values)` prints comma-separated `{values}` to the `cleo_redux.log`

```js
var x = 1;
log("value of x is ", x);
```

### wait 

`wait(timeInMs)` pauses the script execution for at least `{timeInMs}` milliseconds

```js
while (true) {
  wait(1000);
  log("1 second passed");
}
```

### showTextBox

`showTextBox(text)` displays `{text}` in the black rectangular box. Not available on an `unknown` host.

```js
showTextBox("Hello, world!");
```

### exit

`exit(reason?)` terminates the script immediately. `exit` function accepts an optional string argument that will be added to the `cleo_redux.log`.

```js
exit("Script ended");
```

### native

`native(command_name, ...input_args)` is a low-level function to execute a command using its name `{command_name}`. The command name matches `name` property in a JSON file provided by Sanny Builder Library.

```js
native("SET_TIME_OF_DAY", 12, 30); // sets the time of day to 12:30
```

For the commands that return a single value, the result is this value.

```js
const progress = native("GET_PROGRESS_PERCENTAGE");
showTextBox(`Progress is ${progress}`);
```

For the commands that return multiple values, the result is an object where each key corresponds to a returned value. The key names match the output names given in the command definition

```js
var pos = native("GET_CHAR_COORDINATES", char); // returns char's coordinates vector {x, y, z}
showTextBox(`Character pos: x ${pos.x} y ${pos.y} z ${pos.z}`);
```

For the conditional commands the result is the boolean value `true` or `false`
```js
if (native("HAS_MODEL_LOADED", 101)) {
  // checks the condition
  showTextBox("Model with id 101 has been loaded");
}
```

## Static Objects 


### Memory

- `Memory` object allows to manipulate the process memory. See the [Memory guide](using-memory.md) for more information.


### Math


- `Math` object is a standard object available in the JS runtime that provides common mathematical operations. CLEO Redux extends it with some extra commands. See the [Math object](using-math.md) for more information.

### FxtStore

- `FxtStore` allows to update the content of in-game texts. See the [Custom Text](./using-fxt.md) guide for details.


### CLEO

- (since 0.9.4) `CLEO` object provides access to the runtime information and utilities:

  - `CLEO.debug.trace(flag)` toggles on and off command tracing in the current script. When {flag} is true all executed commands get added to `cleo_redux.log`:
  ```js
    CLEO.debug.trace(true)
    wait(50);
    const p = new Player(0);
    CLEO.debug.trace(false)
  ```

  - `CLEO.version` - a complex property providing information about current library version

  ```js
    log(CLEO.version)       // "0.9.4-dev.20220427"
    log(CLEO.version.major) // "0"
    log(CLEO.version.minor) // "9"
    log(CLEO.version.patch) // "4"
    log(CLEO.version.pre)   // "dev"
    log(CLEO.version.build) // "20220427"
  ```