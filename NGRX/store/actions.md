## Writing actions

```javascript
import { createAction, props } from '@ngrx/store';

export const login = createAction(
  '[Login Page] Login',
  props<{ username: string; password: string }>()
)
```

**In component.ts**

```javascript
onSubmit(username: string, password: string) {
  store.dispatch(login({ username: username, password: password }));
}
```
