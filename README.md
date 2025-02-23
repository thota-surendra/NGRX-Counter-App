# NgRx Counter App

This is a basic Angular project demonstrating state management using NgRx.

## Features
- Implements NgRx Store for state management.
- Supports increment, decrement, and reset functionality.
- Uses Selectors for efficient state selection.
- Includes Store DevTools for debugging.

## Installation

1. Clone the repository:
   ```sh
   git clone <your-repo-url>
   cd ngrx-demo
   ```

2. Install dependencies:
   ```sh
   npm install
   ```

3. Run the application:
   ```sh
   ng serve
   ```
   Open `http://localhost:4200/` in the browser.

## NgRx Setup

### Install NgRx Packages
```sh
ng add @ngrx/store
ng add @ngrx/effects
ng add @ngrx/store-devtools
ng add @ngrx/entity
```

### Project Structure
```
ngrx-demo/
│── src/app/
│   │── store/
│   │   │── counter/
│   │   │   │── counter.actions.ts
│   │   │   │── counter.reducer.ts
│   │   │   │── counter.selectors.ts
│   │── app.component.ts
│   │── app.module.ts
```

### Counter Feature Implementation

#### 1️⃣ Define Actions (`counter.actions.ts`)
```typescript
import { createAction } from '@ngrx/store';

export const increment = createAction('[Counter] Increment');
export const decrement = createAction('[Counter] Decrement');
export const reset = createAction('[Counter] Reset');
```

#### 2️⃣ Create Reducer (`counter.reducer.ts`)
```typescript
import { createReducer, on } from '@ngrx/store';
import { increment, decrement, reset } from './counter.actions';

export const initialState = 0;

export const counterReducer = createReducer(
  initialState,
  on(increment, (state) => state + 1),
  on(decrement, (state) => state - 1),
  on(reset, () => 0)
);
```

#### 3️⃣ Define Selectors (`counter.selectors.ts`)
```typescript
import { createSelector, createFeatureSelector } from '@ngrx/store';

export const selectCounter = createFeatureSelector<number>('counter');
```

#### 4️⃣ Register Store in `app.module.ts`
```typescript
import { StoreModule } from '@ngrx/store';
import { counterReducer } from './store/counter/counter.reducer';

@NgModule({
  imports: [
    StoreModule.forRoot({ counter: counterReducer })
  ]
})
export class AppModule { }
```

#### 5️⃣ Use NgRx in `app.component.ts`
```typescript
import { Component } from '@angular/core';
import { Store } from '@ngrx/store';
import { increment, decrement, reset } from './store/counter/counter.actions';
import { selectCounter } from './store/counter/counter.selectors';
import { Observable } from 'rxjs';

@Component({
  selector: 'app-root',
  templateUrl: './app.component.html',
})
export class AppComponent {
  counter$: Observable<number>;

  constructor(private store: Store) {
    this.counter$ = this.store.select(selectCounter);
  }

  increment() { this.store.dispatch(increment()); }
  decrement() { this.store.dispatch(decrement()); }
  reset() { this.store.dispatch(reset()); }
}
```

#### 6️⃣ Update `app.component.html`
```html
<h1>Counter: {{ counter$ | async }}</h1>
<button (click)="increment()">Increment</button>
<button (click)="decrement()">Decrement</button>
<button (click)="reset()">Reset</button>
```

## Debugging with NgRx DevTools
1. Install Redux DevTools Extension for your browser.
2. Enable NgRx Store DevTools in `app.module.ts`:
   ```typescript
   import { StoreDevtoolsModule } from '@ngrx/store-devtools';
   import { environment } from '../environments/environment';

   @NgModule({
     imports: [
       StoreDevtoolsModule.instrument({ maxAge: 25, logOnly: environment.production })
     ]
   })
   export class AppModule { }
   ```

## Next Steps
- Add **NgRx Effects** for API calls.
- Implement **NgRx Entity** for managing lists of data.
- Introduce **Facades** for better state management.

---

Feel free to contribute and improve this project!

