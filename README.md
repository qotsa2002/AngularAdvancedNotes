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

## Resolver

verzögern des Wechseln zu einer Komp., um erst noch Daten zu laden - danach kann die Komponente die Daten vom Resolver "abholen"