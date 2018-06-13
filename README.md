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