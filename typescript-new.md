# Typescript
- Alternative to JavaScript (superset)
- Allows us to use strict types (e.g. number, string, object)
- Supports modern features(arrow functions, let,const)
- Extra features (generics, interfaces, tuples etc)

## Installing Typescript Compiler
```
npm i -g typescript
```

## Run Typescript code
```
tsc sandbox.ts -w
```
-w will be automatically converted this file into javascript at every change

## Difference between Typescript and Javascript
JavaScript and TypeScript are both programming languages used for web development, but there are some key differences between them:

- **Type System:** TypeScript is a statically typed superset of JavaScript, which means it adds a type system on top of JavaScript. JavaScript, on the other hand, is dynamically typed, allowing variables to hold values of any type. TypeScript's type system enables static type checking, which helps catch errors at compile-time rather than runtime.

- **Static Typing**: TypeScript allows you to explicitly declare types for variables, function parameters, and return values. This provides better tooling support, as IDEs and code editors can provide autocompletion, type inference, and type checking. JavaScript does not have built-in static typing and relies on runtime type inference.

- **Compatibility**: JavaScript code is valid TypeScript code since TypeScript is a superset of JavaScript. This means that any JavaScript codebase can be gradually migrated to TypeScript by adding type annotations. TypeScript files have the extension ".ts," while JavaScript files have the extension ".js".

- **ECMAScript Support**: Both JavaScript and TypeScript are based on the ECMAScript standard. TypeScript typically supports the features introduced in the latest ECMAScript version, but it may have additional features and syntactic sugar to enhance developer productivity.

- **Compilation**: JavaScript is an interpreted language executed by the browser or runtime environment directly. On the other hand, TypeScript needs to be transpiled into JavaScript before it can be executed. TypeScript code goes through a compilation step that converts it into JavaScript, ensuring compatibility with different browsers and runtime environments.

- **Tooling and Ecosystem**: While JavaScript has a mature and extensive ecosystem with numerous libraries, frameworks, and tools, TypeScript has gained popularity in recent years and has excellent tooling support. TypeScript integrates with popular development tools like Visual Studio Code and provides rich autocompletion, refactoring, and error checking.

- **Developer Experience**: TypeScript enhances the developer experience by catching potential errors early, improving code maintainability, and enabling code navigation through type information. It offers better code organization and documentation through interfaces, classes, and modules.

In summary, TypeScript builds upon JavaScript by adding static typing, which brings benefits like early error detection and improved tooling support. It provides a more structured approach to writing code and offers enhanced developer experience, especially in large codebases and team collaborations. However, JavaScript remains the de facto language for web development and is still widely used.

|      | JavaScript | TypeScript |
| ---- | ----------- | ----------- |
| Type System  | Dynamically typed (variables can hold any type of value) | Statically typed (variables have predefined types) |
| Compiler	  | No compilation step required, executed by the browser or Node.js | Requires compilation step to convert TypeScript code to JavaScript |
| Type Annotations  | Optional (Types can be inferred, but annotations are not mandatory) | Mandatory (Types must be explicitly declared for variables, parameters, and return types) |
| Type Checking  |No compile-time type checking | Compile-time type checking detects errors and provides static type analysis |
| Language Features  | Prototype-based object-oriented programming | Adds classes, interfaces, and modules on top of JavaScript's functionality |
| Code Maintainability  | May require additional effort to ensure type safety and avoid runtime errors | Enhances code maintainability and allows better refactoring and tooling support |
| Tooling Support	  | Limited IDE support for autocompletion and static analysis | Strong IDE support with features like autocompletion, type inference, and refactoring |
| Compatibility	  | Compatible with all modern web browsers and Node.js | Compiles to JavaScript, making it compatible with all JavaScript environments |
| Adoption  | Widely used and supported across the web | Growing in popularity, especially in larger codebases and enterprise applications |

## Typescript Union
Union types in TypeScript allow you to specify that a variable or parameter can hold values of multiple types. You can use the pipe (|) symbol to separate the individual types within the union. Here's an example that demonstrates the usage of union types:
```
function displayValue(value: string | number) {
  console.log(value);
}

displayValue("Hello"); // Output: Hello
displayValue(42);      // Output: 42
displayValue(true);    // Error: Argument of type 'boolean' is not assignable to parameter of type 'string | number'
```
In the above example, the displayValue function accepts a parameter value of type string | number, which means it can accept either a string or a number. When calling the function, you can pass either a string or a number as an argument, and TypeScript will infer the appropriate type.

However, if you try to pass a value that is not part of the specified union type, TypeScript will throw a type error, as shown in the last line of the example where passing a boolean value causes a compilation error.

Union types are useful when you have a variable or parameter that can accept multiple types, providing flexibility and type safety at the same time.

Example:
```
let mixed: (string|number|boolean)[] = [];
mixed.push('hello');
mixed.push(false);
mixed.push(20);
console.log(mixed);
```

## Typescript Object
```
let ninjaOne: object;
ninjaOne = { name: 'yoshi', age: 30 };

let ninjaTwo: {
  name: string,
  age: number,
  beltColour: string
};
ninjaTwo = { name: 'ken', age: 20, beltColour: 'black' };
```
