# How I Set Up NgRx For Fairly Complex Projects

## Initial Setup

Before I start using NgRx in my project, I begin by installing all the necessary dependencies I'll need which come with the library. Below are a list of the dependecies:

- @ngrx/store
- @ngrx/effects
- @ngrx/entity
- @ngrx/router-store

Currently these are the main components I have found the most use for. _@ngrx/router-store_ is the newbie I added recently to my arsenal to make working with routing in Angular a breeze. We'll talk about that later.

## Folder Structure & Configuration

After installing the above components, I create a `state/` directory where all my ngrx files will go. This is usually how the folder structure looks like

```
├── app.state.ts
├── boards
│   ├── actions
│   │   └── boards.actions.ts
│   ├── board.state.ts
│   ├── effects
│   │   └── boards.effects.ts
│   ├── reducer
│   │   └── boards.reducer.ts
│   └── selectors
│       └── boards.selectors.ts
├── router.selectors.ts
└── theme
    ├── actions
    │   └── theme.actions.ts
    ├── effects
    │   └── theme.effects.ts
    ├── reducer
    │   └── theme.reducer.ts
    ├── selectors
    │   └── theme.selectors.ts
    └── theme.state.ts
```

At the root of the state directory, I have my `app.state.ts` and `router.selectors.ts` file (I'll go into what each file contains very soon). I then create directories for the different states of my application. Each state would have its own actions, effects, reducer and selectors. A more flatter version would look something like this:

```
├── app.state.ts
├── boards
│   ├── board.state.ts
│   ├── boards.actions.ts
│   ├── boards.effects.ts
│   ├── boards.reducer.ts
│   └── boards.selectors.ts
├── router.selectors.ts
└── theme
    ├── theme.actions.ts
    ├── theme.effects.ts
    ├── theme.reducer.ts
    ├── theme.selectors.ts
    └── theme.state.ts
```

For the rest of this document, I'll use the flatter version to explain what's going on.
_NB:_ Also for demonstration purposes, this is from a kanban task management web app. The fundamental concept should be the same for most projects.
Let's start from the `app.state.ts` file. Here's where you define an interface for your `AppState`. This will contain all the states in your application.

### App State

```
// app.state.ts

import { BoardState } from './boards/board.state';
import { ThemeState } from './theme/theme.state';

export interface AppState {
  board: BoardState;
  theme: ThemeState;
}

```

Each state is typed with an entity state. Let's take a look at the BoardState to understand what the entity state is doing.

### Entities

```
// board.state.ts

import { createEntityAdapter, EntityAdapter, EntityState } from '@ngrx/entity';
import { Board } from '../../models/board.model';

export interface BoardState extends EntityState<Board> {
  selectedBoardId: number | null;
  loading: boolean;
  error: string | null;
}

export const boardAdapter: EntityAdapter<Board> = createEntityAdapter<Board>({
  selectId: (board: Board) => board.id,
  sortComparer: false,
});

export const initialBoardState: BoardState = boardAdapter.getInitialState({
  selectedBoardId: null,
  loading: false,
  error: null,
});
```

This creates an entity out of the given state and uses the id of each entity to uniquely identify them. This will come in very handy when writing selectors for an entity.
The entity adapter allows you to create an adapter (literally) for a single entity collection and provides adapter methods for performing CRUD operations on the given collection.

Here, we create the `boardAdapter` with the `createEntityAdapter()` method. the `selectId` is a method used to select the primary id for the collection. The `sortComparer` is used to sort the collection before displaying it. In my case I set it to false because I didn't want the collection to be sorted.

An initial board state is created using the `getInitialState()` method on the newly created adpater and passed the initial values for the state's properties.
After the entity adapter is created, we can now go ahead and define our actions.

### Actions

After our state entity has been created, we can now go ahead and define actions we can take to interact with the state in the store.
For this board state, there are a few actions we can take:

```
// board.actions.ts

import { createAction, props } from '@ngrx/store';
import { Board } from '../../../models/board.model';

export const loadBoards = createAction('[Boards] Load Boards');
export const loadBoardsSuccess = createAction(
  '[Boards] Load Boards Success',
  props<{ boards: Board[] }>()
);
export const loadBoardsFailure = createAction(
  '[Boards] Load Boards Failure',
  props<{ error: string }>()
);
export const addBoard = createAction(
  '[Boards] Add Board',
  props<{ board: Board }>()
);
export const updateBoard = createAction(
  '[Boards] Update Board',
  props<{ board: Partial<Board> & { id: number } }>()
);
export const deleteBoard = createAction(
  '[Boards] Delete Board',
  props<{ id: number }>()
);
```

We use the `createAction()` method to create actions that should be dispatch for a slice of the application state.

### Reducer

Next, we'll write our reducer function which will be responsible for dispatching these actions to the store.

```
import { createReducer, on } from '@ngrx/store';
import { boardAdapter, initialBoardState } from '../board.state';
import {
  addBoard,
  deleteBoard,
  loadBoardsFailure,
  loadBoards,
  loadBoardsSuccess,
  updateBoard,
} from '../actions/boards.actions';

export const boardReducer = createReducer(
  initialBoardState,
  on(loadBoards, (state) => ({ ...state, loading: true, error: null })),
  on(loadBoardsSuccess, (state, { boards }) =>
    boardAdapter.setAll(boards, { ...state, loading: false, error: null })
  ),
  on(loadBoardsFailure, (state, { error }) => ({
    ...state,
    loading: false,
    error,
  })),
  on(addBoard, (state, { board }) => boardAdapter.addOne(board, state)),
  on(updateBoard, (state, { board }) =>
    boardAdapter.updateOne({ id: board.id, changes: board }, state)
  ),
  on(deleteBoard, (state, { id }) => boardAdapter.removeOne(id, state))
);
```

We use the `createReducer()` function to create a reducer for our state. It accepts the initial state and then `on()` function calls for each action. (NB: Note how our state is updated immutably).
Here you can see how our adapter methods come in handy.

### Effects

Effects are responsible for handling side effects like fetching data from a server, saving data to local storage etc. The part of ngrx that uses rxjs heavily is the effect. Usually for applications of this complexity, the effects are used to fetch data from an external source on initial app load. How this works is that, we have an action tp load the application data. Since this would require the application to make a request outside the application to a server or api, the effect will take this action and make that request, when the data is retrieved, an action is dispatched with the retrieved data to the store. Here's how it looks like:

```
// board.effects.ts

@Injectable()
export class BoardEffects {
  constructor(
    private action$: Actions,
    private apiService: ApiService,
    private localStorageService: LocalstorageService
  ) {}

  loadBoards$ = createEffect(() =>
    this.action$.pipe(
      ofType(loadBoards),
      mergeMap(() => {
        const boardsFromLocalStorage =
          this.localStorageService.getItemFromLocalStorage('boards');

        if (boardsFromLocalStorage) {
          return of(loadBoardsSuccess({ boards: boardsFromLocalStorage }));
        } else {
          return this.apiService.fetchData().pipe(
            map((boards) => {
              this.localStorageService.setItemInLocalStorage('boards', boards);
              return loadBoardsSuccess({ boards });
            }),
            catchError((error) =>
              of(loadBoardsFailure({ error: error.message }))
            )
          );
        }
      })
    )
  );
}
```

Here, our effect listens for a loadBoards action and reacts to it accordingly. Once the data is successfully retrieved, we set it in the store. We are also handling some local storage stuff here.

### Router-Store

The router-store lets you connect the Angular Router with the Store. During each router navigation cycle, multiple actions are dispatched that allow you to listen for changes in the store and react accordingly.
I mainly used the router-store to get url params to fetch a specific board from the store when we navigate to a board detail page.
In the root of my state directory, I have a `router.selectors.ts` file which exports a router selector.

```
// router.selectors.ts

import { getRouterSelectors } from '@ngrx/router-store';

export const { selectRouteParams } = getRouterSelectors();

```

Now I can have a selector that selects just one board entity, pass it the `selectRouteParams` which will handle getting the route parameters and using the id passed to the params to get that specific board entity.

```
// board.selectors.ts

export const selectBoard = createSelector(
  selectBoardEntities,
  selectRouteParams,
  (boards, params) => {
    const id = params['id'];
    return id ? boards[id] : undefined;
  }
);
```

I can easily use this selector to get a board based on the route param like so

```
// board-detail.component.ts

@Component({
  selector: 'app-board-detail',
  standalone: true,
  imports: [AsyncPipe, BoardContentComponent, BoardFormComponent],
  templateUrl: './board-detail.component.html',
  styleUrl: './board-detail.component.sass',
})
export class BoardDetailComponent {
  board$ = this.store.select(selectBoard);

  constructor(private store: Store) {}

  ngOnInit() {
    this.board$.subscribe((board) => console.log(board));
  }
}
```
