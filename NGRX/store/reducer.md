# provideState instead of StoreModule.forFeature:

- ## In Angular 17+ with standalone components, we use provider functions instead of NgModules
- ## provideState('featureName', reducer) replaces StoreModule.forFeature()

## Creating the reducer function

```javascript
export interface GameState {
  home: number;
  away: number;
}

export const initialState: GameState = {
  home: 0,
  away: 0,
};

export const scoreboardReducer = createReducer(
  initialState,
  on(ScoreboardActions.homeScore, (state) => ({
    ...state,
    home: state.home + 1,
  })),
  on(ScoreboardActions.awayScore, (state) => ({
    ...state,
    away: state.away + 1,
  })),
  on(ScoreboardActions.resetScore, () => initialState),
  on(ScoreboardActions.setScores, (state, { home, away }) => ({
    ...state,
    home,
    away,
  }))
);
```

# Using the Standalone API

**Update main.ts**

```javascript
bootstrapApplication(AppComponent, {
  providers: [provideStore()],
});
```

**In the component**

_Lazy Loading Feature State_

```javascript
export const routes: Routes = [
  {
    path: "scoreboard",
    loadComponent: () => import("./scoreboard/scoreboard.component"),
    providers: [provideState("game", scoreboardReducer)],
  },
];

reset() {
    this.store.dispatch(ScoreboardActions.resetScore());
}
```

**After the feature is loaded, the game key becomes a property in the object and is now managed in the state**

```javascript
{
  game: { home: 0, away: 0 }
}
```
