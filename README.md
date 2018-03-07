# AngularAdvancedNotes
Kurs vom 07.-08.03.2018

Referent: Manfred Steyer 

twitter: @ManfredSteyer

## Routing

- in appmodule.ts:

RouterModule.forRoot(APP_ROUTES)

- in Feature-Modulen:

RouterModule.forChild(MY\_FEATURE\_ROUTES)

### Hierarchische Views

- ...html
<router-outlet></router-outlet>

- ...routes.ts
export const FLIGHT_BOOKING_ROUTES: Routes = [
  {
    path: 'flight-search',
    component: FlightSearchComponent,
    children: [
        {
            path: '',
            redirectTo: 'child1',
            pathMatch: 'full'
        },
        {
            path: 'child1',
            component: Child1Component
        }
    ]
  },

### Aux Routes

benannte Platzhalter:

```html
<router-outlet></router-outlet> <!--Name: "primary"-->

<router-outlet name="aux"></router-outlet>
```

Routen-Angaben _relativ zur aktuellen Komponente_! 


## Guards

Services die bestimmte Interfaces impl. um das Ansteuern oder Verlassen von Komponenten zu erlauben oder zu verbieten --> werden an Routen gehängt.

- CanActivate 

Impl. kann Observer verwenden, um Benutzerinteraktion zuzulassen, nach der es erst weitergehen soll

```ts
export interface CanDeactivateComponent {
    canDeactivate(): Observable<boolean>;
}
...

export class FlightEditComponent implements OnInit, CanDeactivateComponent {
  sender: Observer<boolean>;
  showWarning = false;

  constructor(private route: ActivatedRoute) { }

  decide(decision: boolean): void {
    this.showWarning = false;
    this.sender.next(decision);
    this.sender.complete();
  }

  canDeactivate(): Observable<boolean> {
    return Observable.create((sender: Observer<boolean>) => {
      this.sender = sender;
      this.showWarning = true;
    });
  }
  ```

## Resolver

verzögern des Wechseln zu einer Komp., um erst noch Daten zu laden - danach kann die Komponente die Daten vom Resolver "abholen"


## Lazy Loading

Nachladen von Modulen

Benötigt:

@ngtools/webpack loader

### Vorsicht: Lazy Loading und Shared Services

jedes nachgeladene Modul bekommt sein eigenen Scope für Services, als eigene Instanzen für diese Services.

1. Lösung:

Service in CoreModule verlagern, das nur im AppModule geladen wird.

Nachteil: Auflösen der Kapselung!

2. Lösung, analog zum Router-Module - eine forRoot-Methode:

```ts
@NgModule({
    ...
    providers: [...]
})
export class MyModule {

    static forRoot: ModuleWithProviders {
        return {
            ngModule: SharedModule,
            providers: [AdditionalServices, ...]
        }
    }
}
```


## Preloading

Im Hintergrund laden nach dem Initialen Laden der Main Component.

```ts
RouterModule.forRoot(APP_ROUTES, {preloadingStrategy: PreloadAllModules})
```
