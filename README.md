# Angular Notes
#### Joachim Canete
#### Personal Projects

---

## Intro

### 1. Archtecture

> Front-End: UI

Typically built with HTML, CSS, TypeScript, & Angular
HTML Templates & **Presentation Logic**

> Back-End: Data & Processing

Database + APIs
**Business Logic**

### 2. Installing Angular Globally
`npm i -g @angular/cli`

Moving forward, I shouldn't have to reinstall `Angular` every time I want to build an `Angular App`.

### 3. Create App
`ng new app-name`

Not unlike `React`'s `npm create-react-app app-name`

> Will ask "Would you like to add Angular routing"

If initially choosing **No**, can be changed later down the road.

### 4. Run The App
`ng serve`

>Proceed to `localhost:4200` 
>
> I don't know if the localhost's port can be changed, like how we worked on port 3000/4000/42069/etc.

Kinda like `React`'s `npm start` to run the app on local host.

### 5. Directory Sctructure
Root: `app-name/src/index.html`
```
<body>
  <app-root></app-root>
</body>
```
This is root of the app itself. Everything gets loaded into `<app-root>` much like React. Not much work needs be done here but we can change the App name and other stuff if need be

Entrypoint: `app-name/src/main.ts`
```
import { AppModule } from './app/app.module';
```
Because `Angular` is a module-based framework, `main.ts` will **import** `Angular`'s modular funcationlity.

```
platformBrowserDynamic().bootstrapModule(AppModule)
  .catch(err => console.error(err));
```
`AppModule` is being passed through `.bootstrapModule` in order to incorporate the aforementioned functionality

`AppModule` can actually be found in `app-name/src/app/app.module.ts`

Inside `app.module.ts` is `NgModule` which is responsible for declarations, imports, providers, and bootstrap
```
@NgModule({
  declarations: [
    AppComponent
  ],
  imports: [
    BrowserModule,
    AppRoutingModule
  ],
  providers: [],
  bootstrap: [AppComponent]
})
```
When a component is created, it'll be put **into** `declarations`.
When a module is being used for the app or a new module is being introduced into the app, it'll be **located** in `imports`.
Global providers or services are **found** in `providers`.
The root `AppComponent` is **actively** `bootstrap`ed.

### 6. Component Structure
`Angular Components generally have **4** files.

>`app.component.ts` files - main class with properties and methods & specifications (templates and stylesheets)

`{ Component }` is brought in from the `@angular/core`. The component declaration is comprised of the following

- selector (the html tag for which the components are imbedded into). Newly built apps are tied to `<app-root>` located in `app-name/src/index.html`

- templateUrl (tied to the components specific html template. in this case, it'll be `app.component.html`)

- styleUrls (the designated css stylesheet: `app.component.css`)

- `export class AppComponent` is responsible for the designated component's properties. titles and methods will be included here

Because freshly built Angular apps are written with the following:
```
export class AppComponent {
  tite = 'hello-world'
}
```
it is advised to add *types* to the classes for clarity and longevity as shown below:
```
export class AppComponent {
  tite: string = 'hello-world'
}
```
Were I to incorrectlty assign a class's type, I would receive an error. Very useful
```
// wrong type
export class AppComponent {
  tite: number = 'hello-world'
}
```
These classes are especially useful when fucking with the `app.compenent.html` file as we can use **string interpolation** to incorporate prebuilt classes into our templates.
With the above class being exported to the predetermined `templateUrl`, we just need to go into the `html` file and, within the desired tag, throw in the double squiggles.
```
<h1>{{ title }}</h1>
```
>Fun fact about string interpolation within `html` docs, they accept **in-line** code, such as math formulas or `JavaScript` methods. Heck, use *ternerary operators* if you'd like
```
<h1>{{ title.toUpperCase() }}</h1>
```
>`app.component.html` files - the template that has all the stuff and things first built. Mostly gets delted at the start.
>
>`app.component.css` files - the styling of things
>
>`app.component.spec.ts` files - used for testing

### 7. Create A New Component
```
ng generate component components/header
```
This is how we create the component `Header` inside the `components` directory. By running the `ng generate component` commande, our CLI will build out the affiliated files as well (`.css`, `.html`, `.spec.ts`, `.ts`)

```
import { Component, OnInit } from '@angular/core';

@Component({
  selector: 'app-header',
  templateUrl: './header.component.html',
  styleUrls: ['./header.component.css']
})
export class HeaderComponent implements OnInit {

  constructor() { }

  ngOnInit(): void {
  }

}
```
- The `header.component.ts` file also includes a `constructor() { }` that runs whenever the component itself is initilized in the app
- Most of the time, you'll actually want to use the `ngOnInit(): void { }` constructor, which is a **lifecyle method**. If you want it to run when the component loads, you throw it into `ngOnInit`

With our new component, we can imbed it into our main app component `app.component.html` via **html-tag** shown as the new component's **selector**
```
@Component({
  selector: 'app-header',
})
```
And then inside the `app.component.html` file:
```
<app-header></app-header>
```
The components contents are shown in the designated `header.component.html` file.

---
### Add a bit of styling

While I have global styling available in the `app.component.css` file, I also want to incorporate styles unique to the **header component**. These styles will be written in the `header.component.css` file.
```
<header>
  <h1>{{ title }}</h1>
</header>
```
---

### Another Component

I want to create a new component for this app, and in order to do that, it's important that I am in the `app` level of this porject tree, so for this project, that'll be `hello-world/src/app`. Then, I'll hop into my terminal and type out 

```
ng generate component components/button
```

and it'll churn out a new component with all it's affiliated files (`.css`, `.html`, `.spec.ts`, `.ts`).

Because components are reusable, the `button` component can be imbedded into different files. For the purpose of its use in the `header.component`, I'll imbed it into the `header.component.html` file using the button's **selector** shown in `button.component.ts`.
```
@Component({
  selector: 'app-button',
})
```

With the selector identified, hop into `header.component.html` and add a new tag for the button
```
<header>
  <h1>{{ title }}</h1>
  <app-button></app-button>
</header>
```

Now that the button has been added to the header, I want to include some uniqe styling for its placement, but not for the component in its entirety. Instead of modifying the `button.component.css` file, I can just do some *in-line* styling, much like adding properties in `React`.
```
<app-button color="green" text="Add">
</app-button>
```

To grab onto the button properties (`color='green' text='Add'`) themselves, I need to hop into `button.component.ts` and bring in `Input` from `@angular/core`.
```
import { Component, OnInit, Input } from '@angular/core';
```

Then declare Input in the `button class`
```
