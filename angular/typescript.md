# Typescript

To watch the file for automatic compiling: 

`tsc app -w`

To tell to TypeScript to watch all the files, we must tell it that we are working in a project:

`tsc -init`

`tsc`


## Types of data:

```javascript

let name:string = "Peter";
let number:number = 123;
let boolean:boolean = true;

let today:Date = new Date();

today = new Date('2020-10-21');

let anykind:any;

anykind = nombre;
anykind = boolean;
```

When declaring objects, the instance MUST have the exact same fields as declaration:

```javascript
let superhero = {
    name: "Peter",
    edad: 20
}
```

## Template Literals

```javascript
let name:string = "John";
let surname:string = "Doe";
let age:number = 32;

let text = `Hello,
${ name } ${ surname }
${ age }`

```

## Optional, Mandatory & Default Params

Mandatory and default params must be declared first, optional last.

```javascript
function activate ( who:string,
                    object:string = "an object",
                    moment?:string) # mandatory, default value, optional
```

## Arroy Functions

```typescript
let myFunction = ( a:number, b:number ) => a + b;
```

## Destructuring

Arrays:

```typescript
let avengers:string[] = [ "Thor", "Steve", "Tony" ];

let [ thor, captn, ironman ] = avengers;
```

## Interfaces

Always declare interfaces with UpperCase

```typescript
interface Xmen {
    name:string,
    power: string
}

function sendMission ( xmen : Xmen ){

    console.log("Sending to : " + xmen.name)
}

let wolverine:Xmen = {
    name: "Wolverine",
    power: "Regenerated"
}
```

