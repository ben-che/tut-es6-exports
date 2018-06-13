# ES6 Exports and Imports
Modules are a concept found fairly commonly in many different languages. They can be thought of as add-ons in a way - they add functionality defined in one module to another module they are imported into.

There are many benefits to modular code:

1. Each module should be responsible for a specific functionality, and as such, we will have a relatively easier time understanding the expected output of each module.

2. Modules can be reused over and over and can be brought in across many applications.

3. Modules encourage loose coupling and reduces the risk of dependency issues. In the case of a dependency issue, the problem should be immediately obvious.

4. Modules reduce the risk of name collisions. For example, if there were two modules with a function called add - we would actually reference the add function by calling moduleOne.add() and moduleTwo.add():
    ```
    // Module One

    export function add () {
        console.log('Add function from Module One')
    }
    ```
    ```
    Module Two 

    export function add () {
        console.log('Add function from Module Two')
    }
    ```

    ```
    import * as ModuleOne from 'ModuleOne';
    import * as ModuleTwo from 'ModuleTwo';

    ModuleOne.add()     // prints 'Add function from Module One'
    ModuleTwo.add()     // 'Add function from Module Two'
    ```

5. Code organization! Keeping unrelated code in separate buckets will inevitablely help with productivity efficiency.

## Exporting and Importing Modules
Once you've written a module that you want to export, ES6 mainly supports two methods - default exporting and named exporting.

### Default Exports
- Only one _export default_ statement can exist in a module
- Default exports don't have to be named - meaning the developer who later imports them can assign a name to the export
- Default exports have different syntax for different values:
    ```
    // exporting a function:

    export default function() {
        console.log("Exporting a function")
    }

    export default () => {
        console.log("Exporting an arrow function")
    }
    ```
    ```
    // exporting a class:
    
    export default class {
        key : value
    }
    ```

- When you import these modules, you cna actually assign a name to them:

    ```
    // ModuleOne.js

    let n;
    export default n = 10;
    ```
    ```
    import customName from 'ModuleOne.js'
    console.log(customName) // logs 10 to the console
    ```

### Named Exports
- Unlike default exports, multiple named exports can exist within one module
- As the name suggests, the item being exported has to have a name

    ```
    // named-exports-module.js

    // exporting a function:

    printMe = () => {
        console.log("I'm a fancy function");
    }

    export { printMe };

    ############################################################
    ############################################################

    // exporting a constant

    export const message = "Alas I am but a simple string.";

    ############################################################
    ############################################################

    // Alternatively, we can export multiple items in one line:

    printMeTwo = () => {
        console.log("I'm still a fancy function");
    }

    const messageTwo = "Alas I am still a simple string.";
    
    export { printMeTwo, messageTwo };

    ```
- And to import them, we use a similar syntax:

    ```
    import { printMeTwo, messageTwo } from 'named-exports-module';

    printMeTwo();
    console.log(messageTwo);

    ```

### Destructuring

## Additional Resources:
Interesting reads related to modules and modular thinking in Javascript:

- When using Node, why do I use _const myVar = require('myModule')_ instead of just using import all the time?
    -  [Modules from an ES6, CommonJS and Asyncronous Modular Definition (AMD) viewpoint](https://auth0.com/blog/javascript-module-systems-showdown/)

