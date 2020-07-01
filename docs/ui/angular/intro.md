- https://angular.io
- Application = Components + Services | Components = View + Class + MetaData
- Modules => Root Angular Module (components) + Feature Angular Modules (components)
- Supports ES 5. Scripts written in ES 2015 (ES 6) or Typescript should be transpiled. ES = ECMA Script (European Computer Manufacturer's Association)
- Angular is modular (consists of many modules) https://www.npmjs.com/~angular

- Why not Javascript
  - Doesn't provide namespace and all methods/function will be under global namespace
  - No Code Organization
  - **AngularJS Modules** helps in addressing it (In ES 2015, a file is a module).
    - ES 2015 module 
    ```
    // exporting module 
    export class Product { }     -> transpiles to ->   function Product() { }
    
    // importing it for use
    import { Product } from './product'
    ```
    - Angular Module: can communicate to other modules and a module can have many components


### Component
- Includes a template (**view**) which is created with HTML. includes binding and directives to show the intended View
- Contains a Class (**Code**, ex: typescript) with Properties and Methods
- Contains **Metadata**, which is defined with a decorator
- Component = Template + Class (Properties,methods) + Metadata
```
import { Component } from '@angular/core';

// Metadata & Template  : Like an attribute for the class
@Component ({      
  selector: 'app-root',   // directive name - a custom element
  template: `<div> <h1> {{ pageTitle }} </h1> 
                <div> My First Component </div>
             </div>`
})
export class AppComponent {                     // Class
  pageTitle: string = 'Hello World!'
}

// index.html
<body>
  <pm-root></pm-root>
</body>

// module
import { BrowserModule } from '@angular/platform-browser';
import { NgModule } from '@angular/core';
import { AppComponent} from './app.component';

@NgModule({
  imports: [ BrowserModule ],         // external modules
  declarations: [ AppComponent ],     // our custom components
  bootstrap: [ AppComponent ]         // startup components
})
export class AppModule {}
```
- Decorator adds metadata to a class, its members, or its method arguments (prefixed with @) (~ attribute)
- Module organizes and loads the required components and bootstraps what is required
- Component should belong to only one module
- Any new component added should be loaded in the base module so that angular can find it.
- template: use "" for inline template. backticks (`) for multiple lines. Or a templateUrl: './test.html'
- Directives: Custom & Angular Built In (Ex: *ngIf, *ngFor - structural directives)
- Binding:   
  - ``` {{ }}  One way Binding ```  Interpolation
  - ``` <img [src]='product.imageUrl' /> ``` Property binding
  - ``` <img src={{product.imageUrl}} /> ``` Property binding
  - ``` <button (click)='toggleImage()' /> ``` Event binding
  - ``` <input [(ngModel)]='listFilter' >  ``` Two way binding (ngModule belongs to FormsNodule)
- Pipes for display
  - ``` {{ name | lowercase }}  ```
  - ``` {{ amount | currency:'USD':'symbol':'1.2-2' }}  ```
  - **Custom Pipes**
  ```
  import { Pipe, PipeTransform }  from '@angular/core';
  @Pipe({
    name: 'convertToSpaces'
  })
  export class ConvertToSpacesPipe implements PipeTransform {
    transform(value: string, character: string) : string {
      return value.replace(character, ' ');
    }
  }

  // use
  @NgModule ({ ... declarations: [ ..., ConvertToSpacesPipe ]  ... })
  {{ product.productCode | convertToSpaces: '-' }}
  ```
- Strong Typing: In Typescript we can specify the type of data for properties and methods
- Interface in typescript
```
export interface IProduct {
  productId: number;
  productName: string;
  releaseDate: Date;
  calculate(percent: number): number;
}

// use
import { IProduct } from './product';

export class ProductList {
  pageTitle: string = "Welcome";
  products: IProduct[] = [...];
}
```
- Encapsulating Component Styles
  - Through @Component decorator properties: styles and styleUrls
```
@Component({
  selector:...,
  styles: ['thead {color: black;}'],      // applicable only to the content of the component
  styleUrls: ['./custom-component.css']   // applicable only to the content of the component
});
```
- Component Life Cycle: Create -> Render -> Create and render children -> process changes -> Destroy
  - Hooks: 
    - OnInit: perform component initialization, retrieve data
    - OnChanges: perform action after change to input properties
    - OnDestroy: perform cleanup
    - ......
```
import {Component, OnInit} from '@angular/core';

@Component({...})
export class TestComponent implements OnInit {
  ngOnInit(): void { ... }
}
```
- **Nested Component**
```
import {}

@Component({
  selector: 'pm-star',
  templateUrl: ...
  styleUrls: ...
})
export class StarComponent implements OnChanges {
  @Input() rating: number;    // @input says that the data should be sent by a parent component
  starWidth: number;

  @Output() notify: eventEmitter<string> = new EventEmitter<string>();

  onclick() {
    this.notify.emit('clicked!');
  }

  ngOnChanges(): void {
    this.startWidth = this.rating *75 / 5;
  }
}

//use - inside the parent component template
// load in NgModule declarations
<pm-star [rating]='product.StarRating'    // works as we have marked rating property as @input
   (notify)='onNotify($event)' >          // works as we are emitting the event from star component
</pm-star>

// inside <pm-star>
<div (click)='onClick()' >
  ... stars ...
</div>

// parentcomponent 
export class ParentComponent {
  onNotify(message: string): void { }
}
```

### Services 
- Shared Service instance, to be registered with angular.
  - can be registered to Root Injector or Component Injector
- use Angular Dependency injector to get the service instance
```
import { Injectable } from '@angular/core';
import { IProduct } from './product';

@Injectable({
  providedIn: 'root'                 // available for whole application (new in Angular 6.0)
})
export class ProductService {
  getProducts(): IProduct[] {
    return [{...},{...}];
  }
}

// for registering service at component level
@Component({
  providers: [ProductService]
})

// usage
@Component({
  ...
})
export class ProductComponent {
  private _productService;
  constructor(productService: ProductService) {                            // dependency Injector injects
    this._productService = productService;
  }

  constructor(private productService: ProductService) {                    // shorthand for above code
  }
}
```

### ROUTING
```
import { RouterModule } from '@angular/router'

@NgModule({
  imports: [ RouterModule.forRoot(
                  [
                    { path: 'products', component: ProductListComponent },
                    { path: 'products/:id', component: ProductDetailComponent },
                    { path: 'welcome', component: WelcomeComponent },
                    { path: '',  redirectTo: 'welcome', pathMatch: 'full' },
                    { path: '**', component: PageNotFoundComponent }
                  ], 
                  { useHash: true }      // if required
           )],
  declarations: [ ]
})
export class AppModule { }


// route html
<div class="nav navbar-nav" >
    <a routerLink="/" class="navigation-element" >Welcome</a>
    <a routerLink="/products" class="navigation-element" >Product List</a>
    <a [routerLink]=['/product/10'] class="navigation-element" >Laptop</a>
</div>

<router-outlet></router-outlet>
```
- Routing with parameters
```
{ path: 'products/:id', component: ProductDetailComponent }

<a [routerLink]="['/products', product.productId]" >{{product.Name}}</a>

...
class ProductDetailComponent{
  constructor(private route: ActivatedRoute) {
    console.log(this.route.snapshot.paramMap.get('id'));   // only initial value
    // use observable to have latest data. With changes (if any)
  }
}
```
- Navigate through Code
```
...
class Component {
  constructor(private router: Router){}

  onBack(): void {
    this.router.navigate(['/products']);
  }
}
```
- Protecting Routes with Guards: CanActivate, canDeactivate
```
import { Injectable } from '@angular/core';
import { CanActivate, ActivatedRouteSnapshot, RouterStateSnapshot, Router } from '@angular/router';
import { Observable } from 'rxjs';

@Injectable({ providedIn: 'root' })
export class ProductDetailGuard implements CanActivate {
  constructor(private router: Router){ }

  canActivate(next: ActivatedRouteSnapshot, 
                   state: RouterStateSnapshot): Observable<boolean> | Promise<boolean> | boolean
  {
    let id = +next.url[1].path;
    if(isNaN(id) || id <1) {
      this.router.navigate(['/products']);
      return false;
    }
    return true;
  }
}

// usage
{ path: 'products/:id', canActivate: [ ProductDetailGuard ], component: ProductDetailComponent }
```

## Links
- [Angular 2 without Typescript and Node](https://medium.com/@areai51/angular-2-without-node-and-typescript-f80cfc853904). [Code](https://github.com/ILearny/angular2-tour-of-heroes-es5)
