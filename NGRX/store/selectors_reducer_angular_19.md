- ## createFeature (Uses createFeature to automatically generate)
  - #### 1- Reducer (state manager)
  - #### 2- Basic selectors (selectCount, selectIsLoading)

Reducer (state manager)

Basic selectors (selectCount, selectIsLoading)

**counter.state.ts**

```typescript
import { createFeature } from "@ngrx/store";

export interface CounterState {
  count: number;
  isLoading: boolean;
}

// Initial state
const initialState: CounterState = {
  count: 0,
  isLoading: false,
};

// Create feature (auto-generates selectors)
export const counterFeature = createFeature({
  name: "counter",
  reducer: createReducer(
    initialState,
    on(CounterActions.increment, (state) => ({
      ...state,
      count: state.count + 1,
    })),
    on(CounterActions.decrement, (state) => ({
      ...state,
      count: state.count - 1,
    })),
    on(CounterActions.load, (state) => ({
      ...state,
      isLoading: true,
    })),
    on(CounterActions.loadSuccess, (state, { newValue }) => ({
      ...state,
      count: newValue,
      isLoading: false,
    }))
  ),
});
```

**counter.selectors.ts**

```javascript
export const { selectCount, selectIsLoading } = counterFeature;

// Create a composed selector
export const selectDoubleCount = createSelector(
  selectCount,
  (count) => count * 2
);
```

**counter.component.ts (USAGE)**

```typescript
count$ = this.store.select(selectCount);
doubleCount$ = this.store.select(selectDoubleCount);
loading$ = this.store.select(selectIsLoading);

constructor(private store: Store) {}

increment() {
  this.store.dispatch(CounterActions.increment());
}

decrement() {
  this.store.dispatch(CounterActions.decrement());
}
```

## state and reducer defintion with combining selectors. (different ways)

1- state definition

```javascript
Copy;
// user.state.ts
import { createFeature, createReducer, on } from "@ngrx/store";
import { User } from "./user.model";

export interface UserState {
  users: User[];
  loading: boolean;
  error: string | null;
  selectedUserId: string | null;
}

const initialState: UserState = {
  users: [],
  loading: false,
  error: null,
  selectedUserId: null,
};

export const userFeature = createFeature({
  name: "user",
  reducer: createReducer(
    initialState
    // Reducer logic would go here
  ),
});
```

2- Basic Selector

```javascript
export const selectAllUsers = userFeature.selectUsers;

// Equivalent manual implementation:
export const selectAllUsersManual = createSelector(
  userFeature.selectUserState, // First argument: feature selector
  (state: UserState) => state.users // Projector function
);
```

- _Arguments:_
  - _userFeature.selectUserState - Base selector that provides the full state slice_
  - _Projector function that extracts specific data_

2- Combined Selector

```javascript
// Get selected user details
export const selectSelectedUser = createSelector(
  selectAllUsers, // First input selector
  userFeature.selectSelectedUserId, // Second input selector
  (users, selectedId) => {
    // Projector function
    return users.find((user) => user.id === selectedId);
  }
);
```

- _Arguments:_
  - _selectAllUsers - Selector for user list_
  - _userFeature.selectSelectedUserId - Selector for selected ID_
  - _Projector receives results in same order as input selectors_

2- Parameterized Selector

```javascript
// Get users by age (parameterized)
export const selectUsersByAge = (targetAge: number) =>
  createSelector(selectAllUsers, (users) =>
    users.filter((user) => user.age === targetAge)
  );
```

- _Arguments:_
  - _Factory function takes parameters_
  - _Returns selector with frozen parameters_
  - _Usage: store.select(selectUsersByAge(25))_

2- Combined Data Selector

```javascript
// Get user statistics
export const selectUserStats = createSelector(
  selectAllUsers,
  selectActiveUsers,
  (allUsers, activeUsers) => ({
    total: allUsers.length,
    active: activeUsers.length,
    ratio: activeUsers.length / allUsers.length,
  })
);
```

- _Arguments:_
  - _selectAllUsers - First input_
  - _selectActiveUsers - Second input_
  - _Projector combines both inputs_

# _NOTE_

- Projector Function

  - Receives results from input selectors as arguments

  - Should be pure function (no side effects)

  - Last argument in createSelector call
