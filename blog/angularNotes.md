---
slug: angular
title: Angular Notes
author: Mark Beebe
tags: [training]
---

- New components can easily be created with this command: ng g c {path}/{to}/{component}

- Reducers sort of act as an intermediary between the user's actions and the state
- Reducer example:

```ts
    import { ɵsetCurrentInjector } from "@angular/core";
    import { createReducer, on} from "@ngrx/store";
    import { CounterCommands } from "../actions/count-actions";


    export interface CountState {
        current: number;
        by: number;
    }

    const initialState:CountState = {
        current: 0,
        by: 0
    }

    export const reducer = createReducer(initialState,
        on(CounterCommands.countby, (s,a) => ({...s, by: a.by})),
        on(CounterCommands.reset, (s) => ({...s, current: 0})),
        on(CounterCommands.incremented, (currentState) => ({...currentState, current: currentState.current + currentState.by})),
        on(CounterCommands.decremented, (s) => ({...s, current: s.current - s.by}))
    );
```

- Actions contain functions that can be called by components
- Actions example:

```ts
    import { createActionGroup, emptyProps, props} from "@ngrx/store";

    

    export const CounterCommands = createActionGroup({
        source: 'Counter Commands',
        events: {
            incremented: emptyProps(),
            decremented: emptyProps(),
            reset: emptyProps(),
            countby: props<{by: 1 | 3 | 5}>()
        }
    })
```

Component calls that action:

```ts
    import { Component } from '@angular/core';
    import { Store } from '@ngrx/store';
    import { selectCountingBy } from '../../state';
    import { CounterCommands } from '../../state/actions/count-actions';

    @Component({
    selector: 'app-prefs',
    templateUrl: './prefs.component.html',
    styleUrls: ['./prefs.component.css']
    })
    export class PrefsComponent {

    countingBy$ = this.store.select(selectCountingBy);
    
    constructor(private store:Store) {}
    setCountBy(by:1 | 3 | 5){
        this.store.dispatch(CounterCommands.countby({by}));
    }
    }
```


- Index.ts is similar to an index.html, where we can set up things to be imported by other classes



