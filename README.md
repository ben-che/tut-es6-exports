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

## Destructuring
- Destructuring in ES6 allows us to bind properties from Arrays and Objects to any number of different variables:

    ```
    // creating an object:

    let myObj = {
        a : 1,
        b : 2
    };

    // destructuring the object:

    let { a, b } = myObj;

    console.log(a);     // will print 1 to the console
    console.log(b);     // will print 2 to the console

    ```
    We can even further assign an alias to the value we pull out by using the following syntax:

    ```
    let myObjAgain = {
        myString : "Hello!"
    };

    let { myString : stringAlias } = myObjAgain;

    console.log("stringAlias");     // will print "Hello!" to the console
    ```

- There are several things to note, however:

    1. If we pull __primitives__ out of an Array or Object, this creates a __binding__. This means that if the original Array or Object is mutated, the newly created variable will not be changed:

        ```
        // creating an object:

            let myObj = {
                stringPrim: "foo"
            };

        // mapping the primitive to an alias:

            let { stringPrim : stringAlias } = myObj;
        
        // mutating the original object:

            myObj.stringPrim = "bar";

        // console logs:

            console.log(myObj.stringPrim) // will print "bar"
            console.log(stringAlias)      // will print "foo"
        ```
        This happens because when we work with primitives in Javascript, we create an actual copy of the primitive and store it in a separate part in memory, as opposed to creating a pointer to the address in memory where the primitive is stored.

    2. However, if we pull an Object (or Array) out, Javascript creates a _reference_ instead:
        ```

        // creating an object:

            let myObjTwo = {
                nestedArr : [1, 2, 3],
                nestedObj : {
                    name: "Ben",
                    pets: 1
                }
            }

        // mapping the nested objects to different aliases:

            let { nestedArr : arrAlias, nestedObj : arrObj } = myObjTwo;

        // mutating the original object:

            myObjTwo.nestedArr.push(4);
            myObjTwo.nestedObj.name = "Bill";
            myObjTwo.nestedObj.pets = false;

        // console logs:

            console.log(myObjTwo.nestedArr);  // will print [1, 2, 3, 4];
            console.log(arrAlias);            // also will print [1, 2, 3, 4]

            console.log(nestedObj);           // will print { name:"Bill", pets: false}
            console.log(objAlias);            // will also print { name:"Bill", pets: false}

        ```
        This means that the variables _arrAlias_ and _arrObj_ don't hold a copy of the value - they merely hold a reference to where that value lies in memory. This means that when the original object is mutated, the 'values' in _arrAlias_ and _arrObj_ are mutated as well.

## Tree shaking
The concept is quite aptly named. When someone shakes a tree, all of the dried and dead leaves will fall to the ground, leaving the healthy and usable leaves. When large numbers of modules are imported and exported in Javascript, there will inevitably be parts of modules that are included in exports but aren't used. These parts will not be included in the final bundle after the build command is run.

```
// my-module.js:

export myFunc = () => {
    console.log("Hello world.")
}

export tempFunc = () => {
    console.log("Today is 27 degrees, yikes.")
}

export sweatyFunc = () => {
    console.log("Ya boy is sweaty today!")
}

```

```
// main.js:

import { myFunc, tempFunc } from 'my-module'

myFunc();
tempFunc();

```

Here, because sweatyFunc() was never used, once the build command runs, the code will not be included in the final bundle.

## Bundles (and Bundlers)
For all the talk of modules and exports, there hasn't been much focus on bundles. In Javscript, a bundler is a tool that grabs all dependencies and code and puts it into one Javascript file. This is especially helpful when we create a number of different modules. Bundlers solve several problems, with the most common being: 

1. Every time we import something in a script tag, there is a high likelihood we create a global variable - something we want to avoid due to memory and collision issues.
2. The order in which the script tags are placed matters - if the module in the first script tag has dependencies found in the module within the fifth script tag, your browser will complain and throw errors.
3. Imagine scaling your website to the size of Amazon - you would need hundreds of script tags at the bottom of the page to account for all of the new modules, and you would have to manually remove individual script tags once the module becomes obsolete. This is not scalable nor is it maintainable.

Common bundlers that solve these problems include:
- [Webpack](https://webpack.js.org/)
- [Browerify](http://browserify.org/)
- [Rollup](https://rollupjs.org/guide/en)



## Additional Resources:
Interesting reads related to modules and modular thinking in Javascript:

- When using Node, why do I use _const myVar = require('myModule')_ instead of just using import all the time?
    -  [Modules from an ES6, CommonJS and Asyncronous Modular Definition (AMD) viewpoint](https://auth0.com/blog/javascript-module-systems-showdown/)

- A more low level understanding of destructuring in JS:
    - [ES6 Destructing Objects and Arrays](https://ponyfoo.com/articles/es6-destructuring-in-depth)
    - [Understanding Binding vs Referencing in JS](https://codeburst.io/explaining-value-vs-reference-in-javascript-647a975e12a0)

- Singletons - a way to ensure that only one "copy" of an imported module exists at a time
    - [What is a singleton and why is it used?](https://www.sitepoint.com/javascript-design-patterns-singleton/)
