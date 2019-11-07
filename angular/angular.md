# Angular

Typescript can't be executed in the browser, it needs to be compiled into javascript (vanilla).

Transcript = change from one language to another

To create a new project in Angular: `ng new miApp`

To run a local server: `ng serve -p 4200`

Default port in Angular: 4200

The selector tells to Angular where to find the place where it needs to place the components.

The **first** code which is executed is the "main.ts" file.

The cycle is like this:  
    `main.ts -> app.module.ts -> app component`

Each **component** has its own markup, style and logic.

At the end of the day, there is only one module, which is the app module, which in turn receive the imports from the rest of the modules (components).

## Convention

The project structure for a new component should be:

    - App
      - components
        - navbar  
            navbar.component.ts
            navbar.component.html
            navbar.component.css

```typescript

import { Component } from '@angular/core';

@Component({
    selector: 'app-navbar',
    templateUrl: 'navbar.component.html',
    styleUrl: 'navbar.component.css'
})
export class NavbarComponent{

}
```

## Markup & Style

If there is more than 4 lines of markup in the template, just write in the js file (inline).

Else, use `templateUrl`.

The same with styles:  
its possible to just write:

`styles: [ h3 {color: blue}]`

## Selectors in the components

The most common way to do things is to use `selector: 'app-servers'`  
`<app-servers></app-servers>`

But you can selector with attribute:  
`selector: '[app-servers]'`  
`<div app-servers="somethign"></div>`

Or even classes:  
`selector: '.app-servers'`  
`<div class="app-servers"></div>`



## Project Structure

The folder **e2e** is for automatic tests

## Create component using the CLI

To create a new component: `ng g c components/footer`

g : generates
c : component

To create a new component without style sheet: `ng g c components/about -si`

## Databinding

Databinding = Communication

We need communication between our TypeScript Code (Business Logic) and the Template (HTML).

There are different type of communication:
    - Output Data:
      - String Interpolatoin({{ data }})
      - Property Binding([property] = "data")
    - React to (User) Events:
      - Event Binding ((event)="expression")

    - Combination of both:
      - Two-Way-Binding([(ngModel)]="data")


## Routes

To use the snippet to create the routes: `ng-router-app`


## Structural Directives

Two main types: ngIf & ngFor

