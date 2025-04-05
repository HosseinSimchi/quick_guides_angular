## Counter

**src/app/state/counter.state.ts**

#### STATE

```javascript
export interface CounterState {
  count: number;
}

export const initialState: CounterState = {
  count: 0,
};
```

#### ACTIONS

**src/app/state/counter.actions.ts**

```javascript
import { createAction } from "@ngrx/store";

export const increment = createAction("[Counter] Increment");
export const decrement = createAction("[Counter] Decrement");
export const reset = createAction("[Counter] Reset");
```

#### REDUCER

**src/app/state/counter.reducer.ts**

```javascript
export const counterReducer = createReducer(
  initialState,
  on(increment, (state) => ({ ...state, count: state.count + 1 })),
  on(decrement, (state) => ({ ...state, count: state.count - 1 })),
  on(reset, (state) => ({ ...state, count: 0 }))
);
```

#### UPDATE CONFIG FILES

**Update src/app/app.config.ts**

```javascript
export const appConfig: ApplicationConfig = {
  providers: [
    provideRouter(routes),
    provideStore({ counter: counterReducer }),
    provideStoreDevtools({
      maxAge: 25, // Retains last 25 states
      autoPause: true, // Pauses recording actions when extension window is not open
    }),
  ],
};
```

**Update src/app/app.component.ts**

```javascript
export class AppComponent {
  private store = inject(Store<{ counter: CounterState }>);

  count$ = this.store.select(state => state.counter.count);

  increment() {
    this.store.dispatch(increment());
  }

  decrement() {
    this.store.dispatch(decrement());
  }

  reset() {
    this.store.dispatch(reset());
  }
}
```
```html
{{ count$ | async }}
```
## Optional: Add NgRx Effects

**Update app.config.ts**

```javascript
provideEffects([CounterEffects]);
```
